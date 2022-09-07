# 1、Spring

## 1.1、简介

- Spring:春天-----给软件行业带来春天。
- 2002，首次推送了spring框架的雏形：interface21框架
- spring框架已interface21框架为基础，经过重新设计，并不断丰富其内涵，于2004年3月24日，发布了1.0正式版。
- Rod Johnson，spring framework创始人，著名作者。很难想象Rod Johnson的学历，真的让好多人大吃一惊，它是悉尼大学的博士，然而他的专业不是计算机，而是音乐学。
- spring理念：使现在的技术更加容易使用，本身是一个大杂烩，整合了现有的技术框架！
- SSH:Structs + Spring + Hibernate
- SSM:SpringMVC + Spring + Mybatis
- 官网：[Spring Framework Overview](https://docs.spring.io/spring-framework/docs/current/reference/html/overview.html#overview)
- github地址：[GitHub - spring-projects/spring-framework: Spring Framework](https://github.com/spring-projects/spring-framework)
- 中文文档：[Spring Framework 中文文档 - Spring Framework 5.1.3.RELEASE Reference | Docs4dev](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/)
- 我的代码：https://gitee.com/zhayuyao/spring-study

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.3.19</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.3.19</version>
</dependency>
```



## 1.2、优点

- spring是一个开源的免费的框架（容器）！
- spring是一个轻量级的、非侵入式的框架！
- 控制反转（IOC）、面向切面编程（AOP）！
- 支持事务的处理，对框架整合的支持！



==总结一句话：spring就是一个轻量级的控制反转（IOC）和面向切面编程（AOP）的框架！==

## 1.3、组成

![img](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/2019102923475419.png)

spring框架是一个分层架构，由7个模块组成，spring模块构建在核心容器之上，核心容器定义了创建、配置和管理bean的方式。

![img](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/2020010313504529.png)

组成spring框架的每个模块（或者组件）都可以单独存在，或者与其他一个或者多个模块联合实现。每个模块的功能如下：

- **核心容器**：核心容器提供 Spring 框架的基本功能。核心容器的主要组件是 BeanFactory，它是工厂模式的实现。BeanFactory 使用*控制反转*（IOC） 模式将应用程序的配置和依赖性规范与实际的应用程序代码分开。
- **Spring 上下文**：Spring 上下文是一个配置文件，向 Spring 框架提供上下文信息。Spring 上下文包括企业服务，例如 JNDI、EJB、电子邮件、国际化、校验和调度功能。
- **Spring AOP**：通过配置管理特性，Spring AOP 模块直接将面向切面的编程功能 , 集成到了 Spring 框架中。所以，可以很容易地使 Spring 框架管理任何支持 AOP的对象。Spring AOP 模块为基于 Spring 的应用程序中的对象提供了事务管理服务。通过使用 Spring AOP，不用依赖组件，就可以将声明性事务管理集成到应用程序中。
- **Spring DAO**：JDBC DAO 抽象层提供了有意义的异常层次结构，可用该结构来管理异常处理和不同数据库供应商抛出的错误消息。异常层次结构简化了错误处理，并且极大地降低了需要编写的异常代码数量（例如打开和关闭连接）。Spring DAO 的面向 JDBC 的异常遵从通用的 DAO 异常层次结构。
- **Spring ORM**：Spring 框架插入了若干个 ORM 框架，从而提供了 ORM 的对象关系工具，其中包括 JDO、Hibernate 和 iBatis SQL Map。所有这些都遵从 Spring 的通用事务和 DAO 异常层次结构。
- **Spring Web 模块**：Web 上下文模块建立在应用程序上下文模块之上，为基于 Web 的应用程序提供了上下文。所以，Spring 框架支持与 Jakarta Struts 的集成。Web 模块还简化了处理多部分请求以及将请求参数绑定到域对象的工作。
- **Spring MVC 框架**：MVC 框架是一个全功能的构建 Web 应用程序的 MVC 实现。通过策略接口，MVC 框架变成为高度可配置的，MVC 容纳了大量视图技术，其中包括 JSP、Velocity、Tiles、iText 和 POI。

## 1.4、拓展

在spring的官网有这个介绍：现代化的java开发！说白就是基于spring的开发！

![image-20220502111643372](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220502111643372.png)

**Spring Boot与Spring Cloud**

- Spring Boot 是 Spring 的一套快速配置脚手架，可以基于Spring Boot 快速开发单个微服务;
- Spring Cloud是基于Spring Boot实现的；
- Spring Boot专注于快速、方便集成的单个微服务个体，Spring Cloud关注全局的服务治理框架；
- Spring Boot使用了约束优于配置的理念，很多集成方案已经帮你选择好了，能不配置就不配置 , Spring Cloud很大的一部分是基于Spring Boot来实现，Spring Boot可以离开Spring Cloud独立使用开发项目，但是Spring Cloud离不开Spring Boot，属于依赖的关系。
- SpringBoot在SpringClound中起到了承上启下的作用，如果你要学习SpringCloud必须要学习SpringBoot。



因为现在大多数公司都在使用springboot进行快速开发，学习springboot的前提，需要完全掌握spring和springmvc！承上启下的作用！



spring弊端：发展了太久，违背了原来的理念，配置十分繁琐，人称：“配置地狱”

# 2、IOC理论推导

新建一个空白的maven项目

我们先用我们原来的方式写一段代码

1. UserDao

   ```
   public interface UserDao {
       void addUser();
   }
   ```

2. UserDaoOracleImpl

   ```java
   public class UserDaoOracleImpl implements UserDao {
       @Override
       public void addUser() {
           System.out.println("添加一个用户");
       }
   }
   ```

3. UserService

   ```java
   public interface UserService {
       void addUser();
   }
   ```

4. UserServiceImpl

   ```java
   public class UserServiceImpl implements UserService {
   
       private UserDao userDao = new UserDaoOracleImpl();
   
       @Override
       public void addUser() {
           userDao.addUser();
       }
   }
   ```

5. 测试一下

   ```java
   public class UserServiceTest {
       
       @Test
       public void addUser() {
           UserService service = new UserServiceImpl();
           service.addUser();
       }
   }
   ```

现在我们修改一下，把Userdao的实现类增加一个

```java
public class UserDaoMySqlImpl implements UserDao {
    @Override
    public void addUser() {
        System.out.println("mysql新增一个用户");
    }
}
```

我们要使用mysql的话，就需要去service类里修改对应的实现

```java
public class UserServiceImpl implements UserService {

    private UserDao userDao = new UserDaoMySqlImpl();

    @Override
    public void addUser() {
        userDao.addUser();
    }
}
```

假设我们再新增一个Userdao的实现类

```java
public class UserDaoOracleImpl implements UserDao {
    @Override
    public void addUser() {
        System.out.println("oracle新增一个用户");
    }
}
```

使用oracle，又需要去service实现类里面修改对应的实现。假设我们的这种需求非常大 , 这种方式就根本不适用了, 甚至反人类对吧 , 每次变动 , 都需要修改大量代码 。 这种设计的耦合性太高了，牵一发而动全身。



那我们如何解决？

我们可以在需要用到他的地方 , 不去实现它 , 而是留出一个接口 , 利用set , 我们去代码里修改下 

```java
public class UserServiceImpl implements UserService {
    
    private UserDao userDao;

    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

    @Override
    public void addUser() {
        userDao.addUser();
    }
}
```

测试

```java
public class UserServiceTest {

    @Test
    public void addUser() {
        UserServiceImpl service = new UserServiceImpl();
        service.setUserDao(new UserDaoMySqlImpl());
        service.addUser();
    }
}
```

这里发生了革命性个改变

- 之前，程序是主动创建对象，控制权在程序员手上

- 使用set注入之后，程序不再具有主动性，而是变成了被动接受的对象！

这种思想，从本质上解决了问题，我们程序员不用再去管理对象的创建了。系统的耦合性大大降低了，可以更加注重在业务上的实现，这是ioc的原型。

## 2.1、IOC本质

**控制反转IoC(Inversion of Control)，是一种设计思想，DI(依赖注入)是实现IoC的一种方法**，也有人认为DI只是IoC的另一种说法。没有IoC的程序中 , 我们使用面向对象编程 , 对象的创建与对象间的依赖关系完全硬编码在程序中，对象的创建由程序自己控制，控制反转后将对象的创建转移给第三方，个人认为所谓控制反转就是：获得依赖对象的方式反转了。

![图片](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/640)

**IoC是Spring框架的核心内容**，使用多种方式完美的实现了IoC，可以使用XML配置，也可以使用注解，新版本的Spring也可以零配置实现IoC。

Spring容器在初始化时先读取配置文件，根据配置文件或元数据创建与组织对象存入容器中，程序使用时再从Ioc容器中取出需要的对象。

采用XML方式配置Bean的时候，Bean的定义信息是和实现分离的，而采用注解的方式可以把两者合为一体，Bean的定义信息直接以注解的形式定义在实现类中，从而达到了零配置的目的。

**控制反转是一种通过描述（XML或注解）并通过第三方去生产或获取特定对象的方式。在Spring中实现控制反转的是IoC容器，其实现方法是依赖注入（Dependency Injection,DI）。**

# 3、HelloSpring

1. 导包

   ```xml
           <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-webmvc</artifactId>
               <version>5.3.19</version>
           </dependency>
   ```

2. 编写代码

   ```java
   public class Hello {
       private String str;
   
       public String getStr() {
           return str;
       }
   
       public void setStr(String str) {
           this.str = str;
       }
   
       @Override
       public String toString() {
           return "Hello{" +
                   "str='" + str + '\'' +
                   '}';
       }
   }
   ```

3. 编写我们的spring文件，这里命名为beans.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <!--
       bean就是java对象 , 由Spring创建和管理
       使用spring来创建对象，在spring这些都称为bean
       类型 变量名 = new 类();
       Hello helloSpring = new Hello();
   
       id = 变量名
       class = new 的对象
       property相当于给对象中属性设置一个值！
   
   
       -->
       <bean id="helloSpring" class="com.zyy.pojo.Hello">
           <property name="str" value="spring"/>
       </bean>
   
   </beans>
   ```

4. 测试

   ```java
   public class HelloTest {
       @Test
       public void test() {
           //解析beans.xml文件 , 生成管理相应的Bean对象
           ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
           //getBean : 参数即为spring配置文件中bean的id .
           Hello helloSpring = (Hello) context.getBean("helloSpring");
           System.out.println(helloSpring);
       }
   }
   ```

   

思考：

- Hello 对象是谁创建的 ?

  hello 对象是由Spring创建的

- Hello 对象的属性是怎么设置的 ?

  hello 对象的属性是由Spring容器设置的

这个过程就叫控制反转：

- 控制：谁在控制对象的创建，传统的应用程序的对象是由程序本身控制创建的，使用spring后，对象由spring来创建
- 反转：程序本身不创建对象，而变成被动的接收对象

依赖注入：就是利用set方法来进行注入的

==IOC是一种编程思想，由主动的编程变成被动的接收==



修改一下一开始的案例

新增一个spring配置文件beans.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="userDaoMySqlImpl" class="com.zyy.dao.UserDaoMySqlImpl">
    </bean>
    <bean id="userDaoOracleImpl" class="com.zyy.dao.UserDaoOracleImpl">
    </bean>

    <bean id="userServiceImpl" class="com.zyy.service.UserServiceImpl">
        <!-- 注意: 这里的name并不是属性 , 而是set方法后面的那部分 , 首字母小写 -->
        <!-- 引用另外一个bean , 不是用value 而是用 ref -->
        <property name="userDao" ref="userDaoOracleImpl"/>
    </bean>
</beans>
```

测试

```java
    @Test
    public void addUser2() {
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        UserServiceImpl service = (UserServiceImpl) context.getBean("userServiceImpl");
        service.addUser();
    }
```

**OK , 到了现在 , 我们彻底不用再程序中去改动了 , 要实现不同的操作 , 只需要在xml配置文件中进行修改 , 所谓的IoC,一句话搞定 : 对象由Spring 来创建 , 管理 , 装配 !** 

# 4、IOC创建对象方式

1. 使用无参构造创建对象，默认

   User.java

   ```java
   public class User {
       private String name;
   
       public User() {
           System.out.println("User无参构造方法");
       }
   
       public String getName() {
           return name;
       }
   
       public void setName(String name) {
           this.name = name;
       }
   
       public void show() {
           System.out.println("name=" + this.name);
       }
   }
   
   ```

   beans.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <bean id="user" class="com.zyy.pojo.User">
           <property name="name" value="zyy"/>
       </bean>
   
   </beans>
   ```

   测试类

   ```java
   public class UserTest {
   
       @Test
       public void test() {
           ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
           User user = (User) context.getBean("user");
           user.show();
       }
   }
   ```

   结果我们可以发现，在调用show()方法之前，User对象已经通过无参构造初始化了。

2. 假设我们要使用有参构造创建对象

   ```java
   public class User {
       private String name;
   
       public User(String name) {
           this.name = name;
           System.out.println("User构造方法给name赋值"+this.name);
       }
   
       public String getName() {
           return name;
       }
   
       public void setName(String name) {
           this.name = name;
       }
   
       public void show() {
           System.out.println("name=" + this.name);
       }
   }
   ```

   1. 下标赋值

      ```xml
          <bean id="user" class="com.zyy.pojo.User">
              <constructor-arg index="0" value="zyy02"/>
          </bean>
      ```

   2. 类型

      ```xml
          <!--    不推荐使用-->
          <bean id="user" class="com.zyy.pojo.User">
              <constructor-arg type="java.lang.String" value="zyy03"/>
          </bean>
      ```

   3. 参数名

      ```xml
          <bean id="user" class="com.zyy.pojo.User">
              <constructor-arg name="name" value="zyy04"/>
          </bean>
      ```

总结：在配置文件加载的时候，容器中管理的对象就已经初始化了。

# 5、spring配置

## 5.1、别名

```xml
    <bean id="user" class="com.zyy.pojo.User">
        <constructor-arg name="name" value="zyy04"/>
    </bean>
    <!--  别名，如果添加了别名，我们也可以使用别名获取到这个对象 -->
    <alias name="user" alias="userNew"/>
```



## 5.2、bean的配置

```xml
    <!--
     id：bean的标识符,要唯一,如果没有配置id,name就是默认标识符
     class：bean对象所对应的全限定名：包名+类名
     name：如果配置id,又配置了name,那么name是别名，而且name可以同时取多个别名，name可以设置多个别名,可以用逗号,分号,空格隔开
     -->
    <bean id="user" class="com.zyy.pojo.User" name="u1 u2,u3;u4">
        <constructor-arg name="name" value="zyy04"/>
    </bean>
```



## 5.3、import

这个import，一般用于团队开发，它可以将多个配置文件，导入合并为一个

假设，现在项目中有多个人开发，这三个人负责不同的类开发，不同的类需要注册在不同的bean中，我们可以利用import将所有的beans.xml合并为一个总的

- 张三

- 李四

- 王五

- applicationContext.xml

  ```xml
      <import resource="beans1.xml"/>
      <import resource="beans2.xml"/>
      <import resource="beans3.xml"/>
  ```

使用的时候，直接使用总的配置就可以了。

# 6、依赖注入

## 6.1、构造器注入

前面已经说过了

## 6.2、set方式注入【重点】

依赖注入：set注入

- 依赖：bean对象的创建依赖于容器
- 注入：beans对象的所有属性，由容器来注入
- 

要求被注入的属性 , 必须有set方法 , set方法的方法名由set + 属性首字母大写 , 如果属性是boolean类型 , 没有set方法 , 是 is



环境搭建

Address.java

```java
public class Address {
    private String address;

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }
}
```

Student.java

```java
public class Student {
    private String name;
    private Address address;
    private String[] books;
    private List<String> hobbys;
    private Map<String, String> card;
    private Set<String> games;
    private String wife;
    private Properties info;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Address getAddress() {
        return address;
    }

    public void setAddress(Address address) {
        this.address = address;
    }

    public String[] getBooks() {
        return books;
    }

    public void setBooks(String[] books) {
        this.books = books;
    }

    public List<String> getHobbys() {
        return hobbys;
    }

    public void setHobbys(List<String> hobbys) {
        this.hobbys = hobbys;
    }

    public Map<String, String> getCard() {
        return card;
    }

    public void setCard(Map<String, String> card) {
        this.card = card;
    }

    public Set<String> getGames() {
        return games;
    }

    public void setGames(Set<String> games) {
        this.games = games;
    }

    public String getWife() {
        return wife;
    }

    public void setWife(String wife) {
        this.wife = wife;
    }

    public Properties getInfo() {
        return info;
    }

    public void setInfo(Properties info) {
        this.info = info;
    }

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", address=" + address +
                ", books=" + Arrays.toString(books) +
                ", hobbys=" + hobbys +
                ", card=" + card +
                ", games=" + games +
                ", wife='" + wife + '\'' +
                ", info=" + info +
                '}';
    }
}
```

### 6.2.1、常量注入

```xml
    <bean id="student" class="com.zyy.pojo.Student">
        <property name="name" value="zyy"/>
    </bean>
```

### 6.2.2、bean注入

```xml
    <bean id="address" class="com.zyy.pojo.Address">
        <property name="address" value="深圳"/>
    </bean>

    <bean id="student" class="com.zyy.pojo.Student">
        <property name="name" value="zyy"/>
        <property name="address" ref="address"/>
    </bean>
```

### 6.2.3、数组注入

```xml
        <property name="books">
            <array>
                <value>西游记</value>
                <value>水浒传</value>
                <value>红楼梦</value>
                <value>三国演义</value>
            </array>
        </property>
```

### 6.2.4、List注入

```xml
        <property name="hobbys">
            <list>
                <value>听歌</value>
                <value>敲代码</value>
                <value>看电影</value>
            </list>
        </property>
```

### 6.2.5、Map注入

```xml
        <property name="card">
            <map>
                <entry key="idNo" value="12343"/>
                <entry key="bankNo" value="54657"/>
            </map>
        </property>
```

### 6.2.6、Set注入

```xml
        <property name="games">
            <set>
                <value>LOL</value>
                <value>CS</value>
            </set>
        </property>
```

### 6.2.7、NULL注入

```xml
        <property name="wife">
            <null/>
        </property>
```

### 6.2.8、Properties注入

```xml
        <property name="info">
            <props>
                <prop key="no">1234</prop>
                <prop key="sex">男</prop>
            </props>
        </property>
```



## 6.3、拓展方式注入

### 6.3.1、P命名和C命名注入

User.java

```java
public class User {
    private String name;
    private int age;

    public User() {
    }

    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

1. P命名注入：需要在头文件中加入约束文件
2. C命名注入：需要在头文件中加入约束文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:c="http://www.springframework.org/schema/c"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

     <!-- P(属性: properties)命名空间 , 属性依然要设置set方法 -->
    <bean id="user" class="com.zyy.pojo.User" p:age="18" p:name="zyy"/>

     <!-- C(构造: Constructor)命名空间 , 属性依然要设置set方法 把有参构造器加上，这里也能知道，c 就是所谓的构造器注入！ -->
    <bean id="user2" class="com.zyy.pojo.User" c:age="18" c:name="zyy2"/>
</beans>
```

测试：

```java
    @Test
    public void test2() {
        ApplicationContext context = new ClassPathXmlApplicationContext("user-beans.xml");
        User user = context.getBean("user2", User.class);
        System.out.println(user.toString());
    }
```



## 6.4、bean的作用域

![image-20220504115307623](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220504115307623.png)

![图片](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7K5cyS8ZRTpajtSInicNHbMYfmmAQF8hrnicY49FRXEkR5xkxD5A4H5pVUia3mFhrDdh4gBt183EiaFaQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

1. 单例模式（Spring默认机制）

   当一个bean的作用域为Singleton，那么Spring IoC容器中只会存在一个共享的bean实例，并且所有对bean的请求，只要id与该bean定义相匹配，则只会返回bean的同一实例。Singleton是单例类型，就是在**创建起容器时就同时自动创建了一个bean的对象**，不管你是否使用，他都存在了，每次获取到的对象都是同一个对象。注意，Singleton作用域是Spring中的缺省作用域。要在XML中将bean定义成singleton，可以这样配置：

   ```xml
   <bean id="user2" class="com.zyy.pojo.User" c:age="18" c:name="zyy2" scope="singleton"/>
   ```

   测试：

   ```java
       @Test
       public void test2() {
           ApplicationContext context = new ClassPathXmlApplicationContext("user-beans.xml");
           User user = context.getBean("user2", User.class);
           User user2 = context.getBean("user2", User.class);
           System.out.println(user.hashCode());
           System.out.println(user2.hashCode());
           System.out.println(user == user2); //ture
       }
   ```

2. 原型模式：每次从容器中get的时候，都会产生一个新对象。

   当一个bean的作用域为Prototype，表示一个bean定义对应多个对象实例。Prototype作用域的bean会导致在每次对该bean请求（将其注入到另一个bean中，或者以程序的方式调用容器的getBean()方法）时都会创建一个新的bean实例。Prototype是原型类型，**它在我们创建容器的时候并没有实例化，而是当我们获取bean的时候才会去创建一个对象**，而且我们每次获取到的对象都不是同一个对象。根据经验，对有状态的bean应该使用prototype作用域，而对无状态的bean则应该使用singleton作用域。在XML中将bean定义成prototype，可以这样配置：

   ```xml
   <bean id="user2" class="com.zyy.pojo.User" c:age="18" c:name="zyy2" scope="prototype"/>
   ```

3. 其余的request、session、application这些个只能再web开发中使用到！

   1. request

      当一个bean的作用域为Request，表示在一次HTTP请求中，一个bean定义对应一个实例；即每个HTTP请求都会有各自的bean实例，它们依据某个bean定义创建而成。该作用域仅在基于web的Spring ApplicationContext情形下有效。考虑下面bean定义：

      ```xml
       <bean id="loginAction" class=cn.csdn.LoginAction" scope="request"/>
      ```

      针对每次HTTP请求，Spring容器会根据loginAction bean的定义创建一个全新的LoginAction bean实例，且该loginAction bean实例仅在当前HTTP request内有效，因此可以根据需要放心的更改所建实例的内部状态，而其他请求中根据loginAction bean定义创建的实例，将不会看到这些特定于某个请求的状态变化。当处理请求结束，request作用域的bean实例将被销毁。

   2. session

      当一个bean的作用域为Session，表示在一个HTTP Session中，一个bean定义对应一个实例。该作用域仅在基于web的Spring ApplicationContext情形下有效。考虑下面bean定义：

      ```xml
       <bean id="userPreferences" class="com.foo.UserPreferences" scope="session"/>
      ```

      针对某个HTTP Session，Spring容器会根据userPreferences bean定义创建一个全新的userPreferences bean实例，且该userPreferences bean仅在当前HTTP Session内有效。与request作用域一样，可以根据需要放心的更改所创建实例的内部状态，而别的HTTP Session中根据userPreferences创建的实例，将不会看到这些特定于某个HTTP Session的状态变化。当HTTP Session最终被废弃的时候，在该HTTP Session作用域内的bean也会被废弃掉。

# 7、bean的自动装配

- 自动装配是使用spring满足bean依赖的一种方法
- spring会在应用上下文中为某个bean寻找其依赖的bean



在spring中有三种装配的方式

1. 在xml中显式配置
2. 在java中显式配置
3. 隐式的bean发现机制和自动装配【重要】

Spring的自动装配需要从两个角度来实现，或者说是两个操作：

1. 组件扫描(component scanning)：spring会自动发现应用上下文中所创建的bean
2. 自动装配(autowiring)：spring自动满足bean之间的依赖，也就是我们说的IoC/DI

## 7.1、测试

环境搭建：一个人有两个宠物！

1、新建一个项目

2、新建两个实体类，Cat  Dog  都有一个叫的方法

Cat.java

```java
public class Cat {
    public void shout() {
        System.out.println("miao~");
    }
}
```

Dog.java

```java
public class Dog {
    public void shout() {
        System.out.println("wang~");
    }
}
```

3、新建一个用户类 People

People.java

```java
public class People {
    private String name;
    private Dog dog;
    private Cat cat;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Dog getDog() {
        return dog;
    }

    public void setDog(Dog dog) {
        this.dog = dog;
    }

    public Cat getCat() {
        return cat;
    }

    public void setCat(Cat cat) {
        this.cat = cat;
    }

    @Override
    public String toString() {
        return "People{" +
                "name='" + name + '\'' +
                ", dog=" + dog +
                ", cat=" + cat +
                '}';
    }
}
```

4、编写Spring配置文件

## 7.2、ByName自动装配

```xml
    <bean id="cat" class="com.zyy.pojo.Cat"/>
    <bean id="dog" class="com.zyy.pojo.Dog"/>

    <!--<bean id="people" class="com.zyy.pojo.People">
        <property name="name" value="zyy"/>
        <property name="dog" ref="dog"/>
        <property name="cat" ref="cat"/>
    </bean>-->

    <!--
     byName:会自动在容器上下本中查找，和自己对象set方法后面的值对应的beanId 如setDog 中的dog对应的beanId
     setDog setdog 都可以找到
     setDOG 就找不到了
     -->
    <bean id="people" class="com.zyy.pojo.People" autowire="byName">
        <property name="name" value="zyy"/>
    </bean>
```

## 7.3、ByType自动装配

```xml
    <bean id="cat" class="com.zyy.pojo.Cat"/>
    <bean id="dog" class="com.zyy.pojo.Dog"/>

    <!--
     byType:会自动在容器上下本中查找，和自己对象属性类型相同的bean
     -->
    <bean id="people" class="com.zyy.pojo.People" autowire="byType">
        <property name="name" value="zyy"/>
    </bean>
```

小结：

- byName的时候，需要保证所有的bean 的id唯一，并且这个bean需要和自动注入的属性的set方法的值一致！
- byType的时候，需要保证所有bean的class唯一，并且这个bean需要和自动注入的属性的类型一致！

## 7.4、使用注解实现自动装配

jdk1.5支持的注解，spring2.5就支持注解了！

The introduction of annotation-based configuration raised the question of whether this approach is “better” than XML.

要使用注解须知：

1. 导入约束，context约束
2. 配置注解的配置`<context:annotation-config/>`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>

</beans>
```

**@Autowired**

直接在属性上使用即可，也可以在set方法上使用！

使用Autowired我们可以不用编写set方法，前提是你这个自动装配的属性在IOC（Spring）容器中存在，且类型符合byType

拓展：

```xml
@Nullable 字段标记了这个注解，说明这个字段可以为NULL
```

```java
public @interface Autowired {
    boolean required() default true;
}
```



如果@Autowired自动装配的环境比较复杂，自动装配无法通过一个注解【@Autowired】完成的时候，我们可以使用@Qualifier(value = "***")去配置@Autowired的使用，指定一个唯一的bean对象注入！

```xml
    <bean id="cat1" class="com.zyy.pojo.Cat"/>
    <bean id="cat2" class="com.zyy.pojo.Cat"/>
    <bean id="dog1" class="com.zyy.pojo.Dog"/>
    <bean id="dog2" class="com.zyy.pojo.Dog"/>
    <bean id="people" class="com.zyy.pojo.People"/>
```

```java
public class People {
    private String name;
    /**
     * 如果显式定义了Autowired的required属性为false,说明这个对象可以为null，否则不允许为空
     * @Autowired是根据类型自动装配的，加上@Qualifier则可以根据byName的方式自动装配
     * @Qualifier不能单独使用
     */
    @Autowired(required = false)
    @Qualifier(value = "dog1")
    private Dog dog;
    @Autowired
    @Qualifier(value = "cat1")
    private Cat cat;
}
```

**@Resource注解**

```java
public class People {
    private String name;
    @Resource(name = "dog1")
    private Dog dog;
    @Resource(name = "cat1")
    private Cat cat;
}
```

小结：

@Resource和@Autowired的区别

- 都是用来自动装配的，都可以放到属性字段上
- @Autowired通过byType的方式实现(ps：只根据byType匹配，不会匹配byName)，而且必须要求这个对象存在！【常用】
- @Resource默认通过byName的方式实现，如果找不到名字，则通过byType实现，如果两个都找不到的情况就报错【常用】
- 执行顺序不同：@Autowired通过byType的方式实现，@Resource默认通过byName的方式实现



# 8、使用注解开发

在spring4之后，要使用注解开发，必须要保证aop的包导入了

![image-20220504214542633](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220504214542633.png)

使用注解需要导入context约束，增加注解的支持！

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>

    <!--
      指定要扫描的包，这个包下的注解就会生效
      -->
    <context:component-scan base-package="com.zyy.pojo"/>


</beans>
```



1. bean

2. 属性如何注入

   ```java
   @Component
   public class User {
       /**
        * 相当于
        *     <bean id="user" class="com.zyy.pojo.User">
        *         <property name="name" value="zyy02"/>
        *     </bean>
        */
   //    @Value("zyy02")
       public String name;
   
   
       @Value("zyy02")
       public void setName(String name) {
           this.name = name;
       }
   }
   ```

   

3. 衍生的注解

   @Component有几个衍生注解，我们在web开发中，会按照mvc三层架构分层

   - dao【@Repository】

   - service【@Service】

   - controller【@Controller】

     这四个注解功能都是一样的，都是代表某个类注册到spring，装配bean

4. 自动装配

   ```
   @Resource
   @Autowired
   @Qualifier
   上面有介绍，这里不赘述
   ```

5. 作用域

   ```java
   /**
    * @Component 等价于<bean id="user" class="com.zyy.pojo.User"/>
    *
    */
   @Component
   @Scope("prototype")
   public class User {
       /**
        * 相当于
        *     <bean id="user" class="com.zyy.pojo.User">
        *         <property name="name" value="zyy02"/>
        *     </bean>
        */
   //    @Value("zyy02")
       public String name;
   
   
       @Value("zyy02")
       public void setName(String name) {
           this.name = name;
       }
   }
   ```

   

6. 小结

   xml与注解：

   - xml更加万能，适用于任何场合，维护简单方便
   - 注解不是自己的类使用不了，维护相对复杂

   xml与注解最佳实践

   - xml用来管理bean

   - 注解只负责完成属性的注入

   - 我们在使用的过程中，需要注意一个问题：必须让注解生效，就需要开启注解的支持

     ```xml
         <context:annotation-config/>
     
         <!--
           指定要扫描的包，这个包下的注解就会生效
           -->
         <context:component-scan base-package="com.zyy"/>
     ```



# 9、使用java的方式配置spring

我们现在要完全不使用spring的xml配置了，全权交给java来做！

javaConfig是spring的一个子项目，在spring4之后，成了一个核心功能。

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
