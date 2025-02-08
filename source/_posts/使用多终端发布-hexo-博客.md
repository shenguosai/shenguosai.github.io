---
title: 使用多终端发布 hexo 博客
date: 2025-02-08 17:18:16
tags: Hexo
categories:
- [技术兴趣,网络服务]
---
# 分支
master分支: 保存博客静态资源，提供博客页面供人访问。
hexo分支(仓库默认分支): 用于备份博客部署文件，共自己维护更新。
<!--more-->
# 在新设备中建立环境
## 拉取hexo分支
```bash
git clone -b hexo git@github.com:shenguosai/shenguosai.github.io.git
```
执行完上述命令后目录下会出现一个 ```shenguosai.github.io``` 的目录。

## 初始化博客目录

```shenguosai.github.io``` 目录只是一个普通的 git 管理目录，需要把该目录初始化为 Hexo 目录。

```bash
cd shenguosai.github.io
npm install hexo
npm install
npm install hexo-deployer-git
```
可能是以前在其它目录使用的关系，只运行了 ‘npm install hexo’ 之后就可以使用了。
## 安装插件(可省略)
```bash
npm install hexo-generator-searchdb --save # 本地搜索插件
npm install hexo-asset-image --save # 图片插件
npm install hexo-generator-sitemap --save # 谷歌站点地图插件
npm install hexo-generator-baidu-sitemap --save # 百度站点地图插件
```
# 在不同终端中写博客的流程
每次更新博客时不只要更新 master 分支中的静态文件，hexo 分支中的配置文件也需要同步。
```bash
git pull # 每次写博客前拉取最新的 hexo 分支代码
hexo n  '新文章' # 开始写博客
hexo clean && hexo g # 清空并生成新的静态文件和缓存文件
git add .
git commit -m '备注'
git push # 提交到 hexo 分支
hexo d # 提交到 master 分支
```