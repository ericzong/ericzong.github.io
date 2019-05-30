---
layout: page
title: "《Windows PowerShell实战指南（第2版）》"
category: 脚本语言
author: "Eric Zong"
---

* content
{:toc}

# 第1章

## 1.5 

版本查看：`$PSVersionTable`。

依赖：.Net Framework

配套：Windows 远程管理服务（WinRM）。



* 基于文本的标准控制台（PowerShell.exe）
* 集成了命令行环境的图形化界面（ISE; PowerShell_ISE.exe）

```powershell
Add-WindowsFeaturePowerShell -ise
```

推荐控制台字体：Lucida（固定宽度）

# 第2章

## 2.2 

Tab 补全：Tab & Shift + Tab 可循环

1. 补全命令
2. 补全命令参数名
3. 补全命令参数候选值
4. 补全路径

## 2.4 如何查看当前版本

每个发布版都安装在 1.0 目录下面，1.0 是引用的 shell 引擎语言版本，即所有版本都向后兼容到 v1。

## 2.5 

PS 针对几乎所有的问题都能提供几种甚至更多方式实现。

# 第3章 使用帮助系统

PS，命令行接口-CLIs（command-line interfaces），通常缺乏可发现性特性，但 PS 有出色的可发现性，只是不明显。

## 3.1 

RTFM, Read The Friendly Manual

命令：Cmdlet、函数、工作流……

## 3.2 可更新帮助

```powershell
Update-Help [-SourcePath <String[]>]
Get-Help Get-Service -Online
Save-Help
```

## 3.4

cmdlet 命名格式：动词-名词

在 Get-Help 中指定带通配符的命令时，如果找到多个则以列表形式列出，否则直接显示唯一命令的帮助。

```powershell
Get-Command [-noun | -verb ] [-CommandType...]
Gcm 
```

命令帮助中，语法部分命令可出现多次，说明有多个参数集。只能选用其中一个，仅提供共享参数时使用首个。

## 3.5

### 3.5.3 定位参数

最佳实践：一直使用参数名，直至熟悉并厌倦。

参数名仅在命令行中使用缩写，不在脚本中使用缩写。

### 3.5.4 参数值

开关参数，无需任何输入值，Switch Parameter

最佳实践：字符串最好坚持使用单引号。

字符串数组 `<string[]>` => 逗号分隔列表 a, b, c （整体不需要引号）

### 3.5.5 发现命令示例

```powershell
Help Get-EventLog -example
```

## 3.6 

```
Help *common*
```

风险缓解参数 risk mitigation parameters

* -WhatIf (wi)
* -Confirm (cf)

# 第4章 运行命令

## 4.2 剖析一个命令

Command        Parameter1             Parameter2                                    Parameter3

Get-EventLog -LogName Security -ComputerName WIN8, SERVER1 -Verbose

​                        参数名称  参数值                                多个参数值       开关参数（无值）

PS Cmdlet 总是以动词-名词形式命名。

## 4.3 Cmdlet 命名惯例

Cmdlet 是一个原生的 PS 命令行工具。

命令：

* Cmdlet：.Net 语言编写
* 函数：脚本语言编写
* 工作流：嵌入 PS 工作流执行系统的一类特殊函数
* 应用程序：外部

查看动词列表 `Get-Verb`

通过一致的命名规则以及有限的动词集合，猜测命令名称变为可能。

## 4.4 别名：命令的昵称

```powershell
get-alias -Definition "Get-Service"
help gsv
```

创建的别名生命周期只持续到当前 Shell 会话结束，因此，需要导出、导入别名。

## 4.5 使用快捷方式

参数别名，3 种，可能造成混淆

参数别名：

* 简化 参数名称
* 参数名称别名
* 定位参数

### 4.5.1 简化参数名

PS 不强制要求输入完整参数名，简化规则是必须输入足够字母让 PS 可识别。

> 至少应是最小可识别前缀，命令输入简化方式，为了可读性，避免脚本中使用。

### 4.5.2 参数名称别名

参数别名不在帮助文件或任何方便查阅处。

```powershell
(get-command get-eventlog | select -ExpandProperty parameters).ComputerName.aliases
# -ComputerName <=> -Cn
```

Tab 补全会展示别名参数，但没有任何信息说明其是别名以及其完整名称。

### 4.5.3 定位参数

```powershell
Get-ChildItem [[-Path]]
```

外层方括号表示参数可选，内层方括号表示定位参数。

> 完整帮助：`Get-Help <cmdlet-name> -Full`。

## 4.6 

```powershell
show-command get-eventlog
```

> 以 GUI 表单展示，以最佳实践方式输出命令

## 4.7 对扩展命令的支持

PS 在后台启动 Cmd.exe 执行扩展命令

```powershell
& $exe -h $host -u $user -p $password
  -s "name:$machine" -r $location
  -t $backupType
```

## 4.9

