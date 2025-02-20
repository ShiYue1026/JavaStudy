# Java概述

## Java有哪些特性

- Java是一门面向对象的编程语言
- 吸收了C++语言中大量的有点，但又抛弃了C++中容易出错的地方，如垃圾回收、指针
- Java是与平台无关的编程语言，即一次编译，处处运行，只需要在对应的平台上安装 JDK，就可以实现跨平台，在 Windows、macOS、Linux 操作系统上运行



## Java和C语言有哪些区别

- Java先将代码编译成字节码，再由JVM解释执行；C语言直接编译成机器码，可直接运行
- Java跨平台，可以实现“一次编译，处处运行”；C语言在不同的操作系统上需要重新编译
- Java实现了自动垃圾回收；C语言需要使用malloc和free来手动管理内存

- Java内置多线程；C语言没有内置多线程机制



## Java语言有哪些特点

- 面向对象
  - 主要是封装、继承、多态
- 平台无关性
  - 一次编译，处处运行
- 支持多线程
  - C++ 语言没有内置的多线程机制，因此必须调用操作系统的 API 来完成多线程程序设计
  - Java 却提供了封装好多线程支持
- 支持JIT编译
  - 可以在程序运行时将字节码转换为热点机器码来提高程序的运行速度



## JVM、JDK和JRE有什么区别

![三分恶面渣逆袭：JDK、JRE、JVM关系](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/javase-3.png)

**JVM**：Java虚拟机，是Java实现跨平台的关键，不同的操作系统有不同的JVM实现。

**JRE**：Java运行时环境，包含了运行Java程序所必需的库和JVM。

**JDK**：一套完整的Java SDK，包括JRE，编译器javac，Java文档生成工具javadoc、Java字节码工具javap（反编译）、java打包工具jar、监控优化Java应用jconsole、jstack、jmap等。为开发者提供了开发、编译、调试Java程序的一整套环境。



## 说说什么是跨平台，原理是什么

所谓的跨平台，是指Java语言编写的程序，一次编译后，可以在多个操作系统上运行。

原理是增加了一个中间件JVM，JVM负责将Java字节码转换为特定平台的机器码。而不同平台有各自的JVM，JVM屏蔽了不同系统操作系统的差异，



## 什么是字节码？采用字节码的好处是什么

所谓的字节码，就是Java程序经过编译后产生的.class文件

**Java** 程序从源代码到运行需要经过三步：

- **编译**：使用javac将源代码文件 .java 编译成 JVM 可以识别的字节码文件 .class
- **解释**：JVM 执行字节码文件，将字节码翻译成操作系统能识别的机器码
- **执行**：操作系统执行二进制的机器码



## 为什么说Java是“编译与解释并存”的语言

**编译型语言**是指编译器针对特定的操作系统，将源代码一次性翻译成可被该平台执行的机器码。

**解释型语言**是指解释器对源代码进行逐行解释，解释成特定平台的机器码并执行。



 Java 是“编译与解释并存”的语言，是因为 Java 程序需要先将 Java 源代码文件编译字节码文件，再解释执行。

![三分恶面渣逆袭：编译与解释](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/javase-5.png)



# 基础语法

## Java有哪些数据类型

分为**基本数据类型**和**引用数据类型**

基本数据类型有：

- 数值型
  - 整数类型：byte、short、int、long
  - 浮点类型：float、double

- 字符型
  - char
- 布尔型
  - boolean

| 数据类型 | 默认值   | 大小            |
| -------- | -------- | --------------- |
| boolean  | false    | 1 字节或 4 字节 |
| char     | '\u0000' | 2 字节          |
| byte     | 0        | 1 字节          |
| short    | 0        | 2 字节          |
| int      | 0        | 4 字节          |
| long     | 0L       | 8 字节          |
| float    | 0.0f     | 4 字节          |
| double   | 0.0      | 8 字节          |



引用数据类型有：

- 类
- 接口
- 数组



## boolean类型实际占用几个字节

这要依据具体的 JVM 实现细节。Java 虚拟机规范中，并没有明确规定 boolean 类型的大小，只规定了 boolean 类型的取值 true 或 false。

```
boolean: The boolean data type has only two possible values: true and false. Use this data type for simple flags that track true/false conditions. This data type represents one bit of information, but its "size" isn't something that's precisely defined.
```



## 给Integer最大值 + 1是什么结果

当给 Integer.MAX_VALUE 加 1 时，会发生溢出，变成 Integer.MIN_VALUE。

```java
int maxValue = Integer.MAX_VALUE;
System.out.println("Integer.MAX_VALUE = " + maxValue); // Integer.MAX_VALUE = 2147483647
System.out.println("Integer.MAX_VALUE + 1 = " + (maxValue + 1)); // Integer.MAX_VALUE + 1 = -2147483648

// 用二进制来表示最大值和最小值
System.out.println("Integer.MAX_VALUE in binary: " + Integer.toBinaryString(maxValue)); // Integer.MAX_VALUE in binary: 0111111111111111111111111111111
System.out.println("Integer.MIN_VALUE in binary: " + Integer.toBinaryString(Integer.MIN_VALUE)); // Integer.MIN_VALUE in binary: 10000000000000000000000000000000
```

因为 Java 的整数类型采用的是二进制补码表示法，溢出时值会变成最小值。

- Integer.MAX_VALUE 的二进制表示是 01111111 11111111 11111111 11111111（32 位）。
- 加 1 后结果变成 10000000 00000000 00000000 00000000，即 -2147483648（Integer.MIN_VALUE）



## 自动类型转换、强制类型转换了解吗

**自动类型转换**：范围较小的变量或数值 => 范围较大的变量

**强制类型转换**：范围较大的变量或数值 => 范围较小的变量

![三分恶面渣逆袭：Java自动类型转换方向](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/javase-7.png)

①、`float f=3.4`，对吗？

不正确。3.4 默认是双精度，将双精度赋值给浮点型属于下转型（down-casting，也称窄化）会造成精度丢失，因此需要强制类型转换`float f =(float)3.4;`或者写成`float f =3.4F`



②、`short s1 = 1; s1 = s1 + 1；`对吗？`short s1 = 1; s1 += 1;`对吗？

`short s1 = 1; s1 = s1 + 1;` 会编译出错，由于 1 是 int 类型，因此 s1+1 运算结果也是 int 型，需要强制转换类型才能赋值给 short 型。

而 `short s1 = 1; s1 += 1;`可以正确编译，因为 `s1+= 1;`相当于 `s1 = (short(s1 + 1);` 其中有隐含的强制类型转换。



## 什么是自动拆箱/装箱

**装箱**：将基本数据类型转换为包装类型，如int 转换为 Integer

**拆箱**：将包装类型转换为基本数据类型

![三分恶面渣逆袭:装箱和拆箱](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/javase-8.png)



## &和&&有什么区别

`&`是按位与，`&&`是短路与



`&&`之所以称为短路运算是因为，如果`&&`左边的表达式的值是 false，右边的表达式会直接短路掉，不会进行运算。



