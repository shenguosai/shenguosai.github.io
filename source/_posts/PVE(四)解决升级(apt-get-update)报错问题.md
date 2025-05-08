---
title: PVE(四)解决升级(apt-get update)报错问题
date: 2025-05-08 18:10:58
tags: PVE
categories:
- [技术兴趣,网络服务]
---
# 问题原因
使用 Proxmox VE 默认的 APT 更新源，在 WEB 更新管理面板点击“刷新”后，会显示错误。这是因为默认的更新源为 Proxmox VE 企业版的订阅，如果我们没有购买订阅，就会提示签名错误，从而使得 APT 更新失败，解决的方法很简单，就是更换软件源就可以了。 Proxmox 官方提供了对应不同版本的源，可以根据自己的情况进行选择。
# 解决方法
修改两个软件源文件即可。
## /etc/apt/sources.list.d/pve-enterprise.list
注释掉 enterprise 字样的企业软件源，添加免费软件源
```bash
#deb https://enterprise.proxmox.com/debian/pve bookworm pve-enterprise
deb http://download.proxmox.com/debian/pve bookworm pve-no-subscription
```
## /etc/apt/sources.list.d/ceph.list
注释掉仅有的一行即可
```bash
#deb https://enterprise.proxmox.com/debian/ceph-quincy bookworm enterprise
```