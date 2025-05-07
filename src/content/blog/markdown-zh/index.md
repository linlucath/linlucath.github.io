---
title: 多元函数微分学
publishDate: 2025-05-07
description: '将分析对象扩展至 Euclid 空间'
tags:
  - Calculus
heroImage: { src: './thumbnail.jpg', color: '#B4C6DA' }
language: '中文'
---

> 在现实生活中, 只有单一变元的情况终究少数, 各个变量之间错综复杂才是常态

---

## 偏导数

_*定义:*_
设 $D \subset \mathbb{R}^2$ 为开集，$z=f(x,y)$，$(x,y) \in D$ 是定义在 $D$ 上的二元函数，$(x_0,y_0) \in D$ 为一定点，如果存在极限

$$
\lim_{\Delta x \to 0} \frac{f(x_0+\Delta x,y_0)-f(x_0,y_0)}{\Delta x},
$$

那么就称函数 $f$ 在点 $(x_0,y_0)$ 关于 $x$ 可偏导，并称此极限为 $f$ 在点 $(x_0,y_0)$ 关于 $x$ 的偏导数，记为

$$
\frac{\partial z}{\partial x}(x_0,y_0) \text{ / } f_x(x_0,y_0) \text{ / } \frac{\partial f}{\partial x}(x_0,y_0).
$$

如果函数 $f$ 在 $D$ 中每一点都关于 $x$ 可偏导，则 $D$ 中每一点 $(x,y)$ 与其相应的 $f$ 关于 $x$ 的偏导数 $f_x(x,y)$ 构成了一种对应关系即二元函数关系，它称为 $f$ 关于 $x$ 的偏导函数（也称为偏导数），记为

$$
\frac{\partial z}{\partial x} \text{ / } f_x(x,y) \text{ / } \frac{\partial f}{\partial x}.
$$
