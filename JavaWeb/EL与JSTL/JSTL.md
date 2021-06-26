## JSTL

> JSTL 标签库全称是指JSP Standard Tag Library JSP 标准标签库。是一个不断完善的开放源代码的JSP 标签库。
>        EL 表达式主要是为了替换jsp 中的表达式脚本，而标签库则是为了替换代码脚本。这样使得整个jsp 页面变得更加简洁。

###  在jsp 标签库中使用taglib 指令引入标签库

> ​		CORE 标签库
> **<%@ taglib prefix="a" uri="http://java.sun.com/jstl/core_rt" %>这个才是对的**
>
> ​		XML 标签库
> <%@ taglib prefix="x" uri="http://java.sun.com/jsp/jstl/xml" %>
>
> ​		FMT 标签库
> <%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
>
> ​		SQL 标签库
> <%@ taglib prefix="sql" uri="http://java.sun.com/jsp/jstl/sql" %>
>
> ​		FUNCTIONS 标签库
> <%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions" %>

### 标签

set标签: 

> ```jsp
> <%--
>     作用：set 标签可以往域中保存数据域对象.setAttribute(key,value);
> 
>         scope 属性设置保存到哪个域
>         page 表示PageContext 域（默认值）
>         request 表示Request 域
>         session 表示Session 域
>         application 表示ServletContext 域
>         var 属性设置key 是多少
>         value 属性设置值
> 
>         --%>
> 
>         <c:set scope="request" var="abc" value="第一次"></c:set>
>         ${requestScope.abc}
> 
> ```

**if（） 标签**

> ```jsp
> <c:set scope="request" var="abc" value="第一次"></c:set>
> ${requestScope.abc}<br/>
> 
> <%--       test = "中表示判断的条件"
> 
>            其中表示如果是true执行的语句--%>
> <c:if test="${12 == 12}">
>     if()
> </c:if><br>
> ```

choose标签

> ```jsp
> <%-- <c:choose>
> 
>       <c:when test="">
>       ......
>        <c:otherWise >
>        在其他情况都不成立的情况下的执行语句（类似于default）
>     与switch case 类似，当值为test内的值的时候执行其中的语句
> 
>     注意点：
>         1.标签里不要使用HTML注释
>         2.when标签的父标签一定要是choose标签
>         3.
>  --%>
> <%
>     request.setAttribute("key", 110);
> %>
> <c:choose>
>     <c:when test="${requestScope.key > 120}">
>         value
>     </c:when>
> 
>     <c:otherwise>
>         vale1
>     </c:otherwise>
> 
> </c:choose>
> ```

**循环标签**

> **foeach 循环**
>
> > 其同样是从域对象的里面去获取对象来进行遍历，
> >
> > items就是从域对象里面获取到的对象，
> >
> > var就是foreach循环中的那个循环变量
> >
> > step就是相当于步幅，类似与i++里的++

> 循环遍历数组
>
> ```jsp
> <%
>     request.setAttribute("arr",new int[]{1,2,3,45,6,45,6});
> %>
> 
> <c:forEach items="${requestScope.arr }" var="item">
>     ${ item }
> </c:forEach>
> ```
>
> 循环遍历list
>
> ```jsp
>  <%
>             List studentList = new ArrayList();
>             for(int i = 1; i < 10; i++){
> 
>                 studentList.add(new student(i,"name" + i, "password" +i,i));
>             }
>         request.setAttribute("stus",studentList);
>         %>
>         <table>
>             <tr>
>                 <th>编号</th>
>                 <th>姓名</th>
>                 <th>密码</th>
>                 <th>年龄</th>
>             </tr>
> 
> <%--         begin end step varstatus --%>
>         <c:forEach items="${requestScope.stus}" var="stu">
>             <tr>
>                 <td>${stu.id}</td>
>                 <td>${stu.name}</td>
>                 <td>${stu.passwrod}</td>
>                 <td>${stu.age}</td>
>             </tr>
> 
>         </c:forEach>
>         </table>
> ```
>
> 循环遍历map
>
> ```jsp
> <%
> 
>     Map map = new HashMap();
>     map.put("key1", "value1");
>     map.put("key2", "value2");
>     map.put("key3", "value3");
>     map.put("key4", "value4");
>     request.setAttribute("map",map);
> 
> %>
> <c:forEach items="${requestScope.map}" var="i">
> 
>     ${i.value}
> </c:forEach>
> ```

**注意点：**

> 调用类返回：
>
> > 返回的值调用类中 get（）
> >
> > 返回布尔数调用类中的is（）
>
> begin（）和 end（）表示开始和结束的位置下标0开始