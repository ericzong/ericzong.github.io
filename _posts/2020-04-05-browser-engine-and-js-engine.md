---
layout: post
title: 浏览器引擎及JS引擎考
category: 科普
tags: 浏览器 排版引擎 JavaScript引擎
excerpt: 追溯浏览器引擎及JS引擎的历史变迁。
author: Eric Zong
---

* content
{:toc}

# 此引擎非彼引擎

提起浏览器，我们应该会觉得很熟悉，因为作为现代人几乎我们天天都在使用浏览器访问互联网，虽然我们使用的未必是同一厂商浏览器。不过，对于大多数人而言，我们只是简单地使用浏览器而已，并不真正了解浏览器。

提到浏览器往往会听到一些概念，比如：内核、引擎、JS 引擎……

我们所说的**浏览器内核**，通常指的是**渲染引擎**（Rendering Engine，或称“**排版引擎**”，即 Layout Engine）以及 **JS 引擎**。

> 最开始排版引擎和 JS 引擎并没有区分得很明确，随着 JS 引擎越来越独立，内核就倾向于指排版引擎了。
>
> 通常，浏览器内核也称“浏览器引擎”，但严格来说，**浏览器引擎**是指查询及操作渲染引擎的接口。

# 从主流说起

虽然浏览器厂商很多，但主流的浏览器无非：Chrome、Firefox、Safari、Opera、IE……

> 微软先是抛弃了 IE，推出了全新的浏览器 Edge，但最终还是拥抱了谷歌，推出了 Chromium 版的 Edge。

而主流的浏览器内核主要有四种：Blink、Gecko、WebKit、Trident……

> 随着 IE 逐渐淡出，Trident 最终可能不再是主流。

# 内核的发展

## Gecko

Gecko 是个开源的跨平台内核，由 C++ 编写。由于现在 Firefox 使用它，因此俗称“Firefox 内核”。

1998 年初，Raptor 排版引擎由网景开源发布；但因商标问题，Raptor 后改名为 NGLayout（即 Next Generation Layout）；最后，网景市场部门将 NGLayout 重命名为 Gecko。

2000 年 11 月，采用 Gecko 引擎的 Netscape 6.0 正式发布。

2003 年 7 月 15 日，美国在线（AOL）解散网景公司，Mozilla 基金会在当天成立。Gecko 由 Mozilla 基金会维护。

> Mozilla.org 是网景成立的非正式组织，是 Mozilla 基金会前身。
>

2016 年 10 月，Mozilla 宣布 Quantum 项目，目标是“构建下一代 Firefox 浏览器引擎”。它从实验项目 Servo 中引入许多改进。2017 年 11 月发布的 Firefox 57 激活 Servo 组件的初始版本。其他组件在未来版本中将逐步从 Servo 合并到 Gecko。

## Trident/EdgeHTML

1997 年 10 月，Trident 的首个版本 随 IE4 发布。因此，Trident 俗称“IE 内核”。由 C++ 编写。

> 微软还有一个 Tasman，它是 Internet Explorer for Mac 的排版引擎，它仍使用在 Office for Mac 中。

IE 与 Trident 版本的对应关系为：IE4~7（Trident 没有明确版本）、IE8（Trident 4）、IE9（Trident 5）、IE10（Trident 6）、IE11（Trident 7）……

> 虽然 Trident 不是开源的，但却是一款开放的内核，其接口内核设计相当成熟。因此，早期有许多国产浏览器都使用“IE 内核”。甚至，现在国内流行的双核浏览器，通常也是一个内核是 Trident，再加一个其他内核。并把其他内核称为“高速浏览模式”，而称 Trident 为“兼容浏览模式”。

说到这里，不得不顺便提下微软之后发布的 Microsoft Edge，它使用名为 EdgeHTML 的排版引擎，该引擎是 Trident 的一个分支。

But，2018 年 12 月，微软声明 Edge 将会重新以 Chromium 为基础，即是说 Chromium 版的 Edge 会沿用 Blink 排版引擎——EdgeHTML 继 IE 之后被微软抛弃了。

## KHTML

KDE 使用 C++ 开发，以 LGPL 授权。

由于 IE 的市场占有率太高，KHTML 显得有些小众，但它并非不重要，因为 WebKit 是它的分支。

## WebKit/WebKit2

WebKit 派生自 KTHML，主要由苹果公司开发，并用于 Apple Safari 浏览器，故俗称“Safari 内核”。

苹果于 2001 年 6 月 25 日开始 WebKit 项目。

