---
layout: post
title: "PowerShell入门：文件系统操作"
category: PowerShell
tags: PowerShell 入门
excerpt: "PowerShell入门教程，介绍文件相关操作。"
---

* content
{:toc}
# 概述

脚本与系统交互过程中，最为频繁的是与文件系统的交互。作为一个管理工具，PowerShell 提供了很多命令来完成这些操作。

# 创建

## 目录

```powershell
mkdir "dir-path-or-name"
New-Item "dir-name" -type Directory
```

> PowerShell 的命令名称通常是比较冗长的，为了兼容常用的操作命令，PowerShell 内置了很多别名。比如，创建目录我们更常用的是别名 `md`。

## 文件

PowerShell 中创建空白文件不需要借助重定向等，有专门的命令（虽然有点长）：

```powershell
New-Item "filename" -type File
```

要将内容输出到新文件有以下 3 种方式：

```powershell
dir > redirect-file.txt
dir | Out-File out-file.txt
dir | Set-Content set-content-file.txt
```

需要注意它们输出的差异，重定向与 `Out-File` 命令输出是一致的，但是 `Set-Content` 命令不自动转换对象为文本，而是抽取一个标准属性，因此，输出可能不同与前两者。

追加内容到文件有 2 种方式：

```powershell
Add-Content append.txt "Second line" -encoding UTF8
"Third line" >> append.txt
```

> 重定向命令输出字符集跟控制台编码有关，可能存在问题，不建议使用。应使用其他命令并指定 `-encoding`。

## 驱动器

PowerShell 支持将普通目录虚拟为驱动器：

```powershell
# 为“桌面”创建驱动器
New-PSDrive desktop FileSystem `
([Environment]::GetFolderPath("Desktop")) | out-null
cd desktop:
# 为“我的文档”创建驱动器
New-PSDrive docs FileSystem `
([Environment]::GetFolderPath("MyDocuments")) | out-null
# 删除驱动器
Remove-PSDrive desktop
```

> 注意，创建的驱动器只是临时的，重启会话就会丢失，除非写入启动脚本中。

# 读取

## 文本文件

下面 2 种方式可以一次性将文本文件的全部内容读取：

```powershell
Get-Content $file
# 变量符号快捷方式，其中不能包含变量
${absolute-file-path}
```

> 变量符号快捷方式读入字符集跟控制台编码有关，可能存在问题，不建议使用。而使用 `Get-Content` 命令时应指定 `-encoding`。
>
> 另外，尽量使用 `Get-Content` 自带的过滤器，不应通过管道来过滤。
>
> 请仔细阅读 `Get-Content` 的帮助文档。

## CSV

PowerShell 对 CSV 格式的文件提供直接的命令支持，导出和导入命令分别为 `Export-CSV` 和 `Import-CSV`。

> 请仔细阅读相关帮助文档。

# 修改和删除

```powershell
Copy-Item "from" "to" [-recurse]
Move-Item "from" "to" [-recurse]
Rename-Item "from" "to" 
Remove-Item "fileOrDir" [-force] [-confirm] [-recurse]
```

# 其他常见操作

## 路径处理

```powershell
# 相对路径转绝对路径
Resolve-Path $relativePath

# 路径拼接
# 仅能拼接2个路径，否则需要适当嵌套
Join-Path $path $subPath
# 可拼接多个路径
[Io.Path]::combine(string[])
```

## 路径检测判断

```powershell
# 路径存在检测
Test-Path $path [-PathType <Container | Leaf>]
[System.Io.Directory]::exists($dir)
[System.Io.File]::exists($file)

# 区分文件与文件夹
# -is 判断对象类型
($path) -is [Io.DirectoryInfo | Io.FileInfo]
# 文件 - System.Io.FileInfo
# 目录 - System.Io.DirectoryInfo
($path).GetType().FullName
```

# 杂项

路径中特殊符号需转义，如：`` `[ `` 和 `` `] ``

