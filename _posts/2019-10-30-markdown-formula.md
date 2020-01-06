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

> 本文发布在 GitHub Pages 中，即使用了 MathJax 支持公式。 不过，并非所有的公式都得到了支持，不支持的公式会以红色原样输出。

# 公式语法

在 Markdown 中有两种插入公式的形式：内联公式和公式块。

内联公式位于一个段落中，语法为：`$公式$`。

公式块通常意味着其独立于前后段落，语法为：`$$公式$$`。

# 基础数学

| 名称   | 表示                                                    | 备注                                                         |
| ------ | ------------------------------------------------------- | ------------------------------------------------------------ |
| 上标   | `^`                                                     |                                                              |
| 下标   | `_`                                                     |                                                              |
| 左右标 | `\sideset`                                              | `\sideset {左标} {右标}`                                     |
| 分数   | 1. `\frac {分子} {分母}`  <br/>2. `{分子} \over {分母}` |                                                              |
| 连分数 | `\cfrac`                                                |                                                              |
| 省略号 | 1. `\ldots`<br/>2. `\cdots`                             | 1. 垂直底部对齐<br/>2. 垂直居中对齐                          |
| 自适应 | 1. `\left` `\right` <br/>2. `\middle`                   | 1. 使跟随的定界符号根据其包裹内容自适应高度。如果定界符号是单边的，无符号那边用 `.` 占位<br/>2. 内部分隔符自适应 |
| 矢量   | `\vec`                                                  | `overrightarrow` 也可以在上方加箭头，但与 `\vec` 的箭头在外观上是有差异的 |
| 根号   | `\sqrt`                                                 | `\sqrt [根指数，默认为 2] {被开方数}`                        |

# 高等数学

| 名称 | 表示    | 备注                                                         |
| ---- | ------- | ------------------------------------------------------------ |
| 极限 | `\lim`  | `\lim_{var \to exp} exp`                                     |
| 累加 | `\sum`  | `\sum_{下标表达式}^{上标表达式} {累加表达式}`                |
| 累乘 | `\prod` |                                                              |
| 积分 | `\int`  | `\int_积分下限^积分上限 {被积表达式}`，积分上下限使用的上下标表示。 |

# 矩阵

```markdown
$$
  \begin{matrix}
    a & b & c \\
    x & y & z \\
    1 & 2 & 3 \\
  \end{matrix}
$$
```

矩阵以 `\begin{matrix}` 开头，`\end{matrix}` 结尾，以 `\\` 分隔行，以 `&` 分隔元素。

矩阵边框由 `\begin` 和 `\end` 后花括号中的名称定义，包括：`matrix`、`pmatrix`、`bmatrix`、`Bmatrix`、`vmatrix`、`Vmatrix`。

# 方程

```markdown
$$
  \begin{align}
    ... \\
  \end{align}
$$
```

方程以 `\begin{align}` 开头，`\end{align}` 结尾，以 `\\` 分隔行。并可在每行某个位置插入 `&` 以使各行在垂直方向上依此对齐 —— 比如，放在每行的递推等号之前使等于对齐。

# 条件表达式

```markdown
$$
  f(n) = 
  \begin{cases}
  case1 \\
  ...
  \end{cases}
$$
```

条件表达式以 `\begin{cases}` 开头，`\end{cases}` 结尾，以 `\\` 分隔行，以 `&` 对齐内容。

上面的公式连接条件表达式的括号在左边，如果想在右边显示应该这样：

```markdown
$$
  \left.
  \begin{array}{r}
  case1 - something \\
  ... \\
  \end{array}
  \right\}
$$
```

注：`{l}`，`{r}`和`{c}`指定条件表达式的对齐方向。

当某个条件表达式行高超过标准行高时，与其相邻的条件表达式可能会紧贴在一起，这时就需要设置行高。

设置行高只需要在行分隔符 `\\` 后紧跟行高的设定值即可。该值由一对 `[]` 包裹，比如：`[2ex]` 。

> `ex` 指 X-Height，即 x 字母高度。
>
> 行高的单位不仅可以是 `ex`，也可以是 `px`（像素）。

# 表格

```markdown
$$
\begin{array}{c|clr}
  \, & \text{居中} & \text{左对齐} & \text{右对齐} \\
  \hline
  o & x & y & z \\
  ...
\end{array}
$$
```

表格以 `\begin{array}` 开头，`\end{array}` 结尾，以 `\\` 分隔行，以 `&`  分隔元素。

在 `\begin{array}` 后指定每列的对齐方式，`c`、`l`、`r` 分别表示居中、左对齐、右对齐。

垂直分割线在对齐定义中插入 `|`，水平分割线在行间插入 `\hline`。

# 杂项

| 名称     | 输入           | 备注                                |
| -------- | -------------- | ----------------------------------- |
| 行标     | `\tag {行标}`  |                                     |
| 文本     | `\text {文字}` | 其作用是可以在其中嵌套使用 `$公式$` |
| 加大字体 | `\large`       |                                     |
| 减小字体 | `\small`       |                                     |

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

