# JVM

##   初始JVM

JVM本质上是一个运行在计算机上的程序，它的职责是运行Java字节码文件

![image-20241214111901655](../../../AppData/Roaming/Typora/typora-user-images/image-20241214111901655.png)

**JVM三大核心功能**

- 解释和运行

- 内存管理

- 即时编译

  ![image-20241214112200828](../../../AppData/Roaming/Typora/typora-user-images/image-20241214112200828.png)



**常见的JVM**

![image-20241214112619456](../../../AppData/Roaming/Typora/typora-user-images/image-20241214112619456.png)



## 字节码文件详解

**Java虚拟机的组成**

![image-20241214132831726](../../../AppData/Roaming/Typora/typora-user-images/image-20241214132831726.png)





**字节码文件的组成**

- 基础信息

  - 魔数：Java字节码文件中，将文件头称为魔数

    <img src="../../../AppData/Roaming/Typora/typora-user-images/image-20241214134057990.png" alt="image-20241214134057990" style="zoom:50%;" />

  - 主副版本号：编译字节码文件的JDK版本号

    ![image-20241214134312435](../../../AppData/Roaming/Typora/typora-user-images/image-20241214134312435.png)

- 常量池：避免相同的内容重复定义，节省空间

  <img src="../../../AppData/Roaming/Typora/typora-user-images/image-20241214135712016.png" alt="image-20241214135712016" style="zoom:50%;" />

  

- 字段

- 方法

  iconst_0: 将0放入操作数栈中

  istore_1: 将操作数栈的值放到下标为1的局部变量表中

  iload_1: 将局部变量表中下标为1的元素复制一份到操作数栈中

  - i++

    ![image-20241214140719687](../../../AppData/Roaming/Typora/typora-user-images/image-20241214140719687.png)

  - ++i

    ![image-20241214140739088](../../../AppData/Roaming/Typora/typora-user-images/image-20241214140739088.png)

  

- 属性



**类的生命周期**

- 生命周期概述

  类的生命周期描述了一个类加载、使用、卸载的整个过程。

- 加载阶段

  ![image-20241214162736993](../../../AppData/Roaming/Typora/typora-user-images/image-20241214162736993.png)

  2.类加载器加载完类后，Java虚拟机会将字节码中的信息保存到方法区中

  ![image-20241214162958503](../../../AppData/Roaming/Typora/typora-user-images/image-20241214162958503.png)

  ![image-20241214163100972](../../../AppData/Roaming/Typora/typora-user-images/image-20241214163100972.png)

- 连接阶段

  - 验证

    验证内容是否满足《Java虚拟机规范》

    ![image-20241214164608693](../../../AppData/Roaming/Typora/typora-user-images/image-20241214164608693.png)

  - 准备

    给静态变量赋初值

    ![image-20241214165315382](../../../AppData/Roaming/Typora/typora-user-images/image-20241214165315382.png)

    ![image-20241214165404098](../../../AppData/Roaming/Typora/typora-user-images/image-20241214165404098.png)

  - 解析

    将常量池中的符号引用替换成指向内存的直接引用



- 初始化阶段（最重要）

  ![image-20241214170000039](../../../AppData/Roaming/Typora/typora-user-images/image-20241214170000039.png)

  clinit方法中的执行顺序与Java中编写的顺序是一致的

  ![image-20241214170624853](../../../AppData/Roaming/Typora/typora-user-images/image-20241214170624853.png)

  ![image-20241214171230884](../../../AppData/Roaming/Typora/typora-user-images/image-20241214171230884.png)

  ![image-20241214171425338](../../../AppData/Roaming/Typora/typora-user-images/image-20241214171425338.png)

 	



- 卸载阶段

  

**类加载器**

![image-20241215162339872](../../../AppData/Roaming/Typora/typora-user-images/image-20241215162339872.png)

- 分类

  ![image-20241215162615363](../../../AppData/Roaming/Typora/typora-user-images/image-20241215162615363.png)

  ![image-20241215162822259](../../../AppData/Roaming/Typora/typora-user-images/image-20241215162822259.png)

  ![image-20241215164434589](../../../AppData/Roaming/Typora/typora-user-images/image-20241215164434589.png)

  - 启动类加载器

    ![image-20241215164326379](../../../AppData/Roaming/Typora/typora-user-images/image-20241215164326379.png)

  - 扩展类加载器

    ![image-20241215164708529](../../../AppData/Roaming/Typora/typora-user-images/image-20241215164708529.png)

  - 应用程序类加载器

    应用程序类加载器的加载内容覆盖了启动类加载器和扩展类加载器

- ==类加载器的双亲委派机制==

  由于Java虚拟机中有多个类加载器，双亲委派机制的核心是解决一个类到底由谁加载的问题。

  <img src="../../../AppData/Roaming/Typora/typora-user-images/image-20241215165632560.png" alt="image-20241215165632560" style="zoom:67%;" />

  ![image-20241215165821130](../../../AppData/Roaming/Typora/typora-user-images/image-20241215165821130.png)

  优先级：启动类加载器 > 扩展类加载器 > 应用程序类加载器

  ![image-20241215170545154](../../../AppData/Roaming/Typora/typora-user-images/image-20241215170545154.png)

