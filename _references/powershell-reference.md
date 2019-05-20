---
layout: page
title: PowerShell参考
category: 编程语言
---

* content
{:toc}
# 快捷键

| 快捷键       | 功能                              | 说明                                                         |
| ------------ | --------------------------------- | ------------------------------------------------------------ |
| Esc          | 清空当前命令行                    |                                                              |
| Insert       | 插入/改写模式切换                 |                                                              |
| Tab          | 自动补齐                          | 连续输入在多个候选项间切换                                   |
| Ctrl + Home  | 删除从光标到行首                  |                                                              |
| Ctrl + End   | 删除从光标到行尾                  |                                                              |
| F4           | 删除命令行至光标右边指定字符处    | 例如，当前输入“Get-Proc”，光标在“-”后，按 F4，键入“o” ，则命令为删除为“Get-oc”。注意：如果键入的字符不存在，则删除至末尾。 |
| F2           | 自动补充历史命令至指定字符        | 从上一条历史命令截取字符串。例如上一条命令为 Get-Process，按 F2，键入“s”，则补齐命令为 Get-Proce。 |
| ↑ / ↓        | 向上/下取回历史命令               | 可连续输入                                                   |
| PgUp / PgDn  | 显示当前会话的第一个/最后一个命令 |                                                              |
| F7           | 显示命令历史记录。                | 可用上下箭头选择执行；历史记录带编号，可与 F9 结合使用。     |
| F9           | 按历史命令编号取回命令            | 命令编号可用 F7 查看。                                       |
| F8           | 搜索历史记录                      | 按光标前的文本搜索包含它的历史命令，匹配多条命令连续按 F8 切换。 |
| ALT+F7       | 清除命令的历史记录                |                                                              |
| F3           | 取回上一条命令                    | 等效于按一次 ↑，连续按不会切换                               |
| F5           | 向上取回历史命令                  | 等效于 ↑                                                     |
| Ctrl + H     | 删除光标左边一个字符              | 等效于 Backspace                                             |
| Ctrl + M     | 执行当前命令                      | 等效于 Enter                                                 |
| Ctrl + Break | 终止当前命令执行                  | 等效于 Ctrl + C                                              |

# 可用cmd/Unix命令别名

| cmd/Unix |         |       |       |
| -------- | ------- | ----- | ----- |
| cat      | dir     | mount | rm    |
| cd       | echo    | move  | rmdir |
| chdir    | erase   | popd  | sleep |
| clear    | h       | ps    | sort  |
| cls      | history | pushd | tee   |
| copy     | kill    | pwd   | type  |
| del      | lp      | r     | write |
| diff     | ls      | ren   |       |

# 缩写

| Noun or Verb | Abbreviation |
| ------------ | ------------ |
| Get          | g            |
| Set          | s            |
| Item         | i            |
| Location     | l            |
| Command      | cm           |
| Alias        | al           |

| Cmdlet name    | Alias |
| -------------- | ----- |
| `Get-Item`     | gi    |
| `Set-Item`     | si    |
| `Get-Location` | gl    |
| `Set-Location` | sl    |
| `Get-Command`  | gcm   |
| `Get-Alias`    | gal   |

# 配置文件位置

| profile 位置                    | 范围             | 文件                             |
| ------------------------------- | ---------------- | -------------------------------- |
| $profile.CurrentUserCurrentHost | 当前用户当前主机 | Microsoft.PowerShell_profile.ps1 |
| $profile.CurrentUserAllHosts    | 当前用户所有主机 | profile.ps1                      |
| $profile.AllUsersCurrentHost    | 所有用户当前主机 | Microsoft.PowerShell_profile.ps1 |
| $profile.AllUsersAllHosts       | 所有用户所有主机 | profile.ps1                      |

