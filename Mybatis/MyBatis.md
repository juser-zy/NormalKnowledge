
# 2、第一个MyBatis程序

思路：搭建环境--》导入Mybatis--》编写代码--》测试

## 2.1、搭建环境

搭建数据库

```sql
CREATE DATABASE `mybatis`;

USE `mybatis`;

CREATE TABLE `user`(
  `id` INT(20) NOT NULL PRIMARY KEY,
  `name` VARCHAR(30) DEFAULT NULL,
  `pwd` VARCHAR(30) DEFAULT NULL
) ENGINE=INNODB DEFAULT CHARSET=utf8;

INSERT INTO `user`(`id`,`name`,`pwd`) VALUES
(1,'zyy','123456'),
(2,'张三','123456'),
(3,'李四','123456');
```

新建项目

1. 新建一个普通的maven项目

2. 删除src目录（作为父工程）

3. 导入maven依赖

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
               <version>3.5.7</version>
           </dependency>
   
           <dependency>
               <groupId>junit</groupId>
               <artifactId>junit</artifactId>
               <version>4.12</version>
           </dependency>
       </dependencies>
   ```

   

## 2.2、创建一个模板

- 编写mybatis的核心配置文件

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

- 编写mybatis工具类

  ```java
  import org.apache.ibatis.io.Resources;
  import org.apache.ibatis.session.SqlSession;
  import org.apache.ibatis.session.SqlSessionFactory;
  import org.apache.ibatis.session.SqlSessionFactoryBuilder;
  
  import java.io.IOException;
  import java.io.InputStream;
  
  /**
   * @Description: 类描述
   * @Author: zyy
   * @Date: 2022/03/12 09:07
   */
  public class MybatisUtils {
  
      private static SqlSessionFactory sqlSessionFactory;
  
      /**
       * 每个基于 MyBatis 的应用都是以一个 SqlSessionFactory 的实例为核心的。
       * SqlSessionFactory 的实例可以通过 SqlSessionFactoryBuilder 获得。
       * 而 SqlSessionFactoryBuilder 则可以从 XML 配置文件或一个预先配置的 Configuration 实例来构建出 SqlSessionFactory 实例。
       */
      static {
          String resource = "mybatis-config.xml";
          try {
              InputStream inputStream = Resources.getResourceAsStream(resource);
              sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
          } catch (IOException e) {
              e.printStackTrace();
          }
  
      }
  
      /**
       * SqlSession 完全包含了面向数据库执行sql命令所需的所有方法
       * @return
       */
      public static SqlSession getSqlSession() {
          return sqlSessionFactory.openSession();
      }
  }
  ```

## 2.3、编写代码

- 实体类

  ```java
  /**
   * @Description: 类描述
   * @Author: zyy
   * @Date: 2022/03/12 09:29
   */
  public class User {
      private int id;
      private String name;
      private String pwd;
  
  
      public User() {
      }
  
      public User(int id, String name, String pwd) {
          this.id = id;
          this.name = name;
          this.pwd = pwd;
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
  
      public String getPwd() {
          return pwd;
      }
  
      public void setPwd(String pwd) {
          this.pwd = pwd;
      }
  
      @Override
      public String toString() {
          return "User{" +
                  "id=" + id +
                  ", name='" + name + '\'' +
                  ", pwd='" + pwd + '\'' +
                  '}';
      }
  }
  ```

- Dao接口

  ```java
  import com.zyy.pojo.User;
  
  import java.util.List;
  
  /**
   * @Description: 接口描述
   * @Author: zyy
   * @Date: 2022/03/12 09:41
   */
  public interface UserDao {
      /**
       * 获取用户列表
       * @return
       */
      List<User> getUserList();
  }
  ```

- 接口实现类(由原来的UserDaoImpl转为一个Mapper配置文件)

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE mapper
          PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  <mapper namespace="com.zyy.dao.UserDao">
      <select id="getUserList" resultType="com.zyy.pojo.User">
      select id, name, pwd from `user`
      </select>
  </mapper>
  ```

## 2.4、测试

注意点：

> org.apache.ibatis.binding.BindingException: Type interface com.zyy.dao.UserDao is not known to the MapperRegistry.

![image-20220312095816822](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220312095816822.png)

> Caused by: java.io.IOException: Could not find resource com/zyy/dao/UserMapper.xml

pom改成：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>maven-study</artifactId>
        <groupId>com.zyy</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>mybatis-01</artifactId>
    <!--在build中配置resources,来防止我们资源导出失败的问题-->
    <build>
        <resources>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>false</filtering>
            </resource>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>false</filtering>
            </resource>
        </resources>
        <plugins>
            <!-- 解决idea maven工程的module的Language Level总是自动变到5-->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.10.0</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

**MapperRegistry是什么？**

核心配置文件中注册mappers

- junit测试

  ```java
  import com.zyy.pojo.User;
  import com.zyy.utils.MybatisUtils;
  import org.apache.ibatis.session.SqlSession;
  import org.junit.Test;
  
  import java.util.List;
  
  /**
   * @Description: 类描述
   * @Author: zyy
   * @Date: 2022/03/12 09:48
   */
  public class UserDaoTest {
  
      @Test
      public void getUserList() {
          SqlSession sqlSession = null;
          try {
              sqlSession = MybatisUtils.getSqlSession();
              UserDao userDao = sqlSession.getMapper(UserDao.class);
              List<User> userList = userDao.getUserList();
              userList.forEach(user -> System.out.println(user.toString()));
          } finally {
              if (sqlSession != null) {
                  sqlSession.close();
              }
          }
      }
  }
  ```

可能会遇到的问题

1. 配置文件没有注册
2. 绑定接口错误
3. 方法名不对
4. 返回类型不对
5. Maven导出资源问题



# 3、CRUD

增加(Create)、检索(Retrieve)、更新(Update)和删除(Delete)

## 1、namespace

namespace中的包名要和dao/mapper接口的包名一致

## 2、select

选择，查询语句：

- id ：就是对应的namespace中的方法
- resultType：sql语句执行的返回值
- parameterType：参数类型



1. 编写接口

   ```java
   
       User getUserById(int id);
   ```

   

2. 编写接口对应的mapper中的语句

   ```xml
       <select id="getUserById" parameterType="int" resultType="com.zyy.pojo.User">
           select id, name, pwd from `user` where id = #{id} limit 1
       </select>
   ```

   

3. 测试

   ```java
       @Test
       public void getUserById() {
           SqlSession sqlSession = null;
           try {
               sqlSession = MybatisUtils.getSqlSession();
               UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
               User user = userMapper.getUserById(1);
               System.out.println(user.toString());
           } finally {
               if (sqlSession != null) {
                   sqlSession.close();
               }
           }
       }
   ```

   

## 3、insert

1. 编写接口

   ```java
   
       int addUser(User user);
   ```

   

2. 编写接口对应的mapper中的语句

   ```xml
       <insert id="addUser" parameterType="com.zyy.pojo.User">
           insert into `user`(id, name, pwd) values (#{id},#{name},#{pwd})
       </insert>
   ```

   

3. 测试

   ```java
       @Test
       public void addUser() {
           SqlSession sqlSession = null;
           try {
               sqlSession = MybatisUtils.getSqlSession();
               UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
               int num = userMapper.addUser(new User(4, "哈哈", "123456"));
               if (num > 0) {
                   System.out.println("插入成功");
               }
               //提交事务
               sqlSession.commit();
           } finally {
               if (sqlSession != null) {
                   sqlSession.close();
               }
           }
       }
   ```

   

## 4、update

1. 编写接口

   ```java
   
       int updateUser(User user);  
   ```

   

2. 编写接口对应的mapper中的语句

   ```xml
       <update id="updateUser" parameterType="com.zyy.pojo.User">
           update `user` set name=#{name},pwd=#{pwd} where id = #{id}
       </update>
   ```

   

3. 测试

   ```java
   
       @Test
       public void updateUser() {
           SqlSession sqlSession = null;
           try {
               sqlSession = MybatisUtils.getSqlSession();
               UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
               int num = userMapper.updateUser(new User(4, "呵呵", "123456"));
               if (num > 0) {
                   System.out.println("修改成功");
               }
               //提交事务
               sqlSession.commit();
           } finally {
               if (sqlSession != null) {
                   sqlSession.close();
               }
           }
       }
   ```

   

## 5、delete

1. 编写接口

   ```java
   
       int deleteUser(int id);
   ```

   

2. 编写接口对应的mapper中的语句

   ```xml
       <delete id="deleteUser" parameterType="int">
           delete from `user` where id = #{id}
       </delete>
   ```

   

3. 测试

   ```java
   
       @Test
       public void deleteUser() {
           SqlSession sqlSession = null;
           try {
               sqlSession = MybatisUtils.getSqlSession();
               UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
               int num = userMapper.deleteUser(4);
               if (num > 0) {
                   System.out.println("删除成功");
               }
               //提交事务
               sqlSession.commit();
           } finally {
               if (sqlSession != null) {
                   sqlSession.close();
               }
           }
       }
   ```

   

注意点：增删改需要提交事务才会生效



## 6、分析错误

- 标签不要匹配错！
- resource绑定mapper，需要使用路径！
- 程序配置文件必须符合规范！
- NullPointerException，没有注册到资源！
- 输出的xml文件中存在中文乱码问题！
- maven资源没有导出问题！



## 7、万能map

假设，我们的实例类，或者数据库中的表，字段或者参数过多，我们应该考虑使用map!

接口

```java

    int addUser2(Map<String, Object> map);
```



mapper

```xml
    <insert id="addUser2" parameterType="map">
        insert into `user`(id, name, pwd) values (#{userId},#{userName},#{password})
    </insert>
```



测试

```java

    @Test
    public void addUser2() {
        SqlSession sqlSession = null;
        try {
            Map<String, Object> map = new HashMap<>();
            map.put("userId", 5);
            map.put("userName", "zyy");
            map.put("password", "123456");

            sqlSession = MybatisUtils.getSqlSession();
            UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
            int num = userMapper.addUser2(map);
            if (num > 0) {
                System.out.println("插入成功！");
            }
            sqlSession.commit();
        } finally {
            if (sqlSession != null) {
                sqlSession.close();
            }
        }
    }
```

Map传递参数，直接在sql中取出key即可！

对象传递参数，直接在sql中取对象的属性即可！

只有一个基本类型的情况下，可以直接在sql中取到！

多个参数用Map，或者注解！

![image-20220312184129486](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220312184129486.png)

## 8、思考题

模糊查询怎么写？

1. java代码执行的时候，传递通配符%

   ```java
   List<User> userList = userMapper.getUserLike("%z%");
   ```

   

2. 在sql中拼接使用通配符 （不推荐，容易引起sql注入问题）

   ```xml
       <!--  select id, name, pwd from `user` where id = ?   -->
       <!--  select id, name, pwd from `user` where id = 1      一般情况-->
       <!--  select id, name, pwd from `user` where id = 1 or 1=1   sql注入情况-->
       <select id="getUserLike" resultType="com.zyy.pojo.User" parameterType="string">
           select id, name, pwd from `user` where name like "%"#{value}"%"
       </select>
   ```



# 4、配置解析

## 1、核心配置文件

- mybatis-config.xml

- MyBatis 的配置文件包含了会深深影响 MyBatis 行为的设置和属性信息。 配置文档的顶层结构如下：

  ![image-20220312214122775](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220312214122775.png)

  

## 2、环境配置（environments）

MyBatis 可以配置成适应多种环境

**不过要记住：尽管可以配置多个环境，但每个 SqlSessionFactory 实例只能选择一种环境。**

学会使用配置多套运行环境！

MyBatis默认的事务管理器就是jdbc，连接池：POOLED

## 3、属性（properties）

我们可以通过properties属性来实现引用配置文件

这些属性可以在外部进行配置，并可以进行动态替换。你既可以在典型的 Java 属性文件中配置这些属性，也可以在 properties 元素的子元素中设置。【db.properties】

在xml中，所有的标签都可以规定其顺序

![image-20220312220720720](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220312220720720.png)

编写一个配置文件

db.properties

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/mybatis?useSSL=false&useUnicode=true&characterEncoding=UTF-8
username=root
password=123456
```

在核心配置文件中引入

```xml
    <properties resource="db.properties">
        <property name="username" value="root"/>
        <property name="password" value="123456"/>
    </properties>
```

- 可以直接引入外部文件
- 可以在其中增加一些属性配置
- **如果两个文件有同一个字段，优先使用外部配置文件的！**

## 4、类型别名（typeAliases）

- 类型别名可为 Java 类型设置一个缩写名字。
- 意在降低冗余的全限定类名书写。

核心配置文件

```xml

    <typeAliases>
        <typeAlias type="com.zyy.pojo.User" alias="User"/>
    </typeAliases>
```

```xml
    <select id="getUserList" resultType="User">
        select id, name, pwd from `user`
    </select>
```

也可以指定一个包名，MyBatis 会在包名下面搜索需要的 Java Bean

扫描实体类的包，它的默认别名就是这个类的类名，首字母小写。

```xml
    <typeAliases>
        <package name="com.zyy.pojo"/>
    </typeAliases>
```

```xml
    <select id="getUserList" resultType="user">
        select id, name, pwd from `user`
    </select>
```

在实体类比较少的时候，使用第一种方式。

如果实体类比较多，建议使用第二种。

第一种可以DIY别名，第二种不行，如果非要改的话，需要在实体上增加注解

```java
@Alias("user")
public class User {}
```

## 5、设置（settings）

![image-20220312223626908](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220312223626908.png)

![image-20220312223637156](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220312223637156.png)

## 6、其他配置

- [typeHandlers（类型处理器）](https://mybatis.org/mybatis-3/zh/configuration.html#typeHandlers)
- [objectFactory（对象工厂）](https://mybatis.org/mybatis-3/zh/configuration.html#objectFactory)
- [plugins（插件）](https://mybatis.org/mybatis-3/zh/configuration.html#plugins)
  - [mybatis-generator-core](https://mvnrepository.com/artifact/org.mybatis.generator/mybatis-generator-core)
  - [mybatis-plus](https://mvnrepository.com/artifact/com.baomidou/mybatis-plus)
  - 通用mapper

## 7、映射器（mappers）

MapperRegistry：注册绑定我们的mapper文件

方式一：

```xml
    <!--   每一个Mapper.xml都需要在mybatis核心配置文件中注册 -->
    <mappers>
        <mapper resource="com/zyy/dao/UserMapper.xml"/>
    </mappers>
```

方式二：使用class文件绑定注册

```xml
    <mappers>
        <mapper class="com.zyy.dao.UserMapper"/>
    </mappers>
```

注意点：

- 接口和它的Mapper配置文件必须同名！
- 接口和它的Mapper配置文件必须在同一个包下！

方式三：使用扫描包进行注入绑定

```xml
    <mappers>
        <package name="com.zyy.dao"/>
    </mappers>
```

注意点：

- 接口和它的Mapper配置文件必须同名！
- 接口和它的Mapper配置文件必须在同一个包下！



练习时间：

- 将数据库配置文件外部引入
- 实体类别名
- 保证UserMapper接口和UserMapper.xml改成一致!并且放到同一个包下！



## 8、生命周期和作用域

生命周期和作用域类别是至关重要的，因为错误的使用会导致非常严重的**并发问题**。

#### SqlSessionFactoryBuilder

- 一旦创建了 SqlSessionFactory，就不再需要它了
- 局部变量

#### SqlSessionFactory

- 说白了就是可以想象为：数据库连接池
- 一旦被创建就应该在应用的运行期间一直存在，**没有任何理由丢弃它或重新创建另一个实例。**
- SqlSessionFactory 的最佳作用域是应用作用域
- 最简单的就是使用**单例模式**或者静态单例模式。

#### SqlSession

- 连接到连接池的一个请求
- 每个线程都应该有它自己的 SqlSession 实例。SqlSession 的实例不是线程安全的，因此是不能被共享的，所以它的最佳的作用域是请求或方法作用域。
- 用完之后需要赶紧关闭，否则资源被占用！

![image-20220326092431602](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220326092431602.png)

这里的每一个mapper,就代表一个具体的业务！

# 5、解决属性名和字段名不一致的问题

## 1、问题

数据库中字段

![image-20220326094813973](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220326094813973.png)

新建一个项目，拷贝之前的，测试实体类字段不一致的情况

```java

/**
 * @Description: 类描述
 * @Author: zyy
 * @Date: 2022/03/12 09:29
 */
public class User {
    private int id;
    private String name;
    private String password;
    //...省略
}

```

测试出现问题

![image-20220326095320183](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220326095320183.png)

解决方法：

- 起别名

  ```xml
      <select id="getUserById" parameterType="int" resultType="com.zyy.pojo.User">
          select id, name, pwd as `password` from `user` where id = #{id} limit 1
      </select>
  ```

  

## 2、resultMap

结果集映射

```
id  name  pwd
id  name  password
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.zyy.dao.UserMapper">

    <resultMap id="userMap" type="com.zyy.pojo.User">
        <!--  column数据库列名    property实体类中的属性名   -->
        <result column="id" property="id"/>
        <result column="name" property="name"/>
        <result column="pwd" property="password"/>
    </resultMap>

    <select id="getUserById" parameterType="int" resultMap="userMap">
        select id, name, pwd from `user` where id = #{id} limit 1
    </select>

</mapper>
```

- `resultMap` 元素是 MyBatis 中最重要最强大的元素
- ResultMap 的设计思想是，对简单的语句做到零配置，对于复杂一点的语句，只需要描述语句之间的关系就行了。
- `ResultMap` 的优秀之处——你完全可以不用显式地配置它们。

# 6、日志

## 6.1、日志工厂

如果一个数据库操作 出现了异常，我们需要排错，日志就是最好的工具

曾经：sout、debug

现在：日志工厂

![image-20220326102044646](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220326102044646.png)

- SLF4J 
- LOG4J(deprecated since 3.5.9) 
- LOG4J2
- JDK_LOGGING 
- COMMONS_LOGGING 
- STDOUT_LOGGING 
- NO_LOGGING

在mybatis中具体使用哪一个日志实现，在设置中设定！

**STDOUT_LOGGING标准日志输出**

在mybatis核心配置文件中添加日志配置

```xml
    <settings>
        <!--        标准的日志工厂实现-->
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>
```



![image-20220326103056897](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220326103056897.png)

## 6.2、Log4J

什么是Log4J

- Log4j是[Apache](https://baike.baidu.com/item/Apache/8512995)的一个开源项目，通过使用Log4j，我们可以控制日志信息输送的目的地是[控制台](https://baike.baidu.com/item/控制台/2438626)、文件、[GUI](https://baike.baidu.com/item/GUI)组件；
- 我们也可以控制每一条日志的输出格式；
- 通过定义每一条日志信息的级别，我们能够更加细致地控制日志的生成过程；
- 通过一个[配置文件](https://baike.baidu.com/item/配置文件/286550)来灵活地进行配置，而不需要修改应用的代码。



1. 导入log4j的包

   ```xml
   <!-- https://mvnrepository.com/artifact/log4j/log4j -->
   <dependency>
       <groupId>log4j</groupId>
       <artifactId>log4j</artifactId>
       <version>1.2.17</version>
   </dependency>
   
   ```

2. log4j.properties

```properties
   #将等级为DEBUG的日志输出到console和file这两个目的地，console和file的定义在下面配置中
   log4j.rootLogger=DEBUG, console, file
   
   #控制台输出的相关配置
   log4j.appender.console=org.apache.log4j.ConsoleAppender
   log4j.appender.console.Target = System.out
   log4j.appender.console.Threshold=DEBUG
   log4j.appender.console.layout=org.apache.log4j.PatternLayout
   log4j.appender.console.layout.ConversionPattern=%d %p [%c] - %m%n
   
   #文件输出的相关配置
   log4j.appender.file=org.apache.log4j.RollingFileAppender
   log4j.appender.file.File=./log/zyy.log
   log4j.appender.file.MaxFileSize=10MB
   log4j.appender.file.Threshold=DEBUG
   log4j.appender.file.layout=org.apache.log4j.PatternLayout
   log4j.appender.file.layout.ConversionPattern=%d %p [%c] - %m%n
   
   #日志输出级别
   log4j.logger.org.mybatis=DEBUG
   log4j.logger.java.sql=DEBUG
   log4j.logger.java.sql.Statement=DEBUG
   log4j.logger.java.sql.ResultSet=DEBUG
   log4j.logger.java.sql.PreparedStatement=DEBUG
```

3. 配置log4j为mybatis日志的实现

   ```xml
       <settings>
           <setting name="logImpl" value="LOG4J"/>
       </settings>
   ```

4. 使用log4j,运行测试

   ![image-20220326110051525](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220326110051525.png)



**简单使用**

1. 在要使用log4j的类中，导入包import org.apache.log4j.Logger;

2. 日志对象，参数为当前类的class

   ```java
   
       static Logger logger = Logger.getLogger(UserDaoTest.class);
   ```

3. 日志级别

   ```java
       @Test
       public void print() {
           logger.info("info...");
           logger.debug("info...");
           logger.error("info...");
       }
   ```

# 7、分页

**思考：为啥要分页？**

- 减少数据的处理量



## 7.1、使用limit分页

```sql
-- 语法
select * from `user` limit startIndex,pageSize;
select * from `user` limit 2;
-- 相当于
select * from `user` limit 0,2;
```

使用mybatis实现分页，核心sql

1. 接口

   ```java
       /**
        * 分页查询
        * @param map
        * @return
        */
       List<User> getUserByLimit(Map<String, Integer> map);
   ```

   

2. Mapper.xml

   ```xml
       <select id="getUserByLimit" parameterType="map" resultMap="userMap">
           select id, name, pwd from `user` limit #{startIndex},#{pageSize}
       </select>
   ```

   

3. 测试

   ```java
       @Test
       public void getUserByLimit() {
   
           SqlSession sqlSession = null;
           try {
               sqlSession = MybatisUtils.getSqlSession();
               UserMapper mapper = sqlSession.getMapper(UserMapper.class);
   
               Map<String, Integer> map = new HashMap<>(16);
               map.put("startIndex", 0);
               map.put("pageSize", 2);
               List<User> userList = mapper.getUserByLimit(map);
               for (User user : userList) {
                   logger.info(user);
               }
           } finally {
               if (sqlSession != null) {
                   sqlSession.close();
               }
           }
   
       }
   ```

   

## 7.2、RowBounds分页

不在使用sql实现分压

1. 接口

   ```java
       /**
        * 分页查询
        * @return
        */
       List<User> getUserByRowBounds();
   ```

   

2. Mapper.xml

   ```xml
       <select id="getUserByRowBounds" resultMap="userMap">
           select id, name, pwd from `user`
       </select>
   ```

   

3. 测试

   ```java
       @Test
       public void getUserByRowBounds() {
           SqlSession sqlSession = null;
           try {
               sqlSession = MybatisUtils.getSqlSession();
               RowBounds rowBounds = new RowBounds(1,2);
               List<User> userList = sqlSession.selectList("com.zyy.dao.UserMapper.getUserByRowBounds",null, rowBounds);
               for (User user : userList) {
                   logger.info(user);
               }
           } finally {
               if (sqlSession != null) {
                   sqlSession.close();
               }
           }
   
       }
   ```

   

## 7.3、分页插件

![image-20220326144801005](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220326144801005.png)

官方文档：https://pagehelper.github.io/



# 8、使用注解开发

## 8.1、面向接口编程

- 根本原因：解耦，可拓展，提高复用，分层开发中，上层不用管具体的实现，大家都遵守共同的标准，使得开发变得容易，规范性更好。
- 在一个面向对象的系统中，系统的各种功能是由许许多多的不同对象协作完成的。在这种情况下，各个对象内部是如何实现自己的，对系统设计人员来讲就不用那么重要了；
- 而各个对象之前的协作关系则成为系统设计的关键。小到不同类之前的通讯，大到各模块之间的交互，在系统设计之初都是要着重要考虑的，这也是系统设计的主要工作内容。面向接口编程就是指按照这种思想来编程。



**关于接口的理解**

- 接口从更深层次的理解，应是定义（规范，约束）与实现（名实分离的原则）的分离
- 接口的本身反映了系统设计人员对系统的抽象理解
- 接口应有两类：
  - 第一类是对一个个体的抽象，它对应为一个抽象体（abstract class）
  - 第二类是对一个个体某一方便的抽象，即形成一个抽象面（interface）
- 一个个体有可能有多个抽象面。抽象体和抽象面是有区别的。



**三个面向区别**

- 面向对象，我们考虑问题时，以对象为单位，考虑它的属性及方法
- 面向过程，我们考虑问题时，以一个具体的流程（事务过程）为单位，考虑它的实现
- 接口设计与非接口设计是针对复用技术而言的，与面向对象（过程）不是一个问题。更多的体现就是对系统整体的架构。



## 8.2、使用注解开发

1. 注解在接口上实现

   ```java
       @Select("select id, name, pwd from `user`")
       List<User> getUserList();
   ```

2. 需要在核心配置文件中绑定接口

   ```xml
       <mappers>
           <mapper class="com.zyy.dao.UserMapper"/>
       </mappers>
   ```

3. 测试

   ```java
       @Test
       public void getUserList() {
           SqlSession sqlSession = null;
           try {
               sqlSession = MybatisUtils.getSqlSession();
               //底层主要应用反射
               UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
               List<User> userList = userMapper.getUserList();
               for (User user : userList) {
                   System.out.println(user.toString());
               }
           } finally {
               if (sqlSession != null) {
                   sqlSession.close();
               }
           }
       }
   ```

本质：反射机制

底层：动态代理



**mybatis详细的执行流程**

![未命名文件](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/%E6%9C%AA%E5%91%BD%E5%90%8D%E6%96%87%E4%BB%B6.png)

## 8.3、CRUD

我们可以造工具类创建的时候实现自动提交事务！

```java
    public static SqlSession getSqlSession() {
        return sqlSessionFactory.openSession(true);
    }
```

编写接口，增加注解

```java
import com.zyy.pojo.User;
import org.apache.ibatis.annotations.Delete;
import org.apache.ibatis.annotations.Insert;
import org.apache.ibatis.annotations.Param;
import org.apache.ibatis.annotations.Select;
import org.apache.ibatis.annotations.Update;

import java.util.List;

/**
 * @Description: 接口描述
 * @Author: com.zyy
 * @Date: 2022/03/12 09:41
 */
public interface UserMapper {

    @Select("select id, name, pwd from `user`")
    List<User> getUserList();

    @Select("select id, name, pwd from `user` where id=#{id} ")
    User getUserById(@Param("id") int id);

    @Insert("insert into `user`(id, name, pwd) values (#{id},#{name},#{password})")
    int addUser(User user);

    @Update("update `user` set name=#{name},pwd=#{password} where id=#{id}")
    int updateUser(User user);

    @Delete("delete from `user` where id=#{uid}")
    int deleteUser(@Param("uid") int id);
}
```





测试类

【注意：我们必须要将接口注册绑定到我们的核心配置文件中】

**关于@Param()注解** 

- 基本类型的参数或者String类型，需要加上
- 引用类型不需要加
- 如果只有一个基本类型的话，可以忽略，但是建议大家都加上
- 我们在sql中引用的就是我们这里的@Param()中设定的属性名



```java
import com.zyy.pojo.User;
import com.zyy.utils.MybatisUtils;
import org.apache.ibatis.session.SqlSession;
import org.junit.Test;

import java.util.List;

/**
 * @Description: 类描述
 * @Author: zyy
 * @Date: 2022/03/27 11:31
 */
public class UserMapperTest {

    @Test
    public void getUserList() {
        SqlSession sqlSession = null;
        try {
            sqlSession = MybatisUtils.getSqlSession();
            //底层主要应用反射
            UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
            List<User> userList = userMapper.getUserList();
            for (User user : userList) {
                System.out.println(user.toString());
            }
        } finally {
            if (sqlSession != null) {
                sqlSession.close();
            }
        }
    }

    @Test
    public void getUserById() {
        SqlSession sqlSession = null;
        try {
            sqlSession = MybatisUtils.getSqlSession();
            //底层主要应用反射
            UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
            User user = userMapper.getUserById(1);
            System.out.println(user);
        } finally {
            if (sqlSession != null) {
                sqlSession.close();
            }
        }
    }

    @Test
    public void addUser() {
        SqlSession sqlSession = null;
        try {
            sqlSession = MybatisUtils.getSqlSession();
            //底层主要应用反射
            UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
            int count = userMapper.addUser(new User(6, "zyy6", "123456"));
            System.out.println(count);
        } finally {
            if (sqlSession != null) {
                sqlSession.close();
            }
        }
    }

    @Test
    public void updateUser() {
        SqlSession sqlSession = null;
        try {
            sqlSession = MybatisUtils.getSqlSession();
            //底层主要应用反射
            UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
            int count = userMapper.updateUser(new User(6, "zyy2", "123123"));
            System.out.println(count);
        } finally {
            if (sqlSession != null) {
                sqlSession.close();
            }
        }
    }

    @Test
    public void deleteUser() {
        SqlSession sqlSession = null;
        try {
            sqlSession = MybatisUtils.getSqlSession();
            //底层主要应用反射
            UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
            int count = userMapper.deleteUser(5);
            System.out.println(count);
        } finally {
            if (sqlSession != null) {
                sqlSession.close();
            }
        }
    }


}
```



**#{}  ${}区别**

别人博客：https://blog.csdn.net/zymx14/article/details/78067452



# 9、Lombok

使用步骤

1. 在idea中安装lombok插件

2. 在项目中导入lombok的jar包

   ```xml
           <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
           <dependency>
               <groupId>org.projectlombok</groupId>
               <artifactId>lombok</artifactId>
               <version>1.18.22</version>
           </dependency>
   ```

3. 在实体类上加注解即可

   ```java
   import lombok.AllArgsConstructor;
   import lombok.Data;
   
   /**
    * @Description: 类描述
    * @Author: com.zyy
    * @Date: 2022/03/12 09:29
    */
   
   @Data
   @AllArgsConstructor
   public class User {
       private int id;
       private String name;
       private String password;
   }
   ```

   

注解

```
@Getter and @Setter
@FieldNameConstants
@ToString
@EqualsAndHashCode
@AllArgsConstructor, @RequiredArgsConstructor and @NoArgsConstructor
@Log, @Log4j, @Log4j2, @Slf4j, @XSlf4j, @CommonsLog, @JBossLog, @Flogger, @CustomLog
@Data
@Builder
@SuperBuilder
@Singular
@Delegate
@Value
@Accessors
@Wither
@With
@SneakyThrows
@val
@var
```

实现原理：

[99%的程序员都在用Lombok，原理竟然这么简单？我也手撸了一个！|建议收藏！！！ - 云+社区 - 腾讯云 (tencent.com)](https://cloud.tencent.com/developer/article/1608174)



# 10、多对一处理

多对一：

- 多个学生，对应一个老师
- 对于学生而言，  **关联**   ，多个学生，关联一个老师【多对一】
- 对于老师而言，  **集合**   ，一个老师有很多学生【一对多】



![image-20220331222349156](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220331222349156.png)

SQL

```sql
CREATE TABLE `teacher`
(
    `id`   INT(10) NOT NULL,
    `name` VARCHAR(30) DEFAULT NULL,
    PRIMARY KEY (`id`)
) ENGINE = INNODB
  DEFAULT CHARSET = utf8;

INSERT INTO teacher(`id`, `name`)
VALUES (1, '秦老师');

CREATE TABLE `student`
(
    `id`   INT(10) NOT NULL,
    `name` VARCHAR(30) DEFAULT NULL,
    `tid`  INT(10)     DEFAULT NULL,
    PRIMARY KEY (`id`),
    KEY `fktid` (`tid`),
    CONSTRAINT `fktid` FOREIGN KEY (`tid`) REFERENCES `teacher` (`id`)
) ENGINE = INNODB
  DEFAULT CHARSET = utf8;

INSERT INTO `student` (`id`, `name`, `tid`)
VALUES ('1', '小明', '1');
INSERT INTO `student` (`id`, `name`, `tid`)
VALUES ('2', '小红', '1');
INSERT INTO `student` (`id`, `name`, `tid`)
VALUES ('3', '小张', '1');
INSERT INTO `student` (`id`, `name`, `tid`)
VALUES ('4', '小李', '1');
INSERT INTO `student` (`id`, `name`, `tid`)
VALUES ('5', '小王', '1');
```

## 测试环境搭建



1. 导入lombok
2. 新建实体类Teacher，Student
3. 建立Mapper接口
4. 建立Mapper.xml文件
5. 在核心配置文件中绑定注册我们的Mapper接口或者文件【方式很多，自选】
6. 测试查询是否能够成功！

![image-20220331225454835](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220331225454835.png)

Student.java

```java
@Data
public class Student {
    private int id;
    private String name;
    private Teacher teacher;
}
```

Teacher.java

```java
@Data
public class Teacher {
    private int id;
    private String name;
}
```



## 按照查询嵌套处理

```sql
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.zyy.dao.StudentMapper">

    <!--
    思路：
      1. 查询所有的学生信息
      2. 根据查询出来的学生的tid，寻找对应的老师  子查询
    -->
    <select id="getStudentList" resultMap="studentTeacherMap">
      select stu.id,stu.name,stu.tid from student stu;
    </select>

    <resultMap id="studentTeacherMap" type="com.zyy.pojo.Student">
        <result property="id" column="id"/>
        <result property="name" column="name"/>
        <!--
        复杂的属性，我们需要单独处理
        对象：association
        集合：collection
        -->
        <association property="teacher" column="tid" javaType="com.zyy.pojo.Teacher" select="getTeacherList"/>
    </resultMap>

    <select id="getTeacherList" resultType="com.zyy.pojo.Teacher">
        select te.id,te.name from teacher te;
    </select>
    
</mapper>
```

## 按照结果嵌套处理

```xml
    <select id="getStudentList2" resultMap="studentTeacherMap2">
      select s.id sId, s.name sName, t.name tName from student s,teacher t where s.tid=t.id;
    </select>

    <resultMap id="studentTeacherMap2" type="com.zyy.pojo.Student">
        <result property="id" column="sId"/>
        <result property="name" column="sName"/>
        <association property="teacher" javaType="com.zyy.pojo.Teacher">
            <result property="name" column="tName"/>
        </association>
    </resultMap>
```

回顾mysql多对一查询方式：

- 子查询
- 链表查询

# 11、一对多处理

比如：一个老师拥有多个学生！

对于老师而言，就是一对多的关系！



## 环境搭建

实体类

```java
import lombok.Data;

/**
 * @Description: 类描述
 * @Author: zyy
 * @Date: 2022/03/31 22:32
 */
@Data
public class Student {
    private int id;
    private String name;
    private int tid;
}
```

```java
import lombok.Data;

import java.util.List;

/**
 * @Description: 类描述
 * @Author: zyy
 * @Date: 2022/03/31 22:32
 */
@Data
public class Teacher {
    private int id;
    private String name;
    private List<Student> studentList;
}
```

## 按照结果嵌套处理

```xml
    <select id="getTeacher" resultMap="teacherStudent" parameterType="int">
        select t.id tId,t.name tName,s.id sId,s.name sName from teacher t ,student s where t.id = s.tid and t.id=#{tId}
    </select>

    <resultMap id="teacherStudent" type="com.zyy.pojo.Teacher">
        <result property="name" column="tName"/>
        <result property="id" column="tId"/>
        <!--
        复杂的属性，我们需要单独处理
        集合：collection
        javaType="" 执行属性的类型
        结婚中的泛型信息，我们使用ofType获取
        -->
        <collection property="studentList" ofType="com.zyy.pojo.Student">
            <result property="id" column="sId"/>
            <result property="name" column="sName"/>
        </collection>
    </resultMap>
```



## 按照查询嵌套处理

```xml

    <select id="getTeacher2" resultMap="teacherStudent2" parameterType="int">
      select id,name from teacher where id=#{tId}
    </select>


    <resultMap id="teacherStudent2" type="com.zyy.pojo.Teacher">
        <collection property="studentList" javaType="ArrayList" ofType="com.zyy.pojo.Student" select="getStudentListByTeacherId" column="id"/>
    </resultMap>


    <select id="getStudentListByTeacherId" resultType="com.zyy.pojo.Student" parameterType="int">
      select id,name,tid from student where tid=#{tId}
    </select>
```

## 小结

1. 关联 - association【多对一】
2. 集合 - collection【一对多】
3. javaType  &  ofType
   -  javaType  用来指定实体类中属性的类型
   - ofType 用来指定映射到List或者集合中pojo类型，泛型中的约束类型。

## 注意点

- 保证sql的可读性，尽量保证通俗易懂
- 注意一对多和多对一中，属性和字段的问题
- 如果问题不好排查错误，可以使用日志，建议使用Log4j



面试高频：

- mysql引擎
- innodb底层原理
- 索引
- 索引优化





# 12、动态SQL

**什么是动态sql：动态sql就是根据不同的条件生成不同的sql语句。**

如果你之前用过 JSTL 或任何基于类 XML 语言的文本处理器，你对动态 SQL 元素可能会感觉似曾相识。在 MyBatis 之前的版本中，需要花时间了解大量的元素。借助功能强大的基于 OGNL 的表达式，MyBatis 3 替换了之前的大部分元素，大大精简了元素种类，现在要学习的元素种类比原来的一半还要少。

- if
- choose (when, otherwise)
- trim (where, set)
- foreach

## 搭建环境

```sql
CREATE TABLE `blog`
(
    `id`          VARCHAR(50)  NOT NULL COMMENT '博客id',
    `title`       VARCHAR(100) NOT NULL COMMENT '博客标题',
    `author`      VARCHAR(30)  NOT NULL COMMENT '博客作者',
    `create_time` DATETIME     NOT NULL COMMENT '创建时间',
    `views`       INT(30)      NOT NULL COMMENT '浏览量'
) ENGINE = INNODB
  DEFAULT CHARSET = utf8;
```

创建一个基础工程

1. 导包

2. 编写配置文件

3. 编写实体类

   ```java
   import lombok.Data;
   
   import java.time.LocalDateTime;
   
   /**
    * @Description: 类描述
    * @Author: zyy
    * @Date: 2022/04/05 17:40
    */
   @Data
   public class Blog {
       private String id;
       private String title;
       private String author;
       private LocalDateTime createTime;
       private int views;
   }
   ```

   

4. 编写实体类对应Mapper接口和Mapper.xml文件



## if

```xml
    <select id="getBlogList" parameterType="map" resultType="com.zyy.pojo.Blog">
        select id,title,author,create_time createTime,views from blog
        where 1=1
        <if test="title != null">
            and title like #{title}
        </if>
        <if test="author != null">
            and author = #{author}
        </if>
    </select>
```

## choose (when, otherwise)

```sql
    <select id="getBlogList" parameterType="map" resultType="com.zyy.pojo.Blog">
        select id,title,author,create_time createTime,views from blog
        <where>
            <choose>
                <when test="title != null">
                    title like #{title}
                </when>
                <when test="author != null">
                    and author = #{author}
                </when>
                <otherwise>
                    and views=#{views}
                </otherwise>
            </choose>
        </where>
    </select>
```



## trim (where, set)

*where* 元素只会在子元素返回任何内容的情况下才插入 “WHERE” 子句。而且，若子句的开头为 “AND” 或 “OR”，*where* 元素也会将它们去除。

```xml
    <select id="getBlogList" parameterType="map" resultType="com.zyy.pojo.Blog">
        select id,title,author,create_time createTime,views from blog
        <where>
            <if test="title != null">
                title like #{title}
            </if>
            <if test="author != null">
                and author = #{author}
            </if>
        </where>
    </select>
```



```xml
    <update id="updateBlog" parameterType="map">
        update blog
        <set>
            <if test="title != null">
                title=#{title},
            </if>
            <if test="author != null">
                author=#{author}
            </if>
        </set>
        where id=#{id}
    </update>
```

如果 *where* 元素与你期望的不太一样，你也可以通过自定义 trim 元素来定制 *where* 元素的功能。比如，和 *where* 元素等价的自定义 trim 元素为：

```xml
<trim prefix="WHERE" prefixOverrides="AND |OR ">
  ...
</trim>
```

```xml
<trim prefix="SET" suffixOverrides=",">
  ...
</trim>
```



**所谓的动态sql。本质还是sql语句，只是我们可以在sql层面，去执行一个逻辑代码**

## SQL片段

有的时候，我们可能会将一些功能的部分抽取出来，方便复用

1. 使用sql标签抽取公共的部分

   ```xml
       <sql id="title-author-views">
           <choose>
               <when test="title != null">
                   title like #{title}
               </when>
               <when test="author != null">
                   and author = #{author}
               </when>
               <otherwise>
                   and views=#{views}
               </otherwise>
           </choose>
       </sql>
   ```

   

2. 在需要使用的地方使用include标签引用即可

   ```xml
       <select id="getBlogList" parameterType="map" resultType="com.zyy.pojo.Blog">
           select id,title,author,create_time createTime,views from blog
           <where>
             <include refid="title-author-views"/>
           </where>
       </select>
   ```

注意事项：

- 最好基于单表来定义sql片段
- 不要存在where标签



## foreach

![image-20220405222435050](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220405222435050.png)

```xml
    <!--
        select id,title,author,create_time createTime,views from blog where 1=1 and (id ='1' or id='2'or id='3')
        -->
    <select id="getBlogListForeach" parameterType="map" resultType="com.zyy.pojo.Blog">
      select id,title,author,create_time createTime,views from blog
      <where>
          <foreach collection="idList" item="id" open="(" close=")" separator="or">
              id = #{id}
          </foreach>
      </where>
    </select>
```



==动态sql就是在拼接sql语句，我们只要保证sql正确性，按照sql的格式，去排列组合就可以了==



# 13、缓存

## 13.1、简介

1. 什么是缓存[cache]
   - 存在内存中的临时数据
   - 将用户经常查询的数据放在缓存（内存）中，用户去查询数据就不用从磁盘上（关系型数据库数据文件）查询，从缓存中查询，从而提高查询效率，解决了高并发系统的性能问题。
2. 为什么使用缓存
   - 减少和数据库的交互次数，减少系统开销，提高系统效率
3. 什么样的数据能使用缓存
   - 经常查询并且不经常改变的数据

## 13.2、mybatis缓存

- mybatis包含一个非常强大的查询缓存特性，它可以非常方便地定制和配置缓存。缓存可以极大的提高查询效率。
- mybatis系统中默认定义了两级缓存：**一级缓存**和**二级缓存**
  - 默认情况下，只有一级缓存开启。（sqlsession级别的缓存，也称为本地缓存）
  - 二级缓存需要手动开启和配置，它是基于namespace级别的缓存。
  - 为了提高扩展性，mybatis定义了缓存接口cache。我们可以通过实现cache接口来自定义二级缓存。

## 13.3、一级缓存

- 一级缓存也叫本次缓存
  - 与数据库同一次会话期间查询到的数据会放到本次缓存中。
  - 以后如果需要获取相同的数据，直接从缓存中拿，没必须再去查询数据库



1. 开启日志

2. 测试再一个session中查询两次相同的记录

   ```java
   public class UserMapperTest {
   
   
       @Test
       public void getUserById() {
   
           SqlSession sqlSession = null;
   
           try {
               sqlSession = MybatisUtils.getSqlSession();
               UserMapper mapper = sqlSession.getMapper(UserMapper.class);
   
               User user1 = mapper.getUserById(1);
               System.out.println(user1);
               System.out.println("==================");
   
               User user2 = mapper.getUserById(1);
               System.out.println(user2);
   
               System.out.println(user1 == user2);
   
           } finally {
               if (sqlSession != null) {
                   sqlSession.close();
               }
           }
   
       }
   }
   ```

   

3. 查询日志输出

   ![image-20220417201424256](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220417201424256.png)



缓存失效的情况：

1. 查询条件不同

   ![image-20220417202504174](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220417202504174.png)

2. 增删改操作，可能会改变原来的数据，所以必定会刷新缓存

   ![image-20220417202359023](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220417202359023.png)

3. 查询不同的Mapper.xml

4. 手动清理缓存

   ```java
       @Test
       public void getUserById2() {
   
           SqlSession sqlSession = null;
   
           try {
               sqlSession = MybatisUtils.getSqlSession();
               UserMapper mapper = sqlSession.getMapper(UserMapper.class);
   
               User user1 = mapper.getUserById(1);
               System.out.println(user1);
               System.out.println("==================");
               //手动清理缓存
               sqlSession.clearCache();
   
               User user2 = mapper.getUserById(1);
               System.out.println(user2);
   
               System.out.println(user1 == user2);
   
           } finally {
               if (sqlSession != null) {
                   sqlSession.close();
               }
           }
       }
   ```

   ![image-20220417202708050](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220417202708050.png)

小结：

- 一级缓存默认是开启的，只在一次sqlsession中有效，也就是拿到连接到关闭了连接这个区间段！
- 一级缓存就是一个map

## 13.4、二级缓存

- 二级缓存也叫全局缓存，一级缓存作用域太低了，所以诞生了二级缓存。
- 基本namespace级别的缓存，一个名称空间，对应一个二级缓存
- 工作机制
  - 一个会话查询一个数据，这个数据就会被放到当前会话的一级缓存中
  - 如果当前会话关闭了，这个会话对应的一级缓存就没了，但是我们想要的是，会话关闭了，一级缓存中的数据被保存到二级缓存中
  - 新的会话查询信息，就可以直接从二级缓存中获取内容
  - 不同的mapper查出的数据会放在自己对应的缓存（map）中



步骤

1. 开启全部缓存

   mybatis-config.xml

   ```xml
       <settings>
           <setting name="logImpl" value="STDOUT_LOGGING"/>
           <!--        显式开启全局缓存-->
           <setting name="cacheEnabled" value="true"/>
       </settings>
   ```

   

2. 在要使用二级缓存的mapper中开启

   ```xml
       <!--    在当前mapper.xml中使用二级缓存-->
       <cache/>
   ```

   也可以自定义参数

   ```xml
       <!--    在当前mapper.xml中使用二级缓存-->
       <cache
               eviction="FIFO"
               flushInterval="60000"
               size="512"
               readOnly="true"/>
   ```

3. 测试

   ```java
      @Test
       public void getUserById2() {
   
   
           SqlSession sqlSession = MybatisUtils.getSqlSession();
           UserMapper mapper = sqlSession.getMapper(UserMapper.class);
   
           User user1 = mapper.getUserById(1);
           System.out.println(user1);
           sqlSession.close();
           System.out.println("==================");
   
   
           SqlSession sqlSession2 = MybatisUtils.getSqlSession();
           UserMapper mapper2 = sqlSession2.getMapper(UserMapper.class);
   
           User user2 = mapper2.getUserById(1);
           System.out.println(user2);
   
           System.out.println(user1 == user2);
           sqlSession2.close();
       }
   ```

   ![image-20220417214903399](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220417214903399.png)

4. 问题：我们需要将实体类序列化，否则就会报错

   ```
   Caused by: java.io.NotSerializableException: com.zyy.pojo.User
   ```

   解决

   ```java
   public class User implements Serializable {
   }
   ```

小结：

- 只要开启了二级缓存，在同一个Mapper下就有效
- 所有的数据都会先放在一级缓存中
- **只有当会话提交或者关闭的时候，才会提交到二级缓存中**。

## 13.5、缓存原理

1. 先查询的二级缓存，查到直接返回
2. 查不到，再查一级缓存，查到直接返回
3. 查不到，查数据库

```java
    @Test
    public void getUserById2() {


        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);

        //查表
        User user1 = mapper.getUserById(1);
        System.out.println(user1);

        //查的一级缓存
        User user11 = mapper.getUserById(1);
        System.out.println(user11);
        System.out.println(user1 == user11);
        sqlSession.close();
        System.out.println("==================");



        SqlSession sqlSession2 = MybatisUtils.getSqlSession();
        UserMapper mapper2 = sqlSession2.getMapper(UserMapper.class);

        //查表
        User user2 = mapper2.getUserById(2);
        System.out.println(user2);
        sqlSession2.close();
        System.out.println("==================");

        SqlSession sqlSession3 = MybatisUtils.getSqlSession();
        UserMapper mapper3 = sqlSession3.getMapper(UserMapper.class);

        //查二级缓存
        User user3 = mapper3.getUserById(2);
        System.out.println(user3);
        System.out.println(user2 == user3);
        sqlSession3.close();
    }
