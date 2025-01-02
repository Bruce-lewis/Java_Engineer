java 8 中两个重要特性，一个是lambda表达式，一个是Stream API (java.util.stream.*)
# Lambda表达式
Lambda 表达式，也可称为闭包，它是推动 Java 8 发布的最重要新特性。
Lambda 允许把函数作为一个方法的参数（函数作为参数传递进方法中）。
使用 Lambda 表达式可以使代码变的更加简洁紧凑。

## 1. 语法 
 1. lambda 表达式的语法格式如下：
``` 
(parameters) -> expression 
或 (parameters) ->{ statements; }
```
`parameters` 是参数列表，`expression` 或 `{ statements; }` 是Lambda 表达式的主体。
如果左边只有一个参数，可以省略括号；如果没有参数，也需要空括号。
如果右边是statements且只有一个语句，return可以省略。


 2. Lambda 表达式需要“函数式接口”的支持
函数式接口：接口中只有一个抽象方法的接口，称为函数式接口，可以使用注解@FunctionalInterface修饰，可以检查是否是函数式接口。

## 1.5 lambda 表达式代表什么（我自己补充的哦）
可以理解lambda表达式就是①接口 抽象方法的实现，②同时也是一个对象（相当于new XxxInterface() ）。

演化的过程：
 前置条件，有一个方法methodA(XxxInterface xxx)，方法的参数是一个接口类型（好处是多态），你想调用。
 
 阶段1： 此时我们需要编写一个接口的实现类，创建一个实现类的对象传给方法。 就像 MyImplementClass mIC = new MyImplementClass().  把mlc 传给方法。
 阶段2：在考虑到有匿名对象， 不用声明变量，所以呢？你可以直接把 new MyImplementClass() 这个匿名对象传给 方法。
 阶段3：当接口中XxxInterface只有一个抽象方法，你在阶段1 编写的实现类 MyImplementClass 就是为了实现这个唯一的抽象方法，所以啊，本质是为了实现抽象方法。 那么好，你现在可以直接传给methodA一个表达式，这个表达式就是抽象方法的实现，同时也能作为methodA方法的参数（本质是上是不是一个对象呢？）


## 2. 内置四大核心函数式接口
	 Consumer<T> : 消费型接口                       void accept（T t );
	 Supplier<T> : 供给型接口                       T get();
	 Function<T,R>: 函数型接口                      R apply(T t);
	 Predicate<T>: 断言型接口                       boolean test(T t);


在Java中，四个主要的函数式接口是 `Predicate`、`Function`、`Consumer` 和 `Supplier`。下面是每个接口的简单说明以及对应的 Lambda 表达式示例：

1. **Predicate \<T>**：接受一个参数并返回一个布尔值。

   ```java
   Predicate<Integer> isEven = n -> n % 2 == 0;
   ```

   使用示例：
   ```java
   boolean result = isEven.test(4); // 返回 true
   ```

>[!info] 如果你调用的方法需要一个 Predicate\<Integer> 类型的参数，你可以直接把 n -> n % 2 == 0 这个表达式传给它

2. **Function<T, R>**：接受一个参数并返回一个结果。

   ```java
   Function<String, Integer> stringLength = str -> str.length();
   ```

   使用示例：
   ```java
   int length = stringLength.apply("Hello"); // 返回 5
   ```

3. **Consumer\<T>**：接受一个参数并不返回结果。

   ```java
   Consumer<String> print = str -> System.out.println(str);
   ```

   使用示例：
   ```java
   print.accept("Hello, World!"); // 输出 "Hello, World!"
   ```

4. **Supplier\<T>**：不接受任何参数但返回一个结果。

   ```java
   Supplier<Double> randomValue = () -> Math.random();
   ```

   使用示例：
   ```java
   double value = randomValue.get(); // 返回一个随机数
   ```

这些 Lambda 表达式可以在 Java 8 及更高版本中使用，能够简化代码并提高可读性。
## 3. 方法引用和构造器引用
### 3.1  方法引用
若Lambda体中的内容有方法已经实现了，我们可以使用方法引用（可以理解为方法引用是Lambda表达式的另一种表现形式）

三种语法格式
对象::实例方法
```java
	PrintStream ps1 = System.out;
	Consumer<String> con = (x)->ps1.println(x);
	
	Consumer<String> con1 = System.out::pringln;
	con1.accept("abcd");
```
类::静态方法
```java
	Comparator<Integer> com = (x,y)->Integer.compare(x,y);

	Comparator<Integer> com = Integer::compare;
```
前两种的条件是：你需要实现的函数式接口的抽象方法的①参数列表与②返回值类型 要与
调用的方法的①参数列表 与 ②返回值类型 保持一致，才能将Lambda体改为方法引用。

类::实例方法
```java
	BiPredicate<String,String> bp = (x,y)->x.equals(y);

	BiPredicate<String,String> bp = String::equals;
```

