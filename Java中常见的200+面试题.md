# 面试200+

![image-01](imgs/d5417d5c-5af0-46af-929c-4d42563c3ec6.png)

## Java基础

**1. JDK和JRE有什么区别?**

​	JDK : Java Development Kit 的简称 , Java开发包 , 提供了 Java 的开发环境和运行环境 . 

​	JRE : Java Runtime Environment 的简称 , Java 运行环境 , 为 Java 的运行环境提供了所需 . 具体来说 JDK 其实包含了 JRE , 同时还包含了编译 Java 源码的编译器 Javac , 还包含了很多 Java 程序测试和分析的工具 . 简单来说 : 如果你需要运行 Java程序 , 只需安装 JRE 就可以了 , 如果你需要编写 Java 程序 , 需要安装 JDK.

****

**2. == 和 equals 的区别是什么?**

​	== 解读 : 对于基本类型和引用类型 == 的作用效果是不同的 , 如下所示 : 

​	基本类型 : 比较的是值是否相同 ;

​	引用类型 : 比较的是引用是否相同 ;

​	代码示例:

```java
String x = "string";
String y = "string";
String z = new String("string");
System.out.println(x == y); // true
System.out.println(x == z); // false
System.out.println(x.equals(y)); // true
System.out.println(x.equals(z)); // true
```

​	代码解读 : 因为 x 和 y 指向的是同一个引用 , 所以 == 也是 true , 而 new String() 方法则重写开辟内存空间 , 所以 == 结果为 false , 而 equals 比较的一直是值 , 所以结果都为true.



​	equals 解读 : equals 本质上就是 == , 只不过 String 和 Integer 等重写了 equals 方法, 把它变成了值比较 . 看下面的代码就明白了.

​	首先来看默认情况下 equals 比较一个有相同值的对象 , 代码如下 : 

```java
class Cat {
    public Cat (String name) {
        this.name = name;
    }
    
    private String name;
    
    public String getName() {
        return name;
    }
    
    public void setName() {
        this.name = name;
    }
    
}

Cat c1 = new Cat("zs");
Cat c2 = new Cat("zs");
System.out.println(c1.equals(c2));  // false
// 输出结果出乎意料,是false,下面来看下equals源码:
public boolean equals(Object obj) {
    return (this == obj);
}
// equals 本质上就是 == ,那为什么两个相同值的String对象,返回的值是true
String s1 = new String("lisi");
String s2 = new String("lisi");
System.out.println(s1.equals(s2)); // true
// 当我们进入 String的equals 方法,找到了答案: 代码如下:
public boolean equals(Object anObject) {
    if (this == anObject) {
        return true;
    }
    if (this == anObject) {
        return true;
    }
    if (anObject instanceof String) {
        String anotherString = (String)anObject;
        int n = value.length;
        if (n == anotherString.value.length) {
            char v1[] = value;
            char v2[] = anotherString.value;
            int i = 0;
            while (n-- != 0) {
                if (v1[i] != v2[i]) {
                    return false;
                    i++;
                }
            }
            return true;
        }
    }
    return false;
}
```

​	原来是 String 重写了 Object 的 equals 方法 , 把引用比较改成了值比较.

​	总结 : == 对于基本类型来说是值比较 , 对于引用类型来说是比较的引用; 而equals 默认情况下是引用比较 , 只是很多类重写了 equals 方法 , 比如 String , Integer 等把它变成了值比较 , 所以一般情况下 equals 比较的是值是否相等

****

**3. 两个对象的 hashCode() 相同 , 则 equals() 也一定为 true , 对吗?**

​	不对 . 两个对象的 hashCode() 相同 , equals() 不一定 true

​	代码示例:

```java
String str1 = "通话";
String str2 = "重地";
System.out.println(String.format("str1: %d | str2: %d" , str1.hashCode(), str2.hashCode()));
System.out.println(str1.equals(str2));
// 执行的结果:
	str1:1179395 | str2: 1179395
    false
```

​	代码解读 : 很显然"通话"和"重地"的 hashCode() 相同 , 然而 equals() 则为 false , 因为在散列表中 , hashCode() 相等即两个键值对的哈希值相等,然而哈希值相等 , 并不一定能得出键值对相等

****

**4. final 在 Java 中有什么作用?**

​	final 修饰的类叫最终类 , 该类不能被继承

​	final 修饰的方法不能被重写

​	final 修饰的变量叫常量 , 常量必须初始化 , 初始化之后值就不能被修改

****

**5. Java中的 Math.round(-1.5) 等于多少?**

​	等于 -1 , Math.round 四舍五入大于 0.5 向上取整的

****

**6. String 属于基础的数据类型吗?**

​	String 不属于基础类型 , 基础类型有8种: `byte` , `boolean` , `char` , `short` , `int` , `float` , `long` , `double` , 而 String 属于对象

****

**7. Java中操作字符都有哪些类? 它们之间有什么区别?**

​	操作字符串的类有 : String , StringBuffer , StringBuilder

​	String 和 StringBuffer , StringBuilder 的区别在于 String 声明的是不可变的对象 , 每次操作都会生成新的 String 对象 , 然后将指针指向新的 String 对象 , 而 StringBuffer , StringBuilder 可以在原有对象的基础上进行操作 , 所以经常改变字符串内容的情况下最好不要使用 String.

