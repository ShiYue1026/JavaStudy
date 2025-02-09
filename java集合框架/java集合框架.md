# 基础

## Java中有哪些常见的集合框架

![二哥的 Java 进阶之路：Java集合主要关系](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/collection/gailan-01.png)

Java集合框架可以分为两大支线：

**1. Collection**：主要由List、Set、Queue组成

- List：代表有序、可重复的集合，如ArrayList和LinkedList
- Set：代表无序、不可重复的集合，如HashSet和TreeSet
- Queue：代表队列，如双端队列ArrayDeque以及优先级队列PriorityQueue

**2. Map**：代表键值对的集合

- 代表键值对的集合，如HashMap



## 简单介绍一下队列Queue

Java中的队列主要通过Queue接口和BlockingQueue两个接口来实现。

**PriorityQueue**

- PriorityQueue 是一个基于优先级堆的无界队列，它的元素按照自然顺序排序或者 Comparator 进行排序。

  ![李豪：优先级队列](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/collection/PriorityQueue-8dca2f55-a7c7-49e1-95a5-df1a34f2aef5.png)

**ArrayDeque**

- ArrayDeque是一个基于数组的双端队列，可以在两端插入和删除元素

  ![李豪：双端队列](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/collection/arraydeque-1e7086a3-3d31-4553-aa16-5eaf2193649e.png)

**LinkedList**

- LinkedList既可以当作List使用也可以当作Queue使用

  ![二哥的 Java 进阶之路](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/collection/list-war-2-02.png)

## 用过哪些集合类，它们的优劣是什么

**1. ArrayList**：

- ArrayList 可以看作是一个动态数组，它可以在运行时动态扩容
- 优点是访问速度快，可以**通过索引直接查到元素**
- 缺点是**插入和删除元素可能需要移动元素**，效率就会降低

**2. LinkedList**

- LinkedList是一个双向链表，适合频繁的插入和删除操作
- 优点是**插入和删除元素的时候只需要改变节点的前后指针**
- 缺点是**访问元素时需要遍历链表**

**3. HashMap**

- HashMap是一个基于哈希表的键值对集合
- 优点是**插入、删除和查找元素的速度都很快**
- 缺点是**不保留键值对的插入顺序**

**4. LinkedHashMap**：

- LinkedHashMap在HashMap的基础上增加了一个**双向链表**来**保持键值对的插入顺序**



## 哪些集合是线程安全的

像 Vector、Hashtable、ConcurrentHashMap、CopyOnWriteArrayList、ConcurrentLinkedQueue、ArrayBlockingQueue、LinkedBlockingQueue 这些都是线程安全的



# List

## ArrayList和LinkedList有什么区别

ArrayList 是基于数组实现的，LinkedList 是基于链表实现的。

![三分恶面渣逆袭：ArrayList和LinkedList的数据结构](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/collection-2.png)

**用途有什么不同**：

多数情况下，ArrayList更利于查找，LinkedList更利于删减

- 由于 ArrayList 是基于数组实现的，所以 `get(int index)` 可以直接通过数组下标获取，时间复杂度是 O(1)；LinkedList 是基于链表实现的，`get(int index)` 需要遍历链表，时间复杂度是 O(n)。

- ArrayList 如果增删的是数组的尾部，直接插入或者删除就可以了，时间复杂度是 O(1)；如果 add 的时候涉及到扩容，时间复杂度会提升到 O(n)。

  但如果插入的是中间的位置，就需要把插入位置后的元素向前或者向后移动，甚至还有可能触发扩容，效率就会低很多，O(n)。

- LinkedList 因为是链表结构，插入和删除只需要改变前置节点、后置节点和插入节点的引用就行了，不需要移动元素。

  如果是在链表的头部插入或者删除，时间复杂度是 O(1)；如果是在链表的中间插入或者删除，时间复杂度是 O(n)，因为需要遍历链表找到插入位置；如果是在链表的尾部插入或者删除，时间复杂度是 O(1)。



注意：**LinkedList 更利于增删不是体现在时间复杂度上，因为二者增删的时间复杂度都是 O(n)**，都需要遍历列表；而是体现在增删的效率上，因为 LinkedList 的增删只需要改变引用，而 ArrayList 的增删可能需要移动元素。



