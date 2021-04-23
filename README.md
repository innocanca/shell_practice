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


## 四、引号和转义

Bash 只有一种数据类型，就是字符串。不管用户输入什么数据，Bash 都视为字符串。因此，字符串相关的引号和转义，对 Bash 来说就非常重要。

### 4.1 转义
反斜杠除了用于转义，还可以表示一些不可打印的字符。
+ \a：响铃
+ \b：退格
+ \n：换行
+ \r：回车
+ \t：制表符

如果想要在命令行使用这些不可打印的字符，可以把它们放在引号里面，然后使用echo命令的-e参数。
```
    $ echo a\tb
    atb

    $ echo -e "a\tb"
    a        b
```

换行符是一个特殊字符，表示命令的结束，Bash 收到这个字符以后，就会对输入的命令进行解释执行。换行符前面加上反斜杠转义，就使得换行符变成一个普通字符，Bash 会将其当作空格处理，从而可以将一行命令写成多行。
```
    $ mv \
    /path/to/foo \
    /path/to/bar

    # 等同于
    $ mv /path/to/foo /path/to/bar
```
如果一条命令过长，就可以在行尾使用反斜杠，将其改写成多行。这是常见的多行命令的写法。

### 4.2 单引号 & 双引号

单引号用于保留字符的字面含义，各种特殊字符在单引号里面，都会变为普通字符

双引号比单引号宽松，大部分特殊字符在双引号里面，都会失去特殊含义，变成普通字符。三个特殊字符除外：美元符号（$）、反引号（`）和反斜杠（\）。这三个字符在双引号之中，依然有特殊含义，会被 Bash 自动扩展

单引号
```
    $ echo '*'
    *

    $ echo '$USER'
    $USER

    $ echo '$((2+2))'
    $((2+2))

    $ echo '$(echo foo)'
    $(echo foo)
```

双引号
```
    $ echo "$SHELL"
    /bin/bash

    $ echo "`date`"
    Mon Jan 27 13:33:18 CST 2020

    $ echo "I'd say: \"hello!\""
    I'd say: "hello!"

    $ echo "\\"
    \
```


双引号会保存原始命令的输出格式 
```
    # 单行输出
    $ echo $(cal)
    一月 2020 日 一 二 三 四 五 六 1 2 3 ... 31

    # 原始格式输出
    $ echo "$(cal)"
        一月 2020
    日 一 二 三 四 五 六
            1  2  3  4
    5  6  7  8  9 10 11
    12 13 14 15 16 17 18
    19 20 21 22 23 24 25
    26 27 28 29 30 31
```

## 五、变量

### 5.1 简介
Bash变量分成环境变量和自定义变量两类。

#### 5.1.1 环境变量
环境变量是 Bash 环境自带的变量，进入 Shell 时已经定义好了，可以直接使用。

env命令 或者 printenv命令，可以显示所有环境变量

```
    $ env
    # 或者
    $ printenv
```

下面是一些常见的环境变量。


#### 5.1.2 自定义变量
set命令可以显示所有变量（包括环境变量和自定义变量），以及所有的bash函数

### 5.2 创建变量

用户创建变量的时候，变量名必须遵守下面的规则。
- 字母、数字和下划线字符组成
- 第一个字符必须是一个字母或者下划线，不能是数字
- 不允许出现空格和标点符号

变量声明语法如下。
```
    variable=value
```
变量左边是等量名、右边是变量。注意：***等号两边不能有空格！！！！***

如果变量的值包含空格，则 ***必须将值放在引号中！！！！***

Bash 没有数据类型的概念，***所有的变量值都是字符串！！***。

下面是一些自定义变量的例子 
```
    a=z                     # 变量 a 赋值为字符串 z
    b="a string"            # 变量值包含空格，就必须放在引号里面
    c="a string and $b"     # 变量值可以引用其他变量的值
    d="\t\ta string\n"      # 变量值可以使用转义字符
    e=$(ls -l foo.txt)      # 变量值可以是命令的执行结果
    f=$((5 * 7))            # 变量值可以是数学运算的结果
```

如果同一行定义多个变量，必须使用分号（;）分隔。

```
    $ foo=1;bar=2
