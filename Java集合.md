## Map：

HashMap：

HashMap是最常用的Map集合，它是线程不安全的。查询速度快。HashMap的Key最多允许一条数据为null，value允许多条为null。默认大小为16，加载因子为0.75f。查询速度为O(logn)

TreeMap：

和HashMap类似，但是它能够根据键值进行排序，默认是按键值的升序排序（自然顺序），在创建TreeMap对象时可以指定比较器来改变排序方式，当用Iterator遍历TreeMap时，得到的记录是排过序的。不允许key值为空，它是线程不安全的。

HashTable：

和HashMap类似，但它底层使用了sync关键字，所以是线程安全的。另外他的键值都不允许为null。因为需要保证多线程的情况下的同步访问，所以在执行效率上没有HashMap高。

ConcurrentHashMap：

首先ConcurrentHashMap是线程安全的，它结合了HashMap和HashTable两者的优势，即是线程安全的，查询速度也不会因为为了保持同步状态而牺牲太大。本质上就是依靠HashTable线程安全的特性，将该集合中的部分数据使用HashTable来存储。这样当不同的线程访问到不同的分片时并不会对数据安全造成影响。同时访问到同一个分片时则后来的线程会去等待另外的一个线程去释放资源后再去操作该分片内的数据

## Set：

HashSet：

HashSet底层实现是使用HashMap的Key来实现的，所以它是线程不安全的、也是不可重复以及无序的。它的初始容量和加载因子与HashMap相同分别都是16和0.75f

TreeSet：

TreeSet和HashSet不同，首先它是有序的。它的底层是使用TreeMap的Key来实现的，所以他也是线程不安全以及不可重复的。

## List：

ArrayList：

ArrayList的实现原理是可变数组。它是线程不安全的。它的初始容量为：10；每次扩充当前数组大小的一半+1。查询速度为O(n)

Vector：

和ArrayList的实现原理一样都是可变数组。但它底层的一些方法都使用了sync关键字。所以他是线程安全的。它的初始容量为10，每次扩充当前数组大小的一倍

LinkList：

他与ArrayList不同，它的底层使用的是双向链表来实现的，也是线程不安全的。在初始化时没有初始容量和加载因子的概念。因为调用Add方法时会直接在链表的最后增加一个node来关联。





## Q：常见的集合有哪些？

回答两方面：

1. 集合的接口有两种分别是Map接口和Collection接口，Collection的子接口有Set和List两种。

2. Map接口的实现类有：HashMap、TreeMap、HashTable、CurrentHashMap、Properties等。

   Set接口实现类有：HashSet、TreeSet、LinkedHashSet等。

   List接口实现类有：ArrayList、LinkedList、Stack以及Vectory等。

## Q：ArrayList和LinkedList有什么不同？

1. 实现原理不同，ArrayList本质上是数组，而LinkedList本质上是双向链表。
2. 进行遍历的话ArrayList相比LinkedList更有优势。因为数组在内存中是连续的。而使用LinkedList进行遍历的需要额外的寻址操作。
3. 应用场景不同，对于ArrayList来说更适合增删少便利多的场景。LinkedList更适合增删多，遍历少的场景。

## Q：ArrayList和Vector有什么不同？

1. ArrayList是线程不安全的，Vector是线程安全的。
2. 扩充方式不同。ArrayList中，当存储的容量到达原始容量的一半时进行数组扩容，扩容到原数组长度+原数组长度的一半+1。而Vector则是当数据容量满时进行数组扩容，扩容到原数组长度的两倍。

## Q： Array和ArrayList有什么不同？

1. Array可以存放基本数据类型和对象数据类型，而ArrayList只能存放对象数据类型。
2. Array的长度是固定的，ArrayList的长度是可变的。

## Q：HashMap是怎么解决哈希冲突的？

拆解成3个问题分别是：什么是哈希？什么是哈希冲突？怎么解决？

1. 哈希简单的说就是一段数据/信息的摘要。所有的哈希函数都有一个定律：如果通过哈希函数计算后得到的结果一致，那么输入值不一定相同。如果通过哈希函数计算后得到的结果不一致，那么输入值一定不同。

2. 不同的输入值根据同一个哈希函数得出相同的哈希值的现象，我们把这种现象叫做哈希冲突/哈希碰撞

3. HashMap的数据结构是由数组+链表的形式构成的。这种数据结构可以融合两种数据结构的优点。在这种数据结构的基础上使用链地址法可以减少哈希碰撞。但是HashMap的初始容量为2的4次方，也就是16，要远远小于int类型的范围。所以我们如果单纯的使用hashCode取余数的话会大大的增加哈希碰撞的概率，并且最坏的情况下HashMap会变成一个单链表。

   所以hashCode方法需要进行优化，不能简简单单的取余。而是要让高位也参与运算。这样可以大大的减少哈希碰撞的概率。这种操作成为扰动。
   在Jdk7种，hashCode方法使用了4次位运算，5次与或运算。而在jdk8种使用了1次位运算1次与或运算。因为链表的存在，那么HashMap的查询效率变成了O(n)。所以在Java8种HashMap引入了红黑树的数据结构。当链表长度大于8时，HashMap中对应的链表会切换为红黑树来存储数据。这样HashMap的查询效率变成了O(logn)。

4. 总的来说解决hash冲突HashMap使用了两种方法1.使用链地址法来链接相同Hash值的数据。2.使用2次扰动来减少碰撞概率。3.使用红黑树来增加查询效率。