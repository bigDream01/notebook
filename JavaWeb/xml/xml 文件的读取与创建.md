### xml 文件的创建

```xml
<?xml version="1.0" encoding="utf-8" ?>


<!--
        version 表示xml的版本
        encoding 表示字符集
-->

<books>
    <book sn = "12212156150">
        <name>helloword</name>
        <price>13</price>
        <author>code</author>
    </book>

    <book sn = "12212156152">
        <name>python</name>
        <price>14</price>
        <author>coder</author>
    </book>
</books>
```



### 读取xml文件

```java
SAXReader reader = new SAXReader();
Document document = reader.read("src/dom4jTest/books.xml");
//因为是在test中使用的所以是module下为起始路径

//获取根元素
Element books = document.getRootElement();

//获取到了book标签的集合
List<Element> book = books.elements("book");

//遍历集合
for(Element book1 : book){
    //这个时候获取的是标签对象
    Element name = book1.element("name");
    Element author = book1.element("author");
    Element price = book1.element("price");
    //获取标签里面的文本内容
    String nameText = name.getText();
    String authorText = author.getText();
    String priceText = price.getText();


    //获取属性值实使用attributeValue（）其直接返回的是string类型的对象
    String snValue = book1.attributeValue("sn");

    System.out.println(new Books(snValue,nameText,Double.parseDouble(priceText),authorText));

}
```