```

### 5.3 读取变量

+ 方式1 echo $variable
+ 方式2 echo ${variable}

***事实上，读取变量的语法$foo，可以看作是${foo}的简写形式。***

读取变量的时候，直接在变量名前加上$就可以了。
```
    $ foo=bar
    $ echo $foo
    bar
```
每当 Shell 看到以$开头的单词时，就会尝试读取这个变量名对应的值。如果变量不存在，Bash 不会报错，**而会输出空字符**。

由于$在 Bash 中有特殊含义，把它当作美元符号使用时，一定要非常小心，
```
    $ echo The total is $100.00
    The total is 00.00
```
上面命令的原意是输入$100，但是 Bash 将$1解释成了变量，该变量为空，因此输入就变成了00.00。所以，如果要使用$的原义，需要在$前面放上反斜杠，进行转义。

读取变量的时候，变量名也可以使用花括号{}包围，比如$a也可以写成${a}。这种写法可以用于变量名与其他字符连用的情况。
```
    $ a=foo
    $ echo $a_file

    $ echo ${a}_file
    foo_file
```
变量名a_file不会有任何输出，因为 Bash 将其整个解释为变量，而这个变量是不存在的。只有用花括号区分$a，Bash 才能正确解读

如果变量的值本身也是变量，可以使用${!varname}的语法，读取最终的值。
```
    $ myvar=USER
    $ echo ${!myvar}
    ruanyf
```

如果变量值包含连续空格（或制表符和换行符），最好放在双引号里面读取。
```
    $ a="1 2  3"
    $ echo $a
    1 2 3
    $ echo "$a"
    1 2  3
```

### 5.4 删除变量

unset命令用来删除一个变量，这个命令不是很有用。因为不存在的 Bash 变量一律等于空字符串，所以即使unset命令删除了变量，还是可以读取这个变量，值为空字符串。

### 5.5 输出变量，export命令

写入环境变量，用户创建的变量仅可用于当前 Shell，子 Shell 默认读取不到父 Shell 定义的变量。为了把变量传递给子 Shell，需要使用export命令。

### 5.6 特殊变量

+ $？:上一个命令的退出码，用来判断上一个命令是否执行成功。
+ $$ : $$为当前Shell的进程ID。
+ $_ : $_为上一个命令的最后一个参数。
```
    $ grep dictionary /usr/share/dict/words
    dictionary

    $ echo $_
    /usr/share/dict/words
```
+ $! : $!为最近一个后台执行的异步命令的进程 ID。
+ $0 : $0为当前 Shell 的名称（在命令行直接执行时）或者脚本名（在脚本中执行时）。
+ $- ：$-为当前 Shell 的启动参数。
 
### 5.7 变量的默认值
略

### 5.8 declare命令

declare命令可以声明一些特殊类型的变量，为变量设置一些限制，比如声明只读类型的变量和整数类型的变量。
declare命令的主要参数（OPTION）如下。

+ -a：声明数组变量。
+ -f：输出所有函数定义。
+ -F：输出所有函数名。
+ -i：声明整数变量。
+ -l：声明变量为小写字母。
+ -p：查看变量信息。
+ -r：声明只读变量。
+ -u：声明变量为大写字母。
+ -x：该变量输出为环境变量

1）-i 参数

-i参数声明整数变量以后，可以直接进行数学运算。
```
    $ declare -i val1=12 val2=5
    $ declare -i result
    $ result=val1*val2
    $ echo $result
    60
```

2) -x 参数

-x参数等同于export命令，可以输出一个变量为子 Shell 的环境变量。
```
    $ declare -x foo
    # 等同于
    $ export foo
```

3) -r 参数

-r参数可以声明只读变量，无法改变变量值，也不能unset变量。
```
    $ declare -r bar=1

    $ bar=2
    bash: bar：只读变量
    $ echo $?
    1

    $ unset bar
    bash: bar：只读变量
    $ echo $?
    1
```

4) -u 参数 
-u参数声明变量为大写字母，可以自动把变量值转成大写字母。

```
    $ declare -u foo
    $ foo=upper
    $ echo $foo
    UPPER
```

5) -l 参数
```
    $ declare -l bar
    $ bar=LOWER
    $ echo $bar
    lower
