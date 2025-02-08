---
title: Hexo搭建个人博客挂载Github(未完)
date: 2023-08-10 15:43:26
tag: Hexo
categories: 
- [技术兴趣,网络服务]
---
# 一、准备工作
## 1.Git
[Git官网下载](https://git-scm.com/downloads)
下载完成后，双击安装包，选项全部默认，一路Next安装完成。
测试安装成功，输入```git --version```显示版本信息即可。
```bash
shenguosai@LAPTOP-FBGFH99L ~$ git --version
git version 2.37.1
```
<!--more-->
## 2.Node.js
[Node官网下载](https://nodejs.org/zh-cn/)
下载完成后，双击安装包，选项全部默认，一路Next安装完成。
测试安装成功，输入```node -v```显示版本信息即可。
```bash
shenguosai@LAPTOP-FBGFH99L ~$ node -v
v18.15.0
```
## 3.Gitee或GitHub(目前只成功挂载GitHub@2023/8/3)
准备Gitee或GitHub账号，这个是用来托管的，可以不需要自己的服务器和域名就可以拥有自己的博客。
[Gitee官网](https://gitee.com/)
[GitHub官网](https://github.com/)
注册完成后，创建一个仓库，然后就可以将hexo挂载到这个仓库中了。
创建仓库的时候用 *YourName.github.io* 或 *YourNama.gitee.io* ，这样后面托管的域名比较好记。
# 二、博客搭建
## 1.Hexo下载
新建一个文件夹作为博客的顶级目录。
打开cmd，进入到新建的文件夹目录，输入```npm install hexo-cli -g```以安装hexo。
## 2.Hexo初始化
安装完成后，输入```hexo init blog```进行初始化。
注：blog就是<b><font color="red">步骤1</font></b>中新建的目录。
初始化后，文件夹下就会有下方形式的目录结构。
*<font color="grey" size=1 >设置好图床后添加</font>*
![image-20230807153014796](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/image-20230807153014796.png)

## 3.启动博客
使用终端将目录进入到blog下
```
shenguosai@LAPTOP-FBGFH99L /mnt/d/99_Git/blog$ la
total 245
drwxrwxr-x    1 shenguos 197121           0 Aug  2 18:38 .
drwxrwxr-x    1 shenguos 197121           0 Aug  2 15:13 ..
drwxrwxr-x    1 shenguos 197121           0 Aug  3 00:39 .deploy_git
drwxrwxr-x    1 shenguos 197121           0 Aug  2 15:14 .github
-rw-rw-r--    1 shenguos 197121          82 Aug  2 15:14 .gitignore
-rw-rw-r--    1 shenguos 197121           0 Aug  2 15:14 _config.landscape.yml
-rw-rw-r--    1 shenguos 197121        2582 Aug  3 00:01 _config.yml
-rwxrwxr-x    1 shenguos 197121      352551 Aug  3 00:38 db.json
drwxrwxr-x    1 shenguos 197121           0 Aug  2 15:34 node_modules
-rwxrwxr-x    1 shenguos 197121       92731 Aug  2 15:34 package-lock.json
-rw-rw-r--    1 shenguos 197121         655 Aug  2 15:34 package.json
drwxrwxr-x    1 shenguos 197121           0 Aug  3 00:08 public
drwxrwxr-x    1 shenguos 197121           0 Aug  2 15:14 scaffolds
drwxrwxr-x    1 shenguos 197121           0 Aug  2 15:14 source
drwxrwxr-x    1 shenguos 197121           0 Aug  2 23:53 themes
shenguosai@LAPTOP-FBGFH99L /mnt/d/99_Git/blog$ hexo s
INFO  Validating config
INFO  Start processing
INFO  Hexo is running at http://localhost:4000/ . Press Ctrl+C to stop.

```
本地静态网页就启动了，在浏览器输入[http://localhost:4000/](http://localhost:4000/)就能看到如下网页。
*<font color="grey" size=1 >设置好图床后添加</font>*
# 三、挂载到GitHub上
## 1.挂载须知
### a) git配置
```bash
git config --global user.name "shenguosai"  //用户名
git config --global user.email "xxx@xx.com"  //邮箱
```
用户名和邮箱根据注册github的信息自行修改。
### b) SSH密钥
挂载到GitHub上时，为了方便，我们要创建```ssh```密钥，使用ssh连接更为方便的推送。
方法：```ssh-keygen -C "xxxx@xxx.com"```就会生成密钥，Linux用户在```~/.ssh/```中，Windows用户在```C:/Users/xxx/.ssh/```中。
在github上绑定公钥:```Settings->SSH and GPG keys->SSH keys->New SSH key```
将生成的```id_rsa.pub```中的文本内容复制到```Key```框中，点击```Add SSH key```。
*<font color="grey" size=1 >此处说明需要添加图片，设置好图床后添加。</font>*
输入```ssh -T git@github.com```，如果如下所示，出现你的用户名即配置成功。
```bash
shenguosai@LAPTOP-FBGFH99L /mnt/d/99_Git/blog$ ssh -T git@github.com
X11 forwarding request failed on channel 0
Hi shenguosai! You've successfully authenticated, but GitHub does not provide shell access.
```
### c) Token

## 2.开始挂载
打开博客目录下的```.config.yml```(hexo的配置文件)，下拉到最下方。
```
deploy:
  type: git
  repo: https://ghp_xxxxxx@github.com/shenguosai/shenguosai.github.io.git
  branch: master
```
```ghp_xxxxxx```即使上面申请的token。
然后输入```hexo cl```清除缓存，完成代码为```hexo clean```。
清除缓存后输入```hexo g```生成静态网页，然后输入```hexo d```推送到**GitHub**。