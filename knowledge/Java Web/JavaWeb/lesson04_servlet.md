### example
>loginServlet

```java
public class loginServlet extends HttpServlet {  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        ServletContext context = this.getServletContext();  
        resp.setContentType("text/html");  
        resp.setCharacterEncoding("utf-8");  
        req.setCharacterEncoding("utf-8");  
  
        String username = req.getParameter("username");  
        String password = req.getParameter("password");  
        String[] hobby = req.getParameterValues("hobbies");  
  
        System.out.println(username);  
        System.out.println(password);  
        System.out.println(Arrays.toString(hobby));  
  
  
        req.getRequestDispatcher("/page/success.jsp").forward(req,resp);  
    }  
  
    @Override  
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        doGet(req, resp);  
    }  
}
```

>success.jsp

```java
<%--  
  Created by IntelliJ IDEA.  User: JWhale  Date: 2022/12/30  Time: 下午 4:30  To change this template use File | Settings | File Templates.--%>  
<%@ page contentType="text/html;charset=UTF-8" language="java" %>  
<html>  
<head>  
    <title>Title</title>  
</head>  
<body>  
<p1>真好啊</p1>  
<br>  
<%=request.getParameter("username")%>  
<%=request.getParameter("password")%>  
  
<br>  
${param.username}  
${param.password}  
${paramValues.hobbies[0]}  
  
  
</body>  
</html>
```

>index.jsp

```java
<h1>登录</h1>  
<div style="text-align: center">  
  <form action="${pageContext.request.contextPath}/login" method="post">  
    用户名：<input type="text" name="username"><br>  
    密  码：<input type="password" name="password"><br>  
    爱  好：  
    <input type="checkbox" name="hobbies" value="看电影"/>看电影  
    <input type="checkbox" name="hobbies" value="阅读"/>阅读  
    <input type="checkbox" name="hobbies" value="爬山"/>爬山  
    <input type="checkbox" name="hobbies" value="摄影"/>摄影  
    <br>  
    <input type="submit"/>  
  </form>  
</div>
```


### servletContext基本

>核心代码

```java
ServletContext context = this.getServletContext(); 
context.setAttribute("username",username);

String username = (String)context.getAttribute("username");  

```

```java
//设置context
public class HelloServlet extends HttpServlet {  
  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
  
        //this.getInitParameter();初始化参数  
        //this.getServletConfig();配置  
        //this.getServletContext();上下文  
        ServletContext context = this.getServletContext();  
  
        String username = "金";  
        context.setAttribute("username",username);//将一个数据保存在ServletContext中  
  
        PrintWriter writer = resp.getWriter();  
        writer.print("hello");  
    }  
  
    @Override  
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        doGet(req, resp);  
    }  
}


//获得context
public class GetServlet extends HttpServlet {  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        ServletContext context = this.getServletContext();  
        String username = (String)context.getAttribute("username");  
  
        resp.setContentType("text/html");  
        resp.setCharacterEncoding("utf-8");  
        PrintWriter writer = resp.getWriter();  
        writer.print("名字"+username);  
    }  
  
    @Override  
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        doGet(req, resp);  
    }  
}

```


### servletContext应用

>获取初始化参数
>web.xml内部即可设置

```XML
<!--配置一些web应用初始化参数-->  
<context-param>  
    <param-name>url</param-name>  
    <param-value>jdbc:mysql://localhost:3306/mybatis</param-value>  
</context-param>

<servlet>  
    <servlet-name>gp</servlet-name>  
    <servlet-class>com.jzy.servlet.servletdemo_03</servlet-class>  
</servlet>  
<servlet-mapping>  
    <servlet-name>gp</servlet-name>  
    <url-pattern>/gp</url-pattern>  
</servlet-mapping>
```

```java
public class servletdemo_03 extends HelloServlet{  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        ServletContext context = this.getServletContext();  
        String url = context.getInitParameter("url");  
        resp.getWriter().print(url);  
    }  
  
    @Override  
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        doGet(req, resp);  
    }  
}
```

>请求转发

```XML
<servlet>  
    <servlet-name>sd4</servlet-name>  
    <servlet-class>com.jzy.servlet.servletdemo_04</servlet-class>  
</servlet>  
<servlet-mapping>  
    <servlet-name>sd4</servlet-name>  
    <url-pattern>/sd4</url-pattern>  
</servlet-mapping>
```

```java
public class servletdemo_04 extends HttpServlet {  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        System.out.println("进入了sd4");  
        ServletContext context = this.getServletContext();  
        //RequestDispatcher requestDispatcher = context.getRequestDispatcher("/gp");//转发的请求路径  
        //requestDispatcher.forward(req,resp);//实现请求转发  
        context.getRequestDispatcher("/gp").forward(req,resp);  
    }  
  
    @Override  
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        doGet(req, resp);  
    }  
}
```


>读取资源文件

```XML
<servlet>  
    <servlet-name>sd5</servlet-name>  
    <servlet-class>com.jzy.servlet.servletdemo_05</servlet-class>  
</servlet>  
<servlet-mapping>  
    <servlet-name>sd5</servlet-name>  
    <url-pattern>/sd5</url-pattern>  
</servlet-mapping>
```


```java
public class servletdemo_05 extends HttpServlet {  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
//        InputStream is = this.getServletContext().getResourceAsStream("/WEB-INF/classes/db.properties");  
        InputStream is = this.getServletContext().getResourceAsStream("/WEB-INF/classes/com/jzy/servlet/db.properties");  
        Properties pro = new Properties();  
        pro.load(is);  
        String user = pro.getProperty("username");  
        String pwd = pro.getProperty("password");  
        resp.getWriter().print("username:" + user + '\n' + "pwd:" + pwd);  
    }  
  
    @Override  
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        doGet(req, resp);  
    }  
}
```