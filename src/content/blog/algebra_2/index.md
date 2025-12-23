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
  - 重数的计算: 使对应的核空间维数不再增加的最小次数

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
  取 $ V $ 的一组基, 其 Gram 矩阵为度量矩阵

  - 度量矩阵正定

- 利用度量矩阵计算内积

  $$
  \langle \alpha, \beta \rangle = \alpha^T G \beta
  $$

- 欧式空间上的同构: 保持内积的线性同构

## 正交

- 正交矩阵: 由一组正交*标准*基构成的矩阵, $A A^T = I$

### 正交变换: 保持内积的线性变换

$\Leftrightarrow$ 保持长度  
 $\Leftrightarrow$ 保持某组一组正交标准基  
 $\Leftrightarrow$ 在某组一组正交标准基下, 其矩阵为正交矩阵

- 第一类正交变换: $det(A) = 1$
- 第二类正交变换: $det(A) = -1$

### 正交补

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

### 使用正交矩阵对实对称矩阵进行对角化

- 求解特征值
- 求解特征向量
- 将特征向量正交单位化
- 令 $C = (\alpha_{11}, \alpha_{12}, \ldots, \alpha_{1r_1}, \ldots \alpha_{kr_k})$, 则 $C^{-1}AC = C^TAC$ 是对角矩阵

### 实对称矩阵的正交对角化方法

- 将待变换矩阵 \(A\) 写在一条横线上方，单位阵 \(I\) 写在横线下方。
- 横线下方矩阵作列变换，上方矩阵作相同列变换，再作对应行变换（下方矩阵2,3列互换，则上方矩阵2,3列互换，再2,3行互换）。
- 设横线上方矩阵 \(A\) 变成对角阵 \(B\) 时，横线下方的 \(I\) 变成矩阵 \(C\)，
- 一定有 \(C^TAC = B\)。

## 二次型

- 二次型: 数域 $\mathbb{F}$ 上的二次齐次多项式函数

- 二次型的矩阵表示:  
   将对应的对称表示矩阵记为 $A$, 则二次型 $f$ 表示为

  $$
  f = x^T A x = \sum_{i=1}^n \sum_{j=1}^n a_{ij} x_i x_j
  $$

- 可逆线性替换: 记可逆矩阵 $C$, 则称 $X = CY$ 为可逆线性变换

- 标准形: 只含平方项的二次型

  - 将二次型化为标准型的方式: 若有平方项, 进行配方, 否则先进行一次中值换元, 再进行配方

### 二次型规范形:

- 实二次型规范形: 平方项系数为 $1$ 的标准形
- 复二次型规范形: 平方项系数为 $\pm 1$ 的标准形

### 合同

若存在可逆矩阵 $C$, 使得 $C^T A C = B$, 则称 $A$ 和 $B$ 合同

- 自反性: $A$ 合同于 $A$
- 对称性: $A$ 合同于 $B$ 当且仅当 $B$ 合同于 $A$
- 传递性: $A$ 合同于 $B$, $B$ 合同于 $C$ $\Rightarrow$ $A$ 合同于 $C$
- 相似 $\Rightarrow$ 合同 $\Rightarrow$ 相抵
- 欧式空间在不同基下的度量矩阵合同
- 合同 $\Leftrightarrow$ 有相同的正特征值个数, 负特征值个数, 零特征值个数

- 正定二次型: 值恒大于 $0$ 的二次型

### 正定矩阵: 正定二次型的矩阵表示

- 对于正定矩阵 $A$, 存在唯一的实对称矩阵 $B$, 使得 $B^2 = A$

- 正定矩阵的乘积不一定是正定矩阵, 因为可能不再对称

- 正定 $\Leftrightarrow$ 与 $I$ 合同 $\Leftrightarrow$ 存在正交矩阵 $C$, 使得 $C^T A C = D$, 其中 $D$ 是对角矩阵, 且对角线上的元素均为正数 $\Leftrightarrow$ 顺序主子式均大于 $0$