- 打破双亲委派机制

  ![image-20241215170737787](../../../AppData/Roaming/Typora/typora-user-images/image-20241215170737787.png)

  ![image-20241215171428611](../../../AppData/Roaming/Typora/typora-user-images/image-20241215171428611.png)

  - 自定义类加载器

     ![image-20241215171831185](../../../AppData/Roaming/Typora/typora-user-images/image-20241215171831185.png)

    <img src="../../../AppData/Roaming/Typora/typora-user-images/image-20241215172112757.png" alt="image-20241215172112757" style="zoom:50%;" />

    <img src="../../../AppData/Roaming/Typora/typora-user-images/image-20241215172310795.png" alt="image-20241215172310795" style="zoom:50%;" />

    ![image-20241215172525432](../../../AppData/Roaming/Typora/typora-user-images/image-20241215172525432.png)

  

  - 线程上下文类加载器

    ![image-20241215172712864](../../../AppData/Roaming/Typora/typora-user-images/image-20241215172712864.png)

    ![image-20241215173117076](../../../AppData/Roaming/Typora/typora-user-images/image-20241215173117076.png)<img src="../../../AppData/Roaming/Typora/typora-user-images/image-20241215173350219.png" alt="image-20241215173350219" style="zoom: 50%;" />

    ![image-20241215173552607](../../../AppData/Roaming/Typora/typora-user-images/image-20241215173552607.png)

    ![image-20241215173751757](../../../AppData/Roaming/Typora/typora-user-images/image-20241215173751757.png)

  - Osgi框架的类加载器

    ![image-20241215174334861](../../../AppData/Roaming/Typora/typora-user-images/image-20241215174334861.png)

    



- JDK9之后的类加载器

  ![image-20241215175147983](../../../AppData/Roaming/Typora/typora-user-images/image-20241215175147983.png)

  ![image-20241215175110729](../../../AppData/Roaming/Typora/typora-user-images/image-20241215175110729.png)

  ![image-20241215175255635](../../../AppData/Roaming/Typora/typora-user-images/image-20241215175255635.png)

  

  

## JVM的内存区域

![image-20241215175843908](../../../AppData/Roaming/Typora/typora-user-images/image-20241215175843908.png)

线程不共享：每个线程单独一份，单独释放

线程共享：存在线程安全问题



**程序计数器**

也叫PC寄存器

- 每个线程会通过程序计数器记录当前要执行的字节码指令的地址。

- 在多线程情况下，Java虚拟机需要通过程序计数器记录CPU切换前解释执行到哪一句指令并继续解释运行。

![image-20241215180628445](../../../AppData/Roaming/Typora/typora-user-images/image-20241215180628445.png)

**栈**

- Java虚拟机栈

  ​	Java虚拟机栈采用栈的数据结构来管理方法调用中的基本数据，先进后出，每一个方法的调用使用一个栈帧来保存。

  ![image-20241215181131395](../../../AppData/Roaming/Typora/typora-user-images/image-20241215181131395.png)

  - 栈帧的组成

    - 局部变量表

      ![image-20241215181433800](../../../AppData/Roaming/Typora/typora-user-images/image-20241215181433800.png)

      ![image-20241215181612721](../../../AppData/Roaming/Typora/typora-user-images/image-20241215181612721.png)

      ![image-20241215181622142](../../../AppData/Roaming/Typora/typora-user-images/image-20241215181622142.png)

    - 操作数栈

      ![image-20241215181849711](../../../AppData/Roaming/Typora/typora-user-images/image-20241215181849711.png)

    - 帧数据

      ![image-20241215182055172](../../../AppData/Roaming/Typora/typora-user-images/image-20241215182055172.png)

      ![image-20241215182205959](../../../AppData/Roaming/Typora/typora-user-images/image-20241215182205959.png)

      ![image-20241215182249208](../../../AppData/Roaming/Typora/typora-user-images/image-20241215182249208.png)

  - 内存溢出

    ![image-20241215193547913](../../../AppData/Roaming/Typora/typora-user-images/image-20241215193547913.png)

    ![image-20241215193622170](../../../AppData/Roaming/Typora/typora-user-images/image-20241215193622170.png)

    ![image-20241215194146061](../../../AppData/Roaming/Typora/typora-user-images/image-20241215194146061.png)

    ![image-20241215194227455](../../../AppData/Roaming/Typora/typora-user-images/image-20241215194227455.png)



**堆**

![image-20241216102657066](../../../AppData/Roaming/Typora/typora-user-images/image-20241216102657066.png)

![image-20241216103042156](../../../AppData/Roaming/Typora/typora-user-images/image-20241216103042156.png)

随着堆中的对象增多，当total可以使用的内存即将不足时，java虚拟机会继续分配内存给堆；如果堆内存不足，java虚拟机就会不断的分配内存，total值会变大。total最多只能与max相等。

