---
layout: post
title: "PowerShell入门：变量"
category: PowerShell
tags: PowerShell 入门
excerpt: "PowerShell入门教程，介绍变量相关知识。"
---

* content
{:toc}

# 概述

PowerShell 变量语法为：`$variable` 或 `${variable}`。

变量类型是宽松的，非必要可不声明类型。类型声明形式为：`[int]$number=42`。

变量名大小写不敏感，可包含空格和特殊字符。

> 当变量名包含空格或特殊字符时，需要用花括号将变量名包围，如：`${my var}`，甚至需要对特殊字符进行转义，`` ${my`{var`}} ``。
>
> 鉴于难以使用，所以通常不会这样命名变量。

变量不必事先声明，默认值为 `$null`。

> 可参考 [这里](/powershell/2019/05/08/powershell-get-start.html#%E5%8F%98%E9%87%8F)。
>
> 变量类型可参考 [这里](/references/powershell-reference.html#%E5%8F%98%E9%87%8F%E7%B1%BB%E5%9E%8B)。

# 常规操作

```powershell
# 创建变量
New-Variable -Name number -Value 42
$number = 42
# 清楚变量值
Clear-Variable -Name number
$number = $null
# 修改变量值
Set-Variable -Name number 43
$number = 43
# 删除变量
Remove-Variable -Name number
Remove-Item -Path Variable:\number
# 位置转换到变量驱动器
Set-Location Variable:
# 查看所有变量
Get-Variable
Get-ChildItem Variable:
```

# 变量类型

PowerShell 变量分为 3 种：

* 用户创建的变量（user-created variable），由用户创建并维护。
* 自动变量（automatic variable），PowerShell 自行创建，存储自身状态。
* 偏好变量（preference variable），PowerShell 创建，存储用户偏好，可配置。

## 自动变量

PowerShell 中自动变量非常多，这里挑几个常用的说说。

`$?`，记录上次操作成功与否（布尔值）。常用于判断操作是否出现异常。

`$_`，等价于 `$PSItem`，代表当前对象，通常用于管道、循环等中。

`$args`，数组，包含所有未声明的参数。

`$PSBoundParameters`，字典，包含所有声明参数的“键-值”对。

`$Error`，数组，包含最近的异常。

> 注意，`$true`、`$false`、`$null` 都是自动变量。

## 偏好变量

也有很多偏好变量，最常见看的应该是 `$ErrorActionPreference`，它控制默认的非终端异常的行为。

# 作用域

默认 PowerShell 解释器会自动处理和限制变量作用域。

# 参考

## 链接

[MS Doc - About Variables](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_variables?view=powershell-6)

[MS Doc - About Automatic Variables](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_automatic_variables?view=powershell-6)

[MS Doc - About Preference Variables](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_preference_variables?view=powershell-6)

[MS Doc - About Scopes](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_scopes?view=powershell-6)