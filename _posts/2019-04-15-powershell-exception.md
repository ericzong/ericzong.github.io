---
layout: post
title: "PowerShell 异常处理"
categories: PowerShell
tags: 脚本语言 PowerShell
excerpt: "介绍 PowerShell 异常及其处理。"
author: "Eric Zong"
---

* content
{:toc}

# 概述

PowerShell 有两种异常类型，终止异常和非终止异常。

* 终止异常（Terminating Errors）：不由命令本身发出的错误。通常与安全相关。
* 非终止异常（Non-Terminating Errors）：来自命令内部的错误。

PowerShell 有两种异常处理机制，Trap 和 try-catch。

需要特别注意的是，Trap/try-catch 只能捕获到终止异常，不能捕获到非终止异常。

如果想要捕获非终止异常，需要将非终止异常转换为终止异常，进而捕获。有两种转换方式：

* `-ErrorAction Stop`。通常命令都支持 `ErrorAction` 选项，将其设置为 `Stop` 就能将产生的非终止异常转换为终止异常。
* `$ErrorActionPreference='Stop'`。如果命令没有指定 `ErrorAction` 选项，可以设置 `$ErrorActionPreference` 以指定后续所有命令的默认异常行为。

> 初次见到 PowerShell 的异常系统时或许会感到奇怪，跟其他主流编程语言不太相同，默认情况下异常处理机制居然不能捕获到异常！不过，如果意识到 PowerShell 是脚本语言的话也就可以理解了。脚本侧重批处理能力，而不是交互能力，因此，出现错误能继续运行而不需要用户干预更重要。故普通命令异常不会阻止脚本执行理应是默认行为。

# Trap

Trap 相当于一个“全局”的异常处理器，它处理终止异常。

```powershell
Trap [ExceptionType] {
	# 异常处理程序
}
```

Trap 后可以指定具体处理的异常类型（用方括号括起来），也可以不指定，处理所有异常。

```powershell
function trapDefault
{
    Trap {"Get Exception"}
    1/$null	# 异常 1
    Get-Process "NoSuchProcess" # 异常 2
    Dir Miss: # 异常 3
}
```

上面的函数中，除 `1/$null` 抛出终止异常外，其他均为非终止异常，因此，Trap 只捕获到一次异常，故结果是：

```
Get Exception
异常 1 堆栈
异常 2 堆栈
异常 3 堆栈
```

## Continue & Break

默认情况下会打印异常堆栈，如果不想打印异常堆栈，可使用 Continue：

```powershell
Trap {
    "Get Exception: $($_.Exception.Message)"
    Continue
}
```

注意：Continue 不会阻止脚本的后续执行，但是，如果捕获的异常是从调用的函数中抛出的，那么，处理该函数的第一个异常后，该函数的执行结束，但脚本继续执行。

如果想要处理异常后终止执行，可使用 Break：

```powershell
Trap {
    "Get Exception: $($_.Exception.Message)"
    Break
}
```

注意：Break 中断脚本执行，即使异常从调用函数中抛出，当前脚本的执行也会终止。

# try ... catch ...

```powershell
try {
  dir "nothing" -ErrorAction Stop
  Write-Host "no executed"
} catch {
  Write-Warning "Error: $_"
  Write-Host $_.getType().fullname
} finally {
  Write-Host "Always run"
}

Write-Host "will be executed"
```

# 获取异常信息

## 重定向

```powershell
function redirectToFile
{
    dir "NoSuchDirectory" 2> Error.txt # 重定向错误流，标准流中没有异常信息
    Get-Content Error.txt
}

function redirectToVar
{
    $myError = dir "NoSuchDirectory" 2>&1
    $myError.Exception
    $myError.InvocationInfo
    $myerror.FullyQualifiedErrorId
}
```

## 异常变量参数

```powershell
function errorVariable
{
    Remove-Item "NoSuchDirectory" -ErrorVariable ErrorStore -ErrorAction "SilentlyContinue"
    $ErrorStore
    $ErrorStore.GetType().fullName
    $ErrorStore[0].getType().fullName
    $ErrorStore[0].Exception.Message
}
```

当然，这要求命令支持 `ErrorVariable` 参数。

注意，不受 `ErrorAction` 影响。

## \$Error

PowerShell 自动收集异常到 `$Error` 中，它是一个数组，最新的异常保存于索引 0 处。

`$Error` 保存的最大异常数于 `$MaximumErrorCount` 定义。

可调用 `$Error.Clear()` 清空该数组。

```powershell
$Error[0].Exception.Message
$Error.Clear()
```

## Trap/catch 中的异常变量

Trap 或 catch 块中可以使用 `$_` 或 `$PSItem` 访问异常记录。

# 抛出异常

使用 `Throw ` 抛出异常，通常可以抛出任何东西，一般指定一个异常信息字符串。

```powershell
# 必需参数缺省异常信息定义
function throwException($a, $b=$(throw "required"))
{
    
}

function throwException2
{
    Param(
        $a,
        $b=$(throw "required")
    )
    $a + $b
}
```

# 异常判断

`$?` bool 值，保存上一操作是否异常。

# 显示风格

```powershell
# 异常信息颜色
$host.PrivateData.ErrorForegroundColor # Red
$host.PrivateData.ErrorBackgroundColor # Black
# 异常信息输出设置
$ErrorView # 默认值 NormalView
$ErrorView='categoryview' # 仅显式一行异常信息
```

# 附录

## ErrorAction 选项

| 候选值           | 说明                                                         |
| ---------------- | ------------------------------------------------------------ |
| Continue         | 默认值，显示错误信息并继续执行                               |
| Ignore           | 3.0+，不显示错误信息并继续执行。与 SilentlyContinue 不同，它不会添加异常记录到 `$Error` |
| Inquire          | 显示错误信息并弹框与用户交互                                 |
| SilentlyContinue | 不显示错误信息并继续执行                                     |
| Stop             | 显示错误信息并退出脚本执行                                   |
| Suspend          | 仅用于 workflow。当终止异常发生时执行会暂停，决定是否恢复执行 |

## 参考

[PSTips - Powershell 在函数中捕获异常](https://www.pstips.net/powershell-trap-error-in-function.html)

[PSTips - Powershell 错误记录:详细错误](https://www.pstips.net/powershell-error-record-details.html)