## switch语句能否用在byte / long / String类型上

Java 5 以前 `switch(expr)` 中，expr 只能是 byte、short、char、int。

从 Java 5 开始，Java 中引入了枚举类型， expr 也可以是 enum 类型。

从 Java 7 开始，expr 还可以是字符串，但是**长整型在目前所有的版本中都是不可以的**。



## break、continue、return的区别及作用

- break 跳出整个循环，不再执行循环(**结束当前的循环体**)
- continue 跳出本次循环，继续执行下次循环(**结束正在执行的循环 进入下一个循环条件**)
- return 程序返回，不再执行下面的代码(**结束当前的方法 直接返回**)



## 用效率最高的方法计算2乘以8

`2 << 3`。**位运算**，数字的二进制位左移三位相当于乘以 2 的三次方。



## 说说自增自减运算

++和--运算符可以放在变量之前，也可以放在变量之后。

当运算符放在变量之前时(前缀)，先自增/减，再赋值；当运算符放在变量之后时(后缀)，先赋值，再自增/减。

例如，当 `b = ++a` 时，先自增（自己增加 1），再赋值（赋值给 b）；当 `b = a++` 时，先赋值(赋值给 b)，再自增（自己增加 1）。也就是，++a 输出的是 a+1 的值，a++输出的是 a 值。

**例子1**

```java
int i  = 1;
i = i++;
System.out.println(i);
```

- 答案是 1

- 对于 JVM 而言，它对自增运算的处理，是会先定义一个临时变量来接收 i 的值，然后进行自增运算，最后又将临时变量赋给了值为 2 的 i，所以最后的结果为 1。

等价于：

```java
int i = 1；
int temp = i;
i++；
i = temp;
System.out.println(i);
```



**例子2**

```java
int count = 0;
for(int i = 0;i < 100;i++)
{
    count = count++;
}
System.out.println("count = "+count);
```

- 答案是0

等价于：

```java
int autoAdd(int count)
{
    int temp = count;
    count = count + 1;
    return temp;
}
```



## float是怎么表示小数的

`float`类型的小数在计算机中是通过 IEEE 754 标准的单精度浮点数格式来表示的。

​					$V = (-1)^S \times M \times 2^E$

- S：符号位，0 代表正数，1 代表负数；
- M：尾数部分，用于表示数值的精度；比如说 $1.25 \times 2^2$；1.25 就是尾数；
- R：基数，十进制中的基数是 10，二进制中的基数是 2；
- E：指数部分，例如 10−1 中的 -1 就是指数。



这种表示方法可以将非常大或非常小的数值用有限的位数表示出来，但这也意味着可能会有精度上的损失。

单精度浮点数占用 4 字节（32 位），这 32 位被分为三个部分：符号位、指数部分和尾数部分。

