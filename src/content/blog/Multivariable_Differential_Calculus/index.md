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

## _*定义 / 概念:*_

### 偏导数

- 设 $D \subset \mathbb{R}^2$ 为开集, $z=f(x,y)$,$(x,y) \in D$ 是定义在 $D$ 上的二元函数, $(x_0,y_0) \in D$ 为一定点, 如果存在极限

  $$
    \lim_{\Delta x \to 0} \frac{f(x_0+\Delta x,y_0)-f(x_0,y_0)}{\Delta x},
  $$

  那么就称函数 $f$ 在点 $(x_0,y_0)$ 关于 $x$ **可偏导**, 并称此极限为 $f$ 在点 $(x_0,y_0)$ 关于 $x$ 的**偏导数**, 记为

$$
\frac{\partial z}{\partial x}(x_0,y_0) \text{ / } f_x(x_0,y_0) \text{ / } \frac{\partial f}{\partial x}(x_0,y_0).
$$

> "偏导" 译自 "Partial derivative", 应当理解为 "部分的导数 -> 标准方向的导数"

- 如果函数 $f$ 在 $D$ 中每一点都关于 $x$ 可偏导, 则 $D$ 中每一点 $(x,y)$ 与其相应的 $f$ 关于 $x$ 的偏导数 $f_x(x,y)$ 构成了一种对应关系即二元函数关系, 它称为 $f$ 关于 $x$ 的**偏导函数** (也称为**偏导数**), 记为

$$
\frac{\partial z}{\partial x} \text{ / } f_x(x,y) \text{ / } \frac{\partial f}{\partial x}.
$$

- 如果函数 $f$ 在点 $(x_0, y_0)$ 关于 $x, y$ 均可偏导, 则称函数 $f$ 在点 $(x_0, y_0)$ **可偏导**

> 可偏导 $\nRightarrow$ 连续. 可偏导只决定函数在两个方向的性质, 对其他方向的性质无能为力

### 方向导数

- 设 $D \subset \mathbb{R}^2$ 为开集, $z=f(x,y)$ , $(x,y) \in D$ 是定义在 $D$ 上的二元函数,$(x_0,y_0) \in D$ 为一定点,$v=(\cos \alpha,\sin \alpha)$ 为一个方向. 如果极限
  $$
  \lim_{t \to 0^+} \frac{f(x_0 + t \cos \alpha, y_0 + t \sin \alpha) - f(x_0, y_0)}{t}
  $$
  存在, 则称此极限为函数 $f$ 在点 $(x_0,y_0)$ 的沿方向 $v$ 的**方向导数**, 记为 $\frac{\partial f}{\partial v}(x_0,y_0)$.

> 可偏导 $\iff \frac{\partial f}{\partial e_i}(x_0,y_0) = \frac{\partial f}{\partial (-e_i)}(x_0,y_0)$

### 全微分

- 设 $D \subset \mathbb{R}^2$ 为开集, $z = f(x,y)$, $(x,y) \in D$ 是定义在 $D$ 上的二元函数, $(x_0, y_0) \in D$ 为一定点. 若存在只与点 $(x_0, y_0)$ 有关而与 $\Delta x, \Delta y$ 无关的常数 $A$ 和 $B$, 使得

  $$
  \Delta z = A \Delta x + B \Delta y + o(\sqrt{\Delta x^2 + \Delta y^2})
  $$

  则称函数 $f$ 在点 $(x_0, y_0)$ 处是**可微**的, 并称其线性主要部分 $A \Delta x + B \Delta y$ 为 $f$ 在点 $(x_0, y_0)$ 处的**全微分**, 记为 $dz(x_0, y_0)$ 或 $df(x_0, y_0)$.

- 若（在 $\sqrt{\Delta x^2 + \Delta y^2} \to 0$ 时）将自变量 $x, y$ 的微分 $\Delta x, \Delta y$ 分别记为 $dx, dy$, 那么有全微分形式
  $dz(x_0, y_0) = A dx + B dy$.

> 直观理解: 可微 $\iff$ 存在完美的线性模拟 (切面)

- **全微分公式**:

  $$
  df(x_0, y_0) = \frac{\partial f}{\partial x}(x_0, y_0) dx + \frac{\partial f}{\partial y}(x_0, y_0) dy.
  $$

- 如果函数 $f$ 在开集（或区域）$D$ 上的每一点都是可微的，则称 $f$ 在 $D$ 上**可微**。此时成立

  $$
  dz = \frac{\partial f}{\partial x}(x,y) dx + \frac{\partial f}{\partial y}(x,y) dy.
  $$

### 梯度

- 设 $D \subset \mathbb{R}^2$ 为开集，$(x_0, y_0) \in D$ 为一定点。如果函数 $z = f(x, y)$ 在 $(x_0, y_0)$ 点可偏导，则称向量 $(f_x(x_0, y_0), f_y(x_0, y_0))$ 为 $f$ 在点 $(x_0, y_0)$ 的**梯度**，记为 $\text{grad} f(x_0, y_0)$，即
  $$
  \text{grad} f(x_0, y_0) = f_x(x_0, y_0) \mathbf{i} + f_y(x_0, y_0) \mathbf{j}.
  $$
