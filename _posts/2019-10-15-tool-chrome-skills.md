---
layout: post
title: Chrome应用技巧（持续更新……）
category: 浏览器
tags: 浏览器 Chrome 应用技巧 持续更新
excerpt: 介绍Chrome各种实用功能、应用技巧以及调试技巧等。
author: Eric Zong
---

* content
{:toc}
# 快捷键

| 快捷键           | 功能描述         | 说明                                               |
| ---------------- | ---------------- | -------------------------------------------------- |
| F12              | 打开控制台       |                                                    |
| Ctrl + Shift + P | 弹出快捷指令面板 | 执行 `capture full size screenshot` 命令全网页截图 |

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
copy($$('img').map(item => item.src).join("\r\n"))
// 输出到控制台
$$('img').map(item => item.src).join('\r\n')
```

# 快捷指令面板

当 Devtools 打开时，使用快捷键 `Ctrl + Shift + P` 激活，在开始输入框中键入“?”可查看所有可用命令。

| 命令前缀 | 说明                                    |
| -------- | --------------------------------------- |
| `...`    | Open file/打开文件                      |
| `:`      | Go to line/前往文件行                   |
| `@`      | Go to symbol/前往标识符（函数、类名等） |
| `!`      | Run snippet/运行脚本文件                |
| `>`      | Run command/执行命令                    |



# 控制台

## 格式化日志

| 指示符  | 说明            | 示例                                  |
| ------- | --------------- | ------------------------------------- |
| %s      | 字符串          |                                       |
| %i / %d | 整型            |                                       |
| %f      | 浮点型          |                                       |
| %o      | DOM 对象        |                                       |
| %O      | JavaScript 对象 |                                       |
| %c      | CSS 样式        | `console.log("%c样式", "color:#f00")` |

## 常用API

| API                           | 说明                                                         | 示例                                    |
| ----------------------------- | ------------------------------------------------------------ | --------------------------------------- |
| `$0`~`$4`                     | 缓存最近查看过的 5 个元素，序号越小越近，`$0` 表示当前查看/选择的元素 | `$0`                                    |
| `$('selector', [startNode])`  | 返回首个匹配元素，根节点可选，默认为 `document`              | `$('#root')`                            |
| `$$('selector', [startNode])` | 返回所有匹配元素，根节点可选，默认为 `document`              | `$$('button')`                          |
| `$x(path, [startNode])`       | XPath 选择器                                                 | `$x('//p')`                             |
| `getEventListeners(element)`  | 查看元素上的事件                                             |                                         |
| `monitorEvents(element)`      | 监控元素上的事件                                             | `monitorEvents(document.body, "click")` |
| `monitor(functionName)`       | 监控函数                                                     | `monitor(add)`                          |
| `copy(value)`                 | 复制内容到剪贴板                                             |                                         |
| `inspect(element)`            | 选中指定的元素                                               |                                         |

# 参考

[Google - Console API Reference](https://developers.google.com/web/tools/chrome-devtools/console/api)

[Google - Console Utilities API Reference](https://developers.google.com/web/tools/chrome-devtools/console/utilities)

[Google - Chrome 开发者工具](https://developers.google.com/web/tools/chrome-devtools/)