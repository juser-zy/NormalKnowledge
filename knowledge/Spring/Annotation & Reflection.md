# 01.什么是注解

- Annotation是从jdk5.0开始引入的新技术
- Annotation的作用
  - 不是程序本身，可以对程序作出解释（这一点和注释(comment)没什么区别）
  - 可以被其他程序（比如：编译器等）读取
- Annotation的格式
  - 注解是以“@注释名”在代码中存在，还可以添加一些参数值，例如：@SuppressWarnings(value="unchecked")
- Annotation在哪里使用？
  - 可以附加在package,class,method,field等上面，相当于给他们添加了额外的辅助信息，我们可以通过反射机制编程实现对这些元数据的访问

# 02.内置注解

- @Override:定义在java.lang.Override中，此注解只适用于修饰方法，表示一个方法声明打算**重写**超类中的另一个方法声明。
- @Deprecated：定义在java.lang.Deprecated中，此注释可以用于修饰方法，属性，类，表示**不鼓励**程序员使用这样的元素，通常是因为它很危险或者存在更好的选择
- @SuppressWarnings:定义在java.lang.SuppressWarnings，用来**抑制编译时的警告信息**
- @SuppressWarning("all")
- @SuppressWarning("unchecked")
- @SuppressWarnings(value={"unchecked","deprecation"})

# 03.元注解

- 元注解的作用就是负责注解其他注解，java定义了4个标准的mata-annotation类型，他们被用来提供对其他annotation类型做说明。
- 这些类型和他们所支付的类在java.lang.annotation包中可以找到（@Target,@Retention,@Documneted,@Inherited）
- **@Target**:用于描述注解的使用范围（即：被描述的注解可以用在什么地方）
- **@Retention**:表示注解在什么地方还有效，用于描述注解的生命周期（SOURCE<CLASS<RUNTIME）
- @Document：说明该注解将被包含在javadoc中
- @Inherited:说明子类可以继承父类中的该注解

自定义注解

```java
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@interface MyAnnotation1 {
    String value();
}
```

@Target可选择的值有

```java
public enum ElementType {
    /** Class, interface (including annotation type), or enum declaration */
    TYPE,

    /** Field declaration (includes enum constants) */
    FIELD,

    /** Method declaration */
    METHOD,

    /** Formal parameter declaration */
    PARAMETER,

    /** Constructor declaration */
    CONSTRUCTOR,

    /** Local variable declaration */
    LOCAL_VARIABLE,

    /** Annotation type declaration */
    ANNOTATION_TYPE,

    /** Package declaration */
    PACKAGE,

    /**
     * Type parameter declaration
     *
     * @since 1.8
     */
    TYPE_PARAMETER,

    /**
     * Use of a type
     *
     * @since 1.8
     */
    TYPE_USE
}
```

@Retention可选择的值有

```java
public enum RetentionPolicy {
    /**
     * Annotations are to be discarded by the compiler.
     */
    SOURCE,

    /**
     * Annotations are to be recorded in the class file by the compiler
     * but need not be retained by the VM at run time.  This is the default
     * behavior.
     */
    CLASS,

    /**
     * Annotations are to be recorded in the class file by the compiler and
     * retained by the VM at run time, so they may be read reflectively.
     *
     * @see java.lang.reflect.AnnotatedElement
     */
    RUNTIME
}
```



# 04.自定义注解

- 使用@interface自定义注解时，自动继承了java.lang.annotation.Annotation接口
- 分析
- @interface用来声明一个注解，格式：public @interface 注解名{ 定义内容 }
- 其中的每一个方法实际上是声明了一个配置参数
- 方法的名称就是参数的名称
- 返回值类型就是参数类型（返回值只能是基本类型，Class,String,enum）
- 可以通过default来声明参数的默认值
- 如果只有一个参数成员，一般参数名为value
- 注解元素必须要有值，我们定义注解元素时，经常使用空字符串，0作为默认值

