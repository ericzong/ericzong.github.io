---
layout: page
title: scoop参考
category: 工具
date: 2019-12-29
author: "Eric Zong"
---

* content
{:toc}
# 安装

```powershell
# 权限设置
Set-ExecutionPolicy RemoteSigned -scope CurrentUser
iex (new-object net.webclient).downloadstring('https://get.scoop.sh')
# 设置用户环境变量 SCOOP
[environment]::setEnvironmentVariable('SCOOP', '/path/to/scoop', 'User')
# 使 SCOOP 在当前命令行生效
$env:SCOOP='/path/to/scoop'
```

# 别名

```powershell
scoop alias add ss 'scoop status' '查看可用更新'
scoop alias add us 'scoop update; scoop status' '更新scoop并查看可用更新'
scoop alias add u 'scoop update $args[0]' '更新应用'
scoop alias add da 'scoop config aria2-enabled false' '禁用aria2'
scoop alias add ea 'scoop config aria2-enabled true' '启用aria2'
```

# 代理配置

```powershell
# 命令行代理
set http_proxy=http://host:port
set https_proxy=http://host:port
set http_proxy_user=
set http_proxy_pass=

# 通过代理安装 scoop
# If you want to use a proxy that isn't already configured in Internet Options
[net.webrequest]::defaultwebproxy = new-object net.webproxy "http://proxy.example.org:8080"
# If you want to use the Windows credentials of the logged-in user to authenticate with your proxy
[net.webrequest]::defaultwebproxy.credentials = [net.credentialcache]::defaultcredentials
# If you want to use other credentials (replace 'username' and 'password')
[net.webrequest]::defaultwebproxy.credentials = new-object net.networkcredential 'username', 'password'

# 为 scoop 配置代理
scoop config proxy [username:password@]host:port
scoop config rm proxy
scoop config proxy currentuser@default
scoop config proxy user:password@default
scoop config proxy none # 绕过代理直连

# 为 git 配置代理
git config --global http.proxy http://host:port
git config --global https.proxy https://host:port
```

# 命令参考

| 命令       | 说明                                                   |
| ---------- | ------------------------------------------------------ |
| alias      | Manage scoop aliases                                   |
| bucket     | Manage Scoop buckets                                   |
| cache      | Show or clear the download cache                       |
| checkup    | Check for potential problems                           |
| cleanup    | Cleanup apps by removing old versions                  |
| config     | Get or set configuration values                        |
| create     | Create a custom app manifest                           |
| depends    | List dependencies for an app                           |
| export     | Exports (an importable) list of installed apps         |
| help       | Show help for a command                                |
| home       | Opens the app homepage                                 |
| info       | Display information about an app                       |
| install    | Install apps                                           |
| list       | List installed apps                                    |
| prefix     | Returns the path to the specified app                  |
| reset      | Reset an app to resolve conflicts                      |
| search     | Search available apps                                  |
| status     | Show status and check for new app versions             |
| uninstall  | Uninstall an app                                       |
| update     | Update apps, or Scoop itself                           |
| virustotal | Look for app's hash on virustotal.com                  |
| which      | Locate a shim/executable (similar to 'which' on Linux) |