### 4.9.2

PS 大小写不敏感，但空格和破折号敏感。

## 4.10 动手实验

```powershell
# No.1
get-process
# No.2
Get-EventLog -LogName Application -Newest 100
# No.3
Get-Command -CommandType Cmdlet
# No.4
Get-Alias
gal
# No.5
New-Alias D Get-ChildItem
# No.6
Get-Service -Name M*
# No.7，条件：Win8+, Win Server 2012+
Get-NetFirewallRule
# No.8，条件：Win8+, Win Server 2012+
Get-NetFirewallRule -Direction Inbound
```

# 第5章 使用提供程序

## 5.1 什么是提供程序

PSProvider，本质是适配器。

`Get-PSProvider`

PS 支持的两种扩展方式：

* 模块
* 管理单元

```powershell
Get-Command -noun *Item*
```

## 5.2 FileSystem 的结构

Windows 文件系统对象：

* 磁盘驱动器
* 文件夹
* 文件

PSDrive 可不指向某个文件系统，比如：注册表
项（Item）：文件、文件夹……
每个项基本上都会存在对应的属性（ItemProperty）。

```powershell
Get-ItemProperty -Path Env:\PSModulePath
```

## 5.3 文件系统-其他数据存储的模板

如要查询名字中带有 */?，须使用 `-LiteralPath` 参数。

文件系统广泛相似性 => 其他形式数据源最佳模板。

## 5.4 

cd, change directory

set-Location

## 5.5 使用其他提供程序

HKEY_CURRENT_USER

```powershell
# 导航
Set-Location
# 查看子项
Get-ChildItem
# 设值
Set-ItemProperty -Path ... -PSPropert ... -Value ...
```

## 5.7 动手实验

```powershell
# No.1
set-Location HKCU:\...
set-itemproperty -path ... -PSPropert ... -Value ...
# No.2 
new-item ... -type file
# No.4
get-childItem -filter ...
get-childItem -exclude ...
get-childItem -include <string[]>
```

`-include` 有效条件（或）：

* 有 `-Recurse`
* `-Path` 指向目录内容 `...\*`



# 第6章 管道：连接命令

## 6.2 

```powershell
Get-Process
Ps
Get-Service
Gsv
Get-EventLog Security -newest 100
```

### 6.2.1 

```powershell
Get-Process | Export-CSV procs.csv
Import-CSV procs.csv
```

### 6.2.2

```
Export-CliXML
Import-CliXML
Compare-Object
Diff -ReferenceObject -DifferenceObject -Property
```

### 6.2.3

```powershell
Get-Process | Export-CliXML reference.xml
Diff -reference (Import-CliXML reference.xml)
    -difference (Get-Process) -property Name
```

## 6.3 

```powershell
Dir > DirectoryList.txt
Dir | Out-File DirectoryList.txt
```

Dir <=> Dir | Out-Default <=> Dir | Out-Default | Out-Host

```powershell
Help Out*
Get-Command Out*
Get-Command -verb Out
```

Out-Cmdlets：

```powershell
Out-Printer
# .Net Framework v3.5 or ISE
Out-GridView
Get-Service | Out-GridView
Out-Null
Out-String
```

## 6.4 转换成 HTML

```powershell
Get-Service | ConvertTo-HTML
```

Export，提取数据 → 转换格式 → 保存

ConvertTo，转换

```powershell
Get-Service | ConvertTo-HTML | Out-File service.html
```

## 6.5 

```powershell
# ! 宕机命令
Get-Process | Stop-Process
Get-Process -name Notepad | Stop-Process
```

`$ConfirmPreference` 
Cmdlet 内部定义影响级别（impact level）

impact level > $ConfirmPreference  询问

```powershell
Get-Service | Stop-Service -confirm
```

## 6.6

```powershell
Get-Content 
Type
Cat
```

## 6.7 动手实验

```powershell
# No.2 
Stop-Service -Exclude -Include
# No.3
Export-CSV -Delimiter
# No.4
export-csv xxx.csv -NoTypeInformation
# No.5 
export-csv -confirm -NoClobber
```

# 第7章 扩展命令

## 7.2 关于产品的“管理 Shell”

@ windowsitpro.com/go/DonJonesPowerShell

## 7.3 扩展：找到并添加插件

PS 扩展：

* 模块
* 管理单元

管理单元：PSSnapin

> 安装，注册，渐移除

```powershell
# 可用管理单元列表
Get-PSSnapin -registered
# 加载管理单元
Add-PSSnapin xxx
# 找出增加的 Cmdlet
gcm -pssnapin xxx
# 查看管理单元提供的 PSDrive
get-psprovider
```

PSSnapin 可增加 Cmdlet 命令，提供 PSDrive。

## 7.4 扩展：找到并添加模块

设计更独立，更易分发，工作原理类似。

模块不需复杂的注册，PS 会在 PSModulePath 环境变量下查找模块。

