```java
//添加元素add(object e）。注意 ：因为其为有序的那么其添加顺序不一样，那么两个对象是不一样的（ArrayList（））
collection.add(22);//自动装箱
collection.add("哈儿");
collection.add("hello");
collection.add(33);
collection.add(44);

collection1.add("哈儿");
collection1.add("hello");

//size(),返回元素个数
System.out.println(collection.size());

//contaions():判断在当前对象中是否存在此元素contains（元素）其比较是运用equals()要对方法进行重写，
// 不然其调用的是object中的equals()，会返回false
System.out.println(collection.contains(new String("hello")));

//containsAll():判断对象中的所有元素是否都存在于此对象中
      System.out.println(collection.containsAll(collection1));

System.out.println("*********");
//remove():移除元素
collection.remove("hello");
System.out.println(collection);

//removeAll(collection1 o):移除此对象中与o中相同的元素
collection.removeAll(collection1);
System.out.println(collection);

//retainAll(collection1 o):得到o对象与此对象共同持有的元素,修改的当前对象的元素，即元素变为相同的元素

Collection collection2 = new ArrayList(22);
collection.retainAll(collection2);
System.out.println(collection);
```



**遍历：**

```java
Iterator iterator = collection.iterator();

while(iterator.hasNext()){

    System.out.println(iterator.next());

}
for(Object obj : collection){
    System.out.println(obj);
}
```