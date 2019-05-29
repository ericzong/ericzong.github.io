---
layout: post
title: nvm入门
category: Node.js
tags: Node.js 入门
excerpt: "nvm入门。nvm是Node.js的一个多版本管理工具，可以灵活的切换Node.js版本。"
author: "Eric Zong"
---

* content
{:toc}

# 简介

nvm 是 Node.js 的一个多版本管理工具，可以灵活的切换 Node.js 版本。

# 下载

https://github.com/coreybutler/nvm-windows/releases

nvm 有两种安装包：安装版与绿色版。

# 安装

## 绿色版

### 解压压缩包

解压到任意目录作为 nvm 根目录，比如：C:/nvm （需全英文）。

### 创建配置文件

在 nvm 根目录新建 settings.txt 文件，添加类似如下配置信息：

```
root:C:/nvm
path:C:/nvm/nodejs
arch:64
proxy:
```

* root：nvm 根目录
* path：node 快捷方式路径
* arch：当前操作系统位数（32/64）
* proxy：代理地址（可选）

### 配置环境变量

**新增**

`NVM_HOME`：nvm 根目录（同配置文件中的 root）

`NVM_SYMLINK`：node 快捷方式路径（同配置文件中的 path）

**修改**

`PATH`：追加 `%NVM_HOME%;%NVM_SYMLINK%`

## 安装版

使用可执行文件安装是最简单的，安装向导会询问 nvm 的安装路径及 node 快捷方式路径，然后会自动生成配置文件，并配置环境变量。

# 安装 node

```shell
nvm install <node_version>
```

执行安装命令并指定 node 版本号即可进行安装。安装完成后，使用 `use` 命令切换要使用的 node 版本。

```shell
nvm use <node_version>
```

注：版本号可使用 latest 指代最新版本。

# 常用命令

```shell
# 查看 nvm 版本
nvm version
# 查看 nvm 根目录
nvm root
# 安装 node 版本
nvm install <node_version>
# 卸载 node 版本
nvm uninstall <node_version>
# 查看已安装 node 版本
nvm list
# 切换到指定 node 版本
nvm use <node_version>
```

# 原理

首先，安装某版本的 node 后，会发现 nvm 根目录下多了一个该版本（比如：v11.0.0）的目录，node 的文件都在其下。安装多个版本后，就会得到多个版本目录。

接下来，最容易想到的版本切换方式是，当需要使用某个版本时，就将该版本目录加到 Path 中即可。但是，由于涉及版本频繁切换，如果单纯将版本目录添加到 Path，就会涉及到需要“清理”旧版本路径，这可能会引起一些问题。

nvm 使用了一个比较通用的技巧，它生成了一个目录联接，比如：`C:/nvm/nodejs`，它指向当前使用的 node 版本目录。而在 Path 中添加的正是这个目录联接的路径。在切换版本时，则不再需要操作环境变量了，仅仅是将该目录联接指向相应的版本目录即可。

# 附录

## 全英文目录

nvm 根目录和 node 快捷方式路径都应该是全英文的。因为，最终它们都会被加入 path 环境变量中，而当 path 环境变量中存在中文等非英文字符时，一些依赖 path 的软件会出现问题。

总之，path 中应尽量保证均为全英文路径。

## 配置镜像

“墙”是常态，所以这些自带中央仓库并且服务器在外国的软件总是需要配置镜像的。

在 nvm 的配置文件 settings.txt 中追加镜像信息（这里以淘宝镜像为例）：

```
node_mirror: https://npm.taobao.org/mirrors/node/
npm_mirror: https://npm.taobao.org/mirrors/npm/
```

