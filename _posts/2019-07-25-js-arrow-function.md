---
layout: post
title: JS箭头函数
category: JS
tags: JS 专题
excerpt: 介绍JS箭头函数与常规函数的差异以及使用限制等。
author: Eric Zong
---

* content
{:toc}

# 箭头函数是什么？

```js
let hi = () => { console.log('Hello, world!' }
```

上面的代码等号右侧部分是一个 ES6 标准新增的一种函数，由于其定义语法使用“箭头”（`=>`），因此称为“箭头函数”。

如果你了解其他的一些语言，那么会发现许多语言都有类似语法，而甚至一些语言开始没有，随后也陆续加入了。实际上，在计算机编程中，这被称为“lambda 表达式”，是一种匿名函数。

显然，它的语法简洁，表现优雅，极其适合编写短小的函数。

但是，JS 本来就支持匿名函数的，而箭头函数并非是常规匿名函数的简写，它们之间存在着差异，并不能任意互换。

# 关于`this`

箭头函数不绑定 `this`，它从自己作用域链的上一层继承 `this`。

不绑定 `this` 导致了一系列的后果，其中比较重要的是：箭头函数不能用作构造函数、`call()/apply()/bind()` 均不能改变 `this` 指向……

笔者在阮一峰老师的《ECMAScript 6入门》中读到“[使用注意点](http://es6.ruanyifeng.com/#docs/function#%E4%BD%BF%E7%94%A8%E6%B3%A8%E6%84%8F%E7%82%B9)”章节时，被其首个关于 `this` 的示例搞懵了，示例代码如下：

```js
function foo() {
  setTimeout(() => {
    console.log('id:', this.id);
  }, 100);
}

var id = 21;

foo.call({ id: 42 });
// id: 42
```

文章中说箭头函数中 `this` 是固定的，随后也举例说明 `call()/apply()/bind()` 均不能修改箭头函数的 `this` 指向。

笔者当时的纠结在于，在上述环境中执行 `foo()` 将输出 `id: 21`——`this` 变了呀！！！（不可否认，这种描述在笔者的思维模式下很容易被引入坑中，最后笔者结合上下文以及其他资料才搞明白。）

本质上来说，箭头函数没有自己的 `this`，它的 `this` 继承自作用域链的上一层，“固定”的其实是这个继承关系，而这个继承的 `this` 代表什么是可变的。

回头说例子，箭头函数继承的是 `foo()` 函数作用域中的 `this`。当使用 `foo.call({ id: 42 })` 调用时，`this` 指向传入的对象，所以输出 `id: 42`；而直接调用 `foo()` 时，`this` 指向全局变量（即 `window`），而全局中我们定义了 `var id = 21`，因此输出 `id: 21`。

因此，不应该将箭头函数作为对象方法，尤其当其中使用了 `this` 时（`this` 不会指向当前对象，很可能它指向全局对象，通常这不是我们想要的效果）。

> 上面的例子在浏览器环境或是 Node.js 交互环境中执行 `foo()` 都会输出 `id: 21`，上文已经分析了。
>
> 但是，如果我们把以上代码保存在 `demo.js` 文件中，并通过 `node demo.js` 执行，会发现 `foo()` 的执行结果输出 `id: undefined`。你可以尝试输出 `this`，通常会看到一个空对象，说明此时 `this` 指向的不是全局对象 `global`。那它指向的是什么对象呢？答案是指向 `module.exports`。这是由于对于 Node.js 而言，每个文件都是一个模块，它的顶级作用域不是全局作用域而是模块作用域。因此，很好理解此时 `this` 的指向了。

# 返回对象字面量

试图使用“简写体”形式`params => {object: literal}` 返回一个对象字面量是不行的：

```js
var func = () => { foo: function() {} }; 
// SyntaxError: function statement requires a name
```

这跟语法解析有关，对象字面量的花括号会被解析为“块体”形式下的块定界符。这通常会导致语法错误，可在编码时通过语法检查发现。但是，在某些情况下却是符合语法的：

```js
var func = () => { foo: 1 };
// Calling func() returns undefined!
```

上例中的 `foo` 被解析为一个标签，随后执行了一条语句 `1`。没有返回值，所以调用结果会是 `undefined`。

解决方案是将对象字面量包裹在圆括号中：

```js
var func = () => ({ foo: 1 });
```

# 其他

不绑定 `arguments`， 可使用 `rest` 参数代替。也不绑定 `caller` 和 `callee` 等。

箭头函数没有`prototype`属性。

箭头函数不能用作生成器。

当需要换行时，不能在箭头函数的参数与箭头之间换行，否则是一个语法错误。

箭头不是运算符，但箭头函数“优先级”很低，在混合运算表达式中通常需要使用括号包裹。

# 参考

[MDN - JavaScript 箭头函数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

[《ECMAScript 6入门》8. 函数的扩展#箭头函数](http://es6.ruanyifeng.com/#docs/function#%E7%AE%AD%E5%A4%B4%E5%87%BD%E6%95%B0)

