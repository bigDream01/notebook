### **Map存储建构**

**key** 

> 在存储中key是无序的，不可重复的。运用set接口进行存储，---------------如果存储的key是个自定义类，那么必须要重写equals（）和hashcode方法（hashmap中）

**value**

> 在存储的value是无序的，可重复的。运用collection接口进行存储-----------------如果value是个自定义类，那么必须要重写equals方法。

一对键值构成了entry对象，其添加元素时，是添加entry对象

在map中对entry存储主要是运用set存储，其实无序的，且不可重复的