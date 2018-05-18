[TOC]

# Linux Manual

https://www.kernel.org/

http://www.gnu.org/

https://github.com/judasn/Linux-Tutorial

https://github.com/arismelachroinos/lscript

---

[TOC]

## Commands

### `alias`

设置命令别名

```bash
$ alias lm='ls -al | more'
# 查看所有命令别名
$ alias
alias cp='cp -i'
alias egrep='egrep --color=auto'
alias fgrep='fgrep --color=auto'
alias grep='grep --color=auto'
alias l.='ls -d .* --color=auto'
alias ll='ls -l --color=auto'
alias lm='ls -al | more'
alias ls='ls --color=auto'
alias mv='mv -i'
alias rm='rm -i'
alias which='alias | /usr/bin/which --tty-only --read-alias --show-dot --show-tilde'
# 删除命令别名
$ unalias lm
```

### `history`

历史命令

```bash
# 查看所有的历史命令
$ history
    1  alias
    2  alias lm='ls -al | more'
    3  alias
    4  unalias lm
    5  history
# 查看最近三笔历史命令
$ history 3
    4  unalias lm
    5  history
    6  history 3
# 把新的历史命令更新到记录文件 ~/.bash_history
$ history -w
```

相关变量：`HISTSIZE`

相关文件：`~/.bash_history`

执行历史命令

```bash
$ history
    1  alias
    2  alias lm='ls -al | more'
    3  alias
    4  unalias lm
    5  history
    6  history 3
    7  history -w
    8  echo ${HISTSIZE}
    9  ll ~/.bash_history
   10  cat ~/.bash_history
   11  history
# 执行第 8 条历史命令
$ !8
echo ${HISTSIZE}
1000
# 执行最近一条历史命令
$ !!
echo ${HISTSIZE}
1000
# 执行最近一条以 al 开头的历史命令
$ !al
alias
alias cp='cp -i'
alias egrep='egrep --color=auto'
alias fgrep='fgrep --color=auto'
alias grep='grep --color=auto'
alias l.='ls -d .* --color=auto'
alias ll='ls -l --color=auto'
alias ls='ls --color=auto'
alias mv='mv -i'
alias rm='rm -i'
alias which='alias | /usr/bin/which --tty-only --read-alias --show-dot --show-tilde'
```

### `type`

查看命令的类型

```bash
$ type ls
ls is aliased to `ls --color=auto'
$ type -t ls
alias
$ type -a ls
ls is aliased to `ls --color=auto'
ls is /usr/bin/ls
$ type cd
cd is a shell builtin
```

### `unalias`

删除命令别名

```bash
unalias lm
```

## Files

### `~/.bash_history`

历史命令

```bash
$ cat ~/.bash_history
alias
alias lm='ls -al | more'
alias
unalias lm
history
history 3
history -w
```

相关命令：`history`

相关变量：`HISTSIZE`

### `~/.bash_login`

个人环境配置

### `~/.bash_logout`

登出 bash 时要做的动作

### `~/.bash_profile`

个人环境配置

```bash
$ cat ~/.bash_profile
# .bash_profile

# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi

# User specific environment and startup programs

PATH=$PATH:$HOME/bin

export PATH
```

### `~/.bashrc`

个人环境设置

```bash
$ cat ~/.bashrc
# .bashrc

# User specific aliases and functions

alias rm='rm -i'
alias cp='cp -i'
alias mv='mv -i'

# Source global definitions
if [ -f /etc/bashrc ]; then
        . /etc/bashrc
fi
```

### `~/.profile`

个人环境配置

### `/etc/bashrc`

系统环境设定，这个文件是 Red Hat 系统特有的。

```bash
$ head /etc/bashrc
# /etc/bashrc

# System wide functions and aliases
# Environment stuff goes in /etc/profile

# It's NOT a good idea to change this file unless you know what you
# are doing. It's much better to create a custom.sh shell script in
# /etc/profile.d/ to make custom changes to your environment, as this
# will prevent the need for merging in future updates.
```

### `/etc/group`

群组

```bash
$ head -n 3 /etc/group
root:x:0:
bin:x:1:
daemon:x:2:
```

### `/etc/gshadow`

群组密码

```bash
$ head -n 3 /etc/gshadow
root:::
bin:::
daemon:::
```

### `/etc/passwd`

账号

```bash
$ head -n 3 /etc/passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
```

### `/etc/profile`

系统环境配置，不要随意修改这个文件，会影响到整个系统。

```bash
$ head /etc/profile
# /etc/profile

# System wide environment and startup programs, for login setup
# Functions and aliases go in /etc/bashrc

# It's NOT a good idea to change this file unless you know what you
# are doing. It's much better to create a custom.sh shell script in
# /etc/profile.d/ to make custom changes to your environment, as this
# will prevent the need for merging in future updates.
```

