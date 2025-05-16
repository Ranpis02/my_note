> [!tip]
>
> 官方中文手册：https://www.numpy.org.cn/
>
> 官方英文手册：https://numpy.org/doc/

NumPy(**Num**erical **Py**thon) 中的基础数据类型为 `numpy.ndarray`，其中 `ndarray` 是指 N-dimensional array，即代表 N 维数组

本文的环境：

- Python 3.12.6
- NumPy 2.1.1

> [!tip]
>
> 如何查看 NumPy 的版本？
>
> ```python
> import numpy
> print(numpy.__version__)
> ```



## 1 how to create a ndarray?

```python
numpy.array(object, dtype=None, *, copy=True, order='K', subok=False, ndmin=0, like=None)
```

- `object`: array_like, 例如列表（list）， 元组（tuple），字符串（string）
- `dtype`: 数组所需的数据类型，未指定就会使用能代表数据的数据类型



1. 创建 ndarray

   ```python
   >>> import numpy as np
   >>> np.array([1, 2, 3, 4])
   array([1, 2, 3])
   ```

2. 向上转型（Upcasting）

   ```python
   >>> np.array([1, 2, 3.0])
   array([ 1.,  2.,  3.])
   ```

   > [!note]
   >
   > object 项中有浮点数，其他的都是整型，因此，object 中所有的数据都向上转型为浮点数

3. 指定最小多维数组

   ```python
   >>> np.array([1, 2, 3], ndmin=2)
   array([[1, 2, 3]])
   ```

4. 多维数组

   ```python
   >>> np.array([[1, 2], [3, 4]])
   array([[1, 2],
          [3, 4]])
   ```

通过 `np.array()` 创建数组返回的数据类型为 `numpy.ndarray`



## 2 constant

> [!warning]
>
> 以下为了方便书写， numpy 统一使用 np 替代

- `np.e`: Euler’s constant, base of natural logarithms, Napier’s constant.

  ```python
  >>> import numpy as np
  >>> np.e
  2.718281828459045
  ```

- `np.inf`: IEEE 754 floating point representation of (positive) infinity.

- `np.nan`: IEEE 754 floating point representation of Not a Number (NaN).





**重要常量：np.newaxis**

`np.newaxis`: 官方对其的介绍为 "A convenient alias for None, useful for indexing arrays."，即是对 `None` 的一个别名，在对数组进行索引的时候有用

```python
np.newaxis is None # True
```

我们主要拿 `np.newaxis` 进行升维，例如：

```python
x = np.array([0, 1, 2, 3])
x.shape # (4,)

# CASE 1: 在当前维度的前面插入一个维度 
x = x[np.newaxis, :]
x # array([[0, 1, 2, 3]])
x.shape # (1, 4)

# CASE 2: 在当前维度的后面插入一个维度
x = x[:, np.newaxis]
x
'''
array([[0],
       [1],
       [2],
       [3]])
'''
x.shape # (4, 1)
```

> [!note]
>
> 参考：https://stackoverflow.com/questions/29241056/how-do-i-use-np-newaxis



## 3 Basic API

1. `np.arange()`：与 `range()` 类似，都是生成一个有起点和终点以及固定步长的序列

   >Return evenly spaced values within a given interval.

   ```python
   # num.arange() 语法
   numpy.arange([start, ]stop, [step, ]dtype=None, *, device=None, like=None)
   
   # range() 语法
   range([start,]stop[, step])
   ```

   上述两个的区别如下：

   1. 返回的对象不同，`np.arange()` 返回的是 ndarray 对象，而 `range()` 返回的是迭代器

      ```python
      np.arange(1, 4) # array([1, 2, 3])
      
      range(1, 4) # range(1, 4)
      [*range(1, 4)] # unpacking, result: [1, 2, 3]
      ```

   2. `np.arange()` 中的 step 参数可以是小数，而 `range()` 的 step 只能是整数。在 Python 的 `.ipy` 类型声明文件中，我们可以看到两者对其的约束

      ```python
      class range(Sequence[int]):
          @property
          def start(self) -> int: ...
          @property
          def stop(self) -> int: ...
          @property
          def step(self) -> int: ...
          ...
      ```

      ```python
      def arange(  # type: ignore[misc]
          start: _FloatLike_co,
          stop: _FloatLike_co,
          step: _FloatLike_co = ...,
          dtype: None = ...,
          *,
          device: None | L["cpu"] = ...,
          like: None | _SupportsArrayFunc = ...,
      ) -> NDArray[floating[Any]]: ...
      
      _FloatLike_co: TypeAlias = float | np.floating[Any] | _IntLike_co
      # 其中 arange 还有其他重载方法, 用来接受 integer 类型的参数等
      ```

      > [!tip]
      >
      > 从源码，我们也可以看出来，range 其实是 Python 自带的一个内置类，而 arange 则是 NumPy 的一个方法