> 实事上，WebKit 项目主要派生了两部分代码，一是从排版引擎 KHTML 派生出了 WebCore，另外是从 JS 引擎 KJS 派生出了 JavaScriptCore。
>
> JavaScriptCore 于 2002 年 6 月发布，而 WebCore 于 2003 年 1 月随 Safari 浏览器发布。

2005 年 6 月 7 日，苹果将 WebKit 开源（之前开源的 WebCore 及 JavaScriptCore）。

> WebCore 及 JavaScriptCore 以 GNU 授权，而 WebKit 其他组件以 BSD 授权。
>
> 但 Safari 浏览器是部分开源的，苹果公司的某些接口未对外开源。

2010 年 4 月 8 日，苹果发布 WebKit2。

> 在 Chrome 28 之前，Google 使用 WebKit 中 WebCore 与自己的 JavaScript V8 引擎（当时，这也统称为“WebKit引擎”）。
>
> WebKit2 会创建一个环境使网页的内容在另外一个进程（Process）运行，而这与 Google 的 V8 引擎的沙箱实现有冲突，这意味着 Google 不能使用 WebKit2 排版引擎。这就是 Google 从 WebCore 创建分支开发 Blink 排版引擎的根源所在。

## Presto

Presto 是 Opera Software 公司自行开发的排版引擎，由 C++ 编写，跨平台。并专用于 Opera 浏览器 7.0~12.18 版本。

2013 年 2 月 12 日，为了减少研发成本，Opera 宣布放弃 Presto 引擎的开发，转而跟随 Chrome 使用 WebKit。

> 不久，谷歌推出 Blink 引擎，Opera 也紧随其后使用 Blink。

2016 年 2 月 15 日，Opera 被中资昆仑万维以及 360 联合收购，同日发布 Opera 12.18，这是最后一个使用 Presto 内核的 Opera 版本。

## Blink

Blink 是谷歌发起的，由 C++ 编写的跨平台排版引擎。使用 GNU 和 BSD 许可证。

> Blink 的名字是受 Netscape Navigator 引入的非标准元素 blink 启发而来。
>
> 有个有趣的段子是说，Blink 引擎永远不会支持 blink 元素。

2013 年 4 月 3 日，谷歌在 Chromium Blog 上发表博客，将在 Chromium 项目中研发 Blink 排版引擎，标志着与 WebKit 分道扬镳。

But，实事上，Blink 并不是一个全新的排版引擎，它只是 WebKit 项目中开源组件 WebCore 的一个分支。

> 还记得上文提到 Opera Software 也跟随谷歌使用 Blink 引擎了吗？因此之后，它们一起共同开发 Blink。

# JavaScript 引擎

> 1995 年，Brendan Eich 被招聘到 Netscape，并在 10 天内设计开发出 JavaScript。

## SpiderMonkey

1996 年秋，Brendan Eich 在家两个礼拜重写 Mocha 程序库，即后来大家熟知的 SpiderMonkey。SpiderMonkey 由 C 和 C++ 实现，（很显然）是第一个 JavaScript 引擎，首先使用于 Netscape Navigator 浏览器。

Mozilla Firefox 1.0～3.0 版本也使用 SpiderMonkey。

> SpiderMonkey 的命名很有“戏剧色彩”噢！

2008 年 8 月 23 日，TraceMonkey 作为 Firefox 3.5 的 SpiderMonkey 中的编译引擎发布，是第一个为 JavaScript 语言编写的 JIT 编译器。

Mozilla Firefox 3.5 ~ 3.6 版本使用了 TraceMonkey。

2010 年初，JägerMonkey（内部称为 MethodJIT）被用来改善性能，采用组合编译（Method JIT）和汇编器（Assembler，移植于 WebKit 的 Nitro）。

Mozilla Firefox 4 开始使用 JägerMonkey。

> 德文 Jäger 原意为“猎人”。
>
> 随后，Method JIT 和 TraceMonkey 的 Tracing JIT 集成。

2013 年初，IonMonkey 作为 Firefox 18 的默认引擎发布。

2013 年 6 月 25 日，OdinMonkey 随 Firefox 22 发布，但它不是一个 JIT 编译器，用于最优化 asm.js，编译器仍为 IonMonkey。

## KJS

KJS 是 KDE 的 JavaScript 引擎，与 KDE 的排版引擎 KHTML 一样，虽不算耳熟能详但十分重要——苹果的 JavaScriptCore 是其分支。

2000 年随 KDE 项目的 Konqueror 浏览器发布。

## JScript/Chakra

1996 年 8 月，JScript 引擎随 IE3.0 发布。直至 2011 年 3 月，IE9.0 发布，IE 一直使用 JScript 引擎。

Chakra（查克拉，好“火影”的名字），准确地说，它是一个 JScript 引擎，用于 IE9 ~ IE11。

