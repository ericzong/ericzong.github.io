---
layout: post
title: "SCSS 入门"
category: 前端
tags: SCSS 入门
excerpt: "SCSS 入门教程。"
author: "Eric Zong"
---

* content
{:toc}

# 简介

SCSS（Sassy CSS） 是 Sass 3 引入的新语法，完全兼容 CSS3。扩展名为 .scss。

> Sass（Syntactically Awesome StyleSheet） 是一种 CSS 扩展语言，它具有编程语言特性。扩展名为 .sass。
>
> 因此，Sass 有两种语法。一种是老的语法，另一种就是 SCSS 语法。后者是 CSS3 语法的超集，换句话说，一个 .css 文件可直接改为 .scss 文件。
>
> 使用 SCSS 不用了解 Sass 的老语法。

# 安装及使用

当然，浏览器是不认 .scss 文件的，需要使用 Sass 编译器将其编译为 .css 文件。

由于 Sass 基于 Ruby，因此，首先要确保安装了 Ruby。

> Sass 是语言名称，但同时它也指代与之使用相关的工具环境。

## 安装

```shell
gem install sass
```

> 安装成功后，建议先用 `sass --help` 查看下帮助。

## 命令行使用

```shell
# 将 .scss 编译为 .css 
sass input.scss output.css
# 监视文件
sass --watch input.scss:output.css
# 监视文件夹
sass --watch app/sass:public/stylesheets
```

> 风格参数：--styple <nested | expanded | compact | compressed>

两种语法间可以进行转换：

```shell
# Sass => SCSS
sass-convert style.sass style.scss
# SCSS => Sass
sass-convert style.scss style.sass
```

## 可选工具

