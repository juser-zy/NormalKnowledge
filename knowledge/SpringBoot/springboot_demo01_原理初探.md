### 自动配置：
#### 概览
pom.xml
- spring-boot-dependencied：核心依赖在父工程中
```xml
<parent>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-parent</artifactId>  
    <version>2.7.4</version>  
    <relativePath/> <!-- lookup parent from repository -->  
</parent>
```

启动器：
- 后续版本中隐藏起来了：可在spring-boot-starter-web依赖的里面找到
- 作用：
	- **springboot-boot-starter-xxx**：就是spring-boot的场景启动器
	- **spring-boot-starter-web**：帮我们导入了web模块正常运行所依赖的组件
	- SpringBoot将所有的功能场景都抽取出来，做成一个个的starter （启动器），只需要在项目中引入这些starter即可，所有相关的依赖都会导入进来 ， 我们要用什么功能就导入什么样的场景启动器即可 ；我们未来也可以自己自定义 starter；

```xml
<dependency>  
  <groupId>org.springframework.boot</groupId>  
  <artifactId>spring-boot-starter</artifactId>  
  <version>2.7.4</version>  
  <scope>compile</scope>  
</dependency>
```
#### 主程序

```java
@SpringBootApplication  
public class HelloworldApplication {  
  
    public static void main(String[] args) {  
        //将应用启动  
        SpringApplication.run(HelloworldApplication.class, args);  
    }  
  
}
```
- **注解：**

```java
@SpringBootConfiguration:springboot的配置
	@Configuration:spring配置类
		@Component:spring的组件
@EnableAutoConfiguration:自动配置
	@AutoConfigurationPackage:自动配置包
		@Import({AutoConfigurationPackages.Registrar.class}):自动配置'包注册'
	@Import({AutoConfigurationImportSelector.class}):自动配置导入选择器

//获取所有配置
//在AutoConfigurationImportSelector.class内部
List<String> configurations = this.getCandidateConfigurations(annotationMetadata, attributes);
```

- 获取候选的配置

```java
protected List<String> getCandidateConfigurations(AnnotationMetadata metadata, AnnotationAttributes attributes) {  
    List<String> configurations = new ArrayList(SpringFactoriesLoader.loadFactoryNames(this.getSpringFactoriesLoaderFactoryClass(), this.getBeanClassLoader()));  
    
    ImportCandidates.load(AutoConfiguration.class, this.getBeanClassLoader()).forEach(configurations::add);  
    
    Assert.notEmpty(configurations, "No auto configuration classes found in META-INF/spring.factories nor in META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports. If you are using a custom packaging, make sure that file is correct.");  
    return configurations;  
}
```

- 结论
1. springboot在启动的时候从类路径在的META-INF/spring.factories中获取@EnableAutoConfiguration指定的值（新版本从`META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports`中获取）
2. 将这些值作为自动配置类导入容器，自助配置类就生效，帮我们进行自动配置工作
3. 整个J2EE的整体解决方法和自动配置都在springboot-autoconfigure的jar包中
4. 它会给容器中导入非常多的自动配置类（***AutoConfiguration），就是给容器中导入这个场景需要的所有组件，并配置好这些组件
5. 有了自动配置类，免去了我们手动编写配置注入功能组件等的工作

- **SpringApplication.run分析**

**这个类主要做了以下四件事情：**

1. 推断应用的类型是普通的项目还是web项目
2. 查找并加载所有可用初始化器，设置到initializers属性中
3. 找出所有的应用程序监听器，设置到listeners属性中
4. 推断并设置main方法的定义类，找到运行的主类