​	StringBuffer 和 StringBuilder 最大的区别在于 , StringBuffer 是线程安全的 , 而 StringBuilder 是非线程安全的 , 但 StringBuilder 的性能却高于 StringBuffer , 所以在单线程环境下推荐使用StringBuilder , 多线程环境下推荐使用 StringBuffer.

****

**8. String str = "i" 与 String str = new String("i")一样吗?**

​	不一样 , 因为内存分配方式不一样 , String str = "i" 的方式 , Java 虚拟机会将其分配到常量池中; 而String str = new String("i") 则会被分到堆内存中.

****

**9. 如何将字符串反转?**

​	使用 StringBuilder 或者 StringBuffer 的 reverse() 方法

​	实例代码:

```java
// StringBuffer reverse
StringBuffer stringBuffer = new StringBuffer();
stringBuffer.append("abcd");
System.out.println(stringBuffer.reverse());
// StringBuilder reverse
StringBuilder stringBuilder = new StringBuilder();
stringBuilder.append("abcd");
System.out.println(stringBuilder.reverse());
```

****

**10. String 类的常用方法都有哪些?**

​	`indexOf()` 返回指定字符的索引 , 

​	`charAt()` 返回指定索引处的字符 , 

​	`replace()` 字符串替换 , 

​	`trim()` 去除字符串两端空白 , 

​	`split()` 分割字符串 , 返回一个分割后的字符串数组 , 

​	`getBytes()` 返回字符串的 byte 类型数组 , 

​	`length()` 返回字符串长度 , 

​	`toLowerCase()` 将字符串转成小写字母 ,

​	`toUpperCase()` 将字符串转成大写字符 ,

​	`substring()` 截取字符串 ,

​	`equals()` 字符串比较

****

**11. 抽象类必须要有抽象方法吗?**

​	不需要 , 抽象类不一定非要有抽象方法

​	示例代码 :

```java
abstract class Cat {
    public static void sayHi() {
        System.out.println("hi~");
    }
}
// 上面代码 , 抽象类并没有抽象方法但完全能正常运行
```

****

**12. 普通类和抽象类都有哪些区别?**

​	普通类不能包含抽象方法 , 抽象类可以包含抽象方法

​	抽象类不能直接实例化 , 普通类可以直接实例化

****

**13. 抽象类能使用 final 修饰吗?**

​	不能 , 定义抽象类就是让其他类继承的 , 如果定义为 final 该类就不能被继承 , 这样彼此就会产生矛盾 , 所以 final 不能修饰抽象类.

****

**14. 接口和抽象类有什么区别?**

​	默认方法实现 : 抽象类型可以有默认的方法实现 ; 接口不能有默认的方法实现

​	实现 : 抽象类的子类使用 extends 来继承 ; 接口必须使用 implements 来实现接口

​	构造函数 : 抽象类可以有构造函数 ; 接口不能有

​	main方法 : 抽象类可以有main方法 , 并且我们能运行它 , 接口不能有main方法

​	实现数量 : 类可以实现很多个接口 , 但是只能继承一个抽象类

​	访问修饰符 : 接口中的方法默认使用 public 修饰 ; 抽象类中的方法可以是任意访问修饰符

****

**15. Java 中 IO 流分为几种**

​	按流向分 : 输入流(Input Stream) , 输出流(Output Stream)

​	按功能来分: 节点流 , 处理流(包装流)

​	按处理数据单位来分 : 字节流和字符流

​	字节流和字符流的区别是 : 字节流按8位传输以字节为单位输入输出数据 , 字符流按16位传输以字符为单位输入输出数据

****

**16. BIO , NIO , AIO 有什么区别**

​	BIO : Block IO 同步阻塞式 IO , 就是我们平常使用的传统 IO , 它的特点是模式简单使用方便 , 并发处理能力低

​	NIO : New IO 同步非阻塞 IO , 是传统 IO 的升级 , 客户端和服务器端通过 Channel(通道)通讯 , 实现了多路复用

​	AIO : Asynchronous IO 是 NIO 的升级 , 也叫 NIO2 , 实现了异步非堵塞 IO , 异步 IO 的操作基于事件和回调机制

****

**17. FIles的常用方法都有哪些**

​	`Files.exists()` : 检测文件路径是否存在

​	`Files.createFile()` : 创建文件

​	`Files.createDirectory()` : 创建文件夹

​	`Files.delete()` : 删除一个文件或目录

​	`Files.copy()` : 复制文件

​	`Files.move()` : 移动文件

​	`Files.size()` : 查看文件个数

​	`Files.read()` : 读取文件

​	`Files.write()` : 写入文件

****

**18. Java容器都有哪些**

​	Java 容器分为 Collection 和 Map 两大类 , 其下又有很多子类 , 如下:

​	`Collection` , `List` , `ArrayList` , `LInkedList` , `Vector` , `Stack` , `Set` , `HashSet` , 	`LInkedHashSet` , `TreeSet` , `Map` , `HashMap` , `LInkedHashMap` , `TreeMap` , `ConcurrentHashMap` , `Hashtable`

