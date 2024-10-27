---
title: 关闭 Adobe Genuine Service Alert 正版验证
date: 2024-09-09 09:10:18
tags: Adobe
categories:
- [技术兴趣,软件应用]
---
# 自用方法
1. 单击 Windows 键，输入"服务"或"Service"，打开服务选项。
2. <!--more-->
![20240909091647](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240909091647.png)
1. 在服务中找到 "Adobe Genuine Software Monitor Service" 服务。
![20240909091917](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240909091917.png)
1. 右键点击 => 左键属性，弹出属性对话框。
![20240909092020](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240909092020.png)
1. 将启动类型改为**禁止**，并在服务状态栏点击**停止**将服务状态改为**已停止**。
![20240909092245](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240909092245.png)
1. 防火墙设置规则让 Adobe Genuine Launcher.exe，AdobeGCClient.exe，AGMService.exe，AGSService.exe 无法联网。
   打开 Windows 防火墙 => 入站规则 => 新建规则 => 程序(下一步) => 此程序路径中选择程序路径 (C:\Program Files (x86)\Common Files\Adobe\AdobeGCClient)并打开 => 操作:阻止连接 => 输入名称即可。
   同样方法设置出站规则。
这样就不会再出现弹窗了。

# 网上博客中介绍的方法
网上介绍的方法都是要删掉 AdobeGCClient 正版检测文件并防止其重建，应为自用方法也可以实现，没有试过。
1. 在弹出 Adobe Genuine Service Alert 提示时打开任务管理器；
2. "任务管理器 => 进程 => 后台进程"中找到类似 "Adobe Genuine Software Client" 的进程；
3. 右击上面的进程 => 打开文件所在位置；( 我的位置是:C:\Program Files (x86)\Common Files\Adobe\AdobeGCClient\ )
4. 在任务管理器结束2中的进程并将以下文件移除，Adobe Genuine Launcher.exe，AdobeGCClient.exe，AGMService.exe，AGSService.exe，网上没有明确指出删除哪些文件，所以建议将其移动到一个专门地方进行备份，防止 Adobe 因缺少某些文件导致无法使用；
5. 新建几个空的文本文件，文件名与上面移除的文件名相同并将后缀改为 exe，以防止程序自动生成上述文件；
6. 可以在打开 Windows 的防火墙将 Adobe 相关的启动文件断网，我没有在防火墙中发现可能是以前关掉了。
  ![20240909093621](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240909093621.png)