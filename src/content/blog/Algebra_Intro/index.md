---
title: 代数学引论 笔记
publishDate: 2025-05-07
description: 'TODO'
tags:
  - TODO
heroImage: { src: './thumbnail.jpg', color: '#B4C6DA' }
language: '中文'
---

# 完备度量空间

## 度量空间的完备化

$\mathcal{Def}.$包含给定度量空间$(X,d)$的最小的完备度量空间称为空间$(X,d)$的完备化空间

目前我们尚不知晓这个“最小”的具体含义

$\mathcal{Def.}$如果度量空间$(X,d)$是完备度量空间$(Y,d)$ 的子空间，而集合$X\subset Y$ 在$Y$中处处稠密，则空间$(Y,d)$称为度量空间$(X,d)$的完备化空间

需要注意的是稠密和这两个空间具有同一种度量

$\mathcal{Def.}$称度量空间$(X_1,d_1)$和$(X_2,d_2)$是等距的，如果存在双射$f:X_1\rightarrow X_2$ ,使得对于$\forall a,b\in X_1$，有

$$
d_1(f(a),f(b))=d_2(a,b)
$$

成立，此时$f$称为等距映射

容易验证，该关系确实是一个等价关系，于是对于等距的度量空间，我们不再对它们彼此区分

现在我们尝试证明若完备化空间存在则唯一，先给出一个引理

$\mathcal{Lemma}.$

$$
|d(a,b)-d(u,v)|\le d(a,u)+d(b,v)
$$

这是三角不等式的直接推论

$\mathcal{Prep}$. 若度量空间$(Y_1,d_1),(Y_2,d_2)$ 是同一个度量空间$(X,d)$的完备化空间，则它们是等距的

按如下方法构造我们想要的等距映射：若$x\in X$, 则定义$f(x)=x$，此时$f$显然符合等距映射的定义；对于$y_1\in Y_1/X$，由完备度量空间的定义，$y_1$是一个$X$中的极限点，则可以选出$X$中的一个点列$\{x_n\}$使它趋向于$y_1$，那么在$Y_2$中，$\{x_n\}$ 也会收敛到一个$y_2\in Y_2$，现在我们定义$f(y_1)=y_2$，从构造过程可以看出它是一个满射，现在我们验证$f$是一个等距映射（同时这个过程也说明它是单射），即成立