****

**19. Collection 和 Collections 有什么区别**

​	Collection 是一个集合接口 , 它提供了对集合对象进行基本操作的通用接口方法 , 所有集合都是它的子集 , 比如 List , Set 等

​	Collections 是一个包装类 , 包含了很多静态方法 , 不能被实例化 , 就像一个工具类 , 比如提供的排序方法 : Collections.sort(list)

****

**20. List , Set , Map 的区别**

​	List , Set , Map 的区别主要体现在两个方面 : 元素是否有序 , 是否允许元素重复

****

**21. HashMap 和 HashTable 有什么区别?**

​	存储: HashMap 运行 key 和 value 为 null , 而 Hashtable 不允许

​	线程安全 : Hashtable 是线程安全的 , 而 HashMap 是非线程安全的

​	推荐使用 : 在 Hashtable 的类注释可以看到 , Hashtable 是保留类不建议使用 , 推荐在单线程环境下使用 HashMap 替代 , 如果需要多线程使用则用 ConcurrentHashMap 替代

****

**22. 如何决定使用 HashMap 还是 TreeMap?**

​	对于在 Map 中插入 , 删除 , 定位一个元素这类操作 , HashMap 是最好的选择 , 因为相对而言 HashMap 的插入会更快 , 但如果你要对一个 key 集合进行有序的遍历 , 那 TreeMap 是更好的选择

****

**23. 说一下 HashMap 的实现原理?**		

​	HashMap 基于 Hash 算法实现的 , 我们通过 put(key,value)存储 , get(key)来获取 , 当传入 key 时 , HashMap 会根据 key , hashCode() 计算出 hash 值 , 根据 hash 值将 value 保存在 bucket 里 , 当计算出的 hash 值相同时 , 我们称之为 hash 冲突 , HashMap 的做法使用链表和红黑树存储相同 hash 值的value , 当 hash 冲突的个数比较少时 , 使用链表否则使用红黑树

****

**24. 说一下 HashSet 的实现原理?**

​	HashSet 是基于 HashMap 实现的 , HashSet 底层使用 HashMap 来保存所有元素 , 因此 HashSet 的实现比较简单 , 相关 HashSet 的操作 , 基本上都是直接调用底层 HashMap 的相关方法来完成 , HashSet 不允许重复的值

****

**25. ArrayList 和 LInkedList 的区别是什么?**

​	数据结构实现: ArrayList 是动态数组的数据结构实现 , 而 LinkedList 是双向链表的数据结构实现.

​	随机访问效率: ArrayList 比 LinkedList 在随机访问的时候效率要高 , 因为 LinkedList 是线性的数据存储方式 , 所以需要移动指针从前往后依次查找

​	增加和删除效率: 在非首尾的增加和删除操作 , LinkedList 要比 ArrayList 效率要高 , 因为 ArrayList 增删操作要影响数组内的其他数据的下标

​	综合来说 , 在需要频繁读取集合中的元素时 , 更推荐使用 ArrayList , 而在插入和删除操作较多时 , 更推荐使用 LinkedList

****

**26. 如何实现数组和 List 之间的转换?**

​	数组转 List: 使用 Arrays.asList(array) 进行转换.

​	List 转数组: 使用 List 自带的 toArray() 方法

​	代码示例:

```java
// list to array
List list = new ArrayList();
list.add("xk");
list.add("的微信");
list.toArray();
// array to list
String[] array = new String[]{"xk","的微信"};
Arrays.asList(array);
```

****

**27. ArrayList 和 Vector 的区别是什么?**

​	线程安全: Vector 使用了 Synchronized 来实现线程同步 , 是线程安全的 , 而ArrayList 是非线程安全的

​	性能: ArrayList 在性能方面要优于 Vector

​	扩容: ArrayList 和 Vector 都会根据实际的需要动态的调整容量 , 只不过在 Vector 扩容每次会增加 1 倍 , 而 ArrayList 只会增加 50%

****

**28. Array 和 ArrayList 有何区别?**

​	Array 可以存储基本数据类型和对象 , ArrayList 只能存储对象

​	Array 是指定固定大小的, 而 ArrayList 大小是自动扩展的

​	Array 内置方法没有 ArrayList 多 , 比如 addAll , removeAll , iteration 等方法只有 ArrayList 有

****

**29. 在 Queue 中 poll() 和 remove() 有什么区别?**

​	相同点: 都是返回第一个元素 , 并在队列中删除返回的对象

​	不同点: 如果没有元素 poll() 会返回 null , 而 remove() 会直接抛出 NoSuchElementException 异常

​	代码示例:

```java
Queue queue = new LinkedList();
queue.offer("string"); //add
System.out.println(queue.poll());
System.out.println(queue.remove);
System.out.println(queue.size());
```

****

**30. 哪些集合类是线程安全的?**

​	Vector , Hashtable , Stack 都是线程安全的 , 而像 HashMap 则是非线程安全的 , 不过在 JDK 1.5 之后随着 Java.util.concurrent 并发包的出现 , 它们也有了自己对应的线程安全类 , 比如 HashMap 对应的线程安全类就是 ConcurrentHashMap

