## Java基础

### 1.  单位换算

+ bit(位)：0,1
+ byte(字节)：1byte = 8bit
+ word(字)：1word = 2byte
+ 1KB = 1024bytes

### 2. Java中有几种基础类型，各占用多少字节

```mermaid
graph LR
A[数据类型] --> B[基础数据类型]
A --> C[引用数据类型]
B --> D[数值型]
B --> E[字符型&#40char&#41]
B --> F[布尔型&#40boolean&#41]
C --> G[类&#40class&#41]
C --> H[接口&#40interface&#41]
C --> I[数组]
D --> J[整数类型&#40byte,short,int,long&#41]
D --> K[浮点类型&#40float,double&#41]
```

| 基础类型 |         大小          |                           取值范围                           |
| :------: | :-------------------: | :----------------------------------------------------------: |
| boolean  |       1字节/8位       |                         true, false                          |
|   byte   |  1字节/8位有符号整数  |                         -128 ~ +127                          |
|  short   | 2字节/16位有符号整数  |    -32768（-2<sup>15</sup>） ~ +32767(+2<sup>15</sup>-1)     |
|   int    | 4字节/32位有符号整数  | -2147483648（-2<sup>31</sup>） ~ +2147483647(2<sup>31</sup>-1) |
|   long   | 8字节/64位有符号整数  |              -2<sup>63</sup> ~ +2<sup>63</sup>               |
|   char   | 2字节/16位Unicode字符 |                0 ~ 65535（2<sup>16</sup>-1 ）                |
|  float   |   4字节/32位浮点数    |                     ±1.4E-45 ~ ±3.4E+38                      |
|  double  |   8字节/64位浮点数    |                    ±4.9E-324 ~ ±1.7E+308                     |

### 3. JDK 和 JRE 有什么区别

+ JDK：Java Development Kit 的简称，java 开发工具包，提供了 java 的开发环境和运行环境。
+ JRE：Java Runtime Environment 的简称，java 运行环境，为 java 的运行提供了所需环境。

> 具体来说 JDK 其实包含了 JRE，同时还包含了编译 java 源码的编译器 javac，还包含了很多 java 程序调试和分析的工具。简单来说：如果你需要运行 java 程序，只需安装 JRE 就可以了，如果你需要编写 java 程序，需要安装 JDK。

### 4. String 能被继承吗？为什么

> 不可以，因为 String 类有 final 修饰符，而 final 修饰的类是不能被继承的，实现细节不允许改变。平常我们定义的 `String str = ”a”` 其实和 `String str = new String(“a”)` 还是有差异的。
>
> * 前者默认调用的是String.valueOf来返回String实例对象，至于调用哪个则取决于你的赋值，比如 String num = 1,调用的是
>
> ```java
> public static String valueOf(int i) {
> 	return Integer.toString(i);
> }
> ```
> * 后者则是调用如下部分：
>
> ```java
> public String(String original) {
> 	this.value = original.value;
> 	this.hash = original.hash;
> }
> ```
>
> * 最后我们的变量都存储在一个char数组中
>
> ```java
> private final char value[];
> ```

### 4. String， Stringbuffer， StringBuilder 的区别。

