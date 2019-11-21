---
layout: posts
title: 使用Git管理SVN项目
category: 工具
tags: Git 专题
excerpt: 介绍使用Git来管理SVN项目。
author: Eric Zong
---

* content
{:toc}
# 概述

尽管目前大多数项目都使用 Git 进行版本控制，但一些老的项目可能还在使用 SVN。那么，你真的需要安装 SVN 客户端吗？通常不需要！

Git 提供了一个桥接命令 `git svn` 来操作 SVN 库，因此，完全可以使用 Git 来接管 SVN 库。

> 应该注意的是 Git 比 SVN 要先进很多，因此，尽量不要使用一些高级的 Git 特性，以免与 SVN 协作出现问题。
>
> 另外，一个可能的过渡形式是，一套代码同时对应了一个 Git 库和一个 SVN 库（即并行同步），对于 Git 来说是可以同时管理的。

# 怎么使用？

事实上，只有有跟远程 SVN 库进行交互操作的时候，我们才会使用 `git svn` 桥接命令，而对本地库的操作仍然使用 Git 命令，这与一般的 Git 仓库没有不同。

## 远程交互

主要与远程 SVN 库的交互操作有：


```shell
# 从 SVN 克隆
git svn clone [-r<start-revision>[:<end-revision>]] <svn-url> [dir] [-s] [--prefix=svn/]
# 从 SVN 更新，等价于 svn update
git svn rebase
# 提交到 SVN
git svn dcommit
```

> * `git svn clone`
>
> ​    `-s`：告知 Git 该 SVN 库是标准布局。它等价于 `-T trunk -b branches -t tags`。这使得 Git 明确了分支、标签等的对应关系。
>
> ​    `--prefix=svn/`：统一为 SVN 远程分支添加前缀，以免与 Git 分支冲突。
>
> ​    `-r`：若 SVN 提交历史过多，克隆会很慢，因此，可只克隆部分版本。比如：`-r1000:HEAD` 只克隆版本号为 1000 到最新版本号之间的版本代码；而 `-rHEAD` 只会克隆最新版本号的代码。

## 冲突解决流程

```shell
git svn rebase
# 手动解决冲突
git add
git rebase --continue
git svn dcommit
```

## 分支管理

```shell
# 创建 SVN 远程分支，如：create_by_git
git svn branch <branch-name> 
# 删除 SVN 远程分支，如：svn/create_by_git
git branch -D -r <remote-branch-name>
# 创建 SVN 标签，如：v1.0
git svn tag <tag-name>
# 删除 SVN 远程标签，如：svn/tags/v1.0
git svn branch -D -r <remote-tag-name>
# 跟踪 SVN 远程分支，3 处远程分支名应任意但一致
git config --add svn-remote.<remote-branch-name>.url /path/to/brach/dir
git config --add svn-remote.<remote-branch-name>.fetch :refs/remotes/<remote-branch-name>
```

## 其他

```shell
# SVN 日志
git svn blame [filename]
# SVN 信息
git svn info
```

# 参考

[Git doc - git-svn](file:///D:/program/scoop/apps/git/2.24.0.windows.2/mingw64/share/doc/git-doc/git-svn.html)

[CSDN - 使用Git管理SVN](https://blog.csdn.net/qq_33285292/article/details/83929543)

[cnblogs - git-svn：通过git来管理svn代码](https://www.cnblogs.com/h2zZhou/p/6136948.html)

[CSDN - git-svn同时管理git与svn两种仓库](https://blog.csdn.net/wyongqing/article/details/79053803)