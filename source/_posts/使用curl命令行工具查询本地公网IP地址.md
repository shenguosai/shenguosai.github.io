---
title: 使用curl命令行工具查询本地公网IP地址
date: 2024-02-04 09:46:39
tags: Linux
categories:
- [技术兴趣,网络服务]
---
在 Linux 中 curl 是一个利用 URL 规则在命令行下工作的文件传输工具，同时也可以查询本机公网 IP 地址，具体使用方法如下:
<!--more-->
1. 使用```curl ipinfo.io```查询:
   <pre>curl ipinfo.io</pre>
   返回值
   ```bash
    [root@MiWiFi-R3D-srv ~]# curl ipinfo.io
   {
    "ip": "1.203.179.218",// 此处是本机公网ip地址
    "city": "Beijing",// 城市名
    "region": "Beijing",// 区域名
    "country": "CN",// 国家代号
    "loc": "39.9075,116.3972",// 经纬度坐标
    "org": "AS4847 China Networks Inter-Exchange",// 运营商信息
    "timezone": "Asia/Shanghai",// 时区
    "readme": "https://ipinfo.io/missingauth"//提示ipinfo.io官网地址
   }
   ```
2. 使用```curl ifconfig.me```查询
   <pre>curl ifconfig.me</pre>
   返回值
   ```bash
   [root@MiWiFi-R3D-srv ~]# curl ifconfig.me
   1.203.179.218 //只显示本机公网IP地址
   [root@MiWiFi-R3D-srv ~]#
   ```
3. 使用```ipinfo.io```加```ip```地址，可查血这个 ip 的信息
   <pre>curl ipinfo.io/192.168.32.84</pre>
   返回值
   ```bash
    [root@MiWiFi-R3D-srv ~]# curl  ipinfo.io/103.208.15.138
   {
    "ip": "103.208.15.138",//被查询的IP地址
    "city": "Beijing",//所属城市
    "region": "Beijing",//所属区域
    "country": "CN",//国家代码
    "loc": "39.9075,116.3972",//经纬度坐标
    "org": "AS9808 China Mobile Communications Group Co., Ltd.",//运营商信息
    "timezone": "Asia/Shanghai",//时区
    "readme": "https://ipinfo.io/missingauth"//ipinfo.io官网地址
   }
   ```
4. 此命令在 Windows 的命令提示符中也可以使用
   使用方法如下:
   ```cmd
    C:\Users\User>curl ipinfo.io
   {
    "ip": "1.203.179.218",
    "city": "Beijing",
    "region": "Beijing",
    "country": "CN",
    "loc": "39.9075,116.3972",
    "org": "AS4847 China Networks Inter-Exchange",
    "timezone": "Asia/Shanghai",
    "readme": "https://ipinfo.io/missingauth"
   }
   ```
5. 最简单的是使用 [https://ip138.com/](https://ip138.com/) 网址进行本机公网IP地址查询
   ![20240204101450](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240204101450.png)