## ArrayList和LinkedList使用场景有什么不同

ArrayList 适用于：

- 随机访问频繁：需要频繁通过索引访问元素的场景。
- 读取操作远多于写入操作：如存储不经常改变的列表。
- 末尾添加元素：需要频繁在列表末尾添加元素的场景。

LinkedList 适用于：

- 频繁插入和删除：在列表中间频繁插入和删除元素的场景。
- 不需要快速随机访问：顺序访问多于随机访问的场景。
- 队列和栈：由于其双向链表的特性，LinkedList 可以高效地实现队列（FIFO）和栈（LIFO）。



## ArrayList的扩容机制了解吗

![三分恶面渣逆袭：ArrayList扩容](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/collection-5.png)

ArrayList 确切地说，应该叫做动态数组，因为它的底层是通过数组来实现的，当往 ArrayList 中添加元素时，会先检查是否需要扩容，如果当前容量+1 超过数组长度，就会进行扩容。

```java
private void grow(int minCapacity) {
    // overflow-conscious code
    int oldCapacity = elementData.length;
    int newCapacity = oldCapacity + (oldCapacity >> 1);
    if (newCapacity - minCapacity < 0)
        newCapacity = minCapacity;
    if (newCapacity - MAX_ARRAY_SIZE > 0)
        newCapacity = hugeCapacity(minCapacity);
    // minCapacity is usually close to size, so this is a win:
    elementData = Arrays.copyOf(elementData, newCapacity);
}
```



## ArrayList怎么序列化的知道吗？为什么应transient修饰数组

ArrayList 的序列化使用`transient`修饰存储元素的`elementData`的数组，`transient`关键字的作用是让被修饰的成员属性不被序列化。

**为什么 ArrayList 不直接序列化元素数组呢？**

- 出于效率的考虑，数组可能长度 100，但实际只用了 50，剩下的 50 不用其实不用序列化，这样可以提高序列化和反序列化的效率，还可以节省内存空间。

**那ArrayList怎么序列化**

