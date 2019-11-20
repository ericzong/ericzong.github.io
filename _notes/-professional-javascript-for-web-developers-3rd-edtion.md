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

### 3.5.2 位操作符

ECMAScript 以 64 位存储数值，但会转换为 32 位执行位操作，并将结果转换回 64 位。这个转换过程导致一个严重的副效应，即在对特殊的 NaN 和 Infinity 值应用位操作时，两个值会被当成 0 来处理。

| 操作    | 英文  | 符号     |
| ----- | --- | ------ |
| 按位非   | NOT | ~      |
| 按位与   | AND | &      |
| 按位或   | OR  | \|     |
| 按位异或  | XOR | ^      |
| 左移    |     | \<\<   |
| 有符号右移 |     | \>\>   |
| 无符号右移 |     | \>\>\> |

### 3.5.3 布尔操作符

| 操作  | 英文  | 符号   |
| --- | --- | ---- |
| 逻辑非 | NOT | !    |
| 逻辑与 | AND | &&   |
| 逻辑或 | OR  | \|\| |

逻辑非操作符可作用于任何数据类型，会先将操作数转换为一个布尔值，再求反。

同时使用两次逻辑非即模拟 Boolean() 转型函数行为。

逻辑与操作不一定返回布尔值。操作数不能是（计算到，短路可能不计算到）未定义的值，否则异常。

逻辑否同逻辑与类似。

### 3.5.10 逗号操作符

逗号操作符总会返回表达式中最后一项：

```js
var num = (5, 1, 4, 8, 0); // num 的值为 0
```

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

## 4.2 执行环境及作用域

执行环境（ execution context ），境定义了变量或函数有权访问的其他数据，决定了它们各自的行为。每个执行环境都有一个与之关联的变量对象（ variable object ），环境中定义的所有变量和函数都保存在这个对象中。编写的代码无法访问这个对象，但解析器在处理数据时会在后台使用它。

作用域链（ scope chain ）用途，是保证对执行环境有权访问的所有变量和函数的有序访问。作用域链的前端，始终都是当前执行的代码所在环境的变量对象。如果这个环境是函数，则将其活动对象（ activation object ）作为变量对象。活动对象在最开始时只包含一个变量，即 arguments 对象（这个对象在全局环境中是不存在的）。

标识符解析是沿着作用域链一级一级地搜索标识符的过程。

### 4.2.1 延长作用域链

有些语句可以在作用域链的前端临时增加一个变量对象，该变量对象会在代码执行后被移除。当执行流进入下列任何一个语句时，作用域链就会得到加长：

* try-catch 语句的 catch 块；
* with语句

> 在 IE8 及之前版本的 JavaScript 实现中，在 catch 语句中捕获的错误对象会被添加到执行环境的变量对象，而不是 catch 语句的变量对象中。换句话说，即使是在 catch 块的外部也可以访问到错误对象。 IE9 修复了这个问题。

> 我的注释：
> 
> 比如如下立即执行函数，
> 
> ```js
> (function() { 
>     try { throw 'error message'; } catch(e) { console.log(e) }
>     console.log(e); // IE8 及之前，e 可访问，且值为 'error message'
> })();
> ```

### 4.2.2 没有块级作用域

使用 var 声明的变量会自动被添加到最接近的环境中。

如果初始化变量时没有使用 var 声明，该变量会自动被添加到全局环境。

变量查询有代价。但 JS 引擎优化不错，差别可忽略。

> 我的注释：
> 
> 已过时，ES6 已经支持块级作用域。

## 4.3 垃圾收集

### 4.3.1 标记清除

JavaScript 中最常用的垃圾收集方式是标记清除（ mark-and-sweep ）。

到 2008 年为止， IE 、 Firefox 、 Opera 、 Chrome 和 Safari 的 JavaScript 实现使用的都是标记清除式的垃圾收集策略（或类似的策略），只不过垃圾收集的时间间隔互有不同。

### 4.3.2 引用计数

另一种不太常见的垃圾收集策略叫做引用计数（ reference counting ）。

Netscape Navigator 3.0 是最早使用引用计数策略的浏览器。

严重的问题：循环引用。

IE9 把 BOM 和 DOM 对象都转换成了真正的 JavaScript 对象。避免了两种垃圾收集算法并存导致的问题，也消除了常见的内存泄漏现象。

### 4.3.4 管理内存

分配给 Web 浏览器的可用内存数量通常要比分配给桌面应用程序的少。目的主要是出于安全方面的考虑，目的是防止运行 JavaScript 的网页耗尽全部系统内存而导致系统崩溃。

一旦数据不再有用，最好通过将其值设置为 null 来释放其引用，这叫解除引用（dereferencing）。

# 第5章 引用类型

引用类型的值（对象）是引用类型的一个实例。

## 5.1 Object类型

ECMAScript 中的表达式上下文（expression context）指的是能够返回一个值（表达式）。

语句上下文（statement context）

