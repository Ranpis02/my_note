# 1 连续型随机变量

## 1.1 连续随机变量及其密度函数

1. 定义：设随机变量 $X$ 的分布函数为 $F(x)$，如果存在一个函数 $f(x) \geq 0$，使得 $F(x)=\int_{-\infty}^x f(t)dt$，则称 $X$ 为连续型 R.V.，$f(x)$ 为 $X$ 的**概率密度函数**，简称为密度函数

> [!tip]
>
> 这里的 `R.V.` 为 "Random Variable" 的缩写，代表随机变量

:pen:备注

1. 连续型随机变量 $X$ 的分布函数 $F(x)$ 是连续函数
2. 若 $f(x)$ 在 $x$ 点连续，则 $F^{\prime}(x)=(\int_{-\infty}^x f(t)dt)^{\prime}=f(x)$
3. $F(x)=\int_{-\infty}^x f(t)dt$ 的几何表示

<img src="https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250611164952437.png" alt="image-20250611164952437" style="zoom: 33%;" />

对于连续型随机变量 $X$，其概率密度函数 $f(x)$ 的性质如下：

1. 非负性（Non-negatvity）：$f(x) \geq 0$

2. 归一性（Normalization）：$\int_{-\infty}^{+\infty}f(x)dx=1$

3. 单点概率为 0， 即 $P(x=a)=0$

   > [!tip]
   >
   > 推导解析：随机变量 $X$ 的为连续性随机变量
   > $$
   > P(x=a)=P(X \leq a) - P(X < a)=F(a)-\lim_{x \to a^-}F(x)=F(a)-F(a)=0
   > $$

4. 区间概率：$P(a < x < b)=p(a \leq x < b)=P(a < x \leq b)=P(a \leq x \leq b)=\int_a^b f(x)dx$ 

   > [!tip]
   > $$
   > P(a < x \leq b)=F(b)-F(a)=\int_{-\infty}^{b}f(x)-\int_{-\infty}^af(x)=\int_{-\infty}^{b}f(x)+\int_{a}^{-\infty}f(x)
   > $$
   > 在根据区间积分的可加性，我们可以得到原式=$\int_a^b{f(x)dx}$

5. 



:bell:连续型随机变量 vs 离散型随机变量

|                     | 连续型随机变量                                 | 离散型随机变量 |
| ------------------- | ---------------------------------------------- | -------------- |
| 概率分布函数（CDF） | 连续                                           | 仅右连续       |
| 概率密度函数（PDF） | 可能连续（正态分布），也可能不连续（均匀分布） |                |
| 概率质量函数（PMF） |                                                | 不连续         |



:memo:例1：设连续性随机变量 $X$ 的 $f(x)=ae^{-|x|}. -\infty<x<+\infty$，现在求：

（1）求 $a$

（2）求 $F(x)$

解析：

（1）$a=\frac{1}{2}$

（2）



:memo:例2：设连续型随机变量 $X$ 的 $F(x)=a+b\ arctanx$, $-\infty < x < +\infty$

（1）求 $a, b$

（2）求 $f(x)$

（3）求 $P(x^2 \geq 1)$



## 1.2 均匀分布、指数分布

**均匀分布**：随机变量 $X$ 在区间 $[a, b]$ 上服从连续均匀分布，记为 $X \sim U(a, b)$，其概率密度函数为：
$$
f(x)= \begin{cases}\frac{1}{b-a}, & a \leq x \leq b \\ 0, & \text { otherwise }\end{cases}
$$
累积分布函数为：
$$
F(x)= \begin{cases}0, & x<a \\ \frac{x-a}{b-a}, & a \leq x < b \\ 1, & x \geq b\end{cases}
$$
均匀分布的性质：

1. 性质 1： 若 $X \sim U(a, b)$，则 $P(c < X < d)=\frac{d-c}{b-a}$

   > [!note]
   > $$
   > P(c < X < d)=\int_c^d f(x)dx=\int_c^d \frac{1}{b-a}dx=\frac{d-c}{b-a}
   > $$





