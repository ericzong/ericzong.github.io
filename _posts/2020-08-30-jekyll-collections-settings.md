---
layout: post
title: Jekyll配置之Collections
category: 工具
tags: Jekyll配置
excerpt: 介绍在Jekyll中配置Collections。
author: Eric Zong
---

* content
{:toc}
# 什么是 Collections？

默认情况下，Jekyll 发布的文章存于 `_posts` 文件夹下，草稿存于 `_drafts` 文件夹下。

可见，文档都是集中管理的。如果想要分类分组管理，可配置 Collections。

> **文件夹即可归类**
>
> 事实上，如果只是想将文档放在不同的文件夹下归类，那么，仅在 `_posts` 下创建相关归类的文件夹即可。
>
> Jekyll 会递归扫描 `_posts` 下所有子文件夹，子文件夹下的文档跟直接置于 `_posts` 下没有差别。甚至，子文件夹下的文档发布得到的 URL 也相同，即不会包含子文件夹层级路径。而 Collections 会影响 URL 层级。

# 配置

Collections 的配置分为两种：分散配置和聚合配置。

## 分散配置

Collections 信息需要在 `_config.xml` 中配置，配置项为 `collections`，示例如下：

```yaml
collections:
  - references
  - notes
```

上面的配置项以列表形式给出各集合名，不过，如果集合带有元数据需要配置，那么应以“键-值对”方式给出，示例如下：

```yaml
collections:
  references:
    output: true
  notes:
    output: true
```

注意，集合对应的文件夹名应添加 `_` 前缀。比如上例中应分别创建文件夹 `_references`  和 `_notes` 以对应 `references` 和 `notes` 集合。

## 聚合配置

上面的配置方式文件夹是分散的，即根目录下会有多个独立的文件夹。如果想要聚合到一个文件夹下来管理，可以设置配置项 `collections_dir`，示例如下：

```yaml
collections_dir: my_collections
```

这样，就需要创建 `my_collections` 文件夹作为聚合的父文件夹，Jekyll 会搜索其中的文件，并将类似 `my_collections/_notes` 的文件夹识别为集合 `notes`。

注意，如果使用这种配置，所有的集合都将被聚合到指定文件夹。这意味着需要移动 `_drafts` 和 `_posts` 文件夹。

# 参考

[Jekyll Docs - Collections](https://jekyllrb.com/docs/collections/)