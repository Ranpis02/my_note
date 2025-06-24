# 第一章 常见分布

## 1.1 一些常用的抽样分布

- 正态分布：设 $X_1, X_2, \ldots, X_n$ 是独立同分布的随机变量，且每个随机变量都服从正态分布 $N(\mu, \sigma^2)$，则样本均值 $\overline{X}=\frac{1}{n}\sum_{i=1}^nx_i$ 服从正态分布 $N(\mu, \frac{\sigma^2}{n})$，那么对于 $\overline{X}$ 而言，其:
  - 均值 $\mu=\frac{1}{n}\sum_{i=1}^n\mu_i$
  - 方差 $\frac{\sigma^2}{n}$，其中 $\sigma^2=\sum^n_{i=1}\sigma_i^2$
- 卡方分布：设 $X_1, X_2, \ldots, X_n$ 是独立同分布的随机变量，而每个随机变量都服从标准正态分布 $N(0, 1)$，则随机变量 $X^2=X_1^2+X_2^2+\ldots+X_n^2$ 服从自由度为 $n$ 的 $\chi^2$ 分布，记作 $\chi^2(n)$，性质为 $E\chi^2=n, D\chi^2=2n$
- t 分布：设随机变量 $X$ 服从标准正态分布 $N(0, 1)$， $Y$ 服从自由度为 $n$ 的 $\chi^2$ 分布，则 $T=\frac{X}{\sqrt{Y/n}} \sim t(n)$
- F 分布：设 $X$ 和 $Y$ 分别服从自由度为 $n_1, n_2$ 的 $\chi^2$ 分布，且 $X$ 和 $Y$ 相互独立，则 $F=\frac{X/n_1}{Y/n_2} \sim F(n_1, n_2) \Rightarrow \frac{1}{F} \sim F(n_2, n_1)$



## 1.2 经验分布函数

经验分布函数（Empirical Distribution Function, EDF） ：经验分布函数 $F_n(x)$ 是基于样本 $X_1, X_2, \ldots, X_n$ 构建的函数，表示样本中小于或等于 $x$ 的观测值的比例：
$$
F_n(x)=\frac{1}{n}\sum^n_{i=1}I(X_i \leq x)
$$
其中，$I(X_i \leq x)$ 为指示函数。



# 第二章 参数估计

## 2.1 数学期望

求数学期望有两种方式:

1. $X$ 是离散型随机变量，其期望 $E(x)$ 由所有可能取值的加权和计算：
   $$
   E(x)=\sum^n_{i=1}x_ip_i
   $$
   其中，$x_i$ 是随机变量 $X$ 的可能取值，$p_i$ 是对应的概率，$\sum{p_i}=1$

2. $X$ 为连续型随机变量，其概率密度函数为 $f(x)$，那么期望可以通过如下公式计算：
   $$
   E(X)=\int^{\infty}_{-\infty}xf(x)dx
   $$
   

期望也可以通过其性质求得，例如若 $X$ 和 $Y$ 是随机变量，$a,b$ 为常数，则：
$$
E(aX+bY)=aE(X)+bE(Y)
$$
