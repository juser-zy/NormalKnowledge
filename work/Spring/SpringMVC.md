代码见：https://gitee.com/zhayuyao/spring-mvc


# 1、回顾MVC

## 1.1、什么是MVC

- MVC是模型（Model）、视图（View）、控制器（Controller）的简称，是一种软件设计规范
- 是将业务逻辑、数据、显示分离的方法来组织代码
- MVC主要作用是**降低了视图与业务逻辑间的双向耦合。**
- MVC不是一种设计模式，**MVC是一种架构模式**。当然不同的MVC存在差异

**Model（模型）：**数据模型，提供要展示的数据，因此包含数据和行为，可以认为是领域模型或JavaBean组件（包含数据和行为），不过现在一般都分离开来：Value Object（数据Dao） 和 服务层（行为Service）。也就是模型提供了模型数据查询和模型数据的状态更新等功能，包括数据和业务。

**View（视图）：**负责进行模型的展示，一般就是我们见到的用户界面，客户想看到的东西。

**Controller（控制器）：**接收用户请求，委托给模型进行处理（状态改变），处理完毕后把返回的模型数据返回给视图，由视图负责展示。也就是说控制器做了个调度员的工作。

**最典型的MVC就是JSP + servlet + javabean的模式。**

![image-20220515103413527](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220515103413527.png)

## 1.2、Model1时代

- 在web早期的开发中，通常采用的都是Model1。

- Model1中，主要分为两层，视图层和模型层。

  ![image-20220515103428877](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220515103428877.png)

Model1优点：架构简单，比较适合小型项目开发；

Model1缺点：JSP职责不单一，职责过重，不便于维护；

## 1.3、Model2时代

Model2把一个项目分成三部分，包括**视图、控制、模型。**

![image-20220515103546189](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220515103546189.png)

1. 用户发请求
2. Servlet接收请求数据，并调用对应的业务逻辑方法
3. 业务处理完毕，返回更新后的数据给servlet
4. servlet转向到JSP，由JSP来渲染页面
5. 响应给前端更新后的页面

**职责分析：**

**Controller：控制器**

1. 取得表单数据
2. 调用业务逻辑
3. 转向指定的页面

**Model：模型**

1. 业务逻辑
2. 保存数据的状态

**View：视图**

1. 显示页面



## 1.4、回顾Servlet

1. 新建一个maven工程当做付工程。导包

   ```xml
          <dependency>
               <groupId>junit</groupId>
               <artifactId>junit</artifactId>
               <version>4.12</version>
           </dependency>
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-webmvc</artifactId>
               <version>5.1.9.RELEASE</version>
           </dependency>
           <dependency>
               <groupId>javax.servlet</groupId>
               <artifactId>servlet-api</artifactId>
               <version>2.5</version>
           </dependency>
           <dependency>
               <groupId>javax.servlet.jsp</groupId>
               <artifactId>jsp-api</artifactId>
               <version>2.2</version>
           </dependency>
           <dependency>
               <groupId>javax.servlet</groupId>
               <artifactId>jstl</artifactId>
               <version>1.2</version>
           </dependency>
   ```

2. 新建一个module：springmvc-servlet，添加web app的支持

3. 导入servlet和jsp的jar依赖

```xml
<dependency>
   <groupId>javax.servlet</groupId>
   <artifactId>servlet-api</artifactId>
   <version>2.5</version>
</dependency>
<dependency>
   <groupId>javax.servlet.jsp</groupId>
   <artifactId>jsp-api</artifactId>
   <version>2.2</version>
</dependency>
```

4. 编写servlet类，用来出来用户的请求

   ```java
   public class HelloServlet extends HttpServlet {
   
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           String method = req.getParameter("method");
           if ("add".equals(method)) {
               req.getSession().setAttribute("msg", "执行了add方法");
           } else if ("update".equals(method)) {
               req.getSession().setAttribute("msg", "执行了update方法");
           } else {
               req.getSession().setAttribute("msg", "暂时无此方法");
           }
           req.getRequestDispatcher("/WEB-INF/jsp/hello.jsp").forward(req, resp);
       }
   
       @Override
       protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           this.doGet(req, resp);
       }
   }
   ```

5. 编写Hello.jsp ， WEB-INF下新增一个jsp文件夹，新建一个Hello.jsp

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
   ${msg}
   </body>
   </html>
   ```

6. web.xml中注册Servlet

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
   
       <servlet>
           <servlet-name>hello</servlet-name>
           <servlet-class>com.zyy.servlet.HelloServlet</servlet-class>
       </servlet>
       <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello</url-pattern>
       </servlet-mapping>
   
       <!--<welcome-file-list>
           <welcome-file></welcome-file>
       </welcome-file-list>-->
   
       <!--<session-config>
           <session-timeout>15</session-timeout>
       </session-config>-->
   </web-app>
   ```

7. 配置tomcat，启动测试

   ![image-20220515110910094](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220515110910094.png)

   ![image-20220515111009546](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220515111009546.png)

   **MVC框架要做哪些事情**

   1. 将url映射到java类或java类的方法 .
   2. 封装用户提交的数据 .
   3. 处理请求--调用相关的业务处理--封装响应数据 .
   4. 将响应的数据进行渲染 . jsp / html 等表示层数据 .



# 2、什么是SpringMVC

## 2.1、概述

==Spring MVC是Spring Framework的一部分，是基于Java实现MVC的轻量级Web框架。==

查看官方文档：https://docs.spring.io/spring/docs/5.2.0.RELEASE/spring-framework-reference/web.html#spring-web

**我们为什么要学习springmvc？**

 Spring MVC的特点：

1. 轻量级，简单易学
2. 高效 , 基于请求响应的MVC框架
3. 与Spring兼容性好，无缝结合
4. 约定优于配置
5. 功能强大：RESTful、数据验证、格式化、本地化、主题等
6. 简洁灵活

spring的web框架围绕DispatcherServlet【调度servlet】设计

**DispatcherServlet的作用是将请求分发到不同的处理器。从Spring 2.5开始，使用Java 5或者以上版本的用户可以采用基于注解形式进行开发，十分简洁；**

正因为SpringMVC好 , 简单 , 便捷 , 易学 , 天生和Spring无缝集成(使用SpringIoC和Aop) , 使用约定优于配置 . 能够进行简单的junit测试 . 支持Restful风格 .异常处理 , 本地化 , 国际化 , 数据验证 , 类型转换 , 拦截器 等等......所以我们要学习。

## 2.2、中心控制器

Spring的web框架围绕DispatcherServlet设计。DispatcherServlet的作用是将请求分分发到不同的处理器。从Spring2.5开始，开始java5或者以上版本的用户可以采用基于注解的controller声明方式。

Spring MVC框架像许多其他MVC框架一样, **以请求为驱动** , **围绕一个中心Servlet分派请求及提供其他功能**，**DispatcherServlet是一个实际的Servlet (它继承自HttpServlet 基类)**。

DispatcherServlet类图如下：

![image-20220516163849517](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220516163849517.png)

SpringMVC的原理如下图所示：

当发起请求时被前置的控制器拦截到请求，根据请求参数生成代理请求，找到请求对应的实际控制器，控制器处理请求，创建数据模型，访问数据库，将模型响应给中心控制器，控制器使用模型与视图渲染视图结果，将结果返回给中心控制器，再将结果返回给请求者。

![image-20220516164434657](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220516164434657.png)

## 2.3、SpringMVC执行原理

![image-20220516170509457](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220516170509457.png)

图为SpringMVC的一个较完整的流程图，实线表示SpringMVC框架提供的技术，不需要开发者实现，虚线表示需要开发者实现。

**简要分析执行流程**

1. DispatcherServlet表示**前置控制器**，是整个SpringMVC的控制中心，用户发出请求，DispatcherServlet接收请求并拦截请求

   我们假设请求的url为 : http://localhost:8080/SpringMVC/hello

   **如上url拆分成三部分：**

   http://localhost:8080 服务器域名

   SpringMVC部署在服务器上的web站点

   hello表示控制器

   通过分析，如上url表示为：请求位于服务器localhost:8080上的SpringMVC站点的hello控制器

2. HandlerMapping为**处理器映射器**。DispatcherServlet调用HandlerMapping，HandlerMapping根据请求Url找Handler

3. HandlerExecution表示具体的Handler，其主要作用是根据URL查找控制器，如上url被查找控制器为：hello

4. HandlerExecution将解析后的信息传递给DispatcherServlet，如解析控制器映射等

5. HandlerAdapter表示**处理器适配器**，其按照特定的规则去执行Handler

6. Handler让具体的Controller执行

7. Controller将具体的执行信息返回给HandlerAdapter，如ModelAndView

8. HandlerAdapter将视图逻辑名或模型传递给DispatcherServlet

9. DispatcherServlet调用**视图解析器**（ViewResolver）来解析HandlerAdapter传递的逻辑视图名

10. 视图解析器将解析的逻辑视图名传给DispatcherServlet

11. DispatcherServlet根据视图解析器解析的视图结果，调用具体的视图

12. 最终视图呈现给用户



在这里先听一遍原理，不理解没有关系，我们马上来写一个对应的代码实现大家就明白了，如果不明白，那就写10遍，没有笨人，只有懒人！

# 3、第一个mvc程序
## 3.1、配置版

![image-20220515224953813](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220515224953813.png)

1. 新建一个Moudle ， springmvc-02-hello ， 添加web的支持！

2. 确定导入了SpringMVC 的依赖！

