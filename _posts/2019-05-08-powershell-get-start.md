---
layout: post
title: "PowerShell入门：语言篇"
categories: PowerShell
tags: PowerShell 入门
excerpt: "PowerShell脚本语言入门教程。"
author: "Eric Zong"
---

* content
{:toc}

# 简介

PowerShell 是实现系统和应用程序管理自动化的命令行脚本环境。

Windows 系统一般已经内置了 PowerShell，内置版本随系统版本更新而更新。

通过微软网站可以下载 PowerShell 的更新程序，目前 PowerShell 5 应该是可下载的最后版本。但这并不是最新版本，因为，PowerShell 6 已经在 GitHub 开源。并且，开源版本的 PowerShell 是跨平台的，也就是说不仅可以在 Windows 系统上使用，也可以在 Linux 或是 MacOS 等系统上使用。

# 环境搭建

对于 Windows 系统而言已经内置了 PowerShell，应该通常不需要额外安装，除非要使用更高版本的特性。如果有必要更新 PowerShell 版本，可以到微软网站下载，或到 [GitHub](https://github.com/PowerShell/PowerShell/releases/latest) 下载开源版本。

> 可以在命令行通过 `$PSVersionTable.PSVersion` 查看 PowerShell 版本。
> 
> 对于其他系统，则必需下载开源版本。本文以说明 Windows 系统环境为主。

更新或安装 PowerShell 后，在命令行可以通过执行 `powershell` 命令进入 PowerShell 交互模式。

另外，PowerShell 还配套有一个集成脚本环境，称为“Windows PowerShell ISE” 。它可以通过 `powershell_ise` 命令启动，或是在开始菜单搜索“ISE”字样查找。

> 除非需要编写脚本文件，否则通常不会使用 ISE。

# 变量

PowerShell 变量使用 `$` 作为前缀，即 `$variable` 的格式。

> 变量名不区分大小写，即 `$var` 与 `$Var` 是同一个变量。
>
> PowerShell 通常名称、字符串操作符等都是大小写不敏感的，除非特别说明是敏感的。

变量不必事先声明，而且也不必指定类型。比如：`$num = 42`。

> 如果不指定类型，PowerShell 会自行推断变量类型，但有可能推断类型跟所需类型不同。这时，指定变量类型就有必要了。
> 
> 如果不清楚某变量类型，可通过 `$variable.gettype().name` 查看。

指定变量类型只需要在变量前将类型用方括号括起来即可。如：`[DateTime]$date="2019-05-07 00:30:00"`。

需要特别注意的是布尔类型值，对于大多数语言而言布尔类型通常都有 `true` 和 `false` 两个字面值，但是在 PowerShell 中分别使用 `$true` 和 `$false`。形式上来说，它们是变量，但是如果试图给其赋值将会导致错误。因为，`$true` 和 `$false` 是只读的，或者说是常量。

# 运算符

字符串连接符应该是最常用的运算符了，与大多数语言相同，PowerShell 也使用 `+`。

与大多数语言不同的是，PowerShell 转义符使用 `` ` ``（重音符/沉音符），而非 `\`（反斜杠） 。要注意的是，`` ` `` 同时也是续行符，当用作续行符时，其后不能有其他字符，包括空白字符，否则视作转义符。

> 续行符，脚本语言中较常见的一个概念，通常在以行为自然分割的语言中，将一行长代码分割为多行。

常用的数学运算符通常都是可用的，比如：`+`、`-`、`*`、`/`、`%` 和 `()` 等。而且，PowerShell 还能识别计算机容量单位，包括 KB、MB、GB、TB、PB，并且进行数学运算，比如：`1gb/1mb`（结果为 1024）。

作为“命令式”语言，PowerShell 大部分运算符都是命令参数或说选项的形式，而非符号。

比如常用的比较运算符有：`-eq`（等于）、`-ne`（不等于）、`-gt`（大于）、`-lt`（小于）、`-ge`（大于等于）、`-le`（小于等于）、`-contains`（包含）、`-notcontains`（不包含）……

常用的逻辑运算符有：`-and`、`-or`、`-xor`、`-not`（等效于 `!`）。

# 数据结构

## 数组

最常用的数据结构就是数组，PowerShell 中创建数组很简单：一是在圆括号前加 `@`，二是直接列出元素。元素间用 `,` 分隔。

```powershell
$array=@(1, 2, 3)
$array=2, 3, 4
$array=@()            # 空数组
$array=@("one")       # 单元素数组
$array=,"one"         # 单元素数组，注意开头的逗号不可省略
$array=1..5           # 连续整数简写形式
```

数组可以使用负索引逆序访问数组元素，如：`$array[-1]`。

甚至支持“切片”操作。

```powershell
$array[1,3]
$array[1..3]
$array[($array.Count)..0] # 逆序输出
```

可以使用 `+` 为数组“添加”元素。

```powershell
$array=$array+6
$array+=7
```

`+` 如果作用于 2 个数组，则会把它们合并起来。

```powershell
$all=$array1+$array2
```

> 注意：数组长度是不可变的，因此，上述所有会改变数组长度的操作都生成了一个新数组。有时意识到这点很关键。

## 哈希表

另一常用的数据结构就是哈希表，即映射，用以存储键值对。

与数组类似，花括号前加 `@` 即可创建哈希表。键值间用 `=` 连接，键值对间用 `;` 分隔。

```powershell
$map=@{key1="value1";key2="value2"}
```

可以用索引或属性方式访问键对应的值，如：`$map["key1"]` 和 `$map.key1`。

哈希表有很多方法和属性可用，常用的有：

```powershell
$map.add("key3", "value3")   # 添加键值对
$map.remove("key3")          # 删除键值对
$map.Item("key3")            # 访问
$map.Item("key3")="value 3"  # 赋值
$map.Contains("key3")
$map.ContainsKey("key3")
$map.ContainsValue("value3")

$map.Count    # 键值对数量
$map.Keys     # 键集
$map.Values   # 值集
```

默认哈希表是无序的，如果希望其有序，可在 `@` 前加 `[Ordered]`。

```powershell
$orderedHashTable=[Ordered]@{}
```

# 控制流程

## 条件分支

### if-elseif-else

```powershell
if (...) {} elseif (...) {} else {}
```

### switch

```powershell
switch($value)
{
    value1 {}
    {exp with $_} {}
    {exp with $value} {break}
    # ...
    default {}
}
```

> 如果匹配分支代码不执行 `break`，则不会退出 `switch`。但注意，此时不会无条件执行后续分支代码，而是会继续测试分支条件。

## 循环

### for

```powershell
for($i=0; $i -le 42; $i++) {}
```

### foreach

```powershell
foreach($i in $array) {}
```

### while

```powershell
while(exp) {}
do {} while (exp)
```

### do-until

```powershell
do {} until (exp)
```

### switch

```powershell
switch($collection)
{
    {exp with $_} {}
    # ...
    default {}
}
```

> `switch` 支持遍历集合，使用 `$_` 引用当前元素。

# 异常处理

PowerShell 的异常跟一般的编程语言中的异常有所差异。默认情况下，一般异常不会导致脚本停止，而是继续运行，这称为“非终止异常”。只有某些特殊的异常会阻止脚本运行，比如：调用不存在的命令、数学表达式运算错误、命令安全性问题等，通常称其为“终止异常”。

> PowerShell 是一门脚本语言，它关注的是自动化或者说批处理特性，因此，出现一般异常继续执行是合理的。

非终止异常是不可捕获的，因此，如果想要捕获处理，需要将其转换为终止异常。有两种方式转换：

* 设置全局配置参数 `$ErrorActionPreference='Stop'`（全局影响）

* 执行命令时指定 `-ErrorAction` 选项为 `Stop`（仅影响当前命令，但需要命令支持）

PowerShell 捕获异常的方式也有两种。

一种是常见的 `try-catch-finally` 形式。

```powershell
try {
    # 可能抛异常的语句
} catch [ExceptionType] {
    # 异常处理程序
} finally {
    # 总是执行的语句
}
```

> catch 后的捕获异常类型是可选，如果不指定则捕获所有终止异常。
> 
> 另一方面，也可以并列多个 `catch` 以捕获多个类型的异常。

另一种是 trap：

```powershell
trap [ExceptionType] {
    # 异常处理程序
}
```

> 同样，`trap` 后的捕获异常类型是可选的。`trap` 捕获的是其所在作用域内的异常。
> 
> 从结构上说，`trap` 比 `try-catch-finally` 更优雅灵活，而且真正做到了将程序正常流与异常流代码分离。
> 
> 更多异常相关知识点将在异常专题讨论。

# 函数

PowerShell 允许我们像大多数语言一样定义函数：

```powershell
function functionName($arg1, $arg2)
{
    # 代码
    return returnValue # 可选
}
```

> 如果没有参数，函数名后的括号是可选的。

但更为常见的定义方式是将参数定义在函数内部，因为这可以获得更多的配置特性：

```powershell
function functionName
{
    Param(
        [Parameter(...)] # 圆括号中可以对参数进行配置
        $arg1,
        [Parameter(...)]
        $arg2
    )
    # 代码
    return returnValue # 可选
}
```

函数的调用形式更类似于命令调用。

对于前一种函数定义方式而言，参数的位置顺序跟定义顺序是相同的。因此，要么按顺序传参，要么按名称传参：

```powershell
functionName argValue1 argValue2       # 按顺序传参
functionName -arg1 value1 -arg2 value2 # 按名称传参
```

对于后一种函数定义方式而言，默认情况下，我们也可以使用上述两种调用方式。

但是，我们可以通过指定 `[Parameter]` 的 `position` 属性来定义参数的位置索引（从 0 开始）。一旦这样做了，其他未指定索引的参数将不得不使用名称传参。比如：

```powershell
function xxx
{
    Param(
        [Parameter()]
        [int]$one,
        [Parameter(position=0)]
        [string]$another='test'
    )

    $one
    $another
}
# 调用
xxx -one 42 -another 'test'
xxx 'test' -one 42
```

> 从上可知，不仅可以指定参数类型，还可以定义缺省值。
> 
> `[Parameter]` 还可以配置参数是否必须、别名、验证等等，这里不一一赘述。
> 
> 更多函数相关知识点将在函数专题讨论。

# 脚本

PowerShell 脚本文件后缀通常为 .ps1。

与函数类似的是，脚本参数也是使用 `Param` 定义。

```powershell
# hello.ps1
Param(
	[string]$name='world'
)
write-host "Hello, $name!"
```

如果脚本在当前活动目录下，则需要用 `./xxx.ps1` 的方式调用。当然，后缀名是可以省略的。

```powershell
./hello.ps1 # Hello, world!
./hello Eric # Hello, Eric!
```

> PowerShell 默认不从当前活动目录加载脚本。因此，除非脚本位于 path 路径下，才可以直接使用脚本名调用。

# 其他

PowerShell 注释分为行注释和块注释，行注释用 `#`，块注释用 `<# ... #>`。

# 资源

[PowerShell Documentation - MS](https://docs.microsoft.com/zh-cn/powershell/)

[Powershell - GitHub](https://github.com/PowerShell/PowerShell)

[PowerShell在线教程 - PSTips.net](https://www.pstips.net/powershell-online-tutorials)