```powershell
get-content env:psmodulepath
```

默认位置：

* 系统模块，操作系统目录
* 个人模块，文档目录

```powershell
Get-Module
Remove-Module
Import-Module
```

未加载模块，依然可补全其中命令。

## 7.5 命令冲突和移除扩展

同名均加载的命令，以最后加载为准。

```powershell
Remove-PSSnapin
Remove-Module
```

## 7.6 玩转一个新的模块

```powershell
help *dns*
import-module -Name DnsClient
get-command -Module DnsClient
Clear-DnsClientCache [-verbose]
```

## 7.7 配置脚本：在启动 Shell 时预加载扩展

①创建控制台文件

```powershell
Export-Console myShell.psc
```

> 只能记录已加载 PSSnapin。xml 文件。

快捷方式，目标：

`...\powershell.exe -noexit -psconsolefile ...\myShell.psc` 

②配置脚本

1. `我的文档\WindowsPowerShell\profile.ps1`
2. `Add-PSSnapin` `Import-Module`
3. `Set-ExecutionPolicy RemoteSigned` （管理员权限）

# 第8章 对象：数据的另一个名称

## 8.1 什么是对象

```powershell
Get-Process | ConvertTo-HTML | Out-File processes.html
```

术语：

* 对象，“表行”，代表单个事物
* 属性，“表列”，代表关于对象的一部分信息
* 方法，“行为”，方法与某个对象关联并使得对象完成某些任务
* 集合，“表”，整个对象的集合

## 8.2 

使用对象来代表数据的原因：

1. Windows 是一个面向对象的操作系统
2. 简单，提供更强大的功能和更好的灵活性

## 8.3 探索对象：Get-Member

```powershell
Get-Member
Gm
Get-Process
Gps
```

成员，一个对象的属性、方法以及其他附加到对象的东西。

## 8.4  对象标签：也就是所谓的“属性”

PS 惯例使用单数名词

PS 有一个扩展类型系统（ETS）负责添加后来的属性。

属性总是包含一个值，通常是只读的。

## 8.5 对象行为，也就是所谓的“方法”

```powershell
Get-Process -Name Notepad | Stop-Process
Stop-Process -Name Notepad
```

事件，以对象方式通知某事发生。

## 8.6 排序对象

```powershell
Get-Process | Sort-Object -property VM
Get-Process | Sort VM -desc
```

## 8.7 选择所需的属性

```powershell
Select-Object 
Get-Process | Select First 10
```

Select-Object 用于选择所需的属性（或列），还可选择输出行的任意子集（-First/-Last）。

Where-Object 基于筛选条件从管道中移除或过滤对象。

## 8.8 在命令结束之前总是对象的形式

PS 管道在最后一个命令执行之前总是传递对象。

GM => Microsoft.PowerShell.Commands.MemberDefinition

```powershell
Get-Process | Gm | Gm
```

## 8.10 动手实验

```powershell
# No.1
Get-random -minium ... -maxinum ...
# No.2
Get-Date
# No.3, System.DateTime
(get-date).getType().fullname
# No.4
(get-date).DayOfWeek
get-date | select-object dayofweek
# No.5
Get-hotfix
# No.6
Get-hotfix | sort installDate, HotFixID, InstalledBy
# No.7
Get-hotfix | sort Description
    | select-object Description, hotfixID, installDate
    | convertTo-HTML
    | Out-File hotfix.html
# No.8
get-eventLog -list | select-object -first 50
```

# 第9章 深入理解管道

## 9.1 

单行命令功能强大的原因：管道工作机制

## 9.2 

管道参数绑定（Pipeline parameter binding）：

* ByValue
* ByPropertyName

## 9.3

大部分情况下，使用相同名词的命令都可以使用 ByValue 的方式相互之间进行管道传输。

## 9.4

ByPorpertyName 仅寻找能够匹配参数名称的属性名称。

## 9.5

```powershell
Import-CSV .\NewUsers.cvs |
    Select-Object -Property *, 
    @{name='samAccountName'; expression={$_.login}}, # 哈希表
    @{label='Name';expression={$_.login}}, # Name/N/Label/L 属性名
    @{n='Department';e={$_.Dept}} # Expression/E 脚本 
```

## 9.6 括号命令

```powershell
Get-WMIObject -Class WIN32_BIOS -ComputerName (Get-Content .\computers.txt)
```

# 第10章 格式化及如何正确使用

## 10.2 默认格式

文件：

* DotNetTypes.format.ps1xml （带数字签名）
* Types.ps1xml

格式化规则：

1. 系统会检查对象类型是否已经被预定义视图处理过
2. 格式化系统寻找是否有针对这个对象类型的“default display property set”
3. 决定输出样式。属性 5+，列表形式；否则，表格形式

## 10.3 格式化表格

