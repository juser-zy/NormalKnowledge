#### 环境变量的配置

1. **新建 MAVEN_HOME & Path 修改**

![](images/Pasted%20image%2020230114121833.png)

![](images/Pasted%20image%2020230114121936.png)

2. **mvn -v确定安装成功**
![](images/Pasted%20image%2020230114122057.png)

#### 配置maven本地仓库

1. **新建repository文件夹**
- 选定一个存放maven下载jar包的路径，创建repository文件夹，可以选择maven解压包同级目录下
![](images/Pasted%20image%2020230114122334.png)

- 配置settings.xml，路径$maven_root_path/conf/settings.xml
![](images/Pasted%20image%2020230114122453.png)

1. **本地仓库配置，为上一步创建的repositroy仓库路径**
```xml
  <!-- localRepository
   | The path to the local repository maven will use to store artifacts.
   |
   | Default: ${user.home}/.m2/repository
  <localRepository>/path/to/local/repo</localRepository>
  -->
  <localRepository>E:\JavaWorkstation\Environment\Maven\repository</localRepository>
```
![](images/Pasted%20image%2020230114122717.png)

2. **远程仓库地址，选择的是阿里云的仓库配置，因为阿里云的maven公共库的配置使用比较广泛。**
```xml
<!-- mirrors
   | This is a list of mirrors to be used in downloading artifacts from remote repositories.
   |
   | It works like this: a POM may declare a repository to use in resolving certain artifacts.
   | However, this repository may have problems with heavy traffic at times, so people have mirrored
   | it to several places.
   |
   | That repository definition will have a unique id, so we can create a mirror reference for that
   | repository, to be used as an alternate download site. The mirror site will be the preferred
   | server for that repository.
   |-->
  <mirrors>
    <!-- mirror
     | Specifies a repository mirror site to use instead of a given repository. The repository that
     | this mirror serves has an ID that matches the mirrorOf element of this mirror. IDs are used
     | for inheritance and direct lookup purposes, and must be unique across the set of mirrors.
     |
    <mirror>
      <id>mirrorId</id>
      <mirrorOf>repositoryId</mirrorOf>
      <name>Human Readable Name for this Mirror.</name>
      <url>http://my.repository.com/repo/path</url>
    </mirror>
     -->
<!-- 阿里云仓库 -->
<mirror>
<id>alimaven</id>
<mirrorOf>central</mirrorOf>
<name>aliyun maven</name>
<url>http://maven.aliyun.com/nexus/content/repositories/central/</url>
</mirror>
    <mirror>
      <id>maven-default-http-blocker</id>
      <mirrorOf>external:http:*</mirrorOf>
      <name>Pseudo repository to mirror external repositories initially using HTTP.</name>
      <url>http://0.0.0.0/</url>
      <blocked>true</blocked>
    </mirror>
  </mirrors>
```
![](images/Pasted%20image%2020230114122842.png)

#### IDEA 配置
![](images/Pasted%20image%2020230114123402.png)
