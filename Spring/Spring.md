# 9、使用java的方式配置spring


实体类User.java

```java
/**
 *
 * @Component这个注解的意思，就是说明这个类被spring接管了，注册到了容器中
 */
@Component
public class User {
    private String name;

    public String getName() {
        return name;
    }

    /**
     * 属性注入值
     * @param name
     */
    @Value("zyy")
    public void setName(String name) {
        this.name = name;
    }

}
```

配置类MyConfig.java

```java
/**
 *
 * @Configuration这个也会被spring容器托管，注册到容器中，因为他本身就是一个@Component
 *
 * @Configuration代表这是一个配置类，就和我们之前看的beans.xml
 * @Import导入其他的配置类
 */
@Configuration
@ComponentScan("com.zyy.pojo")
@Import(MyConfig2.class)
public class MyConfig {

    /**
     * 注册一个bean，就相当于我们之前写的一个bean标签
     * 这个方法的名字，就相当于bean标签的id属性
     * 这个方法的返回这，就相当于bean标签中的class属性
     * @return
     */
    @Bean
    public User getUser(){
        //就是返回要注入到bean的对象。
        return new User();
    }
}
```

MyConfig2.java

```java
@Configuration
@ComponentScan("com.zyy.pojo")
public class MyConfig2 {

}
```

测试类

```java
    @Test
    public void test() {
        //如果完全使用配置类方式去做，我们就只能通过AnnotationConfig上下本去获取容器，通过配置类的class对象加载
        ApplicationContext context = new AnnotationConfigApplicationContext(MyConfig.class);
        User getUser = context.getBean("getUser", User.class);
        System.out.println(getUser.getName());
    }
}
```

这种纯java的配置方式，在springBoot中随处可见！

# 10、代理模式

为什么要学习代理模式？因为就是SpringAOP的底层【SpringAOP 和 SpringMVC】

代理模式的分类：

- 静态代理
- 动态代理

## 10.1、静态代理

角色分析：

- 抽象角色：一般会使用接口或者抽象类来解决
- 真实角色：被代理的角色
- 代理角色：代理真实角色，代理真实角色后，我们一般会做一些附属操作
- 客户：访问代理对象的人



代码步骤：

1. 接口【抽象出具体的功能】

   ```java
   public interface Rent {
       /**
        * 租房
        */
       void rent();
   }
   ```

2. 真实角色

   ```java
   public class House implements Rent {
   
       public void rent() {
           System.out.println("我要出租房子~");
       }
   }
   ```

3. 代理角色

   ```java
   public class HouseProxy implements Rent {
   
       private House house;
   
       public HouseProxy() {
   
       }
   
       public HouseProxy(House house) {
           this.house = house;
       }
   
   
       public void rent() {
           seeHouse();
           house.rent();
           agreement();
           fee();
       }
   
       public void seeHouse() {
           System.out.println("看房~");
       }
   
       public void agreement() {
           System.out.println("签合同~");
       }
   
       public void fee() {
           System.out.println("收费~");
       }
       
   }
   ```

   

4. 客户端访问代理角色

   ```java
   public class Client {
       public static void main(String[] args) {
           House house = new House();
           HouseProxy houseProxy = new HouseProxy(house);
           houseProxy.rent();
       }
   }
   ```

   

代理模式的好处：

- 可以使真实角色的操作更加纯粹，不用去关注一些公共的业务！
- 公共也就交给代理角色，实现了业务的分工！
- 公共业务发生扩展的时候，方便集中管理！



缺点：

- 一个真实角色就会产生一个代理角色，代码量会翻倍，开发效率会变低



## 10.2、静态代理再理解

标准的增删改查功能，突然要在原来的基础上新增日志打印的功能，怎么不改变原代码的基础上，实现功能呢？

1. 接口

   ```java
   public interface UserDao {
       void add();
       void delete();
       void update();
       void query();
   }
   ```

   

2. 真实角色

   ```java
   public class UserDaoImpl implements UserDao {
       public void add() {
           System.out.println("新增一个用户~");
       }
   
       public void delete() {
           System.out.println("删除一个用户~");
       }
   
       public void update() {
           System.out.println("修改一个用户~");
       }
   
       public void query() {
           System.out.println("查询一个用户~");
       }
   }
   ```

   

