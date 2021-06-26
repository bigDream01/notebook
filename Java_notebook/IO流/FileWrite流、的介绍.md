```java
public static void main(String[] args) throws IOException {

    //①实例化对象
    File file = new File("hello1.txt");

    //②设置端点
    FileWriter fileWriter = new FileWriter(file);

    //③写入操作
    fileWriter.write("haer \n");
    fileWriter.write("erha");

    //④关闭流
    fileWriter.close();

}
```

**注意点：**

> 情况一：
>
> 在此目录下没有此文件的话，构造器会直接创建文件
>
> 情况二：
>
> 在此目录下有此文件
>
> > FileWriter（file   false）/ FileWriter（file）的话会覆盖原有数据
> >
> > FileWriter（file   true）会在原有数据上继续写入数据