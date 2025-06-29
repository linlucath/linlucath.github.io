---
title: 高等代数 二 速通
publishDate: 2025-06-29
description: ' 以研究线性变换, 矩阵为主'
tags:
  - Math
heroImage: { src: './thumbnail.jpg', color: '#B4C6DA' }
language: '中文'
---

> 线性变换五花八门的性质

---

## 线性映射

> 线性映射: 线性空间之间保持线性运算的映射

### 线性映射的矩阵表示

给定 $V_1$ 的基 $\{\alpha_1, \ldots, \alpha_n\}$ 和 $V_2$ 的基 $\{\beta_1, \ldots, \beta_m\}$

$$
(\mathcal{A}\alpha_1, \mathcal{A}\alpha_2, \ldots, \mathcal{A}\alpha_n) = (\beta_1, \beta_2, \ldots, \beta_m) \cdot A
$$

即 $\mathcal{A}(\alpha_1, \alpha_2, \ldots, \alpha_n) = (\beta_1, \beta_2, \ldots, \beta_m) \cdot A$  
则将 $A$ 称为线性映射 $\mathcal{A}$ 在基 $\{\alpha_1, \ldots, \alpha_n\}$ 和 $\{\beta_1, \ldots, \beta_m\}$ 下的表示矩阵

### 线性映射的核空间, 像空间

> $$
> \begin{align}
> Im(\mathcal{A}) &= \{\mathcal{A}(\alpha) | \alpha \in V_1\} \\
> Ker(\mathcal{A}) &= \{\alpha \in V_1 | \mathcal{A}(\alpha) = 0\}
> \end{align}
> $$

### 线性同构

> $$
> \text{线性同构} \Leftrightarrow \text{线性映射既是单射又是满射}
> $$

### 线性变换

> 线性变换: 在同一线性空间上的线性映射

## 特征值与特征向量

### 特征值的求解

求解关于 $\lambda$ 方程

$$
\det(\lambda I - A) = 0
$$

### 特征向量的求解

求解关于 $\alpha$ 方程

$$
(\lambda_0 I - A)\alpha = 0
$$

### 特征多项式

> 特征多项式: $\det(\lambda I - A)$

### 相似对角化

记 $A$ 的特征值为 $\lambda_1, \ldots, \lambda_n$，对应的一组线性无关的特征向量为 $\alpha_1, \ldots, \alpha_n$，则令 $P = (\alpha_1, \ldots, \alpha_n)$

$$
AP = P\begin{pmatrix}
\lambda_1 & 0 & \cdots & 0 \\
0 & \lambda_2 & \cdots & 0 \\
\vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & \lambda_n
\end{pmatrix}
$$

## 不变子空间

> $$
> V \text{是} \mathcal{A} \text{的不变子空间} \Leftrightarrow \forall v \in V, \mathcal{A}(v) \in V
> \Leftrightarrow V \text{是} \mathcal{A} -子空间
> $$

## 对偶空间

- 对偶空间
  $$
  \{\phi | \phi: V \to \mathbb{F}, \phi \text{是线性映射}\}
  $$
- 对偶基
  $$
  \{\phi_1, \ldots, \phi_n\} \text{是} V \text{的对偶基} \Leftrightarrow \phi_i(\alpha_j) = \delta_{ij}
  $$
- 二重对偶
  将 $V_*$ 的元素视作一个新了列向量空间, 再应用一次对偶空间的定义

## 最小多项式

- 零化多项式
  $$
  f(\lambda) \text{使得} f(A) = \mathcal{O}
  $$
- 最小多项式  
  次数最小的首一零化多项式, 与特征多项式有着相同的根(重数不一定相同)

## 欧式空间

- 内积: (1) 对称 (2) 线性 (3) 正定 (与自己的内积)

- Gram 矩阵

  $$
  G = \begin{pmatrix}
  \langle \alpha_1, \alpha_1 \rangle &
  \langle \alpha_1, \alpha_2 \rangle & \cdots & \langle \alpha_1, \alpha_n \rangle \\
  \langle \alpha_2, \alpha_1 \rangle & \langle \alpha_2, \alpha_2 \rangle & \cdots & \langle \alpha_2, \alpha_n \rangle \\
  \vdots & \vdots & \ddots & \vdots \\
  \langle \alpha_n, \alpha_1 \rangle & \langle \alpha_n, \alpha_2 \rangle & \cdots & \langle \alpha_n, \alpha_n \rangle
  \end{pmatrix} = (\alpha_1, \alpha_2, \ldots, \alpha_n)^T \cdot (\alpha_1, \alpha_2, \ldots, \alpha_n)
  $$

- 度量矩阵  
   取 $ V $ 的一组基, 其 Gram 矩阵成为度量矩阵

- 利用度量矩阵计算内积

  $$
  \langle \alpha, \beta \rangle = \alpha^T G \beta
  $$

- 欧式空间上的同构: 保持内积的线性同构

## 正交

- 正交矩阵: 由一组正交*标准*基构成的矩阵, $A A^T = I$

- 正交变换: 保持内积的线性变换  
  $\Leftrightarrow$ 保持长度  
  $\Leftrightarrow$ 保持某组一组正交标准基  
  $\Leftrightarrow$ 在某组一组正交标准基下, 其矩阵为正交矩阵

  - 第一类正交变换: $det(A) = 1$
  - 第二类正交变换: $det(A) = -1$

- 正交补
  $$
  V^\perp = \{\alpha \in V | \forall v \in V, \langle \alpha, v \rangle = 0\}
  $$
  - 求解正交补: 将 $V$ 的一组基 $\{\alpha_1, \ldots, \alpha_n\}$ 代入内积定义, 求解线性方程组, 可以得到 $V^\perp$ 的一组基, 将其单位化便是 $V^\perp$ 的标准正交基

## 实对称矩阵的标准形

若 $A$ 是实对称矩阵, $\mathcal{A}$为对应的线性变换, 则

1. A 的特征值都是实数
2. 互异特征值的特征向量正交
3. 存在正交矩阵 $C$, 使得 $C^{-1}AC = C^TAC$ 是对角矩阵
4. 存在 $V$ 的一组正交标准基, 使得 $\mathcal{A}$ 在该基下的矩阵为对角矩阵

- 实对称矩阵的正交对角化步骤
  - 求解特征值
  - 求解特征向量
  - 将特征向量正交单位化
  - 令 $C = (\alpha_{11}, \alpha_{12}, \ldots, \alpha_{1r_1}, \ldots \alpha_{kr_k})$, 则 $C^{-1}AC = C^TAC$ 是对角矩阵

## 二次型

- 二次型: 数域 $\mathbb{F}$ 上的二次齐次多项式函数

- 二次型的矩阵表示:  
   记为 $A$, 则二次型 $f$ 表示为

  $$
  f = x^T A x = \sum_{i=1}^n \sum_{j=1}^n a_{ij} x_i x_j
  $$

- 可逆线性替换
  记可逆矩阵 $C$, 则称 $X = CY$ 为可逆线性变换

- 合同  
   若存在可逆矩阵 $C$, 使得 $C^T A C = B$, 则称 $A$ 和 $B$ 合同
  - 自反性: $A$ 合同于 $A$
  - 对称性: $A$ 合同于 $B$ 当且仅当 $B$ 合同于 $A$
  - 传递性: $A$ 合同于 $B$, $B$ 合同于 $C$ $\Rightarrow$ $A$ 合同于 $C$
  - 相似 $\Rightarrow$ 合同 $\Rightarrow$ 相抵
  - 欧式空间在不同基下的度量矩阵合同

## 双线性函数

## Jordan 标准形
