---
layout: page
title: JavaScript常用API参考
category: JS
date: 2019-07-03
author: Eric Zong
---

* content
{:toc}

# Array

## reduce

对数组中每个元素执行一个指定的函数以将其汇总为单个值。

```js
arr.reduce(callback(accumulator, currentValue[, index[, array]])[, initialValue])
```

回调 reducer 函数的 4 个参数：

1. Accumulator (acc) 累计器
2. Current Value (cur) 当前值
3. Current Index (idx) 当前索引
4. Source Array (src) 源数组

initialValue 初始值

初始值会影响累计器的初值，以及 reducer 的调用次数。
如果有初始值，累计器的初值即为指定的初始值，reducer 从数组索引 0 开始被调用；
如果没有初始值，累计器的初值为数组首个元素（显然空数组会导致异常），reducer 从数组索引 1 开始被调用。

reduce 可用于累加（对象数组里的值）、求极值、高维数组扁平化、统计元素出现次数、数组去重。