> 当前用户配置文件位于：`~\Documents\WindowsPowerShell\`
>
> 所有用户配置文件位于：`C:\Windows\System32\WindowsPowerShell\v1.0\`

# 转义字符

| 转义字符 | 描述       |
| -------- | ---------- |
| `n       | 换行符     |
| `r       | 回车符     |
| `t       | 制表符     |
| `a       | 响铃符     |
| `b       | 退格符     |
| `'       | 单引号     |
| `"       | 双引号     |
| `0       | Null       |
| ``       | 重音符本身 |

# Windows 特殊目录

| 特殊目录                 | 描述                           | 示例                    |
| ------------------------ | ------------------------------ | ----------------------- |
| Application data         | 存储在本地机器上的应用程序数据 | $env:localappdata       |
| User profile             | 用户目录                       | $env:userprofile        |
| Data used incommon       | 应用程序公有数据目录           | $env:commonprogramfiles |
| Public directory         | 所有本地用户的公有目录         | $env:public             |
| Program directory        | 具体应用程序安装的目录         | $env:programfiles       |
| Roaming Profiles         | 漫游用户的应用程序数据         | $env:appdata            |
| Temporary files(private) | 当前用户的临时目录             | $env:tmp                |
| Temporary files          | 公有临时文件目录               | $env:temp               |
| Windows directory        | Windows系统安装的目录          | $env:windir             |

# 变量类型

| 变量类型         | 说明                                                         | 示例                                                         |
| ---------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| [array]          | An array                                                     |                                                              |
| [bool]           | Yes-no value                                                 | [boolean]$flag= $true                                        |
| [byte]           | Unsigned 8-bit integer, 0...255                              | [byte]$value= 12                                             |
| [char]           | Individual Unicode character                                 | [char]$a= "t"                                                |
| [datetime]       | Date and time indications                                    | [datetime]$date= "12.Nov 2004 12:30"                         |
| [decimal]        | Decimal number                                               | [decimal]$a= 12 $a = 12d                                     |
| [double]         | Double-precision floating point decimal                      | $amount= 12.45                                               |
| [guid]           | Globally unambiguous 32-byte identification number           | [guid]$id= [System.Guid]::NewGuid() $id.toString()           |
| [hashtable]      | Hashtable                                                    |                                                              |
| [int16]          | 16-bit integer with characters                               | [int16]$value= 1000                                          |
| [int32],[int]    | 32-bit integers with characters                              | [int32]$value= 5000                                          |
| [int64],[long]   | 64-bit integers with characters                              | [int64]$value= 4GB                                           |
| [nullable]       | Widens another data type to include the ability to contain null values. It can be used, among others, to implement optional parameters | [Nullable``1[[System.DateTime]]]$test= Get-Date $test = $null |
| [psobject]       | PowerShell object                                            |                                                              |
| [regex]          | Regular expression                                           | $text= "Hello World" [regex]::split($text, "lo")             |
| [sbyte]          | 8-bit integers with characters                               | [sbyte]$value= -12                                           |
| [scriptblock]    | PowerShell script block                                      |                                                              |
| [single],[float] | Single-precision floating point number                       | [single]$amount= 44.67                                       |
| [string]         | String                                                       | [string]$text= "Hello"                                       |
| [switch]         | PowerShell switch parameter                                  |                                                              |
| [timespan]       | Time interval                                                | [timespan]$t= New-TimeSpan $(Get-Date) "1.Sep 07"            |
| [type]           | Type                                                         |                                                              |
| [uint16]         | Unsigned 16-bit integer                                      | [uint16]$value= 1000                                         |
| [uint32]         | Unsigned 32-bit integer                                      | [uint32]$value= 5000                                         |
| [uint64]         | Unsigned 64-bit integer                                      | [uint64]$value= 4GB                                          |
| [xml]            | XML document                                                 |                                                              |

# 参数属性

| 参数/属性名                     | 说明                                       | 备注                                                        |
| ------------------------------- | ------------------------------------------ | ----------------------------------------------------------- |
| Mandatory                       | 是否必须                                   | 布尔型；缺省为可选；PS v3+ 支持写法：[Parameter(Mandatory)] |
| Position                        | 参数位置                                   | 数值型；缺省为命名参数                                      |
| ParameterSetName                | 参数集                                     | 每个参数集必须有一个不属于其他参数的唯一参数                |
| ValueFromPipeline               | 接受管道输入                               | 布尔型                                                      |
| ValueFromPipelineByPropertyName | 接受管道对象的属性输入                     |                                                             |
| ValueFromRemainingArguments     | 剩余参数                                   |                                                             |
| HelpMessage                     | 帮助信息                                   |                                                             |
| Alias                           | 别名                                       | 以上 8 种属于静态参数，以下 9 种属于验证参数                |
| AllowNull                       | 允许为 Null                                | [AllowNull()]                                               |
| AllowEmptyString                | 允许空字符串                               |                                                             |
| AllowEmptyCollection            | 允许空集合                                 |                                                             |
| ValidateCount                   | 参数数目范围                               | [ValidateCount(1, 10)]                                      |
| ValidateLength                  | 参数长度范围                               |                                                             |
| ValidatePattern                 | 正则验证                                   | [ValidatePattern("RE")]                                     |
| ValidateRange                   | 参数范围                                   |                                                             |
| ValidateScript                  | 脚本验证                                   | [ValidateScript({...})]                                     |
| ValidateNotNull                 | 不允许为 Null                              |                                                             |
| ValidateNotNullOrEmpty          | 不允许为 Null 或空（包括空字符串、数组等） |                                                             |

# `[system.IO.Path]` 方法

| 方法                          | 描述                                               | 示例                                            |
| ----------------------------- | -------------------------------------------------- | ----------------------------------------------- |
| ChangeExtension()             | 更改文件的扩展名                                   | ChangeExtension("test.txt", "ps1")              |
| Combine()                     | 拼接路径字符串; 对应Join-Path                      | Combine("C:\test", "test.txt")                  |
| GetDirectoryName()            | 返回目录对象：对应Split-Path -parent               | GetDirectoryName("c:\test\file.txt")            |
| GetExtension()                | 返回文件扩展名                                     | GetExtension("c:\test\file.txt")                |
| GetFileName()                 | 返回文件名：对应Split-Path -leaf                   | GetFileName("c:\test\file.txt")                 |
| GetFileNameWithoutExtension() | 返回不带扩展名的文件名                             | GetFileNameWithoutExtension("c:\test\file.txt") |
| GetFullPath()                 | 返回绝对路径                                       | GetFullPath(".\test.txt")                       |
| GetInvalidFileNameChars()     | 返回所有不允许出现在文件名中字符                   | GetInvalidFileNameChars()                       |
| GetInvalidPathChars()         | 返回所有不允许出现在路径中的字符                   | GetInvalidPathChars()                           |
| GetPathRoot()                 | 返回根目录：对应Split-Path -qualifier              | GetPathRoot("c:\test\file.txt")                 |
| GetRandomFileName()           | 返回一个随机的文件名                               | GetRandomFileName()                             |
| GetTempFileName()             | 在临时目录中返回一个临时文件名                     | GetTempFileName()                               |
| GetTempPath()                 | 返回临时文件目录                                   | GetTempPath()                                   |
| HasExtension()                | 如果路径中包含了扩展名，则返回True                 | HasExtension("c:\test\file.txt")                |
| IsPathRooted()                | 如果是绝对路径，返回为True; Split-Path -isAbsolute | IsPathRooted("c:\test\file.txt")                |