---
title: Hexo设置博客文章加密
date: 2023-08-10 13:53:55
tag: Hexo
categories: 
- [技术兴趣,网络服务]
---

# 准备
Hexo搭配使用[hexo-blog-encrypt](https://www.npmjs.com/package/hexo-blog-encrypt)插件可以写一些比较私密的博客，通过密码验证的方式让其他人不能随意浏览。
# 安装插件
在```blog\```目录运行以下代码：
```bash
npm install hexo-blog-encrypt
```
<!--more-->
# 一般配置
1. 在根目录的配置文件```_config.yml```中添加以下代码：
```yml
encrypt:
  enable: true
```
2. 设置加密之后，需要在新建博文时在文章头部添加加密的信息设置：
```config
---
title: {{ title }}
date: {{ date }}
tags: 
categories: 
password: 
message: 
---
```
```password```: 密码。
```message```：输入密码界面的提示说明。

# 针对Tag的加密
将以下代码复制到根目录下的```_config.yml```:
```yml
encrypt: #hexo-blog-encrypt
  enable: true
  abstract: 有东西被加密了, 请输入密码查看.
  message: 您好, 这里需要密码.
  tags:
  - {name: TagName1, password: 密码A}
  - {name: TagName2, password: 密码B}
  wrong_pass_message: 抱歉, 这个密码看着不太对, 请再试试.
  wrong_hash_message: 抱歉, 这个文章不能被校验, 不过您还是能看看解密后的内容.
```
