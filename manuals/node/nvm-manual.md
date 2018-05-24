# NVM Manual

<https://github.com/creationix/nvm>

---

[TOC]

## [Installation](https://github.com/creationix/nvm#installation)

Using cURL:

```bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.6/install.sh | bash
```

or Wget:

```bash
wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.6/install.sh | bash
```

To verify that nvm has been installed, do:

```bash
command -v nvm
```

which should output 'nvm' if the installation was successful. Please note that `which nvm` will not work, since `nvm` is a sourced shell function, not an executable binary.

## Commands

帮助文档

```bash
nvm
num --help
```

列出可安装的版本

```bash
# 列出所有版本
nvm ls-remote

# 只列出 LTS 版本
nvm ls-remote --lts

# 只列出指定版本号的版本
nvm ls-remote 8
nvm ls-remote 8.9

# 只列出指定版本号的 LTS 版本
nvm ls-remote 8.9 --lts
```

安装指定版本

```bash
nvm install 8.9.1
```

列出已安装的版本

```bash
nvm list
```

使用指定版本

```bash
nvm use 8.9.1
```

查看当前使用版本

```bash
nvm current
```

在当前 shell 停用 `nvm`

```bash
$ nvm deactivate
/root/.nvm/*/bin removed from $PATH
$ node -v
-bash: node: command not found
```