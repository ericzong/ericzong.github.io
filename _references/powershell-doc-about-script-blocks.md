---
layout: page
title: "[译]关于脚本块"
category: PowerShell
date: 2019-05-28
---

* content
{:toc}

> 原文：[About Script Blocks](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_script_blocks?view=powershell-6)

# 简介

定义什么是脚本块以及在 PowerShell 编程语言中如何使用脚本块。

# 详细描述

在 PowerShell 编程语言中，脚本块是可作为独立单元使用的一组语句或表达式集合。
脚本块可以接受参数和返回值。

从语法上讲，脚本块是大括号中的语句列表，如以下语法所示：

```powershell
{<语句列表>}
```

脚本块返回脚本块中所有命令的输出，可以是单个对象，也可以是数组。

与函数类似，脚本块可以包含参数。使用 Param 关键字指定命名参数，如以下语法所示：

```powershell
{
Param([type]$Parameter1 [,[type]$Parameter2])
<语句列表>
}
```

> **注意**
>
> 在脚本块中，与函数不同，您不能在大括号外指定参数。

与函数类似，脚本块可以包括 `DynamicParam`、`Begin`、`Process` 和 `End` 关键字。有关更多信息，请参阅[about_Functions](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_functions?view=powershell-6) 和 [about_Functions_Advanced](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_functions_advanced?view=powershell-6)。

# 使用脚本块

脚本块是 Microsoft .NET Framework 类型 `System.Management.Automation.ScriptBlock` 的实例。命令可以具有脚本块参数值。例如，`Invoke-Command` cmdlet 具有一个 `ScriptBlock` 参数，该参数采用脚本块值，如下例所示：

```powershell
Invoke-Command -ScriptBlock { Get-Process }
```

输出

```
Handles  NPM(K)    PM(K)     WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----     ----- -----   ------     -- -----------
999          28    39100     45020   262    15.88   1844 communicator
721          28    32696     36536   222    20.84   4028 explorer
...
```

`Invoke-Command` 还可以执行具有参数块的脚本块。使用 **ArgumentList** 参数按位置分配参数。

```powershell
Invoke-Command -ScriptBlock { param($p1, $p2)
"p1: $p1"
"p2: $p2"
} -ArgumentList "First", "Second"
```

输出

```
p1: First
p2: Second
```

前面示例中的脚本块使用 `param` 关键字创建了参数 `$p1` 和 `$p2`。字符串“First”绑定到第一个参数（`$p1`），而“Second”绑定到（`$p2`）。

您可以使用变量来存储和执行脚本块。下面的示例将脚本块存储在变量中，并将其传递给 `Invoke-Command`。

```powershell
$a = { Get-Service BITS }
Invoke-Command -ScriptBlock $a
```

输出

```
Status   Name               DisplayName
------   ----               -----------
Running  BITS               Background Intelligent Transfer Ser...
```

调用操作符是执行存储在变量中的脚本块的另一种方法。与 `Invoke-Command` 类似，调用操作符在子作用域中执行脚本块。调用操作符可以使您更容易使用脚本块的参数。

```powershell
$a ={ param($p1, $p2)
"p1: $p1"
"p2: $p2"
}
&$a -p2 "First" -p1 "Second"
```

输出

```
p1: Second
p2: First
```

您可以使用赋值将脚本块的输出存储在变量中。

```powershell
PS>  $a = { 1 + 1}
PS>  $b = &$a
PS>  $b
2
```

```powershell
PS>  $a = { 1 + 1}
PS>  $b = Invoke-Command $a
PS>  $b
2
```

有关调用运算符的更多信息，请参阅 [about_Operators](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_operators?view=powershell-6)。

# 使用带参数的延迟绑定脚本块

接受管道输入（按值）或（按属性名）的类型化参数允许在参数上使用延迟绑定脚本块。在延迟绑定脚本块中，您可以使用管道变量 `$_` 引用管道对象。

```powershell
# 将 config.log 重命名为 old_config.log
dir config.log | Rename-Item -NewName {"old_" + $_.Name}
```

在更复杂的 cmdlet 中，延迟绑定脚本块允许重用一个管道对象来填充其他参数。

关于延迟绑定脚本块作为参数的注意事项：

* 您必须显式指定与延迟脚本块一起使用的任何参数名称。
* 参数不能是无类型的，参数的类型不能是 `[scriptblock]` 或 `[object]`。
* 如果在不提供管道输入的情况下使用延迟绑定脚本块，则会接收到一个错误。

```powershell
Rename-Item -NewName {$_.Name + ".old"}
```

输出

```
Rename-Item : Cannot evaluate parameter 'NewName' because its argument is
specified as a script block and there is no input. A script block cannot
be evaluated without input.
At line:1 char:23
+  Rename-Item -NewName {$_.Name + ".old"}
+                       ~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : MetadataError: (:) [Rename-Item],
      ParameterBindingException
    + FullyQualifiedErrorId : ScriptBlockArgumentNoInput,
      Microsoft.PowerShell.Commands.RenameItemCommand
```

# 另见

[about_Functions](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_functions?view=powershell-6)

[about_Functions_Advanced](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_functions_advanced?view=powershell-6)

[about_Operators](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_operators?view=powershell-6)

