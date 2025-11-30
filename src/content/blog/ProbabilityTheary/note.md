---
title: "Probability theary"
publishDate: 2025-10-30
description: "TODO"
tags:
  - Math
language: "Chinese"
heroImage: { src: "./default.jpg", color: "#D58388" }
---

## 一维随机变量及其分布

- 随机变量: 随机试验可能的结果

- 分布函数: $F(x) = P(X \leq x)$
  $F(x)$ 满足: 不减性, 右连续, $F(-\infty) = 0, F(\infty) = 1$

- 密度函数: $f(x) = F'(x) = P(X = x) $

- 离散随机变量的分布
  - 二项分布 $B(n, p)$ = $C_n^k p^k (1-p)^{n-k}$
  - 泊松分布 $P(\lambda)$ = $\frac{\lambda^k e^{-\lambda}}{k!}$
  - 超几何分布 $H(N, M, n)$ = $\frac{C_M^k C_{N-M}^{n-k}}{C_N^n}$s
  - 几何分布 $G(p)$ = $p(1-p)^{k-1}$
- 连续随机变量的分布

  - 均匀分布 $U(a, b)$ = $\frac{1}{b-a}$
  - 指数分布 $E(\lambda)$ = $\lambda e^{-\lambda x}$
  - 正态分布 $N(\mu, \sigma^2)$ = $\frac{1}{\sqrt{2\pi}\sigma} e^{-\frac{(x-\mu)^2}{2\sigma^2}}$
    > $\int^{\infty}_{-\infty}e^{-\frac{x^2}{A}}dx = \sqrt{\pi A} $
  - 标准正态分布 $N(0, 1)$. 通过对任意正态分布 $N(\mu, \sigma^2)$ 进行标准化令 $Z = \frac{X - \mu}{\sigma}$ 可转化为标准正态分布.

- 求解随机变量函数的密度函数
  1. 寻找新的自变量 $y$ 的分段点
  2. 根据分段点分区间求解 $F_{Y}(y)$
  3. 对 $F_{Y}(y)$ 求导得到 $f_{Y}(y)$

## 二维随机变量及其分布

- 边缘分布: $f_X(x) = \int_{-\infty}^{\infty} f(x, y) dy$, $f_Y(y) = \int_{-\infty}^{\infty} f(x, y) dx$

- 求解随机变量函数的密度函数
  1. 寻找新的自变量 $z$ 的分段点
  2. 根据分段点分区间求解 $F_{Z}(z)$
  3. 对 $F_{Z}(z)$ 求导得到 $f_{Z}(z)$