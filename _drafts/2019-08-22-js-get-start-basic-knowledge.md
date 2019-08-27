---
layout: post
title: JS入门：基础知识
category: JS
tags: JS 入门
excerpt: JS入门教程，基础知识讲解。
author: Eric Zong
---

* content
{:toc}

# 值、变量与函数

## 值与类型

跟其他语言一样，JavaScript 用各种值来表示数据。

类型，在编程术语中，指对值的不同表示方法。

因此，任何值都具有类型。

JavaScript 目前有 8 种内置类型：`Undefined`, `Null`, `Boolean`, `Number`, `String`,  `Symbol`(ES6), `BigInt`(ES10), `Object`。

由于 JavaScript 的数据类型借鉴自 Java，所以，与 Java 类似，除 Object 属于引用类型外，其他几种类型统称原始类型，或基本类型。

与值相关的一个概念是“字面量”或“字面值”，它指的是直接包含在源码中的值。比如 `42` 就是一个 `Number` 类型的字面量。

### Undefined

`Undefined` 类型意味着缺失。它的唯一字面量是 `undefined`。

> `undefined` 值出现在以下场景中：
>
> * 未初始化的变量
> * 未提供的函数参数
> * 对象缺少的属性的默认值
> * 无返回值的函数的默认返回值
> * `void` 运算符运算结果
>
> 注意，`undefined` 是合法标识符，即可将其声明为局部变量以及为其赋值（严格模式不可以），但通常这很可能会引发问题。最佳实践：永远不要重新定义 `undefined` 以及为其赋值。详细讨论参考 [这里](/posts/js-undefined-identifier.html)。

### Null

`Null` 类型表示空值。与 `Undefined` 不同，它明确表示没有值，即空值，而非缺省。`null` 是其唯一字面量。

> `null` 的类型为 `Null`，而不是 `Object`。但 `typeof` 误判 `null` 为 `Object` 类型，是因为数据在底层都是以二进制形式存储，当二进制前三位为 0，则会被 `typeof` 判定为 `Object` 类型，而 `null` 的二进制位都是 0。

### Boolean

`Boolean` 是逻辑类型值，包含 2 个字面量：`true` 和 `false`。

> 当其他类型的值被（显式或隐式）转换为 Boolean 类型时，可能被转换为 `true` 或 `false`，以此引出了 Truthy 类型值和 Falsy 类型值的概念。
>
> Falsy 类型值，在类型转换时会被转换为 `false` 的值，与之相对的是“Truthy 类型值”。
>
> Falsy 类型值包括：""（空字符串）、0（+0/-0）、null、undefined、NaN、false。其他均为 Truthy 类型值。这里要特别注意，由于 Boolean 的封装对象是引用类型，所以它总转换为 `true`，即使封装的原始值是 `false`。

### Number

`Number` 是基于 IEEE 754 标准的双精度 64 位二进制格式的值（-(2^63^ -1) 到 2^63^ -1）。

它可以表示整数与小数。

### BigInt

`BigInt` 可以用任意精度表示整数。它的字面量使用后缀 `n` 表示，比如：`42n`。

> 注意，`BigInt` 不能与 `Number` 进行混合运算。

### String

JavaScript 的字符串是不可变的，这与 Java 类似。

> 即使是在字符串字面量上也可以调用方法，这是因为 JavaScript 隐式地将字面量转换成一个临时字符串对象，并调用方法，然后丢弃掉该临时对象。
>
> JavaScript 没有字符类型，只能使用包含单个字符的字符串代替。
>
> 另一方面，字符串可看作具有字符数组结构，因此，可以在字符串上进行某些数组操作。

### Symbol

ES6 新增类型。

它代表某种意义上的唯一值。

相关使用可参考 [这里](/references/js-es6-symbol)。

### Object

JavaScript 的对象以“键-值对”的形式表示，与 `Map` 结构极为相似。

## 变量

在程序中，变量用以保存程序要使用的值，它仅仅是一个符号容器。使用“变量”这个名字是因为这个容器中的值是可以变化的。

变量的主要用途：管理程序状态。状态跟踪了值随着程序运行的变化。

与 Java 等静态语言不同，JavaScript 是动态类型语言，或者说是弱类型，即它允许一个变量在任意时刻存放任意类型的值。

变量赋值通常分为两个阶段：1. 编译器，声明变量；2. 运行时引擎，赋值。

变量在使用时需要进行变量查询，变量查询分为 LHS 和 RHS，其区别在于查询目的和失败行为：

|              | LHS                                                 | RHS            |
| ------------ | --------------------------------------------------- | -------------- |
| 变量查询目的 | 赋值                                                | 获取值         |
| 查询失败行为 | 非严格模式：自动隐式创建； 严格模式：ReferenceError | ReferenceError |

> 注意：严格模式禁止自动或隐式地创建全局变量。

## 函数

函数是“可调用对象”，有一个内部属性 `call`，使其可被调用。

函数可拥有属性，其 `length` 属性表示其参数个数。

在整个声明中，如果 `function` 是第一个词，则是函数声明，否则是函数表达式。函数表达式可以是匿名的，而函数声明不可以省略函数名。

立即执行函数表达式（IIFE，Immediately Invoked Function Expression），常见形式有：

```javascript
// 形式一
(function(){...})();
// 形式二
(function(){...}());
```

`Function.prototype` 本身就是一个没有操作的空函数。

# 附录

## Unicode相关概念

Unicode 码点转义（code point escaping），如：`\u{1D11E}`。

Unicode 规范化（Unicode normalization），`String.normalize(..)`。

组合音标符号（Combining Diacritical Mark），会修改其前面相邻的字符的码点。

字素（grapheme），视觉上看到的渲染出来的单个字符。并不总是与程序处理意义上的单个“字符”严格对应。