![image-20241216103853293](../../../AppData/Roaming/Typora/typora-user-images/image-20241216103853293.png)

![image-20241216104008449](../../../AppData/Roaming/Typora/typora-user-images/image-20241216104008449.png)

![image-20241216104118834](../../../AppData/Roaming/Typora/typora-user-images/image-20241216104118834.png)

![image-20241216104206570](../../../AppData/Roaming/Typora/typora-user-images/image-20241216104206570.png)





**方法区**

方法区是存放基础信息的位置，线程共享，主要包含三部分内容：类的元信息、运行时常量池、字符串常量池

![image-20241216104756355](../../../AppData/Roaming/Typora/typora-user-images/image-20241216104756355.png)

![image-20241216105508367](../../../AppData/Roaming/Typora/typora-user-images/image-20241216105508367.png)

- 类的元信息

  ![image-20241216104612985](../../../AppData/Roaming/Typora/typora-user-images/image-20241216104612985.png)

- 运行时常量池

  ![image-20241216104707429](../../../AppData/Roaming/Typora/typora-user-images/image-20241216104707429.png)

- 字符串常量池

  ![image-20241216110008633](../../../AppData/Roaming/Typora/typora-user-images/image-20241216110008633.png)

  ![image-20241216110144678](../../../AppData/Roaming/Typora/typora-user-images/image-20241216110144678.png)

  ![image-20241216110356820](../../../AppData/Roaming/Typora/typora-user-images/image-20241216110356820.png)

  ![image-20241216110517599](../../../AppData/Roaming/Typora/typora-user-images/image-20241216110517599.png)

  一个是变量，一个是常量

  ![image-20241216110906538](../../../AppData/Roaming/Typora/typora-user-images/image-20241216110906538.png)

  ![image-20241216111029702](../../../AppData/Roaming/Typora/typora-user-images/image-20241216111029702.png)

  

  ![image-20241216111121372](../../../AppData/Roaming/Typora/typora-user-images/image-20241216111121372.png)

  ![image-20241216111136706](../../../AppData/Roaming/Typora/typora-user-images/image-20241216111136706.png)

  

​	

**直接内存**

![image-20241216111433753](../../../AppData/Roaming/Typora/typora-user-images/image-20241216111433753.png)

![image-20241216111704475](../../../AppData/Roaming/Typora/typora-user-images/image-20241216111704475.png)





## JVM的垃圾回收

**自动垃圾回收**

- C/C++的内存管理

  C/C++没有自动垃圾回收机制，一个对象如果不再使用，需要手动释放，否则就会出现**内存泄漏**。

  **内存泄漏**指的是不再使用的对象在系统中未被回收，内存泄漏的积累可能会导致内存溢出。

- Java的内存管理

  ![image-20241216112708233](../../../AppData/Roaming/Typora/typora-user-images/image-20241216112708233.png)

​	线程不共享的部分（程序计数器、Java虚拟机栈、本地方法栈），都是	伴随着线程的创建和创建，线程的销毁而销毁。而方法的栈帧在执行完	方法之后就会自动弹出栈并释放掉对应的内存。



- 方法区的回收

   ![image-20241216113322998](../../../AppData/Roaming/Typora/typora-user-images/image-20241216113322998.png)

  ![image-20241216113450558](../../../AppData/Roaming/Typora/typora-user-images/image-20241216113450558.png)

  ![image-20241216113616416](../../../AppData/Roaming/Typora/typora-user-images/image-20241216113616416.png)

  

  ![image-20241216114023261](../../../AppData/Roaming/Typora/typora-user-images/image-20241216114023261.png)

  

