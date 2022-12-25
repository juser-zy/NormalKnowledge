SpringBoot使用一个全局的配置文件 ， 配置文件名称是固定的

- application.properties

  语法结构：key=value

- application.yml

  语法结构：key: 空格value

>application.yml中加入Person

```yml
person:  
  name: jzy  
  age: 18  
  happy: true  
  birth: 1999/11/20  
  maps: {k1: v1, k2: v2}  
  lists:  
    - code  
    - music  
  dog:  
    name: 小黄  
    age: 5
```

>Pojo的Person.java中添加@ConfigurationProperties

```java
@Data  
@AllArgsConstructor  
@NoArgsConstructor  
@Component  
@ConfigurationProperties(prefix = "person")  
public class Person {  
    private String name;  
    private Integer age;  
    private Boolean happy;  
    private Date birth;  
    private Map<String,Object> maps;  
    private List<Object> lists;  
    private Dog dog;  
}
```

>test中进行测试，注意@Autowired

```java
@SpringBootTest  
class Springboot02ConfigApplicationTests {  
  
    @Autowired  
    private Dog dog;  
    @Autowired  
    private Person person;  
  
    @Test  
    void contextLoads() {  
        System.out.println(dog);  
        System.out.println(person);  
    }  
  
}
```

#### 其他配置形式
>指定配置文件

>Pojo的Person.java中添加@PropertySource(value = "classpath:jzy.properties")  
```java
@Data  
@AllArgsConstructor  
@NoArgsConstructor  
@Component  
//@ConfigurationProperties(prefix = "person")  
@PropertySource(value = "classpath:jzy.properties")  
public class Person {  
    //SPEL表达式取出指定配置文件的值  
    @Value("${name}")  
    private String name;  
    private Integer age;  
    private Boolean happy;  
    private Date birth;  
    private Map<String,Object> maps;  
    private List<Object> lists;  
    private Dog dog;  
}
```
