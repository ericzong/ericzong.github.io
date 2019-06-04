---
layout: page
title: ES6解构示例
category: JS
---

* content
{:toc}
# 解构数组

```js
// 声明时赋值
let [a, b, c] = [1, 2, 3];
// 声明后赋值
let x, y, z;
[x, y, z] = new Set(['a', 'b', 'c']);
// 默认值
let one, two;
[one="hello", two="world"] = [, "Eric"]
// 嵌套数组
let [foo, [[bar], baz]] = [1, [[2], 3]];
// 交换变量
[x, y] = [y, x]
// 函数实参数组解构
function add([x, y]){
  return x + y;
}
add([1, 2]);
[[1, 2], [3, 4]].map(([a, b]) => a + b);
// 解构函数返回数组
function returnArray() {
  return [1, 2, 3];
}
let [a, b, c] = returnArray();
// 忽略值
let [a, , c] = returnArray();
let [ , , third] = ["foo", "bar", "baz"];
[,,] = returnArray();
// 剩余模式
let [head, ...tail] = [1, 2, 3, 4];
// 字符串解构
const [a, b, c, d, e] = 'hello';
```

# 解构对象

```js
// 基本赋值
let { foo, bar } = { foo: "aaa", bar: "bbb" };
let { length: len } = 'hello';
// 完整形式
let { foo: foo, bar: bar } = { foo: "aaa", bar: "bbb" };
// 无声明赋值
let a, b;
({ a, b } = { a: 1, b: 2 });
// 新变量名赋值
let { foo: baz } = { foo: 'aaa', bar: 'bbb' };
// 默认值
let { h = "hello", n = "world" } = { n: "Eric" }
// 新变量名默认值
let { h: msg = "hello", n: name = "world" } = { n: "Eric" }
// For of 迭代和解构
for (let [key, value] of map) {
  console.log(key + " is " + value);
}
for (let [key] of map) {}
for (let [,value] of map) {}
// 函数返回对象解构
function returnObject() {
  return {
    foo: 1,
    bar: 2
  };
}
let { foo, bar } = example();
// 函数实参对象解构
function f({x, y, z}) {}
f({z: 3, y: 2, x: 1});
// 计算属性名解构
let key = "who";
let { [key]: name } = { who: "Eric" };
// 剩余模式*
let { x, y, ...rest } = { x: 1, y: 2, a: 10, b: 20 };
// 无效标识符属性名
const me = { 'my-name': "Eric" };
const { 'my-name': name } = me;
// 数组对象解构
const csvFileLine = '2018,Eric Zong,ericzonglu@126.com';
const { 1: name, 2: email } = csvFileLine.split(','); // 1、2 是数组索引
// 解构到对象属性或数组元素
let obj = {};
let arr = [];
({ foo: obj.prop, bar: arr[0] } = { foo: 123, bar: true });
// 解构对象函数到变量
let {toString: s} = 123;
let {toString: s} = true;
```
