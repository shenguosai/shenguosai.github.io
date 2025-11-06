---
title: Hexo从github克隆到本地的主题库无法上传到自己仓库的thems文件夹
author: 小人市井
top: true
date: 2025-11-06 15:46:24
summary:
img:
coverImg:
tags: Hexo
categories:
- [技术兴趣,网络服务]
password:
---

一直都在使用 Hexo 的 Next 作为个人博客的主题，这两天尝试更换主题遇到两个问题：
1. 更换主题后没有渲染效果
   **现象：**
   除使用 Next，更换为其它主题（landscape，butterfly，Hacker，matery，pure，yilia-plus）都没有渲染效果
   **原因：**
   缺少渲染器插件。
   打开.../themes/*theme title*/layout，发现里面的渲染文件后缀不同。
   |主题|Layout 渲染文件|
   |----|----|----|
   |butterfly|pug|
   |Hacker|ejs|
   |matery|ejs|
   |next|swig|
   |pure|ejs|
   |yilia-plus|ejs|
   而我一直使用的是 Next 主题，所以没有安装 ejs 的渲染器。
   **解决方法：**
   ```bash
   npm isntall hexo renderer-ejs
   ```
  具体哪个插件用来渲染什么模板记不清了，最终安装的插件有：
  ```bash
  npm list

  hexo-renderer-ejs@2.0.0
  hexo-renderer-jade@0.5.0
  hexo-renderer-kramed@0.1.4
  hexo-renderer-markdown-it@7.1.1
  hexo-renderer-stylus@3.0.1
  ```
2. 在 Github 仓库克隆到本地后无法使用 `git push` 上传到自己的仓库
   **现象：**
   使用 `git clone` 命令将 github 中别人仓库的主题现在到本地，然后使用 `git add .` `git commit -m ""` `git push` 上传文件时发现新下载的 hexo-theme-matery 和 butterfly 的两个主题文件夹在仓库里时空的。
   **原因：**
   目前还不清楚原因，网上看到很多建议：
   1) 删除 .git 文件夹，见别人仓库的信息删除；
   2) 使用 `git remote rm origin` 命令切断远程仓库的链接
   但是我使用上述方法均不起作用。
   **解决方法：**
   从暂存区删除文件夹：`git rm --cache ./themes/主题文件名`
   ```bash
   git rm --cashe ./themes/hexo-theme-matery
   git rm --cache ./themes/butterfly
   ```