在通过对象字面量定义对象时，实际上不会调用 Object 构造函数（ Firefox2 及更早版本会调用 Object 构造函数；但 Firefox 3 之后就不会了）。

方括号语法的主要优点是可以通过变量来访问属性。如果属性名中包含会导致语法错误的字符，或者属性名使用的是关键字或保留字，也可以使用方括号表示法。

## 5.2 Array类型

在使用数组字面量表示法时，也不会调用 Array 构造函数（ Firefox3 及更早版本除外）。

数组最多可以包含 4 294 967 295 个项。

### 5.2.1 检测数组

instanceof 操作符的问题在于，它假定单一的全局执行环境。如果网页中包含多个框架，那实际上就存在两个以上不同的全局执行环境，从而存在两个以上不同版本的 Array 构造函数。

## 5.3 Date类型

ECMAScript 中的 Date 类型是在早期 Java 中的 java.util.Date 类基础上构建的。为此， Date 类型使用自 UTC （ Coordinated Universal Time ，国际协调时间） 1970 年 1 月 1 日午夜（零时）开始经过的毫秒数来保存日期。在使用这种数据存储格式的条件下， Date 类型保存的日期能够精确到 1970 年 1 月 1 日之前或之后的 285 616 年。

## 5.4 RegExp类型

```js
var expression = /pattern/flags;
```

元字符：`( [ { \ ^ $ | ) ? * + . ] }`

由于 RegExp 构造函数的模式参数是字符串，所以在某些情况下要对字符进行双重转义。所有元字符都必须双重转义，那些已经转义过的字符也是如此。

ES3 中，正则字面量共享 RegExp 实例，构造函数总创建新实例。ES5 中，正则字面量和构造函数行为一致，总创建新实例。

### 5.4.1 RegExp实例属性

global/ignoreCase/multiline：布尔值，标志

lastIndex：整数，从 0

source：正则字符串表示，字面量形式

### 5.4.2 RegExp实例方法

pattern.exe(text) => {index, input, [match_text, groups...]}

pattern.text(text) => true/false

toLocaleString()/toString() 返回字面值，valueOf() 返回正则本身。

> 我的注释：在 NodeJS（实验版本：v12.6.0）中，返回正好相反。

### 5.4.3 RegExp构造函数属性

| 长属性名         | 短属性名 | 说明                     |
| ------------ | ---- | ---------------------- |
| input        | $_   | 最近要匹配字符串               |
| lastMatch    | $&   | 最近匹配项                  |
| lastParen    | &+   | 最近捕获组                  |
| leftContext  | &`   | input 中 lastMatch 之前文本 |
| rightContext | $'   | input 中 lastMatch 之后文本 |
| multiline    | $*   | 布尔，多行模式                |

注：短属性名访问，`RegExp.$_`。`RegExp.$1` ~ `RegExp.$9`。

调用 exec() / test() 时，这些属性会被自动填充。

### 5.4.4 模式的局限性

不支持特性：

* 开始结尾锚：`\A` `\Z`
* 向后查找（lookbehind）[^5441]
* 并集和交集类
* 原子组（atomic grouping）
* Unicode 支持（单个字符除外，如 \uFFFF）
* 命名的捕获组[^5442]
* s（single，单行）和 ×（free-spacing，无间隔）匹配模式
* 条件匹配
* 正则表达式注释

[^5441]: 支持向前查找（lookahead）
[^5442]: 支持编号捕获组

## 5.5 Function类型

每个函数都是 Function 类型的实例，而且都与其他引用类型一样具有属性和方法。由于函数是对象，因此函数名实际上也是一个指向函数对象的指针，不会与某个函数绑定。

在使用函数表达式定义函数时，没有必要使用函数名。

定义函数的方法：

* 函数声明语法
* 函数表达式
* Function 构造函数

```js
new Function(args..., functionBody)
```

影响性能，导致解析两次代码：第一次是解析常规 ES 代码，第二次是解析传入构造函数中的字符串。

一个函数可能会有多个名字。

### 5.5.2 函数声明与函数表达式

解析器会率先读取函数声明，并使其在执行任何代码之前可用（可以访问）；至于函数表达式，则必须等到解析器执行到它所在的代码行，才会真正被解释执行。

函数声明提升（function declaration hoisting）

### 5.5.4 函数内部属性

arguments，是一个类数组对象，包含着传入函数中的所有参数。

arguments.callee，拥有这个 arguments 对象的函数。

> 递归使用函数名调用，会使函数执行与函数名耦合，而使用 arguments.callee 可消除耦合。

function.caller，调用当前函数的函数的引用。全局作用域中为 null。

### 5.5.5 函数属性和方法

length，函数希望接收的命名参数的个数。

对于 ECMAScript 中的引用类型而言，prototype 是保存它们所有实例方法的真正所在。

prototype 不可枚举。

每个函数都包含两个非继承而来的方法：apply() 和call()。在特定的作用域中调用函数，实际上等于设置函数体内 this 对象的值。能够扩充函数赖以运行的作用域。对象不需要与方法有任何耦合关系。

ES5，bind()，创建一个函数的实例，其 this 值会被绑定到传给 bind() 函数的值。

每个函数继承的 toLocaleString()、toString() 、valueOf() 方法始终都返回函数的代码。

## 5.6 基本包装类型

Boolean、Number、String

实际上，每当读取一个基本类型值的时候，后台就会创建一个对应的基本包装类型的对象，从而让我们能够调用一些方法来操作这些数据。

后台自动完成：

1. 创建 String 类型的一个实例；
2. 在实例上调用指定的方法；
3. 销毁这个实例。

自动创建的基本包装类型的对象，则只存在于一行代码的执行瞬间，然后立即被销毁。这意味着不能在运行时为基本类型值添加属性和方法。

Object 构造函数也会像工厂方法一样，根据传入值的类型返回相应基本包装类型的实例。

> 基本包装类型同名转型函数会将指定的值转换为对应的基本类型。

### 5.6.1 Boolean类型

Boolean 对象在布尔表达式中常造成误解。

Boolean 对象和基本类型布尔值在 typeof 和 instanceof 操作符下执行结果都不同。

### 5.6.2 Number类型

```js
number.toString(2) // 二进制
number.toFixed(2)  // 保留小数位
number.toExponential(2) // 指数表示法，参数表示小数位
number.toPrecision(2)   // 固定大小/指数格式，参数表示所有数字位数（除指数部分）
```

### 5.6.3 String类型

```js
str.charAt(0);
str.charCodeAt(0); // 字符编码
str[0];
str.concat(str1);

