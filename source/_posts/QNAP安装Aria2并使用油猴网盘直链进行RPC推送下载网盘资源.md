---
title: QNAP安装Aria2并使用油猴网盘直链进行RPC推送下载网盘资源
date: 2024-09-03 10:37:18
tags: [QNAP,Aria2]
categories:
- [技术兴趣,网络服务]
---
# QNAP 安装 Aria2
## QNAP 型号
TS-464C
<!--more-->
## 加入第三方资源库
1. AppCenter => 设置 => 程序来源 => 添加
2. 输入名称(MyQNAP)和URL(https://www.myqnap.org/repo.xml)
3. 点击确定即可完成
![20240903104658](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240903104658.png)
## 下载并安装 Aria2
1. 在 AppCenter 中点击 MyQNAP => 所有应用 => 下载
2. 找到 Aria2 1.37 并点击下载
![20240903104931](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240903104931.png)
# 配置并使用 Aria2
## Aria2 命令行下载常用命令及选项
下载并安装完成后就可以使用命令行进行下载了，常用命令如下。
### 命令
- ```aria2c url```: 下载文件，url代表文件下载链接
- ```aria2c -S file.torrent```: 显示 file.torrent 文件的信息
- ```aria2c –stop```: 停止所有下载
- ```aria2c –pause```: 暂停所有下载
- ```aria2c –resume```: 恢复暂停的下载
- ```aria2c –version```: 显示版本信息
### 选项
- ```-x n```: 最大使用 n 个线程下载文件
- ```-s n```: 使用 n 个连接数进行下载
- ```-o filename```: 指定下载文件的文件名
- ```-d directory```: 指定文件的下载目录
- ```-c```: 断点续传
- ```-m n```: 最大下载连接数为 n
- ```–max-download-limit=100K```: 限制下载速度为 100K/S

以上是 Aria2 的常用命令和选项。你可以在终端中输入“man aria2c”命令来查看完整的命令和选项列表。

### 示例
- 直接下载
  ```aria2c http://dl_dir.qq.com/qqfile/qq/QQ2011/QQ2011.exe```
- 后台下载
  ```aria2c -D url```
  ```aria2c –deamon=true url```
- 重命名下载
  ```aria2c --out=QQ http://dl_dir.qq.com/qqfile/qq/QQ2011/QQ2011.exe```
  ```aria2c -o QQ http://dl_dir.qq.com/qqfile/qq/QQ2011/QQ2011.exe```
- 分段下载
  ```aria2c -x 2 http://dl_dir.qq.com/qqfile/qq/QQ2011/QQ2011.exe```
- 配合使用
  ```aria2c -s 2 -x 2 -j 10 http://dl_dir.qq.com/qqfile/qq/QQ2011/QQ2011.exe```
  -s这个参数的意思是使用几个线程进行下载，-x是最大使用几个线程下载，-j就是同时下载几个文件。
- 断点续传
  ```aria2c -c http://dl_dir.qq.com/qqfile/qq/QQ2011/QQ2011.exe```
- 验证文件正确性
  ````aria2c -c -x16 -s20 -j20 --checksum=md5=xxxxxxxxxxxxx http://dl_dir.qq.com/qqfile/qq/QQ2011/QQ2011.exe```
- bt 下载
  ```aria2c ‘path/xxx.torrnet‘```
- 磁力链下载
  - 列出种子内容
    ```aria2c -S target.torrent```
  - 下载种子内编号为 1、4、5、6、7 的文件
    ```aria2c --select-file=1,4-7 target.torrent```
  - 设置 bt 端口
    ```aria2c --listen-port=51413 ‘xxx.torrent’```
  - 设置 dht 端口
    ```aria2c --dht-listen-port=51413 ‘xxx.torrent’```
## Aria2 配置文件
### 配置文件路径
不同的系统可能会有所不同，按上述方法安装的 Aria2 的配置文件路径为: ```/opt/AriaII/aria2.conf```
### 配置文件详解

```bash
##此部分主要分为几部分##
#1、文件保存
#2、下载链接
#3、进度保存
#4、RPC相关
#5、BT\PT下载相关

##===================================##
## 文件保存相关 ##
##===================================##

# 文件保存目录
dir=../download
# 启用磁盘缓存, 0为禁用缓存, 需1.16以上版本, 默认:16M
disk-cache=16M
# 断点续传
continue=true
#日志保存
log=aria2.log

# 文件预分配方式, 能有效降低磁盘碎片, 默认:prealloc
# 预分配所需时间: none < falloc ? trunc < prealloc
# falloc和trunc则需要文件系统和内核支持
# NTFS建议使用falloc, EXT3/4建议trunc, MAC 下需要注释此项
file-allocation=prealloc

##===================================##
## 下载连接相关 ##
##===================================##

# 最大同时下载任务数, 运行时可修改, 默认:5
max-concurrent-downloads=100
# 同一服务器连接数, 添加时可指定, 默认:1
# 官方的aria2最高设置为16, 如果需要设置任意数值请重新编译aria2
max-connection-per-server=16

# 整体下载速度限制, 运行时可修改, 默认:0（不限制）
max-overall-download-limit=0
# 单个任务下载速度限制, 默认:0（不限制）
max-download-limit=0
# 整体上传速度限制, 运行时可修改, 默认:0（不限制）
max-overall-upload-limit=0
# 单个任务上传速度限制, 默认:0（不限制）
max-upload-limit=0

# 禁用IPv6, 默认:false
disable-ipv6=false

# 最小文件分片大小, 添加时可指定, 取值范围1M -1024M, 默认:20M
# 假定size=10M, 文件为20MiB 则使用两个来源下载; 文件为15MiB 则使用一个来源下载
min-split-size=10M

# 单个任务最大线程数, 添加时可指定, 默认:5
# 建议同max-connection-per-server设置为相同值
split=16

##===================================##
## 进度保存相关 ##
##===================================##

# 从会话文件中读取下载任务
input-file=aria2.session
# 在Aria2退出时保存错误的、未完成的下载任务到会话文件
save-session=aria2.session
# 定时保存会话, 0为退出时才保存, 需1.16.1以上版本, 默认:0
save-session-interval=60

##===================================##
## RPC相关设置 ##
##此部分必须启用，否则无法使用WebUI
##===================================##

# 启用RPC, 默认:false
enable-rpc=true
# 允许所有来源, 默认:false
rpc-allow-origin-all=true
# 允许外部访问, 默认:false
rpc-listen-all=true
# RPC端口, 仅当默认端口被占用时修改

rpc-listen-port=6800
# 设置的RPC授权令牌, v1.18.4新增功能, 取代 --rpc-user 和 --rpc-passwd 选项
rpc-secret=000789456

# 设置的RPC访问用户名, 此选项新版已废弃, 建议改用 --rpc-secret 选项
#rpc-user=<USER>
# 设置的RPC访问密码, 此选项新版已废弃, 建议改用 --rpc-secret 选项
#rpc-passwd=<PASSWD>

# 启动SSL
# rpc-secure=true
# 证书文件, 如果启用SSL则需要配置证书文件, 例如用https连接aria2
# rpc-certificate=
# rpc-private-key=

##===================================##
## BT/PT下载相关 ##
##===================================##

# 当下载的是一个种子(以.torrent结尾)时, 自动开始BT任务, 默认:true
follow-torrent=true
# BT监听端口, 当端口被屏蔽时使用, 默认:6881-6999
listen-port=51413
# 单个种子最大连接数, 默认:55
#bt-max-peers=55
# 打开DHT功能, PT需要禁用, 默认:true
enable-dht=true
# 打开IPv6 DHT功能, PT需要禁用
enable-dht6=true
# DHT网络监听端口, 默认:6881-6999
dht-listen-port=6881-6999

# 本地节点查找, PT需要禁用, 默认:false
bt-enable-lpd=true
# 种子交换, PT需要禁用, 默认:true
enable-peer-exchange=true
# 每个种子限速, 对少种的PT很有用, 默认:50K
bt-request-peer-speed-limit=50K

# 客户端伪装, PT需要
peer-id-prefix=-TR2770-
user-agent=Transmission/2.77

# 当种子的分享率达到这个数时, 自动停止做种, 0为一直做种, 默认:1.0
seed-ratio=0
# 强制保存会话, 即使任务已经完成, 默认:false
# 较新的版本开启后会在任务完成后依然保留.aria2文件
force-save=true
# BT校验相关, 默认:true
#bt-hash-check-seed=true
# 继续之前的BT任务时, 无需再次校验, 默认:false
bt-seed-unverified=true
# 保存磁力链接元数据为种子文件(.torrent文件), 默认:false
bt-save-metadata=true
# 单个种子最大连接数, 默认:55 0表示不限制
bt-max-peers=0
# 最小做种时间, 单位:分
# seed-time = 60
# 分离做种任务
bt-detach-seed-only=true
#BT Tracker List ;下载地址：https://github.com/ngosang/trackerslist
bt-tracker=udp://tracker.coppersurfer.tk:6969/announce,udp://tracker.internetwarriors.net:1337/announce,udp://tracker.opentrackr.org:1337/announce
```

### 我的配置文件
```bash
max-concurrent-downloads=100
max-connection-per-server=16
split=16
min-split-size=10M
enable-rpc=true
rpc-listen-all=true
rpc-allow-origin-all=true
# 上面这行非常重要，默认为 false，如果不设置为 True 则只能从特定的 IP 或域名访问，一开始就是因为没有这句在 WebUI AriaNg 里面怎么设置都显示未连接。只有协议设置为websocket时才能连接，但是使用 WebUI 也无法下载到正确的文件。加上这句后 http 协议就可以连接并且进行 RPC 推送了。
rpc-secret=7ixxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxrIZ
```
# WebUI(Aginx)
1. QNAP 中开启网站服务
   控制台 => 应用程序 => Web 服务器 => 勾选"启用Web服务器"
   ![20240903151604](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240903151604.png)
   设置默认即可。
   开启了网站服务之后，QNAP 的共享文件夹中就会多出一个 "Web" 的文件夹。
   ![20240903152350](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240903152350.png)
2. 配置网站服务的虚拟机
   虚拟主机 => 勾选"启用虚拟主机" => 点击"创建虚拟机"
   主机名称: 可以自定义，这里使用 Aria2
   根目录: 点击右部倒三角选择网页所在目录
   协议: 选择 HTTP 即可
   端口号: WebUI 的访问端口，注意与 Aria2 RPC 端口不同，要另外使用空闲端口，这里使用 6801。
   ![20240903152044](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240903152044.png)
   这里单独说一下"根目录"，在按上述方法安装了 QNAP 之后，Arial 会被建立一个软链接 ```/opt/AriaII```，其真实路径为```/share/CACHEDEV1_DATA/.qpkg/AriaII/```，在这个目录里面有一个 "www" 的文件夹，这就是 AriaNg 的网站文件，我开始时将它移动到了 "Web" 文件夹里，配置好之后就不想去动了。其实完全可以在虚拟机的"根目录"里直接指定这里的路径，即```/opt/AriaII/wwww```，这样指定的话我估计就可以直接使用安装的 App 的"打开"按钮(其访问地址为:http://192.168.3.74/AriaII)来进行访问。可能还需要一些配置，因为对建站不是很熟所以不确定，也没有尝试。
3. 配置 AriaNg
   配置完网站服务后就可以使用在浏览器中打开控制页面了。(访问地址:http://192.168.3.74:6801)
   ![20240903153346](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240903153346.png)
   但是现在打开后"Aria2 状态"会显示红色的"未连接"。要进行一些配置来连接 Aria2 服务。
   首先，点击 "AriaNg 设置" => 选择全局右边的 RPC
   ![20240903153732](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240903153732.png)
   Aria2 RPC 别名: 可自定义，也可以空着自己会变成"地址:端口"
   Aria2 RPC 地址: 安装 Aria2 服务的主机地址或IP
   Aria2 协议: 选择"Http" 
   Aria2 RPC Http 请求方法: POST
   Aria2 RPC 请求头:这个不了解是什么，留空不影响使用
   Aria2 RPC 秘钥: 这个秘钥在 Aria2 配置文件 ```/opt/AriaII/aria2.conf``` 中的 ```rpc-secret=``` 行中，可自定义设置，然后点击"重新加载 AriaNg"即可。
   这里再重复一遍，要在配置文件```/opt/AriaII/aria2.conf```中加入```rpc-allow-origin-all=true```才可以在任何主机访问，我被这个地方卡了很长时间。
4. 使用油猴脚本获取网盘资源直链
   - 首先要安装一个浏览器的油猴脚本管理插件，篡改猴或者暴力猴什么的都可以。我使用的是[篡改猴](https://microsoftedge.microsoft.com/addons/detail/%E7%AF%A1%E6%94%B9%E7%8C%B4/iikmkjmpaadaobahmlepeloendndfphd?hl=zh-CN)。在[Edge插件商店](https://microsoftedge.microsoft.com/addons/Microsoft-Edge-Extensions-Home?hl=zh-CN)中直接搜索"tampermonkey"，前三个应该都可以使用。
   - 然后，到[Greasyfork](https://greasyfork.org/zh-CN)中下载脚本，我选择的是[网盘直链下载助手](https://greasyfork.org/zh-CN/scripts/436446-%E7%BD%91%E7%9B%98%E7%9B%B4%E9%93%BE%E4%B8%8B%E8%BD%BD%E5%8A%A9%E6%89%8B)
   - 再打开网盘(这里以夸克网盘为例)的资源页就会开到多出一个"下载助手"的按钮。
    ![20240903155210](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240903155210.png)
   点击按钮或者查看脚本说明就会知道:
   API 下载: IDM 或浏览器自带下载。
   Aria 下载: 这个可以得到资源链接使用命令行或者 Aria2 工具进行下载。
   RPC 下载: 这个就是使用 RPC 远程推送下载链接进行下载，设置方法见"助手设置"。
   cURL 下载: 使用终端命令行工具```curl```下载时的链接，直链地址与 Aria 下载的是一样的，只是前面命令变成了```curl```，而不是```aria2c```。
   BC 下载: 这个使用比特彗星(BitCommet)下载的链接，没有用过。
   助手设置: 这里就是 RPC 推送的设置选项了，与 AriaNg 上的设置基本一致，格式略有差别。
   ![20240903155907](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240903155907.png)




