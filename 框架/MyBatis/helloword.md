## mybatis的环境搭建

> 首先是创建一个bean和这个bean的到接口
>
> 然后在数据库创建一个表

## mybatis的配置

> **导包**
>
> >   log4j-1.2.17.jar
> > 		mybatis-3.4.1.jar
> > 		mysql-connector-java-5.1.7-bin.jar
>
> **编写配置**
>
> > **首先是创建一个mybatis全局的配置文件**
> >
> > ```xml
> > <?xml version="1.0" encoding="UTF-8" ?>
> > <!DOCTYPE configuration
> >         PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
> >         "http://mybatis.org/dtd/mybatis-3-config.dtd">
> > <configuration>
> >     <environments default="development">
> >         <environment id="development">
> >             <transactionManager type="JDBC"/>
> >             <dataSource type="POOLED">
> >                 <property name="driver" value="com.mysql.jdbc.Driver"/>
> >                 <property name="url" value="jdbc:mysql://localhost:3306/mybatis_test01"/>
> >                 <property name="username" value="root"/>
> >                 <property name="password" value="1999826wwb"/>
> >             </dataSource>
> >         </environment>
> >     </environments>
> > 
> > <!--    编写我们实现接口的配置文件-->
> >     <mappers>
> > <!--       从类路径下扫描-->
> >         <mapper resource="Employee.xml"/>
> >     </mappers>
> > </configuration>
> > ```
> >
> > **之后是单个类的配置**
> >
> > ```xml
> > <?xml version="1.0" encoding="UTF-8" ?>
> > <!DOCTYPE mapper
> >         PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
> >         "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
> > 
> > 
> > 
> > <!--    表示使用的接口 -->
> > <mapper namespace="com.company.dao.EmployeeDao">
> > <!--
> >         id： 表示该接口中的方法
> >         resultType： 表示其返回值的类
> > -->
> > <select id="getEmployeeById" resultType="com.company.bean.Employee">
> >     <!--   sql 语句-->
> >     select * from t_employee where id = #{id}
> >   </select>
> > </mapper>
> > 
> > ```
>
> **测试**
>
> > 对数据库进行改变(update)的时候需要执行commit操作,查询的时候是不需要的
>
> ```java
> public class test {
> 
>     @Test
>     public void test01() throws IOException {
> 
>         //获取sqlSessionFactory这个类
>         String resource = "mybatis.xml";
>         InputStream inputStream = Resources.getResourceAsStream(resource);
>         SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
> 
>         //类似于从数据库中获取连接
>         SqlSession session = sqlSessionFactory.openSession();
>         try {
>             //获取到接口的实现类
>             EmployeeDao employeeDao = session.getMapper(EmployeeDao.class);
>             //查询数据
> //            Employee employee = employeeDao.getEmployeeById(1);
> //            System.out.println(employee);
> 
> 
>             //插入数据
>             int i = employeeDao.insertEmployee(new Employee(null, "hhhh", 1, "aaa@qq.com"));
>             //对数据库进行改变的时候需要执行commit操作
>             session.commit();
>             System.out.println(i);
> 
>             //删除数据
> //            int i = employeeDao.deleteEmployeeById(1);
> //            System.out.println(i);
> 
>             //修改数据
> //            int aaa = employeeDao.updateEmployeeById(new Employee(1, "aaa", 1, "aaa@qq.com"));
> //            System.out.println(aaa);
> 
> 
> 
>         } finally {
>             //关闭连接
>             session.close();
>         }
> 
>     }
> }
> ```

> **俩个xml配置文件分别是：**
>
> ​			全局配置文件
>
> ​			sql映射文件
>
> **从session中获取到的employeeDao的对象是一个代理对象，mybatis自动创建的**
>
> **sqlSessionFactory和sqlSession：**
>
> ​				factory创建sqlSession只创建，factory只new一次
>
> ​				sqlSession：相当于connection和数据库交互的一次会话，每一次会话就应该创建一个新											的sqlSession



### dtd文件配置

> ![image-20210420183920050](C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20210420183920050.png)

