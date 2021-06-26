**QueryRunner类**：

> update (connection,sql,parms)执行任何增删改语句
>
> query（connection，sql， ResultSetHandler，parms）执行任何查询语句

**ResultSetHandler**

> BeanHandler: 处理单行数据
>
> BeanListHandler：处理一个表数据，返回一个集合
>
> ScalarHandler：返回单个数据

**查询案例：**

```java
//查询单行数据
@Test
public void test1() throws Exception {

    Connection connection = JDBCUtilDruidConnection.getConnection();

    QueryRunner queryRunner = new QueryRunner();

    Object query = queryRunner.query(connection, "select * from boys where id=?", new BeanHandler(boys.class), 1);

    connection.close();
    System.out.println(query);

}

//查询多行数据
@Test
public void test2() throws Exception {
    Connection connection = JDBCUtilDruidConnection.getConnection();
    QueryRunner qr = new QueryRunner();


    List<boys> boys = qr.query(connection, "select * from boys", new BeanListHandler<>(boys.class));

    for (boys b : boys){
        System.out.println(b);
    }
    connection.close();

}

//查询单个实例
@Test
public void test3() throws Exception {
    Connection connection = JDBCUtilDruidConnection.getConnection();
    QueryRunner qr = new QueryRunner();
    Object query = qr.query(connection, "select id from boys where userCP=50", new ScalarHandler());
    System.out.println(query);

}
```

**修改数据**

```java
//获取连接
Connection connection = JDBCUtilsConnection.getConnection();

//执行查询
QueryRunner queryRunner = new QueryRunner();

String sql = "update boys set boyname=? where id=4";

//1.修改单个数据
int line = queryRunner.update(connection,sql,"段正淳");

System.out.println(line);
```