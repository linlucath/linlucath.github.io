---
title: 代数学引论 笔记
publishDate: 2025-05-07
description: 'TODO'
tags:
  - TODO
heroImage: { src: './thumbnail.jpg', color: '#B4C6DA' }
language: '中文'
---

# 代数的起源

## 线性方程组初步

目标是解形如

$$
a_{11}x_1+\cdots+a_{1n}x_n = b_1\\
\vdots\\
a_{m1}x_1+\cdots+a_{mn}= b_m
$$

的方程

通过初等变换，可以把它变成阶梯型，从而化简方程组

可以证明，所有的线性方程组都等价于一个阶梯型方程组

![image-20250330153111571](C:\Users\15159\AppData\Roaming\Typora\typora-user-images\image-20250330153111571.png)

由定义，每一个方程首个不为零的元素的行数是大于等于列数的

若方程组有解我们称它们为 **相容的**，反之称其为 **不相容的**

若有唯一的解，我们称之为 **确定的**

现在来研究是否相容的问题

显然一个形如 $0=\bar b_t$ 在 $\bar b_t\not=0$ 时是无论如何都不成立的，可以证明 **当不包含这种方程时，方程组就是相容的**

证明的思路是，让上式中所有 $t>r，\bar b_t=0$，然后将每个方程开头的未知量视为主未知量，其余量视为自由变量，随后取定任意自由变量的值（我们只要求有解，于是这样的操作是合理的），从第 $r$ 个方程开始逐个解方程，就得到了结果

接下来研究 **是否确定** 的问题

我们可以看到，如果存在自由变量，那么它们显然无法被约束，此时方程组不可能是确定的，有如下论断

**一个相容的线性方程组是确定的，当且仅当由它得到的阶梯型方程组满足 $r=n$**

于是可以知道如下推论

**线性方程组当 m = n 时是相容且确定的充要条件是将它化为阶梯形后，所得的线性方程组满足 $\Pi \bar a_{ii}\not=0$**

这是因为，一个方程组是否有解不依赖于等号右边的取值，于是一个线性方程组有解等价于与之对应的齐次方程组有解，而一个齐次方程组永远相容，且它至少有一个零解，由行阶梯形和条件 $\Pi a_{ii}\not=0，$，可以保证该方程只有零解

下面推论是等价于这一条推论的

**线性方程组在 $m=n$ 的情况下是相容且确定的充要条件是对应的齐次线性方程组只有零解**

**当 $n>m$ 时，相容的方程组是不定的，且齐次方程组此时有非零解**

<mark> 齐次方程的不定性等价于非零解的存在性 </mark>

上述方法称为高斯消元法，我们来估计计算的次数，由于乘法运算比加法运算困难，只记录乘法运算的次数

为了消去 $x_1$，要先将这一行乘上一个系数，这就要计算 $n$ 次乘法，由于除去这一行，还有 $n-1$ 行，所以一共需要 $n(n-1)$ 次计算，然后继续计算下一行，就可以归纳地得到总共计算次数为

$$
n(n-1)+(n-1)(n-2)+\cdots+1(1-1)=\frac{n^3-n}{3}
$$

当 $n$ 非常大时，上式近似于 $\frac{n^3}{3}$

## 集合与映射

### 笛卡尔积

设 $X,Y$ 是任意两个集合，任取 $x\in X,y\in Y$，称给定顺序的元素对 $(x,y)$ 为一个 **有序对**，全体有序对的集合 $X\times Y$ 称为 **笛卡尔积**

若 $X=Y=\mathbb{R}$, 这时笛卡尔平方 $\mathbb{R^2}=\mathbb{R}\times \mathbb{R}$ 是给定坐标系平面上点的集合，我们可以归纳地得出 $\mathbb{R}^n$

### 基数

基数即元素的个数

当只有有限个元素时

$$
|X|= CardX = n\quad|Y|= CardY = m\\
\rightarrow|X\times Y|= nm
$$

### 映射

记映射 $f$ 的像为 $Imf$

集合 $f^{-1}(y)=\{x\in X|f(x)=y\}$ 叫做 $y\in Y$ 的 **原像**

将每一个元素映射到自身的映射 $e_x$: $X\rightarrow X$ 叫做 **单位映射**

**若 $X$ 是有限集，且 $f:X\rightarrow X$ 是单射（满射），则 $f$ 是双射**

证明的思路为：若 $f$ 为单射，则必有 $CardX=CardImf$，又由于 $Imf\subset X$ 且它们是有限集，于是 $Imf=X$

若 $f$ 为满射，则采用反证法，得出 $CardImf<CardX$​, 这与满射矛盾

## 等价关系与商映射

### 二元关系

设 $X,Y$ 是两个集合，任意子集 $\omega\subset X\times Y$ 叫做 $X$ 与 $Y$ 之间的二元关系，例如 $\mathbb{R}$ 上的 $<$ 就是一个二元关系

### 等价关系

与给定元素 $x$ 等价的所有元素的子集

$$
\bar x =\{x'\in X|x'\sim x\}\subset X
$$

叫做包含 $x$ 的等价类，显然 $x\sim \bar x$, $\bar x$ 中的元素都叫做 $\bar x$ 中的代表元

**由关系 $\sim$ 确定的等价类的集合是集合 $X$ 的一个划分，即 $X$ 是这些子集的不交并**

若它们相交，则它们是同一个集合

**若 $\pi(X)$ 是将原集合分成不相交子集 $C_x$ 的一个划分，则 $C_x$ 是由某一等价关系确定的等价类**

### 商映射

等价关系与划分之间有一一对应的关系，对应于等价关系 $\sim$ 的划分记作 **$X/\sim$** 称之为 $X$ 的 **商集**

$$
p: x\rightarrow p(x)=\bar x
$$

称为 $X\rightarrow X/\sim$​上的商映射

## 置换

### 置换的标准记法

由 $\Omega\rightarrow\Omega$ 的全体一一变换组成的集合记作 $S_n=S(\Omega)$，其元素用小写希腊字母表示，称为置换

任意置换 $\pi:i\rightarrow\pi(i),i=1,2,\cdots,n$，可以直观表述为以下形式：

$$
\pi =\begin{pmatrix}1&2&3&\cdots&n\\
i_1&i_2&i_3&\cdots&i_n\end{pmatrix}
$$

它完全指明了所有的像

容易验证，它的复合（乘法）不是交换的，例如

