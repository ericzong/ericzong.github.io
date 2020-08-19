---
layout: page
title: Git常见问题汇总
category: 工具
author: "Eric Zong"
date: 2019-06-26
updated: 2020-08-19
---

* content
{:toc}

# 配置

## SSL certificate problem

触发场景：`push` 等操作时。

原因：Git 默认使用 SSL 连接，如果未做相关配置，仅使用 https 连接，则出现该错误。

```shell
git config --global http.sslVerify false
# or
set GIT_SSL_NO_VERIFY=true git push
```

## 命令回显中文为转义字符

触发场景一：执行 `status` 等命令。

```shell
git config --global core.quotepath false
```

触发场景二：执行 `git diff`、`git log` 等命令。

设置环境变量 `LANG=zh_CN.UTF-8` 即可。

```shell
# ----- cmd 设置（临时） -----
set LANG=zh_CN.UTF-8
# ----- powershell -----
# 临时设置
$env:LANG='zh_CN.UTF-8'
# 配置当前用户环境变量
[environment]::setEnvironmentVariable('LANG', 'zh_CN.UTF-8', 'User')
```

## 将空文件夹加入版本控制

Git 只跟踪文件，不跟踪文件夹，也就是说空文件夹将不会被加入版本控制中。

但有的情况下，我们的确需要将空文件夹加入版本控制，这怎么办呢？

简单来说，如果文件夹不为空就行了，即在空文件夹下创建一个任意的文件即可。但通常我们又不想把这个任意文件加入版本控制，那么，只要在空文件夹下创建一个名为“.gitkeep”的文件就可以达到效果了。

## `git pull`关于处理冲突的警告

```bash
git pull --all
warning: Pulling without specifying how to reconcile divergent branches is
discouraged. You can squelch this message by running one of the following
commands sometime before your next pull:

 git config pull.rebase false # merge (the default strategy)
 git config pull.rebase true  # rebase
 git config pull.ff only    # fast-forward only

You can replace "git config" with "git config --global" to set a default
preference for all repositories. You can also pass --rebase, --no-rebase,
or --ff-only on the command line to override the configured default per
invocation.
```

当执行 `git pull` 命令时，可能会看到以上警告。其大意是说，未指定 pull 操作遇到冲突时的解决策略，并给出了对应的配置命令。选择一种策略配置后，就不会出现该警告。

```bash
git config [--global] pull.rebase false	# 合并（默认）
git config [--global] pull.rebase true	# 变基
git config [--global] pull.ff only		# 仅快进
```

如果想要临时切换策略，可分别指定 `--no-rebase`、`--rebase`、`--ff-only`。

# 操作

## 创建分支失败

```shell
$ git branch v1
fatal: Not a valid object name: 'master'.
```

通常这意味着当前仓库是新创建的，`git init` 命令不会创建 master 分支，只有当提交时才会创建。

不过，直接提交一个空仓库是不被允许的，因此，应该添加一些文件再提交。

# 技巧

## 避免每次输入密码

如使用 HTTPS URL 推送，Git 服务器会询问用户名与密码。可设置“credential cache”，保存在内存中几分钟：

```shell
git config --global credential.helper cache
```

## 引用远程分支

在跟踪分支中，可通过 `@{upstream}` 或 `@{u}` 引用远程分支。比如：在 master 分支可使用 `git merge @{u}` 来取代 `git merge origin/master`。

