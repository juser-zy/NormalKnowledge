# 01、简介

> 回顾什么是spring

Spring是一个开源框架，2003 年兴起的一个轻量级的Java 开发框架，作者：Rod Johnson。

**Spring是为了解决企业级应用开发的复杂性而创建的，简化开发。**



> spring是如何简化开发的

为了降低Java开发的复杂性，Spring采用了以下4种关键策略：

1. 基于pojo的轻量级和最小侵入性编程，所有东西都是bean
2. 通过ioc，依赖注入（di）和面向接口实现松耦合
3. 基于切面（aop）和惯例进行声明式编程
4. 通过切面和模板减少样式代码，RedisTemplate，xxxTemplate；



> 是什么springboot

随着 Spring 不断的发展，涉及的领域越来越多，项目整合开发需要配合各种各样的文件，慢慢变得不那么易用简单，违背了最初的理念，甚至人称配置地狱。Spring Boot 正是在这样的一个背景下被抽象出来的开发框架，目的为了让大家更容易的使用 Spring 、更容易的集成各种常用的中间件、开源软件；

Spring Boot 基于 Spring 开发，Spirng Boot 本身并不提供 Spring 框架的核心特性以及扩展功能，只是用于快速、敏捷地开发新一代基于 Spring 框架的应用程序。也就是说，它并不是用来替代 Spring 的解决方案，而是和 Spring 框架紧密结合用于提升 Spring 开发者体验的工具。Spring Boot 以**约定大于配置的核心思想**，默认帮我们进行了很多设置，多数 Spring Boot 应用只需要很少的 Spring 配置。同时它集成了大量常用的第三方库配置（例如 Redis、MongoDB、Jpa、RabbitMQ、Quartz 等等），Spring Boot 应用中这些第三方库几乎可以零配置的开箱即用。

简单来说就是SpringBoot其实不是什么新的框架，它默认配置了很多框架的使用方式，就像maven整合了所有的jar包，spring boot整合了所有的框架 。

**Spring Boot的主要优点：**

- 为所有Spring开发者更快的入门
- **开箱即用**，提供各种默认配置来简化项目配置
- 内嵌式容器简化Web项目
- 没有冗余代码生成和XML配置的要求



# 02、微服务

> 单体应用架构

 所谓单体应用架构 (all in one)是指，我们将一个应用的中的所有应用服务都封装在一个应用中。

 无论是ERP、CRM或是其他什么系统，你都把数据库访问，web访问，等等各个功能放到一个war包内。

- 这样做的好处是，易于开发和测试;也十分方便部署;当需要扩展时，只需要将war复制多份，然后放到多个服务器上，再做个负载均衡就可以了。
- 单体应用架构的缺点是，哪怕我要修改一个非常小的地方，我都需要停掉整个服务，重新打包、部署这个应用war包。特别是对于一个大型应用，我们不可能吧所有内容都放在一个应用里面，我们如何维护、如何分工合作都是问题。



> 微服务架构

all in one的架构方式，我们把所有的功能单元放在一个应用里面。然后我们把整个应用部署到服务器上。如果负载能力不行，我们将整个应用进行水平复制，进行扩展，然后在负载均衡。

所谓微服务架构，就是打破之前all in one的架构方式，把每个功能元素独立出来。把独立出来的功能元素的动态组合，需要的功能元素才去拿来组合，需要多一些时可以整合多个功能元素。所以微服务架构是对功能元素进行复制，而没有对整个应用进行复制。

这样做的好处是:

- 节省了调用资源
- 每个功能元素的服务都是一个可替换的、可独立升级的软件代码

![image-20220713215735760](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220713215735760.png)

Martin Flower于2014年3月25日写的《Microservices》，详细的阐述了什么是微服务

原文地址：https://martinfowler.com/articles/microservices.html
翻译：https://www.cnblogs.com/liuning8023/p/4493156.html



> 如何构建一个微服务

一个大型系统的微服务架构，就像一个复杂交织的神经网络，每一个神经元就是一个功能元素，它们各自完成自己的功能，然后通过http相互请求调用。比如一个电商系统，查缓存、连数据库、浏览页面、结账、支付等服务都是一个个独立的功能服务，都被微化了，它们作为一个个微服务共同构建了一个庞大的系统。如果修改其中的一个功能，只需要更新升级其中一个功能服务单元即可。



但是这种庞大的系统架构给部署和运维带来很大的难度。于是，spring为我们带来了构建大型分布式微服务的全套、全程产品:

- 构建一个个功能独立的微服务应用单元，可以使用springboot，可以帮我们快速构建一个应用;
- 大型分布式网络服务的调用，这部分由spring cloud来完成，实现分布式;
- 在分布式中间，进行流式数据计算、批处理，我们有spring cloud data flow。
- spring为我们想清楚了整个从开始构建应用到大型分布式应用全流程方案。



# 03、第一个springboot程序

Spring官方提供了非常方便的工具让我们快速构建应用

Spring Initializr：https://start.spring.io/

**项目创建方式一：使用Spring Initializr的web页面创建项目**

1. 打开Spring Initializr网站

2. 填写项目信息

   ![image-20220714213113737](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220714213113737.png)

3. 按照上图，配置好，然后点击”Generate“按钮生成项目；下载此项目

4. 解压项目包，并用IDEA以Maven项目导入，一路下一步即可，直到项目导入完毕

5. 如果是第一次使用，可能速度会比较慢，包比较多、需要耐心等待一切就绪。



> pom.xml分析

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		 xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<!--	父依赖 -->
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.7.1</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.zyy</groupId>
	<artifactId>helloworld</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>helloworld</name>
	<description>Demo project for Spring Boot</description>
	<properties>
		<java.version>1.8</java.version>
	</properties>
	<dependencies>
		<!--	web	-->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<!--	单元测试	-->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<!--	打包插件	-->
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

</project>

```



> 编写一个http接口

1. 在主程序的同级目录下，新建一个controller包，一定要在同级目录下，否则识别不到

2. 在包中新建一个HelloController类

   ```java
   package com.zyy.helloworld.controller;
   
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   
   /**
    * @Description: 类描述
    * @Author: zyy
    * @Date: 2022/07/14 21:27
    */
   @RestController
   public class HelloController {
   
       @RequestMapping("/hello")
       public String hello() {
           return "hello world";
       }
   
   }
   ```

   回顾一下之前的，或者这样写

   ```java
   package com.zyy.controller;
   
   import org.springframework.stereotype.Controller;
   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.ResponseBody;
   
   /**
    * @Description: 类描述
    * @Author: zyy
    * @Date: 2022/07/14 21:54
    */
   @Controller
   @RequestMapping("/hello")
   public class HelloController {
       @GetMapping("/h1")
       @ResponseBody
       public String hello() {
           return "hello";
       }
   }
   ```

3. 编写完毕后，从主程序启动项目，浏览器发起请求，看页面返回；控制台输出了 Tomcat 访问的端口号！

   ![image-20220714214000458](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220714214000458.png)

   ![image-20220714213926042](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220714213926042.png)

**项目创建方式二：使用idea创建项目**

![image-20220714220013575](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220714220013575.png)



> 将项目打成jar包，点击maven的package

![image-20220714220249862](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220714220249862.png)

打开命令行输入`java -jar .\springboot-01-idea-0.0.1-SNAPSHOT.jar`也可启动服务

![image-20220714220345179](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220714220345179.png)



> 替换服务启动时候的banner

如何更改启动时显示的字符拼成的字母，SpringBoot呢？也就是 banner 图案；

只需一步：到项目下的 resources 目录下新建一个banner.txt 即可。

图案可以到：https://www.bootschool.net/ascii 这个网站生成，然后拷贝到文件中即可！

![image-20220714220550371](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220714220550371.png)



# 04、运行原理初探

我们之前写的helloword，到底是怎么运行的呢，Maven项目，我们一般从pom.xml文件探究起；

## 4.1、pom.xml

### 4.1.2、父依赖

其中它主要是依赖一个父项目，主要是管理项目的资源过滤及插件！

```xml
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.7.1</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
```

点进去，发现还有一个父依赖

```xml
<!-- ... -->
  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.7.1</version>
  </parent>
<!-- ... -->
```

再点进去，这里才是真正管理SpringBoot应用里面所有依赖版本的地方，SpringBoot的版本控制中心；

**以后我们导入依赖默认是不需要写版本；但是如果导入的包没有在依赖中管理着就需要手动配置版本了；**



### 4.1.2、启动器spring-boot-starter

```xml
<!-- ... -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
<!-- ... -->
```

**springboot-boot-starter-xxx**：就是spring-boot的场景启动器

**spring-boot-starter-web**：帮我们导入了web模块正常运行所依赖的组件

SpringBoot将所有的功能场景都抽取出来，做成一个个的starter （启动器），只需要在项目中引入这些starter即可，所有相关的依赖都会导入进来 ， 我们要用什么功能就导入什么样的场景启动器即可 ；我们未来也可以自己自定义 starter；

拓展：maven中的dependencyManagement

![image-20220717110341730](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220717110341730.png)

## 4.2、主启动类

**默认的主启动类**

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Springboot01IdeaApplication {

    public static void main(String[] args) {
        SpringApplication.run(Springboot01IdeaApplication.class, args);
    }

}
```

