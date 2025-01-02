#### 前言
###### 一. 数组的弊端
1. 数组一旦初始化，长度不可变
2. 数组无法应用于无序的，不可重复的场景
3. 数组中的方法，属性极少
4. 针对于删除、插入操作性能极差

###### 二. 集合的框架(java.util包下)
java.util.Collection: 存储一个一个的数据
	 |--------子接口List：存储有序的，可重复的数据
		 |---------ArrayList、LinkedList、Vector
		 
	 |--------子接口Set：存储无序的，不可重复的数据
		|--------HashSet、LinkedHashSet、TreeSet


java.util.Map: 存储一对一对的数据
	|------HashMap、LinedHashMap、TreeMap、Hashtable、Properties


###### 三. Collection 
1. 共15个方法
		add(Object obj);
		addAll(Collection coll)
		clear()
		isEmpty()
		size()
		contains(Object obj)
		containsAll(Collection coll) 
		retainAll(Collection coll)
		remove(Object obj)
		removeAll(Collection coll)
		iterator()
		hashCode()
		eqauls()

2. 集合和数组之间的转换
		集合---->数组：toArray()
		数组---->集合：Arrays.asList(Object ... objs) 可变形参
		Arrays.asList(new Integer[]{1,2,3} ) 三个元素
		Arrays.asList(new int[]{1,2,3}) 一个元素

3. 向Collection集合中添加元素的要求
	要求一定要重写equals（）方法, 因为contains，remove方法会用到
#### List
###### 一. ArrayList
特点：
1. 有序的、可以重复的数据
2. 底层使用了Object[] 数组存储
3. 线程不安全

扩容
JDK 1.7 的时候，初始化数组的长度为10，后续如果超过10，则需要扩容，每次扩容为原来的1.5倍。
JDK 1.8 的时候，首次添加元素的时候，初始化长度为10的数组，后续如果超过10，则需要扩容，每次扩容为原来的1.5倍。

###### 二. Vector
1. 有序的、可以重复的数据
2. 底层使用了Object[] 数组存储
3. 线程安全的（效率低）

扩容
JDK 1.8 的时候，初始化数组的长度为10，后续如果超过10，则需要扩容，每次扩容为原来的2倍。
###### 三. LinkedList
1. 有序的、可以重复的数据
2. 底层使用了双向链表存储
3. 线程不安全

扩容
不需要扩容


###### 四. 开发选择
1. Vector基本不使用了
2. ArrayList底层使用数组结构，查找和添加（尾部添加）操作效率高，时间复杂度为O(1)
						 删除和插入操作效率低，时间复杂度为O( n)
   LinkedList底层使用的双向链表，删除和插入、还有尾部添加 效率高，时间复杂度是O(1)
						  查找的效率低，时间复杂度是O(n)
3. 在选择了ArrayList的前提下，new ArrayList（）； 和new ArrayList（int capacity）；怎么选择？
						 确定了ArrayList的长度，就选后者，省去了扩容的时间。 



#### Set
Set实现类都是1. 无序的、不可重复的

###### 一. HashSet 
特点：
2. 底层使用了HashMap，即数组+单向链表+红黑树 存储

###### 二. LinedHashSet
特点：
2. 是HashSet的子类，底层是LinedHashMap，即在数组+单向链表+红黑树 的基础上 又加了双向链表，用于记录添加元素的先后顺序


无序性到底是什么？
1. 无序性 != 随机性 
2. 无序性 也不是说 添加的顺序和 遍历的顺序不一致
3. 无序性 指的是元素添加的位置，不像List一样依次紧密排列

无可重复性
	添加到Set集合中的元素是不能相同的，比较的标准是判断hashCode()得到的哈希值 以及 equals（）方法得到
	boolean结果，所以一定要重写hashCode（）与equals（）方法。

###### 三. TreeSet
2. 底层是红黑树，可以按照添加的元素的指定的属性的大小进行排序和去重
3. 添加的元素一定要是同一类型，否则会报出ClassCastException
4. TreeSet的元素不可重复性不是通过 hashCode和equals方法了，而是通过①自然排序compareTo  或者②定制排序compare方法的返回值


#### Map
###### 一. HashMap
特点：
1. 底层数组+单向链表+红黑树 存储
2. 线程不安全，效率高
3. 可以添加null的key值和value值
4. HashMap中的所有key彼此之间是不可重复的，无序的，所有的key构成了一个Set集合，Key的元素需要重写hashCode和equals方法
	HashMap中所有的value彼此之间可以重复，无序的，所有的value构成了一个collection集合，value的元素需要重写equals方法。
	hashMap中所有的key-value构成了一个entry，彼此之间是不可重复的，无序的，构成一个set集合。

方法：
1. 增
	put(Object key, Object value)
	putAll(Map m)
1. 删
	remove(Object key)
2. 改
   put(Object key,Object value)
   putAll(Map m)
3. 查
   get(Object key) 
4. 遍历
	遍历key集：Set keySet()
	遍历value集：Collection values()
	遍历entry集：Set entrySet()
###### 二. LinedHashMap
特点：
1. 是HashMap的子类，底层是数组+单向链表+红黑树 的基础上 又加了双向链表，用于记录添加元素的先后顺序

###### 三. HashTable
1. 古老的实现类；线程安全，效率低
2. 不可以添加null的key值或者value值

###### 四. TreeMap
1. 底层是红黑树，可以按照添加的key-value中的key的指定的属性的大小进行排序和去重，需要考虑①自然排序 ②定制排序
2. 添加的元素的key一定要是同一类型，否则会报出ClassCastException

###### 五. Properties
1. 是Hashtable的子类，其key和value都是String类型，常用来处理属性文件。
```java
	File file = new File("info.properties");
	FileInputStream fis = new FileInputStream(file);
	Properties pro = new Properties();
	pro.load(fis);
	String name = pro.getProperty("name");
```


#### Collections工具类
类似于操作数组的工具类Arrays, Collections 是一个操作List、Set和Map等集合的工具类。
1. 排序操作
   reverse(List) 反转List集合中的元素顺序
   shuffle(List) 对List集合元素进行随机排序
   sort(List) 根据自然排序对List中的元素进行排序
   sort(List,Compartor) 根据定制排序Compartor对List中的元素进行排序。
   swap(List,int ,int)  对List集合中的i 和j的位置对应的元素进行交换

2. 查找
   Object max(Collection) 根据元素的自然排序，返回给定集合中的最大元素
   Object max(Collection,Comparator) 根据定制顺序Compartor，返回指定集合中的最大元素
   Object mix(Collection) 根据元素的自然排序，返回给定集合中的最小元素
   Object mix(Collection,Comparator) 根据定制顺序Compartor，返回指定集合中的最小元素
   int binarySearch(List list, T key) 在list集合中查找某个元素的下标
   int frequency(Collection ,Object) 返回Collection集合中某个元素出现的次数

3. 复制、替换
   void copy(List dest, List src) 将src复制到dest中
   boolean replaceAll(List list, Object oldValue, Object newValue) 
4. 添加
   boolean addAll(Collection c, T ... elements)
5. 同步
   synchronizedXxx() 将指定集合包装成线程同步的集合