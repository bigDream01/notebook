#### 强制对象类型转换和instanceof的使用

```java
ublic static void main(String[] args) {

        person a = new man();//对象的多态性
        //因为声明a为person类的变量，那么在man调用对象的结构中就只能
        //调用person中存在的属性和方法

        //a.weight;不被允许
        System.out.println("************");

        //如何进行子类的属性和方法的调用
        man m1 = (man) a;//强制类型转换
        System.out.println(m1.weight);//这时就可以调用m1.weight
        //缺点：有可能转换失败

        System.out.println("*************");



        /*instanceof关键字的使用
        *
        * a instanceof A：判断a是否是A的实例；如果是返回true，如果不是返回false
        可以理解为a的范围里是否包含着A，如果是返回true，如果不是返回false
        *
        *
        *使用情景：为了避免ClassCastException的异常
         */

        if(a instanceof man){

            man s1 = (man) a;//这样就可以进行类型转换//instanceof主要是用来判断是否可以进行类型性转换

        }
        //如果a instanceof A 返回true；a instanceof B返回true；那么类B是类A的父类
        if( a instanceof Object){
            System.out.println("keyi");
        }
        //即object是man的父类，则可以说m0父类可以返回true

        //问题练习
        //问题一：编译通过，运行时不通过
//        person p3 = new women();
//        man m3 = (man)p3;

    }
```

### 强制类型转换

![image-20200622145438023](C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20200622145438023.png)

> 用于调用子类中特有的结构