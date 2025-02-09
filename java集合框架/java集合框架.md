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


    

  - **RL型**

    ![image-20250209124532887](https://github.com/user-attachments/assets/bb34286c-4b43-4cfa-859e-1840e518d33b)


    ![image-20250209124548063](https://github.com/user-attachments/assets/81fc7cea-8af7-4ddd-a494-3cf4ec6feece)


    ![image-20250209124602507](https://github.com/user-attachments/assets/22ddcac8-1dd0-4692-86a7-010ac19983ab)


    ![image-20250209124613327](https://github.com/user-attachments/assets/e3d2c5d7-e5ea-4555-9f2b-7c4b55a3a3b1)


    



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
