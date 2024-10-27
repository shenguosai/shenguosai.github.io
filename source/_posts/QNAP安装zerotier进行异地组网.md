---
title: QNAP安装zerotier进行异地组网
date: 2024-08-14 10:56:19
tags: [QNAP,Zerotier]
categories: 
- [技术兴趣,网络服务]
---
# 准备工作
1. 在 App Center 中安装 QVPN Service
2. <!--more-->
   ![20240814122823](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240814122823.png)
3. 去 [ZeroTier 官网](https://www.zerotier.com/download/) 下载相关的安装包
   ![20240814123138](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240814123138.png)
   ![20240814123221](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240814123221.png)
# 从本地安装 ZeroTier
## 允许没有有效数字签名的程序安装
![20240814123719](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240814123719.png)
## 安装
1. 打开 App Center，右上角"手动安装"；
2. 点击"浏览"，选中安装包；
3. 点击"安装"。
![20240814124524](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240814124524.png)
安装完成后，可以在所有应用中找到 Zerotier，但是和群晖不同，威联通上的 Zerotier 是无法点击打开的。(不明白为什么在"我的应用程序"中无法找到 Zerotier。)
![20240814124911](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240814124911.png)

# 加入网络
Zerotier 的注册及申请方法不在此赘述，威联通安装好 Zerotier 软件后需要使用命令行来加入网络。且必须获取管理员权限才能安装。
```bash
[user@QNAP ~]$ zerotier-cli join xxxxxxxxxxxxxxxx
zerotier-cli: authtoken.secret not found or readable in /share/CACHEDEV1_DATA/.qpkg/zerotier (try again as root)
[user@QNAP ~]$ sudo zerotier-cli join xxxxxxxxxxxxxxxx

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

Password:
200 join OK
```
看到```200 join OK```，即意味着这台 NAS 已经加入了 zerotier 网络，然后就可以返回 zerotier 的网页管理端对这台设备进行授权。

# 设置防火墙允许访问
如果 QNAP 安装了防火墙```QuFirewall```，则需要添加一条规则允许 ZeroTier 虚拟局域网网段允许访问。
1. 打开```QuFirewall```
2. 点击"编辑"修改规则
   ![20240814150010](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240814150010.png)
3. 点击"添加规则"；
   ![20240814170631](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240814170631.png)
4. 输入虚拟局域网的网段地址，子网掩码选```255.255.255.0/24```即可。其它设置不用更改，点击"应用"，防火墙就允许虚拟局域网网段访问了。
   ![20240814170809](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240814170809.png)