$$
\sigma =
\begin{pmatrix}
1 & 2 & 3 & 4 \\
2 & 3 & 4 & 1
\end{pmatrix}, \quad
\tau =
\begin{pmatrix}
1 & 2 & 3 & 4 \\
4 & 3 & 2 & 1
\end{pmatrix}
\\
\sigma \tau =
\begin{pmatrix}
1 & 2 & 3 & 4 \\
2 & 3 & 4 & 1
\end{pmatrix}
\begin{pmatrix}
1 & 2 & 3 & 4 \\
4 & 3 & 2 & 1
\end{pmatrix}
=
\begin{pmatrix}
1 & 2 & 3 & 4 \\
4 & 3 & 2 & 1
\end{pmatrix}
=
\begin{pmatrix}
1 & 2 & 3 & 4 \\
1 & 4 & 3 & 2
\end{pmatrix}
\\
\tau \sigma =
\begin{pmatrix}
1 & 2 & 3 & 4 \\
4 & 3 & 2 & 1
\end{pmatrix}
\begin{pmatrix}
1 & 2 & 3 & 4 \\
2 & 3 & 4 & 1
\end{pmatrix}
=
\begin{pmatrix}
1 & 2 & 3 & 4 \\
3 & 2 & 1 & 4
\end{pmatrix}
$$

置换满足以下规律：

1.乘法是结合的

2.$S_n$ 有单位元 $e$, 满足 $\pi e=\pi=e\pi$

3.每一个元素 $\pi$，都有逆元 $\pi^{-1}:\pi\pi^{-1}=e=\pi^{-1}\pi$

这是因为 $\pi$ 是一个双射

由排列组合的知识，显然 $|S_n|=CardS_n=n!$

### 置换的循环结构

尝试将 $S_n$ 的置换分解为更简单的置换

![image-20250331153734295](C:\Users\15159\AppData\Roaming\Typora\typora-user-images\image-20250331153734295.png)

仔细观察 $\sigma$ 的结构，可以看出只要置换两次（做乘法），无论如何 总是有 $1\rightarrow3，3\rightarrow1$ 和 $2\rightarrow4,4\rightarrow2$

于是有 $\sigma^2=(1\quad3)(2\quad4)$

同理有 $\sigma^4=e,\tau^2=e$​

对于任何映射，归纳地定义其次幂

$$
\pi^s =
\begin{cases}
\pi(\pi^{s-1}), & \text{若 } s > 0, \\
e, & \text{若 } s = 0, \\
\pi^{-1}((\pi^{-1})^{-s-1}), & \text{若 } s < 0.
\end{cases}
$$

可以得到 $\pi^s\pi^t=\pi^{s+t}=\pi^t\pi^s$

由于 $|\Omega|<\infty$，所以对于每一个 $\pi\in S_n$，可以找到唯一的自然数 $q$，使得所有的次幂包含在 $\{e,\pi,\pi^2,\cdots,\pi^{q-1}\}$ 中，且 $\pi^q=e$ ，这个 $q$ 就称为置换的阶

例如，对于上面的例子，$\sigma$ 的阶为 4，$\tau$ 的阶为 2

我们之所以可以这样断言，是因为经历足够多次的乘法，我们最终会得到一个自然排列

<mark> 这让我想起高一月考结束的那个雨夜，我突然被通知到报告厅中听贵州大学章超的报告，他向我们陈述了这个事实，并举了一个生动的例子，“如果你有一个喜欢的女生，她和你的位置离得很近，但是每个星期都要更换位置，纵然如此，经历足够多的换位置，你们还是能坐到一起。”而现在，我终于有机会严格地证明这个结论了 </mark>

证明：由于 $S_n$ 是一个有 $n!$ 个元素的有限集合，于是对于任意 $\pi\in S_n$，对它求任意次的复合（乘法）

记为

$$
\Pi =\{e,\pi,\pi^2,\cdots\}
$$

这样不断求下去，直到 $Card\Pi>n!$，这样根据抽屉原理，就存在 $i,j<n!$, 使得 $\pi^i=\pi^j$

由于 $\pi^i$ 也是一个置换，于是存在一个逆映射 $\pi^{-i}$，将它乘到上式两边，就有

$$
e =\pi^{j-i}
$$

这样就证明了结论

定义一个等价关系，称 $i,j\in\Omega$ 为 $\pi$ 等价的，如果存在 $z$ 是整数，使得 $j=\pi^s(i)$，可以验证它的自反性、对称性、传递性，于是这确是一个等价关系

上面已经知道，等价关系和划分的一一对应的关系，于是有

$$
\Omega =\Omega_1\bigcup\Omega_2\bigcup\cdots\bigcup\Omega_p
$$

它们都是两两不相交的，这些类称为 **$\pi$ 轨道**，某一 $\pi$ 轨道完全由一个 $i\in\Omega$ 和它的像组成

例如上文中的 $\sigma$ 和 $\tau$ 的每一个“循环”都是一个 $\pi$​轨道

令 $l_k=|\Omega|$ 是 $\pi$ 轨道的长度

易见两个置换 $\pi_k,\pi_s$ 是不相交的，于是可以得到 $\pi$ 的分解

$$
\pi =\pi_1\pi_2\cdots\pi_p
$$

其中任意两个循环是可换序的

如果某一置换的长度为 $1$​，那么它实际上是恒等置换，这种情况下可以在乘积中省略

例如

$$
\pi = \begin{pmatrix} 1 & 2 & 3 & 4 & 5 & 6 & 7 & 8 \\ 2 & 3 & 4 & 5 & 1 & 7 & 6 & 8 \end{pmatrix} \in S_8
$$

可以写成

$$
\pi = (1 \ 2 \ 3 \ 4 \ 5)(6 \ 7)(8) = (1 \ 2 \ 3 \ 4 \ 5)(6 \ 7).
$$

对任意 $n\ge7$ 可以看作 $S_n$ 的置换

假设存在另一种 $\pi$​的分解

$$
\pi =\alpha_1\alpha_2\cdots\ \alpha_r
$$

且其中没有恒等变换，存在唯一的一个 $\pi_s(i)$ 和 $\alpha_t(i)$ 使得 $\pi_s(i)=\pi(i)=\alpha_t(i)$

下面运用数学归纳法可以证明，等式

$$
\pi_s^k(i)=\pi^k(i)=\alpha_t^k(i)
$$

对 $k\ge1$ 都成立

再继续对 $m$ 或 $r$ 用归纳法，我们可以证明下述命题成立

**$S_n$ 中的每一个置换 $\pi\not=e$ 都是长度 $\ge 2$，不相交的循环的乘积，这一分解精确到循环的顺序是唯一确定的**

称长度为 2 的循环为 **对换**

可以把循环写成对换的乘积，只要把第一个元素与其余元素一一匹配即可

$$
(1\quad2\quad\cdots\quad l-1\quad l)=(1\quad l)(1\quad l-1)\cdots(1\quad3)(1\quad2)
$$

这样做行之有效的原因是，乘积（复合）是从右往左依次进行的，这意味着它们是不可交换的

然而，可以很容易证明，这种写成乘积的形式并不是唯一的，写成链式也是可行的，将第 $k$ 个元素与其余元素一一匹配也是可行的

### 置换的符号

设 $\pi$ 是 $S_n$ 中的一个置换，将 $\pi$ 分解成对换的乘积

$$
\pi =\tau_1\tau_2\cdots\tau_k
$$

称

$$
\epsilon_{\pi}=(-1)^k
$$