// 以下方法当参数为负数时，行为有差异
str.slice(0, 3);
str.substring(3);
str.substr(3);

str.indexOf('x');
str.lastIndexOf('x');

str.trim();

str.toLocaleUpperCase();
str.toUpperCase();
str.toLocaleLowerCase();
str.toLowerCase();

str.match(pattern);  // => [matchText, groups]
str.search(pattern); // => index
str.replace(strOrPattern, newText);
str.replace(pattern, function(match, pos, originalText) {});
str.split(strOrPattern, returnCount?);

str.localeCompare(str1); // 符号方法

String.fromCharCode(codes...);
```

| 字符序列 | 替换文本                             |
| ---- | -------------------------------- |
| $$   | $                                |
| $&   | 匹配整个模式的子字符串。<=> RegExp.lastMatch |
| $'   | 匹配前子串。 <=> RegExp.leftContext    |
| $`   | 匹配后子串。 <=> RegExp.rightContext   |
| $n   | 捕获组。n∈(0, 9)。无分组为空。              |
| $nn  | 捕获组。n∈(01, 99)。无分组为空。            |

## 5.7 单体内置对象

内置对象：由 ECMAScript 实现提供的、不依赖于宿主环境的对象，这些对象在 ECMAScript 程序执行之前就已经存在了。

### 5.7.1 Global对象

事实上，没有全局变量或全局函数；所有在全局作用域中定义的属性和函数，都是 Global 对象的属性。

```js
encodeURI()
encodeURIComponent()
decodeURI()
decodeURIComponent()
```

encodeURI() 不会对本身属于 URI 的特殊字符进行编码，例如冒号、正斜杠、问号和井字号；而 encodeURIComponent() 则会对它发现的任何非标准字符进行编码。

在 eval() 中创建的任何变量或函数都不会被提升，因为在解析代码的时候，它们被包含在一个字符串中；它们只在 eval() 执行的时候创建。

Global属性

| 分类   | 属性             |
| ---- | -------------- |
| 特殊值  | undefined      |
|      | NaN            |
|      | Infinity       |
| 构造函数 | Object         |
|      | Array          |
|      | Function       |
|      | Boolean        |
|      | String         |
|      | Number         |
|      | Date           |
|      | RegExp         |
| 异常   | Error          |
|      | EvalError      |
|      | RangeError     |
|      | ReferenceError |
|      | SyntaxError    |
|      | TypeError      |
|      | URIError       |

```js
// 取得 Global 对象的方法
var global = function () {
    return this;
}
```

# 第6章 面向对象的程序设计

ECMA-262 对象定义：无序属性的集合，其属性可以包含基本值、对象或者函数。

## 6.1 理解对象

对象字面量已成为创建对象的首选模式。

为了表示特性是内部值，ES5 将其放在两对方括号中，如 [[Enumerable]]。

### 6.1.1 属性类型

ECMAScript 中有两种属性：数据属性和访问器属性。

数据属性包含一个数据值的位置。

数据属性特性：[[Configurable]]、[[Enumerable]]、[[Writable]]、[[Value]]。

```js
Object.defineProperty(obj, prop, descriptor)

Object.defineProperty(person, "name", {
    configurable: false,
    value: 'XXX'
});
Object.defineProperty(person, "name", {
    configurable: true, // 异常，设置为 false 后不能再设为 true
    value: 'YYY'
});
```

