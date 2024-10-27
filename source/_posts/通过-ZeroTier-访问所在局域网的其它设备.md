---
title: 通过 ZeroTier 访问所在局域网的其它设备
date: 2024-08-15 11:18:14
tags: Zerotier
categories:
- [技术兴趣,网络服务]
---
# 环境
硬件: QNAP TS-464C
系统: iStore OS 虚拟机(Zerotier虚拟局域网网关)
软件: Zerotier
Zerotier 网段: 10.147.18.0/24
局域网网段: 192.168.3.0/24
<!--more-->
# 配置
## 配置 Zerotier 设置
![20240815112603](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240815112603.png)
## 配置 Zerotier 网关
1. 插件设置
   ![20240815112706](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240815112706.png)
2. 配置网关开启内核转发
先 ssh 登录到 iStore OS(Zerotier虚拟局域网网关，执行以下命令:
```bash
echo 'net.ipv4.ip_forward=1' >> /etc/sysctl.conf
sysctl -p
```
## 访问方法
1. 重启一下使用的外网设备的 Zerotier 服务；
2. 跟在局域网内访问相同，使用局域网地址访问即可。
# 后续
1. 网上说需要开启 Zerotier 网关的地址伪装，即```MASQUERADE```；
```bash
iptables -t nat -A POSTROUTING -s 10.10.10.0/24 -j MASQUERADE
```
1. 配置静态路由表；
2. 防火墙放行 ```9933``` 端口。
上述三个步骤本次配置没有使用到，可能是 iStore OS 简化了配置，如果以后有需要使用 Linux 标准系统来使用的需求时再研究。