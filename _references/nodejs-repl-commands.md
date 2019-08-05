---
layout: page
title: Node.js REPL基础命令
category: Node.js
date: 2019-08-05
author: Eric Zong
---

| 命令    | 说明                                                         | 备注                                                         |
| ------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| .help   | Print this help message<br>打印帮助信息                      |                                                              |
| .break  | Sometimes you get stuck, this gets you out<br>退出多行输入状态，比如，放弃多行函数书写返回命令提示符 | 等效于 `Ctrl + C`                                            |
| .clear  | Alias for .break<br>2 个作用：1. 等效于 `.break`；2. 清理上下文对象 | 只有在 `repl.start()` 启动的 `REPL` 环境中才可以清理上下文对象 |
| .editor | Enter editor mode<br>编辑模式                                |                                                              |
| .save   | Save all evaluated commands in this REPL session to a file<br>将当前会话中的表达式保存到文件 |                                                              |
| .load   | Load JS from a file into the REPL session<br>从导出的文件中加载表达式到当前会话 |                                                              |
| .exit   | Exit the repl<br>退出                                        | 等效于按 2 次 `Ctrl + C` 或 `Ctrl + D`                       |

> 注意：
>
> 1. 应在“干净”的命令行中执行以上基础命令。所谓“干净”，理想状态是仅包含要执行的基础命令，虽然如空格等空白字符也是允许的。
>
> 2. `Ctrl + C` 仅当在多行输入状态时才与 `.break` 等效，且比其更强大，不要求当前命令行是“干净”的。单行输入状态时，如果有任何输入，`Ctrl + C` 将丢弃当前行，开启一个新行；如果没有任何输入，将提示 `(To exit, press ^C again or type .exit)`，再按一次 `Ctrl + C` 退出 `REPL`。
>
> 3. 普通情况下进入的 `REPL` （比如仅使用 `node` 进入的），`.clear` 与 `.break` 是等效的。但是，如果 `REPL` 是通过执行某个运行 `repl.start()` 的脚本进入的，那么 `.clear` 的另一个效果是清理该 `REPL` 的上下文对象。比如：
>
>    ```js
>    # repl.js
>    const repl = require('repl');
>    
>    const context = repl.start().context;
>    context.msg = 'hello world!'
>    # 执行脚本并进入 REPL
>    P> node repl.js
>    > msg
>    'hello world!'
>    > .clear
>    Clearing context...
>    > msg
>    Thrown:
>    ReferenceError: msg is not defined
>    ```
>
> 4. 使用 `Ctrl + D` 退出 `REPL` 要求当前命令行没有任何输入，否则无效。如果有输入，可以先按 `Ctrl + C` 丢弃当前行，再按 `Ctrl + D`。