3. 代理角色

   ```java
   public class UserDaoProxy implements UserDao {
       private UserDaoImpl userDaoImpl;
   
       public void setUserDaoImpl(UserDaoImpl userDaoImpl) {
           this.userDaoImpl = userDaoImpl;
       }
   
       public void add() {
           log("add");
           userDaoImpl.add();
       }
   
       public void delete() {
           log("delete");
           userDaoImpl.delete();
       }
   
       public void update() {
           log("update");
           userDaoImpl.update();
       }
   
       public void query() {
           log("query");
           userDaoImpl.query();
       }
   
       public void log(String msg) {
           System.out.println("[debug]调用了" + msg + "方法");
       }
   }
   ```

   

4. 访问

   ```java
   public class UserService {
       public static void main(String[] args) {
   
           UserDaoImpl userDaoImpl = new UserDaoImpl();
           UserDaoProxy userDaoProxy = new UserDaoProxy();
           userDaoProxy.setUserDaoImpl(userDaoImpl);
   
           userDaoProxy.add();
       }
   }
   ```

## 10.3、动态代理

- 动态代理和静态代理角色一样
- 动态代理的代理类是动态生成的，不是我们直接写好的
- 动态代理分成两个类：基于接口的动态，基于类的动态代理
  - 基于接口--jdk动态代理【我们在这里使用】
  - 基于类：cglib
  - java字节码实现：javasist



需要了解两个类

- Proxy：代理
- InvocationHandler：调用处理程序



动态代理的好处：

- 可以使真实角色的操作更加纯粹，不用去关注一些公共的业务！
- 公共也就交给代理角色，实现了业务的分工！
- 公共业务发生扩展的时候，方便集中管理！
- 一个动态代理类代理的是一个接口，一般就是对应一类业务
- 一个动态代理类可以代理多个类，只要是实现了同一个接口即可



ProxyInvocationHandle.java

```java
public class ProxyInvocationHandle implements InvocationHandler {
    /**
     * 被代理的接口
     */
    private Object obj;

    public void setObj(Object obj) {
        this.obj = obj;
    }

    /**
     * 生成得到代理类
     *
     * @return
     */
    public Object getProxy() {
        return Proxy.newProxyInstance(this.getClass().getClassLoader(), obj.getClass().getInterfaces(), this);
    }


    /**
     * 处理代理实例，并返回结果
     *
     * @param proxy
     * @param method
     * @param args
     * @return
     * @throws Throwable
     */
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        log(method.getName());
        //动态代理的本质，就是使用反射机制实现
        Object object = method.invoke(obj, args);
        return object;
    }

    public void log(String msg) {
        System.out.println("[debug]调用了" + msg + "方法");
    }
}
```

验证：

```java
public class UserService {
    public static void main(String[] args) {
        //真实角色
        UserDaoImpl userDaoImpl = new UserDaoImpl();
        //代理角色
        ProxyInvocationHandle invocationHandle = new ProxyInvocationHandle();
        //设置要代理的对象
        invocationHandle.setObj(userDaoImpl);
        //动态生成代理类
        UserDao proxy = (UserDao) invocationHandle.getProxy();
        proxy.add();
    }
}
```

**一个动态代理 , 一般代理某一类业务 , 一个动态代理可以代理多个类，代理的是接口！**

# 11、AOP

## 11.1、什么是AOP

AOP为Aspect Oriented Programming的缩写，意为：面向切面编程，通过预编译方式和运行期间动态代理实现程序功能的统一维护的一种技术。AOP是OOP的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生范型。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。

![image-20220511211111236](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220511211111236.png)



## 11.2、AOP在spring中的作用

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

## 11.3、使用spring实现AOP

【重点】使用AOP织入，需要导入一个依赖包

```xml
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.9.9</version>
        </dependency>
```

### 方式一：使用spring的api接口【主要springapi接口实现】

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

### 方式二：自定义来实现AOP【主要是切面定义】

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



### 方式三：使用注解实现

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

# 12、整合mybatis

步骤：

