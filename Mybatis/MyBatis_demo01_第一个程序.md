### 搭建环境
- 数据库搭建
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
- 项目搭建

1. 新建一个普通的maven项目

3. 删除src目录（作为父工程）

4. pom.xml中导入maven依赖
```xml
<dependencies>  
    <dependency>  
        <groupId>mysql</groupId>  
        <artifactId>mysql-connector-java</artifactId>  
        <version>8.0.30</version>  
    </dependency>  
  
    <dependency>  
        <groupId>junit</groupId>  
        <artifactId>junit</artifactId>  
        <version>4.12</version>  
        <scope>test</scope>  
    </dependency>  
  
    <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->  
    <dependency>  
        <groupId>org.mybatis</groupId>  
        <artifactId>mybatis</artifactId>  
        <version>3.5.10</version>  
    </dependency>  
  
    <!-- https://mvnrepository.com/artifact/log4j/log4j -->  
    <dependency>  
        <groupId>log4j</groupId>  
        <artifactId>log4j</artifactId>  
        <version>1.2.17</version>  
    </dependency>  
  
</dependencies>
```


### 创建模板
>resources中创建mybatis-config.xml文件
>编写mybatis的核心配置文件
>==注意：mapper的映射关系==


```xml
<?xml version="1.0" encoding="UTF-8" ?>  
<!DOCTYPE configuration  
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"  
        "http://mybatis.org/dtd/mybatis-3-config.dtd">  
  
  
<configuration>  
    <environments default="development">  
        <environment id="development">  
            <transactionManager type="JDBC"/>  
            <dataSource type="POOLED">  
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>  
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>  
                <property name="username" value="root"/>  
                <property name="password" value="123456"/>  
            </dataSource>  
        </environment>  
    </environments>  
    <mappers>  
        <mapper resource="com/jzy/dao/UserMapper.xml"></mapper>  
    </mappers>  
</configuration>
```

>在utils中编写mybatis的工具类MyBatisUtils.java

```java
package com.jzy.utils;  
  
import org.apache.ibatis.io.Resources;  
import org.apache.ibatis.session.SqlSession;  
import org.apache.ibatis.session.SqlSessionFactory;  
import org.apache.ibatis.session.SqlSessionFactoryBuilder;  
  
import java.io.IOException;  
import java.io.InputStream;  
  
/**  
 * Created with IntelliJ IDEA. * User: JWhale * Date: 2022/8/28 * Time: 下午 9:14  
 * Description: */  
//sqlSessionFactory --> sqlSession  
public class MybatisUtils {  
    private static SqlSessionFactory sqlSessionFactory;  
    static {  
        InputStream inputStream = null;  
        try {  
            String resource = "mybatis-config.xml";  
            inputStream = Resources.getResourceAsStream(resource);  
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);  
        } catch (IOException e) {  
            throw new RuntimeException(e);  
        }  
    }  
    //既然有了 SqlSessionFactory，顾名思义，我们可以从中获得 SqlSession 的实例。  
    // SqlSession 提供了在数据库执行 SQL 命令所需的所有方法。  
    // 你可以通过 SqlSession 实例来直接执行已映射的 SQL 语句。  
    public static SqlSession getSqlSession(){  
        return sqlSessionFactory.openSession();  
    }  
}
```


### 编写代码
>总共有dao，pojo，utils三个文件夹
>dao：baiData Access Object数据访问接口，数据持久层，用来操作DB，完成增删改查
>pojo：Plain Old Java Objects简单的Java对象

>pojo包中建立User实体类

```java
package com.jzy.pojo;  
  
/**  
 * Created with IntelliJ IDEA. * User: JWhale * Date: 2022/8/28 * Time: 下午 9:59  
 * Description: */public class User {  
    private int id;  
    private String name;  
    private String pwd;  
  
    public User(int id, String name, String pwd) {  
        this.id = id;  
        this.name = name;  
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
  
    public User() {  
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
}
```

>dao包中建立UserDao接口
>在mybatis中
>接口UserDao->接口UserMapper
>UserDaoImpl.java->UserMapper.xml