![kaito：浮点数](https://cdn.tobebetterjavaer.com/stutymore/javase-20240321112428.png)

1. **符号位（Sign bit）**：1 位
2. **指数部分（Exponent）**：10 位
3. **尾数部分（Mantissa，或 Fraction）**：21 位



按照这个规则，将十进制数 25.125 转换为浮点数，转换过程是这样的：

1. 整数部分：25 转换为二进制是 11001；
2. 小数部分：0.125 转换为二进制是 0.001；
3. 用二进制科学计数法表示：25.125 = $1.1001001 \times 2^4$



使用浮点数时需要注意，由于精度的限制，进行数学运算时可能会遇到舍入误差，特别是连续运算累积误差可能会变得显著。

对于需要高精度计算的场景（如金融计算），可能需要考虑使用`BigDecimal`类来避免这种误差。



## 讲一下数据准确性高是怎么保证的

在金融计算中，保证数据准确性有两种方案，一种使用 `BigDecimal`，一种将浮点数转换为整数 int 进行计算。

肯定不能使用 `float` 和 `double` 类型，它们无法避免浮点数运算中常见的精度问题，因为这些数据类型采用二进制浮点数来表示，无法准确地表示，例如 `0.1`。

```java
BigDecimal num1 = new BigDecimal("0.1");
BigDecimal num2 = new BigDecimal("0.2");
BigDecimal sum = num1.add(num2);
System.out.println("Sum of 0.1 and 0.2 using BigDecimal: " + sum);  // 输出 0.3，精确计算
```

在处理小额支付或计算时，通过转换为较小的货币单位（如分），这样不仅提高了运算速度，还保证了计算的准确性。

```java
int priceInCents = 199;  // 商品价格199分
int quantity = 3;
int totalInCents = priceInCents * quantity;  // 计算总价
System.out.println("Total price in cents: " + totalInCents);  // 输出597分
```



# 面向对象

## 面向对象和面向过程的区别

**面向过程**是以过程为核心，通过函数完成任务，程序结构是函数+步骤组成的顺序流程。

**面向对象**是以对象为核心，通过对象交互完成任务，程序结构是类和对象组成的模块化结构，代码可以通过继承、组合、多态等方式复用。



## 面向对象编程有哪些特性

面向对象编程有三大特性：封装、继承、多态

**什么是封装**

封装是指将数据（属性，或者叫字段）和操作数据的方法（行为）捆绑在一起，形成一个独立的对象（类的实例）。

```java
class Nvshen {
    private String name;
    private int age;

    public void setName(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

封装是把一个对象的属性私有化，同时提供一些可以被外界访问的方法。



**什么是继承**

继承允许一个类（子类）继承现有类（父类或者基类）的属性和方法。以提高代码的复用性，建立类之间的层次关系。

```java
class Person {
    protected String name;
    protected int age;

    public void eat() {
        System.out.println("吃饭");
    }
}

class Student extends Person {
    private String school;

    public void study() {
        System.out.println("学习");
    }
}
```



**什么是多态**

多态允许不同类的对象对同一消息做出响应，但表现出不同的行为（即方法的多样性）。

多态其实是一种能力——同一个行为具有不同的表现形式；换句话说就是，执行一段代码，Java 在运行时能根据对象类型的不同产生不同的结果。

```java
// 父类：Animal
class Animal {
    void makeSound() {
        System.out.println("动物在发出声音");
    }
}

// 子类：Dog
class Dog extends Animal {
    @Override
    void makeSound() {
        System.out.println("汪汪汪");
    }
}

// 子类：Cat
class Cat extends Animal {
    @Override
    void makeSound() {
        System.out.println("喵喵喵");
    }
}

public class PolymorphismExample {
    public static void main(String[] args) {
        Animal myAnimal = new Dog();  // 父类引用指向子类对象
        myAnimal.makeSound();         // 调用 Dog 的 `makeSound()`，输出 "汪汪汪"

        myAnimal = new Cat();         // 重新指向 Cat 对象
        myAnimal.makeSound();         // 调用 Cat 的 `makeSound()`，输出 "喵喵喵"
    }
}
```

多态的前置条件有三个：

- 子类继承父类
- 子类重写父类的方法
- 父类引用指向子类的对象



## 为什么Java里面要多组合少继承

**继承**适合描述**“is-a”**的关系，但继承容易导致类之间的强耦合，**一旦父类发生改变，子类也要随之改变，违背了开闭原则**（尽量不修改现有代码，而是添加新的代码来实现）。

**组合**适合描述**“has-a”或“can-do”**的关系，通过在类中组合其他类，能够更灵活地扩展功能。组合避免了复杂的类继承体系，同时遵循了开闭原则和松耦合的设计原则。



## 多态解决了什么问题

面向对象编程中，多态允许父类引用指向子类对象，**调用相同方法时，不同子类可以有不同实现**（方法重写）。这样，在不修改已有代码的情况下，就能动态扩展功能。例如，一个 `Animal` 父类有 `makeSound()` 方法，`Dog` 和 `Cat` 继承它并各自实现 `makeSound()`，那么程序可以**以统一方式处理所有动物，而不需要区分具体类型**。



## 多态的实现原理是什么

多态通过**动态绑定 + 虚方法表**实现，Java 使用虚方法表存储方法指针，方法调用时根据对象实际类型从虚方法表查找具体实现。

![截图来自博客园的小牛呼噜噜：虚拟方法表](https://cdn.tobebetterjavaer.com/stutymore/javase-20241126104207.png)



## 重载和重写的区别

如果一个类有多个名字相同但参数个数不同的方法，我们通常称这些方法为**方法重载（overload）**。如果方法的功能是一样的，但参数不同，使用相同的名字可以提高程序的可读性。



如果子类具有和父类一样的方法（参数相同、返回类型相同、方法名相同，但方法体可能不同），我们称之为**方法重写（override）**。方法重写用于提供父类已经声明的方法的特殊实现，是实现多态的基础条件。



方法重载发生在同一个类中，同名的方法如果有不同的参数（参数类型不同、参数个数不同、参数顺序不同）。

方法重写发生在子类与父类之间，要求子类与父类具有相同的返回类型，方法名和参数列表，并且不能比父类的方法声明更多的异常，遵守里氏代换原则。

#### 

## 什么是里氏代换原则

核心思想是：子类对象必须能够替换父类对象，并且程序的行为不会发生变化



子类必须完全具备父类的行为

子类可以扩展父类功能，但不能修改父类原有的行为

程序在使用父类的地方，换成子类也不会影响正确性



## 还有哪些设计原则

SOLID

- **单一职责原则**（Single Responsibility Principle, SRP），指一个类应该只有一个引起它变化的原因，即一个类只负责一项职责。这样做的目的是使类更加清晰，更容易理解和维护。
- **开闭原则**（Open-Closed Principle, OCP），指软件实体应该对扩展开放，对修改关闭。这意味着一个类应该通过扩展来实现新的功能，而不是通过修改已有的代码来实现。
- **接口隔离原则**（Interface Segregation Principle, ISP），指客户端不应该依赖它不需要的接口。这意味着设计接口时应该尽量精简，不应该设计臃肿庞大的接口。
- **依赖倒置原则**（Dependency Inversion Principle, DIP），指高层模块不应该依赖低层模块，二者都应该依赖其抽象；抽象不应该依赖细节，细节应该依赖抽象。这意味着设计时应该尽量依赖接口或抽象类，而不是实现类。



## 访问修饰符public、private、protected以及默认时的区别

![访问修饰符和可见性](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/javase-12.png)

- **default** （即默认，什么也不写）: 在同一包内可见，不使用任何修饰符。可以修饰在类、接口、变量、方法。
- **private** : 在同一类内可见。可以修饰变量、方法。**注意：不能修饰类（外部类）**
- **public** : 对所有类可见。可以修饰类、接口、变量、方法
- **protected** : 对同一包内的类和所有子类可见。可以修饰变量、方法。**注意：不能修饰类（外部类）**。



## this关键字有什么用

this 是自身的一个对象，代表对象本身，可以理解为：**指向对象本身的一个指针**。

this 的用法在 Java 中大体可以分为 3 种：

1. 普通的直接引用，this 相当于是指向当前对象本身
2. 形参与成员变量名字重名，用 this 来区分：

```java
public Person(String name,int age){
    this.name=name;
    this.age=age;
}
```

3. 引用本类的构造方法

```java
class Person {
    String name;
    int age;

    // 构造方法 1（无参数）
    public Person() {
        this("默认姓名", 18); // 调用带参数的构造方法
        System.out.println("调用无参构造");
    }

    // 构造方法 2（带参数）
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
        System.out.println("调用带参构造: " + name + ", " + age);
    }

    void display() {
        System.out.println("姓名: " + name + ", 年龄: " + age);
    }
}

public class ThisConstructorExample {
    public static void main(String[] args) {
        Person p1 = new Person(); // 调用无参构造
        p1.display();

        Person p2 = new Person("Alice", 25); // 调用带参构造
        p2.display();
    }
}
```



## 抽象类和接口有什么区别

**一个类只能继承一个抽象类；但一个类可以实现多个接口。**所以在新建线程类的时候一般推荐使用实现 Runnable 接口的方式，这样线程类还可以继承其他类，而不单单是 Thread 类。

**抽象类符合 is-a 的关系，而接口更像是 has-a 的关系**，比如说一个类可以序列化的时候，它只需要实现 Serializable 接口就可以了，不需要去继承一个序列化类。

抽象类更多地是用来为多个相关的类提供一个共同的基础框架，包括状态的初始化，而接口则是定义一套行为标准，让不同的类可以实现同一接口，提供行为的多样化实现。

抽象类可以定义构造方法，接口不可以定义构造方法

- 抽象类是类，虽然不能实例化，但可以被子类继承，子类需要调用它的构造方法。
- 抽象类仍然有成员变量，构造方法用于初始化这些变量



## Java支持多继承吗

Java 不支持多继承，一个类只能继承一个类，多继承会引发菱形继承问

```java
class A {
    void show() { System.out.println("A"); }
}

class B extends A {
    void show() { System.out.println("B"); }
}

class C extends A {
    void show() { System.out.println("C"); }
}

// 如果 Java 支持多继承
class D extends B, C {
    // 调用 show() 方法时，D 应该调用 B 的 show() 还是 C 的 show()？
}
```

```mathematica
   A
  /  \
 B    C
  \  /
   D  <-- D 继承 B 和 C，如果 A 有方法 show()，D 该继承哪一个？
