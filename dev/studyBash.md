# 1.简介

Bash是Unix和Linux系统的一种shell（命令行环境），是目前大部分发行版Linux默认的shell。

本次Bash学习使用mac m1和parallels kali虚拟机设备。

项目原文地址：[有道-Bash脚本教程-原文地址](https://wangdoc.com/bashhttps://wangdoc.com/bash)

## 1.1.Shell的含义

<mark>Shell</mark>这个单词原意是外壳，跟kernel（内核）相对应，比喻内核外面的一层，即用户与内核的交互界面。

Shell是一个程序，提供一个与用户对话的环境。

Shell是一个命令解释器，解释用户输入的命令。它支持变量、条件判断、循环操作等语法，所以用户可以使用Shell命令写出各种小程序，又称为脚本（script）。

Shell是一个工具箱，提供各种小工具，供用户方便的操作系统的功能。

## 1.2.Shell的种类

Shell的主要种类如下：

| 序号  | Shell简称 | Shell全称                    |
|:---:| ------- | -------------------------- |
| 1   | sh      | Bourne Shell               |
| 2   | Bash    | Bourne Again Shell         |
| 3   | csh     | C Shell                    |
| 4   | tcsh    | TEXNEX C Shell             |
| 5   | ksh     | Korn Shell                 |
| 6   | zsh     | Z Shell                    |
| 7   | fish    | Friendly Interactive Shell |

本文中非特别说明，Shell与Bash做同义词使用，可以互换。

查看当前设备默认Shell

```bash
bash-3.2$ echo $SHELL
/bin/zsh
```

当前正在使用的Shell不一定就是Shell，通常`ps`命令结果的倒数第二行是当前的Shell（mac不是😄）。

```bash
bash-3.2$ ps
  PID TTY           TIME CMD
31850 ttys000    0:00.07 -zsh
32424 ttys000    0:00.02 bash
```

上例中倒数一行是我的正使用的Shell，`zsh`则是我的默认Shell。

查看当前系统安装的所有Shell：

```bash
bash-3.2$ cat /etc/shells
# List of acceptable shells for chpass(1).
# Ftpd will not allow users to connect who are not using
# one of these shells.

/bin/bash
/bin/csh
/bin/dash
/bin/ksh
/bin/sh
/bin/tcsh
/bin/zsh
```

上述三个例子中`$` 是命令环境的提示符，用户只需要输入提示符后面的内容。

## 1.3.命令行环境

### 1.3.1.终端模拟器

        终端模拟器：（terminal emulator）就是一个模拟命令行窗口的程序，让用户在一个窗口中使用命令行环境，并提供各种附加功能，如调整字体颜色、大小、行距等。

### 1.3.2.命令行提示符

```bash
[user@hostname]$
```

如上信息中就是提示符，提示符包含前缀和美元符号`$`：

`[user@hostname]`为前缀，前缀由用户名`user`和`@`加上主机名`hostname`组成；

`$`标识当前用户为普通用户。

`#`标识当前用户为根用户（root），具有根权限，执行各种操作务必小心。

### 1.3.3.进入和退出的方法

进入命令环境，一般就打开了Bash。如果你的Shell不是Bash，可以输入`bash`来启动Bash。

```bash
(base) hatred@子贡 Downloads % bash

The default interactive shell is now zsh.
To update your account to use zsh, please run `chsh -s /bin/zsh`.
For more details, please visit https://support.apple.com/kb/HT208050.
bash-3.2$
```

退出Bash 可以输入`exit`或按`control + D`

```bash
bash-3.2$ exit
```

显示当前所在目录

```bash
bash-3.2$ pwd
/Users/hatred/Downloads
```

## 1.4.Bash的历史

Shell伴随着Unix诞生而诞生。

| 序号  | 年份           | 事件                                                                                                                                              |
|:---:| ------------ | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | 1969年        | Ken Thompson 和 Dennis Ritchie 开发了第一版的 Unix。                                                                                                     |
| 2   | 1971年        | Ken Thompson 编写了最初的 Shell，称为 Thompson shell，程序名是`sh`，方便用户使用 Unix。                                                                               |
| 3   | 1973年至1975年间 | John R. Mashey 扩展了最初的 Thompson shell，添加了编程功能，使得 Shell 成为一种编程语言。这个版本的 Shell 称为 Mashey shell。                                                     |
| 4   | 1976年        | Stephen Bourne 结合 Mashey shell 的功能，重写一个新的 Shell，称为 Bourne shell。                                                                                |
| 5   | 1978年        | 加州大学伯克利分校的 Bill Joy 开发了 C shell，为 Shell 提供 C 语言的语法，程序名是`csh`。它是第一个真正替代`sh`的 UNIX shell，被合并到 Berkeley UNIX 的 2BSD 版本中。                           |
| 6   | 1979年        | UNIX 第七版发布，内置了 Bourne Shell，导致它成为 Unix 的默认 Shell。注意，Thompson shell、Mashey shell 和 Bourne shell 都是贝尔实验室的产品，程序名都是`sh`。对于用户来说，它们是同一个东西，只是底层代码不同而已。 |
| 7   | 1983年        | David Korn 开发了Korn shell，程序名是`ksh`。                                                                                                             |
| 8   | 1985年        | Richard Stallman 成立了自由软件基金会（FSF），由于 Shell 的版权属于贝尔公司，所以他决定写一个自由版权的、使用 GNU 许可证的 Shell 程序，避免 Unix 的版权争议。                                           |
| 9   | 1988年        | 自由软件基金会的第一个付薪程序员 Brian Fox 写了一个 Shell，功能基本上是 Bourne shell 的克隆，叫做 Bourne-Again SHell，简称 Bash，程序名为`bash`，任何人都可以免费使用。后来，它逐渐成为 Linux 系统的标准 Shell。   |
| 10  | 1989年        | Bash 发布1.0版。                                                                                                                                    |
| 11  | 1996年        | Bash 发布2.0版。                                                                                                                                    |
| 12  | 2004年        | Bash 发布3.0版。                                                                                                                                    |
| 13  | 2009年        | Bash 发布4.0版。                                                                                                                                    |
| 14  | 2019年        | Bash 发布5.0版                                                                                                                                     |

### 1.4.1 查看bash版本

查看当前Bash版本，可以使用`bash`的`--version`参数或着环境变量`$BASH_VERSION`。

```bash
bash-3.2$ bash --version
GNU bash, version 3.2.57(1)-release (arm64-apple-darwin22)
Copyright (C) 2007 Free Software Foundation, Inc.
#或者
bash-3.2$ echo $BASH_VERSION
3.2.57(1)-release
```

# 2.Bash 的基本语法

## 2.1.echo 命令

`echo`命令的作用是在屏幕中输出一行文本，可以将该命令的参数原样输出。

```bash
bash-3.2$ echo hello world
hello world
```

上述例子中，`echo`命令的参数是`hello world`，可以原样输出。

如果需要<mark>输出多行文本</mark>，即包括换行符，则需要<mark>将文本放在引号里面</mark>。

```bash
bash-3.2$ echo hello world
hello world
bash-3.2$ echo "<html>
> <head>
> <title>this is a title</title>
> </head>
> <body>
> this is body
> </body>
> </html>
> "
<html>
<head>
<title>this is a title</title>
</head>
<body>
this is body
</body>
</html>
```

### 2.1.1.echo 命令 -n 参数

默认情况下，`echo`末尾会跟一个回车符，`-n`参数可以<mark>取消末尾的回车符</mark>，使的下一个提示紧跟在输出内容的后面。

```bash
bash-3.2$ echo -n hello world
hello worldbash-3.2$
```

通过`-n`参数使两条命令的输出在同一行

```bash
bash-3.2$ echo a ;echo b
a
b
#使用-n参数
bash-3.2$ echo -n a ;echo b
ab
```

### 2.1.2.echo 命令 -e 参数

`-e`参数会<mark>解释引号（单引号和双引号）里面的特殊字符（比如换行符`\n`）</mark>。

如果不使用`-e`参数，即默认情况下，引号会让特殊字符变成普通字符，`echo`不解释他们，原样输出。

```bash
bash-3.2$ echo "a \n b"
a \n b
bash-3.2$ echo -e "a \n b"
a
 b
# 单引号
bash-3.2$ echo -e 'aaa\nbbbbb'
aaa
bbbbb
```

上面代码中，`-e`参数是的`\n`解释为换行符，导致输出的内容出现换行。

## 2.2命令格式

shell命令基本都是命令`/可执行文件 + 参数的格式`。

```bash
$ command [arg1 ...[argN]]
```

`command` 是具体命令或可执行文件；

`arg1...argN` 是传递给命令的参数，是可选的，是命令的配置项。

```bash
bash-3.2$ ls -l
total 416712
drwxr-xr-x@   7 hatred  staff        224 Jun  5  2023 AndroidStudioProjects
drwx------@   5 hatred  staff        160 Nov  3  2023 Applications
drwxr-xr-x@   6 hatred  staff        192 Jul 23 13:41 Applications (Parallels)
drwx------@  34 hatred  staff       1088 Jul 23 09:37 Desktop
drwx------@  11 hatred  staff        352 Jul  5 09:20 Documents
drwx------@ 473 hatred  staff      15136 Jul 23 16:14 Downloads
drwx------@ 126 hatred  staff       4032 Jul 12 14:03 Library
```

        上面的命令 `ls` 是命令，`-l`是参数。

        参数通常以连词线开头，如：`-l`，同一个参数配置项通常有长和短令中形式，如`-l`是短形式，`--list`是长形式，作用完全相同。

        短形式方便手动输入，长形式一般用在脚本之中，可读性更好，利于自身含义。

        长形式参数在mac上的bash环境中执行没有成功，怀疑是`version 3.2.57(1)-release` 版本低，在kali中`version 5.1.16(1)-release` 也有长形式执行失败的情况。

```bash
# mac 长形式参数执行结果
bash-3.2$ ls --list
ls: unrecognized option '--list'
usage: ls [-@ABCFGHILOPRSTUWabcdefghiklmnopqrstuvwxy1%,] [--color=when] [-D format] [file ...]
bash-3.2$ ls --reverse
ls: unrecognized option '--reverse'
usage: ls [-@ABCFGHILOPRSTUWabcdefghiklmnopqrstuvwxy1%,] [--color=when] [-D format] [file ...]

#kali 长形式执行结果
┌──(root㉿kali-linux-2022-2)-[/home/parallels]
└─# ls --list                                                                                                                                                                         
ls: unrecognized option '--list'
Try 'ls --help' for more information.

┌──(root㉿kali-linux-2022-2)-[/home/parallels]
└─# ls --reverse                                                                                                                                                                      
Videos  Templates  reports  Public  Pictures  Music  Downloads  Documents  Desktop
```

### 2.2.1 命令行结尾反斜杠

命令行结尾加反斜杠，bash会将下一行和当前行放在一起解释。

```bash
bash-3.2$ echo a \
> b
a b
# 和下面效果相同
bash-3.2$ echo a b
a b
```

### 2.2.2 空格

bash 使用空格或 tab 键区分不同的参数。如果参数间有多个空格，bash会自动忽略多余的空格。

```bash
bash-3.2$ echo aaa bbb test
aaa bbb test
#多空格忽略情况
bash-3.2$ echo aaa bbb       test
aaa bbb test
```

### 2.2.3 分号

分号` ；`是命令的结束符，是的一行上可以放置多个命令，上一个命令执行结束后，在执行第二个命令。无论第一个命令执行成功或失败，第二个命令总是紧接着第一个命令执行。

```bash
bash-3.2$ clear;ls
bash-3.2$ clear ;which clear
```

### 2.2.4 命令的组合符 `&&` 和 `||`

```bash
# 如果command1 执行成功则运行 command2
command1 && command2
# 如果command1 执行失败则运行 command2
command1 || command2
```

### 2.2.5 `type`命令

```bash
# type 命令判断命令来源
bash-3.2$ type type
type is a shell builtin
bash-3.2$ type ls
ls is hashed (/bin/ls)
```

上面执行结果：`type `命令告诉我们，`type `是内部命令，`ls `是外部程序 `/bin/ls`

```bash
# type 命令 -a 参数：查看一个命令的所有定义
bash-3.2$ type -a type
type is a shell builtin
type is /usr/bin/type
```

上面代码说明，`type`即是内置命令，也有对应的外部程序。

```bash
# type 命令 -t 参数，可以返回一个命令的类型：
# 别名 alias
# 关键词 keyword
# 函数 function
# 内置命令 builtin
# 文件 file
bash-3.2$ type -t type
builtin
bash-3.2$ type -t ls
file
bash-3.2$ type -t if
keyword
```

上面的例子，type是内置命令，ls是文件，if是关键字

### 2.2.6 快捷键

- `Ctrl + L`：清楚屏幕并将当前行移动到页面顶部

- `Ctrl + C`：终止当前正在执行的命令

- `Shift +  PageUp`：向上滚动

- `Shift + PageDown`：向下滚动

- `Ctrl + U`：从光标位置删除到行首

- `Ctrl + K`：从光标位置删除到行尾

- `Ctrl + w`：删除光标前一个单词（以空格分割的内容）

- `Ctrl + D`：关闭shell会话

- `⬆️ + ⬇️ `：浏览已执行命令的历史记录📝

- `tab`：输入命令的一部分，按tab可以进行自动补全；输入路径的一部分，按tab可以进行路径补全。

# 3.模式扩展

### 3.1 模式扩展简介

        shell接收到用户输入的命令之后，会根据空格将用户的输入拆分成一个个`词元（token）`。然后，shell会扩展词元中的特殊符号，扩展完成后才调用相应的命令。

        对词元中特殊符号的扩展，称`模式扩展（globbing`），其中有些用到通配符，由称为`通配符扩展`。

bash提供8种扩展。

```bash
~ 波浪线扩展
? 字符扩展
* 字符扩展
[...] [start-end]方括号扩展
{,,,} {start..end}大括号扩展
$ 变量扩展
$(...) 子命令扩展
$((...))算术扩展

字符类
```

        bash是先进行扩展，再执行命令。因此，扩展的结果是由bash负责的，与所要执行的命令无关。命令本身并不存在参数扩展，收到什么样的参数就原样执行。

        扩展模块的英文单词`globbing`，这个词的来源是早起Unix系统中有一个`/etc/glob`文件，保存扩展模版。后来bash内置了这个功能，但名字保留了下来。

        模式扩展与正则表达式的关系是，扩展模式早于正则表达式出现，可以看作是原始的正则表达式。它的功能没有正则那么强大，但优点是简单和方便。

```bash
# 关闭扩展功能
bash-3.2$ set -o noglob
#或
bash-3.2$ set -f
```

```bash
#开启扩展功能
bash-3.2$ set +o noglob
#或
bash-3.2$ set +f
```

## 3.2 ` ~ `波浪线扩展

```bash
# ~ 波浪线自动扩展成当前用户的的主目录
bash-3.2$ echo ~
/Users/hatred
```

```bash
# ~/dir 表示扩展主目录下的某个子目录 dir是主目录下的一个子目录名
bash-3.2$ cd ~/studyjava
bash-3.2$ pwd
/Users/hatred/studyjava
```

```bash
# ~user 表示扩展成用户 user 的主目录
bash-3.2$ echo ~root
/var/root
# 若 ～user 中的 user 不存在 ，则~扩展不会发生
bash-3.2$ echo ~dede
~dede
```

```bash
# ~+ 会扩展成 当前目录，等同于pwd
bash-3.2$ echo ~+
/Users/hatred
```

## 3.3 ` ？`字符扩展

` ? `字符表示文件路径中的任意单个字符，不包含空字符。如`file????`，匹配以file开始后面跟着四个字符的文件名。` ？`字符扩展属于文件名扩展，只有文件/目录 确实存在前提下才会发生扩展

```bash
# 存在文件a.txt和b.txt 1.txt
bash-3.2$ ls ?.txt
1.txt    a.txt    b.txt
```

```bash
# 如果要匹配多个字符，就使用多个 ?? ;如存在 ab.txt bb.txt zb.txt
bash-3.2$ ls ??.txt
ab.txt    bb.txt    zb.txt

# ？字符扩展属于文件名扩展，只有文件/目录 确实存在前提下才会发生扩展
# 当前目录为空目录，echo ?.bb 会将字符原样输出
bash-3.2$ ls ?.bb
ls: ?.bb: No such file or directory
bash-3.2$ echo ?.bb
?.bb
```

## 3.4 ` * `字符扩展

` * `字符代表文件路径里任意数量的任意字符，包含零个字符。` * `字符扩展属于文件名扩展，只有在文件/目录确实存在的前提下扩展才会发生。

```bash
# 匹配以.txt 结尾的文件和文件夹
bash-3.2$ ls *.txt
bash-3.2$ ls *.txt 
1.txt    a.txt    ab.txt    b.txt    bb.txt    zb.txt

ay.txt:
# * 可以匹配空字符，如下
bash-3.2$ ls *b*
ab.txt    b.txt    bb.txt    zb.txt

zbctxt:
# * 不会匹配隐藏文件（ 以 . 开头的文件 ），即 ls * 不会输出隐藏文件，如果要输出隐藏文件 需要使用 .*
bash-3.2$ ls .*
. .. .abc.txt    .bak        .ooo.sh
# 显示隐藏文件 同时排除 . .. 两个特殊隐藏文件可以与括号扩展结合 .[!.]*  指，匹配两个字符开头，第一位为.第二位不为.的文件或目录
bash-3.2$ echo .[!.]*
.abc.txt .bak .ooo.sh
#` * `字符扩展属于文件名扩展，只有在文件/目录确实存在的前提下扩展才会发生。 
bash-3.2$ ls g*
ls: g*: No such file or directory
bash-3.2$ echo g*
g*
```

默认情况下，`*`只匹配当前目录，不会匹配子目录。如文本文件在子目录中，`*.md`不会产生匹配，需要写做`*/*.md`

```bash
# 匹配子目录下的.md文件
bash-3.2$ ls *.md
ls: *.md: No such file or directory
bash-3.2$ ls */*.md
ay.txt/fisrt.md
```

⚠️ bash 4.0 引入了`globstar`，该参数打开时允许`**`匹配零个或多个子目录。因此`**/*.md`可以匹配顶层的md文件和任意深度子目录md文件。详见`shopt`命令。

## 3.5 方括号扩展
### 3.5.1 `[...]`扩展
方括号扩展的形式是`[...]`，只有文件确实存在的前提下才会扩展，如果文件不存在就会原样输出；如`[aeiou]`可以匹配五个元音字母中的任意一个。 

```bash
# 存在 a.txt b.txt
bash-3.2$ ls [ab].txt
a.txt    b.txt
# 仅存在 b.txt
bash-3.2$ ls [ab].txt
b.txt
# 方括号扩展属于文件名匹配，即扩展后的结果必须符合现有的文件路径。如果不存在，不会进行扩展
# 没有 a.txt b.txt  ---- 由于扩展后的文件不存在，[ab].txt就原样输出了，导致ls命名报错。
bash-3.2$ ls [ab].txt
b.txt
```
方括号的两种变体`[^...]`和`[!...]`，表示匹配不在括号内内的字符，这两种写法等价。
```bash
# 存在 aaa bbb aba 三个文件, 匹配三个字符组成，中间字符不为a的文件
bash-3.2$ ls ?[!a]?
aba	bbb
bash-3.2$ ls ?[^a]?
aba	bbb
```
⚠️ 如果要匹配 `[` 可以直接放在括号里`[[abc]`，如果要匹配只能放在括号的开头和结尾`[-abc]``[abc-]`。

### 3.5.2 `[start-end]`扩展

# 仅存在

# 4.引号和转义

# 5.变量

# 6.字符串操作

# 7.算数运算符

# 8.操作历史

# 9.行操作

# 10.目录堆栈

# 11.脚本入门

# 12.`read`命令

# 13.条件判断

# 14.循环

# 15.函数

# 16.数组

# 17.`set`命令和`shopt`命令

# 18. 脚本排错

# 19。`mktemp`命令和`trap`命令

# 20.启动环境

# 21.命令提示符
