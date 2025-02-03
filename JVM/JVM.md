# JVM基础

## 什么是JVM

JVM就是Java虚拟机，是Java实现跨平台的基石

![三分恶面渣逆袭：Java语言编译运行](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/jvm-1.png)

## JVM的组织架构

![截图来源于网络](https://cdn.tobebetterjavaer.com/stutymore/what-is-jvm-20231030185742.png)

**1. 类加载器**

- 负责从文件系统、网络或其他来源加载 Class 文件，将 Class 文件中的二进制数据读入到内存当中。

**2. 运行时数据区**

- JVM 在执行 Java 程序时，需要在内存中分配空间来处理各种数据，这些内存区域主要包括方法区、堆、栈、程序计数器和本地方法栈。

**3. 执行引擎**

- JVM 的心脏，负责执行字节码。它包括一个虚拟处理器，还包括即时编译器 JIT 和垃圾回收器。



# JVM内存管理

## JVM的内存区域

![三分恶面渣逆袭：Java虚拟机运行时数据区](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/jvm-3.png)

**1. 程序计数器**

- 当前线程所执行的字节码行号指示器。

**2. 虚拟机栈**

![三分恶面渣逆袭：Java虚拟机栈](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/jvm-4.png)

- Java 虚拟机栈的生命周期与线程相同。

- 当线程执行一个方法时，会创建一个对应的**栈帧**，用于存储局部变量表、操作数栈、动态链接、方法出口等信息，然后栈帧会被压入栈中。当方法执行完毕后，栈帧会从栈中移除。

**3. 本地方法栈**

- 本地方法栈是为 Java 调用**本地（native）方法**服务的，由 C/C++ 编写。

- 在本地方法栈中，主要存放了 native 方法的局部变量、动态链接和方法出口等信息。当一个 Java 程序调用一个 native 方法时，JVM 会切换到本地方法栈来执行这个方法。

**4. 堆**

- 堆是 JVM 中最大的一块内存区域，被所有线程共享，在 JVM 启动时创建，主要用来存储 new 出来的对象。

  ![二哥的 Java 进阶之路：堆](https://cdn.tobebetterjavaer.com/stutymore/neicun-jiegou-20231225154450.png)

- 从内存回收的角度来看，由于垃圾收集器大部分都是基于分代收集理论设计的，所以堆也会被划分为`新生代`、`老年代`、`Eden空间`、`From Survivor空间`、`To Survivor空间`等。

![三分恶面渣逆袭：Java 堆内存结构](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/jvm-5.png)

- 从 JDK 7 开始，JVM 已经默认开启逃逸分析了，意味着如果某些方法中的对象引用没有被返回或者未被方法体外使用（也就是未逃逸出去），那么**对象可以直接在栈上分配内存**。



**5. 方法区**

- 方法区并不真实存在，属于 Java 虚拟机规范中的一个逻辑概念，用于存储已被 JVM 加载的类信息、常量、静态变量、即时编译器编译后的代码缓存等。

- 在 HotSpot 虚拟机中，方法区的实现称为永久代（PermGen），但在 Java 8 及之后的版本中，已经被元空间（Metaspace）所替代。

  

![image-20250201233506455](https://github.com/user-attachments/assets/8a7f74a1-10a3-4f1b-b7aa-5e2c09d1054b)



## 什么是native方法

Native 方法是在 Java 中通过 native 关键字声明的，用于调用非 Java 语言（如 C/C++）编写的代码。Java 可以通过 JNI（Java Native Interface）与底层系统、硬件设备、或高性能的本地库进行交互。





## 一个什么都没有的空方法，完全空的参数什么都没有，那局部变量表里有没有变量？

- 对于静态方法，由于不需要访问实例对象（this），因此在局部变量表中不会有任何变量。
- 对于非静态方法，即使是一个完全空的方法，局部变量表中也会有一个用于**存储 this 引用**的变量。this 引用指向当前实例对象，在方法调用时被隐式传入。



##  JVM中堆和栈的区别是什么

- 堆属于线程共享的内存区域，几乎所有 new 出来的对象都会堆上分配，生命周期不由单个方法调用所决定，可以在方法调用结束后继续存在，直到不再被任何变量引用，**最后被垃圾收集器回收**。
- 栈属于线程私有的内存区域，主要存储局部变量、方法参数、对象引用等，通常随着方法调用的结束而自动释放，**不需要垃圾收集器处理**。



## JDK1.6、1.7、1.8内存区域的变化

主要体现在方法区的实现

- JDK1.6使用永久代实现方法区

  ![JDK 1.6内存区域](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/jvm-6.png)

- JDK1.7将字符串常量池、静态变量存放在堆上

  ![JDK 1.7内存区域](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/jvm-7.png)

- JDK1.8彻底干掉了永久代，在直接内存中划出一块区域作为**元空间**，运行时常量池、类常量池都移动到元空间

  ![JDK 1.8内存区域](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/jvm-8.png)

## 为什么使用元空间替代永久代作为方法区的实现

- 使用永久代来实现方法区的决定的设计导致了 Java 应用更容易遇到内存溢出的问题（永久代有-XX：MaxPermSize 的上限，即使不设置也有默认大小）



## 对象创建的过程了解吗

- 检查类是否已经被加载、解析和初始化过
  - 使用 new 关键字创建一个对象的时候，JVM 首先会检查 new 指令的参数是否能在常量池中定位到一个类的符号引用然后检查这个符号引用代表的类是否已被加载、解析和初始化过。如果没有，就先执行相应的类加载过程。（ **类只加载一次，后续对象创建不再触发类加载。**）

- 如果已经加载，JVM为新生对象分配内存
- 将分配到的内存空间初始化为零值
- 设置对象头，对象头里包含了对象是哪个类的实例、对象的哈希码、对象的 GC 分代年龄等信息
- JVM 会执行构造方法（`<init>`），将成员变量赋值为预期的值



## 对象销毁的过程了解吗

- 对象创建完成后，就可以通过引用来访问对象的方法和属性，当对象**不再被任何引用指向时**，对象就会变成垃圾。

- 垃圾收集器会通过可达性分析算法判断对象是否存活，如果对象不可达，就会被回收。
- 垃圾收集器会通过标记清除、标记复制、标记整理等算法来回收内存，将对象占用的内存空间释放出来。



## 什么是指针碰撞和空闲列表

在堆内存分配对象时，主要使用两种策略：指针碰撞和空闲列表。

![三分恶面渣逆袭：指针碰撞和空闲列表](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/jvm-10.png)

- 指针碰撞
  - 堆内存必须连续，一部分是已使用的空间，一部分是未使用的房间
  - 在分配内存时，Java 虚拟机维护一个指针，指向下一个可用的内存地址，每次分配内存时，只需要将指针向后移动（碰撞）一段距离，然后将这段内存分配给对象实例即可。
- 空闲列表
  - JVM 维护一个列表，记录堆中所有未占用的内存块，每个空间块都记录了大小和地址信息。
  - 当有新的对象请求内存时，JVM 会遍历空闲列表，寻找足够大的空间来存放新对象。



## JVM里new对象时，堆会发生抢占吗？JVM是怎么设计来保证线程安全的

**会发生抢占**，假设 JVM 虚拟机上，每一次 new 对象时，指针就会向右移动一个对象 size 的距离，一个线程正在给 A 对象分配内存，指针还没有来的及修改，另一个为 B 对象分配内存的线程，又引用了这个指针来分配内存，这就发生了抢占。

解决方法：

- 采用 CAS 分配重试的方式来保证更新操作的原子性
- 每个线程在 Java 堆中预先分配一小块内存，也就是**本地线程分配缓冲**，要分配内存的线程，先在本地缓冲区中分配，只有本地缓冲区用完了，分配新的缓存区时才需要**同步锁定**。



## 对象的内存布局是什么样的

对象的内存布局因JVM具体实现而异，在HotSpot中，对象在堆内存中的布局可以分为三部分：

**1. 对象头**

- 标记字（Mark Word）
  - 包含了对象自身的运行时数据，如哈希码（HashCode）、垃圾回收分代年龄、锁状态标志、线程持有的锁、偏向线程 ID 等信息。
  - 在 64 位操作系统下占 8 个字节，32 位操作系统下占 4 个字节。

- 类型指针
  - 指向对象所属类的元数据的指针，JVM 通过这个指针来确定对象的类。在开启了压缩指针的情况下，这个指针可以被压缩。
  - 在开启指针压缩的情况下占 4 个字节，否则占 8 个字节。（在 JDK 8 中，压缩指针默认是开启的，以减少 64 位应用中对象引用的内存占用）
- 数组长度
  - 如果对象是数组类型，还会有一个额外的数组长度字段。占 4 个字节。

**2. 实例数据**

- 存储了对象的具体信息，即在类中定义的各种字段数据（不包括由父类继承的字段）。这部分的大小取决于对象的属性和它们的类型（如 int、long、引用类型等）。JVM 会对这些数据进行**对齐**，以确保高效的访问速度。

**3. 对齐填充**

- 为了使对象的总大小是 8 字节的倍数（这在大多数现代计算机体系结构中是最优访问边界），JVM 可能会在对象末尾添加一些填充。

![三分恶面渣逆袭：对象的存储布局](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/jvm-12.png)



## Object a = new object()的大小

对象大小 = 对象头 + 实例数据 + 对齐填充

- 对象头的大小在 64 位 JVM 上是 16 字节（如果开启了压缩指针，就是 **12 字节**）  8字节的Mark Word 和 4字节的类型指针

- 实例数据的大小取决于对象的属性和它们的类型。对于`new Object()`来说，Object 类本身没有实例字段，因此这部分可能非常小或者为零。
- 对齐填充的大小取决于对象头和实例数据的大小，以确保对象的总大小是 8 字节的倍数。



`new Object()`的大小是 16 字节（12 字节的对象头 + 4 字节的对齐填充）。



## 对象怎么访问定位

JVM 通过 "对象引用" 访问对象的实际内存地址，不同JVM采用不同的对象访问方式，主要有两种

- 句柄池

  ![通过句柄访问对象](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/jvm-13.png)

  - JVM 在堆中维护一个 "句柄池（Handle Pool）"，对象引用存储的是 "句柄地址"，而句柄中包含对象的 "数据地址" 和 "类型地址"。

- 直接指针

  ![通过直接指针访问对象](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/jvm-14.png)

  - 对象引用存储的就是对象的 "实际地址"，不需要句柄池，直接指向堆中的对象数据。



使用句柄来访问的最大好处就是 reference 中存储的是稳定句柄地址，在对象被移动（垃圾收集时移动对象是非常普遍的行为）时只会改变句柄中的实例数据指针

使用直接指针来访问最大的好处就是速度更快，它节省了一次指针定位的时间开销



## JVM堆的内存分区了解吗

![三分恶面渣逆袭：Java堆内存划分](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/jvm-21.png)

- 新生代
  - 伊甸园区
  - 幸存者From
  - 幸存者To
- 老年代



## 新生代的内存区域划分

​	![新生代内存划分](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/jvm-25.png)

- 新生代的垃圾收集主要采用**标记复制**算法，因为新生代的存活对象比较少，每次复制少量的存活对象效率比较高

- 虚拟机将内存分为一块较大的 Eden 空间和两块较小的 Survivor 空间，每次分配内存只使用 Eden 和其中一块 Survivor。发生垃圾收集时，将 Eden 和 Survivor 中仍然存活的对象一次性复制到另外一块 Survivor 空间上，然后直接清理掉 Eden 和已用过的那块 Survivor 空间。
- 默认Eden和Survivor的大小比例是8 : 1 : 1



## 对象什么时候会进入老年代

**1. 长期存活的对象**

**2. 大对象直接进入老年代**

- 为了避免在年轻代中频繁复制大对象，直接在老年代分配可以减少在年轻代和老年代之间的数据复制。

**3. 动态对象年龄判定**

- 除了固定的年龄阈值，还会根据各个年龄段对象的存活大小和内存空间等因素动态调整对象的晋升策略。
- 比如说，在 Survivor 空间中相同年龄的所有对象大小总和大于 Survivor 空间的一半，那么年龄大于或等于该年龄的对象就可以直接进入老年代。



## 什么是Stop The World？什么是OopMap？什么是安全点

**Stop The World**

- 进行垃圾回收的过程中，会涉及对象的移动。为了保证对象引用更新的正确性，必须暂停所有的用户线程，像这样的停顿，虚拟机设计者形象描述为`Stop The World`。



**OopMap**

- OopMap（Object Offset Map，面向对象偏移映射）是 JVM 内部用于 **加速 GC（垃圾回收）** 的数据结构。
- 录了方法栈帧、寄存器等位置上哪些是 **指向对象的引用**，从而帮助 GC 在扫描时快速找到存活对象。



**安全点**

- 用户程序执行时并非在代码指令流的任意位置都能够在停顿下来开始垃圾收集，而是必须是执行到安全点才能够暂停。
- 安全点的位置主要在：
  - 1.循环的末尾（非 counted 循环）
  - 2.方法临返回前 / 调用方法的 call 指令后
  - 3.可能抛异常的位置



## 内存溢出和内存泄漏是什么意思

**内存溢出OOM**

- 指当程序请求分配内存时，由于没有足够的内存空间满足其需求，从而触发的错误。在 Java 中，这种情况会抛出 `OutOfMemoryError`。

**内存泄漏**

- 指程序在使用完内存后，未能释放已分配的内存空间，导致这部分内存无法再被使用。随着时间的推移，内存泄漏会导致可用内存逐渐减少，最终可能导致内存溢出。
- 在 Java 中，内存泄漏通常发生在长期存活的对象持有短期存活对象的引用，而长期存活的对象又没有及时释放对短期存活对象的引用，从而导致短期存活对象无法被回收。



## Java哪些内存区域会发生OOM

**1. 堆**

**2. 元空间**

**3. 线程栈**

**4.  直接内存**



## 如何排查内存泄漏问题

严重的**内存泄漏**往往伴随频繁的 **Full GC**，所以排查内存泄漏问题时，可以从 Full GC 入手。

- 第一步，使用 `jps -l` 查看运行的 Java 进程 ID。

- 使用`jstat`命令观察Full GC的次数，如果发现 Full GC次数太多，但内存的使用率并没有下降，就很大概率存在内存泄漏了。

- 生成堆的快照文件（dump文件），使用图形化工具分析，观察内存占用最多的对象



## 如何排查内存溢出问题

- 使用`jstat`命令监控GC状态，看老年代的使用率
- 生成堆快照文件，交给可视化工具进行分析



## 什么情况下会发生栈溢出

![二哥的Java进阶之路：栈帧](https://cdn.tobebetterjavaer.com/stutymore/stack-frame-20231224090450.png)

当一个方法被调用时，JVM 会在栈中分配一个栈帧，用于存储该方法的执行信息。如果方法调用嵌套太深，栈帧不断压入栈中，最终会导致栈空间耗尽，抛出 StackOverflowError。







# 垃圾回收

## 如何判断对象是否可以被回收

**1. 引用计数法**

- 问题：

  ![image-20250202131522866](https://github.com/user-attachments/assets/dd8200c9-0d44-4a45-87c5-cc180cb2e2c0)


  这两个对象互相引用，但不再被其他对象引用，它们的引用计数不为零，因此不会被回收。

**2. 可达性分析算法**



## 可达性分析算法是什么

![三分恶面渣逆袭：GC Root](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/jvm-18.png)

可达性分析算法将对象分为两类：

- 垃圾回收的根对象（GC Root）：不可被回收
- 普通对象

如果从某个对象到GC Root对象是可达的，对象就不可被回收



## 哪些对象被称之为GC Root对象

- Java栈帧中的局部变量

  ```java
  public class StackReference {
      public void greet() {
          Object localVar = new Object(); // 这里的 localVar 是一个局部变量，存在于虚拟机栈中
          System.out.println(localVar.toString());
      }
  
      public static void main(String[] args) {
          new StackReference().greet();
      }
  }
  ```

  - 在 greet 方法中，localVar 是一个局部变量，存在于虚拟机栈中，可以被认为是 GC Roots。

  - 在 greet 方法执行期间，localVar 引用的对象是活跃的，因为它是从 GC Roots 可达的。

  - 当 greet 方法执行完毕后，localVar 的作用域结束，localVar 引用的 Object 对象不再由任何 GC Roots 引用（假设没有其他引用指向这个对象），因此它将有资格作为垃圾被回收掉 

- 方法区/元空间中的静态变量

  ```java
  public class StaticFieldReference {
      private static Object staticVar = new Object(); // 类静态变量
  
      public static void main(String[] args) {
          System.out.println(staticVar.toString());
      }
  }
  ```

  - StaticFieldReference 类中的 staticVar 引用了一个 Object 对象，这个引用存储在元空间，可以被认为是 GC Roots

  - 只要 StaticFieldReference 类未被卸载，staticVar 引用的对象都不会被垃圾回收。如果 StaticFieldReference 类被卸载（这通常发生在其类加载器被垃圾回收时），那么 staticVar 引用的对象也将有资格被垃圾回收（如果没有其他引用指向这个对象）

- 方法区/元空间中的常量引用

  ```java
  public class ConstantPoolReference {
      public static final String CONSTANT_STRING = "Hello, World"; // 常量，存在于运行时常量池中
      public static final Class<?> CONSTANT_CLASS = Object.class; // 类类型常量
  
      public static void main(String[] args) {
          System.out.println(CONSTANT_STRING);
          System.out.println(CONSTANT_CLASS.getName());
      }
  }
  ```

  - 在 ConstantPoolReference 中，CONSTANT_STRING 和 CONSTANT_CLASS 作为常量存储在运行时常量池。它们可以用来作为 GC Roots。

  - 这些常量引用的对象（字符串"Hello, World"和 Object.class 类对象）在常量池中，只要包含这些常量的 ConstantPoolReference 类未被卸载，这些对象就不会被垃圾回收。

- 监视器对象，用来保存同步锁synchronized关键字持有的对象

  ![image-20250202122906221](https://github.com/user-attachments/assets/1a1217a7-4a95-4896-a9de-62e4a61af95f)


- 本地方法栈中JNI的引用

  ![pecuyu：动态链接](https://cdn.tobebetterjavaer.com/stutymore/gc-20240321085719.png)

  ```java
  // 假设的JNI方法
  public native void nativeMethod();
  
  // 假设在C/C++中实现的本地方法
  /*
   * Class:     NativeExample
   * Method:    nativeMethod
   * Signature: ()V
   */
  JNIEXPORT void JNICALL Java_NativeExample_nativeMethod(JNIEnv *env, jobject thisObj) {
      jobject localRef = (*env)->NewObject(env, ...); // 在本地方法栈中创建JNI引用
      // localRef 引用的Java对象在本地方法执行期间是活跃的
  }
  ```

  - 在本地（C/C++）代码中，localRef 是对 Java 对象的一个 JNI 引用，它在本地方法执行期间保持 Java 对象活跃，可以被认为是 GC Roots。
  - 一旦 JNI 方法执行完毕，除非这个引用是全局的（Global Reference），否则它指向的对象将会被作为垃圾回收掉（假设没有其他地方再引用这个对象）。



## finalize()方法了解吗？有什么作用？

如果对象在进行可达性分析后发现没有与 GC Roots 相连接的引用链，那它将会被第一次标记，随后进行一次筛选，筛选的条件是此对象是否有必要执行 finalize()方法。如果对象在在 finalize()中成功拯救自己——只要重新与引用链上的任何一个对象建立关联即可，譬如把自己 （this 关键字）赋值给某个类变量或者对象的成员变量，那在第二次标记时它就”逃过一劫“；但是如果没有抓住这个机会，那么对象就真的要被回收了。



## 四种引用

**1. 强引用**

**2. 软引用**

- 垃圾回收后内存仍不够，回收

**3. 弱引用**

- 只要发生垃圾回收，不管内存是否充足，回收

**4. 虚引用**

- 垃圾回收时，回收

- 必须配合 `ReferenceQueue` ，使用对象被 GC 回收后，会被放入 `ReferenceQueue`，通知开发者进行资源释放

**5. 终结器引用**

- 必须配合 `ReferenceQueue`，当对象没有强引用时，GC 会先把对象放入 `ReferenceQueue`，然后调用 `finalize()` 进行清理。
- 专门用于 `Object.finalize()` 方法的调用，它在对象被垃圾回收（GC）之前会被 JVM 处理。
- 已被 Java 认为是低效且不可靠的方式，JDK 9 之后已经不推荐使用。



## 三种垃圾回收算法

**1. 标记清除算法**

![image-20250202230731812](https://github.com/user-attachments/assets/abaf52d1-15cb-4c39-9cd3-071945270dff)


- **优点**：速度快

- **缺点**：会产生很多内存碎片



**2. 标记整理算法**

![image-20250202230916924](https://github.com/user-attachments/assets/17e342b9-9967-4f93-b8eb-8431883d577b)


- **优点**：避免产生内存碎片

- **缺点**：需要内存移动，速度慢



**3. 复制算法**

![image-20250202231053966](https://github.com/user-attachments/assets/0be6ab00-5cd2-41fa-9ba0-2c160e3e0d38)


![image-20250202231109037](https://github.com/user-attachments/assets/df653f90-9c2a-4c26-8071-bfbbd08f96d0)


![image-20250202231123845](https://github.com/user-attachments/assets/91db6c22-8c32-46fd-8705-465ae339943c)


- **优点**：避免产生内存碎片

- **缺点**：占用双倍的内存空间





## 什么是分代垃圾回收

![二哥的 Java 进阶之路：Java 堆划分](https://cdn.tobebetterjavaer.com/stutymore/gc-20231227131241.png)

**新生代**：存放用完就能销毁的对象，垃圾回收比较频繁

- 伊甸园区：新创建出的对象放在伊甸园区
  - 当伊甸园区放不下时，会触发一次**Minor GC**（可达性算法 + 复制算法）
  - 复制幸存的对象到幸存区To中，且让这些对象的寿命+1，寿命达到一定的阈值 => **晋升到老年代**（最大寿命是15，4个bit）
  - 交换幸存区From和幸存区To
  - **Minor GC** 会引发 **stop the world**，暂停其它用户线程，等垃圾回收结束，用户线程才恢复运行
- 幸存区From / 幸存区To
  - 第二次**Minor GC**的时候也要清理幸存区中的对象
  - 将From中存活的移动到To
  - 清理From
  - 交换幸存区From和幸存区To
  - 当幸存区空间不足时，存活对象会直接进入老年代

**老年代**：存放长时间使用的对象，垃圾回收很久发生一次

- 当对象晋升到老年代，但老年代空间不足时，会先尝试**Minor GC**，如果还不足，触发**Full GC**， **stop the world**时间更长
- 如果老年代垃圾回收后还是不足，会触发OOM
- 如果大对象在新生代放不下但在老年代放的下，就会直接放到老年代中



## 为什么需要分代垃圾回收

新生代的对象生命周期短，使用复制算法可以快速回收。老年代的对象生命周期长，使用标记-整理算法可以减少移动对象的成本。



## 标记复制的标记过程和复制过程会不会停顿

在标记-复制算法 中，标记阶段和复制阶段都会触发STW。

- 标记阶段停顿是为了保证对象的引用关系不被修改。
- 复制阶段停顿是防止对象在复制过程中被修改。



## Minor GC、Major GC、Mixed GC、Full GC都是什么意思

**Minor GC** 也称为 Young GC，是指发生在年轻代（Young Generation）的垃圾收集。

**Major GC** 也称为 Old GC，主要指的是发生在老年代的垃圾收集。CMS 收集器的特有行为。

**Mixed GC** 是 G1 垃圾收集器特有的一种 GC 类型，它在一次 GC 中同时清理年轻代和部分老年代。

**Full GC** 是最彻底的垃圾收集，涉及整个 Java 堆和方法区（元空间）。它是最耗时的 GC，通常在 JVM 压力很大时发生。



## Full GC时怎么去清理的

Full GC 会从 GC Root 出发，标记所有可达对象。新生代使用复制算法，清空 Eden 区。老年代使用标记-整理算法，回收对象并消除碎片。

停顿时间较长（STW），会影响系统响应性能。



## Minor GC什么时候触发

如果 Eden 区没有足够的空间时，就会触发 Young GC 来清理新生代。



## Full GC什么时候触发

- 在进行Minor GC 的时候，如果发现`老年代可用的连续内存空间` < `新生代历次 Minor GC 后升入老年代的对象总和的平均大小`，说明本次 Minor GC 后升入老年代的对象大小，可能超过了老年代当前可用的内存空间，就会触发 Full GC。
- 执行 Minor GC 后老年代没有足够的内存空间存放转入的对象，会立即触发一次 Full GC。
- 方法区空间不足时，可能会触发
- `System.gc()`、`jmap -dump` 等命令会触发 full gc。



## 空间分配担保是什么

空间分配担保是指在进行 Minor GC（新生代垃圾回收）前，JVM 会确保老年代有足够的空间存放从新生代晋升的对象。如果老年代空间不足，可能会触发 Full GC。



## 垃圾回收器的分类

**1. 串行**（SerialGC）

- 单线程

- 堆内存较小，单核CPU

- 复制 + 标记整理

  ![image-20250203001057997](https://github.com/user-attachments/assets/43e54791-c1e0-4399-a892-d367872ddba5)


**2. 吞吐量优先**（ParallelGC）

- 多线程

- 堆内存较大，多核CPU

- 让单位时间内，stop the world的时间最短（垃圾回收总时间的占比）

- 垃圾回收时占用100%的CPU

- 复制 + 标记整理

  ![image-20250203001251008](https://github.com/user-attachments/assets/d1af4cdd-150a-4681-89df-7d069a3ad65b)


  ![吞吐量](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/jvm-31.png)

**3. 响应时间优先**（CMS）

- 多线程
- 堆内存较大，多核CPU
- 尽可能让单次的stop the world的时间最短（初始标记 + 并发标记 + 重新标记 + 并发清理）
- 并发
- 复制 + 标记清除，容易产生内存碎片，可能触发 Full GC。![image-20250203002014110]![image-20250203002014110](https://github.com/user-attachments/assets/849a8b10-42af-4319-b5eb-3a5b59ec8c0d)




## CMS垃圾回收器的详细过程

CMS使用**标记清除**算法进行垃圾收集，分四大步：

- **初始标记**
  - 标记所有从 GC Roots 直接可达的对象，这个阶段需要 STW，但速度很快。
- **并发标记**
  - 从初始标记的对象出发，遍历所有对象，标记所有可达的对象。这个阶段是并发进行的。
- **重新标记**
  - 完成剩余的标记工作，包括处理并发阶段遗留下来的少量变动，这个阶段通常需要短暂的 STW 停顿。
- **并发清除**
  - 清除未被标记的对象，回收它们占用的内存空间。



## 重新标记具体是怎么执行的

**三色标记法**

- **白色**：尚未访问的对象。垃圾回收结束后，仍然为白色的对象会被认为是不可达的对象，可以回收。
- **灰色**：已经访问到但未标记完其引用的对象。灰色对象是需要进一步处理的。
- **黑色**：已经访问到并且其所有引用对象都已经标记过。黑色对象是完全处理过的，不需要再处理。

![Java全栈架构师：三色标记法](https://cdn.tobebetterjavaer.com/stutymore/jvm-20240816132235.png)

当并发标记阶段时引用发生变化，会触发写屏障将变化的对象标记为灰色放入队列中，方便后续重新标记阶段处理。



## 相关VM参数

![image-20250202233750768](https://github.com/user-attachments/assets/c20898e3-6c64-4000-9148-c0a11b86121f)




## 什么是G1垃圾回收器

![有梦想的肥宅：G1](https://cdn.tobebetterjavaer.com/stutymore/gc-collector-20231228213824.png)

- 同时注重吞吐量和低延迟
- 适合超大堆内存，会将堆划分成多个大小相等的区域
- 整体上是**标记 + 整理**算法；两个区域之间**复制**算法
- 优点是停顿时间可控



## G1垃圾回收阶段

![image-20250203110722457](https://github.com/user-attachments/assets/3e1477fe-b3e1-43c2-8dd9-ad1d52dbd667)


**1. Young Collection**

- 会STW

  ![image-20250203111151368](https://github.com/user-attachments/assets/f552436b-761a-48a9-8f36-919997980793)


  ![image-20250203111203829](https://github.com/user-attachments/assets/482bb323-a37d-40c7-89f9-421c1ba0c9fa)


  ![image-20250203111224474](https://github.com/user-attachments/assets/a8662c6a-0050-4616-bb89-95612dc4b1fa)




**2. Young Collection + CM**

- 在Minor GC时会进行GC Root的初始标记

- 老年代占用堆空间比例达到阈值时，进行并发标记（不会STW）

  ![image-20250203111514335](https://github.com/user-attachments/assets/6601830e-772b-41e1-b7c6-fad6aaee5fff)


**3. Mixed Collection**

- 会对E、S、O进行全面垃圾回收（根据最大暂停时间有选择的回收老年代，**选择回收价值最高的老年代**）
  - 最终标记会STW
  - 拷贝存活会STW

![image-20250203111741363](https://github.com/user-attachments/assets/a1ea1423-0ef6-47a2-98f8-3b08c8c0b65e)




为了提高新生代寻找GC Root引用的效率，会记录老年代中的引用

![image-20250203112604457](https://github.com/user-attachments/assets/419e85c8-d226-48c7-97ea-961ca0375c0d)




## 你们线上用的什么垃圾回收器？为什么要用它？

我们生产环境中采用了设计比较优秀的 G1 垃圾收集器，因为它不仅能满足低停顿的要求，而且解决了 CMS 的浮动垃圾问题、内存碎片问题，能够同时考虑吞吐量和暂停时间。



# JVM调优

## 有哪些常用的命令行性能监控和故障处理工具

- 操作系统工具
  - top：显示系统整体资源使用情况
  - vmstat：监控内存和CPU
  - iostat：监控IO使用
  - netstat：监控网络使用
- JDK性能监控工具
  - jps：查看jvm进程
  - jstat：查看jvm运行时信息
  - jinfo：查看虚拟机配置
  - jmap：导出内存快照
  - jstack：虚拟机堆栈跟踪

**`jmap -heap <pid>` 查看堆内存摘要，包括新生代、老年代、元空间等。**

![二哥的Java 进阶之路：jmap -heap](https://cdn.tobebetterjavaer.com/stutymore/jvm-20240806093153.png)

**生成堆转储文件：`jmap -dump:format=b,file=<path> <pid>`。**

![二哥的Java 进阶之路：jmap -dump](https://cdn.tobebetterjavaer.com/stutymore/console-tools-20240106184317.png)



## 用过哪些可视化的性能监控和故障处理工具

**1. JConsole**

**2. VisualVM**

**3. Arthas**



## 如何进行JVM调优

- JVM的堆内存如果设置过小，可能会导致频繁的垃圾回收
  - 调整了 -Xms 和-Xmx 参数

- 使用 VisualVM 定期观察和分析 GC 日志，如果发现频繁的 Full GC，就需要特别关注老年代的使用情况。
- 通过分析 Heap dump 寻找内存泄漏的源头，看看是否有未关闭的资源，长生命周期的大对象
- 之后，就要进行代码优化了，比如说减少大对象的创建、优化数据结构的使用方式、减少不必要的对象持有等。



## CPU占用过高怎么排查

- 首先使用top命令查看CPU占用情况，找到占用CPU较高的进程ID

  ![haikuotiankongdong：top 命令结果](https://cdn.tobebetterjavaer.com/stutymore/jvm-20240527111502.png)

- 接着，使用 jstack 命令查看对应进程的线程堆栈信息。

  ```shell
  jstack -l <pid> > thread-dump.txt
  ```

- 然后再使用 top 命令查看进程中线程的占用情况，找到占用 CPU 较高的线程 ID。

  ```shell
  top -H -p <pid>
  ```

  ![haikuotiankongdong：Java 进程中的线程情况](https://cdn.tobebetterjavaer.com/stutymore/jvm-20240527111356.png)

- 最后，根据堆栈信息定位到具体的业务方法，查看是否有死循环、频繁的垃圾回收（GC）、资源竞争（如锁竞争）导致的上下文频繁切换等问题。



## 内存飙高问题怎么排查

内存飚高一般是因为创建了大量的 Java 对象所导致的，如果持续飙高则说明垃圾回收跟不上对象创建的速度，或者内存泄漏导致对象无法回收。

- 第一，先观察垃圾回收的情况，可以通过 `jstat -gc PID 1000` 查看 GC 次数和时间。
- 第二步，通过 jmap 命令 dump 出堆内存信息。
- 第三步，使用可视化工具分析 dump 文件，比如说 VisualVM，找到占用内存高的对象，再找到创建该对象的业务代码位置，从代码和业务场景中定位具体问题。



## 为什么会频繁发生Minor GC

- 新生代空间太小

  - 可以通过 -Xmn 增加新生代的大小

  ```shell
  java -Xmn256m your-app.jar
  ```

  

- 对象创建过快

- Survivor区太小，对象太早晋升老年代

  - 可以通过 `-XX:SurvivorRatio` 参数调整 Eden 和 Survivor 的比例。默认比例是 8:1，表示 8 个空间用于 Eden，1 个空间用于 Survivor 区。

  ```shell
  -XX:SurvivorRatio=6
  ```



## 为什么会频繁发生Full GC

- 大对象直接分配到老年代
  - `-XX:PretenureSizeThreshold` 参数设置大对象直接进入老年代的阈值。
- 程序中存在内存泄漏
  - 通过分析堆内存 dump 文件找到内存泄漏的对象，再找到内存泄漏的代码位置
- 一些长生命周期的对象进入到了老年代，导致老年代空间不足
  - 及时释放资源，比如说 ThreadLocal、数据库连接、IO 资源等。
- 不合理的 GC 参数配置也导致 GC 频率过高。比如说新生代的空间设置过小



# 类加载机制

## 类的加载机制是什么

- JVM的操作对象是Class文件，JVM把Class文件中 描述类的数据结构加载到内存中，并对数据进行校验、解析和初始化，最终形成可以被JVM直接使用的类型，这个过程被称为类加载机制。

- **类加载器**：负责加载类文件，将类文件加载到内存中，生成Class对象
- **类加载过程**：加载、验证、准备、解析和初始化
- **双亲委派机制**





## 什么是类加载器

![image-20250202000135012](https://github.com/user-attachments/assets/0001c38f-68fe-41e2-a4e2-795d0cf17c33)


- 类加载器（ClassLoader）是JVM提供给应用程序去实现获取类和接口字节码数据的技术



## JDK8及之前和之后的版本中的类加载器有什么区别

- JDK8及之前
  - **启动类加载器Bootstrap**：加载Java中最核心的类，如String
    - 默认加载Java安装目录/jre/lib下的类文件
  - **扩展类加载器Extension**：扩展Java中比较通用但是不重要 的类
    - 默认加载Java安装目录/jre/lib/ext下的类文件
  - **应用程序类加载器Application**：加载应用使用的类，如第三方依赖、自定义的类

- JDK8之后

  - 引入了module模块化的概念，从jmod文件中进行加载

    ![image-20250202104527167](https://github.com/user-attachments/assets/1e7a193b-442f-4dd4-b68e-0bf8a55552ac)


  - 启动类加载器改为使用Java编写，从模块中找到要加载的字节码资源文件
  - 扩展类加载器被替换成了**平台类加载器(Platform ClassLoader)**，遵循模块化方式加载字节码文件



## 类加载过程是什么样的

**1. 加载**

- 将.class文件加载到JVM，在方法区创建Class对象

**2. 验证**

- 检查.class文件是否符合JVM规范

**3. 准备**

- 为静态变量（`static`变量）分配内存，并初始化默认值（不会执行static赋值或static代码块）

**4. 解析**

- 把类、方法、字段的**符号引用**转换为**直接引用**

**5. 初始化**

- 执行static变量赋值和static代码块



## 一个类什么时候会被加载

**1. 访问类的static变量或static方法**

**2. 创建类的实例**

**3. 使用Class.forName()**

**4. JVM入口类**



## 什么是双亲委派机制

- 由于JVM中有多个类加载器，双亲委派机制的核心是解决一个类到底由谁加载的问题

- 当一个类加载器接收到加载类的任务时，会自底向上查找是否加载过，再由顶向下进行加载

![image-20250202004528115](https://github.com/user-attachments/assets/8e9d1ade-bd68-4cff-83b4-79943cb24c9a)




## 为什么要用双亲委派机制

**1. 避免类的重复加载**

**2. 保证核心类库的安全性**

- 如`java.lang.*`只能由Bootstrap ClassLoader加载，防止被篡改





## 如何打破双亲委派机制

**1. 自定义类加载器**

- 重写`loadClass()`方法替换掉原始方法中的双亲委派机制的部分

  

- 正确的去实现一个自定义类加载器（不打破双亲委派机制）的方式是重写`findClass()`方法

**2. 线程上下文类加载器（有争议，实际上并没有打破）**

JDBC案例

![image-20250202103237528](https://github.com/user-attachments/assets/963514fc-b760-4bb5-bdd9-4bb2308ee179)


- 启动类加载器加载DriverManager

- 在初始化DriverManager时，通过SPI机制加载jar包中的mysql驱动

- SPI中利用了线程上下文类加载器（应用程序类加载器）去加载类并创建对象

  ![image-20250202103718778](https://github.com/user-attachments/assets/fa9fb736-97db-48d9-8f31-f24e8fb23230)


**3. Osgi框架的类加载器**

- 存在同级之间的类加载器的委托加载

  

## 两个自定义类加载器加载相同类限定名的类，不会冲突吗

不会冲突，在同一个JVM中，只有**相同类加载器 + 相同的类限定名**才会被认为是同一个类。



## 解释执行和编译执行的区别

**解释**：将源代码**逐行**转换为机器码

**编译**：将源代码**一次性**转换为机器码



**解释执行**：程序**运行时**，将源代码**逐行**转换为机器码，然后执行。

**编译执行**：程序**运行前**，将源代码**一次性**转换为机器码，然后执行。



- Java 一般被称为“解释型语言”，因为 Java 代码在执行前，需要先将源代码编译成字节码，然后在运行时，再由 JVM 的解释器“逐行”将字节码转换为机器码，然后执行。

- 这也是 Java 被诟病“慢”的主要原因。

- 但 JIT 的出现打破了这种刻板印象，JVM 会将热点代码（即运行频率高的代码）编译后放入 CodeCache，当下次执行再遇到这段代码时，会从 CodeCache 中直接读取机器码，然后执行。这大大提升了 Java 的执行效率。

  

# 参考资料

- P48 - P95 （垃圾回收到GC调优）
- P150 - P158 （类加载器）
- P175 - P184 （从CAS看到Synchronized）



