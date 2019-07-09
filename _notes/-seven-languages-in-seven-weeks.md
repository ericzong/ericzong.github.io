---
layout: page
title: 七周七语言：理解多种编程范型
category: 程序语言
author: Eric Zong
---

* content
{:toc}
# 第2章 Ruby

## 2.1 Ruby简史

松本行弘（Yukihiro Matsumoto），1993 年发明 Ruby，称他为 Matz。

从语言角度看，Ruby 出身于所谓的脚本语言家族，是一种解释型、面向对象、动态类型的语言。解释型，意味着 Ruby 代码由解释器而非编译器执行。动态类型，意味着类型在运行时而非编译时绑定。

## 2.2 第一天：找个保姆

### 2.2.2 从命令行执行Ruby

交互命令行：irb

```ruby
puts 'hello, world'
language = 'Ruby'
puts "hello, #{language}"
```

> 声明是指使用某变量之前先宣称该变量存在，或指出该变量的类型。对于 Python、Ruby 这样的语言来说，声明毫无意义，因为变量无需声明，即可直接初始化及赋值。

单引号包含的字符串表示它将不加改动地直接解释，双引号包含的字符串则会引发字符串替换。字符串替换是 Ruby 解释器所做的一种求值。

### 2.2.3 Ruby的编程模型

Ruby 是一门纯面向对象语言。

```ruby
4.class
=> Fixnum
4.methods
```

在 Ruby 中，一切皆为对象，就连每个单独的数字也不例外。

### 2.2.4 判断

Ruby 中有取值为 ture 或 false 的表达式。和其他语言一样，Ruby 中的 true 和 false 也是一等对象（first-class object）。

> 一等对象应具有以下几项性质：可存储于变量或数据结构中；可作为参数传递给函数；可作为返回值从函数返回；可在运行时创建。

```ruby
unless x == 4
    puts 'This appears to be false.'
else
    puts 'This appears to be true.'
end
```

当你使用 if 或 unless 时，既可选用块形式（if condition，statements，end），也可选用单行形式（statements if condition）。

```ruby
x = 10
x = x - 1 until x == 1
while x < 10
    x = x + 1
    puts x
end
```

Ruby 中，每种对象都有自己特有的相等概念。数字对象的值相等时，它们相等。

```ruby
puts 'This appears to be true.' if true
```

除了 `nil` 和 `false` 外，其他值都代表 `true`。0 也是 `true`。

```ruby
true and false
true or false
false && false
true || this_will_not_cause_an_error
true | this_will_cause_an_error
```

### 2.2.5 鸭子类型

Ruby 是强类型语言，这意味着发生类型冲突时，你将得到一个错误。Ruby 在运行时而非编译时进行类型检查的。

利用鸭子类型，实现对接口编码更简单。