访问器属性特性：[[Configurable]]、[[Enumerable]]、[[Get]]、[[Set]]

```js
Object.defineProperty(book, 'year', {
    get: function() {
        return this._year; 
    },
    set: function(newValue) {
            // set _year
    }
});
// 遗留方法
obj.__defineGetter__('year', function() { 
    return xxx; 
});
obj.__defineSetter__('year', function(newValue) {});
```

### 6.1.2 定义多个属性

```js
Object.defineProperties(obj, {
    _year: {
        value: 2004
    },
    year: {
            get: function() {
                return this._year;
        }
    }
});
```

### 6.1.3 读取属性的特性

```js
Object.getOwnPropertyDescriptor(obj, prop);
```

## 6.2 创建对象

### 6.2.1 工厂模式

工厂模式解决了创建多个相似对象的问题，但却没有解决对象识别问题（即获知对象类型）。

### 6.2.2 构造函数模式

构造函数模式 vs. 工厂模式：

* 没有显式地创建对象
* 直接将属性和方法赋给了 this 对象
* 没有 return 语句

构造函数步骤：

1. 创建一个新对象
2. 将构造函数作用域赋给新对象（因此 this 指向了这个新对象）
3. 执行构造函数中的代码
4. 返回新对象

对象构造函数属性：obj.constructor

构造函数的缺点：每个实例都包含不同的 Function 实例，这会导致不同的作用域链和标识符解析。可将函数转移到构造函数外来规避这个问题。（但这会导致声明全局函数的问题，应使用原型。）

### 6.2.3 原型模式

使用原型对象的好处是可以让所有对象实例共享它所包含的属性和方法。

构造函数的原型属性的构造器属性指向构造函数本身：Person.prototype.constructor -> Person

ES5 实例内部指针 [[Prototype]] 指向构造函数的原型对象。

```js
Person.prototype.isPrototypeOf(person); // true
Object.getPrototypeOf(person) == Person.prototype; // true
```

实例属性会覆盖原型中同名属性，除非 delete 实例属性，否则不能访问原型中的同名属性。

```js
person.hasOwnProperty("name"); // true
```

ES5 `Object.getOwnPropertyDescriptor()` 只能用于实例属性，要取得原型属性描述符须在原型对象上调用。

```js
"name" in person; // true，无论在对象上，还是原型上

function hasPrototypeProperty(object, name){     return !object.hasOwnProperty(name) && (name in object); 
}

Object.keys(person); // 可枚举实例属性
Object.getOwnPropertyNames(Person.prototype); // 所有实例属性
```

更简单的原型语法：

```js
function Person() {}
Person.prototype = {
    name : "Nicholas",
    age : 29,
    job: "Software Engineer",
    sayName : function () {
        alert(this.name);     
    } 
};
```

本质上完全重写了默认的 prototype 对象，constructor 属性不再指向 Person 函数，而指向 Object 函数。

```js
// 可手动设置构造器，此时构造器可枚举
Person.prototype = {
    constructor: Person,
    // ...
}
// ES5 修正为不可枚举
Object.defineProperty(Person.prototype, "constructor", {
    enumerable: false,
    value: Person 
});
```

原型与实例之间只有指针，因此不能完全重写原型对象，否则修改不会反应在实例上。

原型对象问题：

* 没有参数传递，默认情况下属性值相同
* 共享引用类型

### 6.2.4 组合使用构造函数模式和原型模式

创建自定义类型的最常见方式，就是组合使用构造函数模式与原型模式。构造函数模式用于定义实例属性，而原型模式用于定义方法和共享的属性。

```js
function Person(name, age, job) {
    this.name = name;
    this.age = age;
    this.job = job;
    this.friends = ["Shelby", "Court"];
}
Person.prototype = {
     constructor: Person, 
    sayName: function() {
        alert(this.name);
    }
}
```

### 6.2.5 动态原型模式

把所有信息都封装在了构造函数中，而通过在构造函数中初始化原型（仅在必要的情况下），又保持了同时使用构造函数和原型的优点。

```js
function Person(name, age, job) {
    // 属性初始化
    // ...
    // 方法
    if(typeof this.sayName != "function") {
           // do sth 
    }
}
```

### 6.2.6 寄生构造函数模式

寄生（ parasitic ）构造函数模式，创建一个函数，该函数的作用仅仅是封装创建对象的代码，然后再返回新创建的对象。

```js
function Person(name, age, job) {
    var o = new Object();
    // 属性和方法初始化
    return o;
}
```

实例与构造函数及其原型属性之间没有关系，instanceof 不能确定其类型。

最佳实践：有其他模式可用时不要使用。

### 6.2.7 稳妥构造函数模式

稳妥对象（ durable objects ），没有公共属性，而其方法也不引用 this 的对象。

