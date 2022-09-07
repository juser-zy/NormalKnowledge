>pojo的实体类User

**User.java**
```java
package com.jzy.pojo;  
  
import org.springframework.beans.factory.annotation.Value;  
import org.springframework.stereotype.Component;  
  
@Component  
public class User {  
    @Value("jzy")  
    private String name;  
  
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
}
```

>config包的MyConfig

**MyConfig.java**
```java
package com.jzy.config;  
  
import com.jzy.pojo.User;  
import org.springframework.context.annotation.Bean;  
import org.springframework.context.annotation.ComponentScan;  
import org.springframework.context.annotation.Configuration;  
import org.springframework.context.annotation.Import;  
  
/**  
 * Created with IntelliJ IDEA. * User: JWhale * Date: 2022/8/31 * Time: 下午 10:37  
 * Description: */  
/**  
 * * @Configuration这个也会被spring容器托管，注册到容器中，因为他本身就是一个@Component  
 * * @Configuration代表这是一个配置类，就和我们之前看的beans.xml  
 * @Import导入其他的配置类  
 */  
  
  
@Configuration  
@ComponentScan("com.jzy.pojo")  
public class MyConfig {  
    /**  
     * 注册一个bean，就相当于我们之前写的一个bean标签  
     * 这个方法的名字，就相当于bean标签的id属性  
     * 这个方法的返回值，就相当于bean标签中的class属性  
     * @return  
     */    @Bean  
    public User getUser(){  
        return new User();  
    }  
  
}
```

**Config.java**
```java
package com.jzy.config;  
  
import org.springframework.context.annotation.ComponentScan;  
import org.springframework.context.annotation.Configuration;  
import org.springframework.context.annotation.Import;  
  
/**  
 * Created with IntelliJ IDEA. * User: JWhale * Date: 2022/9/4 * Time: 下午 2:39  
 * Description: 
 * */

@Configuration  
@ComponentScan("com.jzy.pojo")  
@Import(MyConfig.class)  
public class Config {  
}
```

**MyTest.java**
```java
public class MyTest {  
    @Test  
    public void test() {  
        ApplicationContext context = new AnnotationConfigApplicationContext(Config.class);  
        User getUser = (User) context.getBean("getUser");  
        System.out.println(getUser.getName());  
    }  
  
}
```