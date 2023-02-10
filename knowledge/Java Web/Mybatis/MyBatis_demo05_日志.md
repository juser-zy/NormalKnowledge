>导入log4j的依赖

```xml
<!-- https://mvnrepository.com/artifact/log4j/log4j -->  
<dependency>  
    <groupId>log4j</groupId>  
    <artifactId>log4j</artifactId>  
    <version>1.2.17</version>  
</dependency>
```

>maybatis-config.xml中对日志进行设置

```xml
<settings>  
    <!--标准的日志工厂-->  
    <!--<setting name="logImpl" value="STDOUT_LOGGING"/>-->    <setting name="logImpl" value="LOG4J"/>  
</settings>
```

>logg4j的设置，随意网上找

**log4j.properties**
```properties
log4j.rootLogger=DEBUG, console, file  
  
  
log4j.appender.console=org.apache.log4j.ConsoleAppender  
log4j.appender.console.Target = System.out  
log4j.appender.console.Threshold=DEBUG  
log4j.appender.console.layout=org.apache.log4j.PatternLayout  
log4j.appender.console.layout.ConversionPattern=%d %p [%c] - %m%n  
  
  
log4j.appender.file=org.apache.log4j.RollingFileAppender  
log4j.appender.file.File=./log/jzy.log  
log4j.appender.file.MaxFileSize=10MB  
log4j.appender.file.Threshold=DEBUG  
log4j.appender.file.layout=org.apache.log4j.PatternLayout  
log4j.appender.file.layout.ConversionPattern=%d %p [%c] - %m%n  
  
  
log4j.logger.org.mybatis=DEBUG  
log4j.logger.java.sql=DEBUG  
log4j.logger.java.sql.Statement=DEBUG  
log4j.logger.java.sql.ResultSet=DEBUG  
log4j.logger.java.sql.PreparedStatement=DEBUG
```

>测试以及使用

```java
public class UserDaoTest {  
  
    static Logger logger = Logger.getLogger(UserDaoTest.class);  
  
  
    @Test  
    public void test(){  
        SqlSession sqlSession = MybatisUtils.getSqlSession();  
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);  
//        List<User> users = mapper.getUser();  
//        for (User user : users) {  
//            logger.info(user);  
//        }  
        User userById = mapper.getUserById(1);  
        System.out.println(userById);  
        sqlSession.close();  
    }  
  
}
```