$$
d_2(f(y_1'),f(y_1''))=d_1(y_1',y_1'')
$$

注意：由于这些点都不在$X$中，所以不可以使用完备化空间与原度量空间度量相同的性质，而只能通过极限来“逼近”（回想用有理数逼近无理数）

由

$$
|d_1(y_1',y_1'')-d_1(x_n',x_n'')|\le d_1(y_1',x_n')+d_1(y_2'+x_n'')
$$

知

$$
d_1(y_1',y_1'')=\lim_{n\rightarrow\infty}d_1(x_n',x_n'')=\lim_{n\rightarrow\infty}d_(x_n',x_n'')
$$

同样地有

$$
d_2(f(y_1'),f(y_1''))=\lim_{n\rightarrow\infty}d_2(f(x_n'),f(x_n''))=\lim_{n\rightarrow\infty}d(f(x_n'),f(x_n''))=\lim_{n\rightarrow\infty}d_(x_n',x_n'')
$$

于是就得到了我们的目标式，通过这个等距映射的定义式我们知道，若

$$
d_2(f(y_1'),f(y_1''))=0
$$

只能是

$$
f(y_1')=f(y_1''),y_1'=y_1''
$$

这就验证了它是单射，于是证毕

现在我们根据等距的观点，给出完备化空间的推广定义

$\mathcal{Def}.$称完备度量空间$(Y,d_Y)$为度量空间$(X,d_X)$的完备化空间，如果在$(Y,d_Y)$中存在与$(X,d_X)$等距的处处稠密子空间

这个定义不再要求$X\subset Y$，而只要与$X$等距的空间在$Y$之中

$\mathcal{Prep}.$每一个度量空间都具有完备化空间

# 拓扑空间的连续映射

## 映射的极限

经过拓扑知识的铺垫，现在我们完全有自信用拓扑的语言来定义极限，而不必再利用度量

$\mathcal{Def}.$设$f:X\rightarrow Y$是具有确定的基$\mathcal{B}$ 的集合$X$到拓扑空间$Y$的映射，称$A\in Y$是$f:X\rightarrow Y$在基$\mathcal{B}$上的极限，记作$\lim_{\mathcal{B}}f(x)=A$,若满足

$$
\forall V(A)\subset Y\quad \exist B\in\mathcal{B}(f(B)\subset V(A))
$$

如果引入度量，我们就得到熟知的$\epsilon-\delta$语言

我们说明，如果$Y$不是$Hausdorff$空间，那么极限的唯一性就不再成立了，回忆唯一性的证明，我们正是由两个点有不交的领域这个基本事实来说明矛盾的

如果$Y$是度量空间，那么我们就可以谈论**有界映射**和**最终有界映射**

如果再加上完备的条件，映射极限存在的柯西准则成立

## 连续映射

$\mathcal{Def}.$若$X,Y$都是拓扑空间，称$f:X\rightarrow Y$在$a\in X$是连续的，如果对于所有$V(f(a))\sub Y$，都有$U(a)\sub X$使得$f(U(a))\sub Y$

由连续映射构成的集合记为$C(X,Y)$

$Prep.$设$(X,\tau_X),(Y,\tau_Y)$均为拓扑空间，则映射$f:X\rightarrow Y$连续的充要条件是$Y$的任何开（闭）子集的原像是$X$中的开（闭）集

只需要证明开集的情况

先证必要性，如果$f$是连续的，我们验证$G_X$是开集，那么对任意的$G_Y$,都可以找到点$a$在$X$中的领域$U_X(a)$，使$f(U_X(a))\sub G_Y$，即$U_X(a)\sub f^{-1}(G_Y)=G_X$，于是$G_X=\bigcup_{a\in G_X}U_X(a)$确实是开集

再证充分性，如果任意$G_Y$的原像$G_X$都是开集，那么对于$a\in G_X$,就可以找到一个$U_X(a)\sub G_X$，且$f(U_X(a))\sub f(G_X)=G_Y$，这恰好是连续的定义

这个命题可以看作连续性的等价定义，这样，除了$\epsilon-\delta$语言和领域语言，我们又得到了（在拓扑空间中）连续的一种定义

$\mathcal{Def}.$称拓扑空间$(X,\tau_X)$到拓扑空间$(Y,\tau_Y)$上的双射映射$f$为**同胚映射**或**同胚**，如果$f$和它的逆映射$f^{-1}$都是连续的

$\mathcal{Def}.$两个拓扑空间称为**同胚空间**，如果它们可以找到一个同胚映射

正如我们在度量空间中将等距空间视作等价的，我们在拓扑空间中将同胚空间也视为一种等价关系

现在指出一些拓扑空间中连续映射的局部性质，它们与一元实值函数的情况是相同的

$\mathcal{Prep}.$连续复合映射的连续性、有界性（要求像集是度量空间）、振幅与连续的等价性（在度量空间上）成立

连续映射的整体性质是和紧集密切相关的，引入紧性让我们把一元的情况也看得更加清楚

$\mathcal{Prep}.$连续映射把紧集映为紧集

由于开集的原像是开集，设$\{G_Y=f(G_X)\}$是$(Y,G_Y)$中的开集，那么原像也是开集，对原像取有限覆盖，那么就得到了$f(K)\sub Y$ 的有限覆盖

$\mathcal{Prep}.$紧集上的连续实值函数在该紧集内取到最值

$f(K)$是$\mathbb{R}$上的紧集，即有界闭集，即$\sup f(K)\in f(K)$

下面我们研究拓扑空间上的$Cantor$定理，这需要先引入一个定义

$\mathcal{Def}.$称$f$为拓扑空间$(X,d_X)$到$(Y,d_X)$上的一致连续函数，如果$\forall \epsilon>0$都有$\delta>0$使得$f$在任何直径小于$\delta$的集合上振幅$\omega(f,E)<\epsilon$

直径可以理解为两个点的最远距离

$\mathcal{Def}.$度量紧集$K$到度量空间$(Y,d_Y)$的连续映射$f:K\rightarrow Y$是一致连续映射

由紧集的定义，是显然的

$Prep.$​连续映射下，连通拓扑空间的像是连通的

回忆连通的拓扑空间的定义，如果除了空间本身和$\emptyset$没有其它的开闭子集，或者它无法表示为两个没有公共点的非空开（闭）子集的并集

证明的方法是，设$E_Y$是$(Y,\tau_Y)$上的开闭子集，则$E_X$也是$X$中的开闭集，由于$X$是连通空间，所以要么$E_X=\empty$，要么$E_X=X$，也即要么$E_Y=\empty$，或者$E_Y=Y=f(X)$

我们有以下推论

$Prep.$若连通拓扑空间中的连续函数$f:X\rightarrow \mathbb{R}$取值$f(a)=A,f(b)=B$，对介于$A,B$中的任何一个值$C$，都有$f(c)=C$

这是因为只有区间才是$\mathbb{R}$中的连通集

# 压缩映射原理

$Def.$点$a\in X$称为映射$X\rightarrow X$的**不动点**，如果$f(a)=a$

$Def.$度量空间$(X,d)$到自身的映射$f:X\rightarrow X$称为**压缩映射**，如果存在$q\in (0,1)$使得

$$
d(f(x_1),f(x_2))\le qd(x_1,x_2)
$$

对$\forall x_1,x_2$成立

$Prep.$完备度量空间$(X,d)$到自身的压缩映射$f:X\rightarrow X$具有唯一的不动点$a$

此外，对于任何点$x_0\in X$，迭代序列$x_0,x_1=f(x_0),\cdots,x_{n+1}=f(x_n),\cdots$收敛到$a$，且收敛速度可以如下估计

$$
d(a,x_n)\le \frac{q^n}{1-q}d(x_1,x_0)
$$

这根据三角不等式就可以控制，同时由映射的连续性就有

$$
a=\lim_{n\rightarrow \infty}x_{n+1}=\lim_{n\rightarrow \infty}f(x_n)=f(\lim_{n\rightarrow \infty}x_n)=f(a)
$$

由极限的唯一性就有$a$的唯一性

$Prep.$（不动点的稳定性）设 $(X, d)$ 是完备度量空间，$(\Omega, \tau)$ 是拓扑空间，它在下面起参变量空间的作用。

设参变量 $t \in \Omega$ 的每个值对应空间 $X$ 到自身的压缩映射 $f_t : X \to X$，并且以下条件成立：

a) 映射族 $\{f_t, t \in \Omega\}$ 是一致压缩的，即存在一个数 $q$, $0 < q < 1$，使每个映射 $f_t$ 是压缩映射并且满足不等式

b) 对于每个 $x \in X$，映射 $f_t(x) : \Omega \to X$ 作为 $t$ 的函数在某个点 $t_0 \in \Omega$ 连续，即

$$
\lim_{t \to t_0} f_t(x) = f_{t_0}(x).
$$

这时，方程 $x = f_t(x)$ 的解 $a(t) \in X$ 在点 $t_0$ 连续地依赖于 $t$，即

$$
\lim_{t \to t_0} a(t) = a(t_0).
$$

这个过于抽象的命题也许有广泛的应用，不过目前难以体会，我们通过解微分方程来说明它的作用

若函数$f\in C(\mathbb{R}^2,\mathbb{R})$满足

$$
|f(u,v_1)-f(u,v_2)|\le M|v_1-v_2|\quad(Lipchitz连续)
$$

其中$M$是一个常数，则对于任何初值条件

$$
y(x_0)=y_0
$$

必然存在点$x_0\in \mathbb{R}$的领域$U(x_0)$和定义在该点的唯一的函数$y=y(x)$，使方程

$$
y'=f(x,y)
$$

和初值条件成立

具体操作如下

将上述等式写为

$$
y(x)=y_0+\int_{x_0}^xf(t,y(t))dt
$$

用$A(y)$表示等式的右边，再取自然度量

$$
d(Ay_1, Ay_2) = \max_{x \in V(x_0)} \left| \int_{x_0}^x f(t, y_1(t))\,dt - \int_{x_0}^x f(t, y_2(t))\,dt \right|
$$

$$
\leq \max_{x \in V(x_0)} \left| \int_{x_0}^x M[y_1(t) - y_2(t)]\,dt \right| \leq M|x - x_0|\,d(y_1, y_2).
$$

这样只要取$|x-x_0|\le1/(2M)$ ,就满足了压缩映射的条件，也即它有唯一不动点

# 线性赋范空间

## 范数

$Def.$设$X$是实数域或复数域上的线性空间，让每一个向量$x\in X$对应一个实数的函数$\|x\|$称为$X$中的范数，它满足以下的条件：

非退化性：$\|x\|=0$与$x=0$等价

齐次性：$\|\lambda x\|=|\lambda|\|x\|$

三角不等式：$\|x_1+x_2\|\le \|x_1\|+\|x_2\|$

具有范数的线性空间称为**赋范线性空间**

可以证明，范数总是非负的，且满足$|\|x_1\|-\|x_2\||\le \|x_1-x_2\|$

任意赋范线性空间，我们可以自然地引入一个度量，即将范数作为一个度量，这个意义上，我们就把赋范线性空间当作一个度量空间来研究

此外，由于空间中具有线性结构，我们还有

$$
d(x_1+x,x_2+x)=\|(x_1+x)-(x_2+x)\|=\|x_1-x_2\|=d(x_1,x_2)
$$

即平移不变性，以及

$$
d(\lambda x_1,\lambda x_2)=|\lambda|\|x_1-x_2\|=|\lambda|d(x_1,x_2)
$$

即齐次性

$Def.$称具有自然度量的完备度量空间为$Banach$空间

最重要的范数是$Minkowski$范数

$$
\|x\|_p:=(\Sigma_{i=0}^n|x^i|^p)^{\frac{1}{p}}
$$

其中$p\ge 1$

当$p=1,2,\infty$时都具有重要意义，并且

$$
\|x\|_{\infty}\le\|x\|_p\le\|x\|_1\le n\|x\|_{\infty}
$$

根据夹逼准则，我们可以看出具有$Minkowski$范数的赋范线性空间是$Banach$空间

在线性空间$C_{[a,b]}$中，我们经常考虑如下范数

$$
\|f\|:=\max_{x\in[a,b]}|f(x)|
$$

我们可以验证，这个范数下的度量空间是完备的，然而引入另一个范数

$$
\|f\|_p=(\int_a^b|f|^p(x)dx)^{\frac{1}{p}},\quad p \ge 1
$$

由前面的分析我们知道这个度量空间不是完备的

# 线性算子和多重线性算子

## 定义和实例

$Def.$设$X$和$Y$是同一个数域上的线性空间对于$X$中的任何向量$x_1,x_2$和任何数$\lambda,\mu$,映射$A:X\rightarrow Y$满足等式

$$
A(\lambda x_1+\mu x_2)=\lambda A(x_1)+\mu A(x_2)
$$

则$A$称为线性映射

$Def.$线性空间$X_1,\cdots,X_n$的直积到线性空间$Y$的映射

$$
A:X_1\times X_2\times\cdots\times X_n\rightarrow Y
$$

称为**多重线性映射（$n$重线性映射）**，如果该映射$y=A(x_1,\cdots,x_n)$相对于每一个变量都是线性的

这样的函数的集合记为$\mathcal{L}(X_1,\cdots,X_n;Y)$

$n=2$时，称之为**双线性映射**

应当区分，$n$重线性映射$A\in \mathcal{L}(X_1,\cdots,X_n;Y)$和线性空间$X=X_1\times\cdots\times X_n$的线性映射$A\in \mathcal{L}(X;Y)$

如果$Y=\mathbb{R}$或者$\mathbb{C}$,则线性映射和多重线性映射称为**线性函数**和**多重线性函数**（或者**线性泛函**和**多重线性泛函**）

若$Y$是更一般的线性空间，则线性映射$A:X\rightarrow Y$更经常称为空间$X$到空间$Y$的**线性算子**

现在考虑线性映射的一些例子

$\exp1.$ 设$l_0$是有限数列的线性空间，用以下方式定义算子$A:l_0\rightarrow l_0$:

$$
A((x_1,\cdots,x_n,0,\cdots))=(1x_1,2x_2,\cdots,nx_n,0,\cdots)
$$

$\exp2.$以

$$
A(f):=f(x_0)
$$

定义泛函$A:C([a,b],\mathbb{R})\rightarrow \mathbb{R}$,其中$f\in C([a,b],\mathbb{R})$，而$x_0$是$[a,b]$中的一个固定点

$\exp3.$以

$$
A(f):=\int_a^bf(x)dx
$$

定义泛函$A:=C([a,b],\mathbb{R})\rightarrow \mathbb{R}$

这也是定积分的定义

$\exp4.$以

$$
A(f):=\int_a^xf(t)dt
$$

定义变换$A:C([a,b],\mathbb{R})\rightarrow A:C([a,b],\mathbb{R})$，其中$x$取闭区间$[a,b]$上的全部值

$\exp5.$普遍乘积

$$
A(x_1,\cdots,x_n)=x_1x_2\cdots x_n
$$

$A\in \mathcal{L}(\mathbb{R},\cdots,\mathbb{R};\mathbb{R})$是一个$n$重线性函数

$\exp6.$ $\mathbb{R}$上的欧几里得空间中，标量积是一个双线性函数

$\exp7.$三维欧几里得空间$E^3$中，向量的向量积$A\in \mathcal{L}(E^3,E^3,E^3)$

$\exp8.$ $\det:M_n(\mathbb{R})\rightarrow \mathbb{R}$是一个$n$重线性函数

在之后我们会计算它们的范数

## 算子的范数

$Def.$设$A:X_1\times X_2\times\cdots\times X_n\rightarrow Y$是从赋范空间$X_1,\cdots,X_n$的直积到赋范空间$Y$的多重线性算子，量

$$
\|A\|:=\sup_{x_1,\cdots,x_n}\frac{|A(x_1,\cdots,x_n)_Y}{|x_1|_{X_1}\cdots|x_n|_{X_n}}
$$

称为**多重线性算子的范数**，式中的上确界曲子$X_1,\cdots,X_n$中的所有可能的非零向量组$x_1,\cdots,x_n$

利用多重线性性，我们可以得到这个范数的其它形式

$$
\|A\| = \sup_{\begin{matrix}
x_1, \cdots, x_n \\
x_i \neq 0
\end{matrix}} \left| A \left( \frac{x_1}{\|x_1\|}, \cdots, \frac{x_n}{\|x_n\|} \right) \right| = \sup_{\|e_1\|=\cdots=\|e_n\|=1} |A(e_1, \cdots, e_n)|,
$$

对于线性算子，我们有

$$
\|A\|=\sup_{x\not=0}\frac{|Ax|}{|x|}=\sup_{|e|=1}|Ae|
$$

由定义可得,当不是无穷维时

$$
\begin{align*}
|A(x_1,\cdots,x_n)| \leqslant \|A\| \, \|x_1\|\cdots \|x_n\|.
\end{align*}
$$
