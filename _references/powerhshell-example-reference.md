---
layout: page
title: PowerShell示例参考
category: PowerShell
date: 2019-05-28
---

* content
{:toc}

# 确认对话框

```powershell
$options=[System.Management.Automation.Host.ChoiceDescription[]]`
    @("&Power", "&Shell", "&Quit") # 设置按钮
[int]$defaultchoice=2 # 默认选中按钮
$opt = $host.UI.promptForChoice($Title, $Info, $Options, $defaultChoice)
switch($opt)
{
    0 { Write-Host "Power" -ForegroundColor Green } # 设置输出前景色
    1 { Write-Host "Shell" -ForegroundColor Green }
    2 { Write-Host "Good Bye!!!" -ForegroundColor Green }
}
```

# 密码输入

```powershell
$pwd=Read-Host -AsSecureString "输入密码"
[Runtime.InteropServices.Marshal]::PtrToStringAuto([Runtime.InteropServices.Marshal]::SecureStringToBSTR($pwd))
```

# 启动禁用服务

```powershell
# 如果服务启动类型是“禁用”，是不能直接启动服务的，需要先设置其启动类型
get-service mysql | select startType # Disabled
set-service mysql -startupType manual
start-service mysql
```

# 进制转换

```powershell
# 十六进制转换
(31).ToString('x') # 1f
(31).ToString('X') # 1F
"{0:x}" -f 31  # 1f
"{0:X}" -f 31  # 1F
```

# 数字格式化

```powershell
# 注意：表达式提供值需括号括起来
"{0} ..." -f $num
# 支持带单位，如：1MB、1GB
"{0} * {1} = {2}" -f x, y, z
# 补齐
'{0:d9}' -f 42 # 000000042
# 小数位控制
'{0:n2}' -f 42.67676767 # 42.68
# 百分数
'{0:p0}' -f 0.42 # 42%
```

# 表格格式化

```powershell
$col1_fmt=@{expression="Name"; width=30; label="filename"; alignment="left"}
$col2_fmt=@{expression="LastWriteTime"; width=40; label="last modification"; alignment="right"}
ls | Format-Table $col1_fmt, $col2_fmt
```

表格列属性：

* Expression：绑定的表达式
* Width：列宽度
* Label：列标题
* Alignment：列对齐方式

