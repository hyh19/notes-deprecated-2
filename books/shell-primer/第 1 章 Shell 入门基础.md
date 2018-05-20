[TOC]

# 第 1 章 Shell 入门基础

## 1.1 为什么学习和使用 Shell 编程

略

## 1.2 什么是 Shell

略

## 1.3 作为程序设计语言的 Shell

略

## 1.4 向脚本传递参数

### 1.4.1 Shell 脚本的参数

变量名 | 说明
---|---
`$n` | 传递给脚本的第 n 个参数，例如 `$1` 表示第 1 个参数，`$2` 表示第 2 个参数……
`$#` | 命令行参数的个数
`$0` | 当前脚本的名称
`$*` | 以“参数1参数2参数3……”的形式返回所有参数的值
`$@` | 以“参数1”“参数2”“参数3”……的形式返回所有参数的值
`$_` | 保存之前执行的命令的最后一个参数

```bash
#!/usr/bin/env bash

echo $#
echo $@
```

输出结果：
```bash
$ ./example.sh a "b c"
2
a b c
```

**注意：对于包含空白字符或者其他特殊字符的参数，需要使用单引号或者双引号进行传递。**

**如果用户传递的参数多于 9 个，则不能使用 `$10` 来引用第 10 个参数。为了能够获取第 10 个参数的值，用户必须处理或保存第 1 个参数，即 `$1`，然后使用 `shift` 命令删除参数1，并将所有剩余的参数下移 1 位，此时 `$10` 就变成了 `$9`，依此类推。`$#` 的值将被更新以反映参数的剩余数量。**

### 1.4.2 参数扩展

```bash
#!/usr/bin/env bash

echo "OPTIND starts at $OPTIND"

# getopts 后的选项字符串前面有冒号 :，编译器不会提示选项错误信息，由用户自己处理。
while getopts ":pq:" optname; do
    case $optname in
        "p")
            echo "Option $optname is specified"
            ;;
        "q")
            echo "Option $optname has value $OPTARG"
            ;;
        "?")
            # 自己处理未知选项错误，编译器不提示。
            echo "Unknown option $OPTARG"
            ;;
        ":")
            # 自己处理缺少选项参数错误，编译器不提示。
            echo "No argument value for option $OPTARG"
            ;;
        *)
            # Should not occur
            echo "Unknown error while processing options"
            ;;
    esac
    echo "OPTIND is now $OPTIND"
done
```

输出结果：
```bash
# 只有不带参数的选项
$ ./example.sh -p
OPTIND starts at 1
Option p is specified.
OPTIND is now 2.

# 只有带参数的选项
$ ./example.sh -q Hello
OPTIND starts at 1
Option q has value Hello
OPTIND is now 3

# 同时有两个选项
$ ./example.sh -p -q Hello
OPTIND starts at 1
Option p is specified
OPTIND is now 2
Option q has value Hello
OPTIND is now 4

# 选项缺少参数，自己处理错误。
$ ./example.sh -q
OPTIND starts at 1
No argument value for option q
OPTIND is now 2

# 未知选项，自己处理错误。
$ ./example.sh -f
OPTIND starts at 1
Unknown option f
OPTIND is now 2
```

```bash
省略……

# getopts 后的选项字符串前面没有冒号 :，编译器会提示选项错误信息。
while getopts "pq:" optname
# while getopts ":pq:" optname

省略……
```

输出结果：
```bash
# 编译器提示缺少选项参数错误
$ ./example.sh -q
OPTIND starts at 1
./example.sh: option requires an argument -- q
Unknown option 
OPTIND is now 2

# 编译器提示未知选项错误
$ ./example.sh -f
OPTIND starts at 1
./example.sh: illegal option -- f
Unknown option 
OPTIND is now 2
```

#### 其他参考文章：

**[《getopts 命令行参数处理》](http://www.cnblogs.com/xiangzi888/archive/2012/04/03/2430736.html)**

使用内部命令 `getopts` 可以很方便地处理命令行参数。一般格式为：

`getopts options variable`

`getopts` 的设计目标是在循环中运行，每次执行循环，`getopts` 就检查下一个命令行参数，并判断它是否合法。即**检查参数是否以 `-` 开头**，后面跟一个包含在 `options` 中的字母。如果是，就把匹配的选项字母存在指定的变量 `variable` 中，并返回退出状态 `0`；如果 `-` 后面的字母没有包含在 `options` 中，就在 `variable` 中存入一个 `?`，并返回退出状态 `0`；如果命令行中已经没有参数，或者下一个参数不以 `-` 开头，就返回不为 `0` 的退出状态。
```bash
#!/usr/bin/env bash

while getopts h:ms option; do
    case "$option" in
        "h")
            echo "h: $OPTARG"
            ;;
        "m")
            echo "m"
            ;;
        "s")
            echo "s"
            ;;
        "?")
            echo "Usage: ./example.sh [-h n] [-m] [-s]"
            exit 1
            ;;
    esac
    echo "Next argument index: $OPTIND"
done
```

输出结果：
```bash
$ ./example.sh -h 100 -ms
h: 100
Next argument index: 3
m
Next argument index: 3
s
Next argument index: 4

$ ./example.sh -h 100 -m -s
h: 100
Next argument index: 3
m
Next argument index: 4
s
Next argument index: 5

$ ./example.sh -f
./example.sh: illegal option -- f
Usage: ./example.sh [-h n] [-m] [-s]
```
注：

- `getopts` 允许把多个选项堆叠在一起（如 -ms）。

- 如果选项后要带参数，必须在对应选项后加冒号 `:`，此时选项和参数之间至少要有一个空白字符分隔，这样的选项不能堆叠。

- 如果在需要参数的选项之后没有找到参数，它就在给定的变量中存入 `?` ，并向标准错误中写入错误消息。**否则将实际参数写入特殊变量 `OPTARG`**。

- 另外一个特殊变量 `OPTIND`，表示下一个要处理的参数索引，初值是 1，每次执行 `getopts` 时都会更新。


## 1.5 第一个 Shell 程序：Hello，Bash Shell!

### 1.5.1 Shell 脚本的基本元素

略

### 1.5.2 指定命令解读器

略

### 1.5.3 Shell 脚本中的注释和风格

多行注释
```bash
#!/usr/bin/env bash

:<<EOB
Apple
Orange
EOB

echo "Hello, world!"
```

### 1.5.4 如何执行 Shell 程序

略

### 1.5.5 Shell 程序的退出状态

在 UNIX 或者 Linux 中，每个命令都会返回一个退出状态码。退出状态码是一个整数，其有效范围为 ++**0～255**++。通常情况下，成功的命令返回 0，而不成功的命令返回非 0 值。非 0 值通常都被解释成一个错误码。运行良好的 UNIX 命令、程序和工具都会返回 0 作为退出码来表示成功，偶尔也会有例外。\
同样，Shell 脚本中的 ++**函数和脚本本身**++ 也会返回退出状态码。在脚本或者是脚本函数中执行的最后的命令会决定退出状态码。另外，用户也可以在脚本中使用 `exit` 语句将指定的退出状态码传递给 Shell。

```bash
#!/usr/bin/env bash

echo "Hello, world!"
# 退出状态为 0，因为命令执行成功。
echo $?

# 无效命令
abc

# 非零的退出状态，因为命令执行失败。
echo $?

# 返回 120 退出状态给 Shell
exit 120
```

输出结果：
```
$ ./example.sh
Hello, world!
0
./example.sh: line 6: abc: command not found
127
$ echo $?
120
```

学习结束 \
复习 2017.1126 \
复习 2018.3.18