**UserMapper**
```java
package com.jzy.dao;  
  
import com.jzy.pojo.User;  
  
import java.util.List;  
import java.util.Map;  
  
public interface UserMapper {  
    //查询全部用户  
    List<User> getUserList();  
  
    //根据ID查询  
    User getUserById(int id);  
  
    //insert  
    int addUser(User user);  
  
    //万能的map  
    int addUser2(Map<String,Object> map);  
  
    int updateUser(User user);  
  
    int deleteUser(int id);  
}
```

**UserMapper.xml**
```xml
<?xml version="1.0" encoding="UTF-8" ?>  
<!DOCTYPE mapper  
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"  
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">  
  
<!--namespace绑定一个对应的Dao/Mapper接口  
select id,name,pwd from mybatis.user where id = #{id}  
-->  
<mapper namespace="com.jzy.dao.UserMapper">  
  
    <select id="getUserList" resultType="com.jzy.pojo.User">  
        select * from mybatis.user    </select>  
  
    <select id="getUserById" resultType="com.jzy.pojo.User" parameterType="int">  
        select * from mybatis.user where id = #{id}    </select>  
  
    <insert id="addUser" parameterType="com.jzy.pojo.User">  
        insert into mybatis.user (id,name,pwd) values (#{id},#{name},#{pwd})    </insert>  
  
    <insert id="addUser2" parameterType="map">  
        insert into mybatis.user (id,name,pwd) values (#{userId},#{userName},#{userPwd})    </insert>  
  
    <update id="updateUser" parameterType="com.jzy.pojo.User">  
        update mybatis.user set name=#{name} , pwd=#{pwd} where id=#{id}    </update>  
  
    <delete id="deleteUser" parameterType="int">  
        delete from mybatis.user where id = #{id}    </delete>  
  
</mapper>
```

**UserDaoTest.java**
```java
package com.jzy.dao;  
import com.jzy.pojo.User;  
import com.jzy.utils.MybatisUtils;  
import org.apache.ibatis.session.SqlSession;  
import org.junit.Test;  
  
import java.util.HashMap;  
import java.util.List;  
  
/**  
 * Created with IntelliJ IDEA. * User: JWhale * Date: 2022/8/28 * Time: 下午 10:22  
 * Description: */public class UserDaoTest {  
    @Test  
    public void getUserList() {  
        SqlSession sqlSession = MybatisUtils.getSqlSession();  
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);  
        List<User> userList = userMapper.getUserList();  
        for (User user:userList) {  
            System.out.println(user);  
        }  
        sqlSession.close();  
    }  
  
    @Test  
    public void getUserById(){  
        SqlSession sqlSession = MybatisUtils.getSqlSession();  
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);  
        User user = userMapper.getUserById(1);  
        System.out.println(user);  
        sqlSession.close();  
    }  
  
    @Test  
    public void addUser(){  
        SqlSession sqlSession = MybatisUtils.getSqlSession();  
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);  
        int num = mapper.addUser(new User(4, "dsad", "654321"));  
        if(num > 0){  
            System.out.println("插入成功");  
        }  
        sqlSession.commit();  
        sqlSession.close();  
    }  
  
    @Test  
    public void updateUser(){  
        SqlSession sqlSession = MybatisUtils.getSqlSession();  
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);  
        int res = mapper.updateUser(new User(4, "金", "123456"));  
        if(res > 0) {  
            System.out.println("修改成功");  
        }  
        sqlSession.commit();  
        sqlSession.close();  
    }  
  
    @Test  
    public void deleteUser(){  
        SqlSession sqlSession = MybatisUtils.getSqlSession();  
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);  
        int res = mapper.deleteUser(4);  
        if(res > 0){  
            System.out.println("删除成功");  
        }  
        sqlSession.commit();  
        sqlSession.close();  
    }  
  
    @Test  
    public void addUser2(){  
        SqlSession sqlSession = MybatisUtils.getSqlSession();  
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);  
        HashMap<String, Object> map = new HashMap<>();  
        map.put("userId",5);  
        map.put("userName","hello");  
        map.put("userPwd","123456321");  
        int res = mapper.addUser2(map);  
        if(res > 0){  
            System.out.println("success");  
        }  
        sqlSession.commit();  
        sqlSession.close();  
  
    }  
}
```