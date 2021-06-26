**运行时对象的实例化**

```java
Class<Person> clazz = Person.class;
//创建clazz实例
Person p1 = clazz.newInstance();

//方式二
Person p = new Person();
Class Clazz1 = p.getClass();

//方式三
Class<?> Clazz2 = Class.forName("ReflectionTest.Person");

//加载到内存中会缓存一段时间。可以通过方法获取运行时类

//主要方式
//在设置访问文件时，不要多写也不能少写。在main方法下其对应的是这个module下，而非工程下。
```

**文件配置Properties**（运行时类的插入讲解）

```java
Properties properties = new Properties();
//方式一
FileInputStream fis = new FileInputStream("20_9_26\\src\\star.properties");//识别为当前工程下
properties.load(fis);

String user = properties.getProperty("user");
String passworld = properties.getProperty("passworld");

System.out.println("user= " + user + ",passworld= " + passworld);
```