```



## 抽象类和普通类的区别

- 抽象类使用 abstract 关键字定义，不能被实例化，只能作为其他类的父类。普通类没有 abstract 关键字，可以直接实例化。

- 抽象类可以包含抽象方法和非抽象方法。抽象方法没有方法体，必须由子类实现。普通类只能包含非抽象方法。

  ```java
  abstract class Animal {
      // 抽象方法
      public abstract void makeSound();
  
      // 非抽象方法
      public void eat() {
          System.out.println("This animal is eating.");
      }
  }
  
  class Dog extends Animal {
      // 实现抽象方法
      @Override
      public void makeSound() {
          System.out.println("Woof");
      }
  }
  
  public class Test {
      public static void main(String[] args) {
          Dog dog = new Dog();
          dog.makeSound(); // 输出 "Woof"
          dog.eat(); // 输出 "This animal is eating."
      }
  }
  ```



## 成员变量和局部变量的有哪些区别

**1. 从语法形式上看**

- 成员变量是属于类的，而局部变量是在方法中定义的变量或是方法的参数；成员变量可以被 public , private , static 等修饰符所修饰，⽽局部变量不能被访问控制修饰符及 static 所修饰；但是，成员变量和局部变量都能被 final 所修饰。

**2. 从变量在内存中的存储方式看**

- 如果是实例的成员变量，存储在堆中；如果是类的成员变量，存储在方法区中
- 局部变量存储在栈中

**3. 从变量在内存中的生存时间上看**

- 成员变量是对象的一部分，它随着对象的创建而存在，

- 局部变量随着方法的调用而自动消失。

**4. 从初始值上看**

- 成员变量如果没有赋值，会自动以类型的默认值赋值
- 局部变量如果不赋初值会编译错误



## static关键字了解吗

static 关键字可以用来修饰变量、方法、代码块和内部类，以及导入包。

| 修饰对象 | 作用                                                         |
| -------- | ------------------------------------------------------------ |
| 变量     | 静态变量，类级别变量，所有实例共享同一份数据。               |
| 方法     | 静态方法，类级别方法，与实例无关。                           |
| 代码块   | 在类加载时初始化一些数据，只执行一次。                       |
| 内部类   | 与外部类绑定但独立于外部类实例。                             |
| 导入     | 可以直接访问静态成员，无需通过类名引用，简化代码书写，但会降低代码可读性。 |



## final关键字有什么作用

- 当 final 修饰一个类时，表明这个类不能被继承。比如，String 类、Integer 类和其他包装类都是用 final 修饰的。

- 当 final 修饰一个方法时，表明这个方法不能被重写（Override）。也就是说，如果一个类继承了某个类，并且想要改变父类中被 final 修饰的方法的行为，是不被允许的。

- 当 final 修饰一个变量时，表明这个变量的值一旦被初始化就不能被修改。

  - 如果是基本数据类型的变量，其数值一旦在初始化之后就不能更改

  - 如果是引用类型的变量，在对其初始化之后就不能再让其指向另一个对象。但是引用指向的对象内容可以改变。

    ![三分恶面渣逆袭：final修饰变量](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/javase-13.png)



## final、finally、finalize的区别

- final是一个修饰符，可以修饰类、方法和变量
  - 当final修饰一个类时，表明这个类不能被继承
  - 当final修饰一个方法时，表明这个方法不能被重写
  - 当final修饰一个变量时，表明这个变量是常量，一旦赋值完成就不能再被修改了

- finallyfinally 是 Java 中异常处理的一部分，用来创建 try 块后面的 finally 块。
  - 无论 try 块中的代码是否抛出异常，finally 块中的代码总是会被执行。
  - 通常，finally 块被用来释放资源，如关闭文件、数据库连接等。

- finalize 是Object 类的一个方法，用于在垃圾回收器将对象从内存中清除出去之前做一些必要的清理工作。



## == 和 equals的区别

在 Java 中，`==` 操作符和 `equals()` 方法用于比较两个对象：

**==**：用于比较两个对象的引用，即它们是否指向同一个对象实例。

- 如果两个变量引用同一个对象实例，`==` 返回 `true`，否则返回 `false`。

- 对于基本数据类型（如 `int`, `double`, `char` 等），`==` 比较的是值是否相等。

**equals() 方法**：用于比较两个对象的内容是否相等。默认情况下，`equals()` 方法的行为与 `==` 相同，即比较对象引用，如在超类 Object 中：

```java
public boolean equals(Object obj) {
    return (this == obj);
}
```

然而，`equals()` 方法通常被各种类重写。例如，`String` 类重写了 `equals()` 方法，以便它可以比较两个字符串的字符内容是否完全一样。



## 为什么重写equals时必须重写hashCode方法

当重写 `equals()` 时，必须重写 `hashCode()`，否则相等的对象在哈希集合（如 `HashSet`、`HashMap`）中可能表现不一致，导致数据丢失或重复存储。

`hashCode()` 影响哈希表的存储位置，**默认实现是基于对象的内存地址计算的**。`equals()` 决定对象是否相等。如果两个对象 `equals()` 返回 `true`，但 `hashCode()` 不同，它们可能被存放在哈希表的不同位置，导致无法正确查找或删除。因此，保证相等的对象具有相同的 `hashCode()`，才能保证哈希结构的正确性和性能。*



```java
import java.util.HashSet;
import java.util.Objects;

class Person {
    String name;

    public Person(String name) {
        this.name = name;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || getClass() != obj.getClass()) return false;
        Person person = (Person) obj;
        return Objects.equals(name, person.name);
    }

    // ❌ 没有重写 hashCode()
}

