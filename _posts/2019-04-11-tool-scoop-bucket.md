---
layout: post
title: "Scoop bucket"
categories: 工具
tags: Scoop 应用管理
excerpt: "介绍Scoop Bucket及其创建。"
author: "Eric Zong"
updated: 2020-08-14
---

* content
{:toc}

# 关于 bucket

Scoop 作为一个“安装器（installer）”，不同于软件包管理器，它不需要存储软件包，只是提供应用清单（app manifests）。

应用清单描述应用包在何处下载、如何安装等信息，而应用清单存放在 bucket 中——应用清单仓库，不是软件包仓库。

与 bucket 相关的命令可以通过 Scoop 的帮助系统查看：

```powershell
scoop help bucket
```

Scoop 安装后，内置了一个 main bucket，此外，还提供了几个 known bucket 以提供更多的应用。

main bucket 是内置的，因此不用额外添加，而其他几个 known bucket 需要用户自行添加。而要添加 known bucket 需要先知道它们的名字，这需要使用以下命令：

```
scoop bucket known
```

知道了名字，我们就可以逐一添加它们了：

```
scoop bucket add <name>
```

添加后，我们就可以从这些 bucket 中搜索安装应用了。显然，这些 bucket 不可能囊括所有的应用，如果我们需要的应用不在这些 bucket 中怎么办呢？Scoop 很灵活，它支持用户自己创建 bucket，并添加。

# 创建 bucket

要创建 bucket 其实很简单，bucket 实质上是一个 GitHub 仓库，因此，在自己的 GitHub 帐号下新建一个仓库，你就得到了一个 bucket。并且通过以下命令就可以添加到 Scoop 中：

```powershell
# 添加 bucket（指定名称和 GitHub 仓库地址）
scoop bucket add my-bucket https://github.com/<your-username>/my-bucket
# 查看已添加的 bucket 列表（新添加的 bucket 应该也在其中）
scoop bucket list
```

不过，目前为止，我们创建的 bucket 是空的，尚未包含任何应用。bucket 以应用清单的形式提供应用，因此，我们还需要添加应用清单。

# 应用清单

所谓“应用清单”，实质上就是一个 JSON 文件，文件中使用一个 JSON 对象提供应用信息。

我们可以使用 Scoop 命令创建该文件：

```powershell
scoop create <url>
```

当然，也可以拷贝一个现成的清单文件进行修改。

## 简单示例

比如，在 GitHub 仓库根目录创建文件 `hello.json`，其内容大致如下：

```json
{
    "version": "1.0",
    "url": "https://.../hello.ps1",
    "bin": "hello.ps1"
}
```

这样，就在自己的 bucket 中添加了一个名为“hello”的应用，通过应用名就可以搜索或安装它：

```powershell
# 搜索应用
scoop search hello
# 安装
scoop install hello
```

当然，以上命令有效的前提是已经添加了自定义的 bucket。

## 属性配置

