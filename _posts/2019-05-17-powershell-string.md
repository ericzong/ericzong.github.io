---
layout: post
title: "PowerShell入门：字符串"
category: PowerShell
tags: PowerShell 入门
excerpt: "PowerShell入门教程，字符串相关。"
---

* content
{:toc}

# 概述

PowerShell 字符串定界符可以是单引号或双引号。

> 这跟 JS 有点类似，可以对比理解。

最简单的字符串是单引号字符串，它按原样输出。

# 双引号字符串

与单引号字符串不同的是，双引号字符串会尝试解析（或称“展开”）字符串中的特殊内容，比如：变量、转义字符等。

```powershell
$num=42
"`tnumber is $num"
# 输出：
	number is 42
```

> 注意 PowerShell 的转义前缀是 <code>`</code>。

## 变量解析

双引号字符串中，通常以 `$variable` 的形式引用变量。但变量名必须是明确的，即变量名之后应有明显分隔。这是指变量名后应该紧跟：字符串结束、空白字符、变量前缀 `$`、转义前缀 <code>`</code>、标点等等。

另外，可将变量名置于一对花括号中以明确变量名，如：`${variable}`。通常当变量名中有空格或是后续紧跟其他普通字符等情况下会用到该种表示法。

```powershell
"My name is ${my name}"
"My number is ${num}teen"
```

更进一步，在双引号字符串中表达式也可以被解析，其语法是：`$(expression)`。

```powershell
"当前时间：$(get-date)"
```

小结下，如果想要双引号字符串解析替换一个变量，大致有以下 4 种写法：

```powershell
"$env:windir"
"${env:windir}"
"$($env:windir)"
"$(${env:windir})"
```

> 本质上是 2 种写法，变量表示和表达式表示。

## 属性访问

单纯的字符串变量解析，不会进一步解析其属性访问，比如：

```powershell
"$host.UI.RawUI.BackgroundColor"
```

其输出为 `System.Management.Automation.Internal.Host.InternalHost.UI.RawUI.BackgroundColor`。

字符串原意是想表示访问 `$host` 对象的属性，但结果只是把 `$host` 转换为了其字符串表示形式，属性访问被原样输出了。正确的属性访问语法需要使用 `$(` 和 `)` 将属性访问括起来，像下面这样：

```powershell
"$($host.UI.RawUI.BackgroundColor)"	
```

# 多行字符串

PowerShell 还有一种称为“**here-string**”的多行字符串表示法，语法如下：

```powershell
@"
这里是多行文本
可以自由换行
"@
```

当然，定界符可以用单引号也可以用双引号。

通常，`@"` 和 `"@` 都应独占一行。

> 严格来讲，并没有限制它们必须独占一行，只是这样书写更清晰。
>
> 比如，`@"` 可以跟在赋值等号之后；而 `"@` 之后也可以跟 `+` 拼接字符串等等。
>
> 真正的限制是：
>
> * `@"` 紧随其后直至行末都不能有非空白字符。不过空白字符是允许的，只是会被忽略。
>
> * `"@` 之前不能有任何字符，即使是空白字符也不允许。
>
> 事实上，普通字符串也可以内部换行，只是定界符中所有的空白符都会被输出。而多行字符串会忽略 `@"` 后的空白符（包括换行）以及 `"@` 前的最后一个换行。

# 操作符

除了字符串连接符 `+` 外，字符串操作符通常是命令行选项的形式，比如：`-f`、`-replace`、`-eq`、`-like`、`-match`……

默认字符串操作是大小写不敏感的，当然有的操作符带有 `i`（insensitive）前缀强调，如：`-ireplace`、`-ieq` 等。而带有 `c`（case sensitive）前缀的操作符是大小写敏感的。

# 常见操作

```powershell
# 字符串判空
[String]::IsNullOrEmpty($str)
```

# 显式扩展字符串

实际编写脚本时可能遇到这种情况，字符串是由单引号字符串，或者说是外部传入的，因此，理应按原样输出，但是，我们却想要解析其中的变量或转义字符。这时，我们可以使用如下方法展开字符串：

```powershell
$ExecutionContext.InvokeCommand.ExpandString($input)
```

# 参考

## 字符串操作符

| 操作符               | 描述                                             | 示例                                      |
| -------------------- | ------------------------------------------------ | ----------------------------------------- |
| *                    | 通配符                                           | "PowerShell" -like "*"                    |
| +                    | 字符串连接符                                     | "Power" + "Shell"                         |
| -replace/-ireplace   | 替换字符串，大小写不敏感                         | "PowerScript" -replace "script", "Shell"  |
| -creplace            | 替换字符串，大小写敏感                           | "PowerScript" -creplace "Script", "Shell" |
| -eq, -ieq            | 验证是否相等，大小写不敏感                       | "Power" -eq "power"                       |
| -ceq                 | 验证是否相等，大小写敏感                         | "Power" -ceq "Power"                      |
| -like/-ilike         | 验证模式匹配，大小写不敏感                       | "Power" -like "p*"                        |
| -clike               | 验证模式匹配，大小写敏感                         | "Power" -clike "P*"                       |
| -notlike/-inotlike   | 验证模式不匹配，大小写不敏感                     | "PowerShell" -notlike "PS*"               |
| -cnotlike            | 验证模式不匹配，大小写敏感                       | "PowerShell" -cnotlike "p*"               |
| -match/-imatch       | 验证字符串包含关系，允许模式匹配，大小写不敏感   | "PowerShell" -match "p"                   |
| -cmatch              | 验证字符串包含关系，允许模式匹配，大小写敏感     | "PowerShell" -cmatch "P"                  |
| -notmatch/-inotmatch | 验证字符串不包含关系，允许模式匹配，大小写不敏感 | "PowerShell" -notmatch "ps"               |
| -cnotmatch           | 验证字符串不包含关系，允许模式匹配，大小写敏感   | "PowerShell" -cnotmatch "po"              |

## 链接

[Powershell 定义文本](https://www.pstips.net/powershell-defining-text.html)

[字符串操作符](https://www.pstips.net/string-operators.html)