- 可逆实对称矩阵 $\Leftrightarrow$ 存在实矩阵 $C$, 使得 $AC + C^TA $ 为正定矩阵

> 可逆, 秩相关条件考虑等价为方程的解

### 双线性函数: 固定任一个变量时都是另一个变量的线性函数

- 左线性映射: 固定右变量时是左变量的线性映射
- 右线性映射: 固定左变量时是右变量的线性映射
- 非退化 $\Leftrightarrow$ 其在某组基下的度量矩阵表示可逆
- 对称双线性函数在某组基下的度量矩阵表示为对角阵
- 令 $A=\begin{pmatrix} 0 & 1 \\ -1 & 0 \end{pmatrix}$，反对称双线性函数在某组基下的度量矩阵表示为
  $$
  \begin{pmatrix}
  A & 0 & & \\
  0 & A & & \\
  & & \ddots & \\
  & & & 0
  \end{pmatrix}
  $$

## $\lambda$ 矩阵

- 半单: 可对角化的复方阵

- 幂零指数为 $m$ $\rightarrow$ $A$ 的最小多项式为 $\lambda^m$

### $\lambda$ 矩阵

- $\lambda$ 矩阵: 所有元素为关于 $\lambda$ 的多项式的矩阵

### $\lambda$ 矩阵的相抵标准形

> 任一 $A(\lambda)$ 必可经 $\lambda$-初等变换相抵于如下形状的矩阵：
>
> $$
> D(\lambda)=\begin{pmatrix}
> d_1(\lambda) & & & & \\
> & d_2(\lambda) & & & O \\
> & & \ddots & & \\
> & & & d_r(\lambda) & \\
> & O & & & O
> \end{pmatrix}
> $$
>
> 其中 $d_i(\lambda)$ 都是首 1 多项式，且 $d_i(\lambda) \mid d_j(\lambda)$ 对所有的 $1 \leq i \leq j \leq r, r = R(A(\lambda))$ > $D(\lambda)$ 称为 $\lambda$-矩阵 $A(\lambda)$ 的**相抵标准形**。

- 矩阵 $A$, $B$ 相似 $\Leftrightarrow$ $\lambda I - A$ 和 $\lambda I - B$ 相抵

### $Frobenius$ 块

设 $A$ 是有限维线性空间 $V$ 上的线性变换，$V$ 上非零向量 $\alpha$ 生成一个循环不变 $A$-子空间 $I(\alpha)$，设 $A|_{I(\alpha)}$ 的最小多项式为 $f(\lambda) = \lambda^r + a_{r-1}\lambda^{r-1} + \cdots + a_1\lambda + a_0$

则：

1. $\alpha, A\alpha, \cdots, A^{r-1}\alpha$ 是 $I(\alpha)$ 的一组基；
2. $A|_{I(\alpha)}$ 在基 $\alpha, A\alpha, \cdots, A^{r-1}\alpha$ 下的矩阵为
   $$
   A = \begin{pmatrix}
   0 & 0 & \cdots & 0 & -a_0 \\
   1 & 0 & \cdots & 0 & -a_1 \\
   0 & 1 & \cdots & 0 & -a_2 \\
   \vdots & \vdots & \ddots & \vdots & \vdots \\
   0 & 0 & \cdots & 1 & -a_{r-1}
   \end{pmatrix}
   $$

$A$ 的 $1, 2, \cdots, r-1$ 阶行列式因子为 1。

$A|_{I(\alpha)}$ 的最小多项式 = $A|_{I(\alpha)}$ 的特征多项式 = $A$ 的 $r$ 阶行列式因子

$A$ 的最小多项式 = $A$ 的特征多项式 = $A$ 的 $r$ 阶行列式因子 = $f(\lambda)$

### $Frobenius$ 标准形 / 有理标准形

