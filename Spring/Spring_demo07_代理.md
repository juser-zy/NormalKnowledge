### 静态代理
>接口：实现的功能 
>即抽象角色：一般会使用接口或者抽象类来解决
>
>真实角色：被代理的角色
>
>代理角色：代理真实角色，代理真实角色后，我们一般会做一些附属操作
>
>客户：访问代理对象的人


>抽象角色

**Rent.java**
```java
public interface Rent {  
    public void rent();  
}
```

>真实角色

**Host.java**
```java
public class Host implements Rent{  
  
    @Override  
    public void rent() {  
        System.out.println("房东要出租房");  
    }  
}
```

>代理角色

**Proxy.java**
```java
public class Proxy {  
    private Host host;  
  
    public Proxy() {  
    }  
  
    public Proxy(Host host) {  
        this.host = host;  
    }  
  
    public void rent(){  
        seeHouse();  
        host.rent();  
        agreement();  
        fee();  
    }  
  
    public void seeHouse() {  
        System.out.println("中介带你看房");  
    }  
    public void agreement() {  
        System.out.println("签合同~");  
    }  
  
    public void fee() {  
        System.out.println("收费~");  
    }  
}
```

>客户

**Client.java**
```java
public class Client {  
    public static void main(String[] args) {  
        Host host = new Host();  
        Proxy proxy = new Proxy(host);  
        proxy.rent();  
    }  
}
```

### 动态代理
- 动态代理和静态代理角色一样
- 动态代理的代理类是动态生成的，不是我们直接写好的
- 动态代理分成两个类：基于接口的动态，基于类的动态代理
  - 基于接口--jdk动态代理【我们在这里使用】
  - 基于类：cglib
  - java字节码实现：javasist


需要了解两个类
- Proxy：代理
- InvocationHandler：调用处理程序


动态代理的好处：
- 可以使真实角色的操作更加纯粹，不用去关注一些公共的业务！
- 公共也就交给代理角色，实现了业务的分工！
- 公共业务发生扩展的时候，方便集中管理！
- 一个动态代理类代理的是一个接口，一般就是对应一类业务
- 一个动态代理类可以代理多个类，只要是实现了同一个接口即可

>抽象角色

**Rent.java**
```java
public interface Rent {  
    public void rent();  
}
```

>真实角色

**Host.java**
```java
public class Host implements Rent{  
  
    @Override  
    public void rent() {  
        System.out.println("房东要出租房");  
    }  
}
```

>代理角色

**ProxyInvocationHandler.java**
```java
public class ProxyInvocationHandler implements InvocationHandler {  
  
    /**  
     * 被代理的接口  
     */  
    private Rent rent;  
  
    public void setRent(Rent rent) {  
        this.rent = rent;  
    }  
  
    /**  
     * 生成得到代理类  
     *  
     * @return     */    public Object getProxy() {  
        return Proxy.newProxyInstance(this.getClass().getClassLoader(), rent.getClass().getInterfaces(), this);  
    }  
  
  
    /**  
     * 处理代理实例，并返回结果  
     */  
    @Override  
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {  
        log(method.getName());  
        //动态代理的本质，就是使用反射机制实现  
        Object result = method.invoke(rent, args);  
        return result;  
    }  
  
    public void log(String msg) {  
        System.out.println("[debug]调用了" + msg + "方法");  
    }  
}
```

>客户

**Client.java**
```java
public class Client {  
    public static void main(String[] args) {  
        //真实角色  
        Host host = new Host();  
  
        //代理角色，现在没有  
        ProxyInvocationHandler pih = new ProxyInvocationHandler();  
  
        //通过调用程序处理角色来处理我们要调用的接口  
        pih.setRent(host);  
  
        Rent proxy = (Rent) pih.getProxy();  
        proxy.rent();  
  
    }  
}
```