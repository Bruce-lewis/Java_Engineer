# 1. JUC概述
java并发编程，java.util.concurrent工具包的的简称，处理多线程的工具包。

用户线程：由用户创建和控制的线程。在程序中显式创建的线程，生命周期由用户代码控制。
守护线程：运行在后台的线程，不需要在程序中显示的创建，用于执行一些后台任务，如垃圾回收、日志记录。当所有的用户线程都结束时，守护线程会自动退出。

如果当前JVM中用户线程都结束了，那JVM会结束，不管有没有守护线程。
# 2. Lock接口

### ReentrantLock 可重入锁的使用案例

```java
	ReentrantLock lock = new ReentrantLock();
	....
	lock.lock();
	try{
		//需要上锁的代码
	}
	finally{
		lock.unlock();
	}
```
### synchronized 同步方式 与 Lock对比

1. 定义上：Lock 是一个接口，提供了多种实现类，适用于更加复杂的场景。而 synchronized 是Java 中的关键字，是内置的语言实现;
2. 使用上1：Lock 可以让等待锁的线程==响应中断==，而 synchronized却不行，使用synchronized时，等待的线程会一直等待下去，不能够响应中断;
3. 使用上2：通过 Lock 可以知道线程有没有成功获取锁，而 synchronized 却无法办到
4. 发生异常时：synchronized 在发生异常时，会自动释放线程占有的锁；而 Lock 在发生异常时，如果没有主动通过 unLock去释放锁，则很可能造成死锁现象，因此使用 Lock 时需要在 finally 块中释放锁;
5. 并发支持上：Lock 可以提高多个线程进行读操作的效率。
6. 效率上：在资源竞争激烈的情况下，Lock的性能远大于sychcronized

# 3. 线程间通信
### synchronized编写线程通信的步骤
1. 创建资源类，创建属性和操作方法。
2. 编写操作方法
	1. 判断
	2. 干活
	3. 通知
3.  创建多个线程，调用资源类的操作方法。
4. 防止虚假唤醒，将判断条件加到while循环中

```java
//我只写资源类的创建哈
class ShareNubmer{
	private int number = 0;
	public synchronized void incre(){
		//有时候线程可能在没有收到唤醒信号的情况下被唤醒，即发生虚假唤醒。
		//if(number!=0){//防止虚假唤醒造成的逻辑出错，线程在哪里睡，就在哪里醒，醒了就不会判断了
		//通常会在等待循环中使用while循环来检查条件是否满足，而不是使用if语句。这样，即使发生虚假唤醒，线程仍会重新检查条件并继续等待，直到条件真正满足。
		while(number!=0){
			this.wait();
		}
		number++;
		this.notifyAll();
	}
	public synchronized void decre(){
		while(number!=1){
			this.wait();
		}
		number--;
		this.notifyAll();
	}
}
```

### Lock编写线程通信
```java
class ShareNumber{
	private int number = 0;
	private Lock lock = new ReentrantLock();
	private Condition condition = lock.newCondition();
	public void incre(){

		lock.lock();
		try{
			while(number!=0){
				condition.await();
			}
			number++;
		}finally{
			condition.signalAll();
		}
	}
	public void decre(){

		lock.lock();
		try{
			while(number!=1){
				condition.await();
			}
			number--;
		}finally{
			condition.signalAll();
		}
	}
}
```

# 4. 线程间定制化通信

```java
class ShareNumber{
	private int flag = 1;//这个作为操作方法中的判断用 1 AA       2  BB        3 CC
	private Lock lock = new ReentrantLock();
	private Condition c1 = lock.newCondition();// 三个condition作为定制化通信用
	private Condition c2 = lock.newCondition();
	private Condition c3 = lock.newCondition();
}
```

# 5. 集合的线程安全
## 5.1 ArrayList集合线程不安全问题演示
```java
class ThreadDemon1{
	public static void main(String[] args){
		List<String> list = new ArrayList<>();
		for(int i=0;i<10;i++){
			new Thread(()->{
				list.add(UUID.randomUUID().toString().substring(0,8));
			},String.valueOf(i) ).start();
		}
	}
}
```
java.util.ConcurrentModificationException
1. 解决方案Vector
```java
		//List<String> list = new ArrayList<>();
		List<String> list = new Vector<>();
```
2.  解决方案Collections
```java
		//List<String> list = new ArrayList<>();
		List<String> list = Collections.sychronizedList(new ArrayList());

```
3.  解决方案-CopyOnWriteArrayList

