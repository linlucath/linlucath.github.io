---
title: 'Calculus III'
publishDate: 2025-10-30
description: 'TODO'
tags:
  - Math
language: 'Chinese'
heroImage: { src: './default.jpg', color: '#D58388' }
---

## 常数项级数

### 求和方法

- 几何级数
- 错项级数

### 判断敛散性

- 比较判别法 (与几何级数, p 级数比较)

  - 不等式比较
  - 极限比较 & 泰勒展开

> 对于正项级数, 若 $\lim_{n \to \infty} \frac{a_n}{b_n} = C$, 其中 $C$ 为正常数, 则级数 $\sum a_n$ 与 $\sum b_n$ 同敛散.

- 比值判别法 / 达朗贝尔判别法 (当通项中含有 $n!, a^n$ 时适合使用)
  设 $\sum a_n$ 为正项级数, 若 $\lim_{n \to \infty} \frac{a_{n+1}}{a_n} = L$, 则当 $L<1$ 时级数收敛, 当 $L>1$ 时级数发散, 当 $L=1$ 时, 判别失败.

- 根值判别法 / 柯西判别法 (当通项中含有 $n^{n}, a^n$ 时适合使用)
  设 $\sum a_n$ 为正项级数, 若 $\lim_{n \to \infty} \sqrt[n]{a_n} = L$, 则当 $L<1$ 时级数收敛, 当 $L>1$ 时级数发散, 当 $L=1$ 时, 判别失败.

- \*拉贝判别法
  设 $\sum a_n$ 为正项级数, $\lim_{n \to \infty} n \left( \frac{a_n}{a_{n+1}} - 1 \right) = L$, 则当 $L>1$ 时级数收敛, 当 $L<1$ 时级数发散, 当 $L=1$ 时, 判别失败.

- 交错级数判别法 / 莱布尼兹判别法
  设交错级数 $\sum (-1)^{n} a_n$ 满足: (1) $a_{n+1} \leq a_n$; (2) $\lim_{n \to \infty} a_n = 0$, 则该级数收敛.

- 积分判别法
  设 $f(x)$ 在 $[1, \infty)$ 上连续, 且单调递减, 且 $f(n) = a_n$, 则级数 $\sum_{n=1}^{\infty} a_n$ 与反常积分 $\int_{1}^{\infty} f(x) \, dx$ 同敛散.

  > Tips: 
  > $$
  > \sum_{n=1}^{\infty} (-1)^{n-1} a_n = (a_1 - a_2) + (a_3 - a_4) + \cdots > 0 \\
  > \sum_{n=1}^{\infty} (-1)^{n-1} a_n = a_1 - (a_2 - a_3) - (a_4 - a_5) - \cdots < a_1
  > $$

> \*阿贝尔变换
> 设数列 $\{a_n\}$ 的前 $n$ 项和为 $A_n = \sum_{k=1}^{n} a_k$, 则有
>
> $$
> \sum_{k=1}^{n} a_k b_k = A_n b_n + \sum_{k=1}^{n-1} A_k (b_k - b_{k+1})
> $$
>
> 积分形式:
>
> $$
> \int_{a}^{b} f(x) g(x) \, dx = \left. F(x) g(x) \right|_{a}^{b} - \int_{a}^{b} F(x) d g(x)
> $$

- 阿贝尔判别法
  设级数 $\sum a_n b_n$ 满足:

  - 数列 $\{a_n\}$ 单调有界;
  - 数列部分和 $S_n = \sum_{k=1}^{n} b_k$ 收敛, 则级数 $\sum a_n b_n$ 收敛.

- 狄利克雷判别法
  设级数 $\sum a_n b_n$ 满足:
  - 数列 $\{a_n\}$ 收敛于 0 且单调;
  - 数列部分和 $S_n = \sum_{k=1}^{n} b_k$ 有界, 则级数 $\sum a_n b_n$ 收敛.

### 绝对收敛与条件收敛

- 绝对收敛: 若级数 $\sum a_n$ 满足级数 $\sum |a_n|$ 收敛, 则称级数 $\sum a_n$ 绝对收敛.
- 条件收敛: 若级数 $\sum a_n$ 收敛, 但级数 $\sum |a_n|$ 发散, 则称级数 $\sum a_n$ 条件收敛.
- \*交换项定理: 绝对收敛的级数, 任意交换其项次序后, 级数仍收敛, 且和不变. 而条件收敛的级数, 通过交换项次序, 可以使级数收敛到任意值, 甚至发散.

