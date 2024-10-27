---
title: Nginx反向代理实现访问路由器控制界面
date: 2024-03-28 09:54:56
tag: [iPhone,Nginx]
categories: 
- [技术兴趣,网络服务]
---
前两天把家里空闲的手机翻出来想着干点什么，两个小米 Note，一个 iPhone4，两个 iPhone6，一个三星 Galaxy A9 Pro和一个三星 Galaxy A32。
- iPhone4 被我手残升级 iOS 搞成了砖，现在只能看白苹果了。。。
- 两个小米 Note 折腾了半天也没法解锁 BootLoader, 先放置了
- 一台 iPhone6 安装了“掌上看家” APP 在家里做个监控用来看孩子有没有玩电子设备
- 另一台 iPhone6 越狱出来做了一些小处理，本文重点来讲这个
- 三星 Galaxy A9 Pro 已经解锁 BootLoader 成功还没想好干什么
- 三星 Galaxy A32 还没折腾
<!--more-->

废话少说，今天来聊聊用越狱的 iPhone6 安装 nginx 实现反向代理访问华为路由器 AX3 Pro 的管理界面。

# iPhone6 越狱
这个没有记录，等后期越狱另外一台 iPhone6 的时候再详细描述。

# 配置一下越狱的 iPhone6
这个跟本篇设置关联不大，主要是配置了 bash 和 Vim。
越狱后的 iPhone 系统内核是 Drawin，使用 Linux 命令`uname -a`就可以查看:
```bash
[root@iPhone ~]#uname -a
Darwin iPhone 18.7.0 Darwin Kernel Version 18.7.0: Fri Aug 19 23:28:26 PDT 2022; root:xnu-4903.272.5~1/RELEASE_ARM64_T7000 iPhone7,1 arm Darwin
```
## bash设置
iPhone 越狱后默认的 shell 是 zsh，由于一直习惯于使用 bash，就换成了 bash。

### 使用命令`cat /etc/shells`查看系统支持的 shell:
```bash
[root@iPhone ~]#cat /etc/shells
# /etc/shells: valid login shells
/bin/sh
/usr/bin/sh
/bin/zsh
/usr/bin/zsh
/bin/bash
/usr/bin/bash
/usr/bin/dash
```

### 将默认 shell 更改为 bash:
```bash
chsh -s /bin/bash
```

### 查看当前用户 shell
```bash
[root@iPhone ~]#echo $SHELL
/bin/bash
```

### 更改命令行提示符 PS1 及 常用命令

    ```conf
    export PATH='/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/bin/X11:/usr/games'
    # export PS1='\h:\w \u\$ '
    export PS1='[\u@\h \W]\$'
    umask 022

    export EDITOR=/usr/bin/editor
    export PAGER=/usr/bin/pager

    for i in /etc/profile.d/*.sh ; do
        if [ -r "$i" ]; then
            . $i
        fi
    done

    export LS_COLORS='no=00:fi=00:di=01;34:ln=01;36:pi=40;33:so=01;35:bd=40;33;01:cd=40;33;01:or=01;05;37;41:mi=01;05;37;41:ex=01;32:*.cmd=01;35:*.exe=01;35:*.com=01;35:*.btm=01;35:*.bat=01;35:*.sh=01;35:*.csh=01;35:*.tar=01;31:*.tgz=01;31:*.arj=01;31:*.taz=01;31:*.lzh=01;31:*.zip=01;31:*.z=01;31:*.Z=01;31:*.gz=01;31:*.bz2=01;31:*.bz=01;31:*.tz=01;31:*.rpm=01;31:*.cpio=01;31:*.jpg=01;35:*.gif=01;35:*.bmp=01;35:*.xbm=01;35:*.xpm=01;35:*.png=01;35:*.tif=01;35:'
    alias la='ls -al'
    alias ll='ls -l'
    ```
