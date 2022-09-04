### spring配置

#### 别名

```xml
    <bean id="user" class="com.zyy.pojo.User">
        <constructor-arg name="name" value="zyy04"/>
    </bean>
    <!--  别名，如果添加了别名，我们也可以使用别名获取到这个对象 -->
    <alias name="user" alias="userNew"/>
```

#### bean的配置

```xml
    <!--
     id：bean的标识符,要唯一,如果没有配置id,name就是默认标识符
     class：bean对象所对应的全限定名：包名+类名
     name：如果配置id,又配置了name,那么name是别名，而且name可以同时取多个别名，name可以设置多个别名,可以用逗号,分号,空格隔开
     -->
    <bean id="user" class="com.zyy.pojo.User" name="u1 u2,u3;u4">
        <constructor-arg name="name" value="zyy04"/>
    </bean>
```

#### import

这个import，一般用于团队开发，它可以将多个配置文件，导入合并为一个

假设，现在项目中有多个人开发，这三个人负责不同的类开发，不同的类需要注册在不同的bean中，我们可以利用import将所有的beans.xml合并为一个总的

- 张三

- 李四

- 王五

- applicationContext.xml

  ```xml
      <import resource="beans1.xml"/>
      <import resource="beans2.xml"/>
      <import resource="beans3.xml"/>
  ```

使用的时候，直接使用总的配置就可以了。