```powershell
Format-Table
Ft
Get-WmiObject Wind32_BIOS | Format-Table -autoSize
Get-Process | Format-Table -property * 
Get-Service | Sort-Object Status | Format-Table -groupByStatus
Get-Service | Format-Table Name, Status, DisplayName -autoSize -wrap
```

## 10.4 格式化列表

```powershell
Format-List
Fl
Get-Service | Fl * [-property]
```

## 10.5 宽度的格式化

```powershell
Format-Wide
Fw
Get-Process | Format-Wide name -col 4
```

`-property`：单属性，非通配符

`-col`：默认 2 列

## 10.6 定制列和列表记录

```powershell
Get-Service | Format-Table
    @{n='ServiceName'; e={$_.name}}, # 自定义列头
    Status, DisplayName
Get-Process | Format-Table Name, 
    @{n='VM(MB)'; e={$_.VM/1MB -as [int]}} # 数学表达式
    -autoSize
```

KB, kilobyte	MB, megabyte	GB, gigabyte	TB, terabyte	PB, petabyte

```powershell
Get-Process | Format-Table Name,
    @{n='VM(MB)'; e={$_.VM}; formatstring='F2'; align='right'} -autoSize
```

* FormatString：格式化代码，数值/日期
* Width
* Alignment

> @ MSDN Formatting Types: msdn.microsoft.com/library/26etazsy.aspx

## 10.8 另外一个输出：网格

`Out-Gridview`	GUI 表格

绕过格式化子系统

不接收“Format-” Cmdlet 输出

仅接收其他 Cmdlet 输出的对象

## 10.9

### 10.9.1 

“Format-” Cmdlet 产生格式化指令，仅“Out-” Cmdlet 能合理地处理这些指令。

### 10.9.2

分号允许把两个命令合并在一个命令行中。

# 第11章 过滤和对比

## 11.1

过滤：

* 指定 Cmdlet 只检索指定内容
* 采用迭代的方法，第一个 Cmdlet 获得所有结果，第二个 Cmdlet 过滤

## 11.2 左过滤

左过滤，尽可能把过滤条件放置在左侧或靠近命令行的开始部分。

左过滤缺点：过滤方式特定。性能更好

```powershell
Where-Object 
Where
```

## 11.3 对比操作符

对比文本字符串时忽略大小写

-eq  -ceq  -ne  -cne  -ge  -cge  -le  -cle  -gt  -cgt  -lt  -clt

-and  -or  -not(!)

`$Responding -eq $False` <=> `-not $Responding`

-like  -clike  -notlike  -cnotlike  -match  -cmatch  -notmatch  -cnotmatch

```powershell
get-help about_comparison_operators
```

## 11.4 过滤对象的管道

```powershell
Get-Service | Where-Object -filter {$_.Status -eq 'Running'}
Get-Service | Where {...}
```

`$_` 占位符只能在 PS 能查找的特定位置中使用

```powershell
# v3+，单比较简写
Get-Service | Where Status -eq 'Running' 
Get-Process | Where {$_.name -notlike 'powershell*'} |
    Sort VM -descending | select -first 10 |
    measure -property VM -sum
```

## 11.7 动手实验

```powershell
# No.3
get-hotfix -Description Security*
# No.4 
get-service | where {($_.starttype -eq 'Automatic')
    -and ($_.status -eq 'stopped')}
# No.5
get-hotfix -Description Update | 
    where InstalledBy -like '*admin*'
# No.6
get-process | where {(($_.name -eq 'Conhost')
    -or ($_.name -eq 'Svchost'))
    -and (!$_.HasExited)}
```

# 第12章 学以致用

## 12.2

```powershell
Get-PrinJob -printer "Accounting" | Remove -PrintJob
```

## 12.4 自学的一些技巧

使用帮助并确保阅读示例

不要跳过不是目前寻找的信息

不怕失败，使用虚拟机

一种方法不奏效，尝试其他方法

# 第13章 远程处理：一对一及一对多

## 13.1 PowerShell 远程处理的原理

新通信协议，针对管理的 Web 服务

Web Services for Management, WS-MAN

WS-MAN 实现基于后台服务：Windows 远程管理组件（WinRM）

## 13.3 一对一场景的 Enter-PSSession 和 Exit-PSSession

```powershell
Enter-PSSession -ComputerName ...
Exit-PSSession
```

13.4 一对多场景的 Invoke-Command

```powershell
Invoke-Command -ComputerName ...
    -Command {...}
    -ScriptBlock
```

# 第14章 Windows 管理规范

Windows Management Instrumentation, WMI

## 14.1 WMI 概要

```powershell
Get-CimInstance -Namespace ...
    -ClassName ...
```

每个命名空间实际上是一种有边界的容器。

# 第15章 多任务后台作业

## 15.2 同步 VS 异步 

作业（Job），后台执行的命令。

## 15.3 创建本地作业

本地作业，命令几乎完全运行于本地计算机，并且以后台模式运行。

```powershell
Start-Job -ScriptBlock ...
    -FilePath ...
    -Name ...
    -Credential ... # DOMAIN\UserName
```

每个作业至少包含一个子作业，因此，连续创建的作业 ID 有间隔。

## 15.4 WMI 作业

```powershell
Get-WMI ...
    -ComputerName ...
    -AsJob
Help * -Parameter AsJob
```

## 15.5 远程处理作业

```powershell
Invoke-Command -Command ... -ComputerName ...
    -AsJob -JobName ...
```

## 15.6 获取作业执行结果

```powershell
Get-Job
Get-Job -id ... | format-list *
Receive-Job -ID ... -Keep
```

## 15.7 使用子作业

```powershell
Get-Job -ID | Select-Object -Expand ChildJobs
```

## 15.8 管理作业的命令

```powershell
Remove-Job
Stop-Job
Wait-Job
```

## 15.9 调度作业

```powershell
Register-ScheduledJob -Name ...
    -ScriptBlock ...
    -Trigger (New-JobTrigger -Daily -At 2am)
    -ScheduledJobOption (New-ScheduledJobOption -WakeToRun -RunElevated)
```

# 第16章 同时处理多个对象

批处理Cmdlet、WMI方法以及对象枚举。

ForEach-Object <=> % <=> ForEach

# 第18章 变量：一个存放资料的地方

## 18.2 存储值到变量中

变量名可以包含空格，但名字必须用花括号括起来。${My  Variable}

VBScript 背景用户惯用类型前缀标示变量类型。

## 18.3 使用变量：有趣的引号

字符串转义字符：`

```powershell
get-help about_escape
get-help about_Special_Characters
get-help stop-parsing
get-help about_Quoting_Rules
```

**18.4.4　在PowerShell中展现属性和方法**

```powershell
$services = Get-Service 
$services.Name

Get-Service | ForEach-Object { Write-Output $_.Name }

Get-Service | Select-Object –ExpandProperty Name
```

## 18.5 双引号的其他技巧

通常情况下，PowerShell 尝试解析双引号中从“$”开始的一串常规文本字符为变量，但实际情况中，变量名可能包含非常规文本字符，此时需要使用子表达式：“\$()”。

```powershell
$services = get-service 
$firstname = "The first name is $($services[0].name)"
```

甚至，可以访问一系列对象的内容：

```powershell
$services = get-service 
$var = "Service names are $services.name"
```

## 18.6 声明变量类型

[int]——整型数字。
[single]和[double]——单精度和多精度浮点型数值（小数位部分的数值）。
[string]——字符串。
[char]——仅单个字符（如[char]$c=’X’）。
[xml]——一个XML文档。不管你如何解析里面的值，都要确保它包含有效的XML标记（比如[xml]$doc=Get-Content MyXML.xml）。
[adsi]——一个活动目录服务接口（ADSI）查询。Shell会执行查询并把结果对象存入变量（如[adsi]$user=”WinNT:\MYDOMAIN\Administrator,user”）。

## 18.7 与变量相关的命令

* New-Variable；
* Set-Variable；
* Remove-Variable；
* Get-Variable；
* Clear-Variable。

```powershell
get-help about-about_scope
```

## 18.9 常见误区

Shell有两个解析规则用于获取变量名：

* 如果紧随美元符后的字符是一个字母、数字或下划线，则变量名包含美元符到下一个空白的所有字符（可能是一个空格、Tab或回车）。
* 如果紧随美元符后的是一个左花括弧，则变量名包含左花括号开始但不包含右花括号之间的所有内容。

# 第19章 输入和输出

## 19.1 提示并显示信息

与你进行交互的对象称为主机应用程序。当运行PowerShell.exe时，你看到的命令行控制台称为控制台主机。图形化的PowerShell ISE通常被称为ISE主机或者图形化主机。

与Shell交互的方式由主机应用程序决定，并不是由PowerShell本身决定。

## 19.2 Read-Host 命令

严格来说，键入的信息被放进了管道。

```powershell
# 输入框
[void][System.Reflection.Assembly]::LoadWithPartialName('Microsoft.VisualBasic')
```

抛弃产生结果的方法：

* 强转为 Void
* 管道传递给 Out-Null

```powershell
$ComputerName = [Microsoft.VisualBasic.Interaction]::InputBox('Enter a computer name','Computer Name','localhost')
```

## 19.3 Write-Host 命令

```powershell
Write-Host "COLORFUL!" -Fore Yellow -Back Magenta
```

Write-Host 不会放置任何数据到管道中。会直接写到主机应用程序的界面。

Write-Verbose

设置\$VerbosePreference= "Continue"将会启用Write-Verbose，\$VerbosePreference="SilentlyContinue"会截断其输出。

Write-Debug（\$DebugPreference）

Write-Warning （\$WarningPreference）

## 19.4 Write-Output 命令

Write-Output 命令可以将对象发送给管道。因为它不会直接写到显示界面，所以不允许你指定其他任何的颜色。不是设计出来展示结果的。

## 19.5 其他写入的方式

可选的输出Cmdlet

| Cmdlet        | 作用                                                         | 配置变量                                      |
| ------------- | ------------------------------------------------------------ | --------------------------------------------- |
| Write-Warning | 显示警告信息，默认会以黄色字体显示，同时前面带有“警告：”字样 | $WarningPreference （默认为Continue）         |
| Write-Verbose | 显示详细信息，默认会以黄色字体显示，同时前面带有“详细信息：”字样 | $VerbosePreference （默认为SilentlyContinue） |
| Write-Debug   | 显示调试信息，默认以黄色字体显示，同时前面带有“调试：”字样   | $DebugPreference (默认为SilentlyContinue)     |
| Write-Error   | 产生一个错误信息                                             | $ErrorActionPreference (默认为Continue)       |

Write-Error 会将错误信息写入PowerShell的错误流中。

Write-Progress

# 19.6 动手实验

```powershell
# 1
write-output (100/10)
# 2 
write-host (100/10)
# 3
write-host -fore yellow zonglu
# 4
read-host name | ? {$_.length -gt 5} | write-host
```

# 第20章 轻松实现远程控制

## 20.2 创建并使用可重用会话

会话是一个在PowerShell副本与远程PowerShell副本之间的持久化连接。

```powershell
new-pssession -computername server-r2,server17,dc5
get-pssession
get-pssession | remove-pssession

