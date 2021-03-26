# shell_practice

## 一、什么是bash?
Bash是Unix系统和Linux系统的一种Shell（命令行环境），是目前绝大多数Linux发行版的默认shell

---
1.1 shell的含义

Shell 这个单词的原意是“外壳”，跟 kernel（内核）相对应，比喻内核外面的一层，即用户跟内核交互的对话界面。

通俗解释
* Shell是一个程序，提供与用户对话的环境（也称命令行环境）shell接收输入命令，将命令送入操作系统执行，并将结果返回给用户

* shell是一个命令解释器，支持变量、条件判断、循环，用shell命令写出的各种小程序，也称为脚本 

* shell也是一个工具箱，提供各种工具，方便用户使用操作系统功能

1.2 shell的种类
+ +Bourne Shell（sh）
+ Bourne Again shell（bash）
+ C Shell（csh）: 为 Shell 提供 C 语言的语法
+ TENEX C Shell（tcsh）
+ Korn shell（ksh）
+ Z Shell（zsh）
+ Friendly Interactive Shell（fish）

使用下面命令可以查看当前运行的shell
```
    $ echo $SHELL
    /bin/bash
```

下面的命令可以查看当前Linux系统安装的所有Shell
```
    $ cat /etc/shells
```

Linux 允许每个用户使用不同的 Shell，用户的默认 Shell 一般都是 Bash，或者与 Bash 兼容。

1.3 查看bash shell版本
```
    $ bash --version 
    GNU bash, version 3.2.57(1)-release (x86_64-apple-darwin17)
    Copyright (C) 2007 Free Software Foundation, Inc.
    
    #或者
    $ echo $BASH_VERSION
    3.2.57(1)-release
```