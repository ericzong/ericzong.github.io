---
layout: post
title: Git撤销操作讨论
category: 工具
tags: Git 专题
excerpt: 讨论Git各种撤销操作的使用方法与场景。
author: Eric Zong
---

* content
{:toc}
# 环境说明

| 软件       | 版本             |
| ---------- | ---------------- |
| Git        | 2.22.0.windows.1 |
| ComEnu     | 19.06.23         |
| PowerShell | 5.1.17134.765    |

# 撤销修改

## checkout

`checkout` 命令可以恢复工作树文件，因此，可用于撤销修改。

当且仅当，工作区的文件版本与暂存区/版本库不一致时才会执行撤销修改操作。

```shell
git checkout [--] <files>
```

> 使用该命令前，你应该清楚的是，这是一个危险操作。因为，该命令可能会覆盖工作区，导致工作副本丢失。

### 恢复

当 `checkout` 用于恢复文件时，简单来说是将文件恢复到“上个版本”。而“上个版本”可能存在于两个地方：暂存区和版本库。如果暂存区中有内容将从暂存区中恢复，否则从版本库中恢复，显然仅当暂存区或版本库中存在且版本与工作区不一致时才会进行恢复操作。

我们可以把各种情况列成如下表（数字代表虚拟的版本号，越大越新，`-` 代表不存在或无操作）：

| 场景  | 版本库 | 暂存区 | 工作区 | 结果 | 说明                 |
| ----- | ------ | ------ | ------ | ---- | -------------------- |
| 1     | -      | -      | 1      | -    | 新文件               |
| 2     | -      | 1      | 1      | -    | 新文件，已暂存       |
| ==3== | -      | 1      | 2      | 1    | 新文件，已暂存再修改 |
| 4     | 1      | -      | 1      | -    | 未修改               |
| ==5== | 1      | -      | 2      | 1    | 已修改               |
| ==6== | 1      | 2      | 3      | 2    | 已暂存再修改         |

* 场景 1、2、4 均为无效操作，原因分别是不存在上个版本、工作区与暂存区版本一致、工作区与版本库版本一致。
* 场景 5 由于没有暂存，所以是从版本库恢复的。
* 场景 3、6 由于有暂存，所以无论版本库情况如何都应从暂存区恢复。

### glob支持

文件名可由 glob 模式给出，如：

```shell
git checkout '*.txt'
git checkout '???.txt'
```

这里有一个细节可供讨论：当前所在路径层级对 glob 模式匹配的影响。

假设，我们现在有如下结构的“干净”仓库，并让所有文本文件处于修改状态：

```
└─ 1.txt
└─11.txt
└─sub
   └─ 1.txt
```

然后我们来看看，当处于根目录与处于 `sub` 目录对 glob 模式匹配有何影响。

