title: Cadence的Spectre仿真指定子模块存储波形
author: 小人市井
top: true
date: 2025-12-18 15:28:36
summary:
img:
coverImg:
tags: Spectre
categories:
- [半导体专业,EDA工具]
password:

在模拟 IC 中，常用的 EDA 工具就是 Cadence 的 Virtuoso，而模拟电路仿真一般使用的仿真器是 Spectre。
当电路规模比较大要存储所有的波形就会很占服务器的硬盘，那有没有折中的方法呢？
比如系统中一部分电路已经完成明白了只看几个点就行，但是有一部分电路却需要将所有的信号打出来一个一个地进行分析。
**这时候就会有只保存某个子电路的所有波形的需求。**
在新版本的 Virtuoso 里面可以在ADEL中使用`ADE->Outputs->To Be Saved->Select By Subckt Inst`来实现。
但是我在 IC618 版本中没有发现这个设置，就需要通过以下方法实现：
1. 先建立一个`*.scs`文件；

2. 在文件使用`save`命令来指定子电路：

   ```skill
   save I0.* depth=2
   save I0.I11.* depth=3
   ```

   上面的语句有几点需要注意：

   - 如果没有 depth，默认是下层所有子电路，包含子子电路、子子子电路……
   - depth 的赋值是从顶层往下数的层级，而不是从当前层往下数的层级。如上面代码的第二行：I0 是 top 层电路，所以 I0 所在的层就是 testbench 电路，此为 depth=1；而 I0 的子电路从顶层数就是 depth=2，这里面可能有 I11，也可能有 I12、I13、 ……；而 I11、I12、I13……的 symbol 内就是 depth=3。所以 line1 的意思就是保存 I0 symbol 下的所有电压，而 line2 的意思就是保存 I0 下面的 I11 symbol 下的所有电压。即层数与 instance 名要匹配才行。

3. 在 ADEL 中选择 Setup => Model Libraries ... ，在里面将刚建的`.scs`文件 include 进去，Section 的位置留空即可。

这样，在 Spectre 生成网表的时候就可以将这里的语句也包含进去，可以在 ADEL => Simulation => Output Log... 中进行查看，有专门一部分是说这些语句所实现的保存的电压波形的数量。