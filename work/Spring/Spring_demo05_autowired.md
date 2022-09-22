>在spring中有三种装配的方式
>1. 在xml中显式配置
>2. 在java中显式配置
>3. 隐式的bean发现机制和自动装配【重要】
>
>Spring的自动装配需要从两个角度来实现，或者说是两个操作：
>1.组件扫描(component scanning)：spring会自动发现应用上下文中所创建的bean
>2.自动装配(autowiring)：spring自动满足bean之间的依赖，也就是我们说的IoC/DI
>

### 环境搭建
环境搭建：一个人有两个宠物！
1、新建一个项目
2、新建两个实体类，Cat  Dog  都有一个叫的方法

**Cat.java**
```java
public class Cat {
    public void shout() {
        System.out.println("miao~");
    }
}
```

**Dog.java**
```java
public class Dog {
    public void shout() {
        System.out.println("wang~");
    }
}
```

**People.java**
```java
public class People {
    private String name;
    private Dog dog;
    private Cat cat;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Dog getDog() {
        return dog;
    }

    public void setDog(Dog dog) {
        this.dog = dog;
    }

    public Cat getCat() {
        return cat;
    }

    public void setCat(Cat cat) {
        this.cat = cat;
    }

    @Override
    public String toString() {
        return "People{" +
                "name='" + name + '\'' +
                ", dog=" + dog +
                ", cat=" + cat +
                '}';
    }
}
```


### ByName & ByType自动装配
**beans.xml**
```xml
<bean id="cat" class="com.jzy.pojo.Cat"></bean>  
<bean id="dog" class="com.jzy.pojo.Dog"></bean>  
<!--  
<bean id="people" class="com.zyy.pojo.People">  
<property name="name" value="zyy"/>  
<property name="dog" ref="dog"/>  
<property name="cat" ref="cat"/>  
</bean>  
-->  
<!--  
 byName:会自动在容器上下本中查找，和自己对象set方法后面的值对应的beanId  
 如setDog 中的dog对应的beanId  
 setDog setdog 都可以找到  
 setDOG 就找不到了  
 -->  
<!--  
 byType:会自动在容器上下本中查找，和自己对象属性类型相同的bean  
 -->
 <bean id="people" class="com.jzy.pojo.People" autowire="byName">  
    <property name="name" value="jzy"/>  
</bean>
```

- byName的时候，需要保证所有的bean 的id唯一，并且这个bean需要和自动注入的属性的set方法的值一致
- byType的时候，需要保证所有bean的class唯一，并且这个bean需要和自动注入的属性的类型一致

#### 使用注解实现自动装配
1. 导入约束，context约束
2. 配置注解的配置`<context:annotation-config/>`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>

</beans>
```

**@Autowired**

直接在属性上使用即可，也可以在set方法上使用！

使用Autowired我们可以不用编写set方法，前提是你这个自动装配的属性在IOC（Spring）容器中存在，且类型符合byType


如果@Autowired自动装配的环境比较复杂，自动装配无法通过一个注解【@Autowired】完成的时候，我们可以使用@Qualifier(value = "***")去配置@Autowired的使用，指定一个唯一的bean对象注入！

```xml
    <bean id="cat1" class="com.zyy.pojo.Cat"/>
    <bean id="cat2" class="com.zyy.pojo.Cat"/>
    <bean id="dog1" class="com.zyy.pojo.Dog"/>
    <bean id="dog2" class="com.zyy.pojo.Dog"/>
    <bean id="people" class="com.zyy.pojo.People"/>
```

```java
public class People {
    private String name;
    /**
     * 如果显式定义了Autowired的required属性为false,说明这个对象可以为null，否则不允许为空
     * @Autowired是根据类型自动装配的，加上@Qualifier则可以根据byName的方式自动装配
     * @Qualifier不能单独使用
     */
    @Autowired(required = false)
    @Qualifier(value = "dog1")
    private Dog dog;
    @Autowired
    @Qualifier(value = "cat1")
    private Cat cat;
}
```

**@Resource注解**

```java
public class People {
    private String name;
    @Resource(name = "dog1")
    private Dog dog;
    @Resource(name = "cat1")
    private Cat cat;
}
```

小结：

@Resource和@Autowired的区别

- 都是用来自动装配的，都可以放到属性字段上
- @Autowired通过byType的方式实现(ps：只根据byType匹配，不会匹配byName)，而且必须要求这个对象存在！【常用】
- @Resource默认通过byName的方式实现，如果找不到名字，则通过byType实现，如果两个都找不到的情况就报错【常用】
- 执行顺序不同：@Autowired通过byType的方式实现，@Resource默认通过byName的方式实现



