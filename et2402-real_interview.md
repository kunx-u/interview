>源自真实的面试题, 童叟无欺~

>整理时间: 2024-08
>
>整理人: 庭风
>
>感谢: ET2402的全体童鞋
>
>最后更新自: 24/08/28
>
>※ 暂时还没有答案~



1. 常见的集合?

   List集合(ArrayList , LinkedList , Vector) 

   Set集合(HashSet , TreeSet , LinkedHashSet)

   Map集合(HashMap , TreeMap , Hashtable ConcurrentHashMap)

2. HashMap的数据结构?

   HashMap 的数据结构主要是数组 + 链表 + 红黑树

   - HashMap 在存储数据时，首先通过哈希函数计算键的哈希值，然后根据哈希值和数组长度进行位运算，得到键值对应该存储的桶索引。如果桶中没有其他元素，直接将键值对存储在该桶中；如果发生了哈希冲突，则将键值对以链表或红黑树的形式存储在桶中。
   - 在查找数据时，同样先计算键的哈希值和桶索引，然后在对应的桶中查找键值对。如果是链表结构，就遍历链表查找；如果是红黑树结构，就按照红黑树的查找算法进行查找。
   - 在删除数据时，也是先定位到要删除的键值对所在的桶，然后根据桶中的存储结构进行相应的删除操作。如果是链表，就遍历链表找到要删除的节点并删除；如果是红黑树，就按照红黑树的删除算法进行删除。

3. 往Map里面put元素的流程?

   首先调用键对象的hashCode() 方法来获取键的哈希值 , 得到键的哈希值后 , 会通过一种特定的算法来确定该键值对应该存储在数组的哪个桶中 , 确定桶位置后 , 需要检查该桶中是否已经存在其他键值对 , 这就是检查是否存在哈希冲突 . 如果桶为空 , 说明没有哈希冲突 , 直接将新的键值对放入该桶即可 , (链表形式)如果桶中已经存在键值对，说明发生了哈希冲突 , (红黑树形式)当链表长度达到 8 时，链表会被转换为红黑树结构，新的键值对会按照红黑树的插入规则插入到红黑树中。在插入新键值对时 , 还需要检查桶中是否已经存在与要插入的件相同的键 , 如果存在相同的键，那么就会用新的值替换原来的值。如果不存在相同的键，那么就是新增一个键值对到`Map`中。

   ​		(扩容机制)

   - 在插入键值对后，`HashMap`会检查当前的元素个数是否超过了阈值。阈值的计算方式是数组长度乘以负载因子，默认负载因子是 0.75。
   - 如果超过了阈值，就会触发扩容操作。扩容操作会创建一个新的更大的数组，然后将原数组中的所有键值对重新计算哈希值并分配到新的数组中。

4. Map里面的key和value怎么存的?

   - 节点含 hash 字段存键哈希值，用于定位；key 和 value 字段存键值对。还有 next 字段用于处理哈希冲突形成链表或维护红黑树节点连接。
   - 存时先算 key 哈希确定桶位，桶空直接放，桶不空按链表或红黑树规则插入。
   - 取时同样先定位桶位，再按结构遍历比较找 key 获对应 value 。 不同 Map 实现类存储方式略有不同，TreeMap 按序用红黑树存，LinkedHashMap 维护顺序在 HashMap 基础上改进。

5. HashMap怎么快速定位到元素?

   1. 计算哈希值 : 当向HashMap中插入一个键值对时, 首先会调用键对象的 hashCode() 方法来获取其哈希值 , 得到哈希值映射到 HashMap内部数组的索引位置上
   2. 解决哈希冲突 : 由于不同的键可能会计算出相同的哈希值 , 这就导致了哈希冲突 , 当查找一个元素时 , 首先根据键的哈希值找到对应的数组索引位置 , 如果该位置上有链表 , 就需要遍历链表 , 通过键的 equals 方法来逐一比较链表中的键与要查找的键是否相等 , 直到找到匹配的键值对或者遍历完整整个链表
   3. 动态扩容 : 随着键值对的不断插入 , HashMap的负载因子会逐渐增大 . 负载因子是值 HashMap 中存储的键值对数量与数组长度的比值 . 当负载因子超过默认的阙值 (通常是 0.75) 时 , HashMap会进行扩容操作

6. 哪些集合是线程安全的?

   1. 早期线程安全集合 : Vector(多线程环境下性能较差 , 因为多个线程访问时需要竞争锁 , 容易造成阻塞) , Hashtable(高并发场景下性能不佳 , 并且它不允许键或值为null , 使用程度上有一定的局限性)
   2. 并发包中的集合类 : `ConcurrentHashMap`(采用分段锁的机制 , 将整个哈希表分成多个段 , 适用于高并发场景下的大量数据读写操作) , `CopyOnWriterArrayList`(在进行写操作时 , 先复制一份原数组 , 然后在新的数组上进行修改操作 , 适用于读多写少的场景 , 在并发读取时不需要加锁 , 性能较好)  , `CopyOnWriteArraySet`(基于`CopyOnWriterArrayList`实现的 , 内部使用了一个`CopyOnWriterArrayList`来存储元素 ,  适合于对集合元素进行并发读取和少量并发写入的场景)

