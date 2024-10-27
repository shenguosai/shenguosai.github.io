---
title: 使用PS软件进行一寸证件照片打印排版
date: 2024-08-17 00:13:02
tags: Adobe
categories:
- [技术兴趣,图片处理]
---
# 环境
系统: Windows 11
软件: Adobe Photoshop 24.5.0 版
<!--more-->
# 打开要排版的照片
用 PS 软件打开要排版的照片。
![20240817002004](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240817002004.png)
![20240817002207](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240817002207.png)

# 制作一寸证件照图案
## 设置画布给照片周围加上一圈白边
图像 => 画布大小
![20240817002639](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240817002639.png)
勾选相对，宽度和高度设置为 0.1 厘米，可根据实际情况调整，颜色选择白色，点击确定。
![20240817002909](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240817002909.png)
## 裁剪图片
图像 => 图像大小
![20240817003221](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240817003221.png)
勾选重新采样，一寸照片的宽度为 2.6 厘米，高度为 3.6 厘米，分辨率填写 300 像素/英寸。
![20240817003523](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240817003523.png)
这样，照片就变为一寸大小了。
## 定义图案
编辑 => 定义图案
![20240817003819](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240817003819.png)
图案名称根据自己需要填写，点击确定。
![20240817003856](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240817003856.png)
这样，图案就被定义并存在 PS 软件内备用了。
# 制作画布
文件 => 新建(画布)
主要填写图中框出的三个数值，即宽度、高度和分辨率。
如果要排版 3x3 的画布: 宽度 = 2.6厘米 x 3 = 7.8厘米；高度 = 3.6厘米 x 3 = 10.8厘米；分辨率填 300 像素/英寸。
如果要排版 4x2 的画布: 宽度 = 2.6厘米 x 4 = 10.4厘米；高度 = 3.6厘米 x 2 =7.2厘米；分辨率填 300 像素/英寸。
![20240817004230](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240817004230.png)
点击创建，画布就创建好了，下图为 3x3 画布。
![20240817004638](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240817004638.png)
# 将照片嵌入画布
如上图，点击右下角中间位置的圆圈图标"创建新的填充或调整图层"，然后点击"图案"。
![20240817005017](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240817005017.png)
选择刚刚定义好的图案，一般会在最后一个。
![20240817005110](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240817005110.png)
点击"确定"，这样排版就制作好了。
![20240817005413](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240817005413.png)
# 导出照片
文件 => 导出 => 导出为
![20240817005454](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240817005454.png)
这里基本上没有什么需要设置的，点击"导出"，选择希望保存的位置和文件名即可。
