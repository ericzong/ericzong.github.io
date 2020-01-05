---
layout: post
title: Markdown公式语法
category: 工具
tags: Markdown 专题
excerpt: 介绍Markdown中插入公式的语法。
author: Eric Zong
mathjax: true
---

* content
{:toc}
# 概述

Markdown 的强大之处在于它可以使用纯文本描述复杂公式。

相应地，Markdown 公式的缺点显而易见，它大大降低了 Markdown 文档本身的可读性。我们不得不借助 Markdown 查看器或转换器以输出可见的公式，而不是其“定义形式”。

一个坏消息是，Markdown 查看器、编辑器或者转换器通常不支持所有形式的公式；更坏的消息是，GitHub Pages 也尚不支持 Markdown 公式；但好在，可以通过 MathJax 间接地支持公式。

# 公式语法

在 Markdown 中有两种插入公式的形式：内联公式和公式块。

内联公式位于一个段落中，语法为：`$公式$`。

公式块通常意味着其独立于前后段落，语法为：`$$公式$$`。

# 基础数学

| 名称     | 表示                                                    | 备注                                                         |
| -------- | ------------------------------------------------------- | ------------------------------------------------------------ |
| 上标     | `^`                                                     |                                                              |
| 下标     | `_`                                                     |                                                              |
| 左右标   | `\sideset`                                              | `\sideset {左标} {右标}`                                     |
| 分数     | 1. `\frac {分子} {分母}`  <br/>2. `{分子} \over {分母}` |                                                              |
| 连分数   | `\cfrac`                                                |                                                              |
| 省略号   | 1. `\ldots`<br/>2. `\cdots`                             | 1. 垂直底部对齐<br/>2. 垂直居中对齐                          |
| 加大括号 | `\left` `\right`                                        | 实质作用是加大跟随的定界符号，使其包裹其内容<br/>如果定界符号是单边的，无符号那边用 `.` 占位 |
| 矢量     | `\vec`                                                  | `overrightarrow` 也可以在上方加箭头，但与 `\vec` 的箭头在外观上是有差异的 |
| 根号     | `\sqrt`                                                 | `\sqrt [根指数，默认为 2] {被开方数}`                        |

# 高等数学

| 名称 | 表示    | 备注                                                         |
| ---- | ------- | ------------------------------------------------------------ |
| 极限 | `\lim`  | `\lim_{var \to exp} exp`                                     |
| 累加 | `\sum`  | `\sum_{下标表达式}^{上标表达式} {累加表达式}`                |
| 累乘 | `\prod` |                                                              |
| 积分 | `\int`  | `\int_积分下限^积分上限 {被积表达式}`，积分上下限使用的上下标表示。 |



# 示例

## 上下左右标

上标

`x^y` :  $x^y$

`{xy}^{mn}` : ${xy}^{mn}$

下标

`x_i` : $x_i$ 

`{xy}_{ai}` : ${xy}_{ai}$

左右标

`\sideset{^4_3}{^1_2}` : $\sideset{^4_3}{^1_2}X$

## 分数

`\frac {ab} {cd}` : $\frac {ab} {cd}$ 

`mn \over xy `: $mn \over xy$

`x = a_0 + \cfrac{1^2}{a_1 + \cfrac{2^2}{a_2 + \cfrac{3^2}{a_3 + \cdots}}}` : $x = a_0 + \cfrac{1^2}{a_1 + \cfrac{2^2}{a_2 + \cfrac{3^2}{a_3 + \cdots}}}$

## 矢量

`\vec a`: $\vec {a}$

## 开方

`\sqrt {9}`: $\sqrt {9}$

`\sqrt [3] 27`: $\sqrt [3] 27$

# 参考表格

## 括号

| 表示      | 显示      | 表示      | 显示      |
| --------- | --------- | --------- | --------- |
| `\lbrace` | $\lbrace$ | `\rbrace` | $\rbrace$ |
| `\langle` | $\langle$ | `\rangle` | $\rangle$ |
| `\lceil`  | $\lceil$  | `\rceil`  | $\rceil$  |
| `\lfloor` | $\lfloor$ | `\rfloor` | $\rfloor$ |

