---
layout: post
title: "PowerShell专题：函数与脚本"
category: PowerShell
tags: PowerShell 专题
excerpt: "PowerShell函数与脚本专题讨论。"
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

