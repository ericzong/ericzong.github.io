---
layout: post
title: PowerShell专题：创建快捷方式
category: PowerShell
tags: PowerShell 专题
excerpt: 介绍如何使用PowerShell创建快捷方式
author: Eric Zong
---

* content
{:toc}

# 概述

要创建快捷方式最简单的方式就是调用 WScript.Shell COM 组件。

# 脚本

```powershell
$shell = New-Object -ComObject WScript.Shell
$shortcut = $shell.CreateShortcut("path/to/shortcut.lnk") 
$shortcut.TargetPath = "path/to/target.exe"  # 设置快捷方式目标程序
$shortcut.Arguments =  "some arguments"  # 设置快捷方式参数
$shortcut.WorkingDirectory = "working/dir"  # 设置“起始位置”属性，即工作目录
$shortcut.IconLocation = "path/to/icon/file,0"  # 快捷方式图标
$shortcut.Save()
```

# 附录

## 特殊目录路径获取

```powershell
# “桌面”路径获取
$desktop = [System.Environment]::GetFolderPath('Desktop')
```

## WScript.Shell组件属性

| 属性             | 说明                                                      |
| ---------------- | --------------------------------------------------------- |
| Arguments        | 参数，追加到“TargetPath”后合并为“目标”属性值              |
| Description      | 备注                                                      |
| Hotkey           | 快捷键                                                    |
| IconLocation     | 图标位置                                                  |
| TargetPath       | 目标路径，在“Arguments”前（如果有），一起作为“目标”属性值 |
| WindowStyle      | 运行方式，即窗口样式                                      |
| WorkingDirectory | 起始位置，即工作目录                                      |

