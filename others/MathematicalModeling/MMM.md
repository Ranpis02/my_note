# 1 层次分析模型

我们举例如下：

![image-20230217235022354](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230217235022354.png)

上表为 $5 \times 5$ 的方针，我们即为 A，对应元素为 $a_{ij}$

该方针具有以下特点：

1. $a_{ij}$  表示的意义为，与指标 $j$ 相比相比，$i$ 的重要度，即:
   $$
   a_{ij} =  \frac{i 的重要程度}{j 的重要程度}
   $$

2. 当 $i=j$ 时，两个指标相同，因此同等重要记为 1

3. $a_{ij}>0$ 且满足 $a_{ij} \times a_{ji}=1$（我们称这一矩阵为正互反矩阵）

实际上，上面的这个矩阵就是层次分析法中的<strong style="color:red">判断矩阵</strong>



**一致矩阵**

若矩阵中的每个元素 $a_{ij}>0$，且满足 $a_{ij} \times a_{ji}=1$ ，则我们称该矩阵为 <font color="skyblue">正互反矩阵</font>

在层次分析法中，我们构造的判断矩阵均是正互反矩阵。

若正互反矩阵满足 $a_{ij} \times a_{jk}=a_{ik}$ ，则我们称其为<font color="skyblue">一致矩阵</font>

<font color="red">注意：在使用判断矩阵求权重时，必须对其进行一致性校验</font>

