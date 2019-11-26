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

# 命令列表

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

# 注册表值类型

| 值类型       | 描述                                     | 数据类型      |
| ------------ | ---------------------------------------- | ------------- |
| String       | 字符串                                   | REG_SZ        |
| ExpandString | 包含环境变量的字符串在执行时可以自动处理 | REG_EXPAND_SZ |
| MultiString  | 多行文本                                 | REG_MULTI_SZ  |
| Binary       | 二进制值                                 | REG_BINARY    |
| DWord        | 32 位数值                                | REG_DWORD     |
| QWord        | 64 位数值                                | REG_QWORD     |

> 值类型可用于：
>
> * `New-Item` 命令， `-itemType` 选项候选值
>
> * `New-ItemProperty` 命令，`-propertyType` 选项候选值

# 注意事项

* 如果键包含空格，需用引号引起来。
* 不能如文件系统一次性创建多层文件夹一样创建多层键，即创建一个新键需要确保其父键存在。
* `Get-ItemProperty` 会获取到键值外的其他属性，因此，要获取某个值需要特别指定，如：`(Get-ItemProperty HKCU:\Software\Test).T1`。
* PowerShell 不能删除默认值，只能使用 `Clear-ItemProperty` 清除其内容。

# 示例

```powershell
# 搜索，支持 -recurse, -include, -exclude
Dir HKCU:, HKLM: -recurse -include PowerShell -ErrorAction SilentlyContinue
# 获取键对象
$key = Get-Item HKLM:\Software\Microsoft\PowerShell
# 获取键下的值计数
$key.ValueCount
# 创建新键
New-Item HKCU:\Software\Test
# 创建有默认值的键
New-Item -itemType String HKCU:\Software\Test -value 这是默认值
# 创建值
New-ItemProperty HKCU:\Software\Test -name T1 -value 默认值 -propertyType String
# 创建/修改值
Set-ItemProperty HKCU:\Software\Test -name '(default)' -value 设置值
# 删除值
Remove-ItemProperty HKCU:\Software\Test T1
# 清除默认值
Clear-ItemProperty HKCU:\Software\Test 'default'
# 创建键并添加值（仅新键可写，Get-Item 得到的键是只读的）
$newKey = mk HKCU:\Software\Test2
$newKey.SetValue("T1", "test1", "String")
# 删除非空键
Del "HKCU:\Software\Test Key" -recurse
```