## 6.3 继承

两种继承方式：接口继承和实现继承。接口继承只继承方法签名，而实现继承则继承实际的方法。

由于函数没有签名，在 ECMAScript 中无法实现接口继承。

ECMAScript 只支持实现继承，而且其实现继承主要是依靠原型链来实现的。

### 6.3.1 原型链

基本思想是利用原型让一个引用类型继承另一个引用类型的属性和方法。

构造函数、原型和实例的关系：每个构造函数都有一个原型对象，原型对象都包含一个指向构造函数的指针，而实例都包含一个指向原型对象的内部指针。

确定原型和实例的关系：instanceof 操作符，测试出现过的构造函数；isPrototypeOf() 方法，原型链中出现过的原型。

给原型添加方法的代码一定要放在替换原型的语句之后。

在通过原型链实现继承时，不能使用对象字面量创建原型方法。因为这样做就会重写原型链。

原型链问题：

1. 包含引用类型值；
2. 在创建子类型的实例时，不能向超类型的构造函数中传递参数；

### 6.3.2 借用构造函数

借用构造函数（ constructor stealing ），伪造对象或经典继承，基本思想：在子类型构造函数的内部调用超类型构造函数。

```js
function SubType() {
    SuperType.call(this);
}
```

优势：传递参数，可以在子类型构造函数中向超类型构造函数传递参数；

问题：函数复用问题，在超类型的原型中定义的方法，对子类型而言也是不可见的。

### 6.3.3 组合继承

组合继承（ combination inheritance ），伪经典继承，将原型链和借用构造函数的技术组合到一块，从而发挥二者之长的一种继承模式。思路：使用原型链实现对原型属性和方法的继承，而通过借用构造函数来实现对实例属性的继承。

组合继承避免了原型链和借用构造函数的缺陷，融合了它们的优点， instanceof 和 isPrototypeOf() 也能够用于识别基于组合继承创建的对象。

### 6.3.4 原型式继承

ECMAScript 5 通过新增 Object.create() 方法规范化了原型式继承。

### 6.3.5 寄生式继承

寄生式（ parasitic ）继承，创建一个仅用于封装继承过程的函数，该函数在内部以某种方式来增强对象，最后再像真地是它做了所有工作一样返回对象。

```js
function createAnother(original){
    var clone = object(original);    // 通过调用函数创建一个新对象
    clone.sayHi = function(){        // 以某种方式来增强这个对象
        alert("hi");
    };
    return clone;                    // 返回这个对象
}
```

不能做到函数复用。

### 6.3.6 寄生组合式继承

组合继承最大的问题：无论什么情况下，都会调用两次超类型构造函数：一次是在创建子类型原型的时候，另一次是在子类型构造函数内部。

寄生组合式继承，即通过借用构造函数来继承属性，通过原型链的混成形式来继承方法。基本思路是：不必为了指定子类型的原型而调用超类型的构造函数，需要的无非就是超类型原型的一个副本而已。

```js
function inheritPrototype(subType, superType){     var prototype = object(superType.prototype);       // 创建对象     prototype.constructor = subType;                   // 增强对象     subType.prototype = prototype;                     // 指定对象 }
```

# 第7章 函数表达式

函数声明提升（function declaration hoisting），在执行代码之前会先读取函数声明。

匿名函数（anonymous function），也叫拉姆达函数，name 属性是空字符串。

## 7.1 递归

递归函数，一个通过名字调用自身的函数。

递归函数的一个可能风险是将其赋值给其他引用，而原引用失效时，调用会失败。这是可以使用 `arguments.callee`。

但是，严格模式下，`arguments.callee` 不可用。这时应使用“命名函数表达式”：

```js
var factorial = (function f(num) {
   if (num <= 1)  {
       return 1;
   } else {
       return num * f(num - 1);
   }
});
```

## 7.2 闭包

闭包是指有权访问另一个函数作用域中的变量的函数。

由于闭包会携带包含它的函数的作用域，因此会比其他函数占用更多的内存。过度使用闭包可能会导致内存占用过多。

### 7.2.1 闭包与变量

闭包只能取得包含函数中任何变量的最后一个值。这是作用域配置机制引起的副作用。

如果要在闭包中取得某个变量的中间值，通常需要使用一个立即执行的匿名函数，将变量传入并在该函数内返回一个使用该变量值的函数。

### 7.2.2 关于this对象

匿名函数的执行环境具有全局性。

每个函数在被调用时，其活动对象都会自动取得两个特殊变量：`this` 和 `arguments`。内部函数在搜索这两个变量时，只会搜索到其活动对象为止，因此永远不可能直接访问外部函数中的这两个变量。除非在外部函数中将这两个变量赋值给其他引用，这便可以在内部函数中访问到了。

### 7.2.3 内存泄漏

闭包会引用包含函数的整个活动对象。

## 7.4 私有变量

特权方法（privileged method），有权访问私有变量和私有函数的公有方法。