3. 配置web.xml  ， 注册DispatcherServlet

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
   
       <!--1.注册DispatcherServlet-->
       <servlet>
           <servlet-name>springmvc</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <!--关联一个springmvc的配置文件:【servlet-name】-servlet.xml-->
           <init-param>
               <param-name>contextConfigLocation</param-name>
               <param-value>classpath:springmvc-servlet.xml</param-value>
           </init-param>
           <!--启动级别-1-->
           <load-on-startup>1</load-on-startup>
       </servlet>
   
       <!--/ 匹配所有的请求；（不包括.jsp）-->
       <!--/* 匹配所有的请求；（包括.jsp）-->
       <servlet-mapping>
           <servlet-name>springmvc</servlet-name>
           <url-pattern>/</url-pattern>
       </servlet-mapping>
   
   </web-app>
   ```

4. 编写SpringMVC 的 配置文件！名称：springmvc-servlet.xml  : [servletname]-servlet.xml

   添加 处理映射器

   添加 处理器适配器

   添加 视图解析器

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
   
       <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>
   
   
       <!--
       视图解析器:DispatcherServlet给他的ModelAndView
       1. 获取了ModelAndView的数据
       2. 解析ModelAndView视图的名字
       3. 拼接视图名字，找到对应的视图 /WEB-INF/jsp/hello.jsp
       4. 将数据渲染到这个视图上！
       -->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="InternalResourceViewResolver">
           <!--前缀-->
           <property name="prefix" value="/WEB-INF/jsp/"/>
           <!--后缀-->
           <property name="suffix" value=".jsp"/>
       </bean>
   
       <bean id="/hello" class="com.zyy.controller.HelloController"/>
   
   
   </beans>
   ```

5. 编写我们要操作业务Controller ，要么实现Controller接口，要么增加注解；需要返回一个ModelAndView，装数据，封视图；

   ```java
   public class HelloController implements Controller {
   
       public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
           //ModelAndView 模型和视图
           ModelAndView mv = new ModelAndView();
   
           //封装对象，放在ModelAndView中。Model
           mv.addObject("msg", "HelloSpringMVC!");
           //封装要跳转的视图，放在ModelAndView中
           //  /WEB-INF/jsp/hello.jsp
           mv.setViewName("hello");
           return mv;
       }
   
   }
   ```

6. 将自己的类交给SpringIOC容器，注册bean

   ```xml
   
       <bean id="/hello" class="com.zyy.controller.HelloController"/>
   ```

7. 写要跳转的jsp页面，显示ModelandView存放的数据，以及我们的正常页面；

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
   ${msg}
   </body>
   </html>
   ```

8. 配置tocmat，启动测试

   ![image-20220515224820194](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220515224820194.png)

   **可能遇到的问题：访问出现404，排查步骤：**

   1. 查看控制台输出，看一下是不是缺少了什么jar包。
   2. 如果jar包存在，显示无法输出，就在IDEA的项目发布中，添加lib依赖！
   3. 重启Tomcat 即可解决！

![image-20220516161838340](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220516161838340.png)



## 3.2、注解版

1. 新建一个Moudle，springmvc-03-annotation 。添加web支持！

   ![image-20220517161638628](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220517161638628.png)

2. 由于Maven可能存在资源过滤的问题，我们将配置完善

   ```xml
       <build>
           <resources>
               <resource>
                   <directory>src/main/java</directory>
                   <includes>
                       <include>**/*.properties</include>
                       <include>**/*.xml</include>
                   </includes>
                   <filtering>false</filtering>
               </resource>
               <resource>
                   <directory>src/main/resources</directory>
                   <includes>
                       <include>**/*.properties</include>
                       <include>**/*.xml</include>
                   </includes>
                   <filtering>false</filtering>
               </resource>
           </resources>
       </build>
   ```

3. 在pom.xml文件引入相关的依赖：主要有Spring框架核心库、Spring MVC、servlet , JSTL等。我们在父依赖中已经引入了！

4. 配置web.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
       <!--    注册DispatcherServlet-->
       <servlet>
           <servlet-name>springmvc</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <!--通过初始化参数指定SpringMVC配置文件的位置，进行关联-->
           <init-param>
               <param-name>contextConfigLocation</param-name>
               <param-value>classpath:springmvc-servlet.xml</param-value>
           </init-param>
           <!-- 启动顺序，数字越小，启动越早 -->
           <load-on-startup>1</load-on-startup>
       </servlet>
       <!--所有请求都会被springmvc拦截 -->
       <servlet-mapping>
           <servlet-name>springmvc</servlet-name>
           <url-pattern>/</url-pattern>
       </servlet-mapping>
   </web-app>
   ```

   - 注意web.xml版本问题，要最新版！
   - 注册DispatcherServlet
   - 关联SpringMVC的配置文件
   - 启动级别为1
   - 映射路径为 / 【不要用/*，会404】

   

   **/ 和 /\* 的区别：**

   - < url-pattern > / </ url-pattern > 不会匹配到.jsp， 只针对我们编写的请求；即：.jsp 不会进入spring的 DispatcherServlet类 
   - < url-pattern > /* </ url-pattern > 会匹配 *.jsp，会出现返回 jsp视图 时再次进入spring的DispatcherServlet 类，导致找不到对应的controller所以报404错

5. 添加SpringMVC配置文件

   springmvc-servlet.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:mvc="http://www.springframework.org/schema/mvc"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/context
          https://www.springframework.org/schema/context/spring-context.xsd
          http://www.springframework.org/schema/mvc
          https://www.springframework.org/schema/mvc/spring-mvc.xsd">
   
   
       <!--    自动扫描包，让指定包下的注解生效，有IOC容器统一管理-->
       <context:component-scan base-package="com.zyy.controller"/>
   
       <!--   让Spring MVC不处理静态资源 -->
       <mvc:default-servlet-handler/>
   
       <!--
      支持mvc注解驱动
          在spring中一般采用@RequestMapping注解来完成映射关系
          要想使@RequestMapping注解生效
          必须向上下文中注册DefaultAnnotationHandlerMapping
          和一个AnnotationMethodHandlerAdapter实例
          这两个实例分别在类级别和方法级别处理。
          而annotation-driven配置帮助我们自动完成上述两个实例的注入。
       -->
       <mvc:annotation-driven/>
   
       <!--
       视图解析器:DispatcherServlet给他的ModelAndView
       1. 获取了ModelAndView的数据
       2. 解析ModelAndView视图的名字
       3. 拼接视图名字，找到对应的视图 /WEB-INF/jsp/hello.jsp
       4. 将数据渲染到这个视图上！
       -->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
           <!--前缀-->
           <property name="prefix" value="/WEB-INF/jsp/"/>
           <!--后缀-->
           <property name="suffix" value=".jsp"/>
       </bean>
   
   </beans>
   ```

   在视图解析器中我们把所有的视图都存放在/WEB-INF/目录下，这样可以保证视图安全，因为这个目录下的文件，客户端不能直接访问。

   

   - 让IOC的注解生效
   - 静态资源过滤 ：HTML . JS . CSS . 图片 ， 视频 .....
   - MVC的注解驱动
   - 配置视图解析器

6. 创建Controller

   ```java
   @Controller
   public class HelloController {
   
       @RequestMapping("/h1")
       public String hello(Model model) {
           String str = "Hello, SpringMVC";
           //向模型中添加属性msg与值，可以在JSP页面中取出并渲染
           model.addAttribute("msg", str);
           //WEB-INF/jsp/hello.jsp
           return "hello";
       }
   }
   ```

   - @Controller是为了让Spring IOC容器初始化时自动扫描到
   - @RequestMapping是为了映射请求路径，这里因为只有方法上有映射所以访问时应该是/hello
   - 方法中声明Model类型的参数是为了把Action中的数据带到视图中
   - 方法返回的结果是视图的名称hello，加上配置文件中的前后缀变成WEB-INF/jsp/hello.jsp

7. 创建视图层hello.jsp

   在WEB-INF/ jsp目录中创建hello.jsp ， 视图可以直接取出并展示从Controller带回的信息

   可以通过EL表示取出Model中存放的值，或者对象；

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
   ${msg}
   </body>
   </html>
   ```

8. 配置tomcat，运行测试

   ![image-20220517161825470](C:/Users/ZHAYUYAO/AppData/Roaming/Typora/typora-user-images/image-20220517161825470.png)

使用springMVC必须配置的三大件：

**处理器映射器、处理器适配器、视图解析器**

通常，我们只需要**手动配置视图解析器**，而**处理器映射器**和**处理器适配器**只需要开启**注解驱动**即可，而省去了大段的xml配置



# 4、RestFul和控制器

## 4.1、控制器

- 控制器负责提供访问应用程序的行为，通常通过接口定义或注解定义两种方法实现
- 控制器负责解析用户的请求并将其转换为一个模型
- 在SpringMVC中的一个控制器类可以包含多个方法
- 在SpringMVC中对于控制器的配置方式有很多种



### 4.1.1、实现Controller接口

1. 新建一个Moudle，springmvc-04-controller 

2. 配置web.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
       <servlet>
           <servlet-name>springmvc</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <init-param>
               <param-name>contextConfigLocation</param-name>
               <param-value>classpath:springmvc-servlet.xml</param-value>
           </init-param>
           <load-on-startup>1</load-on-startup>
       </servlet>
       <servlet-mapping>
           <servlet-name>springmvc</servlet-name>
           <url-pattern>/</url-pattern>
       </servlet-mapping>
   </web-app>
   ```

3. 添加Spring配置文件springmvc-servlet.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd">
   
   
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
           <property name="prefix" value="/WEB-INF/jsp/"/>
           <property name="suffix" value=".jsp"/>
       </bean>
   
   </beans>
   ```

4. 编写前端test.jsp，注意在WEB-INF/jsp目录下编写，对应我们的视图解析器

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
   ${msg}
   </body>
   </html>
   ```

5. 添加Controller类

   ```java
   public class ControllerTest1 implements Controller {
       public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
           ModelAndView modelAndView = new ModelAndView();
           modelAndView.addObject("msg", "ControllerTest1");
           modelAndView.setViewName("test");
   
           return modelAndView;
       }
   }
   ```

6. 去Spring配置文件中注册请求的bean

   ```xml
   
       <bean id="/t1" class="com.zyy.controller.ControllerTest1"/>
   ```

7. 配置tomcat，测试

   ![image-20220518182302501](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220518182302501.png)

说明：

- 实现接口Controller定义控制器是较老的办法
- 缺点是：一个控制器中只有一个方法，如果要多个方法则需要定义多个Controller；定义的方式比较麻烦；

### 4.1.2、使用注解@Controller

@Controller注解类型用于声明Spring类的实例是一个控制器



1. 在上面的基础上，注释Spring配置文件中的bean，添加扫描，让注解生效

   （Spring可以使用扫描机制来找到应用程序中所有基于注解的控制器类，为了保证Spring能找到你的控制器，需要在配置文件中声明组件扫描。）

   ```xml
       <!--<bean id="/t1" class="com.zyy.controller.ControllerTest1"/>-->
   
       <!-- 自动扫描指定的包，下面所有注解类交给IOC容器管理 -->
       <context:component-scan base-package="com.zyy.controller"/>
   ```

2. 新增ControllerTest2.java

   ```java
   /**
    * @Controller注解的类会自动添加到Spring上下文中
    */
   @Controller
   public class ControllerTest2 {
       //映射访问路径
       @RequestMapping("/t2")
       public String test(Model model) {
           //Spring MVC会自动实例化一个Model对象用于向视图中传值
           model.addAttribute("msg", "ControllerTest2");
           //返回视图名
           return "test";
       }
   }
   ```

3. 启动tomcat，测试

   ![image-20220518183919825](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220518183919825.png)

4. ControllerTest2.java再新增一个方法

   ```java
       @RequestMapping("/t3")
       public String test3(Model model) {
           //Spring MVC会自动实例化一个Model对象用于向视图中传值
           model.addAttribute("msg", "ControllerTest2...test3");
           //返回视图名
           return "test";
       }
   ```

5. 测试

   ![image-20220518185134800](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220518185134800.png)

**可以发现，我们的两个请求都可以指向一个视图，但是页面结果的结果是不一样的，从这里可以看出视图是被复用的，而控制器与视图之间是弱偶合关系。**



**注解方式是平时使用的最多的方式！**



## 4.2、RequestMapping

- @RequestMapping注解用于映射url到控制器或者一个特定的处理方法。可用于类或者方法上。用于类，表示类中所有响应请求的方法都是以该地址作为父路径

- 新增ControllerTest3.java

  只注解在方法上面

  ```java
  @Controller
  public class ControllerTest3 {
      //映射访问路径
      @RequestMapping("/t1")
      public String test(Model model) {
          //Spring MVC会自动实例化一个Model对象用于向视图中传值
          model.addAttribute("msg", "ControllerTest3...test");
          //返回视图名
          return "test";
      }
  }
  ```

  访问路径：http://localhost:8080/t1

- 同时注解类与方法

  ```java
  @Controller
  @RequestMapping("/c3")
  public class ControllerTest3 {
      //映射访问路径
      @RequestMapping("/t1")
      public String test(Model model) {
          //Spring MVC会自动实例化一个Model对象用于向视图中传值
          model.addAttribute("msg", "ControllerTest3...test");
          //返回视图名
          return "test";
      }
  }
  ```

  访问路径：http://localhost:8080/c3/t1

## 4.3、RestFul风格

**概念**

Restful就是一个资源定位及资源操作的风格。不是标准也不是协议，只是一种风格。基于这个风格设计的软件可以更简洁，更有层次，更易于实现缓存等机制。

**功能**

资源：互联网所有的事物都可以被抽象为资源

资源操作：使用POST、DELETE、PUT、GET，使用不同方法对资源进行操作

分贝对应 添加、删除、修改、查询

**传统方式操作资源**  ：通过不同的参数来实现不同的效果，方法单一，post 和get

- http://127.0.0.1/item/queryItem.action?id=1 查询,GET
- http://127.0.0.1/item/saveItem.action 新增,POST
- http://127.0.0.1/item/updateItem.action 更新,POST
- http://127.0.0.1/item/deleteItem.action?id=1 删除,GET或POST

使用RestFul操作资源：可以通过不同的请求方式来实现不同的效果，如下：请求地址一样，但是功能可以不同

- http://127.0.0.1/item/1 查询,GET
- http://127.0.0.1/item 新增,POST
- http://127.0.0.1/item 更新,PUT
- http://127.0.0.1/item/1 删除,DELETE

**学习测试**

1. 新建一个类RestFulController.java

   ```java
   @Controller
   public class RestFulController {
   
       //http://localhost:8080/add?a=1&b=2
       @RequestMapping("/add")
       public String add(int a, int b, Model model) {
           int res = a + b;
           model.addAttribute("msg", "add结果为" + res);
           return "test";
       }
    }
   ```

2. 启动tomcat，访问测试

   http://localhost:8080/add?a=1&b=2

   ![image-20220518220855977](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220518220855977.png)

3. 新增一个restFul方式请求的方法

   ```java
       //http://localhost:8080/add/1/2
       @RequestMapping(value = "/add/{a}/{b}", method = RequestMethod.GET)
       public String add2(@PathVariable int a, @PathVariable int b, Model model) {
           int res = a + b;
           model.addAttribute("msg", "add2结果为" + res);
           return "test";
       }
   ```

4. 启动tomcat，访问测试

   http://localhost:8080/add/1/2

   ![image-20220518221050967](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220518221050967.png)

5. 用post方式请求，新增一个a.jsp

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
     <head>
       <title>$Title$</title>
     </head>
     <body>
     <form action="/add/1/2" method="post">
       <input type="submit"/>
     </form>
     </body>
   </html>
   ```