public class HashCodeProblem {
    public static void main(String[] args) {
        HashSet<Person> set = new HashSet<>();
        Person p1 = new Person("Alice");
        Person p2 = new Person("Alice");

        System.out.println(p1.equals(p2)); // ✅ true

        set.add(p1);
        set.add(p2); // ❌ 预期不会存入，但实际存入了！

        System.out.println(set.size()); // ❌ 输出 2，应该是 1！
    }
}
```



## Java是值传递还是引用传递

Java 是值传递，不是引用传递。

当一个对象被作为参数传递到方法中时，参数的值就是该对象的引用。引用的值是对象在堆中的地址。

对象是存储在堆中的，所以传递对象的时候，可以理解为把变量存储的对象地址给传递过去。



## 说说深拷贝和浅拷贝的区别

![三分恶面渣逆袭：浅拷贝和深拷贝示意图](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/javase-15.png)

**浅拷贝**会创建一个新对象，但这个新对象的属性（字段）和原对象的属性完全相同。如果属性是基本数据类型，拷贝的是基本数据类型的值；如果属性是引用类型，拷贝的是引用地址，因此**新旧对象共享同一个引用对象**。

- 实现Cloneable接口并重写clone()方法

  ```java
  class Person implements Cloneable {
      String name;
      int age;
      Address address;
  
      public Person(String name, int age, Address address) {
          this.name = name;
          this.age = age;
          this.address = address;
      }
  
      @Override
      protected Object clone() throws CloneNotSupportedException {
          return super.clone();
      }
  }
  
  class Address {
      String city;
  
      public Address(String city) {
          this.city = city;
      }
  }
  
  public class Main {
      public static void main(String[] args) throws CloneNotSupportedException {
          Address address = new Address("河南省洛阳市");
          Person person1 = new Person("沉默王二", 18, address);
          Person person2 = (Person) person1.clone();
  
          System.out.println(person1.address == person2.address); // true
      }
  }
  ```

  



**深拷贝**也会创建一个新对象，但会递归地复制所有的引用对象，确保新对象和原对象完全独立。新对象与原对象的任何更改都不会相互影响。

- 手动赋值所有的引用对象

  ```java
  class Person {
      String name;
      int age;
      Address address;
  
      public Person(String name, int age, Address address) {
          this.name = name;
          this.age = age;
          this.address = address;
      }
  
      public Person(Person person) {
          this.name = person.name;
          this.age = person.age;
          this.address = new Address(person.address.city);
      }
  }
  
  class Address {
      String city;
  
      public Address(String city) {
          this.city = city;
      }
  }
  
  public class Main {
      public static void main(String[] args) {
          Address address = new Address("河南省洛阳市");
          Person person1 = new Person("沉默王二", 18, address);
          Person person2 = new Person(person1);
  
          System.out.println(person1.address == person2.address); // false
      }
  }
  ```

  

- 使用序列化与反序列化

  ```java
  import java.io.*;
  
  class Person implements Serializable {
      String name;
      int age;
      Address address;
  
      public Person(String name, int age, Address address) {
          this.name = name;
          this.age = age;
          this.address = address;
      }
  
      public Person deepClone() throws IOException, ClassNotFoundException {
          ByteArrayOutputStream bos = new ByteArrayOutputStream();
          ObjectOutputStream oos = new ObjectOutputStream(bos);
          oos.writeObject(this);
  
          ByteArrayInputStream bis = new ByteArrayInputStream(bos.toByteArray());
          ObjectInputStream ois = new ObjectInputStream(bis);
          return (Person) ois.readObject();
      }
  }
  
  class Address implements Serializable {
      String city;
  
      public Address(String city) {
          this.city = city;
      }
  }
  
  public class Main {
      public static void main(String[] args) throws IOException, ClassNotFoundException {
          Address address = new Address("河南省洛阳市");
          Person person1 = new Person("沉默王二", 18, address);
          Person person2 = person1.deepClone();
  
          System.out.println(person1.address == person2.address); // false
      }
  }
  ```

  



## Java创建对象有哪几种方式

**1. 构造函数**

```java
Person person = new Person();
```

**2. 通过反射创建**

```java
Class clazz = Class.forName("Person");
Person person = (Person) clazz.newInstance();
```

**3. clone拷贝创建**

需要实现 Cloneable 接口并重写 clone 方法

```java
Person person = new Person();
Person person2 = (Person) person.clone();
```

**4. 序列化机制创建**

通过序列化将对象转换为字节流，再通过反序列化从字节流中恢复对象。需要实现 Serializable 接口

```java
Person person = new Person();
ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("person.txt"));
oos.writeObject(person);
ObjectInputStream ois = new ObjectInputStream(new FileInputStream("person.txt"));
Person person2 = (Person) ois.readObject();
```



# String

## String是Java基本数据类型吗？可以被继承吗

不是，String是一个类，是引用数据类型



不可以被继承，String类使用final修饰，是不可变类，无法被继承



## String、StringBuilder和StringBuffer的区别

- String不可变，修改会创建新对象；StringBuilder可变，修改直接作用于原对象
- String频繁修改时性能低，因为每次修改都会创建新对象；StringBuilder性能最高
- StringBuffer 和 StringBuilder 类似，但每个方法上都加了 synchronized 关键字，所以是线程安全的



## String str1 = new String(“abc”) 和 String str2 = “abc”的区别

直接使用双引号为字符串变量赋值时，Java 首先会检查字符串常量池中是否已经存在相同内容的字符串。

如果存在，Java 就会让新的变量引用池中的那个字符串；如果不存在，它会创建一个新的字符串，放入池中，并让变量引用它。



使用 `new String("abc")` 的方式创建字符串时，实际分为两步：

- 第一步，先检查字符串字面量 "abc" 是否在字符串常量池中，如果没有则创建一个；如果已经存在，则引用它。
- 第二步，在堆中再创建一个新的字符串对象，并将其初始化为字符串常量池中 "abc" 的一个副本。

![三分恶面渣逆袭：堆与常量池中的String](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/javase-17.png)

```java
String s1 = "abc";
String s2 = "abc";
String s3 = new String("abc");