```

### 5.9 readonly命令

readonly命令等同于declare -r，用来声明只读变量，不能改变变量值，也不能unset变量。

### 5.10 let 命令

let命令声明变量时，可以直接执行算术表达式。
```
    $ let foo=1+2
    $ echo $foo
    3

    $ let "v1 = 1" "v2 = v1++"
    $ echo $v1,$v2
    2,1
```

shell中做运算可以使用let/declare -i方法


## 六、字符串处理（***重点！！！***）

---
### 6.1 字符串的长度

shell中获取字符串长度的语法如下：**${#varname}** ，大括号是必须的，否则bash会将$#理解成脚本参数的个数，将变量名理解成文本

```
    $ myPath=/home/cam/book/long.file.name
    $ echo ${#myPath}
    29
```

### 6.2 子字符串

字符串提取字串的语法如下 ：${varname:offset:length}，返回变量$varname的子字符串，从位置offset开始（从0开始计算），长度为length。如果省略length，则从位置offset开始，一直返回到字符串的结尾。

⚠️ varname必须是变量名！！
```
    $ count=frogfootman
    $ echo ${count:4:4}
    foot
```

这种语法不能直接操作字符串，只能通过变量来读取字符串，并且不会改变原始字符串。变量前面的美元符号可以省略。

```
    # 报错
    $ echo ${"hello":2:3}
```

### 6.3 搜索和替换

Bash提供字符串搜索和替换的多种方法

+ （1） 字符串头部的模式匹配

    检查字符串开头，是否匹配给定的模式。如果匹配成功，就删除匹配的部分，返回剩下的部分。原始变量不会发生变化。
```
    # 如果 pattern 匹配变量 variable 的开头，
    # 删除最短匹配（非贪婪匹配）的部分，返回剩余部分
    ${variable#pattern}

    # 如果 pattern 匹配变量 variable 的开头，
    # 删除最长匹配（贪婪匹配）的部分，返回剩余部分
    ${variable##pattern}

    $ phone="555-456-1414"
    $ echo ${phone#*-}
    456-1414
    $ echo ${phone##*-}
    1414
```


+ （2）字符串尾部的模式匹配。

    检查字符串结尾，是否匹配给定的模式。如果匹配成功，就删除匹配的部分，返回剩下的部分。原始变量不会发生变化。

```
    # 如果 pattern 匹配变量 variable 的结尾，
    # 删除最短匹配（非贪婪匹配）的部分，返回剩余部分
    ${variable%pattern}

    # 如果 pattern 匹配变量 variable 的结尾，
    # 删除最长匹配（贪婪匹配）的部分，返回剩余部分
    ${variable%%pattern}


    $ path=/home/cam/book/long.file.name
    $ echo ${path%.*}
    /home/cam/book/long.file

    $ echo ${path%%.*}
    /home/cam/book/long
```

+ （3）任意位置的模式匹配。类似sed命令

    检查字符串内部，是否匹配给定的模式。如果匹配成功，就删除匹配的部分，换成其他的字符串返回。原始变量不会发生变化。

```
    # 如果 pattern 匹配变量 variable 的一部分，
    # 最长匹配（非贪婪匹配）的那部分被 string 替换，但仅替换第一个匹配
    ${variable/pattern/string}

    # 如果 pattern 匹配变量 variable 的一部分，
    # 最长匹配（贪婪匹配）的那部分被 string 替换，所有匹配都替换
    ${variable//pattern/string}

    $ path=/home/cam/foo/foo.name
    $ echo ${path/foo/bar}
    /home/cam/bar/foo.name

    $ echo ${path//foo/bar}
    /home/cam/bar/bar.name
```

+ 改变大小写 # 
```
    # 转为大写
    ${varname^^}

    # 转为小写
    ${varname,,}

    $ foo=heLLo
    $ echo ${foo^^}
    HELLO
    $ echo ${foo,,}
    hello
```

## 七、算术运算
---

### 7.1 算术表达式

((...))语法可以进行**整数**的算术运算，可嵌套使用

```
    $ ((foo = 5 + 5))
    $ echo $foo
    10
```

((...))会自动忽略内部的空格，所以下面的写法都正确，得到同样的结果。

如果要读取算术运算的结果，需要在((...))前面加上美元符号$((...))，使其变成算术表达式，返回算术运算的值。

((...))支持的算术运算符如下。

+ +：加法
+ -：减法
+ *：乘法
+ /：除法（整除）
+ %：取余
+ **：指数
+ ++：自增运算（前缀或者后缀）
+ --：自减运算（前缀后者后缀）

除法运算符的返回结果总是整数，比如5除以2，得到的结果是2，而不是2.5。

这个语法只能计算整数，否则会报错。
```
    # 报错
    $ echo $((1.5 + 1))
    bash: 语法错误
```

### 7.2 数值的进制

Bash的数值默认都是十进制，但是在算术运算表达式中，也可以使用其它进制。

+ number : 没有任何特殊表示法的数字都是十进制数（以10为底）
+ 0nunmber: 八进制数
+ 0xnumber: 十六进制
+ base#number: base进制的数

```
    $ echo $((0xff))
    255
    $ echo $((2#11111111))
    255
```


7.3 位运算

$((...))支持以下二进制位运算符

+ << 位左移运算符
+ \>> 位右移运算符
+ & 位的"与"运算
+ | 位的“或”运算
+ ～ 位的“否”运算
+ ^ 位的“异或”运算符

```
    $ echo $((16>>2))
    4

    $ echo $((16<<2))
    64

    $ echo $((17&3))
    1

    $ echo $((17|3))
    19

    $ echo $((17^3))
    18
```

7.4 逻辑运算 

$((...))支持以下的逻辑运算符。

+ < 
+ \>
+ <=
+ \>=
+ ==
+ !=
+ && 
+ ||
+ !
+ expr1?expr2:expr3

```
    $ echo $((3 > 2))
    1

    $ echo $(( (3 > 2) || (4 <= 1) ))
    1

    $ a=0
    $ echo $((a<1 ? 1 : 0))
    1

    $ echo $((a>1 ? 1 : 0))
    0
```

7.5 赋值运算 

7.6 expr命令

expr命令支持算术运算，可以不使用((...))语法。

```
    $ expr 3 + 2
    5

    # expr命令支持变量替换。
    $ foo=3
    $ expr $foo + 2
    5
```

## 八 Bash行操作
---
Bash内置了Readline库，具有这个库提供的很多“行操作”功能，比如命令的自动补全，可以大大加快操作速度 
 
略 参考：https://wangdoc.com/bash/readline.html


## 九 脚本入门

脚本（script）就是包含一系列命令的文本文件 ，Shell读取这个文件、依此执行里面所有命令

### 9.1 Shebang行

脚本的第一行通常为指定解释器，即这个脚本必须通过什么解释器执行，以#！开头，这个字符称为Shebang，所以这一行叫做Shebang行

#！后面就是脚本解释器的位置，Bash脚本的解释器一般是/bin/sh 或 /bin/bash

Shebang 行不是必需的，但是建议加上这行。如果缺少该行，就需要手动将脚本传给解释器。举例来说，脚本是script.sh，有 Shebang 行的时候，可以直接调用执行。

```
    $ ./script.sh

    # 没有 Shebang 行，就只能手动将脚本传给解释器来执行
    $ /bin/sh ./script.sh
    # 或者
    $ bash ./script.sh
```

如果 Bash 解释器不放在目录/bin，脚本就无法执行了。为了保险，可以写成下面这样。

```
    #!/usr/bin/env bash
```
上面命令使用env命令（这个命令总是在/usr/bin目录），返回 Bash 可执行文件的位置。

### 9.2 执行权限和路径

赋予脚本执行权限。脚本的权限通常设为755（拥有者有所有权限，其他人有读和执行权限）

```
    # 给所有用户执行权限
    $ chmod +x script.sh

    # 给所有用户读权限和执行权限
    $ chmod +rx script.sh
    # 或者
    $ chmod 755 script.sh

    # 只给脚本拥有者读权限和执行权限
    $ chmod u+rx script.sh
```

建议在主目录新建一个~/bin子目录，专门存放可执行脚本，然后把~/bin加入$PATH。
```
    export PATH=$PATH:~/bin
```

### 9.3 env命令

env命令总是指向/usr/bin/env文件，或者说，这个二进制文件总是在目录/usr/bin

+ !/usr/bin/env NAME

让 Shell 查找$PATH环境变量里面第一个匹配的NAME。如果你不知道某个命令的具体路径，或者希望兼容其他用户的机器，这样的写法就很有用

### 9.4 脚本参数

调用脚本的时候，脚本文件名后面可以带有参数

```
    $ script.sh word1 word2 word3
```

script.sh是一个脚本文件，word1、word2和word3是三个参数。

脚本文件内部，可以使用特殊变量，饮用这些参数。

+ $0: 脚本文件名，即script.sh
+ $1 ~ $9 对应脚本的第一个到第9个参数 
+ $#: 参数的总数
+ $@ 全部的参数，参数之间用空格分割 
+ $*: 全部的参数，默认使用空格，但是可以自定义

如果脚本的参数多于9个，那么第10个参数可以用${10}的形式引用，以此类推。

注意，如果命令是command -o foo bar，那么-o是$1，foo是$2，bar是$3。

```
    LEONFYANG-MB2:shell_practice leonfyang$ ./test1.sh a b c
    全部参数 : a b c
    命令行参数数量 ： 3
    $0 =  ./test1.sh
    $1 =  a
    $2 =  b
    $3 =  c
```

### 9.5 shift命令

shift命令可以改变脚本参数，每次执行都会移除脚本当前的第一个参数（$1），使得后面的参数向前一位，
即$2变成$1、$3变成$2、$4变成$3，以此类推。

```
    #!/bin/bash
    echo "一共输入了 $# 个参数"
    while [ "$1" != "" ]; do
    echo "剩下 $# 个参数"
    echo "参数：$1"
    shift
    done
```

### 9.6 getopts命令

略，后面学习


getopts命令用在脚本内部，可以解析复杂的脚本命令行参数，通常与while循环一起使用，取出脚本所有带有前置连接词（_）的参数

getopts optstring name



### 9.7 

变量当作命令的参数时，有时希望指定变量只能作为实体参数，不能当作配置项参数，这时可以使用配置项参数终止符--。

--强制变量$myPath只能当作实体参数（即路径名）解释。
```
    $ myPath="-l"
    $ ls -- $myPath
    ls: 无法访问'-l': 没有那个文件或目录
```

### 9.8 exit命令

```
    $ exit

```

exit命令后面可以跟参数，该参数就是退出状态。

```
    # 退出值为0（成功）
    $ exit 0

    # 退出值为1（失败）
    $ exit 1
```

脚本的退出值，0表示正常，1表示发生错误，2表示用法不对，126表示不是可执行脚本，127表示命令没有发现。如果脚本被信号N终止，则退出值为128 + N。简单来说，只要退出值非0，就认为执行出错。

exit与return命令的差别是，return命令是函数的退出，并返回一个值给调用者，脚本依然执行。exit是整个脚本的退出，如果在函数之中调用exit，则退出函数，并终止脚本执行。


### 9.9 命令执行结果

命令执行结束后，会有一个返回值。0表示执行成功，非0（通常是1）表示执行失败 。环境变量$？ 可以读取前一个命令的返回值



### 9.10 source命令

source命令用于执行一个脚本，通常用于重新加载一个配置文件

```
    $ source .bashrc
```

source命令最大的特点是在当前 Shell 执行脚本，不像直接执行脚本时，会新建一个子 Shell。所以，source命令执行脚本时，不需要export变量

source命令的另一个用途，是在脚本内部加载外部库。
```
    #!/bin/bash

    source ./lib.sh

    function_from_lib

```

source有一个简写形式，可以使用一个点（.）来表示。

```
    $ . .bashrc
```

### 9.11 别名，alias命令

```
    $ alias today='date +"%A, %B %-d, %Y"'
    $ today
    星期一, 一月 6, 2020
```

## 十、read命令
---
### 10.1 用法

可以接受用户输入的多个值,read命令的格式如下。
```
    read [-options] [variable...]
```


```
    #!/bin/bash

    echo -nn "输入一些文本 > "
    read text
    echo "你的输入: $text"

    LEONFYANG-MB2:shell_practice leonfyang$ ./readDemo.sh 
    输入一些文本 > hello
    你的输入: hello
```


read命令除了读取键盘输入，可以用来读取文件。read默认情况下就是按行读取的，一次读取一行。

```
    #!/bin/bash
    filename='/etc/hosts'

    while read myline
    do
    echo "$myline"
    done < $filename
```


### 10.2 参数

+ （1）-t 参数 设置超时时间
```
    #!/bin/bash

    echo -n "输入一些文本 > "
    if read -t 3 response; then
    echo "用户已经输入了"
    else
    echo "用户没有输入"
    fi
```

+ (2) -p 参数：指定用户输入的提示信息。

```
    read -p "Enter one or more values > "
    echo "REPLY = '$REPLY'"
```

上面例子中，先显示Enter one or more values >，再接受用户的输入

+ (3) -a 参数 

-a参数把用户的输入赋值给一个数组，从零号位置开始 
```
    $ read -a people
    alice duchess dodo
    $ echo ${people[2]}
    dodo
```
上面例子中，用户输入被赋值给一个数组people，这个数组的2号成员就是dodo。

+ (4) -n 参数

-n参数指定只读取若干个字符作为变量值，而不是整行读取。

```
    $ read -n 3 letter
    abcdefghij
    $ echo $letter
    abc
```

+ (5) -s: 使得用户的输入不显示在屏幕上，这常常用于输入密码或保密信息。
+ (6) -r: raw 模式，表示不把用户输入的反斜杠字符解释为转义字符。


### 10.3 IFS变量

read命令读取的值，默认是以空格分隔。可以通过自定义环境变量IFS（内部字段分隔符，Internal Field Separator 的缩写），修改分隔标志。


IFS的默认值是空格、Tab 符号、换行符号，通常取第一个（即空格）
```
    #!/bin/bash
    # read-ifs: read fields from a file

    FILE=/etc/passwd

    read -p "Enter a username > " user_name
    file_info="$(grep "^$user_name:" $FILE)"

    if [ -n "$file_info" ]; then
    IFS=":" read user pw uid gid name home shell <<< "$file_info"
    echo "User = '$user'"
    echo "UID = '$uid'"
    echo "GID = '$gid'"
    echo "Full Name = '$name'"
    echo "Home Dir. = '$home'"
    echo "Shell = '$shell'"
    else
    echo "No such user '$user_name'" >&2
    exit 1
    fi
```

## 十一 条件判断 
---

### 10.1 if结构
if是最常用的条件判断结构，只有符合给定条件时，才会执行指定的命令。它的语法如下。

```
    if commands; then
    commands
    [elif commands; then
    commands...]
    [else
    commands]
    fi
```
这个命令分成三个部分：if、elif和else。其中，后两个部分是可选的。

if关键字后面是主要的判断条件，elif用来添加在主条件不成立时的其他判断条件，else则是所有条件都不成立时要执行的部分。


```
    if test $USER = "foo"; then
    echo "Hello foo."
    else
    echo "You are not foo."
    fi
```

***if和then写在同一行时，需要分号分隔。分号是 Bash 的命令分隔符。它们也可以写成两行，这时不需要分号***


if后面可以跟任意数量的命令。这时，所有命令都会执行，但是判断真伪只看最后一个命令，即使前面所有命令都失败，只要最后一个命令返回0，就会执行then的部分。

```
    $ if false; true; then echo 'hello world'; fi
    hello world
```


### 11.2 test命令

if结构的判断条件，一般使用test命令，有三种形式。

```
    # 写法一
    test expression

    # 写法二
    [ expression ] #[]之前必须有空格

    # 写法三
    [[ expression ]] #[]之前必须有空格
```

***上面三种形式是等价的，但是第三种形式还支持正则判断，前两种不支持。***

上面的expression是一个表达式。这个表达式为真，test命令执行成功（返回值为0）；表达式为伪，test命令执行失败（返回值为1）。***注意，第二种和第三种写法，[和]与内部的表达式之间必须有空格。***

eg
```
    $ test -f /etc/hosts  #判断文件是否存在
    $ echo $?
    0

    $ [ -f /etc/hosts ]
    $  echo $?
    0
```

### 11.3 判断表达式 

if关键字后面，跟的是一个命令。这个命令可以是test命令，也可以是其他命令。命令的返回值为0表示判断成立，否则表示不成立。因为这些命令主要是为了得到返回值，所以可以视为表达式。

#### 11.3.1 条件判断 

判断文件状态 

+ [ -a file ]：如果 file 存在，则为true。
+ [ -e file ]：如果 file 存在，则为true。
+ [ -d file ]：如果 file 存在并且是一个目录，则为true。
+ [ -r file ]：如果 file 存在并且可读（当前用户有可读权限），则为true。
+ [ -w file ]：如果 file 存在并且可写（当前用户拥有可写权限），则为true。
+ [ -x file ]：如果 file 存在并且可执行（有效用户有执行／搜索权限），则为true。

#### 11.3.2 字符串判断 

+ [ string ] : 如果string不为空（长度大于0），则判断为真
+ [ -n string ]：如果字符串string的长度大于零，则判断为真。
+ [ -z string ]：如果字符串string的长度为零，则判断为真。
+ [ string1 = string2 ]：如果string1和string2相同，则判断为真。
+ [ string1 == string2 ]： 等同于[ string1 = string2 ]。
+ [ string1 '>' string2 ]：如果按照字典顺序string1排列在string2之后，则判断为真。
+ [ string1 '<' string2 ]：如果按照字典顺序string1排列在string2之前，则判断为真。

***注意，test命令内部的>和<，必须用引号引起来（或者是用反斜杠转义）。否则，它们会被 shell 解释为重定向操作符。***
```
#!/bin/bash

ANSWER=maybe
    if [ -z "$ANSWER" ]; then
    echo "There is no answer." >&2
    exit 1
    fi
    if [ "$ANSWER" = "yes" ]; then
    echo "The answer is YES."
    elif [ "$ANSWER" = "no" ]; then
    echo "The answer is NO."
    elif [ "$ANSWER" = "maybe" ]; then
    echo "The answer is MAYBE."
    else
    echo "The answer is UNKNOWN."
    fi
```

#### 11.3.3 整数判断

+ [ integer1 -eq integer2 ]：如果integer1等于integer2，则为true。
+ [ integer1 -ne integer2 ]：如果integer1不等于integer2，则为true。
+ [ integer1 -le integer2 ]：如果integer1小于或等于integer2，则为true。
+ [ integer1 -lt integer2 ]：如果integer1小于integer2，则为true。
+ [ integer1 -ge integer2 ]：如果integer1大于或等于integer2，则为true。
+ [ integer1 -gt integer2 ]：如果integer1大于integer2，则为true。

```
    #!/bin/bash
    INT=-5

    if [ -z "$INT" ]; then
    echo "INT is empty." >&2
    exit 1
    fi
    if [ $INT -eq 0 ]; then
    echo "INT is zero."
    else
    if [ $INT -lt 0 ]; then
        echo "INT is negative."
    else
        echo "INT is positive."
    fi
    if [ $((INT % 2)) -eq 0 ]; then
        echo "INT is even."
    else
        echo "INT is odd."
    fi
    fi
```

#### 11.3.4 正则表达式 

[[ expression ]] 这种判断形式支持正则表达式

```
    [[ string1 =~ regex ]]
```

上面的语法中，regex是一个正则表示式，=~是正则比较运算符。

```
#!/bin/bash

INT=-5
    if [[ "$INT" =~ ^-?[0-9]+$ ]]; then
    echo "INT is an integer."
    exit 0
    else
    echo "INT is not an integer." >&2
    exit 1
    fi
```

#### 11.3.5 test判断的逻辑运算  

通过逻辑运算，可以把多个test判断表达式结合起来，创造更复杂的判断。三种逻辑运算AND，OR，和NOT，都有自己的专用符号。

+ AND运算：符号&&，也可使用参数-a。
+ OR运算：符号||，也可使用参数-o。
+ NOT运算：符号!。

```
!/bin/bash

MIN_VAL=1
MAX_VAL=100

INT=50
    if [[ "$INT" =~ ^-?[0-9]+$ ]]; then
    if [[ $INT -ge $MIN_VAL && $INT -le $MAX_VAL ]]; then
        echo "$INT is within $MIN_VAL to $MAX_VAL."
    else
        echo "$INT is out of range."
    fi
    else
    echo "INT is not an integer." >&2
    exit 1
    fi
```

#### 11.3.6 算术判断 

Bash 还提供了((...))作为算术条件，进行算术运算的判断。

注意，算术判断不需要使用test命令，而是直接使用((...))结构。这个结构的返回值，决定了判断的真伪。

```
    $ if ((1)); then echo "It is true."; fi
    It is true.
    $ if ((0)); then echo "It is true."; else echo "it is false."; fi
    It is false.
```

#### 11.3.7 普通命令的逻辑运算 

```
    $ command1 && command2
    $ command1 || command2
```

对于&&操作符，先执行command1，只有command1执行成功后， 才会执行command2。对于||操作符，先执行command1，只有command1执行失败后， 才会执行command2。

```
    $ mkdir temp && cd temp
    # 上面的命令会创建一个名为temp的目录，执行成功后，才会执行第二个命令，进入这个目录。

    $ [ -d temp ] || mkdir temp
    # 上面的命令会测试目录temp是否存在，如果不存在，就会执行第二个命令，创建这个目录。这种写法非常有助于在脚本中处理错误。
```

### 11.4 case结构

case结构用于多值判断，可以为每个值指定对应的命令，跟包含多个elif的if结构等价，但是语义更好。它的语法如下。

```
    case expression in
    pattern )
        commands ;;
    pattern )
        commands ;;
    ...
    esac

```
```
    #!/bin/bash
    echo -n "输入一个1到3之间的数字（包含两端）> "
    read character
    case $character in
        1 ) echo 1
        ;;
        2 ) echo 2
        ;;
        3 ) echo 3
        ;;
        * ) echo 输入不符合要求
    esac
```
最后一条匹配语句的模式是*，这个通配符可以匹配其他字符和没有输入字符的情况，类似if的else部分。

case的匹配模式可以使用各种通配符

+ a)：匹配a。
+ a|b)：匹配a或b
+ [[:alpha:]]: 匹配单个字母
+ ？？？）: 匹配三个字符的词
+ *.txt) : 匹配.txt结尾
+ *): 匹配任意输入，通常作为case的最后一个模式

```
    #!/bin/bash

    echo -n "输入一个字母或数字 > "
    read character
    case $character in
    [[:lower:]] | [[:upper:]] ) echo "输入了字母 $character"
                                ;;
    [0-9] )                     echo "输入了数字 $character"
                                ;;
    * )                         echo "输入不符合要求"
    esac