Chakra 还有一个 JavaScript 引擎分支，用于 Microsoft Edge。2015 年 12 月 5 日，微软将 Chakra 核心组件开源，并命名为 ChakraCore。

## JavaScriptCore

2002 年 6 月 13 日，Apple 发布 JavaScriptCore，它是 KJS 的一个分支。

2008 年 6 月 2 日，WebKit 项目宣布重写了 JavaScriptCore，称为“SquirrelFish”（一个字节码解释器），后来变成了 SquirrelFish Extreme（简称 SFX，市场称之为 Nitro，可将 JS 编译为机器语言不需解释器），于 2008 年 9 月 18 日发布。

## V8

V8 是由 Google 丹麦用 C++ 编写的开源 JavaScript 引擎，用于 Google Chrome、Chromium 浏览器以及 Node.js。

# 浏览器趣闻

网景公司将自己的 1.0 版本的研发代号称作 Mozilla。Mozilla 一词是由“Mosaic Killa”（Mosaic杀手/终结者，Killa 是俚语中 Killer 的拼法）和“Godzilla eat the Mosaic”（Godzilla，即“哥斯拉”，日本遭受核打击和“第五福龙丸”事件后创造的经典虚拟生物）合成而来。即Mosaic + Godzilla + Killa = Mozilla！网景公司员工也常将其称作 Moz 或 Mozzie。

自 1.0 版本发行以来，Netscape Navigator 迅速占领了市场，并且成功取代 Mosaic 成为新的 Web 标准。曾一度超过 90% 的市场占有率，并且一直保持这个占有率到 1996 年初（3.0 版本）。

1995年，微软在获得 Mosaic 的技术授权后，开发出了自己的第一代浏览器 Internet Explorer 1.0（官方简称 IE），并于同年 8 月开始在其新版 32 位操作系统 Windows 95 中搭售，这一策略完美的捆绑销售了 IE 浏览器，在当时浏览器的使用可是付费的，对这种捆绑于操作系统的销售，无疑大范围的提升了 IE 浏览器的使用率。

有一种很普遍的说法：Google Chrome 早期使用的是 WebKit 内核。但严格来说，这并不正确。开始，WebKit 仅仅是苹果的一个项目名称，该项目的确是在开发浏览器内核，主要从 KHTML 以及 KJS 中分别派生出了 WebCore 以及 JavaScriptCore。因此，所谓的“WebKit 内核”应该主要包含 WebCore 和 JavaScriptCore 两部分，而 Chrome 只使用了 WebCore，JavaScript 引擎用的是自己的 V8。严格意义上，不能说 Chrome 的内核是 WebKit。但是，由于后来内核越来越倾向于单指排版引擎，所以也称 Chrome 使用的是 WebKit 内核。这其实是语义“讹变”的结果。

# 参考

[MDN web docs - Gecko](https://developer.mozilla.org/zh-CN/docs/Mozilla/Gecko)

[维基百科（中文）- Gecko](https://zh.wikipedia.org/wiki/Gecko)

[维基百科（中文）- Trident](https://zh.wikipedia.org/wiki/Trident_(%E6%8E%92%E7%89%88%E5%BC%95%E6%93%8E))

[维基百科（中文）- EdgeHTML](https://zh.wikipedia.org/wiki/EdgeHTML)

[维基百科（中文）- KHTML](https://zh.wikipedia.org/wiki/KHTML)

[维基百科（中文）- WebKit](https://zh.wikipedia.org/wiki/WebKit)

[维基百科（中文）- Prosto](https://zh.wikipedia.org/wiki/Presto)

[维基百科（中文）- Blink](https://zh.wikipedia.org/wiki/Blink)

[【官网】WebKit - A fast, open source web browser engine.](https://webkit.org/)

[维基百科（中文）- KJS](https://zh.wikipedia.org/wiki/KJS)

[维基百科（中文）- SpiderMonkey](https://zh.wikipedia.org/wiki/SpiderMonkey)

[MDN web docs - SpiderMonkey](https://developer.mozilla.org/zh-CN/docs/Mozilla/Projects/SpiderMonkey)

[维基百科（中文）- JScript](https://zh.wikipedia.org/wiki/JScript)

[维基百科（中文）- Chakra（JScript引擎）](https://zh.wikipedia.org/wiki/Chakra_(JScript%E5%BC%95%E6%93%8E))

[维基百科（中文）- Chakra（JavaScript引擎）](https://zh.wikipedia.org/wiki/Chakra_(JavaScript%E5%BC%95%E6%93%8E))

[维基百科（中文）- V8](https://zh.wikipedia.org/wiki/V8_(JavaScript%E5%BC%95%E6%93%8E))