6. 添加一个post方式的方法

   ```java
       @RequestMapping(value = "/add/{a}/{b}", method = RequestMethod.POST)
       public String add3(@PathVariable int a, @PathVariable int b, Model model) {
           int res = a + b;
           model.addAttribute("msg", "add3结果为" + res);
           return "test";
       }
   ```

7. 也可以简写成，用@PostMapping注解  和6不能同时存在，不然两个有冲突，会报错

   ```java
       @PostMapping(value = "/add/{a}/{b}")
       public String add4(@PathVariable int a, @PathVariable int b, Model model) {
           int res = a + b;
           model.addAttribute("msg", "add4结果为" + res);
           return "test";
       }
   ```

8. 启动tomcat，验证

   http://localhost:8080/add/1/2

   ![image-20220518222134372](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220518222134372.png)

**小结**

SpringMVC的@RequestMapping 注解能够处理 HTTP 请求的方法, 比如 GET, PUT, POST, DELETE 以及 PATCH

**所有的地址栏请求默认都会是 HTTP GET 类型的。**

方法级别的注解变体有如下几个：组合注解

```
@GetMapping
@PostMapping
@PutMapping
@DeleteMapping
@PatchMapping
```

@GetMapping 是一个组合注解，平时使用的会比较多！

它所扮演的是 @RequestMapping(method =RequestMethod.GET) 的一个快捷方式。



**思考**

使用路径变量的好处？

- 使路径变得更加简洁
- 获得参数更加方便，框架会自动进行类型转换
- 通过路径变量的类型可以约束访问参数，如果类型不一样，则访问不到对应的请求方法，如这里访问是的路径是/commit/1/a，则路径与方法不匹配，而不会是参数转换失败



**拓展**

场景一：*我们都有过向别人（甚至可能向完全不会编程的人）提问及解释编程问题的经历，但是很多时候就在我们解释的过程中自己却想到了问题的解决方案，然后对方却一脸茫然。*

场景二：你的同行跑来问你一个问题，但是当他自己把问题说完，或说到一半的时候就想出答案走了，留下一脸茫然的你。

其实上面两种场景现象就是所谓的小黄鸭调试法（Rubber Duck Debuging），又称橡皮鸭调试法，它是我们软件工程中最常使用调试方法之一。

# 5、数据处理及跳转

## 5.1、ModelAndView

设置ModelAndView对象，根据view的名称，和视图解析器跳到指定的页面

页面： {视图解析器前缀} + viewName + {视图解析器后缀}

配置视图解析器

```xml
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
```

### 5.1.1、ServletAPI

通过设置ServletAPI , 不需要视图解析器

- 通过HttpServletResponse进行输出
- 通过HttpServletResponse实现重定向
- 通过HttpServletResponse实现转发

```java
@Controller
public class ModelTest1 {

    @RequestMapping(value = "/m1/t1")
    public String test1(HttpServletRequest request, HttpServletResponse response) {
        String id = request.getSession().getId();
        try {
            response.getWriter().print(id);
        } catch (IOException e) {
            e.printStackTrace();
        }
        return null;
    }
    

    @RequestMapping(value = "/m1/t12")
    public void test12(HttpServletResponse response) {
        try {
            //重定向
            response.sendRedirect("/index.jsp");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    
    @RequestMapping(value = "/m1/t13")
    public void test13(HttpServletRequest request, HttpServletResponse response) {
        request.setAttribute("msg", "ModelTest1...test13");
        try {
            //转发
            request.getRequestDispatcher("/WEB-INF/jsp/test.jsp").forward(request, response);
        } catch (ServletException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### 5.1.2、通过SpringMVC来实现转发和重定向 - 无需视图解析器

测试前，需要将视图解析器注释掉

```java
   @RequestMapping(value = "/m1/t2")
    public String test2(Model model) {
        model.addAttribute("msg", "ModelTest1...test2");
        return "/WEB-INF/jsp/test.jsp";
    }

    @RequestMapping(value = "/m1/t3")
    public String test3() {
        //重定向
        return "redirect:/index.jsp";
    }

    @RequestMapping(value = "/m1/t4")
    public String test4(Model model) {
        //转发
        model.addAttribute("msg", "ModelTest1...test4");
        return "forward:/WEB-INF/jsp/test.jsp";
    }
```

### 5.1.3、通过SpringMVC来实现转发和重定向 - 有视图解析器

重定向 , 不需要视图解析器 , 本质就是重新请求一个新地方嘛 , 所以注意路径问题

```java
    @RequestMapping(value = "/m1/t5")
    public String test5(Model model) {
        //转发
        model.addAttribute("msg", "ModelTest1...test5");
        return "test";
    }

    @RequestMapping(value = "/m1/t3")
    public String test3() {
        //重定向
        return "redirect:/index.jsp";
    }
```



## 5.2、数据处理

### 5.2.1、处理提交数据

1. 提交的域名称和处理方法的参数名一致

   ```java
   @Controller
   @RequestMapping("/u1")
   public class UserController {
   
       // http://localhost:8080/u1/t1?name=zyy
       @GetMapping("/t1")
       public String add(String name, Model model) {
           //1. 获取前端传的入参
           System.out.println("前端传的入参：" + name);
           //2. 将返回结果传输给前端 Model
           model.addAttribute("msg", name);
           //3. 视图跳转
           return "test";
       }
   }
   ```

   ![image-20220519161419967](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220519161419967.png)

2. 提交的域名称和处理方法的参数名不一致

   ```java
       @GetMapping("/t2")
       public String add2(@RequestParam("username") String name, Model model) {
           System.out.println("前端传的入参：" + name);
           model.addAttribute("msg", name);
           return "test";
       }
   ```

   ![image-20220519161516111](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220519161516111.png)

3. 提交是一个对象

   User.java

   ```java
   public class User {
       private int id;
       private String name;
       private int age;
   
       public User() {
       }
   
       public User(int id, String name, int age) {
           this.id = id;
           this.name = name;
           this.age = age;
       }
   
       public int getId() {
           return id;
       }
   
       public void setId(int id) {
           this.id = id;
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
                   "id=" + id +
                   ", name='" + name + '\'' +
                   ", age=" + age +
                   '}';
       }
   }
   ```

   新增方法

   ```java
       // http://localhost:8080/u1/t3?name=zyy&id=1&age=14
       @GetMapping("/t3")
       public String add3(User user, Model model) {
           System.out.println("前端传的入参：" + user.toString());
           model.addAttribute("msg", user.toString());
           return "test";
       }
   ```

   ![image-20220519161600031](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220519161600031.png)



### 5.2.2、数据显示到前端

1. 通过ModelAndView

   ```java
   public class HelloController implements Controller {
   
       public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
           //ModelAndView 模型和视图
           ModelAndView mv = new ModelAndView();
   
           //封装对象，放在ModelAndView中。Model
           mv.addObject("msg", "HelloSpringMVC!");
           //封装要跳转的视图，放在ModelAndView中
           //  /WEB-INF/jsp/hello.jsp
           mv.setViewName("test");
           return mv;
       }
   }
   ```

   ```xml
       <bean class="com.zyy.controller.HelloController" id="/h1"/>
   ```

2. 通过ModelMap

3. 通过Model

   ```java
   @Controller
   @RequestMapping("/c3")
   public class ControllerTest3 {
       //映射访问路径
       @RequestMapping("/t1")
       public String test(Model model) {
           //Spring MVC会自动实例化一个Model对象用于向视图中传值
           model.addAttribute("msg", "ControllerTest3...test");
           //返回视图名
           return "test";
       }
   
       @RequestMapping("/t2")
       public String test2(ModelMap modelMap) {
           //Spring MVC会自动实例化一个Model对象用于向视图中传值
           modelMap.addAttribute("msg", "ControllerTest3...test2");
           //返回视图名
           return "test";
       }
   }
   ```



**对比**

就对于新手而言简单来说使用区别就是：

```
Model 只有寥寥几个方法只适合用于储存数据，简化了新手对于Model对象的操作和理解；

ModelMap 继承了 LinkedMap ，除了实现了自身的一些方法，同样的继承 LinkedMap 的方法和特性；

ModelAndView 可以在储存数据的同时，可以进行设置返回的逻辑视图，进行控制展示层的跳转。
```



**请使用80%的时间打好扎实的基础，剩下18%的时间研究框架，2%的时间去学点英文，框架的官方文档永远是最好的教程。**



## 5.3、乱码问题

1. 新建一个form.jsp

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
   <form action="/e1/t1" method="post">
       <input type="text" name="name"/>
       <input type="submit"/>
   </form>
   </body>
   </html>
   ```

2. 后端新建对应的处理类

   ```java
   @Controller
   public class EncodingController {
      /* @GetMapping("/e1/t1")
       public String test(String name, Model model) {
           model.addAttribute("msg", name);
           return "test";
       }*/
   
       @PostMapping("/e1/t1")
       public String test(String name, Model model) {
           model.addAttribute("msg", name);
           return "test";
       }
   }
   ```

3. 启动tomcat，输入中文，发现乱码（ps:jsp改成get请求，对应接口也改成get，发现中文可以正常展示，但是post方式不行）

   ![image-20220519173325718](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220519173325718.png)



### 5.3.1、自定义过滤器

```java
public class EncodingFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {

        System.out.println("===doFilter======");
        request.setCharacterEncoding("utf-8");
        response.setCharacterEncoding("utf-8");
        chain.doFilter(request, response);
    }

    @Override
    public void destroy() {

    }
}
```

web.xml中注册过滤器

```xml
    <!--    自定义过滤器-->
    <filter>
        <filter-name>encodingFilter</filter-name>
        <filter-class>com.zyy.filter.EncodingFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>encodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```



### 5.3.2、SpringMVC自带的过滤器

SpringMVC给我们提供了一个过滤器 , 可以在web.xml中配置。修改了xml文件需要重启服务器！

```xml
    <filter>
        <filter-name>encoding</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encoding</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```



### 5.3.3、其他方式

1. 修改tomcat配置文件 ：设置编码！

   ```xml
   <Connector URIEncoding="utf-8" port="8080" protocol="HTTP/1.1"
             connectionTimeout="20000"
             redirectPort="8443" />
   ```