System.out.println(s1 == s2); // 输出 true，因为 s1 和 s2 引用的是字符串常量池中同一个对象。
System.out.println(s1 == s3); // 输出 false，因为 s3 是通过 new 关键字显式创建的，指向堆上不同的对象。
```

如果希望 `str1` 也指向常量池的 `"abc"`，可以使用 `intern()` 方法：

```java
String str1 = new String("abc").intern();
String str2 = "abc";
System.out.println(str1 == str2); // true
```



## String是不可变类吗

String 是不可变的，这意味着一旦一个 String 对象被创建，其存储的文本内容就不能被改变。这是因为：

①、不可变性使得 String 对象在使用中更加安全。因为字符串经常用作参数传递给其他 Java 方法，例如网络连接、打开文件等。

如果 String 是可变的，这些方法调用的参数值就可能在不知不觉中被改变，从而导致网络连接被篡改、文件被莫名其妙地修改等问题。

②、不可变的对象因为状态不会改变，所以更容易进行缓存和重用。字符串常量池的出现正是基于这个原因。

当代码中出现相同的字符串字面量时，JVM 会确保所有的引用都指向常量池中的同一个对象，从而节约内存。

③、因为 String 的内容不会改变，所以它的哈希值也就固定不变。这使得 String 对象特别适合作为 HashMap 或 HashSet 等集合的键，因为计算哈希值只需要进行一次，提高了哈希表操作的效率。



## String的字符串拼接是如何实现的

**1. 直接拼接**

```java
String s1 = "Hello" + "World";
```

由于 `"Hello"` 和 `"World"`都是字面量，编译器在 编译期 直接优化为：

```java
String s1 = "HelloWorld";
```

`"HelloWorld"` **直接存入字符串常量池**，不会在运行时发生拼接，不会创建新的对象



**2. +号拼接**

```java
String s2 = "Hello";
String s3 = s2 + "World";
```

由于 `s2` 是 变量，Java 无法在编译期确定最终值。

Java 会在运行时创建 `StringBuilder` 进行拼接：

```java
String s3 = new StringBuilder(s2).append("World").toString();
```

这个拼接操作会创建 **新的 `String` 对象**（在 **堆** 中）。



## 如何保证String不可变

第一，String 类内部使用一个私有的字符数组来存储字符串数据。这个字符数组在创建字符串时被初始化，之后不允许被改变。

```java
private final char value[];
```



第二，String 类没有提供任何可以修改其内容的公共方法，像 concat 这些看似修改字符串的操作，实际上都是返回一个新创建的字符串对象，而原始字符串对象保持不变。

```java
public String concat(String str) {
    if (str.isEmpty()) {
        return this;
    }
    int len = value.length;
    int otherLen = str.length();
    char buf[] = Arrays.copyOf(value, len + otherLen);
    str.getChars(buf, len);
    return new String(buf, true);
}
```



第三，String类本身被声明为final，这意味着它不能被继承。这防止了子类可能通过添加修改方法来改变字符串内容的可能性。

```java
public final class String
```



## intern方法有什么作用

- 如果当前字符串内容存在于字符串常量池，直接返回字符串常量池中的字符串
- 否则，将此String对象添加到池中，并返回String对象的引用



# Integer

## Integer a = 127, Integer b = 127; Integer c = 128, Integer d = 128; 相等吗

答案：a和b相等，c和d不相等



**对于a和b**

```java
Integer a = 127;
Integer b = 127;
```

`a`和`b`是相等的。这是因为 Java 在自动装箱过程中，会使用`Integer.valueOf()`方法来创建`Integer`对象。

`Integer.valueOf()`方法会针对数值在-128 到 127 之间的`Integer`对象使用缓存。因此，`a`和`b`实际上引用了常量池中相同的`Integer`对象。



**对于c和d**

```java
Integer c = 128;
Integer d = 128;
```

`c`和`d`不相等。这是因为 128 超出了`Integer`缓存的范围(-128 到 127)。



## 什么是Integer缓存池

根据实践发现，大部分的数据操作都集中在值比较小的范围，因此 Integer 搞了个缓存池，默认范围是 -128 到 127。

当我们使用自动装箱来创建这个范围内的 Integer 对象时，Java 会直接从缓存中返回一个已存在的对象，而不是每次都创建一个新的对象。这意味着，对于这个值范围内的所有 Integer 对象，它们实际上是引用相同的对象实例。

可以在运行的时候添加 `-Djava.lang.Integer.IntegerCache.high=1000` 来调整缓存池的最大值。



## new Integer(10) == new Integer(10) 相等吗

在 Java 中，使用`new Integer(10) == new Integer(10)`进行比较时，结果是 false。

这是因为 new 关键字会在堆（Heap）上为每个 Integer 对象分配新的内存空间，所以这里创建了两个不同的 Integer 对象，它们有不同的内存地址。

当使用==运算符比较这两个对象时，实际上比较的是它们的内存地址，而不是它们的值，因此即使两个对象代表相同的数值（10），结果也是 false。



## String怎么转成Integer的？原理是什么

String 转成 Integer，主要有两个方法：

- Integer.parseInt(String s)
- Integer.valueOf(String s)

不管哪一种，最终还是会调用 Integer 类内中的`parseInt(String s, int radix)`方法。

```java
public static int parseInt(String s, int radix)
                throws NumberFormatException
    {

        int result = 0;
        //是否是负数
        boolean negative = false;
        //char字符数组下标和长度
        int i = 0, len = s.length();
        ……
        int digit;
        //判断字符长度是否大于0，否则抛出异常
        if (len > 0) {
            ……
            while (i < len) {
                // Accumulating negatively avoids surprises near MAX_VALUE
                //返回指定基数中字符表示的数值。（此处是十进制数值）
                digit = Character.digit(s.charAt(i++),radix);
                //进制位乘以数值
                result *= radix;
                result -= digit;
            }
        }
        //根据上面得到的是否负数，返回相应的值
        return negative ? result : -result;
    }