- 堆回收

  ![image-20241216115318520](../../../AppData/Roaming/Typora/typora-user-images/image-20241216115318520.png)

  ![image-20241216115443309](../../../AppData/Roaming/Typora/typora-user-images/image-20241216115443309.png)

  ![image-20241216115531942](../../../AppData/Roaming/Typora/typora-user-images/image-20241216115531942.png)

  

  - 引用计数法和可达性分析法（判断堆上的对象有没有被引用）

    Java虚拟机使用的是可达性分析算法，而不是引用计数法

    **引用计数法**

    ![image-20241216115643766](../../../AppData/Roaming/Typora/typora-user-images/image-20241216115643766.png)![image-20241216115951676](../../../AppData/Roaming/Typora/typora-user-images/image-20241216115951676.png)

    

    **可达性分析法**

    ![image-20241216120417815](../../../AppData/Roaming/Typora/typora-user-images/image-20241216120417815.png)

    ![image-20241216120502789](../../../AppData/Roaming/Typora/typora-user-images/image-20241216120502789.png)

    ![image-20241216121049718](../../../AppData/Roaming/Typora/typora-user-images/image-20241216121049718.png)

    ![image-20241216121111660](../../../AppData/Roaming/Typora/typora-user-images/image-20241216121111660.png)

    ![image-20241216121219419](../../../AppData/Roaming/Typora/typora-user-images/image-20241216121219419.png)

    

  ​	

  

  - 五种对象引用

    - 强引用

    - 软引用

      ![image-20241216130827431](../../../AppData/Roaming/Typora/typora-user-images/image-20241216130827431.png)

      ![image-20241216131544617](../../../AppData/Roaming/Typora/typora-user-images/image-20241216131544617.png)

      

    - 弱引用

      ![image-20241216132019285](../../../AppData/Roaming/Typora/typora-user-images/image-20241216132019285.png)

    - 虚引用

      ![image-20241216132347589](../../../AppData/Roaming/Typora/typora-user-images/image-20241216132347589.png)

    - 终结器引用

      ![image-20241216132948448](../../../AppData/Roaming/Typora/typora-user-images/image-20241216132948448.png)

    

  - 垃圾回收算法

    java是如何实现垃圾回收的？

    1. 找到内存中存活的对象
    2. 释放不再存活对象的内存，使得程序能再次利用这部分空间

    ![image-20241217103316729](../../../AppData/Roaming/Typora/typora-user-images/image-20241217103316729.png)

    ![image-20241217103600523](../../../AppData/Roaming/Typora/typora-user-images/image-20241217103600523.png)

    ![image-20241217103814493](../../../AppData/Roaming/Typora/typora-user-images/image-20241217103814493.png)

    ![image-20241217104006696](../../../AppData/Roaming/Typora/typora-user-images/image-20241217104006696.png)

    这三个指标不可能兼容。

    

    - 标记清除算法

      ![image-20241217104124211](../../../AppData/Roaming/Typora/typora-user-images/image-20241217104124211.png)

      ![image-20241217104222038](../../../AppData/Roaming/Typora/typora-user-images/image-20241217104222038.png)

      ![image-20241217104301845](../../../AppData/Roaming/Typora/typora-user-images/image-20241217104301845.png)

    - 复制算法

      ![image-20241217104427139](../../../AppData/Roaming/Typora/typora-user-images/image-20241217104427139.png)

      ![image-20241217104529301](../../../AppData/Roaming/Typora/typora-user-images/image-20241217104529301.png)

    - 标记整理算法

      ![image-20241217104610153](../../../AppData/Roaming/Typora/typora-user-images/image-20241217104610153.png)

      ![image-20241217104625667](../../../AppData/Roaming/Typora/typora-user-images/image-20241217104625667.png)

    - 分代GC算法

      ![image-20241217105012667](../../../AppData/Roaming/Typora/typora-user-images/image-20241217105012667.png)

      ![image-20241217105510652](../../../AppData/Roaming/Typora/typora-user-images/image-20241217105510652.png)

      ![image-20241217105625889](../../../AppData/Roaming/Typora/typora-user-images/image-20241217105625889.png)

      ![image-20241217105956070](../../../AppData/Roaming/Typora/typora-user-images/image-20241217105956070.png)

      ![image-20241217110449336](../../../AppData/Roaming/Typora/typora-user-images/image-20241217110449336.png)

      ![image-20241217110546929](../../../AppData/Roaming/Typora/typora-user-images/image-20241217110546929.png)

  - 垃圾回收器

    ![image-20241217110752122](../../../AppData/Roaming/Typora/typora-user-images/image-20241217110752122.png)

    ![image-20241217111123842](../../../AppData/Roaming/Typora/typora-user-images/image-20241217111123842.png)

    ![image-20241217111204858](../../../AppData/Roaming/Typora/typora-user-images/image-20241217111204858.png)

    ![image-20241217111403120](../../../AppData/Roaming/Typora/typora-user-images/image-20241217111403120.png)

      ![image-20241217111636576](../../../AppData/Roaming/Typora/typora-user-images/image-20241217111636576.png)

    ![image-20241217111858023](../../../AppData/Roaming/Typora/typora-user-images/image-20241217111858023.png)

    ![image-20241217111949335](../../../AppData/Roaming/Typora/typora-user-images/image-20241217111949335.png)

    ![image-20241217112243877](../../../AppData/Roaming/Typora/typora-user-images/image-20241217112243877.png)

    ![image-20241217112409933](../../../AppData/Roaming/Typora/typora-user-images/image-20241217112409933.png)

    ![image-20241217112703171](../../../AppData/Roaming/Typora/typora-user-images/image-20241217112703171.png)

    ![image-20241217113147630](../../../AppData/Roaming/Typora/typora-user-images/image-20241217113147630.png)

    ![image-20241217113213199](../../../AppData/Roaming/Typora/typora-user-images/image-20241217113213199.png)

    ![image-20241217113253167](../../../AppData/Roaming/Typora/typora-user-images/image-20241217113253167.png)

    ![image-20241217113318617](../../../AppData/Roaming/Typora/typora-user-images/image-20241217113318617.png)

    ![image-20241217113334902](../../../AppData/Roaming/Typora/typora-user-images/image-20241217113334902.png)

    ![image-20241217113410978](../../../AppData/Roaming/Typora/typora-user-images/image-20241217113410978.png)

    ![image-20241217113532573](../../../AppData/Roaming/Typora/typora-user-images/image-20241217113532573.png)

    <img src="../../../AppData/Roaming/Typora/typora-user-images/image-20241217113639601.png" alt="image-20241217113639601" style="zoom:50%;" />

    ![image-20241217113739561](../../../AppData/Roaming/Typora/typora-user-images/image-20241217113739561.png)

    ![image-20241217113809389](../../../AppData/Roaming/Typora/typora-user-images/image-20241217113809389.png)

    ![image-20241217113831011](../../../AppData/Roaming/Typora/typora-user-images/image-20241217113831011.png)

    ![image-20241217113952274](../../../AppData/Roaming/Typora/typora-user-images/image-20241217113952274.png)

    ![image-20241217114032156](../../../AppData/Roaming/Typora/typora-user-images/image-20241217114032156.png)

    ![image-20241217114047897](../../../AppData/Roaming/Typora/typora-user-images/image-20241217114047897.png)

    ![image-20241217114128848](../../../AppData/Roaming/Typora/typora-user-images/image-20241217114128848.png)

    

    

    ![image-20241217114422301](../../../AppData/Roaming/Typora/typora-user-images/image-20241217114422301.png)



