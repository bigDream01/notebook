

**JOSN的定义**

> **JavaScript Object Notation，本质上即是JS对象**

![image-20210111154953237](C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20210111154953237.png)

**JOSN的常用方法**

> **主要是常用JSON.Stringify(josn):   将json对象转化为json字符串，便于将浏览器端的json对象信息传输给服务器**
>
> **JSON.parse(jsonString)  : 将json字符串转化为json对象，好在浏览器端使用。**

![image-20210111155050153](C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20210111155050153.png)

**json转化为JavaBean**

> json与对象之间的转化
>
> > ```java
> > //对象转换为json
> > String toJsonString = gson.toJson(person1);
> > System.out.println(toJsonString);
> > 
> > //json转化为对象
> > person person = gson.fromJson(toJsonString, person1.getClass());
> > System.out.println(person);
> > ```
>
> json与集合之间的转化
>
> > ```java
> > String listJsonString = gson.toJson(list);
> > 
> > //把json转化为list，匿名内部类的调用
> > list = gson.fromJson(listJsonString, new TypeToken<List<person>>() {
> > }.getType());
> > ```
>
> json与map之间的转化（与list类似）
>
> > ```java
> > String toJson = gson.toJson(map);
> > System.out.println(toJson);
> > 
> > //json转化为map
> > Map<Integer,person> map1 = gson.fromJson(toJson, new TypeToken<HashMap<Integer, person>>() {
> > }.getType());
> > ```

