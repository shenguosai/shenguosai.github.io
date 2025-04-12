---
title: PVE(二)修改IP地址
date: 2025-04-11 16:36:48
tags: PVE
categories:
- [技术兴趣,网络服务]
---
# 登入 PVE 后台命令行
方法一: 在局域网的电脑浏览器输入PVE的IP地址登录后台，从左边的菜单找到“PVE”—“_Shell”菜单，进入网页版的ssh界面下；或者在主机的控制台下输入root密码后登录到ssh下；
<!--more-->
![20250411165444](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20250411165444.png)
方法二: 使用 ssh 工具登入，如 Mobaxterm 。
# 编辑 /etc/network/interfaces
```vi /etc/network/interfaces```
![20250411165645](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20250411165645.png)
# 编辑 /etc/issue
```vi /etc/issue```
![20250411170326](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20250411170326.png)
# 编辑 /etc/hosts
```vi /etc/hosts```
![20250411170433](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20250411170433.png)