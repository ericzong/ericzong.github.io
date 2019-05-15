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

简单来说，函数就是命名的语句块。函数结构包括：函数名、参数、函数体。定义语法如下：

```powershell
Function FunctionName (args[])
{
	# code
}
```

PowerShell 允许访问函数体中的内容，这需要用到虚拟驱动器：

```powershell
$function:FunctionName
```

同理，我们可以借此删除函数：

```powershell
del Function:FunctionName
```

# 参数

与大多数语言类似，PowerShell 函数定义时也支持将参数列表置于函数名后的圆括号中，各参数间以 `,` 分隔。

比较特别的是，PowerShell 的函数调用是“命令式”的——调用时，各参数列在函数名后，并以空格分隔。

## 命名参数与位置参数

通常情况下，参数在函数定义时将绑定名称和位置，即是说参数是可具名且位置敏感的。因此，在调用函数传参时，一方面可以指定参数名并赋值，这称为“**命名参数**”；另一方面，可以依次列出参数值而不需要指定参数名，这称为“**位置参数**”。比如有如下函数：

```powershell
function paramFunction ($arg1, $arg2) 
{
    $arg1
    $arg2
}
```

位置参数调用：

```powershell
PS > paramFunction hello world
```

命名参数调用：

```powershell
PS > paramFunction -arg1 hello -arg2 world
```

甚至可以混合使用位置参数和命名参数：

```powershell
PS > paramFunction hello -arg2 world
PS > paramFunction -arg2 world hello
```

## `$args` 访问所有参数

PowerShell 允许不定义参数，但在调用时传递参数。此时，如果要在函数体中使用传递的参数就需要用到内置的参数变量 `$args`，它是一个数组，依序存储了传入的所有参数。

```powershell
function sum
{
    $sum = 0
    $args | foreach { $sum += $_ }
    $sum
}

PS > sum 2 5 8
```

上面的函数并没有定义任何参数，但在调用时传入了参数，并在函数体中通过 `$args` 访问它们。

> 如果熟悉 JavaScript 的话，会发现这点上很类似。

## 参数类型

到目前为止，都还没有限制参数的类型——这在 PowerShell 中是允许的——如果不指定参数类型，那么理论上，可以传入任何值。但实际上，这通常不是我们想要的。

要为参数指定类型需要在定义函数时，在参数之前加上类型限制，比如：`[int]$num`、`[string]$name`等等。这跟限制变量类型一致。

```powershell
function Add([int]$one, [int]$another)
{
	$one + $another
}
```

对于 PowerShell 而言，对于一个布尔类型的参数，更合适的作法是将其定义为 `[switch]` 类型，称为“**开关参数**”。开关参数与布尔类型参数类似，其默认值均为 `$false`，因此当参数值为 `$false` 时，均不必传此参数。而开关参数优于布尔类型参数的方面在于，当参数值为 `$true` 时，开关参数只需要列出参数名，而不需要给出参数值。

## 默认值

对于大多数类型的参数而言，当调用函数但不传递该参数时，参数都其缺省默认值。如果想自行为参数指定默认值的话，只需要在函数定义时在参数后用 `=` 将默认值赋值给参数即可。

```powershell
function Add([int]$one=1, [int]$another=1)
{
	$one + $another
}
```

> 一个带有技巧性的用法是，为参数指定一个异常默认值。
>
> ```powershell
> function doNotMiss($arg=$(throw "请提供参数 arg！")) {
> 	$arg
> }
> ```
>
> 这样，如果不为 `$arg` 参数传值，那么将得到指定的异常提示。

## 调用

调用函数时，命名参数的名称是可以截断的，只要无歧义即可。换句话说，调用函数时指定参数名可以不写参数全名，而只写部分前缀，前提是通过这个前缀能唯一确定一个参数。

> **参数截断**应当被视为一种命令行快速编写方式，不应在脚本中使用，这会降低脚本的可读性。

## 另一种参数定义方式

PowerShell 函数的参数可以像前文那样定义，这跟大多数语言类似，但还可以用其他方式定义参数——将参数定义在函数内部——而且这是 PowerShell 推荐的定义方式，因为该定义方式赋予更多的配置能力。语法如下：

```powershell
function FunctionName
{
	[CmdletBinding()]
	Param(
		[Parameter(Mandatory=$true, Position=0)]
		[int]$arg,
		[Parameter()]
		[Alias("a2", "p2")]
		$arg2
	)
	
	# code
}
```

`Mandatory=$true` 指定参数为必需的，如调用函数时未传递该参数，会提示输入，手动输入后会继续执行。

> PowerShell 3.0+ 可以省略赋值，直接写为 `Mandatory` 即为必需。

`Position=0` 指定参数位置，索引从 0 开始。

`[Alias()]` 可定义参数别名，多个别名用 `,` 分隔。

> 参数有个 `ParameterSetName` 属性用以设置**参数集**名称。
>
> 参数集有点类似于一种函数重载的实现，它根据不同的参数集名称将参数分组，当传入不同的参数时，匹配不同的参数集，以实现不同的函数功能。相关参考[这里](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_functions_advanced_parameters?view=powershell-6#parametersetname-argument)。
>
> 但是，可能传入的参数对于多个参数集都适用，这时，需要通过 `CmdletBinding()` 配置 `DefaultParameterSetName` 属性，以指定此时默认应该选择哪个参数集。相关参考[这里](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_functions_cmdletbindingattribute?view=powershell-6#defaultparametersetname)。

还有一系列的验证属性可用于配置，比如：`[ValidateNotNull()]`、`[ValidateNotNullOrEmpty()]` 等等，这里不一一例举。可通过 `help about_functions_advanced_parameters` 查看相关说明。

# 返回值

PowerShell 比较特殊的一点是，会将函数中所有的“输出”作为返回值。

> 这里的“输出”指的是除写控制台操作外，执行后会在控制台回显值的语句。
>
> 如果不想某些输出成为返回值，可以用写入控制台的方式避免。

函数的返回值可以不是单值，当有多个输出时，它们将被收集到一个数组中返回。

`return` 语句可以返回值，并且它将阻止后续语句执行。

# 脚本

脚本本质上与函数有很多相似性，除了不能使用第一种参数定义方式外，其他几乎可以照搬。
