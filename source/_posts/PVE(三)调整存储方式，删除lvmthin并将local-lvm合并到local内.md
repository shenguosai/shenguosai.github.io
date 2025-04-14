---
title: PVE(三)调整存储方式，删除lvmthin并将local-lvm合并到local内
date: 2025-04-12 15:00:12
tags: PVE
categories:
- [技术兴趣,网络服务]
---
# 关于 local 和 local-lvm
查了很多相关资料，到现在还没有彻底的搞明白，根据目前的理解如下:
1. lcoal-lvm 就是网上说的 lvm-thin，lvm-thin 算是 local-lvm 的存储类别，根据网上的说法，它的优点是可以设置比物理空间更大的空间，但这个家用基本上不会用到，它的缺点是缺少文件层的缓存机制，映像了IO效率。
<!--more-->
2. 很多教程会给出下面的文件结构:(由于我的已经删掉了，网上找来的配置)
   ```bash
   root@pve:~# lsblk
   NAME               MAJ:MIN RM    SIZE RO TYPE MOUNTPOINT
   sda                  8:0    0     80G  0 disk 
   ├─sda1               8:1    0   1007K  0 part 
   ├─sda2               8:2    0    512M  0 part 
   └─sda3               8:3    0   79.5G  0 part 
     ├─pve-swap       253:0    0      4G  0 lvm  [SWAP]
     ├─pve-root       253:1    0   19.8G  0 lvm  /
     ├─pve-data_tmeta 253:2    0      1G  0 lvm  
     │ └─pve-data     253:4    0   43.9G  0 lvm  
     └─pve-data_tdata 253:3    0   43.9G  0 lvm  
       └─pve-data     253:4    0   43.9G  0 lvm
   ```
   这里正式搞不明白的地方之一，感觉上 pve-data 就是这个 local-lvm
3. 删除 local-lvm 之后才知道，这俩还是分开的好，因为 local 存储里面有 proxmox 的系统文件，简单说就是 pve 系统是安装在这个目录里面，如果把虚拟机的磁盘什么的都放在这里，当遇到磁盘容量耗尽的情况会导致 pve 系统无法启动。
但我已经删掉了，所以才会有这篇笔记。。。
# 删除 local-lvm
首先笔记 local: 数据中心 => 存储 => local => 点击上面的编辑
![20250412152227](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20250412152227.png)
点选所有内容:
![20250412152324](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20250412152324.png)
使用命令```lvremove /dev/pve/data```删除 lvm-thin。
```bash
root@pve-korea:~# lvremove /dev/pve/data
Do you really want to remove active logical volume pve/data? [y/n]: y
  Logical volume "data" successfully removed.
```
然后使用命令```lvextend -rl +100%FREE /dev/pve/root```将 lvm-thin 的空间转移给 pve-root，即 local。
删除之后磁盘空间编程了这样:
```bash
root@pve-korea:~# lsblk
NAME         MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
sda            8:0    0 238.5G  0 disk
└─sda1         8:1    0 238.5G  0 part /mnt/sp600
nvme0n1      259:0    0 476.9G  0 disk
├─nvme0n1p1  259:1    0  1007K  0 part
├─nvme0n1p2  259:2    0     1G  0 part /boot/efi
└─nvme0n1p3  259:3    0 475.9G  0 part
  ├─pve-swap 252:0    0     8G  0 lvm  [SWAP]
  └─pve-root 252:1    0 467.9G  0 lvm  /
```
# 在 web 管理页面中修改储存配置
在网页中删除 lvm-thin
![20250412152835](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20250412152835.png)
