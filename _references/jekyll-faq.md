---
layout: page
title: Jekyll常见问题汇总
category: 工具
date: 2019-06-05
author: "Eric Zong"
---

* content
{:toc}

# 中文路径

问题 1：默认情况下，jekyll 本地预览时，如果文档标题中有中文则路径中也就存在中文，此时服务器会因路径乱码问题导致该文档无法访问到。

解决该问题需要把服务器的路径编码改为 UTF-8，配置文件是 `RUBY_ROOT\lib\ruby\2.4.0\webrick\httpservlet\filehandler.rb` 。

请根据下面给出的上下文搜索并插入带有备注的行（两处）：

```
path = req.path_info.dup.force_encoding(Encoding.find("filesystem"))
path.force_encoding("UTF-8") # 加入编码

break if base == "/"
base.force_encoding("UTF-8") #加入編碼
```

问题 2：`Error:  incompatible character encodings: UTF-8 and GBK`

通常，当本地网站根目录路径包含中文字符时，会出现该问题。推测应该是 jekyll 内部默认使用 UTF-8 编码，但是系统路径默认是 GBK 编码，因此不兼容。

最简单的解决方案就是把网站置于英文路径下。

# msys2 配置源

默认情况下，msys2 下载软件包可能会很慢，可以修改以下几个配置文件修改其下载源：

```
# MSYS2_ROOT\etc\pacman.d\mirrorlist.mingw32
Server = http://mirrors.ustc.edu.cn/msys2/mingw/i686/
Server = https://mirrors.tuna.tsinghua.edu.cn/msys2/mingw/i686

# MSYS2_ROOT\etc\pacman.d\mirrorlist.mingw64
Server = http://mirrors.ustc.edu.cn/msys2/mingw/x86_64/
Server = http://mirror.bit.edu.cn/msys2/REPOS/
Server = https://mirrors.tuna.tsinghua.edu.cn/msys2/mingw/x86_64

# MSYS2_ROOT\etc\pacman.d\mirrorlist.msys
Server = http://mirrors.ustc.edu.cn/msys2/msys/$arch/
Server = https://mirrors.tuna.tsinghua.edu.cn/msys2/msys/$arch
```

# RubyGems 配置源

默认情况下，`gem install` 命令下载软件包可能较慢，可通过以下命令配置镜像源：

```powershell
gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/
```
