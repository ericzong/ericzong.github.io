---
layout: post
title: Node.js专题：模块
category: Node.js
tags: Node.js 专题
excerpt: Node.js模块及导入、导出介绍。
author: Eric Zong
---

* content
{:toc}
# Node.js模块

Node.js 有自己的模块系统，导入使用 `require` 函数，导出使用 `exports` 或 `module.exports` 对象。

## 导出

先来看导出，实际上，Node.js 使用运行时对象来导出接口，这个对象就是 `module.exports`。

```js
const myVar = 42;
// 导出变量
module.exports.myVar = myVar;
function myFunction() {
    // do sth
}
// 导出函数
module.exports.myFunction = myFunction;
```

可以看到，导出总是使用 `module.exports` 稍显繁琐，可以使用 `exports` 替换较为冗长的 `module.exports`。因为，`exports` 是 `module.exports` 的一个快捷引用，故有：

```js
module.exports === exports; // true
```

像上面示例中那样，将变量或函数等作为导出对象的属性导出时，使用 `exports` 或是 `module.exports` 均可。

但是，只有 `module.exports` 才是真正的导出对象，而 `exports` 只是其引用，因此，我们不能为 `exports` 重新赋值一个对象来将其导出，这只会切断它与 `module.exports` 的引用关系，而不能导出对象。

## 导入

再来看导入，使用的是 `require` 函数：

```js
const myModule = require('myModule');
```

讲到这里，应该意识到，对于 Node.js 模块系统而言，模块与文件几乎是一一对应的。因此，导入的概念其实就是引入某个文件中公开的接口。而 `require` 函数的参数可以是：

* 模块标识符
* 相对路径
* 绝对路径

这就涉及文件查找的问题，绝对路径自然不必说明了。

当参数为模块标识符时，通常代表该模块是一个发布模块，Node.js 将从当前目录逐层上溯查找 `node_modules/myModule`，如果直到根目录都没有找到，则进行全局查找，查找位置包括：

* 环境变量 `NODE_PATH`
* `$HOME/.node_modules`
* `$HOME/.node_libraries`

当参数为相对路径时，相对路径的起始位置是当前目录，而当前目录在不同情况下是不同的：

* 在 REPL 中时，指命令行窗口所在目录
* 文件中运行时，指文件所在目录

可导入的模块文件可以是：`.js`、`.json`、`.node`（编译后二进制文件）。不过，导入时可以缺省后缀名。

## 相关使用

```js
// 判断当前是否主模块
if(module === require.main) {}
// 完整模块名
require.resolve('myModule');
// 访问模块缓存
require.cache[require.resolve('myModule')];
// 删除模块缓存
delete require.cache[require.resolve('myModule')];
```

# ES6模块

ES6 的模块系统就“标准”很多了，它使用较为常见的 `import` 和 `export` 关键字。

## 导出

```js
// 导出变量
export var one = 42;
// 导出函数
export function test() {};
// 导出类
export class MyClass {};
// 批量导出
export { x, f1, c1 };
// 默认导出
export default xxx; // <=> export { xxx as default };
// 重命名
export { a as x };
// 重导出
export { x, y, z } from 'anotherModule';
```

## 导入

```js
import {x} from 'myModule';
// 重命名
import {a as x} from 'myModule';
// 默认导入
import defaultValue from 'myModule';
// 混合导入
import dv, {x} from 'myModule';
// 整体导入
import * as myModule from 'myModule';
```

# 比较

1. Node.js 导出的是副本，因此模块内部变化不会影响导出接口；但是 ES 模块不是这样，模块内部变化会影响导出接口。
2. `import` 执行静态解析，不能使用表达式和变量等运行时元素。
3. `import` 导入的接口标识符是只读的，不可重新赋值。
4. 两个模块系统可以混用，如果想要 `import` Node.js 导出的接口需要注意。由于 Node.js 导出的是运行时对象，而 `import` 是编译时解析的，所以 `import` 不允许解构，应该整体导入或默认导入。

```js
// 错误
import {readFile} from 'fs';
// 整体导入
import * as express from 'express';
const app = express.default();
// 默认导入
import express from 'express';
const app = express();
```