---
title: 【转】越狱后玩耍IPhone系统(Darwin)
date: 2024-03-27 17:21:46
tags: iPhone
categories:
- [技术兴趣,设备及使用]
---
无意中找到一个介绍越狱后iOS的说明， 受益匪浅， 转一下:[【Darwin】 越狱后玩耍IPhone系统](https://www.cnblogs.com/franknihao/p/7360695.html)
<!--more-->
大家都知道 iOS 是自 Mac OS 修改而来的。而 Mac OS 和 iOS 的共同核心是 Darwin, 其基于 FreeBSD 发展而来, 整体而言也是个类Unix系统。之前把自己的手机越狱之后正好开始接触 Linux 这类 OS， 然后觉得很有意思就去网上找了些资料来自己玩自己的手机， 也做了一些笔记。说来惭愧， 当时手机系统版本还是 8.3。而后来也因为没什么时间加上这部分笔记维护在另外一个笔记本上， 导致一直没有时间记录下来【捂脸】。本来想等 10.3 的越狱出来， 把Pad越狱之后再来详细研究的， 无奈 10.3 越狱也迟迟不出， 不得已， 只好现在来记录一下了。。
没有什么干货的技术知识， 全部都是一点个人的爱折腾手机的小兴趣， 系统版本又老。。嗯就这样吧。
# 第一步
IPhone 越狱后， 第一标志当然就是在桌面上看到了 Cydia。作为折腾手机的第一步， 当然是要让手机有本地命令行来装逼了。如果没记错的话， 几年前越狱的时候还是会附带一个命令行工具的， 现在好像必须得自己下载安装了。进入 Cydia， 搜索安装 MTerminal 这个命令行工具。其他也有一些命令行工具比如 mobileTerminal 什么的， 不过那些好像都已经过时了， 新系统们都打不开。打开 MTerminal 默认用户是 mobile， 退出时自动也会退出这个用户， 不用担心用户进程会留存在机器上。还有一点比较有意思的是 MTerminal 的非键盘区域， 点击靠上下左右的边部区域就像是键盘上按上下左右方向键， 所以可以进行退格和重复上条命令等操作。至于 Tab 键自动补全命令什么的操作至今还未发现。。
有了命令行， 但是在手机上敲命令太蛋疼了， 我们最好能通过电脑连入手机， 这样方便多了。第一个想到的当然是通过 SSH。在 Cydia 安装 openSSH， 之后就可以通过 22 端口远程连接手机了。一般可以直接用 root 用户连入手机， 进入之后的第一件事
**千万要记得改root账户的密码， 所有越狱后的IPhone手机root用户的默认密码都是一样的！！！都是 alpine ！不马上改掉， 那随便什么人都可以直接连入你的 IPhone， 你就没有任何安全可言了。**
改完密码之后， 就可以对字符界面的 iOS 做一些改造让它看起来更像是一台服务器了。
# 适当改造
首先可以在`/etc/profile`中加上对环境变量 PS1 的定义， 这个变量决定的是字符提示行的样式。这个文件是什么作用在 Linux 笔记中说过了， 就不说了， 样式可以选择`[\u@\h \W]\$`这款。在文件中加上`export PS1='[\u@\h \W]\$'`即可。
默认的`ls`命令出来的文件没有任何颜色特效， 看起来很累。可以加上`LS_COLORS`这个环境变量， 至于具体的值怎么写可以参见已经配置好的 CentOS 系统等等， 我这里给出一个参考: 
```bash
export LS_COLORS='no=00:fi=00:di=01;34:ln=01;36:pi=40;33:so=01;35:bd=40;33;01:cd=40;33;01:or=01;05;37;41:mi=01;05;37;41:ex=01;32:*.cmd=01;35:*.exe=01;35:*.com=01;35:*.btm=01;35:*.bat=01;35:*.sh=01;35:*.csh=01;35:*.tar=01;31:*.tgz=01;31:*.arj=01;31:*.taz=01;31:*.lzh=01;31:*.zip=01;31:*.z=01;31:*.Z=01;31:*.gz=01;31:*.bz2=01;31:*.bz=01;31:*.tz=01;31:*.rpm=01;31:*.cpio=01;31:*.jpg=01;35:*.gif=01;35:*.bmp=01;35:*.xbm=01;35:*.xpm=01;35:*.png=01;35:*.tif=01;35:'
```
在设置好了配置文件之后`source`之以生效， 至于`ls`的颜色， 如果还是没有效果记得看一下`ls`有没有默认`alias ls='ls --color'`， 必须带上`color`参数`ls`才会显色。
忘记了系统是否自带了 vim 程序， 应该是没有， 那就需要安装。vim 的样式文件 vimrc 不放在`/etc/vimrc`， 而是放到`/usr/share/vim/`目录下面去。
# 常用工具安装
在 CentOS 上习惯了用`yum`安装包， 但是这里没有`yum`， 只有`apt-get`。用法是类似的: 
```bash
apt-get install <PackageName>　　# 安装某个包
apt-get update　　# 更新 apt 源（是不是有点眼熟， 其实估计 Cydi 里面的更新源就是运行了下这个命令）
apt-get remove <PackageName>　　# 卸载某个包
```
在update过后， apt会把所有更新到的包信息放到缓存里面， 如果想要从最新的包信息中搜寻某个特定的包可以执行: 
```bash
apt-cache search <PackageName>
```
这条命令用的还是模糊搜索， 可以帮助你寻找想要装的包名到底叫什么。由于一般人记的肯定是命令名而不是包名， 所以下面给出的是一些命令和它们所在包的名字。如果想用这些命令就`apt-get install`安装这些包吧:

|Tools Name|Package Name|
|----|----|
|ifconfig, netstat, route|network-cmds|
|curl|curl|
|top|top|
|wget|wget|
|finger,last,ps|adv-cmds|
|vim|vim|
|locate|mlocate|
|lrz,lsz (命令名和 linux上 略有出入， linux 上是直接 rz， sz， 这里多个 l )|org.thebigboss.lrzsz
|gcc|iphon-gcc|
|less|less|

这里稍微提醒一句， 如果通过SSH经电脑连入手机操作， 虽然是在电脑上打命令， 不过网络走的还是手机的路， 所以要注意手机别开 4G 跑流量了。。。
另外装了一个新包之后不知道有什么用或者怎么用时可以到手机端 Cydia--已安装--<包名> 里面， 划到最下面的文件系统内容中， 看这个包具体安装了的哪些文件， 有什么文件被安装到了`/usr/bin`这种目录下， 那个文件自然就是一个命令了。
# 一些酷酷的事情
上面装的一些东西都是管理机器必须的， 实用的工具。还有一些比较花哨， 适合用于装逼的工具。比如下面: 
## 远程打开程序
bigboss源上有个叫做 open(com.conadkramer.open) 的插件， 装上之后可以在 SSH 里输入`open <APPID>`命令来远程让手机打开一个 APP。APPID 是每个 APP 目录下的 plist.xml 文件中记录的`CFBundleIdent.fier`的值， 比如 safari 的话就是 com.Apple.mobilesafari ， 系统自带的音乐的话就是 com.Apple.Music。
## netstat
`netstat -ntlp`在这个系统里不管用， 要用只能用`netstat -an`。而`-p`在这里的 netstat 是 protocol 的意思。所以你可以`netstat -ntlp TCP`来显示所有 TCP 连接的情况。
## 安装python
Cydia 中搜索一下 Python， 似乎可以找到一个 python2.5.1 版本的包， 虽然老了点， 但是勉强凑合可以用。至于用源码进行安装我没有试过， 估计也可以吧。装上 python 之后就可以更加剧烈地折腾手机了。比方说为了安全性考虑， 我的手机平时都是关闭 22 端口的， 想要开启的时候再开。于是就写了一个 python 脚本， 做成命令来手动开关 22 端口的访问。
## 启停服务
其实上面说的那个 python 脚本就是调用了启停服务的命令。在`/System/Library/LaunchDaemons`和`/System/Library/LaunchAgents`下面有很多跟服务相关的 plist 文件。`LaunchDaemons`指系统启动时自动启动的程序， 而`LaunchAgents`中存放的是针对某个用户登录时自动启动的程序的配置。
想要启动或停止一个服务， 可以这么操作: 　　
```bash
launchctl load/unload -w <plist的路径>
```
因为指定了 plist 文件的路径， 所以 plist 文件未必一定要在那两个指定目录下， 完全可以拷贝出来然后自己手动指定它的路径以启动。
## 查看服务状态
```bash
launchctl list | grep ssh # 查看ssh的服务开启状态
```
## 媒体和 APP 文件的位置(系统版本比较老， 估计新版本系统已经有变化了)
自带播放器的音乐和视频文件都放在`/private/var/mobile/Media/ITunes_Control`里面， 但是文件名是被点窜过的， 要用一定的手段来定位寻找特定的文件。（比如看大小， 看修改日期， MP3 文件的话还可以解析其标签来判断是什么歌等等）
用户的 APP 的文件基本上都放在`/private/var/mobile/Container/Data/Application/<一个被点窜过的目录名>`。因为目录名是一串没有规律的， 很难定位哪个目录对应哪个 APP。办法还是有的， 比如可以通过脚本把每个目录的大小算出来， 然后比较一下手机里的信息。还有定位特定的文件等等， 方法就靠灵活自己想了 。
# 如何通过 usb 直接连系统后台
上面连接进 iOS 系统都是通过了 WIFI 这个媒介。虽然方便一些， 但是存在速度稍慢， 安全性有疑问等问题。其实我们可以通过一些手段通过 USB 来模拟 SSH 连接， 这样通过一条数据线， 即使没有WIFI也可以做到连接系统了。
做法是首先确保系统中有合适的驱动。Mac 之类的机器就不用说了肯定是有的， 像没有装 itunes 的 windows 机器的话可以下载安装`AppleApplicationSupport`和`AppleMobileDeviceSupport`这两个驱动程序， 只要有这两个就行， 也不用安装整个庞大的 itunes了。
然后， 我们下载一个名叫*usbmxud*的工具。这个百度一下就能下载到了， 然后在命令行运行一下这个工具中的一个脚本: 
```bash
python <usbmuxd目录>/python-client/tcprelay.py -t 22:10022
```
然后这个程序就会在 PC 端开启本地的 10022 端口， 并把这个端口映射到 iOS 端的 22 端口。只要 iOS 开了 22 端口， 就可以在本地通过工具 ssh 连接 10022 端口就可以了。
同理， 不仅限于 22 端口， 其他任何 iOS 上的端口都可以进行映射。非常好用。
# iOS上一些重要文件的位置等
这位大神的博客还挺有参考价值的。
http://www.cnblogs.com/ygm900/category/538781.html

> 短信数据库的存放位置在ios的: //private/var/mobile/Library/SMS/sms.db
> 联系人数据库存放的位置在ios的: //private/var/mobile/Library/AddressBook/AddressBook.sqlitedb
> 联系人的头像估计存放在这里: //private/var/mobile/Library/AddressBook/AddressBookImages.sqlitedb
> 通话记录数据库的存放路径是: //private/var/wireless/Library/CallHistory/call_history.db
> 备忘录数据库的存放路径是: //private/var/mobile/Library/Notes/notes.sqlite
> safira 浏览器的收藏夹数据库存放路径是: //private/var/mobile/Library/Safari/Bookmarks.db
> 日历数据库的存放路径是: //private/var/mobile/Library/Calendar/Calendar.sqlitedb
