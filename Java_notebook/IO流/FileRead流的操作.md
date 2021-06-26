**处理对象：文本文件处理**





## FileRead流的操作演示

```java
public static void main(String[] args) {
    FileReader fr = null;
    try {
        //1.实例化File类的对象
        File file = new File("20_9_21\\helloworld.txt");

        //2.提供具体的流
        fr = new FileReader(file);

        //3.数据的读入
        int date = fr.read();//返回一个字符，如果达到文件末尾返回-1.（assic码存储）
        while (date != -1) {
            char s = (char) date;
            System.out.print(s);
            date = fr.read();
        }
    } catch (FileNotFoundException e) {
        e.printStackTrace();
    } catch (IOException e) {
        e.printStackTrace();
    } finally {

        try {
            if (fr != null) {//防止未创建对象时，关闭流（报异常）
                fr.close();//流的关闭
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

}
```

## 优化版：

> 主要是对每次提取的字符进行了优化，能去除更多的字符。

```java
public static void main(String[] args) {
    FileReader fr = null;
    try {
        //对象化文件
        File file = new File("20_9_21\\helloworld.txt");

        //进行流选择和对象化操作
        fr = new FileReader(file);

        //数据的读入
        char[] a = new char[5];
        int length;
        while ((length = fr.read(a)) != -1) {
           
            for (int i = 0; i < length; i++) { //注意：这里的数组是对之前数组的覆盖操作，所以迭代条件变为了length。

                String s = new String(a);
                System.out.print(s);

                length = fr.read(a);
            }

        }
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        try {
            if (fr != null)
                fr.close();//流的关闭
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    
}
```