### `/etc/locale.conf`

语系配置

```bash
$ cat /etc/locale.conf
LANG=en_US.UTF-8
```

### `/etc/man_db.conf`

使用手册的配置

### `/etc/profile.d/`

自定义环境设定，被 `/etc/profile` 读取。

```bash
$ ll /etc/profile.d/
total 48
-rw-r--r--. 1 root root  771 Jan 25 15:59 256term.csh
-rw-r--r--. 1 root root  841 Jan 25 15:59 256term.sh
-rw-r--r--. 1 root root  196 Mar 24  2017 colorgrep.csh
-rw-r--r--. 1 root root  201 Mar 24  2017 colorgrep.sh
-rw-r--r--. 1 root root 1741 Nov  4  2016 colorls.csh
-rw-r--r--. 1 root root 1606 Nov  4  2016 colorls.sh
-rw-r--r--. 1 root root 1706 Jan 25 15:59 lang.csh
-rw-r--r--. 1 root root 2703 Jan 25 15:59 lang.sh
-rw-r--r--. 1 root root  123 Jul 30  2015 less.csh
-rw-r--r--. 1 root root  121 Jul 30  2015 less.sh
-rw-r--r--. 1 root root  164 Jan 27  2014 which2.csh
-rw-r--r--. 1 root root  169 Jan 27  2014 which2.sh
```

### `/etc/shadow`

账号密码

```bash
$ head -n 3 /etc/shadow
root:*:16176:0:99999:7:::
bin:*:17110:0:99999:7:::
daemon:*:17110:0:99999:7:::
```

### `/etc/shells`

可用的 shells

```bash
$ cat /etc/shells
/bin/sh
/bin/bash
/sbin/nologin
/usr/bin/sh
/usr/bin/bash
/usr/sbin/nologin
```

## Variables

### `HISTSIZE`

历史命令的最大数量

```bash
$ echo ${HISTSIZE}
1000
```

相关命令：`history`

相关文件：`~/.bash_history`

lsb_release 查看系统版本
```
# 安装
yum install -y redhat-lsb-core
```

tar
```bash
xz -d example.tar.xz && tar -xvf example.tar
```

## Packages

https://pkgs.org/

## [Coreutils - GNU core utilities](https://www.gnu.org/software/coreutils/manual/coreutils.html)

### [21 System context](https://www.gnu.org/software/coreutils/manual/coreutils.html#System-context)

#### [21.2 `arch`: Print machine hardware name](https://www.gnu.org/software/coreutils/manual/coreutils.html#arch-invocation)

`arch` prints the machine hardware name, and is equivalent to `uname -m`.
```bash
$ arch
x86_64
```

#### [21.3 nproc: Print the number of available processors](https://www.gnu.org/software/coreutils/manual/coreutils.html#nproc-invocation)

Print the number of processing units available to the current process, which may be less than the number of online processors.
```bash
$ nproc
1
```

#### [21.4 `uname`: Print system information](https://www.gnu.org/software/coreutils/manual/coreutils.html#uname-invocation)

Print all information
```
$ uname -a
Linux debian-512mb-sgp1-01 3.16.0-4-amd64 #1 SMP Debian 3.16.43-2+deb8u2 (2017-06-26) x86_64 GNU/Linux
```

Print the kernel release
```bash
$ uname -r
3.16.0-4-amd64
```

Print the kernel version
```
$ uname -v
#1 SMP Debian 3.16.43-2+deb8u2 (2017-06-26)
```

#### [21.5 `hostname`: Print or set system name](https://www.gnu.org/software/coreutils/manual/coreutils.html#hostname-invocation)

Get the hostname 
```bash
$ hostname
mysql.example.com
$ cat /etc/hostname
mysql.example.com
```

Set the hostname. Note, that  this  is  effective  only  until  the  next reboot. Edit /etc/hostname for permanent change.
```bash
$ hostname redis.example.com
$ hostname
redis.example.com
$ cat /etc/hostname
mysql.example.com

$ echo redis.example.com > /etc/hostname
$ reboot ## REBOOT!!! ##
$ hostname
redis.example.com
```

#### [21.7 `uptime`: Print system uptime and load](https://www.gnu.org/software/coreutils/manual/coreutils.html#uptime-invocation)

`uptime` prints the current time, the system's uptime, the number of logged-in users and the current load average.
```bash
$ uptime
 03:48:42 up 28 min,  2 users,  load average: 0.00, 0.00, 0.00
```

```bash
# 读取系统平均负载。后面的分数，分母表示系统进程总数，分子表示正在运行的进程数，最后一个数字表示最近运行的进程 ID。
$ cat /proc/loadavg
0.00 0.00 0.00 1/74 1248

# 读取 CPU 核心数目
$ grep 'model name' /proc/cpuinfo | wc -l
1
```

## Tutorials

http://linux.vbird.org/