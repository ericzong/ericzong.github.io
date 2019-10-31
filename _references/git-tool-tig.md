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

| 键   | 说明                                                         |
| ---- | ------------------------------------------------------------ |
| u    | Update status of file. In the status view, this allows you to add an untracked file or stage changes to a file for next commit (similar to running git-add <filename>). In the stage view, when pressing this on a diff chunk line stages only that chunk for next commit, when not on a diff chunk line all changes in the displayed diff are staged. |
| M    | Resolve unmerged file by launching git-mergetool(1). Note, to work correctly this might require some initial configuration of your preferred merge tool. See the manpage of git-mergetool(1). |
| !    | Checkout file with unstaged changes. This will reset the file to contain the content it had at last commit. |
| 1    | Stage single diff line.                                      |
| @    | Move to next chunk in the stage view.                        |
| ]    | Increase the diff context.                                   |
| [    | Decrease the diff context.                                   |

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

| Key                  | Action                                                       |
| -------------------- | ------------------------------------------------------------ |
| :<number>            | Jump to the specific line number, e.g. `:80`.                |
| :<sha>               | Jump to a specific commit, e.g. `:2f12bcc`.                  |
| :<x>                 | Execute the corresponding key binding, e.g. `:q`.            |
| :!<command>          | Execute a system command in a pager, e.g. `:!git log -p`.    |
| :<action>            | Execute a Tig command, e.g. `:edit`.                         |
| :goto <rev>          | Jump to a specific revision, e.g. `:goto %(commit)^2` to goto the current commit’s 2nd parent or `:goto some/branch` to goto the commit denoting the branch `some/branch`. |
| :save-display <file> | Save current display to ``.                                  |
| :save-options <file> | Save current options to ``.                                  |
| :save-view <file>    | Save view info to `` (for testing purposes).                 |
| :script <file>       | Execute commands from ``.                                    |
| :exec <flags><args…> | Execute command using `` with external user-defined command option flags defined in ``. |
| :echo <args…>        | Display text in the status bar.                              |

## 扩展命令

| Keymap  | Key  | Action                    |
| ------- | ---- | ------------------------- |
| main    | C    | git cherry-pick %(commit) |
| status  | C    | git commit                |
| generic | G    | git gc                    |

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