**@SpringBootApplication**

作用：标注在某个类上说明这个类是SpringBoot的主配置类 ， SpringBoot就应该运行这个类的main方法来启动SpringBoot应用；

进入这个注解：可以看到上面还有很多其他注解！

```java
//...
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = { @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
		@Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {
    //...
}
//...
```

**@ComponentScan**

这个注解在spring中很重要，它对应xml配置中的元素

作用：自动扫描并加载符合条件的附件或者bean，将这个bean定义加载到IOC容器中



**@SpringBootConfiguration**

作用：springboot的配置类，标准在某个类上，表示这是一个springboot的配置类

点进去这个注解，查看

```java
//...
@Configuration
@Indexed
public @interface SpringBootConfiguration {
    //...
}
//...
```

@Configuration

```java
//...
@Component
public @interface Configuration {
    //...
}
//...
```

这里的@Component，说明这个一个配置类，配置类就是对应spring的xml配置文件

里面的@Component这就说明启动类本身也是spring中的一个组件，负责启动应用。



**@EnableAutoConfiguration**开启自助配置功能

以前我们需要自己配置的东西，现在springboot可以自助帮我们配置

@EnableAutoConfiguration告诉springboot开启启动配置功能，这样自动配置才能生效

```java
//...
@AutoConfigurationPackage
@Import(AutoConfigurationImportSelector.class)
public @interface EnableAutoConfiguration {
    //...
}
//...
```

**@AutoConfigurationPackage**自动配置包

```java
//...
@Import(AutoConfigurationPackages.Registrar.class)
public @interface AutoConfigurationPackage {
    //...
}
//...
```

@Import：spring底层注解@Import，给容器中导入一个组件

Registrar.class作用：将主启动类的所在包及包下面所有子包里面的所有㢟扫描到spring容器

**@Import(AutoConfigurationImportSelector.class)**给容器导入组件

AutoConfigurationImportSelector：自助配置导入选择器

那么它会导入哪些组件的选择器呢？我们点开一下源码：

1. 这个类中有一个这样的方法

   ```java
   // 获取候选配置
   protected List<String> getCandidateConfigurations(AnnotationMetadata metadata, AnnotationAttributes attributes) {
       List<String> configurations = new ArrayList<>(
           SpringFactoriesLoader.loadFactoryNames(getSpringFactoriesLoaderFactoryClass(), getBeanClassLoader()));
       ImportCandidates.load(AutoConfiguration.class, getBeanClassLoader()).forEach(configurations::add);
       Assert.notEmpty(configurations,
                       "No auto configuration classes found in META-INF/spring.factories nor in META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports. If you "
                       + "are using a custom packaging, make sure that file is correct.");
       return configurations;
   }
   ```

   注意这句(新版本看`META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports`)

   ```
   No auto configuration classes found in META-INF/spring.factories 
   nor 
   in META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports. 
   If you are using a custom packaging, make sure that file is correct.
   ```

2. 这个方法又调用了SpringFactoriesLoader的静态方法，我们进去loadFactoryNames方法

   ```java
   public static List<String> loadFactoryNames(Class<?> factoryType, @Nullable ClassLoader classLoader) {
       ClassLoader classLoaderToUse = classLoader;
       if (classLoaderToUse == null) {
           classLoaderToUse = SpringFactoriesLoader.class.getClassLoader();
       }
       String factoryTypeName = factoryType.getName();
       //这里又调用了loadSpringFactories方法
       return loadSpringFactories(classLoaderToUse).getOrDefault(factoryTypeName, Collections.emptyList());
   }
   ```

3. 我们继续点击查看loadSpringFactories方法

   ```java
   private static Map<String, List<String>> loadSpringFactories(ClassLoader classLoader) {
       Map<String, List<String>> result = cache.get(classLoader);
       if (result != null) {
           return result;
       }
   
       result = new HashMap<>();
       try {
           Enumeration<URL> urls = classLoader.getResources(FACTORIES_RESOURCE_LOCATION);
           while (urls.hasMoreElements()) {
               URL url = urls.nextElement();
               UrlResource resource = new UrlResource(url);
               Properties properties = PropertiesLoaderUtils.loadProperties(resource);
               for (Map.Entry<?, ?> entry : properties.entrySet()) {
                   String factoryTypeName = ((String) entry.getKey()).trim();
                   String[] factoryImplementationNames =
                       StringUtils.commaDelimitedListToStringArray((String) entry.getValue());
                   for (String factoryImplementationName : factoryImplementationNames) {
                       result.computeIfAbsent(factoryTypeName, key -> new ArrayList<>())
                           .add(factoryImplementationName.trim());
                   }
               }
           }
   
           // Replace all lists with unmodifiable lists containing unique elements
           result.replaceAll((factoryType, implementations) -> implementations.stream().distinct()
                             .collect(Collectors.collectingAndThen(Collectors.toList(), Collections::unmodifiableList)));
           cache.put(classLoader, result);
       }
       catch (IOException ex) {
           throw new IllegalArgumentException("Unable to load factories from location [" +
                                              FACTORIES_RESOURCE_LOCATION + "]", ex);
       }
       return result;
   }
   ```

   FACTORIES_RESOURCE_LOCATION

   ```java
   public static final String FACTORIES_RESOURCE_LOCATION = "META-INF/spring.factories";
   ```

4. 发现一个多次出现的文件：spring.factories，全局搜索它

   

**spring.factories** (ps：最新的版本将被弃用)

我们根据源头打开spring.factories ， 看到了很多自动配置的文件；这就是自动配置根源所在！

![image-20220717103419111](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220717103419111.png)

新版本看

![image-20220717111604210](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220717111604210.png)

我们在上面的自动配置类随便找一个打开看看，比如 ：WebMvcAutoConfiguration

![image-20220717104853321](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220717104853321.png)

@AutoConfiguration

```java
//...
@Configuration(proxyBeanMethods = false)
@AutoConfigureBefore
@AutoConfigureAfter
public @interface AutoConfiguration {
    //...
}
//...
```

可以看到这些一个个的都是JavaConfig配置类，而且都注入了一些Bean，可以找一些自己认识的类，看着熟悉一下！

所以，自动配置真正实现是从classpath中搜寻所有的META-INF/spring.factories配置文件 ，并将其中对应的 org.springframework.boot.autoconfigure. 包下的配置项，通过反射实例化为对应标注了 @Configuration的JavaConfig形式的IOC容器配置类 ， 然后将这些都汇总成为一个实例并加载到IOC容器中。



## 4.3、结论

1. springboot在启动的时候从类路径在的META-INF/spring.factories中获取@EnableAutoConfiguration指定的值（新版本从`META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports`中获取）
2. 将这些值作为自动配置类导入容器，自助配置类就生效，帮我们进行自动配置工作
3. 整个J2EE的整体解决方法和自动配置都在springboot-autoconfigure的jar包中
4. 它会给容器中导入非常多的自动配置类（***AutoConfiguration），就是给容器中导入这个场景需要的所有组件，并配置好这些组件
5. 有了自动配置类，免去了我们手动编写配置注入功能组件等的工作



## 4.4、SpringApplication

我最初以为就是运行了一个main方法，没想到却开启了一个服务；

```java
//...
@SpringBootApplication
public class Springboot01IdeaApplication {

    public static void main(String[] args) {
        SpringApplication.run(Springboot01IdeaApplication.class, args);
    }

}
```

**SpringApplication.run分析**

分析该方法主要分两部分，一部分是SpringApplication的实例化，二是run方法的执行；

> SpringApplication

**这个类主要做了以下四件事情：**

1. 推断应用的类型是普通的项目还是web项目
2. 查找并加载所有可用初始化器，设置到initializers属性中
3. 找出所有的应用程序监听器，设置到listeners属性中
4. 推断并设置main方法的定义类，找到运行的主类

查看构造器：

```java
//...
	public SpringApplication(ResourceLoader resourceLoader, Class<?>... primarySources) {
		this.resourceLoader = resourceLoader;
		Assert.notNull(primarySources, "PrimarySources must not be null");
		this.primarySources = new LinkedHashSet<>(Arrays.asList(primarySources));
		this.webApplicationType = WebApplicationType.deduceFromClasspath();
		this.bootstrapRegistryInitializers = new ArrayList<>(
				getSpringFactoriesInstances(BootstrapRegistryInitializer.class));
		setInitializers((Collection) getSpringFactoriesInstances(ApplicationContextInitializer.class));
		setListeners((Collection) getSpringFactoriesInstances(ApplicationListener.class));
		this.mainApplicationClass = deduceMainApplicationClass();
	}
//...
```

> run方法流程分析

![1111](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/1111.jpg)

# 05、yaml配置注入

## 5.1、yaml语法学习

> 配置文件

SpringBoot使用一个全局的配置文件 ， 配置文件名称是固定的

- application.properties

  语法结构：key=value