前文只给出了一个简单的应用清单示例，不过，应用清单中的 JSON 对象有很多配置属性，主要分为 [必要属性](https://github.com/lukesampson/scoop/wiki/App-Manifests#required-properties) 和 [可选属性](https://github.com/lukesampson/scoop/wiki/App-Manifests#optional-properties)，详细说明请参考官方 [Wiki](https://github.com/lukesampson/scoop/wiki/App-Manifests#required-properties)。

通常，要一个应用清单是可用的，至少必须配置必要属性，即 `version` 和 `url`，分别指定应用版本和下载地址。这样，scoop 就可以依此判断应用状态，以正确执行各种命令。而可选属性并非总是可缺省的，这取决于应用的形式，下文应用清单示例会进行相关说明。

## 自动更新

当应用版本升级时，可以手动维护应用清单，以升级相关信息，主要包括：版本号（`version`）、下载地址（`url`）、哈希码（`hash`）等。

但是，手动维护工作量很大，所以，scoop 提供了自动更新应用清单的功能，只是需要进行相关属性配置即可启用。

简单来说，需要配置 `checkver` 和 `autoupdate` 两个属性，前者用以获取最新版本，后者用以获取更新的下载地址和哈希码等。

### 自动更新配置

> 应用清单的配置难点在于，部分关键属性的结构是可变的，同一个属性，可能既可以是一个字符串，也可以是一个数组，还可以是一个对象……
>
> 这并非是 scoop 的开发者有意为之，而是由于应用的提供方式并没有一个统一的规范，所以需要不同的配置方式与之对应。
>
> 而 `checkver` 和 `autoupdate` 属性正在其列。我们需要清楚哪种应用提供方式对应哪种属性配置方式，否则会很混乱。

#### checkver属性

**在主页匹配版本**

最简单的配置是给一个正则表达式，比如后文的 Q-Dir 的配置 `"checkver": "Q-Dir ([^\\ ]+)"`。scoop 将使用该正则表达式去 `homepage` 属性指定的主页中去匹配版本。

**在指定页面匹配版本**

但是，如果应用主页没有版本信息，而要去其他页面查找，那么 `checkver` 将配置为一个 JSON 对象（`checkver.url` 指定该页面；`checkver.regex` 指定正则），比如后文中的 Typora：

```json
"checkver": {
    "url": "https://typora.io/windows/dev_release.html",
    "regex": "<h4>([\\d\\.]+)"
}
```

> **在 JSON 文件中匹配版本**
>
> 如果查找的不是一个页面，而是一个在线 JSON 文件，则需要利用 JSONPath 等相关配置，详见 [这里](https://github.com/lukesampson/scoop/wiki/App-Manifest-Autoupdate#using-jsonpath-in-checkver)。

**在 GitHub 仓库主页匹配版本**

特别地，当应用发布于 GitHub 上时，`homepage` 正是该仓库位置，那么仅需配置 `"checkver": "github"` 即可。比如后文 PowerShell 的配置。

> 注意，`github` 是一个特殊值。默认的正则表达式是 `\/releases\/tag\/(?:v)?([\d.]+)`。而且它匹配的应该不是页面内容，而是 `/releases/latest` 重定向到的页面的 URL——即最后一个正式版的发布页面，因此，不会匹配到预览版等版本。

**指定 GitHub 仓库匹配版本**

关于 GitHub 发布的应用的另一种情况是，该应用有自己的主页（`homepage` 属性不是 GitHub 仓库位置），但版本信息需要在 GitHub 中获取，则需要用 `checkver.github` 来指定仓库位置。

```json
"checkver": {
    "github": "https://github.com/<user>/<project>"
}
```

**指定页面匹配版本**

更一般的，如果版本信息不在主页，而在其他页面，则需要配置 `checkver.url` 和 `checkver.regex` 分别指定版本所在页面的 URL 和正则表达式。

```json
"checkver": {
    "url": "https://path/to/version/page",
    "regex": "..."
}
```

**`checkver`下常用的其他属性**

* `checkver.reverse`：`true`/`false`，返回最后一次匹配，默认是首次。
* `checkver.replace`：替换版本值。这通常会使用[捕获变量](https://github.com/lukesampson/scoop/wiki/App-Manifest-Autoupdate#captured-variables)（随后说明）。

> 注意：`checkver.replace` 使用捕获变量的格式（`${1}`/`${name1}`）与 `autoupdate` 中格式（`$match1`/`$matchName1`）的区别。
>
> `checkver` 下所有属性可参考 [这里](https://github.com/lukesampson/scoop/wiki/App-Manifest-Autoupdate#properties-of-checkver)。

**关于版本变量**

所谓版本变量，就是存储了当前版本信息的变量。

最常见的应该是 ` $version `，它存储了当前应用的版本。默认情况下，它的值来源于 `checkver` 的正则表达式匹配的第一个捕获分组。通常，它用于需自动更新的各种 URL 中，比如：`autoupdate.url`（可参考 PowerShell 应用清单示例）。

> 注意：经测试，一旦正则匹配中有命名捕获分组，那么 `$version` 就不会有值，除非有捕获分组被命名为 `version`。
>
> 当然，版本变量不止这一个，更多版本变量信息参考 [这里](https://github.com/lukesampson/scoop/wiki/App-Manifest-Autoupdate#version-variables)。

**关于捕获变量**

所谓捕获变量，就是存储了在 `checkver` 正则匹配中捕获分组所匹配文本的变量。

> 更多参考 [这里](https://github.com/lukesampson/scoop/wiki/App-Manifest-Autoupdate#captured-variables)。

#### autoupdate属性

从用途上说，`autoupdate` 属性是要自动更新应用清单文件中的一系列版本相关信息，这包括：`url`（应用链接）、`hash`、`extract_dir`……

`autoupdate` 属性配置大致类似下面这样：

```json
"autoupdate": {
    "architecture": {
        "64bit": {
            "url": ...
        },
        "32bit": {
            "url": ...
        }
    },
    "hash": {
        "url": ...,
        "find": ...
    }
}
```

但是，与 `checkver` 属性类似，这个结构是易变的。

**关于`url`配置**

如果应用不区分 32/64 位系统架构，那么，`autoupdate.architecture` 属性的层次就不是必须的，直接指定 `autoupdate.url` 属性就好。

通常，`url` 都会使用版本变量或捕获变量，除非下载链接是固定的。（可参考本文示例部分）

**关于`hash`配置**

如果 HASH 码的获取无关系统架构，那么，通常独立配置 `autoupdate.hash` 属性；但是，如果有关，那么通常分别配置 `autoupdate.architecture.32bit.hash` 和 `autoupdate.architecture.64bit.hash`。

HASH 码有多种获取方式，这要根据应用提供 HASH 码的形式来确定使用哪种配置方式。通常需要配置 `hash.url` 和 `hash.find` 属性。

> `hash` 属性不是必须的，但应尽力配置，否则自动更新时将会通过下载应用来计算其 HASH 码——这可能很耗时。
>
> HASH 码 可通过多种方式获取，包括：请求链接、在线文件中正则查找、JSON path 查找在线 JSON 文件等。
>
> 更多 `hash` 配置参考 [这里](https://github.com/lukesampson/scoop/wiki/App-Manifest-Autoupdate#adding-hash-to-autoupdate)。
>
> 更多 `autoupdate` 配置参考 [这里](https://github.com/lukesampson/scoop/wiki/App-Manifest-Autoupdate#adding-autoupdate-to-a-manifest)。

### 更新应用清单

scoop 提供了一个脚本，位于 `scoop\apps\scoop\current\bin\checkver.ps1`，用以更新应用清单。不过，通常我们不会直接使用该脚本，而是使用有批处理能力的 checkver.ps1 脚本，它位于任一 known bucket 的 `bin` 目录下，比如：`scoop\buckets\extras\bin\checkver.ps1`。我们应在自定义的 bucket 中创建一个 `bin` 目录，并将该脚本拷贝过去。

这样，我们在自定义 bucket 根目录就可以执行以下命令：

```powershell
# 查看某个或所有可自动更新应用的版本状态（列表应用版本及是否可更新）
./bin/checkver.ps1 [app | *]
# 更新某个或所有可自动更新应用的信息（将更新应用清单文件）
./bin/checkver.ps1 [app | *] -u
```

> 注意：仅配置 `checkver` 属性的情况下，`checkver.ps1 <app>` 仍然具有检查版本更新的能力。但没有配置 `autoupdate` 属性的情况下是不能更新应用清单文件的。
>
> 更多可用的参数可参考 [这里](https://github.com/lukesampson/scoop/wiki/App-Manifest-Autoupdate#parameters-of-checkverps1)。

## 持久化数据

默认情况下，升级应用时，通常相当于全新安装新版本，安装目录下的所有数据或配置文件都不会自动迁移。

Scoop 使用 `persist` 属性来配置处理这些需要持久化的数据或配置文件。

> 原理是安装时将这些文件保存在 `scoop/persist/<app>` 目录下，应用版本目录下创建相应的目录联接（对于目录）或硬链接（对于文件）。

注意：持久化的数据通常是安装后就存在的文件（夹），如果不存在会被视为目录处理。

> 有些应用的配置文件既不在文件夹中，也并非安装后就存在，而是在首次运行时创建的。
>
> 对于这种配置文件，持久化的一种思路是，在安装前自行创建该配置文件。详见 [MouseInc 应用清单示例](#mouseinc)。

### 持久化配置

配置应该如下面这样：

```json
{
    "persist": [
        "keeps_its_name",
        ["original_name", "new_name_inside_the_data_dir"]
    ]
}
```

`persist` 属性可以是一个字符串，当且仅当只有一个文件（夹）需要持久化时；通常，它是一个数组，列出了所有需要持久化的文件（夹）。当然，可以为持久化目录中的文件（夹）取不同的名字，像上面配置示例那样给出一个两个元素的子数组，两个元素分别指定原名称和数据存储别名即可。

### 卸载时清理数据

另一点需要注意的是，带有持久化数据的应用在被卸载时，默认不会删除持久化数据，除非卸载命令指定 `-p`（purge）标志。

```powershell
scoop uninstall -p <app>
```

## 配置参考

Scoop 提供了多个 bucket，每个中都有很多应用清单可供参考。并且这些 bucket 都被同步到了本地，所以，很方便地就可以进行浏览查看。

main bucket 位于：`scoop\apps\scoop\current\bucket\`，其他添加的 bucket 位于：`scoop\buckets\`。

## 共享应用

如果要分享某个应用给他人的话，可以有以下方法：

* 将 bucket 共享给对方添加
* 通过本地或共享的应用清单文件安装，如：`scoop install \\shared\file\to\hello.js`
* 通过应用清单的 URL 安装，如：`scoop install https://path/to/hello.json` 

> 从后两种安装方法可以看出，事实上 bucket 不是必须的，重点是将应用清单传给 `scoop install` 命令。

# 应用清单示例

## [PowerShell ](https://github.com/PowerShell/PowerShell)

涉及知识点：

* GitHub 仓库版本检测。`"checkver": "github"`
* `autoupdate.architecture.xxbit.url` 属性中引用版本变量 `$version`。
* `autoupdate.architecture.hash.find` 属性中引用文件名变量 `$basename`。

```json
{
    ...
    "checkver": "github",
    "autoupdate": {
        "note": "Thanks for using autoupdate, please test your updates!",
        "architecture": {
            "64bit": {
                "url": "https://github.com/PowerShell/PowerShell/releases/download/v$version/PowerShell-$version-win-x64.zip"
            },
            "32bit": {
                "url": "https://github.com/PowerShell/PowerShell/releases/download/v$version/PowerShell-$version-win-x86.zip"
            }
        },
        "hash": {
            "url": "https://github.com/PowerShell/PowerShell/releases/latest",
            "find": "(?:$basename)(?:[\\s\\S]*?)([a-fA-F0-9]{64})"
        }
    }
}
```

> 完整配置参见 [这里](https://raw.githubusercontent.com/ericzong/ericzone/master/powershell.json)。

上例中，由于 [PowerShell  ](https://github.com/PowerShell/PowerShell) 发布于 GitHub，`homepage` 即是其仓库地址，所以，`checkver` 属性指定为 `github` 即可。

`autoupdate` 属性中的 `url` 通常会引用版本变量 `$version`，以生成更新的下载地址。详见 [这里](https://github.com/lukesampson/scoop/wiki/App-Manifest-Autoupdate#add-autoupdate-to-a-manifest)。

示例中，哈希码的获取使用了一些技巧。

首先，`url` 使用的是 latest 路径，这里展示的也是最后一个正式版本，与 `checkver` 查找对应。（可以看下这个页面的结构）

匹配的正则表达式中使用到了 `$basename` 变量，它代表下载地址中的文件名。因为 PowerShell 一个正式版本发布包含多个系统及位数的文件，所以需要文件名去匹配。注意文件名后换了一行列出 SHA256 哈希码，因此使用 `(?:[\\s\\S]*?)` 匹配换行。这里量词不能是 `*`，否则会匹配到最后一个哈希码，应使用 `*?` （勉强匹配）。

## Q-Dir

涉及知识点：

* `extract_dir` 属性指定解压文件夹。
* `persist` 属性持久化数据。
* `checkver` 属性为正则表达式。

```json
{
    ...
    "extract_dir": "Q-Dir",
    ...
    "persist": [
        "Favoriten",
        "Q-Dir.ini"
    ],
    "checkver": "Q-Dir ([^\\ ]+)",
    "autoupdate": {
        ...
        "hash": {
            "url": "https://www.softwareok.com/?Download=Q-Dir",
            "find": "$basename.*?([a-fA-F0-9]{64})"
        }
    }
}
```

> 完整配置参见 [这里](https://raw.githubusercontent.com/ericzong/ericzone/master/qdir.json)。与 PowerShell   类似的配置就不再赘述了。

首先，注意 Q-Dir 配置了 `extract_dir` 属性，但 PowerShell 没有。这是因为，PowerShell   压缩包直接包含所有文件，而 Q-Dir 压缩包包含的是 Q-Dir 目录，所以，需要 `extract_dir` 属性指明解压出 Q-Dir 目录中的文件。

其次，配置了 `persist` 属性，这是指定持久化数据的。这里主要是为了保存 Q-Dir 的配置文件和快捷目录。

`checkver` 属性是一个正则表达式，它从 `homepage` 中匹配版本信息。

Q-Dir 的文件名与 HASH 码间没有换行，所以使用 `.*?` 匹配即可。

## Typora

涉及知识点：

* 安装程序配置
* 添加快捷方式

配置方案一：

```json
{
    ...
    "innosetup": true,
    ...
}
```

Typora 的特殊之处在于它只有安装版，没有便携版。因此，我们需要执行安装程序。

不过，由于 Typora 安装程序是基于 InnoSetup 的，所以直接将 `innosetup` 属性设置为 `true` 即可。

配置方案二：

```json
{
    ...
    "installer": {
        "args": [
            "/VERYSILENT",
            "/SUPPRESSMSGBOXES",
            "/NOCANCEL",
            "/NORESTART",
            "/CLOSEAPPLICATIONS",
	"/GROUP=typora",
            "/DIR=$dir",
            "/LOG=$dir/install.log"
        ]
    },
    "uninstaller": {
        "file": "unins000.exe",
        "args": "/VERYSILENT"
    },
    "shortcuts": [
        [
            "Typora.exe",
            "Typora"
        ]
    ]
}
```

另外，我们也可以通过 `installer` 和 `uninstaller` 属性来指定安装和卸载行为。由于 Typora 提供了命令行安装与卸载接口，因此，配置方案二将具有更多的控制权。

安装文件是确定的，所以只需要 `installer.args` 指定选项即可；而卸载不同，还需要 `uninstaller.file` 指定卸载程序文件。

> 通常，安装/卸载程序都应提供命令行接口，并且有静默安装/卸载的选项，否则将不能自动化的安装与卸载，即不适合用这种方式管理。

此外，我们还通过 `shortcuts` 属性来为 Typora 在开始菜单中添加了快捷方式。

## MouseInc

涉及知识点：

* `persist` 属性持久化数据。
* `pre_install` 预安装脚本，在应用安装前执行。

```json
{
	"homepage": "https://shuax.com/project/mouseinc/",
	"url": "https://dl.shuax.com/MouseInc2.10.21.7z",
	"version": "2.10.21",
	"license": "Freeware",
	"extract_dir": "MouseInc",
	"checkver": "MouseInc ([\\d.]+)",
	"autoupdate": {
		"url": "https://dl.shuax.com/MouseInc$version.7z"
	},
	"pre_install": "ni \"$dir\\MouseInc.json\"",
	"persist": [
		"MouseInc.json"
	],
	"depends": "",
	"hash": "fdb6dc659dc583fc3c7de911910defdae80b2c593180b6039f36d348ef345bb5",
	"bin": [
		[
			"MouseInc.exe",
			"mouseinc"
		]
	]
}
```

关于 MouseInc，我们要讨论是其配置文件的持久化。

MouseInc 配置文件的特殊之处在于，该文件是在首次运行时生成的。因此，如果只配置 `persist` 属性，那么安装后会发现配置文件被 `scoop` 视为文件夹处理了。而 MouseInc 启动时将不能再创建同名的文件，进而导致一些莫名的问题。

想要正确处理这种运行时生成的配置文件，一种方案是使用预安装脚本事先创建配置文件，这样，`scoop` 就可以正确处理了。

从 MouseInc 的应用清单可见，`pre_install` 属性配置了预安装脚本，该脚本会在应用安装前执行。

```powershell
# "ni \"$dir\\MouseInc.json\""  即
ni $dir\MouseInc.json
```

注意：由于脚本是以 json 值的形式提供的，所以，双引号、反斜杠等特殊符号均需进行转义。

> 脚本中可以使用一些相关变量，比如本例中的 `$dir` 即当前安装应用的目录，更多可用变量参考[这里](https://github.com/lukesampson/scoop/wiki/Pre--and-Post-install-scripts)。
>
> 另，`post_install` 属性可配置应用安装后执行的脚本。

# 参考资源

[Scoop Wiki - Buckets](https://github.com/lukesampson/scoop/wiki/Buckets)

[Scoop Wiki - App Manifests](https://github.com/lukesampson/scoop/wiki/App-Manifests)

[Scoop Wiki - Optional Properties](https://github.com/lukesampson/scoop/wiki/App-Manifests#optional-properties)

[Scoop Wiki - Creating an app manifest](https://github.com/lukesampson/scoop/wiki/Creating-an-app-manifest)

[Scoop Wiki - Autoupdate](https://github.com/lukesampson/scoop/wiki/App-Manifest-Autoupdate)

[我的 bucket](https://github.com/ericzong/ericzone)

