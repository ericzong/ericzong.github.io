---
layout: post
title: hexo入门
category: 工具
tags: hexo 入门
excerpt: "hexo入门教程。"
author: "Eric Zong"
date: 2020-07-08
---

* content
{:toc}

# 概述

什么是 Hexo？套用 [Hexo 官方中文文档](https://hexo.io/zh-cn/docs/) 的原话：

> Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。 

# 安装

Hexo 依赖 Node.js 和 Git，因此，应确保已事先安装好了它们。然后执行安装命令：

```bash
npm install -g hexo-cli
```

> **局部安装**
>
> 上面的命令是全局安装，如果希望局部安装有两种方式。
>
> 1. 手动修改 `path` 环境变量
>
> 在希望安装的目录执行 `npm install hexo` 并将该目录下的 `node_modules/.bin`  目录添加到 `path` 环境变量。
>
> 2. 使用 `npx` 执行 `hexo` 命令
>
> `npx` 可以调用项目内部安装的模块，更多说明参见 阮一峰老师的[《npx 使用教程》](http://www.ruanyifeng.com/blog/2019/02/npx.html)。
>
> 因此，如果当前路径正处于某个 Node.js 项目下，而该项目已经安装了 hexo，那么 `npx` 就可以直接执行 hexo 命令。
>
> 但如果找不到可供调用的 hexo 命令，那么 `npx` 会临时下载一个执行，完成后删除（显然效率较低）。
>
> ---
>
> **全局 vs. 局部**
>
> 全局安装最明显的优点就是处处可调用，而局部安装调用就会麻烦一点，但环境“干净”一些——对于一些人而言，这一点更重要，当然笔者也在其列 (^_^)。

# 上手

hexo 使用以下格式调用其子命令：

```bash
hexo <command>
```

对于命令行工具而言，`help` 命令是上手最好的切入点：

```bash
hexo help [command]
```

> **使用 npx**
>
> `npx` 调用 hexo 直接在以上命令前加 `npx` 即可。如：`npx hexo <command>`、`npx help [command]`……

# 创建站点

要创建自己博客，第一步当然是构建基本结构，执行以下命令可以创建一个站点目录：

```bash
hexo init hexo-demo
```

执行成功后，站点目录下会生成如下一些主要的文件夹和文件：

| 文件(夹)     | 说明         |
| ------------ | ------------ |
| _config.yml  | 网站配置文件 |
| package.json | npm 配置文件 |
| scaffolds    | 模板文件夹   |
| source       | 资源文件夹   |
| themes       | 主题文件夹   |

此时，其实站点已经是可以本地运行了，执行以下命令即可：

```bash
hexo server
# 可缩写为
hexo s
```

按命令提示在浏览器中访问 `http://localhost:4000` 即可看到“Hello World”页面了。

> **指定端口**
>
> 默认端口是 4000，如需修改，使用 `--port` 或 `-p`（缩写）选项指定。如：`hexo s -p 6666`。

# 编写文章

博客的基本结构已经搭好了——如果你不关注外观的话——接下来，就是编写自己的文章了。

首先，当然是创建一个“空白”文件开始编写文章了，执行以下命令：

```bash
hexo new "The first post"
```

这样，在 `<hexo>/source/_posts/` 目录下就会创建一个名为 `The first post.md` 的文件，用编辑器打开并进行编辑保存。

还记得吗，我们没有关闭 hexo 的服务，它默认会监听文件变更，所以一旦保存文章就会发布，刷新网页就可以看到新增的文章。

# 中途的小结

至此，我们已经在本地搭建了一个博客服务，并可以本地浏览。上文所做的一切，基本上就是达成该目标最小化的操作了。

还缺什么？

1. 使用了默认的配置，因此博客站点没有任何标识性的信息；
2. 创建文章的模板是默认的，通常需要更多模板化的信息；
3. 使用的是默认主题，它简单但不美观；
4. 默认情况下，页面基础文本是英文的；
5. 没有置顶功能；
6. 只能本地浏览，如何发布到网络？
7. ……

# 定制文章

## new命令

话说 `new` 命令的一般形式是下面这样的：

```bash
hexo new [layout] <title>
```

它使用一个模板来生成新的文件。

## 关于模板文件

模板存在于 `<hexo>/scaffolds` 目录下，默认情况下该目录下有 `post.md`、`page.md`、`draft.md` 三个文件，即对应三种模板；生成的文件，所在路径根据使用的模板不同而不同；生成的文件名，默认为标题，后缀为 `.md`。

| 布局  | 模板文件 | 路径             | 示例命令            | 示例文件生成路径       |
| ----- | -------- | ---------------- | ------------------- | ---------------------- |
| post  | post.md  | `source/_posts`  | `hexo new post pf`  | `source/_posts/pf.md`  |
| page  | page.md  | `source`         | `hexo new page pp`  | `source/pp/index.md`   |
| draft | draft.md | `source/_drafts` | `hexo new draft df` | `source/_drafts/df.md` |

大多数情况下，我们会在模板文件中定制 Front Matter 以免重复编写，大致内容如下：

```yaml
---
layout: {{ layout }}
title: {{ title }}
excerpt: "简介"
categories: 
tags: 
date: {{ date }}
updated: {{ updated }}
---
```

> 除 Front Matter 外，当然可以添加其他内容，这视需求而定。更多可参考官方中文文档 [Front Matter 章节](https://hexo.io/zh-cn/docs/front-matter)。
>

## 生成文章与显示效果

值得注意的是，模板不仅影响生成文章的初始内容及路径，还会影响其显示效果。

**post 模板**

通常而言，使用 post 模板生成的文件就是常规意义上的帖子，它会出现在文章列表中。

> **自定义模板**
>
> 自定义模板相当简单，只需在 `<hexo>/scaffolds` 目录下创建一个 Markdown 文件即可。比如：创建一个 `mydoc.md` 文件，这样就有了一个名为 `mydoc` 的模板。
>
> 自定义模板效果与 post 模板相同，生成文件的路径等都一样，仅仅是内容上的差异。
>
> **缺省值**
>
> `new` 命令中模板参数是可以省略的，默认缺省值为 `post`。缺省值是在 `_config.yml` 中由 `default_layout` 参数指定。

**draft 模板**

draft 模板生成的是草稿，即还未发布的帖子。草稿文件存放于 `source/_drafts` 目录——该目录初始并不存在，会在创建首个草稿时自动创建——默认启动 hexo 服务，草稿不会显示在文章列表中，除非启动命令添加了 `--draft` 选项。

**page 模板**

page 模板生成普通页面，并不显示在文章列表中，但可以在帖子中作为链接显示。

> 使用 `page` 可创建 about 页。如：`hexo new page about`。

## 发布草稿

使用 draft 模板生成的草稿需要通过发布操作成为正式的帖子。本质上来说，就是将草稿文件从 `_draft` 目录发布到 `_posts` 目录。这可以手动操作，但 hexo 提供了专门的命令：

```bash
hexo publish [layout] <filename>
```

## 文件名

`new` 命令中 title（标题）是必需指定的。默认情况下，生成的文件名就是指定的标题。

> **配置**
>
> 新建文件名由 `_config.yml` 中的 `new_post_name` 参数决定，其默认配置为 `:title.md`，所以文件名默认是标题。
>
> 如果是从 Jekyll 迁移到 hexo，或者更偏好类似 Jekyll 的命名方式，可以使用 `:year-:month-:day-:title.md`。
>
> 但是，如果你的标题是中文而又想文件名是纯英文的，或者单纯想自定义文件名，那么，可以为 `new` 命令添加 `--path` 选项以指定文件路径及名称。比如：`hexo new post --path "dir/my-doc" "我的标题"`。
>
> **特殊符号**
>
> 文件名是不能包含某些特殊字符的，但是指定的标题可以是任意的。这时，特殊字符会转换为“-”。但是，如果你想通过 `--path` 创建一个文件名非法的文件，那么，你可能会得到一个错误。

# 定制主题

hexo 提供了很多现成的主题，你只需要在 [官方主题页面](https://hexo.io/zh-cn/docs/themes)  浏览选择你喜欢的主题，并从主题所在的 GitHub 库中下载它，并将主题文件夹拷贝到 `<hexo>/themes` 目录下即可使用。

使用主题只需要配置 `_config.yml` 的 `theme` 参数即可。

以 next 主题为例。首先，我们从 [GitHub 库](https://github.com/theme-next/hexo-theme-next) 将其代码 Clone 或下载 ZIP 压缩包（需解压）到本地；然后，将主题文件夹重命名为 `next` 并拷贝到 `<hexo>/themes` 目录；最后，将 `_config.yml` 的 `theme` 参数设置为 `next`（即主题文件夹名）。

> 如果你是一位前端开发人员，想要完全自己写一个主题，可参考官方中文文档 [主题章节](https://hexo.io/zh-cn/docs/themes)。
>

# 更多的操作命令



# 发布到网络





如果按步骤一边搭建一边查看帮助，会发现 hexo 的帮助是上下文相关的，在不同情况下查看帮助显示的命令列表可能是不同的。



# 附录

## 命令参考



