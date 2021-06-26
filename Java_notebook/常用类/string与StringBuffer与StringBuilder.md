String：不可变字符序列，底层使用char[ ]存储

StringBuffer：可变字符序列，线程安全，效率低(比String高)，底层使用char[ ] 存储

StringBuider：可变字符序列，线程不安全，jdk5.0新增，效率高，底层使用char[ ] 存储



注意点：

①底层char[ ]数组的默认长度为16；

②其长度不是按照数组长度来返回的

扩容问题： 

> 如果添加的底层数组盛不下，默认情况下，扩容为原来容量的2倍 + 2，并复制原来数组的元素到新数组中

```java
StringBuffer stringBuffer = new StringBuffer("hadfj");

System.out.println(stringBuffer.length());//5
```

开发中推荐：

> StringBuffer与StringBuider，因为其可以自定义char [ ]数组长度

方法：

> 增：append（）或“+”（不建议）
>
> 删：delete（int start，int end）
>
> 改：setCharAt(int n，char ch)
>
> 查：charAt(int n)
>
> 插：insert(int offset, xxxx)
>
> 长度：length()