**定理2.** 设 $A \in F^{n \times n}$ 的不变因子组为 $1, \cdots, 1, d_1(\lambda), \cdots, d_k(\lambda)$，其中

$$d_i(\lambda) = \lambda^{r_i} + c_{i,r_i-1}\lambda^{r_i-1} + \cdots + c_{i,1}\lambda + c_0 \text{ 是 } r_i \text{ 次多项式}$$
则

$A$ 相似于分块对角阵 $F = \begin{pmatrix}
F_1 & & \\
& \ddots & \\
& & F_k
\end{pmatrix}$, 其中 $F_i = \begin{pmatrix}
0 & 0 & \cdots & 0 & -c_{i,0} \\
1 & 0 & \cdots & 0 & -c_{i,1} \\
0 & 1 & \cdots & 0 & -c_{i,2} \\
\vdots & \vdots & \ddots & \vdots & \vdots \\
0 & 0 & \cdots & 1 & -c_{i,r_i-1}
\end{pmatrix}$.  
这样的 $F$ 称为 $A$ 的 $Frobenius$ 标准形 / 有理标准形。

### 初等因子

设 $A$ 的相抵标准形为 $\text{diag}(1, \cdots 1, d_1(\lambda), \cdots, d_r(\lambda))$

将 $A$ 的不变因子均分解为首 $1$ 不可约多项式之乘积：

$$d_1(\lambda) = (\lambda - \lambda_1)^{e_{11}} (\lambda - \lambda_2)^{e_{12}} \cdots (\lambda - \lambda_t)^{e_{1t}}$$

$$d_2(\lambda) = (\lambda - \lambda_1)^{e_{21}} (\lambda - \lambda_2)^{e_{22}} \cdots (\lambda - \lambda_t)^{e_{2t}}$$

$$\cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots \cdots$$

$$d_r(\lambda) = (\lambda - \lambda_1)^{e_{r1}} (\lambda - \lambda_2)^{e_{r2}} \cdots (\lambda - \lambda_t)^{e_{rt}}$$

$(\lambda - \lambda_j)^{e_{ij}} (e_{ij} > 0)$ 称为 $A$ 的初等因子

- **$A$ 的初等因子组**：$A$ 的全体初等因子所成集合

- $A$ 与 $B$ 在 $F$ 上相似 $\Leftrightarrow$ $A$ 与 $B$ 有相同的初等因子组。

### $Jordan$ 标准形

- $r$ 阶 $Jordan$ 块 $J_{a,r}$ 的初等因子组为 $(\lambda - a)^r$。

### 已知不变因子组求解标准型

**例1.** 已知 $A$ 的不变因子组为 $1, 1, 1, 1, \lambda - 1, (\lambda - 1)^2, (\lambda - 1)^2 (\lambda - 2)^2$，

求 $A$ 的 $Jordan$ 标准形。

**解：** $A$ 的初等因子组为：$\lambda - 1, (\lambda - 1)^2, (\lambda - 1)^2, (\lambda - 2)^2$

因此其 $Jordan$ 标准形为：

$$
\begin{pmatrix}
1 &  &  &  &  &  &  \\
 & 1 & 1 &  &  &  &  \\
 &  & 1 &  &  &  &  \\
 &  &  & 1 & 1 &  &  \\
 &  &  &  & 1 &  &  \\
 &  &  &  &  & 2 & 1 \\
 &  &  &  &  &  & 2
\end{pmatrix}
$$

### 使用可逆矩阵将矩阵化为 $Jordan$ 标准形

1. 求解特征值
2. 求解最小多项式, 画出 $Jordan$ 标准形
3. 对于每个特征值, 求解对应的特征向量
4. 获得一个对应的特征向量链, 以下方三个式子为例
   $$
   (A - 4I)v_3 = v_2
   $$
   $$
   (A - 4I)v_2 = v_1
   $$
   $$
   (A - 4I)v_1 = 0
   $$
5. 排列各个特征值对应的特征向量链, 得到所求可逆矩阵
