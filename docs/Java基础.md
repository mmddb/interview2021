## 数据类型

数组：随机访问，查找快，增删慢，**可以利用缓存的优势**

链表：顺序访问，查找 O（n），增删快

其他：

线性：数组，链表，栈，队列

非线形：多维数组，树（二叉树，多叉树，堆，AVL 树，红黑树，B 树，B+ 树），图

#### 跳表

原理：可以**实现二分查找的有序链表**，通过建立 **多级索引**

插入过程：确定该元素要建立 N 级索引，然后就插入到 N以及N以下等级索引中，最坏时间复杂度 为 logN, 也就是索引层数。。



###  排序算法时间复杂度，空间复杂度

**冒泡，插入，选择**： **平均和最坏都是 n^2**；**选择 最好是 n^2**，**其他俩最好 都是 n**

**快排**：**最好，平均 都是 nlogn**，**最坏是 n^2**；

**堆排，归并：平均，最好，最坏都是 nlogn**；

**归并**是外部排序，并且其空间复杂度是 **O（n）**，**其他都是原地排序，O（1）**

插入：适用于 一部分已经排好了，

###  不稳定的排序

**选择排序**，**快排**，**堆排**都不稳定



### 图论

遍历方式： DFS，BFS

表示方式：**邻接表，邻接矩阵**

最短遍历算法



## 多态

对某个类型的方法调用，真正执行的方法 取决于 **运行时期的 *实际类型*** 的方法。

好处是，可以方便的对子类进行扩展; **允许添加更多类型的子类实现功能扩展**，却**不需要修改基于父类的代码**。

继承，重写，**父类引用**指向**子类实例** Person p = new Kid();



## 重载与重写

**重载**: 方法签名（名字和参数）不一样就行

**重写：** 子类重写父类方法；签名一样，返回值类型小于等于，抛出异常小于等于，访问权限大于等于；（父类方法不 private）

**方法调用**：名称一样，优先找参数类型一致的，优先本地，不行再找父类的
this.fuc(this) -> super.func(this) -> this.func(super) -> super.func(super);



## 抽象类和接口

#### 抽象类

和普通类（成员变量，方法，构造器，实现的方法）相似，只是不能实例化；内部需要包含抽象方法（只有方法声明，没有方法体）

由 abstract 修饰，其抽象**方法修饰符**为 public，protected， default; 变量可以用 private

存在的目的就是被继承，子类需要 **实现所有抽象方法**。

#### 接口

接口是一个 **抽象集合**，不是类，里面只包含抽象方法；

默认修饰： 只能有成员变量 **public static final** ； 方法只能有 ； **public abstract**

java 8 方法可以有 **默认实现**。8 之前不能有任何方法实现

但是在 java 9 中，可以在接口中加入 private 方法。

#### 比较：

is-a 和 like-a； 一个需要子类完全代替父类对象，一个提供一种方法实现契约

**单继承和多实现**；抽象类成员有多种 **访问权限**



## 类

### 初始化顺序

 **静态**变量，**静态**语句块  ->  **实例**变量，**普通**语句块 -> **构造器**
**有继承**的话：**父类静态**两兄弟 -> **子类静态**两兄弟-> **父类实例**两兄弟 -> **父类构造函数** -> **子类实例**两兄弟 -> 子类构造函数

父类先于子类，静态先于普通； 相同级别取决于 先后顺序

### 内部类：

内部类可以无条件访问外部类 **所有** **成员属性和方法**。

**匿名内部类限制**： 不能有 constructor（）



## 集合

![截屏2021-10-04 22.01.17](/Users/jon/Library/Application Support/typora-user-images/截屏2021-10-04 22.01.17.png)										



### ArrayList和LinkedList区别

数组，双向链表

随机访问，顺序访问

插入和删除，LinkedList 不需要 **扩容**，**移动其余元素**

LinkedList 更占内存，要存两个引用 ( 32位 64位都是 4字节， 堆内存大于 32GB时位 8 字节 )

### ArrayList扩容



### HashMap八股（数据结构，扩容，[链表]()和[红黑树]()转换）





### HashMap 和 HashSet 

当哈希函数使得元素分布均匀时，增加，移除，包含方法 时间复杂度可以达到常数级别。

hashMap 允许 key 和 value 为 null，基本等同于 hashtable，除了 unsynchronised and permit NULL。不保证内部顺序，不保证顺序不变；

遍历 hashmap 取决于 bucket 的数量，以及 buckets 的 大小，因此 **初始容量**，和 **装填因子**（0.75默认），

fast-fail 机制，如果 iterator 创建后，hashmap 出现 **结构性变化**（增加或删除），除非 iterator.remove() , iterator 会 throw `ConcurrentModificationException`

#### put过程

