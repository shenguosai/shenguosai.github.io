---
title: Windows11 激活方法
date: 2024-02-26 10:47:23
tags: Windows
categories:
- [技术兴趣,软件应用]
---
今天将笔记本自带的 win11 家庭版升级到专业版时无意中在知乎中找到了一个方法，一试还真的有效，因为远程运行了一个网站的程序，不知道自己的电脑是不是成为别人的肉鸡了，但是激活方式确实是有效的。风险性后面再研究吧。
<!--more-->

### 两个激活秘钥
```J8WVF-9X3GM-4WVYC-VDHQG-42CXT```
```7Y64F-88DCY-Y6WTC-H33D2-64QHF```
我使用的是第一个，第二个是否有效没有验证。

### 方法
1. 右键“此电脑”，依次点击“属性” => “产品秘钥和激活” => “更改产品秘钥”，输入上述秘钥；
2. win + R，输入powershell，回车。在弹窗中输入命令:```irm massgrave.dev/get.ps1 | iex```
3. 会弹出下面的窗口
   ![20240226105515](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240226105515.png)
   简单解释一下: 按1，Windows永久激活；按2，office永久激活；按3，Windows激活到2038年；按4，180天的kms激活工具；……
4. 然后重复1就会看到“已激活”字样。