---
title: Hexo的环境同步及数据迁移
date: 2024-10-30 11:23:37
tags: Hexo
categories:
- [技术兴趣,网络服务]
---
今年的双 11 添置了零刻的 SER8 mini 小主机，以后就不想背着笔记本来回跑了，于是出现了两台电脑都能写博客的需求。
网上找了很多教程，最后参考了两篇文章完成了迁移：
- [使用hexo，如果换了电脑怎么更新博客？](https://www.zhihu.com/question/21193762)
- [hexo博客同步管理及迁移](https://www.jianshu.com/p/fceaf373d797)
<!--more-->
# 分析目录结构
首先，需要了解 hexo 项目的目录结构，以及各文件夹用来存放哪些文件。下面是我本地 hexo 项目的目录结构。文件夹没有展开，为了和文件区别，在文件夹后加了```/```用以和文件名区分。
```
blog
├──.deploy_git/
├──.github/
├──node_modules/
├──public/
├──scaffolds/
├──source/
├──themes/
├──.gitignore
├──_config.landscape.yml
├──_config.yml
├──db.json
├──package.json
└──package-lock.json
```
![20241030142735](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20241030142735.png)
## ```publi/```和```.deploy_git```
这两个文件夹里的内容是一样的。用来存放由 markdown 文件生成的所有静态文件，猜测```.deploy_git```文件夹里的内容就是在执行```hexo d```命令时唯一会部署到 github 仓库```username.github.io```的内容。换句话说，github pages 托管的只是由 hexo 生成的静态文件，hexo 的环境文件是只保留在本地的，而这也正是想要在另一台电脑上重新部署和保持同步 hexo 博客需要折腾一番的原因。
## ```source/```文件夹
所有发布的文章的 md 文件都放在这里。
## ```themes/```文件夹
主题的相关配置文件以及样式的修改等都在这里。
## ```.gitignore```文件
git 提交时将会忽略的项，文件内容如下：
```
.DS_Store
Thumbs.db
db.json
*.log
node_modules/
public/
.deploy*/
_multiconfig.yml
```
其实从这也可以看出 hexo 的本意也是让我们可以把环境文件放在 github 上托管的，并且帮我们列好了忽略的项，比如```public/```、```.deploy_git/```文件夹都是被忽略的，是不会被提交的。这样，我们在```hexo g -d```时部署到 github上的只有静态文件，而提交环境文件时则不会把静态文件也提交，节省了空间。

# 把环境文件托管到 github
在 hexo 项目根目录下执行命令```git status```，会得到```fatal: not a git repository (or any of the parent directories): .git``` 的提示：当前项目不是一个 git 仓库。我之前以为既然静态文件可以被提交到 github，那 hexo 项目肯定是一个 git 仓库，原来不是，静态文件能被推送到远程仓库 hexo 引擎在背后是怎么实现的我不知道，也不在本文的讨论范围内。当前的任务是把当前项目根一个远程仓库关联起来，好管理环境文件。
在```username.github.io```仓库中，目前默认的认知是 master，用来存放静态文件。目前网上普遍的做法是新建一个分支 hexo，用来存放环境文件。
![20241030145016](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20241030145016.png)
## 托管流程
1. 在 username.github.io 仓库中新建 hexo 分支（后面用来托管环境文件）；
2. 使用```git clone```命令将新建的 hexo 分支复制到本地，并使用本地的环境文件替换```git clone```复制过来的内容（要保留复制过来根目录下的```.git/```文件夹，但要把本体各主题文件夹内的```.git/```文件夹删除。）；
3. 将本地的 hexo 分支内容提交到 github 仓库中；
   ```bash
   git add .
   git commit -m "add branch"  # ""内的内容可随意，本次上传的说明
   git push
   ```

## 在 username.github.io 仓库中建立新分支 hexo
### github 中新建 hexo 分支
进入 username.github.io 仓库，点击分支处并填入想建立的分支名，不可以和已有分支名相同，因为查找和创建分支都在这个地方执行。
![20241030145535](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20241030145535.png)
点击```Create branch test from master```，就成功建立了 hexo 的分支并且其内容与 master 分支的内容完全相同。
### 将默认分支更改为 hexo ，便于以后上传
![20241030161131](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20241030161131.png)
![20241030161220](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20241030161220.png)
### 将环境文件同步到 hexo 分支
1. 将新建的 hexo 分支下载到本地；
   ```bash
   git clone git@github.com:shenguosai/shenguosai.github.io.git
   ```
   ![20241030160431](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20241030160431.png)
2. 进入克隆到本地的 shenguosai.github.io 中，把除了```.git/```文件夹外的所有文件删掉；
3. 把之前我们写的博客源文件全部复制过来，除了.deploy_git。这里应该说一句，复制过来的源文件应该有一个.gitignore，用来忽略一些不需要的文件，如果没有的话，自己新建一个，在里面写上如下，表示这些类型文件不需要git：
   ```
   .DS_Store
   Thumbs.db
   db.json
   *.log
   node_modules/
   public/
   .deploy*/
   _multiconfig.yml
   ```
*注意，如果你之前克隆过theme中的主题文件，那么应该把主题文件中的.git文件夹删掉，因为git不能嵌套上传，最好是显示隐藏文件，检查一下有没有，否则上传的时候会出错，导致你的主题文件无法上传，这样你的配置在别的电脑上就用不了了。*
4. 提交更新后的内容到 hexo 分支
   ```
   git add .
   git commit –m "add branch"
   git push 
   ```
这样就上传完了，可以去你的github上看一看hexo分支有没有上传上去，其中node_modules、public、db.json已经被忽略掉了，没有关系，不需要上传的，因为在别的电脑上需要重新输入命令安装 。
![20241030161916](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20241030161916.png)
### 更换电脑的操作
- 安装 git：主要有两种
  - [git-scm](https://git-scm.com/downloads)：git 官方版本，纯粹的 git 体验
  - [git for windows](https://gitforwindows.org/)：微软官方提供的 git 版本，基于 msysGit 构建，集成了对于 windows 的优化，并有图形化界面。
  - 设置 git 全局邮箱名和用户名
      ```
      git config --global user.name "your github username"
      git config --global user.email "your github e-mail"
      ```
      上面两个设置完后会在 home 文件夹生成配置文件。（windows 系统路径：C:\Users\SGS-SER8\.gitconfig）
- 设置 ssh key
  - 可以重新生成：```ssh-keygen -t rsa -C "your e-mail"，生成后要将公钥上传到 github 中；
  - 也可以使用原来生成的。（windows 系统路径：C:\Users\SGS-SER8\.ssh\id_rsa）
- 安装 [nodejs](https://nodejs.org/en/)
- 安装 cnpm ：```npm install -g cnpm --registry==https://registry.npm.taobao.org```
```
PS C:\Users\SGS-SER8\Documents\git\shenguosai.github.io> npm install -g cnpm --registry==https://registry.npm.taobao.org
npm : 无法加载文件 C:\Program Files\nodejs\npm.ps1，因为在此系统上禁止运行脚本。有关详细信息，请参阅 https:/go.microsof
t.com/fwlink/?LinkID=135170 中的 about_Execution_Policies。
所在位置 行:1 字符: 1
+ npm install -g cnpm --registry==https://registry.npm.taobao.org
+ ~~~
    + CategoryInfo          : SecurityError: (:) []，PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess
```
要解决上面的问题，需要执行下面的操作。
```cmd
PS C:\WINDOWS\system32> set-ExecutionPolicy RemoteSighed
Set-ExecutionPolicy : 无法绑定参数“ExecutionPolicy”。无法将值“RemoteSighed”转换为类型“Microsoft.PowerShell.Executi
onPolicy”。错误:“无法将标识符名称 RemoteSighed 与有效的枚举器名称相匹配。请指定以下枚举器名称之一，然后重试:
Unrestricted, RemoteSigned, AllSigned, Restricted, Default, Bypass, Undefined”
所在位置 行:1 字符: 21
+ set-ExecutionPolicy RemoteSighed
+                     ~~~~~~~~~~~~
    + CategoryInfo          : InvalidArgument: (:) [Set-ExecutionPolicy]，ParameterBindingException
    + FullyQualifiedErrorId : CannotConvertArgumentNoMessage,Microsoft.PowerShell.Commands.SetExecutionPolicyCommand
```
执行上面的操作后又出现错误，通过下面的方法来解决。
```cmd
PS C:\WINDOWS\system32> Set-ExecutionPolicy -Scope CurrentUser

位于命令管道位置 1 的 cmdlet Set-ExecutionPolicy
请为以下参数提供值:
ExecutionPolicy: RemoteSigned

执行策略更改
执行策略可帮助你防止执行不信任的脚本。更改执行策略可能会产生安全风险，如 https:/go.microsoft.com/fwlink/?LinkID=135170
中的 about_Execution_Policies 帮助主题所述。是否要更改执行策略?
[Y] 是(Y)  [A] 全是(A)  [N] 否(N)  [L] 全否(L)  [S] 暂停(S)  [?] 帮助 (默认值为“N”): A
PS C:\WINDOWS\system32> get-ExecutionPolicy
RemoteSigned
PS C:\WINDOWS\system32> set-ExecutionPolicy RemoteSighed
Set-ExecutionPolicy : 无法绑定参数“ExecutionPolicy”。无法将值“RemoteSighed”转换为类型“Microsoft.PowerShell.ExecutionPolicy”。
错误:“无法将标识符名称 RemoteSighed 与有效的枚举器名称相匹配。请指定以下枚举器名称之一，然后重试:
Unrestricted, RemoteSigned, AllSigned, Restricted, Default, Bypass, Undefined”
所在位置 行:1 字符: 21
+ set-ExecutionPolicy RemoteSighed
+                     ~~~~~~~~~~~~
    + CategoryInfo          : InvalidArgument: (:) [Set-ExecutionPolicy]，ParameterBindingException
    + FullyQualifiedErrorId : CannotConvertArgumentNoMessage,Microsoft.PowerShell.Commands.SetExecutionPolicyCommand

PS C:\WINDOWS\system32> set-ExecutionPolicy RemoteSigned

执行策略更改
执行策略可帮助你防止执行不信任的脚本。更改执行策略可能会产生安全风险，如 https:/go.microsoft.com/fwlink/?LinkID=135170 中的
about_Execution_Policies 帮助主题所述。是否要更改执行策略?
[Y] 是(Y)  [A] 全是(A)  [N] 否(N)  [L] 全否(L)  [S] 暂停(S)  [?] 帮助 (默认值为“N”): A
```
然后再执行```npm install -g cnpm --registry==https://registry.npm.taobao.org```就可以正常进行了。
- 安装 hexo ：cnpm install -g hexo-cli
  - 下载 hexo 环境文件：```git clone git@……```（注意使用 ssh 地址）
  - 进入到下载到本地的仓库目录下安装 hexo 组件：
    ```
    cd xxx.github.io
    npm install
    npm install hexo-deployer-git --save
    ```
