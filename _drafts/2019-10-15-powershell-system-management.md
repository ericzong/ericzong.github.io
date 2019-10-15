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

