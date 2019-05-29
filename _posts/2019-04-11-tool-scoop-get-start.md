---
layout: post
title: "Scoop 入门"
categories: 工具
tags: Scoop 应用管理
excerpt: "介绍Scoop的基本使用。"
author: "Eric Zong"
---

* content
{:toc}

# 简介

[Scoop](https://scoop.sh/) 是一个 Windows 应用安装器，可以从命令行安装和管理各种应用。

需要注意的是，Scoop 不是一个软件包管理器，它没有提供任何注册发布应用的仓库。简单而言，Scoop 的“仓库”只提供应用的下载地址、安装方式以及依赖等信息。

# 安装

Scoop 依赖于 Powershell 3+ 和 .NET Framework 4.5+，因此，安装 Scoop 前请确保已经正确安装了它们。

> 注意：Scoop 是基于 PowerShell 的，因此，下文中命令行特指的是 PowerShell 命令行环境。

命令行执行如下命令即可安装：

```powershell
iex (new-object net.webclient).downloadstring('https://get.scoop.sh')
```

> 该命令将从 GitHub 下载并安装 Scoop，因此如果访问速度较慢，可以参考下文“代理配置”部分。

如果出现“权限”错误，请根据错误提示执行以下命令：

```powershell
Set-ExecutionPolicy RemoteSigned -scope CurrentUser
```

## 自定义

如果使用上面的命令直接安装 Scoop，那么 Scoop 会使用默认配置——应用的安装位置将是默认的。

`scoop install` 命令默认情况下仅为当前用户安装应用，除非指定 `-g` 选项将应用安装为全局的。对于个人电脑而言，一般只有一个用户，所以，通常我们不太关心应用是否全局安装。

但是，如果我们想自定义 Scoop 应用的安装位置，就需要在安装 Scoop 前进行一些配置。

### 用户安装位置

Scoop 安装用户应用的默认位置是：`~/scoop`，如下命令可自定义该位置。

```powershell
# 设置用户环境变量 SCOOP
[environment]::setEnvironmentVariable('SCOOP', 'D:\program\scoop', 'User')
# 使 SCOOP 在当前命令行生效
$env:SCOOP='D:\program\scoop'
# 安装用户应用
scoop install <APP-NAME>
```

### 全局安装位置

Scoop 安装全局应用的默认位置是：`C:\ProgramData\scoop`，如下命令可自定义该位置。

```powershell
# 设置全局环境变量
[environment]::setEnvironmentVariable('SCOOP_GLOBAL','D:\program\scoop-apps','Machine')
# 使 SCOOP_GLOBAL 在当前命令行生效
$env:SCOOP_GLOBAL='D:\program\scoop-apps'
# 安装全局应用
scoop install -g <APP-NAME>
```

> `-g` 是 `--global` 的缩写。

# 卸载

Scoop 可以在命令行自己卸载自己哦：

```powershell
scoop uninstall scoop
```

# 开始使用

要上手 Scoop 这样的工具，最简单的当然是使用它自带的帮助系统了。

```powershell
scoop help
```

该命令会列出 Scoop 所有可用的子命令。

然后，还可以进一步查看这些子命令的帮助信息：

```powershell
scoop help <SUB-CMD>
```

这会列出子命令的详细描述、用法、示例、选项等等信息。参考示例便可以快速上手熟悉这些命令。

## 常用命令

使用 Scoop 最常用的操作就是查找、安装、更新、卸载……

以下便是最常用的命令：

```powershell
# 查找指定应用，可仅提供部分应用名
scoop search <QUERY>
# 安装应用
scoop install <APP-NAME>
# 卸载应用
scoop uninstall <APP-NAME>
# 查看状态，列出可更新应用
scoop status
# 更新指定应用
scoop update <APP-NAME>
# 更新所有可更新应用
scoop update *
# 查看已安装的应用
scoop list
```

## Bucket

作为一个“应用安装器”，Scoop 使用 Bucket 来定义软件源。

> 注意：Bucket 维护的是应用相关信息清单，而不是应用本身。Scoop 没有用于存放应用的中央仓库。

Scoop 提供了一个内置的 [main bucket](https://github.com/lukesampson/scoop/tree/master/bucket)，存放了许多“符合标准”的应用。

默认情况下，当执行 `scoop search`、`scoop install` 等命令时，Scoop 便是从 main bucket 中搜索应用的。

但这不足以满足各种人群的需要，因此，Scoop 还提供了几个 known bucket，通过以下命令可查看它们的名字：

```powershell
scoop bucket known
```

这些 known bucket 通常都是 GitHub 项目，使用如下命令可为 Scoop 添加指定的 known bucket：

```powershell
scoop bucket add <NAME>
```

添加 known bucket 后就可以从其中搜索安装应用了。

> 如果这些 bucket 还满足不了你的需求，那么，我们甚至可以自己创建一个 bucket。但这已超出了本文的讨论范围，这里就不进一步说明了。

## 实践

学习了常用命令，以及添加资源库后，就可以开始实践了，真正使用才能熟练并且深入理解。

# 示例

## 切换 JDK 版本

像 JDK 这样的应用，通常会安装多个版本，使用 `scoop reset` 命令可以轻易地在多个版本间切换。

```powershell
scoop install openjdk12
scoop install openjdk11
java -version
# openjdk version "11.0.1" 2018-10-16
scoop reset openjdk12
java -version
# openjdk version "12-ea" 2019-03-19
```

> 其原理是将 `bin` 目录重新添加到 `path`  环境变量的最前面。

## 多线程下载

默认情况下，Scoop 在下载应用时是单线程的，直接的感受是——慢，借助 aria2 可以使 Scoop 具有多线程下载的能力。当然，你需要先安装 aria2，然后执行以下命令：

```powershell
scoop install aria2
scoop config aria2-enabled true
scoop config aria2-retry-wait 3
scoop config aria2-split 5
scoop config aria2-max-connection-per-server 5
scoop config aria2-min-split-size 5M
```

## 代理配置

### 命令行代理

```shell
set http_proxy=http://host:port
set https_proxy=http://host:port
set http_proxy_user=
set http_proxy_pass=
```

### 通过代理安装 scoop

```powershell
# If you want to use a proxy that isn't already configured in Internet Options
[net.webrequest]::defaultwebproxy = new-object net.webproxy "http://proxy.example.org:8080"
# If you want to use the Windows credentials of the logged-in user to authenticate with your proxy
[net.webrequest]::defaultwebproxy.credentials = [net.credentialcache]::defaultcredentials
# If you want to use other credentials (replace 'username' and 'password')
[net.webrequest]::defaultwebproxy.credentials = new-object net.networkcredential 'username', 'password'
```

### 为 scoop 配置代理

```powershell
scoop config proxy [username:password@]host:port
scoop config rm proxy

scoop config proxy currentuser@default
scoop config proxy user:password@default
scoop config proxy none # 绕过代理直连
```

### 为 git 配置代理

scoop 很多更新操作是基于 GitHub 的，因此，通常也会为 git 配置代理。

```powershell
git config --global http.proxy http://host:port
git config --global https.proxy https://host:port
```

# 参考

## 命令列表

| 命令       | 说明                                                   |
| ---------- | ------------------------------------------------------ |
| alias      | Manage scoop aliases                                   |
| bucket     | Manage Scoop buckets                                   |
| cache      | Show or clear the download cache                       |
| checkup    | Check for potential problems                           |
| cleanup    | Cleanup apps by removing old versions                  |
| config     | Get or set configuration values                        |
| create     | Create a custom app manifest                           |
| depends    | List dependencies for an app                           |
| export     | Exports (an importable) list of installed apps         |
| help       | Show help for a command                                |
| home       | Opens the app homepage                                 |
| info       | Display information about an app                       |
| install    | Install apps                                           |
| list       | List installed apps                                    |
| prefix     | Returns the path to the specified app                  |
| reset      | Reset an app to resolve conflicts                      |
| search     | Search available apps                                  |
| status     | Show status and check for new app versions             |
| uninstall  | Uninstall an app                                       |
| update     | Update apps, or Scoop itself                           |
| virustotal | Look for app's hash on virustotal.com                  |
| which      | Locate a shim/executable (similar to 'which' on Linux) |

> 这是 `scoop help` 的输出。

## 资料

[官方 GitHub Wiki](https://github.com/lukesampson/scoop/wiki)

