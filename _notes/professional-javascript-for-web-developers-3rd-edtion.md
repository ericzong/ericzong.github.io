---
layout: page
title: JavaScript高级程序设计（第3版）（阅读中……）
category: JS
author: Eric Zong
---

* content
{:toc}

# 第1章 JavaScript简介

## 1.2 JavaScript实现

完整JavaScript实现的三个部分：

* 核心（ECMAScript）
* 文档对象模型（DOM）
* 浏览器对象模型（BOM）

### 1.2.1 ECMAScript

ECMAScript 兼容实现必须：

* 支持 ECMA-262 描述的所有“类型、值、对象、属性、函数以及程序句法和语义”
* 支持 Unicode 字符标准

可选扩展：

* 添加更多类型、值、对象、属性和函数
* 修改和扩展内置的正则表达式语法

### 1.2.2 文档对象模型 （DOM）

DOM级别

* DOM1级（DOM Level 1） = DOM Core + DOM HTML，目标主要是映射文档的结
  * DOM 核心：如何映射基于 XML 的文档结构，以便简化对文档中任意部分的访问和操作
  * DOM HTML：在 DOM 核心的基础上加以扩展，添加了针对 HTML 的对象和方法。

* DOM2级
  * DOM 视图（DOM Views）：定义了跟踪不同文档（例如，应用 CSS 之前和之后的文档）视图的接口；
  * DOM 事件（DOM Events）：定义了事件和事件处理的接口；
  * DOM 样式（DOM Style）：定义了基于CSS为元素应用样式的接口；
  * DOM 遍历和范围（DOM Traversal and Range）：定义了遍历和操作文档树的接口。
* DOM3级
  * DOM 加载和保存（DOM Load and Save）模块：以统一方式加载和保存文档的方法
  * DOM 验证（DOM Validation）模块：验证文档的方法
  * 开始支持 XML 1.0 规范，涉及 XML Infoset、XPath 和 XML Base

DOM0 级指的是 Internet Explorer 4.0 和 Netscape Navigator 4.0 最初支持的 DHTML。只是 DOM 历史坐标中的一个参照点而已。

其他 DOM 标准：

* SVG（Scalable Vector Graphic，可伸缩矢量图）1.0
* MathML（Mathematical Markup Language，数学标记语言）1.0
* SMIL（Synchronized Multimedia Integration Language，同步多媒体集成语言）
* XUL（ XML User Interface Language，XML 用户界面语言），Mozilla，非 W3C 标准

### 1.2.3 浏览器对象模型（BOM）

HTML5 前没有标准，HTML5 将 BOM 写入规范。

BOM 只处理浏览器窗口和框架；习惯上，把所有针对浏览器的 JavaScript 扩展算作 BOM 的一部分：

* 弹出新浏览器窗口的功能；
* 移动、缩放和关闭浏览器窗口的功能；
* 提供浏览器详细信息的 navigator 对象；
* 提供浏览器所加载页面的详细信息的 location 对象；
* 提供用户显示器分辨率详细信息的 screen 对象；
* 对 cookies 的支持；
* 像 XMLHttpRequest 和 IE 的 ActiveXObject 这样的自定义对象。

## 1.4 小结 

JavaScript 是一种专为与网页交互而设计的脚本语言，包括：

* ECMAScript，由 ECMA-262 定义，提供核心语言功能；
* 文档对象模型（DOM），提供访问和操作网页内容的方法和接口；
* 浏览器对象模型（BOM），提供与浏览器交互的方法和接口。

# 第2章 在HTML中使用JavaScript

### 2.1 \<script\>元素

HTML 4.01 \<script\> 6 个属性：

* async：可选。表示应该立即下载脚本，但不应妨碍页面中的其他操作，比如下载其他资源或等待加载其他脚本。只对外部脚本文件有效。
* charset：可选。表示通过 src 属性指定的代码的字符集。由于大多数浏览器会忽略它的值，因此这个属性很少有人用。
* defer：可选。表示脚本可以延迟到文档完全被解析和显示之后再执行。只对外部脚本文件有效。IE7 及更早版本对嵌入脚本也支持这个属性。
* language：已废弃。原来用于表示编写代码使用的脚本语言（如 JavaScript、JavaScript1.2 或 VBScript）。大多数浏览器会忽略这个属性，因此也没有必要再用了。
* src：可选。表示包含要执行代码的外部文件。
* type：可选。表示编写代码使用的脚本语言的内容类型（也称 MIME 类型）。text/javascript 和 text/ecmascript 不推荐使用，但惯例使用 text/javascript。JS 文件服务器 MIME 类型可能是：application/x-javascript、application/javascript、application/ecmascript。type 默认值为 text/javascript。

\<script\> 嵌入脚本中任何位置都不能出现 \</script\> 字符串。

带有 src 属性的 \<script\> 元素中若嵌入代码，嵌入代码会被忽略。

### 2.1.2 延迟脚本 

HTML5 规范，延迟脚本按顺序执行，并在 DOMContentLoaded 事件之前执行。

