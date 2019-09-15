---
layout: page
title: JavaScript数组去重方法
category: JS
date: 2019-09-14
author: "Eric Zong"
---

* content
{:toc}
# 利用数据结构特性

## Set

```js
// 实现一
function unique(array) {
    return Array.from(new Set(array));
}
// 实现二
function unique(array) {
    return [...new Set(array)];
}
var array = [null, null, undefined, undefined, true, true, 42, 42, 42n, 42n, 'ez', 'ez', Symbol.for('ez'), Symbol.for('ez'), {}, {}];
unique(array); // [ null, undefined, true, 42, 42n, 'ez', Symbol(ez), {}, {} ]
```

利用了 Set 数据结构元素不重复的特性，这正是我们要的。

思路概述：将原数组转换为 Set 以去除重复元素，然后再转回数组。

> [MDN - Set 浏览器兼容性](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Set#%E6%B5%8F%E8%A7%88%E5%99%A8%E5%85%BC%E5%AE%B9)

## Map

```js
function unique(array) {
    const map = new Map();
    const list = [];
    
    for(let i = 0; i < array.length; i++) {
        if(!map.has(array[i])) {
            map.set(array[i], true);
            list.push(array[i]);
        }
	}
    
    return list;
}

const array = [null, null, undefined, undefined, true, true, 42, 42, 42n, 42n, 'ez', 'ez', Symbol.for('ez'), Symbol.for('ez'), {}, {}];

console.log(unique(array)); // [ null, undefined, true, 42, 42n, 'ez', Symbol(ez), {}, {} ]
```

思路概述：将数组元素作为键逐一加入 Map 中，通过 `map.has(key)` 方法判断元素是否已在 Map 中，如果元素不在 Map 中，则不是重复元素，加入结果数组。

## 对象

```js
function unique(array) {
	const obj = {};
	const list = [];
	
	for(let i = 0; i < array.length; i++) {
		if(!obj[typeof array[i] + Object.prototype.toString.call(array[i])]) {
			list.push(array[i]);
			obj[typeof array[i] + Object.prototype.toString.call(array[i])] = true;
		}
	}
	
	return list;
}

const array = [null, null, undefined, undefined, true, true, 42, 42, 42n, 42n, 'ez', 'ez', Symbol.for('ez'), Symbol.for('ez'), {}, {}];

console.log(unique(array)); // [ null, undefined, true, 42, 42n, 'ez', Symbol(ez), {} ]
```

思路概述：JavaScript 的对象与 Map 结构上很类似，因此，把 Map 换为对象也是可行的。

> 由结果可以看到，`bigint` 类型的数据被过滤掉了；重复的空对象也被去掉了。

# 自行判断重复

## 双循环检查

```js
function unique(array) {
	for(let i = 0; i < array.length; i++) {
		for(let j = i + 1; j < array.length; j++) {
			if(typeof array[i] == typeof array[j] && array[i] == array[j]) {
				array.splice(j, 1);
				j--;
			}
		}
	}
	
	return array;
}

const array = [null, null, undefined, undefined, true, true, 42, 42, 42n, 42n, 'ez', 'ez', Symbol.for('ez'), Symbol.for('ez'), {}, {}];

console.log(unique(array)); // [ null, undefined, true, 42, 42n, 'ez', Symbol(ez), {}, {} ]
```

思路概述：外层循环遍历数组，内层循环检查当前元素之后的所有元素是否与当前元素相同，相同着删除。

## 一般包含判断

```js
function unique(array) {
	const list = [];
	for(let i = 0; i < array.length; i++) {
		if(list.indexOf(array[i]) === -1) {
		// if(!list.includes(array[i]) {
			list.push(array[i]);
		}
	}
	
	return list;
}

const array = [null, null, undefined, undefined, true, true, 42, 42, 42n, 42n, 'ez', 'ez', Symbol.for('ez'), Symbol.for('ez'), {}, {}];

console.log(unique(array)); // [ null, undefined, true, 42, 42n, 'ez', Symbol(ez), {}, {} ]
```

思路概述：遍历原数组，当当前访问元素不存在于结果数组中时，将其放入。

## 排序后相邻元素比较

```js
function unique(array) {
	array = array.sort((a, b) => {
		const strA = typeof a + Object.prototype.toString.call(a);
		const strB = typeof b + Object.prototype.toString.call(b);
		if(strA < strB) return -1;
		if(strA > strB) return 1;
		return 0;
	});
	const list = [];
	for(let i = 0; i < array.length; i++) {
		if(array[i] !== array[i-1]) {
			list.push(array[i]);
		}
	}
	
	return list;
}

const array = [null, null, undefined, undefined, true, true, 42, 42, 42n, 42n, 'ez', 'ez', Symbol.for('ez'), Symbol.for('ez'), {}, {}];

console.log(unique(array)); // [ 42n, true, 42, null, {}, {}, 'ez', Symbol(ez), undefined ]
```

思路概述：先将原数组排序，这样就不需要遍历其他元素而只需要查看相邻元素是否相同就可以判断重复了。

# 应用Array高阶函数

## 借助对象属性过滤

```js
function unique(array) {
	const obj = {};
	array = array.filter((item, index, array) => {
		return obj.hasOwnProperty(typeof item + Object.prototype.toString.call(item)) ? false : (obj[typeof item + Object.prototype.toString.call(item)] = true);
	});
	
	return array;
}

const array = [null, null, undefined, undefined, true, true, 42, 42, 42n, 42n, 'ez', 'ez', Symbol.for('ez'), Symbol.for('ez'), {}, {}];

console.log(unique(array)); // [ null, undefined, true, 42, 42n, 'ez', Symbol(ez), {} ]
```

思路概述：在使用高阶函数 `filter` 过滤数组时，借助了一个外部状态对象，原数组信息如果不是该对象的属性，说明不重复，则保留该元素并将其作为状态对象属性存入；如果是该对象的属性，说明重复，则过滤掉。

> 能过滤重复空对象。

## 借助索引查找过滤

```js
function unique(array) {
	return array.filter((item, index, array) => {
		return array.indexOf(item, 0) === index;
	});
}

const array = [null, null, undefined, undefined, true, true, 42, 42, 42n, 42n, 'ez', 'ez', Symbol.for('ez'), Symbol.for('ez'), {}, {}];

console.log(unique(array)); // [ null, undefined, true, 42, 42n, 'ez', Symbol(ez), {}, {} ]
```

思路概述：使用高阶函数 `filter` 过滤数组，过滤函数查找原数组中当前元素首次出现的索引，如果和当前索引一致，则说明不重复；否则，重复，被过滤掉。

## reduce归并处理

```js
function unique(array) {
	return array.reduce((prev, current) =>
		prev.includes(current) ? prev : [...prev, current],
		[]
	);
}

const array = [null, null, undefined, undefined, true, true, 42, 42, 42n, 42n, 'ez', 'ez', Symbol.for('ez'), Symbol.for('ez'), {}, {}];

console.log(unique(array)); // [ null, undefined, true, 42, 42n, 'ez', Symbol(ez), {}, {} ]
```

思路概述：通过 `reduce` 将原数组分割为两部分，后半部分的首个元素如果存在于前半部分中，则重复，则丢弃该元素；否则，不重复，归并入前半部分中。

# 小结

综上可见，如果使用本质上是判等的方式来区分重复，那么必然不能去除空对象。想要去除空对象，需要借用对象属性，因为属性为空对象时，不同的空对象被认为是同一个属性。