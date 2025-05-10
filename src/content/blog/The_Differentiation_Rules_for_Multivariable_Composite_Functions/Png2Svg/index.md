---
title: '多元复合函数求导'
publishDate: 2025-05-10
description: '高维空间中的复合函数求导法则'
tags:
  - Calculus
  - Multivariable
  - Math
heroImage: { src: './thumbnail.jpg', color: '#B4C6DA' }
language: '中文'
---

> 将求导法则推广开

---

## _*性质 / 定理:*_

### 链式法则

$f:\mathbb{R}^2 \to F \quad g:\mathbb{R}^2 \to \mathbb{R}^2$

- 设 $g$ 在 $(u_0, v_0) \in D_g$ 点可导，即 $x=x(u,v)$, $y=y(u,v)$ 在 $(u_0,v_0)$ 点可偏导. 记 $x_0=x(u_0,v_0)$, $y_0=y(u_0,v_0)$，如果 $f$ 在 $(x_0,y_0)$ 点可微，那么
  $$
  \frac{\partial z}{\partial u}(u_0,v_0) = \frac{\partial z}{\partial x}(x_0,y_0) \frac{\partial x}{\partial u}(u_0,v_0) + \frac{\partial z}{\partial y}(x_0,y_0) \frac{\partial y}{\partial u}(u_0,v_0);
  $$
  $$
  \frac{\partial z}{\partial v}(u_0,v_0) = \frac{\partial z}{\partial x}(x_0,y_0) \frac{\partial x}{\partial v}(u_0,v_0) + \frac{\partial z}{\partial y}(x_0,y_0) \frac{\partial y}{\partial v}(u_0,v_0).
  $$

> 平凡的加法原理.