## 内存调优

**内存溢出和内存泄漏**

![image-20241218102657381](../../../AppData/Roaming/Typora/typora-user-images/image-20241218102657381.png)

少量的内存泄漏可以 容忍，但是如果发生持续的内存泄漏，就像滚雪球越滚越大，不管有多大的内存迟早会被消耗完，最终导致的结果就是**内存溢出**。**但是产生内存溢出并不是只有内存泄漏这一种原因。**

![image-20241218103115827](../../../AppData/Roaming/Typora/typora-user-images/image-20241218103115827.png)

![image-20241218103144220](../../../AppData/Roaming/Typora/typora-user-images/image-20241218103144220.png)

**内存泄露的常见场景**

![image-20241218103206091](../../../AppData/Roaming/Typora/typora-user-images/image-20241218103206091.png)

![image-20241218103608250](../../../AppData/Roaming/Typora/typora-user-images/image-20241218103608250.png)

**解决内存溢出的思路**

1. 发现问题
2. 诊断原因
3. 修复问题
4. 测试验证

![image-20241218104004265](../../../AppData/Roaming/Typora/typora-user-images/image-20241218104004265.png)

![image-20241218104530738](../../../AppData/Roaming/Typora/typora-user-images/image-20241218104530738.png)

![image-20241218105432291](../../../AppData/Roaming/Typora/typora-user-images/image-20241218105432291.png)

![image-20241218105627143](../../../AppData/Roaming/Typora/typora-user-images/image-20241218105627143.png)

![image-20241218110231838](../../../AppData/Roaming/Typora/typora-user-images/image-20241218110231838.png)

 ![image-20241218111534732](../../../AppData/Roaming/Typora/typora-user-images/image-20241218111534732.png)

![image-20241218112005853](../../../AppData/Roaming/Typora/typora-user-images/image-20241218112005853.png)

![image-20241218112933387](../../../AppData/Roaming/Typora/typora-user-images/image-20241218112933387.png)

![image-20241218113304205](../../../AppData/Roaming/Typora/typora-user-images/image-20241218113304205.png)

![image-20241218113643763](../../../AppData/Roaming/Typora/typora-user-images/image-20241218113643763.png)

![image-20241218113736175](../../../AppData/Roaming/Typora/typora-user-images/image-20241218113736175.png)

![image-20241218114035168](../../../AppData/Roaming/Typora/typora-user-images/image-20241218114035168.png)

![image-20241218114502735](../../../AppData/Roaming/Typora/typora-user-images/image-20241218114502735.png)

![image-20241218115004926](../../../AppData/Roaming/Typora/typora-user-images/image-20241218115004926.png)

![image-20241218115242781](../../../AppData/Roaming/Typora/typora-user-images/image-20241218115242781.png)

![image-20241218123103045](../../../AppData/Roaming/Typora/typora-user-images/image-20241218123103045.png)

![image-20241218123443598](../../../AppData/Roaming/Typora/typora-user-images/image-20241218123443598.png)

![image-20241218123558850](../../../AppData/Roaming/Typora/typora-user-images/image-20241218123558850.png)

![image-20241218125350158](../../../AppData/Roaming/Typora/typora-user-images/image-20241218125350158.png)



**内存溢出的原因及解决方案**

![image-20241219104447074](../../../AppData/Roaming/Typora/typora-user-images/image-20241219104447074.png)

![image-20241219104502811](../../../AppData/Roaming/Typora/typora-user-images/image-20241219104502811.png)

![image-20241219104519910](../../../AppData/Roaming/Typora/typora-user-images/image-20241219104519910.png)

![image-20241219105302952](../../../AppData/Roaming/Typora/typora-user-images/image-20241219105302952.png)

![image-20241219110335103](../../../AppData/Roaming/Typora/typora-user-images/image-20241219110335103.png)

![image-20241219110423316](../../../AppData/Roaming/Typora/typora-user-images/image-20241219110423316.png)

