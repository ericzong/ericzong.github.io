---
layout: post
title: JS中的undefined
category: JS
tags: JS 专题
excerpt: 介绍undefined，以及为什么它常用“void 0”代替。
author: "Eric Zong"
---

* content
{:toc}

# `undefined` 和 `null`

与其他的一些主流语言不同，在 JS 中，代表“无”的概念有两个值：`null` 和 `undefined`。但在含义上这两个值是有差异的：`null` 代表变量的值为“空”，而 `undefined` 代表变量已声明但未初始化。通俗地讲，`null` 表明变量已赋值，只是值为“空”；`undefined` 表明变量还未赋值。

为什么需要两个代表“无”的值？简单地说，这是个历史遗留问题。

最初，Brendan Eich 设计 JS 时，只有 `null` 没有 `undefined`。但是，由于 `typeof null` 的结果是 'object' —— 现在普遍认为这是 JS 的一个 bug —— 加上 JS 数据类型分为原始类型和对象类型，因此，Brendan Eich 觉得代表“无”的值最好不是对象以及其他一些原因，最终导致增加了 `undefined`。

由于是后加入的，所以 `undefined` 与 `null` 在实现与限制上有较大的不同。本质上说，`null`  是一个字面值，而 `undefined` 是一个全局变量——虽然，通常也把它视为字面值。

```js
// 浏览器中
window.undefined === undefined; // true
// Node.js 中
global.undefined === undefined; // true
```

对于 `null` 而言，它不是一个合法的标识符，因此，如果试图声明名为 `null` 的变量，会抛出异常。

```js
var null = 0; // SyntaxError: Unexpected token null
```

但是，`undefined` 不同，它本身就是一个全局变量，因此，`undefined` 显然是一个合法标识符，因此，可以声明名为 `undefined` 的变量。

但这并不代表可以修改全局的 `undefined`，在最新的宿主环境中，它通常是只读的。

```js
// 大多数最新版浏览器中
console.log(Object.getOwnPropertyDescriptor(window, undefined));
// Node.js 中
console.log(Object.getOwnPropertyDescriptor(global, undefined));
// {value: undefined, writable: false, enumerable: false, configurable: false}
```

> `undefined` 的只读特性是不确定的，尤其在老的浏览器版本中，它可能是可写的，比如 IE8。
>
> 在 IE8 中执行代码 `console.log(Object.getOwnPropertyDescriptor(window, undefined));` 会得到如下结果：
>
> {configurable: false, enumerable: false, value: undefined, writable: true}

然而，关键不在于全局的 `undefined` 是否只读，关键在于只要 `undefined` 是合法标识符，我们就可以声明名为 `undefined` 的局部变量，因而，在局部作用域中局部 `undefined` 变量将隐藏全局 `undefined`。

总之，形如 `xxx === undefined` 这样的判等是不可信的、有风险的。

# 最佳实践

要判断一个变量是否是 `undefined`，常见的是使用 `typeof` 操作符：

```js
typeof oneVar == 'undefined'
```

但是，在某些情况下，我们的确需要使用 `undefined` 的某种可信表示，比如在 `switch` 语句中。这时惯例会使用 `void 0` 代替 `undefined`：

```js
switch (oneVar) {
    case (void 0): 
        // do sth
    // ...
}
```

`void 0` 是什么鬼？

根据规范，`void` 是个一元操作符，无论其右侧的计算表达式是什么，结果都将返回值 `undefined`——这正是我们想要的结果。理论上说，我们可以用任意的 `void xxx`，使用 `void 0` 只是惯例。

> `void` 的另一个用法常见于页面中，用以产生一个“无效”值。
>
> 比如：`<a href='javascript:void(0)'>` 或 `<img src='javascript:void(0)'>`。

