---
title: 数学分析 二 速通
publishDate: 2025-05-07
description: '将分析对象扩展至 Euclid 空间'
tags:
  - Math
heroImage: { src: './thumbnail.jpg', color: '#B4C6DA' }
language: '中文'
---

> 在现实生活中, 只有单一变元的情况终究少数, 各个变量之间错综复杂才是常态

---

## 不定积分

### 需要背诵的不定积分表

$$
\begin{align*}
(1) & \quad \int x^a \, dx = \frac{x^{a+1}}{a+1} + C \quad (a \neq -1) \\
(2) & \quad \int \frac{1}{x} \, dx = \ln |x| + C \\
(3) & \quad \int a^x \, dx = \frac{a^x}{\ln a} + C \\
(4) & \quad \int \sin x \, dx = -\cos x + C \\
(5) & \quad \int \cos x \, dx = \sin x + C \\
(6) & \quad \int \tan x \, dx = -\ln |\cos x| + C \\
(7) & \quad \int \cot x \, dx = \ln |\sin x| + C \\
*(8) & \quad \int \sec x \, dx = \ln |\sec x + \tan x| + C \\
*(9) & \quad \int \csc x \, dx = \ln |\csc x - \cot x| + C \\
(10) & \quad \int \sec^2 x \, dx = \tan x + C \\
(11) & \quad \int \csc^2 x \, dx = -\cot x + C \\
(12) & \quad \int \frac{dx}{1+x^2} = \arctan x + C \\
(13) & \quad \int \frac{dx}{x^2 + a^2} = \frac{1}{a} \arctan \frac{x}{a} + C \\
*(14) & \quad \int \frac{dx}{x^2 - a^2} = \frac{1}{2a} \ln \left| \frac{a-x}{a+x} \right| + C \\
*(15) & \quad \int \frac{dx}{a^2 - x^2} = \frac{1}{2a} \ln \left| \frac{a+x}{a-x} \right| + C \\
(16) & \quad \int \frac{dx}{\sqrt{1-x^2}} = \arcsin x + C \\
(17) & \quad \int \frac{dx}{\sqrt{a^2 - x^2}} = \arcsin \frac{x}{a} + C \\
*(18) & \quad \int \frac{dx}{\sqrt{x^2 \pm a^2}} = \ln |x + \sqrt{x^2 \pm a^2}| + C \\
\end{align*}
$$

### 不定积分的基本方式

- 第一类换元法 (凑微分法)

$$
\begin{align*}
(1) & \quad \int x^2 \cdot \sqrt{x^3 + 1} \, dx \\
(2) & \quad \int \frac{1}{e^x + 1} \, dx \\
(3) & \quad \int \tan^3 x \, dx \\
(4) & \quad \int \frac{1}{\sin^2 x + 2 \cos^2 x} \, dx \\
\end{align*}
$$

- 第二类换元法 (变量代换法)

$$
\begin{align*}
(1) & \quad \int \frac{1}{(1 - x) \cdot \sqrt{1 - x^2}} \, dx \\
(2) & \quad \int \frac{1}{x^2 \sqrt{x^2 + 1}} \, dx \\
(3) & \quad \int \frac{1}{\sqrt{x} + \sqrt[3]{x}} \, dx
\end{align*}
$$

- 分部积分法 (两类不同函数相乘)

> 基本公式: $\int u \, dv = uv - \int v \, du$

$$
\begin{align*}
(1) & \quad \int x^2 e^x \, dx \\
(2) & \quad \int x^2 \cos x \, dx \\
(3) & \quad \int e^{2x} \cos x \, dx
\end{align*}
$$

> "反对幂三指", 越靠后的函数积分越容易凑到微分上, 越容易积分

- 分式的不定积分

1. 分母可因式分解: 裂项法
2. 分母不可因式分解, 分子零次: 配方法
3. 分母不可因式分解, 分子一次: 将分子凑成分母的导数
4. 分式为三角函数: 万能公式代换

> 技巧: 对于分母为多项式的, 尝试将分母化为单项式

## 定积分

### 利用定积分求数列极限

> $$
> \text{将和式转为}\lim_{n \to \infty} \sum_{i=1}^{n} \frac{1}{n} \cdot f\left(\frac{i}{n}\right) = \int_{0}^{1} f(x) \, dx
> $$

$$
(1) \quad \lim_{n \to \infty} \sum_{i=1}^{n} \frac{n}{n^2+i^2}
$$

### 变上限的定积分求导

> $$
> \int_{a}^{f(x)} g(t) \, dt = \int_{a}^{f(x)} g(f(x)) \, df(x) =g(f(x)) \cdot f'(x)
> $$

### 华里氏公式

