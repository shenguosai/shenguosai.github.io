---
title: 部署 Windows RDS 远程桌面服务器(一)
date: 2024-03-02 21:22:01
tags: Windows
categories: 
- [技术兴趣, 网络服务]
---
转自[RD网关部署说明](https://www.cnblogs.com/heavyhe/p/4547058.html)，留待以后部署自己的远程桌面网关。

-----*开始正文*-----
<!--more-->
## 1. 背景
实现通过RD网关服务器实现外网访问内网的远程桌面。

### 1.1 远程桌面网关
远程桌面网关（RD 网关）是一项角色服务，使授权远程用户可以从任何连接到 Internet 并且可以运行远程桌面连接 (RDC) 客户端的设备连接到内部企业网络或专用网络上的资源。网络资源可以是远程桌面会话主机（RD 会话主机）服务器、运行 RemoteApp 程序的RD 会话主机服务器或启用了远程桌面的计算机。
RD 网关使用 HTTPS 上的远程桌面协议 (RDP) 在 Internet 上的远程用户与运行其生产力应用程序的内部网络资源之间建立安全的加密连接。

### 1.2 为什么使用远程桌面网关？
**RD 网关有很多优点，其中包括:**
通过 RD 网关，远程用户可以使用加密连接，**通过 Internet 连接到内部网络资源，而不必配置虚拟专用网络 (VPN) 连接。**
RD 网关提供全面的安全配置模型，使您可以控制对特定内部网络资源的访问。**RD 网关提供点对点的 RDP 连接，而不是允许远程用户访问所有内部网络资源。**
通过 RD 网关，大多数远程用户可以连接到在专用网络中的防火墙后面或跨网络地址转换程序 (NAT) 托管的内部网络资源。在此方案中，**通过 RD 网关，不必对 RD 网关服务器或客户端执行其他配置。**
在此版本的 Windows Server 之前，采用安全措施来阻止远程用户跨防火墙和 NAT 连接到内部网络资源。这是由于出于网络安全考虑，通常阻止端口 3389（用于 RDP 连接的端口）。RD 网关使用 HTTP 安全套接字层/传输层安全 (SSL/TLS) 隧道将 RDP 通信传输到端口 443。由于大多数公司打开端口 443 来支持 Internet 连接，所以，RD 网关利用此网络设计提供跨多个防火墙的远程访问连接。
通过远程桌面网关管理器可以配置授权策略，以定义远程用户要连接到内部网络资源必须满足的条件。例如，可以指定：
可以连接到内部网络资源的用户（即，可以连接的用户组）。
用户可以连接到的网络资源（计算机组）。
客户端计算机是否必须是 Active Directory 安全组的成员。
是否允许设备的重定向。
客户端需要使用智能卡身份验证还是密码身份验证，还是可以使用任一方法。
可以将 RD 网关服务器和远程桌面服务客户端配置为使用网络访问保护 (NAP) 来进一步增强安全性。NAP 是 Windows Server® 2008 R2、Windows Server® 2008、Windows® 7、Windows Vista® 和 Windows(R) XP Service Pack 3 中包含的运行状况策略创建、强制和补救技术。通过 NAP，系统管理员可以强制运行状况要求，可以包括软件要求、安全更新要求、所需的计算机配置以及其他设置。

**note备注**
如果 RD 网关强制 NAP，则运行 Windows Server 2008 R2 或 Windows Server 2008 的计算机不能作为 NAP 客户端使用。如果 RD 网关强制 NAP，则仅运行 Windows 7、Windows Vista 或 Windows XP SP3 的计算机可以作为 NAP 客户端使用。
有关如何将 RD 网关配置为使用 NAP 对连接到 RD 网关服务器的远程桌面服务客户端强制运行状况策略的信息，请参阅 Windows Server 2008 R2 技术中心上的"远程桌面服务"页。 [http://go.microsoft.com/fwlink/?linkid=140433（可能为英文网页）](http://go.microsoft.com/fwlink/?linkid=140433)。
可以将 RD 网关服务器与 Microsoft Internet Security and Acceleration (ISA) 服务器一起使用来提高安全性。在此方案中，可以在专用网络中（而不是在外围网络中）托管 RD 网关服务器，且可以在外围网络中托管 ISA 服务器。远程桌面服务客户端与 ISA 服务器之间的安全套接字层 (SSL) 连接可以在连接到 Internet 的 ISA 服务器上终止。
有关如何将 ISA 服务器配置为 RD 网关服务器方案的 SSL 终结设备的信息，请参阅 Windows Server 2008 R2 技术中心上的"远程桌面服务"页。 [http://go.microsoft.com/fwlink/?linkid=140433（可能为英文网页）](http://go.microsoft.com/fwlink/?linkid=140433)。
远程桌面网关管理器提供的工具帮助您监视 RD 网关服务器状态和事件。通过使用远程桌面网关管理器，可以指定为了进行审核要监视的事件（例如尝试连接到 RD 网关服务器不成功）。

### 1.3 配置远程桌面网关
此清单列出了要成功地为 RD 网关核心方案配置 RD 网关需要完成的任务。通过此方案，可以配置 RD 网关服务器，使远程用户可以通过 RD 网关服务器，从 Internet 访问内部公司网络资源或专用网络资源。在此方案中，内部网络资源可以是远程桌面会话主机（RD 会话主机）服务器、运行 RemoteApp 程序的RD 会话主机服务器或启用了远程桌面的计算机。
**配置步骤**|....................................
----|----
安装远程桌面网关角色服务。|
获取 RD 网关服务器的证书。|
创建远程桌面连接授权策略(RD CAP)。|
创建远程桌面资源授权策略(RD RAP)。|
配置 RD 网关的远程桌面服务客户端。|

有关 RD 网关的详细信息，请参见 Windows Server 2008 R2 TechCenter 上的远程桌面服务页。 [http://go.microsoft.com/fwlink/?LinkId=140433](http://go.microsoft.com/fwlink/?LinkId=140433)。

### 1.4 安装远程桌面网关的先决条件
**若要正常使用 RD 网关，必须满足下列先决条件：**
*必须拥有安装了 Windows Server 2008 R2 的服务器。*
在您要配置的 RD 网关服务器上具有*本地"管理员"组成员身份*（或同等身份），是完成此过程的最低要求。 查看有关使用适当帐户和组成员关系的详细信息，请访问本地默认组和域默认组。 [http://go.microsoft.com/fwlink/?LinkId=83477（可能为英文链接）](http://go.microsoft.com/fwlink/?LinkId=83477)。
*必须为 RD 网关服务器获取安全套接字层 (SSL) 证书*（如果还没有该证书）。默认情况下，在 RD 网关 服务器上，Internet 信息服务 (IIS) 服务使用传输层安全 (TLS) 1.0 对通过 Internet 在客户端与 RD 网关服务器之间进行的通信加密。若要正常使用 TLS，必须在 RD 网关服务器上安装 SSL 证书。
**note备注**
如果可以使用其他方法获取符合 RD 网关的要求的外部信任证书，则不需要在组织中部署证书颁发机构 (CA) 基础结构。如果您的公司没有独立 CA 或企业 CA，并且您没有受信任公用 CA 颁发的兼容证书，则可以为 RD 网关服务器创建并导入自签名证书，以便进行技术评估和测试。有关详细信息，请参阅为远程桌面网关服务器创建自签名证书。
有关 RD 网关 的证书要求以及如何获取并安装证书的信息，请参阅获取远程桌面网关服务器的证书。
*如果配置的 RD 网关授权策略要求客户端计算机上的用户是 Active Directory 安全组的成员，才能连接到 RD 网关服务器，则 RD 网关服务器必须也是 Active Directory 域的成员。*
**角色、角色服务和功能的依存关系**
若要正常使用 RD 网关，要求安装并运行多个角色服务和功能。使用服务器管理器安装 RD 网关角色服务时，将自动安装并启动下列附加的角色、角色服务和功能（如果尚未安装）：
*HTTP 代理上的远程过程调用 (RPC)*
*Web 服务器 (IIS) [Internet 信息服务]*
*若要正常使用 HTTP 代理上的 RPC 功能，必须安装并运行 IIS。*
**网络策略和访问服务**
还可以将 RD 网关配置为使用另一台运行网络策略服务器 (NPS) 服务的服务器上存储的远程桌面连接授权策略 (RD CAP)。这样，您将使用运行 NPS 的服务器（以前称为远程身份验证拨入用户服务 (RADIUS) 服务器）来集中存储、管理和验证RD CAP。如果已为远程访问方案（例如 VPN 和拨号网络）部署了运行 NPS 的服务器，则对 RD 网关方案同样使用这个现有的运行 NPS 的服务器，可以增强您的部署。

## 2. 实验环境
### 2.1 实验环境
服务器名称|系统|功能|备注
----|----|----|----
AD<br>192.168.10.1|Windows Server 2012|AD域服务，AD证书服务|VMHost上虚机
RD<br>192.168.10.14<br>10.0.1.1|Windows Server 2012|RD网关服务器|VMHost上虚机
~~RD01~~<br>~~192.168.0.15~~<br>~~10.0.1.3~~|~~Windows Server 2012~~|~~RD网关服务器~~|~~VMHost上虚机~~
Win-TTLSA6FVTKG<br>192.168.10.22|Windows 7 32 sp1|远程桌面|VMHost上虚机
Win-J3SKRGLCDLT<br>10.0.1.2|Windows 7 32 sp1|客户端|VMHost3上虚机

### 2.2 拓扑图
![20240302221216](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302221216.png)

## 3. 实验步骤
### 3.1 AD 证书服务安装与配置
#### 3.1.1 添加 ADCS 角色和功能
![20240302221315](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302221315.png)
![20240302221323](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302221323.png)
![20240302221333](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302221333.png)
![20240302221351](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302221351.png)
#### 3.1.2 ADCS 配置
![20240302221411](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302221411.png)
![20240302221424](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302221424.png)
此处要选【企业 CA】
![20240302221510](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302221510.png)
![20240302221523](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302221523.png)
![20240302221541](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302221541.png)
![20240302221551](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302221551.png)
![20240302221600](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302221600.png)
![20240302221611](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302221611.png)
![20240302221621](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302221621.png)
![20240302221634](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302221634.png)
![20240302221642](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302221642.png)
#### 3.1.3 创建针对计算机的证书模板
在 AD 中，创建证书模板。
【服务器管理器】-【工具】-【证书颁发机构】。
右键【证书模板】-【新建】-【要颁发的证书模板】。
  (1) 右击【证书模板】-【管理】- 右击【计算机】-【复制模板】，在【常规】tab 定义模板名称，在【使用者名称】选择【在请求中提供】，点击【应用】-【确定】
  (2) 右键【证书模板】-【新建】-【要颁发的证书模板】，选择模板名称为步骤 (1) 中创建的模板，点击确定。
### 3.2 安装远程桌面网关服务
![20240302222203](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302222203.png)
![20240302222213](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302222213.png)
![20240302222228](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302222228.png)
![20240302222238](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302222238.png)
![20240302222251](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302222251.png)
![20240302222301](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302222301.png)
![20240302222312](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302222312.png)
![20240302222323](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302222323.png)
![20240302222332](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302222332.png)
![20240302222347](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302222347.png)
![20240302222355](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302222355.png)
![20240302222405](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302222405.png)
![20240302222413](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302222413.png)
![20240302222425](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302222425.png)
![20240302222432](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302222432.png)
### 3.3 配置远程桌面网关服务器
以下所有配置均在 RD 网关服务器上进行。
#### 3.3.1 证书
在【控制台】完成申请证书的相关操作。证书已由前面的ADCS证书服务配置完成。
申请完证书后。
首先，要对RD网关服务器进行导入证书。
其次，要把证书保存到本地硬盘，再拷贝到客户端进行安装，可进行静默安装。
##### 3.3.1.1 申请证书
【开始】-【运行】 mmc
![20240302222633](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302222633.png)
【文件】-【添加/删除管理单元】，选择【证书】，点击【添加】。
![20240302222718](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302222718.png)
![20240302222727](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302222727.png)
![20240302222738](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302222738.png)
![20240302222745](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302222745.png)
![20240302222751](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302222751.png)
右键【个人】-【证书】-【所有任务】-【申请新证书】
![20240302222827](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302222827.png)
![20240302222837](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302222837.png)
![20240302222847](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302222847.png)
点击下一步
![20240302222907](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302222907.png)
选择新创建的模板下的【注册此证书需要详细信息。单击这里以配置设置】，弹出如下窗口
![20240302223005](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302223005.png)
在类型处选择【公用名】，在值处填写路由器的外网地址，点击【添加】按钮，点击【应用】-【确定】
证书申请完成。
![20240302223129](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302223129.png)
##### 3.3.1.2 导出证书
![20240302223156](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302223156.png)
![20240302223207](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302223207.png)
![20240302223216](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302223216.png)
![20240302223225](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302223225.png)
![20240302223233](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302223233.png)
将导出的证书拷贝到客户端进行安装。
##### 3.3.1.3 导入证书
【开始】-【运行】输入【tsgateway.msc】调用 RD 网关管理器。
![20240302223350](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302223350.png)
![20240302223401](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302223401.png)
![20240302223410](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302223410.png)
![20240302223418](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302223418.png)
#### 3.3.2 策略
##### 3.3.2.1 创建策略
![20240302223507](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302223507.png)
![20240302223515](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302223515.png)
![20240302223524](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302223524.png)
![20240302223534](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302223534.png)
![20240302223543](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302223543.png)
![20240302223554](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302223554.png)
![20240302223608](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302223608.png)
![20240302223617](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302223617.png)
![20240302223627](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302223627.png)
![20240302223649](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302223649.png)
![20240302223657](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302223657.png)
![20240302223718](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302223718.png)
![20240302223726](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302223726.png)
![20240302223734](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302223734.png)
### 3.4 客户端配置
#### 3.4.1 导入证书
从 RD 网关上拷贝导出的证书文件到客户端，双击进行安装。
#### 3.4.2 Host 文件修改
![20240302223915](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302223915.png)
#### 3.4.3 mstsc 设置
![20240302223942](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302223942.png)
![20240302223952](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240302223952.png)
### 3.5 测试远程桌面登录
### 3.6 RD 网关服务器场

## 4. 参考资料
http://wangchunhai.blog.51cto.com/225186/1139388

证书服务安装
http://lixun.blog.51cto.com/4198640/942447

证书
http://technet.microsoft.com/zh-cn/library/cc725949(v=ws.10).aspx

http://technet.microsoft.com/zh-cn/library/cc725706(v=ws.10).aspx

配置远程桌面网关服务器
http://technet.microsoft.com/zh-cn/library/cc754191(v=ws.10).aspx

TMG发布远程桌面网关服务器
http://hi.baidu.com/comet82/item/e62dc17664760f480c0a0701

安装ADCS
http://yuelei.blog.51cto.com/202879/309092/

配置CA证书
http://235898457.blog.51cto.com/512763/307612

RD网关负载均衡
http://blog.sina.com.cn/s/blog_643754960101b79n.html