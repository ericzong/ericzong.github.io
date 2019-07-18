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

### 2.2.7 第一天自习

[Ruby 中文官网](http://www.ruby-lang.org/zh_cn/)

[Ruby API 文档](http://ruby-doc.org/core-2.6.3/)

```ruby
# 字符串替换
'hello'.gsub(/o/, '*') # => "hell*"
```

[Ruby 正则表达式](https://www.runoob.com/ruby/ruby-regular-expressions.html)

[Ruby 区间（range）](https://www.runoob.com/ruby/ruby-range.html)

```ruby
# 输出“Hello, world.”
puts 'Hello, world.'
# 查找“Ruby”下标
'Hello, Ruby.'.index('Ruby')
# 打印 10 遍
puts 'Eric '*10
10.times {puts 'Eric'}
# 范围
(1..10).each do |num|
	puts "This is sentence number #{num}"
end
# 猜数字
number = rand(10)
begin
	n = gets.to_i
	puts "up" if number > n
	puts "down" if number < n
end while number != n

puts "Good"
```

## 2.3 第二天：从天而降

### 2.3.1 定义函数

每个函数都会返回结果。如果没有显式指定某个返回值，函数就将返回退出函数前最后处理的表达式的值。函数是对象。

### 2.3.2 数组

Ruby 1.9 新引入了有序散列表。

```ruby
animals = ['lions', 'tigers', 'bears']
animals[10]
# => nil
animals[-1]
animals[0..1]
```

为了便于使用而添加的特性就是语法糖。

```ruby
[1].class
# => Array
[1].methods.include?('[]')
# => true
# Ruby 1.9 使用[1].methods.include?(:[])
```

[] 和 []= 不过是访问数组的语法糖而已。

多维数组是数组的数组。

```ruby
a.push(1)
a.pop
```

### 2.3.3 散列表

```ruby
numbers = {1 => 'one', 2 => 'two'}
numbers[1]
stuff = {:array => [1, 2, 3], :string => 'Hi, mom!'}
stuff[:string]
```

Ruby 虽然不支持命名参数，但可以用散列表来模拟。

```ruby
def tell_the_truth(options={})
    if options[: profession]==: lawyer 
        'it could be believed that this is almost certainly not false.'
    else
        true 
    end
end
# => nil 
tell_the_truth 
# => true tell_the_truth: profession=>: lawyer
# => "it could be believed that this is almost certainly not false."
```

可选参数。将散列表用作函数的最后一个参数时，大括号可有可无。

### 2.3.4 代码块和 yield

代码块是没有名字的函数。可以作为参数传递给函数或方法。

```ruby
3.times {puts 'Eric'}
```

可采用 {/} 或 do/end 两种界定代码块的形式。惯例：代码块只占一行是用大括号，代码块战多行时用 do/end。

```ruby
animals.each {|a| puts a}
```

```ruby
class Fixnumdef
    def my_times
        i=self 
        while i>0
            i=i-1
            yield
        end
    end
end
# => nil
3.my_times {puts 'mangy moose'}
```

打开一个现有的类，并向其中添加一个方法。

代码块可用作一等参数（first-class  parameter）。

```ruby
def call_block(&block)
    block. call
end
# => nil
def pass_block(&block)
	ca11_block(&block)
end
# => nil 
pass_block {puts 'Hel1o, block'}
```

在Ruby中，参数名之前加一个“&”，表示将代码块作为闭包传递给函数。

在Ruby中，代码块不仅可用于循环，还可用于延迟执行。

编辑器：TextMate, vi, emacs。

### 2.3.5 定义类

单继承。根：Object。

一个 Class 同时也是一个 Module。Class 的实例将作为对象的模板。Fixnum 是 Class 的一个实例，而 4 又是 Fixnum 的一个实例。每一个类同时也是一个对象。

```ruby
class Tree 
  attr_accessor :children, :node_name 

  def initialize(name, children=[])
    @children=children
    @node_name=name 
  end 

  def visit_all(&block)
    visit &block
    children.each {|c| c.visit_all &block}
  end

  def visit(&block)
    block.ca11 self
  end
end 

ruby_tree = Tree.new("Ruby",
  [ Tree.new("Reia"), Tree.new("MacRuby")])

puts "Visiting a node"
ruby_tree.visit {|node| puts node.node_name}
puts 

puts "visiting entire tree"
ruby_tree.visit all {|node| puts node.node_name}
```

惯例和规则：类用骆驼命名法；实例变量前必须加 @；类变量前必须加 @@；实例变量和方法名小写字母开头，并采用下划线命名法（underscore_style）；常量全大写，并采用下划线命名法（ALL_CAPS）。

attr 定义实例变量和访问变量的同名方法，而 attr_accessor 定义实例变量、访问方法和设置方法。