| 当前目录 | glob      | 1.txt | 11.txt | sub/1.txt |
| -------- | --------- | ----- | ------ | --------- |
| root     | *.txt     | √     | √      | √         |
|          | ?.txt     | √     | -      | ×         |
|          | sub*.txt  | -     | -      | √         |
|          | ?????.txt | -     | -      | √         |
| sub      | *.txt     | ×     | ×      | √         |
|          | ?.txt     | ×     | -      | √         |
|          | ../*.txt  | √     | √      | √         |
|          | ../?.txt  | √     | -      | ×         |

从上表我们可以总结出一些结论：

* 匹配是基于相对路径的。对于根目录而言，3 个文件的路径分别为 `1.txt`、`11.txt`、`sub/1.txt`；而对于 `sub` 目录而言，3 个文件的路径分别为 `../1.txt`、`../11.txt`、`11.txt`。
* 无论是 `*` 还是 `?` 均可以匹配路径分隔符，但是不能匹配父目录符号 `..`。

> 注意：==【不一致】==根据官方文档说明，glob 模式字符串应以单引号包裹，但实际操作中发现没有单引号效果是相同的。

## rm

`rm` 实际作用是从暂存区或/和工作树删除文件。严格来说，如果说用它来撤销修改的话，实际是指丢弃一个新添加已暂存但未提交的文件。

```shell
git rm [-f | --cached]
```

> 使用该命令前，你应该清楚的是，这是一个危险操作。因为，该命令可能会丢弃工作区文件，导致工作副本丢失。

### 丢弃

让我们来分析下各种情况下的执行结果：

| 场景  | 版本库 | 暂存区 | 工作区 | `-f` 必需？ | 结果 |
| ----- | ------ | ------ | ------ | ----------- | ---- |
| 1     | -      | -      | 1      | -           | -    |
| ==2== | -      | 1      | 1      | √           | 丢弃 |
| ==3== | -      | 1      | 2      | √           | 丢弃 |
| 4     | 1      | -      | 1      | ×           | D    |
| 5     | 1      | -      | 2      | √           | D    |
| 6     | 1      | 2      | 2      | √           | D    |
| 7     | 1      | 2      | 3      | √           | D    |

* 场景 1，`rm` 不能用于新增文件，即如果在版本库和暂存区中都不存在，则不能用其删除文件。
* 场景 2、3，已暂存未提交文件修改将被丢弃，`-f` 参数是必需的。
* 场景 4、5、6、7，执行操作后文件状态被标记为“已删除”。

> 注意以下几点：
>
> 1. 该命令不能删除仅存在于工作区的文件；
> 2. 除此之外的情况下，工作区和暂存区（如果有）中指定的文件都将被删除。如文件不在版本库中，则表示删除的文件被丢弃；否则，版本库对应文件标记为“已删除”。
> 3. 显然大多数场景下删除文件都意味着有内容丢失，因此需要指定 `-f` 选项强制删除；当且仅当，某次提交后版本库与工作区一致时才不会丢失内容，即不需要 `-f` 选项。

### glob支持

`rm` 也支持 glob 模式匹配文件名，可参见 `checkout` 的 [glob支持](#glob支持) 章节。

## reset

`reset` 能将当前状态退回到某一次提交，根据模式选项适当处理现在与退回后的文件差异。

```shell
git reset [--mixed | --soft | --hard] [HEAD | <commit>]
```

总之，`reset` 会将当前的提交状态退回到指定的版本号时的提交状态，差异将根据指定的模式进行处理：

`--mixed` 模式选项，默认值，暂存区会清除，差异保留在工作区；

`--soft` 模式选项，不会清除暂存区，保留工作区；

`--hard` 模式选项，强制退回指定版本状态，暂存区和工作区都将丢弃。

`HEAD` 是一个特殊值，它代表当前版本。可以在 `HEAD` 之后追加 `^` 来代表前一个版本，加 n 个 `^` 代表前 n 个版本，如 `HEAD^^` 代表上上个版本。但当回溯版本过多时，我们并不想键入一堆 `^`，可以简化为 `~n` 的形式，代表 n 个 `^`。比如，`HEAD~10` 代表回溯 10 个版本；其默认值是 1，因此 `HEAD~` 等价于 `HEAD^`。

# 附加说明

## 关于`checkout`的`--`

[官方文档](https://git-scm.com/docs/git-checkout#_argument_disambiguation) 指出 `--` 是用于消除歧义的。以上文仓库结构为例，存在这样一种场景：该仓库有一个名为 `1.txt` 的分支。因此，`git checkout 1.txt` 命令存在歧义，不能确定是切换到 `1.txt` 分支，还是恢复文件 `1.txt`。但鉴于切换分支更为普遍，所以该命令将执行切换分支，而命令 `git checkout -- 1.txt` 表示恢复文件。

> 注意：
>
> 根据官方文档说明，添加 `--` 后应执行恢复文件操作，但如果你在本文所述的环境中执行，实际上仍然是切换分支操作。（这种情况下，唯一可行的方式似乎是使用 glob 模式匹配要恢复的文件。）
>
> 事实上，在 `cmd` 或是 `bash` 中执行结果都符合官方文档说明，这个差异应该是由 `PowerShell` 参数解析引起的。
>
> 最佳实践：恢复文件时总是添加 `--` 选项。

## 关于`HEAD^`

在 cmd 中执行 `git reset HEAD^` 时，可能会得到一个“More?”的询问。这是由于在 cmd 中，行末的 `^` 是续行符，它代表本行命令未结束，将换行后继续。

最简单的解决方案是不要让 `^` 出现在行末，在其后加个空格即可。当然也可以这样：

```shell
git reset --hard "HEAD^"
git reset --hard HEAD^^
git reset --hard HEAD~
git reset --hard HEAD~1
```

> 这个问题源自 cmd 的续行符解析，而在 bash 或 powershell 不存在这个问题。

## 关于glob的扩展差异

[官方文档](https://git-scm.com/docs/git-rm#_examples) 指出，glob 模式中的通配符 `*` 需要前缀 `\` 进行转义。这是由于，需要通过转义避免外壳（shell）对 `*` 进行过早的扩展，而使用 Git 的扩展。比如：`doc/*.txt` 可能被外壳扩展为 `doc` 目录下所有的文本文件，而如果希望执行 Git 扩展匹配 `doc` 目录及其所有子目录下的所有文本文件，则应该使用转义形式 `doc/\*.txt`。

> 注意：不同的 shell 对 `*` 的扩展行为不同，`bash` 的扩展行为符合官方文档说明。但是 `cmd` 和 `PowerShell` 并不会进行通配符扩展，使用 `doc/\*.txt` 反而匹配不到文件。

# 参考

[`git-checkout` 官方文档](https://git-scm.com/docs/git-checkout)

[`git-rm` 官方文档](https://git-scm.com/docs/git-rm)

[`git-reset` 官方文档](https://git-scm.com/docs/git-reset)