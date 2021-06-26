**复制操作：**

```java
public class CopyTest {
    public static void main(String[] args) {

        FileInputStream fis = null;
        FileOutputStream fos = null;
        try {
            File srcFile = new File("20_9_22\\夜晚+两个女孩++唯美背影+蝴蝶+好看的动漫人物4k高清壁纸_彼岸图网.jpg");
            File desFile = new File("20_9_22\\夜晚+两个女孩++唯美背影+蝴蝶+好看的动漫人物4k高清壁纸_彼岸图网1.jpg");

            fis = new FileInputStream(srcFile);
            fos = new FileOutputStream(desFile);

            int len;
            byte[] buffer = new byte[5];

            while ((len = fis.read(buffer)) != -1) {

                fos.write(buffer, 0, len);

            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (fos != null) {
                try {
                    fos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (fis != null) {
                try {
                    fis.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }


            }
        }


    }
}
```



**注意：**

> FileInputStream不可以运用于字符的传递，会产生乱码



