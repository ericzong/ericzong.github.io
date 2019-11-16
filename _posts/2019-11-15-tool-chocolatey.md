---
layout: posts
title: Chocolatey入门
category: 工具
tags: Chocolatey 入门
excerpt: Chocolatey入门教程。
author: Eric Zong
---

* content
{:toc}

# 概述

Chocolatey 是一个 Windows 包管理器，使得软件管理自动化。

> 事实上，Chocolatey 与 [scoop](https://scoop.sh/) 在功能上很相似。只是前者更倾向于管理安装软件，而后者倾向于管理便携式软件；另外，前者是有商业版本的。

# 安装

> 如果你使用的是较新的 Windows 系统，比如 Win10，那么安装 Chocolatey 是很简单的。因为，Chocolatey 需要 PowerShell v2+ 和 .NET Framework 4+ 支持，而较新的系统这些都是内置的。

## PowerShell在线安装

在 PowerShell 中在线安装 Chocolatey 是极为方便的，仅需要执行以下命令：

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

以上是一个兼容脚本，如果你的 PowerShell 版本是 3+，那么下面命令也是可用的：

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; iwr https://chocolatey.org/install.ps1 -UseBasicParsing | iex
```

> 更多安装方式参考 [这里](https://chocolatey.org/install) 或 [教程](https://chocolatey.org/courses/installation/installing)。

## 自定义安装位置

直接执行安装命令会将 Chocolatey 安装到默认位置，如果想要自定义安装位置，仅需要通过 `ChocolateyInstall` 环境变量指定即可。

```powershell
[environment]::setEnvironmentVariable('ChocolateyInstall', '/path/to/install/dir', 'User')
$env:ChocolateyInstall = '/path/to/install/dir'
```

## 安装代理

如果需要通过代理来安装，临时配置以下环境变量即可：

```powershell
$env:chocolateyProxyLocation = 'https://local/proxy/server'
$env:chocolateyProxyUser = 'username'
$env:chocolateyProxyPassword = 'password'
```

## GUI

商业版的 Chocolatey 是有 GUI 支持，如果你习惯于在 GUI 模式下工作，但又不想购买商业版，那么可以安装 Chocolatey GUI 应用，它提供有限的 GUI 支持：

```powershell
choco install chocolateygui
```

# 常用命令

> 学习 Chocolatey 命令最好的方式不是查看相关文档，而是使用它的帮助系统。

Chocolatey 的主命令是 `chocolatey`，但它很长，因此，通常我们使用缩略命令 `choco`。

**帮助系统**

```powershell
choco --help
```

> 与大多数现代终端命令类似，Chocolatey 的命令格式为 `choco 子命令 选项/开关`。
>
> 需要注意的是，Chocolatey 支持 3 种**选项/开关前缀**：`-`、`--` 和 `/`。
>
> 而对于帮助选项/开关而言，它有 3 种形式：`?`、`help`、`h`。
>
> 因此，我们可以组合出 9 种进入帮助系统的命令，虽然传递选项/开关的说明中指出 `-` 通常不与单个字符的缩写选项/开关一起使用，但是实际上这 9 个命令在最新的版本（v0.10.15）中都是有效的。

**配置**

```powershell
# 查看配置
choco config
choco config list
# 查看配置项
choco config get <name>
choco config get --name <name>
# 设置配置项
choco config set <name> <value>
choco config set --name <name> --value <value>
# 删除配置项
choco config unset <name>
choco config unset --name <name>
# 代理配置
choco config set proxy <locationandport>
choco config set proxyUser <username> #optional
choco config set proxyPassword <passwordThatGetsEncryptedInFile> # optional
choco config set proxyBypassList "'<bypasslist, comma separated>'" # optional, Chocolatey v0.10.4 required
choco config set proxyBypassOnLocal true # optional, Chocolatey v0.10.4 required
# 运行时配置参数
--proxy="'value'" --proxy-user="'<user>'" --proxy-password="'<pwd>'" --proxy-bypass-list="'<comma separated, list>'" --proxy-bypass-on-local
```

> 更多参考 [这里](https://github.com/chocolatey/choco/wiki/Proxy-Settings-for-Chocolatey#proxy-support-for-chocolatey)。

**其他**

```powershell
choco search <filter>
choco list <filter>
choco list --local-only
choco list --page=0 --page-size=25

choco install <app>
choco uninstall <app>

choco outdated

choco upgrade all
choco upgrade <app>
```

# 参考

[Chocolatey 官网](https://chocolatey.org/)

[Chocolatey 官网文档](https://chocolatey.org/docs)

[Chocolatey 官网教学](https://chocolatey.org/courses)

[Chocolatey GitHub](https://github.com/chocolatey/choco/)

[Chocolatey GitHub Wiki - 几乎能找到关于 Chocolatey 的一切](https://github.com/chocolatey/choco/wiki)