> * String 字符串常量（final修饰，不可被继承），String是常量，当创建之后即不能更改。(可以通过StringBuffer和StringBuilder创建String对象（常用的两个字符串操作类）。)
>
> * StringBuffer 字符串变量（线程安全），其也是final类别的，不允许被继承，其中的绝大多数方法都进行了同步处理，包括常用的Append方法也做了同步处理（synchronized修饰）。其自 jdk1.0 起就已经出现。其toString方法会进行对象缓存，以减少元素复制开销。
>
> ```java
> public synchronized String toString() {
> 	if (toStringCache == null) {
> 		toStringCache = Arrays.copyOfRange(value, 0, count);
> 	}
> 	return new String(toStringCache, true);
> }
> ```
>
> * StringBuilder 字符串变量（非线程安全）==其自jdk1.5起开始出现。与StringBuffer一样都继承和实现了同样的接口和类，方法除了没使用synch修饰以外基本一致，不同之处在于最后toString的时候，会直接返回一个新对象。
>
> ```java
> public String toString() {
> 	// Create a copy, don’t share the array
> 	return new String(value, 0, count);
> }
> ```
>
> ---
>
> 1. 可变与不可变
>
>    * String 类中使用字符数组保存字符串，如下就是，因为有“final”修饰符，所以可以知道 string 对象是不可变的。`private final char value[];`
>
>    * StringBuilder 与 StringBuffer 都继承自AbstractStringBuilder类，在 AbstractStringBuilder 中也是使用字符数组保存字符串，如下就是，可知这两种对象都是可变的。`char[] value;`
>
> 2. 是否多线程安全
>
>    * String中的对象是不可变的，也就可以理解为常量，显然线程安全。
>
>    * AbstractStringBuilder 是 StringBuilder 与 StringBuffer 的公共父类，定义了一些字符串的基本操作，如 expandCapacity、append、insert、indexOf 等公共方法。
>
>    * StringBuffer对方法加了同步锁或者对调用的方法加了同步锁，所以是线程安全的。看如下源码：
>
>    ```java
>    public synchronized StringBuffer reverse() {
>        super.reverse();
>        return this;
>    }
>    
>    public int indexOf(String str) {
>    	//存在 public synchronized int indexOf(String str, int fromIndex) 方法
>        return indexOf(str, 0);
>    }
>    ```
>
>    * StringBuilder 并没有对方法进行加同步锁，所以是非线程安全的。
>
> 3. StringBuilder 与 StringBuffer 共同点
>
>    * StringBuilder 与StringBuffer 有公共父类 AbstractStringBuilder（抽象类）。
>    * 抽象类与接口的其中一个区别是：抽象类中可以定义一些子类的公共方法，子类只需要增加新的功能，不需要重复写已经存在的方法；而接口中只是对方法的申明和常量的定义。
>    * StringBuilder、StringBuffer 的方法都会调用 AbstractStringBuilder 中的公共方法，如super.append(...)。只是 StringBuffer 会在方法上加 synchronized 关键字，进行同步。
>
> 4. 最后，如果程序不是多线程的，那么使用 StringBuilder 效率高于 StringBuffer。

### 5. ArrayList 和 LinkedList 有什么区别。

​	ArrayList和LinkedList都实现了List接口，有以下的不同点：

> 1.  ArrayList是基于索引的数据接口，它的底层是数组。它可以以O(1)时间复杂度对元素进行随机访问。与此对应，LinkedList是以元素列表的形式存储它的数据，每一个元素都和它的前一个和后一个元素链接在一起，在这种情况下，查找某个元素的时间复杂度是O(n)。
> 2.  相对于ArrayList，LinkedList的插入，添加，删除操作速度更快，因为当元素被添加到集合任意位置的时候，不需要像数组那样重新计算大小或者是更新索引。
> 3.  LinkedList比ArrayList更占内存，因为LinkedList为每一个节点存储了两个引用，一个指向前一个元素，一个指向下一个元素。

### 6. 讲讲类的实例化顺序，比如父类静态数据，构造函数，字段，子类静态数据，构造函数，字段，当 new 的时候， 他们的执行顺序。

**此题考察的是类加载器实例化时进行的操作步骤（加载–>连接->初始化）。**

