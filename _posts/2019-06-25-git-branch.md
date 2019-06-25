---
layout: post
title: Git入门：分支的使用
category: 工具
tags: Git 入门
excerpt: 介绍Git分支的常规使用。
author: "Eric Zong"
---

* content
{:toc}

# 概述

Git 分支本质上是指向提交对象的可变指针。由于分支操作是指针操作，并不需要创建文件副本，因此，相当迅速。

# 本地分支操作

## 查看

```shell
# 查看当前HEAD所指的分支历史
git log --oneline --decorate
# 查看所有分支
git branch
# 查看所有分支最后一次提交
git branch -v
# 查看已/未合并到当前分支的分支
git branch <--merged | --no-merged>
```

> 说明：
>
> `git branch` 列出的分支中前面带“*”的代表当前检出的分支。
>
> 已合并分支不仅包括 `git merge` 合并的分支，还包括当前分支的上游分支。

## 创建

```shell
# 创建一个新分支
git branch [BRANCH_NAME]
# 创建新分支并切换到新分支
git checkout -b [BRANCH_NAME]
```

> 说明：
>
> `git branch` 会在当前所在的提交对象上创建一个指针。注意仅创建了分支，但不会自动切换。
>
> Git 使用 HEAD 特殊指针引用当前所在的本地分支。
>
> `git checkout -b mybranch` 等价于如下两条命令：
> `git branch mybranch`
> `git checkout mybranch`

## 切换

```shell
git checkout [OTHER_BRANCH_NAME]
```

> 说明：
>
> 分支切换会改变工作副本。
>
> 当然，也可以用上文提到的命令创建并切换分支。
>
> 注意，切换分支时，当前分支应该是干净的状态，即工作副本和暂存区都应没有修改的文件。否则，Git 将阻止切换——因为这可能导致修改丢失。

## 合并

```shell
git merge [--no-ff] [-m <"comment">] [ANOTHER_BRANCH_NAME]
```

> 说明：
>
> 合并是将另一个分支合并到当前分支。
>
> 如果当前分支是被合并分支的上游，则合并是“快进（fast-forward）”的，仅向前移动指针即可。
> 如果两个分支来源于不同的分叉（diverged），那么Git使用两个分支的末端快照和它们的共同祖先（合并基础，由Git选择最优的）做一个简单的三方合并。将产生新快照和一个指向它的新提交（合并提交，有2个父提交）。
> 合并冲突，Git 仅做合并，但没创建合并提交。手动处理冲突流程：
>
> 1. 使用 `git status` 命令查看并处理未合并（unmerged）文件；
> 2. 编辑冲突文件；
> 3. 使用 `git add` 标记冲突已解决；
> 4. `git status` 确认冲突已解决；
> 5. `git commit` 合并提交。
>
> 注意：不带 `--no-ff` 参数，当可快进合并（Fast forward）时，会进行快进合并；带 `--no-ff` 参数，不进行快进合并。前者看不出曾经做过合并，后者可看到历史有分支。

## 变基

变基，将一个分支的修改在另一分支上应用。整合分支的另一种方法。

```shell
git rebase <BASE_BRANCH> <TOPIC_BRANCH>
```

> 说明：
>
> 原理是，查找两个分支的最近共同祖先，将当前分支与祖先对比，提取修改并存为临时文件，然后在目标基底上应用这些修改。
>
> 变基的目的是推送时提交历史整洁，作为代价，显然会丢弃掉一些历史。
>
> 应该在未推送的提交上执行变基，否则……

## 删除

```shell
git branch -d [BRANCH_NAME]
```

> 说明：
>
> 删除未合并的分支意味着有工作会丢失，因此会失败，除非使用 `-D` 强制删除。

# 远程分支操作

## 查看

```shell
git ls-remote [SHORT_NAME]
git remote show [SHORT_NAME]
```

## 推送

本地分支不会自动与远程仓库同步，必须显式推送。

```shell
git push <SHORT_NAME> <BRANCH_NAME>
git push <SHORT_NAME> <LOCAL_BRANCH>:<REMOTE_BRANCH>
```

> 说明：
>
> 第二行的命令指定了远程分支名，它可以和本地分支不一样，这可以在远程仓库中创建与本地分支不同名的远程分支（如指定远程分支不存在），或推送到另一远程分支（远程分支存在）。

## 跟踪

跟踪分支，是与远程分支有直接关系的本地分支。

在跟踪分支上 `pull`，Git 能自动识别抓取源和合并目标。

自动创建跟踪分支：

1. 克隆仓库时，自动创建跟踪 origin/master 的 master 分支；
2. 从一个远程分支检出一个本地分支时，自动创建跟踪分支。