7. 线程安全的List?

   1. 早期线程安全集合 : Vector
   2. 并发包中的线程安全 : CopyOnWriteArrayList

8. 哪些集合有排序功能?

   1. 列表类 : ArrayList , LinkedList (依赖 `Collections.sort()`方法对其进行排序)
   2. 集合类 : TreeSet (基于红黑树实现的有序集合), TreeMap (基于红黑树实现的有序映射)
   3. 其他类 : PriorityQueue(基于优先级堆实现的队列 , 它的元素会按照优先级进行排序)

9. TreeSet怎么保证有序?

   1. 红黑树特性(自平衡二叉查找树 , 有序性)
   2. 比较规则(自然顺序 , 指定比较器)

10. JUC里面其他集合用过吗?

    1. 阻塞队列(`ArrayBlockingQueue`<基于数组实现的有界阻塞队列> , `LinkedBlockingQueue`<基于链表实现的阻塞队列>, `PriorityBlockingQueue`<支持优先级排序的无界阻塞队列>)
    2. 并发映射(`ConcurrentHashMap`<线程安全的哈希表> , `ConcurrentSkipListMap`<基于跳表(Skip List)实现的并发有序映射> )
    3. 并发集合工具类(`Collections.synchronizedSet`<通过对普通集合进行包装 , 使用同步机制来实现线程安全> , `Collections.synchronizedList`<与synchronizedSet类似 , 也是对普通列表进行包装 , 通过在每个操作方法上添加同步块来实现线程安全>)

11. 什么场景使用的ConcurrentHashMap?

    1. **多线程缓存系统** : 本地缓存 , 分布式缓存节点存储
    2. **多线程计数器** : 全局计数器 , 对象引用计数
    3. **并发数据处理** : 多线程数据聚合 , 并发数据过滤与转换
    4. **多线程任务调度** : 任务队列管理 , 任务进度跟踪

12. ThreadLocal了解过吗?

    是 Java 中的一个类 , 它提供了**线程局部变量**`(每个线程都有自己独立的变量副本,各个线程之间互不干扰)`的功能 ,  当线程访问 ThreadLocal 变量时 , 它首先获取当前线程的 ThreadLocalMap , 然后根据 TreadLocal 对象作为键来查找对应的变量值并返回 `(避免共享变量的并发问题,保存线程上下文信息)`

    ```java
    public class ThreadLocalExample {
    
        // 创建一个ThreadLocal对象，用于存储每个线程的名称
        private static final ThreadLocal<String> threadLocalName = new ThreadLocal<>();
    
        public static void main(String[] args) {
            // 创建并启动两个线程
            Thread thread1 = new Thread(() -> {
                // 设置线程1的名称
                threadLocalName.set("Thread 1");
                System.out.println("Thread 1 set name: " + threadLocalName.get());
            });
    
            Thread thread2 = new Thread(() -> {
                // 设置线程2的名称
                threadLocalName.set("Thread 2");
                System.out.println("Thread 2 set name: " + threadLocalName.get());
            });
    
            thread1.start();
            thread2.start();
    
            try {
                // 等待两个线程执行完毕
                thread1.join();
                thread2.join();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
    
            // 主线程访问ThreadLocal变量，输出为null，因为每个线程都有自己的副本
            System.out.println("Main thread get name: " + threadLocalName.get());
        }
    }
    ```

    

13. 底层怎么实现的?

    **内部属性**

    1. `private final int threadLocalHashCode = nextHashCode();`：每个 ThreadLocal 对象都有一个唯一的哈希码，用于在 ThreadLocalMap 中定位存储位置 。
    2. `private static AtomicInteger nextHashCode = new AtomicInteger();`：用于生成唯一的哈希码，通过原子操作保证在多线程环境下每个 ThreadLocal 对象的哈希码唯一性。
    3. `private static final int HASH_INCREMENT = 0x61c88647;`：哈希码的增量值，用于计算下一个 ThreadLocal 对象的哈希码，这个值是通过黄金分割点相关的算法得出的，能让哈希码在一定程度上均匀分布，减少哈希冲突。

    **主要方法**

    - `public T get()`：用于获取当前线程的 ThreadLocalMap 中与该 ThreadLocal 对象对应的变量值。首先通过`Thread.currentThread()`获取当前线程，然后调用线程的`threadLocalMap.getEntry(this)`方法获取对应的键值对 Entry，如果 Entry 不为空，则返回 Entry 的 value 值，否则返回`setInitialValue()`方法设置的初始值。
    - `public void set(T value)`：用于设置当前线程的 ThreadLocalMap 中与该 ThreadLocal 对象对应的变量值。同样先获取当前线程，然后获取线程的`threadLocalMap`，如果`threadLocalMap`不为空，则直接将值设置到`threadLocalMap`中，以当前 ThreadLocal 对象为键；如果`threadLocalMap`为空，则先创建一个新的`threadLocalMap`并将值设置进去。
    - `public void remove()`：用于移除当前线程的 ThreadLocalMap 中与该 ThreadLocal 对象对应的变量值。通过当前线程获取`threadLocalMap`，然后调用`threadLocalMap.remove(this)`方法移除对应的键值对。