```

![image-20250220112633355](C:/Users/shiyu/AppData/Roaming/Typora/typora-user-images/image-20250220112633355.png)



# Object

## Object类的常见方法

Java中所有类都是Object类的子类

**1. 对象比较**

- hashCode()
- equals(Object obj)

**2. 对象拷贝**

- clone()
  - 默认实现只做浅拷贝，且类必须实现 Cloneable 接口。
  - Object 本身没有实现 Cloneable 接口，所以在不重写 clone 方法的情况下直接直接调用该方法会发生 CloneNotSupportedException 异常。

**3. 对象转字符串**

- toString()
  - 默认实现返回类名@哈希码的十六进制表示，但通常会被重写以返回更有意义的信息。

**4. 多线程调度**

- wait()
- wait(long timeout)
- wait(long timeout, int nanos)
- notify()
- notifyAll()

**5. 反射**

- getClass()

**6. 垃圾回收**

- finalize()



# 异常处理

## Java中异常处理体系

Java 中的异常处理机制用于处理程序运行过程中可能发生的各种异常情况，通常通过 try-catch-finally 语句和 throw 关键字来实现。

![三分恶面渣逆袭：Java异常体系](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/javase-22.png)

- `Throwable` 是 Java 语言中所有错误和异常的基类。它有两个主要的子类：`Error` 和 `Exception`，这两个类分别代表了 Java 异常处理体系中的两个分支。

- `Error` 类代表那些严重的错误，这类错误通常是程序无法处理的。比如，OutOfMemoryError 表示内存不足，StackOverflowError 表示栈溢出。这些错误通常与 JVM 的运行状态有关，一旦发生，应用程序通常无法恢复。

- `Exception` 类代表程序可以处理的异常。它分为两大类：**编译时异常**（Checked Exception）和**运行时异常**（Runtime Exception）。

  - 编译时异常（Checked Exception）：这类异常在编译时必须被显式处理（捕获或声明抛出）。

    如果方法可能抛出某种编译时异常，但没有捕获它（try-catch）或没有在方法声明中用 throws 子句声明它，那么编译将不会通过。例如：IOException、SQLException 等。

  - 运行时异常（Runtime Exception）：这类异常在运行时抛出，它们都是 RuntimeException 的子类。对于运行时异常，Java 编译器不要求必须处理它们（即不需要捕获也不需要声明抛出）。

    运行时异常通常是由程序逻辑错误导致的，如 NullPointerException、IndexOutOfBoundsException 等。



## 异常的处理方式

1. 遇到异常时可以不处理，直接通过throw 和 throws 抛出异常，交给上层调用者处理。

   throws 关键字用于声明可能会抛出的异常, 可以声明**多个异常**，不代表真的会抛出

   ```java
   public void test() throws Exception {
       throw new Exception("抛出异常");
   }
   ```

   而 throw 关键字用于抛出异常，只能抛出**一个具体的异常对象**

   ```java
   public class ThrowsExample {
       public static void main(String[] args) throws InterruptedException {
           Thread.sleep(1000); // 可能抛出 InterruptedException
       }
   }
   ```

2. 使用try - catch捕获异常，处理异常

   ```java
   try {
       //包含可能会出现异常的代码以及声明异常的方法
   }catch(Exception e) {
       //捕获异常并进行处理
   }finally {
       //可选，必执行的代码
   }
   ```

   

## catch和finally的异常可以同时抛出吗

如果 catch 块抛出一个异常，而 finally 块中也抛出异常，那么最终抛出的将是 finally 块中的异常。catch 块中的异常会被丢弃，而 finally 块中的异常会覆盖并向上传递。

```java
public class Example {
    public static void main(String[] args) {
        try {
            throw new Exception("Exception in try");
        } catch (Exception e) {
            throw new RuntimeException("Exception in catch");
        } finally {
            throw new IllegalArgumentException("Exception in finally");
        }
    }
}
```

- try 块首先抛出一个 Exception。
- 控制流进入 catch 块，catch 块中又抛出了一个 RuntimeException。
- 但是在 finally 块中，抛出了一个 IllegalArgumentException，最终程序抛出的异常是 finally 块中的 IllegalArgumentException。



## 异常处理代码题

```java
public class TryDemo {
    public static void main(String[] args) {
        System.out.println(test1());
    }
    public static int test1() {
        try {
            return 2;
        } finally {
            return 3;
        }
    }
}
```

执行结果：3。

try 返回前先执行 finally，结果 finally 里不按套路出牌，直接 return 了，自然也就走不到 try 里面的 return 了。



```java
public class TryDemo {
    public static void main(String[] args) {
        System.out.println(test1());
    }
    public static int test1() {
        int i = 0;
        try {
            i = 2;
            return i;
        } finally {
            i = 3;
        }
    }
}
```

执行结果：2。

在执行 finally 之前，JVM 会先将 i 的结果暂存起来，然后 finally 执行完毕后，会返回之前暂存的结果，而不是返回 i，所以即使 i 已经被修改为 3，最终返回的还是之前暂存起来的结果 2。



# I/O

## Java中IO流分为几种

**按照数据流方向划分**

- 输入流：从源（如文件、网络等）读取数据到程序。
- 输出流：将数据从程序写出到目的地（如文件、网络、控制台等）。



**按处理数据单位划分**

- 字节流：以字节为单位读写数据，主要用于处理二进制数据，如音频、图像文件等。
- 字符流：以字符为单位读写数据，主要用于处理文本数据。



**按功能划分**

- 节点流：直接与数据源或目的地相连，如 FileInputStream、FileOutputStream。
- 处理流：对一个已存在的流进行包装，如缓冲流 BufferedInputStream、BufferedOutputStream。
- 管道流：用于线程之间的数据传输，如 PipedInputStream、PipedOutputStream。



## IO流用到了什么设计模式

Java 的 IO 流体系用到了一个设计模式——**装饰器模式**。

![Java IO流用到装饰器模式](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/javase-25.png)



## 有了字节流为什么还需要字符流

其实字符流是由 Java 虚拟机将字节转换得到的，问题就出在这个过程还比较耗时，并且，如果我们不知道编码类型就很容易出现乱码问题。

所以， I/O 流就干脆提供了一个直接操作字符的接口，方便我们平时对字符进行流操作。如果音频文件、图片等媒体文件用字节流比较好，如果涉及到字符的话使用字符流比较好。



## BIO、NIO、AIO之间的区别

Java 常见的 IO 模型有三种：BIO、NIO 和 AIO。

BIO：采用阻塞式 I/O 模型，线程在执行 I/O 操作时被阻塞，无法处理其他任务，适用于连接数较少的场景。

NIO：采用非阻塞 I/O 模型，线程在等待 I/O 时可执行其他任务，通过 Selector 监控多个 Channel 上的事件，适用于连接数多但连接时间短的场景。

AIO：使用异步 I/O 模型，线程发起 I/O 请求后立即返回，当 I/O 操作完成时通过回调函数通知线程，适用于连接数多且连接时间长的场景。



## 简单说一下BIO

BIO，也就是传统的 IO，基于字节流或字符流（如 FileInputStream、BufferedReader 等）进行文件读写，基于 Socket 和 ServerSocket 进行网络通信。

对于每个连接，都需要创建一个独立的线程来处理读写操作。

![三分恶面渣逆袭：BIO](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/javase-27.png)



## 简单说一下NIO

NIO 的魅力主要体现在网络编程中，服务器可以用一个线程处理多个客户端连接，通过 Selector 监听多个 Channel 来实现多路复用，极大地提高了网络编程的性能。

![三分恶面渣逆袭：NIO](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/javase-28.png)

缓冲区 Buffer 也能极大提升一次 IO 操作的效率。

![三分恶面渣逆袭：NIO完整示意图](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/javase-29.png)



## 简单说一下AIO



它引入了异步通道的概念，使得 I/O 操作可以异步进行。这意味着线程发起一个读写操作后不必等待其完成，可以立即进行其他任务，并且当读写操作真正完成时，线程会被异步地通知。

```java
AsynchronousFileChannel fileChannel = AsynchronousFileChannel.open(Paths.get("test.txt"), StandardOpenOption.READ);
ByteBuffer buffer = ByteBuffer.allocate(1024);
Future<Integer> result = fileChannel.read(buffer, 0);
while (!result.isDone()) {
    // do something
}
```



# 序列化

## 什么是序列化和反序列化

序列化（Serialization）是指将对象转换为字节流的过程，以便能够将该对象保存到文件、数据库，或者进行网络传输。

反序列化（Deserialization）就是将字节流转换回对象的过程，以便构建原始对象。



## serialVersionUID有什么用

serialVersionUID 是 Java 序列化机制中用于标识类版本的唯一标识符。它的作用是确保在序列化和反序列化过程中，类的版本是兼容的。

```java
import java.io.Serializable;

public class MyClass implements Serializable {
    private static final long serialVersionUID = 1L;
    private String name;
    private int age;

    // getters and setters
}
```

serialVersionUID 被设置为 1L 是一种比较省事的做法，也可以使用 Intellij IDEA 进行自动生成。

但只要 serialVersionUID 在序列化和反序列化过程中保持一致，就不会出现问题。

如果不显式声明 serialVersionUID，Java 运行时会根据类的详细信息自动生成一个 serialVersionUID。那么当类的结构发生变化时，自动生成的 serialVersionUID 就会发生变化，导致反序列化失败。



## Java序列化不包含静态变量吗

是的，序列化机制只会保存对象的状态，而静态变量属于类的状态，不属于对象的状态。



## 如果有些变量不想序列化怎么办

```java
public class Person implements Serializable {
    private String name;
    private transient int age;
    // 省略 getter 和 setter 方法
}
```



## 说说几种序列化方式

**1. Java对象序列化**

- Java 原生序列化方法即通过 Java 原生流(InputStream 和 OutputStream 之间的转化)的方式进行转化，一般是对象输出流 `ObjectOutputStream`和对象输入流`ObjectInputStream`。

**2. Json序列化**

- 个可能是我们最常用的序列化方式，Json 序列化的选择很多，一般会使用 jackson 包，通过 ObjectMapper 类来进行一些操作，比如将对象转化为 byte 数组或者将 json 串转化为对象。

**3. ProtoBuff序列化**

- ProtocolBuffer 是一种轻便高效的结构化数据存储格式，ProtoBuff 序列化对象可以很大程度上将其压缩，可以大大减少数据传输大小，提高系统性能。



# 网络编程

## 了解过Socket网络套接字吗

Socket 是网络通信的基础，表示两台设备之间通信的一个端点。Socket 通常用于建立 TCP 或 UDP 连接，实现进程间的网络通信。



## RPC框架了解吗





# 泛型

## Java泛型了解吗

泛型主要用于提高代码的类型安全，它允许在定义类、接口和方法时使用类型参数，这样可以在编译时检查类型一致性，避免不必要的类型转换和类型错误。

没有泛型的时候，像 List 这样的集合类存储的是 Object 类型，导致从集合中读取数据时，必须进行强制类型转换，否则会引发 ClassCastException。

```java
List list = new ArrayList();
list.add("hello");
String str = (String) list.get(0);  // 必须强制类型转换
```



泛型一般有三种使用方式:**泛型类**、**泛型接口**、**泛型方法**。



**1. 泛型类**

定义泛型类

```java
//此处T可以随便写为任意标识，常见的如T、E、K、V等形式的参数常用于表示泛型
//在实例化泛型类时，必须指定T的具体类型
public class Generic<T>{