实际情况，延迟脚本不一定按顺序执行，不一定在 DOMContentLoaded 事件前执行。

最佳实践：只包含一个延迟脚本。把延迟脚本放在页面底部。

### 2.1.3 异步脚本

异步脚本不保证顺序执行。

最佳实践：异步脚本不要在加载期间修改 DOM。

异步脚本在页面 load 事件前执行，可能会在 DOMContentLoaded 事件触发前或后执行。

## 2.2 嵌入代码与外部文件

外部文件优点：

* 可维护性
* 可缓存
* 适应未来。无须进行 XHTML 处理和老版本浏览器注释。HTML 和 XHTML 包含外部文件的语法是相同的。

## 2.3 文档模式

混杂模式（quirks mode）

标准模式（standards mode）

准标准模式（almost standards mode）

所有浏览器默认（文档开始处没有发现文档类型声明）开启混杂模式，不推荐，不同浏览器行为差异非常大。

开启标准模式，使用严格型（strict）文档类型。

开启准标准模式，使用过渡型（transitional）或框架集型（frameset）文档类型。

## 2.4 \<noscript\>元素

平稳退化。

显示情况：

* 浏览器不支持脚本
* 浏览器支持脚本，但脚本被禁用

# 第3章 基本概念

## 3.1 语法

### 3.1.2 标识符

标识符，就是指变量、函数、属性的名字，或者函数的参数。

* 第一个字符必须是字母、下划线（_）或=美元符号（$）
* 其他字符可以是字母、下划线、美元符号或数字。

字母包含扩展的 ASCII 或 Unicode 字母字符。

### 3.1.4 严格模式

```js
"use strict";
```

它是一个编译指示（pragma），用于告诉支持的JavaScript引擎切换到严格模式。

## 3.3 变量

ECMAScript 变量是松散类型的。

松散类型，可以用来保存任何类型的数据。

严格模式下，不能定义名为 eval 或 arguments 的变量，否则语法错误。

## 3.4 数据类型

### 3.4.3 Null类型

实际上，undefined 值是派生自 null 值。

### 3.4.5 Number类型

八进制，如果字面值中的数值超出了范围，那么前导零将被忽略，后面的数值将被当作十进制数值解析。

八进制字面量在严格模式下是无效的，会导致支持的JavaScript引擎抛出错误。

在默认情况下，ECMASctipt 会将那些小数点后面带有 6 个零以上的浮点数值转换为以 e 表示法表示的数值。

浮点数值的最高精度是 17 位小数，但在进行算术计算时其精确度远远不如整数。

数值转换函数：Number()、parseInt() 和 parseFloat()。

一元加操作符的操作与 Number() 函数相同。

ES5 引擎中，parseInt() 已经不具有解析八进制值的能力。

### 3.4.7 Object类型

```js
var o = new Object;  //有效，但不推荐省略圆括号
```

ECMA-262 不负责定义宿主对象，因此宿主对象可能会也可能不会继承 Object。

## 3.5 操作符

### 3.5.1 一元操作符

执行前置递增和递减操作时，变量的值都是在语句被求值以前改变的。

前/后置递增/减可适用于任意类型值。字符串的计算结果为 NaN。

## 3.6 语句

### 3.6.1 if语句

```js
if（condition）statement1 else statement2
```

condition不一定是布尔值。ECMAScript会自动调用Boolean()转换函数将这个表达式的结果转换为一个布尔值。

最佳实践是始终使用代码块，即使要执行的只有一行代码。

### 3.6.8 with语句

由于大量使用with语句会导致性能下降，同时也会给调试代码造成困难，因此在开发大型应用程序时，不建议使用with语句。

### 3.6.9 switch语句

可以在switch语句中使用任何数据类型。

## 3.7 函数

严格模式对函数有一些限制：

* 不能把函数命名为eval或arguments;
* 不能把参数命名为eval或arguments;
* 不能出现两个命名参数同名的情况。

### 3.7.1 理解参数

arguments对象只是与数组类似（它并不是Array的实例）。只能索引访问元素以及访问 length 属性。

ECMAScript函数的一个重要特点：命名的参数只提供便利，但不是必需的。

> 原文指出 arguments 元素与相应的命名参数内存空间是独立的，但值会同步。并且影响是单向的：修改命名参数不会改变 arguments 中对应值。
>
> 但实验表明 arguments 元素与对应的命名参数严格相等，且修改影响也是双向的。（环境 Node.js v12.4.0）
>
> 过时？

为缺省的命名参数对应的 arguments 元素赋值不会反应到命名参数。

参数是值传递。

### 3.7.2 没有重载

ECMAScript函数没有签名，因此不可能真正重载。

> 很多方式可以模仿重载。

## 3.8 小结

n/a 或 N/A，是 not applicable 缩写，意为“不适用”。

# 第4章 变量、作用域和内存问题

## 4.1 基本类型和引用类型的值

### 4.1.4 检测类型

typeof 操作符是检测一个变量是不是基本数据类型的最佳工具；但检测引用类型的值时要使用 instanceof 操作符。



