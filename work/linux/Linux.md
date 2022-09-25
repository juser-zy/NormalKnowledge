[(4条消息) 狂神说Linux学习笔记_As_theWind的博客-CSDN博客_狂神说linux笔记](https://blog.csdn.net/qq_40492693/article/details/124413806)


### 一、系统概述
1. 关机命令

```linux
sync 			  # 将数据有内存同步到硬盘中
shutdown		  # 关机指令，你可以man shutdown 来看一下帮助文档。例如你可以运行如下命令关机：
shutdown -h -10	  # 这个命令，计算机将在10秒后关机
shutdown -h  now  # 立马关机
shutdown -h 20:25 # 系统会在今天的20点25关机
shutdown -h +10   # 十分钟后关机
shutdown -r now   # 系统立马重启
shutdown -r +10   # 十分钟后重启
reboot			  # 就是重启，等同于 shutdown -r now
halt			  # 关闭系统，等同于 shutdown -h now 和 poweroff
```
2. 系统目录结构

```linux
ls \
```
![](photo/Pasted%20image%2020220923094956.png)


### 二、常用的基本命令

用man [命令]来查看各个命令的使用文档，如：man cp。

#### 1.目录管理

cd：切换目录
./：当前目录
cd ..：返回上一级目录

>**ls命令**

在Linux中 `ls` 是最经常使用的！  
`-a`参数：all，查看全部的文件，包括隐藏文件  
`-l`参数：列出所有的文件，包含文件的属性和权限，没有隐藏文件  
所有的Linux可以组合使用！（d表示目录，-表示文件）
![](photo/Pasted%20image%2020220923104918.png)

>**cd命令**

cd 目录名 （绝对路径都是以/开头）
~表示用户的home目录

>**pwd**

显示当前用户所在的目录

>**mkdir**

创建一个目录：mkdir [-p] dirName    
-p：确保目录名存在，不存在的就创建一个，通过此选项，可以实现多层目录同时创建

>**rmdir**

删除目录
rmdir仅能删除空的目录，如果下面存在文件，需要先删除文件，递归删除多个目录 `-p` 参数即可

>**cp**

复制文件或者目录
cp -r 原来的地方 新的地方
-r ：如果复制的是目录需要使用此选项，此时将复制该目录下所有的子目录和文件
cp -r hello/ ./world 将hello目录和目录下的所有文件复制到world目录下
cp -r hello/* ./world 将hello目录下所有文件复制到world目录下

>**rm**

移除文件或者目录
`-f`忽略不存在的文件，不会出现警告，强制删除  
`-r`递归删除目录  
`-i`互动，删除询问是否删除

>**mv**

移动文件或者目录（重命名文件）
mv 源文件 目标  
`-f` 强制移动  
`-u` 只替换已经更新过的文件
举例：
mv hello.txt hi.txt 将hello.txt改名为hi.txt
mv hi.txt hello/ 将文件hi.txt移动到hello目录中
mv hello/ world/ 如果hello目录不存在，将hello目录改名为world
mv hello/ world/ 如果hello目录存在，将hello移动到world目录中

>**tar**

作用：对文件进行打包、解包、压缩、解压
语法：tar [-zxcvf] fileName [files]

包文件后缀名 `.tar`表示只是完成了打包，并没有压缩
包文件后最为`.tar.gz`表示打包的同时还进行了压缩

-z：z代表的是gzip，通过gzip命令处理文件，gzip可以对文件压缩或者解压
-c：c代表的是create，即创建新的包文件
-x：x代表的是extract，实现从包文件中还原文件
-v：v代表的是verbose，显示命令的执行过程
-f：f代表的是file，用于指定包文件的名称

`tar -cvf 打包`  
`tar -zcvf 打包压缩`  
`tar -xvf 解包`  
`tar -zxvf 解压`

举例：

打包
tar -cvf hello.tar ./* 将当前目录下所有文件打包，打包后的文件名为hello.tar
tar -zcvf hello.tar.gz ./* 将当前目录下所有文件打包并压缩，打包后的文件名为hello.tar.gz

解包
tar -xvf hello.tar 将hello.tar文件进行解包，并将解包后的文件放在当前目录
tar -zxvf hello.tar.gz 将hello.tar.gz文件进行解压，并将解压后的文件放在当前目录
tar -zxvf hello.tar.gz -C /usr/local 将hello.tar.gz文件进行解压，并将解压后的文件放在/usr/local目录
常用：
`tar -zxvf xxxx.tar.gz -C /usr/local`

>**find & grep**

find：
作用：在指定目录下查找文件
语法：find dirName -option fileName

举例：
`find . -name "*.java"` 在当前目录及其子目录下查找.java结尾文件
`find /itcast -name "*.java"` 在/itcast目录及其子目录下查找.java结尾的文件

grep：
作用：从指定文件中查找指定的文本内容
语法：grep word fileName

举例：

`grep Hello HelloWorld.java` 查找HelloWorld.java文件中出现的Hello字符串的位置
`grep hello *.java` 查找当前目录中所有.java结尾的文件中包含hello字符串的位置

#### 2.基本属性

>**文件基本属性**

![](photo/Pasted%20image%2020220923131902.png)
boot文件的第一个属性用“d”表示，“d”在Linux中代表该文件是一个目录文件。
在Linux中第一个字符代表这个文件是目录、文件链接文件等等：
- 当为[ d ]则是目录
- 当为[ - ]则是文件
- 若是[ l ]则表示为链接文档（link file）
- 若是[ b ]则表示为装置文件里面的可供存储的接口设备（可随机存取装置）
- 若是[ c ]则表示为装置文件里面的串行端口设备，例如键盘、鼠标（一次性读取装置）

接下来的字符中，以三个为一组，且均为[ rwx ]的三个参数的组合。其中，[ r ]代表可读（read）、[ w ]代表可写( write )、[ x ]代表可执行(execute)。要注意的是，这三个权限的位置不会改变，如果没有权限，就会出现[ - ]而已。每个文件的属性由左边第一部分的10个字符来确定。

>**修改文件属性**

- **chgrp：更改文件属组**
`chgrp [-R] 属性名 文件名`
 -R： 递归更改文件属组，就是在更改某个目录文件的属组时，如果加上-R的参数，那么该目录下的所有文件的属组都会更改。
 
- **chown：更改文件属组，也可以同时更改文件属组**
`chown [-R] 属性名	文件名
`chown [-R] 属性名：属性名 文件名`

- **chmod：更改文件9个人属性**
`chmod [-R] xyz 文件或目录`

`chmod 770 filename`
Linux文件属性有两种设置方法，一种是数字（常用的是数字），一种是符号。
Linux文件的基本权限就有九个，分别是owner/group/others三种身份各有自己的read/write/execute权限。
文件的权限字符：[ -rwx rwx rwx ]，这九个权限三个三个一组，我们使用数字来代表各个权限

>文件内容查看

- cat 由第一行开始显示文件内容，用来读文章，获取读取配置文件都使用cat命令 cat -n 由1开始对所有输出的行号编号
- tac从最后一行开始显示，可以看出tac是cat的倒着写！
- nl显示的时候，顺道输出行号！看代码的时候希望显示行号！（常用）
- `more 一页一页的显示文件内容（空格代表翻页，enter代表向下看一行，:f行号）`
	- 回车键 向下滚动一行
	- 空格键 向下滚动一屏
	- b 返回上一屏
	- q或者Ctrl + c 退出more
- `less和more类似，但是比more更好的是，他可以往前翻页！（空格下翻页，上下键代表翻动页面！退出q命令，查找字符串/要查询的字符，向上查询使用?要查询的字符串，n继续搜寻下一个，N向上寻找！）`
- head只看头几行（通过-n参数来控制显示几行）
- tail 行数 只看尾巴几行


>**硬链接和软链接**
- **硬链接：** A----B；假设B是A的硬链接，那么他们两个指向同一个文件！允许一个文件拥有多个路径，用户可以通过这种机制来建立硬链接到一些重要文件上，防止误删！  
- **软链接：** 类似windows下的快捷方式，删除的源文件，快捷方式也访问不了！
- `ln A B` 硬链接
- `ln -s A B`软链接
```bash
[root@Aug home]# touch f1						# 创建一个f1文件
[root@Aug home]# ls
aug_  f1  Yang
[root@Aug home]# ln f2							# 创建一个硬链接 f2
ln: failed to access "f2": 没有那个文件或目录
[root@Aug home]# ln f1 f2
[root@Aug home]# ls
aug_  f1  f2  Yang
[root@Aug home]# ln -s f1 f3					# 创建一个软链接(符号链接) f3
[root@Aug home]# ls
aug_  f1  f2  f3  Yang
[root@Aug home]# echo "i love dddd " >> f1 		# 向f1文件中写入一些字符串
[root@Aug home]# ll
总用量 12
drwx------. 15 aug_ aug_ 4096 4月  25 20:43 aug_
-rw-r--r--.  2 root root   13 4月  26 14:13 f1
-rw-r--r--.  2 root root   13 4月  26 14:13 f2
lrwxrwxrwx.  1 root root    2 4月  26 14:12 f3 -> f1
drwx-wx-wx.  2 root root   24 4月  26 11:19 Yang
[root@Aug home]# cat f1					# 查看f1
i love dddd 
[root@Aug home]# cat f2					# 查看f2
i love dddd 
[root@Aug home]# cat f3					# 查看f3
i love dddd 

[root@Aug home]#  rm -rf f1
[root@Aug home]# ll
总用量 8
drwx------. 15 aug_ aug_ 4096 4月  25 20:43 aug_
-rw-r--r--.  1 root root   13 4月  26 14:13 f2
lrwxrwxrwx.  1 root root    2 4月  26 14:12 f3 -> f1
drwx-wx-wx.  2 root root   24 4月  26 11:19 Yang
[root@Aug home]# cat f2		# f2 硬链接还在
i love dddd 
[root@Aug home]# cat f3		# f3（软链接、符号链接）快捷方式失效！
cat: f3: 没有那个文件或目录


```