## 希腊字母

| 表示       | 显示       | 表示       | 显示       |
| ---------- | ---------- | ---------- | ---------- |
| `\alpha`   | $\alpha$   | `\Alpha`   | $\Alpha$   |
| `\beta`    | $\beta$    | `\Beta`    | $\Beta$    |
| `\gamma`   | $\gamma$   | `\Gamma`   | $\Gamma$   |
| `\delta`   | $\delta$   | `\Delta`   | $\Delta$   |
| `\epsilon` | $\epsilon$ | `\Epsilon` | $\Epsilon$ |
| `\zeta`    | $\zeta$    | `\Zeta`    | $\Zeta$    |
| `\eta`     | $\eta$     | `\Eta`     | $\Eta$     |
| `\theta`   | $\theta$   | `\Theta`   | $\Theta$   |
| `\iota`    | $\iota$    | `\Iota`    | $\Iota$    |
| `\kappa`   | $\kappa$   | `\Kappa`   | $\Kappa$   |
| `\lambda`  | $\lambda$  | `\Lambda`  | $\Lambda$  |
| `\mu`      | $\mu$      | `\Mu`      | $\Mu$      |
| `\nu`      | $\nu$      | `\Nu`      | $\Nu$      |
| `\xi`      | $\xi$      | `\Xi`      | $\Xi$      |
| `o`        | $o$        | `O`        | $O$        |
| `\pi`      | $\pi$      | `\Pi`      | $\Pi$      |
| `\rho`     | $\rho$     | `\Rho`     | $\Rho$     |
| `\sigma`   | $\sigma$   | `\Sigma`   | $\Sigma$   |
| `\tau`     | $\tau$     | `\Tau`     | $\Tau$     |
| `\upsilon` | $\upsilon$ | `\Upsilon` | $\Upsilon$ |
| `\phi`     | $\phi$     | `\Phi`     | $\Phi$     |
| `\chi`     | $\chi$     | `\Chi`     | $\Chi$     |
| `\psi`     | $\psi$     | `\Psi`     | $\Psi$     |
| `\omega`   | $\omega$   | `\Omega`   | $\Omega$   |

变量专用形式

| 表示          | 显示          |
| ------------- | ------------- |
| `\varepsilon` | $\varepsilon$ |
| `\vartheta`   | $\vartheta$   |
| `\varrho`     | $\varrho$     |
| `\varsigma`   | $\varsigma$   |
| `\varphi`     | $\varphi$     |

## 关系运算符

| 输入       | 显示       | 输入         | 显示         | 输入        | 显示        | 输入         | 显示         |
| :--------- | :--------- | :----------- | :----------- | :---------- | :---------- | :----------- | ------------ |
| `\pm`      | $\pm$      | `\times`     | $\times$     | `\div`      | $\div$      | `\mid`       | $\mid$       |
| `\nmid`    | $\nmid$    | `\cdot`      | $\cdot$      | `\circ`     | $\circ$     | `\ast`       | $\ast$       |
| `\bigodot` | $\bigodot$ | `\bigotimes` | $\bigotimes$ | `\bigoplus` | $\bigoplus$ | `\leq`       | $\leq$       |
| `\geq`     | $\geq$     | `\neq`       | $\neq$       | `\approx`   | $\approx$   | `\equiv`     | $\equiv$     |
| `\sum`     | $\sum$     | `\prod`      | $\prod$      | `\coprod`   | $\coprod$   | `\backslash` | $\backslash$ |

## 集合运算符