14. 线程有哪些状态?

    1. **新建状态**`(New)` : 当用`new`关键字创建一个线程对象时 , 该线程处于新建状态 , 此时它和其他 Java 对象一样 , 仅仅在堆内存中分配了内存空间 , 还没有开始执行
    2. **就绪状态**`(Runnable)` : 也叫可运行状态 , 当线程对象调用了`start()`方法后 , 线程就进入了就绪状态 , 处于此状态的线程位于可运行线程池中 , 等待获取 CPU 资源 , 一旦获取到 CPU 时间片 , 就可以立即执行
    3. **运行状态**`(Blocked)` : 当就绪状态的线程获得 CPU 时间片，开始执行`run()`方法中的代码时，线程就处于运行状态。在运行状态的线程可能会因为时间片用完或者被高优先级线程抢占 CPU 等原因，再次回到就绪状态。
    4. **阻塞状态**`(Blocked)` :当线程在执行过程中遇到某些阻塞事件时，如等待获取锁、等待 I/O 操作完成、调用`Thread.sleep()`方法等，线程会进入阻塞状态。处于阻塞状态的线程会暂停执行，直到阻塞事件解除。
    5. **等待状态**`(Waiting)` : 线程调用`Object.wait()`、`Thread.join()`或`LockSupport.park()`等方法时，会进入等待状态。等待状态的线程需要等待其他线程执行特定操作来唤醒它，如调用`Object.notify()`或`Object.notifyAll()`等方法。
    6. **终止状态**`(Terminated)` : 当线程的`run()`方法执行完毕或者线程执行过程中出现了未捕获的异常导致线程提前结束时，线程就进入了终止状态。处于终止状态的线程已经结束了生命周期，不会再切换到其他状态。

15. Java有哪些方式可以阻塞一个线程?

    使用sleep() 方法

    使用wait() 方法

    使用join() 方法

    使用阻塞式IO操作 : 当线程执行阻塞式IO操作时,如从网络套接字读取数据或从文件读取数据,如果数据尚未准备好,线程会被阻塞,直到数据可读或可写为止

    使用信号量 : 信号量是一种用于控制多个线程对共享资源访问的机制.通过 acquire() 方法获取信号量 , 如果信号量的数量小于请求的数量,线程会被阻塞,直到有足够的信号量可用

    使用锁(Lock)的 lock() 和 unlock() 方法

16. Java保证线程安全的关键字?

17. Synchronized怎么保证线程安全，持有的锁对象是什么?

18. 聊一下对JVM的认识?

19. JVM的内存结构?

20. 堆和栈分别有哪些区域?

21. GC对堆内每一个区域怎么收集，有哪些收集器、算法?

22. 新生代老年代了解过吗?

23. Spring的核心思想?

24. 将bean交给Spring管理的方式?

25. Spring创建对象的过程?

26. Spring为什么是单例的，没有线程安全问题吗?

27. AOP是什么?

28. 哪些业务场景使用AOP?

29. AOP的实现原理是什么?

30. SQL分库分表了解过吗?

31. MySQL索引有哪些?

32. 创建索引的规范?

33. 索引的数据结构?

34. Redis的使用场景?

35. Redis怎么保证和数据库的一致性?

36. 项目中的Redis使用的什么数据类型?

37. MyBatis的一级缓存都存什么?

38. 公司主要做什么项目?



1. 定时任务弊端?
2. 订单超时的解决方式?
3. iterator的基本原理?
4. map遍历?
5. list遍历 for中会出现的问题?
6. int包装类底层原理?
7. mybatis的动态?
8. 索引类型?
9. 索引失效?
10. 事务失效?
11. 权限框架?
12. hashMap, hashtable的区别?