这一种条件是：若Lambda参数列表的第一参数 是实例方法的调用者，而第二个参数是实例方法的参数时，可以使用ClassName::method

### 3.2 构造器引用
格式
ClassName::new
```java
	Function<Integer,Employee> fun = (x) -> new Employee(x);

	Function<Integer,Employee> fun = Employee::new;
```
注意：需要调用的构造器参数要与函数式接口中的抽象方法的参数列表保持一致。

### 3.3 数组引用
格式
Type[]::new
```java
	Function<Integer,String[]> fun = (x) -> new String[x];

	Function<Integer,String[]> fun = String[]::new;
```

# Stream API
Stream 是Java 8 中处理集合的关键抽象概念，它可以指定你希望对集合进行的操作，可以执行非常复杂的查找、过滤、和映射数据等操作。
使用Stream API对集合数据进行操作，就类似使用sql执行的数据库查询。
也可以使用Stream API来并行执行操作。
总之，Stream API 提供了一种高效且简易的处理数据的方式



Stream 到底是什么？
是数据渠道，用于操作数据源（集合、数组等）所生成的元素序列。
“集合讲究的是数据，流讲究的是计算”

### 3.1 Stream的操作的三个步骤
1. 创建Stream
   一个数据源（集合、数组），获取一个流
2. 中间操作
   一个中间操作链，对数据源的数据进行处理
3. 终止操作
   一个终止操作，触发中间操作链，并产生结果

### 3.2 获取流的方式
1. 可以通过Collection系列集合提供的stream() 或者parallelStream() 方法
```java
   List<String> list = new ArrayList<>();
   Stream<String> stream1 = list.stream();
```
2. 通过Arrays中的静态方法stream（）获取数组流
```java
   Employee[] emps = new Employee[10];
   Stream<Employee> stream2 = Arrays.stream(emps);
```
3. 通过Stream类中的静态方法of()
   ```java
   Stream<String> stream3 = Stream.of("aa","bb","cc");
```
4. 创建无限流
```java
//迭代
Stream<Integer> stream4 = Stream.iterate(0,(x)->x+2);
//生成
Stream.generate(()->Math.random());
```

### 3.3 中间操作
#### 筛选与切片
filter-接收lambda，从流中排除某些元素。
limit(n)-截断流，使流的元素不超过给定的数量。
skip(n)-跳过元素，若流中元素不足n，则返回空流。
distinct-元素去重，通过元素的hashCode() 和equals() 方法去重。

中间操作中会自动进行==内部迭代==：迭代由Stream API完成。
```java
	employees.stream()
		.filter((e)->e.getSalary()>5000)
		.skip(2)
		.forEach(System.out::println);
```

#### 映射
map-接收lambda表达式，将元素转换为其它形式或提取信息。接收一个函数作为参数，该函数会被应用到每个元素上，并将其映射成一个新元素。

```java
List<String> list = Arrays.asList("asdf", "asdf", "22", "asdrf");  
list.stream()  
        .map(String::toUpperCase)  
        .forEach(System.out::println);
employees.stream()
		.map(Employee::getName)
		.forEach(System.out.println);
```
flatMap-接收lambda表达式作为参数，将流中每个元素都换成另一个流，然后把所有流连接成一个流。
相当于把多维数组转成一维数组。

#### 排序
sorted()-自然排序
sorted(Comparator com)-定制排序
```java
	List<String> list = Arrays.asList("asdf", "asdf", "22", "asdrf");  
	list.stream()
		.storted()
		.forEach(System.out::println);

	List<Employee> employees = Arrays.asList(new Employee(1, "AA"),  
        new Employee(3, "aa"),  
        new Employee(2, "cc"),  
        new Employee(3, "bb"),  
        new Employee(4, "dd"));  
	employees.stream()  
        .sorted((e1,e2)->{  
            if(e1.getAge()==(e2.getAge())){  
                return e1.getName().compareTo(e2.getName());  
            }else{  
                return e1.getAge()-(e2.getAge());  
            }  
        }).forEach(System.out::println);
```

### 3.4 终止操作
#### 查找与匹配
allMatch-检查是否匹配所有元素
anyMatch-检查是否至少匹配一个元素
noneMatch-检查是否没有匹配所有元素
findFirst-返回第一个元素
findAny-返回当前流中的任意元素
count-返回当前流中元素的个数
max-返回流中的最大值
min-返回流中的最小值


#### 归约
reduce(T identity, BinaryOperator)
reduce(BinaryOperator)  
可以将流中元素反复结合起来，得到一个值。
```java
	List<Integer> list = Arrays.asList(1,2,3,4,5,6,7);
	Integer sum = list.stream()
					  .reduce(0,(x,y)->x+y);
```

#### 收集
collect-将流转换成其它形式，接收一个Collector接口的实现，它是用于给stream中元素汇总的方法

