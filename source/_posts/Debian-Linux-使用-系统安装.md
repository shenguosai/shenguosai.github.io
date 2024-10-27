---
title: Debian Linux 使用 -- 系统安装
date: 2024-05-11 08:32:17
tags: Linux
categories:
- [技术兴趣,软件应用]
---
# 机器配置
这台机器比较老，是200x年在日本买的富士通的 FMV-DESKPOWER，型号是 FMVFB70T。
配置如下:

配件名称|型号|规格
----|----|----
主板|FJNB023|
CPU|Core(TM)2 Duo P8400|2.26GHz 2核2线程
Chipset|Intel GM45 Express|1066MHz
内存|型号不明，看实物为尔必达颗粒|DDR3-1066 PC3-8500 1G x2
硬盘|Seagate ST3500820AS|500G容量，7200转/s，5V 0.65A，12V 0.42A
LAN|Marvell 88E8055|1000BASE-T/100BASE-TX/10BASE-T,支持WOL
WIFI|Qualcomm Atheros AR928X|IEEE802.11/a/b/g/n
USB||USB2.0(4pin x5)
显示器||19inch，1440 x 900

<!--more-->

> 官网建议最大内存 4G(2G x2)，是因为当时预装的 Windows Vista Home 是 32 位系统，最大支持 4G 内存，而根据 GN45 Express 的 Chipset 规格最大可支持 8G(4G x2) 内存。

相关命令:
```bash
lscpu #使用程序将 sysfs 和 /proc/cpuinfo 中的详细 CPU 信息输出到屏幕
sudo dmidecode -t 2 #主板信息
sudo dmidecode -t 16 #内存信息
lspci | grep Ethernet #有线网卡
lspci | greo Wireless #无线网卡
iwconfig #无线网卡
```

# 下载镜像
Debian 有很多版本，由于机器比较老，我使用的是 32 位的 dvd 映像进行的安装。
下载地址: [debian-12.5.0-i386-DVD-1.iso.torrent](https://cdimage.debian.org/debian-cd/current/i386/bt-dvd/)

# 安装系统
使用 Ventoy 正常安装即可。

# 配置
## 软件安装
由于使用的 DVD 映像进行的系统安装，在后续安装软件时会优先使用介质安装，需要在 `/etc/apt/source.list` 中关闭映像安装，如下在第一行中添加 `#` 进行注释(我在安装系统时选择的是华为的镜像源)。
```bash
# deb cdrom:[Debian GNU/Linux 12.5.0 _Bookworm_ - Official i386 DVD Binary-1 with firmware 20240210-11:28]/ bookworm contrib main non-free-firmware

deb http://mirrors.huaweicloud.com/debian/ bookworm main non-free-firmware
deb-src http://mirrors.huaweicloud.com/debian/ bookworm main non-free-firmware

deb http://security.debian.org/debian-security bookworm-security main non-free-firmware
deb-src http://security.debian.org/debian-security bookworm-security main non-free-firmware

# bookworm-updates, to get updates before a point release is made;
# see https://www.debian.org/doc/manuals/debian-reference/ch02.en.html#_updates_and_backports
deb http://mirrors.huaweicloud.com/debian/ bookworm-updates main non-free-firmware
deb-src http://mirrors.huaweicloud.com/debian/ bookworm-updates main non-free-firmware
```
## 用户权限
在安装系统时会设置两个用户，一个 root 用户，一个普通用户，而使用普通用户时默认无法使用`sudo`，所以需要安装`sudo`并添加权限。
1. 安装`sudo`
切换至 root 用户并安装`sudo`:
```bash
su root
apt install sudo
```
2. 添加权限
网上介绍了两种方法，但我使用方法一的时候没有成功，建议直接使用方法二。
方法一: 切换至 root 用户并执行下面的命令。
```bash
sudo usermod -aG sudo username
```
上面命令中的 username 换成自己的用户名。
方法二: 切换至 root 用户，在 `/etc/sudoer.d/` 目录下新建一个以自己用户名为文件名的文件。
```bash
touch /etc/sudoer.d/username
nano /etc/sudoer.d/username
```
添加以下内容:
```bash
username ALL=(ALL)ALL
```
注意上面的 username 需要换成自己的用户名，然后 Ctrl+X 保存退出并切换回自己的用户就可以了。

## 配置语言
在安装过程中如果选择系统语言为英语进行安装的话进入系统后还要设置语言为中文并安装中文输入法，折腾了一番成功了但是具体的步骤不知道是否合理。
1. 安装语言包
```bash
sudo apt update
sudo apt install locales
```
2. 配置 `locale`
```bash
sudo dpkg-reconfigure locales
```
在出现的对话框的表里一直下拉选择 `zh_CN.UTF-8` 。然后在下一个界面中选择 `zh_CN.UTF-8` 作为默认的 `locale` 。
3. 安装中文字体(非必需)
```bash
sudo apt install fonts-wqy-microhei fonts-wqy-zenhei xfonts-wqy
```
4. 安装中文输入法
  (1) 如果原来有安装先卸载
```bash
sudo apt purge fcitx* ibus*
```
  (2) 安装 fcitx5 中文拼音输入法
```bash
sudo apt install fcitx5 fcitx5-chinese-addons
```
  (3) 然后重启系统打开 fcitx 配置根据自己的需要进行配置即可。