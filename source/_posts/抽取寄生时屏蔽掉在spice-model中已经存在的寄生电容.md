---
title: 抽取寄生时屏蔽掉在spice model中已经存在的寄生电容
date: 2023-12-03 22:14:43
tags: Calibre
categories:
- [半导体专业,EDA工具]
---
在提取寄生参数时，除了需要关注金属导体的寄生电容外，还需要分析mos器件的节点寄生参数提取是否准确。
下面是一个典型的 nmos 4端器件:


该mos管的4个节点 D, G, S, B 的寄生电容是否准确呢？
首先用calibre xrc提取一下其寄生参数，结果如下:
```
cc_1 S G 3.32511f
cc_2 S D 6.73464e-19
cc_3 S B 12.7622f
cc_4 G D 3.32511f
cc_5 G B 1.16212f
cc_6 D B 12.7506f
```
<!--more-->
上述寄生电容分为4类:
* Gate 到 Source/Drain 电容
* Source/Drain 到 Bulk 电容
* Gate 到 Bulk 电容
* Source 到 Drain 电容

由于在spice model中一般会自动把上述4类电容考虑，如果寄生参数提取中再提取一遍，就会发生重复提取。
因此，需要把这4类电容屏蔽，屏蔽方法如下:
* 屏蔽 Gate 到 Source/Drain 电容
```
PEX IGNORE CAPACITANCE ALL poly nsd
PEX IGNORE CAPACITANCE ALL poly psd
```
* 屏蔽 Gate 到 Bulk 电容，其中 ALLGATE 是 mark layer
```
PEX IGNORE CAPATANCE DEVICE INTRINSIC poly ALLGATE
```

设置上述3个参数后，提取结果如下:
```
cc_1 S D 0.00129446f
cc_2 S B 12.7622f
cc_3 G B 0.211916f
cc_4 D B 12.7506f
```
可以看到，Gate 到 Source/Drain 的电容没有了，被屏蔽掉了。Gate 到 Bulk 的电容比刚才的值小了，说明在mos管区域的电容被屏蔽了，只剩下 Gate 露头的电容了，这个符合预期。
但是，Source/Drain 到 Bulk 的电容没有屏蔽掉，该如何设置呢？
首先看一下再原始的工艺文件定义中，有如下语句:
```
diffusion = diff {
    thickness = 0.004
    min_width = 0.15
    min_spacing = 0.18
    resistivity = 10
    src_drn_layers = {nsd_psd_}
}
```
注意 diffusion, src_drn_layers是关键字，nsd_psd_ 是两个 process layer。
calibre命令文件中原来有如下语句:
```
PEX MAP diff NTAP PTAP nsd psd
```
它把 nsd, psd 映射到了diff层，但是没有告诉工具映射到 nsd_psd_层。
如果修改该语句如下:
```
PEX MAP diff NTAP PTAP
PEX MAP nsd_nsd_
PEX MAP psd_psd_
```
当calibre xrc识别到该语句后，自动会把nsd之间的相互电容屏蔽掉，也自动会把 nsd/psd 到衬底的电容屏蔽掉。
屏蔽后提取结果如下:
```
cc_1 S B 0.0777512f
cc_2 G B 0.211916f
cc_3 D B 0.0540989f
```
以上3个电容都是连接到mos管的金属之间的耦合电容了，电容值很小，不是器件本身的电容，符合预期。

-------------------

如果用户在原始的工艺定义中没有用 diffusion 的关键字，而是用 conductor 的关键字，如下:
```
conductor = diff {
    thickness = 0.004
    min_width = 0.15
    min_spacing = 0.18
    resistivity = 10
}
```
则用户需要增加额外的语句来屏蔽 source 到 drain，source 到衬底的寄生电容。
* 屏蔽 Gate 到 Source/Drain 的电容
```
PEX IGNORE CAPATANCE ALL poly nsd
PEX IGNORE CAPATANCE ALL poly psd
```
* 屏蔽 Gate 到 Bulk
```
PEX IGNORE CAPACITANCE DEVICE INTRINSIC poly ALLGATE
```
* 屏蔽 Source/Drain 到 Bulk
```
PEX IGNORE CAPACITANCE DEVICE INTRINSIC nsd ALLGATE
```
* 屏蔽 Source 到 Drain
```
PEX IGNORE CAPACITANCE DEVICE nsd nsd ALLGATE
```
可以看到，这种写法比较繁琐，不如前面的方法简洁。

总结，屏蔽掉 spice model 中的寄生参数最简单的方法是:在工艺文件定义中用 diffusion 的关键字，用 src_drn_layers 指定具体的layer名称。然后在提取命令文件中，把 gate 的电容通过 PEX IGNORE 语句来屏蔽掉，不需要写 source, drain 的屏蔽语句。