



String --------->  char[]: 调用string的tocharArray（）



char[] -----------> string : 调用String的构造器

```java
String str = "hello";

char[] str1 = str.toCharArray();

String s = new String(str1);
System.out.println(s + "*********");
for (int i = 0; i < str1.length; i++) {
    System.out.println(str1[i]);
}
```