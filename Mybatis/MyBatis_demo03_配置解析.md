>主要在mybatis-config.xml上做修改

**db.properties**
```properties
driver = com.mysql.cj.jdbc.Driver  
url = jdbc:mysql://localhost:3306/mybatis?useSSL=true&useUnicode=true&characterEncoding=UTF-8  
username = root  
password = 123456
```


**mybatis-config.xml**
```xml
<?xml version="1.0" encoding="UTF-8" ?>  
<!DOCTYPE configuration  
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"  
        "http://mybatis.org/dtd/mybatis-3-config.dtd">  
  
<configuration>  
  
    <!--引入外部配置文件-->  
    <properties resource="db.properties"></properties>  
  
    <!--给实体类起别名-->  
    <!--    <typeAliases>-->    <!--        <typeAlias type="com.jzy.pojo.User" alias="User"></typeAlias>-->    <!--    </typeAliases>-->  
    <typeAliases>  
        <package name="com.jzy.pojo"/>  
    </typeAliases>  
  
  
  
    <environments default="test">  
        <environment id="development">  
            <transactionManager type="JDBC"/>  
            <dataSource type="POOLED">  
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>  
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>  
                <property name="username" value="root"/>  
                <property name="password" value="123456"/>  
            </dataSource>  
        </environment>  
        <environment id="test">  
            <transactionManager type="JDBC"/>  
            <dataSource type="POOLED">  
                <property name="driver" value="${driver}"/>  
                <property name="url" value="${url}"/>  
                <property name="username" value="${username}"/>  
                <property name="password" value="${password}"/>  
            </dataSource>  
        </environment>  
    </environments>  
    <mappers>  
        <mapper resource="com/jzy/dao/UserMapper.xml"></mapper>  
    </mappers>  
  
  
</configuration>
```


### 环境配置（environments）

一般选择默认的development环境

**不过要记住：尽管可以配置多个环境，但每个 SqlSessionFactory 实例只能选择一种环境**



### 属性（properties）

通过properties属性来实现引用配置文件


### 类型别名（typeAliases）
指定一个包名，MyBatis 会在包名下面搜索需要的 Java Bean

扫描实体类的包，它的默认别名就是这个类的类名，首字母小写。

```xml
    <typeAliases>
        <package name="com.jzy.pojo"/>
    </typeAliases>
```

```xml
    <select id="getUserList" resultType="user">
        select id, name, pwd from `user`
    </select>
```

### 映射器（mappers）

MapperRegistry：注册绑定我们的mapper文件

方式一：

```xml
    <!--   每一个Mapper.xml都需要在mybatis核心配置文件中注册 -->
    <mappers>
        <mapper resource="com/jzy/dao/UserMapper.xml"/>
    </mappers>
```

方式二：使用class文件绑定注册

```xml
    <mappers>
        <mapper class="com.jzy.dao.UserMapper"/>
    </mappers>
```

方式三：使用扫描包进行注入绑定

```xml
    <mappers>
        <package name="com.zyy.dao"/>
    </mappers>
```

注意点：
- 接口和它的Mapper配置文件必须同名！
- 接口和它的Mapper配置文件必须在同一个包下！
