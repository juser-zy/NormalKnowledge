### filter
>导包

```java
import javax.servlet.Filter;
```

CharacterEncodingFilter中文编码过滤器
```java
public class CharacterEncodingFilter implements Filter {  
    @Override  
    //初始化  
    //web服务器启动的时候，初始化就会执行，随时等待过滤  
    public void init(FilterConfig filterConfig) throws ServletException {  
        System.out.println("已经初始化了");  
    }  
  
    @Override  
    //Chain：链  
    /**  
     * chain 链  
     * 1.过滤器中的代码，在过滤特定请求的时候会执行  
     * 2.必须让过滤器继续通行  
     *   chain.doFilter(request, response);  
     * @param servletRequest  
     * @param servletResponse  
     * @param filterChain  
     * @throws IOException  
     * @throws ServletException  
     */  
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {  
        servletRequest.setCharacterEncoding("utf-8");  
        servletResponse.setCharacterEncoding("utf-8");  
        servletResponse.setContentType("text/html;charset=utf-8");  
  
        System.out.println("CharacterEncodingFilter执行前");  
        //让请求继续走，如果不写程序到这里就被拦截停止了！  
        filterChain.doFilter(servletRequest,servletResponse);  
        System.out.println("CharacterEncodingFilter执行后");  
    }  
  
    @Override  
    //销毁  
    public void destroy() {  
        System.out.println("已经销毁了");  
    }  
}
```

```xml
<filter>  
    <filter-name>CharacterEncodingFilter</filter-name>  
    <filter-class>com.jzy.filter.CharacterEncodingFilter</filter-class>  
</filter>  
<filter-mapping>  
    <filter-name>CharacterEncodingFilter</filter-name>  
    <!--/servlet的任何请求，都会经过这个过滤器-->  
    <url-pattern>/servlet/*</url-pattern>  
</filter-mapping>
```

OnlineCountListener网上人数监听

```java
package com.jzy.listen;  
  
import javax.servlet.ServletContext;  
import javax.servlet.http.HttpSessionEvent;  
import javax.servlet.http.HttpSessionListener;  
  
/**  
 * Created with IntelliJ IDEA. * User: JWhale * Date: 2022/8/12 * Time: 下午 3:28  
 * Description: */public class OnlineCountListener implements HttpSessionListener {  
    @Override  
    /**  
     * 创建session监听，一旦创建session就会触发这个事件  
     * @param se  
     */  
    public void sessionCreated(HttpSessionEvent se) {  
        System.out.println(se.getSession().getId());  
        ServletContext ctx = se.getSession().getServletContext();  
        Integer onlineCount = (Integer) ctx.getAttribute("OnlineCount");  
        if (onlineCount==null){  
            onlineCount = new Integer(1);  
        }else {  
            int count = onlineCount.intValue();  
            onlineCount = new Integer(count + 1);  
        }  
        ctx.setAttribute("OnlineCount",onlineCount);  
  
    }  
  
    @Override  
    /**  
     * 销毁session监听，一旦销毁session就会触发这个事件  
     * @param se  
     */  
    public void sessionDestroyed(HttpSessionEvent se) {  
        ServletContext ctx = se.getSession().getServletContext();  
        Integer onlineCount = (Integer) ctx.getAttribute("OnlineCount");  
        if (onlineCount==null){  
            onlineCount = new Integer(0);  
        }else {  
            int count = onlineCount.intValue();  
            onlineCount = new Integer(count - 1);  
        }  
        ctx.setAttribute("OnlineCount",onlineCount);  
    }  
    /**  
     * session的销毁  
     * 1.手动销毁  getSession().invalidate();  
     * 2.自动销毁  
     *     <session-config>  
     *         <session-timeout>1</session-timeout>  
     *     </session-config>  
     *     * 3.关闭服务器  
     */  
}
```

```xml
<listener>  
    <listener-class>com.jzy.listen.OnlineCountListener</listener-class>  
</listener>
```