为 $\pi$ 的符号（亦称符号差或奇偶性），它由置换 $\pi$ 唯一确定并不依赖于分解，任取 $\alpha,\beta\in S_n$，有

$$
\epsilon_{\alpha\beta}=\epsilon_\alpha\epsilon_\beta
$$

该定理可以用逆序数简单地证明，若将一个置换分解为不同的对换，每次对换都会改变逆序数的奇偶性，为了达到相同的置换，最终所达到的状态必然经历奇数个或者偶数个对换，而不可能混用

<mark> 这揭示了对换和逆序数的对应关系 </mark>

若 $\epsilon_\pi=1$，则称置换 $\pi\in S_n$ 为偶置换，若 $\epsilon_\pi=-1$，则称 $\pi$ 为奇置换

有如下推论

设置换 $\pi\in S_n$ 分解为长为 $l_1,l_2,\cdots,l_m$ 的互不相交的循环的乘积，则

$$
\epsilon_\pi =(-1)^{\Sigma_{k = 1}^m(l_k-1)}
$$

因为

$$
\epsilon_\pi =\epsilon_{\pi_1}\epsilon_{\pi_2}\cdots\epsilon_{\pi_m}=(-1)^{l_1-1}\cdots(-1)^{l_m-1}=(-1)^{\Sigma_{k = 1}^m(l_k-1)}
$$

### $S_n$ 在函数上的作用

设 $\pi\in S_n$ 是 $n$ 个自变量的函数，令

$$
(\pi\circ f)(x_1, x_2,\cdots, x_n)= f(x_{\pi(1),},\cdots, x_{\pi(n)})
$$

则称函数 $g=\pi\circ f$ 是由 $\pi $ 作用在 $f$ 上得到的

以下引理成立：

设 $\alpha,\beta$ 是 $S_n$ 的任意置换，则

$$
(\alpha\beta)\circ f =\alpha\circ(\beta\circ f)
$$

定义一个 $n$ 元函数 $f$ 叫做 **斜对称的**，若 $f(\cdots,x_k,x_{k+1}\cdots)=-f(\cdots,x_{k+1},x_k,\cdots)$

可以证明 $f$ 并非恒为零的，例如范德蒙行列式

引入这样一类函数，可以得到 **4.3** 中定理的另一种证明：

$$
\pi\circ f =(\tau_1\cdots\tau_{k-1})\circ(\tau_k\circ f)=-(\tau_1\cdots\tau_{k-1})\circ f =\cdots =(-1)^kf =\epsilon_\pi f
$$

可以看出 $LHS$ 依赖于 $\pi$，但是不依赖于 $\pi$ 的分解，于是 $\epsilon_\pi$ 完全由 $\pi$ 确定

## 整数的算术

### 算术基本定理

若存在某个 $t \in \mathbb{Z}$，使得 $n = st$，则整数 $s$ 叫作整数 $n$ 的 **因数**（或 **因子**），$n$ 本身叫作 $s$ 的 **倍数**。$n$ 被 $s$ 整除记作 $s \mid n$，而 $n$ 不能被 $s$ 整除记作 $s \nmid n$。整除性是 $\mathbb{Z}$ 上的传递关系。如果 $m \mid n$ 且 $n \mid m$，则 $n = \pm m$，而整数 $n,m$ 叫作 **相伴** 的。

如果整数 $p$ 的全部因子为 $\pm p, \pm 1$（非真因子），则称 $p$ 为 **素数**。通常约定素数是正的且大于 $1$。

每一个正整数 $n\not= 1$ 都可以分解成素数的乘积 $n=p_1p_2\cdots p_s$，这一写法除了交换律外是唯一的

<mark> 这指明了素数的真正基本元素地位 </mark>

进一步地，若有相同因子，把它们收集到一起

$$
n = p_1^{\epsilon_1}p_2^{\epsilon_2}\cdots p_k^{\epsilon_k}
$$

有理数也可以做类似的分解

**(欧几里得定理）全体素数集 $P$​是一个无穷集**

反证法，设 $P$ 是一个有限集 $\{p_1,p_2,\cdots,p_k\}$, 那么 $c=p_1p_2\cdots p_k+1$ 可以被某个 $p_i$ 整除，设 $c=p_1c',$ 则 $p_1(c'-p_2\cdots p_k)$，而 1 的因子只有 1，这就产生了矛盾

下面的例子说明该定理的非平凡性

$$
S =\{4k+1|k = 0,1,2,\cdots\}
$$

容易验证 $S$ 对乘法封闭，可以验证 $S$ 中所有的元素的分解有存在性，我们取 $n=q_1q_2\cdots q_n$

其中 $q_i$ 不能用 $S$ 中的元素作进一步分解，称之为 **拟素数**

然而可以知道，$S$ 中的元素对拟素数的分解不是唯一的，如 $441=9\times49=21^2$

### 最大公约数和最小公倍数

约定 $p_i^0=1$, 对任意的整数 $m、n$ 有

$$
n =\pm p_1^{\alpha_1}p_2^{\alpha_2}\cdots p_k^{\alpha_k}\quad m =\pm p_1^{\beta_1}p_2^{\beta_2}\cdots p_k^{\beta_k}
$$

引入两个整数

$$
g.c.d(n, m)= p_1^{\gamma_1}p_2^{\gamma_2}\cdots p_k^{\gamma_k}\\
l.c.m(n, m)= p_1^{\delta_1}p_2^{\delta_2}\cdots p_k^{\delta_k}
$$

其中 $\gamma_i=min\{\alpha_i,\beta_i\},\delta_i=max\{\alpha_i,\beta_i\}$

可以得出以下论断：

1.$g.c.d(n,m)|n,g.c.d(n,m)|m$，并且若 $d|m,d|n$，则 $d|g.c.d(n,m)$（整除的传递性）

2.$n|l.c.m(n,m),m|l.c.m(n,m)$，并且若 $n|d,m|d$，则 $l.c.m(n,m)|d$

称 $g.c.d$ 为 **最大公因数**，$l.c.m$ 为 **最小公倍数**

它们满足 $g.c.d(n,m)l.c.m(n,m)=mn$

若 $g.c.d(n,m)=1$，则称整数 $n,m$ 为互素的，这时 $l.c.m(n,m)=mn$

### 带余除法

给定 $a,b\in \mathbb{Z},b>0$，总可以找到 $q,r\in\mathbb{Z}$，使得

$$
a = bq+r,\quad0\le r < b
$$

证明的想法是找出 $S$ 中的一个最小元素 $r=a-bq$，并通过反证法说明 $0<r<b$

这说明我们可以通过有限步找出商数 $q$ 和余数 $r$, 用这种方法给出 $g.c.d$ 的另一种定义

给定不全为零的 $m,n$

$$
J = \{nu + mv | u, v \in \mathbb{Z}\} \tag{3}
$$