```java
		//List<String> list = new ArrayList<>();
		List<String> list = new CopyOnWriteArrayList<>();
```
写时复制技术：
并发读，独立写，写的时候复制一份，等到写完合并至原来的集合。

## 5.2 HashSet集合线程不安全问题演示
```java
class ThreadDemon1{
	public static void main(String[] args){
		Set<String> set = new HashSet<>();
		for(int i=0;i<10;i++){
			new Thread(()->{
				set.add(UUID.randomUUID().toString().substring(0,8));
			},String.valueOf(i) ).start();
		}
	}
}
```
1.  解决方案-CopyOnWriteArraySet
```java
		//Set<String> set = new HashSet<>();
		Set<String> set = new CopyOnWriteArraySet<>();
```
2. 解决方案-Collections
```java
		//Set<String> set = new HashSet<>();
		Set<Object> set = Collections.synchronizedSet(new HashSet<>());
```
## 5.3 HashMap集合线程不安全问题演示
```java
class ThreadDemon1{
	public static void main(String[] args){
		Map<String,String> map = new HashMap<>();
		for(int i=0;i<10;i++){
			String key = String.valueOf(i);
			new Thread(()->{
				map.put(key,UUID.randomUUID().toString().substring(0,8));
			},String.valueOf(i) ).start();
		}
	}
}
```
1.  解决方案-ConcurrentHashMap
```java
		//Map<String,String> map = new HashMap<>();
		Map<String,String> map = new ConcurrentHashMap<>();
```
2. 解决方案-Collections
```java
		//Map<String,String> map = new HashMap<>();
		Map<String,String> map = Collections.synchronizedMap(new HashMap<>());
```
# 6. 多线程锁
## 6.1 sychronized 锁对象
sychronzied 实现同步的基础，java中每一个对象都可以作为锁对象，
具体表现为以下三种形式。
对于普通方法，锁是当前实例的对象。
对于静态方法，锁是当前类的Class对象。
对于同步方法块，锁是Sychronized括号里配置的对象。

## 6.2 公平锁和非公平锁
非公共锁：new ReentranLock();
		有些线程没有执行的机会。
		但是效率高。
		
公平锁：new ReentranLock(true);
		线程都有执行的机会。
		 但是效率低。
## 6.3 ReentranLock 可重入锁
可重入锁：
可重入锁（Reentrant Lock）是一种支持重复进入的锁机制。在Java的并发编程中，它允许某个线程在持有锁的情况下再次获取同一个锁，而不会发生死锁。

特点1，当一个线程持有锁时，它可以多次进入由该锁保护的临界区或代码块，而不会被阻塞。这种机制使得线程可以递归地调用同步方法或代码块，从而简化了编程模型。
特点2，它会为每个线程维护一个持有计数器。当一个线程第一次获取锁时，计数器值为1，每次重入时，计数器值加1；当线程释放锁时，计数器值减1。只有当计数器值为0时，锁才会完全释放，其他线程才能获取该锁。