1. springcloud和springboot的区别?
2. springcloud的哪些组件?
3. 动态刷新配置中心（nacos）的配置? `@RefreshScope  @ConfigurationProperties(prefix = "user")`
4. springboot启动的时候如何打印自己名字?`(至少两种:  创建bean实现implements ApplicationRunner; 直接在启动类中打印; 创建bean对象实现任一生命周期方法打印)`
5. 介绍一下gateWay?
6. 介绍一下负载均衡?
7. mysql 中 varchar和char的区别?
8. mysql的Timestamp与dateTime区别?
9. springboot配置文件的优先级?
10. vue中 ts和 js的区别?
11. docker 怎么定义一个dockerFile?
12. docer 如何运行dockerFile?
13. 介绍一下镜像和容器?
14. vue事件修饰符?
15. redis 如何查看所有key?  keys
16. mysql 取前50条数据? limit
17. Quartz每分钟执行一次，有的响应超时了，导致任务堆积，Quartz有几种策略解决



1. 他说他做的项目都是微服务的，所以问的微服务比较多，springcloud都有什么，怎么用
2. 前端两个等号和三个等号之间的区别
3. 前端@click.stop(事件修饰符)是什么意思?
4. 还有其他的什么scss如何定义一个参数[看不懂]?
5. redis 设置过期时间? 
6. docker流程 写一个dockerfile? `乐哥课件都有嗷`
7. 微服务之间如何交互?`[远程调用]`



1. 有关MQ, 他说订阅模式（好像叫这个）?`目测是指如何消费消息 或者 如何创建某种类型的交换机及队列`
2. springcloud组件有哪些?
3. 项目上使用过多线程吗, 知道锁有哪些吗?



1. 做过前端页面吗?
2. Vue用过吗?
3. StringBuffer和StringBuilder的区别?那个速度快?
4. 如何跳出双层for循环(标签,外层循环使用标志位boolean变量,内层抛出异常,内层修改外层条件),如何跳出本次遍历
5. 类的变量加载顺序?
6. 多线程做过吗?
7. linux接触过吗?说几个基本命令?
8. linux修改文件内容用什么命令?
9. linux中如何按名称去查找一个文件?
10. Vue了解什么,讲一下?
11. Vue组件间传值?(子父传值,父子传值)
12. 用过属性传值吗?
13. 插槽用过吗,插槽的关键字是什么?
14. Vue的下拉框做过吗?给下拉框做一个可编辑的,可以输入过滤的,怎么实现 ?
15. 前台调试用过吗,F12在文件上加Debug会不会?
16. linux中755什么意思知道吗? rwx  r-x r-x
17. linux中如何给文件加可执行权限怎么加?  chmod +x 文件
18. SpringBoot的常用注解有哪些?
19. 数据库的注解是什么?
20. 事务的注解是什么,简单讲一下什么是事务?
21. 如果对一个表操作, 先删再查可以放到一个事务里吗?
22. 平常定位过堆栈问题吗?服务器上CPU过高的话怎么定位?
23. 查询数据库的话in的最大数量是多少?
24. 一千万个数据如果想快速查询的话怎么做?
25. 查询一年当中上个月的数据要加什么?
26. 做过分区吗?
27. 执行计划了解过吗?
28. 数据库中的自定义函数以及函数的关键字,触发器,存储过程?
29. 了解表锁跟行锁吗?
30. 用Echarts都做过什么图?柱状图的Type是什么?(bar)
31. 平常代码开发的时候有什么要注意的吗?
32. 多线程之间不能用什么变量?
33. 多线程之间想让变量是可见的需要加什么?
34. 接口用的多吗?
35. 有一个卡车有一个摩托车和自行车怎么设计代码, 采取什么模式?
36. 报表做导入导出的时候需要注意什么(Excel和PDF)?
37. 有一千万个数据的话做导入导出的话怎么做?
38. 有没有做过安全方面的考虑?个人敏感数据方面?
39. 密码加密用的什么加密算法
40. Redis的缓存穿透?
41. 导出过大文件吗?
42. 现在用ai工具吗?(gpt,文言一心等)



1. IOC, AOP的基本概念了解吗?
2. 有没有做过不同数据库之间的迁移适配，是怎么做的? 如果函数不一样，mybatis是怎么处理的？
3. 前端新增一个字段，在列表展示，这种简单的功能会做吗？
4. 项目部署做过吗？
5. 做过Tomcat的项目部署吗？主要是如何部署war包？
6. 项目设计过复杂的存储过程和函数吗？
7. 根据项目问的数据库的内容比较多?



1. java方面哪些东西比较了解?
2. hashMap底层数据结构, 设计?
3. 红黑树的特点?
4. hashMap的桶, 是通过什么实现的?
5. 文件导入导出?
6. easyExcel怎么使用的?
7. SQL优化?
8. 索引的最左匹配原则?
9. 其他的MQ有了解的吗?
10. nacos怎么使用的?
11. 远程调用的实现?
12. 如何通过OpenFeign调用nacos上其他组件的服务?
13. Redis常用数据类型? 各自用在哪些场景下?
14. Cloud常用组件?
15. 网关主要用来做什么?
16. SpringBean的初始化过程, 扩展点?
17. Spring注解?
18. SpringBoot自动加载机制?
19. 了解哪些设计模式?
20. 以下是结合项目问的:
    * 平时是如何根据需求进行开发的?
    * 项目用什么部署?
    * 一个服务有多少个实例?
    * 系统有多少个微服务?
    * 各个模块之间互相调用怎么实现的?



