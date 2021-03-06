---
layout: page
title: 正则表达式参考
category: 编程综合
date: 2019-05-06
author: "Eric Zong"
---

* content
{:toc}

# 常见元字符

| 元字符 | 名称        | 代码点        | 作用                  |
| ------ | ----------- | ------------- | --------------------- |
| .      | 句点        | U+002E        | 匹配任意字符          |
| \      | 反斜线      | U+005C        | 对字符转义            |
| \|     | 竖线符      | U+007C        | 选择操作（或）        |
| ^      | 脱字符      | U+005E        | 行起始锚位符          |
| $      | 美元符      | U+0024        | 行结束锚位符          |
| ?      | 问号        | U+003F        | 匹配零次或一次的量词  |
| *      | 星号        | U+002A        | 匹配零次或多次的量词  |
| +      | 加号        | U+002B        | 匹配一次或多次的量词  |
| []     | 左/右方括号 | U+005B/U+005D | 字符组起始/结束       |
| {}     | 左/右花括号 | U+007B/U+007D | 量词或代码块起始/结束 |
| ()     | 左/右圆括号 | U+0028/U+0029 | 分组起始/结束         |

# 常见字符组简写

| 简写 | 等价形式            | 说明                                                         |
| ---- | ------------------- | ------------------------------------------------------------ |
| \d   | [0-9]               | 数字（digit）字符                                            |
| \D   | [^0-9]              | 非数字字符，\d 的反面                                        |
| \w   | [0-9a-zA-Z_]        | 单词字符，包括数字、字母和下划线                             |
| \W   | [^0-9a-zA-Z_]       | 非单词字符，\w 的反面                                        |
| \s   | [ \t\v\n\r\f]       | 空白（white space）字符，包括空格、水平/垂直制表符、换行符、回车符、换页符。 |
| \S   | [^ \t\v\n\r\f]      | 非空白字符，\s 的反面                                        |
| .    | [^\n\r\u2028\u2029] | 通配符，包括几乎所有字符，但排除换行符、回车符、行分隔符和段分隔符 |

# 常见量词简写

| 简写   | 等价形式 | 说明             |
| ------ | -------- | ---------------- |
| {m, n} |          | 匹配 m~n 次      |
| {m,}   |          | 匹配 m 次及以上  |
| {m}    | {m, m}   | 匹配 m 次        |
| ?      | {0, 1}   | 匹配 0 次或 1 次 |
| +      |          | 匹配 1 次及以上  |
| *      |          | 匹配 0 次及以上  |

# 常见的锚

| 表示 | 名称       | 说明                                        |
| ---- | ---------- | ------------------------------------------- |
| ^    | 开头       | 多行匹配模式下匹配行开头                    |
| $    | 结尾       | 多行匹配模式下匹配行结尾                    |
| \b   | 单词边界   | \w 和 \W 之间（包括 \w 与 ^、\w 与 $ 之间） |
| \B   | 非单词边界 | \w 与 \w、\W 与 \W、^ 与 \W、\W 与 $ 之间   |

# 环视分类

| 表示   | 名称                | 说明                          |
| ------ | ------------------- | ----------------------------- |
| (?=p)  | positive lookahead  | 位置前方/右边匹配给定子模式   |
| (?!p)  | negative lookahead  | 位置前方/右边不匹配给定子模式 |
| (?<=p) | positive lookbehind | 位置后方/左边匹配给定子模式   |
| (?<!p) | negative lookbehind | 位置后方/左边不匹配给定子模式 |

# 环视名称翻译对照

| positive lookahead | negative lookahead | positive lookbehind | negative lookbehind |
| ------------------ | ------------------ | ------------------- | ------------------- |
| 肯定顺序环视       | 否定顺序环视       | 肯定逆序环视        | 否定逆序环视        |
| 正前瞻             | 反前瞻             | 正后顾              | 反后顾              |
| 正向先行断言       | 负向先行断言       | 正向后行断言        | 负向后行断言        |

> 通常环视全称为 zero-width x assertion。
>
> 环视的限制方式分为肯定（positive）与否定（negative），通常翻译为：肯定、正、正向以及否定、反、负向。
>
> 环视的方向分为向前（lookahead）和向后（lookbehind），通常翻译为：顺序、前瞻、先行以及逆序、后顾、后行。

# 运算符优先级

| 运算符                 | 说明             | 优先级 |
| ---------------------- | ---------------- | ------ |
| \                      | 转义符           | 1      |
| () (?:) (?=) []        | 圆括号、方括号   | 2      |
| * + ? {m} {m, } {m, n} | 量词限定符       | 3      |
| ^ $ \元字符 一般字符   | 位置（锚）、序列 | 4      |
| \|                     | 管道符，或、分支 | 5      |

# 术语表

| 术语                                                         | 说明         |
| ------------------------------------------------------------ | ------------ |
| 标记符(flag)/修饰符(modifier)                                | i            |
| 分类表达式(bracketed expression)/字符组(character class)/字符集(character set) |              |
| 工作缓冲区(work buffer)/模式空间(pattern space)              |              |
| 环视(lookaround)                                             |              |
| 前瞻(lookahead)                                              |              |
| 后顾(lookbehind)                                             |              |
| 断言(assertion)/零宽度断言(zero-width assertion)             |              |
| 反向引用(backreference)                                      |              |
| 量词(quantifier)/限定符(bound)                               | ? * + {m, n} |
| 区间量词(interval quantifier)                                | {m, n}       |
| 选择操作(alternation)                                        | \|           |
| 灾难性回溯(catastrophic backtracking)                        |              |
| 转义字符(character escape)                                   |              |
| 文字(literal)/字符串字面值(string literal)/普通文本字符（normal character） |              |
| 特殊字符(special character)/元字符(metacharacter)            |              |
| 元字符序列(metasequence)                                     |              |
| 字符组(character class)/字符集(character set)/方括号表达式(bracketed expression) | [abc]        |
| 单词分界符(word boundary)                                    | \b \B        |
| 简记法(shorthand)                                            |              |
| 出现约束(occurrence constraint)                              |              |
| 可选元素(Optional Item)                                      |              |
| 宽松排列表达式(free-form expression)                         |              |
| 变量插值(variable interpolation)                             |              |