如果不习惯命令行使用，也可以使用 IDE 工具。比如，[Koala](http://koala-app.com/)。

# 基础语法

SCSS 语法是 CSS3 的超集，因此，除了 CSS 的功能外，SCSS 还允许使用变量、嵌套规则、混合器、导入、函数、指令等。

## 变量

> 变量，是编程语言的基础组成部分。在 SCSS 中，声明变量主要是为了引用其值，做到一处声明，一改全改的目的。

SCSS 使用 `$` 作为变量前缀。

```scss
$font-stack: sans-serif;
// 以空格分隔的多个属性值
$basic-border: 1px solid black;
```

> 标准 CSS 属性值可存在的地方，变量就可存在。
>
> 变量分隔符可以是中划线（-）或下划线（_），并且它们是等效的，即 `$base-font` 和 `$base_font` 是同一变量。

可使用`!default` 将变量指定为**默认值**，在必要时可以赋值修改。

```scss
$baseFontSize: 16px !default;
```

变量的作用域分为 2 种：

* 全局作用域（定义于规则外或定义于规则内但使用 `!global` 修饰）
* 局部作用域（定义于规则内）

```scss
$color: green; // 全局变量
div.top {
	$height: 100px; // 局部变量
}

body {
	$maxWidth: 1024px !global; // 在规则内声明全局变量
}
```

> 该作用域与 JS 的作用域类似，采用的也是词法作用域，声明时确定。

## 数据类型

SCSS 主要的数据类型有：数字、字符串、颜色、布尔型、空值、数组（list）、map……

### 字符串

SCSS 中字符串可以是带引号或不带引号的：

* 有引号字符串（quoted strings）
* 无引号字符串（unquoted strings）

```scss
$string1: "string";
$string2: string;
```

> SCSS 也支持其他 CSS 属性值，如：Unicode 字符集、`!important` 声明等，它们被视为无引号字符串。
>
> 通常，编译时不会改变其类型。除非使用插值 `#{}`，有引号字符串将编译为无引号字符串，以便于在混合器中引用作为选择器名等。

### 数值

SCSS 中数值型是可以带单位的。

```scss
$number1: 10;
$number2: 10px;
```

### 布尔值

除 `false` 和 `null` 外，其他都被视为 `true`。

### 颜色

颜色值可以是：十六进制符号、rgb、rgba、hsl、hsla、关键词……

```scss
$color1: blue;
$color2: #fff;
$color3: rgba(0, 0, 0, .5);
$color: darken($color, 20%);
$color: (green + red);
```

### null

```scss
$null1: null;
```

> `null` 不能进行字符串连接，否则报错。
>
> 对于一个默认值变量而言，如果赋值为 `null` 则将视为未赋值，而使用其默认值。

### 列表

列表项与项之间可使用空格或逗号分隔。

```scss
$list: Helvetica, Arial, sans-serif;
$properties: (margin, padding);
```

> 最佳实践：
>
> 1. 除非过长，否则不应换行。
> 2. 除非适用于 CSS，否则应使用逗号分隔。
> 3. 除非为空或嵌套于另一列表，否则不使用括号。
> 4. 尾部不添加逗号。

### map

```scss
$map: (
    $key1: value1,
    $key2: value2,
    // ...
)
```

## 操作符

前面已经见过，变量赋值使用 `:`。

数值也是可以进行算术运算的， `+ - * / %` 等算术运算符都是可用的，而且还可以带单位运算。 

> 乘除法优先级高于加减法。
>
> 带单位的加、减运算，两个操作数的单位应该相同，否则报错。
>
> 带单位的乘、除运算必须得到合法的结果，否则报错。
>
> 由于 CSS 中允许使用 `/`，所以，可能 `/` 不被认为是除法运算符，除非表达式中出现变量、函数、圆括号、其他运算符等等。

所有算术运算都支持颜色值。 

> 十六进制的运算是对应位进行运算。
>
> rgba 运算是 `rgba()` 函数的对应参数进行运算。

字符串连接符是 `+`。

> 无引号字符串和有引号字符串混合连接时，结果是否有引号取决于左操作数。

SCSS 也可以使用常规的比较操作符： `==  !=  >  >=  <  <=`

逻辑运算符包括：`and`、`or`、`not`……

> 注意除了 `false` 和 `null` 之外，其他值都为 true。

`#{}` 称为插值（interpolation），本质上是占位符，它返回括号中表达式的值，并编译为无引号字符串可与左右内容拼接。

> 字符串中，通过插值可以添加动态值。比如：`"#{1+1}"`。

## 嵌套（nesting）

### 嵌套规则

SCSS 允许规则嵌套，这有利于避免重复的父选择器，以及复杂的 CSS 层级。

```scss
#main {
	h1 { // #main h1
		font-size: 1.5em;
	}
	h2 { // #main h2
		font-size: 1.3em;
	}
}
```

> 过度嵌套仍然会使得规则难以维护，最佳实践是：不过度使用嵌套。

### 引用父选择器

使用 `&` 可以引用父选择器。常见于嵌套规则中定义伪类。

```scss
#main {
	a {
		text-decoration: none;
		&:hover { // #main a:hover
			text-decoration: underline;
		}
	}
	
	&-note { // #main-note
		background-color: red;
	}
}
```

> 显而易见，新选择器相当于父选择器添加了一个后缀。但注意，它引用的是完整的父选择器。
>
> 有文档提及，当父选择器不能应用后缀时，将抛出一个错误。但是没有给出例子，笔者尚未发现这样的实例。

### 嵌套属性

CSS 中有一些属性在同一命名空间中，比如：font-family、font-size……可以简写命名空间：

```scss
p.note {
	font: {
        size: 1em; // font-size
		weight: bold; // font-weight
	}
}
```

甚至，命名空间也可以有值：

```scss
p.note {
	font: 12px fantasy {
		weight: bold;
	}
}
```

嵌套属性规则：

1. 属性名从中划线 - 断开；
2. 根属性（命名空间）后添加一个冒号 `:` 并紧跟一个块，把子属性部分写在块中。

## 混合器（mixin）

混合器允许重用整块 CSS、属性或者规则，甚至可以传参。

它使用 `@mixin` 指令声明，并使用 `@include` 指令调用。

```scss
@mixin border-radius($radius:3px){
  -webkit-border-radius: $radius;
  border-radius: $radius;
}
.btn {
  @include border-radius;
}
.box { 
  @include border-radius(50%);
}
```

> 混合器可以声明参数，并且可以为参数定义默认值。
>
> 本质上说，混合器定义的是一块命名的代码块，以便后续复用。

## 继承

使用 `@extent` 指令继承，可以继承某个规则或者占位符。

占位符由 `%` 定义，编译后占位符不会保留。

```scss
%placeholder {}

.con {}

.content {
    @extent %placeholder;
    @extent .con;
}
```

> 如果继承的规则或占位符不存在，就会报错。如果不关注这种问题的话，可以使用 `!optional` 标记该继承行为是“可选的”，以避免报错。

## 函数

### 自定义函数

```scss
@function function-name($args, $default:value, $rest...) {
    // ...
    @return $return-value;
}
```

> 函数名中的中划线 `-` 和下划线 `_` 等效，因此，`xxx-yyy` 和 `xxx_yyy` 是同一个函数。
>

### 内置函数

| 列表函数                              | 说明                                 |
| ------------------------------------- | ------------------------------------ |
| length($list)                         | 返回一个列表的长度值。               |
| nth(\$list, $n)                       | 返回一个列表中指定的某个标签值。     |
| join(\$list1, \$list2, [\$separator]) | 将两个列给连接在一起，变成一个列表。 |
| append(\$list1, \$val, [\$separator]) | 将某个值放在列表的最后。             |
| zip(\$lists…)                         | 将几个列表结合成一个多维的列表。     |
| index(\$list, \$value)                | 返回一个值在列表中的位置值。         |

| Map 函数                  | 说明                                                         |
| ------------------------- | ------------------------------------------------------------ |
| map-get(\$map,\$key)      | 根据给定 key，返回 map 中对应的 value。                      |
| map-merge(\$map1, \$map2) | 将两个 map 合并成一个新的 map。                              |
| map-remove(\$map,​\$key)   | 从 map 中删除一个key，返回一个新 map。                       |
| map-keys(\$map)           | 返回 map 中所有的 key。                                      |
| map-values(\$map)         | 返回 map 中所有的 value。                                    |
| map-has-key(\$map,\$key)  | 根据给定 key 判断 map 是否有对应 value，有返回 true，否则 false。 |
| .keywords(\$args)         | 返回一个函数的参数，这个参数可以动态的设置 key 和 value。    |

## 指令

### @import

SCSS 代码可分割为多个片段文件，并以下划线 `_` 作为文件前缀（如：\_partial.scss）。

片段文件不会被编译为 CSS 文件，而是由其他 SCSS 文件通过 `@import` 指令导入。片段文件内容将被插入其他文件中，然后整体被编译。

```scss
// _left.scss
body {
  // ...
}

// main.scss
@import 'left';
div.content {
  // ...
}
```

> `@import` 可省略文件扩展名。

由于 SCSS 完全兼容 CSS，因此，可以导入 CSS 文件：

```scss
@import "xxx.css";
@import "http://xxx.com";
@import url(xxx);
```

### @if...@else...

```scss
@if [not] $bool {

} @else if {

} @else {
    
}
```

### @for

```scss
// 包括 end，[start, end]
@for $i from (start) through (end)
// 不包括 end，[start, end)
@for $i from (start) to (end)
```

### @while

```scss
$baseFontSize: 12px;
$i: 1;
@while $i < 5 {
    .while-#{$i} {
    	font-size: $baseFontSize + $i * 2px;
    }
    $i: $i + 1;
}
```

### @each

`@each` 可遍历列表、数组或 map。

```scss
@mixin list{
    $list: a b c;
    @each $item in $list{
        .item-#{$item}{
            content: $item;
        }
    }
}
@include list;

@mixin arr{
    $arr: (d,e,f);
    @each $i in $arr{
        .i-#{$i}{
            content: $i;
        }
    }
}
@include arr;

$map: (key1: val1, key2: val2, key3: val3);
@each $key, $val in $map {
    // ...
}
```

### @media

SCSS 可以在规则中嵌套 `@media`，但编译后将提取到最外层。

`@media` 本身也可以嵌套，编译后都将提取到最外层并用 `and` 连接。

### @at-root

`@at-root` 指令可以将一个或多个规则提取到根层级。

> 该指令可以跟 `without` 和 `with` 配合使用，以指定提取出或不提取出哪些嵌套。形式如下：
>
> * @at-root (without: \<keyword\>)
> * @at-root (with: \<keyword\>)
>
> 其中关键词可以是：all、rule、media、support。

### @content

`@content` 用于混合器中，混合器被调用时传入的样式代码将替换它。

```scss
@mixin antzone {
  #head {
    @content;
  }
}
@include antzone {
  #logo {
    width:180px;
    height:50px;
  }
}
// 编译结果：
// #head #logo {
//   width: 180px;
//   height: 50px; 
// }
```

## 注释

```scss
// 行注释，不会编译到 CSS 文件

/*
  块注释，会编译到 CSS 文件
*/
```

# 参考

## 比较

### 混合器、占位符、继承

混合器可以带参数，但是相同的调用选择器不会组合；

占位符不会在编译后的 css 文件中保留，相同的调用选择器会进行组合；

不使用占位符的继承会在css文件中保留基类，相同的调用选择器会进行组合；

混合器调用时不能使用插值，不过继承可以。

## 示例

### 混合器声明全局变量

```scss
@mixin button-style {
  $btn-bg-color: lightblue !global;
}

.wrap {
  @include button-style; // 注释该行将报错
  background: $btn-bg-color;
}
```

混合器只声明不调用，其内部代码不生效，即使其中声明了全局变量。

```scss
// import.scss
div {
	color: blue;
	@import 'partial';
}
// _partial.scss
.content {
	max-width: 800px;
}
// import.css（编译结果）
div {
  color: blue;
}
div .content {
  max-width: 800px;
}
```

## 链接

[Sass Reference（英文）](https://sass-lang.com/documentation/frames.html)

[Sass 中文文档](http://sass.bootcss.com/docs/sass-reference/)

[Sass 中文网](https://www.sass.hk/docs/)

[SCSS 教程 - 蚂蚁部落](http://www.softwhy.com/qiduan/SCSS_source/)

