# Git Manual

@(Git)[git,manual]

<https://git-scm.com/>

<https://git-scm.com/doc>

<https://git-scm.com/book/en/v2>

<https://github.com/git/git>

---

[TOC]

## [Installation](https://git-scm.com/downloads)

### YUM

```bash
yum install -y git
```

### [Source](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

[install-git-from-source.sh](https://github.com/mrhuangyuhui/git/blob/master/install-git-from-source.sh)

```bash
# CentOS 6.9/7.4, Ubuntu 16.04
curl -L https://github.com/mrhuangyuhui/git/raw/master/install-git-from-source.sh | bash
```

```bash
# CentOS
yum install epel-release -y
yum install wget tar autoconf automake libtool -y
yum install dh-autoreconf libcurl-devel expat-devel gettext-devel openssl-devel perl-devel zlib-devel -y
yum install asciidoc xmlto docbook2X -y

# Ubuntu
apt-get install wget tar
apt-get install dh-autoreconf libcurl4-gnutls-dev libexpat1-dev  gettext zlib1g-dev libssl-dev -y
apt-get install asciidoc xmlto docbook2x -y

ln -s /usr/bin/db2x_docbook2texi /usr/bin/docbook2x-texi

wget https://www.kernel.org/pub/software/scm/git/git-2.15.0.tar.gz
tar -zxf git-2.15.0.tar.gz
cd git-2.15.0
make configure
./configure --prefix=/usr/local/git/git-2.15.0
make all doc info
make install install-doc install-html install-info
```

## Commands

快速帮助

```bash
git
git --help
# 子命令使用手册
git help pull
# 列出所有子命令
git help -a
```

使用手册

```bash
man git
```

### `clone`

克隆仓库

```bash
git clone https://github.com/mrhuangyuhui/notes.git
```

### `pull`

更新本地工作目录

```bash
git pull
```

### `checkout`

切换分支

```bash
git checkout develop
```

### `brance`

```bash
# 列出本地分支
git branch
# 列出远程分支
git branch -r
# 列出所有分支，包括本地和远程。
git branch -a
```

### `ssh-keygen`

<https://www.git-tower.com/learn/git/ebook/cn/command-line/advanced-topics/ssh-public-keys>

<https://help.github.com/articles/connecting-to-github-with-ssh/>

## [References](https://git-scm.com/docs)

### Cheat-sheets

<https://www.git-tower.com/blog/git-cheat-sheet-cn/>

[English](https://services.github.com/on-demand/downloads/github-git-cheat-sheet/)

[中文](https://services.github.com/on-demand/downloads/zh_CN/github-git-cheat-sheet/)

### [gitignore](https://git-scm.com/docs/gitignore)

<https://github.com/github/gitignore>

<https://help.github.com/articles/ignoring-files/>

注意：

- 一个仓库 repo 内可以有多个 `.gitignore` 文件，可以在仓库根目录下，如 `repo/.gitignore`，也可以在仓库的子目录下，如 `repo/foo/.gitignore`。
- 文件 `.gitignore` 所列规则的根目录指的是该文件所在的目录，如 `repo/.gitignore` 的根目录是 `repo/`，`repo/foo/.gitignore` 的根目录是 `repo/foo/`。
- 已纳入版本控制的文件不受 `.gitignore` 影响，应尽量在建立仓库时创建忽略规则。

`/` 表示目录

```bash
# 忽略根目录下名称为 foo 的文件或目录
/foo

# 忽略名称为 foo 的目录
foo/

# 忽略根目录下名称为 foo 的目录
/foo/

# 忽略名称为 foo 的文件或目录
foo
```

`**` 表示任意层目录

```bash
# 忽略名称为 foo 的文件或目录
**/foo
# 等价于
foo

# 忽略 foo 目录下名称为 bar 的文件或目录
**/foo/bar

# 忽略 abc 目录（注意：该目录与 .gitignore 在同一目录）下所有文件和目录
abc/**

# 忽略 a 目录（注意：该目录与 .gitignore 在同一目录）下名称为 b 的文件或目录，如 a/b a/x/b a/x/y/b。
a/**/b
```

`*` 表示任意多个字符

```bash
# 忽略名称以 .class 结尾的文件或目录
*.class
```

`?` 表示任意单个字符

```bash
# 忽略名称为任意单个字符加 .class 的文件或目录，如 a.class b.class 会被忽略，而 .class ab.class 都不会被忽略。
?.class
```

`[]` 表示匹配方括号中的其中一个字符

```bash
# 忽略名称为 a.class b.class c.class 的文件或目录
[abc].class
```

`!` 表示不忽略

```bash
# 忽略名称以 .class 结尾的文件或目录，a.class 除外。
*.class
!a.class
```

`\` 表示字符转义

```bash
# 忽略名称为 #.class 的文件或目录
\#.class
# 忽略名称为 !.class 的文件或目录
\!.class
```

## Tutorials

<https://www.git-tower.com/learn/>

## Tools

[GUI Clients](https://git-scm.com/downloads/guis)

[GitLab](https://gitlab.com/)

[Gogs](https://gogs.io/)