## 括号

| 表示      | 显示      | 表示      | 显示      |
| --------- | --------- | --------- | --------- |
| `\lbrace` | $\lbrace$ | `\rbrace` | $\rbrace$ |
| `\langle` | $\langle$ | `\rangle` | $\rangle$ |
| `\lceil`  | $\lceil$  | `\rceil`  | $\rceil$  |
| `\lfloor` | $\lfloor$ | `\rfloor` | $\rfloor$ |

## 间隔

| 输入              | 说明               |
| ----------------- | ------------------ |
| `\,`              | 宽度较一个空格稍窄 |
| `\;`              | 宽度一个空格       |
| `\quad`           | 宽度 4 个空格      |
| `\qquad`          | 宽度 8 个空格      |
| `\text {n个空格}` |                    |

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

## 删除线

| 输入                                             | 显示                                                         | 说明                                    |
| ------------------------------------------------ | ------------------------------------------------------------ | --------------------------------------- |
| `\require{cancel}`                               |                                                              | 使能 `\cancel`                          |
| `\cancel`                                        | $\require{cancel}\cancel{x}$                                 |                                         |
| `\bcancel`                                       | $\require{cancel}\bcancel{x}$                                |                                         |
| `\xcancel`                                       | $\require{cancel}\xcancel{x}$                                |                                         |
| `\cancelto`                                      | $\require{cancel}\cancelto{y}{x}$                            |                                         |
| `\require{enclose}`                              |                                                              | 使能 `\enclose`                         |
| `\enclose{删除线效果}{字符}`                     |                                                              | 有 4 种删除线效果，可组合使用，`,` 分隔 |
| `\enclose{horizontalstrike}`                     | $\require{enclose}\enclose{horizontalstrike}{x}$             |                                         |
| `\enclose{verticalstrike}`                       | $\require{enclose}\enclose{verticalstrike}{x}$               |                                         |
| `\enclose{updiagonalstrike}`                     | $\require{enclose}\enclose{updiagonalstrike}{x}$             |                                         |
| `\enclose{downdiagonalstrike}`                   | $\require{enclose}\enclose{downdiagonalstrike}{x}$           |                                         |
| `\enclose{updiagonalstrike, downdiagonalstrike}` | $\require{enclose}\enclose{updiagonalstrike, downdiagonalstrike}{x}$ | 组合使用                                |

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

| 输入                          | 显示                          |
| :---------------------------- | :---------------------------- |
| `\fbox{a+b+c}`                | $\fbox{a+b+c}$                |
| `\overleftarrow{a+b+c}`       | $\overleftarrow{a+b+c}$       |
| `\overrightarrow{a+b+c}`      | $\overrightarrow{a+b+c}$      |
| `\overleftrightarrow{a+b+c}`  | $\overleftrightarrow{a+b+c}$  |
| `\underleftarrow{a+b+c}`      | $\underleftarrow{a+b+c}$      |
| `\underrightarrow{a+b+c}`     | $\underrightarrow{a+b+c}$     |
| `\underleftrightarrow{a+b+c}` | $\underleftrightarrow{a+b+c}$ |
| `\overline{a+b+c}`            | $\overline{a+b+c}$            |
| `\underline{a+b+c}`           | $\underline{a+b+c}$           |
| `\overbrace{a+b+c}^{sum}`     | $\overbrace{a+b+c}^{sum}$     |
| `\underbrace{a+b+c}_{sum}`    | $\underbrace{a+b+c}_{sum}$    |

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

## 矩阵

### 边框

| 输入        | 显示                                                | 输入        | 显示                                                |
| ----------- | --------------------------------------------------- | ----------- | --------------------------------------------------- |
| `{matrix}`  | $ \begin{matrix} a & b \\ c & d \\ \end{matrix} $   | `{pmatrix}` | $ \begin{pmatrix} a & b \\ c & d \\ \end{pmatrix} $ |
| `{bmatrix}` | $ \begin{bmatrix} a & b \\ c & d \\ \end{bmatrix} $ | `{Bmatrix}` | $ \begin{Bmatrix} a & b \\ c & d \\ \end{Bmatrix} $ |
| `{vmatrix}` | $ \begin{vmatrix} a & b \\ c & d \\ \end{vmatrix} $ | `{Vmatrix}` | $ \begin{Vmatrix} a & b \\ c & d \\ \end{Vmatrix} $ |

### 内联显示

`{smallmatrix}` 可用于显示内联矩阵，它的效果是使得矩阵显示得更小，更适合在行内显示。

### 省略号

| 输入     | 显示     | 输入     | 显示     | 输入     | 显示     |
| -------- | -------- | -------- | -------- | -------- | -------- |
| `\cdots` | $\cdots$ | `\ddots` | $\ddots$ | `\vdots` | $\vdots$ |

# 参考

[Cmd Markdown 公式指导手册](https://www.zybuluo.com/codeep/note/163962)

[Detexify$^2$ - LaTeX symbol classifier](http://detexify.kirelabs.org/classify.html)

