---
title: 【转】PassWall2配置
date: 2025-02-17 14:16:54
tags: 网络
categories:
- [技术兴趣,网络服务]
---
PassWall2 能够给我们带来什么？
1. 解决 DNS 泄露
2. IPv4/IPv6 双栈共存
3. 去广告
4. 无缝访问 github、国外论文网站
<!--more-->

# 一、 规则管理
1. 删除默认的所有规则，探后添加 Reject、Direct、Proxy 规则；
![20250217142557](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20250217142557.png)
这里有更好的规则，可以替代 passwall2 自带的:
https://github.com/Loyalsoldier/v2ray-rules-dat
将 geoip 文件链接更改为:
https://github.com/Loyalsoldier/v2ray-rules-dat/releases/latest/download/geoip.dat
将 geosite 文件链接更改为:
https://github.com/Loyalsoldier/v2ray-rules-dat/releases/latest/download/geosite.dat
2. Reject 规则填入域名；
geosite:category-ads-all
![20250217143100](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20250217143100.png)
3. Direct 规则填入；
域名:
xn--ngstr-lra8j.com
geosite:private
geosite:cn
IP：
geoip:private
geoip:cn
![20250217143216](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20250217143216.png)
4. Proxy 规则填入域名；
geosite:geolocation-!cn
![20250217143336](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20250217143336.png)

规则的顺序一定要这样排列:
1、 Reject
2、 Direct
3、 Proxy

**注释:**
geosite:category-ads-all 去广告
xn--ngstr-lra8j.com 谷歌商店
geosite:private 内网地址
geosite:cn 国内域名
geoip:private 内网IP
geoip:cn 国内IP
geosite:geolocation-!cn 国外域名
# 二、 高级设置
1、如果不使用UDP代理，就把UDP不转发端口改成-所有，即可
2、设置TCP转发端口为：仅网页 ，也就是仅允许代理80、443这两个端口
3、TCP代理方式可以改成TPROXY，也可以默认REDIRECT（勾选IPv6透明代理(TProxy)后，会自动变更为TPROXY）
4、如果你的节点支持IPv4/IPv6互通或者有IPv6的出口IP，可以勾选IPv6透明代理(TProxy)，反之不勾选
# 三、 修改节点配置 Xray 分流:[分流总结点]
![20250217144013](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20250217144013.png)
1、Reject设置为：黑洞（丢弃）
2、Direct设置为：直连（绕过）
3、Proxy设置为：默认（代理）
3、*默认设置为：按照自己需求选择一个节点
4、域名解析策略设置为：AsIs （也就是跳过IP匹配、只匹配域名）-- 可能是版本原因，没找到相关设置项
# 四、 基本配置
1. 远程 DNS 协议: DoH
2. 勾选 Fake DNS
![20250217144233](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20250217144233.png)
1. 主要---节点设置为：Xray 分流：[分流总节点]
2. 以上都配置好之后，勾选上主开关保存应用即可

**注释：**
1、远程 DNS 出站一定要设置为：远程（走代理），不然你走直连大概率是不通的，但是 DOH 走代理都是会增加延迟
2、勾选 FakeDNS 后，在 Proxy 规则内的域名，匹配上的会返回一个假 IP 作为 DNS 响应（例如198.18.0.0/16），最后将域名请求发送给节点服务器 VPS 在它的网络环境中进行解析IP
3、不在Proxy规则内的域名则会使用设置的 CloudFlare 远程 DNS 进行本地 DNS 解析真实 IP ，最后将这个真实 IP 发送给节点服务器 VPS ，而如果遇到解析出的是被污染的 IP ，由于开启了嗅探功能，会探测 HTTP 请求里的域名，所以就算本地解析出的 IP 是被污染也不影响
4、如何判断是走 FakeDNS 还是走远程 DNS ，电脑运行 CMD 窗口，输入```nslookup youtube.com```回车，输入```nslookup ipleak.net```回车
上述都配置完成后，就能够解决 DNS 泄露、污染问题和去广告，并且支持双栈 IPv4/IPv6 上网，不需要再去套任何插件和关闭或禁止解析 IPv6 DNS 记录等。
# 五、 Steam 设置
上述的配置完成后，发现 steam 下载走的是代理，需要通过以下操作解决。
1、在 Direct 规则中的域名添加 geosite:category-games，这样可以使 steam、epic 等软件都走直连。
2、为了避免走直连后 steam 社区、商店打不开，需要指定其域名走代理。添加 steam 分流规则，其中域名为```store.steampowered.com```、```steamcommunity.com```。
3、将 steam 规则排在 direct 上方，即最终规则顺序为 reject、steam、direct、proxy。

【转自】[Pass Wall2（完美配置教程）)](https://nanodesu.net/archives/36/)