![image-20241219110701720](../../../AppData/Roaming/Typora/typora-user-images/image-20241219110701720.png)

![image-20241219111004370](../../../AppData/Roaming/Typora/typora-user-images/image-20241219111004370.png)



## GC调优

![image-20241219111707912](../../../AppData/Roaming/Typora/typora-user-images/image-20241219111707912.png)

**GC调优的核心指标**

1. 吞吐量

   ![image-20241219112102704](../../../AppData/Roaming/Typora/typora-user-images/image-20241219112102704.png)

2. 延迟

   ![image-20241219112121088](../../../AppData/Roaming/Typora/typora-user-images/image-20241219112121088.png)

3. 内存使用量

   ![image-20241219112226227](../../../AppData/Roaming/Typora/typora-user-images/image-20241219112226227.png)



![image-20241219112354683](../../../AppData/Roaming/Typora/typora-user-images/image-20241219112354683.png)

![image-20241219112621551](../../../AppData/Roaming/Typora/typora-user-images/image-20241219112621551.png)

![image-20241219112849367](../../../AppData/Roaming/Typora/typora-user-images/image-20241219112849367.png)



![image-20241219113014039](../../../AppData/Roaming/Typora/typora-user-images/image-20241219113014039.png)

![image-20241219113128585](../../../AppData/Roaming/Typora/typora-user-images/image-20241219113128585.png)

![image-20241219113353897](../../../AppData/Roaming/Typora/typora-user-images/image-20241219113353897.png)

![image-20241219113627526](../../../AppData/Roaming/Typora/typora-user-images/image-20241219113627526.png)

![image-20241219113744425](../../../AppData/Roaming/Typora/typora-user-images/image-20241219113744425.png)

![image-20241219113842770](../../../AppData/Roaming/Typora/typora-user-images/image-20241219113842770.png)

![image-20241219113915273](../../../AppData/Roaming/Typora/typora-user-images/image-20241219113915273.png)

![image-20241219114128748](../../../AppData/Roaming/Typora/typora-user-images/image-20241219114128748.png)

![image-20241219114358898](../../../AppData/Roaming/Typora/typora-user-images/image-20241219114358898.png)

![image-20241219114620380](../../../AppData/Roaming/Typora/typora-user-images/image-20241219114620380.png)

![image-20241219115331881](../../../AppData/Roaming/Typora/typora-user-images/image-20241219115331881.png)

![image-20241219115349895](../../../AppData/Roaming/Typora/typora-user-images/image-20241219115349895.png)

![image-20241219115418127](../../../AppData/Roaming/Typora/typora-user-images/image-20241219115418127.png)

![image-20241219115521913](../../../AppData/Roaming/Typora/typora-user-images/image-20241219115521913.png)

![image-20241219115716634](../../../AppData/Roaming/Typora/typora-user-images/image-20241219115716634.png)



![image-20241221103459547](../../../AppData/Roaming/Typora/typora-user-images/image-20241221103459547.png)

![image-20241221103717193](../../../AppData/Roaming/Typora/typora-user-images/image-20241221103717193.png)

![image-20241221103735183](../../../AppData/Roaming/Typora/typora-user-images/image-20241221103735183.png)

![image-20241221104043243](../../../AppData/Roaming/Typora/typora-user-images/image-20241221104043243.png)



## 性能调优

![image-20241221110055921](../../../AppData/Roaming/Typora/typora-user-images/image-20241221110055921.png)

![image-20241221110113920](../../../AppData/Roaming/Typora/typora-user-images/image-20241221110113920.png)![image-20241221110328617](../../../AppData/Roaming/Typora/typora-user-images/image-20241221110328617.png)

![image-20241221110850759](../../../AppData/Roaming/Typora/typora-user-images/image-20241221110850759.png)

![image-20241221110910436](../../../AppData/Roaming/Typora/typora-user-images/image-20241221110910436.png)

![image-20241221110946289](../../../AppData/Roaming/Typora/typora-user-images/image-20241221110946289.png)

![image-20241221111009460](../../../AppData/Roaming/Typora/typora-user-images/image-20241221111009460.png)

![image-20241221111137851](../../../AppData/Roaming/Typora/typora-user-images/image-20241221111137851.png)

![image-20241221111248340](../../../AppData/Roaming/Typora/typora-user-images/image-20241221111248340.png)

![image-20241221111331019](../../../AppData/Roaming/Typora/typora-user-images/image-20241221111331019.png)

![image-20241221111833056](../../../AppData/Roaming/Typora/typora-user-images/image-20241221111833056.png)

![image-20241221111945390](../../../AppData/Roaming/Typora/typora-user-images/image-20241221111945390.png)

![image-20241221112013344](../../../AppData/Roaming/Typora/typora-user-images/image-20241221112013344.png)

![image-20241221112133210](../../../AppData/Roaming/Typora/typora-user-images/image-20241221112133210.png)

![image-20241221112544400](../../../AppData/Roaming/Typora/typora-user-images/image-20241221112544400.png)

