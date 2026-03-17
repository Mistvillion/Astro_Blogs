---
title: MLE 与 OLS 合理性推导
published: 2026-03-17
description: 线性回归中，最大似然估计和最小二乘法合理性的推导
---
做线性回归时，为什么偏偏选择最小二乘作为代价函数？这不是随便选的。在很自然的概率假设下，最小二乘 = 最大似然估计，是数学上推出来的必然结果。这篇文章将用最自然的语言和最朴素的数学推导来回答这个问题。

## 一、基础概念

先来弄清楚几个基础的概念：

### 1. 什么是“似然”

* 概率：已知模型或参数，求数据（举例：已知硬币是公平的，求抛一次正面的概率）
* 似然：已知数据，求模型或参数（举例：已知抛10次硬币，有9次朝上，问硬币公平吗）

### 2. 什么是“似然函数”

一元线性回归中，假设函数 $h_\theta(x) = \theta_0 + \theta_1x$ 。我们已经有了一系列的真实数据 $(x^{(1)},y^{(1)}),(x^{(2)},y^{(2)}),…$ ，现在想要求出参数 $\theta$ （是个向量， $\theta = (\theta_0,\theta_1)^T$）。

当我选择这一组参数值 $\theta$ 时，相当于确定了一条直线，如果我的一系列真实数据都很贴合这条直线，那么数据出现的很自然，可能性很大；如果我的一系列真实数据都离这条线很远，那么数据出现的很不自然，可能性很小。每一个 $\theta$ 对应一个数据出现的自然程度（似然值），合起来形成的一条似然曲线就是似然函数 $L(\theta)$ 。

### 3. 什么是“最大似然估计”（MLE）

使似然函数 $L(\theta)$ 最大的那个 $\theta$ ，就是最大似然估计。

## 二、假设前提

现实数据是有一定误差的，一元线性回归中的假设函数 $h_\theta(x) = \theta_0 + \theta_1x$ 与真实值 $y$ 是有一定误差的。将一元线性回归扩展到 $d$ 元，将第一项 $\theta_0$ 变为 $\theta_0x_0$ ，并令 $x_0=1$ ，假设函数变为：

$$
h_\theta(x) = \sum_{i=0}^{d} \theta_i x_i = \theta^T x
$$

由于 $h_\theta(x)$ 和 $y$ 之间会有误差，我们令 $y^{(i)} = \theta^T x^{(i)} + \epsilon^{(i)}$ ，前面的 $\theta^T x^{(i)}$ 是我们可以预测的部分，后面的 $\epsilon^{(i)}$ 是误差值（噪声）。

我们假设：误差的均值为0（不会整体偏高也不会整体偏低），误差服从正态分布（小误差多，大误差少）。

## 三、数学推导

### 1. 写出 $y^{(i)}$ 的条件分布

误差 $\epsilon^{(i)}$ 服从正态分布，那么误差的高斯分布概率密度：

$$p(\epsilon^{(i)}) = \frac{1}{\sqrt{2\pi}\sigma} \exp\left( -\frac{(\epsilon^{(i)})^2}{2\sigma^2} \right)$$

把 $\epsilon^{(i)} = y^{(i)} - \theta^T x^{(i)}$ 代入高斯分布概率密度：

$$p(y^{(i)} \mid x^{(i)}; \theta) = \frac{1}{\sqrt{2\pi}\sigma} \exp\left( -\frac{(y^{(i)} - \theta^T x^{(i)})^2}{2\sigma^2} \right)$$

含义：给定 $x^{(i)}$ 和参数 $\theta$ ，观测 $y^{(i)}$ 的概率密度。

### 2. 构造似然函数

因为样本**独立**，所以联合概率 = 各样本概率密度的乘积：

$$L(\theta) = p(\mathbf{y} \mid X; \theta) = \prod_{i=1}^{n} p(y^{(i)} \mid x^{(i)}; \theta)$$

代入上面的概率密度：

$$L(\theta) = \prod_{i=1}^{n} \left\{ \frac{1}{\sqrt{2\pi}\sigma} \exp\left( -\frac{(y^{(i)} - \theta^T x^{(i)})^2}{2\sigma^2} \right) \right\}$$

### 3. 取似然对数

对数把乘积变求和，且不改变最大值点（log 是单调递增函数）。然后逐项展开，再把常数项提出来：

$$
\begin{aligned} 
\ell(\theta) &= \log L(\theta) \\
&= \log\left( \prod_{i=1}^{n} \left\{ \frac{1}{\sqrt{2\pi}\sigma} \exp\left( -\frac{(y^{(i)} - \theta^T x^{(i)})^2}{2\sigma^2} \right) \right\} \right) \\ 
&= \sum_{i=1}^{n} \log\left( \frac{1}{\sqrt{2\pi}\sigma} \exp\left( -\frac{(y^{(i)} - \theta^T x^{(i)})^2}{2\sigma^2} \right) \right) \\ 
&= \sum_{i=1}^{n} \left[ \log\left( \frac{1}{\sqrt{2\pi}\sigma} \right) - \frac{(y^{(i)} - \theta^T x^{(i)})^2}{2\sigma^2} \right] \\ 
&= n \cdot \log\left( \frac{1}{\sqrt{2\pi}\sigma} \right) - \frac{1}{2\sigma^2} \sum_{i=1}^{n} (y^{(i)} - \theta^T x^{(i)})^2 
\end{aligned}
$$

### 4. 问题转化

$\ell(\theta)$ 中，第一项是与 $\theta$ 无关的常数，不用管他。为了使似然函数最大，就要使第二项最小，也就是要使

$$\sum_{i=1}^{n} \left( y^{(i)} - \theta^T x^{(i)} \right)^2$$

最小，这个式子前面乘一个 $1/2$ 就是最小二乘。

因此，线性回归选择最小二乘作为代价函数。