2. `np.linspace()`

   ```python
   numpy.linspace(start, stop, num=50, endpoint=True, retstep=False, dtype=None, axis=0, *, device=None)
   ```

   - endpoint 控制是否包含最后一个数，默认为 False
   - 

3. `np.ones()` 创建全一矩阵

   > Return a new array of given shape and type, filled with ones.

   ```python
   numpy.ones(shape, dtype=None, order='C', *, device=None, like=None)
   ```

4. `np.zero()` 创建全零矩阵

   >Return a new array of given shape and type, filled with zeros.

   ```python
   numpy.zeros(shape, dtype=float, order='C', *, like=None)
   ```

   - shape: int or tuple of ints
   - dtype: data-type, optional

5. `np.astype()`: 转换数据类型

   ```python
   a = np.zeros((3, 2))
   b = a.astype(int)
   ```

6. `np.where()`: 类似于三元表达式

   ```python
   numpy.where(condition, [x, y, ]/)
   ```

   >Return elements chosen from *x* or *y* depending on *condition*.



对 `np.where()` 的补充：

1. 普通应用: 当满条件满足时，执行 x ，否则执行 y

   ```python
   >>> a = np.arange(10)
   >>> np.where(a < 5, a, 10*a) # array([ 0,  1,  2,  3,  4, 50, 60, 70, 80, 90])
   array([ 0,  1,  2,  3,  4, 50, 60, 70, 80, 90])
   ```

2. 我们也可以将其应用到多维数组

   ```python
   >>> np.where([[True, False], [True, False]], 
            [[1, 2], [3, 4]],
            [[9, 8], [7, 6]])
   
   array([[1, 8],
          [3, 6]])
   ```

   > [!tip]
   >
   > 1. 第一个元素为 True, 从第一个二维数组中取出对应位置的元素 1
   > 2. 第二个元素为 False，从第二个二维数组中取出对应位置的元素 8
   > 3. 第三个元素为 True，从第一个二维数组中取出对应位置的元素 3
   > 4. 第四个元素为 False，从第二个二维数组中取出对应位置的元素 6



## 4 Basic Operation

1. 两个**相同尺寸**的数组可以直接进行四则运算

   ```python
   a = np.array([1, 2, 3])
   b = np.array([4, 5, 6])
   
   a + b
   a - b
   a * b
   a / b
   ```

2. 两个相同数组的点乘运算

   ```python
   np.dot(a, b) # x1 * y1 + x2 * y2 + ...
   ```

3. 矩阵乘法运算

   ```python
   a = np.array([[1, 2],
                [3, 4]])
   b = np.array([[2, 0],
                [0, 2]])
   a @ b # 等价于 np.matmul(a, b)
   '''
   array([[2, 4],
      	   [6, 8]])
   '''
   ```

4. 对数运算： `np.log(a)`

5. 指数运算：`np.power(a, n)`

6. 获取数组中的最小值、最大值、平均值等

   ```python
   a = np.array([1, 2, 3, 4])
   a.min() # 最小值
   a.max() # 最大值
   a.mean() # 平均值
   np.median(a) # 中位数
   a.var() # 方差
   a.std() # 标准差
   a.sum() # 求和
   a.sum(axis=0) # 按行求和
   a.sum(axis=1) # 按列求和
   a[a < 3] # 按条件筛选指定
   a[(a > 3) & (a % 2 == 0)] # 按逻辑运算符组合不同的条件进行筛选，与运算使用 & 表示，或运算用 | 表示
   ```



## 5 Broadcasting

```python
a = np.array([1, 2, 3])
a * 3
```

```python
a = np.array([[1], [10], [20]])
b = np.array([0, 1, 2])
a + b
```

![image-20241018001034571](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20241018001034571.png)

```
array([[ 1,  2,  3],
       [10, 11, 12],
       [20, 21, 22]])
```



## 6 跨行列取值

```python
a = np.array([[1, 2, 3], [4, 5, 6]])
# 取出第一列所有元素
a[:, 0]
# 取出第一行的所有元素
a[0, :]
```

```python
# 取出从 1 到 9 的所有元素，要求跨度为 2
a = np.array([1, 2, 3, 4, 5, 6, 7, 8, 9])
a[0:9:2] # 0 对应的索引，表示取值范围为 [0, 9)

# 跨度也可以是负数，如下就是一个反转数组的写法
a[::-1]
```



7 水平堆叠和垂直堆叠

- 水平堆叠：`np.hstack()`
- 垂直堆叠：`np.stack()`

