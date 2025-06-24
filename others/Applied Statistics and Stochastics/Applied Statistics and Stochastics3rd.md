# 1 两个随机变量的分布

## 1.1 两个离散型随机变量的分布

![image-20250617150713415](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250617150713415.png)



## 1.2 两个连续型随机变量的分布

:memo:例1：假设 $(X, Y)$ 的密度函数为
$$
f(x, y)=\left\{\begin{array}{cc}
\frac{1}{2} e^{-\frac{y}{2}}, & 0<x<1, \quad y>0 \\
0, & \text { else }
\end{array}\right.
$$
求 $Z=X-Y$ 的密度函数 $f_Z(z)$



# 2 随机变量的数字特征

## 2.1 数学期望

1. 离散型随机变量的数学期望为：$\sum_{k=1}^{\infty}x_kP_k$。对于二维随机变量 $Z=g(x, y)$，则 $E(g(x, y))=\sum_{i=1}^{\infty}\sum_{j=1}^{\infty}g(x_i, y_j)P_{ij}$

2. 连续性随机变量的数学期望为：设随机变量 $X$ 的密度函数为 $f_X(x)$，则称 $E(x)=\int_{-\infty}^{+\infty}xf_X(x)dx$ 为 $X$ 的数学期望

   > [!note]
   >
   > 即连续随机变量的期望=取值 $\times$ 密度函数，再进行积分

   - 若 $Y=g(x)， x \sim f_X(x)，则 $ $E(g(x))=\int_{-\infty}^{+\infty}g(x)f_X(x)dx$ 为 $Y=g(x)$ 的数学期望
   - 若 $Z=g(X, Y)$， $(x, y) \sim f(x, y)$，则 $E(g(x, y))=\int_{-\infty}^{+\infty}\int_{-\infty}^{+\infty} g(x, y)f(x, y)dxdy$ 

   

![image-20250618013652801](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250618013652801.png)



数学期望的性质：

1. 假设 $C$ 为常数，则 $E(C)=C$
2. $E(CX)=CE(X)$
3. 若 $X$, $Y$ 分别为两个随机变量，那么 $E(X+Y)=E(X)+E(Y)$

4. 若 $X$ 和 $Y$ 相互独立，则 $E(XY)=E(X)E(Y)$



## 2.2 方差

假设 $X$ 为随机变量，若 $E[(X-E[X])^2]$ 存在，则称其为方差，记为 $D(X)$，即 $D(X)=E[(X-E[X])^2]$

> [!note]
>
> 1. $D(X)$ 的含义：表示 $X$ 与 $E(X)$ 的偏离程度
> 2. $D(X) \geq 0$

计算公式：$D(X)=E(X^2)-E^2(X)$



方差的性质：

1. 设 $C$ 为常数，$D(C)=0$
2. $D(CX)=C^2D(X)$
3. $D(X+Y)=D(X)+D(Y)+2[E(XY)-E(X)E(Y)]$

4. 若 $D(X)=0$，则 $P(X=E(X))=1$



![image-20250618032558276](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250618032558276.png)



## 2.3 常见分布的期望和方差

| 常见分布 | 期望                | 方差                  |
| -------- | ------------------- | --------------------- |
| 0-1 分布 | p                   | $p(1-p)$              |
| 二项分布 | np                  | np(1-p)               |
| 泊松分布 | $\lambda$           | $\lambda$             |
| 均匀分布 | $\frac{a+b}{2}$     | $\frac{(b-a)^2}{12}$  |
| 指数分布 | $\frac{1}{\lambda}$ | $\frac{1}{\lambda^2}$ |
| 正态分布 | $\mu$               | $\sigma^2$            |

> [!note]
>
> - $a^3+b^3=(a+b)(a^2-ab+b^2)$
> - $a^3-b^3=(a-b)(a^2+ab+b^2)$



# 3 协方差和相关系数

协方差：设 $(X, Y)$ 为二维随机变量，称 $E[(X-E(X))][(Y-E(Y))]$ 为 $X$ 与 $Y$ 的协方差，记为 $COV(X, Y)$，即 $COV(X, Y)=E[(X-E(X)][Y-E(Y)]$

> [!note]
> $$
> COV(X, X)=D(X)
> $$

重要公式：$COV(X, Y)=E[XY]-E[X]E[Y]$

> [!note]
>
> 原来的公式我们就可以修改为 $D(X+Y)=D(x)+D(Y)+2COV(X, Y)$



协方差的性质：

1. $COV(X, Y)=COV(Y, X)$
2. $COV(aX, bY)=ab\ COV(X, Y)$
3. $COV(X+Y, Z)=COV(X, Z)+COV(Y, Z)$
4. 若 $X$ 与 $Y$ 相互独立，则 $COV(X, Y)=0$



**相关系数**
$$
\rho_{XY}=\frac{COV(X, Y)}{\sqrt{D(X)}\sqrt{D(Y)}}
$$
为 $X$ 与 $Y$ 的相关系数。

> [!important]
>
> 相关系数 $\rho_{XY}$ 刻画了 $X$ 与 $Y$ 的线性相关程度，$\rho$ 的范围为 $[-1, 1]$，换言之，$\rho^2 \leq 1$，越靠近 -1 或 1 代表线性相关性越强，越靠近 0 代表线性相关性越弱



**相关系数的性质**

1. $\rho_{XY}=0$：$X$ 与 $Y$ 不相关
2. $\rho_{XY}=1$: $X$ 与 $Y$ 正相关，即 $Y=aX+b, a> 0$
3. $\rho_{XY}=-1$：$X$ 与 $Y$ 负相关，即 $Y=aX+b, a < 0$ 

> [!warning]
>
> $X$ 与 $Y$ 独立 => $X$ 与 $Y$ 不相关（即 $\rho=0$ 不存在线性关系），但是反过来则不成立，因为不存在线性关系，也有可能是幂级数关系或指数关系



![image-20250619205638818](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250619205638818.png)



:memo:例2：设 $X \sim N(0, 1), Y=X^2$，求 $\rho_{XY}$



#  4 大数定律

切比雪夫不等式：假设随机变量的数学期望为 $E(X)$, 方差为 $D(X)$，则对于任意正数 $\varepsilon$，有：
$$
P(|X-E(X)| \geq \varepsilon) \leq \frac{D(X)}{\varepsilon^2}
$$

> [!note]
>
> 切比雪夫不等式表明：$D(X)$ 越小，事件 $|X-E(X)| \geq \varepsilon$ 发生的概率越小，即事件 $|X-E(X)| < \varepsilon$ 发生的概率越大，即 $X$ 的取值越集中在  $E(X)$ 附近



:memo:例1：设 $E(X)=-2, E(Y)=2, D(X)=1, D(Y)=4, \rho_{XY}=-\frac{1}{2}$，试估计 $P(|X+Y| \geq 6)$



切比雪夫大数定律：设 $X_1, X_2, \ldots, X_n$ 是相互独立的随机变量序列，且 $E(X_i)，D(X_i)$ 均存在，且 $D(X_i) \leq C$（即方差有一个上界），记 $\bar{X}=\frac{1}{n}\sum_{i=1}^nX_i$ ，则对于任意正数 $\varepsilon$，有：
$$
\lim_{n \rightarrow \infty}P(|\bar{X}-E(\bar{X})| < \varepsilon) =1
$$


# 5 中心极限定理

基本思想：在一定条件下，即时原来并不服从正态分布的一些独立随机变量的和的分布，当随机变量的个数充分大时，也近似服从正态分布。







# 6 数理统计的基本概念

**总体 vs 个体**

1. 总体：研究对象的某项数量指标的全体称为总体，记为 $X$
2. 个体：组成总体的每个基本单位称为个体

> [!note]
>
> 总体又可以被分为有限总体和无限总体



**样本**：从总体中抽取的部分个体称为样本，记为 $X_1, X_2, \ldots, X_n$。若满足：

1. $X_1, X_2, \ldots, X_n$ 相互独立
2. $X_1, X_2, \ldots, X_n$ 与总体 $X$ 同分布

那么我们就称 $X_1, X_2, \ldots, X_n$ 为来自总体 $X$ 的简单随机样本，可简称为样本，其中 $n$ 为样本容器，$X_1, X_2, \ldots, X_n$ 为样本观测值，简称为样本值



**样本分布函数：** 设总体 $X$ 的分布函数为 $F(x)$， $x_1, x_2, \ldots, x_n$ 是来自总体 $X$ 的一个样本，$x_1, x_2, \ldots, x_n$ 为样本值，把样本值由小到大进行排列，如下：
$$
x_{(1)} \leq x_{(2)} \leq \ldots \leq x_{(n)}
$$
定义一个函数：对于任意实数 $x$，$F_n(x)=\frac{样本中小于 x 的个数}{n}$ ，该函数被称为总体 $X$ 的经验分布函数（或样本分布函数）

![image-20250620005653781](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250620005653781.png)

> [!warning]
>
> 注意：当 $n$ 充分大时，$F_n(x)=F(x)$



**统计量**： 设 $X_1, X_2, \ldots, X_n$ 是总体 $X$ 的一个样本，样本函数 $g(X_1, X_2, \ldots, X_n)$ 中不含有任何未知参数，称 $g(X_1, X_2, \ldots, X_n)$ 为一个统计量



**常用统计量：**

设总体 $X$ 的 $E(X)=\mu, D(X)=\sigma^2$

1. 样本均值：$\bar{X}=\frac{1}{n}\sum_{i=1}^{n}X_i$

   - $E(\bar{X})=\mu$
   - $D(\bar{X})=\frac{\sigma^2}{n}$

2. 样本方差:

   - $S^2=\frac{1}{n-1}\sum_{i=1}^n(X_I-\bar{X})^2$

   > [!note]
   >
   > $S^2=\frac{1}{n-1}\sum(x_i-\bar{x})^2=\frac{1}{n-1}(\sum{x_i^2-n\bar{x}^2)}$

3. 总体样本方差的期望等于总体样本的方差：$E(S^2)=D(X)=\sigma^2$

4. 样本标准差： $S=\sqrt{S^2}=\sqrt{\frac{1}{n-1}\sum{(X_i-\bar{X})^2}}$

5. 样本 $k$ 阶原点矩：$A_k=\frac{1}{n}\sum{X_i^k}$

6. 样本 $k$ 阶中心矩：$B_k=\frac{1}{n}\sum{(X_i - \bar{X})^k}$



# 7 抽样分布

1. 抽样分布：统计量 $g(x_1, x_2, \ldots, x_n)$ 的分布被称为抽样分布



## 7.1 $\chi^2$ 分布

定义：设 $X_1,X_2, \ldots, X_n$ 相互独立，且均服从 $N(0, 1)$，则有 $X=X_1^2+X^2+\ldots +X_n^2$ 服从自由度为 $n$ 的 $\chi^2$ 分布，记为 $X\sim \chi^2(n)$

> [!note]
>
> $\chi^2$ 分布三要素：独立、标准正态、平方和



$\chi^2$ 分布的密度函数

![image-20250620104247502](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250620104247502.png)



$\chi^2$ **分布的性质**

1. 可加性：若 $X \sim \chi^2(n_1), Y \sim \chi^2(n_2)$，并且 $X$ 和 $Y$ 相互独立，那么 $X+Y \sim \chi^2(n_1 +n_2)$

2. 若 $X \sim \chi^2(n)$，则 $E(X)=n$，$D(X)=2n$

3. $\chi^2$ 分布的上 $\alpha$ 分位点：设 $X \sim \chi^2(n)$，其密度函数图像为：

   <img src="https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250620140049366.png" alt="image-20250620140049366" style="zoom:50%;" /> 

   这里我们称 $\alpha$ 为 $\chi^2$ 分布的上分位点，即 $P(X > \chi_{\alpha}^2(n)=\alpha)$



## 7.2 $t$ 分布

若 $X_1 \sim N(0, 1), X_2 \sim \chi^2(n)$， 并且 $X_1$ 与 $X_2$ 相互独立，则 $X=\frac{X_1}{\sqrt{X_2/n}}$ 服从自由度为 $n$ 的 $t$ 分布，即 $X \sim t(n)$

若 $X \sim t(n)$，则 $X$ 的密度函数图像类似于标准正态分布 $N(0, 1)$

![image-20250620144327637](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250620144327637.png)

> [!warning]
>
> 若 $X \sim t(n)$ ，则 $E(X)=0, D(X)=\frac{n}{n-2}$

同样地，$t$ 分布也有上 $\alpha$ 分位点
$$
P(X > t_{\alpha}(n))=\alpha
$$
由于 $t$ 分布关于 $y$ 轴对称，因此:

1. $-t_{\alpha}(n)=t_{1-\alpha}(n)$

 

## 7.3 $F$ 分布

定义：若 $X_1 \sim \chi^2(n_1), X_2 \sim \chi^2(n_2)$，并且 $X_1, X_2$ 相互独立，那么 $\frac{X_1/n_1}{X_2/n_2}$ 服从自由度为 $(n_1, n_2)$ 的 $F$ 分布，记为 $F(n_1, n_2)$



性质：

1. 若 $X \sim F(n_1, n_2)$，则 $\frac{1}{x} \sim F(n_2, n_1)$

2. 若 $X \sim F(n_1, n_2)$，则其概率密度函数图像类似于卡方分布。如下：

   ![image-20250620150836482](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250620150836482.png)

   ![image-20250620150953983](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250620150953983.png)



> [!warning]
>
> $F_{\alpha}(m,n =\frac{1}{F_{1-\alpha}(n,m )})$

![image-20250620160923094](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250620160923094.png)



## 7.4 正态分布的一些结论

正态分布总体的样本均值和样本方差的分布

1. 设总体 $X \sim N(\mu, \sigma^2)$, $X_1, X_2, \ldots, X_n$ 为总体 $X$ 的一个样本，则：
   - $\bar{X} \sim N(\mu, \frac{\sigma^2}{n})$
   - $\frac{(n-1)S^2}{\sigma^2} \sim \chi^2(n-1)$
   - $\bar{X}$ 与 $S^2$ 独立

2. 设总体 $X \sim N(\mu, \sigma^2)$， $X_1, X_2, \ldots, X_n$ 是 $X$ 的样本，则 :
   $$
   \frac{\bar{X}-\mu}{S/\sqrt{n}} \sim t(n-1)
   $$

<img src="https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250620174144040.png" alt="image-20250620174144040" style="zoom:80%;" />

![image-20250620174758350](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250620174758350.png)





# 8 参数估计

## 8.1 点估计

定义：设总体 $X$ 的分布形式已知，但含有未知参数 $\theta$，用样本 $X_1, X_2, \ldots, X_n$ 构造统计量 $\hat{\theta}(X_1, X_2, \ldots, X_n)$ 来估计未知参数 $\theta$，称 $\hat{\theta}$ 为点估计量， $\hat{\theta}(x_1, x_2, \ldots, x_n)$ 为点估计值

> [!tip]
>
> 例如，$X \sim U(0, \theta)$， $\theta$ 值未知，$X_1, X_2, \ldots, X_n$ 为样本

![image-20250620203002076](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250620203002076.png)

> [!note]
>
> 其中 $E(X)$ 为总体矩，$\bar{X}$ 为样本矩



总结：**点估计是指用一个样本统计量来估计总体分布的某个参数**

常用的点估计有：

- 最大似然估计（MLE, Maximum Likelihood Estimation）
- 矩估计（Method of Moments）
- 贝叶斯估计（Bayesian Estimation）



## 8.2 矩估计

基本思想：用样本矩 $A_k=\frac{\sum{X_i^k}}{n}$ 代替总体矩 $u_k=E(X^k)$



求解矩估计的基本步骤：

1. 先求总体矩：$E(X)$
2. 再令 $\bar{X}=E(X)$，解的的 $\hat{\theta}$ 即为据矩估计量



![image-20250620212207119](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250620212207119.png)





## 8.3 最大似然估计

基本思想：使得当前样本 $x_1, x_2, \dots, x_n$ 出现的概率最大的参数值 $\hat{\theta}$ 即为最大似然估计

似然函数：一种刻画样本  $x_1, x_2, \dots, x_n$ 出现的概率大小的函数

离散型：
$$
L(x_1, x_2, \dots, x_n; \theta) = P(X_1 = x_1, X_2 = x_2, \dots, X_n = x_n) = P(X = x_1) \cdot P(X = x_2) \cdots P(X = x_n)
$$
连续型：
$$
L(x_1, x_2, \dots, x_n; \theta) = f(x_1; \theta) \cdot f(x_2; \theta) \cdots f(x_n; \theta)
$$
解题步骤：

1. 写出样本似然函数 $L(\theta)$
2. 取对数 $ln{L(\theta)}$
3. 求导数，得驻点： $\frac{dlnL(\theta)}{d\theta}=0$，得到的驻点  $\hat{\theta}$ 即为最大似然估计

![image-20250620220811848](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250620220811848.png)



![image-20250620222411956](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250620222411956.png)

> [!warning]
>
> 注意：小写的 $\bar{x}$ 为估计值，大写的 $\bar{X}$ 为估计量，由于最后求的时估计量，应该需要大写

## 8.4 估计量评选标准

估计量的评选标准如下：

1. 无偏性：设 $X_1, X_2, \ldots, X_n$ 时来自总体 $X$ 的一个样本，$\hat{\theta}=\hat{\theta}(x_1, x_2, \ldots,x_n)$ 为未知参数 $\theta$ 的估计量，若 $E(\hat{\theta})=\theta$, 则称 $\hat{\theta}$ 为 $\theta$ 的无偏估计量，若 $\lim_{n \rightarrow \infty}E(\hat{\theta})=\theta$，则称 $\hat{\theta}$ 为 $\theta$ 的渐进无偏估计量

![image-20250620230321898](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250620230321898.png)

> [!note]
>
> 总结：$aX_1 + bX_2$ 是 $\mu$ 的无偏估计 $\Leftrightarrow$ $a+b=1$

![image-20250620232428948](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250620232428948.png)



2. 有效性：设 $\hat{\theta_1}=\hat{\theta_1}(X_1, X_2, \ldots, X_n)，\hat{\theta_2}=\hat{\theta_2}(X_1, X_2, \ldots, X_n)$ 均为未知参数 $\theta$ 的无偏估计量，若 $D(\hat{\theta_1}) \leq D(\hat{\theta_2})$，则称 $\hat{\theta_1}$ 比 $\hat{\theta_2}$ 更有效

![image-20250620234021444](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250620234021444.png)



3. 一致性（相和性）：设 $\hat{\theta_n}=\hat{\theta}(X_1, X_2, \ldots, X_n)$ 为未知参数 $\theta$ 的估计量，若对于任意正数  $\varepsilon$ ，有 $\lim_{n \rightarrow \infty}P(|\hat{\theta_n-\theta}| < \varepsilon)=1$ ，则称 $\hat{\theta_n}$ 为 $\theta$ 的一致性估计

   > [!note]
   >
   > 若 $E(\hat{\theta}) \rightarrow \theta$ 且 $D(\hat{\theta}) \rightarrow 0$, 则 $\hat{\theta_n}$ 为 $\theta$ 的一致性估计

![image-20250621000656700](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250621000656700.png)



## 8.5 区间估计

置信区间：设总体 $X$ 的分布中含有未知参数 $\theta$，$X_1, X_2, \ldots, X_n$ 为来自 $X$ 的样本，若对于给定 $\alpha$， 存在 $\hat{\theta_1}$ 和 $\hat{\theta_2}$，使得：$P(\hat{\theta_1} < \theta <  \hat{\theta_2}) = 1- \alpha$ ，则称 $(\hat{\theta_1}, \hat{\theta_2})$ 为 $\theta$ 的置信区间，$1-\alpha$ 为置信水平或置信度。

> [!note]
>
> 1. 置信区间的含义：$(\hat{\theta_1}, \hat{\theta_2})$ 包含真值 $\theta$ 的概率为 $1-\alpha$ 
> 2. 单侧置信区间
>    - 若 $P(\theta < \hat{\theta_2})=1-\alpha$，则 $(-\infty , \hat{\theta_2})$ 为 $\theta$ 的单侧置信区间
>    - 若 $P(\theta \geq \hat{\theta_1})=1-\alpha$，则 $(\hat{\theta_1}, +\infty)$ 为 $\theta$ 的单侧置信区间

 

![image-20250621012908742](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250621012908742.png)



求解置信区间的步骤：

![image-20250621013020465](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250621013020465.png)



![image-20250621013611751](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250621013611751.png)



![image-20250621022446399](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250621022446399.png)

![image-20250621023508658](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250621023508658.png)

![image-20250621024245325](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250621024245325.png)

![image-20250621025017326](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250621025017326.png)

![image-20250621025828871](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250621025828871.png)



# 9 假设检验

正态总体下均值 $\mu$ 的假设检验（U 检验， T 检验）

![image-20250621031736020](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250621031736020.png)



![image-20250621032546389](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250621032546389.png)

![image-20250621033659020](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250621033659020.png)

> [!tip]
>
> $\alpha$ 为显著性水平，表示愿意接受错误的概率



# 9 一元线性回归分析

估计

 