在构造函数中定义特权方法的缺点：必须使用构造函数模式。

### 7.4.2 模块模式

JS 是以对象字面量的方式来创建单例对象的。

# 第8章 BOM

## 8.1 window对象

### 8.1.1 全局作用域

全局变量不能通过 delete 操作符删除，而直接在 window 对象上的定义的属性可以。

全局变量添加的 `window` 属性 `[[Configurable]]` 特性值为 `false`。

### 8.1.2 窗口关系及框架

`top` 总指向浏览器窗口。

没有框架的情况下，`parent` 一定等于 `top`。

除非最高层窗口是通过 `window.open()` 打开，否则其 `window` 对象的 `name` 属性不会包含任何值。    

`self` 始终指向 `window`，引入只是为了与 `top` 和 `parent` 对应起来。

每个框架都有一套自己的构造函数，并不相等，影响对跨框架传递的对象使用 `instanceof` 操作符。

### 8.1.6 间歇调用和超时调用

超时调用的代码是在全局作用域中执行的，故函数中 `this` 在非严格模式下指向 `window`，严格模式下为 `undefined`。

最佳模式：使用超时调用来模拟间歇调用。因为后一个间歇调用可能会在前一个间歇调用结束之前启动。

## 8.2 location对象

`location` 对象很特别，它既是 `window` 对象的属性，也是 `document` 对象的属性。即：

```js
window.location === document.location; // true
```

### 8.2.2 位置操作

```js
location.assign('http://www.baidu.com');
window.location = 'http://www.baidu.com'
location.href = 'http://www.baidu.com';
```

# 第9章 客户端检测

## 9.1 能力检测

能力检测/特性检测，目标是识别浏览器的能力。

## 9.2 怪癖检测

怪癖检测（quirks detection），目标是识别浏览器的特殊行为。

## 9.3 用户代理检测

电子欺骗（spoofing），浏览器通过在自己的用户代理字符串加入一些错误或误导性信息，来达到欺骗服务器的目的。

# 第10章 DOM

## 10.1 节点层次

### 10.1.1 Node类型

NodeList 对象的独特之处在于，它实际上是基于 DOM 结构动态执行查询的结果，因此 DOM 结构的变化能够自动反映在 NodeList 对象中。

### 10.1.2 Document类型

```js
var hasXmlDom = document.implementation.hasFeature("XML", "1.0");
```

如果在文档加载结束后再调用 `document.write()`，那么输出的内容将会重写整个页面。

### 10.1.3 Element类型

只有公认的（非自定义的）特性才会以属性的形式添加到 DOM 对象中。

两类特殊特性，属性的值与 `getAttribute()` 返回的值并不相同。一类是 `style`，`getAttribute()` 返回字符串，属性访问返回对象；另一类是事件处理程序，`getAttribute()` 返回字符串，属性访问返回 JS 函数。

通过 `setAttribute()`方法既可以操作 HTML 特性，也可以操作自定义特性。通过这个方法设置的特性名会被统一换为小写形式。

因为所有特性都是属性，所以直接给属性赋值可以设置特性的值。

为 DOM 元素添加一个自定义的属性，该属性不会自动成为元素的特性。

## 10.2 DOM操作

动态脚本，在页面加载时不存在，但将来的某一时刻通过修改 DOM 动态添加的脚本。

动态样式，页面刚加载时不存在的样式；动态模式是在页面加载完成后动态添加到页面中的。

必须将 `<link>` 元素添加到 `<head>` 而不是 `<body>` 元素，才能保证在所有浏览器中的行为一致。

# 第11章 DOM扩展

## 11.2 元素遍历

Element Traversal 规范（ www.w3.org/TR/ElementTraversal/ ）：

* childElementCount：返回子元素（不包括文本节点和注释）的个数。
* firstElementChild：指向第一个子元素； firstChild 的元素版。
* lastElementChild：指向最后一个子元素； lastChild 的元素版。
* previousElementSibling ：指向前一个同辈元素； previousSibling 的元素版。
* nextElementSibling：指向后一个同辈元素； nextSibling 的元素版。

## 11.3 HTML5

## 11.3.1 与类相关的扩充

`getElementsByClassName()`，参数是一个包含一或多个类名的字符串。传入多个类名时，先后顺序不重要。

`element.classList` => `DOMTokenList` => `item`/`[n]`

`DOMTokenList` 方法：

* add(value)
* contains(value)
* remove(value)
* toggle(value)

### 11.3.2 焦点管理

文档加载中，`document.activeElement`  →   `null`

文档加载完成，`document.activeElement`  →  `document.body`

`document.hasFocus()`

无障碍性。

### 11.3.3 HTMLDocument的变化

`document.readyState` 属性：`loading` 和 `complete`。

标准模式：`document.compatMode = 'CSS1Compat'` ；混杂模式：`document.compatMode = 'BackCompat'`

### 11.3.4 字符集属性

