---
layout: post
title: Jekyll入门
category: 工具
tags: Jekyll 入门
excerpt: "Jekyll入门教程。"
author: "Eric Zong"
---

* content
{:toc}

# 概述

Jekyll 是一个简单的博客形态的静态站点生成器。

# 安装

由于 Jekyll 是基于 Ruby 的，因此首先需要安装 Ruby 的开发环境。

## Ruby

一个简单的方法是在 [RubyInstaller](https://rubyinstaller.org/downloads/) 下载 Ruby+Devkit 的安装文件进行安装。

但是，如果你像笔者一样使用 scoop 等集中安装管理软件的话，只需要执行以下命令即可：

```powershell
scoop install ruby
# Install MSYS2 via 'scoop install msys2' and then run 'ridk install' to install the toolchain!
ruby -v
scoop install msys2
msys2
ridk install
```

注意：如果 msys2 下载软件包较慢，可以参考 [FAQ]({{site.url}}/references/jekyll-faq.html#msys2-%E9%85%8D%E7%BD%AE%E6%BA%90) 为其配置源。

## Jekyll

使用 `gem install` 命令安装各种软件包即可：

```powershell
gem -v
gem sources -l
gem sources [--add | -a] http://gems.ruby-china.com/ [--remove | -r] https://rubygems.org/
gem install jekyll bundler
# 可能需要安装以下插件等
gem install jekyll-paginate
gem install jekyll-feed
gem install jemoji
```

# 基本使用

Jekyll 安装成功后，就可以使用 `jekyll` 命令创建项目了！

下面的命令即创建一个“空”项目，并启动服务：

```shell
jekyll new demo
cd demo
jekyll build
jekyll [serve | s] --port <port>
```

浏览器访问 `127.0.0.1:4000` 即可预览。

> 推荐的快速上手方式不是自己从头全新创建，而是找一个成品模板进行修改。
>
> 当然，你也可以直接参考本文所在的 [站点](https://github.com/ericzong/ericzong.github.io)。

# 资源与参考

[Jekyll 官网（英文）](https://jekyllrb.com/)

[Jekyll 官网中文](http://jekyllcn.com/)：中文（推荐，较完整）

* [参考站点](http://jekyllcn.com/docs/sites/)

* [Windows 运行Jekyll](http://jekyllcn.com/docs/windows/)

[Jekyll 官网中文](https://www.jekyll.com.cn/)：中文（部分模块翻译）

