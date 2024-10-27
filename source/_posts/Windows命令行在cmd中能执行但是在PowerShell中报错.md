---
title: Windows命令行在cmd中能执行但是在PowerShell中报错
date: 2024-03-26 08:50:58 
tags: Windows
categories:
- [技术兴趣,软件应用]
---

使用ssh远程windows端的时候默认使用cmd，而用powershell可以使用很多linux的命令，像`ls`，`ps`，`cd`什么的(实际上只是映射了一下)。
使用管理员权限在*PowerShell*执行以下命令，使SSH连接Windows时默认使用*Powershell*。
```powershell
New-ItemProperty -Path "HKLM:\SOFTWARE\OpenSSH" -Name DefaultShell -Value "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -PropertyType String -Force
```
<!--more-->
但是今天远程Windows下开启`http-server`服务时却遇到一个奇怪的问题。
> 这个问题也可以描述为: powershell中无法执行命令/脚本

![20240326090059](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240326090059.png)
尝试了一下发现一部分命令是可以执行的，就考虑是不是权限相关的问题，通过多方查证，总结了以下解决办法:
- 运行命令行查看系统权限:
```powershell
Get-ExecutionPolicy -List
```
![20240326090340](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240326090340.png)
- 通过这个命令发现`CurrentUser`的权限是`Undefined`，那么就是权限的问题，尝试以下命令通过增加权限解决
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```
修改后再次执行`Get-ExecutionPolicy -List`查看权限
![20240326090815](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240326090815.png)
再次开启`http-server`:
![20240326090934](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240326090934.png)
成功！