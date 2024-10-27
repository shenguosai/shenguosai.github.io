---
title: Debian 12.6 - 安装TCP测试工具TCPing
date: 2024-08-14 13:00:46
tags: Linux
categories:
- [技术兴趣,软件应用]
---
```TCPing```是基于 TCP 协议的一种```ping```命令，用来测试数据包能否通过 TCP 协议到达目标主机。他的一大特点，就是可以在禁```ping```的时候检测网络连通率。
<!--more-->
# 安装```tcptraceroute```
```TCPing```本质上是一个 shell 脚本，依赖```tcptraceroute```运行。
```bash
sudo apt install tcptraceroute
```
# 下载 TCPing
```bash
sudo wget http://www.vdberg.org/~richard/tcpping -O /usr/bin/tcping
```
# 配置权限
```bash
sudo chmod +x /usr/bin/tcping
```
# 使用方法
```bash
tcping 目标IP 目标端口
```
