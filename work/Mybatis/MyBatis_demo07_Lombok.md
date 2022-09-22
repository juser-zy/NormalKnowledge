>导入依赖

```xml
<dependency>  
    <groupId>org.projectlombok</groupId>  
    <artifactId>lombok</artifactId>  
    <version>1.18.24</version>  
    <scope>provided</scope>  
</dependency>
```

>pojo包的User类中加入注解

```java
@Data  
@AllArgsConstructor  
public class User {  
    private int id;  
    private String name;  
    private String password;  
  
  
}
```

>dao包的UserMapper接口中使用注解

```java
public interface UserMapper {  
  
    @Select("select * from user")  
    List<User> getUser();  
  
    @Select("select * from user where id = #{id}")  
    User getUserById(@Param("id")int id);  
}
```

>在mybatis-config.xml的mapper中注册接口

```xml
<!--绑定接口-->  
<mappers>  
    <mapper class="com.jzy.dao.UserMapper"></mapper>  
</mappers>
```

实现原理：

[原理](https://cloud.tencent.com/developer/article/1608174)