```

结果如下

```
PooledDataSource forcefully closed/removed all connections.
PooledDataSource forcefully closed/removed all connections.
PooledDataSource forcefully closed/removed all connections.
PooledDataSource forcefully closed/removed all connections.
Cache Hit Ratio [com.zyy.dao.UserMapper]: 0.0
Opening JDBC Connection
Created connection 867398280.
==>  Preparing: select id,name,pwd from user where id=?
==> Parameters: 1(Integer)
<==    Columns: id, name, pwd
<==        Row: 1, zyy, 123456
<==      Total: 1
User(id=1, name=zyy, pwd=123456)
Cache Hit Ratio [com.zyy.dao.UserMapper]: 0.0
User(id=1, name=zyy, pwd=123456)
true
Closing JDBC Connection [com.mysql.jdbc.JDBC4Connection@33b37288]
Returned connection 867398280 to pool.
==================
Cache Hit Ratio [com.zyy.dao.UserMapper]: 0.0
Opening JDBC Connection
Checked out connection 867398280 from pool.
==>  Preparing: select id,name,pwd from user where id=?
==> Parameters: 2(Integer)
<==    Columns: id, name, pwd
<==        Row: 2, 张三, 111111
<==      Total: 1
User(id=2, name=张三, pwd=111111)
Closing JDBC Connection [com.mysql.jdbc.JDBC4Connection@33b37288]
Returned connection 867398280 to pool.
==================
As you are using functionality that deserializes object streams, it is recommended to define the JEP-290 serial filter. Please refer to https://docs.oracle.com/pls/topic/lookup?ctx=javase15&id=GUID-8296D8E8-2B93-4B9A-856E-0A65AF9B8C66
Cache Hit Ratio [com.zyy.dao.UserMapper]: 0.25
User(id=2, name=张三, pwd=111111)
false
```

![image-20220501164206272](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220501164206272.png)

## 13.6、自定义缓存-ehcache

Ehcache是一种广泛使用的开源java分布式缓存，只要面向通用缓存。



要在程序中使用ehcache，先导包

```xml
        <dependency>
            <groupId>org.mybatis.caches</groupId>
            <artifactId>mybatis-ehcache</artifactId>
            <version>1.1.0</version>
        </dependency>