## 函数项级数

### 一致收敛

- 证明函数数列不一致收敛

  - 使用定义判别
    > $$
    > \sup_x|S_n(x) - S(x)| < \varepsilon
    > $$
  - 统一变量
    > $$
    > \exist \{x_n\} \in D, \text{ 使得 } \lim_{n \to \infty  } |S_n(x_n) - S(x_n)| \ge \varepsilon_0
    > $$

- 证明函数项一致收敛

  - Cauchy 收敛原理
    > $$ \forall \varepsilon > 0, \exist N, \text{ 当 } m,n > N, \text{ 对 } \forall x \in D, |S_n(x) - S_m(x)| < \varepsilon $$
  - Weierstrass 判别法
    > 若存在正项级数 $\sum a_n$ 收敛, 且对 $\forall x \in D$, 有 $|u_n(x)| \le a_n$, 则函数项级数 $\sum u_n(x)$ 在 $D$ 上**一致收敛**.
  - 阿贝尔判别法
    > 设函数项级数 $\sum u_n(x) v_n(x)$ 满足: (1) ${u_n(x)}$ 在 $D$ 上单调且有界; (2) 函数项级数 $\sum v_n(x)$ 在 $D$ 上一致收敛, 则函数项级数 $\sum u_n(x) v_n(x)$ 在 $D$ 上一致收敛.
  - 狄利克雷判别法
    > 设函数项级数 $\sum u_n(x) v_n(x)$ 满足: (1) $\{u_n(x)\}$ 在 $D$ 上单调且收敛于 0; (2) 函数项级数 $\sum v_n(x)$ 在 $D$ 上一致有界, 则函数项级数 $\sum u_n(x) v_n(x)$ 在 $D$ 上一致收敛.
  - Dini 判别法
    > 设函数项级数 $\sum S_n(x)$ 在闭区间 $[a,b]$ 上逐点收敛于函数 $S(x)$, 且
    > - $S(x)$ 在 $[a,b]$ 上连续;
    > - 对 $\forall n$, $u_n(x)$ 在 $[a,b]$ 上连续;
    > - $|S_n(x)|$关于$n$单调  
    > 则 $|S_n(x)|$ 在 $[a,b]$ 上一致收敛于 $S(x)$.
  - Levi 判别法
    > 如果函数项级数的每一项都在积分区间上非负，那么积分与求和可以交换顺序。即：
    > $$ \int \sum u_n(x) dx = \sum \int u_n(x) dx $$
    > 无论结果是收敛到一个有限数还是发散到 $+\infty$，等式都成立。

- 一致收敛的性质

### 判断幂级数敛散性

- 阿贝尔定理
  - 若级数 $\sum_{n=1}^{\infty} a_n x^n$ 在 $x=x_0$ 处收敛, 则当 $|x|<|x_0|$ 时, 级数**绝对收敛**.
  - 若级数 $\sum_{n=1}^{\infty} a_n x^n$ 在 $x=x_0$ 处发散, 则当 $|x|>|x_0|$ 时, 级数发散.
    > Tips:
        $$
        \sum_{n=1}^{\infty} a_n x_0^n \text{ 收敛 } \Rightarrow \lim_{n \to \infty} a_n x_0^n = 0 \Rightarrow |a_n x_0^n| \le M \\
        \Rightarrow | a_n | \le \frac{M}{|x_0|^n} \Rightarrow |a_n x^n| \le M \left| \frac{x}{x_0} \right|^n \Rightarrow \sum_{n=1}^{\infty} |a_n x^n| \text{ 收敛 }
        $$

### 条件收敛

> 条件收敛点只可能在收敛区间的端点

### 收敛半径的求解

- 不等式法
  利用判别收敛性的比值法或根值法, 求出收敛区间的范围, 进而得到收敛半径.

### 幂级数 -> 和函数

1. 求解收敛域
2. 在收敛域内对幂级数逐项积分或逐项微分, 从而将和函数表示为初等函数的形式 (等比, 指数的展开式)
3. 求解出和函数表达式

### 和函数 -> 幂级数

1. 将和函数通过积分或微分转化为易于展开的函数
2. 将转化后的函数展开为幂级数
3. 通过积分或微分将幂级数转化为原函数的幂级数