```shell
# 本地分支和远程分支名称可不相同
git checkout -b <LOCAL_BRANCH> <SHORT_NAME>/<REMOTE_BRANCH>
# 快捷方式，本地分支和远程分支名称相同
git checkout --track <SHORT_NAME>/<REMOTE_BRANCH>
# 设置已有本地分支跟踪远程分支
git branch <-u | --set-upstream-to> <SHORT_NAME>/<REMOTE_BRANCH>

# 查看所有跟踪分支
git branch -vv
```

## 删除

```shell
git push <SHORT_NAME> --delete <BRANCH>
```

# 附录

## 示例

### 重命名分支

```shell
# 1. 重命名本地分支（本地分支名称修改；远程分支名称不变；跟踪关系不变）
git branch -m <old_local_branch_name> <new_local_branch_name>
# 2. 删除远程分支（本地分支对应的远程分支改变为缺失状态）
git push origin --delete <old_remote_branch_name>
# 3. 推送本地分支（新名称的远程分支被创建；本地分支仍跟踪旧远程分支，缺失状态）
git push origin <new_local_branch_name>
# 4. 跟踪新远程分支
git branch -u origin/<new_remote_branch_name>
```

### 本地简单变基

假设我们有一个“空”的本地仓库，现在进行一些开发后，创建一个分支，然后在新分支上进行后续开发，然后将新分支通过变基合并回master分支，最后删除新分支。

主要步骤如下：

1. 在 master 分支下进行开发；
2. 创建 v1 分支，并转到 v1 分支；
3. 在 v1 分支下进行后续开发；
4. 变基，将 v1 的修改合并到 master；
5. 删除 v1 分支。

首先，查看下仓库目录，它应该是这样的：

```shell
$ ls -a
./  ../  .git/  .gitignore
```

创建 1.txt、2.txt、3.txt 等3个文件，模拟开发。

查看下当前仓库的分支情况：

```shell
$ git branch
* master
```

可见，当前仅有 master 分支。创建 v1 分支，并查看仓库分支：

```shell
$ git branch v1
$ git branch -v
* master a0cf45a init
  v1     a0cf45a init
```

注意，这里查看分支命令增加了 `-v` 参数，这会得到更多的分支信息，包括提交 ID 和注释。

从上面结果可以看到，master 和 v1 分支的 ID 都是“a0cf45a”，这其实说明当前 master 和 v1 指向的是同一次提交。

然后将修改（增加的文件）提交。

v1 分支创建成功了，现在我们可以切换到它上面继续工作了：

```shell
$ git checkout v1
Switched to branch 'v1'
```

当然，我们也可以快捷地使用带 `-b` 参数的 `checkout` 命令创建并切换到一个新分支：

```shell
$ git checkout -b v1
```

该命令会直接创建 v1 分支，并切换到其上。当然，必须指定一个不存在的分支，否则会报错提示已存在。

在 v1 分支中创建 4.txt、5.txt、6.txt 文件，提交，切换回 master 分支。

这时，就可以执行变基操作了：

```shell
$ git rebase v1
First, rewinding head to replay your work on top of it...
Applying: develop
```

当然，也可以不切换回master分支，直接执行如下命令：

```shell
$ git rebase v1 master
First, rewinding head to replay your work on top of it...
Fast-forwarded master to v1.
```

此时，变基后会自动切换回master分支。

再查看目录，会发现目录中已经有了4.txt、5.txt、6.txt 三个文件了。

最后，v1 分支的开发任务已完成，通常应该删除它：

```shell
$ git branch -d v1
Deleted branch v1 (was 3eb945c).
```

## 技巧

### 引用远程分支

在跟踪分支中，可通过 `@{upstream}` 或 `@{u}` 引用远程分支。比如：在 master 分支可使用 `git merge @{u}` 来取代 `git merge origin/master`。

### 从 GitHub 克隆多分支项目

当我们从 GitHub 克隆一个多分支项目时，会发现只能“看到” master 分支。比如：以[《You Don't Know JS》](https://github.com/getify/You-Dont-Know-JS/)为例。

当将该项目克隆到本地后，查看其分支情况：

```shell
$ git branch
* master
```

可见，本地只有 master 分支。但是，我们查看所有分支是可以看到其他远程分支的：

```shell
$ git branch -a
* master
  remotes/origin/1ed-zh-CN
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
```

由此可知，还有一个叫为“1ed-zh-CN”的中文版分支，我们直接检出这个分支，即会自动跟踪对应的远程分支：

```shell
$ git checkout 1ed-zh-CN
Switched to a new branch '1ed-zh-CN'
Branch '1ed-zh-CN' set up to track remote branch '1ed-zh-CN' from 'origin'.
```

