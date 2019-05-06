---
layout: post
title: "PowerShell 入门"
categories: PowerShell
tags: PowerShell 入门
excerpt: "PowerShell 入门教程。"
author: "Eric Zong"
---

* content
{:toc}

# 概述

正则表达式是一种描述字符串结构模式的形式化表达方法。

它匹配的是字符或位置。因此，应按字符来解读正则表达式。

> 正则表达式不是编程语言，因此，它需要宿主环境实现，这可能是某种编程语言或是某种文本编辑器。
>
> 正则表达式流派众多，需要注意各种流派的差异。

# 字符匹配

最简单的字符自然就是描述它自身的字符，即“**普通字符**”。

> 普通字符，又叫：**literal（文字）**、**string literal（字符串字面值）**、**normal character（普通文本字符）**……
>
> 需要注意，正则表达式中，同一概念在不同的文档中所用的术语可能不同。

显然，普通文本字符组成的字符串严格来说就代表它自身，还算不上模式。要表示模式需要将一些字符作为特殊字符，用以描述模式。这类字符通常称为“**元字符**（metacharacter）”。

> 元字符，又称：**special character（特殊字符）**。

比如：定义字符组的方括号、用于分组的圆括号等都是元字符，后面会一一介绍。

元字符有特殊含义和作用，因此，如果要匹配元字符本身需要转义，转义前缀为 `\`。比如，要匹配圆括号应使用 `\(`、`\)`，要匹配转义前缀反斜杠需要用两个反斜杠 `\\`……

> 如果字符串中所有字符都应被视为普通字符，而又包含过多的元字符需要转义，那么最简单的方法是将字符串定义为**文字文本**，使用 `\Q` 和 `\E` 包围起来。这样，字符串中的元字符就会被视为普通字符，而不需转义。
>
> 比如，使用 `\Q\r\n\E` 可以匹配字符串 `\r\n` 而不是换行。

## 纵向模糊匹配

**纵向模糊匹配**是指一个正则表达式匹配的字符串具体到某一位字符时，可以不是某个确定字符，而是多种可能。

纵向模糊匹配的实现方式是**字符组（character class）**，从字面就可以看出它代表一组字符。

字符组使用一对方括号将一组可选字符括起来，比如：`[abcd]`。

> 字符组，也称“**字符集（character set）**”，由于使用方括号定义，也称“**方括号表达式（bracketed expression）**”。
>
> 注意：字符组罗列的一组可选字符，因此它匹配的是一个字符，而不是一个字符串；另外这些字符彼此是独立的，因此，其罗列顺序不重要。

显然，如果可选的字符过多，一一罗列会极为冗长繁琐，这时需要使用**范围表示法**。顾名思义，就是以表示范围的方式列举出可选字符。比如：`[a-z]` 即表示从 a 到 z 所有的小写字母。

> 能以范围表示法表示，说明这些字符都是具有“自然顺序”的，比如：数字、字母、Unicode 字符……
>
> 字符组基本上是一门独立的微型语言，有自己独立的元字符。
>
> 中划线（-），是**字符组元字符（character-class metacharacter）**，但只有在字符组内部才是。当中划线紧临中括号时，它仅仅是普通字符，比如：`[-az]`、`[az-]`。因此，如果字符组需要匹配中划线时，可以将其置于这些位置。但如果一定要将其放在将被视为元字符的位置，那么，可以对其进行转义，比如：`[a\-z]`。
>
> . 和 ? 不是字符组元字符。

正则表达式日常使用中，有一些字符组是固定但频繁使用的，比如数字字符、单词字符等等。显然即使使用范围表示法，还是稍显冗长了。因此，对于这些频繁使用的固定字符组会有对应的简写形式。如：`\d`（数字字符）、`\w`（单词字符）、`\s`（空白字符）……

这称为“**字符组简写式（character shorthand）**”。

> 字符组简写式，也称“**转义字符（character escape）**”。
>
> 字符组简写式可以出现在普通字符组中，如：`[\da-f]`（可表示一位 16 进制的数）。

不过，有时字符组中的字符很多，或者是不可列举的，但是从反面定义这样的字符组又相对容易。这时，我们需要使用“反义字符组”，定义方式与字符组类似，只是在左方括号后加一个脱字符（`^`），如：`[^abc]`。

**反义字符组**，也称“**排除（型）字符组**”。意为字符组代表的字符是排除所罗列的字符之外的字符。

简写形式的字符组也有反义形式，比如：`\D`（非数字字符）、`\W`（非单词字符）、`\S`（非空白字符）……

> 排除型字符组表示匹配一个未列出的字符，而不是不匹配。
>
> 注意通常所说的通配符 `.`（英文句点）不能匹配任意的字符，如果想匹配任意字符可使用：`[\d\D]`、`[\w\W]`、`[\s\S]`、`[^]`。基本原理是取两个互补的字符组的并集或对空字符组取反。

## 横向模糊匹配

**横向模糊匹配**是指一个正则表达式可匹配的字符串的长度不是固定的。

横向模糊匹配的实现方式是**量词（quantifier）**，其实就是重复。其标准形式为：`{m, n}`，表示重复匹配次数为 [m, n]。

实际使用中，可能会有一些特殊情况——只想限定次数下限、确定次数，以及有一些使用频率很高的量词。因此，需要提供一些简写形式。比如：`{m,}`（m 次及以上）、`{m}`（m 次）、`?`（0 或 1 次）、`+`（1 次及以上）、`*`（0 次及以上）……

默认情况下，量词是贪婪的，称为“**贪婪量词**”，此时，正则表达式也处于“**贪婪模式**”下。“贪婪”的含义是指会尽可能多地匹配字符。

> 多个贪婪量词挨着出现并相互有冲突时，优先匹配前面的。

贪婪匹配可能不总是我们想要的，这时，可在量词后加 `?`，将其转变为“**非贪婪量词**”，则会尽可能少地匹配字符。

> 非贪婪量词，也称“懒惰量词”或“惰性量词”。仍可能回溯。

还有一种量词，叫“**占有量词（possessive quantifier）**”，它会尽可能多匹配，覆盖整个目标然后尝试寻找匹配的内容，但只尝试一次，不会回溯。语法是在贪婪量词后加 `+` 后缀。

> 并非所有宿主环境都支持占有量词。

## 分支

**分支**，也称“**多选分支（alternation）**”，可选择匹配多个子模式之一。

分支使用 `|` 将可选子模式并列起来。如，`(lat|b)est` 可匹配字符串 latest 或 best。

> 分支结构是惰性的，即当前面的子模式匹配上时，后面的不再尝试。因此，编写分支时需要注意子模式的顺序。
>
> 分支仍可能回溯尝试其他分支。

# 位置匹配

如果只描述字符，那么，具有某些限制条件的正则表达式可能会很难编写，甚至可能无法实现。这时，就需要另一种匹配方式来补充某些限制了——这就是位置匹配。

**位置**，是相邻字符之间的位置。

> 想必大家都用过 Word 编写文档，那么，肯定也为文本应用过样式。你可能会认为样式是应用在文本之上的。这是一个误解！想想当你未选中任何文本是不是也可以应用样式呢？是可以的。其实，样式不是应用在文本之上的，样式的应用单位就是“位置”，即相邻字符之间的位置。
>
> 了解这点后，或许有助于理解“位置”这一概念。

## 开始与结尾

最为常见的位置就是开头与结尾，它们分别用 `^` 与 `$` 表示。

默认情况下，`^` 与 `$` 分别匹配的是给定文本的两头。所谓的默认情况，实际上是指“**单行模式**”，即文本被视为单行，也就只有一头一尾。

但实际情况是，给定的文本可能包含多行，而我们想要匹配每一行的开头与结尾，这时，正则表达式需要工作在“**多行模式**”下。多行模式下，`^` 与 `$` 分别匹配行开头与行结尾。则如果文本有 n 行，则应有 n 头 n 尾。

> 单行模式与多行模式的设置在不同语言中可能方式不同，比如在 JS 中使用 m 选项：`/^|$/gm`。

## 单词边界

`\b` 和 `\B` 分别代表**单词边界（word boundary）**和**非单词边界**。

比如，`\btest\b` 将匹配独立的单词 test，而不会匹配诸如 testcard 这种包含 test 的单词等。

## 环视

**环视（lookaround）**，限制位置前后内容需（不）匹配给定子模式。

环视给定了子模式，但是它会丢弃匹配内容，只关注是否匹配。因此，又称为“**断言**”。

> 环视丢弃了匹配内容，因此，它是非捕获分组。

从限制方向上看，可以限制位置左或右方；从限制方式上看，可以确定或否定匹配。因此，组合起来就有 4 种环视类型：肯定顺序环视（`(?=p)`）、否定顺序环视（`(?!p)`）、肯定逆序环视（`(?<=p)`）、否定逆序环视（`(?<!p)`）。

> 上述环视语法中的 `p` 指的是子模式。

# 捕获数据

正则表达式中，圆括号将导致分组，通常分组匹配的内容将被捕获，捕获的内容可以在正则表达式里反向引用，或在 API 里引用。

**捕获分组（capturing group）**默认情况下，每对圆括号都是一个分组，匹配的数据会捕获到该分组中。

显然，括号是可以嵌套的，当存在嵌套时，分组以开/左括号为准，即分组的索引号以开括号出现先后为准。

**反向引用（back reference）**，在正则表达式中，允许使用 `\index` 反向引用分组。

> 反向引用的不是子模式，而是子模式匹配的文本内容。
>
> 正则表达式引用不存在分组时，通常不会报错，而是匹配反向引用字符本身。
>
> 分组后有量词的话，分组最终捕获到的数据是最后一次的匹配。

通常情况下，使用括号会导致分组和数据捕获，通常这也是我们使用括号的目的，但有时，使用括号的目的不是这样，而只是这了提升优先级。

此时，我们使用括号但又不想分组，就需要使用**非捕获分组（non-capturing group）**，即在开括号后紧跟问号和冒号： `(?:p)`。

> 由于字符序列的优先级比分支要高，因此，对于一些分支结构而言需要使用括号提升其优先级。
>
> 比如，要匹配字符串 good 与 food，正则表达式不能写为 `g|food`，这样匹配的是 g  和 food，而应写为 `(g|f)ood`。
>
> 可是，这样将会导致分组捕获，而这不是我们需要的，要避免分组捕获正则表达式要写为非捕获分组的形式，即 `(:?g|f)ood`。
>
> 分组是可命名的，称为“**命名分组（named group）**”，语法大致是 `(?<name>p)`（并非所有宿主环境都支持，支持的语法也不尽相同）。
>
> **原子分组（atomic group）**，可关闭回溯，语法：`(?>p)`。

# 回溯法

**回溯（backtracking）**，正则表达式在匹配过程中会尝试逐步向前匹配，直至匹配失败，则会回退几个步骤，尝试其他匹配。

构造不佳的正则表达式在匹配过程中可能导致**灾难性回溯(catastrophic backtracking)**，使得匹配效率低下。

> 回溯法本质上是深度优先搜索算法。
>
> 正则引擎分两种：
>
> * NFA，非确定型有限自动机，表达式主导（regex-directed）引擎，特性：匹配慢，编译快
> * DFA，确定型有限自动机，文本主导（text-directed）引擎

# 构建正则

## 准确性

匹配预期的字符串，不匹配非预期的字符串。

## 可读性

正则表达式本身书写的可读性并不好，但是可以通过添加注释来增强可读性，语法为：`(?#comment)`。

> 如果想在注释中添加空格、制表符、换行等，需启用“忽略模式里的空白符”选项。此时，`#` 后至行尾都是注释。
>
> ```
> (?<=    # 断言要匹配的文本的前缀
> <(\w+)> # 查找尖括号括起来的字母或数字(即HTML/XML标签)
> )       # 前缀结束
> .*      # 匹配任意文本
> (?=     # 断言要匹配的文本的后缀
> <\/\1>  # 查找尖括号括起来的内容：前面是一个"/"，后面是先前捕获的标签
> )       # 后缀结束
> ```

## 效率

使用具体型字符组来代替通配符，来消除回溯。

使用非捕获型分组。

独立出确定字符。

> 加快判断是否匹配失败，加快移位的速度。

提取分支公共部分。

> 减少匹配中可消除的重复。

减少分支的数量，缩小它们的范围。

# 总结

## 关于字符匹配方式

字符匹配不外乎是纵向模糊匹配（字符组）、横向模糊匹配（量词）、分支以及它们的组合。

> **如何描述字符串？**
>
> 最简单的方式就是依次说出字符串的每个字符是什么，这就是确定描述。确定描述只能用于描述确定的某个字符串，而不能描述某类字符串。
>
> 如果想要描述某类字符串，就要使用正则表达式。本质上说，正则表达式是一种非确定的描述方式，描述的是字符串模式。但是，同样它需要逐位进行描述，只是不是用确定的某个字符来描述，而是用一系列的字符来描述，这就是字符组。
>
> 更进一步，如果字符串中某段序列中的字符都属于某个字符集合，那么就可以通过指明重复次数来进行简写，这就是量词。从某种程度上说，量词是一种简写方式，但并不仅限于简写。
>
> 编写正则表达式的底层逻辑大致是这样的：将目标字符串集合中的所有字符串逐一归纳为正则表达式（当然一个字符串归纳出的正则表达式不是唯一的），然后取正则表达式的<u>交集</u>求得统一的正则表达式。问题在于，求交集的结果可能是空，这时可行的方案就是把不能统一的部分分别列出来，作为可选项。这就是分支。
>
> 梳理一下：正则表达式使用字符组描述某位上的字符，使用量词描述可重用度，而分支将不能协调的子模式部分并起来。

## 关于位置匹配

位置匹配也称为“**锚（anchor）**”。

开头、结尾、单词边界等等是**简单锚**；而环视是**复杂锚**。

> **锚（anchor）**，又称“**锚位符**”或“**锚点**”。
>
> **如何描述位置？**
>
> 简单来说，位置是根据周围的字符确定的。
>
> **各类环视的名称**
>
> 如果你看了很多正则表达式的书籍或文章，那么会发环视的叫法很多。下面一一看下。
>
> **环视（lookaround）**，应该是字面最贴切的名称。而它只关注匹配与否，因此又称为**断言（assertion）**。又由于位置匹配都是**零宽（zero-width）**的，所以又称“**零宽断言（zero-width）**”。

如果一个正则表达式全是位置匹配，那么，其匹配结果是空字符串。

可以用连续多个位置匹配表达式来限制同一个位置，等价于交集运算。

位置是可以替换为字符的，这相当于插入操作。

## 正则表达式结构组成

正则结构一般可分为如下部件：

* 字面量。`a \n \.`
* 字符组。`[0-9] \d`
* 量词。`a{1, 3}`
* 分支。`abc|bcd`
* 锚。`^ \b (?=\d)`
* 分组。`(ab) (?:ab)`
* 反向引用。`\2`

## 正则表达式的使用

通常使用正则表达式目的有下列几种：

* 验证
* 切分
* 提取
* 替换

# 参考

《JavaScript正则表达式迷你书》

《学习正则表达式》

《精通正则表达式》

<https://www.regular-expressions.info/>

[正则表达式语言 - 快速参考（.Net）](https://docs.microsoft.com/zh-cn/dotnet/standard/base-types/regular-expression-language-quick-reference)

[regex laboratory](http://www.regexlab.com/zh/regref.htm)

[正则表达式30分钟入门教程](http://deerchao.net/tutorials/regex/regex.htm)