现在选取 $J$ 中的最小正数 $d = nu_0 + mv_0$，运用带余除法将 $n$ 写作 $n = dq + r, 0 \le r < d$. 可以断言，这个 $r$ 必然为 0，否则 $d$ 将不再是 $J$ 中的最小正数，于是 $d|n$，同理 $d|m$，假设 $d'$ 是数 $n$ 和 $m$ 的任意一个公因子。这时

$$
d'|n, d'|m \Rightarrow d'|nu_0, d'|mv_0 \Rightarrow d'|(nu_0 + mv_0) \Rightarrow d'|d.
$$

这样，$d$ 就具备了所有最大公因数的性质，于是 $d=g.c.d(m,n)$，我们证明了以下论断

$$
\text{g.c.d}(n, m) = nu + mv, \quad u, v \in \mathbb{Z}
$$

当 $m,n$ 互素，当且仅当存在 $u,v\in\mathbb{Z}$，使得 $nv+mv=1$

这时因为最小的正整数为 1，如果上式能取到 1，那么必有 $g.c.d(m,n)=1$，这恰好就是互素的定义

# 矩阵

## 行与列的向量空间

### 基本定义

对于 $n\in\mathbb{N}$，$\mathbb{R}$ 上长为 $n$ 的 **行向量空间** 是 $\mathbb{R}^n$​上的向量，和它的加法以及数乘关系

同时，一些常用的运算法则也是适用于这个向量空间的

相对应地，可以定义 **列向量空间**

### 线性组合

$X=\alpha_1X_1+\cdots+\alpha_kX_k$ 是 $\mathbb{R}^k$ 上的向量 $X_i$ 上一个线性组合

如果 $Y=\beta_1X_1+\cdots+\beta_kX_k$

那么容易验证 $\alpha X+\beta Y$ 也是 $X_i$ 的一个线性组合

我们可以知道，由给定的向量组 $X_i$ 的所有线性组合组成的集合 $V$ 具有性质

$$
X, Y\in V\rightarrow \alpha X+\beta Y\in V
$$

显然零向量永远在 $V$​中

$V$ 用符号 $\langle X_1,X_2,\cdots,,X_K\rangle$ 表示，并称之为向量组 $X_i$ 的 **线性包（向量子空间）**，称包是在 $X_i$ 上 **张成的**，或 **生成的**

可以很容易证明，若 $V$ 是一个线性包，则 $\langle V \rangle=V$，特别地，若 $S\subset V$，则 $\langle S \rangle \subset V$, 于是我们得到了另一种定义

$$
\langle S\rangle =\bigcap_{S\subset V}V
$$

上式的含义是：$\langle S\rangle$ 可以定义成 $\mathbb{R}^n$ 中所有 **包含 $S$ 的线性包（或向量子空间）的交集**

注意等式右边，对于每一个 $V$，有 $X,Y\in V$，那么就有 $\alpha X+\beta Y\in V$, 于是 $\alpha X+\beta Y\in \bigcap V$，我们验证了 $RHS$ 是一个线性包

然而，两个线性包的并不一定是线性包，例如在 $\mathbb{R}^2,U=\{(\lambda,0)\},V=\{(0，\lambda)\}$, 我们取 $(1,0)$ 和 $(0,1)$，而 $(0,1)+(1,0)=(1,1)\notin U\bigcup V$

约定: 我们这样写单位列向量：

$$
E^{(k)}=\begin{pmatrix}0\\\vdots\\k\\\vdots\\0\end{pmatrix}= [0,\cdots, k,\cdots,0]
$$

### 线性相关性

如果存在不全为零的 $\alpha_1,\cdots,\alpha_k$ 使得空间 $\mathbb{R}^n$ 中的向量组 $X_1,\cdots,X_k$ 满足

$$
\alpha_1X_1+\cdots+\alpha_kX_k = 0
$$

就称这个向量组是 **线性相关** 的，否则就称这个向量组是 **线性无关** 的

<mark> 如果一个向量组是线性相关的，那么就代表这个向量组有“多余”的向量存在，因为它可以被其它向量所表示 </mark>

依靠这样的直观理解和简单的代数运算，我们可以直接得出以下推论

1.如果一个向量组的某一部分是线性相关的，那么它整体是线性相关的.反之若一个向量组线性无关，那么它的任意部分线性无关

2.若一个向量组线性相关，存在至少一个向量是其它向量的线性组合.若一个向量组中存在一个向量是其它向量的线性组合，则这个向量组线性相关

3.如果 $X_1,\cdots,X_k$ 线性无关，而 $X_1,\cdots,X_k,X$ 线性相关，则 $X$ 是向量 $X_1,\cdots,X_k$ 的线性组合

### 基与维数

设 $V$ 是 $\mathbb{R}^n$ 中的一个非零线性包，若线性无关的向量组 $X_1,X_2,\cdots,X_k$ 的线性包等于 $V$，就称这个向量组是 $V$ 的 **基**

也即 $V$ 中的每一向量可以由这个向量组唯一线性表出，这个系数称为该向量对于这个向量组的 **坐标**

下面我们要明确的问题是，$\mathbb{R}^n$ 中的每一个线性包中是否都有基，以及基的个数是否是不变的（可以很容易证明，一个线性包的基并非唯一的）

为了回答这两个问题，我们需要以下的引理

**设 $V$ 是 $\mathbb{R}^n$ 中的一个以 $X_1,\cdots,X_r$ 为基的线性包，$Y_1,\cdots,Y_s$ 是 $V$ 中一个线性无关的向量组，则 $s\le r$**

<mark> 这个引理揭示了如果一个向量组线性无关，那么这个向量组的个数应该不多于基的个数 </mark>

证明的方法是先写出 $Y_i$​的线性组合

$$
Y_1 = a_{11}X_1 + a_{21}X_2 + \dots + a_{r1}X_r,
$$

$$
Y_2 = a_{12}X_1 + a_{22}X_2 + \dots + a_{r2}X_r,
$$

$$
\vdots
$$

$$
Y_s = a_{1s}X_1 + a_{2s}X_2 + \dots + a_{rs}X_r,
$$

用反证法，如果 $s>r$，现在考察任意一个 $Y_i$ 的线性组合

$$
x_1Y_1 + \dots + x_sY_s = (a_{11}x_1 + a_{12}x_2 + \dots + a_{1s}x_s)X_1 + \dots
$$

$$
+ (a_{r1}x_1 + a_{r2}x_2 + \dots + a_{rs}x_s)X_r
$$

由于 $Y_i$ 是线性无关的，我们考虑如下方程组

$$
a_{11}x_1 + a_{12}x_2 + \dots + a_{1s}x_s = 0,
$$

$$
\vdots
$$

$$
a_{r1}x_1 + a_{r2}x_2 + \dots + a_{rs}x_s = 0.
$$

根据前面的知识，该方程组有非零解，然而，由线性无关的定义，该方程组不应该有非零解，于是就产生了矛盾

于是我们可以引出我们需要的答案

**$\mathbb{R}^n$ 中的每一个非零线性包都有一组有限基，并且它的所有基都含有相同个数的向量，它的个数 $r\le n$，我们称 $r$ 为该线性包的维数，记为 $dim_{\mathbb{R}}V$ 或 $dimV$**

