---
layout: page
title: 
category: 编程语言
tags: JS
date: 2020-05-16
author: Eric Zong
---

* content
{:toc}

> 英文原文：[A case for using `void` in modern JavaScript](https://gist.github.com/slikts/dee3702357765dda3d484d8888d3029e)
>
> 原谅作者：Reinis Ivanovs
>
> 原文日期：2020-03-24

过去，[`void` 运算符](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/void)有一些用例：

* 用于 `<a href="javascript:void(0)">` 以实现动态按钮，但这是一种不良且过时的做法

* 在可能覆盖标识符 `undefined` 的邪恶的前 ES5 时代，用于访问原始 `undefined` 值
* 用于节省一些字符而将 `undefined` 缩短为 `void 0`；这仍被压缩器（minifier）使用

除了由 ES6 中的箭头函数引入的新用例外，这些用例对于现代 JavaScript 均没有普世价值：

# 使用`void`来说明无返回值（`void`）的箭头函数

ES6 箭头函数表达式允许函数体省略花括号以使代码更为简洁，但是这在返回值是否打算被使用方面产生了二义性，因为该函数也许仅仅只是使用其副作用：

```javascript
// 省略花括号
const id = x => x;

// 调用函数仅仅是为了副作用
const log = x => console.log(x);
```

控制台 API 是众所周知的，因此该示例并不具二义性，但它说明了原理。

添加花括号可以显式地创建刻意返回 void 的箭头函数，但这将降低简洁性：

```
const log = x => {
  console.log(x);
}
```

该函数仍然可以在一行中格式化，但是像 Prettier 这样的自动格式化工具通常会添加换行符，并且不返回值的意图不如使用 `void` 运算符时清晰：

```javascript
const log = x => void console.log(x);
```

像这样使用 `void` 的最明显的好处是减少了行数；尤其是像 CPS 这样的编程风格，它们经常依赖 void 回调。

## 类型系统与 [void 类型](https://en.wikipedia.org/wiki/Void_type)

许多类型系统，包括 TypeScript 的类型系统，都具有 void 类型的概念，这使得 `void` 运算符对无返回值函数的使用是互补的并且更易于识别。

## React [`useEffect`](https://reactjs.org/docs/hooks-effect.html) 钩子

`void` 的常见实用案例是 React 中的 `useEffect` 钩子：

```javascript
useEffect(() => void (document.title = 'example'));
```

这是一个特别突出的例子，因为 `useEffect` 钩子可以使用它的回调返回值来清理效果，所以意外地返回一个函数可能是一个运行时错误。

# 在异步 IIFES 中使用 `void`

对于使用 `void` 运算符的异步的立即调用函数表达式（IIFES），还有另外一个但不太重要的用例。传统的 IIFES 用于实现模块化作用域，因为 JS 没有块作用域或实际的模块，但是现代 JS 两者兼具，所以 IIFES 除了一种情况外，在很大程度上没有意义：在[模块的顶层](https://2ality.com/2019/12/top-level-await.html)使用 `await` 或 `for-await-of` 语法：

```javascript
void (async () => {
  await fetch(something);
})();
```

这是一个足够流行的用例，为此存在第 3 阶段的建议。由于优先级，仍需在函数表达式周围加上括号，但是使用 `void` 解决了前括号不能与 ASI 一起使用的问题（更具体地说，如果没有显式分号行终止符，则括号会中断）。

# 风格指南与 [`no-void`](https://eslint.org/docs/rules/no-void)

流行的风格指南，比如 Airbnb 和标准 JS，使用 `no-void` ESLint 规则来阻止使用 `void` 运算符，但是可以说这是一种疏忽而应由用户重写。

* [Airbnb 关于移除 `no-void` 的问题](https://github.com/airbnb/javascript/issues/2145)
* [标准 JS 关于移除 `no-void` 的问题](https://github.com/standard/standard/issues/1464)
* [ESLint `no-void` 规则更改请求](https://github.com/eslint/eslint/issues/12688)

---

（精彩留言，归纳、概述、意译）

**话题一**：

为什么需要用

```javascript
useEffect(() => void (document.title = 'example'));
```

代替

```javascript
useEffect(() => { document.title = 'example' });
```

后者比使用 `void` 更短并更具可读性。

**作者答疑**：

启用 [`brace-style`](https://eslint.org/docs/rules/brace-style) 的 ESLint 或 Prettier 会将带有花括号的块体（body） 格式化为三行，而不是一行。`void` 使无返回值更为明显，而相对地，忽略 `return` 语句可能仍是错误的。

**话题二**：

在 React 中，以下代码可以阻止用户修改文本框内容，同时支持选中文本拷贝：

```react
<input type="text" onChange={void(0)} value="This value cannot be changed by the user" />
```

**作者观点**：

这应该是 `readonly` 属性要做的。

**反馈**：

`readonly` 对于 UX 而言并不是好的选择，大多数浏览器实现都阻止用户拷贝文本框内容。

`readonly` 的这种实现可能在未来会被各浏览器修改，但使用上面所提到的技巧，现在就可以得到期望的行为。

