---
layout: post
title: PowerShell专题：系统管理（持续更新……）
category: PowerShell
tags: PowerShell 专题
excerpt: 介绍PowerShell与系统管理相关的命令。
author: Eric Zong
---

* content
{:toc}
# SMB

```powershell
# SMB v1
Get-WindowsOptionalFeature –Online –FeatureName SMB1Protocol
Disable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol
Enable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol
# SMB v2/v3
Get-SmbServerConfiguration | Select EnableSMB2Protocol
Set-SmbServerConfiguration –EnableSMB2Protocol $false
Set-SmbServerConfiguration –EnableSMB2Protocol $true
```

# 回收站

## 清空回收站

```powershell
Clear-RecycleBin [[-DriveLetter] <string[]>]  [<CommonParameters>]
```

注意：默认情况下，会询问确定执行。如果想直接执行可以使用 `-Force` 或者 `-confirm $false`。

## 删除文件到回收站

```powershell
$shell = new-object -comobject 'Shell.Application'
$item = $shell.NameSpace(0).ParseName((Resolve-Path 'xxx').Path) # 绝对路径
$item.InvokeVerb('delete')
```

# 驱动器

```powershell
Get-PSDrive -PSProvider FileSystem
```

注意：如果不指定任何选项，将列出所有 PS 驱动器，这包括注册表、环境变量等等。

