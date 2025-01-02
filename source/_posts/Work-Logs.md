---
title: Work Logs
date: 2024-01-30 18:00:00
tags: Work
categories:
- [半导体专业,模拟电路]
---
# CXT4644
## 完成项
1. VOL_GEN3电路功能分析:
2. POWER_SEL_CTRL逻辑分析
3. 部分电源域及启动时序分析
   - VIN启动 => VDD_1和VDD_2紧跟着启动(不需要EN信号的使能)，但启动不完全；
   - EN=H => C144_PLUS(VOL_GEN3的输出也是BG模块的电源)启动 => BG输出开始启动，初始阶段只有0.8V(大约持续100us，参数待调)左右，之后缓慢上升 => 稳定于1.2V之后BG输出GOOD信号并配置VLO_GEN3使其完全启动；
   - 逻辑部分EN起主导作用，并在DC内部穿插多个模块，需要整理；
   - 非EN控制直接启动部分需列表。

![20240131101346](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240131101346.png)
![20240131101405](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240131101405.png)

## 次日计划
按照正常启动时POWER_SEL的控制逻辑A0A1时序 00=>10=>11 来进行内部参数调整，争取完成此模块的设计。
* 注: A0A1 无 01 逻辑，因为 EN=H 后 A0 马上变 H，而 A1 则需要等到 BG 模块的输出 GOOD 信号变 H，EN=L 时 BG 模块不允许启动。

# CXT4710-Buckboost
## 2024年11月20日
### 电路设计
1. 确认 EN=>BG PG 的启动时间；（TOP：354us；BG：151us）——————*已确认，网表更新后已一致。 2024/11/20*
2. 追踪 EN_Delay 信号的源头及原理，如果追不出来则在电路和版图中放上 VIN UVLO 信号做 PWM 的使能的预备，防止芯片无法启动时可做 FIB 及简单改版；
3. PWM 启动的第一个 Pulse 的时机需要确认，目前在一个无根据的位置，是否需要调整到 VC 信号上升到 Current Limiti 下限位置。

### 版图
1. SMIC18 BCDM 工艺中的 P 管，包括 LDMOS 的 N+ 衬底都与 Deep NWell 物理相连，无法起到隔离的效果；
2. POWER MOS 的尺寸与实际有偏差，委托聚跃修正。

## 2024年11月25日
### 电路设计
1. 在启动之初，PFM 比较器的延迟高达 20us，但在负载过度相应时，在 VC 等效电压超过基准后 2us 之内 PFM 比较器就完成了翻转，需确认比较器；
2. 11月20日的第2条分析：当 PFM 比较器的 ENP1/2 使能时，此时由于 VC 等效电压低于基准，输出翻转为 H，当 VC 等效电压高于基准时，要延迟 20+us 输出才会翻转为 L；