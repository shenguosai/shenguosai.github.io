---
title: Sublime Text (Build 4169) 注册方法
date: 2023-12-03 00:37:03
tags: Sublime
categories: 
- [技术兴趣,软件应用]
---

截止2023年12月2日，最新版本是[Sublime Text 4 (Build 4169)](https://www.sublimetext.com/)。
注册方法如下:
* 安装软件:
  * 去官方网站下载并安装，[Sublime Text - Text Editing, Done Right](https://www.sublimetext.com/)
* 修改sublime的执行文件:
  * 使用16进制编辑器或打开[https://hexed.it](https://hexed.it/)
  * 点击“打开文件”打开 Sublime Text 安装目录下的 sublime_text.exe
  * 在右边的“搜索”中输入```807805000f94c1```并按回车键
  * 将搜索方案中的“启用替换”选中，在“替换为”中输入```c64005014885c9```即可
  * 点击“另存为”，保存修改后的文件到本地，将文件名设定为 sublime_text.exe
  * 进入安装目录，备份原 sublime_text.exe 文件(如修改为 sublime_text_bak20231202.exe )
  * 将修改后刚刚保存到本地的 sublime_text.exe 复制到原 Sublime Text 4 的安装目录中

<!--more-->
![20231203005935](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20231203005935.png)

如此便成功激活了，可以发现原来标题栏中的 “Unregistered” 变为了 “ADMIN”
![20231203010151](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20231203010151.png)

Sublime Text 4 (Build 4200)
将：
89 D9 FF 50 38 EB 02 31 C0 48 8B 8E D8 04 00 00 0F B6 51 05 83 F2 01 44 8D 04 55 04 00 00 00 01 D2 80 79 04 00 44 0F
替换为：
89 D9 FF 50 38 EB 02 31 C0 48 8B 8E D8 04 00 00 *C6 41 05 01 31 D2 90 44 8D 04 55 04* 00 00 00 01 D2 80 79 04 00 44 0F