****

**31. 迭代器 Iterator 是什么?**

​	Iterator 接口提供遍历任何 Collection 的接口 . 我们可以从一个 Collection 中使用迭代器方法来获取迭代器实例 , 迭代器取代了 Java 集合框架中的 Enumeration , 迭代器允许调用者在迭代过程中移除元素

****

**32. Iterator 怎么使用? 有什么特点?**

​	Iterator 使用代码如下:

```java
List list = new ArrayList<>();
Iterator it = list.iterator();
while(it.hasNext()){
    String obj = it.next();
    System.out.println(obj);
}
```

​	Iterator 的特点是更加安全 , 因为它可以确保 , 在当前遍历的集合元素被更改的时候 , 就会抛出 ConcurrentModificationException 异常

****

**33. Iterator 和 ListIterator 有什么区别?**

​	Iterator 可以遍历 Set 和 List 集合 , 而 ListIterator 只能遍历 List

​	Iterator 只能单向遍历，而 ListIterator 可以双向遍历（向前/后遍历）。

​	ListIterator 从 Iterator 接口继承 , 然后添加了一些额外的功能 , 比如添加一个元素 , 替换一个元素 , 获取前面或后面元素的索引位置

****

**34. 怎么确保一个集合不能被修改?**

​	可以使用 Collections.unmodifiableCollection(Collection c) 方法来创建一个只读集合 , 这样改变集合的任何操作都会抛出 Java.lang.UnsupportedOperationException 异常

​	实例代码如下:

```java
List list = new ArrayList<>();
list.add("x");
Collection clist = Collections.unmodifiableCollection(list);
clist.add("y"); // 运行时此行报错
System.out.println(list.size());
```

****

## 多线程

**35. 并行和并发有什么区别?**

​	并行 : 多个任务在同一个 CPU 核上 , 按细分的时间片轮流(交替)执行 , 从逻辑上来看那些任务是同时执行.

​	并发 : 多个处理器或多核处理器同时处理多个任务

****

**36. 线程和进程的区别?**

​	一个程序下至少有一个进程 , 一个进程下至少有一个进程 , 一个进程下也可以有多个线程来增加程序的执行速度

****

**37. 守护线程是什么?**

​	守护线程是运行在后台的一种特殊进程 , 它独立于控制终端并且周期性地执行某种任务或等待处理某些发生的事件 , 在 Java 中垃圾回收线程就是特殊的守护线程

****

**38. 创建线程有哪几种方式?**

​	创建线程有三种方式 : 

​		继承 Thread 重写 run() 方法 ;

​		实现 Runnable 接口 ;

​		实现 Callable 接口 .

**39. 说一下 Runnable 和 Callable 有什么区别?**

 Runnable 没有返回值 , Callable 可以拿到有返回值 , Callable 可以看作是 Runnable 的补充

****

**40. 线程有哪些状态?**

​	`NEW  尚未启动`   ,   `RUNNABLE  正在执行中`   ,   `BLOCKED  阻塞的(被同步锁或者IO锁阻塞)`   ,   `WAITING  永久等待状态`   ,   `TIMED_WAITING  等待指定的时间重新被唤醒的状态`    ,    `TERMINATED 执行完成`

****

**41. sleep() 和 wait() 有什么区别?**

​	类的不同 : sleep() 来自 Thread , wait() 来自 Object

​	释放锁 : sleep() 不释放锁 ; wait() 释放锁

​	用法不同 : sleep() 时间到会自动恢复 ; wait() 可以使用 notify() / notifyAll() 直接唤醒

****

**42. notify() 和 notifyAll() 有什么区别?**

​	notifyAll() 会唤醒所有的线程 , notify() 之后唤醒一个线程 , notifyAll() 调用后 , 会将全部线程由等待池移到锁池 , 然后参与锁的竞争 , 竞争成功则继续执行 , 如果不成功则留在锁池等待锁被释放后再次参与竞争 , 而 notify() 只会唤醒一个线程 , 具体唤醒哪一个线程由虚拟机控制

****

**43. 线程的 run() 和 start() 有什么区别?**

​	start() 方法用于启动线程 , run() 方法用于执行线程的运行时代码 , run() 可以重复调用 , 而 start() 只能调用一次

****

**44. 创建线程池有哪几种方式?**

​	线程池创建有七种方式 , 最核心的是最后一种 :

​		`newSingleThreadExecutor():`它的特点在于工作线程数目被限制为 1 , 操作一个无界的工作队列 , 所以它保证了所有任务的都是被顺序执行 , 最多会有一个任务处于活动状态 , 并且不允许使用者改动线程池实例 , 因此可以避免其改变线程数目;

​		`newCachedThreadPool():`它是一种用来处理大量短时间工作任务的线程池 , 具有几个鲜明特点 : 它会试图缓存线程并重用 , 当无缓存线程可用时 , 就会创造新的工作线程 ; 如果线程闲置的时间超过 60 秒 , 则被终止并移出缓存 ; 长时间闲置时 , 这种线程池 , 不会消耗什么资源 , 其内部使用 SynchronousQueue 作为工作队列

