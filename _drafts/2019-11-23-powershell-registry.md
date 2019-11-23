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

# 命令

| 命令                  | 描述                   |
| --------------------- | ---------------------- |
| `HKCU:`, `HKLM:`      | 预定义注册表虚拟驱动器 |
| `Get-ChildItem`/`dir` | 列出键                 |
| `Set-Location`/`cd`   | 导航                   |
| `New-Item`/`md`       | 新建键                 |
| `Remove-Item`/`del`   | 删除键                 |
| `Test-Path`           | 测试键存在             |
| `New-ItemProperty`    | 新建键值               |
| `Set-ItemProperty`    | 设置键值               |
| `Get-ItemProperty`    | 获取键值               |
| `Clear-ItemProperty`  | 清除键值内容           |
| `Remove-ItemProperty` | 删除键值               |

