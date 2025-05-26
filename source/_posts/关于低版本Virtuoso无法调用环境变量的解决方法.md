---
title: 关于低版本Virtuoso无法调用环境变量的解决方法
date: 2025-05-26 11:31:50
tags: Virtuoso
categories:
- [半导体专业,EDA工具]
---
由于在 Virtuoso 中，系统的环境变量没有扩展到工具内部，如果在写 SKILL 脚本时需要调用系统环境变量的方法如下:
1. 作业环境
   Virtuoso 6.18.040
2. 确认系统变量已被定义
   比如: MGC_HOME 这个变量
   首先在终端中确认是否已定义: 
   ```csh
   echo $MGC_HOME
   ```
3. 配置命令
   如果是直接在 CIW 中直接使用:
   ```skill
   load(simplifyFilename("$MGC_HOME/lib/calibre.skl"))
   ```
   如果是在启动时就需要调用，则需要在 .cdsinit 中加入以下两行
   ```skill
   getShellEnvVar("MGC_HOME")
   load(simplifyFilename("$MGC_HOME/lib/calibre.skl"))
   ```
在网上查到也可以通过加载 ```(CCSexpandEnvVars("$MDTOOLS/MDT_MarkCoordinate.il")``` 命令来使用，不过没有验证过。
据说在新的 Virtuoso 版本中已经扩展了系统环境变量，可以直接方便的调用。
另外，如果要在 Virtuoso 中定义新的环境变量，可以使用 ```SetShellEnvVar()``` 函数:
```skill
setShellEnvVar("TOTO=1")
```