| 输入        | 显示          | 输入        | 显示        | 输入        | 显示        |
| :---------- | :------------ | :---------- | :---------- | :---------- | :---------- |
| `\emptyset` | $$\emptyset$$ | `\in`       | $\in$       | `\notin`    | $\notin$    |
| `\subset`   | $\subset$     | `\supset`   | $\supset$   | `\subseteq` | $\subseteq$ |
| `\supseteq` | $\supseteq$   | `\bigcap`   | $\bigcap$   | `\bigcup`   | $\bigcup$   |
| `\bigvee`   | $\bigvee$     | `\bigwedge` | $\bigwedge$ | `\biguplus` | $\biguplus$ |

## 对数运算符

| 输入   | 显示   |
| ------ | ------ |
| `\log` | $\log$ |
| `\lg`  | $\lg$  |
| `\ln`  | $\ln$  |

## 三角运算符

| 输入       | 显示       | 输入   | 显示   | 输入       | 显示       |
| :--------- | :--------- | :----- | :----- | :--------- | :--------- |
| `90^\circ` | $90^\circ$ | `\bot` | $\bot$ | `\angle A` | $\angle A$ |
| `\sin`     | $\sin$     | `\cos` | $\cos$ | `\tan`     | $\tan$     |
| `\csc`     | $\csc$     | `\sec` | $\sec$ | `\cot`     | $\cot$     |

## 微积分运算符

| 输入      | 显示      | 输入     | 显示     | 输入     | 显示     |
| :-------- | :-------- | :------- | :------- | :------- | :------- |
| `\int`    | $\int$    | `\iint`  | $\iint$  | `\iiint` | $\iiint$ |
| `\iiiint` | $\iiiint$ | `\oint`  | $\oint$  | `\prime` | $\prime$ |
| `\lim`    | $\lim$    | `\infty` | $\infty$ | `\nabla` | $\nabla$ |

## 逻辑运算符

| 输入       | 显示       | 输入         | 显示         | 输入          | 显示          |
| :--------- | :--------- | :----------- | :----------- | :------------ | :------------ |
| `\because` | $\because$ | `\therefore` | $\therefore$ |               |               |
| `\forall`  | $\forall$  | `\exists`    | $\exists$    | `\not\subset` | $\not\subset$ |
| `\not<`    | $\not<$    | `\not>`      | $\not>$      | `\not=`       | $\not=$       |

## 戴帽符号

| 输入         | 显示         | 输入              | 显示              |
| :----------- | :----------- | :---------------- | :---------------- |
| `\hat{xy}`   | $\hat{xy}$   | `\widehat{xyz}`   | $\widehat{xyz}$   |
| `\tilde{xy}` | $\tilde{xy}$ | `\widetilde{xyz}` | $\widetilde{xyz}$ |
| `\check{x}`  | $\check{x}$  | `\breve{y}`       | $\breve{y}$       |
| `\grave{x}`  | $\grave{x}$  | `\acute{y}`       | $\acute{y}$       |

## 连线符号

| 输入                                             | 显示                                             |
| :----------------------------------------------- | :----------------------------------------------- |
| `\fbox{a+b+c+d}`                                 | $\fbox{a+b+c+d}$                                 |
| `\overleftarrow{a+b+c+d}`                        | $\overleftarrow{a+b+c+d}$                        |
| `\overrightarrow{a+b+c+d}`                       | $\overrightarrow{a+b+c+d}$                       |
| `\overleftrightarrow{a+b+c+d}`                   | $\overleftrightarrow{a+b+c+d}$                   |
| `\underleftarrow{a+b+c+d}`                       | $\underleftarrow{a+b+c+d}$                       |
| `\underrightarrow{a+b+c+d}`                      | $\underrightarrow{a+b+c+d}$                      |
| `\underleftrightarrow{a+b+c+d}`                  | $\underleftrightarrow{a+b+c+d}$                  |
| `\overline{a+b+c+d}`                             | $\overline{a+b+c+d}$                             |
| `\underline{a+b+c+d}`                            | $\underline{a+b+c+d}$                            |
| `\overbrace{a+b+c+d}^{sum}`                      | $\overbrace{a+b+c+d}^{sum}$                      |
| `\underbrace{a+b+c+d}_{sum}`                     | $\underbrace{a+b+c+d}_{sum}$                     |
| `\overbrace{a+\underbrace{b+c}_{10}+d}^{10}`     | $\overbrace{a+\underbrace{b+c}_{10}+d}^{10}$     |
| `\underbrace{a\cdot a\cdots a}_{n\text{ times}}` | $\underbrace{a\cdot a\cdots a}_{n\text{ times}}$ |

