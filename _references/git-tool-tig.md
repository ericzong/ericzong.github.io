---
layout: page
title: tig参考
category: 工具
date: 2019-10-31
author: Eric Zong
---

* content
{:toc}
# 键绑定

## 视图切换

| 键       | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| `m`      | <span title="Switch to main view.">切换到主视图</span>       |
| `d`      | <span title="Switch to diff view.">切换到差异视图</span>     |
| `l`      | <span title="Switch to log view.">切换到日志视图</span>      |
| `t`      | <span title="Switch to (directory) tree view.">切换到（文件夹）树视图</span> |
| `f`      | <span title="Switch to (file) blob view.">切换到（文件）大对象视图</span> |
| `b`      | <span title="Switch to blame view.">切换到责任人视图</span>  |
| `r`      | <span title="Switch to refs view.">切换到引用视图</span>     |
| `s`, `S` | <span title="Switch to status view">切换到状态视图</span>    |
| `c`      | <span title="Switch to stage view">切换到暂存视图</span>     |
| `y`      | <span title="Switch to stash view.">切换到贮藏视图</span>    |
| `g`      | <span title="Switch to grep view.">切换到正则视图</span>     |
| `p`      | <span title="Switch to pager view.">切换到分页视图</span>    |
| `h`      | <span title="Switch to help view">切换到帮助视图</span>      |

## 视图操作

| 键                      | 说明                         |
| ----------------------- | ---------------------------- |
| `Enter`                 | 进入并打开选中行。上下文敏感 |
| `<`                     | 返回前一个视图状态           |
| `J`, `Down`, `Ctrl + n` | 下移                         |
| `K`, `Up`, `Ctrl + p`   | 上移                         |
| `,`                     | 移动父级                     |
| `Tab`                   | 切换到下个视图               |
| `R`, `F5`               | 刷新当前视图                 |
| `O`                     | 最大化当前视图               |
| `q`                     | 关闭当前视图                 |
| `Q`,  `Ctrl + c`        | 关闭所有视图并退出           |

## 特定视图操作

| 视图   | 种类     | 键                             | 说明                                            |
| ------ | -------- | ------------------------------ | ----------------------------------------------- |
| 搜索   | 视图操作 | `Ctrl + C`                     | 关闭当前视图                                    |
|        | 搜索中   | `Up`, `Ctrl + P`, `Ctrl + K`   | 查找下一个匹配                                  |
|        |          | `Down`, `Ctrl + n`, `Ctrl + j` | 查找上一个匹配                                  |
| 主视图 | 选项开关 | `G`                            | 提交标题图型                                    |
|        |          | `F`                            | 提交标题引用                                    |
|        | 扩展命令 | `C`                            | `?git cherry-pick %(commit)`                    |
| 差异   | 选项开关 | `[`                            | <span title="diff-context">差异上下文</span> -1 |
|        |          | `]`                            | <span title="diff-context">差异上下文</span> +1 |
|        | 内部命令 | `@`                            | `/^@@`                                          |
| 引用   | 扩展命令 | `C`                            | `?git checkout %(branch)`                       |
|        |          | `!`                            | `?git branch -D %(branch)`                      |
| 状态   |          | `u`                            | 更新，暂存/取消暂存                             |
|        |          | `!`                            | 还原，检出                                      |
|        |          | `M`                            | 合并                                            |
|        | 扩展命令 | `C`                            | `!git commit`                                   |
| 暂存   |          | `1`                            | 单行暂存/取消暂存                               |
|        |          | `\`                            | 分割当前差异区块                                |
| 贮藏   | 扩展命令 | `A`                            | `?git stash apply %(stash)`                     |
|        |          | `P`                            | `?git stash pop %(stash)`                       |
|        |          | `!`                            | `?git stash drop %(stash)`                      |

## 光标导航

| 键                  | 说明               |
| ------------------- | ------------------ |
| `k`                 | 光标上移一行       |
| `j`                 | 光标下移一行       |
| `PageUp`, `-`       | 光标上移一页       |
| `PageDown`, `Space` | 光标下移一页       |
| `Ctrl + u`          | 光标上移半页       |
| `Ctrl + d`          | 光标下移半页       |
| `Home`              | 光标跳转到第一行   |
| `End`               | 光标跳转到最后一行 |

## 滚动

| 键                    | 说明         |
| --------------------- | ------------ |
| `Insert`， `Ctrl + Y` | 向上滚动一行 |
| `Delete`, `Ctrl + E`  | 向下滚动一行 |
| `ScrollBack`          | 向上滚动一页 |
| `ScrollFwd`           | 向下滚动一页 |
| `|`                   | 滚动到第一列 |
| `Left`                | 向左滚动一列 |
| `Right`               | 向右滚动一列 |

## 搜索

| 键   | 说明                                             |
| ---- | ------------------------------------------------ |
| `/`  | 搜索视图。打开一个提示符以输入搜索正则。正向搜索 |
| `?`  | 反向搜索                                         |
| `n`  | 查找下一个匹配                                   |
| `N`  | 查找上一个匹配                                   |

## 杂项

| 键         | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| `e`        | 在编辑器中打开文件                                           |
| `:`        | 打开提示符。                                                 |
| `o`        | 打开选项菜单                                                 |
| `Ctrl + l` | 重绘屏幕                                                     |
| `z`        | 停止所有后台加载                                             |
| `v`        | 显示 tig 版本                                                |

## 选项切换

| 键       | 说明         |
| ------- | --------- |
| `#`        | 开关行号                                |
| `D`        | 开关日期显示 on/off/relative/relative-compact/custom |
| `A`        | 开关作者显示 on/off/abbreviated/email/email user name. |
| `~`        | 开关（行）绘图模式                           |
| `F`        | 开关引用显示 on/off （标签和分支名称） |
| `W`        | 开关差异忽略空白 on/off for          |
| `X`        | 开关提交 ID 显示 on/off                    |
| `$`        | 开关提交标题溢出的高亮显示 |
| `%`        | 开关文件筛选。以便查看与当前选定文件有关的完整差异，而不是仅查看差异。 |
| `G`        | 开关版本图可视化 on/off.          |