证明的想法是先找出 $V$ 的一个线性无关向量组，然后不断的扩充它，然而因为 $\mathbb{R}^n$ 可以由 $n$ 个标准基表示，于是根据上面的引理，任意线性无关组最多只能有 $n$ 个元素，于是存在某个 $r$ 使得线性无关组 $X_1,\cdots,X_r\in V$ 是极大的，于是它们构成 $V$ 的一组基

设 $Y_1,\cdots,Y_s$ 也是 $V$ 的一组基，就有 $s\le r$，交换 $X$ 和 $Y$，就有 $r\le s$，于是 $s=r$​，命题得证

<mark> 这已经向我们提示了，维数就是一个向量组的极大无关组的个数 </mark>

定义 **秩**

$$
rank\{X_1, X_2,\cdots\}= dim\{X_1, X_2,\cdots\}
$$

## 矩阵的秩

### 秩

假设在 $\mathbb{R}^m$ 上有 $n$ 个列向量

$$
A^{(j)}= [a_{1j}, a_{2j},\cdots, a_{mj}]\quad j = 1,2,\cdots, n
$$

以及它们的线性包 $V=\langle A^{(1)},\cdots,A^{(n)}\rangle$，假设有一个向量 $B$ 在 $V$ 中，我们要将它表示出来，就要研究

$$
x_1
\begin{pmatrix}
a_{11} \\
a_{21} \\
\vdots \\
a_{m1}
\end{pmatrix}
+ x_2
\begin{pmatrix}
a_{12} \\
a_{22} \\
\vdots \\
a_{m2}
\end{pmatrix}
+ \cdots + x_n
\begin{pmatrix}
a_{1n} \\
a_{2n} \\
\vdots \\
a_{mn}
\end{pmatrix}=
\begin{pmatrix}
b_1 \\
b_2 \\
\vdots \\
b_m
\end{pmatrix}
$$

等价于研究一个线性方程组

$$
\begin{aligned}
a_{11}x_1 + a_{12}x_2 + \cdots + a_{1n}x_n &= b_1, \\
a_{21}x_1 + a_{22}x_2 + \cdots + a_{2n}x_n &= b_2, \\
\vdots \qquad\qquad\qquad\quad & \vdots \\
a_{m1}x_1 + a_{m2}x_2 + \cdots + a_{mn}x_n &= b_m.
\end{aligned}
\tag{2}
$$

写出它的系数矩阵和增广矩阵

$$
A =
\begin{pmatrix}
a_{11} & a_{12} & \cdots & a_{1n} \\
a_{21} & a_{22} & \cdots & a_{2n} \\
\vdots & \vdots &        & \vdots \\
a_{m1} & a_{m2} & \cdots & a_{mn}
\end{pmatrix}
$$

$$
(A | B) =
\begin{pmatrix}
a_{11} & a_{12} & \cdots & a_{1n} & b_1 \\
a_{21} & a_{22} & \cdots & a_{2n} & b_2 \\
\vdots & \vdots &        & \vdots & \vdots \\
a_{m1} & a_{m2} & \cdots & a_{mn} & b_m
\end{pmatrix}
\tag{3}
$$

上面的 $V$ 叫做 $m\times n$ 阶长方阵 $A$ 的 **列空间**，将 $V$ 记作 $V_c(A)$ 或 $V_c$，维数 $r_c(A)=dimV_c$ 叫做矩阵 $A$ 的 **列秩**

对于行向量，我们可以类似地定义 **行空间** $V_r=\langle A_{(1)},\cdots,A_{(m)}\rangle$，**行秩** $r_r(A)=dimV_r$

也即

$$
r_c(A)= rank\langle A^{(1)},\cdots, A^{(n)}\rangle\\r_r(A)= rank\langle A_{(1)},\cdots, A_{(m)}\rangle
$$

分别为行向量组和列向量组的秩

可以证明，**初等变换都是可逆的，且初等变换不改变矩阵的秩**

证明的思路为：

1.先证明行秩的情况，我们着眼于生成这个矩阵的线性包，将一行乘一个常数加到另一行后，这个线性包变为

$$
\langle A_{(1)},\cdots, A_{(s)}+\lambda A_{(t)},\cdots, A_{(m)}\rangle
$$

对比原来的 $\langle A_{(1)},\cdots,A_{(m)}\rangle$，这个线性包只是原来的一个线性组合，于是两者相等

2.只要证明对应方程组有同一个解即可，如果解不变，就意味着线性相关性不变，也即秩不变

**对于任意 $m \times n$ 阶矩阵 $A$，等式 $r_c(A)=r_r(A)$ 成立，这个数叫做矩阵的秩，记作 $rankA$**

### 可解性原则

**对于齐次线性方程组，主未知数的个数不依赖于化为阶梯形的方法，而是等于 $rankA$，其中 $A$ 是方程组的系数矩阵**

因为主未知数的个数等于阶梯型矩阵非零行的个数等一矩阵的秩，且矩阵的秩唯一确定

<mark> 这表明了矩阵的秩是矩阵的一个根本内在特征 </mark>

**线性方程组是可解的充要条件是 $rankA=rank(A|B)$**

可解性可以归结为以下问题：一个列向量 $B$ 是否可以被表示为 $A$ 的列向量的线性组合，如果 $B$ 可以这样表示，那么 $B\in \langle A^{(1)},\cdots,A^{(n)}\rangle$，那么 $rank\{A^{(1)},\cdots,A^{(n)}\}=rank\{A^{(1)},\cdots,A^{(n)},B\}$, 于是 $rank(A)=r_c(A)=r_c((A|B))=rank(A|B)$

## 线性映射与矩阵的运算

### 矩阵和映射

直接摘抄卓里奇这一部分的内容

$\mathbb{R}^n$ 是向量空间。

线性映射：$\mathbb{R}^m\rightarrow\mathbb{R}^n$

只需要将 $\mathbb{R}^m$ 中的每一个基向量在 $\mathbb{R}^n$ 中的像给出，就可以确定这个线性映射。

$$
L(e_i)= a_i^1\tilde e_1+\cdots+a_i^n\tilde e_n
$$

于是

$$
L(h)= L(h^ie_i)= h^iL(e_i)= h^ia_i^j\tilde e_j\quad (i = 1,2,\cdots, m)\quad j =(1,2,\cdots, n)
$$

写成矩阵形式，也即

$$
L(h)=\begin{pmatrix}L^1(h)\\
L^2(h)\\
\vdots \\
L^n(h)\end{pmatrix}
=\begin{pmatrix}a_1^1&\cdots&a_m^1\\
\vdots&&\vdots\\
a_1^n&\cdots&a_m^n\end{pmatrix}
\begin{pmatrix}h^1\\
h^2\\
\vdots \\
h^m\end{pmatrix}
$$