- 如果 $f$ 在 $(x_0, y_0)$ 点可微，则得到它的另一种表达：
  $$
  \frac{\partial f}{\partial \boldsymbol{v}}(x_0, y_0) = \operatorname{grad} f(x_0, y_0) \cdot \boldsymbol{v} = \|\operatorname{grad} f(x_0, y_0)\| \cos(\operatorname{grad} f, \boldsymbol{v}),
  $$
  其中 $(\operatorname{grad} f, \boldsymbol{v})$ 表示 $\operatorname{grad} f(x_0, y_0)$ 与 $\boldsymbol{v}$ 的夹角.

> "Gradient" 被译作 "梯度", 但其本义为行走, 在这里描述函数值上升最快的行走方式, **是一个向量**, 私以为翻译为"梯度"不太妥当

### 向量值函数的导数

$f:\mathbb{R}^n \to \mathbb{R}^m$

- 若 $f$ 的每一个分量函数 $f_i(x_1, x_2, \cdots, x_n)$ $(i = 1, 2, \cdots, m)$ 都在 $\boldsymbol{x}$ 点可偏导，就称向量值函数 $f$ 在 $\boldsymbol{x}$ 点可导，并称矩阵

  $$
   \left( \frac{\partial f_i}{\partial x_j} \bigg|_{\boldsymbol{x}} \right)_{m \times n}
  =
  \begin{pmatrix}
  \frac{\partial f}{\partial x_1} \bigg|_{\boldsymbol{x}} & \frac{\partial f}{\partial x_2} \bigg|_{\boldsymbol{x}} & \cdots & \frac{\partial f}{\partial x_n} \bigg|_{\boldsymbol{x}}
  \end{pmatrix} \\
  =
  \begin{pmatrix}
  \frac{\partial f_1}{\partial x_1} \bigg|_{\boldsymbol{x}} & \frac{\partial f_1}{\partial x_2} \bigg|_{\boldsymbol{x}} & \cdots & \frac{\partial f_1}{\partial x_n} \bigg|_{\boldsymbol{x}} \\
  \frac{\partial f_2}{\partial x_1} \bigg|_{\boldsymbol{x}} & \frac{\partial f_2}{\partial x_2} \bigg|_{\boldsymbol{x}} & \cdots & \frac{\partial f_2}{\partial x_n} \bigg|_{\boldsymbol{x}} \\
  \vdots & \vdots & \ddots & \vdots \\
  \frac{\partial f_m}{\partial x_1} \bigg|_{\boldsymbol{x}} & \frac{\partial f_m}{\partial x_2} \bigg|_{\boldsymbol{x}} & \cdots & \frac{\partial f_m}{\partial x_n} \bigg|_{\boldsymbol{x}}
  \end{pmatrix}


  $$

  为向量值函数 $f$ 在 $\boldsymbol{x}$ 点的导数或 Jacobi 矩阵，记为 $f'(\boldsymbol{x})$ (或 $Df(\boldsymbol{x}), J_f(\boldsymbol{x})$)。

> 感到这个定义莫名其妙, 难以理解? 听听 [3Blue1Brown 的讲解](https://www.bilibili.com/video/BV1NJ411r7ja/) 吧

> 导数不是数, 梯度不是度

## _*性质 / 定理:*_

### 可微的充分必要条件

- 设函数 $z = f(x, y)$ 在 $(x_0, y_0)$ 点的某个邻域上存在偏导数，并且偏导数在 $(x_0, y_0)$ 点连续，那么 $f$ 在 $(x_0, y_0)$ 点可微。

> $$
> f(x_0 + \Delta x, y_0 + \Delta y) - f(x_0, y_0)
> \\ = f(x_0 + \Delta x, y_0 + \Delta y) - f(x_0, y_0 + \Delta y) + f(x_0, y_0 + \Delta y) - f(x_0, y_0)
> \\ = f_x(x_0 + \theta_1 \Delta x, y_0 + \Delta y) \Delta x + f_y(x_0, y_0 + \theta_2 \Delta y) \Delta y, \quad 0 < \theta_1, \theta_2 < 1,
> $$

### 梯度的性质

- $ \text{grad} (f \cdot g) = g \cdot \text{grad} f + f \cdot \text{grad} g$.
- $ \text{grad} (\frac{f}{g}) = \frac{g \cdot \text{grad} f - f \cdot \text{grad} g}{g^2}$.

> 有没有觉得和求导法则很像? 想一想梯度是如何计算的

### 高阶偏导数

- 如果函数 $z = f(x, y)$ 的两个混合偏导数 $f_{xy}$ 和 $f_{yx}$ 在点 $(x_0, y_0)$ 连续，那么等式 $f_{xy}(x_0, y_0) = f_{yx}(x_0, y_0)$ 成立。

> 在 Geogebra 中绘制 $f(x,y)=\frac{xy(x^2-y^2)}{x^2+y^2}$ 的图像, 观察一下这个结论的几何意义是什么?

```geogebra title="Geogebra"
f(x,y)=((x*y (x^(2)-y^(2)))/(x^(2)+y^(2))) // [!code highlight]
```

### 高阶全微分

- $$
   d^k z = (dx \frac{\partial}{\partial x} + dy \frac{\partial}{\partial y})^k \cdot z
  $$

> 自己推导一下呢

### 向量值函数的导数

- 向量值函数 $f$ 在 $\mathbf{x}^0$ 点可微的充分必要条件是它的坐标分量函数 $f_i(x_1, x_2, \cdots, x_n)$ $(i = 1, 2, \cdots, m)$ 都在 $\mathbf{x}^0$ 点可微. 此时成立微分公式
  $$
  \text{d}\mathbf{y} = f'(\mathbf{x}^0) \text{d}\mathbf{x}.
  $$

## 习题
