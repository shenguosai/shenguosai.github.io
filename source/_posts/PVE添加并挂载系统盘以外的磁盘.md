---
title: PVE添加并挂载系统盘以外的磁盘
date: 2025-04-11 14:49:21
tags: 虚拟机
categories: 
- [技术兴趣,网络服务]
---
最近买了一个内存 16G，NVME 硬盘 512G 的 N305 的中柏小主机，想用来做家庭服务器。以前折腾黑群晖时使用了 ESXi 6.5，但是过程确实比较费劲，收费而且支持的驱动还少，所以这次准备用 PVE 来实现，开源而且支持硬件丰富。
<!--more-->
然后在装上家里闲置的 SATA SSD 时就遇到了问题，这块 SSD 是以前升级笔记本时的系统盘，因为 PVE 除了系统盘其它硬盘都需要手动挂载，所以这块硬盘在 PVE 管理界面能看到但是却没有办法使用，通过以下步骤操作后就可以用了。
![20250411151716](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20250411151716.png)
注: 图为借用，我原来的硬盘是 256G，安装 win10 共由 3 个分区: sda1、sda2、sda3。
# 首先打开终端工具登录到 PVE 的 shell，这里用的是 Mobaxterm；
```bash
HUAWEI@MB14-SGS ~$ ssh root@192.168.3.91
Warning: Permanently added '192.168.3.91' (ED25519) to the list of known hosts.
root@192.168.3.91's password:
X11 forwarding request failed on channel 0
Linux pve-korea 6.8.12-9-pve #1 SMP PREEMPT_DYNAMIC PMX 6.8.12-9 (2025-03-16T19:18Z) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Fri Apr 11 09:59:13 2025
```
# 确认系统中所有的磁盘分区信息```fdisk -l```；
```bash
root@pve-korea:~# fdisk -l
Disk /dev/sda: 238.47 GiB, 256060514304 bytes, 500118192 sectors
Disk model: ADATA SP600
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0xf923dfaf

Device     Boot     Start       End   Sectors   Size Id Type
/dev/sda1  *         2048    104447    102400    50M  7 HPFS/NTFS/exFAT
/dev/sda2          104448 498943783 498839336 237.9G  7 HPFS/NTFS/exFAT
/dev/sda3       498944000 500113407   1169408   571M 27 Hidden NTFS WinRE


Disk /dev/nvme0n1: 476.94 GiB, 512110190592 bytes, 1000215216 sectors
Disk model: JUMPER 512G
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 000744D7-7F27-4068-BBFA-084F6BC54EC7

Device           Start        End   Sectors   Size Type
/dev/nvme0n1p1      34       2047      2014  1007K BIOS boot
/dev/nvme0n1p2    2048    2099199   2097152     1G EFI System
/dev/nvme0n1p3 2099200 1000215182 998115983 475.9G Linux LVM


Disk /dev/mapper/pve-swap: 8 GiB, 8589934592 bytes, 16777216 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/mapper/pve-root: 96 GiB, 103079215104 bytes, 201326592 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
```
# 使用 fdisk 命令行工具将原有分区删除并建立新的分区；
由上面可以知道，这次需要删除分区的硬盘的是第一个: /dev/sda 的 ADATA SP600，下面开始使用 fdisk 工具删除分区。
启动 fdisk 命令:```fdisk /dev/sda```，sda为将要删除分区的硬盘。
```bash
root@pve-korea:~# fdisk /dev/sda
/dev/sda   /dev/sda1  /dev/sda2  /dev/sda3
root@pve-korea:~# fdisk /dev/sda

Welcome to fdisk (util-linux 2.38.1).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.


Command (m for help): m

Help:

  DOS (MBR)
   a   toggle a bootable flag
   b   edit nested BSD disklabel
   c   toggle the dos compatibility flag

  Generic
   d   delete a partition
   F   list free unpartitioned space
   l   list known partition types
   n   add a new partition
   p   print the partition table
   t   change a partition type
   v   verify the partition table
   i   print information about a partition

  Misc
   m   print this menu
   u   change display/entry units
   x   extra functionality (experts only)

  Script
   I   load disk layout from sfdisk script file
   O   dump disk layout to sfdisk script file

  Save & Exit
   w   write table to disk and exit
   q   quit without saving changes

  Create a new label
   g   create a new empty GPT partition table
   G   create a new empty SGI (IRIX) partition table
   o   create a new empty MBR (DOS) partition table
   s   create a new empty Sun partition table


Command (m for help): d
Partition number (1-3, default 3):

Partition 3 has been deleted.

Command (m for help): d
Partition number (1,2, default 2):

Partition 2 has been deleted.

Command (m for help): d
Selected partition 1
Partition 1 has been deleted.

Command (m for help): d
No partition is defined yet!
```
删到没有分区可删了就是所有分区都删完了，下面开始建立新的分区。fdisk 工具根据版本不同快捷键可能会有差别，一定先按```m```确认。
```bash
Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-500118191, default 2048):
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-500118191, default 500118191):

Created a new partition 1 of type 'Linux' and of size 238.5 GiB.
Partition #1 contains a ntfs signature.

Do you want to remove the signature? [Y]es/[N]o: Y

The signature will be removed by a write command.
```
整个硬盘建立了一个分区并删除了 ntfs signature。
然后确认一下新的分区信息。
```bash
Command (m for help): p
Disk /dev/sda: 238.47 GiB, 256060514304 bytes, 500118192 sectors
Disk model: ADATA SP600
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0xf923dfaf

Device     Boot Start       End   Sectors   Size Id Type
/dev/sda1        2048 500118191 500116144 238.5G 83 Linux

Filesystem/RAID signature on partition 1 will be wiped.
```
确认没有问题后进行写入。
```bash
Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.
```
然后会自动跳出 fdisk。
这时候就能看到硬盘已经合并成一个分区了。
![20250411153422](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20250411153422.png)
# 使用 ext4 类型格式化硬盘分区
使用命令```mkfs -t ext4 /dev/sda1```将硬盘格式化为 ext4。
```bash
root@pve-korea:~# mkfs -t ext4 /dev/sda1
mke2fs 1.47.0 (5-Feb-2023)
Discarding device blocks: done
Creating filesystem with 62514518 4k blocks and 15630336 inodes
Filesystem UUID: 3290e3bf-4a06-41b7-b4fb-266746190f64
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
        4096000, 7962624, 11239424, 20480000, 23887872

Allocating group tables: done
Writing inode tables: done
Creating journal (262144 blocks): done
Writing superblocks and filesystem accounting information: done
```
分区也弄好了，文件系统也弄好了，接下来要将这个分区挂载到系统的文件目录下，linux中有一个目录/mnt专门用来挂载各种U盘，硬盘等：
```bash
mkdir /mnt/sp600 # 创建需要挂载到的文件目录，sp600这个目录名字可以自定义
mount -t ext4 /dev/sda1 /mnt/sp600 # 将sda1分区挂载到/mnt/sp600下
```
设置开机自动挂载:
```bash
echo “mount -t ext4 /dev/sda1 /mnt/sp600” >> /etc/fstab
```
有些教程里会让修改到```/etc/rc.local```中，为防止有些程序用到挂载硬盘中的文件，还是挂载到```/etc/fstab```中保险，因为 fstab 是在所有程序启动之前进行的配置。
# 回到 PVE 控制台添加磁盘
之后就可以回到PVE后台，按照下图中步骤操作，添加刚刚挂载好的磁盘。
![20250411153927](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20250411153927.png)
这里的```内容```选项，表示你可以放什么类型的东西，全选就好。
之后就可以通过PVE管理这个分区了：
![20250411154130](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20250411154130.png)