## 6.4 死锁
1. [[Multi-thread#线程同步造成的死锁问题|概念]]
2. 因素
3. 验证是否产生死锁：①jps 查看运行进程号，类似linux中的ps -ef ②jstack pid 这个是jvm自带的堆栈跟踪工具。



# 7. [[Multi-thread#创建一个类，实现Callable接口|Callable接口]]



# 8. JUC强大的辅助类
### 8.1 减少计数CountDownLatch
1. 说明
   CountDownLatch 类可以设置一个计数器，然后通过 countDown 方法来进行减1的操作，使用 await 方法等待计数器不大于0，然后继续执行 await 方法之后的语句。
	CountDownLatch 主要有两个方法，当一个或多个线程调用await方法时，这些线程会阻寒
	其它线程调用 countDown 方法会将计数器减 1(调用 countDown 方法的线程不会阻塞).
	当计数器的值变为 0时，因 await 方法阻寒的线程会被唤醒，继续执行
2. 使用场景
   当教室里的6个同学走完后，班长才关门
```java
//CountDownLatch
public class CountDownlatchDemo(
	public static void main(string[]args){
	CountDownLatch latch = new CountDownLatch(6);
	for(int i =1;i<=6;i++){
		new Thread(()->{
			System.out.println(Thread.currentThread().getName()+"号同学离开了教室");
			latch.countDown();
		},string.valueOf(i)).start();
		latch.await();
		System.out.println(Thread.currentThread().getNane()+"班长锁门走人了");
	}
}
```
### 8.2 循环栅栏 CyclicBarrier
1. 说明
   个同步辅助类，它允许一组线程互相等待，直到到达某个公共屏障点(common barrier point)。在涉及一组固定大小的线程的程序中，这些线程必须不时地互相等待，此时CycicBarier很有用。因为该 barier 在释放等待线程后可以重用，所以称它为婚环的 barrier.
2.  使用场景：七个人各自收集龙珠一起去召唤神龙 (循环体现出来：我觉得更像是几个小伙伴闯关，一起通过一关后，才能开始下一关)
   ```java
//CyclicBarrierDemo
public class CyclicBarrierDemo {  
    private final static int NUMBER = 7;  
  
    public static void main(String[] args) {  
        CyclicBarrier barrier = new CyclicBarrier(NUMBER, () -> {  
            System.out.println("七颗龙珠集齐，召唤神龙！");  
        });  
  
        for (int i = 0; i < 14; i++) {  
            new Thread(()->{  
                try {  
                    System.out.println(Thread.currentThread().getName()+"星被收集到了");  
                    barrier.await();  
                } catch (InterruptedException e) {  
                    e.printStackTrace();  
                } catch (BrokenBarrierException e) {  
                    e.printStackTrace();  
                }  
            },String.valueOf(i)).start();  
  
        }  
    }  
}
```
### 8.3 信号灯 Semaphore
1. 说明：有点像线程池的概念
   一个计数信号量，从概念上讲，信号量维护了一个许可集，如果必要，在许可可用前会阻塞每一个acquire(), 然后再获取该许可，每个release（）释放一个许可，从而可能释放一个正在阻塞的获取者，但是，不使用实际的许可对象，Semaphore只对许可的号码进行计数，并采用相应的行动。
2. 使用场景：6个汽车停3个车位
```java
public class SemaphoreDemo {  
    public static void main(String[] args) {  
        Semaphore semaphore = new Semaphore(3);  
        for (int i = 1; i <= 6; i++) {  
            new Thread(()->{  
                try {  
                    semaphore.acquire();  
                    System.out.println(Thread.currentThread().getName()+"抢到车位");  
                    TimeUnit.SECONDS.sleep(new Random().nextInt(5));  
                    System.out.println(Thread.currentThread().getName()+"离开了车位");  
                } catch (InterruptedException e) {  
                    e.printStackTrace();  
                }finally {  
                    semaphore.release();  
                }  
            },String.valueOf(i)).start();  
        }  
    }  
}
```
# 9. ReentrantReadWriteLock 读写锁
### 9.1 悲观锁和乐观锁的概念
1. 悲观锁：
   悲观锁的思想是，对于共享资源的访问，假设会发生冲突，因此在访问共享资源之前先获取锁。悲观锁认为在并发环境中，冲突是常态，因此默认情况下假设其他线程会修改共享资源，所以会阻塞其他线程的访问。
2. 乐观锁：
   乐观锁的基本思想是，对于共享资源的访问，假设不会发生冲突，因此在访问共享资源之前不会获取锁。乐观锁认为在并发环境中，冲突是较少发生的，所以不会阻塞其他线程的访问，而是在更新共享资源时进行检查。
### 9.2 读锁和写锁的概念
1. 读锁：又叫共享锁，多个线程一起访问
2. 写锁：又叫独占锁，一次一个线程访问

读锁发生死锁的场景：线程1和2都对数据A先读后修改，因为是读锁，可以共同读访问。
线程1在修改的时候，需要等待线程2的读之后，
线程2在修改的时候，需要等待线程1的读之后。

写锁发生死锁的场景：线程1对记录A进行写操作，再对记录B进行写操作，线程2对记录B进行写操作，再对记录A进行写操作。他两互相抱住了。

### 9.3 使用
```java
class MyCache{  
    private volatile Map<String,Object> map = new HashMap<>();  
    //创建读写锁对象  
    private ReentrantReadWriteLock rwLock = new ReentrantReadWriteLock();  
    public void put(String key,Object value){  
        //添加写锁  
        rwLock.writeLock().lock();  
        System.out.println(Thread.currentThread().getName()+"正在写操作，key是"+key);  
        try {  
            TimeUnit.SECONDS.sleep(2);  
            map.put(key,value);  
            System.out.println(Thread.currentThread().getName()+"写完了，key是"+key);  
        } catch (InterruptedException e) {  
            e.printStackTrace();  
        }finally {  
            rwLock.writeLock().unlock();  
        }  
    }  
    public Object get(String key){  
        //添加写锁  
        rwLock.readLock().lock();  
        Object o = null;  
        System.out.println(Thread.currentThread().getName()+"正在读操作，key是"+key);  
        try {  
            TimeUnit.SECONDS.sleep(2);  
            o = map.get(key);  
            System.out.println(Thread.currentThread().getName()+"读完了，key是"+key);  
        } catch (InterruptedException e) {  
            e.printStackTrace();  
        }finally {  
            rwLock.readLock().unlock();  
        }  
        return o;  
    }  
}  
public class ReentrantReadWriteLockDemo {  
    public static void main(String[] args) throws InterruptedException {  
        MyCache myCache = new MyCache();  
        for (int i = 1; i <= 5; i++) {  
            final int num = i;  
            new Thread(()->{  
                myCache.put(num+"",num+"_value");  
            },String.valueOf(i)).start();  
        }  
        TimeUnit.SECONDS.sleep(2);  
        for (int i = 1; i <= 5; i++) {  
            final int num = i;  
            new Thread(()->{  
                myCache.get(num+"");  
            },String.valueOf(i)).start();  
        }  
    }  
}
```

### 9.4. 读写锁的演变
1. 阶段1：多线程不加锁，产生线程安全问题。
2. 阶段2：加独占锁，如sychronized 和ReentrantLock ，每次只能有一个线程操作。
3. 阶段3：读写锁ReentrantReadWriteLock, 读读可以共享，提升性能。缺点是：①造成锁饥饿，一直读，没有写操作，比如挤地铁，一个人下，100个上，1个人一直下不去。②读的时候不能写，只有读完后才能写，写操作可以读。

4. 锁降级：获取写锁---> 获取读锁--->释放写锁----> 释放读锁。读锁不能升级为写锁。
```java
public class DegradeLock {  
    public static void main(String[] args) {  
        ReentrantReadWriteLock rwLock = new ReentrantReadWriteLock();  
        ReentrantReadWriteLock.ReadLock readLock = rwLock.readLock();  
        ReentrantReadWriteLock.WriteLock writeLock = rwLock.writeLock();  
        writeLock.lock();  
        System.out.println("获取读锁");  
        readLock.lock();  
        System.out.println("获取写锁");  
        writeLock.unlock();  
        System.out.println("释放写锁");  
        readLock.unlock();  
        System.out.println("释放读锁");  
    }  
}
```
# 10. BlockingQueue队列阻塞
## 10.1 阻塞队列的概述
Java并发编程中的一个接口，它扩展了Queue接口，提供了一种在多线程环境下进行线程间通信的机制。它的主要特点是当队列为空时，从队列中获取元素的操作会被阻塞，直到队列中有新的元素可用；当队列已满时，往队列中添加元素的操作会被阻塞，直到队列有空闲位置。

## 10.2 阻塞队列的架构

## 10.3 阻塞队列分类
## 10.4 阻塞队列核心方法

# TreadPool线程池

# Fork/Join 分支合并框架

# CompetableFuture 异步回调