2. 更加强大的自定义过滤器

   ```java
   import javax.servlet.*;
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletRequestWrapper;
   import javax.servlet.http.HttpServletResponse;
   import java.io.IOException;
   import java.io.UnsupportedEncodingException;
   import java.util.Map;
   
   /**
    * 解决get和post请求 全部乱码的过滤器
    */
   public class GenericEncodingFilter implements Filter {
   
       @Override
       public void destroy() {
       }
   
       @Override
       public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
           //处理response的字符编码
           HttpServletResponse myResponse=(HttpServletResponse) response;
           myResponse.setContentType("text/html;charset=UTF-8");
   
           // 转型为与协议相关对象
           HttpServletRequest httpServletRequest = (HttpServletRequest) request;
           // 对request包装增强
           HttpServletRequest myrequest = new MyRequest(httpServletRequest);
           chain.doFilter(myrequest, response);
       }
   
       @Override
       public void init(FilterConfig filterConfig) throws ServletException {
       }
   
   }
   
   //自定义request对象，HttpServletRequest的包装类
   class MyRequest extends HttpServletRequestWrapper {
   
       private HttpServletRequest request;
       //是否编码的标记
       private boolean hasEncode;
       //定义一个可以传入HttpServletRequest对象的构造函数，以便对其进行装饰
       public MyRequest(HttpServletRequest request) {
           super(request);// super必须写
           this.request = request;
       }
   
       // 对需要增强方法 进行覆盖
       @Override
       public Map getParameterMap() {
           // 先获得请求方式
           String method = request.getMethod();
           if (method.equalsIgnoreCase("post")) {
               // post请求
               try {
                   // 处理post乱码
                   request.setCharacterEncoding("utf-8");
                   return request.getParameterMap();
               } catch (UnsupportedEncodingException e) {
                   e.printStackTrace();
               }
           } else if (method.equalsIgnoreCase("get")) {
               // get请求
               Map<String, String[]> parameterMap = request.getParameterMap();
               if (!hasEncode) { // 确保get手动编码逻辑只运行一次
                   for (String parameterName : parameterMap.keySet()) {
                       String[] values = parameterMap.get(parameterName);
                       if (values != null) {
                           for (int i = 0; i < values.length; i++) {
                               try {
                                   // 处理get乱码
                                   values[i] = new String(values[i]
                                           .getBytes("ISO-8859-1"), "utf-8");
                               } catch (UnsupportedEncodingException e) {
                                   e.printStackTrace();
                               }
                           }
                       }
                   }
                   hasEncode = true;
               }
               return parameterMap;
           }
           return super.getParameterMap();
       }
   
       //取一个值
       @Override
       public String getParameter(String name) {
           Map<String, String[]> parameterMap = getParameterMap();
           String[] values = parameterMap.get(name);
           if (values == null) {
               return null;
           }
           return values[0]; // 取回参数的第一个值
       }
   
       //取所有值
       @Override
       public String[] getParameterValues(String name) {
           Map<String, String[]> parameterMap = getParameterMap();
           String[] values = parameterMap.get(name);
           return values;
       }
   }
   ```

   注册过滤器

   ```xml
       <filter>
           <filter-name>genericEncodingFilter</filter-name>
           <filter-class>com.zyy.filter.GenericEncodingFilter</filter-class>
       </filter>
       <filter-mapping>
           <filter-name>genericEncodingFilter</filter-name>
           <url-pattern>/*</url-pattern>
       </filter-mapping>
   ```

   这个也是我在网上找的一些大神写的，一般情况下，SpringMVC默认的乱码处理就已经能够很好的解决了！

   乱码问题，需要平时多注意，在尽可能能设置编码的地方，都设置为统一编码 UTF-8！



# 6、json交互处理

前后端分离时代

前端：独立部署，负责渲染后端数据

后端：部署后端，提交接口，提供数据

## 6.1、什么是JSON

- JSON(JavaScript Object Notation, JS 对象标记) 是一种轻量级的数据交换格式，目前使用特别广泛
- 采用完全独立于编程语言的**文本格式**来存储和表示数据
- 简洁和清晰的层次结构使得 JSON 成为理想的数据交换语言
- 易于人阅读和编写，同时也易于机器解析和生成，并有效地提升网络传输效率



在 JavaScript 语言中，一切都是对象。因此，任何JavaScript 支持的类型都可以通过 JSON 来表示，例如字符串、数字、对象、数组等。看看他的要求和语法格式：

- 对象表示为键值对，数据由逗号分隔
- 花括号保存对象
- 方括号保存数组



**JSON 键值对**是用来保存 JavaScript 对象的一种方式，和 JavaScript 对象的写法也大同小异，键/值对组合中的键名写在前面并用双引号 "" 包裹，使用冒号 : 分隔，然后紧接着值：

```json
{"name":"土土","age":12,"sex":"男"}
```



很多人搞不清楚 JSON 和 JavaScript 对象的关系，甚至连谁是谁都不清楚。其实，可以这么理解：

JSON 是 JavaScript 对象的字符串表示法，它使用文本表示一个 JS 对象的信息，本质是一个字符串

```
var obj = {a: 'Hello', b: 'World'}; //这是一个对象，注意键名也是可以使用引号包裹的

var json = '{"a": "Hello", "b": "World"}'; //这是一个 JSON 字符串，本质是一个字符串
```



**JSON 和 JavaScript 对象互转**

要实现从JSON字符串转换为JavaScript 对象，使用 JSON.parse() 方法

要实现从JavaScript 对象转换为JSON字符串，使用 JSON.stringify() 方法

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript">
        // js对象
        const user = {
            name: "土土",
            age: 12,
            sex: "男"
        }
        console.log(user)
        console.log("=======")
        // js对象 转 json对象
        const json = JSON.stringify(user)
        console.log(json)
        console.log("=======")
        // json对象 转 js对象
        const obj = JSON.parse(json);
        console.log(obj)
    </script>
</head>
<body>

</body>
</html>
```

![image-20220520103130943](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220520103130943.png)



## 6.2、Controller返回json数据

### 6.2.1、Jackson

Jackson应该是目前比较好的json解析工具了

当然工具不止这一个，比如还有阿里巴巴的 fastjson 等等。

我们这里使用Jackson

1. 导包

   ```xml
           <!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind -->
           <dependency>
               <groupId>com.fasterxml.jackson.core</groupId>
               <artifactId>jackson-databind</artifactId>
               <version>2.13.3</version>
           </dependency>
   
           <!-- 实体类用到，这里一起导入 -->
           <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
           <dependency>
               <groupId>org.projectlombok</groupId>
               <artifactId>lombok</artifactId>
               <version>1.18.24</version>
           </dependency>
   ```

2. 配置SpringMVC需要的配置

   web.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
       <servlet>
           <servlet-name>springmvc</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <init-param>
               <param-name>contextConfigLocation</param-name>
               <param-value>classpath:springmvc-servlet.xml</param-value>
           </init-param>
           <load-on-startup>1</load-on-startup>
       </servlet>
       <servlet-mapping>
           <servlet-name>springmvc</servlet-name>
           <url-pattern>/</url-pattern>
       </servlet-mapping>
   </web-app>
   ```

3. springmvc-servlet.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:mvc="http://www.springframework.org/schema/mvc"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/mvc
          https://www.springframework.org/schema/mvc/spring-mvc.xsd
          http://www.springframework.org/schema/context
          https://www.springframework.org/schema/context/spring-context.xsd">
   
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
           <property name="prefix" value="/WEB-INF/jsp/"/>
           <property name="suffix" value=".jsp"/>
       </bean>
   
       <context:component-scan base-package="com.zyy.controller"/>
   
   </beans>
   ```

4. 实体类

   ```java
   //需要导入lombok包
   @Data
   @AllArgsConstructor
   @NoArgsConstructor
   public class User {
       private int id;
       private String name;
       private int age;
   }
   ```

5. controller

   ```java
   @Controller
   public class UserController {
       @RequestMapping(value = "/user/j1")
       //加了@ResponseBody就不会走视图解析器，直接返回一个字符串
       @ResponseBody
       public String json1() throws JsonProcessingException {
           //创建一个jackson的对象映射器，用来解析数据
           ObjectMapper objectMapper = new ObjectMapper();
           //创建一个对象
           User user = new User(1, "啊哈哈", 18);
           //将我们的对象解析成为json格式
           String str = objectMapper.writeValueAsString(user);
           //由于@ResponseBody注解，这里会将str转成json格式返回；十分方便
           return str;
       }
   }
   ```

6. 配置tomcat，启动测试

   ![image-20220520144342193](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220520144342193.png)



发现有乱码问题，我们可以通过@RequestMaping的produces属性来实现，修改下代码

```java
    //produces = MediaType.APPLICATION_JSON_UTF8_VALUE 可以解析乱码问题，但是每个接口加这个太复杂，所以一般不用
    @RequestMapping(value = "/user/j1", produces = MediaType.APPLICATION_JSON_UTF8_VALUE)
    //加了@ResponseBody就不会走视图解析器，直接返回一个字符串
    @ResponseBody
    public String json1() throws JsonProcessingException {
        //创建一个jackson的对象映射器，用来解析数据
        ObjectMapper objectMapper = new ObjectMapper();
        //创建一个对象
        User user = new User(1, "啊哈哈", 18);
        //将我们的对象解析成为json格式
        String str = objectMapper.writeValueAsString(user);
        //由于@ResponseBody注解，这里会将str转成json格式返回；十分方便
        return str;
    }
```

再次启动，乱码解决！



**乱码统一解决**

上一种方法比较麻烦，如果项目中有许多请求则每一个都要添加，可以通过Spring配置统一指定，这样就不用每次都去处理了！

我们可以在springmvc的配置文件上添加一段消息StringHttpMessageConverter转换配置！

```xml
    <mvc:annotation-driven>
        <mvc:message-converters register-defaults="true">
            <bean class="org.springframework.http.converter.StringHttpMessageConverter">
                <constructor-arg value="UTF-8"/>
            </bean>
            <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
                <property name="objectMapper">
                    <bean class="org.springframework.http.converter.json.Jackson2ObjectMapperFactoryBean">
                        <property name="failOnEmptyBeans" value="false"/>
                    </bean>
                </property>
            </bean>
        </mvc:message-converters>
    </mvc:annotation-driven>
```



**返回json字符串统一解决**

在类上直接使用 **@RestController** ，这样子，里面所有的方法都只会返回 json 字符串了，不用再每一个都添加@ResponseBody ！我们在前后端分离开发中，一般都使用 @RestController ，十分便捷！

```java
@RestController
public class UserController {
    @RequestMapping(value = "/user/j1")
    public String json1() throws JsonProcessingException {
        //创建一个jackson的对象映射器，用来解析数据
        ObjectMapper objectMapper = new ObjectMapper();
        //创建一个对象
        User user = new User(1, "啊哈哈", 18);
        //将我们的对象解析成为json格式
        String str = objectMapper.writeValueAsString(user);
        //由于@ResponseBody注解，这里会将str转成json格式返回；十分方便
        return str;
    }
    
    //测试集合输出
    @RequestMapping(value = "/user/j2")
    public String json2() throws JsonProcessingException {
        ObjectMapper objectMapper = new ObjectMapper();

        List<User> userList = new ArrayList<User>();
        User user = new User(1, "啊哈哈", 18);
        User user2 = new User(2, "啊哈哈2", 18);
        User user3 = new User(3, "啊哈哈3", 18);
        User user4 = new User(4, "啊哈哈4", 18);

        userList.add(user);
        userList.add(user2);
        userList.add(user3);
        userList.add(user4);

        String s = objectMapper.writeValueAsString(userList);
        return s;
    }
} 
```

输出时间对象

```java
    @RequestMapping(value = "/user/j3")
    public String json3() throws JsonProcessingException {
        ObjectMapper objectMapper = new ObjectMapper();

        Date date = new Date();

        String s = objectMapper.writeValueAsString(date);
        return s;
    }
```

![image-20220520145302252](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220520145302252.png)

- 默认日期格式会变成一个数字，是1970年1月1日到当前日期的毫秒数！
- Jackson 默认是会把时间转成timestamps形式



**解决方案1：先格式化好，再转为json返回**

```java
    @RequestMapping(value = "/user/j4")
    public String json4() throws JsonProcessingException {
        ObjectMapper objectMapper = new ObjectMapper();

        Date date = new Date();
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-mm-dd hh:MM:ss");
        String s = objectMapper.writeValueAsString(sdf.format(date));
        return s;
    }
```

**解决方案2：取消timestamps形式 ， 自定义时间格式**

```java
    @RequestMapping(value = "/user/j5")
    public String json5() {
        return JsonUtil.getJson(new Date());
    }
```

格式化方法这里我们提取一个工具类出来

```java
public class JsonUtil {

    public static String getJson(Object object) {
        return getJson(object, "yyyy-MM-dd HH:mm:ss");
    }

    public static String getJson(Object object, String dateFormat) {

        ObjectMapper objectMapper = new ObjectMapper();

        //不使用时间戳的方式
        objectMapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS, false);

        //自定义日期格式对象
        SimpleDateFormat sdf = new SimpleDateFormat(dateFormat);
        objectMapper.setDateFormat(sdf);

        try {
            return objectMapper.writeValueAsString(object);
        } catch (JsonProcessingException e) {
            e.printStackTrace();
        }

        return null;
    }
}
```



### 6.2.2、FastJson

fastjson.jar是阿里开发的一款专门用于Java开发的包，可以方便的实现json对象与JavaBean对象的转换，实现JavaBean对象与json字符串的转换，实现json对象与json字符串的转换。实现json的转换方法很多，最后的实现结果都是一样的。

导包

```xml
        <!-- https://mvnrepository.com/artifact/com.alibaba/fastjson -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.78</version>
        </dependency>