1. 你用的什么前端技术？
2. 你是选项式还是组合式Api？ 
3. ref 和 reactive的区别？
4. 前端UI你是用的什么？
5. 事务的注解？
6. Sql优化？
7. 系统性能优化怎么优化的？
8. sql语句书写？ 查询某个字段 重复的信息
9. 你项目用的是mybatis-plus需要写sql语句吗？它与mybatis的区别？



结合项目一并提问:

1. MyBatis动态标签?

2. NACOS的作用?

3. MyBatis中事务的标签?

4. RabbitMQ在什么场景下使用?

5. Redis在什么地方使用?

6. Redis做持久化了吗?

7. 面向终端用户吗?

8. 系统大概有多少用户?


选择题, 判断题(没有图):

1. try, catch, finally说法正确的是?
2. spring事务的注解?
3. spring依赖注入的注解?
4. vue.js生命周期?
5. JSON转对象的方法?
6. vue子组件调用父组件的方法?
7. vue2与vue3的区别?
8. vue: 双向绑定, 计算属性, 函数?
9. v-if与v-show?
10. MyBatis一级缓存的作用域?
11. SQL: 分组, 排序, 求和?

笔试部分:

1. int数组, 求正数的和[`警惕数据溢出`]?

2. SQL: 求员工表中比平均工资高的员工?

3. 前端: 通过Json字符串, 获得姓名, friends数组的第一个朋友的名字, 修改邮箱的内容? js提供了对应的解析方法,

   ~~~json
   {
     "name": "John Doe",
     "age": 30,
     "friends": [
       {"name": "Jane Smith", "age": 28},
       {"name": "Michael Johnson", "age": 32}
     ],
     "email": "john.doe@example.com"
   }
   ~~~

   ~~~js
   let json_str = `{
     "name": "John Doe",
     "age": 30,
     "friends": [
       {"name": "Jane Smith", "age": 28},
       {"name": "Michael Johnson", "age": 32}
     ],
     "email": "john.doe@example.com"
   }`
   
   // 解析
   let pojo = JSON.parse(json_str)
   // 姓名
   let name = pojo.name
   // 第一个朋友
   let firiend = pojo.friends[0]
   // 修改邮箱
   pojo.email = "xxx@555.com"
   // 反解析
   json_str = JSON.stringify(pojo)
   ~~~



1. 做过哪些项目?
2. 微服务注册中心用的哪个?
3. 简单说一下Nacos?
4. 了解过设计模式吗?
5. 代码中写过哪些设计模式?
6. 用过Lambda表达式吗?
7. MyBatisPlus更新语句?
8. MySQL执行顺序?
9. Having用过吗?
10. Redis在你的项目中怎么用的?
11. 自己部署过项目吗?
12. Linux的部署命令?
13. 查询实时的2000条日志使用什么命令?`tail -n 2000 日志.log`
14. Redis的持久化方式?
15. 在代码中遇到过什么问题(业务难题, 不是CRUD!)?
16. 数据库主键用什么(主键自增还是雪花ID)?
17. 前端失真(数据溢出, 例999 -> 990)怎么解决的?
18. Linux的命令?
19. Docker的命令?
20. 启动容器用过compose吗?
21. Vue的生命周期?
22. MySQL数据库有哪些索引?
23. 除了增删改查做过什么难的项目吗(项目中的技术难点)?



1. mysql和Oracle的字段类型的区别，groupBy的区别?
2. 查询一个表中按照两个字段把重复的过滤掉?
3. 一个表中有班级，科目，分数三个字段，按照一个班级中不同的科目展示分数（一个班级展示一行）?
4. 索引失效？`违背最左匹配原则, 索引列参与了函数计算, 比较时数据类型不一致`
5. 查询数据很大的话，怎么找到问题，怎么优化?
6. in用什么可以代替？`exists`



1. springcloud注册中心用的什么?
2. Java审批流(工作流 activiti)?
3. 常见设计模式?
4. 部署项目?
5. 做过支付模块吗?
6. java接口的幂等性, 防止重复提交?
7. MyBatis的实体类中剔除不是表中的字段?
8. 数据库的主键的作用?
9. id很长, 到前端失真如何处理?
10. 字符串处理, 方法?
11. Redis一般是做什么的?
12. Redis数据一致性?
13. Redis持久化方式?

 

1. Vue2生命周期?
2. vue的keep alive组件?
3. vue组件间通讯方式?
4. 样式穿透了解吗?
5. nextTick了解吗?
6. hashMap中的加载因子有了解吗?
7. 用List去存放一组对象并且根据年龄排序?
8. 判断字符串是否为空?
9. 涉及金额计算你是用的什么?
10. SpringBoot注解?



