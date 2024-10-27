---
title: >-
  打开word时MathType报错:MathType has detected an error inAutoExecCls.Main: 文件未找到:
  MathPage.WLL.
date: 2024-09-04 15:00:33
tags: [Windows,MathType]
categories:
- [技术兴趣,软件应用]
---
# 报错信息
不知道为什么打开 Work 时出现了 MathType 的报错。
<!--more-->
![20240904150316](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240904150316.png)
连续点击两次"确定"报错对话框消失，如果点击 MathType 按钮会再次出现报错信息。
![20240904150429](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240904150429.png)
# 解决方法
找到 MathPage.wll 文件将其复制到 Office 指定目录下。
1. MathPage.wll
   如果是 64 位的 Office，就去复制```C:\Program Files (x86)\MathType\Office Support\64\```中的 ```MathPage.wll```。
   如果是 32 位的 Office，就去复制```C:\Program Files (x86)\MathType\Office Support\32\```中的 ```MathPage.wll```。
2. Office 指定目录
   将上面的文件复制到目录: ```C:\Program Files\Microsoft Office\root\Office16```中即可正常。