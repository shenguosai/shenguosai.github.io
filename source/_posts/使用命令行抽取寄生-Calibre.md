---
title: 使用命令行抽取寄生(Calibre)
date: 2023-12-03 23:12:02
tags: Calibre
categories:
- [半导体专业,EDA工具]
---
```
% calibre -lvs -hier -auto rule.file
% calibre -xrc -pdb -rcc -xcell cell.file rule.file
% calibre -xrc -fmt -all -xcell cell.file rule.file
```
rule.file 中应指定layout(gds)的路径及文件名
[Instructions for Extracting Parasitics for your Layout using xCalibre Tool](https://www.engr.colostate.edu/ECE571/class_materials/xcalibre_instructions.htm)