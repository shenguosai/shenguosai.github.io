---
title: 放大器的谐波失真(THD)
date: 2023-08-21 21:47:21
tags: 
categories: 
- [半导体专业,模拟电路]
mathjax: true
---
本文主要总结和收集关于放大器谐波失真的原因及改良方法。
## 一、什么是谐波失真
>THD是英文Total Harmonic Distortion的缩写，译成中文即为总谐波失真。

### 谐波失真是什么？
谐波失真的原因是电路电路的线性度不好。
理想线性电路的输入x和输出y可以使用一次方程表示：$y=ax+b$
<!--more-->
如果输出和输入的关系偏离了上述方程的直线我们称之为线性度差。
下面是一个典型的线性度差的代表电路：Diode Clip电路。
![20230821215601](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20230821215601.png)
根据二极管的顺方向I-V特性可知，在±0.6V之外被clip，就是说当输入电压在±0.6V之外时，电路显现非线性特性，如下图：
![20230821215939](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20230821215939.png)
如果将正弦波作为此电路的输入信号，输出信号变为在原信号基础上叠加了“不存在”的信号。其输入即输出入下式所示：
输入：$V_{in}=sinωt$
输出：$V_{out}=a_1sin(ωt+φ_1)+a_2sin(ωt+φ_2)+a_3sin(ωt+φ_3)+...$
输入输出的FFT(Fast Fourier Transform)结果如下图所示：
![20230821220543](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20230821220543.png)
## 二、谐波失真的定量分析
在进行谐波失真的定量分析时经常用到的指标为THD和THD+N。
>THD+N即Total Harmonic Distortion + Noise的缩写。

通常我们所说的谐波失真都是叠加了输入信号的高次谐波。THD的定义即为在基波的高次谐波的平方和的平方根与基波的电压之比。
即：$V_{THD}=\frac{\sqrt{V_2^2+V_3^2+V_4^2+...+V_N^2}}{V_1}$
其中，$V_1$为基波电压成分，$V_2$、$V_3$、$V_4$···$V_N$为整数倍的谐波电压成分。
高次谐波成分+噪声，即THD+N就是在高次谐波中再加入噪声：
$V_{THD}=\frac{\sqrt{V_2^2+V_3^2+V_4^2+...+V_N^2+V_{Noise}^2}}{V_1}$
## 三、通过反馈改善谐波失真
改善THD方法中最一般的方法就是利用反馈，还是用上面的Diode Clip电路举例说明，在此电路中加入放大器及其反馈后电路结构如下：
![20230821222803](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20230821222803.png)

*我们使用LTSpice中的理想放大器，设置其直流增益为100dB，带宽为10MHz。则1kHz的输入信号的增益为80dB。*
![20230821223007](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20230821223007.png)

由于作为中间节点的放大器的输出可以对非线性进行抵消，所以整个电路的输入输出特性有了较大的改善。
![20230821223242](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20230821223242.png)

使用1kHz的正弦波作为输入时可以看出只有少量的谐波成分的残留。
![20230821223841](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20230821223841.png)

通过FFT结果可以看到奇数倍的高次谐波成分。$THD=3.2\%$
![20230821224015](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20230821224015.png)

### 下面讨论如果进一步削减谐波失真应该怎么做
首先为了加强反馈网络我们可以保持放大器带宽的情况下将放大器的增益提高到120dB以上。
![20230821224157](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20230821224157.png)

可以看到THD完全没有改善还是3.2%，原因是1kHz的反馈量没有变化。
![20230821224254](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20230821224254.png)

然后尝试保持放大器的直流增益不变将带宽增加到100MHz。由于是单极点放大器，所以1kHz时的增益约上升10倍。
![20230821224406](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20230821224406.png)

THD改善了0.35%，约上升了1/10。所以，<b><font color="red"> 改善AC的谐波失真需要重点关注带宽！！</font></b>
![20230821224550](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20230821224550.png)

为确保放大器的稳定性，在高频阶段通常降低增益。也就是说在单位增益频率附近的信号无法使用反馈来改善THD。所以，一般来说高频信号的THD会比较差。

## 四、输出端形成的开关谐波失真
经过妥当设计的放大器在规格范围内使用时，造成谐波失真的最多原因是输出端的谐波失真。
通过使用射极跟随器驱动负载电路的电路举例说明：
![20230821224912](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20230821224912.png)

射极跟随器通常对输入电压进行约1倍的放大输出(即，输出=输入)，但如果集电极的电流在接近0A附近工作时，线性度会发生恶化。
![20230821225043](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20230821225043.png)

使用正弦波作为输入信号，在集电极电流为0(cut off)时输出波形出现严重失真。
![20230821225153](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20230821225153.png)

现实中放大器则通常通过一个npn和一个pnp组成推挽式结构，在一侧无法正常工作时通过另一侧进行补偿。
![20230821225302](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20230821225302.png)

即使使用合适的偏置在输入信号为0时两侧的三极管均不会发生cut off(Class AB)，在大振幅或大负载电流的情况下也会使一侧的三极管发生cut off，不可避免地导致THD指标恶化。
![20230821225458](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20230821225458.png)

开关谐波失真在Class A结构中可以避免，但是会增加功耗及电路的复杂度。

## 五、耗尽层电容引起的失真
去除开关因素导致的失真之后，剩余的就是耗尽层电容为起因导致的失真。
由于耗尽层是绝缘的，所以在Si和耗尽层之间可以看做形成电容，这个电容称之为耗尽层电容。耗尽层的长度随两边的电压不同而变化，就是说随着两端电压的不同耗尽层电容值是发生变化的。可变容量二极管就是利用这一原理制作而成的。普通的小信号用PN结二极管以及晶体管的集电极和基电极之间的容值也会跟随两端电压而变化。
耗尽层电容导致的失真由以下电路模型展示。
虽然在直流区域有反馈，但是在100kHz时输出由G1的互导和Q1的寄生电容决定。
Q1以外均使用理想器件，Q1为共基级放大电路，所以输出波形仅会受到由集电极电位影响的非线性度影响而导致失真。Q1的集极-基级之间的反向偏置，所以在集电极上形成耗尽层电容$C_{ob}$经电源V2连接GND。
![20230821230626](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20230821230626.png)

若集电极电压发生变化，则$C_{ob}$的容值发生变化，导致$C_{ob}$的充电电流释放至GND而造成了失真发生。THD为0.47%。
![20230821230849](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20230821230849.png)

其对策为增加射极跟随器使$C_{ob}$的充电电流能够回流。通过Q2可以使Q1的集电极==> $C_{ob}$==> GND释放的电流进行回流而抵消$C_{ob}$造成的非线性。为使开环增益相同增加C3.
![20230821231335](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20230821231335.png)

此时，THD有了巨大的改善，变为0.0008%。
![20230821231413](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20230821231413.png)

>参考文献：
*<font size=2>Douglas Self, Small Signal Audio Design, 2010, Focal Press</font>*
*<font size=2>黒田徹, 解析OPアンプ&トランジスタ活用, 2002, CQ出版</font>*