`charset` 属性默认值为 UTF-16。

### 11.3.5 自定义数据属性

前缀 `data-`，为元素提供与渲染无关的信息，或提供语义信息。

`element.dataset` 属性，`DOMStringMap`，`data-name`。

### 11.3.7 scrollIntoView()方法

为某个元素设置焦点也会导致浏览器滚动并显示出获得焦点的元素。

>  https://developer.mozilla.org/zh-CN/docs/Web/API/Element/scrollIntoView 

## 11.4 专有扩展

### 11.4.1 文档模式

IE8 引入，决定可以使用什么功能。

```js
document.documentMode
```

### 11.4.4 插入文本

`innerText` 会忽略行内样式和脚本；`textContent` 会返回行内样式和脚本代码。

### 11.4.5 滚动

```js
scrollIntoViewIfNeeded(alignCenter);
scrollByLines(lineCount);
scrollByPages(pageCount);
```

scrollIntoView() 和 scrollIntoViewIfNeeded() 的作用对象是元素的容器，而 scrollByLines() 和 scrollByPages() 影响的则是元素自身。

# 第12章 DOM2和DOM3

DOM3 级 -> XPath 模块

## 12.2 样式

DOM2 级定义的 CSS 能力

## 12.4 范围

Range

# 第13章 事件

## 13.1 事件流

IE 的事件流是事件冒泡流，而 Netscape Communicator 的事件流是事件捕获流。

### 13.1.1 事件冒泡

事件冒泡（ event bubbling ），即事件开始时由最具体的元素（文档中嵌套层次最深的那个节点）接收，然后逐级向上传播到较为不具体的节点（文档）。

### 13.1.2 事件捕获

事件捕获（ event capturing ），不太具体的节点应该更早接收到事件，而最具体的节点应该最后接收到事件。事件捕获的用意在于在事件到达预定目标之前捕获它。

### 13.1.3 DOM事件流

“DOM2级事件”规定的事件流包括三个阶段：事件捕获阶段 → 处于目标阶段 → 事件冒泡阶段。

## 13.2 事件处理程序

事件就是用户或浏览器自身执行的某种动作。

事件处理程序/事件侦听器，响应某个事件的函数。以“on”开头。

### 13.2.1 HTML事件处理程序

HTML 中指定事件处理程序的缺点：

* 时差问题。事件可能在处理程序不具备执行条件（比如加载）前触发。解决方案：`try-catch` 块；
* 扩展事件处理程序的作用域链在不同浏览器中会导致不同结果。不同 JavaScript 引擎遵循的标识符解析规则略有差异，很可能会在访问非限定对象成员时出错；
* HTML 与 JavaScript 代码紧密耦合。解决方案：使用 JavaScript 指定事件处理程序。

## 13.3 事件对象

### 13.3.1 DOM中的事件对象

只有 cancelable 属性设置为 true 的事件，才可以使用 preventDefault() 来取消其默认行为。

stopPropagation() 方法用于立即停止事件在 DOM 层次中的传播，即取消进一步的事件捕获或冒泡。

### 13.3.2 IE中的事件对象

事件处理程序的作用域是根据指定它的方式来确定的，因此，`this` 不一定指向事件目标。最佳实践：使用 `event.srcElement`。

## 13.4 事件类型

### 13.4.3 鼠标与滚轮事件

位置：

* 客户区坐标位置
* 页面坐标位置
* 屏幕坐标位置

## 13.5 内存和性能

### 13.5.1 事件委托

对“事件处理程序过多”问题的解决方案就是事件委托。事件委托利用了事件冒泡，只指定一个事件处理程序，就可以管理某一类型的所有事件。

## 13.6 模拟事件

可使用 JS 模拟各种事件。

# 第24章 最佳实践

## 24.1 可维护性

### 24.1.2 代码约定

支持各种编程风格，传统面向对象式→声明式→函数式。

可读性的大部分内容都是和代码的缩进相关的。

可读性与注释相关部分：

* 函数和方法、
* 大段代码
* 复杂的算法
* Hack —— 修正浏览器差异

命名一般规则：

* 变量名应为名词
* 函数名应该以动词开始。返回布尔类型值的函数一般以 is 开头。
* 变量和函数都应使用合乎逻辑的名字，不要担心长度。

表示变量数据类型的方式：

* 初始化暗示。缺点：无法用于函数声明中的函数参数。

* 匈牙利标记法来指定类型。（o→对象；s→字符串；i→整数；f→浮点数；b→布尔型）。优点：函数参数可用；缺点：让代码某种程度上难以阅读，阻碍了没有用它时代码的直观性和句子式的特质。

* 用类型注释。在变量旁边放一段指定类型的注释。优点：维持了代码的整体可读性，同时注入了类型信息；缺点：不能用多行注释一次注释大块的代码，因为类型注释也是多行注释，两者会冲突。
  
  ```js
  var found /* : Boolean */ = false;
  ```

