---
layout: page
title: Git命令参考
category: 工具
date: 2019-07-09
author: Eric Zong
---

* content
{:toc}
# 信息查看

## 已提交未推送内容

```shell
# 仅可见未推送提交次数
git status
# On branch master
# Your branch is ahead of 'origin/master' by n commits.

# 仅可见未推送提交描述信息
git cherry -v
# commit id comment

# 可见未推送提交的详细信息
git log master ^origin/master
# commit id
# Author: ...
# Date: ...
# comment
```

## 修改

```shell
# 工作区 vs. 暂存区
git diff HEAD -- <file>
# 暂存区 vs. 本地库
git diff HEAD --cache <file>
```

## 标签

```shell
# 查看标签列表，示例：git tag -l 'v1.0*'
git tag [-l <PATTERN>]
# 查看标签信息及对应的提交信息
git show <TAG>
```

## 分支

```shell
# 查看所有分支
git branch
# 查看所有分支最后一次提交
git branch -v
# 查看已/未合并到当前分支的分支
git branch <--merged | --no-merged>
```

# 撤销

```shell
# 撤销暂存
git checkout --
git checkout <FILES>
# 撤销暂存，不指定文件则清空暂存区
git reset HEAD <FILES>
# 撤销暂存，支持 glob 模式
# 注意：* 需要 \ 转义
git rm \*~
# 回退到指定版本
git reset --hard <COMMIT-ID>
# 回退到上个版本
git reset --hard HEAD^
```

