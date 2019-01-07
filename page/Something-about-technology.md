---
layout: page
title: 科技那些事儿
permalink: /page/something-about-technology
---

* content
{:toc}
# 那些科技定律

## 摩尔定律

1965年英特尔公司的创始人戈登摩尔博士提出，至多在十年内，集成电路的集成度会每两年翻一番，后来大家把这个周期缩短到18个月。简单的说，每过18个月计算机等IT产品的性能会翻一番，或者说相同性能的计算机IT产品每18个月价格会降一半。

## 安迪比尔定律

安迪是原英特尔公司CEO安迪格鲁夫，比尔就是微软公司的创始人比尔盖茨。这个定律的意思是说安迪所给的最终被比尔拿走了。

# 那些历史

## Mozilla User-Agent

我们很容易发现，众多浏览器都被探测为 Mozilla User-Agent。

必须承认，事实上，这不是 JavaScript  的错，是各个浏览器有意为之。比如，以下是用 JavaScript 探测当前最新版的 Chrome 时得到的结果：
```
console.log(navigator.userAgent )
Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/72.0.3610.2 Safari/537.36
```
IE11 结果如下：

```
Mozilla/5.0 (Windows NT 10.0; WOW64; Trident/7.0; .NET4.0C; .NET4.0E; rv:11.0) like Gecko
```

是否注意到其中的第一个单词 Mozilla/5.0，为什么都被探测为 Mozilla，为什么它们要这样误导开发者。事实上，你会发现，绝大多数浏览器把他们的 User Agent 设置为 Mozilla，答案要回到10年前，这更多是一种策略。
User Agent 是一段用来标识当前浏览器身份的字符串，世界上第一个浏览器 Mosaic， 曾这样标志自己：

```
Mosaic/0.9 //browser name/version number
```
这很合理，因此当 Netscape 出来的时候，它保留了 Mosaic 这个传统，还在后面添加了一个加密方式部分。
```
Mozilla/2.02 [en] (Win95; I) // browser name/version/encryption
```
到目前为止，一切安好，直到 IE3 发布，当 IE3 发布的时候，Netscape 正如日中天，那时，很多服务器和程序已经部署了客户端探测机制，以便认出 Netscape，虽然现在看来，这很值得争议，但当时并没什么。当 IE 初次推出它们的 User Agent 标志的时候，是这个样子：
```
MSIE / 3.0 (Win95; U)
```
这让 IE 很被动，因为 Netscape 已经能被很多服务器识别，因此，开发者们干脆希望 IE 被误认为 Mozilla，然后，再单独加一个 IE 的标签。
```
Mozilla/2.0(compatible; MSIE 3.0; Windows 95)
```
如今，几乎所有浏览器都步 IE 后尘，将自己标识为 Mozilla，这大概是一种连锁反应。

## JavaScript 命名

名称演变：Mocha → LiveScript → JavaScript

根据历史记录，JavaScript 的命名与 Netscape 和 Sun 之间的合作有关，作为交换条件，Netscape 在他们备受欢迎的浏览器中创建了 Java 运行时。值得一提的是，这个名字的出台几近一个玩笑，要知道，LiveScript 和 Java 在客户端脚本方面存在敌对关系。

不管怎么说，人们后来不得不一再澄清的一件事就是，JavaScript 和 Java 毫无关系。

## OS 历史

* 1964.4.7，IBM System/360 系列机器，OS/360 操作系统；
* 1965， AT & T 公司下贝尔实验室（Bell Labs）加入通用电器（General Electric）和麻省理工（MIT）合作计划，Multics 操作系统；
* 1969，Multics 被放弃；Ken Thompson，借鉴 Multics 优点开发小作业系统——Unics；
* 1970，移植改进 Unics，并更名为 Unix；
* 1973，Thompson 和 Ritchie 改进 B 语言，发明 C 语言；
* 1979，AT & T 收回 Unix 版权，不对学生提供源码，教学受影响；
* 1984~1986，Andrew Tanenbaum 教授，Minix 教学型操作系统；
* 1988，Linus Torvalds 进入赫尔辛基大学计算机科学系；
* 1991，Linus 在 BBS 上发布 Linux;
* 2003，Andy Rubin 创办 Android 公司；
* 2005，Android 被 Google 收购；
* 2007.11.5，Google 发布 Android 操作系统。

## 世是上第一块硬盘