![image-20241221112636167](../../../AppData/Roaming/Typora/typora-user-images/image-20241221112636167.png)

![image-20241221112948560](../../../AppData/Roaming/Typora/typora-user-images/image-20241221112948560.png)

![image-20241221113141003](../../../AppData/Roaming/Typora/typora-user-images/image-20241221113141003.png)

![image-20241221113241085](../../../AppData/Roaming/Typora/typora-user-images/image-20241221113241085.png)

![image-20241221113654892](../../../AppData/Roaming/Typora/typora-user-images/image-20241221113654892.png)

![image-20241221114910317](../../../AppData/Roaming/Typora/typora-user-images/image-20241221114910317.png)

## GraalVm

![image-20241222190952884](../../../AppData/Roaming/Typora/typora-user-images/image-20241222190952884.png)

![image-20241222191137231](../../../AppData/Roaming/Typora/typora-user-images/image-20241222191137231.png)

 ![image-20241222191648865](../../../AppData/Roaming/Typora/typora-user-images/image-20241222191648865.png)

![image-20241222192054493](../../../AppData/Roaming/Typora/typora-user-images/image-20241222192054493.png)

![image-20241222192716002](../../../AppData/Roaming/Typora/typora-user-images/image-20241222192716002.png)



## 栈上的数据存储

![image-20241222200134798](../../../AppData/Roaming/Typora/typora-user-images/image-20241222200134798.png)

![image-20241222200153877](../../../AppData/Roaming/Typora/typora-user-images/image-20241222200153877.png)

![image-20241222200240246](../../../AppData/Roaming/Typora/typora-user-images/image-20241222200240246.png)

![image-20241222201603771](../../../AppData/Roaming/Typora/typora-user-images/image-20241222201603771.png)

![image-20241222201644419](../../../AppData/Roaming/Typora/typora-user-images/image-20241222201644419.png)

## 对象在堆上是如何存储的

![image-20241222204721825](../../../AppData/Roaming/Typora/typora-user-images/image-20241222204721825.png)

![image-20241222202829015](../../../AppData/Roaming/Typora/typora-user-images/image-20241222202829015.png)

![image-20241222203311940](../../../AppData/Roaming/Typora/typora-user-images/image-20241222203311940.png)

![image-20241222203406793](../../../AppData/Roaming/Typora/typora-user-images/image-20241222203406793.png)

![image-20241222203513568](../../../AppData/Roaming/Typora/typora-user-images/image-20241222203513568.png)

![image-20241222203628968](../../../AppData/Roaming/Typora/typora-user-images/image-20241222203628968.png)

![image-20241222203711996](../../../AppData/Roaming/Typora/typora-user-images/image-20241222203711996.png)

  ![image-20241222204030032](../../../AppData/Roaming/Typora/typora-user-images/image-20241222204030032.png)

![image-20241222204203997](../../../AppData/Roaming/Typora/typora-user-images/image-20241222204203997.png)

![image-20241222204316938](../../../AppData/Roaming/Typora/typora-user-images/image-20241222204316938.png)

![image-20241222204342553](../../../AppData/Roaming/Typora/typora-user-images/image-20241222204342553.png)



 

## 方法调用的原理

![image-20241222205157230](../../../AppData/Roaming/Typora/typora-user-images/image-20241222205157230.png)

![image-20241222205829672](../../../AppData/Roaming/Typora/typora-user-images/image-20241222205829672.png)

![image-20241222205849777](../../../AppData/Roaming/Typora/typora-user-images/image-20241222205849777.png)

![image-20241222210222845](../../../AppData/Roaming/Typora/typora-user-images/image-20241222210222845.png)

![image-20241222210248898](../../../AppData/Roaming/Typora/typora-user-images/image-20241222210248898.png)

![image-20241222210306252](../../../AppData/Roaming/Typora/typora-user-images/image-20241222210306252.png)

![image-20241222210622872](../../../AppData/Roaming/Typora/typora-user-images/image-20241222210622872.png)

![image-20241222210821990](../../../AppData/Roaming/Typora/typora-user-images/image-20241222210821990.png)



## 异常捕获的原理

![image-20241222211827418](../../../AppData/Roaming/Typora/typora-user-images/image-20241222211827418.png)

![image-20241222211902464](../../../AppData/Roaming/Typora/typora-user-images/image-20241222211902464.png)

![image-20241222212450945](../../../AppData/Roaming/Typora/typora-user-images/image-20241222212450945.png)

![image-20241222213426566](../../../AppData/Roaming/Typora/typora-user-images/image-20241222213426566.png)

![image-20241222213716898](../../../AppData/Roaming/Typora/typora-user-images/image-20241222213716898.png)

![image-20241222214238155](../../../AppData/Roaming/Typora/typora-user-images/image-20241222214238155.png)



## JIT即时编译器

![image-20241223103853254](../../../AppData/Roaming/Typora/typora-user-images/image-20241223103853254.png)

![image-20241223103943688](../../../AppData/Roaming/Typora/typora-user-images/image-20241223103943688.png)

