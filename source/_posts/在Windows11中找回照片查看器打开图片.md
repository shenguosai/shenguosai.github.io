---
title: 在Windows11中找回照片查看器打开图片
date: 2023-12-12 23:41:05
tags: Windows
categories:
- [技术兴趣,软件应用]
---
1. 使用快捷键```Win + R```打开运行窗口， 输入```regedit```命令回车打开注册表
   ![20231212234749](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20231212234749.png)
<!--more-->
2. 找到```HKEY_LOCAL_MACHINE``` -> ```SOFTWARE```->```Microsoft``` -> ```Windows Photo Viewer``` -> ```Capabilities``` -> ```FileAssociations```，右键=>新建=>字符串值，名称填写```.jpg```，数值填写```PhotoViewer.FileAssoc.Tiff```，同理添加 ```.png```  ```.jpeg```，数值相同。
   ![20231212234917](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20231212234917.png)
3. 添加之后就可以使用Windows照片查看器看图。
   ![20231212235008](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20231212235008.png)
