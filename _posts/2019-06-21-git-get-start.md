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

# 基本使用

## 创建库

通常有两种方式，但无论如何，总需要事先创建一个远程仓库。

方法一：

```shell
git clone <URL>
```

> 说明：
>
> 该方法适用于全新开始建库，结果是将克隆一个“空”的库；或者克隆一个已经存在的库继续工作。
>
> `clone` 一个仓库时，会自动将其添加为远程仓库，默认简写“origin”。
>
> `clone` 会自动设置默认分支（通常是 master 分支）跟踪。

方法二：

```shell
# 在工程根目录执行
git init
git add .
git commit -m "comment"
git remote add origin <URL>
# 场景1：远程仓库不为空
git pull --rebase origin master
# 场景2：远程仓库为空
git push -u origin master
```

> 说明：该方法适用于中途建库。基本思路是，首先创建本地仓库，然后指定远程仓库，最后将本地内容推送到远程仓库。其中可能出现两种场景，一种是远程仓库为空，另一种是不为空。前者很简单，直接推送即可；后者由于远程仓库已有内容，所以需要先拉取并变基，然后才能推送。

## 暂存区操作

### 添加、移除

```shell
git add <.|FILES>
git reset HEAD [FILES]
```

> 说明：
>
> `git add .` 将把当前工作目录中所有改变加入暂存区。注意，它的影响是递归的。
>
> 对于 SVN 而言，新建的文件（未跟踪）才可以添加到索引；而对于 Git 而言，可添加到暂存区的文件不仅包括新建的未跟踪文件，还包括已修改的跟踪文件。
>
> `git reset HEAD` 不指定文件则清空暂存区。

### 删除

```shell
git rm [--force|-f] --cached <FILES>
```