- application.yml

  语法结构：key: 空格value

**配置文件的作用 ：**修改SpringBoot自动配置的默认值，因为SpringBoot在底层都给我们自动配置好了；

比如我们可以在配置文件中修改Tomcat 默认启动的端口号！测试一下！

```properties
server.port=8081
```



> yaml概述

YAML是 "YAML Ain't a Markup Language" （YAML不是一种标记语言）的递归缩写。在开发的这种语言时，YAML 的意思其实是："Yet Another Markup Language"（仍是一种标记语言）

**这种语言以数据作为中心，而不是以标记语言为重点！**

以前的配置文件，大多数都是使用xml来配置；比如一个简单的端口配置，我们来对比下yaml和xml

传统xml配置

```xml
<server>
    <port>8081<port>
</server>
```

yaml配置

```yaml
server:
  port: 8080
```

> yaml基础语法

说明：语法要求严格

1. 空格不能省略
2. 以缩进来控制层级关系，只要是左边对齐的一列数据都是同一个层级的
3. 属性和值的大小写都是十分敏感的



**字面量：普通的值  [ 数字，布尔值，字符串  ]**

字面量直接写在后面就可以 ， 字符串默认不用加上双引号或者单引号；

```yaml
k: v
```

注意：

- ""双引号，不会转义字符串里面的特殊字符 ， 特殊字符会作为本身想表示的意思；

  比如 ：name: "kuang \n shen"  输出 ：kuang  换行  shen

- ''单引号，会转义特殊字符 ， 特殊字符最终会变成和普通字符一样输出

  比如 ：name: ‘kuang \n shen’  输出 ：kuang  \n  shen



**对象、Map（键值对）**

```yaml
#对象、Map格式
k: 
    v1:
    v2:
```

在下一行来写对象的属性和值的关系，注意缩进；比如：

```yaml
student: 
  name: zyy
  age: 18
```

行内写法

```yaml
student: {name: zyy, age: 18}
```

**数组（ List、set ）**

用 - 值表示数组中的一个元素,比如：

```yaml
pets:
  - cat
  - dog
  - pig
```

行内写法

```yaml
pets: [cat, dog, pig]
```



## 5.2、注入配置文件

yaml文件更强大的地方在于，他可以给我们的实体类直接注入匹配值！

> yaml注入配置文件

1. 在springboot项目中的resources目录下新建一个文件 application.yaml

2. 编写一个实体类Dog，并用@Value给bean注入属性值(注：通过配置文件给属性注入值，一定要写set方法，下面类其实可以暂时不用写set方法，不过后面会用到)

   ```java
   @Component //bean注入到容器中
   public class Dog {
       @Value("大黄")
       private String name;
       @Value("2")
       private int age;
   
       public void setName(String name) {
           this.name = name;
       }
   
       public void setAge(int age) {
           this.age = age;
       }
   
       @Override
       public String toString() {
           return "Dog{" +
                   "name='" + name + '\'' +
                   ", age=" + age +
                   '}';
       }
   }
   ```

3. 在测试类验证一下

   ```java
   @SpringBootTest
   class Springboot01IdeaApplicationTests {
   
       @Autowired
       private Dog dog;
       @Test
       void contextLoads() {
           System.out.println(dog);
       }
   }
   ```

4. 打印输入如下

   ```
   Dog{name='大黄', age=2}
   ```

   ![image-20220720112938609](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220720112938609.png)

5. 再新建一个复杂点的实体类Person

   ```java
   import org.springframework.boot.context.properties.ConfigurationProperties;
   import org.springframework.stereotype.Component;
   
   import java.util.Date;
   import java.util.List;
   import java.util.Map;
   
   
   /*
   @ConfigurationProperties作用：
   将配置文件中配置的每一个属性的值，映射到这个组件中；
   告诉SpringBoot将本类中的所有属性和配置文件中相关的配置进行绑定
   参数 prefix = “person” : 将配置文件中的person下面的所有属性一一对应
   */
   @Component //注册bean到容器中
   @ConfigurationProperties(prefix="person")
   public class Person {
       private String name;
       private int age;
   
       private boolean happy;
   
       private Date birth;
   
       private Map<String, Object> map;
   
       private List<Object> list;
   
       private Dog dog;
   
       public void setName(String name) {
           this.name = name;
       }
   
       public void setAge(int age) {
           this.age = age;
       }
   
       public void setHappy(boolean happy) {
           this.happy = happy;
       }
   
       public void setBirth(Date birth) {
           this.birth = birth;
       }
   
       public void setMap(Map<String, Object> map) {
           this.map = map;
       }
   
       public void setList(List<Object> list) {
           this.list = list;
       }
   
       public void setDog(Dog dog) {
           this.dog = dog;
       }
   
       @Override
       public String toString() {
           return "Person{" +
                   "name='" + name + '\'' +
                   ", age=" + age +
                   ", happy=" + happy +
                   ", birth=" + birth +
                   ", map=" + map +
                   ", list=" + list +
                   ", dog=" + dog +
                   '}';
       }
   }
   
   ```

6. IDEA 提示，springboot配置注解处理器没有找到，让我们看文档，我们可以查看文档，找到一个依赖！

   ![image-20220720115451426](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220720115451426.png)

   点击打开，跳转链接

   https://docs.spring.io/spring-boot/docs/2.7.1/reference/html/configuration-metadata.html#appendix.configuration-metadata.annotation-processor

   ![image-20220720115550298](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220720115550298.png)

   按照文档提供在pom.xml中导入依赖包,就不会报错了

   ```xml
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-configuration-processor</artifactId>
               <optional>true</optional>
           </dependency>
   ```

   

7. application.yaml中新增配置

   ```yaml
   person:
     name: zyy
     age: 18
     happy: true
     birth: 1995/11/20
     map: {k1: v1, k2: v2}
     list:
       - code
       - music
     dog:
       name: 小黄
       age: 1
   ```

8. 测试

   ```java
   @SpringBootTest
   class Springboot01IdeaApplicationTests {
   
       @Autowired
       private Person person;
       
       @Test
       void contextLoads() {
           System.out.println(person);
       }
   }
   ```

   ![image-20220720114838924](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220720114838924.png)



> 加载指定的配置文件

**@PropertySource ：**加载指定的配置文件；

**@ConfigurationProperties**：默认从全局配置文件中获取值；

1. 我们去在resources目录下新建一个**person.properties**文件

   ```properties
   name=zyy1234
   ```

2. 新建Person2，指定配置文件

   ```java
   @Component
   @PropertySource(value = "classpath:person.properties")
   public class Person2 {
       @Value("${name}")
       private String name;
       //...
   }
   ```

3. 测试，验证

   ![image-20220720140144117](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220720140144117.png)



> 配置文件占位符

配置文件还可以编写占位符生成随机数

```yaml
person:
  name: zyy${random.uuid}   # 随机uuid
  age: ${random.int} # 随机int
  happy: true
  birth: 1995/11/20
  map: {k1: v1, k2: v2}
  hello: z
  list:
    - code
    - music
  dog:
    name: ${person.hello:other}_小黄 # other代表默认值
    age: 1
```

打印结果如下

```
Person{name='zyye360040f-0b1b-4562-b555-fa409901a021', age=218521969, happy=true, birth=Mon Nov 20 00:00:00 CST 1995, map={k1=v1, k2=v2}, list=[code, music], dog=Dog{name='z_小黄', age=1}}
```

> 回顾properties配置

【注意】properties配置文件在写中文的时候，会有乱码 ， 我们需要去IDEA中设置编码格式为UTF-8；

![image-20220720164000038](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220720164000038.png)



1. 新建一个配置文件user.properties

   ```properties
   user.name=zyy
   user.age=18
   user.sex=男
   ```

2. 新建一个实体类User，并给属性注入值

   ```java
   import org.springframework.beans.factory.annotation.Value;
   import org.springframework.context.annotation.PropertySource;
   import org.springframework.stereotype.Component;
   
   @Component
   @PropertySource(value = "classpath:user.properties")
   public class User {
       /**
        * 从配置文件中取值
        */
       @Value("${user.name}")
       private String name;
       /**
        * #{SPEL} Spring表达式
        */
       @Value("#{9*2}")
       private int age;
       /**
        * 字面量
        */
       @Value("女")
       private String sex;
   
       @Override
       public String toString() {
           return "User{" +
                   "name='" + name + '\'' +
                   ", age=" + age +
                   ", sex='" + sex + '\'' +
                   '}';
       }
   }
   
   ```

3. 测试验证

   ```java
   import com.zyy.pojo.User;
   import org.junit.jupiter.api.Test;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.test.context.SpringBootTest;
   
   @SpringBootTest
   class Springboot01IdeaApplicationTests {
   
       @Autowired
       private User user;
   
       @Test
       void contextLoads() {
           System.out.println(user);
       }
   }
   ```

   ![image-20220720165810809](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220720165810809.png)



> 对比小结

