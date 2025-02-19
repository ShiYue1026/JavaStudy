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

