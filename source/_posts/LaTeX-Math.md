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

LaTeX可以排版公式与文字，这篇文章只介绍与数学公式相关的排版。更具体地说是写到Markdown文件中的LaTeX公式。

# 基础语法

## 显示区域

LaTeX公式有两种，一种是嵌入到正文中的，用`$公示内容$`定义，另一种是单独成行显示的，用`$$公示内容$$`定义。

例如，

```
我们定义$f(x) = \sum_{i=0}^{N}\int_{a}^{b} g(t,i) \text{ d}t$. (行内公式)

或者定义$f(x)$如下（行间公式）: 
$$f(x) = \sum_{i=0}^{N}\int_{a}^{b} g(t,i) \text{ d}t{6}\tag{1}$$
```

得到结果为



> 我们定义$f(x) = \sum_{i=0}^{N}\int_{a}^{b} g(t,i) \text{ d}t$. (行内公式)
>
> 或者定义$f(x)$如下（行间公式）: 
> $$f(x) = \sum_{i=0}^{N}\int_{a}^{b} g(t,i) \text{ d}t{6}\tag{1}$$

## 常用公式命令

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

### 其他常用命令

| 命令               | 显示     |      | 命令            | 显示            |
| ---------------- | ------ | ---- | ------------- | :------------ |
| `\sqrt[3]{2}`    | 2√3    |      | `\sqrt{2}`    | 2√            |
| `x^{3}`          | x3     |      | `x_{3}`       | x3            |
| `\lim_{x \to 0}` | limx→0 |      | `\frac{1}{2}` | $\frac{1}{2}$ |

### 添加公式代号

在命令的最后加上`\tag{}`即可。例如`\frac{1}{2} = 0.5\tag{1}`显示结果为$\frac{1}{2} = 0.5\tag{1}$

# 参考

http://blog.csdn.net/bendanban/article/details/44196101

https://math.meta.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference