---
layout: post
title: "PowerSehll扩展"
category: PowerShell
tags: PowerShell
excerpt: "PowerShell扩展工具介绍"
---

* content
{:toc}

# posh-git

posh-git 是一个 PowerShell 模块，为 Git 提供状态统计信息以及 tab 提示等功能。

笔者推荐通过 scoop 安装 posh-git（位于 extras bucket）。

## 启用

posh-git 安装成功后需要使用 `Import-Module posh-git` 命令来导入其模块使用。

如果不想每次打开 PowerShell 时都手动执行导入语句，可以把相关配置写入 profile 脚本——使用命令或手动编写 profile 文件。

| 命令                                       | profile 位置                    | 范围             |
| ------------------------------------------ | ------------------------------- | ---------------- |
| `Add-PoshGitToProfile`                     | $profile.CurrentUserCurrentHost | 当前用户当前主机 |
| `Add-PoshGitToProfile -AllHosts`           | $profile.CurrentUserAllHosts    | 当前用户所有主机 |
| `Add-PoshGitToProfile -AllUsers`           | $profile.AllUsersCurrentHost    | 所有用户当前主机 |
| `Add-PoshGitToProfile -AllUsers -AllHosts` | $profile.AllUsersAllHosts       | 所有用户所有主机 |

## 自定义

启用 posh-git 后，Git 统计信息会在命令提示符（prompt）中动态显示，当然，我们也可以自定义命令提示符。

比如笔者工作路径通常很深，因此，使用如下配置，使得命令提示符中仅显示活动目录名，而不是其绝对路径。

```powershell
Function getPath
{	
	$drive = $(Split-Path (Get-Location) -qualifier)
	$parent = $(split-Path (get-location) -parent)
	if($parent) {
		$prompt = $(split-Path (get-location) -leaf)
	} else {
		$prompt = ''
	}
	
	$fullPrompt = $drive + '* ' + $prompt
	
    return $fullPrompt
}

function prompt {
    $origLastExitCode = $LASTEXITCODE

    $prompt = ""

    $prompt += Write-Prompt (getPath)
    $prompt += Write-VcsStatus
    $prompt += Write-Prompt "$(if ($PsDebugContext) {' [DBG]: '} else {''})" -ForegroundColor Magenta
    $prompt += "$('>' * ($nestedPromptLevel + 1)) "

    $LASTEXITCODE = $origLastExitCode
    $prompt
}

Import-Module posh-git
```

## 参考

[GitHub posh-git](https://github.com/dahlbyk/posh-git)