Collector接口中方法的实现决定了如何对流执行收集操作（如收集到List，Set、Map）。==Collectors==这个工具类提供了很多静态方法可以方便地创建收集器的实例
```java
	List<String> list = employees.stream()
								.map(Empolyee::getName)
								.collect(Collectors.toList());

	Set<String> set = employees.stream()
								.map(Empolyee::getName)
								.collect(Collectors.toSet());
								
	HashSet<String> set = employees.stream()
								.map(Empolyee::getName)
									.collect(Collectors.toCollection(HashSet::new));
	//工资的总数、平均值、总和、最大值、最小值
	Double avg = employees.stream()
						  .collect(Collectors.averagingDouble(Employee::getSalary);
	//一下全都求出来
	DoubleSummaryStatistic dss = employees.stream()
							.collect(Collectors.summarizingDouble(Employee::getSalary));
	System.out.println(dss.getSum());
	System.out.println(dss.getMax());

	//分组
	Map<Status,List<Employee>> map = employees.stream()
							.collect(Collectors.groupingBy(Employee::getStatus));

	//多级分组
	Map<Status,Map<String,List<Employee>>> map = employees.stream()
							.collect(Collectors.groupingBy(Employee::getStatus,Collectors.groupingBy((e)->{
								if(((Employee)e).getAge()<=35){
									return "青年";
								}else if(((Employee)e).getAge()<=60){
									return "中年";
								}else{
									return "老年"
								}
							})));
	//分区
	Map<Boolean,List<Employee>> map = employees.stream()
					.collect(Collectors.partitioningBy((e)->e.getSalary()>8000));

	//拼接
	String str = employees.stream()
					.map(Employee::getName)
					.collect(Collectors.joining(",")); 
	
```


### 3.5 并行流
并行流就是把一个内容分成多个数据块，并用不同的线程分别处理每个数据块的流。

Stream API可以声明性通过parallel() 与sequential() 在并行流与顺序流之间进行切换。
```java
	LongStream.rangeClosed(0,100000000L)
			.parallel()
			.reduce(0,Long::sum);//快很多
```

### 3.6 Optional 类
	Optional<T>类（java.util.Optional）是一个容器类，代表一个值存在不存在，原来用null表示一个值不存在，现在Optional可以更好地表达这个概念，并且可以避免空指针异常。

#### 常用方法
Optional.of(T t) 创建一个Optional的实例
Optional.empty() 创建一个空的Optional实例
Optional.ofNullable(T t) 若t不为null，创建Optional实例，否则创建空实例
isPresent() 判断是否包含值
orElse(T t) 如果调用对象包含值，则返回该值，否则返回t
orElseGet(Supplier s) 如果调用对象包含值，则返回该值，否则返回s获取的值。
map(Function f) 如果有值对其处理，并处理返回后的Optional，否则返回Optional.Empty()
flatMap(Function mapper)：与map类似，要求返回的值必须是Optional
get() 返回Optional中的对象


```java
package com.bruce;  
  
public class Man {  
    private String name;  
    private Godness godness;  
  
    public String getName() {  
        return name;  
    }  
  
    public void setName(String name) {  
        this.name = name;  
    }  
  
    public Godness getGodness() {  
        return godness;  
    }  
  
    public void setGodness(Godness godness) {  
        this.godness = godness;  
    }  
}
```

```java
package com.bruce;  
  
public class Godness {  
    private String name;  
  
    public String getName() {  
        return name;  
    }  
  
    public void setName(String name) {  
        this.name = name;  
    }  
}
```

```java

@org.junit.Test  
public void test1() {  
    Man man = new Man();  
    String godnessName = getGodnessName(man);  
    System.out.println(godnessName);  
}  
public String getGodnessName(Man man){  
    if(man!=null){//如果程序员不判断，就会发生空指针异常  
        Godness godness = man.getGodness();  
        if(godness!=null){  
            return godness.getName();  
        }  
    }  
    return "没有女神";  
}
```


使用Optional

```java
package com.bruce;  
  
import java.util.Optional;  
  
public class NewMan {  
    private String name;  
    private Optional<Godness> godness = Optional.empty();  
  
    public String getName() {  
        return name;  
    }  
  
    public void setName(String name) {  
        this.name = name;  
    }  
  
    public Optional<Godness> getGodness() {  
        return godness;  
    }  
  
    public void setGodness(Optional<Godness> godness) {  
        this.godness = godness;  
    }  
}
```

```java
@org.junit.Test  
public void test1() {  
    Optional<NewMan> newMan = Optional.ofNullable(new NewMan());  
    String godnessName2 = getGodnessName2(newMan);  
    System.out.println(godnessName2);  
}  
public String getGodnessName2(Optional<NewMan> man){  
    return man.orElse(new NewMan())  
            .getGodness()  
            .orElse(new Godness())  
            .getName();  
}
```