![image-20250209114002266](https://github.com/user-attachments/assets/2d02d289-fcc9-4970-8af9-c56e6063e829)


- ArrayList 通过两个方法**readObject、writeObject**自定义序列化和反序列化策略，实际直接使用两个流`ObjectOutputStream`和`ObjectInputStream`来进行序列化和反序列化。



## 快速失败(fail-fast)和安全失败(fail-safe)了解吗

**快速失败(fail-fast)**：是Java集合的一种错误检测机制

- 在用迭代器遍历一个集合对象时，如果线程 A 遍历过程中，线程 B 对集合对象的内容进行了修改（增加、删除、修改），则会抛出 Concurrent Modification Exception。
- 原理：迭代器在遍历时直接访问集合中的内容，并且在遍历过程中使用一个 `modCount` 变量。集合在被遍历期间如果内容发生变化，就会改变`modCount`的值。每当迭代器使用 hashNext()/next()遍历下一个元素之前，都会检测 `modCount` 变量是否为 `expectedmodCount` 值，是的话就返回遍历；否则抛出异常，终止遍历。

**安全失败(fail-safe)**:

- 采用安全失败机制的集合容器，在遍历时不是直接在集合内容上访问的，而是**先复制原有集合内容，在拷贝的集合上进行遍历**。

- 由于迭代时是对原集合的拷贝进行遍历，所以在遍历过程中对原集合所作的修改并不能被迭代器检测到，所以不会触发 Concurrent Modification Exception。



## 有几种实现ArrayList线程安全的方法

- 可以使用 `Collections.synchronizedList()` 方法，它将返回一个线程安全的 List。

  ```java
  SynchronizedList list = Collections.synchronizedList(new ArrayList());
  ```

  

- 也可以直接使用 CopyOnWriteArrayList，它是线程安全的，遵循**写时复制**的原则，每当对列表进行修改（例如添加、删除或更改元素）时，都会创建列表的一个新副本，这个新副本会替换旧的列表，而对旧列表的所有读取操作仍然可以继续。

  ```java
  CopyOnWriteArrayList list = new CopyOnWriteArrayList();
  ```



## ArrayList和Vector区别

Vector 属于 JDK 1.0 时期的遗留类，已不推荐使用，仍然保留着是因为 Java 希望向后兼容。

ArrayList 是在 JDK 1.2 时引入的，用于替代 Vector 作为主要的非同步动态数组实现。因为 Vector 所有的方法都使用 synchronized 关键字进行了同步，单线程环境下效率较低。



## CopyOnWriteArrayList了解多少

CopyOnWriteArrayList 就是线程安全版本的 ArrayList。

它的名字叫`CopyOnWrite`——**写时复制**，已经明示了它的原理。

![CopyOnWriteArrayList原理](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/collection-7.png)

- CopyOnWriteArrayList 采用了一种**读写分离**的并发策略。CopyOnWriteArrayList 容器允许并发读，读操作是无锁的，性能较高。
- 至于写操作，比如向容器中添加一个元素，则首先将当前容器复制一份，然后在新副本上执行写操作，结束之后再将原容器的引用指向新容器。



# Map

## 能说一下HashMap的底层数据结构吗

JDK8中HashMap的数据结构是**数组+链表+红黑树**

![三分恶面渣逆袭：JDK 8 HashMap 数据结构示意图](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/collection-8.png)

- 当多个键经过哈希处理后得到相同的索引时，需要通过链表来解决哈希冲突——即将具有相同索引的键值对通过链表连接起来

- 不过，链表过长时，查询效率会比较低，于是当链表的长度超过 8 时（且数组的长度大于 64），链表就会转换为红黑树。红黑树的查询效率是 O(logn)，比链表的 O(n) 要快
- HashMap 的初始容量是 16，随着元素的不断添加，HashMap 的容量可能不足，于是就需要进行扩容，阈值是`capacity * loadFactor`，capacity 为容量，loadFactor 为负载因子，默认为 0.75
- 扩容后的数组大小是原来的 2 倍，然后把原来的元素重新计算哈希值，放到新的数组中，这一步也是**HashMap 最耗时的操作**



## 对红黑树了解多少

![三分恶面渣逆袭：红黑树](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/collection-9.png)

- **左根右**：红黑树是一棵二叉搜索树（左 < 根 < 右）
- **根叶黑**：根和叶子(NULL)都是黑色
- **不红红**：红色节点的所有子节点都是黑色（不存在连续的两个红色节点）
- **黑路同**：任一节点到叶所有路径黑色节点数量相同
- 插入节点默认为红色，只可能违反根叶黑或者不红红

这些性质保证了红黑树任一节点左右子树的高度差不会超过两倍



**红黑树怎么保持平衡**

- 如果插入后没有破坏上面四个性质，不用调整

- 插入节点是根节点 => 直接变黑

- 插入节点的叔叔是红色 => 叔父爷变色，爷爷变插入节点接着判断

  ![image-20250209124711316](https://github.com/user-attachments/assets/9449dcf5-84d4-4c0d-b3fd-5d609e044792)


- 插入节点的叔叔是黑色

  - **LL型**

    ![image-20250209124205439](https://github.com/user-attachments/assets/e3711ef2-88e4-4e58-971c-817f572e97a7)

![image-20250209124223068](https://github.com/user-attachments/assets/aa66f020-74c6-4e8f-8e9b-b744c91ce245)

![image-20250209124248563](https://github.com/user-attachments/assets/b2492dea-6468-4b54-b01d-1bfaffcebc30)


  - **RR型**

    ![image-20250209124300269](https://github.com/user-attachments/assets/9bed35f2-825a-4170-8025-2e2049728d12)

![image-20250209124314613](https://github.com/user-attachments/assets/60dd64df-fce7-46a6-a6fd-d4996b2034a9)

![image-20250209124336050](https://github.com/user-attachments/assets/4e554624-99a5-423a-b21f-bf346102d7ca)



  - **LR型**

    ![image-20250209124406486](https://github.com/user-attachments/assets/7d2338de-b3e9-442f-93b9-8b62c364a24e)

![image-20250209124440414](https://github.com/user-attachments/assets/4dc42c99-936b-42b7-9f90-574049b1b8a1)

![image-20250209124450097](https://github.com/user-attachments/assets/76b909b1-c1e1-4001-9c26-aa289c348026)

![image-20250209124506273](https://github.com/user-attachments/assets/ea5be996-8881-49cd-af6c-02b83b9528d2)


​    

  - **RL型**

    ![image-20250209124532887](https://github.com/user-attachments/assets/bb34286c-4b43-4cfa-859e-1840e518d33b)

![image-20250209124548063](https://github.com/user-attachments/assets/81fc7cea-8af7-4ddd-a494-3cf4ec6feece)

![image-20250209124602507](https://github.com/user-attachments/assets/22ddcac8-1dd0-4692-86a7-010ac19983ab)

![image-20250209124613327](https://github.com/user-attachments/assets/e3d2c5d7-e5ea-4555-9f2b-7c4b55a3a3b1)


​    



## 为什么不用二叉树/平衡树呢

**为什么不用二叉树**

- 二叉树容易出现极端情况，比如插入的数据是有序的，那么二叉树就会退化成链表，查询效率就会变成 O(n)。

**为什么不用平衡二叉树**

- 平衡二叉树比红黑树的要求更高，每个节点的左右子树的高度最多相差 1，这种高度的平衡保证了极佳的查找效率，但在进行插入和删除操作时，可能需要频繁地进行旋转来维持树的平衡，维护成本更高。



## HashMap的put流程知道吗

![三分恶面渣逆袭：HashMap插入数据流程图](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/collection-13.jpg)

**1. 通过hash方法计算key的哈希值**

```java
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```

**2. 数组进行第一次扩容**

```java
if ((tab = table) == null || (n = tab.length) == 0)
    n = (tab = resize()).length;
```

**3. 根据哈希值计算key在数组中的下标，如果对应下标没有元素，直接插入**

```java
if ((p = tab[i = (n - 1) & hash]) == null)
    tab[i] = newNode(hash, key, value, null);
```

否则，判断是否为相同的key，是则覆盖value。不是的话需要判断是否为树节点，是则向树中插入节点，否则向链表中插入数据

```java
else {  // 🚀 发生哈希冲突（key 映射到已有的桶 `p`）
    Node<K,V> e; K k;
    
    // 🚀 1. 检查头节点 `p` 是否是要插入的 key
    if (p.hash == hash &&
        ((k = p.key) == key || (key != null && key.equals(k))))
        e = p; // ✅ key 已存在，直接覆盖 value
    
    // 🚀 2. 如果是红黑树（TreeNode），调用 `putTreeVal()` 处理
    else if (p instanceof TreeNode)
        e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
    
    // 🚀 3. 否则，采用链表遍历查找或插入
    else {
        for (int binCount = 0; ; ++binCount) {  
            if ((e = p.next) == null) {  // 🚀 3.1 遍历到链表尾部，插入新节点
                p.next = newNode(hash, key, value, null);
                
                // 🚀 3.2 如果链表长度 ≥ `TREEIFY_THRESHOLD - 1`（即 >= 7），转换为红黑树
                if (binCount >= TREEIFY_THRESHOLD - 1)
                    treeifyBin(tab, hash); 
                break;
            }
            
            // 🚀 3.3 如果找到相同 key，则跳出循环，后续覆盖 value
            if (e.hash == hash &&
                ((k = e.key) == key || (key != null && key.equals(k))))
                break;
            
            p = e; // 🚀 继续遍历链表
        }
    }
}
```

所有元素处理完后，还需要判断是否超过阈值`threshold`，超过则扩容。

```java
if (++size > threshold)
    resize();
```



## 只重写equals没重写hashcode，Map put时会发生什么

如果只重写 equals 方法，没有重写 hashcode 方法，那么会导致 equals 相等的两个对象，hashcode 不相等，这样的话，这两个对象会被放到不同的桶中，这样就会导致 get 的时候，找不到对应的值。

```java
public class HashMapTest {
    public static void main(String[] args) {
        Map<Person, String> map = new HashMap<>();

        Person p1 = new Person("Alice");
        Person p2 = new Person("Alice");

        map.put(p1, "Developer");
        map.put(p2, "Manager"); // 🚀 p2.equals(p1) 为 true，但 hashCode 不同

        System.out.println(map.size()); // ❌ 2（应该是 1）
        System.out.println(map.get(p1)); // "Developer"
        System.out.println(map.get(p2)); // "Manager"（两个不同的 key）
    }
}
```



## HashMap怎么查找元素呢

![HashMap查找流程图](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/collection-14.png)

HashMap 的查找就简单很多：

1. 使用扰动函数，获取新的哈希值
2. 计算数组下标，获取节点
3. 当前节点和 key 匹配，直接返回
4. 否则，当前节点是否为树节点，查找红黑树
5. 否则，遍历链表查找



## HashMap的hash函数是怎么设计的

![三分恶面渣逆袭：hash 函数示意图](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/collection-16.jpg)

HashMap 的哈希函数是先拿到 key 的 hashcode，是一个 32 位的 int 类型的数值，然后让 hashcode 的高 16 位和低 16 位进行异或操作。

**高 16 位不变**

**低 16 位 = 原低 16 位 ⊕（异或） 高 16 位**

```java
static final int hash(Object key) {
    int h;
    // key的hashCode和key的hashCode右移16位做异或运算
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```

这么设计是为了降低哈希碰撞的概率。



## 为什么这个hash函数能降低哈希碰撞

比如一开始HashMap的数组大小为16，通过`hash & (n-1)`来计算下标，

但问题是hash & (24−1) 的结果实际上是取 hash 的低 4 位，那么一个大的哈希值，也只取最后 4 位，不就等于哈希值的高位都丢弃了吗

改进后的处理既考虑了高位的信息，又没有完全忽视低位原本的信息，尝试达到一个平衡状态。



## 为什么HashMap的容量是2 的整数次幂呢

为了使两个运算 `hash & (length - 1)`替换`hash % length` 。在计算机中，位运算的速度要远高于取余运算，因为计算机本质上就是二进制。



## 如果初始化HashMap，传一个17容量，它会怎么处理

HashMap会将这个值转换为大于或等于17的最小的2的幂。

因此，如果传入 17 作为初始容量，HashMap 实际上会被初始化为大小为 32 的哈希表。



在 HashMap 的初始化构造方法中，有这样⼀段代码：

```java
public HashMap(int initialCapacity, float loadFactor) {
 ...
 this.loadFactor = loadFactor;
 this.threshold = tableSizeFor(initialCapacity);
}
```

阀值 threshold 会通过方法` tableSizeFor()` 进行计算。

```java
static final int tableSizeFor(int cap) {
    int n = cap - 1;
    n |= n >>> 1;
    n |= n >>> 2;
    n |= n >>> 4;
    n |= n >>> 8;
    n |= n >>> 16;
    return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
}
```

 n 的所有低位都是 1，所以 n + 1 就是大于 cap 的最小的 2 的幂次方。



## 初始化HashMap的时候需要传入容量值吗

在创建 HashMap 时可以指定初始容量值。这个容量是指 Map 内部用于存储数据的数组大小。

如果预先知道 Map 将存储大量键值对，提前指定一个足够大的初始容量可以**减少因扩容导致的重哈希（rehashing）操作**，从而提高性能。

因为每次扩容时，HashMap 需要新分配一个更大的数组并重新将现有的元素插入到这个新数组中，这个过程相对耗时，尤其是当 Map 中已有大量数据时。

当然了，过大的初始容量会浪费内存，特别是当实际存储的元素远少于初始容量时。**如果不指定初始容量，HashMap 将使用默认的初始容量 16**。



## 还知道哪些哈希函数的构造方法

**1. 除留取余法（HashMap中的方法）**

**2. 直接定址法**

- 直接根据`key`来映射到对应的数组位置，例如 1232 放到下标 1232 的位置。

**3. 数字分析法**

- 取`key`的某些数字（例如十位和百位）作为映射的位置

**4. 平方取中法**

- 取`key`平方的中间几位作为映射的位置

**5. 折叠法**

- 将`key`分割成位数相同的几段，然后把它们的叠加和作为映射的位置

**6. 一致性哈希**



## 解决哈希冲突有哪些方法

**1. 再哈希法**

- 准备两套哈希算法，当发生哈希冲突的时候，使用另外一种哈希算法，直到找到空槽为止。对哈希算法的设计要求比较高。

**2. 开放地址法**

- 遇到哈希冲突的时候，就去寻找下一个空的槽。有 3 种方法：
  - 线性探测：从冲突的位置开始，依次往后找，直到找到空槽。
  - 二次探测：从冲突的位置 x 开始，第一次增加 12 个位置，第二次增加 22，直到找到空槽。
  - 双重哈希：和再哈希法类似，准备多个哈希函数，发生冲突的时候，使用另外一个哈希函数。

**3. 拉链法**

- 当发生哈希冲突的时候，使用链表将冲突的元素串起来。HashMap 采用的正是拉链法。



## 哈希表怎么判断两个Key是否相等

依赖于`key`的`equals()`方法和`hashCode()`方法，以及 `==` 运算符。

```java
if (e.hash == hash &&
((k = e.key) == key || (key != null && key.equals(k))))
```

- `e.hash == hash`：先比较哈希值，确保可能是相同的键。

- **`k == key`**：直接使用 `==` 比较**内存地址**。

- **`key.equals(k)`**：如果 `key` 不是 `null`，再调用 `equals()` 方法进行**内容比较**。



## 为什么HashMap链表转红黑树的阈值是8

树化发生在 table **数组的长度大于 64**，且**链表的长度大于 8** 的时候。

红黑树节点的大小大概是普通节点大小的两倍，所以转红黑树，牺牲了空间换时间，更多的是一种兜底的策略，保证极端情况下的查找效率。

**阈值为什么要选 8 呢？**

和统计学有关。理想情况下，使用随机哈希码，链表里的节点符合泊松分布，出现节点个数的概率是递减的，节点个数为 8 的情况，发生概率仅为`0.00000006`。

**红黑树转回链表的阈值为什么是6，而不是8？**

如果这个阈值也设置成 8，假如发生碰撞，节点增减刚好在 8 附近，会发生链表和红黑树的不断转换，导致资源浪费。



## 什么时候扩容，为什么扩容因子是0.75？

HashMap会在存储的键值对数量超过 **容量 $\times$ 扩容因子** 时进行扩容

**0.75是时间复杂度和空间利用率之间的权衡**

- 负载因子越大
  - 空间利用率越高
  - 但冲突概率增大，导致链表/红黑树长度增长，查询效率从O(1)退化 到O(N)
- 负载因子越小
  - 冲突减少，查询更快
  - 但扩容更频繁，内存浪费严重



## 扩容机制了解吗

扩容时，HashMap 会创建一个新的数组，其容量是原数组容量的两倍。然后将键值对放到新计算出的索引位置上。**一部分索引不变，另一部分索引为“原索引+旧容量”**。

```java
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```

在 JDK 8 的新 hash 算法下，数组扩容后的索引位置，要么就是原来的索引位置，要么就是“原索引+原来的容量”，遵循一定的规律。

![三分恶面渣逆袭：扩容节点迁移示意图](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/collection-27.png)

这种规则**仅通过 `hash & oldCapacity` 判断**，不需要重新计算 `hashCode()`，提升效率。



## 扩容的时候每个节点都要进行位运算吗，如果这个HashMap中有几十万条数据，都要进行位运算吗

在 JDK 8 的新 hash 算法下，数组扩容后的索引位置，要么就是原来的索引位置，要么就是“原索引+原来的容量”，遵循一定的规律。

具体来说，就是判断原哈希值的高位中新增的那一位是否为 1，如果是，该元素会被移动到原位置加上旧容量的位置；如果不是，则保持在原位置。

**仅通过 `hash & oldCapacity` 即可判断**

- 当 `hash & oldCapacity == 0`：
  - **新索引 = 旧索引**

- 当 `hash & oldCapacity != 0`：
  - 新索引 = 旧索引 + oldCapacity

所以，尽管有几十万条数据，每个数据项的位置决定仅需要一次简单的位运算。位运算的计算速度非常快，因此，尽管扩容操作涉及遍历整个哈希表并对每个节点进行操作，但这部分操作的计算成本是相对较低的。



## JDK8对HashMap主要做了哪些优化？为什么

**1. 底层数据结构由 数组 + 链表 改成了 数组 + 链表 / 红黑树**

**2. 链表的插入方式由 头插法 改为了 尾插法**

**3. 扩容的时机由 插入时判断 改为了 插入后判断**

- putVal() **先执行插入**，保证数据先存进去，即使发生扩容，也不会影响当前插入操作。

**4. 优化了哈希算法**

- JDK7：多次移位和异或操作

  ![二哥的 Java 进阶之路：JDK 7 的 hash 方法](https://cdn.tobebetterjavaer.com/stutymore/collection-20240512093223.png)

- JDK8：只进行了一次异或操作，但仍能有效地减少冲突。并且能够保证扩容后，元素的新位置要么是原位置，要么是原位置加上旧容量大小。

  ![二哥的 Java 进阶之路：JDK 8 的 hash 方法](https://cdn.tobebetterjavaer.com/stutymore/collection-20240512093327.png)



## 你能自己设计实现一个HashMap吗

略



## HashMap是线程安全的吗？多线程下会有什么问题

HashMap不是线程安全的，主要有以下几个问题：

**1. 多线程下扩容会死循环**

![二哥的 Java 进阶之路](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/collection/hashmap-thread-nosafe-07.png)

- JDK1.7 中的 HashMap 使用的是头插法插入元素，在多线程的环境下，扩容的时候就有可能导致出现环形链表，造成死循环。

**2. 多线程的put可能会导致元素丢失**

- 因为计算出来的位置可能会被其他线程的 put 覆盖。本来哈希冲突是应该用链表的，但多线程时由于没有加锁，相同位置的元素可能就被干掉了。

**3. put和get并发时，可能导致get为null**



## 有什么办法能解决HashMap线程不安全问题呢

**1. Hashtable**

- Hashtable 是 Map 接口的一个早期的同步实现，它的**所有方法都是同步的**，即每个方法都用 synchronized 关键字修饰，以确保线程安全。

**2. Collection.synchronizedMap**

内部是通过 synchronized 对象锁来保证线程安全的。

**3. ConcurrentHashMap**

ConcurrentHashMap 在 JDK 7 中使用了**分段锁**来保证线程安全，在 JDK 8 中使用了**CAS + synchronized** 关键字。



## HashMap内部节点是有序的吗

HashMap 是无序的，根据 hash 值随机插入。如果想使用有序的 Map，可以使用 LinkedHashMap 或者 TreeMap。



## LinkedHashMap怎么实现有序的

LinkedHashMap 维护了 **数组 + 双向链表**，每个Node有头尾节点，同时 LinkedHashMap 节点 Entry 内部除了继承 HashMap 的 Node 属性，还有 before 和 after 用于标识前置节点和后置节点。

![Entry节点](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/collection-33.png)

![LinkedHashMap实现原理](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/collection-34.png)



## TreeMap怎么实现的有序

TreeMap 通过 key 的比较器来决定元素的顺序，如果没有指定比较器，那么 key 必须实现Comparable 接口。

TreeMap 的底层是**红黑树**，红黑树是一种自平衡的二叉查找树，每个节点都大于其左子树中的任何节点，小于其右子节点树种的任何节点。

![三分恶面渣逆袭：TreeMap](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/collection-35.png)

插入或者删除元素时通过旋转和着色来保持树的平衡。



## TreeMap和HashMap的区别

①、HashMap 是基于数组+链表+红黑树实现的，put 元素的时候会先计算 key 的哈希值，然后通过哈希值计算出元素在数组中的存放下标，然后将元素插入到指定的位置，如果发生哈希冲突，会使用链表来解决，如果链表长度大于 8，会转换为红黑树。

②、TreeMap 是基于红黑树实现的，put 元素的时候会先判断根节点是否为空，如果为空，直接插入到根节点，如果不为空，会通过 key 的比较器来判断元素应该插入到左子树还是右子树。

- 在没有发生哈希冲突的情况下，HashMap 的查找效率是 `O(1)`。适用于查找操作比较频繁的场景。

- TreeMap 的查找效率是 `O(logn)`。并且保证了元素的顺序，因此适用于需要大量范围查找或者有序遍历的场景。



# Set

## HashSet的底层实现

HashSet 其实是由 HashMap 实现的，只不过值由一个固定的 Object 对象填充，而键用于操作。

```java
public class HashSet<E>
    extends AbstractSet<E>
    implements Set<E>, Cloneable, java.io.Serializable
{
    static final long serialVersionUID = -5024744406713321676L;
    private transient HashMap<E,Object> map;
    // Dummy value to associate with an Object in the backing Map
    private static final Object PRESENT = new Object();
    // ……
}
```

