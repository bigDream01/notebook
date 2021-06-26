**Xml配置AOP**

> 一、将目标方法加入到容器内部
>
> 二、指定切面的类（id：刚刚在ioc中配置的）
>
> 三、指定切面的方法和一些参数信息

**细节问题**

> 一、在执行顺序上，环绕通知的前置通知跟配置的顺序有关。在切面标签中也存在order标签，可以指定切面执行的顺序。
>
> 二、在切面中也可以使用引用，分为方法内引用和切面引用。aop：poinCut
>
> 三、我们配置切面的时候，重要的使用xml配置，不重要的使用注解配置。外部引用的使用注解配置

```xml
<bean id="calculate" class="com.atguigu.bean.impl.CalculateImpl"></bean>
<bean id="calculateAspect" class="com.atguigu.bean.aspect.CalculateAspect"></bean>

<aop:config>
    <aop:pointcut id="pointCut" expression="execution(public int com.atguigu.bean.impl.CalculateImpl.*(int ,int ))"/>

    <aop:aspect ref="calculateAspect">
        <aop:before method="before" pointcut-ref="pointCut"></aop:before>
        <aop:after-returning method="afterReturning" pointcut-ref="pointCut" returning="result"></aop:after-returning>
        <aop:after method="after" pointcut-ref="pointCut"></aop:after>

    </aop:aspect>

</aop:config>
```

