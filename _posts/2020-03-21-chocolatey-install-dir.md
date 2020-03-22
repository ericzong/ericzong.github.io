---
layout: post
title: Chocolatey安装目录选项
category: 工具
tags: Chocolatey 专题
excerpt: 介绍Chocolatey安装应用时指定安装目录的方法。
author: Eric Zong
---

* content
{:toc}

# 概述

简单地使用 `choco install app` 命令显然结果也是简单地默认安装，但如果你对安装应用有一定的配置要求——比如，指定安装目录——那么默认安装通常是远远不够的。

这就需要为安装命令传递相关配置选项/开关。

# 安装选项

```powershell
--ia, --installargs, --installarguments, --install-arguments=VALUE
```

该选项用以传递安装参数给本地安装器（native installer）。

# 指定默认安装位置

对于 MSI 安装程序而言，通常如下传递参数：

```powershell
-ia "INSTALLDIR=""D:\Program Files"""
```

> 注意其中双引号的转义。

对于 [NSIS](http://nsis.sourceforge.net/Main_Page)（NullSoft Scriptable Install System）安装程序，它使用 `/D` 开关指定安装路径，那么参数传递方式如下：

```powershell
-ia "'/D=/path/to/install/dir'"
```

> 当然，如果你是 Chocolatey 的**许可证用户**，那么你还可以使用 `--dir` 等统一的选项，Chocolatey 会自行适配设置安装路径。

# 参考

[Chocolatey 官网 - Install 命令](https://chocolatey.org/docs/commands-install)

[Chocolatey 官网 - 安装目录选项（许可证版）](https://chocolatey.org/docs/features-install-directory-override)

