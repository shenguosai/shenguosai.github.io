---
title: smic工艺中带有ckt后缀的器件
date: 2023-12-08 14:16:14
tags: PDK
categories:
- [半导体专业,Fab]
---
在SMIC的PDK中存在后面带ckt和不带ckt的PDK，其中不带ckt的器件模型是bsim4，而带ckt的器件模型是subckt。网上查询，subckt就是sub circuit，即子电路的意思。
查询并猜想如下:
不带ckt: 类理想器件，即只针对此种器件建模；
带ckt: 考虑其内部构造所产生的各种寄生成分，并且包含寄生器件参数随电路参数的变化