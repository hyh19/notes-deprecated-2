# NPM Manual

@(Node)[node,npm,manual]

<https://www.npmjs.com/>

<https://npm.taobao.org/>

<https://npm.runkit.com/>

---

[TOC]

## [Installation](https://www.npmjs.com/get-npm)

`npm` is distributed with Node.js- which means that when you download Node.js, you automatically get `npm` installed on your computer.

升级 `npm` 到最新版本

```bash
npm install npm@latest -g
```

[CNPM](https://npm.taobao.org/)

淘宝 NPM 镜像

配置淘宝镜像

```bash
npm config set registry https://registry.npm.taobao.org -g
```

安装 `cnpm`

```bash
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

## Commands

快速帮助

```bash
npm
npm help
# 子命令的快速帮助
npm install -h
```

列出所有子命令的使用方法

```bash
npm -l
```

使用手册

```bash
npm help npm
```

### [`install`](https://docs.npmjs.com/cli/install)

安装

```bash
# 安装 package.json 列出的包
npm install
# 局部安装指定包
npm install jshint
# 安装并保存到 package.json
npm install jshint --save
npm install jshint --save-dev
# 全局安装指定包
npm install -g jshint
```

[Installing npm packages locally](https://docs.npmjs.com/getting-started/installing-npm-packages-locally)

[Installing npm packages globally](https://docs.npmjs.com/getting-started/installing-npm-packages-globally)

### [`init`](https://docs.npmjs.com/cli/init)

创建 `package.json`

```bash
npm init
# 按默认方式创建
npm init -y
```

[Using a `package.json`](https://docs.npmjs.com/getting-started/using-a-package.json)

## Files

### [`package.json`](https://docs.npmjs.com/files/package.json)

- [devDependencies](https://docs.npmjs.com/files/package.json#devdependencies)
- [files](https://docs.npmjs.com/files/package.json#files)
- [keywords](https://docs.npmjs.com/files/package.json#keywords)
- [main](https://docs.npmjs.com/files/package.json#main)
- [repository](https://docs.npmjs.com/files/package.json#repository)
- [scripts](https://docs.npmjs.com/files/package.json#scripts)

[Working with `package.json`](https://docs.npmjs.com/getting-started/using-a-package.json)

### [`npmrc`](https://docs.npmjs.com/files/npmrc)

## References

<https://docs.npmjs.com/>

## Packages

### [nodemon](https://www.npmjs.com/package/nodemon)

开发工具，可检测文件变化并自动重启程序。

<https://github.com/remy/nodemon>

在线体验 <https://npm.runkit.com/nodemon>