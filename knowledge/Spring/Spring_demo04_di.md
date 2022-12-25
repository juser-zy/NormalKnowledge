### 依赖注入
#### **构造器注入**

#### **set方式注入**
>pojo层Address和Student两个实体类

**Address.java**
```java
package com.jzy.pojo;  
  
public class Address {  
    private String address;  
  
    @Override  
    public String toString() {  
        return "Address{" +  
                "address='" + address + '\'' +  
                '}';  
    }  
  
    public String getAddress() {  
        return address;  
    }  
  
    public void setAddress(String address) {  
        this.address = address;  
    }  
}
```
**Student.java**
```java
public class Student {  
    private String name;  
    private Address address;  
    private String[] books;  
    private List<String> hobbies;  
    private Map<String,String> card;  
    private Set<String> games;  
    private String wife;  
    private Properties info;
}
```

**beans.xml**
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">  
  
    <bean id="address" class="com.jzy.pojo.Address">  
        <property name="address" value="六安"></property>  
    </bean>  
  
    <bean id="student" class="com.jzy.pojo.Student">  
        <!--普通值注入-->  
        <property name="name" value="jzy"></property>  
        <!--Bean注入-->  
        <property name="address" ref="address"></property>  
        <!--数组注入-->  
        <property name="books">  
            <array>  
                <value>红楼萌</value>  
                <value>西游记</value>  
                <value>三国</value>  
            </array>  
        </property>  
        <!--list注入-->  
        <property name="hobbies">  
            <list>  
                <value>听歌</value>  
                <value>Av</value>  
                <value>电影</value>  
            </list>  
        </property>  
        <!--map注入-->  
        <property name="card">  
            <map>  
                <entry key="idNo" value="123456"></entry>  
                <entry key="bankNo" value="54657"/>  
            </map>  
        </property>  
        <!--set注入-->  
        <property name="games">  
            <set>  
                <value>LoL</value>  
                <value>原神</value>  
            </set>  
        </property>  
  
        <!--null注入-->  
        <property name="wife">  
            <null></null>  
        </property>  
  
        <!--properties注入-->  
        <property name="info">  
            <props>  
                <prop key="no">1234</prop>  
                <prop key="sex">男</prop>  
            </props>  
        </property>  
    </bean>  
  
</beans>
```

#### 拓展方式注入
>p命名和c命名注入

```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
       xmlns:p="http://www.springframework.org/schema/p"  
       xmlns:c="http://www.springframework.org/schema/c"  
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">  
  
    <!-- P(属性: properties)命名空间 , 属性依然要设置set方法 -->  
    <bean id="user" class="com.jzy.pojo.User" p:age="23" p:name="jzy"></bean>  
  
    <!-- C(构造: Constructor)命名空间 , 属性依然要设置set方法  
    把有参构造器加上，这里也能知道，c 就是所谓的构造器注入！ -->  
    <bean id="user2" class="com.jzy.pojo.User" c:age="23" c:name="jzyc"></bean>  
  
</beans>
```
