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

## Tutorials

<https://www.git-tower.com/learn/>

## Tools

[GUI Clients](https://git-scm.com/downloads/guis)

[GitLab](https://gitlab.com/)

[Gogs](https://gogs.io/)