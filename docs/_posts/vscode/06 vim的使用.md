---
draft: true
title: vim 的常用操作命令
vssue-title: vim001
date: 2020-12-14
tags:
  - vim
---

> 执行 git commit 时又遇到了 vim 的编辑模式.. 
<!-- more -->

## git commit 中的使用

按 i 进行文字读写

读写后 :wq 进行保存退出

通常单步执行： git commit -m ""，当文字较多的时候就调用 vim 编辑模式

## vim 认知

刚开始用 VIM 打开文件的时候，需要从宏观的去了解一下 VIM 这个编辑器。

VIM 有四个模式：

### 正常模式 (Normal-mode)

正常模式一般用于浏览文件，也包括一些复制、粘贴、删除等操作。这时击键时，一些组合键就是 vim 的功能键，而不会在文本中键入对应的字符。

在这个模式下，我们可以通过键盘在文本中快速移动光标，光标范围从小到大是字符、单词、行、句子、段落和屏幕。
启动 VIM 后默认位于正常模式。不论是什么模式，按一下 <Esc> 键 ( 有时可能需要按两下，插入模式按一下 Esc ，就会切换到正常模式，命令模式或者可视模式下执行完操作以后，就会自动进入正常模式，如果进入命令模式或者可视模式没有执行任何操作，按两下 Esc 即可 ）都会进入正常模式。

下面的三个模式都是过键盘上相应的键位去触发的。

### 插入模式 (Insert-mode)

按 i 键会进行插入模式。该模式启动以后，就会进入编辑状态，通过键盘输入内容。

### 命令模式 (Command-mode)

在正常模式中，按下：（冒号）键或者/ （斜杠），会进入命令模式。在命令模式中可以执行一些输入并执行一些 VIM 或插件提供的指令，就像在 shell 里一样。这些指令包括设置环境、文件操作、调用某个功能等等。

### 可视模式 (Visual-mode)

在正常模式按下 v, V, <Ctrl>+v，可以进入可视模式。可视模式中的操作有点像拿鼠标进行操作，选择文本的时候有一种鼠标选择的即视感，有时候会很方便。
（通常 VIM 中复制一段形状不规则的代码就是通过可视模式进行复制的）

以上是关于 VIM 四种模式的解读，我们在使用 VIM 操作文本的时候，编辑区底部一般都会显示当前处于什么模式下  
（插入模式会有 INSERT 提示，可视模式会有 VISUAL 或者 VISUAL LINE 的提示）。

## Esc

VIM 有一个很重要的按键需要一开始就做出说明，那就是键盘中的 <ESC> , 这个按键用来切换模式，该按键可以快速切换到正常模式。
<ESC> 这个按键有点特殊，它脱离了主键盘区，每次操作这个按键的时候都会有些蛮烦。估计很多使用 VIM 的人都会有这个痛点，因此有了一个解决方案，control + [ 这两个按键取代 <ESC>。

曾经很长一段时间我都是用 control + [ 用来取代 <ESC> ，但是还是感觉有些难受？

VIM 有一个配置文件，在 linux 系统中，该配置文件是 `.vimrc` , 该文件位于 ～ 目录下面 （～ 目录是家目录，也就是用户目录），是一个隐藏文件，如果该文件不存在可以手动创建一个。

`.vimrc` 可以有很多配置，例如显示行号，快捷键配置，插件配置等等。  
VIM 很多个性化的设置都离不开这个配置文件。`.vimrc` 有一个特别重要的配置，那就是配置如下的一行：

```
#将 ESC 键映射为两次 q 键  ,`#` 开头的为注释                         
inoremap qq <Esc>
```

这个配置是将 <ESC> 功能键用 qq （连续按两次 q) 来取代。这个配置可以很大程度提高 VIM 的使用效率 否则退出文件的时候有些远离键盘不太雅观

具体用什么来替换 Esc 还是看个人爱好，反正离键盘的中区域应该近一些
## 用 vim 打开文件

### 用 vim 打开一个文件

现在假如有一个文件 file1 , 只需要在文件前面加上 vim 关键字就好：
`vim file1`
上面这个命令将会打开 file1 这个文件，file1 是指你具体操作的文件名
当不存在这个文件的时候，就进行创建，使用该命令进入 vim 后 直接进入 vim 的插入模式

### 用 VIM 一次性打开多个文件

现在有多个文件 `file1 ，file2 , ... ,filen.`

现在举例打开两个文件 `file1，file2`

`vim file1 file2`

该方式打开文件，显示屏默认显示第一个文件也就是 file1，  
VIM 的正常模式下按下键盘上的冒号 `：`这时会在显示屏底部出现冒号` ：`（进入了 VIM 的命令模式），  
然后在输入 ls （ls 为调出列表命令），  
屏幕上会出现打开的所有文件的序号和文件名，我们继续输入冒号 ： ，然后输入 bn （这里的 n 需要做一个解释并不是键盘上的 n , 而是文件序号的代指，如 b1 代表显示屏上切换到第一个文件，b2 代表显示屏上切换到第二个文件）。

`:ls`

上面这个命令将会列出 VIM 打开的所有文件。

`:b2`

上面的这个命令将会在显示屏上显示第二个文件。

### 在显示屏上一次性显示多个文件

VIM 可以实现分屏操作，一个屏幕被多个文件给分占，有左右和上下两种分屏的方式。

左右分屏如下操作：

`vim -On file1 file2 ... filen`

这里的 n （ n 是要打开的具体文件的数目：1,2,3 ...）是代表有几个文件需要分屏，从左至右依次显示 n 个文件。

上下分屏如下操作：

`vim -on file1 file2 ... filen`

这个命令跟上一个命令不同的是其中的参数 -on（ n 是要打开的具体文件的数目：1,2,3 ...） 中的 o 是小写，这样将会上下依次显示 n 个文件。

上下分屏显示文件使用小写 o 记号+n 
左右分屏显示文件使用大写 O 记号+n

`vim -O2 test01.cpp test02.cpp `

## 分屏操作

记住一个重要的组合键 `Ctrl + w`, 操作分屏离不开这个组合键  （关于窗口的操作 windows-w）

### vim 文件分屏

按住组合键 Ctrl + w ，然后在按下 s

`Ctrl + w s`

上面这个命令将会上下分割当前打开的文件。

按住冒号`：`，紧接着输入 `sp` , 在键入文件名，如下：

`:sp file`

上面的这个命令将会上下分割当前文件和新打开的 file 。

按住组合键 `Ctrl + w `, 然后在按下` v`

`Ctrl +w v`

上面的这个命令将会左右分割当前的文件

按住冒号 `：`，紧接着输入 `vsp` , 在键入文件名称，如下：

`:vsp file`

上面的这个命令将会左右分割当前打开的文件和新打开的文件 file 。

### 切换光标，移动分屏

1. 切换左右分屏的光标 ：

将当前光标定位到左边的屏幕 `Ctrl + w h`

将当前的光标定位到右边的屏幕 `Ctrl + w l`

（因为 vim 和 linux 中使用 hjkl 代表上下左右，所以 ctrl+w+h 代表将光标移动到左屏幕，ctrl+w+l 代表将光标移动到右边的屏幕，而不是使用 lr 代表左右，这里的键盘的地理位置和功能也对应，h 在左，l 在右）

2. 移动左右分屏 ：

将当前的分屏移动到左边 `Ctrl + w H`

将当前的分屏移动到右边 `Ctrl + w L`

与移动光标操作不同的是，需要 caps 进行锁定，大写字母为分屏的相关操作

3. 切换上下分屏的光标 ：

将当前的光标移动到下面的分屏 `Ctrl + w j`

将当前光标移动到上面的分屏 `Ctrl + w k`

4. 移动上下分屏：

将当前的分屏移动到下面的分屏 `Ctrl + w J`

将当前的分屏移动到上面的分屏 `Ctrl + w K`

### 关闭分屏

这个命令是关闭当前的分屏
`Ctrl + w c` （close）

上面的这个命令也是关闭当前的分屏，如果是最后一个分屏将会退出 VI 不太行
`Ctrl + w q`

## VIM 的退出

VIM 的最终操作就是 VIM 的退出，命令模式下执行以下操作就可以实现对应的功能

保存当前对文件的修改，但是不退出文件。`:w`

强制保存但是不退出文件。`:w!`

保存当前的文件修改到 file 文件当中。`:w file`
（例如写了一个 cpp 文件，然后执行这个命令就可以将写好的内容放到另一个已经创建好的 cpp 文件，就是 windows 里面的：test.cpp 已存在，是否覆盖？）

退出文件，对文件的修改不做保存。`:q!` （相当于强制退出）

退出所有的文件，对所有的文件修改都不做保存。`:qa!` （quit all ！）

退出文件并保存对文件的修改。`:wq` （这个就是最常用的~）

退出文件并保存对文件的修改。`:x` （功能等价：wq）

打开另一个文件。`:e file` （edit-e）

放弃对文件的所有修改，恢复文件到上次保存的位置。`:e!`（代表文件编辑失效，edit ！）

另存为 file。`:saveas file` （saveas 没有空格）

当打开多个文件的时候可以输入 :bn 和 :bp 进行上一个文件或者下一个文件的切换。`:bn 和 :bp`  
（这个之前有说）切换后再进行想添加的操作

## 使用 VIM 编辑文本

这里有必要再强调一下，在使用 VIM 打开文件的时候，这时候的状态是正常模式（Normal-mode）, 请务必记住这个模式，如果你不确定当前是否处在正常模式，请连续按两下键盘上的 ESC（或者自定义以后的按键：qq），VIM 处理编辑文本需要从正常模式 (Normal) 切换到插入模式 (Insert-mode), 进入插入模式的时候你应该会在屏幕底部看到提示，这时候就可以编辑文本了。

（现在的 vim 都已经存在汉化了，左下角提示为插入/INSERT，可视，等等）

### 从正常模式进入插入模式

请记住下面几个常用启动录入文本的键盘字符 `i,I,a,A,o,O,s,S` 。（一个 ios，aios）

`i` 是在光标所在的字符之前插入需要录入的文本。

`I` 是在光标所在行的行首插入需要录入的文本。

`a` 是在光标所在的字符之后插入需要录入的文本。

`A` 是在光标所在行的行尾插入需要录入的文本。

`o` 是光标所在行的下一行行首插入需要录入的文本。

`O` 是光标所在行的上一行行首插入需要录入的文本。

`s` 删除光标所在处的字符然后插入需要录入的文本。

`S` 删除光标所在行，在当前行的行首开始插入需要录入的文本

vim 中的光标是宽的，不是一个窄条，所以存在 s 操作

还有一个可能经常用到的就是 cw ，删除从光标处开始到该单词结束的所有字符，然后插入需要录入的文本（这个命令是两个字符的合体 cw ）。相当于 Windows 窗口下的鼠标双击进行的单词替换

## VIM 的命令模式

关于命令模式上文有提到过，下面主要来列举几个常用的命令模式操作（命令输入完以后，需要按下 Enter 键去执行命令，和 Bash，shell 中一样）：

### 行号

文本的行号设置最好不要设置在配置文件中（因为复制文件的时候行号的出现会很麻烦，还得删），在命令行实现就好。

该命令会显示行号`:set nu`

该命令会取消行号。`:set nonu`

定位到 n 行 `:n`

### VIM 进行关键字的查找。

`/{目标字符串}` ：会在文本中匹配相应字符串的地方高亮。（有两种命令模式的起始调用方式~`:` `/`）

查找文本中匹配的目标字符串，查到以后，输入键盘上的 n 会去寻找下一个匹配，N 会去寻找上一个匹配。  
(next)

### VIM 处理大小写的区分

编辑器将不会区分大小写，如果你进行该设置之后，进行关键字查询只要是字符相同不会区分大小写都会进行匹配。

`:set ic`

区分大小写的查询。

`:set noic`

### VIM 删除多行文本

n1 和 n2 指的是起始行号和结束行号，d 是删除关键字 `:n1,n2d` （开启行号显示后挺好用的）

### VIM 处理文本的替换

`substitute:`
动词：替代，替换，代替，取代，代，替，取而代之，抵   
名词：替身，代替者，替补员

`:{作用范围}s/{目标}/{替换}/{替换的标志}`

作用范围分为当前行、全文、选区等等。

将会把当前光标所在行的 ugly 替换成 handsome  `:s/ugly/handsome/g`

将会把全文中的 ugly 替换成 handsome `:%s/ugly/handsome/g`

（无论作用范围是什么都要添加`:..s`）

这里的 n1 和 n2 值得是行号，将会替换掉 n1 到 n2 的所有 ugly 为 handsome.
`:n1,n2s/ugly/handsome/g`

选区，在可视模式下选择区域后输入 : ，VIM 会自动补全为 `:'<,'>` ,（可是模式为鼠标可以操作的那个模式）

`:'<,'>s/ugly/handsome/g`

这个操作咋一看起来有点懵逼，这个操作是可视模式 (Visual-mode) 下选区中的替换操作（可视模式下文会谈到），可视模式下输入：会自动补全 :'<,'> 这个是可视范围下的操作范围，类似于 % 和 n1,n2，代表操作的文本范围，上面的例子就是替换掉可视区域的 ugly 为 handsome。 

> 当前行 全文：%  和两种选区替换 ：n1 n2 可视化选取替换

下面来谈谈替换的标志。  
上文中命令结尾的 g 即是替换标志之一，表示全局 global 替换（即替换目标的所有出现）。 还有很多其他有用的替换标志：

空替换标志表示只替换从光标位置开始，目标的第一次出现

`:s/ugly/handsome`

作用于当前行，从光标处开始查找替换，仅仅替换第一次匹配 ugly 的地方为 handsome 。

`:%s/ugly/handsome`

替换掉文件中所有行第一次出现 ugly 的地方为 handsome 。

i 表示大小写不敏感查找，I 表示大小写敏感： `:%s/ugly/handsome/i`

替换掉所有行第一个出现 ugly （不区分大小写） 为 handsome `:%s/ugly/handsome/gi`

替换掉所有行出现 ugly （不区分大小写） 为 handsome 。

c 表示需要确认，例如全局查找"ugly"替换为"handsome"并且需要确认：（用来以防万一手抖等等等）  
`:%s/ugly/handsome/gc`

## VIM 执行 Linux 命令

`:!command`

: 后面紧跟着 ! ，! 后面紧跟着 linux 命令  
（ command 指操作 Linux 系统的一系列命令，如创建文件，新建文件夹，查询文件的属性的等），例子如下，

:!date （输出日期，提醒用户输入新日期）

执行 date 命令显示时间，执行完命令以后按下键盘上的 Enter 就会返回到文件。

### 添加结果至操作文本光标处

`:r !command`

: 后面紧跟着 r , r 后面是空格，紧接着是 !command( command 解释同上），例子如下，

`:r !date`

执行 date 命令显示时间，并且添加命令结果到文本中。

## 定义快捷键

下面举例说明：

`:map ^M I# <ESC>`

上面的例子也就是通过快捷键 Ctrl + m 在文件光标处所在行的行首插入 # （ # 代表注释）。

: 后面的 map 是关键字 ，后面是 key 和 value 。

key 对应的是 ^M ， 这个 key 需要强调一下 ^M 是 Ctrl + v + m 打出来的（按下这三个键，VIM 会显示成 ^M ）,^M 代表快捷键是 Ctrl + m , Ctrl + v + n 就是 ^N , 代表快捷键是 Ctrl + n 。Ctrl + v + x 就是 ^X （这里的 x 是代表 26 个字母中的任意一个） 代表快捷键 Ctrl + x。

value 对应的是 I#<ESC>, 表示按下快捷键以后执行的相应操作，I 是切换光标至行首并切换到编辑模式，#是行首输入的内容（ # 是 VIM 文件中的注释符号 ），<ESC> 是退出编辑模式。

举例如下：

`:map ^D Ahelloworld<ESC>` 表示在文件的光标所在行的行尾，添加 helloworld 字符串，按住组合键 `ctrl + d` 就会执行操作。

a 是在光标所在的字符之后插入需要录入的文本。  
A 是在光标所在行的行尾插入需要录入的文本。  

## 使用 ab（长字符串替换）

`:ab email kickcodeman@gmail.com`

: 后面的 ab 是关键字 , 该命令执行后，然后切换到编辑模式下，输入 email 会把输入的 email 自动替换成 kickcodeman@gmail.com。

这个命令主要是处理频繁输入同样的长串字符串。

## VIM 的正常模式（Normal-model）

VIM 正常模式下，主要进行的操作有光标的移动，复制文本，删除文本，黏贴文本等。
### 快速移动光标

几个重要的快捷键

请记住这几个快捷键 h,j,k,l 这几个按键主要是用来快速移动光标的，h 是向左移动光标，l 是向右移动光标，j 是向下移动光标，k 是向上移动光标，h , j , k ,l 在主键盘区完全可以取代键盘上的 ↑ ,↓ ,← , → 的功能（上下左右）。

### 在当前行上移动光标

`0` 移动到行头 //O 无中央的像素点，而 0 有中间的像素点，正常模式按 0 就可以将光标移动到选中行的行头

`^` 移动到本行的第一个不是 blank 字符（TY 中间上方，小键盘 6 对应的位置）

`$` 移动到行尾 （4 对应的符号）

`g_` 移动到本行最后一个不是 blank 字符的位置

`w` 光标移动到下一个单词的开头

`e`光标移动到下一个单词的结尾 （常用）

`fa` 移动到本行下一个为 a 的字符处，fb 移动到下一个为 b 的字符处 （find）

`nfa` 移动到本行光标处开始的第 n 个 字符为 a 的地方（n 是 1，2，3，4 ... 数字）

`Fa` 同 fa 一样，光标移动方向同 fa 相反

`nFa` 同 nfa 类似，光标移动方向同 nfa 相反

`ta` 移动光标至 a 字符的前一个字符

`nta` 移动到第 n 个 a 字符的前一个字符处

Ta 同 ta 移动光标方向相反

`nTa` 同 `nta` 移动光标方向相反

; 和，当使用 f, F, t ,T, 关键字指定字符跳转的时候，使用 ；可以快速跳转到下一个指定的字符，, 是跳到前一个指定的字符

### 跨行移动光标

`nG` 光标定位到第 n 行的行首

`gg` 光标定位到第一行的行首

`G` 光标定位到最后一行的行首

`H` 光标定位到当前屏幕的第一行行首

`M` 光标移动到当前屏幕的中间

`L` 光标移动到当前屏幕的尾部

`zt` 把当前行移动到当前屏幕的最上方，也就是第一行

`zz` 把当前行移动到当前屏幕的中间 （常用）

`zb` 把当前行移动到当前屏幕的尾部

`%` 匹配括号移动，包括 ( , { , [ 需要把光标先移动到括号上

`*` 和 # 匹配光标当前所在的单词，移动光标到下一个（或者上一个）匹配的单词（ `*` 是下一个，# 是上一个）
8 和 3 对应的额外字符


### 翻页操作

`ctrl+f` 查看下一页内容

`ctrl+b` 查看上一页内容


### vim 的复制 粘贴 删除

三个重要的快捷键 `d , y , p` （删除，复制，粘贴）

d 是删除的意思，通常搭配一个字符 ( 删除范围 ) 实现删除功能，常用的如下：

dw 删除一个单词

dnw 删除 n 个单词，

dfa 删除光标处到下一个 a 的字符处（ fa 定位光标到 a 处 ）

dnfa 删除光标处到第 n 个 a 的字符处

dd 删除一整行

ndd 删除光标处开始的 n 行

d$ 删除光标到本行的结尾

dH 删除屏幕显示的第一行文本到光标所在的行

dG 删除光标所在行到文本的结束



y 是复制的意思，通常搭配一个字符（复制范围）实现复制的功能，常用的如下：

yw 复制一个单词，还有 ynw

yfa 复制光标到下一个 a 的字符处,还有ynfa

yy 复制一行，还有 nyy

`y$` 复制光标到本行的结尾

yH 复制屏幕显示的第一行文本到光标所在的行

yG 复制光标所在行到文本的结束



p ，P是黏贴的意思，当执行完复制或者黏贴的命令以后，VIM 会把文本寄存起来。

p 在光标后开始复制

P 大写的 P 光标前开始复制

### 撤销操作和恢复

`u` 撤销刚才的操作

`ctrl + r` 恢复撤销操作


### 删除字符操作和替换


x 删除光标当前所在的字符

r 替换掉光标当前所在的字符

R 替换掉从光标开始以后的所有字符，除非 <ESC > 退出，或者 jj （代替 <ESC> 上文有提到）退出。


### 大小写转换

~ 将光标下的字母改变大小写
3~ 将光标位置开始的3个字母改变其大小写
g~~ 改变当前行字母的大小写
gUU 将当前行的字母改成大写
guu 将当前行的字母全改成小写

3gUU 将从光标开始到下面3行字母改成大写
gUw 将光标下的单词改成大写。
guw 将光标下的单词改成小写

### VIM 的重复命令

. 该命令是重复上一个操作的命令
`n<command>` 重复某个命令 n 次，
如 10p 复制 10 次，10dd 删除十次。

## VIM 可视化模式（Visual-mode)

v,V,Ctrl+v

v字符可视化，按下键盘上的v以后，屏幕底部应该会有一个 VISUAl 的提示，操作 h,j,k,l就选中文本，继续按 v 退出可视化模式。

V 行可视化，按下键盘上的 V 以后，屏幕底部应该有一个 VISUAL LINE 的提示，操作 j,k 可以向上或者向下以行为单位选中文本，继续按下 V 退出可视化模式。

Ctrl+v 块状可视化，按下键盘上的 Ctrl+v 以后，屏幕底部应该会有一个提示 VISUALBLOCK ，可以通过 h,j,k,l 块状的操作选择区域，这是很多编辑器都不可以做到的，继续按下 Ctrl+v 会退出可视化模式。

### 可视化模式下操作文本

可视化模式下选择操作区域以后，
按下 d会删除选择的区域，
按下 y 会复制选择的区域，按下 p 会黏贴选择的区域。


### 可视化模式下 v 的特殊操作

当操作的文本光标在 “”，‘’ ，（），{} ，[（双引号，单引号，小括号，大括号，中括号）
当中的时候,可以通过 va"选中 ”“ 内的所有内容包括双引号 ，vi" 选中 "" 内的所有内容，不包括 ""。va,vi 会快速选择区域，va 后面会紧跟一个区域结束标志，a 会选中结束符标志，i 就不会。例子如下：

"hello world [VIM is so strong],{we all can master vim skill}"

假设当前光标定位在上面的文本 M 处：
va] 操作将会选中以下文本（加粗部分）：
“hello world [VIM is so strong],{we all can master vim skill}“
vi] 操作将会选中如下的区域，没有包含 []：
“hello world [VIM is so strong],{we all can master vim skill}“


### 块区域下的特殊操作

Ctrl+v 选中块区域以后，按下大写的 I 或者 A 可以在区域的前面或者后面输入内容，按下 jj 或者 <ESC>,可以看到选中的区域前面或者后面会有输入的内容。

### VIM 的代码提示功能

在编辑模式下 ，快捷键 Ctrl+n 或者 Ctrl+p 会有代码提示功能，我们可以实现快速录入的效果。


VIM 的宏录制

假设需要操作的文本如下,需要将如下的多行文本的首行键入一个 tab 键。

hello
hello world
hello world , vim


## 宏录制的录制操作

先将光标移动到第一行，在普通模式下按下 q 键（宏录制是 q 键启动的),在按一个 a （字母随意）,表示该宏注册为 a ，按下 I 在行首插入一个 tab 键，按下jj或者 <ESC>退出编辑模式,按下 j 将光标移动到下一行行首，最后按下 q 键完成录制操作（宏录制是 q 键结束的）。
总结上面例子的操作流程：
q → a → I → tab → jj → j → q
上面的例子成功地把在行首插入 tab 的功能录制了下来，那么如何应用到其他行呢？


## 宏录制的使用

上述的例子，在正常模式下，按 @a执行宏录制的一系列动作，将会在第二行执行插入 tab 。
@@ 是对上一次宏使用的重复操作。n@a 就会执行 n 次一系列的动作。使用宏录制可以一次执行一系列的操作，可以针对一些重复度较高的操作进行宏录制。

