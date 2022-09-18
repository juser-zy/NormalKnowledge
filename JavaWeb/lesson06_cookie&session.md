### cookie
>基本:
>从请求中拿到cookie信息
>服务器响应给客户端cookie

pom.xml
```xml
<dependencies>  
  <dependency>  
    <groupId>javax.servlet</groupId>  
    <artifactId>javax.servlet-api</artifactId>  
    <version>4.0.1</version>  
  </dependency>  
  
  <!-- https://mvnrepository.com/artifact/javax.servlet.jsp/javax.servlet.jsp-api -->  
  <dependency>  
    <groupId>javax.servlet.jsp</groupId>  
    <artifactId>javax.servlet.jsp-api</artifactId>  
    <version>2.3.3</version>  
    <scope>provided</scope>  
  </dependency>  
  
</dependencies>
```

```java
Cookie[] cookies = req.getCookies();//获取cookie
cookie.getName();//获取cookie中的key
cookie.getValue();//获取cookie中的value
Cookie cookie = new Cookie("lastLoginTime", "" + System.currentTimeMillis());//新建一个cookie
cookie.setMaxAge(24*60*60);//设置cookie的有效期
resp.addCookie(cookie);//响应给客户端一个cookie
```

```java
public class CookieDemo01 extends HttpServlet {  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        //解决中文乱码  
        req.setCharacterEncoding("utf-16");  
        resp.setCharacterEncoding("utf-16");  
  
        PrintWriter out = resp.getWriter();  
  
        //Cookie,服务器端从客户端获取  
        Cookie[] cookies = req.getCookies();//这里返回的是数组，说明Cookie可能存在多个  
  
        //判断Cookie是否存在  
        if(cookies != null){  
            //如果存在  
            out.write("上一次访问的时间是：");  
  
            for(Cookie cookie:cookies){  
                if(cookie.getName().equals("lastLoginTime")){  
                    //获取cookie的值  
                   //cookie.getValue();字符串，转一下  
                    long lastLoginTime = Long.parseLong(cookie.getValue());  
                    Date date = new Date(lastLoginTime);  
                    out.write(date.toLocaleString());  
                }  
            }  
            /*  
            for(int i = 0 ; i < cookies.length;i++){                Cookie cookie = cookies[i];            }*/  
        }else{  
            out.write("这是您第一次访问");  
        }  
  
        //服务端给客户端响应一个cookie  
        Cookie cookie = new Cookie("lastLoginTime",System.currentTimeMillis()+"");  
        cookie.setMaxAge(28*60*60);  
  
        resp.addCookie(cookie);  
    }  
  
    @Override  
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        doGet(req, resp);  
    }  
}
```

### session
>服务器会给每一个用户（浏览器）创建一个session对象
>一个session独占一个浏览器，只要浏览器没有关闭，session就存在
>用户登录之后，整个网站它都可以访问->保存用户的信息，保存购物车的信息

```java
public class SessionDemo01 extends HttpServlet {  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        //解决乱码问题  
        resp.setContentType("text/html;charset=utf-8");  
        resp.setCharacterEncoding("utf-8");  
        req.setCharacterEncoding("utf-8");  
        //得到session  
        HttpSession session = req.getSession();  
        //给session存东西  
        //session.setAttribute("name","金正");  
        session.setAttribute("name",new Person("金正",64));  
  
        //获取sessionID  
        String sessionId = session.getId();  
  
        //判断session是否为新  
        if(session.isNew()){  
            resp.getWriter().write("session创建成功，ID为" + sessionId);  
        }else{  
            resp.getWriter().write("session已经存在，ID为"+sessionId);  
        }  
  
        //session创建的时候做了什么事情  
        //Cookie cookie = new Cookie("JSESSIONID",sessionId);  
        //resp.addCookie(cookie);  
  
    }  
  
    @Override  
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        doGet(req, resp);  
    }  
}

public class Person {  
    private String name;  
    private int age;  
  
    public Person() {  
    }  
  
    public Person(String name, int age) {  
        this.name = name;  
        this.age = age;  
    }  
  
    @Override  
    public String toString() {  
        return "Person{" +  
                "name='" + name + '\'' +  
                ", age=" + age +  
                '}';  
    }  
  
    public String getName() {  
        return name;  
    }  
  
    public void setName(String name) {  
        this.name = name;  
    }  
  
    public int getAge() {  
        return age;  
    }  
  
    public void setAge(int age) {  
        this.age = age;  
    }  
}
```


得到session的值

```java
public class SessionDemo02 extends HttpServlet {  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        resp.setCharacterEncoding("utf-8");  
        req.setCharacterEncoding("utf-8");  
        resp.setContentType("text/html;charset=utf-8");  
  
        HttpSession session = req.getSession();  
  
        Person person = (Person) session.getAttribute("name");  
  
        resp.getWriter().write(person.toString());  
    }  
  
    @Override  
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        doGet(req, resp);  
    }  
}
```

session失效
```java
session.invalidate();
```
或者
```xml
<!--设置session默认的失效时间-->  
<session-config>  
<!--15min后自动失效-->  
    <session-timeout>15</session-timeout>  
</session-config>
```
