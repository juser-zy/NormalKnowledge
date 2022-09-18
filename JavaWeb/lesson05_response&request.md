>概要
>
>web服务器接收到客户端的http请求，针对这个请求，分别创建
>一个代表请求HttpServletRequest对象
>一个代表响应的HttpServletResponse对象
>
>如果要获取客户端请求过来的参数：HttpServletRequest
>如果要给客户端响应一些信息：HttpServletResponse

### response常见应用

1. 向浏览器输出消息
2. 下载文件
	1. 获取下载文件的路径
	2. 下载的文件名是啥？
	3. 设置想办法让浏览器能都支持我们下载的东西 文件名是中文的时候，可以设置URLEncoder.encode(fileName, "UTF-8")，否则有可能乱码
	4. 获取下载文件的输入流
	5. 创建缓冲区
	6. 获取OutputStrem对象
	7. 将FileOutputStream流写入到buffer缓冲区，使用OutputStream将缓冲区中的数据输出到客户端

```XML
<servlet>  
    <servlet-name>filedown</servlet-name>  
    <servlet-class>com.jzy.servlet.FileServlet</servlet-class>  
</servlet>  
<servlet-mapping>  
    <servlet-name>filedown</servlet-name>  
    <url-pattern>/filedown</url-pattern>  
</servlet-mapping>
```

```java
public class FileServlet extends HttpServlet {  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        //1. 获取下载文件的路径  
        String realPath = "E:\\JavaWorkstation\\javaweb_02_servlet\\response\\src\\main\\resources\\1.png";  
        //String realPath = this.getServletContext().getRealPath("/1.png");  
        System.out.println("下载的文件路径：" + realPath);  
        //2. 下载的文件名是啥？  
        String fileName = realPath.substring(realPath.lastIndexOf("\\")+1);  
        //3. 设置想办法让浏览器能都支持我们下载的东西 文件名是中文的时候，可以设置URLEncoder.encode(fileName, "UTF-8")，否则有可能乱码  
        resp.setHeader("Content-Disposition", "attachment;filename=" + URLEncoder.encode(fileName, "UTF-8"));  
        //4. 获取下载文件的输入流  
        FileInputStream in = new FileInputStream(realPath);  
        //5. 创建缓冲区  
        int len = 0;  
        byte[] buffer = new byte[1024];  
        //6. 获取OutputStrem对象  
        ServletOutputStream out = resp.getOutputStream();  
        //7. 将FileOutputStream流写入到buffer缓冲区，使用OutputStream将缓冲区中的数据输出到客户端  
        while ((len = in.read(buffer))>0){  
            out.write(buffer,0,len);  
        }  
        in.close();  
        out.close();  
    }  
  
    @Override  
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        doGet(req, resp);  
    }  
}
```
### response重定向
```java
public class RedirectServlet extends HttpServlet {  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        resp.setHeader("Location","/response_war/image");  
        resp.setStatus(302);  
        //相当于  
        //resp.sendRedirect("/response_war/image");  
    }  
  
    @Override  
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        doGet(req, resp);  
    }  
}
```
### request获取参数&请求转发

```XML
<servlet>  
    <servlet-name>LoginServlet</servlet-name>  
    <servlet-class>com.jzy.servlet.LoginServlet</servlet-class>  
</servlet>  
<servlet-mapping>  
    <servlet-name>LoginServlet</servlet-name>  
    <url-pattern>/login</url-pattern>  
</servlet-mapping>
```

初始页面
注意form的action
```HTML
<%--  
  Created by IntelliJ IDEA.  User: JWhale  Date: 2022/8/10  Time: 下午 8:36  To change this template use File | Settings | File Templates.--%>  
<%@ page contentType="text/html;charset=UTF-8" language="java" %>  
<html>  
<head>  
    <title>登录</title>  
</head>  
<body>  
<h1>登录</h1>  
<div style="text-align: center">  
<%--表示以post的方式提交表单，提交到我们的login请求--%>  
    <form action="${pageContext.request.contextPath}/login" method="post">  
        用户名：<input type="text" name="username"/><br/>  
        密码：<input type="password" name="pwd"/><br/>  
        爱好：  
        <input type="checkbox" name="hobbies" value="看电影"/>看电影  
        <input type="checkbox" name="hobbies" value="阅读"/>阅读  
        <input type="checkbox" name="hobbies" value="爬山"/>爬山  
        <input type="checkbox" name="hobbies" value="摄影"/>摄影  
        <br/>  
        <input type="submit"/>  
    </form>  
</div>  
  
</body>  
</html>
```

```java
public class LoginServlet extends HttpServlet {  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        //后台接收中文乱码问题  
        req.setCharacterEncoding("utf-8");  
  
        String username = req.getParameter("username");  
        String password = req.getParameter("pwd");  
        String[] hobbies = req.getParameterValues("hobbies");  
  
        System.out.println("======================");  
        System.out.println(username);  
        System.out.println(password);  
  
        System.out.println(Arrays.toString(hobbies));  
        System.out.println("======================");  
        //通过请求转发  
        //这里的/代表当天的web应用  
  
        req.getRequestDispatcher("/success.jsp").forward(req, resp);  
        //这样子也行  
        //resp.sendRedirect("/request_war/success.jsp");  
  
    }  
  
    @Override  
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        doGet(req, resp);  
    }  
}
```