IBM公司在1956年9月推出了世界上第一块硬盘IBM 350 RAMAC，容量为5MB，用到50个自行车轮子那么大的盘片，传输速率也仅有9KB/s——仅仅比Modem拨号上网的速率稍快一点，价格高达35000美元（可以买一辆宝马了）。 

> 成本和工艺水平决定了硬盘传输速率的上限，而网速定义了下限。

# 那些应该知道的

## 汉字排版的的优点

与任何拼音制文字相比，汉字所占空间要小得多，根据全苏印刷科学研究所的统计，印刷同一内容，以一个汉字与一个俄文字母大小相同的铅字排印，汉字本仅为俄 文本的五分之一。比如，印一部《战争与和平》，俄文本为汉文译本工料的5倍。以阅速相同来计算，俄文本要用1000小时的话，读汉文本只用200小时，要节约80%的时间。据统计，同样的内容，英文本比汉文本至少多一倍以上，把《为人民服务》一文按一个汉字对一个与汉字相同型号的汉语拼音字母翻改成英文， 连同字与字之间的空格，其篇幅是汉字篇幅的2.5倍还多。也就是说，若用拉丁化拼音汉字印书，比用汉字印书要多花一倍半的纸张，印工和阅读时间。

>鉴于汉字排版的优越性，在同等内容质量的情况下，中文版才是阅读的首选。
>
>但是，从另一方面来看，不同的语言思维模式是存在差异的，翻译丢失或加入某些信息是很常见的。况且，越精练的语言越更具二义性。因此，在我看来，同一本书的不同译本“内容质量”几乎不可能是相同的。

## 键盘输入真的快吗？ 
最初，打字机的键盘是按照字母顺序排列的，但打字速度过快时某些键的组合很容易出现卡键问题，于是克里斯托夫拉森授斯（Christopher Latham Sholes）发明了QWERTY键盘布局来减慢输入速度，以避免卡键问题。具有讽刺意味的是，这种129年前形成的，以放慢敲键速度为目的的键盘排列方式却延续至今。 

> 还记得小时候第一次接触电脑，就被要求学会“盲打”，于是，我整整花了一个下午的时间记忆键位和练习，才勉强能“盲打”出 ABCD……
>
> 如果字母本身是有序的，那么，键位排列理应按相同的顺序呀，现在这是个什么顺序？小小的我很困惑。
>
> 原来，QWERTY 键盘是机械工艺跟不上打字速度的产物。
>
> 等等，现在机械工艺水平应该不存在这种问题了，但是，键盘布局为什么却没有改回去？
>
> 习惯？也许不仅仅这么简单。
>
> 先看另一个问题：如果铁轨间距更大一些，那么，相同长度的车厢将有更大的容量，那为什么不把铁轨间距修大一点呢？
>
> 一个重要原因是，一旦一个间距成为标准，那么所有的生产、使用等等环节都会为其配套。那么，正所谓牵一发而动全身，变更的成本太高了嘛。

## 奇怪的按键

vi 编辑器中，H、J、K、L 分别代表左、下、上、右；Linux 等系统中，~ 代表当前用户主目录（Home）。

为什么？这其中有什么逻辑关系吗？

最直接的答案是：在古老的键盘上，H、J、K、L 分别和 ←、↓、↑、→ 在同一个键位，而 ~ 和 Home 在同一个键位。

> 老一代的程序员中流传一句看似笑话的话：这不是一个 Bug，这是一个特性。
>
> 现在的不可理喻，也许只是当初的理所应当。

## 时区不是永恒不变的

随着时间的变化，时区是在改变的。这意味着欧洲/伦敦在新纪元的时候是 1970/1/1 01:00 而不是 00:00，为什么？因为伦敦在 1968 年到 1971 年这两年间的时间内使用的是夏令时。

在过去的这些年里面，还有不少时区也发生了变化。莫斯科以前是东三区（GMT+3），现在是东四区（GMT+4）（从2011 年 3 月 27 日开始）。如果你看下 2010 年的时间，你会发现它是东三区而不是东四区。

还有些事你听起来或许会感觉很意外：

* 1721 年的瑞典的 2 月有 30 天。
* 1751 年英格兰的第一天是 3 月 25 日，和法国相比差了 11 天。
* 美国采用公历纪年后，它往前追溯了上百年，这样原先记录的那些日期都可以用两种日历来进行表示（通常为了更精确会同时提供两个日期）。比如乔治华盛顿的生日从 1731 年 2 月 11 变成了 1732 年 2 月 22。