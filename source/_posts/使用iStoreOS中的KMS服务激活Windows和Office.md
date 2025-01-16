---
title: 使用iStoreOS中的KMS服务激活Windows和Office
date: 2025-01-16 15:28:31
tags: Windows
categories:
- [技术兴趣,网络服务]
---

## 安装 iStoreOS 路由系统
本机环境，在威联通 NAS 的虚拟机中安装了 iStoreOS。
## 安装 KMS 服务插件
在 iStore 中找到 KMS 服务器并点击安装。
![20250116153253](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20250116153253.png)
安装完成后再服务中会出现 “KMS 服务器”，点击进入并将“启用”和“自动激活局域网客户端”勾选。
服务端设置到此完成。
## 端口映射
如果需要在外网使用可以在主路由中进行端口映射，建议使用默认端口"1688"，不然在客户端还要指定端口。
## 激活 Windows 系统
如果以前使用了公共的 KMS 服务器需要替换掉。
1. 打开命令行提示符（管理员权限）
   - 在 Windows系统中，按下 Win+X 键，在弹出菜单中选择“终端管理员”
   - 在 macOS 和 Linux 系统中，打开终端
2. 查看 KMS 服务器信息
   ```slmgr /dlv```
   后会弹出相关信息。（但是我没有找到 KMS 服务器的地址信息，可能是系统版本问题。及时看不到也不影响下面的替换。）
3. 停止 KMS 服务
   ```net stop sppsvc```
4. 删除原 KMS 服务器地址
   ```slmgr /ckms```
5. 添加新的 KMS 服务器地址
   ```slmgr /skms starnest.mycloudnas.com```
6. 激活系统
   ```slmgr /ato```
7. 启动 KMS 服务
   ```net start sppsvc```
其中 3 和 7 根据情况可进行省略。
## 激活 OFFICE
1. 进入 Office 的安装目录
   ```cd 'C:\Program Files\Microsoft Office\Office16'```
2. 执行命令查看 Office 激活状态及 KMS 服务器信息
   ```cscript ospp.vbs /dstatus```
3. 更新 KMS 服务器地址
   ```cscript opss.vbs /sethst:starnest.mycloudnas.com```
4. 激活 OFFICE
   ```cscript opss.vbs /act```