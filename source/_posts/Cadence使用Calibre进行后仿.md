---
title: Cadence使用Calibre进行后仿
date: 2023-12-18 13:50:59
tags: Calibre
categories:
- [半导体专业,EDA工具]
---
1. 通过 Virtuoso 的 Layout XL => Calibre => Run PEX 进入 Calibre PEX，并将工艺厂提供的 xRC(Rule) 文件设置好 Calibre PEX。
<!--more-->
1. 为了减少寄生参数，以及提高后仿效率，建议在 Calibre PEX => PEX Options => Netlist => Reduction and CC 中将 Enable MinRes reduction 和 Enable MinCap Reduction 选中，并在 COMBINE 和 REMOVE 后的框中填写所需数值，一般 Cap 的 Combine 为 0.5fF，Remove 为 0.1fF。Res 的 Combine 为 1Ω，Remove 为 0.1Ω。
2. 在 Outputs 选项中，Outputs => Netlist 框中，Format 选 CALIBREVIEW(可增加 Geometry 可在 PEX 运行完后弹出寄生参数的总结)，Use Names From : SCHEMATIC 。
3. 点击 Run PEX 开始寄生参数提取，之后进入 Calibre View Setup 界面。
4. Calibre View Setup 设置中，Output 选项填被提取寄生参数的 library，Calibre View Type 选择 masklayout 意味着直接以 layout 的摆放位置提取寄生参数，选择 Schematic 意味着以 schematic 的位置生成寄生参数，建议选择 schematic，这可以在后仿中如前仿一样直接查看 schematic 的电路仿真信息，Create Terminal => if matching terminal exists on symbol，Device Placement => Layout Location，设置完毕，点击 OK 。
5. 提取寄生参数完毕后，如果有错误可能是 calibre 中部分线短路了，手动改正即可，可通过 calibre view type 更改类型为 masklayout 可以避免这种错误，但是后仿不能通过直接点击。
6. 在 testbench 中新建 config view，将 config 中的 view to use 更改为提取的寄生参数 calibre，加入 calibre 提取出来的参数，同时 ADE L => Setup => Design 更改类型为 config，即可开始后仿。
7. 仿真结束后，可以直接在 schematic 中查看信号线。

生成 calibre 耗费时间过长，可以直接更改 calibre view type 为 masklayout，但是此种方法对于查看结果没那么方便。
或者直接在 pex 设置中，Output => Netlist => Format 更改为 SPECTRE 生成网表。
1. 该方法生成的网表不一定能直接用于 spectre 仿真，需要先将该网表的 pin 顺序更正。更改方法为，首先需要先用前仿电路跑一次仿真生成网表，然后在 simulation 这个文件夹中找到 input.scs 的网表，将该网表中的 pin 复制到后仿的网表中。
2. 后仿生成的网表更改完毕，在 testbench 中新建 config view，将 config 中的 view to use 更改为 Specify SPICE Source File，同时选择后仿的网表即可，这就是用网表仿真，速度快，但是不方便查看信号，但是利于排除寄生电容，需要排除哪项将其注释即可。