> 说明：
>
> 简而言之，该命令的效果就是清除暂存文件，并使工作副本文件不再被跟踪。
>
> 换句话说，如果文件没有暂存，执行该命令是无效的。
>
> 文件可用 glob 模式指定，如：
>
> ```shell
> git rm log/\*.log
> git rm \*~
> ```
>
> 注意，其中的 `*` 使用了 `\` 进行转义，这是因为 Git 自己处理模式，而不需要 shell 帮忙展开。
>
> 根据文件状态讨论下该命令的效果。
>
> | 当前状态 | 结果状态 | 必须 -f ？ | 文件版本 |
> | -------- | -------- | ---------- | -------- |
> | A        | ??       | N          | 工作副本 |
> | AM       | ??       | Y          | 工作副本 |
> | M        | D        | N          | 工作副本 |
> | MM       | D        | Y          | 工作副本 |
>
> 总结：
>
> 1. 不影响工作副本文件；
> 2. 对于新增文件，将还原为未跟踪状态；而修改的文件，将标记为删除。
> 3. 当暂存后又进行了修改，则需要加 `-f` 强制删除。因为此时，暂存的文件跟原文件和工作副本都不相同，删除可能存在内容丢失。
> 4. 命令执行后，仍可以使用 `add` 命令将文件重新加入暂存区。需要注意的是，已跟踪的文件重新加入暂存区后，状态是 M（右），因为它仍和原文件不同，是修改过的。

### 移动/重命名

```shell
git mv <FILE_FROM> <FILE_TO>
```

> 说明：
>
> 移动或重命名不能操作未跟踪的文件，等价于如下操作：
>
> ```shell
>mv <FILE_FROM> <FILE_TO>
> git rm <FILE_FROM>
>git add <FILE_TO>
> ```

### 提交

```shell
git commit -m <COMMENT>
# 修改上一次提交
git commit --amend
```

> 说明：命令执行后 Git 会打开编辑器用以修改上次提交的注释。如果命令执行前暂存区为空，则仅仅修改了上一次的提交注释；但是如果暂存区有文件，则会合并入上一次提交的历史记录中。

## 工作区还原

```shell
git checkout <FILES>
```

> **注意：这是一个危险操作，工作副本将被覆盖。**

## 已提交未推送查询

```shell
# 次数
git status 
# 描述
git cherry -v
# 详细信息
git log master ^origin/master
```

## 远程仓库操作

### 查看

```shell
# 查看所有远程服务器简写、地址
git remote [-v | --verbose]
# 查看某远程仓库信息
git remote show [REMOTE_NAME]
```

### 添加、移除和重命名

```shell
# 添加远程仓库
git remote add <SHORT_NAME> <URL>
# 重命名
git remote rename <OLD_SHORT_NAME> <NEW_SHORT_NAME>
## 移除
git remote rm <MISS_NAME>
```

> 注意：远程仓库简写名只是本地仓库简化远程仓库地址的一种方式，它不存在于远程仓库中，对于多个本地仓库而言，重命名只会修改自身信息，不会影响其他本地仓库。

### 获取/抓取、拉取

```shell
git fetch [REMOTE_NAME]
git pull
```

> 说明：
>
> `git fetch` 将数据获取到本地仓库，并不会自动合并或修改工作副本。
>
> `git pull` 效果相当于在 `fetch` 的基础上，合并到当前分支。

### 推送

```shell
# 示例：git push origin master
git push [REMOTE_NAME] [BRANCH_NAME]
```

## 标签

> 分类：轻量标签（lightweight）、附注标签（annotated）。
>
> 轻量标签：特定提交的引用。像一个不会改变的分支。
>
> 附注标签：存储在Git数据库中的一个完整对象。可校验，有附加信息。
>
> 建议创建附注标签，因为即可拥有附加信息等，轻量标签通常作为临时标签。

### 查看

```shell
# 查看标签列表，示例：git tag -l 'v1.0*'
git tag [-l <PATTERN>]
# 查看标签信息及对应的提交信息
git show <TAG>
```

> 说明：`git show` 命令查看附注标签和轻量标签反馈信息区别在于，附注标签会显示额外的标签信息，而轻量标签没有。

### 添加、删除

```shell
# 附注标签
git tag -a <ANNOTATE> -m <MESSAGE>
# 轻量标签
git tag <TAG>
# 补打标签，示例：git tag v0.0.1 e6d105
git log --pretty=oneline
git tag -a <TAG> <CHECK_SUM>
# 删除本地标签
git tag -d <TAG>
```

> 说明：标签本质上来说是对某次提交的引用，因此，补打标签则需要指定某次提交即可。而提交需使用其校验和来指定，故提供校验和（或部分校验和）即可。
>

### 共享、移除

```shell
# 推送单个标签
git push origin [TAG]
# 推送多个标签
git push origin --tags
# 移除
git push origin --delete [tag] <TAG>
```

> 说明：
>默认，`git push` 不会推送标签到远程仓库。要推送必须显式指定。
> 对于 GitHub 等公共的 Git 服务而言，标签一般被用来标注发布点，因此，当一个标签被推送到其上时，会压缩生成一个Release以供下载。

### 检出

```shell
git checkout -b [BRANCH_NAME] [TAG]
```

> 说明：由于标签仅仅是对提交的引用，因此，并不能检出一个标签。如果想在某个标签状态下工作，只能将该标签检出为一个新分支。

## 查看状态

> 当需要进行 Git 操作时，通常都会先查看各文件的状态，以确定哪些文件会被处理，怎样处理。

```shell
# 默认是详细模式
# 当使用 --short 选项或其缩写 -s 时显示简览
git status [-s|--short]
```

> 说明：详细模式下会分状态列出各文件，并列出可进行的命令提示。简览模式下会每行列出一个文件，并在行首用标记标明其状态。

简览模式标记：

| 标记    | 描述               |
| :------ | ------------------ |
| ??      | 未跟踪             |
| A       | 新添加到暂存区     |
| M（左） | 被修改并放入暂存区 |
| M（右） | 被修改未放入暂存区 |

> 说明：
>
> 状态标记占2个字符的位置。从 M 标记有左右之分也可以看出来。
>
> 如果使用的是可着色的命令行，可以看出，已加入暂存区的标记是绿色的，如：A 和 M（左）；否则是红色的，如：?? 和 M（右）。
>
> MM 标记是可能的，即 M（左）和 M（右）标记不是互斥的。因此，对于 Git 而言，暂存操作不是仅仅标记文件，而是把暂存操作时的文件副本保存到了暂存区。可以想像这样的情况，某个文件被加入暂存区后，又被修改了，此时，对于这个文件而言，它的第 1 个版本被暂存了，但当前版本没有被暂存。所以其标记为 MM，即表示暂存后又被修改过。类似的 AM 也是可能的。
>
> 注意一点，有的命令选项是可以缩写的，但是完整选项是以“--”（双短线）为前缀的，而缩写是以“-”（单短线）为前缀的。

## 比较差异

```shell
git diff [--cached] [<PATH>...]
```

## 查看日志

```shell
git log [-p] [-n] [--stat] [--pretty=oneline|short|full|fuller] --graph
```

# 参考

[GitHub - gitignore](https://github.com/github/gitignore) 

# 附录

## 文件存储

直接记录快照，而非差异比较。

Git 对待数据更像是一个 快照流。每次提交更新，或在 Git 中保存项目状态时，它主要对当时的全部文件制作一个快照并保存这个快照的索引。为了高效，如果文件没有修改，Git 不再重新存储该文件，而是只保留一个链接指向之前存储的文件。

## 三种状态

* 已提交（committed）：数据已经安全的保存在本地数据库中。

* 已修改（modified）：修改了文件，但还没保存到数据库中。

* 已暂存（staged）：对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中。

## 三个工作区域

* Git仓库（Repository，.git directory）：Git 用来保存项目的元数据和对象数据库的地方。从其它计算机克隆仓库时，拷贝的就是这里的数据。

* 工作目录（Working Directory）：对项目的某个版本独立提取出来的内容。 

* 暂存区域（Staging Area）：一个文件，保存了下次将提交的文件列表信息，一般在 Git 仓库目录中。也称“索引”。

> Git 的管理方式与 SVN 相比有类似的方面也有不小的差异：
>
> * 远程仓库。对于远程仓库而言，概念上，所有的版本控制工具都是类似的，通常只是文件管理方式不同。
> * 本地仓库。对于 Git 而言，本地存储有整个远程仓库的镜像（即 .git 目录）。SVN 没有对应的概念。
> * 工作目录。简单来说就是工作副本，SVN 和 Git 大致类似。
> * 暂存区域。放入其中的内容（新增/修改）才会被提交。SVN 中的索引有所区别，只有新增的内容可以加入索引。