```

## 十二 循环
---

Bash 提供三种循环语法for、while和until。
### 12.1 while循环
while循环有一个判断条件，只要符合条件，就不断循环执行指定的语句。
```
    while condition; do
    commands
    done
```

```
#!/bin/bash
    number=0
    while [ "$number" -lt 10 ]; do
    echo "Number = $number"
    number=$((number + 1))
    done
```

### 12.2 until循环

until循环与while循环恰好相反，只要不符合判断条件（判断条件失败），就不断循环执行指定的语句。一旦符合判断条件，就退出循环
```
    until condition; do
    commands
    done
```

### 12.3 for...in循环

for循环会依次从list列表中取出一项，作为变量variable，然后在循环体中进行处理。

```
    for variable in list
    do
    commands
    done
```

```
    #!/bin/bash
    count=0
    for i in $(cat ~/.bash_profile); do
    count=$((count + 1))
    echo "Word $count ($i) contains $(echo -n $i | wc -c) characters"
    done
```

### 12.4 break，continue

Bash 提供了两个内部命令break和continue，用来在循环内部跳出循环。

### 12.5 select命令

select生成一个菜单，内容是列表list的每一项，并且每一项前面还有一个数字编号。
Bash 提示用户选择一项，输入它的编号。
用户输入以后，Bash 会将该项的内容存在变量name，该项的编号存入环境变量REPLY。如果用户没有输入，就按回车键，Bash 会重新输出菜单，让用户选择。
执行命令体commands。
执行结束后，回到第一步，重复这个过程

```
    select name
    [in list]
    do
    commands
    done
```

```
    LEONFYANG-MB2:shell_practice leonfyang$ ./select.sh 
    1) Samsung
    2) Sony
    3) iphone
    4) symphony
    5) Walton
    #? 1
    You have chosen Samsung
    #? 2
    You have chosen Sony
    #? 3
    You have chosen iphone
    #? 
    1) Samsung
    2) Sony
    3) iphone
    4) symphony
    5) Walton
    #? 4
    You have chosen symphony
    #? ^C
```

