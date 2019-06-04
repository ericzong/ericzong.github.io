---
layout: post
title: Git入门
category: 工具
tags: Git 入门
excerpt: 介绍Git日常使用。
author: "Eric Zong"
---

* content
{:toc}

# 配置

> 在使用 Git 管理工程前，应该事进行一些配置工作，这可能是全局的或工程相关的。

## 信息配置

用户名和邮件地址的配置很重要，因为提交历史中会记录操作者的这两项信息。因此，很有必要事先配置好。

```shell
git config --global user.name <USERNAME>
git config --global user.email <EMAIL>
```

> 说明：`--global` 选项指明该配置是全局的，但如果仅仅想当前工程使用该配置，则不需要指定该选项。

Git 的配置项都是以“键-值对”的形式指定的，如果值输入错误了，再执行一次正确的命令即可；如果是键输入错误了，最好删除它，可以用命令删除，也可以直接编辑配置文件：

```shell
git config --global --unset <KEY>
# 直接编辑配置文件
git config --global --edit
```

## 别名

Git 命令行原生不具有命令补齐功能——当然，可以安装 posh-git 一类的扩展为其增加该功能——一个方便命令快速输入的技巧是为常用命令设置别名。

```shell
# 配置别名
git config --global alias.<NAME> <COMMAND>
# 查看已配置的别名
git config --global --get-regexp alias
# 删除别名
git config --global --unset alias.<NAME>
```

别名不仅仅是命令的缩写，还可以包含命令选项，比如我常用的别名有以下这些：

```shell
* > git config --global --get-regexp alias
alias.unstage reset HEAD --
alias.last log -1 HEAD
alias.ss status -s
alias.aa add .
alias.cm commit -m
alias.ps push
alias.co checkout
alias.bc branch
alias.acm commit -a -m
alias.cg config --global
alias.bl branch -a
```

而且，不仅可以为 Git 子命令设置别名，也可以调用外部命令，其形式为 `!command`。

## 忽略文件

工程中总有一些文件是不需要版本控制的，我们希望 Git 能自动忽略掉它们。这需要编写配置文件 `.gitignore`，该文件通常放置于工程根目录下，当然也可以放置在其他子目录，并且可以有多个，但它只对其所在的目录及子目录有效。

`.gitignore` 文件遵循以下格式规范：

* 使用 `#` 进行注释
* 使用标准 glob 模式表示忽略文件
* 以 `/` 开头防止递归
* 以 `/` 结尾指定目录
* 用 `!` 否定模式

glob 模式，是 shell 所使用的简化了的正则表达式。

| 符号 | 说明                                            |
| ---- | ----------------------------------------------- |
| ?    | 匹配一个任意字符                                |
| *    | 匹配 0 或多个任意字符                           |
| []   | 匹配任何一个方括号中的字符，比如：`[abc] [0-9]` |
| **   | 匹配任意中间目录，a/**/z                        |
| !    | 取反                                            |

> GitHub 发布了各种常见工程的忽略配置文件，可以从 [这里](https://github.com/github/gitignore) 获取。

## 其他

```shell
# 编辑器
git config --global core.editor=vim
```

