 ###  Stream的概念

**1.Stream API的理解：**

> 1.1 Stream关注的是对数据的运算，与CPU打交道
> 集合关注的是数据的存储，与内存打交道
>
> 1.2 java8提供了一套api,使用这套api可以对内存中的数据进行过滤、排序、映射、归约等操作。类似于sql对数据库中表的相关操作。

**2.注意点：**

> ①Stream 自己不会存储元素。
>
> ②Stream 不会改变源对象。相反，他们会返回一个持有结果的新Stream。
>
> ③Stream 操作是延迟执行的。这意味着他们会等到需要结果的时候才执行。

**3.Stream的使用流程：**

> ① Stream的实例化
>
> ② 一系列的中间操作（过滤、映射、...)
>
> ③ 终止操作

**4.使用流程的注意点：**

> 4.1 一个中间操作链，对数据源的数据进行处理
>
> 4.2 一旦执行终止操作，就执行中间操作链，并产生结果。之后，不会再被使用

### stream的使用

**Stream的创建方式**

> ```java
> public static void main(String[] args) {
>     //方式一：通过集合创建Stream
>     ArrayList arrayList = new ArrayList();
> 
>     arrayList.add("h");
>     arrayList.add("q");
>     arrayList.add("d");
>     arrayList.add("f");
>     arrayList.add("b");
> 
>     Stream stream = arrayList.stream();
> 
> 
>     //方式二：通过数组创建Stream
> 
>     int []arr = new int[]{1,2,3,5,4,5,4};
>     IntStream stream1 = Arrays.stream(arr);
> 
>     //方式三：通过Stream自身进行Stream的创建
> 
>     String str1 = "hi ";
>     String str2 = "hello ";
> 
>     Stream<String> strSum = Stream.of(str1, str2);
> ```





**streamAPI的方法中间测试**

> **筛选与切片**
>
> ```java
> List<Employee> employees = EmployeeData.getEmployees();
> //筛选与切片
> 
> //筛选：筛选元素filter（lambda表达式）
> 
> Stream<Employee> stream1 = employees.stream();
> stream1.filter(e -> e.getSalary() > 7000).forEach(System.out :: println);
> 
> System.out.println();
> 
> //切片:截取元素limit(n)，截取元素n个元素。
> Stream<Employee> stream2 = employees.stream();//因为Stream的特性，需要重新创建一个Stream。
> stream2.limit(3).forEach(System.out ::println);
> System.out.println();
> 
> //跳过：skip（n）跳过元素n个元素
> Stream<Employee> stream4 = employees.stream();
> stream4.skip(3).forEach(System.out ::println);
> System.out.println();
> 
> 
> //去除重复的数据，通过equals（）和hashcode（）来进行去重
> Stream<Employee> strea3 = employees.stream();
> strea3.distinct().forEach(System.out ::println);
> ```
>
> **映射:可以用函数思想理解，对每个未知数进行操作然后返回一个值**
>
> ```java
> 
> //map（fuction） 将stream中的每一个单一元素操作（转为大写）
> List<String> strings = Arrays.asList("aa", "dd", "ss", "ff");
> 
> 
> List list = EmployeeData.getEmployees();
> 
> Stream stream = list.stream();
> 
> 
> strings.stream().map(s -> s.toUpperCase()).forEach(System.out::println);
> ```
>
> ```java
> 
> //flatMap（fuction） 将stream中的每一个单一元素操作（转为大写）
> List<String> strings = Arrays.asList("aa", "dd", "ss", "ff");
> 
> List list = EmployeeData.getEmployees();
>  //flatMap ( fuction ) 接收一个函数作为一个参数，
>         //将流中的每一个值转换为另外的一个流，然后把所有的流连接为一个流
>         Stream<Character> characterStream = strings.stream().flatMap(StreamTest2::fromString);
>         haracterStream.forEach(System.out::print);
> 
> //将字符串中的多个字符转换为集合对应的Stream的实例
>     public static Stream<Character> fromString(String s) {
> 
>         ArrayList<Character> list = new ArrayList();
>         for (Character c : s.toCharArray()) {
>             list.add(c);
> 
>         }
> 
>         return list.stream();
> 
>     }
> ```
>
> **排序**
>
> ```java
>  //排序测试
>     @Test
>     public void test (){
> 
>         //自然排序
>         List<Integer> integers = Arrays.asList(1, 2, 3, 6, 9, 23, 26, 4, 8);
>         integers.stream().sorted().forEach(System.out::println);
> 
> 
>         //定制排序
>         List<Employee> employees = EmployeeData.getEmployees();
> 
>         employees.stream().sorted((e1,e2) ->{
>            return Integer.compare(e1.getAge(),e2.getAge());
>         }).forEach(System.out::println);
> 
> 
>     }
>         employees.stream().sorted((e1,e2) ->{
>            return Integer.compare(e1.getAge(),e2.getAge());
>         }).forEach(System.out::println);
> 
> 
>     }
> ```

### StreamAPI的终止操作

> **查找与匹配**
>
> ```java
>   //allMatch(Predicate p) 检查是否匹配所有元素
>     @Test
>     public void test1(){
>         List<Employee> employees = EmployeeData.getEmployees();
>         boolean b = employees.stream().allMatch(e -> e.getSalary() > 11000);
>         System.out.println(b);
> 
>     }
>     //anyMatch(Predicate p) 检查是否至少匹配一个元素
>     @Test
>     public void test2(){
>         List<Employee> employees = EmployeeData.getEmployees();
>         boolean house = employees.stream().anyMatch(employee -> employee.getName().startsWith("马"));
> 
>         System.out.println(house);
> 
>     }
>     //noneMatch(Predicate p) 检查是否没有匹配所有元素
>     @Test
>     public void test3(){
>         List<Employee> employees = EmployeeData.getEmployees();
>         boolean b = employees.stream().noneMatch(e -> e.getSalary() > 5000);
>         System.out.println(b);
>     }
> 
>     //findFirst() 返回第一个元素
> 
>     @Test
>     public void test4(){
>         List<Employee> employees = EmployeeData.getEmployees();
>         Optional<Employee> first = employees.parallelStream().findFirst();
>         System.out.println(first);
>     }
>     //findAny() 返回当前流中的任意元素
> 
>     @Test
>     public void test5(){
>         List<Employee> employees = EmployeeData.getEmployees();
>         Optional<Employee> any = employees.parallelStream().findAny();
>         System.out.println(any);
>     }
> 
> 
> }
> ```
>
> **归约**
>
> ```java
> //归约
> @Test
> public void test6() {
>     List<Employee> employees = EmployeeData.getEmployees();
> 
>     //计算所有元素的和
>     Stream<Employee> employeeStream = employees.stream();
> 
>     Stream<Double> doubleStream = employeeStream.map(e -> e.getSalary());
> 
>     double sumSalary = doubleStream.reduce(0.0, Double::sum);
>     System.out.println(sumSalary);
>     
>     //将流中的元素反复的结合起来，得到一个值，返回Optional<>
> 
> 
> }
> ```
>
> **收集**
>
> ```java
> //收集
> @Test
> public void test7() {
>     List<Employee> employees = EmployeeData.getEmployees();
>     List<Employee> salaryList = employees.parallelStream().filter(employee -> employee.getSalary() > 3000).collect(Collectors.toList());
> 
>     salaryList.forEach(System.out::println);
> 
> 
> 
> }
> ```