```

在mapper中指定使用我们的ehcache缓存实现

```xml
    <cache type="org.mybatis.caches.ehcache.EhcacheCache"/>
```

配置文件ehcache.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="http://ehcache.org/ehcache.xsd"
         updateCheck="false">
    <!--
       diskStore：为缓存路径，ehcache分为内存和磁盘两级，此属性定义磁盘的缓存位置。参数解释如下：
       user.home – 用户主目录
       user.dir – 用户当前工作目录
       java.io.tmpdir – 默认临时文件路径
     -->
    <diskStore path="./tmpdir/Tmp_EhCache"/>

    <defaultCache
            eternal="false"
            maxElementsInMemory="10000"
            overflowToDisk="false"
            diskPersistent="false"
            timeToIdleSeconds="1800"
            timeToLiveSeconds="259200"
            memoryStoreEvictionPolicy="LRU"/>

    <cache
            name="cloud_user"
            eternal="false"
            maxElementsInMemory="5000"
            overflowToDisk="false"
            diskPersistent="false"
            timeToIdleSeconds="1800"
            timeToLiveSeconds="1800"
            memoryStoreEvictionPolicy="LRU"/>
    <!--
       defaultCache：默认缓存策略，当ehcache找不到定义的缓存时，则使用这个缓存策略。只能定义一个。
     -->
    <!--
      name:缓存名称。
      maxElementsInMemory:缓存最大数目
      maxElementsOnDisk：硬盘最大缓存个数。
      eternal:对象是否永久有效，一但设置了，timeout将不起作用。
      overflowToDisk:是否保存到磁盘，当系统当机时
      timeToIdleSeconds:设置对象在失效前的允许闲置时间（单位：秒）。仅当eternal=false对象不是永久有效时使用，可选属性，默认值是0，也就是可闲置时间无穷大。
      timeToLiveSeconds:设置对象在失效前允许存活时间（单位：秒）。最大时间介于创建时间和失效时间之间。仅当eternal=false对象不是永久有效时使用，默认是0.，也就是对象存活时间无穷大。
      diskPersistent：是否缓存虚拟机重启期数据 Whether the disk store persists between restarts of the Virtual Machine. The default value is false.
      diskSpoolBufferSizeMB：这个参数设置DiskStore（磁盘缓存）的缓存区大小。默认是30MB。每个Cache都应该有自己的一个缓冲区。
      diskExpiryThreadIntervalSeconds：磁盘失效线程运行时间间隔，默认是120秒。
      memoryStoreEvictionPolicy：当达到maxElementsInMemory限制时，Ehcache将会根据指定的策略去清理内存。默认策略是LRU（最近最少使用）。你可以设置为FIFO（先进先出）或是LFU（较少使用）。
      clearOnFlush：内存数量最大时是否清除。
      memoryStoreEvictionPolicy:可选策略有：LRU（最近最少使用，默认策略）、FIFO（先进先出）、LFU（最少访问次数）。
      FIFO，first in first out，这个是大家最熟的，先进先出。
      LFU， Less Frequently Used，就是上面例子中使用的策略，直白一点就是讲一直以来最少被使用的。如上面所讲，缓存的元素有一个hit属性，hit值最小的将会被清出缓存。
      LRU，Least Recently Used，最近最少使用的，缓存的元素有一个时间戳，当缓存容量满了，而又需要腾出地方来缓存新的元素的时候，那么现有缓存元素中时间戳离当前时间最远的元素将被清出缓存。
   -->

</ehcache>
```



Redis做数据库缓存！



