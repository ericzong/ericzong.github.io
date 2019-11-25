---
layout: post
title: PowerShell专题：注册表
category: PowerShell
tags: PowerShell 专题
excerpt: 介绍PowerShell管理注册表。
author: Eric Zong
---

* content
{:toc}

> 注册表对于 Windows 系统而言十分重要，因此，在执行每一条修改注册表的命令时都应该十分小心。
> 
> 除非你真的知道你在做什么，否则，不应该执行任何修改注册表的相关命令。

# 命令

先来看命令，PowerShell 中操作注册表的常用命令有如下这些：

| 命令                  | 描述                   |
| --------------------- | ---------------------- |
| `HKCU:`, `HKLM:`      | 预定义注册表虚拟驱动器 |
| `Get-ChildItem`/`dir` | 列出键                 |
| `Set-Location`/`cd`   | 导航                   |
| `New-Item`/`md`       | 新建键                 |
| `Get-Item`            | 获取键对象             |
| `Remove-Item`/`del`   | 删除键                 |
| `Test-Path`           | 测试键存在             |
| `New-ItemProperty`    | 新建键值               |
| `Set-ItemProperty`    | 设置键值               |
| `Get-ItemProperty`    | 获取键值               |
| `Clear-ItemProperty`  | 清除键值内容           |
| `Remove-ItemProperty` | 删除键值               |

注册表入口被预定义为提供器，或者说虚拟驱动器，因此，访问注册表很像是在访问文件系统（从以上命令也可以看出，大多是文件系统的命令），而注册表的键被视为文件夹，但值并非被视为文件，而是键的属性。

> 通过 `Get-PSProvider registry` 命令可以查看注册表的提供程序。
>
> 通过 `Get-PSDrive -PSProvider Registry` 命令可以查看注册表的虚拟驱动器。

还有其他一些与注册表相关的常用命令：

```powershell
# 搜索，支持 -recurse, -include, -exclude
Dir HKCU:, HKLM: -recurse -include PowerShell -ErrorAction SilentlyContinue
# 获取键（对象）下的值计数
$key.ValueCount
```

