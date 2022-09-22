### AOP

#### AOP

AOP为Aspect Oriented Programming的缩写，意为：面向切面编程，通过预编译方式和运行期间动态代理实现程序功能的统一维护的一种技术。AOP是OOP的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生范型。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。

![image-20220511211111236](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220511211111236.png)



#### AOP在spring中的作用

提供声明式事务，允许用户自定义切面

- 横切关注点：跨越应用程序多个模块的方法或功能。即是，与我们业务逻辑无关的，但是我们需要关注的部分，就是横切关注点，如：日志，安全，缓存，事务等等。。。
- 切面（Aspect）：横切关注点，被模块化的特殊对象。即，它是一个类
- 通知（Advice）：切面必须要完成的工作。即，它是类中的一个方法
- 目标（Target）：被通知对象
- 代理（Proxy）：向目标对象应用通知之后创建的对象
- 切入点（PointCut）：切面通知 执行的“地点”的定义
- 连接点（JoinPoint）：与切入点匹配的执行点

![image-20220511211921345](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220511211921345.png)

SpringAOP中，通过Advice定义横切逻辑，Spring中支持5中类型的Advice

![image-20220511212139453](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220511212139453.png)

即Aop在不改变原有代码的情况下，去增加新的功能

#### 使用spring实现AOP

【重点】使用AOP织入，需要导入一个依赖包

```xml
<dependency>
	<groupId>org.aspectj</groupId>
	<artifactId>aspectjweaver</artifactId>
	<version>1.9.9</version>
</dependency>
```

##### 方式一：使用spring的api接口【主要springapi接口实现】

```java
public interface UserService {
    void add();

    void delete();

    void update();

    void query();
}
```

```java
public class UserServiceImpl implements UserService {
    public void add() {
        System.out.println("增加了一个用户");
    }

    public void delete() {
        System.out.println("删除了一个用户");
    }

    public void update() {
        System.out.println("更新了一个用户");
    }

    public void query() {
        System.out.println("查询了一个用户");
    }
}
```

```java
public class BeforeLog implements MethodBeforeAdvice {
    /**
     * @param method 要执行的方法
     * @param args   参数
     * @param target 目标对象
     * @throws Throwable
     */
    public void before(Method method, Object[] args, Object target) throws Throwable {
        System.out.println(target.getClass().getName() + "的" + method.getName() + "被执行了");
    }
}
```

```java
public class AfterLog implements AfterReturningAdvice {
    /**
     *
     * @param returnValue 返回值
     * @param method
     * @param args
     * @param target
     * @throws Throwable
     */
    public void afterReturning(Object returnValue, Method method, Object[] args, Object target) throws Throwable {
        System.out.println("执行了" + method.getName() + "方法，返回结果为" + returnValue);
    }
}
```

beans.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">

    <!--    注册bean-->
    <bean id="userService" class="com.zyy.service.UserServiceImpl"/>
    <bean id="beforeLog" class="com.zyy.log.BeforeLog"/>
    <bean id="afterLog" class="com.zyy.log.AfterLog"/>

    <!--    方式一：使用原生Spring API接口-->
    <!--配置aop：需要导入aop的约束-->
    <aop:config>
        <!--        切入点  expression:表达式  execution(要执行的位置 * * * * *) -->
        <aop:pointcut id="logCut" expression="execution(* com.zyy.service.UserServiceImpl.*(..))"/>
        <!--        执行环绕增加-->
        <aop:advisor advice-ref="beforeLog" pointcut-ref="logCut"/>
        <aop:advisor advice-ref="afterLog" pointcut-ref="logCut"/>
    </aop:config>

</beans>
```

[execution表达式](https://www.cnblogs.com/szqengr/p/14758983.html)


测试

```java
public class UserServiceTest {
    public static void main(String[] args) {

        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        //动态代理代理的是接口
        UserService userService = context.getBean("userService", UserService.class);
        userService.add();
    }
}
```

直接结果

```
com.zyy.service.UserServiceImpl的add被执行了
增加了一个用户
执行了add方法，返回结果为null
```

##### 方式二：自定义来实现AOP【主要是切面定义】

```java
public class DiyLog {
    public void before(){
        System.out.println("=======方法执行前========");
    }

