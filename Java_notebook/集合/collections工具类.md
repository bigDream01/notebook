### collections工具类：

**排序操作：**

> reverse(List)：反转 List 中元素的顺序
>
> shuffle(List)：对 List 集合元素进行随机排序
>
> sort(List)：根据元素的自然顺序对指定 List 集合元素按升序排序
>
> sort(List，Comparator)：根据指定的 Comparator 产生的顺序对 List 集合元素进行排序
>
> swap(List，int， int)：将指定 list 集合中的 i 处元素和 j 处元素进行交换

```java
List list = new ArrayList();


list.add(113515);
list.add(521231);
list.add(1322);
list.add(12151);

//void copy(List dest,List src)：将src中的内容复制到dest中
List dest = Arrays.asList(new Object[list.size()]);
Collections.copy(dest,list);
System.out.println(dest);

//随机排序
Collections.shuffle(list);
System.out.println(list);

Collections.sort(list);
```

**查找、替换**

> Object max(Collection)：根据元素的自然顺序，返回给定集合中的最大元素
>
> Object max(Collection，Comparator)：根据 Comparator 指定的顺序，返回给定集合中的最大元素
>
> Object min(Collection)
>
> Object min(Collection，Comparator)
>
> int frequency(Collection，Object)：返回指定集合中指定元素的出现次数
>
> void copy(List dest,List src)：将src中的内容复制到dest中 
>
> boolean replaceAll(List list，Object oldVal，Object newVal)：使用新值替换List 对象的所有旧值

**Collections常用方法：同步控制**

>  Collections 类中提供了多个 synchronizedXxx() 方法，该方法可使将指定集合包装成线程同步的集合，从而可以解决多线程并发访问集合时的线程安全问题

说明：ArrayList和HashMap为线程不安全的需要对其进行代码同步。