> $$
> \begin{align*}
> & \int_{0}^{\frac{\pi}{2}} \sin^n x \, dx = \frac{(n-1)!!}{n!!} \cdot \frac{\pi}{2} \quad (n \text{ 为偶数}) \\
> & \int_{0}^{\frac{\pi}{2}} \sin^n x \, dx = \frac{(n-1)!!}{n!!} \cdot 1 \quad (n \text{ 为奇数}) \\
> \end{align*}
> $$

### 定积分换元

> $$
> \int_{g(t_1)}^{g(t_2)} f(x) \, dx = \int_{t_1}^{t_2} f(g(t)) \, dg(t)
> $$

### 定积分的求解

1. 利用奇偶性简化式子
2. 如果是两类不同积分相乘 or 存在变限积分, 尝试分部积分法
3. 如果是分式, 尝试裂项法或配方法

### 定积分的应用

- 计算图形的面积

  1. 尝试以 $x$ 或 $y$ 为自变量, 计算图形的面积
  2. 尝试以 $\theta$ 为自变量, 计算图形的面积
     > $$
     > S = \int_{\theta_1}^{\theta_2} \frac{1}{2} r^2 \, d\theta
     > $$

- 计算弧长

  1. $L = \int_{a}^{b} \sqrt{1 + y'^2} \, dx$，其中 $l$ 的方程为直角坐标方程 $y = y(x)$，$a \leq x \leq b$
  2. $L = \int_{\alpha}^{\beta} \sqrt{x'^2 + y'^2} \, dx$，其中 $L$ 为参数方程
     $
     \begin{cases}
     x = x(t) \\
     y = y(t)
     \end{cases}
     $
     $\alpha \leq t \leq \beta$
  3. $L = \int_{\alpha}^{\beta} \sqrt{r^2(\theta) + r'(\theta)^2} \, d\theta$，其中 $l$ 为极坐标方程：$r = r(\theta)$，$\alpha \leq \theta \leq \beta$

- 求旋转体的侧面积

  4. $S_{\text{侧}} = \int_{a}^{b} 2\pi y(x) \sqrt{1 + y'^2} \, dx$
  5. $S_{\text{侧}} = \int_{\alpha}^{\beta} 2\pi y(t) \sqrt{x'^2 + y'^2} \, dt$，其中曲线为参数方程
     $
     \begin{cases}
     x = x(t) \\
     y = y(t)
     \end{cases}
     $
     $\alpha \leq t \leq \beta$
  6. $S_{\text{侧}} = \int_{\alpha}^{\beta} 2\pi r(\theta) \cdot \cos(\theta) \sqrt{r^2(\theta) + r'(\theta)^2} \, d\theta$，其中曲线为极坐标方程 $r = r(\theta)$

## 多元函数的微分学

### 多元函数求极限

0. 判断极限是否存在 (从不同路径趋近一下)
1. 直接代入法
2. 利用恒等变形
3. 整体代换 + 洛必达
4. 无穷小替换
5. 使用极坐标进行代换

$$
\begin{align*}
(1) & \quad \lim_{(x,y) \to (0,0)} \frac{2 - \sqrt{xy+4}}{xy} \\
(2) & \quad \lim_{(x,y) \to (0,0)} \frac{x^2y}{x^4 + y^2} \\
(3) & \quad \lim_{(x,y) \to (0,0)} \frac{x^3y}{x^4 + y^4} \\
\end{align*}
$$

### 多元隐函数求偏导数

> 左右同时对对应未定元求偏导数

### 多元函数求极值

1. 求驻点
2. 利用二阶偏导数判别法

> 判别式：
>
> $$
>    D = \begin{vmatrix}
>    f_{xx} & f_{xy} \\
>    f_{yx} & f_{yy}
>    \end{vmatrix} = f_{xx} f_{yy} - (f_{xy})^2
> $$

### 多元函数在约束条件下求极值

令约束条件为 $\phi(x,y) = 0$，所求极值为 $f(x,y)$，则只需求函数 $g(x,y, \lambda) = f(x,y) + \lambda \cdot \phi(x,y)$ 的驻点

## 二重积分的计算

$$
\iint_D f(x,y) \, dx \, dy
$$

0. 画出积分区域 $D$ 的草图

### 非圆周相关区域下二重积分的计算

1. 定 $x$ 穿 $y$ 或定 $y$ 穿 $x$ , 转换为分步 两次定积分

> $$
> \begin{align*}
> (1) & \quad \iint_D f(x,y) \, dx \, dy = \int_{a}^{b} \left( \int_{g_1(x)}^{g_2(x)} f(x,y) \, dy \right) dx \\
> (2) & \quad \iint_D f(x,y) \, dx \, dy = \int_{c}^{d} \left( \int_{h_1(y)}^{h_2(y)} f(x,y) \, dx \right) dy
> \end{align*}
> $$

### 利用三角换元求解三重积分

> 令
> $
\begin{cases}
x = r \cos \theta \\
y = r \sin \theta
\end{cases}
$从而 $dx \, dy = r \, dr \, d\theta$, 其中系数由几何意义求出

## 三重积分的计算

$$
\iiint_{\Omega} f(x,y,z) \, dx \, dy \, dz
$$

0. 画出积分区域 $\Omega$ 的草图

### 利用投影法 ("先一后二") 求二重积分

> 定 $xy$ 穿 $z$，转为对二重积分进行积分

### 利用平面截割法 ("先二后一") 求三重积分

> 定 $z$ 穿 $xy$，转为对三重积分进行积分

### 利用三角换元求三重积分

- 令
  $
\begin{cases}
x = r \sin \phi \cos \theta \\
y = r \sin \phi \sin \theta \\
z = r \cos \phi
\end{cases}
$, 则 $dx \, dy \, dz = r^2 \sin \phi \, dr \, d\phi \, d\theta$

## 曲线积分

0. 画出曲线的草图

### 对弧长曲线积分 (第一类曲线积分) 的计算

$$
\int_L f(x,y) \, ds
$$

1. 转为常规积分

   > $$
   > \begin{align*}
   > ds &= \sqrt{1 + y'(x)^2} \, dx \\
   > ds &= \sqrt{x'(t)^2 + y'(t)^2} \, dt
   > \end{align*}
   > $$

2. 代入, 转换为单变量积分

### 对坐标的曲线积分 (第二类曲线积分) 的计算

$$
\int_L P \, dx + Q \, dy
$$

> 即在对应投影平面上的积分

1. 若 $L$ 为直角坐标方程 $y = y(x)$，则
   > $$
   > \int_L P \, dx + Q \, dy = \int_{a}^{b} P(x,y(x)) \, dx + Q(x,y(x)) \cdot y'(x) \, dx
   > $$
2. 若 $L$ 为参数方程
   > $$
   > \int_L P \, dx + Q \, dy = \int_{\alpha}^{\beta} P(x(t),y(t)) \cdot x'(t) + Q(x(t),y(t)) \cdot y'(t) \, dt
   > $$

### 格林公式

> 使用场景: 曲线为简单闭合曲线

$$
\oint_L P \, dx + Q \, dy = \pm \iint_D \left( \frac{\partial Q}{\partial x} - \frac{\partial P}{\partial y} \right) \, dx \, dy
$$

其中 $D$ 为 $L$ 所围成的区域

> 一般来说, 逆时针方向为正, 顺时针方向为负

## 曲面积分

### 对面积的曲面积分 (第一类曲面积分) 的计算

$$
\iint_S f(x,y,z) \, dS
$$

1. 转为常规积分
   > $$
   > dS = \sqrt{1 + \left( \frac{\partial z}{\partial x} \right)^2 + \left( \frac{\partial z}{\partial y} \right)^2} \, dx \, dy
   > $$
2. 代入, 转换为双变量积分

### 对坐标的曲面积分 (第二类曲面积分) 的计算

$$
\iint_{\sum} P \, dx + Q \, dy + R \, dz
$$

> $$
> \iint_{\sum}R(x,y,z) \, dx \, dy = \pm \iint_{D_{xy}} R(x,y,z(x,y)) ,dx \, dy
> $$
>
> 坐标轴正方向 (前, 上, 右) 为正方向, 负方向 (后, 下, 左) 为负方向

### 高斯公式

> 使用场景: 曲面为简单闭合曲面

> $$
> \oiint_{\sum} P \, dx\, dy + Q \, dy \, dz + R \, dz \, dx = \pm \iiint_{\Omega} \left( \frac{\partial R}{\partial y} + \frac{\partial Q}{\partial x} + \frac{\partial P}{\partial z} \right) \, dx \, dy \, dz
> $$
>
> 外侧为正, 内侧为负

## 期末考点

### 定积分

- 黎曼积分定义

  设 $f(x)$ 是定义在 $[a,b]$ 上的有界函数，在 $[a,b]$ 上任意取分点 $\{x_i\}_{i=0}^n$，作成一种划分

  $$
  P: a = x_0 < x_1 < x_2 < \cdots < x_n = b
  $$

  并任意取点 $\xi_i \in [x_{i-1}, x_i]$。记小区间 $[x_{i-1}, x_i]$ 的长度为 $\Delta x_i = x_i - x_{i-1}$，并令 $\lambda = \max_{1 \leq i \leq n}(\Delta x_i)$，若当 $\lambda \to 0$ 时，极限

  $$
  \lim_{\lambda \to 0} \sum_{i=1}^{n} f(\xi_i) \Delta x_i
  $$

  存在，且极限值既与划分 $P$ 无关，又与对 $\xi_i$ 的取法无关，则称 $f(x)$ 在 $[a,b]$ 上 **Riemann 可积**，和式

  $$
  S_n = \sum_{i=1}^{n} f(\xi_i) \Delta x_i
  $$

  称为 **Riemann 和**，其极限值 $I$ 称为 $f(x)$ 在 $[a,b]$ 上的**定积分**，记为

  $$
  I = \int_a^b f(x) \, dx
  $$

  这里 $a$ 和 $b$ 分别被称为积分的下限和上限。

- 可积条件

> 有界函数 $f(x)$ 在 $[a,b]$ 上积分的充分必要条件是，对于任意分割 $P$，当 $\lambda = \max(\Delta x_i) \to 0$ 时，Darboux 大和与 Darboux 小和的极限相等，即成立
>
> $$
> \lim_{\lambda \to 0} \overline{S}(P) = L = l = \lim_{\lambda \to 0} \underline{S}(P)
> $$

- 积分线函数

  见 [定积分的应用](#定积分的应用)

- 黎曼求和

  见 [利用定积分求数列极限](#利用定积分求数列极限)

- 定积分的计算技巧

  见[定积分的求解](#定积分的求解), [华里氏公式](#华里氏公式)

- 反常积分的敛散性判定

  放缩, 计算其定积分即可

### 多元函数微分学

- 可微的判定

  设函数 $z = f(x, y)$ 在 $(x_0, y_0)$ 点的某个邻域上存在偏导数，并且偏导数在 $(x_0, y_0)$ 点连续，那么 $f$ 在 $(x_0, y_0)$ 点可微。

- 混合偏导相等判定

  如果函数 $z = f(x, y)$ 的两个混合偏导数 $f_{xy}$ 和 $f_{yx}$ 在点 $(x_0, y_0)$ 连续，那么等式

  $$
  f_{xy}(x_0, y_0) = f_{yx}(x_0, y_0)
  $$

  成立。

- Taylor 展开

  **定理 12.3.3（Taylor 公式）** 设函数 $f(x,y)$ 在点 $(x_0, y_0)$ 的邻域 $U = O((x_0, y_0), r)$ 上具有 $k+1$ 阶连续偏导数，那么对于 $U$ 内每一点 $(x_0 + \Delta x, y_0 + \Delta y)$ 都成立

  $$
  \begin{align}
  f(x_0 + \Delta x, y_0 + \Delta y) &= f(x_0, y_0) + \left( \Delta x \frac{\partial}{\partial x} + \Delta y \frac{\partial}{\partial y} \right) f(x_0, y_0) + \\
  &\quad \frac{1}{2!} \left( \Delta x \frac{\partial}{\partial x} + \Delta y \frac{\partial}{\partial y} \right)^2 f(x_0, y_0) + \cdots + \\
  &\quad \frac{1}{k!} \left( \Delta x \frac{\partial}{\partial x} + \Delta y \frac{\partial}{\partial y} \right)^k f(x_0, y_0) + R_k
  \end{align}
  $$

  其中 $R_k = \frac{1}{(k+1)!} \left( \Delta x \frac{\partial}{\partial x} + \Delta y \frac{\partial}{\partial y} \right)^{k+1} f(x_0 + \theta \Delta x, y_0 + \theta \Delta y)$，$(0 < \theta < 1)$ 称为 **Lagrange 余项**。

- 无条件极值

  见 [多元函数求极值](#多元函数求极值)

- 条件极值

  见 [多元函数在约束条件下求极值](#多元函数在约束条件下求极值)

### 多元函数积分学

- 二重积分的计算

  见 [二重积分的计算](#二重积分的计算)

- 三重积分的计算

  见 [三重积分的计算](#三重积分的计算)

- 曲线积分

  见 [曲线积分](#曲线积分)

- 曲面积分

  见 [曲面积分](#曲面积分)

- 积分与路径无关

  设 $D$ 为平面上的单连通区域，$P(x,y)$，$Q(x,y)$ 在 $D$ 上具有连续偏导数，则下面的四个命题等价：

  1.  对于 $D$ 内的任意一条光滑（或分段光滑）闭曲线 $L$，
      $$\oint_L P\,dx + Q\,dy = 0;$$

  2.  曲线积分 $\int_L P\,dx + Q\,dy$ 与路径无关；

  3.  存在 $D$ 上的可微函数 $U(x,y)$，使得
      $$dU = P\,dx + Q\,dy,$$

  即 $P\,dx + Q\,dy$ 为 $U(x,y)$ 的全微分，这时称 $U(x,y)$ 为1-形式 $P\,dx + Q\,dy$ 的**原函数**；

  4.  在 $D$ 内成立等式
      $$\frac{\partial P}{\partial y} = \frac{\partial Q}{\partial x}.$$
