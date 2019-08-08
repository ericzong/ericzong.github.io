---
layout: post
title: "Node.js 入门"
categories: Node.js
tags: Node.js 入门
excerpt: "Node.js 入门教程。"
author: "Eric Zong"
---

* content
{:toc}

# 概述

Node.js 是运行在服务端的 JavaScript。

> Node.js 不是语言名称，它只是 JS 的一种宿主环境。

它使用 Google 的 V8 引擎，V8 引擎使用 C++ 开发，与 C 语言执行效率非常相近。

它的高性能依赖于两种机制：非阻塞型 I/O 和 事件循环。

# 交互式解释器

跟大多数现代语言环境一样，Node.js 提供名为 REPL(Read-Eval-Print-Loop) 的交互式解释器。

> REPL 内部使用 eval 函数来评估表达式执行结果。

Node.js 安装成功后，通过 `node` 命令可以在命令行进入交互模式。

## 上下文对象

如果直接进入 REPL 环境中，通常，只能进行一些简单的测试或实验工作，否则，输入量会较大，不适合通过交互模式进行。

一个可选的方案是，将某些任务的基础工作放在一个脚本文件中执行，以初始化交互环境。

Node.js 可通过脚本向 REPL 上下文对象中添加变量或函数。

比如，使用 `node repl.js` 执行包含如下内容的 js 脚本，将进入交互模式并可以直接使用脚本中定义的变量 `msg` 和函数 `test`。

```js
// repl.js
var repl = require('repl');
var context = repl.start().context;
context.msg = 'Hello! World!';
context.test = function() {console.log(context.msg);};
```

> start() 开始了一个 REPL，这就是为什么执行后会进入交互模式而不是回到命令行。

## 基础命令

REPL 自带一些基础命令，它们均以“.”开头。通过其中一个基础命令 `.help` 就可查看所有基础命令帮助。

```
> .help
.break    Sometimes you get stuck, this gets you out
.clear    Alias for .break | Break, and also clear the local context
.editor   Enter editor mode
.exit     Exit the repl
.help     Print this help message
.load     Load JS from a file into the REPL session
.save     Save all evaluated commands in this REPL session to a file
```

交互模式下，我们不只可以输入单行表达式，也可以输入多行，比如定义函数时往往会是多行。多行表达式从第 2 行开始以英文省略号为前缀。

```js
> var test = function() {
... console.log('Hello! World!');
... };
```

编写多行表达式时，很容易出现输入错误，而换行后就不可以修改前面的行，于是只得取消，返回交互模式。这时就要使用 `.break` 命令了。

> 值得注意的是，`.break` 命令尽可能新起一行输入。
>
> 命令之前可以有空格，但有其它字符将不会返回交互模式。
>
> 命令之后也可以有其它字符，不过当然是以空格分隔的。
>
> 另外，Ctrl + C 是个更快捷的选择。

`.clear` 命令通常是 `.break` 的别名。但是，如果交互模式是由 start() 启动的，那么 `.clear` 除了与 `.break` 相同的效果外还有一个附加效果——清理上下文对象——即交互中定义的所有变量和函数都将清除。

> 当 `.clear` 命令启动清理效果时，会回显“Clearing context...”字样。

`.editor` 命令会进入编辑器模式。它允许一次性输入大段代码，可以包含多个表达式。但是，编辑器模式仍然不能修改光标所在行之前的行。

```shell
> .editor
// Entering editor mode (^D to finish, ^C to cancel)
```

`.exit` 退出交互模式。

> 在“干净”的交互状态下，有两个快捷键可用于退出：Ctrl + C（按两次） 或 Ctrl + D。
>
> 所谓“干净”，是指命令提示符（“>”）后没有任何输入。如果有，可以按 Ctrl + C 抛弃它。
>
> 按两次 Ctrl + C 退出时，第一次会回显“(To exit, press ^C again or type .exit)”提示。

`.save` 和 `.load` 是一对命令，前者将当前会话中执行的命令保存到文件，后者用以加载这种命令文件。

# 基础知识

## 变量

`global` 代表全局命名空间，是 Node.js 的顶级变量。

下划线（_）可访问最近使用的表达式。

## 模块

### 导入导出

exports，导出变量、函数或方法

module.exports，等价于 exports。但将模块定义为类时，仅能用它。

require(module)，将从当前目录逐层上溯查找 node_modules/module 以及全局目录：

* 环境变量 NODE_PATH
* \$HOME/.node_modules
* \$HOME/.node_libraries

### 模块文件

* .js
* .json
* .node，编译后的二进制模块文件

### 相对路径

模块文件的相对路径相对于当前目录而言，当前目录：

* REPL 运行时，指命令行窗口所在目录
* 文件中运行时，指文件所在目录

### 相关使用

```js
// 判断当前是否主模块
if(module === require.main) {}
// 查询完整模块名
require.resolve(module);
// 已加载模块缓存，键-值对
require.cache[require.resolve(module)]
// 删除模块缓存
delete require.cache[require.resolve(module)]
```

# 参考

## 命令

```shell
# V8 开发中特性
(node --v8-options) -match "in progress"
# V8 版本
node -p process.versions.v8
```

## 链接

[菜鸟教程](http://www.runoob.com/nodejs/nodejs-tutorial.html)

[Node入门](https://www.nodebeginner.org/index-zh-cn.html)

[GitHub](https://github.com/nodejs/node)

[NodeCloud](https://www.nodecloud.org/)

[Node.js中文网 - API](http://nodejs.cn/)