1. 计算hash值，获取到该位置 bin 的第一个元素
2. 是 null 的话就直接插入
3. 否则，判断是否 key 一样，hash一样，看是不是更新操作； 再看是不是 treenode 的instance，是的话就插入到红黑树中；如果不是，那就还是普通 node，就找到最后一个 node，尾插，过程中记录 bincount，超过 threhold 就 treeifyBin()，转为红黑树，除非 table size 小于 MIN_TREEIFY_CAPACITY 64 默认
4. 如果是发现是更新操作，就更改并返回旧值
5. modcount++, 由于是结构化更改；size++，有需要的话 resize
6. return null

```java
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
               boolean evict) {
    Node<K,V>[] tab; Node<K,V> p; int n, i;
    if ((tab = table) == null || (n = tab.length) == 0) // 长度为 0 的话 resize
        n = (tab = resize()).length;
  	// 得到应该插入点现存的 Node p
    if ((p = tab[i = (n - 1) & hash]) == null) // (n - 1) & hash = n % hash; length = 2^n
        tab[i] = newNode(hash, key, value, null); // 该插入的地方没有，就生成一个放进去
    else {
        Node<K,V> e; K k;
        if (p.hash == hash &&
            ((k = p.key) == key || (key != null && key.equals(k)))) // 不仅 hash相等，key 也一样 （更新操作）
            e = p;
        else if (p instanceof TreeNode) // 如果 p 是一个 treenode，也就是说已经变成红黑树了
            e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value); // 插入红黑树
        else {
            for (int binCount = 0; ; ++binCount) { // 找到链表最后一个，然后插入
                if ((e = p.next) == null) {
                    p.next = newNode(hash, key, value, null);
                    if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                        treeifyBin(tab, hash); // 长度大于 8 转换为红黑树
                    break;
                }
                if (e.hash == hash &&
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    break;
                p = e;
            }
        }
        if (e != null) { // existing mapping for key
            V oldValue = e.value;
            if (!onlyIfAbsent || oldValue == null)
                e.value = value;
            afterNodeAccess(e);   // 留一个未实现的callback函数
            return oldValue; // 返回旧值，
        }
    }
    ++modCount;  // modCound 增加
    if (++size > threshold) // size 大于 threshold = length * load factor
        resize();
    afterNodeInsertion(evict);// 留一个未实现的callback函数
    return null;
}
```

借用美团技术团队的图

![截屏2021-10-05 10.40.04](/Users/jon/Library/Application Support/typora-user-images/截屏2021-10-05 10.40.04.png)

### HashSet 

内部实现较为简单，充分利用了 HashMap， 内部维护一个 `HashMap<E,Object> map`; **只用到了 map 的 key**，**所有的 value 都是** 一个 叫做 **Present** 的 **Object** 。 

常用方法，add ->  map.put(e, PRESENT) == null ;  remove() --> map.remove(o) == PRESENT; contains(O) --> map.containsKey(o);



HashTable ConcurrentHashMap





### 线程安全

HashTable，ConcurrentHashMap，

CopyOnWriteArrayList（写时复制，修改时在复制的数组上进行，最终一致性，）





## 异常

try catch finally

可以没有 finally; 多个 catch 连接要小的**子类在上**；

**catch 中 return 值会先被存起来**，如果finally 里面也有 return的话就会被覆盖; 在 catch 中加入 **System.exit(**int status number) 就不会进入 finally了。





## 双层循环嵌套

多的在里面，**减少 CPU 切入内层循环的次数**



short 最大值 **32767**

接口默认修饰法： 成员变量 **public static final**, 方法 ： **public abstract**

**重载**: 方法签名（名字和参数）不一样就行

**重写：** 签名一样，返回值类型小于等于，抛出异常小于等于，访问权限大于等于；（父类方法不 private） 



### 内部类：

内部类可以无条件访问外部类 **所有** **成员属性和方法**。

**匿名内部类限制**： 不能有 constructor（）

perterson算法 ： 解决临界区问题， 满足互斥，有限等待





**MVC**: Web APP 的模式； 3 个东西分别：负责数据库的存取，处理用户交互，业务逻辑；数据显示

好处：耦合性低（维护性高），面显示分离；





### **synchronized 原理**

synchronized的语义底层是通过一个 **monitor的对象** 来完成，其实 **wait/notify等方法也依赖于monitor对象**，这就是为什么**只有在同步的块或者方法中才能调用wait/notify**等方法，否则会抛出 java.lang.**IllegalMonitorStateException**的异常的原因。



ArrayList , 内部是 **object数组，因此只能存储 引用类型，不能存基础类型；动态扩容，会先创建一个两倍大小的数组，copy过去后扔掉旧数组，如果频繁扩容的话，效率会比较低。

