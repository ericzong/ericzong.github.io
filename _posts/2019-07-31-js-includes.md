---
layout: post
title: JS中的几种包含判断
category: JS
tags: JS 专题
excerpt: 介绍JS中几种包含关系的判断方法以及各自的适用范围。
author: Eric Zong
---

* content
{:toc}
# `in`操作符

`in` 操作符判断指定的属性是否包含在指定的对象或其原型链中。

```js
prop in object
```

`in` 操作符作用于对象值，因此，显然不能用于基本数据类型，但不那么明显的是，它也不能用于字符串字面量。

```js
// 以下代码均导致 TypeError
1 in 110

const str = 'test'
'length' in str
```

并且 `in` 操作符判断的是属性，因此，其左操作数应该是某种合法的属性名类型，这包括：字符串和 `Symbol`。有两点需要说明的是：

1. 如果左操作数不是 `Symbol` 类型，将强制转换为字符串（左操作数可为带 `toString()` 函数的对象）；
2. 作用于数组时，左操作数可以是索引。（注意，字符串对象有类似数组的结构）。

```js
let names = ['Eric', 'Zong'];
0 in names; // true，包含数组索引
'Eric' in names; // false，不能判断数组是否包含某个元素
'length' in names; // true，包含属性
Symbol.iterator in names; // true，ES6+
let indexObj = { toString() {return 0} };
indexObj in names; // true，等价于 0 in names
```

# `indexOf()`方法

字符串与数组上均存在 `indexOf()` 方法，但严格来说它们都不是判断方法，因此注意，其返回的不是布尔值，而是索引数值。

```js
// 字符串方法
str.indexOf(searchValue[, fromIndex = 0])
// 数组方法，ES5+
arr.indexOf(searchElement[, fromIndex = 0])
```

> 应该注意 2 个方法在开始搜索索引参数（`fromIndex`）上的行为差异。
>
> 注意 ES 支持版本。

# `includes()`方法

```js
// 字符串方法，ES6+
str.includes(searchString[, position])
// 数组方法，ES7+
arr.includes(valueToFind[, fromIndex])
```

> 应该注意 2 个方法在开始搜索索引参数上的行为差异。
>
> 注意 ES 支持版本。

# 小结

* 判断对象是否存在某个属性，可用 `in`。
* 判断字符串是否包含某个子串，可用 `indexOf()`、`includes()`。
* 判断数组是否包含某个元素，可用 `indexOf`、`includes()`。

> 注意：`indexOf()` 和 `includes()` 是严格判等的，因此，如果数组元素为对象，那可能不能得到预期的结果。

