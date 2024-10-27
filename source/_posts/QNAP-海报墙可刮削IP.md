---
title: QNAP-海报墙可刮削IP
date: 2024-06-12 15:25:21
tags: QNAP
categories:
- [技术兴趣,网络服务]
---
目前解决海报墙不可刮削问题的方法基本都是修改 hosts，因为墙屏蔽的是特殊 IP，而不是 themovie 等网站本身。
1. 修改 QNAP 的 hosts 文件:
   这种方法比较简单方便，缺点就是每次威联通重启就会重置 hosts 文件。
   `sudo vi /etc/hosts`
   在 hosts 文件中指定域名的 IP 地址: `13.226.238.82 api.themoviedb.org`
2. 修改路由器的 hosts 文件:
   这种方法一劳永逸，但是需要能够 ssh 路由器并取得 root 权限。
   操作方法及添加内容与 1. 相同。

在网上看到一个刮削网站的 IP 清单，将测试通过的记录如下:(摘自:[NAS干货分享 篇一：一招解决影片削挂问题，打造完美海报墙](https://post.smzdm.com/p/az6x9m00/))
```hosts
13.224.161.90 api.themoviedb.org
104.16.61.155 image.themoviedb.org
13.35.67.86 api.themoviedb.org
54.192.151.79 www.themoviedb.org
13.249.175.212 api.thetvdb.com
13.35.161.120 api.thetvdb.com
13.226.238.76 api.themoviedb.org
13.35.7.102 api.themoviedb.org
13.226.191.85 api.themoviedb.org
52.85.79.89 api.themoviedb.org
13.225.41.40 api.themoviedb.org
13.226.251.88 api.themoviedb.org
```

