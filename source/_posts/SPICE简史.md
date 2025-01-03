---
title: SPICE简史
tags: Spice
categories: 
- [半导体专业,EDA工具]
date: 2023-09-04 15:10:04
---

　　如今，SPICE广泛应用在仿真模拟电路(运放、基准、电源、AD/DA等)，混合信号电路(PLL、SRAM/DRAM，高速接口)，精确数字电路(延时、时序、功耗、漏电流等)，建立SoC的时序及功耗单元库，分析系统级的信号完整性，等等。
作为最早的电子设计自动化软件，它今天仍然是最重要的软件之一。可以说，没有SPICE，就没有电子设计自动化这个产业，也就没有今天的半导体工业。它的市场超过上亿美元。所有这一切，都是从1970年加州大学伯克利分校电机工程系的一堂课开始的。
<!--more-->
## SPICE的诞生
　　时间回到1970年，在加州大学伯克利分校电机工程与计算机科学系(UC Berkeley, Dept. EECS)，Ron Rohrer教授给7个研究生上“电路综合”课。Rohrer教授那时刚刚从仙童半导体公司(Fairchild Semiconductor)返回伯克利，没有时间准备教材。所以，在第一堂课，他就宣布：学生们一起写一个电路仿真程序。他跟系里管教学的助人Don Peterson教授达成一个协议：只要Peterson教授认可学生们写的仿真程序，他们就全部通过。否则的话，他们就全部不及格。这七个学生中有一个还是从机械系来的。他感到十分委屈：教授啊，俺啥电路都不会，俺就是来学电路的。这倒好，电路没学到，反而要去写电路仿真程序。这可咋办啊？Rohrer教授想了想，说没关系。虽然电路你不懂，但你的数值分析不是很厉害吗？OK，你就负责解方程这块吧。最后的结果证明了恰恰是学生们自己开发的解稀疏矩阵的模块是一个亮点，它使得可处理的电路规模成倍的增大。为什么这么说呢？如果你学过数值方法，你就知道一般解方程组用的是高斯消元法。它的时间复杂度是$O^{(n*3)}$。也就是说，电路规模增大一倍，你的运算时间就要增大到8倍。当时的电路仿真程序最多可以仿真10个晶体管。超过这个数，不是你的预算被烧没了，就是你的耐心被耗没了。但是，学生们注意到从电路搭出来的矩阵有个特点，就是它的稀疏性。一个电路矩阵里很多元素都是0(意味着两个电路节点之间没有连接关系)。既然是0，那就没有必要去存储和计算它了。这样一来，存储量和计算量大大减少了。
　　很多SPICE里面的基本要素都来自于Rohrer教授指导的这一堂电路分析课的项目，包括上面讲到的解稀疏矩阵的模块，还有隐式积分算法的使用使得瞬态分析更加稳定。并且，程序里加入了自带的半导体器件模型，用户只要给出一组模型参数，用不着自己提供器件模型的FORTRAN模块了。
这7个学生推举Laurence Nagel为代表，由他负责向Peterson教授汇报结果。这个结果就是CANCER。没错，它的意思就是“癌症”。它是“不包括辐射的非线性电路计算机分析”("Computer Analysis of Nonlinear Circuits, Excluding Radiation")的缩写。不要忘了，这是在一个叛逆的时代。当时绝大部分的电路分析软件来自于大公司与政府/军方的合同开发。在冷战和核威胁的环境下，政府/军方要求这些软件都具有分析电路抗辐射的能力。伯克利是反战的大本营，学生们自己开发的程序当然要跟政府/军方的要求对着干了。
有同学可能会问：为什么要开发一个电路仿真程序？呵呵，要知道在这之前，人们分析电路，要么是用笔和纸，要么就要搭电路板(Bread Board)。Peterson教授就被学生们称之为“信封教授”，因为他认为电路分析用个信封的背面来做就足够了。但随着电路规模的增大，用笔纸变得越来越不可能，搭电路板又不能精确反应芯片上的电路特性，而且费用也越来越高。因此，用软件来做仿真就变得日益迫切了。
　　当课程结束，Nagel向Peterson教授汇报CANCER之后，Peterson教授给予了认可。学生们都通过了！CANCER成了Nagel的硕士论文课题。它在伯克利被很多本科生及研究生使用，并且给了大量的建议去改进它。呵呵，都说学生是最好的“小白鼠”，这话果然不假(再插一段话：基于这堂课的巨大成功，Rohrer教授后来又用同样的办法试了几堂课，但都失败了。他自己总结说，是因为有Nagel，伯克利的那堂课才成功了。所以，如果没有Rohrer教授那样的功力和Nagel那样天分的学生，SPICE也不可能从一堂课里诞生出来。)
　　到了1971年的秋天，Nagel在伯克利又开始了他的博士生生活，这一回是在Peterson教授的指导下了。(在这之前，Rohrer教授离开了伯克利到工业界去发展。原因嘛，据说Rohrer教授与Peterson教授在是否要公开CANCER的源代码上有不同意见。Rohrer教授后来又回到了学术界，在卡内基-梅隆大学(CMU)做教授，并指导开发了AWE，这是后话。)
　　Peterson教授给Nagel的第一个任务是给程序起个新名字。确实，CANCER太难听了，谁都不喜欢。Nagel花了天知道多长时间才想出来这样好听的，也就是我们现在还在用的名字：SPICE(Simulation Program with Integrated Circuit Emphasis)。(所以，如果你要写一个新程序，创建一个新公司，生一个小孩，一定要给他/她起个好听的名字。)
　　1971年被正式认定为SPICE诞生的年份。
<div>
    <center>
    <img src="https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20230901155141.png"
         alt="加载错误"
         style="zoom:100%">
    <br>
    Laurence Nagel当年在伯克利的照片
    </center>
</div>
<br>
<div>
    <center>
    <img src="https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20230901155358.png"
         alt="加载错误"
         style="zoom:100%">
    <br>
    Ron Rohere教授
    </center>
