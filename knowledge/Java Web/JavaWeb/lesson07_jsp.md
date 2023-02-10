### jsp基本语法

pom.xml

```xml
<dependencies>  
    <dependency>  
        <groupId>javax.servlet</groupId>  
        <artifactId>javax.servlet-api</artifactId>  
        <version>4.0.1</version>  
    </dependency>  
    <dependency>  
        <groupId>javax.servlet.jsp</groupId>  
        <artifactId>javax.servlet.jsp-api</artifactId>  
        <version>2.3.3</version>  
    </dependency>  
    <!-- https://mvnrepository.com/artifact/javax.servlet.jsp.jstl/jstl-api -->  
    <dependency>  
        <groupId>javax.servlet.jsp.jstl</groupId>  
        <artifactId>jstl-api</artifactId>  
        <version>1.2</version>  
    </dependency>  
  
    <!-- https://mvnrepository.com/artifact/taglibs/standard -->  
    <dependency>  
        <groupId>taglibs</groupId>  
        <artifactId>standard</artifactId>  
        <version>1.1.2</version>  
    </dependency>  
  
  
</dependencies>
```

example.jsp
```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>  
<html>  
  <head>  
    <title>$Title$</title>  
  </head>  
  <body>  
  <%--JSP表达式  
  作用,用来将程序的输出,输出到客户端  
  <%=变量或者表达式%>  
  --%>  <%=new java.util.Date()%>  
  
  <hr>  
  <%--JSP脚本片段--%>  
  <%  
    int sum = 0;  
    for(int i = 0 ; i <=100 ;i++){  
      sum += i;  
    }    out.println("<h1>Sum=" + sum + "</h1>");  
  %>  
  
  <%  
    int x = 10;  
    out.println(x);  
  %>  
  <p>这是一个JSP文档</p>  
  <%  
    int y = 2;  
    out.println(y);  
  %>  
  <hr>  
  <%--在代码中嵌入html元素--%>  
  <%  
    for (int j = 0; j < 5; j++) {  
  %>  
  <h2>hello<%=j%></h2>  
  <%  
    }  
  %>  
  
  <hr>  
  <%--声明--%>  
  <%!  
    static {  
      System.out.println("servlet is loading...");  
    }  
    private String globalVar = "jzy";  
  
    public void jzy() {  
      System.out.println("jzy method！");  
    }  %>  
  
  <%jzy();%>  
  
  
  </body>  
</html>
```

>**内置对象**

1. PageContext【存东西】
2. Request 【存东西】
3. Response
4. Session【存东西】
5. Application（ServletContext）【存东西】
6. config（ServletConfig）
7. out
8. page 【基本用不到】
9. exception

>**作用域**

```jsp
<%--  
  Created by IntelliJ IDEA.  User: JWhale  Date: 2022/8/12  Time: 上午 10:00  To change this template use File | Settings | File Templates.--%>  
<%@ page contentType="text/html;charset=UTF-8" language="java" %>  
<html>  
<head>  
    <title>Title</title>  
</head>  
<body>  
<%--内置对象--%>  
<%  
  session.setAttribute("user","jzy");  
  pageContext.setAttribute("name1","jzyy01");//保存的数据只在一个页面中有效  
  request.setAttribute("name2","jzyy02");//保存的数据只在一次请求中有效，请求转发会携带这个数据  
  session.setAttribute("name3","jzyy03");//保存的数据只在一次会话中有效，从打开浏览器到关闭浏览器  
  application.setAttribute("name4","jzy04");//保存的数据只在服务器中有效，从打开服务器到关闭服务器  
%>  
  
<%--脚本片段中的代码，会原封不动生成xxx.jsp.java  
要求：这里面的代码，必须保证java语法的正确性  
--%>  
<%  
  //从pageContext取出，我们通过寻找的方式来  
  String name1 = (String) pageContext.findAttribute("name1");  
  String name2 = (String) pageContext.findAttribute("name2");  
  String name3 = (String) pageContext.findAttribute("name3");  
  String name4 = (String) pageContext.findAttribute("name4");  
  String name5 = (String) pageContext.findAttribute("name5");//不存在  
%>  
  
<%--使用el表达式输出  ${} --%><h1>取出的值为</h1>  
<h3>${name1}</h3>  
<h3>${name1}</h3>  
<h3>${name2}</h3>  
<h3>${name3}</h3>  
<h3>${name4}</h3>  
<h3>${name5}不存在</h3>  
<h3><%=name5%>不存在</h3>  
</body>  
</html>
```
### jsp标签

>请求转发并带有参数

```jsp
<%--  
  Created by IntelliJ IDEA.  User: JWhale  Date: 2022/8/12  Time: 上午 11:11  To change this template use File | Settings | File Templates.--%>  
<%@ page contentType="text/html;charset=UTF-8" language="java" %>  
<html>  
<head>  
    <title>Title</title>  
</head>  
<body>  
  
<h2>1</h2>  
  
  
<%--<jsp:include></jsp:include>--%>  
<%-- http://localhost:8080/jsptag.jsp?name=jzy&age=2 --%>  
<jsp:forward page="/jsptag2.jsp">  
    <jsp:param name="name" value="jzy"/>  
    <jsp:param name="age" value="2"/>  
</jsp:forward>  
  
  
</body>  
</html>
```

>取出参数

```jsp
<%--  
  Created by IntelliJ IDEA.  User: JWhale  Date: 2022/8/12  Time: 上午 11:13  To change this template use File | Settings | File Templates.--%>  
<%@ page contentType="text/html;charset=UTF-8" language="java" %>  
<html>  
<head>  
    <title>Title</title>  
</head>  
<body>  
  
<%request.setCharacterEncoding("utf-8");%>  
  
<h2>2</h2>  
  
<%--取出参数--%>  
名字：<%=request.getParameter("name")%>  
年龄：<%=request.getParameter("age")%>  
  
</body>  
</html>
```

### javabean
写法：
- 必须要有一个无参构造
- 属性必须私有化
- 必须有对应的get/set方法

一般用来和数据库字段做映射

ORM：对象关系映射
- 表---》类
- 字段---》属性
- 行记录--》对象

```jsp
<%--  
  Created by IntelliJ IDEA.  User: JWhale  Date: 2022/8/12  Time: 下午 1:06  To change this template use File | Settings | File Templates.--%>  
<%@ page import="com.jzy.pojo.People" %>  
<%@ page contentType="text/html;charset=UTF-8" language="java" %>  
<html>  
<head>  
    <title>Title</title>  
</head>  
<body>  
  
<%  
//  People people = new People();  
//  people.setAddress("南京");  
//  people.setId(1);  
//  people.setAge(5);  
//  people.setName("jzy");  
%>  
  
<jsp:useBean id="people" class="com.jzy.pojo.People" scope="page"></jsp:useBean>  
<jsp:setProperty name="people" property="address" value="南京"></jsp:setProperty>  
<jsp:setProperty name="people" property="age" value="12"></jsp:setProperty>  
<jsp:setProperty name="people" property="id" value="1"></jsp:setProperty>  
<jsp:setProperty name="people" property="name" value="jzy"></jsp:setProperty>  
  
姓名：<jsp:getProperty name="people" property="name"/>  
ID ：<jsp:getProperty name="people" property="id"/>  
年龄：<jsp:getProperty name="people" property="age"/>  
  
</body>  
</html>
```
