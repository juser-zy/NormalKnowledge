#### 关键接口说明
```java
// Driver 驱动接口 -> Interface
// DriverManager 驱动管理器（注册驱动，获取数据库连接）-> Class
// Connection 数据库连接对象 -> Interface
// Statement 执行对象 -> Interface
// ResultSet 结果集对象 -> Interface
```

```java
public static void main(String[] args) throws ClassNotFoundException, SQLException {  
    // 1. 加载和注册驱动  
    Class<?> cls = Class.forName("com.mysql.cj.jdbc.Driver");  
    System.out.println(cls);  
  
    // 2. 创建数据库连接  
    // 协议:具体数据库://地址:端口号/数据库  
    String url = "jdbc:mysql://localhost:3306/jdbcstudy";  
    String username = "root";  
    String password = "123456";  
    Connection connection = DriverManager.getConnection(url, username, password);  
    System.out.println(connection);  
  
    // 3. 编写sql  
    String sql = "select * from users";  
  
    // 4. 创建一个执行SQL的对象  
    Statement statement = connection.createStatement();  
    System.out.println(statement);  
  
    // 5. 执行SQL，并且获取执行结果  
    ResultSet resultSet = statement.executeQuery(sql);  
    System.out.println(resultSet);  
  
    connection.close();  
}
```
