#### File

>创建
---

- 方式1 new File()
```java
public void create01(){  
    String filePath = "E:/JavaWorkstation/Back_end/IO/news1.txt";  
    File file = new File(filePath);  
    try {  
        file.createNewFile();  
        System.out.println("创建成功");  
    } catch (IOException e) {  
        throw new RuntimeException(e);  
    }  
}
```

- 方式2 new File(File parent,String child)//根据父目录文件+子路径构建
```java
public void create02(){  
    File parentFile = new File("E:/JavaWorkstation/Back_end/IO/");  
    String fileName = "new2.txt";  
    File file = new File(parentFile, fileName);  
    try {  
        file.createNewFile();  
        System.out.println("文件创建成功");  
    } catch (IOException e) {  
        throw new RuntimeException(e);  
    }  
}
```

- 方式3 new File(String parent,String child)//根据父目录+子路径构建
```java
public void create03(){  
    String parentPath = "E:/JavaWorkstation/Back_end/IO/";  
    String fileName = "new3.txt";  
    File file = new File(parentPath, fileName);  
    try {  
        file.createNewFile();  
        System.out.println("创建成功");  
    } catch (IOException e) {  
        throw new RuntimeException(e);  
    }  
}
```

>目录
---
```java
public void m1(){  
    String filePath = "E:/JavaWorkstation/Back_end/IO/news1.txt";  
    File file = new File(filePath);  
    if(file.exists()){  
        if (file.delete()) {  
            System.out.println("删除成功");;  
        }else{  
            System.out.println("删除失败");  
        }  
    }else{  
        System.out.println("文件不存在");  
    }  
}  
public void m2(){  
    String filePath = "E:/JavaWorkstation/Back_end/IO/demo/a";  
    File file = new File(filePath);  
    if(file.exists()){  
        System.out.println("目录已经存在");  
    }else {  
        if(file.mkdirs()){  
            System.out.println("目录创建成功");  
        }else {  
            System.out.println("目录创建失败");  
        }  
    }  
}
```
#### Stream字节流
- FileInputStream
>逐个按字节读取，或者利用`byte[] buf = new byte[8]`读取
```java
public void readFile01() {  
    String filePath = "E:/JavaWorkstation/Back_end/IO/hello.txt";  
    int readData = 0;  
    FileInputStream fileInputStream = null;  
    try {  
        //创建fileInputStream对象，用于读取文件  
        fileInputStream = new FileInputStream(filePath);  
        //从该输入流读取一个字节的数据，返回-1，表示读取完毕  
        while ((readData = fileInputStream.read()) != -1) {  
            System.out.print((char) readData);  
        }  
    } catch (IOException e) {  
        e.printStackTrace();  
    } finally {  
        try {  
            fileInputStream.close();  
        } catch (IOException e) {  
            e.printStackTrace();  
        }  
    }  
  
}  

public void readFile02() {  
    String filePath = "E:/JavaWorkstation/Back_end/IO/hello.txt";  
    FileInputStream fileInputStream = null;  
    byte[] buf = new byte[8];  
    int readLen  = 0;  
    try {  
        fileInputStream = new FileInputStream(filePath);  
        while ((readLen = fileInputStream.read(buf)) != -1) {  
            System.out.print(new String(buf,0,readLen));  
        }  
    } catch (IOException e) {  
        e.printStackTrace();  
    } finally {  
        try {  
            fileInputStream.close();  
        } catch (IOException e) {  
            throw new RuntimeException(e);  
        }  
    }  
  
}
```

- FileOutputStream
```java
public void writeFile(){  
    String filePath = "E:/JavaWorkstation/Back_end/IO/a.txt";  
    FileOutputStream fileOutputStream = null;  
    try {  
        //创建方式是覆盖  
        //fileOutputStream = new FileOutputStream(filePath);  
  
        //创建方式是追加  
        fileOutputStream = new FileOutputStream(filePath,true);  
  
        //写入一个字节  
        //fileOutputStream.write('H');  
  
        //写入字符串  
        String str = "hello,world";  
        //fileOutputStream.write(str.getBytes());  
  
        fileOutputStream.write(str.getBytes(),4,3);  
  
  
  
    } catch (IOException e) {  
        e.printStackTrace();  
    }finally {  
        try {  
            fileOutputStream.close();  
        } catch (IOException e) {  
            throw new RuntimeException(e);  
        }  
    }  
  
}
```

- 针对图片或者视频可以进行拷贝
>FileInputStream和FileOutputStream实现
```java
public static void main(String[] args) {  
    String fileInputPath = "E:/JavaWorkstation/Back_end/IO/w.jpg";  
    String fileOutputPath = "E:/JavaWorkstation/Back_end/IO/w_c.jpg";  
    FileInputStream fileInputStream = null;  
    FileOutputStream fileOutputStream = null;  
  
    try {  
  
        fileInputStream = new FileInputStream(fileInputPath);  
        fileOutputStream = new FileOutputStream(fileOutputPath);  
        byte[] buf = new byte[1024];  
        int readLen = 0;  
        while ((readLen = fileInputStream.read(buf)) != -1) {  
            fileOutputStream.write(buf, 0, readLen);//如果只使用buf，最后还剩点，打不开  
        }  
  
    } catch (IOException e) {  
        throw new RuntimeException(e);  
    } finally {  
        if (fileInputStream != null) {  
            try {  
                fileInputStream.close();  
            } catch (IOException e) {  
                throw new RuntimeException(e);  
            }  
        }  
        if (fileOutputStream != null) {  
            try {  
                fileOutputStream.close();  
            } catch (IOException e) {  
                throw new RuntimeException(e);  
            }  
        }  
    }  
}
```

