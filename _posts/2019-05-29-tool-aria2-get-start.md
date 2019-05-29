---
layout: post
title: "aria2使用入门"
category: 工具
tags: 网络工具 下载
excerpt: "Aria2是一个命令行下轻量级、多协议、多来源的下载工具（支持 HTTP/HTTPS、FTP、BitTorrent、Metalink），内建 XML-RPC 和 JSON-RPC 用户界面。"
author: "Eric Zong"
---

* content
{:toc}

# 概述

Aria2是一个命令行下轻量级、多协议、多来源的下载工具（支持 HTTP/HTTPS、FTP、BitTorrent、Metalink），内建 XML-RPC 和 JSON-RPC 用户界面。

# 安装

个人喜欢用 scoop 管理这类软件，所以，直接使用 scoop 命令安装即可：

```powershell
scoop install aria2
```

当然也可以到 [官网](https://aria2.github.io/) 下载安装，这里不赘述了。

# 配置

要使用 aria2，首先应该进行一些必要的配置。

aria2 的配置文件名默认为 aria2.conf。配置文件可参考 [这里](/file/aria2.conf)。

需要修改或比较重要的配置项有以下这些：

* dir：下载目录。
* input-file/save-session：会话文件，保存和恢复下载需要依赖它。
* bt-tracker：默认情况下，BT 下载速度很慢，需要配置 BT Tracker 以加速下载，它是一个服务器列表，可以在 [这里](https://github.com/ngosang/trackerslist) 获得更新。

# 启动

在 Windows 下，通常我们使用命令参数指定其配置文件来启动 aria2：

```shell
aria2c --conf-path=aria2.conf
```

# 管理

通常，我们习惯在管理界面中进行下载操作，aria2 有不少可选的 WebUI 管理界面。

只是有的是在线的，通常简单可用；有的可能需要自行部署服务，对技术有一定的要求。这就要根据自己的需求和能力来进行选择啰。

## [YAAW](http://yaaw.qiniudn.com/)

[YAAW](http://yaaw.qiniudn.com/) 实质是一个在线的前端，最简单的使用方式是浏览器打开该页面，进行配置连接到本地 aria2 即可。

如果你使用 chrome 浏览器，还可以安装相应插件 [YAAW For Chrome：控制台界面](https://chrome.google.com/webstore/detail/hbjpfaalboebibgfmedmjijhbjapcnki)，方便使用。

WebUI 管理界面通常只需要设置 JSON-RPC Path 即可，格式如下：

```
http://localhost:6800/jsonrpc
```

如果配置了 token，则路径应该如下：

```
http://token:ericzong@localhost:6800/jsonrpc
```

> 其中，ericzong 是指配置文件中配置项 `rpc-secret`，应根据配置自行替换。另外，端口也是可配置的，注意替换。
>
> 新版本的 aria2 推荐配置 token，以替代配置用户名和密码。
>
> 有的 WebUI 仅支持 token 连接，因此，最好配置好 token。

## [webui-aria2](https://github.com/ziahamza/webui-aria2)

[webui-aria2](https://github.com/ziahamza/webui-aria2) 是官方推荐的前端，不过需要自行部署服务。

从 GitHub 上把项目下载到本地，在项目根目录执行以下命令运行即可：

```shell
node node-server.js
```

显然，需要事先安装 Node.js。

配置跟 YAAW 类似。

# 应用

从上述说明可知，aria2 使用不胜繁琐，既需要编写配置文件，还需要搭建管理前端，一般用户不太容易上手。

如果不是要基于 aria2 进行开发，不建议直接使用 aria2 进行下载任务。目前已经有不少基于 aria2 的下载工具了，比如 [Motrix](https://motrix.app/zh-CN/)，一般就能满足普通用户需要了。

# 参考资源

[官网](https://aria2.github.io/)

[GitHub](https://github.com/aria2/aria2)

