

String：字符串，使用一对“”引起来表示

①String声明为final的，不可以被继承

②String实现了Serialiable接口：表示字符串是支持序列化的

​             实现了Comparable接口：表示String可以比较大小

③String内部定义了final char[]  value 用于储存字符串数据

④String为不可变的字符序列。

⑤通过字面量的方式（区别于new）给一个字符串赋值，此时字符串声明在常量池中

⑥字符串常量池中不可以声明相同内容的文字

### 字符串常量储存与对象的区别

![image-20200831000316945](C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20200831000316945.png)

### 字符串对象是如何储存的

![image-20200831000400733](C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20200831000400733.png)

![image-20200831000531056](C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20200831000531056.png)