​		`newFixedThreadPool(int nThreads):`重用指定数目(nThreads)的线程 , 其背后使用的是无界的工作队列 , 任何时候最多有 nThreads 个工作线程是活动 , 这意味着 , 如果任务数量超过了活动队列数目 , 将在工作队列中等待空闲线程出现 ; 如果有工作线程退出 , 将会有新的工作线程被创建 , 以补足指定的数目 nThread;

​		`newSingleThreadScheduledExecutor():`创建单线程池 , 返回 ScheduledExecutorService , 可以进行定时或周期性的工作调度;

​		`newScheduledThreadPool(int corePoolSize)`: 和 newSingleThreadScheduledExecutor() 类似 , 创建的是个 ScheduledExecutorService , 可以进行定时或周期性的工作调度 , 区别在于单一工作线程还是多个工作线程;

​		`newWorkStealingPool(int parallelism)`: 这是一个经常被人忽略的线程池，Java 8 才加入这个创建方法，其内部会构建 ForkJoinPool，利用 Work-Stealing 算法，并行地处理任务，不保证处理顺序；

​	`ThreadPoolExecutor()`: 是最原始的线程池创建，上面 1-3 创建方式都是对ThreadPoolExecutor 的封装。

****

**45. 线程池都有哪些状态?**

​	`RUNNING : `这是最正常的状态 , 接收新的任务 , 处理等待队列中的任务

​	`SHUTDOWN : `不接受新的任务提交 , 但是会继续处理等待队列中的任务

​	`STOP : `不接受新的任务提交 , 不再处理等待队列中的任务 , 中断正在执行任务的线程

​	`TIDYING : `所有的任务都销毁了 , workCount 为 0 , 线程池的状态在转换为`TIDYING`状态时 , 会执行钩子方法 `terminated()`

​	`TERMINATED : ` `terminated()`方法结束后 , 线程池的状态就会变成这个

****

**46. 线程池中 submit() 和 execute() 方法有什么区别?**

​	execute() : 只能执行 Runnable 类型的任务

​	submit() : 可以执行 Runnable 和 Callable 类型的任务

​	Callable 类型的任务可以获取执行的返回值 , 而 Runnable 执行无返回值

****

**47. 在 Java 程序中怎么保证多线程的运行安全?**

​	方法一 : 使用安全类 , 比如 Java.util.concurrent 下的类

​	方法二 : 使用自动锁 synchronized

​	方法三 : 使用手动锁 Lock

​	手动锁 Java 实例代码如下:

```java
Lock lock = new ReentrantLock();
lock.lock();
try{
    System.out.println("获得锁");
} catch (Exception e) {
    // TODO: handle exception
} finally {
    System.out.println("释放锁");
    lock.unlock();
}
```

****

**48. 多线程中 synchronized 锁升级的原理是什么?**

​	synchronized 锁升级原理: 在锁对象的对象头里面有一个 threadid 字段 , 在第一次访问的时候 threadid 为空 , jvm 让其持有偏向锁 , 并将 threadid 设置为其线程 id , 再次进入的时候会先判断 threadid 是否与其线程 id 一致 , 如果一致则可以直接使用此对象 , 如果不一致 , 则升级偏向锁为轻量级锁 , 通过自旋循环一定次数来获取锁 , 执行一次次数之后 , 如果还没有正常获取到要使用的对象 , 此时就是把锁从轻量级升级为重量级锁 , 此过程就构成 synchronized 锁的升级

​	锁的升级的目的: 锁升级是为了降低了锁带来的性能消耗 , 在 Java 6 之后优化 synchronized 的实现方式 , 使用了偏向锁升级为轻量级锁再升级到重量级锁的方式 , 从而降低了锁带来的性能消耗.

****

**49. 什么是死锁?**

​	当线程 A 持有独占锁 a , 并尝试去获取独占锁 b 的同时 , 线程 B 持有独占锁 b , 并尝试获取独占锁 a 的情况下 , 就会发生 AB 两个线程由于互相持有对方需要的锁 , 而发生的阻塞现象 , 我们称为死锁

****

**50. 怎么防止死锁?**

​	尽量使用 tryLock(long timeout, TimeUnit unit)的方法(ReentrantLock , ReentrantReadWriteLock) , 设置超时时间 , 超时可以退出防止死锁.

​	尽量使用 Java.util.concurrent 并发类代替自己手写锁

​	尽量降低锁的使用粒度 , 尽量不要几个功能用同一把锁

​	尽量减少同步的代码块

****

**51. ThreadLocal 是什么? 有哪些使用场景?**

​	ThreadLocal 为每个使用该变量的线程提供独立的变量副本 , 所以每一个线程都可以独立地改变自己的副本 , 而不会影响其他线程所对应的副本

​	ThreadLocal 的经典使用场景是数据库连接和 session 管理等

****

**52. 说一下 synchronized 底层实现原理?**

​	synchronized 是由一对 `monitorenter/monitorexit` 指令实现的 , monitor 对象是同步的基本实现单元 , 在 Java 6 之前 , monitor 的实现完全是依靠操作系统内部的互斥锁 , 因为需要进行用户态到内核态的切换 , 所以同步操作是一个无差别的重量级操作 , 性能也很低 , 但在 Java 6 的时候 , Java 虚拟机 对此进行了大刀阔斧地改进 , 提供了三种不同的 monitor 实现 , 也就是常说的三种不同的锁 : 偏向锁(Biased Locking) , 轻量级锁和重量级锁 , 大大改进了其性能

