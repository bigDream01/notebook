**特点:**

> list中元素有序，且可重复,collection接口的子接口-------**“动态”数组**

```java
    ArrayList arrayList = new ArrayList();

    Collection collection = new ArrayList();

    collection.add("哈儿");
    collection.add("hello");

    arrayList.add(122);
    arrayList.add(235);
    arrayList.add("哈儿");
    arrayList.add(new Person("哈儿",20));




    //int indexOf(Object obj):返回obj在集合中首次出现的位置，没有返回-1
    int index = arrayList.indexOf("猪");
    System.out.println(index);




    // List subList(int fromIndex, int toIndex):返回从fromIndex到toIndex
    List list = arrayList.subList(0, 3);
    System.out.println(list);



```





常用方法总结：

增：add（），addAll()

```java
//void add(int index, Object ele):在index位置插入ele元素
arrayList.add(0,"猪");

//boolean addAll(int index, Collection eles):从index位置开始将eles中的所有元素添加进来
arrayList.addAll(1,collection);
System.out.println(arrayList.size());
System.out.println(arrayList);
```
删：remove(int index) , removeAll()

```java
//Object remove(int index):移除指定index位置的元素，并返回此元素
arrayList.remove(3);
```
改：set (int index, Object ele )

```java
//Object set(int index, Object ele):设置指定index位置的元素为ele
arrayList.set(2,"花");
```
查：get( )

```java
//Object get(int index):获取指定index位置的元素,没有返回-1
Object o = arrayList.get(0);
System.out.println(o);
```
插：add(int Index)

长度：size()

遍历：

```java
//遍历
ListIterator listIterator = arrayList.listIterator();
while (listIterator.hasNext()){
    System.out.println(listIterator.next());
}
```

**数组 --->集合**

```java
//拓展：数组 --->集合:调用Arrays类的静态方法asList(T ... t)
List<String> list = Arrays.asList(new String[]{"AA", "BB", "CC"});
System.out.println(list);

List arr1 = Arrays.asList(new int[]{123, 456});
System.out.println(arr1.size());//1

List arr2 = Arrays.asList(new Integer[]{123, 456});
System.out.println(arr2.size());//2
```

