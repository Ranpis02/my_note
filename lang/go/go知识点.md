# 1 加密算法

## 1.1 AES 加密算法

AES 加密算法的流程如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/AES%E5%8A%A0%E5%AF%86%E7%AE%97%E6%B3%95.jpg" alt="AES加密算法" style="zoom: 33%;" />

### 1.1.1 初始变换

![初始变换](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/%E5%88%9D%E5%A7%8B%E5%8F%98%E6%8D%A2.jpg)

### 1.1.2 字节替换

字节替换的过程其实通过一个 S-box 线性表来将初始化后的字符进行一一转换，例如 19 → 对于（1,9）这个坐标位置，我们通过查询表可以知道其对应的是 d4 ，我们就拿 d4 去替换 19 ，其他位置依次类推。

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220911143120456.png" alt="image-20220911143120456" style="zoom:33%;" />

原先的 4*4 矩阵转换成了如下矩阵

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220911143617204.png" alt="image-20220911143617204" style="zoom:33%;" />

### 1.1.3 行位移

将原先的矩阵每行按规律移位，例如下图：

![行移位](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/%E8%A1%8C%E7%A7%BB%E4%BD%8D.jpg)

第一行保持不变，第二行向左移动1个字节，第三行向左循环2个字节，第四行向左循环3个字节

### 1.1.4 列混合

