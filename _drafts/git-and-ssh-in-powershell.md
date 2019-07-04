---
layout: post
title: 在PowerShell中通过SSH使用Git
category: 工具
tags: Git 专题
excerpt: 介绍通过SSH访问Git的相关配置，并且使其可与PowerShell协作。
author: Eric Zong
---

* content
{:toc}
# HTTPS访问及其缺点

使用 Git 最简单的方式就是通过 HTTPS 进行访问，这样就不需要过多的配置即可使用。想来应该有相当一部分初学者都会使用这种较为简单的方式使用 Git，笔者亦是如此。通常来说，这是“够用”的，但长久下来会发现并不“好用”。

主要的问题在于，使用 HTTPS 方式推送时，凭证的存储。默认 Git 每次都会询问用户名和密码，不过，Git 拥有一个凭证系统可以存储凭证（见“参考”部分链接），使得我们不必每次都输入用户名和密码。这个系统有多种方式存储凭证，可通过以下命令查看本地配置：

```shell
git config --global --get-regexp credential.helper
```

笔者的 Windows 系统环境使用的是 `credential.helper manager`，这是一种较常用的永久存储方式。But，长期使用过程中会发现，仍然可能存在凭证失效需要重新输入的情况，通常这发生在某个本地仓库“较长时间”不与远程仓库同步的状况下。我的同步操作通常是计划任务自动执行的，当切换到另一个工作环境拉取代码的时候才发现另一边的代码因为凭证莫名失效而没有推送，想像一下那种沮丧的心情吧！

因此，笔者决定切换到 SSH。

这里假设已经有了一个正常工作在 HTTPS 模式下的 Git 环境，下面我们一步步将其切换到 SSH 模式下来工作。





# 参考

[《Pro Git》Git 工具 - 凭证存储](https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E5%87%AD%E8%AF%81%E5%AD%98%E5%82%A8)