---
layout: post
title: Chrome应用技巧
category: 浏览器
tags: 浏览器 Chrome 应用技巧
excerpt: 介绍Chrome各种实用功能、应用技巧以及调试技巧等。
author: Eric Zong
---

* content
{:toc}
# 快捷键

| 快捷键           | 功能描述                 | 说明                                               |
| ---------------- | ------------------------ | -------------------------------------------------- |
| F12              | 打开控制台               |                                                    |
| Ctrl + Shift + P | 弹出命令搜索框和命令列表 | 执行 `capture full size screenshot` 命令全网页截图 |

# 实用技巧

## 临时创建笔记页面

地址栏输入：`data:text/html, <html contenteditable>` 回车，即创建一可编辑的空白页面，可用于临时记录笔记等。

## 编辑页面

以下方法可以使一个页面可编辑：

* 地址栏输入 `javascript:void(document.body.contentEditable='true')` 回车执行
* 控制台输入 `document.body.contentEditable = true` 回车执行
* 控制台输入 `document.designMode = "on"` 回车执行

## 复制页面图片链接

```js
// 控制台输入
// 图片链接复制到剪贴板
copy($$('img').map(item => item.src).join("\r\n")
// 输出到控制台
$$('img').map(item => item.src).join('\r\n')
```

