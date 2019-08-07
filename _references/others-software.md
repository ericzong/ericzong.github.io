---
layout: page
title: 常用软件集锦
category: 其他
date: 2019-01-23
author: "Eric Zong"
---

* content
{:toc}

# 编辑器

## Markdown

### [Typora](https://www.typora.io/)

Typora 是一个 Markdown 编辑器。

它的最大亮点是编辑查看一体化设计。不同于大多数其他 Markdown 编辑器，Typora 没有预览窗口，文本在编辑后是即时渲染的，这使得 Typora 拥有“所见即所得”的效果。因此，Typora 同时也是一个优秀的查看器。

另外，Typora 支持自定义主题。你可以选择使用多种内置主题，或者下载安装喜欢的官方主题，甚至如果熟悉 CSS 的话还可以自行编写样式文件。

**参考**

* [官网 - Add Custom CSS](https://support.typora.io/Add-Custom-CSS/)
* [官网 - About Themes](https://support.typora.io/About-Themes/)
* [官网 - Write Custom Theme for Typora](https://theme.typora.io/doc/Write-Custom-Theme/)

# 知识管理

## 思维导图

### Desktop Naotu

Desktop Naotu 是基于百度脑图的一款本地应用，不需要网络连接也可以正常使用。

Desktop Naotu 实现了百度脑图的基本功能，同时支持自动保存。Desktop Naotu 支持 macOS、Windows 及 Linux，你可以在 [GitHub](https://github.com/NaoTu/DesktopNaotu) 下载它。

## 资料管理

### POLAR

POLAR 是一个跨平台的文档管理器，可以管理网页内容、书籍、笔记等等，并支持打标签、注释、高亮和保存阅读进度等功能。它是开源的，从 [GitHub](https://github.com/burtonator/polar-bookshelf) 可以得到其源代码。

POLAR 基于 Electron 等技术，本质上是一个 Web 应用，因此，可以安装桌面版或是在浏览器中打开其 [网页](https://app.getpolarized.io/)。

POLAR 自带云服务，可同步上传文档。但要注意，云服务不是完全免费的。免费用户会有同步设备数量、存储空间、网页文档数量等限制。

与云服务相关的“中国特色”问题是，其登录、同步等服务都有“墙”。当然，桌面版可以本地工作，不使用云服务。

# 网络

## 下载

### Motrix

一款全能的下载工具，支持下载 HTTP、FTP、BT、磁力链、百度网盘等资源。

甚至可以解析 `thunder://` 开头的迅雷下载链接。配合 Chrome 插件，Motrix 也可以支持百度网盘文件的下载。

Motrix 支持 macOS、Windows 及 Linux 系统，你可以在 [GitHub](https://github.com/agalwood/Motrix) 或 [官网](https://motrix.app/) 下载 Motrix。

# 编程

## 管理

### Tig

[Tig](https://jonas.github.io/tig/) 是一个 Git 增强工具，它可以在命令行模式下更方便地浏览版本库，甚至还可以浏览暂存内容。

Git 目前已经内置了 Tig，因此，不必再额外安装。

## 数据库

### DB Browser for SQLite

DB Browser for SQLite（[DB4S](http://sqlitebrowser.org/)）是一个高质量可视化的开源工具，用于创建、设计和编辑与 SQLite 兼容的数据库文件。

它是跨平台桌面数据库，支持的数据库文件后缀有：.db、.db3、.sqlite、.sqlite3。

Windows 版甚至包含 PortableApp 安装文件。

### KEXI

[KEXI](http://www.kexi-project.org/) 是一个开源的可视化数据库应用程序，同 MS Access 和 Filemaker 一样，属于桌面数据库。

号称是 Linux 版的 Access。

跨平台，但目前尚不完全，Windows 和 Mac OS X 版本仍在开发中。

是 [Calligra Suite](https://www.calligra.org/) 的一部分。

> Calligra Suite 是跟 MS Office 类似的办公套件，不同的是前者是开源软件。

### nuBuilder Forte

[nuBuilder Forte](https://www.nubuilder.com/) 是一个基于 Web 的开源数据库。

它是 PHP 开发的，因此……

## 转换

### [PS2EXE](https://gallery.technet.microsoft.com/PS2EXE-Convert-PowerShell-9e4e07f1)

通过一个 PowerShell 脚本将 PowerShell 脚本转换为 exe 文件。

记录版本：v0.5

目前支持到 PowerShell 4，因此，如果安装的 PowerShell 更新，则不能转换成功。

### [Ps1 To Exe](http://www.f2ko.de/en/p2e.php)

将 PowerShell 脚本转换为 exe 文件。

可视化操作。

还具有如下特性：

* 转换为控制台或 GUI 程序
* 管理员权限执行
* 包含文件、文件夹、icon 及版本信息
* 转换为 32 位或 64 位 exe 文件
* 命令行接口
* 便携式程序
* 可加密
* 多语言支持，支持中文

官网还提供相应的在线转换工具：<http://www.f2ko.de/en/op2e.php>

# 系统工具

## 配置查看

### [Windows Hotkey Explorer](http://hkcmdr.anymania.com)

它是一个系统热键查看工具。

当你常用的某个系统热键突然失效，或者想要设置的热键有冲突时，通常你并不知道具体是哪个软件占用了该热键。这时，Windows Hotkey Explorer 可以简单地列出所有系统热键以及注册它们的进程。

> 该官网还有另一个名为 Hotkey Commander 的工具，它是 Windows Hotkey Explorer 的增强版，不仅可以查看热键，还可以管理和覆写热键。
>
> 不过它是收费软件，需要注册，不过目前看来已经不能注册了。

# 浏览器插件

## 内容过滤

### AdGuard 广告拦截器

AdGuard 广告拦截器可有效的拦截所有网页上的所有类型的广告，甚至是在 Facebook、Youtube 以及其他万千网站上的广告！

### uBlock Origin

一款高效的请求过滤工具：占用极低的内存和CPU，和其他常见的过滤工具相比，它能够加载并执行上千条过滤规则。

## 能力增强

### Tampermonkey

Tampermonkey 是一款 JavaScript 脚本管理器。从内容屏蔽到样式美化、从功能增强到精简提速，Tampermonkey 脚本几乎无所不能。如果你厌倦了原始网页的诸多不便，不妨试试能否从 [Greasy Fork](https://greasyfork.org/zh-CN) 中找到其他用户编写好的优化脚本，或者干脆自己创建一个，并分享给大家。

### IE Tab

在标签页中以IE内核显示网页。快捷、强健、可靠。这个版本是最流行的一个原因。

## 效率

### Toby

查询资料时，我们可能会同时打开许多标签页，如果这时突然来了其它紧要工作，只有关闭所有页面下次重新搜索么？有了 Toby，你就可以一键保存当前的标签页组，随时恢复此前的进度。Toby 支持暂存多个标签页组，并通过标签、笔记和备注整理归纳，还能跨设备同步收藏记录。

<mark>仅谷歌</mark>

### [书签侧边栏](https://chrome.google.com/webstore/detail/bookmark-sidebar/jdbnofccmhefkmjbkkdkfiicjkgofkdh/related)

书签侧边栏则可以将 Chrome 的书签栏移动至浏览器两侧，并通过滑动或点击等方式唤出，还可以自定义其外观、位置、行为等，自由度较高。

<mark>仅谷歌</mark>

## 学习

### 达达划词翻译

UI 设计简约美观，释义来自牛津词典，支持发音和词根词缀。此外，达达划词翻译还内置了生词本功能并可同步至扇贝和有道，基于艾宾浩斯记忆曲线进行温习提醒，帮助你科学掌握外文词汇。

<mark>仅谷歌</mark>

## 娱乐

### 豆瓣电影传送门

为豆瓣电影右侧添加资源链接。

可以在 [chrome 网上应用商店](https://chrome.google.com/webstore/detail/%E8%B1%86%E7%93%A3%E7%94%B5%E5%BD%B1%E4%BC%A0%E9%80%81%E9%97%A8/pkidecliagangmpphpelecaoogfbnihi) 安装，或者在 [GitHub](https://github.com/Neulana/douban-movie-extension) 找到它。