>BufferedInputStream和BufferedOutputStream实现
```java
public static void main(String[] args) throws IOException {  
    String srcFilePath = "E:/JavaWorkstation/Back_end/IO/w.jpg";  
    String desFilePath = "E:/JavaWorkstation/Back_end/IO/w_c2.jpg";  
    byte[] buf = new byte[1024];  
    int dataLen = 0;  
    BufferedInputStream bufferedInputStream = new BufferedInputStream(new FileInputStream(srcFilePath));  
    BufferedOutputStream bufferedOutputStream = new BufferedOutputStream(new FileOutputStream(desFilePath));  
    while ((dataLen = bufferedInputStream.read(buf)) != -1) {  
        bufferedOutputStream.write(buf, 0, dataLen);  
    }  
    bufferedInputStream.close();  
    bufferedOutputStream.close();  
}
```
#### Reader & Writer字符流
- FileReader
```java
public void m1() {  
    String filePath = "E:/JavaWorkstation/Back_end/IO/a.txt";  
    FileReader fileReader = null;  
    char[] cbuf = new char[8];  
    int readLen = 0;  
    try {  
        fileReader = new FileReader(filePath);  
        while ((readLen = fileReader.read(cbuf)) != -1) {  
            System.out.print(new String(cbuf, 0, readLen));  
        }  
    } catch (IOException e) {  
        e.printStackTrace();  
    } finally {  
        try {  
            fileReader.close();  
        } catch (IOException e) {  
            throw new RuntimeException(e);  
        }  
    }  
}
```

- FileWriter
```java
public void m1() {  
    String filePath = "E:/JavaWorkstation/Back_end/IO/note.txt";  
    FileWriter fileWriter = null;  
    try {  
        fileWriter = new FileWriter(filePath);  
        // 1. 写入单个字符  
        //fileWriter.write('H');  
  
        // 2. 写入数组  
        //char[] chars = {'a','b','c'};  
        //fileWriter.write(chars);  
        // 3. 写入指定数组的指定部分  
        //fileWriter.write("小金肩膀疼".toCharArray(), 0, 3);  
  
        // 4. 写入字符串  
        //String str = "风雨之后，彩虹";  
        //fileWriter.write(str);  
        // 5. 写入字符串的指定部分  
        String str = "风雨之后，彩虹";  
        fileWriter.write(str,1,4);  
  
    } catch (IOException e) {  
        throw new RuntimeException(e);  
    } finally {  
        if (fileWriter != null) {  
            try {  
                fileWriter.close();  
            } catch (IOException e) {  
                throw new RuntimeException(e);  
            }  
        }  
    }  
}
```

- BufferedReader
```java
public static void main(String[] args) {  
    String filePath = "E:/JavaWorkstation/Back_end/IO/a.txt";  
    BufferedReader bufferedReader = null;  
    try {  
         bufferedReader = new BufferedReader(new FileReader(filePath));  
        String line = "";  
        while((line = bufferedReader.readLine())!= null){  
            System.out.println(line);  
        }  
    } catch (IOException e) {  
        e.printStackTrace();  
    }finally {  
        try {  
            bufferedReader.close();  
        } catch (IOException e) {  
            throw new RuntimeException(e);  
        }  
    }  
}
```

- BufferedWriter
```java
public static void main(String[] args) throws IOException {  
    String filePath = "E:/JavaWorkstation/Back_end/IO/hello.txt";  
    BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(filePath,true));  
  
    bufferedWriter.write("\nhello,小金");  
    bufferedWriter.close();  
}
```

- BufferedCopy
```java
public static void main(String[] args) throws IOException {  
    String fileSrcPath = "E:/JavaWorkstation/Back_end/IO/a.txt";  
    String fileDestPath = "E:/JavaWorkstation/Back_end/IO/a_c.txt";  
    BufferedReader bufferedReader = new BufferedReader(new FileReader(fileSrcPath));  
    BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(fileDestPath));  
    String line = "";  
    while((line = bufferedReader.readLine())!=null){  
        bufferedWriter.write(line);  
        bufferedWriter.newLine();  
    }  
    bufferedWriter.close();  
    bufferedReader.close();  
}
```

#### Properties
```java
public static void main(String[] args) throws IOException {  
    Properties properties = new Properties();  
    properties.load(new FileInputStream("src\\mysql.properties"));  
    properties.list(System.out);  
    System.out.println(properties.getProperty("pwd"));  
    System.out.println(properties.getProperty("user"));  
    properties.setProperty("pwd","123456465456");  
    System.out.println(properties.getProperty("pwd"));  
    System.out.println(properties.getProperty("user"));  
}

public static void main(String[] args) throws IOException {  
    Properties properties = new Properties();  
    properties.setProperty("charset","utf8");  
    properties.setProperty("pwd","123456");  
    properties.setProperty("user","小金");  
    properties.store(new FileOutputStream("src/mysql2.properties"),null);  
}
```