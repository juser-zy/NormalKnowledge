
### 整合mybatis

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



### 回忆mybatis

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

   

### mybatis-spring

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
