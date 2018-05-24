# Node Manual

<https://nodejs.org/en/>

<https://www.npmjs.com/>

<https://npm.taobao.org/>

<https://npm.runkit.com/>

---

[TOC]

## Installation

[Downloads](https://nodejs.org/en/download/)

[All download options](https://nodejs.org/dist/)

[Previous Releases](https://nodejs.org/en/download/releases/)

### Linux Binaries

推荐这种方式

[node-install-bin.sh](https://github.com/mrhuangyuhui/node/blob/master/node-install-bin.sh)

```bash
yum install -y wget && curl -L https://github.com/mrhuangyuhui/node/raw/master/node-install-bin.sh | bash
```

### [Package Manager](https://nodejs.org/en/download/package-manager/)

<https://github.com/nodesource/distributions>

[CentOS](https://nodejs.org/en/download/package-manager/#enterprise-linux-and-fedora)

```bash
# 8.x
curl --silent --location https://rpm.nodesource.com/setup_8.x | sudo bash -

# 9.x
curl --silent --location https://rpm.nodesource.com/setup_9.x | sudo bash -

yum -y install nodejs
```

epel

```bash
yum install -y epel-release
yum install -y nodejs
```

[Ubuntu](https://nodejs.org/en/download/package-manager/#debian-and-ubuntu-based-linux-distributions)

```bash
# 8.x
curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -

# 9.x
curl -sL https://deb.nodesource.com/setup_9.x | sudo -E bash -

apt-get install -y nodejs
```

### Version Manager

<https://github.com/creationix/nvm>

相关笔记：NVM Manual

### Source Code

<https://github.com/nodejs/node/blob/master/BUILDING.md#building-nodejs-on-supported-platforms>

### Docker Image

<https://hub.docker.com/_/node/>

## References

[Node API](https://nodejs.org/dist/latest-v8.x/docs/api/)

[Node.js Built-in Modules](https://www.w3schools.com/nodejs/ref_modules.asp)

## Packages

### [nodemon](https://www.npmjs.com/package/nodemon)

开发工具，可检测文件变化并自动重启程序。

<https://github.com/remy/nodemon>

在线体验 <https://npm.runkit.com/nodemon>

## Tutorials

[Learn Node.js](https://www.w3schools.com/nodejs/)

## Tools

### [runkit](https://runkit.com/)

在线 node 环境

在线体验 node package <https://npm.runkit.com/>