```java
public class Test01 {
    @MyAnnotation1(age = 18)
    public void test1() {

    }

    @MyAnnotation2("hello")
    public void test2() {

    }

}

@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@interface MyAnnotation1 {
    //注解的参数：参数类型 + 参数名();
    String name() default "";
    int age();
    //默认返回-1，代表不存在
    int id() default -1;
    String[] schools() default {"西工大", "西电"};
}

@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@interface MyAnnotation2 {
    String value();
}
```



# 05.反射概述

动态语言

- 是一类在运行时可以改变其结构的语言：例如新的函数、对象、甚至代码可以被引进，已有的函数可以被删除或是其他结构上的变化。通俗点说就是在运行时代码可以根据某些条件改变自身结构。
- 主要动态语言：Object-C、C#、JavaScript、PHP、Python等。

静态语言

- 与动态语言相对应的，运行时结构不可变的语言就是静态语言。如java、C、C++
- Java不是动态语言，但Java可以称之为“准动态语言”。即Java有一定的动态性，我们可以利用反射机制获得类似动态语言的特性。Java的动态性让编程的时候更加灵活！



Java Reflection

- Reflection（反射）是Java被视为动态语言的关键，反射机制允许程序在执行期借助于Reflection API取得任何类的内部信息，并能直接操作任意对象的内部属性和方法（Class c = Class.forName("java.lang.String")）
- 加载完类之后，在堆内存的方法区中就产生了一个Class类型的对象（一个类只有一个Class对象），这个对象就包含了完整的类的结构信息。我们可以通过这个对象看到类的结构。这个对象就像一面镜子，透过这个镜子看到类的结构，所以，我们形象的称之为：反射

![image-20210405140025622](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20210405140025622.png)






# 06.获得反射对象

Java反射机制提供的功能

- 在运行时判断任意一个对象所属的类
- 在运行时构造任意一个类的对象
- 在运行时判断任意一个类所具有的成员变量和方法
- 在运行时获取泛型信息
- 在运行时调用任意一个对象的成员变量和方法
- 在运行时处理注解
- 生成动态代理
- 。。。



Java反射优点和缺点

优点：

- 可以实现动态创建对象和编译，体现出很大的灵活性

缺点：

- 对性能有影响。使用反射基本上是一种解释操作，我们可以告诉JVM，我们希望做什么并且它满足我们的要求。这类操作总是慢于直接执行相同的操作。



反射相关的主要API

- java.lang.Class  代表一个类
- java.lang.reflect.Method  代表类的方法
- java.lang.reflect.Field  代表类的成员变量
- java.lang.reflect.Constructor 代表类的构造器
- ...



Class类

在Object类中定义了以下的方法，此方法将被所有子类继承

public final Class getClass()

- 以上的方法返回的类型是一个Class类，此类是Java反射的源头，实际上所谓反射从程序的运行结果来看也好理解，即：可以通过对象反射求出类的名称

