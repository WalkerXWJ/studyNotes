# 1.简介

Bash是Unix和Linux系统的一种shell（命令行环境），是目前大部分发行版Linux默认的shell。

本次Bash学习使用mac m1的设备。

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

# 3.模式扩展

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
