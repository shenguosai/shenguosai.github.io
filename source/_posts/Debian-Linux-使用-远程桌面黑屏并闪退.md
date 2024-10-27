---
title: Debian Linux 使用 -- 远程桌面黑屏并闪退
date: 2024-05-11 10:39:11
tags: Linux
categories:
- [技术兴趣,软件应用]
---
# 安装 Xrdp 使用 Xorg 进行远程登录
网上有很多远程桌面的登录方法，如 VNC、X11 转发等等，由于不喜欢安装太多软件，想使用 Windoews 自带的远程桌面。
```bash
sudo apt install xrdp
```
<!--more-->
安装完成后该服务将自动开启，可通过下面命令确认是否开启:
```bash
sudo systemctl status xrdp
```
![20240511104423](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240511104423.png)
红框处显示 `active(running)` 则说明已开启。
# 解决远程桌面连接上之后黑屏并闪退问题
这个经常会有人说更换 test 版本的 xrdp 来解决，但是通过以下办法可以正常登录。
编辑`/etc/xrdp/startwm.sh`
```bash
sudo vi /etc/xrdp/startwm.sh
```
加入下面三行内容:
```bash
unset DBUS_SESSION_BUS_ADDRESS
unset XDG_RUNTIME_DIR
. $HOME/.profile
```
![20240511104947](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240511104947.png)
然后就可以通过 Xorg 登录看到桌面了。
![20240511105324](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240511105324.png)