****

**53. synchronized 和 volatile 的区别是什么?**

​	`volatile`是变量修饰符 ; synchronized 是修饰类 , 方法 , 代码段

​	`volatile`仅能实现变量的修改可见性 , 不能保证原子性 ; 而 synchronized 则可以保证变量的修改可见性和原子性

​	`volatile`不会造成线程的阻塞 ; synchronized 可能会造成线程的阻塞

****

**54. synchronized 和 Lock 有什么区别?** 

​	`synchronized` 可以给类 , 方法 , 代码块加锁 ; 而 lock 只能给代码块加锁

​	`synchronized`不需要手动获取锁和释放锁 , 使用简单 , 发生异常会自动释放锁 , 不会造成死锁;而 lock需要自己加锁和释放锁 , 如果使用不当没有 unlock() 去释放锁就会造成死锁

​	通过 Lock 可以知道有没有成功获取锁 , 而 synchronized 却无法办到.

****

**55. synchronized 和 ReentrantLock 区别是什么?**

​	synchronized 早期的实现比较低效 , 对比 ReentrantLock , 大多数场景性能都相差较大 , 但是在 Java 6 中对 synchronized 进行了非常多的改进

​	主要区别如下 : 

​		`ReentrantLock`使用起来比较灵活 , 但是必须有释放锁的配合动作;

​		`ReentrantLock`必须手动获取与释放锁 , 而 `synchronized` 不需要手动释放和开启锁;

​		`ReentrantLock`只适用于代码块锁 , 而 `synchronized` 可用于修饰方法 , 代码块等

​		`volatile` 标记的变量不会被编译器优化 ; `synchronized` 标记的变量可以被编译器优化

****

**56. 说一下 atomic 的原理?**

​	`atomic` 主要利用 CAS (Compare And Swap) 和 `volatile`和`native`方法来保证原子操作 , 从而避免 synchronized 的高开销 , 执行效率大为提升

****

## 反射

**57. 什么是反射?**

​	反射是在运行状态中 , 对于任意一个类 , 都能知道这个类的所有属性和方法 ; 对于任意一个对象 , 都能够调用它的任意一个方法和属性 ; 这种动态获取的信息以及动态调用对象的方法的功能称为 Java 语言的反射机制

**58. 什么是 Java 序列化?什么情况下需要序列化?**

​	`Java 序列化是为了保存各种对象在内存中的状态 , 并且可以把保存的对象状态再读出来`

​	以下情况需要使用 Java 序列化:

​		想把内存中的对象状态保存到一个文件中或者数据中的时候;

​		想用套接字在网络上传送对象的时候;

​		想通过 `RMI(远程方法调用)`传输对象的时候

****

**59. 动态代理是什么?有哪些应用?**

​	动态代理是运行时动态生成代理类

​	动态代理的应用有 `Spring AOP` , `Hibernate` 数据查询 , 测试框架的后端 mock , rpc , Java 注解对象获取等

****

**60. 怎么实现动态代理?**

​	JDK 原生动态代理和 cglib 动态代理 , JDK 原生动态代理是基于接口实现的 , 而 cglib 是基于继承当前类的子类实现的

****

## 对象拷贝

**61. 为什么要使用克隆?**

​	克隆的对象可能包含一些已经修改过的属性 , 而 new 出来的对象的属性都还是初始化时候的值 , 所以当一个新的对象来保存当前对象的"状态"就靠克隆方法了

**62. 如何实现对象克隆?**

​	实现 Cloneable 接口并重写 Object 类中的 clone() 方法

​	实现 Serializable 接口 , 通过对象的序列化和反序化实现克隆 , 可以实现真正的深度克隆

**63. 深拷贝和浅拷贝区别是什么?**

​	浅克隆: 当对象被复制时只复制它本身和其中包含的值类型的成员变量 , 而引用类型的成员对象并没有复制

​	深克隆: 除了对象本身被复制外 , 对象所包含的所有成员变量也将复制

****

**64. JSP 和 servlet 有什么区别?**

​	JSP 是 servlet 技术的扩展 , 本质上就是 servlet 的简易方式 , servlet 和 JSP 最主要的不同点在于 , servlet 的应用逻辑是在 Java 文件中 , 并且完全从表示层中的 html 里分离开来 , 而 JSP 的情况是 Java 和 html 可以组合成一个扩展名为 JSP 的文件 , JSP 侧重于视图 , servlet 主要用于控制逻辑

****

**65. JSP 有哪些内置对象? 作用分别是什么?**

​	JSP 有 8 大内置对象:

​		`request :` 封装客户端的请求 , 其中包含来自 `get`或`post`请求的参数

​		`response :` 封装服务器对客户端的响应;

​		`pageContext :` 通过该对象可以获取其他对象

​		`session :` 封装用户会话的对象

​		`application :` 封装服务器运行环境的对象;

​		`out :` 输出服务器响应的输出流对象