```

fastjson 三个主要的类：

- JSONObject 代表json对象
  - JSONObject实现了Map接口, 猜想 JSONObject底层操作是由Map实现的
  - JSONObject对应json对象，通过各种形式的get()方法可以获取json对象中的数据，也可利用诸如size()，isEmpty()等方法获取"键：值"对的个数和判断是否为空。其本质是通过实现Map接口并调用接口中的方法完成的

- JSONArray 代表json对象数组
  - 内部是有List接口中的方法来完成操作的

- JSON代表JSONObject 和JSONArray 的转化
  - JSON类源码分析与使用
  - 仔细观察这些方法，主要是实现json对象，json对象数组，javabean对象，json字符串之间的相互转化

![image-20220520152646273](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220520152646273.png)

![image-20220520152711619](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220520152711619.png)

**代码测试，我们新建一个FastJsonDemo 类**

```java
import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.JSONObject;
import com.zyy.pojo.User;

import java.util.ArrayList;
import java.util.List;

public class FastJsonDemo {
    public static void main(String[] args) {
        //创建一个对象
        User user1 = new User(1, "秦疆1号", 18);
        User user2 = new User(2, "秦疆2号", 18);
        User user3 = new User(3, "秦疆3号", 18);
        User user4 = new User(4, "秦疆4号", 18);
        List<User> list = new ArrayList<User>();
        list.add(user1);
        list.add(user2);
        list.add(user3);
        list.add(user4);

        System.out.println("*******Java对象 转 JSON字符串*******");
        String str1 = JSON.toJSONString(list);
        System.out.println("JSON.toJSONString(list)==>" + str1);
        String str2 = JSON.toJSONString(user1);
        System.out.println("JSON.toJSONString(user1)==>" + str2);

        System.out.println("****** JSON字符串 转 Java对象*******");
        User jp_user1 = JSON.parseObject(str2, User.class);
        System.out.println("JSON.parseObject(str2,User.class)==>" + jp_user1);

        System.out.println("****** Java对象 转 JSON对象 ******");
        JSONObject jsonObject1 = (JSONObject) JSON.toJSON(user2);
        System.out.println("(JSONObject) JSON.toJSON(user2)==>" + jsonObject1.getString("name"));

        System.out.println("****** JSON对象 转 Java对象 ******");
        User to_java_user = JSON.toJavaObject(jsonObject1, User.class);
        System.out.println("JSON.toJavaObject(jsonObject1, User.class)==>" + to_java_user);
    }
}
```

这种工具类，我们只需要掌握使用就好了，在使用的时候在根据具体的业务去找对应的实现。和以前的commons-io那种工具包一样，拿来用就好了！



# 7、整合SSM

> 环境要求

代码见：https://gitee.com/zhayuyao/ssmbuild

环境：

- IDEA
- MySQL 5.7.19
- Tomcat 9
- Maven 3.6

要求:

- 需要熟练掌握MySQL数据库，Spring，JavaWeb及MyBatis知识，简单的前端知识



> 数据库环境

创建一个存放书籍数据的数据库表

```sql
CREATE DATABASE `ssmbuild`;

USE `ssmbuild`;

DROP TABLE IF EXISTS `books`;

CREATE TABLE `books` (
`bookID` INT(10) NOT NULL AUTO_INCREMENT COMMENT '书id',
`bookName` VARCHAR(100) NOT NULL COMMENT '书名',
`bookCounts` INT(11) NOT NULL COMMENT '数量',
`detail` VARCHAR(200) NOT NULL COMMENT '描述',
KEY `bookID` (`bookID`)
) ENGINE=INNODB DEFAULT CHARSET=utf8

INSERT  INTO `books`(`bookID`,`bookName`,`bookCounts`,`detail`)VALUES
(1,'Java',1,'从入门到放弃'),
(2,'MySQL',10,'从删库到跑路'),
(3,'Linux',5,'从进门到进牢');
```



> 基本环境搭建

1. 新建一Maven项目！ssmbuild ， 添加web的支持

2. 导入相关的pom依赖

   ```xml
       <!--    依赖-->
       <dependencies>
           <!--Junit-->
           <dependency>
               <groupId>junit</groupId>
               <artifactId>junit</artifactId>
               <version>4.12</version>
           </dependency>
           <!--数据库驱动-->
           <dependency>
               <groupId>mysql</groupId>
               <artifactId>mysql-connector-java</artifactId>
               <version>5.1.47</version>
           </dependency>
           <!-- 数据库连接池 -->
           <dependency>
               <groupId>com.mchange</groupId>
               <artifactId>c3p0</artifactId>
               <version>0.9.5.5</version>
           </dependency>
   
           <!--Servlet - JSP -->
           <dependency>
               <groupId>javax.servlet</groupId>
               <artifactId>servlet-api</artifactId>
               <version>2.5</version>
           </dependency>
           <dependency>
               <groupId>javax.servlet.jsp</groupId>
               <artifactId>jsp-api</artifactId>
               <version>2.2</version>
           </dependency>
           <dependency>
               <groupId>javax.servlet</groupId>
               <artifactId>jstl</artifactId>
               <version>1.2</version>
           </dependency>
   
           <!--Mybatis-->
           <dependency>
               <groupId>org.mybatis</groupId>
               <artifactId>mybatis</artifactId>
               <version>3.5.9</version>
           </dependency>
           <dependency>
               <groupId>org.mybatis</groupId>
               <artifactId>mybatis-spring</artifactId>
               <version>2.0.6</version>
           </dependency>
   
           <!--Spring-->
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-webmvc</artifactId>
               <version>5.3.19</version>
           </dependency>
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-jdbc</artifactId>
               <version>5.3.19</version>
           </dependency>
           <dependency>
               <groupId>org.projectlombok</groupId>
               <artifactId>lombok</artifactId>
               <version>1.18.24</version>
           </dependency>
       </dependencies>
   ```

3. maven资源过滤设置

   ```xml
       <!--    静态资源问题-->
       <build>
           <resources>
               <resource>
                   <directory>src/main/java</directory>
                   <includes>
                       <include>**/*.properties</include>
                       <include>**/*.xml</include>
                   </includes>
                   <filtering>false</filtering>
               </resource>
               <resource>
                   <directory>src/main/resources</directory>
                   <includes>
                       <include>**/*.properties</include>
                       <include>**/*.xml</include>
                   </includes>
                   <filtering>false</filtering>
               </resource>
           </resources>
       </build>
   ```

4. 建立基本结构和配置框架

   - com.zyy.pojo

   - com.zyy.mapper

   - com.zyy.service

   - com.zyy.controller

   - mybatis-config.xml

     ```xml
     <?xml version="1.0" encoding="UTF-8" ?>
     <!DOCTYPE configuration
             PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
             "http://mybatis.org/dtd/mybatis-3-config.dtd">
     <configuration>
     
     </configuration>
     ```

   - applicationContext.xml

     ```xml
     <?xml version="1.0" encoding="UTF-8"?>
     <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd">
         
     </beans>
     ```

     

> Mybatis层编写

1. 数据库配置文件 database.properties

```properties
   jdbc.driver=com.mysql.jdbc.Driver
   jdbc.url=jdbc:mysql://localhost:3306/ssmbuild?useSSL=false&useUnicode=true&characterEncoding=utf8
   jdbc.username=root
   jdbc.password=123456
```

2. idea关联数据库

   ![image-20220521164441270](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220521164441270.png)

3. 编写mybatis的核心配置文件 mybatis-config.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
       <mappers>
           <mapper resource="com/zyy/mapper/BookMapper.xml"/>
       </mappers>
   
   </configuration>
   ```

4. 编写数据库表对应的实体类 Books 

   这里用到lombok插件

   ```java
   @Data
   @NoArgsConstructor
   @AllArgsConstructor
   public class Books {
       private int bookID;
       private String bookName;
       private String bookCounts;
       private String detail;
   }
   ```

5. 编写dao层的mapper接口

   ```java
   public interface BookMapper {
       //增加一本书
       int addBook(Books book);
   
       //删除一本书
       int deleteBook(@Param("bookID") int id);
   
       //更新一本书
       int updateBook(Books book);
   
       //查询一本书
       Books queryBookById(@Param("bookID") int id);
   
       //查询全部的书
       List<Books> queryAllBook();
   
   
       //根据书名模糊查询
       List<Books> queryBooksByName(@Param("bookName") String bookName);
   }
   
   ```

