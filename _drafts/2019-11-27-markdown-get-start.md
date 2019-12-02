---
layout: post
title: Markdown入门
category: 工具
tags: Markdown 入门
excerpt: Markdown入门教程。
author: Eric Zong
---

* content
{:toc}
# 概述

Markdown 是一种纯文本轻量级标记语言。

Markdown 是一种创作格式，更多的关注语义。因而，即便是查看其纯文本形式，依然有很高的可读性。

Markdown 可以转换成多种格式的文件，常见的 Markdown 编辑器通常是将其转换为 HTML 文档进行预览或查看。

# Markdown vs. HTML

Markdown 并未涵盖所有 HTML 标签特性，但它与 HTML 是兼容的。换句话说，可以在 Markdown 文档中直接使用 HTML。虽然 Markdown 与 HTML 可以混合使用，但是有一些限制：

* HTML 块级元素需要始于新行，但行级元素可以与 Markdown 文本混合出现于同一行中。

* Markdown 语法在 HTML 块级元素中不会被处理，但在行级元素会被解析。
* HTML 最外层标签前不能有缩进（因为这是代码块的语法）。

另外一点差异在于特殊字符的书写，对于 HTML 而言需要处理诸如 `<` 和 `&` 等特殊字符，而 Markdown 通常可以自然书写它们。在 Markdown 中特殊字符的书写与转义有以下一些规则：

* 如 `&` 是 HTML 字符实体的一部分，它会保留原状，否则转换成 `&amp;`。
* 如 `<` 是 HTML 标签定界符，它会保留原状，否则转换成 `&lt;`。
* 在代码区块中，`&` 和 `<` 一定会转换成字符实体。
* `\` 是转义前缀，可以转义：````\ ` * _ {} [] () # + - . ! ````。 

# 块级元素

## 段落

对于 Markdown 而言，段落就是连续行上的文本。不同的段落前后以至少一个空行分隔。

> 注意，段落前后的空行不必是严格意义上的空行，只包含空格和制表符的行也视为空行。

与段落相关的一个概念是换行，它代表的是“硬换行”，其前后的内容还是属于同一段落。

> 在 Word 中，段落对应的是回车（Enter），而换行对应的是 `Shift` + `Enter`。如果设置“段落符号”可视的话，段落显示的是类似回车键上的符号，而换行显示的是向下的箭头。

在 Markdown 中，键入回车并不能起到换行的效果，它造成的效果仅仅是相当于插入了一个空格。要换行，需在行末插入至少 2 个空格，然后再键入回车。

换行与插入 `<br />` 标签是等效的，而想要插入多个换行的唯一方式就是使用 `<br />`。

## 标题

Markdown 支持 2 种形式的标题：Setext 和 atx。

Setext 样式的标题如下：

```markdown
一级标题
======
二级标题
------
```

其中 `=` 和 `-` 的数量是任意的，但通常至少要有 3 个。

> 从语法可知，Setext 样式仅支持两级标题，这通常不够用。
>
> 另一缺点是，并非所有 Markdown 编辑器都支持它。

atx 样式的标题如下：

```markdown
# 一级标题
## 二级标题 ####
###### 六级标题 ##
```

atx 最多支持六级标题。`#` 闭合是可选，并且闭合字符数量不必与开始的匹配。

## 分隔线

```markdown
***
---
___
```

分隔线有 3 种候选的符号：`*`（星号）、`-`（连接线）、`_`（下划线）。

需要 3 个及以上字符，且除空格和制表符外，行内无其他字符。

使用 `-`（连接线）时，与上一段落间至少应有一空行，否则会将上一行文字解释为二级标题。

> 大多 Markdown 编辑器中，二级标题预览样式下方也是有分隔符的。

## 块引用

```markdown
> 段落一
> 还是段落一
>
> 段落二
还是段落二

> 段落三
```

块引用使用 `>` 字符，最佳实践是在每一行前面都添加 `>`。也可以只在每个段落前添加 `>`。

块引用可嵌套使用其他 Markdown 元素，比如，标题、列表、代码块，甚至是块引用。

> 通常，Markdown 编辑器生成的块引用都是每行前加 `>`。

## 代码块

顾名思义，代码块用以插入代码段。其语法是：在每一行前添加 4 个空格或 1 个制表符。

每一行仅移除一级缩进，其他的会被保留。

代码块中所有字符都能自然书写，比如 HTML 标签，甚至是 Markdown 代码。

代码块会一直持续到没有缩进的那一行或文件结束。

## 列表

Markdown 支持有序列表、无序列表和任务列表。

```markdown
无序列表：
* item1
+ item2
- item3
有序列表：
8. item1
2. item2
5. item3
任务列表：
- [ ] task todo
- [x] task done
```

列表标记前最多可有 3 个空格（否则解析为代码块），之后至少接一个空格或制表符。

当列表项包含多个段落时，每个段落必须用 4 个空格或 1 个制表符来缩进。可以缩进每一行，以使列表美观，但这不是必须的。

列表项中嵌套块引用、代码块等时，需要双倍缩进。

与列表格式相同的文本需要使用 `\` 转义列表标记，有序列表转义的是英文句点（`.`）。

在同一文档中，尽量使用同一个无序列表标记，而不应混用。

有序列表前的数字不必是有序的，甚至可以是任意数字，但是为了 Markdown 原始文档的可读性，应使数字与其序号一致。

> 在一些 Markdown 编辑器中，并不支持任务列表。
>
> 支持任务列表的编辑器中，通常预览显示为复选框开头的列表。

## 表格

表格通常有 2 写法，其中一种简洁写法如下：

```markdown
First Header|Second Header
------------ | -------------
Content from cell 1 | Content fromcell 2
Content inthe first column | Content in the second column
```

> 有的 Markdown 编辑器不一定支持这种写法；也有可能会正确识别，但会转换为下一种“标准”写法。

```markdown
| Table | Col1 | Col2 |
| ----- |:----:|----: |
| Row1  | 1-1  | 1-2  |
| Row2  | 2-1  | 2-2  |
| Row3  | 3-1  | 3-2  |
```

注意，`:` 在哪端表示向哪端对齐，两端都有表示居中。默认为左对齐。

> 并非所有 Markdown 编辑器都支持文本对齐特性，但通常都能正常解析表格。


# 参考

## 文章

[《GitHub 风格的 Markdown 正式规范》发布](https://linux.cn/article-8399-1.html)：GFM 规范发布说明

## 规范文档

[GFM 规范](https://github.github.com/gfm/)

[CommonMark 规范](https://spec.commonmark.org/)

## 站点

[CommonMark 官网](https://commonmark.org/)：目前 GFM 基于此构建

[CommonMark 论坛讨论区](https://talk.commonmark.org/)： 可以提出关于该规范的的问题和更改建议 

## GitHub 仓库

[Sundown](https://github.com/vmg/sundown)：一个 Markdown 解析器，老版本的 GitHub Markdown 解析器基于此构建

[cmark](https://github.com/commonmark/cmark)：CommonMark 的 C 语言实现

[cmark-gfm](https://github.com/github/cmark-gfm)：GFM 的 C 语言实现，cmark 的衍生版本

[commonmark-spec](https://github.com/commonmark/commonmark-spec)：CommonMark 各种语言实现的列表