​		`config :`Web 应用的配置对象;

​		`page :` JSP 页面本身(相当于 Java 程序中的 this)

​		`exception :`封装页面抛出异常的对象

****

**66. 说一下 JSP 的 4 种作用域?**

​	`page :`代表与一个页面相关的对象和属性

​	`request :`代表与客户端发出的一个请求相关的对象和属性 , 一个请求可能跨越多个页面 , 涉及多个 Web 组件 , 需要在页面显示的临时数据可以置于此作用域

​	`session :`代表与某个用户与服务器建立的一次会话相关的对象和属性 , 跟某个用户相关的数据应该放在用户自己的 `session`中

​	`application :`代表与整个 Web 应用程序相关的对象和属性 , 它实质上是跨越整个 Web 应用程序 , 包括多个页面 , 请求和会话的一个全局作用域

****

**67. session 和 cookie 有什么区别?**

​	存储位置不同: session 存储在服务器端 ; cookie 存储在浏览器端

​	安全性不同: cookie 安全性一般 , 在浏览器存储 , 可以被伪造和修改

​	容量和个数限制: cookie 有容量限制 , 每个站点下的 cookie 也有个数限制

​	存储的多样性: session 可以存储在 Redis 中 , 数据库中 , 应用程序中; 而 cookie 只能存储在浏览器中

****

**68. 说一下 session 的工作原理?**

​	session 的工作原理是客户端登录完成之后 , 服务器会创建对应的 session , session 创建完之后 , 会把 session 的 id 发送给客户端 , 客户端再存储到浏览器中 , 这样客户端每次访问服务器时 , 都会带着 session 的 id 发送给客户端 , 客户端再存储浏览器中 , 这样客户端每次访问服务器时, 都会带着 sessionid , 服务器拿到 sessionid 之后 , 在内存找到与之对应的 session 这样就可以正常工作了

****

**69. 如果客户端禁止 cookie 能实现 session 还能用吗?**

​	可以用 , session 只是依赖 cookie 存储 sessionid , 如果 cookie 被禁用了 , 可以使用 url 中添加 sessionid 的方式保证 session 能正常使用

****

**70. Spring MVC 和 Struts2 的区别是什么?**

​	`拦截级别:` struts2 是类级别的拦截; spring mvc 是方法级别的拦截

​	`数据独立性:` spring mvc 的方法之间基本独立的 , 方法之间不共享变量 , 而 struts2 虽然方法之间也是独立的 , 但其所有 action 变量是共享的 , 这不会影响程序运行 , 却给我们编码和读程序时带来了一定的麻烦

​	`拦截机制:`struts2 有以自己的 interceptor 机制 , spring mvc 用的是独立的 aop 方式 , 这样导致 struts2 的配置文件量比 spring mvc 大

​	`对 ajax的支持:`spring mvc 集成了 ajax , 所有 ajax 使用很方便 , 只需要一个注解 @ResponseBody 就可以实现了 ; 而 struts2 一般需要安装插件或者自己写代码才行

****

**71. 如何避免 SQL 注入?**

​	使用预处理 PreparedStatement

​	使用正则表达式过滤掉字符中的特殊字符

****

**72. 什么是 XSS 攻击 , 如何避免?**

​	XSS 攻击: 即跨站脚本 , 它是 Web 程序中常见的漏洞 , 原理是攻击者往 Web 页面里插入恶意的脚本代码(css 代码 , JavaScript 代码等) , 当用户浏览该页面时 , 嵌入其中的脚步代码会被执行 , 从而达到恶意攻击用户的目的 , 如盗取用户 cookie , 破坏页面结构 , 重定向到其他网站等

​	预防 XSS 的核心是必须对输入的数据做过滤页面

****

**73. 什么是 CSRF 攻击 , 如何避免?**

​	CSRF: Cross-Site Request Forgery (中文: 跨站请求伪造) , 可以理解为攻击者盗用了你的身份 , 以你的名义发送恶意请求 , 比如: 以你的名义发送邮件 , 发消息 , 购买商品 . 虚拟货币转账等

​	防御手段:

​		验证请求来源地址;

​		关键操作添加验证码;

​		在请求地址添加 token 并验证

## 异常

**74. throw 和 throws 的区别?**

​	`throw` : 是真实抛出一个异常

​	`throws` : 是声明可能会抛出一个异常

****

**75. final , finally , finalize 有什么区别?**

​	`final` : 是修饰符 , 如果修饰类 , 此类不能被继承 ; 如果修饰方法和变量 , 则表示此方法和此变量不能在被改变 , 只能使用

​	`finally` : 是 try{} catch{} finally{} 最后一部分 , 表示不论发生任何情况都会执行 , finally 部分可以省略 , 但如果 finally 部分存在 , 则一定会执行 finally 里面的代码

​	`finalize` : 是 Object 类的一个方法 , 在垃圾收集器执行的时候会被调用被回收对象的此方法

****

**76. try-catch-finally 中哪个部分可以省略?**

​	try-catch-finally 其中 catch 和 finally 都可以被省略 , 但是不能同时省略 , 也就是说有 try 的时候 , 必须后面跟一个 catch 或者 finally

****

