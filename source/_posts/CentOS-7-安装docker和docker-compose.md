---
title: CentOS 7 安装docker和docker-compose
date: 2024-03-25 15:06:45
tags: docker
categories: 
- [技术兴趣,软件应用]
---
# 安装 Docker
更新 `yum`
```bash
yum update
```
安装配置时的依赖包
```bash
yum install -y yum-utils device-mapper-persistent-data lvm2
```
<!--more-->
配置阿里云的`yum`源
```bash
yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```
查看仓库中所有的docker版本
```bash
yum list docker-ce --showduplicates | sort -r
```
![20240325151136](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240325151136.png)
默认使用`yum install docker-ce`安装最新版的docker，当然也可以指定安装特定的版本
```bash
yum install docker-ce-24.0* -y
```
验证是否安装成功
```bash
docker --version
```
或者
```bash
docker -v
```
# 安装 Docker-compose
国外源安装:
```bash
curl -SL https://github.com/docker/compose/releases/download/v2.26.0/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
```
国内源安装:
```bash
curl -SL https://get.daocloud.io/docker/compose/releases/download/v2.26.0/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
```
赋予可执行权限:
```bash
chmod +x /usr/local/bin/docker-compose
```
验证是否安装成功:
```bash
docker compose version
```