---

指数分布：若随机变量 $X$ 的概率密度函数如下：
$$
f(x)= \begin{cases}\lambda e^{-\lambda x}, & x>0 \\ 0, & x \leqslant 0\end{cases}
$$
，其中 $\lambda > 0$，则称 $X$ 服从指数分布，记为 $X \sim e(\lambda)$

指数分布的累积分布函数为：
$$
F(x)=\left\{\begin{array}{cl}
1-e^{-\lambda x}, & x>0 \\
0, & x \leqslant 0
\end{array}\right.
$$
![image-20250614010623347](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250614010623347.png)

指数分布的性质如下：

1. 无记忆性：若 $X \sim e(\lambda)$，则 $X$ 具有无记忆性：$P(X > t_0 + T | X > t_0)=P(X > T)$



:memo:例1：设 $X \sim U(0, 5)$，则关于 $t$ 的一元二次方程 $4t^2+4xt+x+2=0$ 有实根的概率为？

解析：判断一个方程是否有实根，只需要判断 $\Delta \geq 0$，即 $(4x)^2-16(x+2) \geq 0$ 

因此，$x \in (-\infty, -1) \cup [2, +\infty)$

有实根的概率为 $\frac{3}{5}$



:memo:例2：设 $X \sim e(\lambda)$， 则 $P(-2 < X < 4)$ 为多少？

$P(-2 < X < 4) =F(4)-F(-2)=1-e^{-4\lambda}$



## 1.3 正态分布

若 $X$ 的概率密度函数 $f(x)=\frac{1}{\sqrt{2\pi}\sigma}e^{-\frac{(x-\mu)^2}{2\sigma^2}}$，其中 $-\infty < x < +\infty$，则称 $X$ 服从正态分布，记作 $X \sim N(\mu, \sigma^2)$

- $\mu$：位置参数，决定对称轴所在的位置
- $\sigma$：形状参数，空间 PDF 的陡峭程度，$\sigma$ 越大，PDF 越平缓；$\sigma$ 越小，PDF 越陡峭

正态分布（Normal Distribution）也被称为高斯分布（Gaussian Distribution），其性质如下：

1. 若 $P(X \leq \mu)=\frac{1}{2}$
2. $\int_{-\infty}^{+\infty}e^{-\frac{x^2}{A}}=\sqrt{A\pi}$，其中 $A > 0$

3. 当 $\mu =0, \sigma=1$ 时，即 $X \sim N(0, 1)$， 我们就可以称为该分布为标准正态分布，概率密度函数我们记为 $\varphi(x)$
   $$
   \varphi(x)=\frac{1}{\sqrt{2\pi}}exp(-\frac{x^2}{2})
   $$

4. 对于标准正态分布，其分布函数 $\Phi(x)=\int_{-\infty}^{+\infty} \varphi(x)dx$， 其中 $\Phi(0)=\frac{1}{2}$

5. 若 $X \sim N(0, 1)$，则 $\Phi(-x)=1-\Phi(x)$
6. 标准化：假设 $X \sim N(\mu, \sigma^2)$，则 $\frac{X-\mu}{\sigma} \sim N(0, 1)$
7. 若 $X \sim N(\mu, \sigma^2)$， 则 $P(a < X < b) \iff P(\frac{a-\mu}{\sigma} < \frac{X-\mu}{\sigma} < \frac{b-\mu}{\sigma})$



:sailboat:补充内容：上分位点（upper quantile）和下分位点（lower quantile）

- 上分位点：是指使得右尾概率为 $\alpha$ 的点，即 $P(X > \mu_{\alpha})=\alpha$
- 下分位点：是指使得左尾概率为 $\alpha$ 的点，即 $P(X \leq \mu_{\alpha})=\alpha$

> [!tip]
>
> 在标准正态分布中，根据对称性，我们可以得到 $\mu_{\alpha}=-\mu_{1-\alpha}$



:memo:例1：假设 R.V. $X \sim N(\mu, \sigma^2)$，则随着 $\sigma$ 的增大，概率 $P(|x-\mu|<\sigma)$ 是（）

（A）单调递增

（B）单调递减

（C）保持不变

（D）不确定

解析：$P(|X-\mu|<\sigma) \Rightarrow P(-\sigma<X-\mu<\sigma) \Rightarrow P(-1 <\frac{X-\mu}{\sigma}<1)=\Phi(1)-(1-\Phi(1))=2\Phi(1)-1$

因此，概率 $P$ 是保持不变的，选 $C$







# 2 随机变量函数的分布

## 2.1 离散型变量函数的分布

:memo:例1：设 $X$ 的分布律

| X    | -1   | 0    | 1    | 2    |
| ---- | ---- | ---- | ---- | ---- |
| P    | 0.1  | 0.2  | 0.3  | 0.4  |

求：

（1）$Y=2X-1$ 的分布律

（2）$Y=X^2$ 的分布律



## 2.2 连续性随机变量函数的密度函数

:memo:例1：已知 $X \sim f_X(x)$，其中
$$
f_X(x)= \begin{cases}\frac{x}{8}, & 0<x<4 \\ 0, & \text { otherwise }\end{cases}
$$


求 $Y=2X+8$ 的密度函数 $f_Y(y)$

解析：



> [!note]
>
> 定理：假设随机变量 $X$ 的密度函数为 $f_X(y)$，函数 $Y=g(x)$，且 $y=g(x)$ 为单调函数，则 $Y=g(x)$ 的密度函数 $f_Y(y)$ 为：
> $$
> f_Y(y)=\left\{\begin{array}{cc}
> f_X(h(y)) \mid h^{\prime}(y) \mid, & \alpha<y<\beta \\
> 0, & \text { otherwise }
> \end{array}\right.
> $$
> 其中 $h(y)$ 为 $y=g(x)$ 的反函数， $\alpha$ 为 $g(x)$ 的最小值，$\beta$ 为 $g(x)$ 的最大值



:memo:例2：假设随机变量 $X$ 在区间 $(0, 1)$ 上服从均匀分布，求 $Y=-2lnx$ 的概率密度函数



:memo:例3：假设 $X \sim U(0, 2)$，求 $Y=X^2$ 的密度函数 $f_Y(y)$

解析：虽然 $y=x^2$ 并不是一个单调函数，由于 $X$ 的有效区间为 $(0, 2)$，而 $Y$ 在 $X$ 的有效区间是满足单调递增的，因此，我们仍然可以套用前面的公式



## 2.3 二维随机变量及其分布

定义：设 $\Omega$ 为随机试验 $E$ 样本空间，$X$ 和 $Y$ 都是定义在 $\Omega$ 上的随机变量，则称 $(X, Y)$ 为二维随机变量

联合分布函数：对于任意实数 $x, y$，称 $P(X \leq x, Y \leq y)$的联合分布函数，记为 $F(x, y)$， 即 $F(x, y)=P(X \leq x, Y \leq y)$

> [!tip]
>
> $F(x, y)$ 的意思是 $X$ 小于 $x$， $Y$ 小于 $y$ 的概率



:taco:**性质**

1. $0 \leq F(x, y) \leq 1$
2. 单调不减性：$F(x, y)$ 是关于的 $x, y$ 的不减函数，即：
   - 固定 $y$, $x_1 < x_2 \Rightarrow F(x_1, y) \leq F(x_2, y)$
   - 固定 $x$，$y_1 < y_2 \Rightarrow F(x, y_1) \leq F(x, y_2)$
3. 规范性：
   - $F(-\infty, \infty)=0$
   - $F(-\infty, y)=0$
   - $F(x, -\infty)=0$
   - $F(+\infty, +\infty)=1$

4. 右连续性：$F(X, Y)$ 关于 $x$ 右连续，关于 $y$ 右连续，即 
   $$
   \lim_{\Delta x \to 0^{+}} F(x + \Delta x, y) = F(x, y), \quad \lim_{\Delta y \to 0^{+}} F(x, y + \Delta y) = F(x, y)
   $$



:memo:例1：假设随机变量 $(x, y)$ 的 $F(x, y)=A[B+arctan \frac{x}{2}][c+arctan \frac{y}{2}]$，求 $A, B, C$

解析：$B=C=\frac{\pi}{2}, A=\frac{1}{\pi^2}$



### 2.3.1 二维离散型随机变量及其取值

二维离散型随机变量：当 $(X, Y)$ 的取值为有限个或可列无限个时，称 $(X, Y)$ 为二维离散型随机变量

> [!note]
>
> 例如：一个箱子中有 3 个白球，2 个红球, 1 个黄球，从中任取一球，$X$ 表示抽取到的白球数目，$Y$ 表示抽取到的红球数目，此时 $(X, Y)$ 我们就可以称二维离散型随机变量，其中 $(X, Y)$ 可能的取值为 $(0, 1), (1, 0), (0, 0)$



联合分布律：设 $(X, Y)$ 的可能值为 $(x_i, y_j), i=1, 2, \ldots, j=1, 2, \ldots$，那么我们可以用表格的形式表示其联合分布律，如下：

| X\Y      | $y_1$    | $y_2$    | $\ldots$ | $y_j$    |
| -------- | -------- | -------- | -------- | -------- |
| $x_1$    | $P_{11}$ | $P_{12}$ | $\ldots$ | $P_{1j}$ |
| $x_2$    | $P_{21}$ | $P_{22}$ | $\ldots$ | $P_{2j}$ |
| $\ldots$ | $\ldots$ | $\ldots$ | $\ldots$ | $\ldots$ |
| $x_i$    | $P_{i1}$ | $P_{i2}$ | $\ldots$ | $P_{ij}$ |



性质：

1. 非负性：$P_{ij} \geq 0$
2. 归一性：$\sum_{i=1}^{\infty}\sum_{j=1}^{\infty} P_{ij}=1$



:memo:例1：将两封信随机地往编号为 1， 2， 3，4 的 4 个邮箱内投放，其中 $X_1$ 表示第一个邮箱内的信的数目，$X_2$ 表示第二个邮箱内信的数目，求 $(X_1, X_2)$ 的分布律



### 2.3.2 二维连续型随机变量及其分布

假设 $(X, Y)$ 的联合分布函数为 $F(x, y)$，若存在非负函数 $f(x, y) \ge 0$，使得 $F(x, y)=\int_{-\infty}^x\int_{-\infty}^y f(u, v)dudv$ 成立，则称 $(X, Y)$ 为二维连续性随机变量，其中 $F(x, y)$ 为 $(X, Y)$ 的联合密度函数。$f(x, y)$ 的性质如下：

1. 非负性：$f(x, y) \ge 0$

2. 归一性：$\int_{-\infty}^{+\infty} \int_{-\infty}^{+\infty} f(x, y)dxdy=1$

3. 若 $f(x, y)$ 在点 $(x, y)$ 处连续，则 $\frac{\partial^2(F(x, y))}{\partial x \cdot \partial y}=f(x, y)$

4. 区域概率：
   $$
   P\{(x, y) \in D\}=\iint_D f(x, y) d x d y
   $$



:memo:例1：设
$$
(x, y) \sim f(x, y)= \begin{cases}k x, & 0 \leqslant x \leqslant y \leqslant 1 \\ 0, & \text { otherwise }\end{cases}
$$
（1）求 $k$ 

（2）求 $P(X+Y \leq 1)$

解析：

（1）$k=6$

（2）$P(X+Y \leq 1)=\frac{1}{4}$



:memo:例2：设 
$$
(x, y) \sim f(x . y)=\left\{\begin{array}{cl}
2 e^{-2 x-y}, & x>0, y>0 \\
0, & \text { else }
\end{array}\right.
$$
求 $F(x, y)$

解析：

$F(x, y)=P(X \leq x. Y \leq y)=\int_{0}^x\int_{0}^y f(x, y)dxdy$
$$
F(x, y)=\left\{\begin{array}{cl}
\left(1-e^{-2 x}\right) \cdot\left(1-e^{-y}\right), & x>0, y>0 \\
0, & else
\end{array}\right.
$$


> [!note]
>
> 二维积分可拆分的条件为：积分区域为矩形，并且被积函数可以表示两个变量的乘积形式，具体而言：
>
> 1. **矩形区域**：积分的定义域必须是 $ a \leq x \leq b $ 和 $ c \leq y \leq d $ 这样的矩形。
>
> 2. **可分离函数**：如果 $ f(x, y) $ 可以写成 $ f(x, y) = g(x) \cdot h(y) $，则双重积分可以拆分为： 
>    $$
>    \iint_D f(x, y) \, dx \, dy = \left( \int_a^b g(x) \, dx \right) \cdot \left( \int_c^d h(y) \, dy \right)
>    $$





:sailboat:补充：两个常见的二维连续性分布

1. 二维均匀分布：若 $(X, Y)$ 的 
   $$
   f(x, y)=\left\{\begin{array}{cl}
   \frac{1}{S_D}, & {(x, y) \in D} \\
   0, & \text { else }
   \end{array}\right.
   $$
   



![image-20250616180209543](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250616180209543.png)	



![image-20250616180526850](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250616180526850.png)





# 3 边缘分布

边缘分布函数(Marginal Distribution Function)：设 $(X, Y)$ 的联合分布函数为 $F(x, y)$，则称 :

1. $F_x(x)=P(X \leq x)=P(X \leq x, Y < +\infty)=F(x, +\infty)$：二维随机变量 $(X, Y)$ 关于 $X$ 的边缘分布函数
2. $F_y(y)=P(Y \leq y)=P(X < +\infty,Y \leq y)=F(+\infty, y)$：二维随机变量 $(X, Y)$ 关于 $Y$ 的边缘分布函数



边缘分布律：已知 $(X, Y)$ 的联合分布律，$P(X=x_i, Y=y_j)=P_{ij}, i=1, 2, 3, \ldots$，则称 $P(X=x_i)=P(X=x_i, \Omega)=\sum_{j=1}^{\infty}P(X=x_i, Y=y_i)$ 为 $(X, Y)$ 关于 $X$ 的边缘分布律



边缘密度函数：$设 (X, Y)$ 的联合密度函数为 $f(x, y)$，则称 ：

1. $f_x(x)=\int_{-\infty}^{+\infty} f(x, y)dy$ 为 $(X, Y)$ 关于 $X$ 的边缘密度函数
2. $f_y(y)=\int_{-\infty}^{+\infty}f(x, y)dx$ 为 $(X, Y)$ 关于 $Y$ 的边缘密度函数 



:memo:例1：设 $X, Y$ 的联合密度函数为
$$
f(x, y)= \begin{cases}6, & x^2 \leqslant y \leqslant x \\ 0 & \text { else }\end{cases}
$$


，求 $f_x(x), f_y(y)$



> [!note]
>
> 反函数和原函数在图像上关于 $y=x$ 对称





# 4 条件分布

条件分布律：设离散型 $(X, Y)$ 的联合分布律为 $P(X=x_i, Y=y_j)=p_{ij}$，固定 $Y=y_j$，当 $P(Y=y_j) \not = 0$ 时，称 $P(X=x_i, Y=y_j)=\frac{P(X=x_i, Y=y_j)}{P(Y=y_j)}$ 为 $X$ 在 $Y=y_j$ 条件下的条件分布律，同理 $Y$ 在 $X=x_i$ 也是同样的道理

> [!note]
> $$
> 条件分布=\frac{联合分布}{边缘分布}
> $$

![image-20250617015745004](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250617015745004.png)



条件密度函数：设随机变量 $(X, Y)$ 的概率密度为 $f(x, y)$

（1）固定 $Y=y$ ，当 $f_Y(y) \not = 0$，称 $f_{X|Y}=\frac{f(x, y)}{f_Y(y)}$ 为 $X$ 在 $Y=y$ 条件下的条件密度

（2）固定 $X=x$，当 $f_X(x) \not =0$，称 $f_{Y|X}=\frac{f(x, y)}{f_X(x)}$ 为 $Y$ 在 $X=x$ 条件下的条件密度

> [!warning]
>
> 注意：$f_{X|Y}(x|y)=\frac{f(x, y)}{f_Y(y)}$ 是 $x$ 的一元函数

![image-20250617022433194](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250617022433194.png)

![image-20250617024112269](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250617024112269.png)



:memo:例2：设 $X$ 在 $(0, 1)$ 上服从均匀分布，在 $X=x, (0 < x <1)$ 的条件下，$Y$ 在 $(x, 1)$ 上服从均匀分布，求 $Y$ 的密度函数 $f_Y(y)$



# 5 随机变量的独立性

判断随机变量 $X$ 与 $Y$ 相互独立：假设 $F(x, y)$ 为 $(X, Y)$ 的联合分布函数，$F_x(x), F_y(y)$ 分别为 $X$ 和 $Y$ 的边缘分布函数，若 $F(x, y)=F_X(x)F_Y(y)$，则判断随机变量 $X$ 与 $Y$ 相互独立

1. 当 $(X, Y)$ 为离散型随机变量时，$X$ 与 $Y$ 独立 $\Leftrightarrow$ $P(X=x_i, Y=y_j)=P(X=x_i)P(Y=y_j)$
2. 当 $(X, Y)$ 为连续型随机变量时，$X$ 与 $Y$ 独立 $\Leftrightarrow$ $f(x, y)=f_X(x)f_Y(y)$



:memo:例1：设 $(X, Y)$ 的分布律为 

| X\Y  | 1    | 2    | 3    |
| ---- | ---- | ---- | ---- |
| 0    | 0.04 | 0.06 | 0.1  |
| 1    | 0.16 | 0.24 | 0.4  |



> [!note]
>
> $X$ 与 $Y$ 独立的特点：$X$ 与 $Y$ 的联合分布律每行均成比例



:memo:例2：假设随机变量 $X$ 与 $Y$ 相互独立，下表列出了部分数值，将剩余数值补充完整

| X\Y   | 0    | 1    | 2    | $P_i$ |
| ----- | ---- | ---- | ---- | ----- |
| 1     |      | 1/8  |      |       |
| 2     | 1/8  |      |      |       |
| $P_j$ | 1/8  |      |      |       |



$P(X=1, Y=0)=P(Y=0)-P(X=2,Y=0)=\frac{1}{8}$

再根据随机变量 $X$ 和 $Y$ 相互独立，$P(X=x_i, Y=y_i)=P(X=x_i)P(Y=y_j)$，可以得到 

$P(X=x_1)=\frac{1}{2}$，再根据变量独立时，每行成比例，可以将剩下填完



:memo:例3：假设 $(X, Y)$ 的概率密度函数为：
$$
f(x, y)=\left\{\begin{array}{cl}
2 e^{-2 x-y} & , x>0, y>0 \\
0 & , \text { else }
\end{array}\right.
$$
，请问 $X$ 和 $Y$ 相互独立吗？

解析：

只需要先求出随机变量 $X$ 和 $Y$ 的边缘分布函数，然后判断相乘是否等于其联合分布函数即可



