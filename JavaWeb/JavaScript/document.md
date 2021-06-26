**定义**

> document主要就是将文本数据变为一个对象，通过对象来对文本数据进行操作

### **getElementById**

> > 主要是通过标签里的id属性获取该位置的对象，然后通过对象，对文本数据操作
>
> **案例**
>
> ```html
> <head>
>     <meta charset="UTF-8">
>     <title>Title</title>
> 
> 
>     <script type="text/javascript">
>         //当用户点击后验证用户输入是否正确
>         function onclickfun1() {
>             //1.获得这个标签对象
>             var username = document.getElementById("username");
>             var usernamespan = document.getElementById("usernamespan");
>             
>             // alert(username);
> 
>             //2.获取输入框内的内容
>             //.value是获取框内内容
>             var v = username.value;
> 
>             //.innerhtml 是获取样式
>             // alert(v);
> 
>             //正则表达式验证
>             var patr   = /^\w(5,12)$/;
>             
> 
>             //验证是否合乎正则表达式的条件
>             if(patr.test(v)){
>                 usernamespan.innerHTML = "用户合法";
>                 // usernamespan.innerHTML = "<img src='../imgs/2.jpg' width = '15' height='15'>";
>             }else {
>                 usernamespan.innerHTML = "用户输入不合法";
>                 // usernamespan.innerHTML = "<img src='../imgs/2.jpg' width = '15' height='15'>";
>             }
> 
>         }
> 
>     </script>
> 
> 
> </head>
>     <body>
>         用户名：<input type="text" id="username" value="ahfjg"></input>
>             <span id="usernamespan" style="color: red"></span>
>             <button onclick="onclickfun1()">校验</button>
> </body>
> ```

### getElementByName

> > 通过name属性来获取对象，这个可以一次获取多个对象，他会返回一个集合，但是对集合的操作是数组操作
>
> **案例**
>
> ```html
> <head>
>     <meta charset="UTF-8">
>     <title>Title</title>
> 
>         <script type="text/javascript">
> 
>             //按照name属性获得一个集合，进行遍历集合可以对其修改数据（数组的遍历）
> 
>             function checkAll() {
> 
>                 var hobies = document.getElementsByName("hoby");
> 
>                 for (var i = 0; i < hobies.length; i++){
>                     hobies[i].checked = true;
>                 }
>             }
> 
>             function checkNone() {
>                 var hobies = document.getElementsByName("hoby");
> 
>                 for (var i = 0; i < hobies.length; i++){
>                     hobies[i].checked = false;
>                 }
>             }
> 
>             function checkReverse() {
>                 var hobies = document.getElementsByName("hoby");
>                 for (var i = 0; i < hobies.length; i++){
> 
>                     hobies[i].checked = !hobies[i].checked;
>                 }
>             }
> 
>     </script>
> </head>
> <body ALINK="red">
> 
>     <input type="checkbox" name="hoby" checked="checked" value="Java">1
>     <input type="checkbox" name="hoby" value="C++">c++
>     <input type="checkbox" name="hoby" value="js">js
>     <br/>
>     <button onclick="checkAll()">全选</button>
>     <button onclick="checkNone()">全不选</button>
>     <button onclick="checkReverse()">反选</button>
> 
> </body>
> ```

### getElementsByTagName

> 通过标签名来获取对象，返回的也是一个集合，操作跟数组一致

### 建议

> 一、
>
> > 在选择获取对象的方法时，
> >
> > 第一考虑的是通过 id 因为他的范围较小，可以不用筛选
> >
> > 第二考虑的是通过 name
> >
> > 第三 考虑通过 tagname 
>
> 二、
>
> > 在使用获取对象时，可以重复筛选，对其进行调用，以达到是该目标或该目标群