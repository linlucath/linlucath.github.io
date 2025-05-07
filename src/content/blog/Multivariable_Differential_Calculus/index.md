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

## _*定义:*_

### 偏导数

- 设 $D \subset \mathbb{R}^2$ 为开集，$z=f(x,y)$，$(x,y) \in D$ 是定义在 $D$ 上的二元函数，$(x_0,y_0) \in D$ 为一定点，如果存在极限

  $$
    \lim_{\Delta x \to 0} \frac{f(x_0+\Delta x,y_0)-f(x_0,y_0)}{\Delta x},
  $$

  那么就称函数 $f$ 在点 $(x_0,y_0)$ 关于 $x$ **可偏导**，并称此极限为 $f$ 在点 $(x_0,y_0)$ 关于 $x$ 的**偏导数**，记为

$$
\frac{\partial z}{\partial x}(x_0,y_0) \text{ / } f_x(x_0,y_0) \text{ / } \frac{\partial f}{\partial x}(x_0,y_0).
$$

> "_偏导_" 译自 "_Partial derivative_", 应当理解为 "_部分的导数_ -> _标准方向的导数_"

- 如果函数 $f$ 在 $D$ 中每一点都关于 $x$ 可偏导，则 $D$ 中每一点 $(x,y)$ 与其相应的 $f$ 关于 $x$ 的偏导数 $f_x(x,y)$ 构成了一种对应关系即二元函数关系，它称为 $f$ 关于 $x$ 的**偏导函数**（也称为**偏导数**），记为

$$
\frac{\partial z}{\partial x} \text{ / } f_x(x,y) \text{ / } \frac{\partial f}{\partial x}.
$$

- 如果函数 $f$ 在点 $(x_0, y_0)$ 关于 $x, y$ 均可偏导, 则称函数 $f$ 在点 $(x_0, y_0)$ **可偏导**

> _可偏导_ $\nRightarrow$ _连续_ . 可偏导只决定函数在两个方向的性质, 对其他方向的性质无能为力

### 方向导数

- 设 $D \subset \mathbb{R}^2$ 为开集， $z=f(x,y)$ ， $(x,y) \in D$ 是定义在 $D$ 上的二元函数，$(x_0,y_0) \in D$ 为一定点，$v=(\cos \alpha,\sin \alpha)$ 为一个方向.如果极限
  $$
  \lim_{t \to 0^+} \frac{f(x_0 + t \cos \alpha, y_0 + t \sin \alpha) - f(x_0, y_0)}{t}
  $$
  存在，则称此极限为函数 $f$ 在点 $(x_0,y_0)$ 的沿方向 $v$ 的**方向导数**，记为 $\frac{\partial f}{\partial v}(x_0,y_0)$.

> _可偏导_ $\iff \frac{\partial f}{\partial e_i}(x_0,y_0) = \frac{\partial f}{\partial (-e_i)}(x_0,y_0)$

### 全微分

- 设 $D \subset \mathbb{R}^2$ 为开集， $z = f(x,y)$， $(x,y) \in D$ 是定义在 $D$ 上的二元函数，$(x_0, y_0) \in D$ 为一定点。若存在只与点 $(x_0, y_0)$ 有关而与 $\Delta x, \Delta y$ 无关的常数 $A$ 和 $B$，使得

  $$
  \Delta z = A \Delta x + B \Delta y + o(\sqrt{\Delta x^2 + \Delta y^2})
  $$

  则称函数 $f$ 在点 $(x_0, y_0)$ 处是可微的，并称其线性主要部分 $A \Delta x + B \Delta y$ 为 $f$ 在点 $(x_0, y_0)$ 处的全微分，记为 $dz(x_0, y_0)$ 或 $df(x_0, y_0)$。

- 若（在 $\sqrt{\Delta x^2 + \Delta y^2} \to 0$ 时）将自变量 $x, y$ 的微分 $\Delta x, \Delta y$ 分别记为 $dx, dy$, 那么有全微分形式
  $dz(x_0, y_0) = A dx + B dy$。