**77. try-catch-finally 中 , 如果 catch 中 return 了 , finally 还会执行吗?**

​	finally 一定会执行 , 即使是 catch 中 return 了 , catch 中的 return 会等 finally 中的代码执行完之后 , 才会执行

****

**78. 常见的异常类有哪些?**

​	`NullPointerException 空指针异常`

​	`ClassNotFoundException 指定类不存在`

​	`NumberFormatException 字符串转换为数字异常`

​	`IndexOutOfBoundException 数组下标越界异常`

​	`ClassCastException 数据类型转换异常`

​	`FileNotFoundException 文件未找到异常`

​	`NoSuchMethodException 方法不存在异常`

​	`IOException IO异常`

​	`SocketException Socket异常`

## 网络

**79. http 响应码 301 和 302 代表的是什么? 有什么区别?**

​	301: 永久重定向

​	302: 暂时重定向

​	它们的区别是 , 301 对搜索引擎优化 (SEO) 更加有利 ; 302 有被提示为网络拦截的风险

****

**80. forward 和 redirect 的区别?**

​	注 : `(forward是javax.servlet.RequestDispatcher接口中的一个方法，用于将一个 HTTP 请求从一个 Servlet 或 JSP 页面转发到另一个资源，如另一个 Servlet、JSP 页面或 HTML 文件等。)`

​	forward 是转发 和 redirect 是重定向:

​		地址栏 url 显示 : forward url 不会发生改变 , redirect url 会发生改变;

​		数据共享 : forward 可以共享 request 里的数据 , redirect 不能共享;

​		效率 : forward 比 redirect 效率高

****

**81. 简述 tcp 和 udp 的区别?**

​	tcp 和 udp 是 OSI 模型中的运输层中的协议 . tcp 提供可靠的通信传输 , 而 udp 则常被用于让广播和细节控制交给应用的通信传输

​	两者的区别大致如下：

​		tcp 面向连接，udp 面向非连接即发送数据前不需要建立链接；

​		tcp 提供可靠的服务（数据传输），udp 无法保证；

​		tcp 面向字节流，udp 面向报文；

​		tcp 数据传输慢，udp 数据传输快；

****

**82. tcp 为什么要三次握手 , 两次不行吗? 为什么?**

​		如果采用两次握手，那么只要服务器发出确认数据包就会建立连接，但由于客户端此时并未响应服务器端的请求，那此时服务器端就会一直在等待客户端，这样服务器端就白白浪费了一定的资源。若采用三次握

手，服务器端没有收到来自客户端的再此确认，则就会知道客户端并没有要求建立请求，就不会浪费服务

器的资源。

*****

**83. 说一下 tcp 粘包是怎么产生的？**

​	tcp 粘包可能发生在发送端或者接收端，分别来看两端各种产生粘包的原因：

​		发送端粘包：发送端需要等缓冲区满才发送出去，造成粘包；

​		接收方粘包：接收方不及时接收缓冲区的包，造成多个包接收。

****

**84. OSI 的七层模型都有哪些？**

​	物理层：利用传输介质为数据链路层提供物理连接，实现比特流的透明传输。

​	数据链路层：负责建立和管理节点间的链路。

​	网络层：通过路由选择算法，为报文或分组通过通信子网选择最适当的路径。

​	传输层：向用户提供可靠的端到端的差错和流量控制，保证报文的正确传输。

​	会话层：向两个实体的表示层提供建立和使用连接的方法。

​	表示层：处理用户信息的表示问题，如编码、数据格式转换和加密解密等。

​	应用层：直接向用户提供服务，完成用户希望在网络上完成的各种工作。

****

**85. get 和 post 请求有哪些区别?**

​	get 请求会被浏览器主动缓存 , 而 post 不会

​	get 传递参数有大小限制 , 而 post 没有

​	post 参数传输更安全 , get 的参数会明文限制在 url 上 , post 不会

****

**86. 如何实现跨域?**

​	实现跨域有以下几种方案 : 

​		服务器端运行跨域 设置 CORS 等于 *;

​		在单个接口使用注解 @CrossOrigin 运行跨域;

​		使用 jsonp 跨域;

****

**87. 说一下 JSONP 实现原理？**

​	jsonp：JSON with Padding，它是利用 script 标签的 src 连接可以访问不同源的特性，加载远程返回的 “JS 函数” 来执行的。

## 设计模式

**88. 说一下你熟悉的设计模式?**

​	单例模式 : 保证被创建一次 , 节省系统开销

​	工厂模式(简单工厂 , 抽象工厂) : 解耦代码

​	观察者模式 : 定义了对象之间的一对多的依赖 , 这样一来 , 当一个对象改变时 , 它的所有依赖者都会收到通知并自动更新

​	外观模式 : 提供一个统一的接口 , 用来访问子系统中的一群接口 , 外观定义了一个高层的接口 , 让子系统更容易使用 

​	模版方法模式 : 定义了一个算法的骨甲 , 而将一些步骤延迟到子类中 , 模版方法使得子类可以在不改变算法结构的情况下 , 重新定义算法的步骤

​	状态模式 : 允许对象在内部状态改变时改变它的行为 , 对象看起来好像修改了它的类
