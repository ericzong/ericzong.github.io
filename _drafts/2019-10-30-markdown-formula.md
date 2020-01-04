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

# 语法

在 Markdown 中有两种插入公式的形式：内联公式和公式块。

内联公式位于一个段落中，语法为：`$公式$`。

公式块通常意味着其独立于前后段落，语法为：`$$公式$$`。

# 基础数学

| 名称     | 表示                                                    | 备注                                                         |
| -------- | ------------------------------------------------------- | ------------------------------------------------------------ |
| 上标     | `^`                                                     |                                                              |
| 下标     | `_`                                                     |                                                              |
| 左右标   | `\sideset`                                              |                                                              |
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



# 参考

[Cmd Markdown 公式指导手册](https://www.zybuluo.com/codeep/note/163962)

[Detexify^2^ - LaTeX symbol classifier](http://detexify.kirelabs.org/classify.html)