6. 编写接口对应的Mapper.xml文件（BookMapper.xml），需要导入mybatis的包

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="com.zyy.mapper.BookMapper">
   
       <insert id="addBook" parameterType="com.zyy.pojo.Books">
         insert into ssmbuild.books(bookName,bookCounts,detail)
         values (#{bookName},#{bookCounts},#{detail})
       </insert>
   
       <update id="updateBook" parameterType="com.zyy.pojo.Books">
         update ssmbuild.books set bookName=#{bookName},bookCounts=#{bookCounts},detail=#{detail}
         where bookID=#{bookID}
       </update>
   
       <delete id="deleteBook" parameterType="int">
         delete from ssmbuild.books where bookID=#{bookID}
       </delete>
   
       <select id="queryBookById" parameterType="int" resultType="com.zyy.pojo.Books">
         select bookID,bookName,bookCounts,detail from ssmbuild.books where bookID=#{bookID}
       </select>
   
       <select id="queryAllBook" resultType="com.zyy.pojo.Books">
         select bookID,bookName,bookCounts,detail from ssmbuild.books
       </select>
       
       <select id="queryBooksByName" resultType="com.zyy.pojo.Books" parameterType="string">
         select bookID,bookName,bookCounts,detail from ssmbuild.books where bookName like #{bookName}
       </select>
   </mapper>
   ```

7. 编写service层的接口和实现类

   BookService.java

   ```java
   public interface BookService {
       //增加一本书
       int addBook(Books book);
   
       //删除一本书
       int deleteBook(int id);
   
       //更新一本书
       int updateBook(Books books);
   
       //查询一本书
       Books queryBookById(int id);
   
       //查询全部的书
       List<Books> queryAllBook();
   
       //根据书名模糊查询
       List<Books> queryBooksByName(String bookName);
   }
   ```
   
   BookServiceImpl.java
   
   ```java
   public class BookServiceImpl implements BookService {
   
       private BookMapper bookMapper;
   
       public void setBookMapper(BookMapper bookMapper) {
           this.bookMapper = bookMapper;
       }
   
       public int addBook(Books book) {
           return bookMapper.addBook(book);
       }
   
       public int deleteBook(int id) {
           return bookMapper.deleteBook(id);
       }
   
       public int updateBook(Books book) {
           return bookMapper.updateBook(book);
       }
   
       public Books queryBookById(int id) {
           return bookMapper.queryBookById(id);
       }
   
       public List<Books> queryAllBook() {
           return bookMapper.queryAllBook();
       }
   
       public List<Books> queryBooksByName(String bookName) {
           bookName = "%" + bookName + "%";
           return bookMapper.queryBooksByName(bookName);
       }
   }
   ```



> Spring层

1. 配置spring整合mybatis，我们这里数据源使用c3p0连接池

2. 我们去编写spring整合mybatis的相关的配置文件 spring-dao.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
   
       <!--1.关联数据库配置文件-->
       <context:property-placeholder location="classpath:database.properties"/>
       <!--2.连接池
           dbcp 半自动化，不能自动连接
           c3p0 自动化操作（自动化的加载配置文件，并且可以自动设置到对象中）
           druid
           hikari
       -->
       <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
           <!-- 配置连接池属性 -->
           <property name="driverClass" value="${jdbc.driver}"/>
           <property name="jdbcUrl" value="${jdbc.url}"/>
           <property name="user" value="${jdbc.username}"/>
           <property name="password" value="${jdbc.password}"/>
   
           <!-- c3p0连接池的私有属性 -->
           <property name="maxPoolSize" value="30"/>
           <property name="minPoolSize" value="2"/>
           <!-- 关闭连接后不自动commit -->
           <property name="autoCommitOnClose" value="false"/>
           <!-- 获取连接超时时间 -->
           <property name="checkoutTimeout" value="10000"/>
           <!-- 当获取连接失败重试次数 -->
           <property name="acquireRetryAttempts" value="2"/>
       </bean>
   
       <!--3.配置sqlSessionFactory对象-->
       <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
           <!-- 注入数据库连接池 -->
           <property name="dataSource" ref="dataSource"/>
           <!--   绑定mybatis的配置文件     -->
           <property name="configLocation" value="classpath:mybatis-config.xml"/>
       </bean>
   
       <!--4.配置dao接口扫描包，动态的实现了dao接口可以注入到spring容器中-->
       <!--解释 ：https://www.cnblogs.com/jpfss/p/7799806.html-->
       <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
           <!--注入sqlSessionFactory-->
           <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
           <!--要扫描的dao包-->
           <property name="basePackage" value="com.zyy.mapper"/>
       </bean>
   </beans>
   ```

3. spring整合service层  spring-service.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
   
       <!--1.扫描service下的包-->
       <context:component-scan base-package="com.zyy.service"/>
   
       <!--2.将我们的所有的业务类，注入到spring，可以通过配置或者注解实现-->
       <bean id="bookService" class="com.zyy.service.impl.BookServiceImpl">
           <property name="bookMapper" ref="bookMapper"/>
       </bean>
   
       <!--3.声明式事务配置-->
       <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
           <!-- 注入数据库连接池 -->
           <property name="dataSource" ref="dataSource"/>
       </bean>
   
       <!--4.aop事务支持 这里省略-->
   
   </beans>
   ```

> SpringMVC层

1. web.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
       <!--1.配置dispatcherServlet-->
       <servlet>
           <servlet-name>springmvc</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <init-param>
               <param-name>contextConfigLocation</param-name>
               <!--一定要注意:我们这里加载的是总的配置文件，否则可能找不到bean， 之前被这里坑了！--> 
               <param-value>classpath:applicationContext.xml</param-value>
           </init-param>
           <load-on-startup>1</load-on-startup>
       </servlet>
       <servlet-mapping>
           <servlet-name>springmvc</servlet-name>
           <url-pattern>/</url-pattern>
       </servlet-mapping>
   
       <!--2.乱码问题-->
       <filter>
           <filter-name>encodingFilter</filter-name>
           <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
           <init-param>
               <param-name>encoding</param-name>
               <param-value>UTF-8</param-value>
           </init-param>
       </filter>
       <filter-mapping>
           <filter-name>encodingFilter</filter-name>
           <url-pattern>/*</url-pattern>
       </filter-mapping>
   
       <!--3.session设置-->
       <session-config>
           <session-timeout>15</session-timeout>
       </session-config>
   
   </web-app>
   ```

2. spring-mvc.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:mvc="http://www.springframework.org/schema/mvc"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">
   
       <!--1..开启SpringMVC注解驱动-->
       <mvc:annotation-driven/>
       <!--2.静态资源默认servlet配置
       https://blog.csdn.net/codejas/article/details/80055608-->
       <mvc:default-servlet-handler/>
       <!--3.扫描web相关的bean-->
       <context:component-scan base-package="com.zyy.controller"/>
       <!--4.视图解析器-->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
           <property name="prefix" value="/WEB-INF/jsp/"/>
           <property name="suffix" value=".jsp"/>
       </bean>
   
   </beans>
   ```

3. spring配置整合文件 applicationContext.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <import resource="classpath:spring-dao.xml"/>
       <import resource="classpath:spring-service.xml"/>
       <import resource="classpath:spring-mvc.xml"/>
   </beans>
   ```



**配置文件，暂时结束！Controller 和 视图层编写**

1. BookController编写

   ```java
   @Controller
   @RequestMapping("/book")
   public class BookController {
   
       private BookService bookService;
   
       @Autowired
       @Qualifier("bookService")
       public void setBookService(BookService bookService) {
           this.bookService = bookService;
       }
   
       @RequestMapping("/allBook")
       public String queryAllBook(Model model) {
           List<Books> bookList = bookService.queryAllBook();
           model.addAttribute("bookList", bookList);
           return "allBook";
       }
   
       @RequestMapping("/toAddBook")
       public String toAddBook() {
           return "addBook";
       }
   
       @RequestMapping("/addBook")
       public String addBook(Books book) {
           bookService.addBook(book);
           return "redirect:/book/allBook";
       }
   
       @RequestMapping("/toUpdateBook")
       public String toUpdateBook(@RequestParam("id") int bookId, Model model) {
           Books book = bookService.queryBookById(bookId);
           model.addAttribute("bookInfo", book);
           return "updateBook";
       }
   
       @RequestMapping("/updateBook")
       public String updateBook(Books book) {
           bookService.updateBook(book);
           return "redirect:/book/allBook";
       }
   
       @RequestMapping("/deleteBook/{id}")
       public String deleteBook(@PathVariable("id") int id) {
           bookService.deleteBook(id);
           return "redirect:/book/allBook";
       }
   
       @RequestMapping("/queryBookByName")
       public String queryBookByName(@RequestParam("bookName") String bookName, Model model) {
           List<Books> bookList = null;
           if (bookName == null || bookName.length() == 0) {
               bookList = bookService.queryAllBook();
           } else {
               bookList = bookService.queryBooksByName(bookName);
           }
           model.addAttribute("bookList", bookList);
           return "allBook";
       }
   }
   ```

2. 编写首页 index.jsp

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
     <title>首页</title>
     <style type="text/css">
       a {
         text-decoration: none;
         color: black;
         font-size: 18px;
   
       }
       h3 {
         width: 180px;
         height: 38px;
         margin: 100px auto;
         text-align: center;
         line-height: 38px;
         background: deepskyblue;
         border-radius: 4px;
       }
     </style>
   </head>
   <body>
   
   <h3>
     <a href="${pageContext.request.contextPath}/book/allBook">进入书籍列表页</a>
   </h3>
   </body>
   </html>
   ```
   
3. 书籍列表页面 allBook.jsp

   ```jsp
   <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>书籍列表</title>
       <!-- 引入 Bootstrap -->
       <link href="https://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
   </head>
   <body>
   <div class="container">
   
       <div class="row clearfix">
           <div class="col-md-12 column">
               <div class="page-header">
                   <h1>
                       <small>书籍列表</small>
                   </h1>
               </div>
           </div>
       </div>
   
       <div class="row">
           <div class="col-md-4 column">
               <a class="btn btn-primary" href="${pageContext.request.contextPath}/book/toAddBook">新增书籍</a>
           </div>
   
           <div class="col-md-8 column">
               <form action="${pageContext.request.contextPath}/book/queryBookByName" method="post" class="form-inline" style="float: right">
                   <input type="text" class="form-control" id="bookName" name="bookName" placeholder="请输出书籍名称">
                   <input class="btn btn-primary" type="submit" value="查询">
               </form>
           </div>
   
       </div>
   
       <div class="row clearfix">
           <div class="col-md-12 column">
               <table class="table table-hover table-striped">
                   <thead>
                   <tr>
                       <th>书籍编号</th>
                       <th>书籍名称</th>
                       <th>书籍数量</th>
                       <th>书籍详情</th>
                       <th>操作</th>
                   </tr>
                   </thead>
                   <tbody>
   
                   <c:forEach var="book" items="${bookList}">
                       <tr>
                           <td>${book.bookID}</td>
                           <td>${book.bookName}</td>
                           <td>${book.bookCounts}</td>
                           <td>${book.detail}</td>
                           <td>
                               <a href="${pageContext.request.contextPath}/book/toUpdateBook?id=${book.bookID}">修改</a>
                               &nbsp; | &nbsp;
                               <a href="${pageContext.request.contextPath}/book/deleteBook/${book.bookID}">删除</a>
                           </td>
                       </tr>
                   </c:forEach>
                   </tbody>
               </table>
           </div>
   
       </div>
   </div>
   
   </body>
   </html>
   
   ```
   
4. 添加书籍页面 addBook.jsp

   ```jsp
   <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>新增书籍</title>
       <!-- 引入 Bootstrap -->
       <link href="https://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
   </head>
   <body>
   <div class="container">
   
       <div class="row clearfix">
           <div class="col-md-12 column">
               <div class="page-header">
                   <h1>
                       <small>新增书籍</small>
                   </h1>
               </div>
           </div>
       </div>
   
       <form action="${pageContext.request.contextPath}/book/addBook" method="post">
           <div class="form-group">
               <label for="bookName">书籍名称</label>
               <input type="text" class="form-control" id="bookName" name="bookName" required>
           </div>
           <div class="form-group">
               <label for="bookCounts">书籍数量</label>
               <input type="text" class="form-control" id="bookCounts" name="bookCounts" required>
           </div>
           <div class="form-group">
               <label for="detail">书籍描述</label>
               <input type="text" class="form-control" id="detail" name="detail" required>
           </div>
           <div class="form-group">
               <input type="submit" class="form-control" value="添加"/>
           </div>
       </form>
   
   
       </div>
   </div>
   
   </body>
   </html>
   
   ```

5. 修改书籍页面updateBook.jsp

   ```jsp
   <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>修改书籍</title>
       <!-- 引入 Bootstrap -->
       <link href="https://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
   </head>
   <body>
   <div class="container">
   
       <div class="row clearfix">
           <div class="col-md-12 column">
               <div class="page-header">
                   <h1>
                       <small>修改书籍</small>
                   </h1>
               </div>
           </div>
       </div>
   
       <form action="${pageContext.request.contextPath}/book/updateBook" method="post">
           <input type="hidden" name="bookID" value="${bookInfo.bookID}" >
           <div class="form-group">
               <label for="bookName">书籍名称</label>
               <input type="text" class="form-control" id="bookName" name="bookName" value="${bookInfo.bookName}" required>
           </div>
           <div class="form-group">
               <label for="bookCounts">书籍数量</label>
               <input type="text" class="form-control" id="bookCounts" name="bookCounts" value="${bookInfo.bookCounts}" required>
           </div>
           <div class="form-group">
               <label for="detail">书籍描述</label>
               <input type="text" class="form-control" id="detail" name="detail" value="${bookInfo.detail}" required>
           </div>
           <div class="form-group">
               <input type="submit" class="form-control" value="修改"/>
           </div>
       </form>
   
       </div>
   </div>
   
   </body>
   </html>
   
   ```


项目整体结构：

![image-20220522155835974](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220522155835974.png)

这个是同学们的第一个SSM整合案例，一定要烂熟于心！

# 8、ajax

> 简介

- **AJAX = Asynchronous JavaScript and XML（异步的 JavaScript 和 XML）**
- AJAX 是一种在无需重新加载整个网页的情况下，能够更新部分网页的技术
- AJAX 不是一种新的编程语言，而是一种用于创建更好更快以及交互性更强的web应用程序技术
- 在 2005 年，Google 通过其 Google Suggest 使 AJAX 变得流行起来。Google Suggest能够自动帮你完成搜索单词
- Google Suggest 使用 AJAX 创造出动态性极强的 web 界面：当您在谷歌的搜索框输入关键字时，JavaScript 会把这些字符发送到服务器，然后服务器会返回一个搜索建议的列表
- 就和国内百度的搜索框一样
- 传统的网页（即不用ajax技术的网页），想要更新或者提交一个表单，都需要重新加载整个网页
- 使用ajax技术的网页，通过在后台服务器进行少量的数据交换，就可以实现异步局部更新
- 使用ajax，用户可以创建接近本地桌面应用的直接、高可用、更丰富、更动态的web用户界面



> 伪造ajax

我们可以使用前端的一个标签来伪造一个ajax的样子。iframe标签

1. 新建一个module ：sspringmvc-06-ajax ， 导入web支持

2. 编写一个 test.html使用 iframe 测试，感受下效果

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>iframe体验页面无刷新</title>
       <script>
           function go() {
               let url = document.getElementById("url").value;
               document.getElementById("myIframe").src = url;
           }
       </script>
   </head>
   <body>
   
   <div>
       <p>请输入地址</p>
       <p>
           <input type="text" id="url" value="https://cn.bing.com/?scope=web&FORM=ANNTH1"/>
           <input type="button" value="提交" onclick="go()"/>
       </p>
   </div>
   <div>
       <iframe id="myIframe" style="width: 100%; height: 500px"></iframe>
   </div>
   </body>
   </html>
   ```

**利用ajax可以做：**

- 注册时，输入用户名自动检测用户是否已经存在
- 登陆时，提示用户名密码错误
- 删除数据行时，将行ID发送到后台，后台在数据库中删除，数据库删除成功后，在页面DOM中将数据行也删除
- 。。。



> jQuery.ajax

纯JS原生实现Ajax我们不去讲解这里，直接使用jquery提供的，方便学习和使用，避免重复造轮子，有兴趣的同学可以去了解下JS原生XMLHttpRequest ！

ajax的核心是XMLHttpRequest对象（XHR）。XHR为向服务器发送请求和解析服务器响应提供了接口。能够以异步方式从服务器获取新数据。

jQuery 提供多个与 AJAX 有关的方法。

通过 jQuery AJAX 方法，您能够使用 HTTP Get 和 HTTP Post 从远程服务器上请求文本、HTML、XML 或 JSON – 同时您能够把这些外部数据直接载入网页的被选元素中。

jQuery 不是生产者，而是大自然搬运工。

jQuery Ajax本质就是 XMLHttpRequest，对他进行了封装，方便调用！

```
jQuery.ajax(...)
      部分参数：
            url：请求地址
            type：请求方式，GET、POST（1.9.0之后用method）
        headers：请求头
            data：要发送的数据
    contentType：即将发送信息至服务器的内容编码类型(默认: "application/x-www-form-urlencoded; charset=UTF-8")
          async：是否异步
        timeout：设置请求超时时间（毫秒）
      beforeSend：发送请求前执行的函数(全局)
        complete：完成之后执行的回调函数(全局)
        success：成功之后执行的回调函数(全局)
          error：失败之后执行的回调函数(全局)
        accepts：通过请求头发送给服务器，告诉服务器当前客户端可接受的数据类型
        dataType：将服务器端返回的数据转换成指定类型
          "xml": 将服务器端返回的内容转换成xml格式
          "text": 将服务器端返回的内容转换成普通文本格式
          "html": 将服务器端返回的内容转换成普通文本格式，在插入DOM中时，如果包含JavaScript标签，则会尝试去执行。
        "script": 尝试将返回值当作JavaScript去执行，然后再将服务器端返回的内容转换成普通文本格式
          "json": 将服务器端返回的内容转换成相应的JavaScript对象
        "jsonp": JSONP 格式使用 JSONP 形式调用函数时，如 "myurl?callback=?" jQuery 将自动替换 ? 为正确的函数名，以执行回调函数
```

**我们来个简单的测试，使用最原始的HttpServletResponse处理 , .最简单 , 最通用**

1. 配置web.xml 和 springmvc的配置文件，复制上面案例的即可 【记得静态资源过滤和注解驱动配置上】

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:mvc="http://www.springframework.org/schema/mvc"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/mvc
          https://www.springframework.org/schema/mvc/spring-mvc.xsd
          http://www.springframework.org/schema/context
          https://www.springframework.org/schema/context/spring-context.xsd">
   
       <!--静态资源过滤-->
       <mvc:default-servlet-handler/>
       <mvc:annotation-driven/>
   
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
           <property name="prefix" value="/WEB-INF/jsp/"/>
           <property name="suffix" value=".jsp"/>
       </bean>
   
       <context:component-scan base-package="com.zyy.controller"/>
   
   </beans>
   ```

2. 编写一个AjaxController

   ```java
   @RestController
   public class AjaxController {
       @RequestMapping("/ajax/t2")
       public void test2(String name, HttpServletResponse response) throws IOException {
           if ("zyy".equals(name)) {
               response.getWriter().print("true");
           } else {
               response.getWriter().print("false");
           }
       }
   }
   ```

   

3. 导入jquery ， 可以使用在线的CDN ， 也可以下载导入

   ```jsp
     <%--导入JQuery--%>
     <script src="${pageContext.request.contextPath}/statics/js/jquery-3.6.0.js"></script>
     <%--<script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>--%>
   ```

4. 编写index.jsp测试

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
     <title>$Title$</title>
     <%--导入JQuery--%>
     <script src="${pageContext.request.contextPath}/statics/js/jquery-3.6.0.js"></script>
     <%--<script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>--%>
     <script>
       function a1() {
         $.post(
                 {
                   url: "${pageContext.request.contextPath}/ajax/t2",
                   data: {"name": $("#username").val()},
                   success: function (data, status) {
                     console.log(data)
                     console.log(status)
   
                   }
                 }
         )
       }
     </script>
   </head>
   <body>
   用户名：<input type="text" id="username" onblur="a1()">
   </body>
   </html>
   
   ```

   

5. 启动tomcat测试！打开浏览器的控制台，当我们鼠标离开输入框的时候，可以看到发出了一个ajax的请求！是后台返回给我们的结果！测试成功！

   ![image-20220525221951745](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220525221951745.png)



> Springmvc实现

1. 实体类User

   ```java
   @Data
   @AllArgsConstructor
   @NoArgsConstructor
   public class User {
       private int id;
       private String name;
       private int age;
   }
   ```

2. 我们来获取一个集合对象，展示到前端页面  controller添加方法

   ```java
       @RequestMapping("/ajax/t3")
       public List<User> test3() {
           List<User> userList = new ArrayList<User>();
           userList.add(new User(1, "zyy1", 18));
           userList.add(new User(2, "zyy2", 18));
           userList.add(new User(3, "zyy3", 18));
           return userList;
       }
   ```

3. 前端页面

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
       <script src="${pageContext.request.contextPath}/statics/js/jquery-3.6.0.js"></script>
       <script>
           $(function () {
               $("#btn").click(function () {
                   /**
                    * $.post(url, param【可以省略】, success)
                    */
                   $.post("${pageContext.request.contextPath}/ajax/t3", function (data) {
                       // console.log(data)
                       let html = "";
                       for (let i = 0; i < data.length; i++) {
   
                           html += "<tr>"
                               + "<td>" + data[i].id + "</td>"
                               + "<td>" + data[i].name + "</td>"
                               + "<td>" + data[i].age + "</td>"
                               + "</tr>"
                       }
                       $("#content").html(html);
                   })
               })
           })
       </script>
   </head>
   <body>
   <input type="button" value="加载数据" id="btn">
   <table>
       <thead>
       <tr>
           <td>编号</td>
           <td>姓名</td>
           <td>年龄</td>
       </tr>
       </thead>
       <tbody id="content">
       </tbody>
   </table>
   
   </body>
   </html>
   
   ```

4. 测试

   ![image-20220526230703051](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220526230703051.png)



> 注册提示效果

我们再测试一个小Demo，思考一下我们平时注册时候，输入框后面的实时提示怎么做到的；如何优化

我们写一个Controller

```java

    @RequestMapping("/ajax/t4")
    public String test4(String name, String pwd) {
        String msg = "";
        if (name != null) {
            if ("admin".equals(name)) {
                msg = "ok";
            } else {
                msg = "用户名有误";
            }
        }

        if (pwd != null) {
            if ("123456".equals(pwd)) {
                msg = "ok";
            } else {
                msg = "密码有误";
            }
        }
        return msg;
    }
```

前端页面 login.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>登录</title>
    <script src="${pageContext.request.contextPath}/statics/js/jquery-3.6.0.js"></script>
    <script>
        function a1() {
            $.post({
                url: "${pageContext.request.contextPath}/ajax/t4",
                data: {"name": $("#username").val()},
                success: function (data) {
                    if (data.toString() === "ok") {
                        $("#userInfo").css("color", "green")
                    } else {
                        $("#userInfo").css("color", "red")
                    }
                    $("#userInfo").html(data)
                }
            })
        }

        function a2() {
            $.post({
                url: "${pageContext.request.contextPath}/ajax/t4",
                data: {"pwd": $("#pwd").val()},
                success: function (data) {
                    if (data.toString() === "ok") {
                        $("#pwdInfo").css("color", "green")
                    } else {
                        $("#pwdInfo").css("color", "red")
                    }
                    $("#pwdInfo").html(data)
                }
            })
        }
    </script>
</head>
<body>

<p>
    用户名：<input type="text" id="username" onblur="a1()">
    <span id="userInfo"></span>
</p>
<p>
    密码：<input type="text" id="pwd" onblur="a2()">
    <span id="pwdInfo"></span>
</p>

</body>
</html>

```



【记得处理json乱码问题】

```xml

    <mvc:annotation-driven>
        <mvc:message-converters register-defaults="true">
            <bean class="org.springframework.http.converter.StringHttpMessageConverter">
                <constructor-arg value="UTF-8"/>
            </bean>
            <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
                <property name="objectMapper">
                    <bean class="org.springframework.http.converter.json.Jackson2ObjectMapperFactoryBean">
                        <property name="failOnEmptyBeans" value="false"/>
                    </bean>
                </property>
            </bean>
        </mvc:message-converters>
    </mvc:annotation-driven>
```



测试一下效果，动态请求响应，局部刷新，就是如此！

![image-20220528095508542](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220528095508542.png)

> 获取baidu接口Demo



新建一个baidu.html

```html
<!DOCTYPE HTML>
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
    <title>JSONP百度搜索</title>
    <style>
        #q {
            width: 500px;
            height: 30px;
            border: 1px solid #ddd;
            line-height: 30px;
            display: block;
            margin: 0 auto;
            padding: 0 10px;
            font-size: 14px;
        }

        #ul {
            width: 520px;
            list-style: none;
            margin: 0 auto;
            padding: 0;
            border: 1px solid #ddd;
            margin-top: -1px;
            display: none;
        }

        #ul li {
            line-height: 30px;
            padding: 0 10px;
        }

        #ul li:hover {
            background-color: #f60;
            color: #fff;
        }
    </style>
    <script>

        // 2.步骤二
        // 定义demo函数 (分析接口、数据)
        function demo(data) {
            const Ul = document.getElementById('ul');
            let html = '';
            // 如果搜索数据存在 把内容添加进去
            if (data.s.length > 0) {
                // 隐藏掉的ul显示出来
                Ul.style.display = 'block';
                // 搜索到的数据循环追加到li里
                for (let i = 0; i < data.s.length; i++) {
                    html += '<li>' + data.s[i] + '</li>';
                }
                // 循环的li写入ul
                Ul.innerHTML = html;
            }
        }

        // 1.步骤一
        window.onload = function () {
            // 获取输入框和ul
            const Q = document.getElementById('q');
            const Ul = document.getElementById('ul');

            // 事件鼠标抬起时候
            Q.onkeyup = function () {
                // 如果输入框不等于空
                if (this.value != '') {
                    // ☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆JSONPz重点☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆
                    // 创建标签
                    const script = document.createElement('script');
                    //给定要跨域的地址 赋值给src
                    //这里是要请求的跨域的地址 我写的是百度搜索的跨域地址
                    script.src = 'https://sp0.baidu.com/5a1Fazu8AA54nxGko9WTAnF6hhy/su?wd=' + this.value + '&cb=demo';
                    // 将组合好的带src的script标签追加到body里
                    document.body.appendChild(script);
                }
            }
        }
    </script>
</head>

<body>
<input type="text" id="q"/>
<ul id="ul">
</ul>
</body>
</html>
```

效果

![image-20220528100529705](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220528100529705.png)



# 9、拦截器

> 概述

SpringMVC的处理器拦截器类似于servlet开发中的过滤器Filter，用于对处理器预处理和后处理，开发者可以自己定义一些拦截器来实现特定的功能。

过滤器和拦截器的区别：

- 过滤器

  - servlet规范的一部分，任务javaweb功能都可以使用
  - 在url-pattern中配置了/*之后，可以对所有要访问的资源进行拦截

- 拦截器

  - 拦截器是SpringMVC框架自己的，只有使用了springmv框架的工程才能使用
  - 拦截器只会拦截访问的控制器方法，如果访问的是jsp html css image  js是不会进行拦截的

  

> 自定义拦截器

想要自定义拦截器，必须实现 HandlerInterceptor 接口

1. 新建一个Moudule ， springmvc-07-Interceptor  ， 添加web支持

2. 配置web.xml 和 springmvc-servlet.xml 文件 (这里忽略。。。)

3. 编写一个拦截器

   ```java
   public class MyInterceptor implements HandlerInterceptor {
   
       public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
           //返回 true :放行，可以处理下一个拦截器
           //返回 false :不放行，不可以处理下一个拦截器
           System.out.println("=========处理前=========");
           return true;
       }
   
       public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
           System.out.println("=========处理后=========");
       }
   
       public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
           System.out.println("=========清理=========");
       }
   }
   ```

4. 在springmvc的配置文件中配置拦截器

   ```xml
       <!--关于拦截器的配置-->
       <mvc:interceptors>
           <mvc:interceptor>
               <!--/** 包括路径及其子路径-->
               <!--/admin/* 拦截的是/admin/add等等这种 , /admin/add/user不会被拦截-->
               <!--/admin/** 拦截的是/admin/下的所有-->
               <mvc:mapping path="/**"/>
               <!--bean配置的就是拦截器-->
               <bean class="com.zyy.config.MyInterceptor"/>
           </mvc:interceptor>
       </mvc:interceptors>
   ```

5. 编写一个Controller，接收请求

   ```java
   @RestController
   public class TestController {
       @GetMapping("/t1")
       public String test1() {
           System.out.println("TestController===>test1执行了");
           return "OK";
       }
   }
   ```

6. 启动tomcat 测试一下！

   请求http://localhost:8080/t1

   控制打印

   ```
   =========处理前=========
   TestController===>test1执行了
   =========处理后=========
   =========处理清理=========
   ```

>  验证用户是否登录 (认证用户)

**实现思路**

1. 有一个登陆页面，需要写一个controller访问页面
2. 登陆页面有一提交表单的动作。需要在controller中处理。判断用户名密码是否正确。如果正确，向session中写入用户信息。返回登陆成功
3. 拦截用户请求，判断用户是否登陆。如果用户已经登陆。放行， 如果用户未登陆，跳转到登陆页面



**实现**

1. 编写一个登陆页面  login.jsp

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
   <%--在WEB-INF下的所有页面和资源，只能通过controller或者servlet进行访问--%>
   <h1>登录</h1>
   <form action="${pageContext.request.contextPath}/user/login" method="post">
       用户名：<input type="text" name="username"/>
       密码：<input type="text" name="password"/>
       <input type="submit" value="提交"/>
   </form>
   
   </body>
   </html>
   ```

2. 编写一个Controller处理请求

   ```java
   package com.zyy.controller;
   
   import org.springframework.stereotype.Controller;
   import org.springframework.ui.Model;
   import org.springframework.web.bind.annotation.RequestMapping;
   
   import javax.servlet.http.HttpSession;
   
   /**
    * @Description: 类描述
    * @Author: zyy
    * @Date: 2022/05/28 11:10
    */
   @Controller
   public class LoginController {
   
   
       @RequestMapping("/user/goLogin")
       public String goLogin() {
           // 跳转视图 -- 登录页面
           return "login";
       }
   
       @RequestMapping("/user/main")
       public String main() {
           // 主页
           return "main";
       }
   
       @RequestMapping("/user/login")
       public String login(String username, String password, HttpSession session, Model model) {
           session.setAttribute("userInfo", username);
           model.addAttribute("username", username);
           // 登录成功后，跳转主页
           return "main";
       }
   
       @RequestMapping("/user/logout")
       public String logout(HttpSession session) {
           // 移除
           session.removeAttribute("userInfo");
           return "login";
       }
   
   }
   
   ```

   

3. 编写一个登陆成功后的首页 main.jsp

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
   <h1>首页</h1>
   ${username}
   <p>
       <a href="${pageContext.request.contextPath}/user/logout">注销</a>
   </p>
   
   </body>
   </html>
   ```

   

4. 在 index 页面上测试跳转！启动Tomcat 测试，未登录也可以进入首页

5. 编写用户登录拦截器

   ```java
   package com.zyy.config;
   
   import org.springframework.web.servlet.HandlerInterceptor;
   
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletResponse;
   import javax.servlet.http.HttpSession;
   
   /**
    * @Description: 类描述
    * @Author: zyy
    * @Date: 2022/05/28 11:26
    */
   public class LoginInterceptor implements HandlerInterceptor {
       public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
           //哪些可以放行
   
           //第一次登录
           if (request.getRequestURI().contains("login")) {
               return true;
           }
           //已经登录成功的
           HttpSession session = request.getSession();
           if (session.getAttribute("userInfo") != null) {
               return true;
           }
           // 用户没有登陆跳转到登陆页面
           request.getRequestDispatcher("/WEB-INF/jsp/login.jsp").forward(request, response);
           return false;
       }
   }
   
   ```

6. 在Springmvc的配置文件中注册拦截器

   ```xml
           <mvc:interceptor>
               <mvc:mapping path="/user/**"/>
               <bean class="com.zyy.config.LoginInterceptor"/>
           </mvc:interceptor>
   ```

7. 重启tocat，测试



# 10、文件上传和下载

> 准备工作

文件上传是项目开发中最常见的功能之一 ,springMVC 可以很好的支持文件上传，但是SpringMVC上下文中默认没有装配MultipartResolver，因此默认情况下其不能处理文件上传工作。如果想使用Spring的文件上传功能，则需要在上下文中配置MultipartResolver。

前端表单要求：为了能上传文件，必须将表单的method设置为POST，并将enctype设置为multipart/form-data。只有在这样的情况下，浏览器才会把用户选择的文件以二进制数据发送给服务器；

**对表单中的 enctype 属性做个详细的说明：**

- application/x-www=form-urlencoded：默认方式，只处理表单域中的 value 属性值，采用这种编码方式的表单会将表单域中的值处理成 URL 编码方式
- multipart/form-data：这种编码方式会以二进制流的方式来处理表单数据，这种编码方式会把文件域指定文件的内容也封装到请求参数中，不会对字符编码
- 除了把空格转换为 "+" 号外，其他字符都不做编码处理，这种方式适用直接通过表单发送邮件

```xml
<form action="" enctype="multipart/form-data" method="post">
   <input type="file" name="file"/>
   <input type="submit">
</form>
```

一旦设置了enctype为multipart/form-data，浏览器即会采用二进制流的方式来处理表单数据，而对于文件上传的处理则涉及在服务器端解析原始的HTTP响应。在2003年，Apache Software Foundation发布了开源的Commons FileUpload组件，其很快成为Servlet/JSP程序员上传文件的最佳选择

- Servlet3.0规范已经提供方法来处理文件上传，但这种上传需要在Servlet中完成
- Spring MVC则提供了更简单的封装
- Spring MVC为文件上传提供了直接的支持，这种支持是用即插即用的MultipartResolver实现的
- Spring MVC使用Apache Commons FileUpload技术实现了一个MultipartResolver实现类
- CommonsMultipartResolver。因此，SpringMVC的文件上传还需要依赖Apache Commons FileUpload的组件

> 文件上传

1. 导入文件上传的jar包，commons-fileupload ， Maven会自动帮我们导入他的依赖包 commons-io包

   ```xml
           <!--文件上传-->
           <dependency>
               <groupId>commons-fileupload</groupId>
               <artifactId>commons-fileupload</artifactId>
               <version>1.3.3</version>
           </dependency>
           <!--servlet-api导入高版本的-->
           <dependency>
               <groupId>javax.servlet</groupId>
               <artifactId>javax.servlet-api</artifactId>
               <version>4.0.1</version>
           </dependency>
   ```

2. 配置bean：multipartResolver

   【**注意！！！这个bena的id必须为：multipartResolver ， 否则上传文件会报400的错误！在这里栽过坑,教训！**】

   ```xml
       <!--文件上传配置-->
       <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
           <!-- 请求的编码格式，必须和jSP的pageEncoding属性一致，以便正确读取表单的内容，默认为ISO-8859-1 -->
           <property name="defaultEncoding" value="utf-8"/>
           <!-- 上传文件大小上限，单位为字节（10485760=10M） -->
           <property name="maxUploadSize" value="10485760"/>
           <property name="maxInMemorySize" value="40960"/>
       </bean>
   ```

   CommonsMultipartFile 的 常用方法：

   - **String getOriginalFilename()：获取上传文件的原名**
   - **InputStream getInputStream()：获取文件流**
   - **void transferTo(File dest)：将上传文件保存到一个目录文件中**

    我们去实际测试一下

3. 编写前端页面

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
     <title>$Title$</title>
   </head>
   <body>
   <form action="${pageContext.request.contextPath}/upload" enctype="multipart/form-data" method="post">
     <input type="file" name="file"/>
     <input type="submit" value="上传">
   </form>
   </body>
   </html>
   ```

4. Controller

   ```java
   import org.springframework.stereotype.Controller;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RequestParam;
   import org.springframework.web.multipart.commons.CommonsMultipartFile;
   
   import javax.servlet.http.HttpServletRequest;
   import java.io.File;
   import java.io.FileOutputStream;
   import java.io.IOException;
   import java.io.InputStream;
   import java.io.OutputStream;
   
   /**
    * @Description: 类描述
    * @Author: zyy
    * @Date: 2022/05/29 16:22
    */
   @Controller
   public class FileController {
       //@RequestParam("file") 将name=file控件得到的文件封装成CommonsMultipartFile 对象
       //批量上传CommonsMultipartFile则为数组即可
       @RequestMapping("/upload")
       public String fileUpload(@RequestParam("file") CommonsMultipartFile file, HttpServletRequest request) throws IOException {
           System.out.println("=upload====");
   
           //获取文件名 : file.getOriginalFilename();
           String uploadFileName = file.getOriginalFilename();
   
           //如果文件名为空，直接回到首页！
           if ("".equals(uploadFileName)) {
               return "redirect:/index.jsp";
           }
           System.out.println("上传文件名 : " + uploadFileName);
   
           //上传路径保存设置
           String path = request.getServletContext().getRealPath("/upload");
           //如果路径不存在，创建一个
           File realPath = new File(path);
           if (!realPath.exists()) {
               realPath.mkdir();
           }
           System.out.println("上传文件保存地址：" + realPath);
   
           //文件输入流
           InputStream is = file.getInputStream();
           //文件输出流
           OutputStream os = new FileOutputStream(new File(realPath, uploadFileName));
   
           //读取写出
           int len = 0;
           byte[] buffer = new byte[1024];
           while ((len = is.read(buffer)) != -1) {
               os.write(buffer, 0, len);
           }
           os.flush();
           os.close();
           is.close();
           return "redirect:/index.jsp";
       }
   }
   
   ```

5. 测试上传文件，OK！

   ```
   上传文件名 : 个人小汽车增量指标申请表.pdf
   上传文件保存地址：D:\IdeaProjects\SpringMVC\out\artifacts\springmvc_08_file_war_exploded\upload
   ```

   

**采用file.Transto 来保存上传的文件**

1. 编写Controller

   ```java
       /*
        * 采用file.Transto 来保存上传的文件
        */
       @RequestMapping("/upload2")
       public String  fileUpload2(@RequestParam("file") CommonsMultipartFile file, HttpServletRequest request) throws IOException {
   
           //上传路径保存设置
           String path = request.getServletContext().getRealPath("/upload");
           File realPath = new File(path);
           if (!realPath.exists()){
               realPath.mkdir();
           }
           //上传文件地址
           System.out.println("上传文件保存地址："+realPath);
   
           //通过CommonsMultipartFile的方法直接写文件（注意这个时候）
           file.transferTo(new File(realPath +"/"+ file.getOriginalFilename()));
   
           return "redirect:/index.jsp";
       }
   ```

2. 前端表单提交地址修改

3. 测试

   ```
   上传文件保存地址：D:\IdeaProjects\SpringMVC\out\artifacts\springmvc_08_file_war_exploded\upload
   ```



> 文件下载

**文件下载步骤：**

1. 设置 response 响应头
2. 读取文件 -- InputStream
3. 写出文件 -- OutputStream
4. 执行操作
5. 关闭流 （先开后关）

**代码实现：**

```java

    @RequestMapping(value = "/download")
    public String downloads(HttpServletResponse response, HttpServletRequest request) throws Exception {
        //要下载的图片地址
        String path = request.getServletContext().getRealPath("/upload");
        String fileName = "个人小汽车增量指标申请表.pdf";

        //1、设置response 响应头
        response.reset(); //设置页面不缓存,清空buffer
        response.setCharacterEncoding("UTF-8"); //字符编码
        response.setContentType("multipart/form-data"); //二进制传输数据
        //设置响应头
        response.setHeader("Content-Disposition",
                "attachment;fileName=" + URLEncoder.encode(fileName, "UTF-8"));

        File file = new File(path, fileName);
        //2、 读取文件--输入流
        InputStream input = new FileInputStream(file);
        //3、 写出文件--输出流
        OutputStream out = response.getOutputStream();

        byte[] buff = new byte[1024];
        int index = 0;
        //4、执行 写出操作
        while ((index = input.read(buff)) != -1) {
            out.write(buff, 0, index);
            out.flush();
        }
        out.close();
        input.close();
        return null;
    }
```

前端

```jsp
<p>
  <a href="${pageContext.request.contextPath}/download">下载</a>
</p>
```

测试，文件下载OK，大家可以和我们之前学习的JavaWeb原生的方式对比一下，就可以知道这个便捷多了!
