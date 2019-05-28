---
layout: post
title: "PowerShell专题：数组"
category: PowerShell
tags: PowerShell 专题
excerpt: "PowerShell数组专题。"
author: "Eric Zong"
---

* content
{:toc}
# 概述

数组是最常用的数据结构，而 PowerShell 的数组很灵活，要达到相同的目标通常可以采用多种方式。

# 创建

PowerShell 有多种方式来创建一个数组。最常见的是用“字面形式”一一列出元素，元素间用“,”分隔：

```powershell
$listArray = 1, 3, 5
```

> 允许在“,”前后添加空格。

如果数组仅有一个元素，开头需要有一个逗号，否则不会被认为是数组：

```powershell
$oneArray = , 42
```

如果数组元素是连续的整数，那么可以使用**范围操作符（range operator）**表示：

```powershell
$rangeArray = 5..10
```

还可以使用**子表达式操作符（sub-expression operator）**，即使是创建空数组或只有一个元素的数组都是很优雅的：

```powershell
$emptyArray = @()
$oneArray = @(42)
```

读者或许已经注意到了，以上创建方式都同时指定了数组元素，如果仅仅是想初始化一个数组待用呢？仅用 PowerShell 没有办法达到这个要求，需要借助 .Net Framework。

```powershell
[int[]]$numbers = [int[]]::new(10)
```

另外，PowerShell 的数组是一维的，即使嵌套，本质上还是一维的。同样，除非借助 .Net Framework，否则不能创建多维数组。

```powershell
[int[,]]$rank = [int[,]]::new(5,5)
$rank.rank # 2
```

# 访问元素

不仅可以通过正整数索引正序访问数组元素，也可以通过负整数索引逆序访问数组元素。比如 `$array[-1]` 访问最后一个元素。

甚至可以指定一组索引提取多个元素组成新数组：

```powershell
# 逗号分隔的索引序列
$array[0, 2, 4]
# 范围
$array[4..7]
```

当索引使用范围指定时，需要注意截取顺序，比如：

```powershell
$array=1..10
$array[1..3] # 2, 3, 4
$array[3..1] # 4, 3, 2
$array[-3..-1] # 8, 9, 10
$array[-1..-3] # 10, 9, 8
$array[1..-3] # 2, 1, 10, 9, 8
$array[-1..3] # 10, 1, 2, 3, 4 
```

可见，当起始索引号小于结束索引号时，是正向截取的，相反是反向截取的。其中，最易被误解的是 `$array[1..-3]`，很可能被认为是从索引 1 正向截取到索引 -3，而误以为结果是 2~8。

另外注意，反向截取时，是允许越过数组边界的。

更为强大的是，可以使用“+”合并多组索引：

```powershell
# 可以“混合”合并，单个、序列和范围形式的索引
$array[0,2+4..6+8]
```

# 属性、方法及操作

`Length` 属性表示数组长度，而 `Count` 是其别名，`Longlength` 用以表示元素个数超过 2,147,483,647 时的长度。

`Rank` 表示数组维度，当使用多维数组时会用到。

`Clear` 方法会将所有元素重置为默认值。这跟声明数组时是否指定类型有关，如果指定了类型，比如：`[int[]]$numbers`，则所有元素将被设为 0；如果没有指定类型，将被视为 Object 类型，设为 `$null`。注意，该方法不会重置数组大小。

`$array.setValue(value, index)` 可设置元素值，等价于 `$array[index] = value`。

可以使用“+”向数组添加元素：

```powershell
$array = @(1..5)
$array = $array + 6
$array += 7
```

> 注意，数组大小是固定的，因此任何会改变数组大小的操作都会生成一个新数组。

但是当“+”的两个操作符都是数组时，会合并这两个数组。

```powershell
$a = 1, 3
$b = 2, 4
$c = $a + $b # 1, 3, 2, 4
```

如果想要遍历数组元素，可以使用原始的 `for` 循环：

```powershell
for ($i = 0; $i -lt $array.length; $i++) {
	$array[$i]
}
```

或者 `while` 循环：

```powershell
$i = 0
while($i -lt $array.length) {
	$array[$i]
	$i++
}
```

更简洁一点是使用 `foreach` 循环：

```powershell
foreach ($element in $array) {
	$element
}
```

更为强大的是数组的 `foreach` 方法，它有多个重载版本：

```powershell
ForEach(scriptblock expression)
ForEach(type convertToType)
ForEach(string propertyName)
ForEach(string propertyName, object[] newValue)
ForEach(string methodName)
ForEach(string methodName, object[] arguments)
ForEach(scriptblock expression, object[] arguments)
```

> 详细使用可参考 [这里](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_arrays?view=powershell-6#foreach)。

数组还有过滤方法，语法如下：

```powershell
Where(scriptblock expression[, WhereOperatorSelectionMode mode
                            [, int numberToReturn]])
```

* expression：过滤条件表达式
* mode：过滤模式，`default`、`first`、`last`、`SkipUntil`、
* numberToReturn：限制返回元素数量

`Default` 模式下，将返回条件为真的元素，数量受 `numberToReturn` 限制。

`First` 模式下，如不指定 `numberToReturn`，返回条件为真的第一个元素，否则数量受 `numberToReturn` 限制。

`Last` 模式下，如不指定 `numberToReturn`，返回条件为真的最后一个元素，否则数量受 `numberToReturn` 限制。

`SkipUntil` 模式下，跳过条件为假的元素，直到条件为真的元素，返回该元素及剩余元素，数量受 `numberToReturn` 限制。

`Until` 模式下，返回条件为假的元素，直至条件为真，数量受 `numberToReturn` 限制。

`Split` 模式下，将返回 2 个数组，前者的元素为条件测试为真，后者的元素为条件测试为假，`numberToReturn` 仅限制前者数量。

```powershell
(0..9).Where{ $_ % 2 }
```

# 特殊情况

即使一个数组为 `$null`，仍然可以安全地访问其 `Count` 和 `Length` 属性（均为 0）。但 `LongLength` 属性不存在。

# 参考

[MS PS Doc - About Arrays](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_arrays?view=powershell-6#properties-of-arrays)