1. 导入jar包

   - junit

   - mybatis

   - mysql数据库

   - spring相关

   - aop织入

   - mybatis-spring

     ```xml
     
         <dependencies>
             <dependency>
                 <groupId>mysql</groupId>
                 <artifactId>mysql-connector-java</artifactId>
                 <version>5.1.47</version>
             </dependency>
     
             <dependency>
                 <groupId>org.mybatis</groupId>
                 <artifactId>mybatis</artifactId>
                 <version>3.5.2</version>
             </dependency>
             <dependency>
                 <groupId>org.springframework</groupId>
                 <artifactId>spring-jdbc</artifactId>
                 <version>5.3.19</version>
             </dependency>
             <dependency>
                 <groupId>org.aspectj</groupId>
                 <artifactId>aspectjweaver</artifactId>
                 <version>1.9.9</version>
             </dependency>
             <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
             <dependency>
                 <groupId>org.mybatis</groupId>
                 <artifactId>mybatis-spring</artifactId>
                 <version>2.0.6</version>
             </dependency>
             <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
             <dependency>
                 <groupId>org.projectlombok</groupId>
                 <artifactId>lombok</artifactId>
                 <version>1.18.22</version>
             </dependency>
             <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
             <dependency>
                 <groupId>org.springframework</groupId>
                 <artifactId>spring-webmvc</artifactId>
                 <version>5.3.19</version>
             </dependency>
             <dependency>
                 <groupId>junit</groupId>
                 <artifactId>junit</artifactId>
                 <version>4.12</version>
             </dependency>
         </dependencies>
     ```

2. 编写配置文件

3. 测试



## 12.1、回忆mybatis

1. 编写实体类

   ```java
   @Data
   public class User {
       private int id;
       private String name;
       private String pwd;
   }
   ```

2. 编写核心配置文件

   mybatis-config.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <!--configuration 核心配置文件-->
   <configuration>
       <!--    environment 元素体中包含了事务管理和连接池的配置-->
       <environments default="development">
           <environment id="development">
               <transactionManager type="JDBC"/>
               <dataSource type="POOLED">
                   <property name="driver" value="com.mysql.jdbc.Driver"/>
                   <property name="url"
                             value="jdbc:mysql://localhost:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
                   <property name="username" value="root"/>
                   <property name="password" value="123456"/>
               </dataSource>
           </environment>
       </environments>
       <!--    mappers 元素则包含了一组映射器（mapper），这些映射器的 XML 映射文件包含了 SQL 代码和映射定义信息。-->
       <!--   每一个Mapper.xml都需要在mybatis核心配置文件中注册 -->
       <mappers>
           <mapper resource="com/zyy/dao/UserMapper.xml"/>
       </mappers>
   </configuration>
   ```

3. 编写接口

   ```java
   public interface UserMapper {
       List<User> getUserList();
   }
   ```

4. 编写Mapper.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="com.zyy.dao.UserMapper">
       <select id="getUserList" resultType="com.zyy.pojo.User">
           select id, name, pwd from user
       </select>
   </mapper>
   ```

5. 测试

   ```java
   public class UserMapperTest {
       @Test
       public void getUserList(){
           try {
               InputStream inputStream = Resources.getResourceAsStream("mybatis-config.xml");
               SqlSessionFactory build = new SqlSessionFactoryBuilder().build(inputStream);
               SqlSession sqlSession = build.openSession();
   
               UserMapper mapper = sqlSession.getMapper(UserMapper.class);
               System.out.println(mapper.getUserList());
           } catch (IOException e) {
               e.printStackTrace();
           }
       }
   }
   ```

   

## 12.2、mybatis-spring

