---
layout: post
title: "PowerShell专题：脚本块"
category: PowerShell
tags: PowerShell 专题
excerpt: "PowerShell脚本块专题"
author: "Eric Zong"
---

* content
{:toc}

# 概述

脚本块是一个语句或表达式的集合，可作为独立单元使用。

脚本块位于一对花括号中，将作为整体执行。

跟函数很类似，函数常被当作命名的脚本块。因此，脚本块自然可以接受参数——只是没有名称，参数不能定义在花括号外——也可以有返回值。

语法如下：

```powershell
{
Param([type]$Parameter1 [,[type]$Parameter2])
<statement list>
}
```

# 执行、传参和返回值

脚本块最简单的执行方式是使用 `&` 操作符，参数直接跟在脚本块后，如：

```powershell
& {param($who="world") Write-Host "Hello, $who" } Eric
```

如果脚本块带有返回值，可将调用结果赋值给变量：

```powershell
$value = & {return 42}
$value # 42
```

# 常见使用

通常，为了复用或传递等原因，脚本块常被赋值给变量进行使用。

```powershell
$add = {
    param($one, $two)

    "$one + $two = $($one + $two)"
}
&$add -two 3 -one 4 # 4 + 3 = 7
```

当然，也可以使用 `Invoke-Command` cmdlet 来执行脚本块，比如上面的脚本块可以这样执行：

```powershell
Invoke-Command -ScriptBlock $add -ArgumentList 4,3
```

# 处理管道对象

同样，脚本块也可以处理管道对象，比如：

```powershell
Get-Process | Select -last 5 | & {
    Begin {
        "Start..."
    }
    Process
    {
        $_.Name
    }
    End
    {
        "End."
    }
}
```

# 参考

[MS PS Doc - About Script Blocks](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_script_blocks?view=powershell-6)