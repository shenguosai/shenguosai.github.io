---
title: 更换新电脑后，hexo无损重新部署
date: 2023-11-14 21:44:02
tags: Hexo
categories: 
- [技术兴趣,网络服务]
---
趁着双十一在京东￥499入手了[樊想S500PRO](https://item.jd.com/100048210573.html#crumb-wrap)的2T SSD固态硬盘。
到货后就迫不及待的换下了原来笔记本里500G的老硬盘。然后发现换硬盘新装系统后各种软件的安装和配置都要重新来一遍。

### 下面记录下hexo的迁移过程。
<!--more-->

* 首先将原硬盘的hexo目录拷贝到新硬盘。我自己的目录是```D:\99_Git\blog```。
* 配置hexo环境：
  - 安装nodejs和git，nodejs安装好才可以使用npm安装hexo，git就是用来上传博客用的；（如果不希望多装软件可只安装git而不需要git bash）
  - 如果可以将原来电脑中的```C:\User\username\.gitconfig```文件和```C:\User\username（斜体）\.ssh\```目录拷贝到同目录下就不需要重新配置了。(其中username是自己电脑的用户名)
  如果没有就只能重新设置并生成ssh key文件了，命令如下：
  ```
  git config --global user.name "username"
  git config --global user.mail "user mail address"
  ssh-keygen -t rsa -C "user mail address"
  ```
  - 进入到hexo部署文件夹，执行命令安装hexo和hexo-depolyer-git插件
  ```
  npm install hexo-cli -g
  npm install hexo-deployer-git --save
  ```
  - <b>然后这里会出现一个问题:</b>
我是使用vs code中的终端来操作的，怎么样也识别不了hexo命令，提示如下：（由于忘记截图了，网上找了张类似的）
![20231114224848](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20231114224848.png)

  - <b>解决方法</b>
  可以通过访问“请参阅……”后面的链接查看，核心问题是power shell的安全策略，将hexo命令视为了不安全脚本，不允许执行。只需要放开权限就行。
  通过管理员权限运行power shell，然后输入命令：```set-ExecutionPolicy RemoteSigned```
  ![20231115001919](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20231115001919.png)
  选择“Y”即可。
  然后就可以正常在[VS Code](https://code.visualstudio.com/)的终端里使用[hexo](https://hexo.io/zh-cn/index.html)了。
  - <b><font color=blue>碰到的第二个问题：</font></b>
  上传时提示有换行符的问题，在执行```hexo d```后会出现整屏的提示信息，是我这个强迫症患者所无法忍受的。
  - <b><font color=blue>解决方法：</font></b>
  通过执行命令```git config --global core.autocrlf false```将默认的换行符转换功能去掉。
  ```
  #提交时转换为LF，检出时转换为CRLF（默认）
  git config --global core.autocrlf true

  #提交和检出时均不做换行符转换（修改）
  git config --global core.autocrlf false
  ```

* 图床设置
下载插件vs code的插件PicGo，配置方法网上有很多，但是需要注意一点在```Picgo > Pic Bed > Github: Custom Url```中需要填入图传链接格式为```https://raw.githubusercontent.com/用户名/仓库名/分支名```，一开始这个地方没有设置对对上传没有影响但是在博客中看不到图片。
