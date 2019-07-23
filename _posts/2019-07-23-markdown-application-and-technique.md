---
layout: post
title: Markdown应用与技巧
category: 工具
tags: Markdown 专题
excerpt: Markdown中高级应用及使用技巧。
date: 2019-07-23
author: Eric Zong
---

* content
{:toc}
# 注释

有时我们会想在 Markdown 文档中保留一些文本，但又不想在输出网页中可见，甚至最好在网页源码中也没有。

## 缺陷方案

最简单的方案是使用 HTML 原生注释 `<!-- HTML 注释 -->`，其缺点是其会在源码中保留。

如果你使用 Jekyll 那么可以：

```liquid
{% raw %}{% comment %} 
    These commments will not include inside the source.
{% endcomment %}{% endraw %}
```

由于这种注释会在 Markdown 处理器执行之前被移除，所以在源码中也不可见。唯一的问题在于环境依赖。

## 通用方案

比较通用的方案在使用链接标签：

```markdown
[comment]: <> (This is a comment, it will not be included)
[comment]: <> (in  the output file unless you use it in)
[comment]: <> (a reference style link.)
```

或者可以指定一个更特殊的标识符：

```markdown
[//]: <> (This is a comment)
```

为了更好的兼容性，应使用 `#` 代替空链接 `<>`，因为 `#` 是一个合法的超链接：

```markdown
[//]: # (This may be the most platform independent comment)
```

## 语法要点

* 链接标签前后的空白行可能是重要的
* 链接可用尖括号包裹（可选）
* “可选标题”（即用作注释文本部分）可用圆括号、单引号或双引号等包裹

## 参考资料

* [stackoverflow - Comments in Markdown](https://stackoverflow.com/questions/4823468/comments-in-markdown)
* [Markdown 链接语法](https://daringfireball.net/projects/markdown/syntax#link)

# base64 图片

在 Markdown 中插入外源图片会导致内容分离，而如图床失效等原因，将导致内容丢失。

一个可选的解决方案是将图片文本化编码，即 base64。

在 Markdown 中有两种插入图片的方式，都支持插入 base64 编码的图片：

1. 直接插入。语法为：`![image](base64)`。
2. 插入标签并引用。引用语法：`![image][id]`；标签语法：`[id]: base64`。

## 编码解码

有很多在线编码的工具站，可以在线转换。如果熟悉编程，也可以编写程序进行转换。

> base64 图片编码有类似的统一前缀：`data:image/png,base64,`。

下面是 Python 代码：

```python
# 编码
import base64
f = open(path, 'rb')
ls_f = base64.b64encode(f.read())
f.close()
print(ls_f)
# 解码
bs = '...'
imgdata = base64.b64decode(bs)
file = open(path, 'wb')
file.write(imgdata)
file.close()
```

## 问题

使用 base64 编码的图片，固然使得图文一体，但是也有一个明显的缺点：编码信息巨长。

这带来两个问题：

1. 编辑文档时会有更多的滚动；
2. 可能导致编辑器性能问题[^1]。

[^1]: 目前来看，Typora 存在较严重的性能问题，滚动易卡死，即便是仅插入一个 base64 编码的图片也是如此；Mark Text 性能尚可，但仍有轻微可察的卡顿。