```java
public class Test01 {
    public static void main(String[] args) {
        try {
            Class<?> c1 = Class.forName("kuangJava.reflects.User");
            System.out.println(c1);
            Class<?> c2 = Class.forName("kuangJava.reflects.User");
            Class<?> c3 = Class.forName("kuangJava.reflects.User");
            System.out.println(c2.hashCode());
            System.out.println(c3.hashCode());
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}

class User {
    private int id;
    private String name;

    public User() {
    }


    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

输出

class kuangJava.reflects.User
895328852
895328852

# 07.得到Class类的几种方式

Class类

- 对象照镜子后可以得到的信息：某个类的属性、方法和构造器、某个类到底实现了哪些接口。对于每个类而言，JRE都为其保留一个不变的Class类型的对象。一个Class对象包含了特定某个结构（class/interface/enum/annotation/primitive type/void/[]）的相关信息

  - Class本身也是一个类
  - Class对象只能由系统建立对象
  - 一个加载的类在JVM中只会有一个Class实例
  - 一个Class对象对应的是一个加载到JVM中的一个.class文件
  - 每个类的实例都会记得自己是由哪个Class实例所生成
  - 通过Class可以完整地得到一个类中的所有被加载的结构
  - Class类是Reflection的根源，针对任何你想动态加载、运行的类，唯有先获得相应的Class对象。

  

  Class类的常用方法

  ![image-20210405143018327](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20210405143018327.png)

  

  获取Class类的实例

  - 若已知具体的类，通过类的class属性获取，该方法最为安全可靠，程序性能最高

  Class clazz = Person.class;

  - 已知某个类的实例，调用该实例的getClass()方法获取Class对象

  Class clazz = person.getClass();

  - 已知一个类的全类名，且该类在类路径下，可以通过Class类的静态方法forName()获取，可以抛出ClassNotFoundException

  Class clazz = Class.forName("demo01.Student");

  - 内置基本数据类型可以直接用类名.Type
  - 还可以利用ClassLoader（之后讲解）

```java
public class Test03 {
    public static void main(String[] args) {
        Person person = new Student();
        System.out.println("这个人是："+person.getName());

        //方式一：通过对象获得
        Class c1 = person.getClass();
        System.out.println(c1.hashCode());
        //方式二：forName获得
        try {
            Class c2 = Class.forName("kuangJava.reflects.Student");
            System.out.println(c2.hashCode());
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
        //方式三：通过类名.class获得
        Class c3 = Student.class;
        System.out.println(c3.hashCode());

        //方法四：基本内置类型的包装类都有一个Type属性
        Class c4 = Integer.TYPE;
        System.out.println(c4);

        //获取父类类型
        Class c5 = c1.getSuperclass();
        System.out.println(c5);

    }
}

class Person {
    private String name;

    public Person() {

    }

    public Person(String name) {
        this.name = name;
    }


