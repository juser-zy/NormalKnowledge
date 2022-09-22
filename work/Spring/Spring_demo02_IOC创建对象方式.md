### 无参
**User.java**
```java
package com.jzy.pojo;  
  
/**  
 * Created with IntelliJ IDEA. * User: JWhale * Date: 2022/8/31 * Time: 下午 2:30  
 * Description: */public class User {  
    private String name;  
  
    public User(String name) {  
        this.name = name;  
        System.out.println("User构造方法给name赋值"+this.name);  
    }  
  
    @Override  
    public String toString() {  
        return "User{" +  
                "name='" + name + '\'' +  
                '}';  
    }  
  
    public String getName() {  
        return name;  
    }  
  
    public void setName(String name) {  
        this.name = name;  
    }  
  
    public void show(){  
        System.out.println("name = " + name);  
    }  
}
```


**beans.xml**
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
       xsi:schemaLocation="http://www.springframework.org/schema/beans  
        https://www.springframework.org/schema/beans/spring-beans.xsd">  
  
    <bean id="user" class="com.jzy.pojo.User">  
        <property name="name" value="jzy"></property>
    </bean>  
    <alias name="user" alias="userAlias"></alias>  
  
</beans>

```

**MyTest.java**
```java
public class MyTest {  
    @Test  
    public void UserTest(){  
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");  
        User user = (User)context.getBean("userAlias");  
        System.out.println(user.toString());  
        user.show();  
    }  
}
```

### 有参
>分别为下标赋值和参数名赋值

**beans.xml**
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
       xsi:schemaLocation="http://www.springframework.org/schema/beans  
        https://www.springframework.org/schema/beans/spring-beans.xsd">  
  
    <bean id="user" class="com.jzy.pojo.User">  
        <!--<property name="name" value="jzy"></property>-->  
        <constructor-arg index="0" value="jzy01"></constructor-arg>  
        <!--<constructor-arg name="name" value="jzy02"/>-->  
    </bean>  
    <alias name="user" alias="userAlias"></alias>  
  
</beans>
```