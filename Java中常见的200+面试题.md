# 面试200+

![d5417d5c-5af0-46af-929c-4d42563c3ec6](C:\Users\86183\AppData\Local\Temp\d5417d5c-5af0-46af-929c-4d42563c3ec6.png)

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

​	等于 -1 , Math.round 四舍五入大于 0.5 向上取整