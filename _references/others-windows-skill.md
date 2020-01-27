---
layout: page
title: Windows应用技巧
category: 其他
date: 2020-01-27
author: Eric Zong
---

* content
{:toc}
# 系统配置

## 启动项

| 启动项名称       | 位置                                                         |
| ---------------- | ------------------------------------------------------------ |
| 系统             | `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run`         |
| 系统32位         | `HKLM\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Run` |
| 用户             | `HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run`         |
| 当前用户         | `%UserProfile%\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup` |
| 本机             | `HKLM\SYSTEM\CurrentControlSet\Control\Session Manager`      |
| 新用户           | `HKU\DEFAULT\SOFTWARE\Microsoft\Windows\CurrentVersion\Run`  |
| 新用户（仅一次） | `HKU\DEFAULT\SOFTWARE\Mirosoft\Windows\CurrentVersion\RunOnce` |