1. springboot注解?
2. springcloud开发的流程?
3. redis的击穿 穿透 雪崩?
4. mysql oracle的区别 函数 分页等?
5. redis分布式事务?
6. vue3组件间通讯?
7. redis使用场景?



1. vue2的生命周期?
2. vue2 v-for 中的key的作用 如何比较的?
3. vue2的为什么要使用 nextTick?
4. var let const 的区别?
5. cookie 和session的区别?



1. stream流，有哪些方法，分组是什么方法?
2. list的实现有哪些，有什么区别?
3. HashSet的底层?
4. HashMap默认空间大小，扩容机制，阈值是多少?
5. boolean的初始值是什么?
6. Boolean初始值是多少?
7. AOP?
8. JDK原生AOP用过吗?
9. MyBatis  实体类名称和字段名称不一致怎么解决?
10. 接收JSON参数用什么注解?
11. @Autowired‌和@Resource‌的区别?
12. 事务的注解是什么，底层是怎么实现的?
13. SpringBoot启动类的注解是什么，里面包含什么注解?
14. 这些注解的作用是什么?
15. 校验前端传来的code使用什么注解?
16. MyBatis 一对一，一对多?
17. Advice全局捕获异常怎么做?
18. MySQL的分页使用什么，查看第11-20条怎么写?
19. Spring自动装配原理?
20. SQL数据量比较大怎么解决?
21. 创建线程的方式?



1. 缓存穿透?

2. 用过Redis吗，主要有什么用?

3. 微服务用的注册中心是什么?

4. 网关用的是什么?

5. Nacos的作用是什么?

6. 接口幂等性?

7. Linux部署，基本命令?

8. 了解过国产数据库吗?

9. @Autowired‌和@Resource‌的区别?

10. 设计模式，项目中用到了哪些?

11. Redis持久化?`RDB AOF`



1. Vue传值方式有哪些，怎么传?`路由传值 组件间传值`

2. 集合有哪些，list集合用过哪些，区别是什么?

3. 用过哪些注解?

4. 怎么理解的SpringBoot?

5. SpringMVC的流程?

6. Linux熟悉的命令?

7. SpringBoot打什么包，打包之后怎么运行 （不在docker里面）?`jar`

8. 部署过应用吗?

9. @Autowired和@Resource有什么区别?

10.  list是可变的吗，自动扩容底层，数组是可变的吗?



1. 反射的原理?反射获得类对象的三种方式?

2. list map set 的存储特点?

3. BIO、NIO、AIO 有什么区别?

4. java socket 建立连接的流程?

5. Boot 的常用注解? 作用是什么?

6. MVC工作原理?

7. Boot解决跨域问题?

8. Boot中配置文件加载顺序?

9. 数据库事务的四个特性及其含义?

10. 使用MyBatis的mapper接口调用的时候, 有哪些要求?

11. #{} 和 ${} 的区别?

12. MyBatis有哪些Executor执行器?

13. 模糊查询Like语句应该怎么写?

14. 熟悉哪些设计模式?

15. sql编程题

    `T_score`

    | `编号` | `科目`  | `成绩` | `学生编号` |
    | :----: | :-----: | :----: | :--------: |
    | stu_id | subject | score  | studentId  |
    |   1    |  数学   |   98   |     1      |
    |   2    |  数学   |   88   |     2      |
    |   3    |  英语   |  100   |     2      |
    |   4    |  英语   |   90   |     1      |
    |   5    |  语文   |   80   |     1      |
    |   6    |  语文   |   99   |     2      |

    `T_student`

    | `编号` | `姓名` | `性别` | `年龄` |
    | :----: | :----: | :----: | :----: |
    |   id   |  name  |  sex   |  age   |
    |   1    |  张三  |   男   |   22   |
    |   2    |  李四  |   男   |   4    |

    1. 联表查询学生成绩, 并按照姓名,科目排序?
    2. 求每个学生的总分和平均分?
    3. 求每科的总分和平均分?

16. 简述ajax的过程?

17. 行内元素有哪些?块级元素有哪些?CSS盒子模型?



* 单选

1) 下面哪个Linux命令可以一次显示一页内容？

A.pause

B.cat

C.more

D.grep

2) 在vi中退出不保存的命令是？

A. :q

B. :w

C. :wq

D. :q!

3. linux文件权限既可以用数字表示，也可以用rwx表示。那权限652代表：

A. rwxr-x-w

B. r-xrwx-wx

C. r-xrwxx-w

D. rw-rx-w

4. 分布式系统的CAP理论不包括下面哪项：

A. 可用性

B. 完整性

C. 分区容错性

D. 一致性

5. Spark中，下面哪个操作肯定是宽依赖：

A. map

B. flatMap

C. reduceByKey

D. sample

6) 清空表tsuer中所有数据，性能最优的语句是:

