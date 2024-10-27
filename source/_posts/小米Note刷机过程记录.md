---
title: 小米Note刷机过程记录
date: 2024-03-04 11:10:07
tags: 小米
categories:
- [技术兴趣,设备及使用]
---
## 1. 背景
查了一下订单，2015年5月回国的时候给爸爸和我买了两台小米Note，应该用了没有多久就出问题了一直闲置在家。上网查了一下回收价只有2元，所以就像改造一下看能不能当个简单的服务器来使用。
## 2. 准备
### 2.1 小米手机助手
近些年一直都没有关注小米，上网一查发现小米官网已经不开放手机助手的下载了，在网上找到这么一个网页:[MiPhoneAssistant 小米手机助手下载合集](https://miuiver.com/mi-phone-assistant/)。
留个底:
由于小米助手官方已经很久没更新了，最近一次更新是 2021-10-28，所以强烈不建议在这之后发布的手机和 MIUI 版本上使用，大概率会有适配兼容问题，备份恢复功能很可能不能按预期工作，它只适合在老机型和 MIUI 老版本上使用！

下面是小米助手官方下载地址，版本由新到旧排列，可按个人喜好或需要选择。
[https://miuirom.xiaomi.com/rom/u1106245679/4.2.1028.10/MiAssistant-4.2.1028.10.zip](https://miuirom.xiaomi.com/rom/u1106245679/4.2.1028.10/MiAssistant-4.2.1028.10.zip)
[https://miuirom.xiaomi.com/rom/u1106245679/3.2.522.32/MiAssistant-3.2.522.32.zip](https://miuirom.xiaomi.com/rom/u1106245679/3.2.522.32/MiAssistant-3.2.522.32.zip)
[https://miuirom.xiaomi.com/rom/u491047765/2.3.0.4071/mipctoolexe-2.3.0.4071.exe](https://miuirom.xiaomi.com/rom/u491047765/2.3.0.4071/mipctoolexe-2.3.0.4071.exe)
[https://miuirom.xiaomi.com/rom/u491047765/2.3.0.3101/mipctoolexe-2.3.0.3101.exe](https://miuirom.xiaomi.com/rom/u491047765/2.3.0.3101/mipctoolexe-2.3.0.3101.exe)
[https://bigota.d.miui.com/MiFlash/MiSetup2.3.0.1031_2717.exe](https://bigota.d.miui.com/MiFlash/MiSetup2.3.0.1031_2717.exe)
[https://bigota.d.miui.com/MiFlash/MiSetup2.2.0.7241_2717.exe](https://bigota.d.miui.com/MiFlash/MiSetup2.2.0.7241_2717.exe)
[https://bigota.d.miui.com/MiFlash/MiSetup2.2.0.6261_2717.exe](https://bigota.d.miui.com/MiFlash/MiSetup2.2.0.6261_2717.exe)
[https://bigota.d.miui.com/MiFlash/MiSetup2.2.0.4091_2717.exe](https://bigota.d.miui.com/MiFlash/MiSetup2.2.0.4091_2717.exe)
[https://bigota.d.miui.com/MiFlash/MiSetup2.2.0.3171_2717.exe](https://bigota.d.miui.com/MiFlash/MiSetup2.2.0.3171_2717.exe)
[https://bigota.d.miui.com/MiFlash/MiSetup2.2.0.2131_2717.exe](https://bigota.d.miui.com/MiFlash/MiSetup2.2.0.2131_2717.exe)
[https://bigota.d.miui.com/MiFlash/MiSetup2.2.0.1301_2717.exe](https://bigota.d.miui.com/MiFlash/MiSetup2.2.0.1301_2717.exe)
[https://bigota.d.miui.com/MiFlash/MiSetup2.1.0.12251_2717.exe](https://bigota.d.miui.com/MiFlash/MiSetup2.1.0.12251_2717.exe)
[https://bigota.d.miui.com/MiFlash/MiSetup2.1.0.11211_2717.exe](https://bigota.d.miui.com/MiFlash/MiSetup2.1.0.11211_2717.exe)

文件 MD5 校验值如下。
```
b9aaa49dd92a828179b4994b1eb82136  MiAssistant-3.2.522.32.zip
7bc76814006a340667dbe72669c66e1f  MiAssistant-4.2.1028.10.zip
b4ebf49b441a3f4997fc1ac4a82486a5  mipctoolexe-2.3.0.3101.exe
d274427550884f389c7e417a13bfd3c1  mipctoolexe-2.3.0.4071.exe
d4580a3adff0db692f7a0420b568499f  MiSetup2.1.0.11211_2717.exe
936fc4c41e247baf21528e13e26e577e  MiSetup2.1.0.12251_2717.exe
8251f9670237d91f22cc70fa8f910086  MiSetup2.2.0.1301_2717.exe
80a68985a0d063d32d7884d008a0a71e  MiSetup2.2.0.2131_2717.exe
98c7c5185ee0cd2f1940fcc8a0f95f99  MiSetup2.2.0.3171_2717.exe
e1073d32737075b51709c9a2b1c0cc99  MiSetup2.2.0.4091_2717.exe
aea5d32a8c4839fc35a2ed4fbb53e70f  MiSetup2.2.0.6261_2717.exe
62d3bd3427038a67ea8772972da1f82f  MiSetup2.2.0.7241_2717.exe
539cd92545f27978e2fe29aae527d4b2  MiSetup2.3.0.1031_2717.exe
```

尝试了一下上面下载地址的前4个还可以下载，但是后面的已经无法打开了，会提示`403 Forbidden`。