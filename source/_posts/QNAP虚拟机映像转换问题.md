---
title: QNAP虚拟机映像转换问题
date: 2024-08-12 09:05:48
tags: QNAP
categories: 
- [技术兴趣,网络服务]
---
QNAP 的虚拟机必须要使用自己转换出的```.img```，下面记录一下自己安装虚拟机的映像转换过程。
<!--more-->
# IMG文件
QNAP 上安装了iStoreOS 系统的旁路由虚拟机，先使用 [StarWind V2V Convertor](https://www.starwindsoftware.com/starwind-v2v-converter#download) 软件将```IMG```文件转换为 QNAP Virtual Station 中可以使用的 ```.vdi``` 或 ```.vdmk``` 文件，然后使用 QNAP Vistual Station 内置的转换工具进行转换，转换完就是其可以使用的 ```.img``` 文件了。
> 这个系统已安装成功！

# ISO文件
## 方法一
```.iso``` 文件网上的信息很少，唯一有点用处的如下[将ISO镜像转换成VDI、VDH和VMDK格式](https://blog.csdn.net/LiniLing/article/details/119864889):
1. 首先，要先下载两个软件: [VMWare](https://downloads2.broadcom.com/?file=VMware-workstation-full-17.5.2-23775571.exe&oid=30581048&id=sQ9ZBRmMMB_QK60kfaL-tcNlwWCDromgou84_F7-Mkxcey9zd6BIhyhHVbcjvGfqYxM=&verify=1722998592-oxwenmxozDwNlEOLBkXvPXCUcQOw2EOmWEej20K6%2FeU%3D) 和 [VirtualBox](https://download.virtualbox.org/virtualbox/7.0.20/VirtualBox-7.0.20-163906-Win.exe)
2. 在 VMWare 中使用```.iso```映像安装好虚拟机；
3. 选中安装好的虚拟机，文件=>导出为OVF，然后会生成 4 个文件；
   ![20240812092446](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240812092446.png)
4. 打开 VirtualBox 软件，
5. 
上述方法由于还没有实践过，未验证成功，等验证成功后再补充完整步骤。
## 方法二
**.iso**的文件大概介绍如下：iso格式，ISO是一种光盘镜像文件，无法直接使用，需要利用一些工具进行解压后才能使用。ISO一般都是将光盘文件做成一个文件，而有一些光盘软件设定只能从光驱进行安装，那么直接解压后还是不能使用，需要用到虚拟光驱软件（虚拟光驱的软件很多，Daemon Tools是一款不错的虚拟光驱软件）
**.img**文件大概介绍如下：img格式，IMG是一种文件压缩格式（archive format），.IMG这个文件格式可视为.ISO格式的一种超集合。由于.ISO只能压缩使用ISO9660和UDF这两种文件系统的存储媒介，意即.ISO只能拿来压缩CD或DVD，因此才发展出了.IMG，它是以.ISO格式为基础另外新增可压缩使用其它文件系统的存储媒介的能力，.IMG可向后兼容于.ISO，如果是拿来压缩CD或DVD，则使用.IMG和.ISO这两种格式所压缩出来的内容是一样的。
从上面的介绍，可以看出.img是.iso的一种超集合，两者解压得到的东西是一样的，所以将一个.iso转化为.img，可以直接将.iso重命名为.img即可。
重命名为```.img```之后确实可以通过 StarWind V2V Convertor 软件转化为 ```.vmdk``` 文件了，并且也能够通过 QNAP Virtual Station 的转换工具转换为其可以使用的 ```.img``` 文件，也能开始引导安装，但是安装(Debian 12.6 和 Ubuntu 18.04)到磁盘分区的时候出现错误无法识别到硬盘，可能是设置哪里有问题，后续尝试成功后补充。