    private T key;

    public Generic(T key) {
        this.key = key;
    }

    public T getKey(){
        return key;
    }
}
```

实例化泛型类

```java
Generic<Integer> genericInteger = new Generic<Integer>(123456);
```



**2. 泛型接口**

定义泛型接口

```java
public interface Generator<T> {
    public T method();
}
```

实现泛型接口，指定类型

```java
class GeneratorImpl<T> implements Generator<String>{
    @Override
    public String method() {
        return "hello";
    }
}
```



**3. 泛型方法**

定义泛型方法

```java
   public static < E > void printArray( E[] inputArray )
   {
         for ( E element : inputArray ){
            System.out.printf( "%s ", element );
         }
         System.out.println();
    }
```

使用

```java
// 创建不同类型数组： Integer, Double 和 Character
Integer[] intArray = { 1, 2, 3 };
String[] stringArray = { "Hello", "World" };
printArray( intArray  );
printArray( stringArray  );
```



## 什么是泛型擦除

所谓的泛型擦除，官方名叫“类型擦除”。

Java 的泛型是伪泛型，这是因为 Java 在编译期间，所有的类型信息都会被擦掉。

也就是说，在运行的时候是没有泛型的。

例如这段代码，往一群猫里放条狗：

```java
LinkedList<Cat> cats = new LinkedList<Cat>();
LinkedList list = cats;  // 注意我在这里把范型去掉了，但是list和cats是同一个链表！
list.add(new Dog());  // 完全没问题！
```

因为 Java 的范型只存在于源码里，编译的时候给你静态地检查一下范型类型是否正确，而到了运行时就不检查了。上面这段代码在 JRE（Java**运行**环境）看来和下面这段没区别：

```java
LinkedList cats = new LinkedList();  // 注意：没有范型！
LinkedList list = cats;
list.add(new Dog());
```



## 为什么要类型擦除

主要是为了向下兼容，因为 JDK5 之前是没有泛型的，为了让 JVM 保持向下兼容，就出了类型擦除这个策略。



# 注解

## 说一下你对注解的理解

注解可以标记在类上、方法上、属性上等，标记自身也可以设置一些值，有了标记之后，我们就可以在编译或者运行阶段去识别这些标记，然后搞一些事情，这就是注解的用处。



**注解的生命周期**

- RetentionPolicy.SOURCE：给编译器用的，不会写入 class 文件
- RetentionPolicy.CLASS：会写入 class 文件，在类加载阶段丢弃，也就是运行的时候就没这个信息了
- RetentionPolicy.RUNTIME：会写入 class 文件，永久保存，可以通过反射获取注解信息



# 反射

## 什么是反射

反射允许 Java 在运行时检查和操作类的方法和字段。通过反射，可以动态地获取类的字段、方法、构造方法等信息，并在运行时调用方法或访问字段。

![三分恶面渣逆袭：Java反射相关类](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/javase-36.png)

比如说我们可以动态加载类并创建对象：

```java
String className = "java.util.Date";
Class<?> cls = Class.forName(className);
Object obj = cls.newInstance();
System.out.println(obj.getClass().getName());
```

比如说我们可以这样来访问字段和方法：

```java
// 加载并实例化类
Class<?> cls = Class.forName("java.util.Date");
Object obj = cls.newInstance();

// 获取并调用方法
Method method = cls.getMethod("getTime");
Object result = method.invoke(obj);
System.out.println("Time: " + result);

// 访问字段
Field field = cls.getDeclaredField("fastTime");
field.setAccessible(true); // 对于私有字段需要这样做
System.out.println("fastTime: " + field.getLong(obj));
```



## 反射有哪些应用场景

**1. Spring框架大量使用了反射来动态加载和管理Bean**



**2. Java的动态代理机制就使用了反射来创建代理类。代理类可以在运行时动态处理方法调用**



**3. JUnit 和 TestNG 等测试框架使用反射机制来发现和执行测试方法。反射允许框架扫描类，查找带有特定注解（如 `@Test`）的方法，并在运行时调用它们。**



## 反射的原理是什么

Java 程序的执行分为编译和运行两步，编译之后会生成字节码(.class)文件，JVM 进行类加载的时候，会加载字节码文件，将类型相关的所有信息加载进方法区，反射就是去获取这些信息，然后进行各种操作。



# JDK1.8新特性

- Java 8 允许在接口中添加默认方法和静态方法。

  ```java
  public interface MyInterface {
      default void myDefaultMethod() {
          System.out.println("My default method");
      }
  
      static void myStaticMethod() {
          System.out.println("My static method");
      }
  }
  ```

- Lambda 表达式描述了一个代码块（或者叫匿名方法），可以将其作为参数传递给构造方法或者普通方法以便后续执行。

  ```java
  public class LamadaTest {
      public static void main(String[] args) {
          new Thread(() -> System.out.println("沉默王二")).start();
      }
  }
  ```

- Stream 是对 Java 集合框架的增强，它提供了一种高效且易于使用的数据处理方式。

  ```java
  List<String> list = new ArrayList<>();
  list.add("中国加油");
  list.add("世界加油");
  list.add("世界加油");
  
  long count = list.stream().distinct().count();
  System.out.println(count);
  ```

- 引入 Optional 减少空指针异常。

  ```java
  Optional<String> optional = Optional.of("沉默王二");
  optional.isPresent();           // true
  optional.get();                 // "沉默王二"
  optional.orElse("沉默王三");    // "bam"
  optional.ifPresent((s) -> System.out.println(s.charAt(0)));     // "沉"
  ```



## Lambda表达式了解多少

Lambda 表达式主要用于提供一种简洁的方式来表示匿名方法，使 Java 具备了函数式编程的特性。

比如说我们可以使用 Lambda 表达式来简化线程的创建：

```java
new Thread(() -> System.out.println("Hello World")).start();
```

比以前的匿名内部类要简洁很多。



所谓的函数式编程，就是把函数作为参数传递给方法，或者作为方法的结果返回。比如说我们可以配合 Stream 流进行数据过滤：

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6);
List<Integer> evenNumbers = numbers.stream()
    .filter(n -> n % 2 == 0)
    .collect(Collectors.toList());
```

其中 `n -> n % 2 == 0` 就是一个 Lambda 表达式。表示传入一个参数 n，返回 `n % 2 == 0` 的结果



