---
layout: post
title: Windows 注册表的应用
date: 2020-02-29 14:00:00
category: Windows
tags: 系统 持续更新
excerpt: 介绍 Windows 注册表的应用技巧。
author: "Eric Zong"
---

* content
{:toc}
# 右键菜单

文件夹右键菜单注册表项位于：`HKEY_CLASSES_ROOT\Directory\Shell\`。

* `...\xxx`：键。默认情况下，它就是文件夹右键菜单项显示的文本。
* `...\xxx\(默认)`：String 值。默认为空，此时菜单项显示为键名称；否则，其值为菜单项显示文本，可以包含中文。
* `...\xxx\Icon`：String 值。菜单项的图标，通常设置为其执行程序的路径，即使用该程序的图标。
* `...\xxx\command`：键。配置菜单项的执行命令。
* `...\xxx\command\(默认)`：String 值。菜单项执行命令的具体内容，通常为某个可执行程序路径后跟参数列表。`%1` 通常表示菜单项关联的文件夹。注意：可执行程序路径以及 `%1`，最好都用 `"`（双引号）包裹起来。
* `...\xxx\MultiSelectMode`：String 值。它用以限制文件夹的选择方式，候选值有 `Player`（默认值，无论单选或多选文件夹都会显示菜单项）/ `Single`（仅单选时显示菜单项）。

