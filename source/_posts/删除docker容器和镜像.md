---
title: 删除docker容器和镜像
date: 2024-03-26 22:10:34
tags: Docker
categories:
- [技术兴趣,软件应用]
---
# 什么是 Docker?
`Docker`是基于`Go语言`实现的在 2013 年发布的云开源项目，它利用了围绕容器这个现有的计算概念，特别是在`Linux`世界中，这些原始概念被称为`cgroups`和命名空间。`Docker`的技术之所以独特是因为它专注于开发人员和系统操作员的需求，以将应用程序依赖项与基础架构分开。
`Docker`的主要目标是 “Build，Ship and Run Any App,Anywhere”，也就是通过对应用组件的封装、分发、部署、运行等生命周期的管理，使用户的 APP (可以是一个WEB应用或数据库应用等等) 及其运行环境能够做到“一次封装，到处运行”。
一句话概括，`Docker`的出现解决了运行环境和配置环境不一致的情况，从而更方便的做持续集成并有助于应用的整体发布。
<!--more-->
# Docker 三要素: 镜像、容器和仓库
## 镜像
在了解镜像这个概念之前，我们需要先大致了解一下联合文件系统:`UnionFS`，它是`Docker`镜像的基础，联合文件系统是一种分层，轻量级并且高性能的文件系统，它支持对文件系统的修改作为一次提交来一层一层的叠加，同时可以将不同目录挂载到同一个虚拟文件系统下，镜像可以通过分层来进行集成，我们可以基于一个基础的镜像，然后制作出各种各样满足我们需求的应用镜像。
同时，对于一个精简的`OS`，`rootfs`可以很小，有常见的命令就行，同时，底层又是直接使用的操作系统的内核，所以往往`Docker`中一个镜像的体积相对来说可以很小，比如一个完整版的`centos`可能要几个 G，但是`Docker`中的`centos`大概只有 300M.
对于docker镜像，官方的定义如下:
> An image is a read-only template with instructions for creating a Docker container. Often, an image is based on another image, with some additional customization. For example, you may build an image which is based on the ubuntu image, but installs the Apache web server and your application, as well as the configuration details needed to make your application run.‘
> 映像是一个只读模板，带有创建Docker容器的指令。通常，一个映像是基于另一个映像的，还需要进行一些额外的定制。例如，您可以构建一个基于ubuntu映像的映像，但是安装`Apache web`服务器和您的应用程序，以及使您的应用程序运行所需的配置细节。

PS: **<font color="red">一个镜像可以创建多个容器</font>**。
## 容器
容器是用镜像创建的运行实例。
每个容器都可以被启动，开始，停止，删除，同时容器之间相互隔离，保证应用运行期间的安全。
我们可以把容器理解为一个精简版的`linux`操作系统，包括`root`用户权限，进程空间，用户空间和网络空间等等这些，然后加上再它之上运行的应用程序。
比如我们现在基于`mysql`镜像创建了一个容器，那么，这个容器其实并不是只有一个`mysql`程序，而是`mysql`同样也是安装运行在我们容器内的`linux`环境中的。
## 容器和镜像的关系
再说这个问题之前，我们不妨先来看一下下面这段`java`代码:
```java
Person p = new Person();
Person p1 = new Person();
Person p2 = new Person();
```
镜像在这里就是我们的Person，容器就是一个个Person类的实例。一个Person可以创建多个实例，一个镜像也可以创建多个容器。
## 仓库
仓库相对来说就比较容易理解了，仓库(Repository)是集中存放镜像文件的场所。
仓库分为公开仓库和私有仓库，目前的话，全世界最大的仓库是Docker官方的 Docker Hub
由于一些不可抗拒的因素，导致我们如果从 Docker Hub 上下载公开的镜像是非常蛋疼的，这点大家可以参考你用百度网盘官方下载时的感觉。所以，国内我们一般使用阿里云或者网易云的镜像仓库。
镜像 容器 仓库 他们三者之间的关系图如下:
![20240326223417](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240326223417.png)
# Docker 容器的创建、启动
docker 容器的启动有三种方式:
## 交互方式: 基于镜像新建容器并启动
例如我们可以启动一个容器，打印出当前的日历表:
```bash
docker run my/python:v1 cal   ## my/python:v1 为镜像名和标签
```
![20240326223332](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240326223332.png)
我们还可以通过制定参数，启动一个 bash 交互终端:
```bash
docker run -it my/python:v1 /bin/bash
```
![20240326223619](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240326223619.png)
参数`-t`让 Docker 分配一个伪终端并绑定在容器的标准输入上，`-i`让容器的标准输入保持打开。