</div>
<br>
<div>
    <center>
    <img src="https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20230901155442.png"
         alt="加载错误"
         style="zoom:100%">
    <br>
    Don Peterson教授
    </center>
</div>
<br>
　　SPICE还是开源代码的先驱。当时也有开源代码，但都没有太大的商业价值。SPICE就不同了。有人已经看到它的商业价值，但Peterson教授坚持要把代码开源(我们都得真心的感谢Peterson教授)。任何人只要花20美元的手续费，就可以得到SPICE的源代码(当然，在冷战时期，SPICE被禁止出口到政府认为的“共产国家)。有人会问，那这样一来，伯克利是不是损失了一大笔钱呢？事实并非如此。伯克利的SPICE帮助数字设备公司(DEC)卖出了很多台VAX机。反过来，DEC给伯克利电子系捐赠了1800万美元(这可是二十年年的数目，考虑到通货膨胀，你可以想象现在值多少钱)。这么多钱可不是一个学校卖代码能获得的。所以，做好事终究还是会得到好报的。

## SPICE2和SPICE3
　　在70年代初期，伯克利电子系用的计算机是CDC6400大型机，它的运算能力相当于286(时钟频率是10MHz，可它的成本是六百万美元。再看看今天你手中的iphone，它的时钟频率超过1GHz，成本不到600美元——这是100万倍性价比的差别！)分给每个学生的主内存白天为256KB。到了晚上人少，你就可得到384KB。运行一个不算太大的电路仿真，用Nagel的话说，就像把你11码大的脚穿进婴儿的鞋里——你得想尽一切办法节省内存。能仿真的最大的电路规模也就是25个双极型晶体管(相当于50个电路节点)。而且，那时候SPICE还只有双极型晶体管模型。71年的秋季，从贝尔实验室来到伯克利的David Hodges教授带来了第一个MOSFET模型：Shichman-Hodges模型。如果你用过SPICE(并且年头足够多的话)，你应该知道这就是Level1 MOSFET模型。它是所有MOSFET模型的鼻祖。
　　1975年Nagel从伯克利博士毕业。他的论文 ***“SPICE2:A COMPUTER PROGRAM TO SIMULATE SEMICONDUCTOR CIRCUITS”***，成为了EDA行业被引用最多的文章。
　　SPICE2这个版本基本上奠定了今天电路仿真程序的基石，其中包括：改进的节点分析法(Modified Nodal Analysis)，稀疏矩阵解法(Sparse Matrix Solver)，牛顿-拉夫逊迭代(Newton-Raphson Iteration)，隐形数值积分(Implicit Numerical Integration)，动态步长的瞬态分析(Dynamic Time Step Control)，局部截断误差(Local Truncation Error)，等等——说太多技术细节了，还是接着讲故事吧。
<div>
    <center>
    <img src="https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20230901165841.png"
         alt="加载错误"
         style="zoom:100%">
    <br>
    Nagel博士论文的封面
    </center>
</div>
<br>