> 父类静态变量、
> 父类静态代码块、
> 子类静态变量、
> 子类静态代码块、
> 父类非静态变量（父类实例成员变量）、
> 父类构造函数、
> 子类非静态变量（子类实例成员变量）、
> 子类构造函数。
>
> [参阅博客《深入理解类加载》](http://blog.csdn.net/u014042066/article/details/77394480)

### 7. 用过哪些 Map 类，都有什么区别？

> 1. HashMap：
>
>    最常用的Map，根据键的hashcode值来存储数据，根据键可以直接获得他的值（因为相同的键hashcode值相同，在地址为hashcode值的地方存储的就是值，所以根据键可以直接获得值），具有很快的访问速度，遍历时，取得数据的顺序完全是随机的，HashMap最多只允许一条记录的键为null，允许多条记录的值为null，HashMap不支持线程同步，即任意时刻可以有多个线程同时写HashMap，这样对导致数据不一致，如果需要同步，可以使用synchronziedMap的方法使得HashMap具有同步的能力或者使用concurrentHashMap。
>
> 2. HashTable：
>
>    与HashMap类似，不同的是，它不允许记录的键或值为空，支持线程同步，即任意时刻只能有一个线程写HashTable，因此也导致HashTable在写入时比较慢!
>
> 3. LinkedHashMap：
>
>    是HahsMap的一个子类，但它保持了记录的插入顺序，遍历时先得到的肯定是先插入的，也可以在构造时带参数，按照应用次数排序，在遍历时会比HahsMap慢，不过有个例外，当HashMap的容量很大，实际数据少时，遍历起来会比LinkedHashMap慢（因为它是链啊），因为HashMap的遍历速度和它容量有关，LinkedHashMap遍历速度只与数据多少有关。
>
> 4. TreeMap：
>
>    实现了sortMap接口，能够把保存的记录按照键排序（默认升序），也可以指定排序比较器，遍历时得到的数据是排过序的。

### 8. 什么情况用什么类型的Map：

> * 在Map中插入，删除，定位元素：HashMap
> * 要按照自定义顺序或自然顺序遍历：TreeMap
> * 要求输入顺序和输出顺序相同：LinkedHashMap

### 9. HashMap 是线程安全的吗，并发下使用的 Map 是什么，他们内部原理分别是什么，比如存储方式， hashcode，扩容， 默认容量等。

> * hashMap是线程不安全的，HashMap是数组 + 链表 + 红黑树（JDK1.8增加了红黑树部分）实现的，采用哈希表来存储的。
> * HashTable、ConcurrentHashMap 
>
> 

### 10. 有没有有顺序的 Map 实现类， 如果有， 他们是怎么保证有序的。

> TreeMap 和 LinkedHashMap 是有序的（TreeMap 默认是key升序，LinkedHashMap 默认是数据插入顺序）。
>
> * TreeMap 是基于比较器Comparator来实现有序的。
> * LinkedHashmap 是基于链表来实现数据插入有序的。

### 11. 抽象类和接口的区别，类可以继承多个类么，接口可以继承多个接口么,类可以实现多个接口么。

> 1. 抽象类和接口都不能直接实例化，如果要实例化，抽象类变量必须指向实现所有抽象方法的子类对象，接口变量必须指向实现所有接口方法的类对象。
> 2. 抽象类要被子类继承，接口要被类实现。
> 3. 接口只能做方法申明，抽象类中可以做方法申明，也可以做方法实现。
> 4. 接口里定义的变量只能是公共的静态的常量，抽象类中的变量是普通变量。
> 5. 抽象类里的抽象方法必须全部被子类所实现，如果子类不能全部实现父类抽象方法，那么该子类只能是抽象类。同样，一个实现接口的时候，如不能全部实现接口方法，那么该类也只能为抽象类。
> 6. 抽象方法只能申明，不能实现。abstract void abc(); 不能写成 abstract void abc() {}。
> 7. 抽象类里可以没有抽象方法。
> 8. 如果一个类里有抽象方法，那么这个类只能是抽象类。
> 9. 抽象方法要被实现，所以不能是静态的，也不能是私有的。
> 10. 接口可继承接口，并可多继承接口，但类只能单根继承。

### 12. 继承、实现、依赖、关联、聚合、组合的联系与区别

> 1. **继承**：指的是一个类（称为子类、子接口）继承另外的一个类（称为父类、父接口）的功能，并可以增加它自己的新功能的能力，继承是类与类或者接口与接口之间最常见的关系；在 Java 中此类关系通过关键字extends明确标识，在设计时一般没有争议性。
> 2. **实现**：指的是一个 class 类实现 interface 接口（可以是多个）的功能；实现是类与接口之间最常见的关系；在 Java 中此类关系通过关键字 implements 明确标识，在设计时一般没有争议性；
> 3. **依赖**：可以简单的理解，就是一个类A使用到了另一个类B，而这种使用关系是具有偶然性的、临时性的、非常弱的，但是B类的变化会影响到A；比如某人要过河，需要借用一条船，此时人与船之间的关系就是依赖；表现在代码层面，为类B作为参数被类A在某个 method 方法中使用。
> 4. **关联**：他体现的是两个类、或者类与接口之间语义级别的一种强依赖关系，比如我和我的朋友；这种关系比依赖更强、不存在依赖关系的偶然性、关系也不是临时性的，一般是长期性的，而且双方的关系一般是平等的、关联可以是单向、双向的；表现在代码层面，为被关联类B以类属性的形式出现在关联类A中，也可能是关联类A引用了一个类型为被关联类B的全局变量。
> 5. **聚合**：是关联关系的一种特例，他体现的是整体与部分、拥有的关系，即has-a的关系，此时整体与部分之间是可分离的，他们可以具有各自的生命周期，部分可以属于多个整体对象，也可以为多个整体对象共享；比如计算机与CPU、公司与员工的关系等；表现在代码层面，和关联关系是一致的，只能从语义级别来区分。
> 6. **组合 (a拥有b，a没了b也就没了，实心)**：组合也是关联关系的一种特例，他体现的是一种contains-a的关系，这种关系比聚合更强，也称为强聚合；他同样体现整体与部分间的关系，但此时整体与部分是不可分的，整体的生命周期结束也就意味着部分的生命周期结束；比如你和你的大脑；表现在代码层面，和关联关系是一致的，只能从语义级别来区分。

### 13. 讲讲你理解的 NIO 和 NIO 的区别是啥，谈谈 reactor 模型。

> IO(BIO)是面向流的，NIO是面向缓冲区的
>
> * BIO：Block IO 同步阻塞式 IO，就是我们平常使用的传统 IO，它的特点是模式简单使用方便，并发处理能力低。
> * NIO：New IO 同步非阻塞 IO，是传统 IO 的升级，客户端和服务器端通过 Channel（通道）通讯，实现了多路复用。
> * AIO：Asynchronous IO 是 NIO 的升级，也叫 NIO2，实现了异步非堵塞 IO ，异步 IO 的操作基于事件和回调机制。
>
> ---
>
> |   IO   |   NIO    |
> | :----: | :------: |
> | 面向流 | 面向缓冲 |
> | 阻塞IO | 非阻塞IO |
> |   无   |  选择器  |
>
> [Java NIO与BIO](https://www.jianshu.com/p/3f703d3d804c)

### 14. 反射的原理，反射创建类实例的三种方式是什么？

> 1. **什么是JAVA的反射机制？**
>
>    Java反射是Java被视为动态（或准动态）语言的一个关键性质。这个机制允许程序在运行时透过Reflection APIs取得任何一个已知名称的class的内部信息，包括其modifiers（诸如public, static 等）、superclass（例如Object）、实现之interfaces（例如Cloneable），也包括fields和methods的所有信息，并可于运行时改变fields内容或唤起methods。
>
>    Java反射机制容许程序在运行时加载、探知、使用编译期间完全未知的classes。
>
>    换言之，Java可以加载一个运行时才得知名称的class，获得其完整结构。
>
> 2. 















### 7. List、Set、Map 之间的区别是什么？

### 

------------------------------------------------
















### 4. == 和 equals 的区别是什么？

+ == 解读

  对于基本类型和引用类型 == 的作用效果是不同的，如下所示：

  + 基本类型：比较的是值是否相同；
  + 引用类型：比较的是引用是否相同；

代码示例：

```java
String x = "string";
String y = "string";
String z = new String("string");
System.out.println(x == y); // true
System.out.println(x == z); // false
System.out.println(x.equals(y)); // true
System.out.println(x.equals(z)); // true
```

代码解读：因为 x 和 y 指向的是同一个引用，所以 == 也是 true，而 new String()方法则重写开辟了内存空间，所以 == 结果为 false，而 equals 比较的一直是值，所以结果都为 true。