> 将第3行注释掉；
> 添加第3行；
> 添加15、16、17行；
> 其它都是默认的。
> **注:不知道原因，但是在这个系统里只能修改这个文件(/etc/profile)来配置 bash，在用户目录下使用`.bashrc`修改没有效果，应该是默认不 load `.bashrc`文件。**

## 配置 VIM
这个就在用户目录下使用`touch .vimrc`和`mkdir .vim`创建相关的配置文件和目录即可。
.vimrc 配置如下:

```vim
set nu
set backspace=indent,eol,start
"set cursorline
"hi cursorline ctermbg=lightgray guibg=lightblue

set nobackup
set noswapfile
set nowritebackup
set noundofile

set nocompatible
set wrap
set laststatus=2
set statusline=%f%m%r%h%w\ [FORMAT=%{&ff}]\ [TYPE=%Y]\ [POS=%l,%v][%p%%]\ [LEN=%L]
set ruler
set mouse=a
set showcmd
set cmdheight=2
set history=1000

set completeopt=preview,menu
set clipboard+=unnamed
set autoindent
set tabstop=2
set softtabstop=2
set shiftwidth=2
set noexpandtab

syntax on
filetype on
filetype plugin on

if version >= 603
    set helplang=cn
    set encoding=utf-8
endif

inoremap ( ()<ESC>i
inoremap [ []<ESC>i
inoremap { {}<ESC>i
inoremap < <><ESC>i
inoremap ' ''<ESC>i
inoremap " ""<ESC>i
```

# 在越狱的 iPhone6 上安装 OpenSSH
这个属于常规操作，在越狱的市场里下载就行。(略)

# 在越狱的 iPhone6 上安装 Nginx
使用命令:
```bash
apt-get install nginx
```
就会下载软件和相应的依赖包，当时没有记录，应该是下载了`nginx`，`nginx-common`，`nginx-core`等文件。

# 配置 Nginx 使其访问当前设备时指向路由器管理界面

## 修改 Nginx 配置文件

安装完成后在```/etc/nginx```目录中会有```nginx.conf```和```nginx.conf.defaut```两个文件，其中```nginx.conf```是 Nginx 实际运行时使用的配置文件，```nginx.conf.default```应该是个模板示例。
1. ```cp nginx.conf.default nginx.conf```使用模板覆盖原来的```nginx.conf```；
2. 在 http 模块的 server 里加入语句: ```proxy_pass http://192.168.3.1/; #即反向代理要访问的地址```。
   示例如下:
   /etc/nginx/nginx.conf 文件设置如下:
```nginx
    server {
    listen       80;
    server_name  localhost;
    #charset koi8-r;
    #access_log  logs/host.access.log  main;
    location / {
        proxy_pass http://192.168.3.1/;
        root   html;
        index  index.html index.htm;
    }
```
其实其它都是 nginx.conf.default 中的内容，只要在`location`里面加入`proxy_pass http://192.168.3.1/;`这一句就可以，这样当访问此台设备的`80`端口时就会直接指向到路由器管理台的`192.168.3.1`了。

## Nginx 常用命令
因为没有找到 Nginx 的 plist 文件来启停服务，所以需要使用软件命令来进行操作。
首先可以使用```nginx -h```来查询命令使用方法。
常用命令:
```bash
nginx # 启动 Nginx
nginx -s reopen # 重新启动 Nginx
nginx -s reload # 在不关闭 Nginx 的情况下重新加载 /etc/nginx/nginx.conf 配置文件
nginx -s stop # 关闭 Nginx
nginx -t # 测试配置文件 /etc/nginx/nginx.conf
nginx -T # 测试 配置文件 /etc/nginx/nginx.conf 并将其输出到屏幕
nginx -v # 输出版本信息
```

# 外网访问路由器管理台
将设置了 nginx 的设备端口`80`映射到外网即可，比如当前设备的局域网 ip 为`192.168.3.111`，映射外网端口为`11111`。
这样在外网访问时只要输入网址: `http://domain或者IP:1111` 就可以实现外网直接访问路由器管理后台了。