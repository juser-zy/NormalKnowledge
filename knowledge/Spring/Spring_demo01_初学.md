### 导入依赖


**pom.xml**

```xml
<dependencies>  
    <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->  
    <dependency>  
        <groupId>org.springframework</groupId>  
        <artifactId>spring-webmvc</artifactId>  
        <version>5.3.22</version>  
    </dependency>  
  
    <dependency>  
        <groupId>junit</groupId>  
        <artifactId>junit</artifactId>  
        <version>4.12</version>  
        <scope>test</scope>  
    </dependency>  
  
    <!-- https://mvnrepository.com/artifact/javax.annotation/javax.annotation-api -->  
    <dependency>  
        <groupId>javax.annotation</groupId>  
        <artifactId>javax.annotation-api</artifactId>  
        <version>1.3.2</version>  
    </dependency>  
  
    <dependency>  
        <groupId>org.aspectj</groupId>  
        <artifactId>aspectjweaver</artifactId>  
        <version>1.9.9</version>  
    </dependency>  
  
    <dependency>  
        <groupId>mysql</groupId>  
        <artifactId>mysql-connector-java</artifactId>  
        <version>8.0.30</version>  
    </dependency>  
  
    <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->  
    <dependency>  
        <groupId>org.mybatis</groupId>  
        <artifactId>mybatis</artifactId>  
        <version>3.5.10</version>  
    </dependency>  
  
    <!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->  
    <dependency>  
        <groupId>org.springframework</groupId>  
        <artifactId>spring-jdbc</artifactId>  
        <version>5.3.22</version>  
    </dependency>  
  
  
    <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->  
    <dependency>  
        <groupId>org.mybatis</groupId>  
        <artifactId>mybatis-spring</artifactId>  
        <version>2.0.7</version>  
    </dependency>  
  
    <dependency>  
        <groupId>org.projectlombok</groupId>  
        <artifactId>lombok</artifactId>  
        <version>RELEASE</version>  
        <scope>compile</scope>  
    </dependency>  
  
    <dependency>  
        <groupId>org.springframework</groupId>  
        <artifactId>spring-webmvc</artifactId>  
        <version>5.3.22</version>  
    </dependency>  
  
  
  
  
  
  
  
</dependencies>
```
### 编写pojo的代码
**hello.java**
```java
package com.jzy.pojo;  
  
public class Hello {  
    private String str;  
  
    @Override  
    public String toString() {  
        return "Hello{" +  
                "str='" + str + '\'' +  
                '}';  
    }  
  
    public String getStr() {  
        return str;  
    }  
  
    public void setStr(String str) {  
        this.str = str;  
    }  
}
```
### 编写spring的配置文件
**beans.xml**
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
    <bean id="helloSpring" class="com.jzy.pojo.Hello">  
        <property name="str" value="spring"/>  
    </bean>  
  
  
</beans>
```
### 测试
```java
public class MyTest {  
    public static void main(String[] args) {  
        //获取spring的上下文对象  
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");  
        Hello helloSpring = (Hello) context.getBean("helloSpring");  
        System.out.println(helloSpring.toString());  
        //System.out.println(((Hello)context.getBean("helloSpring")).toString());  
  
    }  
}
```