|                | @ConfigurationProperties | @Value     |
| -------------- | ------------------------ | ---------- |
| 功能           | 批量注入配置文件中的属性 | 一个个指定 |
| 松散绑定       | 支持                     | 不支持     |
| SPEL           | 不支持                   | 支持       |
| JSR303数据校验 | 支持                     | 不支持     |
| 复杂类型封装   | 支持                     | 不支持     |

1. @ConfigurationProperties只需要写一次即可 ， @Value则需要每个字段都添加

2. 松散绑定：这个什么意思呢? 比如我的yml中写的last-name，这个和lastName是一样的， - 后面跟着的字母默认是大写的。这就是松散绑定。可以测试一下（注意set方法必须修改）

   ```yaml
   person:
     last-name: zyy${random.uuid}   # 随机uuid
     age: ${random.int} # 随机int
   # ....
   ```

   ```java
   //...
   @Component
   @ConfigurationProperties(prefix="person")
   public class Person {
       private String lastName;
       //...
       public void setLastName(String lastName) {
           this.lastName = lastName;
       }
       //...
   }
   ```

   这种也是可以正常输出的

   ![image-20220720170739635](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220720170739635.png)

3. JSR303数据校验 ， 这个就是我们可以在字段是增加一层过滤器验证 ， 可以保证数据的合法性

   导入jar包

   ```xml
           <dependency>
               <groupId>javax.validation</groupId>
               <artifactId>validation-api</artifactId>
           </dependency>
   
           <dependency>
               <groupId>org.hibernate.validator</groupId>
               <artifactId>hibernate-validator</artifactId>
           </dependency>
   ```

   实体类校验

   ```java
   //...
   @Component
   @ConfigurationProperties(prefix = "person")
   @Validated //数据校验
   public class Person {
       @NotBlank(message = "姓名不可为空")
       private String name;
       private int age;
       //...
   }
   ```

   application.yaml配置类

   ```yaml
   person:
     age: 18
   ```

   测试验证，报错如下

   ![image-20220720175908543](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220720175908543.png)

   拓展：

   ```
   部分代码：
   @NotNull(message="名字不能为空")
   private String userName;
   @Max(value=120,message="年龄最大不能查过120")
   private int age;
   @Email(message="邮箱格式错误")
   private String email;
   -----------------------
   空检查
   @Null       验证对象是否为null
   @NotNull    验证对象是否不为null, 无法查检长度为0的字符串
   @NotBlank   检查约束字符串是不是Null还有被Trim的长度是否大于0,只对字符串,且会去掉前后空格.
   @NotEmpty   检查约束元素是否为NULL或者是EMPTY.
       
   Booelan检查
   @AssertTrue     验证 Boolean 对象是否为 true  
   @AssertFalse    验证 Boolean 对象是否为 false  
       
   长度检查
   @Size(min=, max=) 验证对象（Array,Collection,Map,String）长度是否在给定的范围之内  
   @Length(min=, max=) string is between min and max included.
   
   日期检查
   @Past       验证 Date 和 Calendar 对象是否在当前时间之前  
   @Future     验证 Date 和 Calendar 对象是否在当前时间之后  
   @Pattern    验证 String 对象是否符合正则表达式的规则
   ```

4. 复杂类型封装，yml中可以封装对象 ， 使用value就不支持



**结论**

1. 配置yml和配置properties都可以获取到值 ， 强烈推荐 yml；
2. 如果我们在某个业务中，只需要获取配置文件中的某个值，可以使用一下 @value；
3. 如果说，我们专门编写了一个JavaBean来和配置文件进行一一映射，就直接@ConfigurationProperties，不要犹豫



# 06、多环境切换

profile是Spring对不同环境提供不同配置功能的支持，可以通过激活不同的环境版本，实现快速切换环境；

> 多配置文件

我们在主配置文件编写的时候，文件名可以是 application-{profile}.properties/yaml，用来指定多个环境版本

例如：

- application-test.properties 代表测试环境配置

- application-dev.properties 代表开发环境配置

但是Springboot并不会直接启动这些配置文件，它**默认使用application.properties主配置文件**；

我们需要通过一个配置来选择需要激活的环境：

application.properties

 ```properties
 #比如在配置文件中指定使用dev环境，我们可以通过设置不同的端口号进行测试；
 #我们启动SpringBoot，就可以看到已经切换到dev下的配置了；
 spring.profiles.active=dev
 ```

application-dev.properties

```properties
server.port=8081
```

application-stg.properties

```properties
server.port=8082
```

这里系统启动的端口就是8081



> yaml的多文档块

和properties配置文件中一样，也是可以创建多个配置文件，yml也可以在同一个文件中区分配置（不推荐使用）

```yaml
#选择要激活那个环境块
spring:
  profiles:
    active: prod
---
server:
  port: 8083
spring:
  profiles: dev #配置环境的名称 不过现在已经废弃了，不推荐使用
---
server:
  port: 8084
spring:
  profiles: prod  #配置环境的名称
```

**注意：如果yml和properties同时都配置了端口，并且没有激活其他环境 ， 默认会使用properties配置文件的**



> 配置文件加载位置

**外部加载配置文件的方式十分多，我们选择最常用的即可，在开发的资源文件中进行配置！**

官方外部配置文件说明参考文档

 ![image-20220721141535246](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220721141535246.png)

springboot 启动会扫描以下位置的application.properties或者application.yml文件作为Spring boot的默认配置文件：

```
优先级1：项目路径下的config文件夹配置文件
优先级2：项目路径下的配置文件
优先级3：资源路径下的config文件夹配置文件
优先级4：资源路径下的配置文件
```

优先级由高到底，高优先级的配置会覆盖低优先级的配置；

**SpringBoot会从这四个位置全部加载主配置文件；互补配置；**

![微信截图_20220721142419](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20220721142419.png)



> 拓展

指定位置加载配置文件

我们还可以通过spring.config.location来改变默认的配置文件位置

项目打包好以后，我们可以使用命令行参数的形式，启动项目的时候来指定配置文件的新位置；这种情况，一般是后期运维做的多，相同配置，外部指定的配置文件优先级最高

```bash
java -jar spring-boot-config.jar --spring.config.location=F:/application.properties
```



# 07、自动配置原理

> 分析自动配置原理

我们以**HttpEncodingAutoConfiguration（Http编码自动配置）**为例解释自动配置原理；

```java
//表示这是一个配置类
@AutoConfiguration
//启动指定类的ConfigurationProperties功能；
//--进入这个HttpProperties查看，将配置文件中对应的值和HttpProperties绑定起来；
//--并把HttpProperties加入到ioc容器中
@EnableConfigurationProperties(ServerProperties.class)
//Spring底层@Conditional注解
//--根据不同的条件判断，如果满足指定的条件，整个配置类里面的配置就会生效；
//--这里的意思就是判断当前应用是否是web应用，如果是，当前配置类生效
@ConditionalOnWebApplication(type = ConditionalOnWebApplication.Type.SERVLET)
//判断当前项目有没有这个类CharacterEncodingFilter；SpringMVC中进行乱码解决的过滤器；
@ConditionalOnClass(CharacterEncodingFilter.class)
//判断配置文件中是否存在某个配置：spring.http.encoding.enabled；
//--如果不存在，判断也是成立的
//--使我们配置文件中不配置pring.http.encoding.enabled=true，也是默认生效的
@ConditionalOnProperty(
    prefix = "server.servlet.encoding", 
    value = "enabled", 
    matchIfMissing = true)
public class HttpEncodingAutoConfiguration {

	private final Encoding properties;

	public HttpEncodingAutoConfiguration(ServerProperties properties) {
		this.properties = properties.getServlet().getEncoding();
	}
    //给容器中添加一个组件，这个组件的某些值需要从properties中获取
	@Bean
	@ConditionalOnMissingBean //判断容器没有这个组件
	public CharacterEncodingFilter characterEncodingFilter() {
		CharacterEncodingFilter filter = new OrderedCharacterEncodingFilter();
		filter.setEncoding(this.properties.getCharset().name());
		filter.setForceRequestEncoding(this.properties.shouldForce(Encoding.Type.REQUEST));
		filter.setForceResponseEncoding(this.properties.shouldForce(Encoding.Type.RESPONSE));
		return filter;
	}
    //...
}
```

**一句话总结：根据当前不同的条件判断，决定这个配置类是否生效！**

- 一但这个配置类生效；这个配置类就会给容器中添加各种组件
- 这些组件的属性是从对应的properties类中获取的，这些类里面的每一个属性又是和配置文件绑定的
- 所有在配置文件中能配置的属性都是在***Properties类中封装着
- 配置文件能配置什么就可以参照某个功能对应的这个属性类

```java
@ConfigurationProperties(prefix = "server", ignoreUnknownFields = true)
public class ServerProperties {

	/**
	 * Server HTTP port.
	 */
	private Integer port;

	/**
	 * Network address to which the server should bind.
	 */
	private InetAddress address;
    //...
}
```

我们去配置文件里面试试前缀，看提示！

![image-20220721173500948](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220721173500948.png)

**这就是自动装配的原理！**

> 精髓

