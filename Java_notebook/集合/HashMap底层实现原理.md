**HashMap的底层实现原理**



> HashMap hashmap  =  new HashMap （）：此时会在底层创建一个长度为16 的entry类型的数组table
>
> -------此时已经添加了数据--------
>
> hashmap.put(key1 , value1)
>
> > 此时会调用key的hashcode()方法对key1进行hashcode算法得到一个hash值，然后通过某种算法，计算出其在table数组里面的位置，如果位置为空，即添加其元素
> >
> > > 如果该位置已经存在了数据，那么会对比存在该位置的数据的key的hash值进行比较，如果不相同那么便在该位置上以链表的形式插入数据
> > >
> > > > 如果该数据的hash值与该位置上的某一个元素（key2  value2）的hash值相同，那么便调用key1的equals（）与该元素key2
> > > >
> > > > > 如果返回false： 该链表后插入该元素
> > > > >
> > > > > 如果返回true：将hash值相同的元素的value2  替换为  value1

![image-20200916141615984](C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20200916141615984.png)

**扩容问题：**

>  ①当超出临界值且在下一个位置不为空时，数组扩容为原来的两倍
>
>  ②为了防止一个位置上的链表活红黑树过长，所以设置了影响因子来对数组进行扩容

**jdk8 与 jdk7 在底层的不同：**

> **1**.new HashMap（）在底层没有创建长度16的底层数组（）
>
> **2**.jdk8 底层是Node（）而非entry[ ]
>
> **3.**首次调用put（）时才会创建长度为16的数组
>
> **4**.jdk7 底层存储是：数组 + 链表 
>
>    jdk8： 底层存储原理是： 数组 + 链表 + 红黑树
>
> **5.**在HashMap底层上，在这个位置上添加元素，如果这一个位置的链表长度 >  8, 且长度大于64的时候。此位置才采用红黑树进行存储。如果没有达到以上条件，会对数组进行扩容来进行数据的存储。



**DEFAULT_INITIAL_CAPACITY :** HashMap的默认容量，16

**MAXIMUM_CAPACITY** **：** HashMap的最大支持容量，2^30

**DEFAULT_LOAD_FACTOR**：HashMap的默认加载因子

**TREEIFY_THRESHOLD**Bucket中链表长度大于该默认值，转化为红黑树

**UNTREEIFY_THRESHOLD***：Bucket中红黑树存储的Node小于该默认值，转化为链表

**MIN_TREEIFY_CAPACITY**：桶中的Node被树化时最小的hash表容量。（当桶中Node的数量大到需要变红黑树时，若hash表容量小于MIN_TREEIFY_CAPACITY时，此时应执行resize扩容操作这个MIN_TREEIFY_CAPACITY的值至少是TREEIFY_THRESHOLD的4倍。）

