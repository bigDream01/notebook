![image-20200831232243273](C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20200831232243273.png)

![image-20200831232252644](C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20200831232252644.png)

![image-20200831232301202](C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20200831232301202.png)

```java
package Learn;

/*
 *
 *
 *
 *@description:
 *@author：bigDream
 *@date：2020-08-31 22:41
 **/
public class StringTest {

    public static void main(String[] args) {

        String s1 = "good";
        String s2 = "nice";
        String s3 = "oo";

        System.out.println(s1.length());
        System.out.println(s1.charAt(2));
        System.out.println(s1.isEmpty());
        System.out.println(s1.toUpperCase());
        System.out.println(s2.compareTo(s1));
        System.out.println(s1.substring(1,2));

        System.out.println(s1.endsWith("d"));

        //测试此字符串从指定索引开始的子字符串是否以指定前缀开始
        System.out.println(s1.startsWith("o",2));

        //判断s3字符串是否被包含于s1字符串当中
        System.out.println(s1.contains(s3));

        //返回该字符在指定字符串第一次出现的位置
        System.out.println(s1.indexOf("o"));

        //返回指定子字符串在此字符串中第一次出现处的索引，从指定的索引开始.返回-1表示没找到
        System.out.println(s1.indexOf("o",2));
        
        


    }


}
```