![image-20241223104102392](../../../AppData/Roaming/Typora/typora-user-images/image-20241223104102392.png)

![image-20241223104348900](../../../AppData/Roaming/Typora/typora-user-images/image-20241223104348900.png)

![image-20241223104434075](../../../AppData/Roaming/Typora/typora-user-images/image-20241223104434075.png)

![image-20241223104444310](../../../AppData/Roaming/Typora/typora-user-images/image-20241223104444310.png)

![image-20241223104802754](../../../AppData/Roaming/Typora/typora-user-images/image-20241223104802754.png)

![image-20241223105324945](../../../AppData/Roaming/Typora/typora-user-images/image-20241223105324945.png)



![image-20241223105556577](../../../AppData/Roaming/Typora/typora-user-images/image-20241223105556577.png)

![image-20241223105712331](../../../AppData/Roaming/Typora/typora-user-images/image-20241223105712331.png)

![image-20241223105738291](../../../AppData/Roaming/Typora/typora-user-images/image-20241223105738291.png)

 ![image-20241223110030358](../../../AppData/Roaming/Typora/typora-user-images/image-20241223110030358.png)

## 垃圾回收器原理

![image-20241223110903796](../../../AppData/Roaming/Typora/typora-user-images/image-20241223110903796.png)

![image-20241223110927387](../../../AppData/Roaming/Typora/typora-user-images/image-20241223110927387.png)

![image-20241223111034531](../../../AppData/Roaming/Typora/typora-user-images/image-20241223111034531.png)

![image-20241223111102002](../../../AppData/Roaming/Typora/typora-user-images/image-20241223111102002.png)

![image-20241223111223704](../../../AppData/Roaming/Typora/typora-user-images/image-20241223111223704.png)

![image-20241223111359275](../../../AppData/Roaming/Typora/typora-user-images/image-20241223111359275.png)

![image-20241223111510216](../../../AppData/Roaming/Typora/typora-user-images/image-20241223111510216.png)

![image-20241223111645299](../../../AppData/Roaming/Typora/typora-user-images/image-20241223111645299.png)

![image-20241223111725135](../../../AppData/Roaming/Typora/typora-user-images/image-20241223111725135.png)

 ![image-20241223112111397](../../../AppData/Roaming/Typora/typora-user-images/image-20241223112111397.png)

![image-20241223112354511](../../../AppData/Roaming/Typora/typora-user-images/image-20241223112354511.png)

![image-20241223112420581](../../../AppData/Roaming/Typora/typora-user-images/image-20241223112420581.png)

![image-20241223112556497](../../../AppData/Roaming/Typora/typora-user-images/image-20241223112556497.png)

![image-20241223112800337](../../../AppData/Roaming/Typora/typora-user-images/image-20241223112800337.png)

![image-20241223112835520](../../../AppData/Roaming/Typora/typora-user-images/image-20241223112835520.png)

![image-20241223113049224](../../../AppData/Roaming/Typora/typora-user-images/image-20241223113049224.png)



## 面试篇

### 什么是JVM

**定义：**JVM指的是Java虚拟机，本质上是一个运行在计算机上的程序，职责是运行java字节码文件

**作用：**JVM是为了支持Write Once，Run Anywhere；编写一次，到处运行的跨平台特性

![image-20241223113629345](../../../AppData/Roaming/Typora/typora-user-images/image-20241223113629345.png)

![image-20241223113643030](../../../AppData/Roaming/Typora/typora-user-images/image-20241223113643030.png)

**功能：**

- 解释和运行：将字节码中的指令实时的解释成机器码
- 内存管理：自动为对象、方法分配内存；自动的垃圾回收机制
- 即时编译：对热点代码进行优化，提升执行效率

**组成：**类加载子系统 + 运行时数据区 + 执行引擎 + 本地接口

![image-20241223114110364](../../../AppData/Roaming/Typora/typora-user-images/image-20241223114110364.png)

**常见的JVM：**

最常用的：Oracle的Hotspot

云原生架构高性能需求：GraalVM、OpenJ9



### 字节码文件的组成

字节码文件本质上是一个二进制的文件，无法直接用记事本等工具打开阅读其内容。需要通过专业的工具打开。

- 开发环境使用jclasslib插件
- 服务器环境使用javap -v命令

**内容：**

![image-20241223121737955](../../../AppData/Roaming/Typora/typora-user-images/image-20241223121737955.png)



### 什么是运行时数据区

运行时数据区指的是JVM所管理的内存区域，其中分为两大类：

- 线程共享：方法区、堆
- 线程不共享：本地方法栈、虚拟机栈、程序计数器
- 直接内存主要是NIO使用，由操作系统直接管理，不属于JVM内存

![image-20241223122943346](../../../AppData/Roaming/Typora/typora-user-images/image-20241223122943346.png)

![image-20241223123523897](../../../AppData/Roaming/Typora/typora-user-images/image-20241223123523897.png)

![image-20241223123944984](../../../AppData/Roaming/Typora/typora-user-images/image-20241223123944984.png)

