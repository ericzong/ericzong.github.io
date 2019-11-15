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

# 安装

```powershell
# 安装位置配置
[environment]::setEnvironmentVariable('ChocolateyInstall', 'install path', 'User')
$env:ChocolateyInstall = 'install path'
# 安装代理配置
$env:chocolateyProxyLocation = 'https://local/proxy/server'
$env:chocolateyProxyUser = 'username'
$env:chocolateyProxyPassword = 'password'
# 在 PowerShell 中安装
Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
# PowerShell v3+
Set-ExecutionPolicy Bypass -Scope Process -Force; iwr https://chocolatey.org/install.ps1 -UseBasicParsing | iex
# 安装 GUI
choco install chocolateygui
```

> 更多参考 [这里](https://chocolatey.org/install)。

# 常用命令

```powershell
# 查看更新
choco outdated
# 更新
choco update all
```

# 代理配置

```powershell
choco config set proxy <locationandport>
choco config set proxyUser <username> #optional
choco config set proxyPassword <passwordThatGetsEncryptedInFile> # optional
choco config set proxyBypassList "'<bypasslist, comma separated>'" # optional, Chocolatey v0.10.4 required
choco config set proxyBypassOnLocal true # optional, Chocolatey v0.10.4 required
# 运行时配置参数
--proxy="'value'" --proxy-user="'<user>'" --proxy-password="'<pwd>'" --proxy-bypass-list="'<comma separated, list>'" --proxy-bypass-on-local
```

> 更多参考 [这里](https://github.com/chocolatey/choco/wiki/Proxy-Settings-for-Chocolatey#proxy-support-for-chocolatey)。

# 参考

[Chocolatey 官网](https://chocolatey.org/)

[Chocolatey 官网文档](https://chocolatey.org/docs)

[Chocolatey 官网教学](https://chocolatey.org/courses)

[Chocolatey GitHub](https://github.com/chocolatey/choco/)

[Chocolatey GitHub Wiki - 几乎能找到关于 Chocolatey 的一切](https://github.com/chocolatey/choco/wiki)