A. delete from tsuer

B. truncate table tuser

C. drop table tuser

D. delete tuser

7) 如下哪个命令，可以在已运行的容器中执行命令：

A. docker exec

B. docker go

C. docker run

D. docker do

8) 下列关于kafka的描述，说法错误的是：

A. KafkaConsumer是非线程安全的

B. Broker是kafka集群中的一个节点

C. Kafka基于时间轮实现了延迟队列

D. 每个topic的消息会保证有序

9. 下列哪个选项是已知的不安全的加密算法：

A. AES128

B. RSA3072

C. SHA3

D. MD5

10) 在Java中，关于HashMap类的描述，错误的是：

A. HashMap使用键/值的形式保存数据

B. HashMap能够保证其中元素的顺序

C. HashMap允许将null用作键

D. HashMap允许将null用作值

* 简答

1. 有一个1G大小的一个文件,里面每一行是一个词,词的大小不超过16字节,内存限制大小是1M。返回频数最高的100个词。简述实现的思路即可。

2. 用Java实现单例（Singleton ）模式。

3. 指出下面程序的运行结果。

~~~java
class A {
    static {
    	System.out.print("1");
    }
    public A() {
    	System.out.print("2");
    }
}



class B extends A {
    static {
    	System.out.print("a");
    }
    public B() {
    	System.out.print("b");
    }
}

public class Hello {
    public static void main(String[] args) {
        A ab = new B();
        ab = new B();
    }
}
~~~

4. 用Java写一个折半查找。

5. 当客户反映，应用变得很慢的时候，你是怎么处理及定位问题的？

6. 现有dept(部门表)，表中有 部门编号（deptno ） 部门名字 (dname ) 员字段，

emp(员工表)，表中有 员工编号（empno ）员工名字(ename ) 员工职位(job) 管理者(mgr) 工资(sal) 部门编号(deptno )

~~~sql
## 插入实验数据

insert into dept values ('1','事业部');

insert into dept values ('2','销售部');

insert into dept values ('3','技术部');

insert into emp values ('01','jacky','clerk','tom','1000','1');

insert into emp values ('02','tom','clerk','','2000','1');

insert into emp values ('07','biddy','clerk','','2000','1');

insert into emp values ('03','jenny','sales','pretty','600','2');

insert into emp values ('04','pretty','sales','','800','2');

insert into emp values ('05','buddy','jishu','canndy','1000','3');

insert into emp values ('06','canndy','jishu','','1500','3');
~~~

（1） 列出emp表中各部门job为'CLERK'的员工的最低工资，最高工资（数据库题）

（2） 对于emp中最低工资小于2000的部门，列出job为'CLERK'的员工的部门号，最低工资，最高工资；（数据库题）



1. 项目是如何部署的?
2. 单元测试方式?
3. 全面测试?
4. redis中的基本类型?
5. string相关?
6. string的工具类?
7. 常见的异常?
8. 发现异常怎么处理的?
9. try里面有return语句 finally还执行吗?
10. 索引类型?
11. 索引的创建?
12. 三个索引的最左原则?
13. redis的基本类型?
14. JVM 怎么扩容?
15. map底层数据结构了解过吗？如何实现数据扩容？
16. map删除一个key 内存会不会释放？
17. 如何判断一个字段是否不适合建立索引?
18. 分布式环境下需要数据一致性的话，你有几种设计方案 TCC?
19. 索引为什么快?
20. mysql存储json数据?
21. 分布式锁如何设计？setnx和setex有什么区别？如何续期?
22. 二级缓存cache 数据一致性如何保证 reids呢，如何保持数据一致性？
23. 分布式锁如何设计的？追问细节，问的很细，实现细节?
24. 消息队列宕机之后重启怎么知道它上次消费到哪里？offset记录？offset数据存储在哪里？
25. 消息幂等性如何设计？



1. 什么是事务？什么是锁？Oracle中的乐观锁怎么实现的？
2. 存储过程是什么？能不能用java写出一个简单的存储过程，有输入参数和输出参数?
3. Mysql和Oracle的区别？
4. 项目里除了crud还用到了遇到什么复杂的业务逻辑吗？
5. Mybatis的小于号怎么替换的？
6. v-if和V-show的区别？
7. 熟悉springboot吗？到哪种程度的熟悉？
8. spring Bean的作用域?
9. StringBuffer和StringBuilder的区别?



1. Websocket用于可视化大屏合适吗？

2. SpringCloud 用了哪些主键

3. SQl慢查询 优化你是使用的 什么级别的?

4. 传感器传回的数据你是直接使用的吗？

5. 远程调用用的什么 它是怎么使用的

6. echarts怎么画折线图？

7. mysql 数据库的事务是啥？



