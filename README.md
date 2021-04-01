# shell_practice

## 一、什么是bash?
Bash是Unix系统和Linux系统的一种Shell（命令行环境），是目前绝大多数Linux发行版的默认shell

---
### 1.1 shell的含义

Shell 这个单词的原意是“外壳”，跟 kernel（内核）相对应，比喻内核外面的一层，即用户跟内核交互的对话界面。

通俗解释
* Shell是一个程序，提供与用户对话的环境（也称命令行环境）shell接收输入命令，将命令送入操作系统执行，并将结果返回给用户

* shell是一个命令解释器，支持变量、条件判断、循环，用shell命令写出的各种小程序，也称为脚本 

* shell也是一个工具箱，提供各种工具，方便用户使用操作系统功能

### 1.2 shell的种类
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

### 1.3 查看bash shell版本
```
    $ bash --version 
    GNU bash, version 3.2.57(1)-release (x86_64-apple-darwin17)
    Copyright (C) 2007 Free Software Foundation, Inc.
    
    #或者
    $ echo $BASH_VERSION
    3.2.57(1)-release
```


## 二、bash的基本语法?
---
### 2.1 echo命令
* 单行echo： 可以不加引号
* 多行echo： 输出的是多行文本，即包括换行符。这时需要把多行文本放在引号里面

#### 2.1.1 参数解析
+ -n：参数取消末尾的回车符
+ -e: 解释引号（双引号和单引号）的特殊字符（比如换行符\n），默认情况下，echo不解释它们，原样输出
```
    $ echo "Hello\nWordld"
    Hello\nWorld

    双/单引号
    $ echo -e "Hello\nWorld" 
    Hello
    World
```

### 2.2 命令格式
命令行中，主要通过使用shell命令，进行各种操作，shell命令基本都是以下格式
```
    $ command [ arg1 ... [ argN ]]
```

### 2.3 空格
Bash 使用空格（或 Tab 键）区分不同的参数。

### 2.4 分号
分号（;）是命令的结束符，使得一行可以放置多个命令，上一个命令执行结束后，再执行第二个命令。

### 2.5 命令的组合符 && 和 ||
除了分号，Bash 还提供两个命令组合符&&和||，允许更好地控制多个命令之间的继发关系。
```
    Command1 && Command2
```
上面命令的意思是，如果Command1命令运行成功，则继续运行Command2命令。

```
    Command1 || Command2
```
上面命令的意思是，如果Command1命令运行失败，则继续运行Command2命令。

### 2.6 type命令
Bash 本身内置了很多命令，同时也可以执行外部程序。怎么知道一个命令是内置命令，还是外部程序呢？
```
    $ type echo
    echo is a shell builtin
    $ type ls
    ls is hashed (/bin/ls)
```
上面代码中，type命令告诉我们，echo是内部命令，ls是外部程序（/bin/ls）。


## 三、bash的模式扩展?
### 3.1 简介
Shell接收到用户输入的命令后，会根据空格将用户输入拆分成一个个词元（token），然后，Shell会扩展词元里面的特殊字符，扩展完成后才会调用相应的命令。

这种特殊字符的扩展，称为模式扩展（globbing）。其中有些用到通配符，又称为通配符扩展，Bash一共提供八种扩展

- 波浪线扩展
- '?' 字符扩展
- '*' 字符扩展
- 方括号扩展
- [start-end]扩展
- 大括号扩展
- {start..end}扩展
- 变量扩展
- 子命令扩展
- 算术扩展

### 3.2 波浪线扩展
波浪线 ～ 会自动扩展成用户的主目录。

```
    $ echo ~
    /home/me
```

~/dir 表示扩展成主目录的某个子目录，dir是主目录里面的一个子目录名

```
    # 进入 /home/me/foo目录
    $ cd ~/foo
```
～user 表示扩展成用户user的主目录

### 3.3 '?' 字符扩展
？字符代表文件路径里面任意单个字符，不包括空字符，比如，Data???匹配所有Data后面跟着三个字符的文件名

```
    # 存在文件 a.txt 和 b.txt
    $ ls ?.txt
    a.txt b.txt
```

? 字符扩展属于文件名扩展，只有文件确实存在的前提下，才会发生扩展。
```
    # 当前目录有 a.txt 文件
    $ echo ?.txt
    a.txt

    # 当前目录为空目录
    $ echo ?.txt
    ?.txt
```

### 3.4 '*' 字符扩展
*字符代表文件路径里面的任意数量的任意字符，包括零个字符。


### 3.5 方括号扩展（只能匹配单字符,文件名扩展）
只有文件确实存在的前提才会扩展，如果文件不存在会原样输出。括号之中的任意一个字符。比如，[aeiou]可以匹配五个元音字母中的任意一个。
```
    # 存在文件 a.txt 和 b.txt
    $ ls [ab].txt
    a.txt b.txt

    # 只存在文件 a.txt
    $ ls [ab].txt
    a.txt
```
方括号扩展还有两种变体：[^...]和[!...]。它们表示匹配不在方括号里面的字符，这两种写法是等价的。比如，[^abc]或[!abc]表示匹配除了a、b、c以外的字符

### 3.6 【start-end】扩展
方括号扩展有一个简写形式[start-end]，表示匹配一个连续的范围。比如，[a-c]等同于[abc]，[0-9]匹配[0123456789]。
```
    # 存在文件 a.txt、b.txt 和 c.txt
    $ ls [a-c].txt
    a.txt
    b.txt
    c.txt

    # 存在文件 report1.txt、report2.txt 和 report3.txt
    $ ls report[0-9].txt
    report1.txt
    report2.txt
    report3.txt
```
下面是一些常用简写的例子。