## <span title="Prompt">提示符</span>

| Key                    | Action                                                       |
| ---------------------- | ------------------------------------------------------------ |
| `:<number>`            | 跳转到指定行号，比如：`:80`                                  |
| `:<sha>`               | 跳转到指定提交，比如：`:2f12bcc`                             |
| `:<x>`                 | 执行相应的键绑定，比如： `:q`                                |
| `:!<command>`          | 在分页器中执行一个系统命令，比如：`:!git log -p`             |
| `:<action>`            | 执行一个 tig 命令，比如： `:edit`                            |
| `:goto <rev>`          | 跳转到一个指定的修改，比如： `:goto %(commit)^2` 跳转到当前提交的第 2 个父级，或者 `:goto some/branch` 跳转到这个提交指向的分支 `some/branch` |
| `:save-display <file>` | 保存当前显示到文件                                           |
| `:save-options <file>` | 保存当前选项到文件                                           |
| `:save-view <file>`    | 保存视图信息到文件（为了测试目的）                           |
| `:script <file>`       | 从文件执行命令                                               |
| `:exec <flags><args…>` | 使用具有外部用户定义命令选项标志的命令来执行命令             |
| `:echo <args…>`        | 在状态条中显示文本                                           |

## 扩展命令

| Keymap  | Key  | Action                      |
| ------- | ---- | --------------------------- |
| main    | C    | `git cherry-pick %(commit)` |
| status  | C    | `git commit`                |
| generic | G    | `git gc`                    |

# 版本限定

```powershell
# 路径限定
tig Makefile README
# 路径限定，特殊路径放在 -- 后
tig -- status
# 日期/数字限定
tig --since=1.month
tig --n400
tig --after="May 5th" --before="2006-05-16 15:44"
tig --after=May.5th
# 提交范围限定
tig tag-1.0..tag-2.0
tig origin..HEAD
# 可达性限定
tig tag-2.0 ^tag-1.0
# 组合限定
tig --since=1.month -n20 -- Documentation/
# 检查所有仓库引用
tig --all --since=1.week -- Makefile
```

