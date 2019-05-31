---
layout: post
title: "YAML入门"
category: 综合
tags: YAML 入门
excerpt: "YAML入门教程。"
author: "Eric Zong"
---

* content
{:toc}

# 概述

YAML 是一个人类可读的数据序列化语言。

常用于配置文件。

YAML 使用 Unicode 编码作为字符标准编码，如 UTF-8。

## 与其他语言的关系

* YAML 1.2 是 JSON 的超集。
* 使用 Python 风格的缩进表示嵌套。
* 数据类型基于 Perl。
* 以 `:` 为中心键值对语法来源于电子邮件头（RFC 0822）。
* 文档分隔符 `---` 借鉴自 MIME（RFC2045）。
* 转义序列重用 C 的。
* 多行字符串空白包装（whitespace wrapping）继承自 HTML。
* 预期在流（stream）中读写，源自 SAX。

## 支持情况

* 能被几乎所有的主流语言读写。
* 许多编辑器或 IDE 都提供支持。

# 语法

* 空白缩进代表结构，但不允许使用 Tab。
* 注释以 `#` 开始，且“#”之前至少要有一个空白字符。
* 列表项用 `-` 列出，或者列表用 `[]` 列出并用 `,` 分隔。
* 映射项用 `: ` 列出，或者映射用 `{}` 列出并用 `,` 分隔。如果键有 `?` 前缀，则可以是复杂类型。
* 字符串可以直接原样使用（不用引号），也可以使用双引号和单引号，双引号支持转义。
* 块标量（block scalar）以缩进定界，使用 `|` 保留或 `>` 折叠换行。
* 多文档流使用 `---`（三个连字符）开始一个文档，并使用可选的 `...`（三个句点）结束一个文档。
* 使用 `&` 定义一个锚，并使用 `*` 引用它。
* 节点可 `!!` 后跟类型指定数据类型或 `!` 后跟标签（tag）标记。
* 文档前可能有指令，YAML 1.1 定义了两个指令：`%YAML`（指定版本），`%TAG`（URI 前缀缩写）。

# 基本组件

## 数据结构

```yaml
# 序列（block sequence）
- Item1
- Item2
- Item3
# flow style
[Item1, Item2, Item3]

# 映射（Mapping）
key1: value1
key2: value2
# flow style
{key1: value1, key2: value2}
# 复杂键
? [male, female]: gender
# JS: { '[object Object]': null }
```

## 文本

```yaml
# 多行文本
# 保留换行
multi-preserving: |
  这是一个多行文本
  换行会被保留
# JS: { 'multi-preserving': '这是一个多行文本\n换行会被保留\n' }
# 折叠换行
multi-fold: >
  多行文本会被合并为一行
  换行会替换为空格
  
  空行分段
# JS: { 'multi-fold': '多行文本会被合并为一行 换行会替换为空格\n空行分段\n' }

# 标量
# 1. plain style
plain-text: plain text
# 2. quoted style
# 2.1 double-quoted style（支持转义）
double-quoted: "支持'转义'	前面有一个\"\\t\"，后面有\"\\n\"

新的一行"
# JS: { 'single-quoted': '支持\'转义\'\t前面有一个"\\t"，后面有"\\n"\n新的一行' }
# 2.2 single-quoted style
single-quoted: '不支持''转义''	前面有一个"\t"，后面有"\n"

新的一行'
# JS: { 'single-quoted': '支持\'转义\'\t前面有一个"\\t"，后面有"\\n"\n新的一行' }
```

## 文档结构

```yaml
--- # 文档开始
# 文档内容（省略）
... # 文档结束
```

## 锚点引用

```yaml
name: &key Eric # 定义锚点
alias: *key     # 引用锚点
# { name: 'Eric', alias: 'Eric' }
```

# 高级特性

## 引用修改

```yaml
- step:  &id001        # 定义锚点 id001
    name: Eric
    age: 30
- step:
    <<: *id001         # 引用锚点 id001
    name: Lily         # 改写
    gender: female     # 增加
```

# 数据类型

数据类型分为三类：核心类（core）、定义类（defined）、用户定义类（user-defined）。

核心类是所有解析器都要实现的，比如：float、int、string、list、map……

定义类是高级数据类型，不一定所有解析器都会实现，比如：binary。

用户定义类是用户自行扩展的数据类型，可以是类（class）、结构（structure）、原始类型（primitive）。

下面是常见的内置类型：

| 内置类型  | 说明         |
| --------- | ------------ |
| int       | 整型         |
| float     | 浮点型       |
| bool      | 布尔型       |
| str       | 字符串       |
| binary    | 二进制       |
| timestamp | 日期时间类型 |
| null      | 空值         |
| set       | 集合         |
| seq       | 序列，列表   |
| map       | 映射         |
| omap      | 键值列表     |
| pairs     | 对象列表     |

YAML 能自动识别数据类型，通常不需要明确指定。

```yaml
octal: 0o52             # 八进制表示
hex: 0xFF               # 十六进制表示

float: 1.23e+6          # 浮点数
fixed: 42.5             # 固定小数
minmin: -.inf           # 表示负无穷
notNumber: .NaN         # 无效数字

null:                   # 空值
boolean: [true, false]  # 布尔值
string: 'text'          # 字符串

date: 2018-07-30                       # 日期
datetime: 2018-07-30T00:00:00.0Z       # 日期时间
iso8601: 2018-07-30T08:00:00.00+08:00  # iso8601 日期格式
spaced: 2018-07-30 08:00:00.00 +8  
```

但如果不想某个数据因为格式被误认为某种类型，可以明确指定数据类型。

使用“!”指定数据类型，单叹号表示自定义类型，双叹号表示内置类型。

```yaml
isString: !!str 2018-07-30     # 强调是字符串不是日期数据
picture: !!binary |            # Base64  图片
    R0lGODlhDAAMAIQAAP//9/X
    17unp5WZmZgAAAOfn515eXv
    Pz7Y6OjuDg4J+fn5OTk6enp
    56enmleECcgggoBADs=
```

# 参考

[YAML Reference card](http://yaml.org/refcard.html)

[YAML Wiki](https://en.wikipedia.org/wiki/YAML)

