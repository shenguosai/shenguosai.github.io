---
title: QNAP虚拟机安装ubuntu 18.04 无法登录远程桌面
date: 2024-08-08 15:10:29
tags: QNAP
categories: 
- [技术兴趣,网络服务]
---
# 在 QNAP 上安装 Ubuntu Linux 工作站
本来想在 QNAP 上使用 "Vitualization Station 虚拟化工作站"中安装一个 Linux 服务器版系统跑一些服务，但是尝试了 Debian 12.6 和 Ubuntu 18.04 都卡在了磁盘分区的设置上，感觉像是识别不到硬盘，无法安装文件系统。
没有办法只有转战 QNAP 自带的 Ubuntu Linux 工作站了，通过跟客服咨询，这个 App 只有桌面版无法安装 Server 版。
既然安装了桌面版，那当然要设置成能够远程登录了。
<!--more-->
# 安装 xrdp 服务器等软件
由于一直登录不上，所以搞清楚原因之前杂七杂八装了很多，也不知道哪个是非必需的，总之目前安装上的软件如下:(安装命令从上到下为个人感觉必要性高低)
```bash
sudo apt-get update
sudo apt-get install xrdp
sudo apt-get install xfce4 xfce4-goodies
sudo apt-get install vnc4server tightvnc
sudo apt-get install xorg
```
刚开始的时候只安装了 xrdp 和 xfce4，感觉只用这两个就可以了，但是由于后面一直登录不上不知道原因所以又装了一大堆。
还有好像可以不用 xfce4，直接连接 GNOME 的远程桌面，那样的话我感觉才 xfce4 都不需要安装，没有详细研究先不讨论。

安装完之后打开服务，由于使用 zerotier 进行组网，所以没有进行端口映射和防火墙的设置。
```bash
sudo systemctl start xrdp
```

# 无法登录远程桌面及解决方法
通过 Win11 连接远程桌面的时候发现输入完登录信息点击"OK"按钮之后就一直没有任何反应，通过查看服务状态```systemctl status xdrp```。发现错误信息:```[ERROR] xrdp_wm_log_msg: Error connecting to sesman: 127.0.0.1 port: 3350``` 。
通过搜索，在 Github 上找到了[答案](https://github.com/neutrinolabs/xrdp/issues/1777)，虽然对原理不是很理解，但是觉得说的很有道理于是开始尝试。
![20240808153629](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240808153629.png)
这是在设置的时候有点问题，本人设置步骤如下:
```bash
sysctl -a | grep disable_ipv6
```
> sysctl: reading key "net.ipv6.conf.all.stable_secret"
> net.ipv6.conf.all.disable_ipv6 = 1
> sysctl: reading key "net.ipv6.conf.default.stable_secret"
> net.ipv6.conf.default.disable_ipv6 = 1
> sysctl: reading key "net.ipv6.conf.eth0.stable_secret"
> net.ipv6.conf.eth0.disable_ipv6 = 0
> sysctl: reading key "net.ipv6.conf.eth1.stable_secret"
> net.ipv6.conf.eth1.disable_ipv6 = 0
> sysctl: reading key "net.ipv6.conf.eth2.stable_secret"
> net.ipv6.conf.eth2.disable_ipv6 = 0
> sysctl: reading key "net.ipv6.conf.lo.stable_secret"
> net.ipv6.conf.lo.disable_ipv6 = 1
> sysctl: reading key "net.ipv6.conf.ztjlhq7oim.stable_secret"
> net.ipv6.conf.ztjlhq7oim.disable_ipv6 = 1

于是根据上面的解答解答需要将```net.ipv6.conf.eth0.disable_ipv6```、```net.ipv6.conf.eth1.disable_ipv6```、```net.ipv6.conf.eth2.disable_ipv6```的值设为 1。
## 方法一
```bash
sudo sysctl -w net.ipv6.conf.eth0.disable_ipv6=1
sudo sysctl -w net.ipv6.conf.eth1.disable_ipv6=1
sudo sysctl -w net.ipv6.conf.eth2.disable_ipv6=1
```
## 方法二
设置```/etc/sysctl.conf```文件
```bash
sudo vi /etc/sysctl.conf
```
在文件末尾加入下面三行:
```bash
net.ipv6.conf.all.disable_ipv6=0
net.ipv6.conf.default.disable_ipv6=0
net.ipv6.conf.lo.disable_ipv6=0
```
使配置修改生效
```bash
sudo sysctl -p
```
经实验，上述两种方法只能暂时禁用```ipv6```，重启之后就会失效。长期生效方法:[如何在 Ubuntu Linux 上禁用 IPv6](https://linux.cn/article-12689-1.html)

然后就登录进去了，虽然是 xfce 桌面。
![20240808154353](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240808154353.png)
