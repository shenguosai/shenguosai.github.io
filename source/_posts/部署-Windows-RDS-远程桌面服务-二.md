---
title: 部署 Windows RDS 远程桌面服务(二)
date: 2024-03-02 20:35:58
tags: Windows
categories: 
- [技术兴趣, 网络服务]
---
转自[超详细的 部署Windows RDS 远程桌面服务来啦！！！确定不来看一下嘛~](https://blog.csdn.net/m0_70919503/article/details/134965019)，留待以后部署自己的远程桌面网关。

-----*开始正文*-----
<!--more-->
用 Windows Server 2016 操作系统来完成 RDS 的部署，包括添加远程桌面服务角色，创建会话集合，发布 Remote App 程序，以及测试访问 Remote App 程序。
首先准备两台部署好域环境的虚拟机: Windows Server 2016 和 Windows 7

### 添加远程桌面服务角色
打开 Windows Server 2016:
    (1) 点进服务器管理器，点击“添加角色和功能”
    ![20240302204322](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302204322.png)
    (2) 在安装类型选择“远程桌面服务安装”
    ![20240302204403](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302204403.png)
    (3) 在“选择部署类型”窗口中选择“标准部署”按键，后“下一步”
    ![20240302204503](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302204503.png)
    (4) 在“选择部署方案”窗口中，选择“基于会话的桌面部署”按键，后“下一步”
    ![20240302204559](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302204559.png)
    (5) 系统会自动安装和配置必需的角色服务，直接“下一步”
    ![20240302204643](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302204643.png)
    (6) 依次指定下面三个服务所在的服务器，在“指定 RD 连接代理服务器”窗口中，将服务器池中的当前服务器添加到右侧窗格中，后“下一步”
    ![20240302204837](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302204837.png)
    (7) 选择服务器池中的当前的服务器后，双击添加到右侧窗格中，后“下一步”
    ![20240302205006](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302205006.png)
    (8) 选择服务器池中的当前的服务器后，双击添加到右侧窗格中，后“下一步”
    ![20240302205108](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302205108.png)
    (9) 选择“需要时自动启动目标服务器”后，单击“部署”按键
    ![20240302205210](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302205210.png)
    (10) 安装过程中系统自动重启，重新后会自动完成剩下的安装任务
    ![20240302205308](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302205308.png)
登录自己的域账号
![20240302205410](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302205410.png)

### 创建会话集合
    (1) 打开“服务器管理器”窗口，在左侧窗格单击刚安装好的“远程桌面服务”，在中间窗格选择“概述”选项，在右侧窗格单击“创建会话合集”
    ![20240302205757](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302205757.png)
    (2) 在“开始之前”窗口中，直接“下一步”
    (3) 自己取一个便于识别的名称后，单击“下一步”
    ![20240302205902](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302205902.png)
    (4) 将服务器池中的目标服务双击添加至右侧窗格中后，单击“下一步”
    ![20240302205959](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302205959.png)
    (5) 此页面可以点击“添加”按键可以添加需要授权的用户组，我们此处直接“下一步”
    ![20240302210101](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302210101.png)
    (6) 在“指定用户配置文件磁盘”窗口中，输入用户配置文件所在的位置后，单击“下一步”
    ![20240302210201](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302210201.png)
    (7) 确认信息无误后，单击“创建”
    ![20240302210228](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302210228.png)
    (8) 创建成功后单击“关闭”按键
    ![20240302210300](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302210300.png)

### 发布 Remote App 程序
    (1) 打开“服务器管理器”窗口，单击左侧窗格“远程桌面服务”，在中间窗格单击刚创建的集合名称，单击“发布 Remote App 程序”
    ![20240302210553](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302210553.png)
    (2) 选择要发布集合的应用程序，咱选择发布自带的“画图”
    ![20240302210640](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302210640.png)
    (3) 确认要发布的 Remote App 程序列表是否正确，正确就单击“发布”按键
    ![20240302210731](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302210731.png)
    (4) 发布“画图”完成
    ![20240302210802](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302210802.png)

### AD 服务端新建用户
    (1) 打开“AD 用户和计算机”窗口
    ![20240302210852](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302210852.png)
    (2) 设置用户登录名称
    ![20240302210913](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302210913.png)
    (3) 设置用户登录密码
    ![20240302210937](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302210937.png)

### 访问 Remote App 程序(测试)
进入 Windows 7 (客户端):
    (1) 在 Web 浏览器地址栏输入 URL: “http://FQDN/rdweb”，在打开的页面中单击“继续浏览此网站”
    ![20240302211135](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302211135.png)
    (2) 输入"服务端域名\刚刚新建的用户的名字"，和登录密码后，点击“登录”
    ![20240302211242](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302211242.png)
        登录成功后出现“画图”程序图表即代表发布成功
        ![20240302211336](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302211336.png)
    (3) 单击程序图标，在浏览器底部弹出的对话框单击“打开”按键
    ![20240302211436](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302211436.png)
    (4) 点击“连接”
    ![20240302211505](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302211505.png)
    (5) 输入账号和登录密码
    ![20240302211534](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302211534.png)
    ![20240302211551](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302211551.png)

以上便是所有内容，欢迎大家前来借鉴！