## 箭头符号

| 输入                  | 显示                  | 输入                  | 显示                  |
| --------------------- | --------------------- | --------------------- | --------------------- |
| `\to`                 | $\to$                 | `\mapsto`             | $\mapsto$             |
| `\implies`            | $\implies$            | `\impliedby`          | $\impliedby$          |
| `\iff`                | $\iff$                |                       |                       |
| `\uparrow`            | $\uparrow$            | `\Uparrow`            | $\Uparrow$            |
| `\downarrow`          | $\downarrow$          | `\Downarrow`          | $\Downarrow$          |
| `\leftarrow`          | $\leftarrow$          | `\Leftarrow`          | $\Leftarrow$          |
| `\rightarrow`         | $\rightarrow$         | `\Rightarrow`         | $\Rightarrow$         |
| `\leftrightarrow`     | $\leftrightarrow$     | `\Leftrightarrow`     | $\Leftrightarrow$     |
| `\longleftarrow`      | $\longleftarrow$      | `\Longleftarrow`      | $\Longleftarrow$      |
| `\longrightarrow`     | $\longrightarrow$     | `\Longrightarrow`     | $\Longrightarrow$     |
| `\longleftrightarrow` | $\longleftrightarrow$ | `\Longleftrightarrow` | $\Longleftrightarrow$ |

## 字体转换

| 输入    | 说明       | 显示            |
| :------ | :--------- | :-------------- |
| `\rm`   | 罗马体     | $\rm{Sample}$   |
| `\it`   | 意大利体   | $\it{Sample}$   |
| `\frak` | 旧德式字体 | $\frak{Sample}$ |
| `\bf`   | 粗体       | $\bf{Sample}$   |
| `\mit`  | 数学斜体   | $\mit{SAMPLE}$  |
| `\Bbb`  | 黑板粗体   | $\Bbb{SAMPLE}$  |
| `\cal`  | 花体       | $\cal{SAMPLE}$  |
| `\sf`   | 等线体     | $\sf{Sample}$   |
| `\tt`   | 打字机体   | $\tt{Sample}$   |
| `\scr`  | 手写体     | $\scr{SAMPLE}$  |

注：`{\字体 string}`，默认为 `\it`。

## 文本颜色

| 颜色   | 显示                   | 颜色    | 显示                    |
| :----- | :--------------------- | :------ | :---------------------- |
| black  | $\color{black}{text}$  | grey    | $\color{grey}{text}$    |
| silver | $\color{silver}{text}$ | white   | $\color{white}{text}$   |
| maroon | $\color{maroon}{text}$ | red     | $\color{red}{text}$     |
| yellow | $\color{yellow}{text}$ | lime    | $\color{lime}{text}$    |
| olive  | $\color{olive}{text}$  | green   | $\color{green}{text}$   |
| teal   | $\color{teal}{text}$   | auqa    | $\color{auqa}{text}$    |
| blue   | $\color{blue}{text}$   | navy    | $\color{navy}{text}$    |
| purple | $\color{purple}{text}$ | fuchsia | $\color{fuchsia}{text}$ |

注：文本颜色依赖浏览器支持，以上是 HTML4/CSS2 预定义的颜色，另外也可能支持 HTML5/CSS3 预定义的颜色。可以用 `\color {color name} {text}` 或 `\color {#rgb} {text}`。



# 参考

[Cmd Markdown 公式指导手册](https://www.zybuluo.com/codeep/note/163962)

[Detexify^2^ - LaTeX symbol classifier](http://detexify.kirelabs.org/classify.html)