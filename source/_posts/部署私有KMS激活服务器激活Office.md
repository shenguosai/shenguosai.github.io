---
title: 部署私有KMS激活服务器激活Office
date: 2024-06-14 19:17:19
tags: Windows
categories: 
- [技术兴趣, 网络服务]
---
# 创建 iStoreOS 虚拟机
今年 618 入坑威联通，终于知道为什么大家都推荐群晖了，软件使用还好但是配置方法确实不太人性化。话归正传:
## 准备工作
1. QTS 中要安装有 `Virtualization Station 虚拟化工作站`；
2. 下载 iStoreOS 映像文件，本次使用的是 `istoreos-22.03.6-2024052410-x86-64-squashfs-combined.vmdk`，可以到[这里](https://fw.koolcenter.com/iStoreOS/x86_64/)下载。我下载的 vmdk 文件，然后发现威联通不认 vmdk，又用威联通自带的工具转成了 img 映像。感觉应该是可以直接使用下载的 img 映像来安装。
<!--more-->
## 安装 iStoreOS 虚拟机系统
打开 Virtualization Station，点击"建立"，弹出如下对话框；
![iStoreOS1](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/iStoreOS1.png)
虚拟机名称自己起一个喜欢的名字就可以，文件位置是虚拟机要安装的位置。
![iStoreOS2](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/iStoreOS2.png)
自定义设置里选择"使用现有映像"，然后指定转换好的的 img 映像存放路径。
其它默认即可，最后会出来配置总览，确认无误后点击"建立"，iStoreOS 的虚拟机系统就建立成功了。
## 配置 iSoreOS
由于我是使用 iStoreOS 作为旁路网关，除了配置以下网络之外其它默认即可:
首先在主路由中确认 iStoreOS 的 IP 地址，然后在浏览器中输入就可以进入到 iStaoreOS 的管理界面。
![20240615125041](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240615125041.png)
点击"网络向导"根据个人需求选择上网方式即可，这里选择"配置为旁路由"。
然后将 IP 地址、子网掩码、网关、DNS 依次输入即可。
> IP 地址: 192.168.50.73 (设置 iStorOS 的访问地址)
> 子网掩码: 255.255.255.0
> 网关: 192.168.50.1 (主路由的 IP 地址)
> DNS: 192.168.50.1 (主路由的 IP 地址)
也可 ssh 登录使用命令`vi /etc/config/network`编辑。
   编辑方法:
   找到带有`lan`字符的位置，添加如下文本:
   ```vi
   option ipaddr '192.168.50.73'
   option netmask '255.255.255.0'
   option gateway '192.168.50.1'
   ```
旁路网关其它设置默认即可。
# 安装 KMS 服务器插件
点击"iStore"，找到"KMS服务器"插件，点击右下角的"安装"按钮。
![20240615144400](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240615144400.png)
安装完成后就可以在"iStore"的已安装中看到，点击打开，看到绿色的"KMS 运行中"就说明已经可以了。
![20240615144523](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240615144523.png)
# 激活Office
激活 Office 这里出了一点小问题，由于以前通过激活软件进行的激活，现在到期了，如果激活时出现`0x80080005`错误码需要设置一下把原来的配置清空。
参照:[[IDEA] 增加修复0x80080005错误的功能 #216](https://github.com/YerongAI/Office-Tool/issues/216)
问题描述:
目前发现 服务器运行失败 (0x80080005) 是由于KMS工具劫持 SppExtComObj.exe 程序，而未能正确运行导致，因此可以在Office Tool 中增加对此劫持的检测，并进行修复。
如运行 C:\WINDOWS\system32\SppExtComObj.exe，正常情况应会无任何提示的返回，失效的KMS劫持会导致弹窗报错如下图
![20240615144857](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240615144857.png)
解决方法:
1. 检测注册表是否存在配置项 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\SppExtComObj.exe
   如存在，则将其删除
2. 运行 C:\WINDOWS\system32\SppExtComObj.exe，如正常则会立刻返回，退出码应为0
3. 提示用户尝试再次激活

上述操作完成之后，如为安装可以先使用 [Office Tool Plus.exe](https://otp.landian.vip/zh-cn/download.html) 选择需要的版本进行部署。
本文以 office 2021 举例:
开始 => CMD => 使用管理员打开，分别输入以下命令进行激活:
```bash
cd /d "%ProgramFiles%\Microsoft Office\Office16" #进入指定目录
cscript OSPP.VBS /sethst: 192.168.50.73:1688 #指定 kms 服务器地址和端口，默认端口 1688，如采用默认可不指定，如外网需要则要将端口映射出去
cscript OSPP.VBS /act #激活当前安装的Office。
cscript OSPP.VBS /dstatus #显示当前已安装产品密钥的许可证信息。可以查看到自已安裝的版本有多少个序列号。
```
输入上面命令后的输出分别如下:
```bash
c:\Windows\System32>cd /d "%ProgramFiles%\Microsoft Office\Office16"
C:\Program Files\Microsoft Office\Office16>cscript OSPP.VBS /sethst:kms-server addr.
Microsoft (R) Windows Script Host Version 5.812
版权所有(C) Microsoft Corporation。保留所有权利。

---Processing--------------------------
---------------------------------------
Successfully applied setting.
---------------------------------------
---Exiting-----------------------------

C:\Program Files\Microsoft Office\Office16>cscript OSPP.VBS /act
Microsoft (R) Windows Script Host Version 5.812
版权所有(C) Microsoft Corporation。保留所有权利。

---Processing--------------------------
---------------------------------------
Installed product key detected - attempting to activate the following product:
SKU ID: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
LICENSE NAME: Office 21, Office21VisioPro2021VL_KMS_Client_AE edition
LICENSE DESCRIPTION: Office 21, VOLUME_KMSCLIENT channel
Last 5 characters of installed product key: XXXXX
<Product activation successful>
---------------------------------------
Installed product key detected - attempting to activate the following product:
SKU ID: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
LICENSE NAME: Office 21, Office21ProPlus2021VL_KMS_Client_AE edition
LICENSE DESCRIPTION: Office 21, VOLUME_KMSCLIENT channel
Last 5 characters of installed product key: XXXXX
<Product activation successful>
---------------------------------------
---------------------------------------
---Exiting-----------------------------

C:\Program Files\Microsoft Office\Office16>cscript OSPP.VBS /dstatus
Microsoft (R) Windows Script Host Version 5.812
版权所有(C) Microsoft Corporation。保留所有权利。

---Processing--------------------------
---------------------------------------
PRODUCT ID: XXXXX-XXXXX-XXXXX-XXXXX
SKU ID: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
LICENSE NAME: Office 21, Office21VisioPro2021VL_KMS_Client_AE edition
LICENSE DESCRIPTION: Office 21, VOLUME_KMSCLIENT channel
BETA EXPIRATION: 1601/1/1
LICENSE STATUS:  ---LICENSED---
REMAINING GRACE: 179 days  (259172 minute(s) before expiring)
Last 5 characters of installed product key: K2HT4
Activation Type Configuration: ALL
        DNS auto-discovery: KMS name not available
        KMS machine registry override defined: kms-server addr:1688
        Activation Interval: 120 minutes
        Renewal Interval: 10080 minutes
        KMS host caching: Enabled
---------------------------------------
PRODUCT ID: XXXXX-XXXXX-XXXXX-XXXXX
SKU ID: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
LICENSE NAME: Office 21, Office21ProPlus2021VL_KMS_Client_AE edition
LICENSE DESCRIPTION: Office 21, VOLUME_KMSCLIENT channel
BETA EXPIRATION: 1601/1/1
LICENSE STATUS:  ---LICENSED---
REMAINING GRACE: 179 days  (259172 minute(s) before expiring)
Last 5 characters of installed product key: 6F7TH
Activation Type Configuration: ALL
        DNS auto-discovery: KMS name not available
        KMS machine registry override defined: kms-server addr:1688
        Activation Interval: 120 minutes
        Renewal Interval: 10080 minutes
        KMS host caching: Enabled
---------------------------------------
---------------------------------------
---Exiting-----------------------------
```
