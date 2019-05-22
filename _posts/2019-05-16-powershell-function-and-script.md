---
layout: post
title: "PowerShell入门：函数与脚本"
category: PowerShell
tags: PowerShell 入门
excerpt: "PowerShell函数与脚本入门教程。"
---

* content
{:toc}

# 函数

简单来说，函数就是命名的语句块。

简单函数定义语法如下：

```powershell
Function <function-name> ($param1, $param2)
{
	# code
}
```

> 完整的函数结构包括：关键字、作用域、函数名、参数、函数体。完整语法参考 [这里](#函数完整方法)。

# 关键字

函数定义通常以 `Function` 开头，说明定义类型为函数。

> 关键字还可以是 `Filter`，定义的是过滤器，它是一种特殊的函数。

# 作用域

通常，函数属于创建它的作用域，除非有必要否则一般由 PowerShell 自行管理其作用域。

如需指定作用域，需在函数名前加作用域前缀（\<scope:\>）。

# 函数名

PowerShell 建议的函数名格式为“动名词对（verb-noun pair）”，并且动词还规定有标准的，可通过命名 `Get-Verb` 查看。

> 函数名理论上可以是“任意”的，但符合 PowerShell 建议格式的函数名更易被他人理解。

# 参数

## 定义方式

PowerShell 有两种参数定义方式，一种跟大多数语言类似，将参数放置于函数名后的圆括号中，如：

```powershell
function test($a, $b) {
  $a + $b
}
```

也可以将参数定义在函数体内，置于 `param` 语句中，如：

```powershell
function test1 {
  param($a, $b)
  
  $a + $b
}
```

效果上是相同的，但 PowerShell 推荐后者。

## 命名参数与位置参数

像上面的示例一样，如果在定义函数时有声明参数，这些参数都是有名字的，称为“**命名参数**”。

也允许定义函数时不声明参数，而在执行时传递参数，这种参数没有名称，只能通过相对位置访问，称为“位置参数”。

## 参数类型

到目前为止，都还没有限制参数的类型——这在 PowerShell 中是允许的——如果不指定参数类型，那么理论上，可以传入任何值。但实际上，这通常不是我们想要的。

要为参数指定类型需要在定义函数时，在参数之前加上类型限制，比如：`[int]$num`、`[string]$name`等等。这跟限制变量类型一致。

```powershell
function Add([int]$one, [int]$another)
{
	$one + $another
}
```

## 开关参数

对于 PowerShell 而言，一个布尔类型的参数，更合适的作法是将其定义为 `[switch]` 类型，称为“**开关参数**”。

开关参数不要求传值，如果为 `$true` 只需要列出该参数，为 `$false` 不列出。

## 默认值

命名参数可以指定默认值，只需要在参数声明时，后跟 `=` 将默认值赋值给参数即可。如：

```powershell
function Add([int]$one=1, [int]$another=1)
{
	$one + $another
}
```

> 一个带有技巧性的惯用法是，为参数指定一个异常默认值。
>
> ```powershell
> function doNotMiss($arg=$(throw "请提供参数 arg！")) {
> 	$arg
> }
> ```
>
> 这样，如果不为 `arg` 参数传值，那么将得到指定的异常提示。

# 函数体

## 访问所有参数

通过自动变量 `$args` 数组，可以访问所有未声明的参数。

```powershell
function sum
{
    $sum = 0
    $args | foreach { $sum += $_ }
    $sum
}

PS > sum 2 5 8 
# 15
```

上面的函数并没有声明任何参数，但在调用时传入了参数，并在函数体中通过 `$args` 访问它们。

如果函数调用时，所传参数都声明了，那么，可以使用自动变量 `$PSBoundParameters` 访问，它是一个字典，以参数名为键，传入的参数值为值。

```powershell
function print($a, $b)
{
    $PSBoundParameters
}

PS > print xxx yyy
# Key Value
# --- -----
# a   xxx  
# b   yyy  
```

`$args` 包含函数未声明的参数，而 `$PSBoundParameters` 包含函数声明的参数。因此，如果传递的参数既有命名参数又有位置参数，那么，两个变量合在一起才能访问所有参数。

> 显然，`$args`  和 `$PSBoundParameters` 是互补关系，不存在交集。

## 管道参数

函数可以接收并处理管道对象，不过其函数体结构略有不同，语法如下：

```powershell
function pipingFunction
{
    Begin {...}
    Process {...}
    End {...}
}
```

`Begin` 和 `End` 块中的语句分别在开始和结束时执行一次，`Process` 块中的语句针对管道中每一个对象执行一次。这 3 个块都是可选，如果都没有，相当于所有语句都位于 `End` 块中。

```powershell
function printPipe
{
    Begin { 'begin' }
    Process { $_ }
    End { 'end' }
}

PS > 1,2,3|printPipe
# begin
# 1
# 2
# 3
# end
```

`Process` 块中使用 `$_` 访问当前处理的管道对象。

管道参数还涉及一个自动变量 `$input`。在开始时（`Begin` 块中）它是空的。如果没有 `Process` 块，那么管道对象会逐一追加到其中，因此，在结束时（`End` 块中）它包含所有管道对象；但是如果有 `Process` 块，管道对象将从 `$input` 移到 `$_` 中，因此，结束时（`End`块中）它是空的。

## 返回值

PowerShell 比较特殊的一点是，会将函数中所有的“输出”作为返回值。

> 这里的“输出”指的是除写控制台操作外，执行后会在控制台回显值的语句。
>
> 如果不想某些输出成为返回值，可以用写入控制台的方式避免。

函数的返回值可以不是单值，当有多个输出时，它们将被自动收集到一个数组中返回。

`return` 语句可以返回值，并且它将阻止后续语句执行。

# 调用

PowerShell 调用函数跟执行命令格式类似：

```powershell
PS > function-name -arg1 value1 others
```

## 传递数组

如果某个参数是数组，则各元素用逗号分隔即可。

```powershell
PS > function-name -arrayArg e1,e2,e3
```

## 参数截断

调用函数时，命名参数的名称是可以截断的，只要无歧义即可。换句话说，调用函数时指定参数名可以不写参数全名，而只写部分前缀，前提是通过这个前缀能唯一确定一个参数。

> **参数截断**应当被视为一种命令行快速编写方式，不应在脚本中使用，这会降低脚本的可读性。

# 管理

我们可以通过 `Function:` 驱动器查看函数定义：

```powershell
# 查看函数定义
$function:<function-name>
# 复杂一点的方法
(Get-ChildItem function:<function-name>).Definition
```

同理，我们可以借此删除函数：

```powershell
del Function:FunctionName
```

# 脚本

脚本本质上与函数有很多相似性，除了不能使用第一种参数定义方式外，其他几乎可以照搬。

# 参考

## 函数语法

```powershell
function [<scope:>]<name> [([type]$parameter1[,[type]$parameter2])]
{
  param([type]$parameter1 [,[type]$parameter2])
  dynamicparam {<statement list>}
  begin {<statement list>}
  process {<statement list>}
  end {<statement list>}
}
```

## 链接

[MS PS Doc - About Functions](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_functions?view=powershell-6)