1. 项目相关，项目中遇到的重难点?
2. java如何保证ArrayList的线程安全?
3. 接口是否支持多继承?
4. Java中io流有哪些?
5. 创建线程的方式?
6. 线程中的  sleep方法和wait方法的区别?
7. Thread的run方法和start方法的区别?
8. 如何预防死锁?
9. sql优化?
10. mybatis的缓存?
11. mybatis实现一对多?
12. redis的持久化?
13. ArrayList中有 1,2,3,4,1  五个数; 把2取出来用什么方法?
14. 并发与并行?



1. 是不是前后端都会?
2. 后端主要用到了哪些框架?
3. 数据库用到了什么?
4. MySQL sql优化?
5. 重复子查询怎么处理?
6. 后台接口是不是以mybatis xml语句为主?
7. myziker api  有没有用过    （这个什么api没听懂）?
8. 消息队列用到什么?
9. 微服务了解么?
10. 服务注册与发现用的啥?
11. 源码管理用的啥?
12. vue熟悉么?



1. 会写dockerfile吗?
2. 用过Linux吗?
3. 会用UniApp吗?
4. SQL语句写的怎么样?
5. Echarts用的怎么样? 
6. 国产化系统有了解吗?
7. 可以全栈开发吗?



1. 项目相关?
2. 为什么选择使用SpringCloud架构?
3. 对spring的了解?
4. spring配置方式?
5. 什么是mybatis?
6. 什么框架是全ORM?
7. mybatis实现分页方式?
8. RowBounds是什么?
9. 自己手写过mybatis插件么?
10. 动态sql?
11. 项目中使用的动态sql场景?
12. mybatis接口绑定?
13. 什么情况下使用注解绑定 什么情况下使用xml绑定?
14. mybatis 一对多  多对多?
15. mysql 存储引擎?
16. 为什么使用innodb?
17. 数据库的事务在项目中的应用?
18. 数据库锁?
19. 自增主键  10条记录 插入两条  最新插入的id  删除两条  在插入一条 最新的一条id?
20. 数据库备份?



1. 索引失效?
2. 多线程   线程池的核心参数?
3. 分布式调度框架  xx-job?
4. 消息中间件  项目中的用到的场景?
5. 延时消费?
6. 消息重复消费?
7. 部署  docker命令?
8. 鉴权?
9. spring security 源码了解过么?
10. mybatis plus 字段映射没有绑定  什么注解解决?
11. mysql 实现判断?
12. mysql string 处理函数?
13. mybatis 传递String int 类型参数  xml怎么接收?



1. 自我介绍?
2. 线程并发?
3. 计算金钱用哪个类?
4. 数据类型，包装类?
5. string是基本数据类型吗?
6. redis在其他场景下用到吗?
7. rabbitmq在哪里用到了, 怎么用的?
8. 定时任务怎么防止数据丢失?
9. 询问项目细节，流程，时间，工作公司?



1. 谈一下spring的ioc和aop
2. 数据库的索引了解过吗?使用过那些索引?
3. 项目中你的aop注解用的哪几个?实现了什么功能?
4. 多线程了解过吗?线程池使用过吗?如何创建一个线程池?
5. 讲一下你对SpringBoot框架和SpringCloud框架的了解?
6. SpringCloud中用到了那些组件?远程调用使用的那个组件?
7. 数据库发生死锁怎么解决?
8. 如何防止消息重复消费?(还得考虑分布式锁)
9. 设计一个前端,后端登录功能的流程,发送了什么信息?如何校验的?
10. 后端一接口特别慢怎么优化?(1.先查看前端的请求时间2.查看后端代码逻辑3.考虑数据库sql优化)
11. 数据量特别大时,达到千万级怎么办?
12. 数据库死锁遇到过吗?问如何解决的
13. 使用过docker吗?DockerFile会写吗?(他们项目使用的k8s,说是和docker差不多,会提问docker相关的)
14. 使用过redis吗?redis有哪几种类型?
15. 将一些不太用到的信息存放在redis中,如何操作?用那种数据类型?



1. 什么是事务？什么是锁？Oracle中的乐观锁怎么实现的？
2. 存储过程是什么?用java写出一个简单的存储过程，有输入参数和输出参数
3. Mysql和Oracle的区别？
4. 项目里除了crud还用到了遇到什么复杂的业务逻辑吗？
5. Mybatis的小于号怎么替换的？(转义字符)
6. v-if和V-show的区别？
7. mybatis默认有几级作用域?如何开启?
8. mybatis中#{}和${}的区别?
9. $nextTick的作用是什么?在什么时候使用?
10. 熟悉springboot吗？到哪种程度的熟悉？
11. 笔试题还有一个表的增删改查，就是第二周的那种练习题,单表的,很简单
12. Bean的作用域?
13. 数据库死锁遇到过吗?怎么解决?
14. Oracle和MySql的区别?
15. SpringBoot核心注解有哪些?大体讲一下有什么功能
16. 后面就是问了一些项目上的功能实现,业务流程