　　下载这篇论文：[SPICE2:A COMPUTER PROGRAM TO SIMULATE SEMICONDUCTOR CIRCUITS](https://www2.eecs.berkeley.edu/Pubs/TechRpts/1975/9602.html)
　　如果你想了解SPICE的核心秘密，就下载一份好好读读吧！
　　最早的SPICE2没有用户界面。它的运行是批处理方式。也就是说，你准备好了你的电路描述和仿真命令，就把它们提交给主机系统里。然后呢？然后你就可以回家了。因为你的几十(百)个同事也在做着同样的事情。所以，等第二天早上上了班再看结果吧！
　　SPICE2的输入是用打卡。你可能会问：什么是打卡啊？呵呵，祝贺你年纪够小。对那些年过半百的人，最初接触到的计算机输入界面就是像下面这样的卡：

<div>
    <center>
    <img src="https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20230901171423.png"
         alt="加载错误"
         style="zoom:100%">
    <br>
    IBM Punch Card
    </center>
</div>

　　你把你的电路描述及仿真命令打在一叠这样的卡上，然后放到读卡机里。你可能听说过SPICE的输入叫“SPICE DECK”，这个名字就是从这叠卡来的。
　　SPICE2的输出是行打印机。是的，就是用下面这样的打印机打出仿真结果在纸上(想象一下那时消耗了多少纸张)。

<div>
    <center>
    <img src="https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20230901172227.png"
         alt="加载错误"
         style="zoom:100%">
    <br>
    Fujitsu FACOM 655B Line Printer
    </center>
</div>

　　你也可以打印输入输出的信号波形。每个波形是用不同的字符画的。像下面这样(看着是不是很粗糙呀)：

<div align="center">
<img src="https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20230901172528.png">
</div>

　　有同学读SPICE手册时会看到一个奇怪的选项叫“NOPAGE”。这是因为SPICE的输出在页与页之间的折线处会加入一个分页符，流出空白。这个选项就是要求不要停止打印的。这样一来，波形就不会因为换页而在页与页之间断掉了。随着行打印机的消失，这个选择项也进入了历史。
后来SPICE2的输入/输出也进化成了文件输入/出。像下面这样：

<div align="center">
<img src="https://raw.githubusercontent.com/shenguosai/MyPic/img/img/111.png">
</div>

　　Nagel毕业后去了贝尔实验室。从此以后，SPICE2的改进就由Nagel的室友，Ellis Cohen，继续进行下去。Ellis是个计算机编程能手。用当时周围学生的话说，他就是一个长成人形的计算机。是他(以及后来的Andrei Vladimirescu和Sally Liu)把学校里开发的程序SPICE改造成了实用的SPICE2G6。在SPICE的早期开发中，他是个无名英雄。今天工业界里的很多商业SPICE就是基于SPICE2G6开发出来的。
　　到了八十年代，SPICE2已经遍及了各个大学。但它的问题也显现了出来：FORTRAN代码太难维护，加新的器件模型需要改动的地方太多，等等。在此同时，C作为一种新的程序语言正方兴未艾。于是，用C语言重新写SPICE就被提到了议事日程上来。这个任务被伯克利的Thomas Quarles在89年完成了。比起SPICE2来，SPICE3增加了用户界面，你可以使用命令，甚至命令串来控制程序。另外，还增加了图形界面看波形。更重要的是，SPICE3的程序构架更加清晰，更加模块化。维护及修改起来更加容易。八十年代也是计算机硬件突飞猛进的时代：大型机(mainframe)被工作站(workstation)取代。UNIX及架构在它上面的C-shell和X-window成为软件开发及应用的基本框架。另外，个人电脑(PC)也越来越普及。这些都为SPICE的广泛应用打下了坚实的基础。

<div>
    <center>
    <img src="https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20230901180559.png"
         alt="加载错误"
         style="zoom:100%">
    <br>
    Quarles论文封面
    </center>
</div>

　　同样，你可以用这里的链接下载 [Quarles的论文](http://www.eecs.berkeley.edu/Pubs/TechRpts/1989/ERL-89-46.pdf)。
　　下面是SPICE3(版本3f5)的执行语句，注意它是交互式的。每一个“Spice<number>->”后面是一个spice3的命令。不如“source”就是把电路读入，“run”就是运行，“display”就是显示，“quit”就是退出。

<div align="center">
<img src="https://raw.githubusercontent.com/shenguosai/MyPic/img/img/222.png">
</div>

　　SPICE3自带了一个图形模块nutmeg。下面是nutmeg显示的波形，是不是比SPICE2的行打印的字符波形好看多了？

<div align="center">
<img src="https://raw.githubusercontent.com/shenguosai/MyPic/img/img/333.png">
</div>

　　自从上世纪90年代后，学术界SPICE的发展基本就停止在SPICE3f5这个版本了。这是不是意味着SPICE停滞不前了呢？非也。至少在两个方向上SPICE还在一直发展：一个是器件模型(特别是MOSFET模型)，另一个是商业SPICE程序。(这里值得提一下，有一批SPICE的爱好者及高校把SPICE3f5接过来，并整合了其它几个开源软件(xspice, cider, gss, adms 等)，建成了ngspice。Ngspice也在缓慢的进化着，但比起商业SPICE进化的速度慢多了。你可以在 [sourceforce](https://sourceforge.net/) 上找到 ngspice，也可以去 [ngspice](https://ngspice.sourceforge.io/index.html) 的主页上去查看。

## SPICE中器件模型的演变
　　SPICE里面自带了很多模型。像无源元件电阻、电容、电感等等，以及有源器件二极管、双极管等。但花样最多的、变化最频繁的、复杂度最高的，当属MOSFET的模型了。这主要是因为从七八十年代以后，MOSFET的工艺因为它的低功耗，高集成度而变成了主流。那时候还是个半导体工业百花争鸣的年代。很多半导体公司如雨后春笋般的冒出来(就像现在的社交媒体公司一样)。几乎每一家公司都在工艺及器件上有点自己的绝活，所以集成电路公司大多是个独立器件制造商(IDM)。这就造成了MOSFET的模型也层出不穷。谁家的SPICE支持的MOSFET模型越多，谁的SPICE用户群就越大。
　　前面我们说过SPICE2中加上了MOSFET Level1的模型。等到SPICE3出来的时候，里面已经加入了Level2及Level3模型。到了九十年代，又加入了著名的BSIM(Berkeley Short-channel IGFET Model)模型。可以这样说，现在所有的Foundry用的模型都来自于BSIM家族。为什么在众多MOSFET模型中BSIM胜出了呢？
　　我们知道，SPICE是用来解含有非线性器件的电路方程的。解非线性方程的一个有效方法就是牛顿迭代--把非线性方程在某个点给它线性化，然后逐次逼近最终解。这个过程有点像两个宇航飞船对接--如果对方的接口在你的左边，你就往左偏一下。如果你偏多了，对方的接口在你的右边了，你就再稍往右偏点，知道最后两个接口对准锁定。但这里面又个要求：就是非线性曲线的一阶导数要连续。如果不连续的话，就好像喝醉酒的人来控制飞船对接，忽左忽右，或者根本就掉过头来，布置东南西北，上下左右了，如何能对接上呢？不幸的是，很多早期的MOSFET模型(包括Level1、2、3)都有这个问题--模型的电流曲线的一阶导数在工作区域内不连续。这是因为人为的把器件分成了不同的工作区域。不同区之间能保证电流连续已经不错了，哪还去管它的导数呢！这样做的后果就像管对接的人喝醉了酒，没法瞄准目标，最后导致SPICE不收敛　(Non-convergence)，或时间步长太小(Time Step Too Small--这有很大可能也是不收敛造成的)。
　　早期的BSIM模型还保留了工作区的概念。但在不同的区域之间加入了平滑过渡曲线，以保证电流曲线及其一阶导数的连续性。在它后来的版本中，就彻底抛弃了工作区域的观念--干脆只用一个(连续且可导)曲线来代表整个工作区域里的特性。这就从根本上解决了不连续的问题。BSIM家族中最成功的代表是BSIM3v3(HSPICE中的Level49)和BSIM4v5(HSPICE中的Level54)。从此以后，再也没有其他的模型能出其右。它们俩也是工业界的MOSFET器件模型标准。BSIM3v3跨越了亚微米的工艺(0.3微米至0.13微米，大致从1993年到2000年)，BSIM4跨越了深亚微米到纳米的工艺(90纳米至20纳米，大致从2002年到2012年)。
　　你可能会问：这么好的器件模型是谁做的？猜一下--对了，还是伯克利。是伯克利电子系器件模型小组。它的掌门人就是胡正明教授(Pref. Chenmin Hu)。
<div align="center">
<img src="https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20230903215808.png">
</div>
<br>
　　今天的SPICE开发者要感谢胡教授。如果BSIM不是工业标准，那你就得像HSPICE一样加几十上百个MOSFET模型。不光工作量大，容易出错，还有很多内部的模型人家还不愿意给你呢(参见下面Smart-Spice的故事)。而现在，你只要加一、二个开源的BSIM标准模型就可以覆盖绝大部分用户了。<br>
　　有同学要问：现在的最新工艺不是已经到了16纳米、14纳米，以致10纳米，那这些工艺的器件结构与模型又是什么呢？答案已经有了：还是胡教授的小组开发的FinFET(也叫3维FET)模型。实际上，早在99年胡教授就发表了FinFET的文章。因此他也被称作FinFET之父。他同时是美国和中国的科学院士。同学，如果你的一生中能达到以上其中的任何一项，是不是就可以笑傲江湖了呢?<br>
　　就像半导体工艺由简到繁的过程一样，MOS器件模型也从Level1的几个公式/几十行代码，发展到BSIM的几百个公式/上万行代码。这里值得提出的一点，与BSIM3/BSIM4模型不同的是，FinFET模型不是用C语言，而是用Verilog-A语言写的。这直接导致了把它加到SPICE3中的困难。虽然很多商业SPICE已经支持Verilog-A，但现在开源的SPICE3却还没有做到(这里插一句：基于SPICE3的ngspice当中包含了支持Verilog-A的开源编译器ADMS。但要做到完全自动编译FinFET模型这样重量级的模块还有一段路要走)。也就是说，虽然FinFET模型是开源的，但现在它的仿真载体并不开源。这种现象与早期的SPICE研发反了过来。现在学术界落到了工业界的后面。看到这儿，学术界的同学是不是要深思一下呢?

## 商业SPICE的演变
　　前面我们提到当CANCER出来的时候就有人意识到了它的商业价值。毫无疑问，SPICE的出世必定会有人把它商业化。事实却是如此。八九十年代是商业SPICE出现的高峰期。至少有几十个SPICE的变种冒出来。有的获得了巨大成功，有的毫无声息的消失了，有的还在惨淡经营着。同学，如果你想创业，这里面有太多的经验和教训了。
#### HSIPCE
　　先来说说HSPICE，记得我们前面讲过的批处理运行吧。在当时的大公司里，这是电路仿真标准的运行方式，但这么做的效率太低了。设计者需要尽量短的时间看到仿真结果，然后修改电路参数再做仿真。如此多次以达到最佳结果。有两个孪生兄弟Shawn Hailey及Kim Hailey，当时都在AMD做设计，看到了这里面的问题。与其让几百个客户排队等一个银行柜员，为什么不让每一个客户都有一个柜员呢？问题就是商机。他们决定跳出来开自己的公司。于是78年，Meta-Software成立了，他们把改进的SPICE变种取名为HSPICE(你现在明白了吧，为什么要以H开头？这可是兄弟俩姓的第一个字母)。他们把SPICE2从大型机移植到了VAX小型机上，后来又移植到Sun工作站上。就这样，借着计算机硬件改朝换代的东风，越来越多的公司开始使用HSPICE了。直到如今，这个HSPICE成了工业界的“金标准”。只要你做个仿真器，人们一定会跟HSPICE比结果的。而且，在SPICE前面加一个字母成了时尚。到今天，有人开玩笑说A-SPICE到Z-SPICE都已经被人用过了(当然，HSPICE仍然是最出名的)。
　　有人可能会问：要是我当时也把SPICE移植到小型机上，我是不是也可以成功？呵呵，成功的要素有很多，光用一条是远远不够的。比如说用户的反馈就是相当重要的一条。举个例子，HSPICE是第一个把器件模型库卡(.LIB)和结果测量卡(.MEASURE)做进去的。像这样的例子还有很多。这些虽然不是什么革命性的技术创新，但它们很实用，能大大提高用户的使用效率。甚至某些时候，对用户来说，这样的小改进比创新的算法更重要。
　　前面我们提到了七八十年代有很多的MOSFET器件模型。HSPICE把能拿到的器件模型都收进去了。所以，HSPICE的MOSFET器件模型是最全的(不信的话，你就去拿本HSPICE的MOSFET模型手册读一下--注意，它是一本独立的手册。也就是说，光是它里面的七八十个MOSFET模型就是一本书了)。但这样还不够，Meta还开发了自己的MOSFET模型：Level28.他们跟用户的工艺线紧密联系。在工艺线流片之前，相应的器件模型参数已由芯片加工厂(Foundry)提供给芯片设计者了。如果你是设计者，你还能不用它吗？这样做的结果直接导致了HSPICE用户群急速的扩大。就像滚雪球一样，一旦超过了临界质量(critical mass),它自己就会越滚越大。据Meta-Software的人说，在公司巅峰的时候，它们的销售员就是一台传真机。你只要把传真机号码告诉客户，他们就把订单发来啦(那时候的钱真好赚啊，当然公司里肯定不止一台传真机)。从78年成立到96年这18年期间，公司一共卖出了一万一千多套HSPICE，它的年成长率达到了25%~30%。
　　1996年Meta-Software被Avant收购，到2001年，Avant又被Synopsys收购。关于Avant的故事有很多。这个公司(包括它的头儿 Jerry Hsu)就像EDA业界的一匹黑马。它的故事足可以写另一个长篇了。
　　Meta-Software兄弟中的老大，Shawn Hailey，已于2011年去世。在此之前，他把自己的名字改成了Ashawna Hailey。
<div align="center">
<img src="https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20230903224801.png">
</div>
<br>

#### PSICE
　　PSPICE像HSPICE一样，PSPICE的故事也跟它的名字有关。首先，这第一个字母“P”并不是其创始人的名字。事实上，创始人的名字Wolfram Blume里面根本没有字母“P”。那这字母“P”到底是什么依稀呢？对了，它就是PC。PSPICE的发展跟PC的发展是密不可分的。但这并不是PSPICE的初衷。
　　时间回到1984年，那时Wolfram Blume从加州理工(CalTech)毕业加入南加州一家半导体公司。工作中，他听到很多抱怨，说公司内部的SPICE速度太慢了。这位老兄也不含糊，立马对其SPICE来了一个详尽的分析。结果发现，大部分时间花在了算MOSFET模型的方程上(记得前面我们讲的MOSFET的复杂性吧)。他一想，如果能用硬件来并行处理这些方程，岂不就可以加快仿真速度了吗？(呵呵，又是一个看到商机的主)？恰恰那个时候英特尔推出了支持硬件并行的8085/8086/8087.说干就干，这位老哥创立了MicroSim公司。又是在这时，IBM推出了基于Intel芯片的IBM-PC。另一个机会又来了：只要把SPICE从大型机上移植到PC上就行了。这事儿比起第一个事儿简单太多了。可是，人们当时认为PC就是个游戏机而已，没人拿它来做什么正经事儿(呵呵，看看现在不还是这样吗？)。所以，这个老哥并没有把这第二件事看得太重，而是集中绝大部分精力和资源去做硬件并行。
　　当时的IBM-PC有640KB内存。最大的数组只允许64KB内存。而SPICE是用一个巨长的数组来存储所有的数据。把SPICE的数据放到IBM-PC的结构，用这位老哥的话说，就像把一只鲸鱼塞进一个金鱼缸里。但他们做到了(中间略去他们N个睡不着的工作之夜)。并行硬件的确加快了方程的处理，可他们也快没钱了。这位老兄忽然想到，咱不是把SPICE移植到PC上了吗？咱就先卖着这个软件，用卖它的钱继续开发咱得并行硬件。就这样，PSPICE就开始在PC上出现了。
　　最初这位老兄是想卖硬件加速器的PSPICE版本，可结果恰恰相反，两年后，纯软件的PSPICE卖出去一千多套，而硬件加速器只卖了两套。到这时候，这位老兄也明白了，做硬件吃力不讨好，市场并不需要。他把卖出去的两套硬件加速器又自己买了回来(当然又半卖半送给人家N套纯软件的版本)。
　　同学你看，一个高新复杂的技术并不一定会做出一个卖座的产品。反过来，一个貌似简单的技术可能很受市场的欢迎。另外，PSPICE虽然不是赚钱最多的，但它的用户数绝对是最大的(遍及全世界五大洲)。你可以下载一个免费的PSPICE用。当然，只限于十个晶体管。但这对一般学生的学习来讲，大部分情况下已经够用了(想一想当年的大型机也就只能算这么多)。你如果在网上搜一搜，就会发现阿拉伯语(以及其它语言)的PSPICE的教材。你如果是在校生的话，很可能也在用PSPICE。

<div>
    <center>
    <img src="https://raw.githubusercontent.com/shenguosai/MyPic/img/img/444.png"
         alt="加载错误"
         style="zoom:100%">
    <br>
    PSPICE第二版封面
    </center>
</div>

　　MicroSim在1998年被OrCAD收购，OrCAD在2000年又被Cadence收购。

## Spectre
　　话说89年，伯克利毕业了最后一批做SPICE研究的学生。其中一个叫Ken Kundent。Ken非常有才气，他在伯克利的研究成果后来成为了安捷伦的微波仿真软件。同时他的傲气也不小。在加入Cadence后，他看到HSPICE卖的很火，就决定做个新的仿真工具去取代它。这就是Spectre。据说他用了两个星期就写出了第一个版本(呵呵，不愧是伯克利SPICE大本营出来的)。SPECTRE比HSPICE要快两三倍，还具有更高的精度及更好的收敛性。但它并没能取代HSPICE。为什么呢？一个原因是兼容性。SPECTRE的输入格式跟HSPICE有很大不同。Ken计算机编程的功底很深，他设计的Spectre的输入格式像C语言一样。虽然从计算机语言角度看，Spectre的输入比HSPICE的输入更规范，但SPICE的用户是电路设计者，他们才不管你的语言多么优美，只要好用就行。另外，如果你是个电路设计者，花了几年功夫好不容易才学会了一种语言格式，用它已经写了成百上千个电路网表，而且它们都工作的好好的，为什么要去换成另外一个呢？另外，还有一个重要的原因，就是用户对HSPICE的信赖。这种信赖不是一时半会儿就能建立起来的。它是经过几十年，成千上万遍仿真，几百次tape-out(流片)才能形成的。怎么能说换就换呢。
　　Ken琢磨着，既然更快更好还没办法取代SPICE，那我们就得做点SPICE没有的东西。做什么呢？恰好在九十年代中期，一种标准的设计语言VHDL开始向模拟电路扩展，这就是VHDL-AMS(VHDL的模拟电路及数模混合电路描述语言)。(这里再插一句，最早的数模混合电路描述语言是MAST，它是Analog公司的仿真器Saber里面使用的。VHDL-AMS是基于欧洲Anacad公司开发的HDL-A语言发展而来的。后来Anacad的仿真器成为Mentor的Eldo)。但当时还没有Verilog的AMS扩展(原因是VHDL主要在欧洲使用。而Verilog主要在美国使用)。Ken就想，好吧，我们也来做个标准的设计语言到Spectre里。这就是Verilog-AMS(Verilog的模拟电路及数模混合电路描述语言)。不过这事儿说起来容易做起来难。首先，既然你是标准，那就要大伙儿都同意。让大伙儿都同意的事是要花时间的，没那么快。其次更重要的，是你要让模拟电路设计者来学习并使用这个语言。这可是比登天还难的事儿。如果你是一个模拟电路设计者，你想想你在学校的课本上看到的是运放的电路还是它的描述语言？当然是电路了。至少到今天为止，还没有一本模拟电路的教科书是只用描述语言的。你再看看数字电路的教材，几乎全部都是VHDL或Verilog描述语言(呵呵，如果你还用晶体管来设计数字电路，那你的年龄够大了)。另外，当你做模拟设计的时候，你是在搭晶体管电路呢，还是在写描述语言？对模拟电路设计者来说，用语言而不用电路来做设计是不可想象的。反过来，对数字电路设计者来说，用电路而不用语言来做设计也是不可想象的。
　　Spectre-AMS做出来后，Ken发现当时的感兴趣者寥寥无几(呵呵，这哥们专找硬骨头啃)。那怎么办？在公司做产品是要卖钱的。Ken有点儿绝望了。这时，他想到了回去做他在学校的老本行：射频电路仿真。至少这个功能别的SPICE还没有。他把这个想法告诉了当时Cadence的市场经理Jim Hogan。Jim做了个市场调查。那时射频电路设计市场几乎不存在，只有几家做镓砷电路的算搭点边。当Jim把这调查结果告诉Ken，Ken也无可奈何的耸耸肩。Jim对Ken看了好一会儿，说，管它呢，你就做去吧。谁知道这一次却是歪打正着了。九十年代中后期正是无线通信市场腾飞的时候。很多在学校用Spectre-RF的毕业生加入了新的做射频电路芯片的设计公司。这些公司必须要用Spectre-RF做射频仿真。而Spectre-RF是Spectre的一个选项。因此，Spectre也就借着Spectre-RF的东风开始流行起来了。后来，HSPICE和Smart-Spice也跟风在自己的SPICE中加进了RF的选项。这也算是Spectre对SPICE的功能扩展做的贡献吧。
<div>
    <center>
    <img src="https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20230903235201.png"
         alt="加载错误"
         style="zoom:100%">
    <br>
    Ken Kundent
    </center>
</div>

#### Smart Spice
　　Smart-Spice是Silvaco公司的产品。说道Silvaco，就不得不说它的创始人Ivan Pesic。Ivan来自黑山共和国(Montenegro，欧洲巴尔干半岛的一个小国家)。像所有第三世界国家的穷学生一样，通过自己的勤奋努力来到美国。来美国之后，他先开了一家修车店。知道攒够了钱，才在1984年成立了Silvaco。他有一个儿子，可能是年幼时受了老爸的修车店的熏陶，决定长大了当个汽车修理工。因此学习也不上进。怎么让这小子好好学习呢？简单。有一天，老Ivan把儿子带到了圣荷塞（SanJose，硅谷一大城市）一个最破的修车厂的马路对面，对儿子说：你就坐在这儿，看看汽车修理工一天的工作是什么样的。自从那一天结束以后，儿子的学习成绩就全变成A了。
　　说到Ivan Pesic，我们还不得不说他打官司的故事。Silvaco的历史上与N家公司打过官司(而且大部分都赢了)。在此我们只讲讲与Meta-Software(后被Avant并购)的官司。话说八十年代末到九十年代初，Meta-Software和它的HSPICE如日中天，这其中它自己的Level28模型起了重要作用。Silvaco最初的产品是TCAD(Technology CAD)，并不是SPICE。这时它也准备开发自己的Smart-Spice，但它拿不到HSPICE的Level28模型。怎么办？Silvaco采用了一个瞒天过海的迂回战术。Silvaco有个不错的模型参数提取软件叫Utmost。它就找到Meta-Software说，你看，如果把你们的Level28模型公式放到我们的Utmost中，就会有更多的用户用你们的HSPICE。Meta一想也对，就把Level28模型给了Silvaco。没成想，过了两年，Silvaco自己的Smart-Spice出来了，而且里面还带着Level28模型。这下Meta-Software气坏了。就把Silvaco告上了法庭。也就在这个前后，Avant并购了Meta-Software。但Avant只看到了HSPICE这只下金蛋的鸡，却忽略了Meta-Software跟Silvaco的官司。也许是因为Avant恰恰正在和Cadence打着一场更大的官司，从而忽略了这个小案子。不管是什么原因，当法庭开庭要宣判的那一天，Avant居然没有人出庭。这下法官可气坏了。好啊，竟然藐视本法庭，来啊，判Avant输，并赔Silvaco两千万！本来Silvaco上庭前战战兢兢的，盼望着和解就不错了。这下倒好，不光不用和解了，还得了一大笔钱。呵呵，人们都说国外重视知识产权。这种重视其实是来自于众多这样的动不动就成败上千万的官司。所以同学，如果你是学理工出身的，那你不妨去学学法律。如果你是学文科出身的，那你不妨去学学理工。估摸着在不久的将来，国内这样的涉及知识产权的大官司也会越来越多。作为一个懂高科技的律师(或者一个懂知识产权法律的工程师)会很抢手的。
　　但是，一个公司如果光靠打官司，那也是赢得不了客户的。说实话，Smart-Spice做得还是蛮不错的，价格又便宜。Smart-Spice还是第一个“基于使用时间许可证”(use-time based license)的工具。这对许多小公司或个人用户是个好消息。如果你没几万美元去买高大上的商业SPICE，或者你就只需要跑几次仿真，那就可以最少花十几美元用Smart-Spice完成你要做的事。这就像买车还是租车一样。卖车店能赚钱，租车店也会有很多顾客的。这不也是一个很好的商业模式吗？
　　Ivan Pesic于2012年因癌症在日本去世。如今，他本来想当汽车修理工的儿子已经继承了老爸的事业，接替掌管Silvaco了。
![20230904002306](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20230904002306.png)

#### Aeolus-AS
　　我们应该感到骄傲--这是我们中国本土的SPICE。虽然名字叫起来很拗口。光从名字上也看不出这是SPICE。它是由华大九天开发的。至于为什么起这样一个名字，还是请华大的刘总来解答吧。本人并没有用过这个工具。下面的几句话是从华大的网页上摘下来的，也算给他们做个广告吧。“它是新一代高速高精度并行晶体管级电路仿真工具，能够在保持高精度的前提下突破目前验证大规模电路所遇到的容量、速度瓶颈。Aeolus-AS能够处理上千万个元器件规模的设计，仿真速度也比上一代晶体管级电路仿真工具有大幅提升，同时支持多核并行。”
　　还有一类是工业界但非商业(也就是不拿出来卖的)SPICE，通常它们都是公司内部开发使用的。一般只有拥有Fab的大公司(像Intel、前Infenion、前Motorola、Fujitsu等)才能负担得起一个开发团队。这种公司内部的SPICE基本都会有自己的器件模型。在这里我们就不多说了。

## SPICE的变异与进化
#### SPICE的变异与进化
　　SPICE最初是用来做小型电路仿真的。电路中的元器件数也就几十最多到几百个。随着电路规模越做越大，电路种类越来越多，人们会问：SPICE能不能跑得更快一些，能运行的电路更大一些？自然而然的，SPICE的变种就出现了。我们在这儿讲三个方面：第一是快速仿真；第二是数模混合仿真；第三是扩展应用。
　　先说说快速仿真(fast SPICE)。这也是市场最大，发展最多的一块儿。因为SPICE是把整个电路放到一个矩阵中来解。人们就想能不能把电路分成小块单独解，然后再把各块之间连接起来，这样不就快了吗？的确，对数字电路，确实可以用分割的方法。因为数字电路的信号是有方向的，我们可以在没有直流通路的地方把它分开(例如在两个串联的反相器中间)。另外就是数字信号是离散的，我们可以把它分成几段。分的段越大，时间步长也就能越大，需要解的次数就少了(当然结果也就没那么精确了)。还有就是器件模型。我们前面讲过如今的mosfet模型非常复杂，要花很多时间去算，那能不能把它简化呢？可以。事实证明对数字电路以及数模混合电路(像PLL、Memory、Serdes)来说，用表格模型(table model)来代替复杂的方程模型是个不错的选择。通过这些简化，快速仿真可以比原来的SPICE快几十到上百倍，而精度是在SPICE的5%~10%之内。像EPIC的PowerMill(后来成为Synopsys的NanoSim)，Anagram的ADM(后来成为Avant的StarSim)，Celestry(后来成为Cadence)的UltraSim，Nassda(后来成为Synopsys)的HSIM，等等。最近比较流行的是Magma(现在是Synopsys)的FineSim，BDA(现在是Mentor)的AFS，Proplus的NanoSpice。
　　其次来说说数模混合仿真。当一个系统中既有模拟电路，又有数字电路，人么自然就会想到把SPICE和数字仿真器(如Synopsys的VerilogVCS、Cadence的NC、Mentor的ModelSim)连在一起运行。SPICE去算模拟电路部分，数字仿真器去算数字电路部分，它们之间用数模/模数转换器(AD/DA)连接。注意这种运行方式跟上面的快速仿真不同。这样的混合仿真需要两个仿真器。而且这样的构架有缺点。主要的问题是数模转换没有一个标准。市面上有很多SPICE以及Verilog仿真工具，每一个工具的转换界面都不一样，这就造成混合仿真的界面非常复杂。因此，最近发展的混合仿真都采用数模一体化的构架，大大简化了转换界面，而且用户只需要再一个环境下就可以进行混合仿真。这样的工具有Cadence的Virtuoso-AMS、Synopsys的HSIM-plusHDL、Silvaco的Harmony、华大的Aeolus-ADS等。
　　最后再来说说SPICE的扩展应用。虽说SPICE是针对集成电路(IC)开发的，但它的应用已扩展到系统级(System Level)，主要是电路板(PCB)级的仿真。那系统级仿真与集成电路仿真有何区别呢？它们不都是电路吗？呵呵，没错，它们都是电路，但区别还是蛮大的。主要是它们的规模与尺寸的不同。我们知道，集成电路是集成在芯片上的。其器件尺寸现在已做到纳米级。而系统的尺寸还在毫米、厘米甚至米的数量级。学过电磁的同学都知道，当器件的尺寸大于信号波长的时候，就要考虑分布的场效应了。拿一段导线做例子。一段在芯片上的导线，你可以把它看做一个电阻。而一段电路板上的导线，你就必须用传输线(Transmission Line)来代表它，否则误差就太大了。如果信号的频率再高，那就要用S参数了(S-Parameter)。因此，电路仿真发展出一大分支，这就是所谓的“信号完整性”工具。像Agilent的ADS、Mentor的HyperLynx以及Cadence的OrCAD和Allegro
#### SPICE今后的道路
　　从70年代初到如今的四十多年里，SPICE从只能仿真十几个节点/器件到今天可以仿真上百万个节点/器件的电路，这是一个非常惊人的成就。但这个成就的主要原因是摩尔定律。前面我们讲述过自从90年代中期，SPICE本身就没有太大的变化了。这怪就怪(不，应该是感谢才对)SPICE的先驱们。他们奠定了一个坚实的基础，使得我们后面的人都没什么可做的了。的确，要改变SPICE的基石，例如改进的节点分析法(Modified Nodal Analysis)，稀疏矩阵解法(Sparse Metrix Solver)，牛顿-拉夫逊迭代(Newton-Raphson Iteration)，隐形数值积分(Implicit Numberical Integration)，等等，确实不容易。说到底，SPICE是一个解非线性常微分方程的工具。你要想从根本上有个革命性的改变，那你还是从数学上着手吧。
　　SPICE是一个非常通用的工具。虽然集成电路是它的着重点，但我们看到它也被广泛应用到了系统级、电源级甚至延伸到了不同领域的仿真。我们前面讲到了数模混合(Mixed-Signal)，但它还是在电路的范畴内。可不可以把它扩展到其他领域(Mixed-Domain/Multiple Discipline)，比如机械、热力甚至生物领域？答案是可以的。例如，在电路领域中，我们解的是跨过两个节点的电压和通过一个支路的电流。而在机械领域中，我们解的是两个点的位置和力。从早期Saber的MAST语言，到现在的工业标准Verilog-AMS和VHDL-AMS都已经支持不同领域的描述。这就给跨领域的仿真带来了可能。虽然Verilog-AMS还没有被模拟电路设计者广泛采用，但它很可能先从另一个地方发扬光大。比如，微机电系统(MEMS)很有可能是下一个大的应用领域。
　　另一方面，虽然SPICE可以解很多类型的电路，但它的运算速度也因此受到了制约。每一种电路都有它自己的特点，比如数字电路信号的离散型，存储器(RAM)结构的重复性，等等。我们可以在SPICE的基础上，利用这些电路的特点来开发特制的“SPICE”以提高仿真的效率。前面说的快速SPICE仿真工具就属于这一类。它们的通用性不如SPICE，但它们针对某一类电路的仿真效率是非常高的。
　　最后一方面，我们从SPICE的发展可以清晰的看到，软件的发展是与硬件的发展密不可分的。现在的处理器基本上都是多核、多线程的，新一代的商业SPICE也利用了这些新的处理器架构。最新的图形处理器(GPU)更是达到了上百个核，上万个线程。并行的开发工具像开放计算语言(OpenCL)，CUDA也逐渐成熟。高性能计算(HPC)以及云计算也在日益普及。SPICE能否利用这些新的环境来提高仿真效率呢？呵呵，这个问题就需要你来解答了。
下面的图给出了主要SPICE的发展过程。其中的代号如下：
<style>
    tr td,th{
        border:2px solid lightgrey;
    }
    .mt{
        border-collapse: collapse;
    }
</style>
<table class="mt">
    <tr>
        <th>代号</th>
        <td>UCB</td>
        <td>gEDA</td>
        <td>Meta</td>
        <td>SNPS</td>
        <td>μSIM</td>
        <td>CDN</td>
        <td>MENT</td>
    </tr>
    <tr>
        <th>意义</th>
        <td>伯克利</td>
        <td>GNU EDA</td>
        <td>Meta-Software</td>
        <td>Synopsys</td>
        <td>MicroSIM</td>
        <td>Cadence</td>
        <td>Mentor</td>
    </tr>
</table>

<div align="center">
<img src="https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20230904094920.png">
</div>

　　下面的图给出了主要快速仿真工具的发展过程，“+”代表并购。
　　注意这些快速仿真工具都是商业化的。目前还没有一个开源的快速仿真工具具有像伯克利SPICE那样广泛的影响力。

<div align="center">
<img src="https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20230904144358.png">
</div>

　　哪里可以找到SPICE仿真模型？
　　寻找SPICE模型最好的方法就是去浏览厂商或是制造商的网页。下面列出的是部分最常用的并且在其网站上提供SPICE模型的芯片制造商。
|<center>厂商</center>|<center>描述</center>|
|----|----|
|Analog Devices <img width=420/> |放大器和比较器、AD/DA、嵌入式处理与DSP、MEMS和传感器、RF/IF组件、开关/多路复用器、模拟微控制器、接口、电源与温度管理|
|Analog and RF Models|模拟与射频模型|
|Apex Microtechnology|线性放大器、PWM放大器|
|Christophe Basso|开关电源|
|Coilcraft, Inc.|功率磁技术、射频电感、EMI/RFI滤波器、宽带磁技术|
|Directed Energy|二极管、开关模式MOSFETs、HF/VHF线性MOSFET、MOSFET驱动IC|
|Duncan Amps|放大器、真空管|
|Fairchild Semiconductors|放大器与比较器、二极管与整流二极管、接口、数字逻辑设备、信号转换、电压频率转换器、微控制器、光电管、开关、功率控制器、功率驱动器、晶体管、滤波器、稳压器|
|Infineon Technologies AG|光纤、微控制器、功率半导体、小型信号离散原件|
|International Rectifier|HEXFET功率MOSFET、二极管、桥、晶闸管、继电器、高压IC、只能功率模块、只能功率开关、HiRel功率MOSFETs、HiRel高压门驱动器|
|Kemet|含有铝、陶瓷和钽的表面覆盖电容器与含有陶瓷和钽的贴片电容|
|Linear Technology|信号调理、数据转换、功率管理、接口、高频率与光学|
|Maxim|放大器与比较器、模拟开关与多路复用器、时钟、计数器、继电器线路、振荡器、RTC、数据转换器、采样与保持、数字电势剂、光纤通信、滤波器(模拟)、高频ASIC、热插拔与功率开关、接口与互联、内存：暂时、非暂时、多功能、温度管理、传感器、传感器调理、电压参考、无线、射频与电缆|
|National Semiconductor|放大器、功率管理、温度传感器、接口、LVDS、以太网、USB技术、Micro SMD|
|ON Semiconductor|功率管理、放大器、比较器、模拟开关、晶闸管、二极管、整流器、双极性晶体管、FET、标准逻辑、差分逻辑|
|Philips|模拟/线性、音频、汽车、连接器、数据/媒体/视频处理、离散、显示器、接口与控制、逻辑、微控制器、功率与功率管理、射频、传感器|
|Polyfet|Polyfet晶体管|
|Protek|瞬态电压抑止|
|SMPS Power Supplies|开关电源仿真|
|Supertex|混和信号半导体、高压接口产品|
|ST Microelectronics|放大器与线性IC、模拟与混和信号IC、二极管、EMI滤波与调理、逻辑、信号开关、内存、微控制器、电源管理、保护设备、传感器、智能卡IC、晶闸管与交流开关、晶体管|
|Texas Instruments|缓冲器、驱动器与收发器、触发器、锁存存器、门、计数器、解码器/编码器/多路复用器、数字比较器|
|Tyco Electronics(前身Amp)|电磁元件、无源元件、电源、射频与微波产品|
|Vishay|模拟开关、电容、二极管、电感、集成模块、功率IC、LED、功率MOSFET、电阻以及热敏电阻的制造商。|
|Zetex|直流—直流变换控制器、参考电压源、电流监控、电机控制、Acoustar™声音解决方案、线性稳压器|