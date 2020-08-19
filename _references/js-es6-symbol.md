---
layout: page
title: "ES6 Symbol类型示例"
category: JS
date: 2019-06-17
author: "Eric Zong"
---

* content
{:toc}
# 基本使用

```js
let s = Symbol();
typeof s;	// "symbol"
// 添加描述字符串，利于区分
let s1 = Symbol('ez');
s1.toString(); // 'Symbol(ez)'
// 对象参数，调用 toString 方法转换为字符串
const obj = {
    toString() {
        return 'ez';
    }
}
const s2 = Symbol(obj);
s2.toString(); // 'Symbol(ez)'
// 相同描述字符串创建的 Symbol 不相同
s1 === s2; // false
```

# 作属性名

```js
let symbolPro = Symbol();
// 写法一
let s1 = {};
s1[symbolPro] = 'EZ';
// 写法二
// 方括号是必须的，否则属性名被认为是字面值 symbolPro
let s2 = {
    [symbolPro]: 'EZ';
};
// 写法三
let s3 = {};
Object.defineProperty(s3, symbolPro, { value: 'EZ' });
// 访问写法类似，结果相同
s1[symbolPro]; // 'EZ'
// 不能用点号访问以 Symbol 为属性名的属性，否则被认为是访问名为 symbolPro 的普通属性
s2.symbolPro = 'hi';
s2[symbolPro]; // 'EZ'
s2['symbolPro']; // 'hi'
// 注意增强对象写法时的方法表述形式
let obj = {
    [symbolPro](arg) {
        // ...
    }
}
```

# 常量

```js
// Symbol 很适合用于定义常量
const log = {};

log.levels = {
  DEBUG: Symbol('debug'),
  INFO: Symbol('info'),
  WARN: Symbol('warn')
};
```

# 注册机制

```js
// 注册指定键值的 Symbol
let s1 = Symbol.for('EZ');
let s2 = Symbol.for('EZ');
s1 === s2; // true
// 获取 Symbol 注册的键值
Symbol.keyFor(s1); // 'EZ'
let s3 = Symbol('hi');
Symbol.keyFor(s3); // undefined，因为未注册
```