1. SpringBoot启动会加载大量的自动配置类
2. 我们看我们需要的功能有没有在SpringBoot默认写好的自动配置类当中
3. 我们再来看这个自动配置类中到底配置了哪些组件；（只要我们要用的组件存在在其中，我们就不需要再手动配置了）
4. 给容器中自动配置类添加组件的时候，会从properties类中获取某些属性。我们只需要在配置文件中指定这些属性的值即可；

**xxxxAutoConfigurartion：自动配置类；**给容器中添加组件

**xxxxProperties:封装配置文件中相关属性；**

> 了解：@Condition

了解完自动装配的原理后，我们来关注一个细节问题，**自动配置类必须在一定的条件下才能生效；**

**@Conditional派生注解（Spring注解版原生的@Conditional作用）**

作用：必须是@Conditional指定的条件成立，才给容器中添加组件，配置配里面的所有内容才生效；

![image-20220721173854428](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220721173854428.png)

**那么多的自动配置类，必须在一定的条件下才能生效；也就是说，我们加载了这么多的配置类，但不是所有的都生效了。**

我们怎么知道哪些自动配置类生效？

**我们可以通过启用 debug=true属性；来让控制台打印自动配置报告，这样我们就可以很方便的知道哪些自动配置类生效；**

```properties
#开启springboot的调试类
debug=true
```

![image-20220721174059704](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220721174059704.png)

**Positive matches:（自动配置类启用的：正匹配）**

**Negative matches:（没有启动，没有匹配成功的自动配置类：负匹配）**

**Unconditional classes: （没有条件的类）**



# 08、自定义starter

> 说明

启动器模块是一个 空 jar 文件，仅提供辅助性依赖管理，这些依赖可能用于自动装配或者其他类库；

**命名归约：**

官方命名：

- 前缀：spring-boot-starter-xxx
- 比如：spring-boot-starter-web

自定义命名：

- xxx-spring-boot-starter
- 比如：mybatis-spring-boot-starter



TODO:后面再补充

# 09、静态资源处理

**SpringBoot最大的特点就是自动装配**

首先我们新建一个干净的springboot项目，导入web依赖

## 9.1、静态资源处理

> 静态资源映射规则

写请求非常简单，那我们要引入我们前端资源，我们项目中有许多的静态资源，比如css，js等文件，这个SpringBoot怎么处理呢？

如果我们是一个web应用，我们的main下会有一个webapp，我们以前都是将所有的页面导在这里面的，对吧！但是我们现在的pom呢，打包方式是为jar的方式，那么这种方式SpringBoot能不能来给我们写页面呢？当然是可以的，但是SpringBoot对于静态资源放置的位置，是有规定的！

**我们先来聊聊这个静态资源映射规则：**

SpringBoot中，SpringMVC的web配置都在 WebMvcAutoConfiguration 这个配置类里面；

内部类WebMvcAutoConfigurationAdapter中有很多配置方法

有一个方法：addResourceHandlers添加资源处理

```java
//...
		@Override
		public void addResourceHandlers(ResourceHandlerRegistry registry) {
			if (!this.resourceProperties.isAddMappings()) {
				logger.debug("Default resource handling disabled");
				return;
			}
            // webjar配置
			addResourceHandler(registry, "/webjars/**", "classpath:/META-INF/resources/webjars/");
            // 静态资源配置
			addResourceHandler(registry, this.mvcProperties.getStaticPathPattern(), (registration) -> {
				registration.addResourceLocations(this.resourceProperties.getStaticLocations());
				if (this.servletContext != null) {
					ServletContextResource resource = new ServletContextResource(this.servletContext, SERVLET_LOCATION);
					registration.addResourceLocations(resource);
				}
			});
		}
//...
```



> 什么是webjar呢？

Webjars本质就是以jar包的方式引入我们的静态资源 ， 我们以前要导入一个静态资源文件，直接导入即可。

使用SpringBoot需要使用Webjars，我们可以去搜索一下：

网站：https://www.webjars.org/all

要使用jQuery，我们只要要引入jQuery对应版本的pom依赖即可！

```xml
        <dependency>
            <groupId>org.webjars</groupId>
            <artifactId>jquery</artifactId>
            <version>3.4.1</version>
        </dependency>
```

导入完毕，查看webjars目录结构，并访问Jquery.js文件！

![image-20220721213225632](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220721213225632.png)

访问：只要是静态资源，SpringBoot就会去对应的路径寻找资源，我们这里访问：

http://localhost:8080/webjars/jquery/3.4.1/jquery.js

![image-20220721213256390](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220721213256390.png)



> 第二种静态资源映射规则

那我们项目中要是使用自己的静态资源该怎么导入呢？我们看下一行代码；