$iis_servers = new-pssession -comp web1,web2,web3 -credential WebAdmin
$iis_servers | remove-pssession

$s_server1,$s_server2 = new-pssession -computer server-r2,dc01
```

## 20.3 利用Enter-PSSession命令使用会话

```powershell
enter-pssession -session $sessions[0]

enter-pssession -session ($sessions | where { $_.computername -eq 'server-r2' })

enter-pssession -session (get-pssession -computer server-r2)
```

# 第21章 你把这叫作脚本 

## 21.5 为脚本添加文档

```powershell
help about_comment_based_help
```

## 21.6 g 一个脚本， 一个管道

在脚本中，任何产生管道输出结果的命令都会被写入同一个管道中：脚本自身运行的管道。

## 21.7 作用域初探

作用域是特定类型PowerShell元素的容器，主要是别名、变量和函数。

Shell本身具有最高级的作用域，称为全局域（global scope）。

当运行一个脚本时，会在脚本范围内创建一个新的作用域，就是所谓的脚本作用域（script scope）。

函数还有其特有的私有作用域（private scope）。

# 第22章 优化可传参脚本

## 22.3 将参数定义为强制化参数

```powershell
[Parameter(Mandatory=$True，HelpMessage="Enter a computer name to query")
```

## 22.4 添加参数别名

```powershell
param ( 
  [Parameter(Mandatory=$True)] 
  [Alias('hostname')]    
  [string]$computername， 
```

## 22.5 验证输入的参数

```powershell
param ( 
  [ValidateSet(2，3)] 
  [int]$drivetype = 3 
)

help about_functions_advanced_parameters
```

# 第23章 高级远程配置

## 23.1 使用其他端点

```powershell
Get-PSSessionConfiguration
```

# 第24章 使用正则表达式解析文本文

## 24.2 正则表达式入门

PowerShell默认的匹配规则是不区分大小写。

## 24.3 通过-Match使用正则表达式

## 24.4 通过Select-String使用正则表达式

```powershell
select-string -pattern xxx
```

# 第25章 额外的提示、技巧以及技术

## 25.1 Profile、提示以及颜色：自定义Shell界面

### 25.1.1 PowerShell Profile脚本

```powershell
Import-Module ActiveDirectory  
Add-PSSnapIn SqlServerCmdletSnapIn100

Help About_Profiles
```

控制台主机尝试载入的文件及顺序：

（1）\$PsHome/Profile.PS1——不管使用何种主机应用程序，该脚本都会对计算机上所有用户执行（请记住，\$PSHome路径是已经被预定义在PowerShell中，并且包含PowerShell的安装文件夹的路径）。
（2）\$PsHome/Microsoft.PowerShell_Profile.PS1——如果计算机上的用户均使用控制台主机应用程序，那么该脚本会对所有用户执行。如果他们使用的是PowerShell的ISE，那么\$PsHome/Microsoft.PowerShellISE_Profile.ps1脚本会被执行。
（3）\$Home/Documents/WindowsPowerShell/Profile.PS1——该脚本仅会对当前用户执行（因为该脚本存在于用户的根目录下），不管该用户使用的是何种主机应用程序。
（4）\$Home/Documents/WindowsPowerShell/Microsoft.PowerShell_Profile.ps1——仅会针对当前使用PowerShell控制台的用户使用。如果用户使用的是PowerShell ISE，那么会执行​\$Home/Documents/WindowsPowerShell/Microsoft.PowerShellISE_Profile.PS1。

```powershell
help About_Profiles
```

### 25.1.2 自定义提示

```powershell
Function Prompt  
{  
     $(IF (Test-Path Variable:/PSDebugContext) { '[DBG]: ' }  
     ELSE { '' }) + 'PS ' + $(Get-Location) `  
     + $(IF ($NestedPromptLevel -Ge 1) { '>>' }) + '> '  
}

# 我的配置， 自定义 posh-git 
Function getPath
{	
	$drive = $(Split-Path (Get-Location) -qualifier)
	$parent = $(split-Path (get-location) -parent)
	if($parent) {
		$prompt = $(split-Path (get-location) -leaf)
	} else {
		$prompt = ''
	}
	
	$fullPrompt = $drive + '>> ' + $prompt
	
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

# https://github.com/dahlbyk/posh-git/wiki/Customizing-Your-PowerShell-Prompt
```

### 25.1.3 调整颜色

```powershell
(Get-Host).PrivateData.ErrorForegroundColor="Green"
```

可设置属性：

ErrorForegroundColor
ErrorBackgroundColor
WarningForegroundColor
WarningBackgroundColor
DebugForegroundColor
DebugBackgroundColor
VerboseForegroundColor
VerboseBackgroundColor
ProgressForegroundColor
ProgressBackgroundColor

颜色：

Red Yellow Black White Green Cyan Magenta Blue

深色：

DarkRed，DarkYellow，DarkGreen，DarkCyan，DarkBlue

## 25.2 运算符：-AS,-IS,-Replace,-Join,-Split,-IN,-Contains

### 25.2.1 –AS和–IS

`-as` 将一种已存在的对象转换到新的对象类型，从而产生一个新的对象。

```powershell
1000/3 -as [int]
```

可用类型：[String]，[XML]，[INT]，[Single]，[Double]，[Datetime]

`-is` 主要用来判断某个对象是否为特定类型，如果是，则返回True，否则为False。

```powershell
123.45 -is [int]
```

### 25.2.2 -Replace

`-replace` 在某个字符串中寻找特定字符（串），最后将该字符（串）替换为新的字符（串）。

```powershell
"192.168.34.12" -Replace "34","15"  
```

### 25.2.3 –Join和-Split

`-join` 将数组转化为分隔列表。

```powershell
$array -join ","
```

`-split` 将分隔列表转化为数组。

```powershell
xxx -split ','
```

### 25.2.4 –Contains和-IN

`-contains` 在一个集合中是否存在特定对象。

```powershell
$Collection -Contains 'xyz' 
```

`-in` 集合在右边，而需要检查的对象在左边。

```powershell
'abc' -IN $Collection 
```

## 25.5 处理WMI日期

```powershell
Get-WMIObject Win32_OperatingSystem | Select LastBootUpTime  
```

## 25.6 设置参数默认值

PS3+ 可以对任意命令的任意参数——甚至是针对多个命令，指定自定义的默认值。

默认值保存在名为$PSDefaultParameterValues的特殊内置变量中。

```powershell
PS C:\>$PSDefaultParameterValues.Add('Invoke-Command:Credential',  {Get-Credential -Message 'Enter Administrator Credential' –UserName Administrator})
```

参数一：\<Cmdlet\>:\<Parameter\> 可接受 * 

参数二：默认值或执行其他（一个或多个）命令的脚本块。

About_Scope

About_Parameters_Default_Values

## 25.7 学习脚本块

脚本块是指包含在大括号中的全部命令——哈希表除外（哈希表在大括号之前会带有@符号）。

在命令行中输入一个脚本块，然后将该脚本块赋值给一个变量，再使用&该调用运算符来执行该脚本块。

About_Script_Block

# 第26章 使用他人的脚本 

##  26.1 脚本 

```powershell
$System = [Environment]::GetFolderPath("System") 
$script:hostsPath = ([System.IO.Path]::Combine($System, "drivers\etc\")) 
```

About_Try_ Cacth_Finally

# 第28章　PowerShell备忘清单

## 28.1 标点符号

`（重音符）——重音符是PowerShell中的转义字符。

\~（波浪符）——当将 \~ 作为路径的一部分时，该字符表示当前用户的根目录，也就是在系统变量UserProfile中定义的值。

()（括号）——定义执行顺序；包围方法参数。

[]（方括号）——包围序列索引号；类型转换时，包围转换为类型

{}（花括号）——定义脚本段（Script Block）；包含哈希表键-值对（前缀“@”，$HashTable=@{l='Label'; e={expression}}）；包围包含空格或其他非法字符的变量（\${My Variable}）。

'（单引号）——单引号可用作包含字符串（String）。PowerShell并不会对包含在单引号中的字符串查找转义字符或者变量。

"（双引号）——双引号也可用作包含字符串，但与单引号不同的是，PowerShell会针对双引号中的字符串数据进行查找转义字符以及\$字符。其中会进行针对转义字符的处理，同时\$符号后面带有的字符（到下一个空格为止）会被识别为一个变量名字，并且其值会被替换掉。

\$（美元符号）——该符号告诉PowerShell \$后面的字符（截止到下一个空格处）为一个变量的名称。

%（百分号）——百分号是ForEach-Object Cmdlet的别名，同时它也是模运算符，返回除法运算后的余数。

?（问号）——问号是Where-Object Cmdlet的别名。

\>（右尖括号）——该符号类似Out-File Cmdlet的一个别名。但是严格来讲，它并不是一个真正的别名，但是却提供了Cmd.exe类型的文件重定向功能

\+ \- \* / %（数学运算符）——这些运算符是作为标准算术运算符使用。

+也可以用作字符串连接使用。

-（破折号或者叫连字符）——可以用作连接参数名称或者其他运算符；分离Cmdlet名称中的动词与名词；减法

@（at符号）——花括号前；数组，\$Array= @(1,2,3,4)。其中的@字符与括号是可选的，因为PowerShell默认会将以逗号分隔的列表识别为数组；可以指一个Here-String。Here-String是指包含在单引号或者双引号中的字符串。一个Here-String以“@”字符作为开始和结束的标志，结束的“@”必须位于另起一行的起始位置。（About_Quoting_Rules）；传递符（Splat Operator）。如果构建了一个哈希表，在哈希表中，键名称能匹配参数名称，并且键的值为参数的值，那么你就可以将该哈希表传递给一个Cmdlet。

&（与符号）——这是PowerShell中的一个调用运算符，使得PowerShell可以将某些字符识别为命令，并运行这些命令

;（分号）——分号一般用作分隔PowerShell中同一行的两个命令：Dir;Get-Process。

#（井号）——该符号为注释符号。跟在#之后的文字，到下一个回车之前，均会被PowerShell忽略掉。尖括号<>可以被用作定义一个注释块的标签，“<#”作为起始，“#>”作为结束。

=（等号）——等号是PowerShell中的赋值运算符

\或者/（反斜杠或斜杠）——斜杠可以作为数学表示中的除法运算符

.（句号）——成员访问；“.”引用源码来执行一段脚本，意味着该脚本运行在当前作用域下，并且该脚本定义的任何对象在脚本运行完毕之后均存在，比如.C:\myscript.ps1。两个“.”（..）会形成一个范围运算

,（逗号）——当用在引号外面时，逗号可以用作分隔数组或者列表中的项："One",2,"Three",4。将多个静态值传递给可接收这些值的参数：Get-Process –ComputerName Server1,Server2,Server3。

:（冒号）——冒号（严格来说应该是两个冒号）可用作访问类的静态成员。

!（感叹号）——是“非”(Not)布尔运算符的别名。

## 28.2　帮助文档

[]——大括号，用作表达包含在大括号中的文本为可选项。

<>——尖括号可用来包含数据类型，表示值的类型或者参数匹配的对象

## 28.3　运算符

-eq——等于（-ceq用作字符串比较，包括大小写是否一致）。

-ne——不等于（-cne用作字符串比较，包括大小写是否一致）。

-ge——大于或等于（-cge用作字符串比较，包括大小写是否一致）。

-le——小于或等于（-cle用作字符串比较，包括大小写是否一致）。

-gt——大于（-cgt用作字符串比较，包括大小写是否一致）。

-lt——小于（-clt用作字符串比较，包括大小写是否一致）。

-contains——若数据集包含特定对象，则返回真（True）。（\$Collection –Contains \$Object。）-nocontains表示相反含义。

-in——若特定对象包含在数据集中，则返回真（True）。（\$Object –in \$Collection。）

-noin表示相反含义。

逻辑运算符可用作组合运算：

-not——将真假值取反（!是该运算符的别名）。

-and——如果整个表达式要为真，则所有子表达式均需要为真。

-or——如果整个表达式要为真，则其中一个子表达式需要为真。 

执行特定操作的运算符：

-Join——将一个数组的元素连接为分隔的字符串。
-Split——将一个分隔的字符串分离为一个数组。
-Replace——将一个字符串中特定字符（串）替换为另外的字符（串）。
-Is——若一个对象为指定类型，返回为真（True）。（\$ID –Is [INT]）
-As——将对象转化为特定类型（\$ID –As [INT]）
..——一个范围运算符，1..10会返回1到10的十个对象。
-F——格式化运算符，会使用后面提供的值替换对应的占位符。（"{0},{1}" –F "Hello","World"）

## 28.4　自定义属性与列的语法

```powershell
@{Label='Column_or_Property_Name';Expression={Value_Expression}}
```

两个键“Label”和“Expression”，可以分别缩写为“l”和“e”

