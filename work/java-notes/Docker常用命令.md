# Docker常用命令

## 帮助命令

## 镜像命令

## 容器命令

说明：我们有了镜像才可以创建容器

```shell
docker pull centos
```

**新建容器并启动**

```shell
docker run[可选参数] image
# 参数说明
--name="Name"
-d
-it
-p 
-P

```

**列出所有的容器**

**退出容器**

**删除容器**

**启动和停止容器的操作**

## 常用的其他命令

**后台启动容器**

```shell

```

**查看日志**

  

## 作业

> 作业：docker安装tomcat

```shell
# 官方的使用

# 下载再启动
# 启动运行
# 测试访问没有问题
# 进入容器
```

> 作业：部署es+kibana

```shell
# es 暴露的端口很多!
# es 十分的耗内存
# es 的数据一般放置到安全目录！挂载
# 启动 elastic search
# 启动了 linux卡了 docker status 查询cpu状态
# es 是十分耗内存了
# 查看docker status
# 测试一下es是否成功了
# 赶紧关闭，增加内存限制
```