我们去找staticPathPattern发现第二种映射规则 ：/** , 访问当前的项目任意资源，它会去找 `resourceProperties`这个类，我们可以点进去看一下分析：

```java
//...

	private String staticPathPattern = "/**";
//...
    public String getStaticPathPattern() {
		return this.staticPathPattern;
	}
//...
```

```java
//...

		private static final String[] CLASSPATH_RESOURCE_LOCATIONS = { 
            "classpath:/META-INF/resources/",
            "classpath:/resources/", 
            "classpath:/static/", 
            "classpath:/public/" };
//...
		private String[] staticLocations = CLASSPATH_RESOURCE_LOCATIONS;
//...
		public String[] getStaticLocations() {
			return this.staticLocations;
		}
//...
```

ResourceProperties 可以设置和我们静态资源有关的参数；这里面指向了它会去寻找资源的文件夹，即上面数组的内容。

所以得出结论，以下四个目录存放的静态资源可以被我们识别：

```
            "classpath:/META-INF/resources/",
            "classpath:/resources/", 
            "classpath:/static/", 
            "classpath:/public/" 
```

我们可以在resources根目录下新建对应的文件夹，都可以存放我们的静态文件；

比如我们访问 http://localhost:8080/1.html , 他就会去这些文件夹中寻找对应的静态资源文件；

![image-20220721214355419](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220721214355419.png)

验证

![image-20220721214421973](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220721214421973.png)

发现优先级：`resources > static > public`



> 自定义静态资源路径

我们也可以自己通过配置文件来指定一下，哪些文件夹是需要我们放静态资源文件的，在application.properties中配置

```properties

spring.web.resources.static-locations=classpath:/coding/,classpath:/zyy/
```

一旦自己定义了静态文件夹的路径，原来的自动配置就都会失效了！

![image-20220721215239252](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220721215239252.png)

这个时候1.html就直接访问不了了，2.html就可以访问

![image-20220721215315426](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220721215315426.png)



![image-20220721215327416](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220721215327416.png)

## 9.2、首页处理

静态资源文件夹说完后，我们继续向下看源码！可以看到一个欢迎页的映射，就是我们的首页！

WebMvcAutoConfiguration

```java
//...
@Bean
public WelcomePageHandlerMapping welcomePageHandlerMapping(ApplicationContext applicationContext,
                                                           FormattingConversionService mvcConversionService, ResourceUrlProvider mvcResourceUrlProvider) {
    WelcomePageHandlerMapping welcomePageHandlerMapping = new WelcomePageHandlerMapping(
        new TemplateAvailabilityProviders(applicationContext), applicationContext, getWelcomePage(),
        this.mvcProperties.getStaticPathPattern());
    welcomePageHandlerMapping.setInterceptors(getInterceptors(mvcConversionService, mvcResourceUrlProvider));
    welcomePageHandlerMapping.setCorsConfigurations(getCorsConfigurations());
    return welcomePageHandlerMapping;
}
//..
private Resource getWelcomePage() {
    for (String location : this.resourceProperties.getStaticLocations()) {
        Resource indexHtml = getIndexHtml(location);
        if (indexHtml != null) {
            return indexHtml;
        }
    }
    ServletContext servletContext = getServletContext();
    if (servletContext != null) {
        return getIndexHtml(new ServletContextResource(servletContext, SERVLET_LOCATION));
    }
    return null;
}
//...
private Resource getIndexHtml(String location) {
    return getIndexHtml(this.resourceLoader.getResource(location));
}

private Resource getIndexHtml(Resource location) {
    try {
        //欢迎页就是一个location下的的 index.html  
        //location其实就是
        //--"classpath:/META-INF/resources/",
		//--"classpath:/resources/", 
        //--"classpath:/static/", 
        //--"classpath:/public/"
        Resource resource = location.createRelative("index.html");
        if (resource.exists() && (resource.getURL() != null)) {
            return resource;
        }
    }
    catch (Exception ex) {
    }
    return null;
}
//...
```

那么我们在指定目录下新增一个index.html，比如：public下

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>首页</h1>
</body>
</html>
```

![image-20220722113013079](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220722113013079.png)

然后启动服务，验证功能

![image-20220722113052037](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220722113052037.png)

> 图标说明

与其他静态资源一样，Spring Boot在配置的静态内容位置中查找 favicon.ico。如果存在这样的文件，它将自动用作应用程序的favicon。



之前的修改配置已经被弃用了

```properties
spring.mvc.favicon.enabled=false
```

其实自己直接放一个图标在静态资源目录下即可，我放在 public 目录下

![ico](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/favicon.ico)

![image-20220722114346860](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220722114346860.png)

启动服务，清除浏览器缓存！刷新网页，发现图标已经变成自己的了！

![image-20220722114412584](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220722114412584.png)





# 10、Thymeleaf模板引擎

> 模板引擎

前端交给我们的页面，是html页面。如果是我们以前开发，我们需要把他们转成jsp页面，jsp好处就是当我们查出一些数据转发到JSP页面以后，我们可以用jsp轻松实现数据的显示，及交互等。

jsp支持非常强大的功能，包括能写Java代码，但是呢，我们现在的这种情况，SpringBoot这个项目首先是以jar的方式，不是war，像第二，我们用的还是嵌入式的Tomcat，所以呢，**他现在默认是不支持jsp的**。

那不支持jsp，如果我们直接用纯静态页面的方式，那给我们开发会带来非常大的麻烦，那怎么办呢？

**SpringBoot推荐你可以来使用模板引擎：**

模板引擎，我们其实大家听到很多，其实jsp就是一个模板引擎，还有用的比较多的freemarker，包括SpringBoot给我们推荐的Thymeleaf，模板引擎有非常多，但再多的模板引擎，他们的思想都是一样的，什么样一个思想呢我们来看一下这张图：

![image-20220726224213168](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220726224213168.png)

模板引擎的作用就是我们来写一个页面模板，比如有些值呢，是动态的，我们写一些表达式。

而这些值，从哪来呢，就是我们在后台封装一些数据。

然后把这个模板和这个数据交给我们模板引擎，

模板引擎按照我们这个数据帮你把这表达式解析、填充到我们指定的位置，

然后把这个数据最终生成一个我们想要的内容给我们写出去，

这就是我们这个模板引擎，

不管是jsp还是其他模板引擎，都是这个思想。



> 引入thymelead

Thymeleaf 官网：https://www.thymeleaf.org/

Thymeleaf 在Github 的主页：https://github.com/thymeleaf/thymeleaf

Spring官方文档：找到我们对应的版本

https://docs.spring.io/spring-boot/docs/2.7.2/reference/htmlsingle/#using-boot-starter

找到对应的pom依赖：可以适当点进源码看下本来的包！

```xml
        <!--thymeleaf-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
```

Maven会自动下载jar包，我们可以去看下下载的东西；

![image-20220727222237960](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220727222237960.png)

> thymeleaf分析

前面呢，我们已经引入了Thymeleaf，那这个要怎么使用呢？

我们首先得按照SpringBoot的自动配置原理看一下我们这个Thymeleaf的自动配置规则，在按照那个规则，我们进行使用。

我们去找一下Thymeleaf的自动配置类：ThymeleafProperties

 ```java
 //...
 @ConfigurationProperties(prefix = "spring.thymeleaf")
 public class ThymeleafProperties {
     
 	private static final Charset DEFAULT_ENCODING = StandardCharsets.UTF_8;
 
 	public static final String DEFAULT_PREFIX = "classpath:/templates/";
 
 	public static final String DEFAULT_SUFFIX = ".html";
     //...
 
 }
 ```

我们可以在其中看到默认的前缀和后缀！

我们只需要把我们的html页面放在类路径下的templates下，thymeleaf就可以帮我们自动渲染了。

使用thymeleaf什么都不需要配置，只需要将他放在指定的文件夹下即可！

> 测试

1. 编写一个TestController

   ```java
   @Controller
   public class TestController {
       @RequestMapping("/t1")
       public String test1() {
           return "test";
       }
   }
   ```

2. 编写一个测试页面  test.html 放在 templates 目录下

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
   </head>
   <body>
   <h1>hello</h1>
   </body>
   </html>
   ```

3. 启动项目请求测试

   ![image-20220727222915428](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220727222915428.png)

> thymeleaf语法学习

要学习语法，还是参考官网文档最为准确，我们找到对应的版本看一下；

Thymeleaf 官网：https://www.thymeleaf.org/ ， 简单看一下官网！我们去下载Thymeleaf的官方文档！

**我们做个最简单的练习 ：我们需要查出一些数据，在页面中展示**

1. 修改测试请求

   ```java
   @Controller
   public class TestController {
       @RequestMapping("/t1")
       public String test1(Model model) {
           model.addAttribute("msg", "hello,thymeleaf");
           return "test";
       }
   }
   ```

2. 我们要使用thymeleaf，需要在html文件中导入命名空间的约束，方便提示。

   我们可以去官方文档的#3中看一下命名空间拿来过来：

   ```html
   <html xmlns:th="http://www.thymeleaf.org">
   ```

3. 优化前端页面

   ```html
   <!DOCTYPE html>
   <html xmlns:th="http://www.thymeleaf.org">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
   </head>
   <body>
   <h1>hello</h1>
   <div th:text="#{msg}"></div>
   </body>
   </html>
   ```

4. 启动服务，验证功能

   ![image-20220727223720130](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220727223720130.png)

**OK，入门搞定，我们来认真研习一下Thymeleaf的使用语法！**

**1、我们可以使用任意的 th:attr 来替换Html中原生属性的值！**

![image-20220727223849746](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220727223849746.png)

**2、我们能写哪些表达式呢？**

 ```
 Simple expressions:（表达式语法）
   Variable Expressions: ${...}   获取变量值；
   Selection Variable Expressions: *{...}   选择表达式：和${}在功能上是一样
   Message Expressions: #{...}  获取国际化内容
   Link URL Expressions: @{...}  定义URL
   Fragment Expressions: ~{...}  片段引用表达式
   
  
 使用内置的基本对象：
   #ctx : the context object.
   #vars: the context variables.
   #locale : the context locale.
   #request : (only in Web Contexts) the HttpServletRequest object.
   #response : (only in Web Contexts) the HttpServletResponse object.
   #session : (only in Web Contexts) the HttpSession object.
   #servletContext : (only in Web Contexts) the ServletContext object.
 
 内置的一些工具对象：
   #execInfo : information about the template being processed.
   #uris : methods for escaping parts of URLs/URIs
   #conversions : methods for executing the configured conversion service (if any).
   #dates : methods for java.util.Date objects: formatting, component extraction, etc.
   #calendars : analogous to #dates , but for java.util.Calendar objects.
   #numbers : methods for formatting numeric objects.
   #strings : methods for String objects: contains, startsWith, prepending/appending, etc.
   #objects : methods for objects in general.
   #bools : methods for boolean evaluation.
   #arrays : methods for arrays.
   #lists : methods for lists.
   #sets : methods for sets.
   #maps : methods for maps.
   #aggregates : methods for creating aggregates on arrays or collections.
 ==================================================================================
 
 Literals（字面量）
       Text literals: 'one text' , 'Another one!' ,…
       Number literals: 0 , 34 , 3.0 , 12.3 ,…
       Boolean literals: true , false
       Null literal: null
       Literal tokens: one , sometext , main ,…
       
 Text operations:（文本操作）
     String concatenation: +
     Literal substitutions: |The name is ${name}|
     
 Arithmetic operations:（数学运算）
     Binary operators: + , - , * , / , %
     Minus sign (unary operator): -
     
 Boolean operations:（布尔运算）
     Binary operators: and , or
     Boolean negation (unary operator): ! , not
     
 Comparisons and equality:（比较运算）
     Comparators: > , < , >= , <= ( gt , lt , ge , le )
     Equality operators: == , != ( eq , ne )
     
 Conditional operators:条件运算（三元运算符）
     If-then: (if) ? (then)
     If-then-else: (if) ? (then) : (else)
     Default: (value) ?: (defaultvalue)
     
 Special tokens:
     No-Operation: _
 ```

练习测试：

1. 优化controller

   ```java
   @Controller
   public class TestController {
       @RequestMapping("/t1")
       public String test1(Model model) {
           model.addAttribute("msg", "<h1>hello,thymeleaf</h1>");
           model.addAttribute("msg2", "<h1>hello,thymeleaf</h1>");
           model.addAttribute("users", Arrays.asList("zyy", "zyy1", "zyy2"));
           return "test";
       }
   }
   ```

2. 优化html

   ```html
   <!DOCTYPE html>
   <html xmlns:th="http://www.thymeleaf.org">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
   </head>
   <body>
   <h1>hello</h1>
   <div th:text="${msg}"></div>
   <!--不转义-->
   <div th:utext="${msg2}"></div>
   
   <h4 th:each="user:${users}" th:text="${user}"></h4>
   
   <h4>
       <!--行内写法：官网#12-->
       <span th:each="user:${users}">[[${user}]]</span>
   </h4>
   
   </body>
   </html>
   ```

3. 启动服务，验证功能

   ![image-20220727225039819](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220727225039819.png)



**我们看完语法，很多样式，我们即使现在学习了，也会忘记，所以我们在学习过程中，需要使用什么，根据官方文档来查询，才是最重要的，要熟练使用官方文档！**



# 11、mvc自动配置原理

> 官网阅读

在进行项目编写前，我们还需要知道一个东西，就是**SpringBoot对我们的SpringMVC还做了哪些配置，包括如何扩展，如何定制**。

只有把这些都搞清楚了，我们在之后使用才会更加得心应手。途径一：源码分析，途径二：官方文档！

官网位置：https://docs.spring.io/spring-boot/docs/2.7.2/reference/htmlsingle/#web.servlet.spring-mvc.auto-configuration

```
//Spring Boot 为 Spring MVC 提供了自动配置，适用于大多数应用程序。
Spring Boot provides auto-configuration for Spring MVC that works well with most applications.


//自动配置在 Spring 的默认值之上添加了以下特性：
The auto-configuration adds the following features on top of Spring’s defaults:
//包含视图解析器
- Inclusion of ContentNegotiatingViewResolver and BeanNameViewResolver beans.
//支持提供静态资源，包括对 WebJars 的支持（本文档后面会介绍）。
- Support for serving static resources, including support for WebJars (covered later in this document).
//自动注册了Converter
//转换器，这就是我们网页提交数据到后台自动封装成为对象的东西，比如把"1"字符串自动转换为int类型
//Formatter：【格式化器，比如页面给我们了一个2019-8-10，它会给我们自动格式化为Date对象】
- Automatic registration of Converter, GenericConverter, and Formatter beans.
//SpringMVC用来转换Http请求和响应的的，比如我们要把一个User对象转换为JSON字符串，可以去看官网文档解释
- Support for HttpMessageConverters (covered later in this document).
//定义错误代码生成规则的
- Automatic registration of MessageCodesResolver (covered later in this document).
//首页定制
- Static index.html support.
//初始化数据绑定器：帮我们把请求数据绑定到JavaBean中！
- Automatic use of a ConfigurableWebBindingInitializer bean (covered later in this document).


//如果您想保留那些 Spring Boot MVC 自定义并进行更多 MVC 自定义（拦截器、格式化程序、视图控制器和其他功能），
//您可以添加自己的WebMvcConfigurer 类型的 @Configuration 类，但不添加 @EnableWebMvc。
If you want to keep those Spring Boot MVC customizations and make more MVC customizations (interceptors, formatters, view controllers, and other features), you can add your own @Configuration class of type WebMvcConfigurer but without @EnableWebMvc.


//如果您想提供 RequestMappingHandlerMapping、RequestMappingHandlerAdapter 
//或 ExceptionHandlerExceptionResolver 的自定义实例，并且仍然保留 Spring Boot MVC 自定义，
//您可以声明一个 WebMvcRegistrations 类型的 bean 并使用它来提供这些组件的自定义实例。
If you want to provide custom instances of RequestMappingHandlerMapping, RequestMappingHandlerAdapter, or ExceptionHandlerExceptionResolver, and still keep the Spring Boot MVC customizations, you can declare a bean of type WebMvcRegistrations and use it to provide custom instances of those components.


//如果您想完全控制 Spring MVC，您可以添加自己的带有 @EnableWebMvc 注释的 @Configuration，
//或者添加您自己的 @Configuration-annotated DelegatingWebMvcConfiguration，
//如 @EnableWebMvc 的 Javadoc 中所述。
If you want to take complete control of Spring MVC, you can add your own @Configuration annotated with @EnableWebMvc, or alternatively add your own @Configuration-annotated DelegatingWebMvcConfiguration as described in the Javadoc of @EnableWebMvc.
```



> ContentNegotiatingViewResolver 内容协商视图解析器

自动配置了ViewResolver，就是我们之前学习的SpringMVC的视图解析器；

即根据方法的返回值取得视图对象（View），然后由视图对象决定如何渲染（转发，重定向）。

我们去看看这里的源码：我们找到 WebMvcAutoConfiguration，其内部类WebMvcAutoConfigurationAdapter下的viewResolver方法

```java
//...
@Bean
@ConditionalOnBean(ViewResolver.class)
@ConditionalOnMissingBean(name = "viewResolver", value = ContentNegotiatingViewResolver.class)
public ContentNegotiatingViewResolver viewResolver(BeanFactory beanFactory) {
    ContentNegotiatingViewResolver resolver = new ContentNegotiatingViewResolver();
    resolver.setContentNegotiationManager(beanFactory.getBean(ContentNegotiationManager.class));
    // ContentNegotiatingViewResolver使用所有其他视图解析器来定位视图，因此它应该具有较高的优先级
    resolver.setOrder(Ordered.HIGHEST_PRECEDENCE);
    return resolver;
}
//...
```

进入ContentNegotiatingViewResolver源码，发现实现了ViewResolver接口，接口中只有一个resolveViewName，在ContentNegotiatingViewResolver找到他的实现方法

```java
@Override
@Nullable
public View resolveViewName(String viewName, Locale locale) throws Exception {
    RequestAttributes attrs = RequestContextHolder.getRequestAttributes();
    Assert.state(attrs instanceof ServletRequestAttributes, "No current ServletRequestAttributes");
    List<MediaType> requestedMediaTypes = getMediaTypes(((ServletRequestAttributes) attrs).getRequest());
    if (requestedMediaTypes != null) {
        //获取候选的视图对象  方法源码见下方
        List<View> candidateViews = getCandidateViews(viewName, locale, requestedMediaTypes);
        //选择一个最适合的视图对象，然后把这个对象返回
        View bestView = getBestView(candidateViews, requestedMediaTypes, attrs);
        if (bestView != null) {
            return bestView;
        }
    }

    String mediaTypeInfo = logger.isDebugEnabled() && requestedMediaTypes != null ?
        " given " + requestedMediaTypes.toString() : "";

    if (this.useNotAcceptableStatusCode) {
        if (logger.isDebugEnabled()) {
            logger.debug("Using 406 NOT_ACCEPTABLE" + mediaTypeInfo);
        }
        return NOT_ACCEPTABLE_VIEW;
    }
    else {
        logger.debug("View remains unresolved" + mediaTypeInfo);
        return null;
    }
}

//...

private List<View> getCandidateViews(String viewName, Locale locale, List<MediaType> requestedMediaTypes)
    throws Exception {

    List<View> candidateViews = new ArrayList<>();
    if (this.viewResolvers != null) {
        Assert.state(this.contentNegotiationManager != null, "No ContentNegotiationManager set");
        for (ViewResolver viewResolver : this.viewResolvers) {
            View view = viewResolver.resolveViewName(viewName, locale);
            if (view != null) {
                candidateViews.add(view);
            }
            for (MediaType requestedMediaType : requestedMediaTypes) {
                List<String> extensions = this.contentNegotiationManager.resolveFileExtensions(requestedMediaType);
                for (String extension : extensions) {
                    String viewNameWithExtension = viewName + '.' + extension;
                    view = viewResolver.resolveViewName(viewNameWithExtension, locale);
                    if (view != null) {
                        candidateViews.add(view);
                    }
                }
            }
        }
    }
    if (!CollectionUtils.isEmpty(this.defaultViews)) {
        candidateViews.addAll(this.defaultViews);
    }
    return candidateViews;
}
//...
```

所以得出结论：**ContentNegotiatingViewResolver 这个视图解析器就是用来组合所有的视图解析器的** 

我们再去研究下他的组合逻辑，看到有个属性viewResolvers，看看它是在哪里进行赋值的！

 ```java
 @Override
 protected void initServletContext(ServletContext servletContext) {
     // 这里它是从beanFactory工具中获取容器中的所有视图解析器
     // ViewRescolver.class 把所有的视图解析器来组合的
     Collection<ViewResolver> matchingBeans =
         BeanFactoryUtils.beansOfTypeIncludingAncestors(obtainApplicationContext(), ViewResolver.class).values();
     if (this.viewResolvers == null) {
         this.viewResolvers = new ArrayList<>(matchingBeans.size());
         for (ViewResolver viewResolver : matchingBeans) {
             if (this != viewResolver) {
                 this.viewResolvers.add(viewResolver);
             }
         }
     }
     else {
         for (int i = 0; i < this.viewResolvers.size(); i++) {
             ViewResolver vr = this.viewResolvers.get(i);
             if (matchingBeans.contains(vr)) {
                 continue;
             }
             String name = vr.getClass().getName() + i;
             obtainApplicationContext().getAutowireCapableBeanFactory().initializeBean(vr, name);
         }
 
     }
     AnnotationAwareOrderComparator.sort(this.viewResolvers);
     this.cnmFactoryBean.setServletContext(servletContext);
 }
 ```

既然它是在容器中去找视图解析器，我们是否可以猜想，我们就可以去实现一个视图解析器了呢？

我们可以自己给容器中去添加一个视图解析器；这个类就会帮我们自动的将它组合进来；**我们去实现一下**

1. 新建一个配置类

   ```java
   @Configuration
   public class MyMvcConfig implements WebMvcConfigurer {
   
       @Bean
       public ViewResolver myViewResolver() {
           return new MyViewResolver();
       }
   
       public static class MyViewResolver implements ViewResolver {
           @Override
           public View resolveViewName(String viewName, Locale locale) throws Exception {
               return null;
           }
       }
   
   }
   ```

2. 怎么看我们自己写的视图解析器有没有起作用呢？

   我们给 DispatcherServlet 中的 doDispatch方法 加个断点进行调试一下，因为所有的请求都会走到这个方法中

   ![image-20220731103330994](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220731103330994.png)
   
3. 我们启动我们的项目，然后随便访问一个页面，看一下Debug信息

   找到视图解析器，我们看到我们自己定义的就在这里了；

   ![image-20220731103450424](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220731103450424.png)

   所以说，我们如果想要使用自己定制化的东西，我们只需要给容器中添加这个组件就好了！剩下的事情SpringBoot就会帮我们做了！



> 转换器和格式化器

`WebMvcAutoConfiguration`下的 EnableWebMvcConfiguration的`mvcConversionService`方法

```java
//...
@Bean
@Override
public FormattingConversionService mvcConversionService() {
    Format format = this.mvcProperties.getFormat();
    WebConversionService conversionService = new WebConversionService(new DateTimeFormatters()
                                                                      .dateFormat(format.getDate()).timeFormat(format.getTime()).dateTimeFormat(format.getDateTime()));
    addFormatters(conversionService);
    return conversionService;
}
//...
```

getFormat()

```java
//...
private final Format format = new Format();

//...
public Format getFormat() {
    return this.format;
}
//...
```

可以看到在我们的Properties文件中，我们可以进行自动配置它！

如果配置了自己的格式化方式，就会注册到Bean中生效，我们可以在配置文件中配置日期格式化的规则：

```properties
spring.mvc.format.date=dd/MM/yyyy
```

其余的就不一一举例了，大家可以下去多研究探讨即可！

> 修改SpringBoot的默认配置

这么多的自动配置，原理都是一样的，通过这个WebMVC的自动配置原理分析，我们要学会一种学习方式，通过源码探究，得出结论；这个结论一定是属于自己的，而且一通百通。

SpringBoot的底层，大量用到了这些设计细节思想，所以，没事需要多阅读源码！得出结论；

SpringBoot在自动配置很多组件的时候，先看容器中有没有用户自己配置的（如果用户自己配置@bean），如果有就用用户配置的，如果没有就用自动配置的；

如果有些组件可以存在多个，比如我们的视图解析器，就将用户配置的和自己默认的组合起来！

**扩展使用SpringMVC**  官方文档如下

```
If you want to keep those Spring Boot MVC customizations and make more MVC customizations (interceptors, formatters, view controllers, and other features), you can add your own @Configuration class of type WebMvcConfigurer but without @EnableWebMvc.
```

我们要做的就是编写一个@Configuration注解类，并且类型要为WebMvcConfigurer，还不能标注@EnableWebMvc注解；我们去自己写一个；我们新建一个包叫config，写一个类MyMvcConfig；

 ```java
 //因为类型要求为WebMvcConfigurer，所以我们实现其接口
 //可以使用自定义类扩展MVC的功能
 @Configuration
 public class MyMvcConfig implements WebMvcConfigurer {
 
     @Override
     public void addViewControllers(ViewControllerRegistry registry) {
         //浏览器 请求 /zyy  ，就会跳转到test页面
         registry.addViewController("/zyy").setViewName("test");
     }
 }
 ```

我们去浏览器访问一下：

![image-20220731113358622](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220731113358622.png)

**确实也跳转过来了！所以说，我们要扩展SpringMVC，官方就推荐我们这么去使用，既保SpringBoot留所有的自动配置，也能用我们扩展的配置！**

我们可以去分析一下原理：

1. WebMvcAutoConfiguration 是 SpringMVC的自动配置类，里面有一个类WebMvcAutoConfigurationAdapter

   ```java
   //...
   @Configuration(proxyBeanMethods = false)
   @Import(EnableWebMvcConfiguration.class)
   @EnableConfigurationProperties({ WebMvcProperties.class, WebProperties.class })
   @Order(0)
   public static class WebMvcAutoConfigurationAdapter implements WebMvcConfigurer, ServletContextAware {
       //...
   
   }
   //...
   ```

2. 我们点进EnableWebMvcConfiguration这个类看一下

   ```java
   //...
   @Configuration(proxyBeanMethods = false)
   @EnableConfigurationProperties(WebProperties.class)
   public static class EnableWebMvcConfiguration extends DelegatingWebMvcConfiguration implements ResourceLoaderAware {
       //...
   }
   //...
   ```

3. 它继承了一个父类：DelegatingWebMvcConfiguration

   ```java
   //...
   @Configuration(proxyBeanMethods = false)
   public class DelegatingWebMvcConfiguration extends WebMvcConfigurationSupport {
   
   	private final WebMvcConfigurerComposite configurers = new WebMvcConfigurerComposite();
   
       // 从容器中获取所有的webmvcConfigurer
   	@Autowired(required = false)
   	public void setConfigurers(List<WebMvcConfigurer> configurers) {
   		if (!CollectionUtils.isEmpty(configurers)) {
   			this.configurers.addWebMvcConfigurers(configurers);
   		}
   	}
       //...
   }
   //...
   ```

4. 我们可以在这个类中去寻找一个我们刚才设置的viewController当做参考，发现它调用了一个

   ```java
   //...
   @Override
   protected void addViewControllers(ViewControllerRegistry registry) {
       this.configurers.addViewControllers(registry);
   }
   //...
   ```

5. 在点进去

   ```java
   //...
   @Override
   public void addViewControllers(ViewControllerRegistry registry) {
       // 将所有的WebMvcConfigurer相关配置来一起调用！包括我们自己配置的和Spring给我们配置的
       for (WebMvcConfigurer delegate : this.delegates) {
           delegate.addViewControllers(registry);
       }
   }
   //...
   ```

所以得出结论：所有的WebMvcConfiguration都会被作用，不止Spring自己的配置类，我们自己的配置类当然也会被调用；



> 全面接管springmvc

官方文档：

```
If you want to take complete control of Spring MVC, you can add your own @Configuration annotated with @EnableWebMvc, or alternatively add your own @Configuration-annotated DelegatingWebMvcConfiguration as described in the Javadoc of @EnableWebMvc.
```

全面接管即：SpringBoot对SpringMVC的自动配置不需要了，所有都是我们自己去配置！

只需在我们的配置类中要加一个@EnableWebMvc。

我们看下如果我们全面接管了SpringMVC了，我们之前SpringBoot给我们配置的静态资源映射一定会无效，我们可以去测试一下；

不加注解之前，访问首页：

![image-20220731114703665](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220731114703665.png)

给配置类加上注解：@EnableWebMvc

```java
//...
@Configuration
@EnableWebMvc
public class MyMvcConfig implements WebMvcConfigurer {

    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        //浏览器 请求 /zyy  ，就会跳转到test页面
        registry.addViewController("/zyy").setViewName("test");
    }
}
//...
```

我们发现所有的SpringMVC自动配置都失效了！（实际并没有，这个后面研究一下。。。）

**当然，我们开发中，不推荐使用全面接管SpringMVC**

思考问题？为什么加了一个注解，自动配置就失效了！我们看下源码：

1. 这里发现它是导入了一个类，我们可以继续进去看

   ```java
   //...
   @Retention(RetentionPolicy.RUNTIME)
   @Target(ElementType.TYPE)
   @Documented
   @Import(DelegatingWebMvcConfiguration.class)
   public @interface EnableWebMvc {
   }
   ```

2. 它继承了一个父类`WebMvcConfigurationSupport`

   ```java
   //...
   @Configuration(proxyBeanMethods = false)
   public class DelegatingWebMvcConfiguration extends WebMvcConfigurationSupport {
      private final WebMvcConfigurerComposite configurers = new WebMvcConfigurerComposite();
   
       //从容器中获取所有的webmvcConfigurer
   	@Autowired(required = false)
   	public void setConfigurers(List<WebMvcConfigurer> configurers) {
   		if (!CollectionUtils.isEmpty(configurers)) {
   			this.configurers.addWebMvcConfigurers(configurers);
   		}
   	}
       //...
   }
   //...
   ```

3. 我们来回顾一下Webmvc自动配置类

   ```java
   //...
   @AutoConfiguration(after = { DispatcherServletAutoConfiguration.class, TaskExecutionAutoConfiguration.class,
   		ValidationAutoConfiguration.class })
   @ConditionalOnWebApplication(type = Type.SERVLET)
   @ConditionalOnClass({ Servlet.class, DispatcherServlet.class, WebMvcConfigurer.class })
   // 这个注解的意思就是：容器中没有这个组件的时候，这个自动配置类才生效
   @ConditionalOnMissingBean(WebMvcConfigurationSupport.class)
   @AutoConfigureOrder(Ordered.HIGHEST_PRECEDENCE + 10)
   public class WebMvcAutoConfiguration {
       //...
   }
   //...
   ```



总结一句话：@EnableWebMvc将WebMvcConfigurationSupport组件导入进来了；

而导入的WebMvcConfigurationSupport只是SpringMVC最基本的功能！

**在SpringBoot中会有非常多的扩展配置，只要看见了这个，我们就应该多留心注意~**