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
* HTML 最外层标签前不能有缩进（因为这是代码的语法）。

另外一点差异在于特殊字符的书写，对于 HTML 而言需要处理诸如 `<` 和 `&` 等特殊字符，而 Markdown 通常可以自然书写它们。在 Markdown 中特殊字符的书写与转义有以下一些规则：

* 如 `&` 是 HTML 字符实体的一部分，它会保留原状，否则转换成 `&amp;`。
* 如 `<` 是 HTML 标签定界符，它会保留原状，否则转换成 `&lt;`。
* 在代码区块中，`&` 和 `<` 一定会转换成字符实体。
* `\` 是转义前缀，可以转义：````\ ` * _ {} [] () # + - . ! ````。 

# 参考

## 文章

[《GitHub 风格的 Markdown 正式规范》发布](https://linux.cn/article-8399-1.html)：GFM 规范发布说明，

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