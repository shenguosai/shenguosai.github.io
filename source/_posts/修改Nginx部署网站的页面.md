---
title: 修改 Nginx 部署网站的页面
author: 小人市井
top: true
date: 2025-11-05 19:52:40
summary:
img:
coverImg:
tags: nginx
categories:
- [技术兴趣,网络服务]
password:
---

前段时间使用 Nginx 部署了反向代理，一直使用 Nginx 的默认主页也不是回事，因为自己用 Hexo 在 github 上部署了博客，所以想着建个网站把自己平时常用的东西整合到一起。
在修改时发现，虽然我设置了 root 在 `/var/www/html` 下，而且使用域名访问其下的 blog 时也能正常访问，但是修改 `/var/www/html/` 目录下的 `index.html` 却怎么也无法更新访问页面。

```html
    location  /blog {
      root  /var/www/html;
      index index.html index.htm;
    }
```

最后发现原来访问域名时 Nginx 展示的网页是 `/usr/share/nginx/html/` 目录下的 `index.html` ，所以就把原来的文件删掉创建了一个软链接：`ln -s /var/www/html/index.html index.html` 这样解决了。

至于原因为何还没有搞明白。