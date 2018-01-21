---
title: LaTeX-Math
date: 2018-01-14 10:18:37
categories: Hexo&Markdown
tags:
- LaTeX
- Math
- Markdown
---

# 简介

\LaTeX可以排版公式与文字，这篇文章只介绍与数学公式相关的排版。更具体地说是写到Markdown文件中的LaTeX公式。

`原生hexo并不支持数学公式，需要安装插件 mathJax。mathJax 是一款运行于浏览器中的开源数学符号渲染引擎，使用 mathJax 可以方便的在浏览器中嵌入数学公式。mathJax 使用网络字体产生高质量的排版，因此可适应各种分辨率，它的显示是基于文本的而非图片，因此显示效果更好。这些公式可以被搜索引擎使用，因此公式里的符号一样可以被搜索引擎检索到。`

# mathJax的安装和配置

安装命令：

```
npm install hexo-math --save
```

打开站点配置文件_config.yml，添加配置：

```
# MathJax Plugin
## Docs: https://www.mathjax.org/
math:
  engine: 'mathjax' # or 'katex'
  mathjax:
    # src: custom_mathjax_source
    config:
      # MathJax config
```

打开主题配置文件，如next的配置文件\themes\hexo-theme-next-master\_config.yml，修改mathJax项为true：

```
# MathJax Support
mathjax:
  enable: true
  per_page: false
  cdn: //cdn.bootcss.com/mathjax/2.7.1/latest.js?config=TeX-AMS-MML_HTMLorMML
```

另外可以在文章开头通过js代码来临时应用mathJax插件

```
<script type="text/javascript"
   src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
```

# 基础语法

## 显示区域

LaTeX公式有两种，

一种是嵌入到正文中，用`$公示内容$`定义；

另一种单独成行显示，用`$$公示内容$$`定义。

例如，

```
我们定义$f(x) = \sum_{i=0}^{N}\int_{a}^{b} g(t,i) \text{ d}t$. (行内公式)

或者定义$f(x)$如下（行间公式）: 
$$f(x) = \sum_{i=0}^{N}\int_{a}^{b} g(t,i) \text{ d}t{6}\tag{1}$$
```

得到结果为

我们定义$f(x) = \sum_{i=0}^{N} \int_{a}^{b} g(t,i) \text{ d}t$. (行内公式)

或者定义$f(x)$如下（行间公式）: 
$$f(x) = \sum_{i=0}^{N} \int_{a}^{b} g(t,i) \text{ d}t{6} \tag{1}$$

## 上下标

用^表示上标，用_表示下标

```
$$f(x) = a_1x^n + a_2x^{n-1} + a_3x^{n-2}$$
```

显示为$f(x) = a_1x^n + a_2x^{n-1} + a_3x^{n-2}$

左右两侧都有上下标时，使用`\sideset`命令

```
$$\sideset{^n_k}{^x_y}a$$
```

显示结果为$\sideset{^n_k}{^x_y}a$

## 括号

在markdown语法中，大括号`{}`有特殊意义，如果要显示括号，可以使用反斜杠`\`转义`\brace`和`rbrace`来显示大括号。

```
$$\{x*y\}$$
$$\lbracex*y\rbrace$$
```

显示为

$$\{{x*y}\}$$
$$\lbrace{x*y}\rbrace$$

单纯使用`\lbrace`和`\rbrace`显示的大括号并不会随着公式内容自动缩放。要实现自动缩放需要添加上`\left`和`\right`来修饰。

```
$$\left \lbrace \sum_{i=0}^n i^3 = \frac{(n^2+n)(n+6)}{9} \right \rbrace$$
```

显示为

$$\left \lbrace \sum_{i=0}^n i^3 = \frac{(n^2+n)(n+6)}{9} \right \rbrace$$

不然则会显示为

$$\lbrace \sum_{i=0}^n i^3 = \frac{(n^2+n)(n+6)}{9} \rbrace$$

## 分数

使用语法`\frac`或者`\over`来实现

```
$\frac {x+1}{y-2}$
$ x+1 \over y-2 $
```

显示结果为$\frac {x+1}{y-2}$和$ x+1 \over y-2 $

## 开方

开方语法为`\sqrt`

```
$ \sqrt{x^5} $
$ \sqrt[3]{\frac xy} $
```

分别显示为：$ \sqrt{x^5} $和 $ \sqrt[3]{\frac xy} $

## 求和与积分

求和用`\sum`，后面常跟着上下标

```
$ \sum_{i=0}^n $
```

显示为$ \sum_{i=0}^n $

积分用`\int`，后面也是常跟着上下标

```
$ \int_1^\infty $
$ \iint_1^\infty $
```

显示为$ \int_1^\infty $和$ \iint_1^\infty $

## 极限

极限使用`\lim`

```
$ \lim_{x \to 0} $
```

显示为$ \lim_{x \to 0} $

## 表格

表格样式lcr表示居中，|加入一条竖线，\hline表示行间横线，列之间用&分隔，行之间用\分隔。

```
$$\begin{array}{c|lcr}
n & \text{Left} & \text{Center} & \text{Right} \\\\
\hline
1 & 1.97 & 5 & 12 \\\\
2 & -11 & 19 & -80 \\\\
3 & 70 & 209 & 1+i \\\\
\end{array}$$
```

显示为
$$
\begin{array}{c|lcr}
n & \text{Left} & \text{Center} & \text{Right} \\\\
\hline
1 & 1.97 & 5 & 12 \\\\
2 & -11 & 19 & -80 \\\\
3 & 70 & 209 & 1+i \\\\
\end{array}
$$

## 常用公式命令汇总

### 希腊字母

| 命令         | 显示   |      | 命令       | 显示   |
| ---------- | ---- | ---- | -------- | ---- |
| `\alpha`   | α    |      | `\beta`  | β    |
| `\gamma`   | γ    |      | `\delta` | δ    |
| `\epsilon` | ϵ    |      | `\zeta`  | ζ    |
| `\eta`     | η    |      | `\theta` | θ    |
| `\iota`    | ι    |      | `\kappa` | κ    |
| `\lambda`  | λ    |      | `\mu`    | μ    |
| `\xi`      | ξ    |      | `\nu`    | ν    |
| `\pi`      | π    |      | `\rho`   | ρ    |
| `\sigma`   | σ    |      | `\tau`   | τ    |
| `\upsilon` | υ    |      | `\phi`   | ϕ    |
| `\chi`     | χ    |      | `\psi`   | ψ    |
| `\omega`   | ω    |      |          |      |

如果要改成大写字母，把命令的首字母变成大写字母即可。例如`\Gamma`的显示结果为$\Gamma$。如果要使用斜体的Gamma，可在Gamma命令前加上var，例如`\varGamma`的显示结果为$\varGamma$。

### 和号和积分号

| 命令                  | 显示    |      | 命令                  | 显示    |
| ------------------- | ----- | ---- | ------------------- | ----- |
| `\sum`              | ∑     |      | `\int`              | ∫     |
| `\sum_{i=1}^{N}`    | ∑Ni=1 |      | `\int_{a}^{b}`      | ∫ba   |
| `\prod`             | ∏     |      | `\iint`             | ∬     |
| `\prod_{i=1}^{N}`   | ∏Ni=1 |      | `\iint_{a}^{b}`     | ∬ba   |
| `\bigcup`           | ⋃     |      | `\bigcap`           | ⋂     |
| `\bigcup_{i=1}^{N}` | ⋃Ni=1 |      | `\bigcap_{i=1}^{N}` | ⋂Ni=1 |

### 矩阵

使用`$$\begin{matrix}...\end{matrix}$$`来显示矩阵。

```
$$
    \begin{matrix}
    1 & x & x^2 \\
    1 & y & y^2 \\
    1 & z & z^2 \\
    \end{matrix}