<mark> 注意，此处有 n 个坐标，而非 m 个，是代表着映射过后可以分解为 n 个分量，其中每一个都与 $h$ 的 $m$ 个分量有关。</mark>

**这个矩阵和每一个 $h$ 是一一对应的**

记

$$
\begin{pmatrix}a_1^1&\cdots&a_m^1\\
\vdots&&\vdots\\
a_1^n&\cdots&a_m^n\end{pmatrix}
= A
$$

这样，对于每一个 $f,g:X\rightarrow\mathbb{R}^n$，我们就有

$$
(\lambda f+\mu g)(x)=\lambda f(x)+\mu g(x)
$$

特别地，若 $f=L_1,g=L_2$, 则

$$
(\lambda L_1+\mu gL_2)(x)=\lambda L_1(x)+\mu L_2(x)=(\lambda A_1+\mu A_2)(x)
$$

若 $A:\mathbb{R}^m\rightarrow \mathbb{R}^n,B:\mathbb{R}^n\rightarrow \mathbb{R}^k$，则

$$
B\circ A(x)= B(A(x))= BA(x)
$$

我们同样引入爱因斯坦的求和约定（缩并）

### 矩阵的乘积

下面我们来具体得到矩阵乘积的法则

设

$$
[x_1, \cdots, x_n] \xrightarrow{\varphi_B} [y_1, \cdots, y_s] \xrightarrow{\varphi_A} [z_1, \cdots, z_m]
$$

用更为直观的方式来表示，就是

![image-20250405182638089](./《代数学引论》笔记.assets/image-20250405182638089.png)

$$
z_i = a_j^iy^i = a_j^ib_k^jx^k\\
z_i = c_k^ix^k
$$

对比两式，有

$$
c_k^i = a_j^ib_k^j
$$

其中

$$
i = 1,\cdots, m\quad
j = 1,\cdots, s\quad
k = 1,\cdots, n
$$

如果仍然按照我们的约定，上标表示列，下标表示行

那么可以将 $c_k^i,a_j^i,b_k^j$ 都写成矩阵，就有

$$
C_{m\times n}= A_{m\times k}\times B_{s\times n}
$$

而矩阵乘法（复合）的法则就由上面来给出

推论：**矩阵乘法满足结合律，不满足分配律**

### 矩阵的转置

矩阵的转置记为 $A^T$

显然它满足以下性质

$$
(A^T)^T = A\quad(A+B)^T = A^T+B^T\quad(\lambda A)^T =\lambda A^T
$$

矩阵乘积的转置满足以下等式

$$
(AB)^T = B^TA^T
$$

<mark> 老师称之为穿脱原理 </mark>

只要记

$$
A^T =(a'_{ki})\quad B^T =(b'_{jk})\\D = B^TA^T
$$

就有

$$
c_{ij}=\Sigma_{k = 1}^na_{ik}b_{kj}\quad d_{ji}=\Sigma_{k = 1}^nb'_{jk}a'_{ki}=\Sigma_{k = 1}^na_{ik}b_{kj}
$$

于是 $C^T=D$

这样就证明了结论

我们可以归纳地证明 $(A_1A_2\cdots A_r)^T=A_r^T\cdots A_2^TA_1^T$

并且 $rankA^T=rankA$

### 矩阵乘积的秩

有不等式

$$
rankAB\le min\{rankA,rankB\}
$$

成立

证明：可以把矩阵“分”为行与列

$$
C_{(i)}=A_{(i)}B\quad C^{(j)}=AB^{(j)}
$$

另外有

$$
r_1=rankA=dim\langle A_{(1)},\cdots,A_{(m)}\rangle
$$

不妨设$A_{(1)},\cdots,A_{r_1}$当作行向量基，那么就有

$$
A_{(k)}=\Sigma_{i=1}^{r_1}\lambda_{ki}A_{(i)}\quad r_1<k\le m
$$

于是

$$
C_{(k)} = A_{(k)} B = \left( \sum_{i=1}^{r_1} \lambda_{ki} A_{(i)} \right) B = \sum_{i=1}^{r_1} \lambda_{ki} (A_{(i)} B) = \sum_{i=1}^{r_1} \lambda_{ki} C_{(i)},
$$

这揭示了

$$
\langle C_{(1)}, \dots, C_{(m)} \rangle = \langle C_{(1)}, \dots, C_{(r_1)} \rangle.\\\text{rank} C = \text{dim} \langle C_{(1)}, \dots, C_{(m)} \rangle \le r_1 = \text{rank} A.
$$

对 $B$ 也有类似的结论，于是命题得证

取等条件有待研究

### 方阵

