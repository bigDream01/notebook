## HashSet：

> 存储无序的，不可重复的，可以存储null，元素要重写HashCode（）和equals（）

**存储方式：**数组 + 链表 存储

**存储过程：**

> ①在HashSet这个类的存储元素a中，会先对元素a进行HashCode（）进行元素编码，得到hash值
>
> ②然后会对哈希值进行，通过某种散列函数决定该对象在 HashSet 底层数组中的存储位置，如果该位置没有元素，那么便存储到该位置
>
> > 如果该位置存储了元素，会对存储到该位置的元素与存储的元素a的hash值进行对比，不一样即进行存储
> >
> > > 如果hash值也相等那么便会调用equals（）。如果返回true那么存储失败
> > >
> > > 如果返回false，即以链表的方式存储（单向链表），方式为“七上八下”（对象为插入元素）

![image-20200914115604008](C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20200914115604008.png)

## LinkedHashSet

**特点：**

> ①HashSet的子类，**双向链表**维护元素的次序，这使得元素看起来是以插入顺序保存的
>
> ②LinkedHashSet插入性能略低于 HashSet，但在迭代访问 Set 里的全部元素时有很好的性能。

**要求：**

> ①必须要重写HashCode（）和equals（）方法要进行重写
>
> ②重写的HashCode（）和equals（）要保持一致性，让Hash值能够保持一致性

**存储与HashSet存储一致**。

## TreeSet

**特点**

>  按照对象的指定属性排序

**底层实现：**

> 主要是红黑树
>
> ![image-20200914161805449](C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20200914161805449.png)

要求

> ①插入元素必须同一个类
>
> ②插入对象的类中必须要重写comparaTo（）或compator方法（）

排序原理：

> 其排序原理主要是参照comparaTo（）和compator（）方法进行排序

添加元素原理：

> 在TreeSet这个类中，添加元素时判断其是否相同主要是依据comparaTo()和compator（）来进行判断，如果comparaTo()和compator（）判断返回0。那么元素便会添加失败。