使用`docker run`命令来启动容器，docker 在后台运行的标准操作包括
1.检查本地是否存在指定的镜像，不存在则从公有仓库下载;
2.使用镜像创建并启动容器;
3.分配一个文件系统，并在只读的镜像层外面挂载一层可读可写层;
4.从宿主主机配置的网桥接口中桥接一个虚拟接口道容器中去;
5.从地址池分配一个ip地址给容器;
6.执行用户指定的应用程序;
7.执行完毕之后容器被终止。
![20240326223751](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240326223751.png)
`my/sinatra:v2`基于`training/sinatra`镜像进行修改后的镜像，`training/sinatra`为公有仓库上的镜像。

## 短暂方式: 直接将一个已经终止的容器启动运行起来
可以使用`docker start`命令，直接将一个已经终止的容器启动运行起来。
```bash
[root@rocketmq-nameserver4 ~]# docker run my/python:v1 /bin/echo hello test
hello test
```
命令执行完，控制台会打印`hello test`，container 就终止了，不过并没有消失，
可以用`docker ps -n 5`看一下最新前 5 个的 container，第一个就是刚刚执行过的 container，可以再次执行一遍：`docker start container_id`
不过这次控制台看不到`hello test`了，只能看到 ID，用`logs`命令才能看得到：`docker logs container_id`。
可以看到两个`hello test`了，因为这个 container 运行了两次。
![20240326224054](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240326224054.png)

## Daemen 方式(守护态运行)
即让软件作为长时间服务运行，这就是 SAAS 啊！
例如我们启动 centos 后台容器，每隔一秒打印当天的日历。
```bash
$ docker run -d centos /bin/sh -c "while true;do echo hello docker;sleep 1;done"
```
启动之后，我们使用`docker ps -n 5`查看容器的信息

要查看启动的centos容器中的输出，可以使用如下方式：
```bash
$ docker logs $CONTAINER_ID   ##在container外面查看它的输出
$ docker attach $CONTAINER_ID   ##连接上容器实时查看
```
# Docker 容器的终止
使用以下命令来终止一个运行中的容器。并且可以使用`docker ps -a`来看终止状态的容器。
```bash
docker stop $CONTAINER_ID
```
![20240326224511](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240326224511.png)
终止状态的容器，可以使用`docker start`来重新启动。
![20240326224547](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240326224547.png)
使用docker restart命令来重启一个容器。
![20240326224612](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240326224612.png)

# 删除 Docker 容器
如果容器还在运行，将无法删除镜像，所以从删除容器开始。
- 查看 Docker 所有容器状态
```bash
docker ps -a
```
![20240326225006](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240326225006.png)
- 停止容器运行
```bash
docker stop $CONTAINER_ID
```
![20240326225218](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240326225218.png)
- 删除容器
```bash
docker rm $CONTAINER_ID
```
![20240326225338](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240326225338.png)

# 删除 Docker 镜像
- 查看 docker 要删除的镜像
```bash
docker images
```
![20240326225523](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240326225523.png)
- 由于刚才已终止此镜像的容器，所以直接删除
```bash
docker rmi $IMAGE_ID
```
![20240326225653](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240326225653.png)