全体$n$阶实方阵记为$M_n(\mathbb{R})$(这里的实方阵指元素都是实数）

称$n$阶方阵的集合构成一个**结合环**

称集合$M_n(\mathbb{R})$为$\mathbb{R}$上的代数

定义单位矩阵$E=(\delta_{kj})$，其中

$$
\delta_{kj}=\begin{cases}1\quad k=j\\
0\quad k\not= j\end{cases}
$$

我们有 $\text{rank } E=n $

$$
EA=A=AE\quad A\in M_n(\mathbb{R})
$$

更加一般地

$$
diag_n(\lambda)A=\lambda A=Adiag_n(\lambda)
$$

**在$M_n$中，与任意矩阵可交换的矩阵是数量矩阵**

引入矩阵$E_{ij}$，它在第$i$行第$j$列的取值为1，其余部分取值为0，对于满足题设条件的矩阵$A$，有

$$
E_{ij}A=AE_{ij}
$$

将表达式显式地写出就可以发现，欲两者相等，需要满足当$k\not=i$时，该处为0，和$a_{ii}=a_{jj}$，于是命题得证

于是我们可以尝试对于矩阵$A\in M_n(\mathbb{R})$，找到一个$A'$，使得$AA'=E=A'A$，同时有

$$
AA'=E=A''A\rightarrow A''=A
$$

因为

$$
A''=A''E=A''(AA')=(A''A)A'=EA'=A'
$$

也即若$A'$存在，它必唯一，我们将它记作$A$的**逆矩阵**

若$A'$存在，就称$A$​是可逆的

称$A\in M_n(\mathbb{R})$为**非退化的**，若它的行（或列）向量组是线性无关的，即$\text{rank} A=n$，若$\text{rank} A<n$，则$A$称为**退化的**

**若$A\in M_n(\mathbb{R})$是可逆的的充要条件是$A$是非退化的**

先证必要性，由秩不等式

$$
n=\text{rank} E=\text{rank} AB\le min\{\text{rank} A,\text{rank} B\}\le n
$$

于是

$$
\text{rank} A=n
$$

再证充分性，我们的目的是构造出两个矩阵$A',A''$，使得$AA'=E=A''A$​

由$\text{rank} A=n$

$$
\langle E^{(1)},\cdots,E^{(n)}\rangle=\mathbb{R}^n=\langle A^{(1)},\cdots,A^{(n)}\rangle
$$

也即每一个$E^{(i)}$都可以由$A^{(i)}$的线性组合表示出来

$$
E^{(j)}=\Sigma_{i=1}^na'_{ij}A^{(i)}
$$

令矩阵$A'=(a'_{ij})\in M_n(\mathbb{R})$

按矩阵乘法的逆运算，上式就可以表示为

$$
E^{(j)}=AA'^{(j)}
$$

所以

$$
E=\langle E^{(1)},\cdots,E^{(n)}\rangle=\langle AA'^{(1)},\cdots,AA'^{(n)}\rangle=AA'
$$

于是我们找出了这个右逆，下面我们来找左逆，可以重复上面的步骤，也可以通过转置的性质

$A^T$同样也是非退化的，于是可以找出$A^TB=E$，令$A''=B^T$，则

$$
E=E^T=(A^TB)^T=B^T(A^T)^T=A''A
$$

于是也找出了右逆，证明完毕

<mark>双逆才表示矩阵可逆，存在右逆代表矩阵的行向量线性无关，存在左逆代表矩阵的列向量线性无关</mark>

有如下推论

**若$B$和$C$是$m$阶和$n$阶非退化方阵，而$A$是任意的$m\times n$矩阵，则**

$$
\text{rank} BAC=\text{rank} A
$$

这是秩不等式的直接推论

**若$A,B\in M_n(\mathbb{R})$且$AB=E$或$BA=E$,则$B=A^{-1}$**

这个引理说明，方阵且左（右）逆，那么矩阵就可逆

**若$A_1,A_2,\cdots,A_k$都是非退化的$n$阶方阵，那么$A_1A_2\cdots A_k$也是非退化的，且**

$$
(A_1A_2\cdots A_k)^{(-1)}=A_k^{-1}\cdots A_2^{-1} A_1^{-1}
$$

非退化性由推论一给出，两边同乘$A_1A_2\cdots A_k$可以验证该结论

### 矩阵的等价类

我们用$E_{st}$表示在第$s$行第$t$列为1，其余元素为0的$m$阶方阵，并称之为**矩阵单位**

下面研究$M_m(\mathbb{R})$​中的**初等矩阵**

$F_{s,t}=E-E_{s,s}-E_{t,t}+E_{s,t}+E_{t,s}$

$$
P_{s,t} =
\begin{pmatrix}
1 &        &        &        &        \\
  & \ddots &        &        &        \\
  &        & 0      & \cdots & 1      \\
  &        & \vdots & \ddots & \vdots \\
  &        & 1      & \cdots & 0      \\
  &        &        &        & &\ddots \\
  &        &        &        &        && 1
\end{pmatrix}
$$

记为第$(I)$型标准矩阵

$F_{s,t}(\lambda)=E+\lambda E_{s,t}$

$$
F_{s,t}(\lambda) = E + \lambda E_{s,t} =
\begin{pmatrix}
1 &        &        &        &        \\
  & &\ddots &        &        &        \\
  &        & \cdots & 1      & \cdots \lambda \cdots \\
  &        &        & &\ddots &        \\
  &        &        &        && 1
\end{pmatrix}


$$

记为$(II)$型标准矩阵

$F_s(\lambda)=E+(\lambda -1)E_{s,s}=diag\{1,\cdots,\lambda,\cdots,1\}$

记为$(III)$型标准矩阵

可以证明，**对矩阵进行一次初等行（列）变换，等价于对矩阵左（右）乘一个对应的初等矩阵**

对矩阵$A_{m\times n}$施以有限次初等变换，可以将它变为以下形式

$$
\begin{pmatrix}E_r&O\\
O&O\end{pmatrix}(*)
$$

这等价于

$$
P_kP_{k-1}\cdots P_1AQ_1\cdots Q_l=\begin{pmatrix}E_r&O\\
O&O\end{pmatrix}
$$

其中$P_i(Q_i)$是$m(n)$阶初等矩阵

初等变换是可逆的，这与初等矩阵是可逆的是相对应的

由前面的结论$P=P_k\cdots P_1$和$Q=Q_1\cdots Q_l$也是可逆的

称两个$m\times n$阶矩阵$A,B$是**等价的（相抵的）**,并记作$A\sim B$，如果能找到非退化的$m$阶和$n$阶矩阵$P,Q$，使得$B=PAQ$

容易验证，这确实是一个等价关系

再由前面的等价关系与划分一一对应的关系，由由于等价类的秩都相等，就选择$(*)$作为一个等价类的代表元，我们可以得到以下论断

**$m\times n$阶矩阵的集合可以划分成$p=min\{m,n\}+1$个等价类，所有秩为$r$的矩阵和代表元在同一类中**

<mark>$p$的个数，是由秩不等式和零矩阵确定的</mark>

于是得到以下推论

**每一个非退化的$n$阶方阵都可以写为初等矩阵的乘积**

### 逆矩阵的运算

此处给出了一个矩阵求逆的方法，也即将增广矩阵的一部分化为单位矩阵（满秩时）或者行阶梯形矩阵（退化时）

这种矩阵求逆的方法叫做**约化**

### 解空间

一个系数矩阵为$m\times n$阶矩阵$A$的线性方程组可以写为

$$
AX=B
$$

其中$X=[x_1,\cdots,x_n]$

如果假设$m=n$且$A$可逆，那么我们可以通过一种十分具有美感的方式求解该方程组

$$
A^{-1}AX=X=A^{-1}B
$$

并且由逆的唯一性，这个解也是唯一的

现在我们来求

$$
AX=0
$$

的全部解，显然若$X^{(1)},X^{(2)}$是齐次线性方程组的解，那么它们的线性组合也是该方程组的解，这个事实促使我们去研究它们的线性包

$$
V_A=\langle X\in\mathbb{R}^n|AX=0\rangle\subset \mathbb{R}^n
$$

<mark>此处的$V_A$与$V_c(A)$是不同的，下面的等式揭示了它与$V_c(A)$的关系</mark>

设$s=\dim V_A,r=\text{rank} A$，我们断言以下等式成立

$$
r+s=n
$$

如果用映射的眼光来看，将$A$看作一个线性映射$\phi_A:\mathbb{R}^m\rightarrow \mathbb{R}^n$，就有

$$
\ker \phi_A=V_A\quad \Im\phi_A=V_c(A)
$$

对于矩阵$A$,如果想要找到它的一组基，就要在$A$中选取$r$个列向量基，不妨设找到的是$A^{(1)},\cdots,A^{(r)}$，现在任取一个$A^{(r+k)}$，都有

$$
x_1^{(k)}A^{(1)}+\cdots+x_r^{(k)}A^{(r)}+A^{(r+k)}=0
$$

设有$(n-r)$个列向量

$$
X^{(1)}=[x_1^{(1)},\cdots,x_r^{(1)},1,\cdots,0]\\
X^{(2)}=[x_1^{(2)},\cdots,x_r^{(2)},0,1,\cdots,0]\\
\vdots\\
X^{(n-r)}=[x_1^{(n-r)},\cdots,x_r^{(n-r)},0,\cdots,1]
$$

由后$(n-r)$个向量的特殊性，这一组列向量显然是线性无关的，同时它们也是原线性方程组的解，也是$V_A$ 的一组基，自由变量如果像上面这样取，我们就可以得到解$X^{(k)}$

$\text{rank}=r$的齐次线性方程组$AX=0$的解空间的任意一组基称为一个**基础解系**，上面的向量组称为**规范基础解系**，它的秩$s=\dim V_A=n-r$,等于该方程组自由未知量的个数

# 行列式

## 基本构造和性质

### 几何背景

$n$阶方阵对应于一个广义平行六面体

$$
\Pi(A)=\Pi(A^{(1)},\cdots,A^{(n)})
$$

$n$阶方阵所对应的行列式可以看作这个广义平行六面体的体积

$$
\det A=v(\Pi(A))
$$

每一列可以看作一个标准列向量在映射之下的像

$$
A^{(j)}=\phi_A(E^{(j)})
$$

其中$\phi_A:X\rightarrow AX$

### 组合解析方法

即$Laplace$展开

$$
\det A=\Sigma_{\sigma\in S_n}\epsilon_{\sigma}a_{1\sigma(1)}\cdots a_{n\sigma(n)}
$$

### 基本性质

我们可以把$\det$看作一个函数：$\det:M_n(\mathbb{R})\rightarrow \mathbb{R}$

$det$根据需要，可以记为一个$n$个变量的函数

$$
\det[A_{(1)},\cdots,A_{(n)}]或\det(A^{(1)},\cdots,A^{(n)})
$$

现在我们着眼于一个函数$\mathcal{D}$

(1)如果它对于每一个变量都是线性的，我们就称它为多重线性的

(2)如果$\mathcal{D}(A_{(1)},\cdots,A_{(i)},A_{(i+1)},\cdots,A_{(n)})=-\mathcal{D}(A_{(1)},\cdots,A_{(i+1)},A_{(i)},\cdots,A_{(n)})$，就称它为斜对称的

从线性函数的定义出发，对于$\mathcal{D}$的每一列，如果把这一列看作变量，那么像可以看作这一列的多项式函数

由（2），有$\mathcal{D}(A_{(1)},\cdots,X,X,\cdots,A_{(n)})=0$

现在将目光回到$\det$函数上，来比较行与列的差别

$$
\det A=\det A^T
$$

用$Laplace$​展开即可证明

<mark>这个定理说明了对行成立的结论对列也成立，给我们提供了一种便利</mark>

现在给出$\det$函数的主要性质

$D1$.$\det$是关于$A$的多重线性函数

$D2.$ $\det$是斜对称函数

这两条性质是至关重要的，因为其它性质都是它们的推论，展开可以直接证明

$D3.\det E=0$

$D4.det\lambda A=\lambda^n\det A$

由$n$重线性性可以很容易地看出这一点

$D5.$有一行为零的行列式为零

$D6.$有两行相同的行列式为零

$D7.$对行列式施以行$(\Iota\Iota)$型初等变换，其值不变

可以看出，这不仅是对$\det$成立的，而是对任意的$\mathcal{D}$都成立

设$\bar A$是上三角矩阵，则

$$
\mathcal D(\bar A)=\mathcal D(E)\bar a_{11}\cdots\bar a_{nn}
$$

推论：

$$
\det\bar A=\bar a_{11}\cdots\bar a_{nn}
$$

现在引入代数余子式$A_{ij}$，就可以得到一个快速求矩阵的行列式的值的方法

$$
\det A=(-1)^q\bar a_{11}\cdots\bar a_{nn}
$$

其中$q$是矩阵变为上三角矩阵所经历的$(\Iota)$​型初等变换的次数

结合以上等式，就有推论

$$
\mathcal D(A)=\mathcal D(E)\det A
$$

这是因为

$$
\det A=(-1)^q\det\bar A=(-1)^q\bar a_{11}\cdots\bar a_{nn}\\
\mathcal D(A)=(-1)^q\mathcal D(\bar A)(第(\Iota\Iota)型初等变换不改变值)
$$

## 行列式的应用

### 非退化矩阵的判别标准

由$AA^{(-1)}=E$，得

$$
\det AA^{(-1)}=1
$$

这说明，**非退化矩阵的行列式不为零**，且

$$
\det(A^{-1})=(\det A)^{-1}
$$

现在我们来考察一个矩阵的**伴随矩阵**

$$
adj(A)=A^{\vee} = \left(
\begin{array}{cccc}
A_{11} & \cdots & A_{n1} \\
\vdots & \ddots & \vdots \\
A_{1n} & \cdots & A_{nn}
\end{array}
\right).
$$

也即用该矩阵元素的代数余子式代替原矩阵上该元素的位置而得来的矩阵

现在，我们有了第二种矩阵求逆的方法

$$
A^{-1}=(\det A)^{-1}adjA
$$

也可以写为

$$
\left(
\begin{array}{cccc}
a_{11} & \cdots & a_{1n} \\
\vdots & \ddots & \vdots \\
a_{n1} & \cdots & a_{nn}
\end{array}
\right)^{-1}
=
\left(
\begin{array}{cccc}
\frac{A_{11}}{\det A} & \cdots & \frac{A_{n1}}{\det A} \\
\vdots & \ddots & \vdots \\
\frac{A_{1n}}{\det A} & \cdots & \frac{A_{nn}}{\det A}
\end{array}
\right).
$$

在证明这个定理之前，先证明一个引理

若$A\in M_n(\mathbb{R})$,则

$$
a_{i1}A_{j1} + a_{i2}A_{j2} + \dots + a_{in}A_{jn} = \delta_{ij} \det A\\
a_{1i}A_{1j} + a_{2i}A_{2j} + \dots + a_{ni}A_{nj} = \delta_{ij} \det A
$$

这个公式可以通过更改行列式得到简单的证明

现在我们来着手证明这个定理

$$
\begin{pmatrix}
c_{11} & \dots & c_{1n} \\
\vdots & \ddots & \vdots \\
c_{n1} & \dots & c_{nn}
\end{pmatrix}
=
\begin{pmatrix}
a_{11} & \dots & a_{1n} \\
\vdots & \ddots & \vdots \\
a_{n1} & \dots & a_{nn}
\end{pmatrix}
\begin{pmatrix}
A_{11} & \dots & A_{n1} \\
\vdots & \ddots & \vdots \\
A_{1n} & \dots & A_{nn}
\end{pmatrix}.
$$

可知$(c_{ij})=(\delta_{ij}\det A)=(\det A)E$，也即

$$
(\det A)E=Aadj(A)\\
E=A(\det A)^{-1}adj(A)
$$

再由逆的唯一性，就有$A^{-1}=(\det A)^{-1}adj(A)$

推论：**一个行列式为零，当且仅当其行（或列）线性相关**

### 加边子式法

这是一个有效的方法，实际运用效果更强，因此不必证明，运用的过程内蕴证明