官方文档：[mybatis-spring –](http://mybatis.org/spring/zh/getting-started.html)



步骤（在上面的基础上改造）

1. 配置数据源

2. sqlSessionFactory

3. SqlSessionTemplate

   配置文件spring-dao.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <!--
       dataSource:使用spring的数据源替换mybatis的配置
       这里我们使用spring提供的：org.springframework.jdbc.datasource
       -->
       <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
           <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
           <property name="url"
                     value="jdbc:mysql://localhost:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
           <property name="username" value="root"/>
           <property name="password" value="123456"/>
       </bean>
   
       <!--
       sqlSessionFactory
       -->
       <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
           <property name="dataSource" ref="dataSource"/>
           <!--   绑定mybatis配置文件     -->
           <property name="configLocation" value="classpath:mybatis-config.xml"/>
           <property name="mapperLocations" value="classpath:com/zyy/dao/UserMapper.xml"/>
       </bean>
   
       <bean id="sqlSessionTemplate" class="org.mybatis.spring.SqlSessionTemplate">
           <!--    只能使用构造器注入sqlSessionFactory，因为它没有set方法    -->
           <constructor-arg index="0" ref="sqlSessionFactory"/>
       </bean>
   
   </beans>
   ```

   mybatis-config.xml改造后如下：

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <!--configuration 核心配置文件-->
   <configuration>
   
       <!--
       设置
       -->
       <!--<settings>
           <setting name="" value=""/>
       </settings>-->
   </configuration>
   ```

4. 实现接口

   UserMapperImpl.java

   ```java
   public class UserMapperImpl implements UserMapper {
   
   
       /**
        * 我们原来所有的操作都使用sqlSession来执行，现在都使用SqlSessionTemplate
        */
       private SqlSessionTemplate sessionTemplate;
   
       public void setSessionTemplate(SqlSessionTemplate sessionTemplate) {
           this.sessionTemplate = sessionTemplate;
       }
   
       public List<User> getUserList() {
           UserMapper mapper = sessionTemplate.getMapper(UserMapper.class);
           return mapper.getUserList();
       }
   }
   ```
   
5. 配置bean

   这里不直接在spring-dao.xml中配置，为了保持spring-dao.xml的干净整洁，因为这个配置文件后续基本不变，新增再下面这里操作即可！

   applicationContext.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <import resource="spring-dao.xml"/>
   
       <bean id="userMapper" class="com.zyy.dao.UserMapperImpl">
           <property name="sessionTemplate" ref="sqlSessionTemplate"/>
       </bean>
   
   </beans>
   ```

6. 测试（修改测试类）

   ```java
   public class UserMapperTest {
       @Test
       public void getUserList(){
           ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
           UserMapper userMapper = context.getBean("userMapper", UserMapper.class);
           System.out.println(userMapper.getUserList());
       }
   }
   ```



**另外一种SqlSessionDaoSupport实现**

新增一个UserMapperImpl2.java

```java
public class UserMapperImpl2 extends SqlSessionDaoSupport implements UserMapper {

    public List<User> getUserList() {
        return getSqlSession().getMapper(UserMapper.class).getUserList();
    }
}
```

注入spring  applicationContext.xml配置文件新增如下配置

```xml
    <bean id="userMapper2" class="com.zyy.dao.UserMapperImpl2">
        <property name="sqlSessionTemplate" ref="sqlSessionTemplate"/>
    </bean>
```

测试：

```java
public class UserMapperTest {
    @Test
    public void getUserList(){
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        UserMapper userMapper = context.getBean("userMapper2", UserMapper.class);
        System.out.println(userMapper.getUserList());
    }
}
```

**在上面的基础上还可以再简化一下**

注入spring改成

```xml
    <bean id="userMapper2" class="com.zyy.dao.UserMapperImpl2">
        <property name="sqlSessionFactory" ref="sqlSessionFactory"/>
    </bean>
```

这样的话spring-dao.xml中的sqlSessionTemplate也可以干掉

```xml
    <!--<bean id="sqlSessionTemplate" class="org.mybatis.spring.SqlSessionTemplate">
        &lt;!&ndash;    只能使用构造器注入sqlSessionFactory，因为它没有set方法    &ndash;&gt;
        <constructor-arg index="0" ref="sqlSessionFactory"/>
    </bean>-->
```

这里可以看一下SqlSessionDaoSupport的源码，注入sqlSessionFactory也会同时注入sqlSessionTemplate

```java
//...
  public void setSqlSessionFactory(SqlSessionFactory sqlSessionFactory) {
    if (this.sqlSessionTemplate == null || sqlSessionFactory != this.sqlSessionTemplate.getSqlSessionFactory()) {
      this.sqlSessionTemplate = createSqlSessionTemplate(sqlSessionFactory);
    }
  }
//...
```



# 13、声明式事务

## 13.1、回顾事务

- 要么都成功，要么都失败
- 事务在项目开发中，十分重要，涉及到数据的一致性问题，不能马虎
- 确保完整性和一致性



事务ACID原则：

- 原子性（atomicity）
  - 事务是原子性操作，由一系列动作组成，事务的原子性确保动作要么全部完成，要么完全不起作用
- 一致性（consistency）
  - 一旦所有事务动作完成，事务就要被提交。数据和资源处于一种满足业务规则的一致性状态中
- 隔离性（isolation）
  - 可能多个事务会同时处理相同的数据，因此每个事务都应该与其他事务隔离开来，防止数据损坏
- 持久性（consistency）
  - 事务一旦完成，无论系统发生什么错误，结果都不会受到影响。通常情况下，事务的结果被写到持久化存储器中



## 13.2、spring中事务管理

- 声明式事务：AOP
- 编程式事务：需要在代码中，进行事务的管理



思考：为什么需要事务？

- 如果不配置事务，可能存在数据不一致的情况下
- 如果我们不在spring中取配置声明式事务，我们就需要在代码中手动配置事务
- 事务在项目的开发中十分重要，设计到数据的一致性和完整性问题，不容马虎！



**声明式事务**

spring核心配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/tx
        http://www.springframework.org/schema/tx/spring-tx.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">

    <!--
    dataSource:使用spring的数据源替换mybatis的配置
    这里我们使用spring提供的：org.springframework.jdbc.datasource
    -->
    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        <property name="url"
                  value="jdbc:mysql://localhost:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
        <property name="username" value="root"/>
        <property name="password" value="123456"/>
    </bean>

    <!--
    sqlSessionFactory
    -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <!--   绑定mybatis配置文件     -->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <property name="mapperLocations" value="classpath:com/zyy/mapper/UserMapper.xml"/>
    </bean>

    <bean id="sqlSessionTemplate" class="org.mybatis.spring.SqlSessionTemplate">
        <!--    只能使用构造器注入sqlSessionFactory，因为它没有set方法    -->
        <constructor-arg index="0" ref="sqlSessionFactory"/>
    </bean>

    <!--    配置声明式事务-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <constructor-arg ref="dataSource"/>
    </bean>
    <!--    结合aop实现事务的织入   -->
    <!--配置事务通知-->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
        <tx:attributes>
            <!--给哪些方法配置事务-->
            <!--配置事务的传播特性：new propagation-->
            <tx:method name="add" propagation="REQUIRED"/>
            <tx:method name="delete" propagation="REQUIRED"/>
            <tx:method name="update" propagation="REQUIRED"/>
            <tx:method name="query" read-only="true"/>
            <tx:method name="*" propagation="REQUIRED"/>
        </tx:attributes>
    </tx:advice>

    <aop:config>
        <aop:pointcut id="txPointCut" expression="execution(* com.zyy.mapper.*.*(..))"/>
        <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointCut"/>
    </aop:config>

</beans>
```

**spring事务传播特性：**

事务传播行为就是多个事务方法相互调用时，事务如何在这些方法间传播。spring支持7种事务传播行为：

- propagation_requierd：如果当前没有事务，就新建一个事务，如果已存在一个事务中，加入到这个事务中，这是最常见的选择。
- propagation_supports：支持当前事务，如果没有当前事务，就以非事务方法执行。
- propagation_mandatory：使用当前事务，如果没有当前事务，就抛出异常。
- propagation_required_new：新建事务，如果当前存在事务，把当前事务挂起。
- propagation_not_supported：以非事务方式执行操作，如果当前存在事务，就把当前事务挂起。
- propagation_never：以非事务方式执行操作，如果当前事务存在则抛出异常。
- propagation_nested：如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行与propagation_required类似的操作

Spring 默认的事务传播行为是 PROPAGATION_REQUIRED，它适合于绝大多数的情况。

接口新增功能

```java
public interface UserMapper {
    /**
     * 查询一个用户
     * @return
     */
    List<User> getUserList();

    int addUser(User user);

    int deleteUser(int id);

}
```

实现类

```java
public class UserMapperImpl extends SqlSessionDaoSupport implements UserMapper {

    public List<User> getUserList() {
        //先新增一个用户
        User user = new User(3, "哈哈", "12345");
        addUser(user);
        //故意制造一个异常
//        System.out.println(1/0);
        //在删除一个用户
        deleteUser(4);
        return getSqlSession().getMapper(UserMapper.class).getUserList();
    }

    public int addUser(User user) {
        return getSqlSession().getMapper(UserMapper.class).addUser(user);
    }

    public int deleteUser(int id) {
        return getSqlSession().getMapper(UserMapper.class).deleteUser(id);
    }
}
```

Mapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.zyy.mapper.UserMapper">
    <select id="getUserList" resultType="com.zyy.pojo.User">
        select id, name, pwd from user
    </select>

    <insert id="addUser" parameterType="com.zyy.pojo.User">
        insert into user(id, name, pwd) values (#{id}, #{name}, #{pwd})
    </insert>

    <delete id="deleteUser" parameterType="int">
        delete from user where id=#{id}
    </delete>
</mapper>
```

测试类

```java
public class UserMapperTest {
    @Test
    public void getUserList(){
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        UserMapper userMapper = context.getBean("userMapper", UserMapper.class);
        System.out.println(userMapper.getUserList());
    }
}
```

如果不配置事务，

UserMapperImpl类中，现新增然后删除一个用户，在中间故意制造异常，会导致，**新增成功，删除失败**

配置事务后，

UserMapperImpl类中，现新增然后删除一个用户，在中间故意制造异常，那么，**新增失败，删除失败**