### 24.1.3 松散耦合

只要应用的某个部分过分依赖于另一部分，代码就是耦合过紧，难于维护。

1. 解耦 HTML/JavaScript。HTML是数据，JavaScript是行为。
2. 解耦 CSS/JavaScript。显示问题的唯一来源应该是CSS，行为问题的唯一来源应该是JavaScript。
3. 解耦应用逻辑/事件处理程序 

### 24.1.4 编程实践

1. 尊重对象所有权。不能修改不属于你的对象。

2. 避免全局量。
   
   单一的全局量的延伸便是命名空间的概念。

3. 避免与 null 进行比较。

4. 使用常量。

## 24.2 性能

Chrome 是第一款内置优化引擎，将 JavaScript 编译成本地代码的浏览器。

### 24.2.1 注意作用域

随着作用域链中的作用域数量的增加，访问当前作用域以外的变量的时间也在增加。访问全局变量总是要比访问局部变量慢，因为需要遍历作用域链。只要能减少花费在作用域链上的时间，就能增加脚本的整体性能。

1. 避免全局查找
2. 避免 with 语句

和函数类似， with 语句会创建自己的作用域。

with 主要用于消除额外的字符。

### 24.2.2 选择正确方法

1. 避免不必要的属性查找
2. 优化循环
   1. 减值迭代
   2. 简化终止条件
   3. 简化循环体
   4. 使用后测试循环（do-while）
3. 展开循环
4. 避免双重解释
5. 性能的其他注意事项
   1. 原生方法较快
   2. Switch 语句较快
   3. 位运算符较快

使用变量和数组要比访问对象上的属性更有效率，后者是一个 O(n) 操作。对象上的任何属性查找都要比访问变量或者数组花费更长时间，因为必须在原型链中对拥有该名称的属性进行一次搜索。简而言之，属性查找越多，执行时间就越长。

Duff 装置，循环展开技术，Tom Duff 创建，最早在 C 语言中使用。Jeff Greenberg 用 JavaScript 实现了 Duff 装置。

Duff 装置的基本概念是通过计算迭代的次数是否为 8 的倍数将一个循环展开为一系列语句。

```js
//credit: Jeff Greenberg for JS implementation of Duff’s Device  
// 假设  values.length > 0 
var iterations = Math.ceil(values.length / 8);  
var startAt = values.length % 8;
var i = 0;
do {
    switch(startAt){
        case 0: process(values[i++]);
        case 7: process(values[i++]);
        case 6: process(values[i++]);
        case 5: process(values[i++]);
        case 4: process(values[i++]);
        case 3: process(values[i++]);
        case 2: process(values[i++]);
        case 1: process(values[i++]);
    }
    startAt = 0;
} while (--iterations > 0);
```

```js
//credit: Speed Up Your Site (New Riders, 2003)  
var iterations = Math.floor(values.length / 8);  
var leftover = values.length % 8;  
var i = 0;  

if (leftover > 0){ 
    do {
        process(values[i++]);
    } while (--leftover > 0);
}
do {
    process(values[i++]);
    process(values[i++]);
    process(values[i++]);
    process(values[i++]);
    process(values[i++]);
    process(values[i++]);
    process(values[i++]);
    process(values[i++]);
} while (--iterations > 0);
```

当 JavaScript 代码想解析 JavaScript 的时候就会存在双重解释惩罚。当使用 eval() 函数或者是 Function 构造函数以及使用 setTimeout() 传一个字符串参数时都会发生这种情况。

包含在字符串中的代码在 JavaScript 代码运行的同时必须新启动一个解析器来解析。实例化一个新的解析器有不容忽视的开销。

### 24.2.3 最小化语句数

JavaScript 代码中的语句数量也影响所执行的操作的速度。完成多个操作的单个语句要比完成单个操作的多个语句快。

1. 多个变量声明
2. 插入迭代值
3. 使用数组和对象字面量

### 24.2.4 优化DOM交互

1. 最小化现场更新
2. 使用innerHTML
3. 使用事件代理
4. 注意HTMLCollection

减少现场更新的方法之一是使用文档碎片来构建 DOM 结构。

```js
fragment = document.createDocumentFragment();
fragment.appendChild(item);
list.appendChild(fragment);
```

当把 innerHTML 设置为某个值时，后台会创建一个 HTML 解析器，然后使用内部的 DOM 调用来创建 DOM 结构，而非基于 JavaScript 的 DOM 调用。由于内部方法是编译好的而非解释执行的，所以执行快得多。

返回 HTMLCollection 对象：

* 进行了对 getElementsByTagName() 的调用；
* 获取了元素的 childNodes 属性；
* 获取了元素的 attributes 属性；
* 访问了特殊的集合，如 document.forms 、 document.images 等。

## 24.3 部署

### 24.3.3 压缩

代码长度：浏览器所需解析的字节数。

配重（Wire weight）：实际从服务器传送到浏览器的字节数。