+ [a-z]：所有小写字母。
+ [a-zA-Z]：所有小写字母与大写字母。
+ [a-zA-Z0-9]：所有小写字母、大写字母与数字。
+ [abc]*：所有以a、b、c字符之一开头的文件名。
+ program.[co]：文件program.c与文件program.o。
+ BACKUP.[0-9][0-9][0-9]：所有以BACKUP.开头，后面是三个数字的文件名。


### 3.7 大括号扩展（可匹配多字符模式）
由于大括号扩展{...}不是文件名扩展，所以它总是会扩展的。这与方括号扩展[...]完全不同，如果匹配的文件不存在，方括号就不会扩展。这一点要注意区分。
```
    # 不存在 a.txt 和 b.txt
    $ echo [ab].txt
    [ab].txt

    $ echo {a,b}.txt
    a.txt b.txt
```
上面例子中，如果不存在a.txt和b.txt，那么[ab].txt就会变成一个普通的文件名，而{a,b}.txt可以照样扩展。

### 3.8 {start..end扩展}
大括号扩展有一个简写形式{start..end}，表示扩展成一个连续序列。比如，{a..z}可以扩展成26个小写英文字母。
```
    $ echo {a..c}
    a b c

    $ echo d{a..d}g
    dag dbg dcg ddg

    $ echo {1..4}
    1 2 3 4

    $ echo Number_{1..5}
    Number_1 Number_2 Number_3 Number_4 Number_5

    #这种简写形式支持逆序。
    $ echo {c..a}
    c b a

    $ echo {5..1}
    5 4 3 2 1

    # 这种简写形式可以嵌套使用，形成复杂的扩展
    $ echo .{mp{3..4},m4{a,b,p,v}}
    .mp3 .mp4 .m4a .m4b .m4p .m4v   

    # 大括号扩展的常见用途为新建一系列目录。
    $ mkdir {2007..2009}-{01..12}
```

高级用法
如果整数前面有前导0，扩展输出的每一项都有前导0。
```
    $ echo {01..5}
    01 02 03 04 05

    $ echo {001..5}
    001 002 003 004 005

    # 这种简写形式还可以使用第二个双点号（start..end..step），用来指定扩展的步长。
    $ echo {0..8..2}
    0 2 4 6 8

    # 简写形式连用
    $ echo {a..c}{1..3}
    a1 a2 a3 b1 b2 b3 c1 c2 c3
```


### 3.9 变量扩展
Bash 将美元符号$开头的词元视为变量，将其扩展成变量值，详见《Bash 变量》一章。

两种用法:
+ echo $SHELL
+ echo ${SHELL}

### 3.10 子命令扩展 
$(...)可以扩展成另一个命令的运行结果，该命令的所有输出都会作为返回值。
```
    $ echo $(date)
    
```

还有另一种较老的语法，子命令放在反引号之中，也可以扩展成命令的运行结果。
```
    $ echo `date`
    Thu Apr  1 13:10:15 CST 2021
```
$(...)可以嵌套，比如$(ls $(pwd))。

### 3.11 算术扩展
$((...))可以扩展成整数运算的结果
```
    echo $((2+2))
    4
```

### 3.12 字符类扩展(文件名扩展)
[[:class:]]表示一个字符类，扩展成某一类特定字符中的一个。常用字符类如下：
+ [[:alnum:]]：匹配任意英文字母与数字
+ [[:alpha:]]：匹配任意英文字母
+ [[:blank:]]：空格和 Tab 键。
+ [[:cntrl:]]：ASCII 码 0-31 的不可打印字符。
+ [[:digit:]]：匹配任意数字 0-9。
+ [[:graph:]]：A-Z、a-z、0-9 和标点符号。
+ [[:lower:]]：匹配任意小写字母 a-z。
+ [[:print:]]：ASCII 码 32-127 的可打印字符。
+ [[:punct:]]：标点符号（除了 A-Z、a-z、0-9 的可打印字符）。
+ [[:space:]]：空格、Tab、LF（10）、VT（11）、FF（12）、CR（13）
+ [[:upper:]]：匹配任意大写字母 A-Z。
+ [[:xdigit:]]：16进制字符（A-F、a-f、0-9）。

```
    输出所有大写字母开头的文件名。
    $ echo [[:upper:]]*
```
字符类的第一个方括号后面，可以加上感叹号!，表示否定。比如，[![:digit:]]匹配所有非数字
```
    $ echo [![:digit:]]*
```

字符类也属于文件名扩展，如果没有匹配的文件名，字符类就会原样输出。
```
    # 不存在以大写字母开头的文件
    $ echo [[:upper:]]*
    [[:upper:]]*
```

## 四、使用注意点
### 4.1 通配符是先解释、再执行
Bash 接收到命令以后，发现里面有通配符，会进行通配符扩展，然后再执行命令。
```
    $ ls a*.txt
    ab.txt
```

### 4.2 文件名扩展在不匹配时，会原样输出。
```
    # 不存在 r 开头的文件名
    $ echo r*
    r*
```
***大括号扩展{...}不是文件名扩展***。

### 4.3 只适用于单层路径。
所有文件名扩展只匹配单层路径，不能跨目录匹配，即无法匹配子目录里面的文件。或者说，?或*这样的通配符，不能匹配路径分隔符（/）。

如果要匹配子目录里面的文件，可以写成下面这样。
```
    $ ls */*.txt
```

### 4.4 文件名可以使用通配符
Bash 允许文件名使用通配符，即文件名包括特殊字符。这时引用文件名，需要把文件名放在单引号里面。