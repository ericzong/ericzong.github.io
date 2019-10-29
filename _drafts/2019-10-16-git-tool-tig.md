---
layout: posts
title: tig入门
category: 工具
tags: Git 入门
excerpt: 介绍Git的一个辅助工具tig的使用。
author: Eric Zong
---

* content
{:toc}

# tig是什么?

简单来说，tig 是  Git 的文本模式界面（text-mode interface for Git）。

官方文档对它的说明如下：

>  Tig is an ncurses-based text-mode interface for git. It functions mainly as a Git repository browser, but can also assist in staging changes for commit at chunk level and act as a pager for output from various Git commands. 
>
>  Tig 是 git 的基于 ncurses[^1] 的文本界面。它主要用作 Git 仓库浏览器，但也有助于在块级别暂存提交更改，并作为各种 Git 命令的输出分页器。 

# 为什么需要tig?

Git 是一个优秀的版本控制系统，它的操作命令都很方便强大。但是，长期使用 Git 的小伙伴肯定会发现，使用 Git 命令在终端中浏览批量数据——比如浏览日志——就是一个灾难，内容通常相当混乱。

一种解决方案是使用相关的辅助 GUI 软件，但对于程序员而言命令行始终是最高效的环境，这至少不是一个追求高效的解决方案。

而 tig 为终端提供了一个基于文本的用户界面，可以方便地浏览 Git 日志，并且可以在提交记录间进行跳转，而它可以做的却不限于此。

# 如何安装？

如果你使用的是 Windows 系统，那么在安装 Git 时应该已经内置安装了 tig。通常在 `<Git>/usr/bin` 目录下可以找到。

其他系统安装命令如下：

* MacOS: `brew install tig`
* Ubuntu/Debian: `sudo apt install tig`
* Fedora/RHEL: `sudo dnf install tig`

# 从主视图说起

> tig 布局通常是这样，包含一个状态窗口（最后那行）和一个或多个视图。默认通常只显示一个视图，当然也可以被分割。

在终端执行 `tig` 命令即可进入 tig 的主视图，主视图中显示的是当前工作目录所在的 Git 仓库的提交记录。默认高亮选中第一行，即最后的记录。

> 注意，这不一个静态提交记录列表，你可以通过快捷键在列表中进行导航。其操作方式类似于 vi，如果你熟悉 vi，那么应该可以快速上手。

`j`/`k` 键：可以让光标上下移动选中相应的提交记录行。

`Enter` 键：选中某条提交记录行的状态下按 `Enter` 键，终端窗口会被垂直分屏，左屏仍显示提交记录列表，右屏显示被选中的提交记录的详细信息。此时，焦点会自动切换到右屏中，即`j`/`k` 键将在右屏进行导航。

`Shift + j/k`：如果此时需要在左屏各条提交记录间进行导航，可使用 `Shift + j/k`，右屏内容会随着选中的提交记录而即时改变。

> 也可以使用上下方向键进行左屏导航。

`Tab` 键：当然，也可以按 `Tab` 键将焦点切换回左侧窗口，使用 `j`/`k` 键进行导航。

`Q`/`Ctrl + C`：退出 tig。

很显然，快捷键很多，而这只是一小部分。因此，通常你不可能记住所有快捷键，好在在 tig 的任一视图你都可以按下 `h` 键来查看帮助视图，其中完整列出了所有快捷键。

# 参考

[官方主页 - GitHub Pages](https://jonas.github.io/tig/)

[官方手册 - GitHub Pages](https://jonas.github.io/tig/doc/manual.html)

[GitHub](https://github.com/jonas/tig/) 

[^1]: 译注：ncurses，字符终端处理库。