    public void after() {
        System.out.println("=======方法执行后========");
    }

    public void around(ProceedingJoinPoint joinPoint) throws Throwable {
        System.out.println("环绕前");
        Object proceed = joinPoint.proceed();
        System.out.println("环绕后");
        System.out.println(joinPoint.getSignature());
        System.out.println(proceed);
    }
}
```

beans.xml 注释掉方式一，新增方式二

```xml
    <!--    第二种方式:自定义类-->
    <bean id="diyLog" class="com.zyy.diy.DiyLog"/>

    <aop:config>
        <!--        自定义切面，ref要引用的类-->
        <aop:aspect ref="diyLog">
            <!--            切入点-->
            <aop:pointcut id="logCut" expression="execution(* com.zyy.service.UserServiceImpl.*(..))"/>
            <aop:around method="around" pointcut-ref="logCut"/>
            <!--            通知-->
            <aop:before method="before" pointcut-ref="logCut"/>
            <aop:after method="after" pointcut-ref="logCut"/>
        </aop:aspect>
    </aop:config>
```

测试类不变，结果如下

```
环绕前
=======方法执行前========
增加了一个用户
环绕后
void com.zyy.service.UserService.add()
null
=======方法执行后========
```

修改around的位置后

```xml
    <aop:config>
        <!--        自定义切面，ref要引用的类-->
        <aop:aspect ref="diyLog">
            <!--            切入点-->
            <aop:pointcut id="logCut" expression="execution(* com.zyy.service.UserServiceImpl.*(..))"/>
            <!--            通知-->
            <aop:before method="before" pointcut-ref="logCut"/>
            <aop:after method="after" pointcut-ref="logCut"/>
            <aop:around method="around" pointcut-ref="logCut"/>
        </aop:aspect>
    </aop:config>
```

测试类不变，结果如下

```xml
=======方法执行前========
环绕前
增加了一个用户
环绕后
void com.zyy.service.UserService.add()
null
=======方法执行后========
```



##### 方式三：使用注解实现

```java
@Aspect
public class AnnotationPointCut {
    @Before(value = "execution(* com.zyy.service.UserServiceImpl.*(..))")
    public void before() {
        System.out.println("=======方法执行前========");
    }

    @After(value = "execution(* com.zyy.service.UserServiceImpl.*(..))")
    public void after() {
        System.out.println("=======方法执行后========");
    }

    @Around(value = "execution(* com.zyy.service.UserServiceImpl.*(..))")
    public void around(ProceedingJoinPoint joinPoint) throws Throwable {
        System.out.println("环绕前");
        Object proceed = joinPoint.proceed();
        System.out.println("环绕后");
        System.out.println(joinPoint.getSignature());
        System.out.println(proceed);
    }
}
```

beans.xml注释之前的两种方式，新增配置

```xml
    <!--    第三种方式-->
    <bean id="annotationPointCut" class="com.zyy.diy.AnnotationPointCut"/>
    <!--    开启注解支持 jdk（默认 proxy-target-class="false"） cglib（proxy-target-class="true"）-->
    <aop:aspectj-autoproxy/>
```

aop:aspectj-autoproxy：说明

```
通过aop命名空间的<aop:aspectj-autoproxy />声明自动为spring容器中那些配置@aspectJ切面的bean创建代理，织入切面。当然，spring 在内部依旧采用AnnotationAwareAspectJAutoProxyCreator进行自动代理的创建工作，但具体实现的细节已经被<aop:aspectj-autoproxy />隐藏起来了

<aop:aspectj-autoproxy />有一个proxy-target-class属性，默认为false，表示使用jdk动态代理织入增强，当配为<aop:aspectj-autoproxy  poxy-target-class="true"/>时，表示使用CGLib动态代理技术织入增强。不过即使proxy-target-class设置为false，如果目标类没有声明接口，则spring将自动使用CGLib动态代理。
```



测试类不变，结果如下

```
环绕前
=======方法执行前========
增加了一个用户
=======方法执行后========
环绕后
void com.zyy.service.UserService.add()
null
```

![image-20220512170228542](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220512170228542.png)
