---
title: VIM学习笔记--状态行
date: 2023-12-06 23:33:25
tags: VIM
categories: 
- [技术兴趣,软件应用]
---
<center>状态行参数列表</center>

|参数|说明|
|----|----|
|%(...%)|定义一个项目|
|%{n}*|%对其余的行使用高亮显示组```User n```，知道另一个```%n*```。数字```n``` 必须从```1```到```9```。用```%*```或```%0*```可以恢复正常的高亮显示。|
|%<|如果状态行过长，在何处换行。缺省是在开头。|
|%=|左对齐和右对齐之间的分割点。|
|%|字符```%```|
|%B|光标下字符的十六进制形式。|
|%F|缓冲区的文件完整路径。|
|%H|如果为帮助缓冲区则显示为```HLP```。|
|%L|缓冲区中的行数。|
|%M|如果缓冲区修改过则显示未```+```。|
|%N|打印机页号。|
|%O|以十六进制方式显示文件中的字符偏移。|
|%P|文件中光标前的```%```。|
|%R|如果缓冲区只读则为```RO```。|
|%V|列数。如果与```%c```相同则为空字符。|
|%W|如果窗口为预览窗口则为```PRV```。|
|%Y|缓冲区的文件类型，如 vim。|
|%a|如果编辑多行文本，这个字行串就是```({current} of {arguments})```，例如: ```(5 of 18)```。如果在命令行中只有一行，这个字符串为空。|
|%b|光标下的字符的十进制表示形式。|
|%c|列号。|
|%f|缓冲区的文件路径。|
|%h|如果为帮助缓冲区显示为```[Help]```。|
|%l|行号。|
|%m|如果缓冲区已修改则表示为```[+]```。|
|%n|缓冲区号。|
|%o|在光标前的字符数(包括光标下的字符)。|
|%p|文件中所在行的百分比。|
|%r|如果缓冲区为只读则表示为```[RO]```。|
|%t|文件名(无路径)。|
|%v|虚列号。|
|%w|如果为预览窗口则显示为```[Preview]```。|
|%y|缓冲区的文件类型，如```[vim]```。|
|%{expr}|表达式的结果。|

<!--more-->

例:
```vim
:set statusline=%F%m%r%h%w\ [FORMAT=%{&ff}]\ [TYPE=%Y]\ [POS=%04l,%04v][%p%%]\ [LEN=%L]
```

定义显示颜色
```vim
:set statusline=%2*%n%m%r%h%w%*\ %F\ %1*[FORMAT=%2*%{&ff}:%{&fenc!=''?&fenc:&enc}%1*]\ [TYPE=%2*%Y%1*]\ [COL=%2*%03v%1*]\ [ROW=%2*%03l%1*/%3*%L(%p%%)%1*]\
```

自定义高亮显示颜色
```vim
hi User1 guifg=gray
hi User2 guifg=red
hi User3 guifg=white
```

还可以通过在```.vimrc```文件中包括以下命令，使状态行根据状态的不同，显示不同的颜色。
```vim
function! InsertStatuslineColor(mode)
if a:mode == 'i'
  hi statusline guibg=peru
elseif a:mode == 'r'
  hi statusline guibg=blue
else
  hi statusline guibg=black
endif
endfunction
au InsertEnter * call InsertStatuslineColor(v:insertmode)
au InsertLeave * hi statusline guibg=orange guifg=white
hi statusline guibg=black
```