    public void setName(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}

class Student extends Person {
    public Student() {
        setName("学生");
    }
}

class Teacher extends Person {
    public Teacher() {
        setName("老师");
    }
}
```

输出

```
这个人是：学生
895328852
895328852
895328852
int
class kuangJava.reflects.Person
```



# 08.所有类型的Class对象

哪些类型可以有Class对象

- class：外部类，成员（成员内部类，静态内部类），局部内部类，匿名内部类
- interface：接口
- []：数组
- enum：枚举
- annotation：注解@interface
- primitive type：基本数据类型
- void

```java
public class Test04 {
    public static void main(String[] args) {
        //类
        Class c1 = Object.class;
        //接口
        Class c2 = Comparable.class;
        //一维数组
        Class c3 = String[].class;
        //二维数组
        Class c4 = int[][].class;
        //注解
        Class c5 = Override.class;
        //枚举
        Class c6 = ElementType.class;
        //基本数据类型
        Class c7 = Integer.class;
        //void
        Class c8 = void.class;
        //Class
        Class c9 = Class.class;


        System.out.println(c1);
        System.out.println(c2);
        System.out.println(c3);
        System.out.println(c4);
        System.out.println(c5);
        System.out.println(c6);
        System.out.println(c7);
        System.out.println(c8);
        System.out.println(c9);

        //只要元素类型和维度一样的，就是同一个Class
        int[] a = new int[10];
        int[] b = new int[100];
        System.out.println(a.getClass().hashCode());
        System.out.println(b.getClass().hashCode());

    }
}
```

输出

```
class java.lang.Object
interface java.lang.Comparable
class [Ljava.lang.String;
class [[I
interface java.lang.Override
class java.lang.annotation.ElementType
class java.lang.Integer
void
class java.lang.Class
895328852
895328852
```



# 09.类加载内存分析

![image-20210405150539415](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20210405150539415.png)

类的加载过程

![image-20210405150748467](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20210405150748467.png)

# 10.分析类初始化

什么时候会发生类的初始化

- 类的主动引用（一定会发生类的初始化）
  - 当虚拟机启动，先初始化main方法所在的类
  - new一个类的对象
  - 调用类的静态成员（除了final常量）和静态方法
  - 使用java.lang.reflect包的方法对类进行反射调用
  - 当初始化一个类，如果其父类没有被初始化，则先会初始化它的父类
- 类的被动引用（不会发生类的初始化）
  - 当访问一个静态域时，只有真正声明这个域的类才会被初始化，如：当通过子类引用父类的静态变量，不会导致子类初始化
  - 通过数组定义类引用，不会触发此类的初始化
  - 引用常量不会触发此类的初始化（常量在链接链接就存入类的常量池中了）

下面5种情况可以分别单独打开，练习一下

```java
public class Test06 {
    static {
        System.out.println("Main类被加载");
    }

    public static void main(String[] args) {
        //1.主动引用
//        Son son = new Son();

        //2.反射也会产生主动引用
//        try {
//            Class.forName("kuangJava.reflects.Son");
//        } catch (ClassNotFoundException e) {
//            e.printStackTrace();
//        }
        //不会产生类的引用的方法
        //3.
//        System.out.println(Son.b);
        //4.
//        Son[] sons = new Son[2];
        //5.
        System.out.println(Son.M);

    }
}

class Father {
    static int b = 2;
    static {
        System.out.println("父类静态代码块");
    }
}

class Son extends Father {
    static {
        System.out.println("子类静态代码块");
        m = 300;
    }
    static int m = 100;
    static final int M = 1;

}
```

输出

```
1.
Main类被加载
父类静态代码块
子类静态代码块

2.
Main类被加载
父类静态代码块
子类静态代码块

3.
Main类被加载
父类静态代码块
2

4.
Main类被加载

5.
Main类被加载
1
```

# 11.类加载器

类加载器的作用

- 类加载的作用：将class文件字节码内容加载到内存中，并将这些静态数据准换成方法区的运行时数据结构，然后再堆中生成一个代表这个类的java.lang.Class对象，作为方法区中类数据的访问入口
- 类缓存：标准的JavaSE类加载器可以按照要求查找类，但一旦某个类被加载到类加载器中，它将维持加载（缓存）一段时间。不过JVM垃圾回收机制可以回收这个Class对象

![image-20210405220003735](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20210405220003735.png)

类加载器的作用

类加载器作用是用来把类（class）装载进内存的。JVM规范定义了如下类型的类的加载器

![image-20210405220216786](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20210405220216786.png)

```java
public class Test07 {
    public static void main(String[] args) {
        //获取系统类加载器
        ClassLoader classLoader = ClassLoader.getSystemClassLoader();
        System.out.println(classLoader);
        //获取系统类加载器的父类加载器--》扩展类加载器
        ClassLoader extendClassLoader = classLoader.getParent();
        System.out.println(extendClassLoader);
        //获取扩展类加载器的父类加载器--》根加载器（C++）
        ClassLoader rootClassLoader = extendClassLoader.getParent();
        System.out.println(rootClassLoader);


        try {
            //获取当前类是哪个加载器加载的
            ClassLoader classLoader1 = Class.forName("kuangJava.reflects.Test07").getClassLoader();
            System.out.println(classLoader1);
            //获取JDK内置的类是哪个加载器加载的
            ClassLoader classLoader2 = Class.forName("java.lang.Object").getClassLoader();
            System.out.println(classLoader2);
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }

        //如何获取系统类加载器的加载路径
        //System.out.println(System.getProperty("java.class.path"));
        /**
         * D:\software\Java\jdk1.8.0_241\jre\lib\charsets.jar
         * D:\software\Java\jdk1.8.0_241\jre\lib\deploy.jar
         * D:\software\Java\jdk1.8.0_241\jre\lib\ext\access-bridge-64.jar
         * D:\software\Java\jdk1.8.0_241\jre\lib\ext\cldrdata.jar
         * D:\software\Java\jdk1.8.0_241\jre\lib\ext\dnsns.jar
         * D:\software\Java\jdk1.8.0_241\jre\lib\ext\jaccess.jar
         * D:\software\Java\jdk1.8.0_241\jre\lib\ext\jfxrt.jar
         * D:\software\Java\jdk1.8.0_241\jre\lib\ext\localedata.jar
         * D:\software\Java\jdk1.8.0_241\jre\lib\ext\nashorn.jar
         * D:\software\Java\jdk1.8.0_241\jre\lib\ext\sunec.jar
         * D:\software\Java\jdk1.8.0_241\jre\lib\ext\sunjce_provider.jar
         * D:\software\Java\jdk1.8.0_241\jre\lib\ext\sunmscapi.jar
         * D:\software\Java\jdk1.8.0_241\jre\lib\ext\sunpkcs11.jar
         * D:\software\Java\jdk1.8.0_241\jre\lib\ext\zipfs.jar
         * D:\software\Java\jdk1.8.0_241\jre\lib\javaws.jar
         * D:\software\Java\jdk1.8.0_241\jre\lib\jce.jar
         * D:\software\Java\jdk1.8.0_241\jre\lib\jfr.jar
         * D:\software\Java\jdk1.8.0_241\jre\lib\jfxswt.jar
         * D:\software\Java\jdk1.8.0_241\jre\lib\jsse.jar
         * D:\software\Java\jdk1.8.0_241\jre\lib\management-agent.jar
         * D:\software\Java\jdk1.8.0_241\jre\lib\plugin.jar
         * D:\software\Java\jdk1.8.0_241\jre\lib\resources.jar
         * D:\software\Java\jdk1.8.0_241\jre\lib\rt.jar
         * D:\IdeaProjects\mystudy\target\classes
         * D:\mavenRepository\org\apache\httpcomponents\httpclient\4.5.12\httpclient-4.5.12.jar
         * D:\mavenRepository\org\apache\httpcomponents\httpcore\4.4.13\httpcore-4.4.13.jar
         * D:\mavenRepository\commons-logging\commons-logging\1.2\commons-logging-1.2.jar
         * D:\mavenRepository\commons-codec\commons-codec\1.11\commons-codec-1.11.jar
         * D:\mavenRepository\org\apache\poi\poi\4.1.2\poi-4.1.2.jar
         * D:\mavenRepository\org\apache\commons\commons-collections4\4.4\commons-collections4-4.4.jar
         * D:\mavenRepository\org\apache\commons\commons-math3\3.6.1\commons-math3-3.6.1.jar
         * D:\mavenRepository\com\zaxxer\SparseBitSet\1.2\SparseBitSet-1.2.jar
         * D:\mavenRepository\org\apache\poi\poi-ooxml\4.1.2\poi-ooxml-4.1.2.jar
         * D:\mavenRepository\org\apache\poi\poi-ooxml-schemas\4.1.2\poi-ooxml-schemas-4.1.2.jar
         * D:\mavenRepository\org\apache\xmlbeans\xmlbeans\3.1.0\xmlbeans-3.1.0.jar
         * D:\mavenRepository\org\apache\commons\commons-compress\1.19\commons-compress-1.19.jar
         * D:\mavenRepository\com\github\virtuald\curvesapi\1.06\curvesapi-1.06.jar
         * D:\mavenRepository\com\alibaba\fastjson\1.2.73\fastjson-1.2.73.jar
         * D:\mavenRepository\commons-io\commons-io\2.8.0\commons-io-2.8.0.jar
         * D:\software\ideaIU-2019.3.3.win\lib\idea_rt.jar
         */
    }
}
```

输出

```
sun.misc.Launcher$AppClassLoader@18b4aac2
sun.misc.Launcher$ExtClassLoader@355da254
null
sun.misc.Launcher$AppClassLoader@18b4aac2
null
```



# 12.获取类的运行时结构

获取运行时类的完整结构

通过反射获取运行时类的完整结构

Field  Method  Construtor  Superclass  Interface  Annotation

- 实现的全部接口
- 所继承的父类
- 全部的构造器
- 全部的方法
- 全部的Field
- 注解

```java
public class Test08 {
    public static void main(String[] args) {
        try {
            Class<?> c1 = Class.forName("kuangJava.reflects.User");
            //获取类的名字
            System.out.println(c1.getName());
            System.out.println(c1.getSimpleName());
            System.out.println("========");
            //获取类的属性
            //--只能找到public属性
            Field[] fields = c1.getFields();
            //--找到全部的属性
            fields = c1.getDeclaredFields();
            for (Field field : fields) {
                System.out.println(field);
            }
            //--获得指定属性的值
            Field nameField = c1.getDeclaredField("name");
            System.out.println(nameField);
            //获得类的方法
            //--获取本类及其父类的全部public方法
            Method[] methods = c1.getMethods();
            for (Method method : methods) {
                System.out.println("===getMethods==="+method);
            }
            methods = c1.getDeclaredMethods();
            for (Method method : methods) {
                System.out.println("===getDeclaredMethods==="+method);
            }
            //--获得指定的方法
            Method getName = c1.getDeclaredMethod("getName", null);
            Method setName = c1.getDeclaredMethod("setName", String.class);
            System.out.println(getName);
            System.out.println(setName);
            //获取指定的构造器
            Constructor<?>[] constructors = c1.getConstructors();
            for (Constructor<?> constructor : constructors) {
                System.out.println("===getConstructors==="+constructor);
            }
            constructors = c1.getDeclaredConstructors();
            for (Constructor<?> constructor : constructors) {
                System.out.println("===getDeclaredConstructors==="+constructor);
            }
            //获取指定构造器
            Constructor<?> declaredConstructor = c1.getDeclaredConstructor(int.class, String.class);
            System.out.println("指定构造器："+declaredConstructor);
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (NoSuchFieldException e) {
            e.printStackTrace();
        } catch (NoSuchMethodException e) {
            e.printStackTrace();
        }
    }
}

class User {
    private int id;
    private String name;

    public User() {
    }

    public User(int id, String name) {
        this.id = id;
        this.name = name;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    private void test() {

    }
}
```

输出

```
kuangJava.reflects.User
User
========
private int kuangJava.reflects.User.id
private java.lang.String kuangJava.reflects.User.name
private java.lang.String kuangJava.reflects.User.name
===getMethods===public java.lang.String kuangJava.reflects.User.getName()
===getMethods===public int kuangJava.reflects.User.getId()
===getMethods===public void kuangJava.reflects.User.setName(java.lang.String)
===getMethods===public void kuangJava.reflects.User.setId(int)
===getMethods===public final void java.lang.Object.wait() throws java.lang.InterruptedException
===getMethods===public final void java.lang.Object.wait(long,int) throws java.lang.InterruptedException
===getMethods===public final native void java.lang.Object.wait(long) throws java.lang.InterruptedException
===getMethods===public boolean java.lang.Object.equals(java.lang.Object)
===getMethods===public java.lang.String java.lang.Object.toString()
===getMethods===public native int java.lang.Object.hashCode()
===getMethods===public final native java.lang.Class java.lang.Object.getClass()
===getMethods===public final native void java.lang.Object.notify()
===getMethods===public final native void java.lang.Object.notifyAll()
===getDeclaredMethods===public java.lang.String kuangJava.reflects.User.getName()
===getDeclaredMethods===public int kuangJava.reflects.User.getId()
===getDeclaredMethods===public void kuangJava.reflects.User.setName(java.lang.String)
===getDeclaredMethods===private void kuangJava.reflects.User.test()
===getDeclaredMethods===public void kuangJava.reflects.User.setId(int)
public java.lang.String kuangJava.reflects.User.getName()
public void kuangJava.reflects.User.setName(java.lang.String)
===getConstructors===public kuangJava.reflects.User()
===getConstructors===public kuangJava.reflects.User(int,java.lang.String)
===getDeclaredConstructors===public kuangJava.reflects.User()
===getDeclaredConstructors===public kuangJava.reflects.User(int,java.lang.String)
指定构造器：public kuangJava.reflects.User(int,java.lang.String)
```



# 13.动态创建对象执行方法

有了Class对象，能做什么？

- **创建类的对象：调用Class对象的newInstance()方法**
  - **类必须有一个无参数的构造器**
  - **类的构造器的访问权限必须足够**

思考：**难道没有无参的构造器就不能创建对象了吗？只要在操作的时候明确的调用类中的构造器，并将参数传递进去之后，才可以实例化操作。**

步骤如下：

1. 通过Class类的getDeclaredConstructor(Class... parameterTypes)取得本类的指定形参类型的构造器
2. 向构造器的形参中传递一个对象数组进去，里面包含了构造器中所需的各个参数
3. 通过Constructor实例化对象



**调用指定的方法**

通过反射，调用类中的方法，通过Method类实现

1. 通过Class类的getMethod(String name, Class ...parameterTypes)方法取得一个Method对象，并设置此方法操作时所需要的的参数类型。
2. 之后使用Object invoke(Object obj, Object... args)进行调用，并向方法中传递要设置的obj对象的参数信息

![image-20210408235455244](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20210408235455244.png)

**Object invoke(Object obj, Object... args)**

- Object对应原方法的返回值，若原方法无返回值，此时返回null
- 若原方法为静态方法，此时形参Object obj可为null
- 若原方法形参列表为空，则Object... args为null
- 若原方法声明为private,则需要在调用此invoke()方法前，显式调用方法对象的setAccessible(true)方法，将可访问private的方法



**setAccessible**

- Method和Field、Constructor对象都有setAccessible()方法
- setAccessible作用是启动和禁用方案安全检查的开关
- 参数值为true则指示反射的对象在使用时应该取消java语言访问检查
  - 提高反射的效率，如果代码中必须用反射，而该句代码需要频繁的被调用，那么请设置为true
  - 使得原本无法访问的私有成员也可以访问
- 参数值为false则指示反射的对象应该实施java语言访问检查

```java
public class Test09 {
    public static void main(String[] args) {
        try {
            //获取Class对象
            Class c1 = Class.forName("kuangJava.reflects.User");
            //1.构造一个对象（本质是调用了类的无参构造器）
//            User user = (User) c1.newInstance();
//            System.out.println(user);
            //2.通过构造器创建对象
//            Constructor constructor = c1.getDeclaredConstructor(int.class, String.class);
//            User user2 = (User) constructor.newInstance(18, "zyy");
//            System.out.println(user2);
            //3.通过反射调用普通方法
            User user3 = (User) c1.newInstance();
            //通过反射获取一个方法
            Method setName = c1.getDeclaredMethod("setName", String.class);
            //invoke :激活的意思    (对象, "方法的值")
            setName.invoke(user3, "zyy");
            System.out.println(user3.getName());
            //反射静态方法
            Method print = c1.getDeclaredMethod("print", String.class);
            print.invoke(null, "hello");
            //4.通过反射操作属性
            User user4 = (User) c1.newInstance();
            Field name = c1.getDeclaredField("name");
            //不能直接操作私有属性，我们需要关闭程序的安全监测，属性或者方法的setAccessible(true)
            name.setAccessible(true);
            name.set(user4, "zyy2");
            System.out.println(user4.getName());
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (InstantiationException e) {
            e.printStackTrace();
        } catch (NoSuchMethodException e) {
            e.printStackTrace();
        } catch (InvocationTargetException e) {
            e.printStackTrace();
        } catch (NoSuchFieldException e) {
            e.printStackTrace();
        }
    }
}

class User {
    private int id;
    private String name;

    public User() {
    }

    public User(int id, String name) {
        this.id = id;
        this.name = name;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    private void test() {

    }

    public static void print (String msg) {
        System.out.println("print:"+msg);
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                '}';
    }
}
```



# 14.性能对比分析

```java
public class Test10 {
    /**
     * 普通方法
     */
    public static void normalWay() {
        User user = new User();
        long startTime = System.currentTimeMillis();
        for (int i = 0; i < 1000000000; i++) {
            user.getName();
        }
        long endTime = System.currentTimeMillis();
        System.out.println("普通方法执行10亿次用时：" + (endTime - startTime) + "ms");
    }

    /**
     * 反射
     */
    public static void reflectWay() {
        try {
            User user = new User();
            Class c1 = user.getClass();
            Method getName = c1.getDeclaredMethod("getName", null);
            long startTime = System.currentTimeMillis();
            for (int i = 0; i < 1000000000; i++) {
                getName.invoke(user, null);
            }
            long endTime = System.currentTimeMillis();
            System.out.println("反射方法执行10亿次用时：" + (endTime - startTime) + "ms");
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (NoSuchMethodException e) {
            e.printStackTrace();
        } catch (InvocationTargetException e) {
            e.printStackTrace();
        }

    }

    /**
     * 反射，关闭安全校验
     */
    public static void reflectWay2() {
        try {
            User user = new User();
            Class c1 = user.getClass();
            Method getName = c1.getDeclaredMethod("getName", null);
            getName.setAccessible(true);
            long startTime = System.currentTimeMillis();
            for (int i = 0; i < 1000000000; i++) {
                getName.invoke(user, null);
            }
            long endTime = System.currentTimeMillis();
            System.out.println("反射方法(关闭校测)执行10亿次用时：" + (endTime - startTime) + "ms");
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (NoSuchMethodException e) {
            e.printStackTrace();
        } catch (InvocationTargetException e) {
            e.printStackTrace();
        }
    }

    public static void main(String[] args) {
        normalWay();
        reflectWay();
        reflectWay2();
    }
}
```

输出

```
普通方法执行10亿次用时：5ms
反射方法执行10亿次用时：3096ms
反射方法(关闭校测)执行10亿次用时：1300ms
```




# 15.获取注解信息

反射操作注解

- getAnnotations
- getAnnotation

![image-20210409224424042](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20210409224424042.png)



```java
public class Test12 {
    public static void main(String[] args) {
        //通过反射获取注解
        Class c1 = Student2.class;
        Annotation[] annotations = c1.getAnnotations();
        for (Annotation annotation : annotations) {
            System.out.println(annotation);
            //获取注解的value的值
            if (annotation instanceof TableStudent) {
                TableStudent tableStudent = (TableStudent)annotation;
                System.out.println(tableStudent.value());
            }
        }
        //获得类指定的注解
        try {
            Field id = c1.getDeclaredField("name");
            FieldStudent annotation = id.getAnnotation(FieldStudent.class);
            System.out.println(annotation.columnName());
            System.out.println(annotation.length());
            System.out.println(annotation.type());
        } catch (NoSuchFieldException e) {
            e.printStackTrace();
        }

        /*Field[] declaredFields = c1.getDeclaredFields();
        for (Field declaredField : declaredFields) {
            FieldStudent annotation = declaredField.getAnnotation(FieldStudent.class);
            System.out.println(annotation.columnName());
            System.out.println(annotation.length());
            System.out.println(annotation.type());
        }*/
    }

}

/**
 * 属性的注解
 */
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
@interface FieldStudent {
    String columnName();
    String type();
    int length();
}

/**
 * 类名的注解
 */
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@interface TableStudent {
    String value();
}


@TableStudent("t_student")
class Student2 {
    @FieldStudent(columnName = "id", type = "int", length = 30)
    private int id;
    @FieldStudent(columnName = "name", type = "varchar", length = 20)
    private String name;
    @FieldStudent(columnName = "age", type = "int", length = 3)
    private int age;

    public Student2() {
    }

    public Student2(int id, String name, int age) {
        this.id = id;
        this.name = name;
        this.age = age;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
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

    @Override
    public String toString() {
        return "Student2{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

输出

```
@kuangJava.reflects.TableStudent(value=t_student)
t_student
name
20
varchar
```


