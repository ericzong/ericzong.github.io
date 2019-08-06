---
layout: post
title: PowerShell专题：操作环境变量
category: PowerShell
tags: PowerShell 专题
excerpt: 介绍使用PowerShell如何操作环境变量。
author: Eric Zong
---

* content
{:toc}
# 环境提供器

在 PowerShell 中，使用环境提供器（Environment provider）来操作环境变量，而环境提供器通过 `Env:` 驱动来暴露数据。

很显然，由于 `Env:` 是一个 PowerShell 虚拟驱动器，所以几乎所有可用于虚拟驱动器的命令都可以应用其上：

```powershell
# 导航到驱动器路径下
Set-Location Env:
# 导航到其下后，可查看当前位置
Get-Location
# 列出当前会话所有环境变量
Get-Item [-Path] Env:
Get-ChildItem [-Path] Env:
# 查看某个环境变量
Get-ChildItem [-Path] Env:Path
# 创建环境变量
New-Item -Path Env: -Name Tester -Value Eric
# 重命令环境变量
Rename-Item -Path Env:Tester -NewName Dev
# 修改环境变量值
Set-Item -Path Env:Dev -Value zonglu
# 通过清除环境变量值删除环境变量
Clear-Item -Path Env:Dev
# 删除环境变量
Remove-Item -Path Env:Dev
```

访问或修改环境变量，也可以不使用命令，这需要使用一个变量前缀 `$env:`：

```powershell
$Env:<variable-name>
$Env:<variable-name> += "<new-value>"
```

# 环境变量持久化

应该注意，上文所有操作仅对当前会话有效，不会持久化。

如果需要对环境变量的操作持久化，需要修改 PowerShell 的启动文件，比如：

```powershell
Add-Content -Path $Profile.CurrentUserAllHosts -Value '$Env:Path += ";C:\Temp"'
```

不过，这仅仅是对 PowerShell 生效，其他终端或程序不会被影响，如果想要全局生效需要用到 .Net Framework 的 `System.Environment` 类，语法如下：

```powershell
[System.Environment]::GetEnvironmentVariable(String variableName, EnvironmentVariableTarget)
[System.Environment]::SetEnvironmentVariable(String variableName, String variableValue, EnvironmentVariableTarget)
```

`EnvironmentVariableTarget` 是指环境变量持久化的范围，可以是 `Process`（进程级）、`User`（用户级）、`Machine`（系统级）。

> 可以在 [这里](https://docs.microsoft.com/en-us/dotnet/api/system.environment) 参考 .Net Framework 相关文档。

示例：

```powershell
[Environment]::SetEnvironmentVariable("myVar", "Eric", "User")
[System.Environment]::GetEnvironmentVariable("myVar", "User")
# 通过清除环境变量值删除环境变量
[System.Environment]::SetEnvironmentVariable("myVar", $null, "User")
```

# 注意事项

## 持久化污染

当需要持久化环境变量时，需要注意，`$Env:Path` 混合了用户路径和系统路径。不同用户执行同一命令回写系统路径时，存在污染的风险。

要纯粹获取和设置系统路径可使用以下方法：

```powershell
# 注册表获取和设置
(Get-Itemproperty -path 'hklm:\system\currentcontrolset\control\session manager\environment' -Name Path).Path
Set-ItemProperty -path 'hklm:\system\currentcontrolset\control\session manager\environment' -Name Path -Value $NewPath
# .Net Framework 获取和设置
[System.Environment]::GetEnvironmentVariable("Path", "Machine")
[System.Environment]::SetEnvironmentVariable("Path", $NewPath, "Machine")
```

## 环境变量名包含句点或括号

环境变量名可能包含句点 `.` 或括号 `()` ，当使用 `$Env:<variable>` 访问环境变量时会出现错误，此时，应该使用 `${Env:<variable>}` 方法访问：

```powershell
${Env:ProgramFiles(x86)}
```

## 使用`$Env:`还是`Env:`

在命令中，可能会发现有时使用 `$Env:<variable>` 有时使用 `Env:<variable>`。那什么时候使用前者，什么时候使用后者呢？（这一度让我困惑。）

事实上，它们的语义是不同的。

`Env:<variable>` 表示的是一个虚拟路径，跟 `C:/Windows` 在形式上没有什么不同。使用时关注的是其路径形式而非其代表的值，因此它通常用于操作路径的命令，大多数时候作为 `-Path` 参数值。

`$Env:<variable>` 开头的 `$` 暗示了它是一个变量，虽然同时 `Env:` 暗示它是一个环境变量，但本质上与普通变量并无不同，使用时关注的是其对应的值。命令中将其置换为它引用的值是等价的。

