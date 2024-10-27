---
title: EDA工具使用备忘
date: 2024-02-01 08:49:38
tags: Virtuoso
categories:
- [半导体专业,EDA工具]
---

## Cadence
#### Virtuoso
1. .cdsinit: 负责Cadence软件的初始化配置，包括加载快捷方式、嵌入Calibre软件接口等。
<!--more-->
   - 示例存放目录:```Candence安装目录/tools/dfII/samples/local/cdsinit```
   - 读取顺序: 
        > *<font color="#d3d3d3">Cadence安装目录</font>*/tools/dfII/samples/local/cdsinit => *<font color="#d3d3d3">用户home目录</font>*/.cdsinit => *<font color="#d3d3d3">virtuoso启动目录</font>*/.cdsinit
   - 设置默认读取的display.drf文件: 
        <pre>drLoadDrf(<em>filePath</em>)</pre>
   - 设置默认快捷键文件:
     - 添加单个快捷键文件
        <pre>load("/home/IC/Desktop/workspace/analog_ic/set_env/bindKeys_ICSkillSharing.il")</pre>
     - 添加多个快捷键文件
        ```skill
        let((bindKeyFileList file path saveSkillPath)
            bindKeyFileList = '(
                "leBindKeys.il"
                "schBindKey.il"
                "bindKeys_ICSkillSharing.il"
            )
        )
        ; This is the path that is searched for the files
            path = strcat(
        ; If you want to add another path add it here as a string
                ". ~/home/IC/Desktop/workspace/analog_ic/set_env "
                prependInstallPath("local")
                prependInstallPath("samples/local")
            )
        saveSkillPath = getSkillPath()
        saveSkillPath(path)

        foreach(file bindKeyFileList
            if(isFile(file) then
                loadi(file)
            )
        )
        setSkillPath(saveSkillPath)
        ```
   - 快捷键设置:
     - 设置快捷键“0”消除高亮net|按“0”后鼠标点击欲消除的net:
        <pre>hiSetBindKey("Schematics" "<Key>0" "geEnterDeleteNetProbe()")</pre>
     - 设置快捷键“-”消除所有高亮net: 
        <pre>hiSetBindkey("Schematics" "<Key>-" "geDeleteAllProbe(getCurrentWindow() t)")</pre>
     - 设置快捷键“F6”打开Library Manager(启动Virtuoso后):
        <pre>hiSetBindKey("Command Interpreter" "<Key>F6" "ddsOpenLibManager()")</pre>
     - 设置快捷键“Shift+S”在上层schematic界面下编辑Symbol:
        <pre>hiSetBindKey("Schematics" "Shift<Ket>S" "schHiEditInPlace()")</pre>
    - 设置仿真结果默认存放目录为“/DATA/shenguosai/simulasion”:
        <pre>envSetVal("asimenv.startup" "projectDir" 'string "/DATA/shenguosai/simulasion")</pre>
    - 调用其它软件接口:
      - Calibre
        <pre>load("<em>Calibre_install_dir</em>/lib/calibre.skl")
      - HSPICE
        <pre>load("<em>HSPICE_install_dir</em>/interface/HSPICE.ile")
2. .cdsenv: 包含了Cadence软件的变量设置，用户通过对变量赋值，改变软件设置。
   - 示例存放目录: ```Candence安装目录/tools/dfII/samples/.cdsenv```
   - 读取顺序: 
        > *<font color="#d3d3d3">Cadence安装目录</font>*/tools/dfII/samples/.cdsenv => *<font color="#d3d3d3">用户home目录</font>*/.cdsenv => *<font color="#d3d3d3">virtuoso启动目录</font>*/.cdsenv
   - 设置仿真结果显示有效位数:(示例文件中有，通过搜索改变赋值即可,以下同)
        <pre>auCore.misc labelDigits int 6</pre>
   - 改变仿真结果保存路径:
        <pre>asimenv.startup projectDir string" ./simulation"</pre>
   - 按Library名称设置仿真结果保存路径:
        <pre>asimenv.startup appendLibNameToProjectDir boolean t
   - 设置原理图和版图中的 label 默认字体改为 roman:
        ```skill
        schematic createLabelFontStyle cyclic "roman"
        layout labelFontStyle cyclic "roman"
        ```
   - 设置版图网格点默认值:视工艺而定
        ```skill
        layout xSnapSpacing float 0.005
        layout ySnapSpacing float 0.005
        ```
   - 设置仿真波形与坐标显示:
        ```skill
        viva.graph  titleFont  string  "Default,14,-1,5,75,0,0,0,0,0"
        viva.rectGraph    background  string  "white"
        viva.axis     font   string   "Default,14,-1,5,75,0,0,0,0,0"
        viva.horizMarker font    string   "Default,14,-1,5,75,0,0,0,0,0"
        viva.vertMarker     font    string   "Default,14,-1,5,75,0,0,0,0,0"
        viva.referenceLineMarker    font  string    "Default,14,-1,5,75,0,0,0,0,0"
        ...
        viva.trace  lineThickness  string  "Thick"
        viva.trace  lineStyle  string  "solid"
        wavescan.trace  lineThickness  string  "Thick"
        wavescan.trace  lineStyle  string  "solid"
        ```
        其中```viva.axis font string "Defaut,14,-1,5,75,0,0,0,0,0"```中的 14 为字号；75 为颜色深度，可以理解为是否加粗，取值在0-99之间。
        > <b>注意</b>:仿真波形显示方法会与 PDK 中环境变量设置和 display.drf 文件中的设置有关，如果按照上面的方法设置完后不能达到实际效果，首先考虑是否是 PDK 自带脚本加载了其它软件变量或者使用的 display.drf 文件中做了相应修改，如果发现有问题可以修改 PDK 中环境变量或者 display.drf 文件的相应设置。
   - 实现在 Library Manager 界面双击 view 以只读方式打开:
        <pre>cdsLibManager.main dblClickEditCellView boolean nil</pre>
        > <b>特别注意</b>: 可能按照上面的方法设置完.cdsenv文件之后，发现设置并未生效，这时候需要注意一个问题，<b>Cadence每次启动时都会加载 .cdsinit 文件，然而对于 .cdsenv 文件并非如此，而且 .cdsenv文件中的变量值会被覆盖，具体软件加载 .cdsenv 文件的顺序需要再用户的环境变量设置文件中定义。</b>一般叫做 .bashrc (使用 export 定义环境变量)或者 .cshrc (使用 setenv 定义环境变量)。

        ```bash
        # 加载软件启动目录下的 .cdsenv 文件
        export CDS_LOAD_ENV = CWD(setenv CDS_LOAD_ENV CWD)
        
        # 首先加载用户主目录下的 .cdsenv 文件，然后加载软件启动目录下的 .cdsenv 文件。
        export CDS_LOAD_ENV = addCWD(setenv CDS_LOAD_ENV addCWD)
        ```
3. display.drf: 一般在工艺库中会有初始化的显示文件。这里面会有电路的连线粗细及颜色，器件的一些显示，版图layout里面的layer颜色等等，还有ADE波形图里面的线的属性。总之就是显示相关的一些设置。
   - 文件的修改保存: 当修改过显示后，每次退出 virtuoso 会显示一个保存显示信息，这里输入名称，比如display_1.drf，这个文件默认保存在打开 virtuoso 的文件夹下面。
   - 默认文件的设置: 在.cdsinit 文件里添加载入命令 ```drLoadDrf(filepath)``` 。