$$
```

得到显示
$$
\begin{matrix}
    1 & x & x^2 \\
    1 & y & y^2 \\
    1 & z & z^2 \\
    \end{matrix}
$$
若要显示矩阵外的各种括号，可以替换`matrix`为`pmatrix` $\begin{pmatrix}1 & 2 \\3 & 4  \\\end{pmatrix}$, `bmatrix` $\begin{bmatrix}1 & 2 \\3 & 4  \\\end{bmatrix}$, `Bmatrix` $\begin{Bmatrix}1 & 2 \\3 & 4  \\\end{Bmatrix}$, `vmatrix` $\begin{vmatrix}1 & 2 \\3 & 4  \\\end{vmatrix}$, `Vmatrix` $\begin{Vmatrix}1 & 2 \\3 & 4  \\\end{Vmatrix}$

若要在矩阵中添加省略用的点，可以使用`\cdots`$\cdots$,`\cdots`$\cdots$,`\vdots`$\vdots$，例如，
$$
\begin{pmatrix}
 1 & a_1 & a_1^2 & \cdots & a_1^n \\
 1 & a_2 & a_2^2 & \cdots & a_2^n \\
 \vdots  & \vdots& \vdots & \ddots & \vdots \\
 1 & a_m & a_m^2 & \cdots & a_m^n    
 \end{pmatrix}
$$

多个矩阵组成等式时，

```
$$\left[
\begin{matrix}
V_A \\\\
V_B \\\\
V_C \\\\
\end{matrix}
\right] =
\left[
\begin{matrix}
1 & 0 & L \\\\
-cosψ & sinψ & L \\\\
-cosψ & -sinψ & L
\end{matrix}
\right]
\left[
\begin{matrix}
V_x \\\\
V_y \\\\
W \\\\
\end{matrix}
\right] $$
```

显示为
$$
\left[

\begin{matrix}

V_A \\

V_B \\

V_C \\

\end{matrix}

\right] =

\left[

\begin{matrix}

1 & 0 & L \\

-cosψ & sinψ & L \\

-cosψ & -sinψ & L

\end{matrix}

\right]

\left[

\begin{matrix}

V_x \\

V_y \\

W \\

\end{matrix}

\right]
$$

### 其他常用命令

| 命令               | 显示               |      | 命令            | 显示            |
| ---------------- | ---------------- | ---- | ------------- | :------------ |
| `\sqrt[3]{2}`    | $\sqrt[3]{2}$    |      | `\sqrt{2}`    | $\sqrt{2}$    |
| `x^{3}`          | $x^{3}$          |      | `x_{3}`       | $x_{3}$       |
| `\lim_{x \to 0}` | $\lim_{x \to 0}$ |      | `\frac{1}{2}` | $\frac{1}{2}$ |

### 添加公式代号

在命令的最后加上`\tag{}`即可。例如`\frac{1}{2} = 0.5\tag{1}`显示结果为,
$$
\frac{1}{2} = 0.5\tag{1}
$$

# 参考

http://blog.csdn.net/bendanban/article/details/44196101

https://math.meta.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference

http://stevenshi.me/2017/06/26/hexo-insert-formula/