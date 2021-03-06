---
layout: page
title: Git命令参考
category: 工具
date: 2019-07-09
author: Eric Zong
---

* content
{:toc}
# 别名

```powershell
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.ss 'status -s'
git config --global alias.aa 'add .'
git config --global alias.cm 'commit -m'
git config --global alias.ps push
git config --global alias.co checkout
git config --global alias.bc branch
git config --global alias.acm 'commit -a -m'
git config --global alias.cg 'config --global'
git config --global alias.bl 'branch -a'
git config --global alias.setuser "!git config user.name 'Eric Zong'; git config user.email 'ericzonglu@126.com'"
```

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

## 暂存

```shell
# 支持 glob，需用单引号引起来
git checkout -- <files>
# 撤销暂存，支持 glob 模式
# 注意：* 需要 \ 转义
git rm \*~
# 回退到指定版本
git reset --hard <COMMIT-ID>
# 回退到上个版本
git reset --hard HEAD^
```

# 远程操作

## 多分支/标签

```shell
# 推送所有分支
git push REMOTE '*:*'
git push REMOTE --all
git push --all origin
# 推送所有标签
git push REMOTE --tags
# 拉取所有分支
git pull --all
# 抓取所有分支
git fetch --all
```



