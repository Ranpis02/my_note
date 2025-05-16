# 1 Canvas 初识

## 1.1 Canvas 介绍

Canvas API  提供了一个通过 JS 和 HTML 的 **canvas 标签**来绘制图形的方式，它的作用主要包括如下几类：

- 动画
- 游戏画面
- 数据可视化
- 图片编辑
- 实时视频处理
- …

Canvas API 主要聚焦于 <u>2D 图形</u>，而同样使用的 canvas 元素的 <u>webGL API 则绘制硬件加速的 2D 和 3D 图形</u>



## 1.2 简单的演示

我们如果想要通过 canvas 来实现绘制一些图形，则需要以下三个部分：

1. id：标识元素的唯一性
2. width、height：设置画布的宽度和高度
3. 代码绘制
   - 获取元素（注册元素）
   - 获取画笔及上下文对象
   - 绘制图形

**示例**：绘制一个简单的矩形

```js
<canvas id="c1" width="300px" height="300px"></canvas>
<script>
    // 1. 注册元素
    var c1 = document.getElementById("c1");
    // 2. 获取画笔以及上下文对象
    var ctx = c1.getContext('2d');
    // 3. 绘制图形
    // 绘制一个矩形,设置画笔颜色以及填充的一些参数
    // 3.1 画笔的颜色为绿色
    ctx.fillStyle = 'green';
    // 四个参数分别对应这 x 坐标、y 坐标、宽度、高度
    ctx.fillRect(10, 50, 100, 100);
</script>
```

显示效果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221120145232335.png" alt="image-20221120145232335" style="zoom:33%;" />

## 1.3 canvas 上下文对象

Canvas API 中最重要的一部分便是 **CanvasRenderingContext2D 接口**，可以为 Canvas 元素的绘图表面提供 <u>2D 渲染上下文</u>。它用于<u>绘制形状、文本、图像和其他对象</u>

> 我们可以举例子来形象地说明，通过 id 来获取 canvas 元素相当于获取 "画布"，通过，然后就是通过 getContext 来获取 "画笔"
>
> - 参数如果为 "2d"，表示获取一只 2d 画笔
> - 参数如果为 "3d"，表示获取一只 3d 画笔

我们在控制台打印一下 CanvasRenderingContext2D，可以看到里面有许多内置的参数和方法

![image-20221120151230649](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221120151230649.png)

我们可以到 MDN 中进行查阅，[传送入口](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D)

**示例**：利用 2D 渲染上下文

```js
<canvas id="c1" width="300px" height="300px"></canvas>
<script>
    var c1 = document.getElementById("c1");
    var ctx = c1.getContext('2d');
    // 设置线条粗细
    ctx.lineWidth = 10;

    // 1. 绘制墙
    ctx.strokeRect(75, 140, 150, 110);
    // stroke 表示 "描边"，与此对应的就是 fill,表示 "填充"
    // strokeRect 表示绘制一个描边矩形，四个参数分别代表 x 坐标、y 坐标、width 宽度、height 高度
    // 2. 绘制大门
    ctx.fillRect(130, 190, 40, 60);
    // fillRect 表示绘制一个填充矩形，四个参数分别代表 x 坐标、y 坐标、width 宽度、height 高度
    // 3. 绘制屋顶
    // 如果我们想要自定义图形，就需要使用到 beginPath 和 colsePath
    ctx.beginPath();
    // moveTo 画笔抬起到指定位置后落下
    ctx.moveTo(50, 140);
    // lineTo 移动线条
    ctx.lineTo(150, 60);
    ctx.lineTo(250, 140);
    ctx.closePath();
    ctx.stroke(); // lineTo 只是使用直线连接子路径最终到指定 x、y 的位置，最后要画线则需要用到 stroke
</script>
```

显示效果如下：

![image-20221120153707977](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221120153707977.png)



## 1.4 Canvas 浏览器支持

现在市面上大部分浏览器都支持 Canvas，但也有些低版本浏览器是不支持 Canvas，我们可以添加以下代码进行判断是否支持 Canvas

```js
<canvas id="c1" width="300px" height="300px">您的浏览器版本太低，请换成高版本浏览器打开此页面</canvas>
<script>
    var c1 = document.getElementById("c1");
    if (!c1.getContext) {
        console.log("当前浏览器不支持 canvas,请下载最新版本的浏览器");
    } else {
        // 代码段
    }
</script>
```

- 如果当前浏览器版本太低，不支持 Canvas，则 Canvas 则会被识别成为了一个普通标签，就会显示里面的内容





# 2 Canvas 绘制

## 2.1 Canvas 填充与路径绘制

Canvas 绘制图形一共有两种形式：

1. 路径绘制（描边绘制）

   ```js
   ctx.strokeRect(x, y, width, height)
   ```

2. 填充绘制

   ```js
   ctx.fillRect(x, y, width, height)
   ```

其中 ，fillRect 和 strokeRect 是可以拆开来写的

也就是说：

- `fillRect` 等效于下面书写形式

  ```js
  ctx.rect(x, y, width, height)
  ctx.fill()
  ```

- `strokeRect` 等效于下面书写形式

  ```js
  ctx.rect(x, y, width, height)
  ctx.stroke()
  ```



**示例**：现在我先要绘制一双"眼睛"，一只睁开的，一只紧闭的

```js
<canvas id="c1" width="400px" height="400px"></canvas>
<script>
    var c1 = document.getElementById("c1");
    var ctx = c1.getContext("2d");
    ctx.lineWidth = 5
    ctx.beginPath();
    ctx.rect(100, 100, 50, 50);
    ctx.fill();
    ctx.closePath();
    ctx.beginPath();
    ctx.rect(250, 100, 50, 50);
    ctx.stroke();
    ctx.closePath();
</script>
```

显示效果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221120164053312.png" alt="image-20221120164053312" style="zoom:25%;" />

> 这里需要注意：程序并不知道你啥时候笔画是连着的，我们需要通过 beginPath 和 closePath 来确定那些线条是 "一笔画" 的，如果我们不添加并且先进行路径绘制再进行填充绘制则会出现错误，如下：
>
> <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221120164807865.png" alt="image-20221120164807865" style="zoom:25%;" />



## 2.2 图像拉伸

当我们给 canvas 添加好 width 和 height ，那么画布的大小就固定不变了，我们可以在画布中绘制图形，这就相当于一张 image，我们通过 sytle 可以修改其中的样式，包括了 宽度 和 高度，这就相当于进行了拉伸

```html
<canvas id="c1" width="400px" height="400px" style="width: 200px;"></canvas>
```

大小对比：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221120165942435.png" alt="image-20221120165942435" style="zoom: 25%;" />



## 2.3 绘制弧形段

绘制圆弧或者圆，我们可以使用 `arc()` 方法，当然也可以使用 `arcTo` 方法，不过这个是根据控制点和半径来绘制圆弧路径，比较复杂，我们先对 `arc()` 方法进行讲述，语法如下：

```js
arc(x, y, radius, startAngle, endAngle, anticlockwise)
```

- `x`：圆弧中心的 x 坐标
- `y`：圆弧中心的 y 坐标
- `radius`：圆弧的半径
- `stratAngle`：圆弧的起始点，x 轴方向开始计算，单位以弧度表示
- `endAngle`：圆弧的终点，y 轴方向开始计算，单位以弧度表示
  
  - $\dfrac{\pi}{6}=30^\circ$
  - $\dfrac{\pi}{3}=60^\circ$
  - $\dfrac{\pi}{2}=90^\circ$
  
- `acticlockwise`：翻译过来即是"逆时针方向的"，如果参数值为 true，表示顺时针；如果为 false，表示逆时针（*默认*）

  <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221120191632735.png" alt="image-20221120191632735" style="zoom:25%;" />

> :warning:由于 arc 是绘制圆弧的方法，故里面的 6 个参数除了方向，其他的 5 个参数都不能缺省

**案例**：绘制圆弧和笑脸

```js
var c1 = document.getElementById("c1");
var ctx = c1.getContext("2d");
ctx.lineWidth = 10;
ctx.beginPath();
// 笑脸轮廓
ctx.arc(250, 250, 200, 0, Math.PI*2);
ctx.moveTo(250+150*Math.PI/3-30,250+150*Math.PI/6);
ctx.arc(250, 250, 150, Math.PI/6, Math.PI*5/6);
ctx.moveTo(230, 180);
ctx.arc(180, 180, 50, 0, Math.PI*2);
ctx.moveTo(370, 180);
ctx.arc(320, 180, 50, 0, Math.PI*2);
ctx.stroke();
ctx.closePath();
```

显示效果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/simle.png" alt="simle" style="zoom:25%;" />

## 2.4 绘制折线线段

### 2.4.1 绘制三角形

```js
<canvas id="c1" width="400px" height="400px"></canvas>
<script>
    var c1 = document.getElementById("c1");
    var ctx = c1.getContext("2d");
    ctx.lineWidth = 1
    ctx.beginPath();
    ctx.moveTo(200, 100);
    ctx.lineTo(300, 300);
    ctx.lineTo(100, 300);
    ctx.lineTo(200, 100)
    // ctx.lineTo(350, 350);
    ctx.stroke();
    ctx.closePath();
</script>
```

显示效果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/triangle.png" alt="triangle" style="zoom:25%;" />



## 2.5 arcTo绘制圆弧



## 2.6 贝塞尔曲线

### 2.6.1 二次贝塞尔曲线

`**CanvasRenderingContext2D.quadraticCurveTo()**` 是 Canvas 2D API 是新增二次贝塞尔曲线路径的方法，它需要 2 个点，第一个是控制点，第二个是终点。起始点是当前路径最新的点，当创建二次贝塞尔曲线之前，可以用过 `moveTo` 方法进行改变。语法规则如下：

```js
void ctx.quadraticCurveTo(cpx, cpy, x, y);
```

- `cpx`：控制点的 x 坐标
- `cpy`：控制点的 y 坐标
- `x`：终点的 x 坐标
- `y`：终点的 y 坐标

> 开始点一般由 `moveTo()` 或者 `lineTo()` 提供，控制点和结束点是由 `quadraticCurveTo()` 提供

二次方贝兹曲线的路径由给定的$P_0、P_1、P_2$的函数 $B(t)$ 追踪
$$
B(t) = (1 - t)^2P_0+2t(1-t)P_1+t^2P_2\mbox{  ,  }t\in[0,1]
$$
![bézier_2_big](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/b%C3%A9zier_2_big.gif)

**示例1**：画一条简单的贝塞尔曲线

```js
<canvas id="c1" width="400px" height="400px"></canvas>
<script>
    const cnv = document.getElementById('c1')
    // 获取上下文对象context
    const ctx = cnv.getContext('2d')
    ctx.moveTo(100, 300);
    ctx.quadraticCurveTo(200, 100, 300, 300);
    ctx.stroke();
    ctx.fillStyle = 'blue';
    ctx.fillRect(100, 300, 10, 10);
    ctx.fillStyle = 'red';
    ctx.fillRect(200, 100, 10, 10);
    ctx.fillStyle = 'blue';
    ctx.fillRect(300, 300, 10, 10);
</script>
```

实现效果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/bezer_01.png" alt="bezer_01" style="zoom: 50%;" />

> 蓝色控制点表示起始点和终点，红色点表示控制点



**示例2**：实现聊天气泡框

绘制聊天气泡框的原理图如下：



```js
<canvas id="c1" width="400px" height="400px"></canvas>
<script>
    const cnv = document.getElementById('c1')
    // 获取上下文对象context
    const ct = cnv.getContext('2d')
    // 封装一个绘制聊天气泡框的函数
    /**
    * @ctx 
    * @x 左上角点的 x 坐标
    * @y 左上角点的 y 坐标
    * @w 整体的宽度
    * @h 整体的高度
    */
    function drawBubble(ctx, x, y, w, h) {
        ctx.beginPath();
        ctx.moveTo(x, y + 0.35 * h);
        ctx.quadraticCurveTo(x + 0.04 * w, y + 0.02 * h, x + 0.5 * w, y);
        ctx.quadraticCurveTo(x + 0.96 * w, y + 0.02 * h, x + w, y + 0.35 * h);
        ctx.quadraticCurveTo(x + w, y + 0.7 * h, x + 0.58 * w, y + 0.72 * h);
        ctx.quadraticCurveTo(x + 0.5 * w, y + 0.9 * h, x + 0.2 * w, y + h);
        ctx.quadraticCurveTo(x + 0.38 * w, y + 0.80 * h, x + 0.38 * w, y + 0.72 * h);
        ctx.quadraticCurveTo(x, y + 0.70 * h, x, y + 0.35 * h);
        ctx.stroke();
    }
    drawBubble(ct, 10, 10, 300, 300);
</script>
```

显示效果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221121130851040.png" alt="image-20221121130851040" style="zoom:25%;" />



### 2.6.2 三次贝塞尔曲线

**`CanvasRenderingContext2D.bezierCurveTo()`** 是 Canvas 2D API 绘制三次贝赛尔曲线路径的方法。该方法需要三个点，第一、第二个是控制点、第三个是结束点，起始点是当前路径的最后一个点，绘制三次贝塞尔曲线，可以通过 `moveTo()` 进行修改

> 三次贝塞尔曲线相对于二次贝塞尔曲线，能绘制更加柔和的曲线

**语法**

```js
CanvasRenderingContext2D.bezierCurveTo() 是 Canvas 2D API
```

**参数**

- `cp1x`：第一个控制点的 x 轴坐标
- `cp1y`：第一个控制点的 y 轴坐标
- `cp2x`：第二个控制点的 x 轴坐标
- `cp2y`：第二个控制点的 y 轴坐标
- `x`：结束点的 x 轴坐标
- `y`：结束点的 y 轴坐标



![bezier_03](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/bezier_03.gif)
$$
B(t)=(1-t^2)P_0+2t(1-t)
$$


贝塞尔曲线的阶数取决于**控制点的个数**

- 控制点的个数为 0，则称为线性贝塞尔；
- 控制点的个数为 1，则称为二阶贝塞尔曲线；
- 控制点的个数为 2，则称为三阶贝塞尔曲线
- 依此类推……

**示例1**：绘制一个简单的三节贝塞尔曲线

```js
canvas id="c1" width="400px" height="400px"></canvas>
<script>
    const cnv = document.getElementById('c1')
    const ctx = cnv.getContext('2d')
    ctx.moveTo(100, 100);
    ctx.bezierCurveTo(150, 300, 250, 300, 300, 100);
    ctx.stroke();
    function drawPoint(ctx, x, y, color) {
        ctx.beginPath();
        ctx.moveTo(x, y);
        ctx.arc(x, y, 5, 0, Math.PI*2);
        ctx.fillStyle = color;
        ctx.fill();
        // ctx.closePath();
    }
    drawPoint(ctx, 100, 100, 'blue');
    drawPoint(ctx, 150, 300, 'red');
    drawPoint(ctx, 250, 300, 'red');
    drawPoint(ctx, 300, 100, 'blue');
</script>
```

显示效果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/bezier.png" alt="bezier" style="zoom: 50%;" />

> - 红色的为控制点
> - 蓝色的为起始点和终点



**示例2**：绘制一个爱心

```js
<canvas id="c1" width="400px" height="400px"></canvas>
<script>
    const cnv = document.getElementById('c1');
    const ctx = cnv.getContext('2d');
    ctx.beginPath();
    ctx.moveTo(200, 300);
    ctx.bezierCurveTo(50, 250, 120, 100, 200, 200);
    ctx.moveTo(200, 300);
    ctx.bezierCurveTo(350, 250, 280, 100, 200, 200);
    ctx.stroke();
</script>
```

实现效果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221121193923872.png" alt="image-20221121193923872" style="zoom:25%;" />





## 2.7 创意绘画

### 2.7.1 抽象自画像

```js
<canvas id="c1" width="400px" height="400px"></canvas>
<script>
    var c1 = document.getElementById("c1");
    var ctx = c1.getContext("2d");
    ctx.lineWidth = 5
    ctx.beginPath();
    ctx.rect(100, 100, 50, 50);
    ctx.fill();
    ctx.closePath();
    ctx.beginPath();
    ctx.rect(250, 100, 50, 50);
    ctx.stroke();
    ctx.beginPath();
    ctx.moveTo(250+Math.cos(), 200);
    ctx.arc(200, 200, 100, Math.PI/6, Math.PI*5/6);
    ctx.stroke();
    ctx.beginPath();
    ctx.moveTo(180, 220);
    ctx.lineTo(220, 220);
    ctx.lineTo(200, 200);
    ctx.lineTo(177, 220);
    ctx.stroke();
    ctx.closePath();
</script>
```

显示效果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/me-01.png" alt="me-01" style="zoom: 50%;" />

# 3 高级技巧

## 3.1 Path2D

`Path2D` 是一个正在实验中的接口，用于声明路径（或者说封装路径），和函数的作用相同或类似，主要是为了代码复用，语法如下：

```js
new Path2D();
new Path2D(path);
new Path2D(d);
```

- `path`：可选，用于创建一个 `path` 变量的拷贝
- `d`：可选，调用 SVG 数据构成的构成的字符串创建一个新的路径

```js
<canvas id="c1"></canvas>
<script>
    var c1 = document.querySelector('#c1');
    var ctx = c1.getContext('2d');
    // 1. 创建一个长和宽都为 100 的矩形，矩形左上角点在(10, 10)
    var path1 = new Path2D();
    path1.rect(10, 10, 100, 100);
    // 拷贝 path1 的路径到 path2 中
    var path2 = new Path2D(path1);
    // path2 再自己创建一个圆的路径
    path2.moveTo(220, 60);
    path2.arc(170, 60, 50, 0, Math.PI*2);
    ctx.stroke(path2);
</script>
```

显示结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221121204831366.png" alt="image-20221121204831366" style="zoom:50%;" />



**示例**：绘制一个正方形

```js
<canvas id="c1"></canvas>
<script>
    var c1 = document.querySelector('#c1');
    var ctx = c1.getContext('2d');
    var path1 = new Path2D("M10 10 h 80 v 80 h -80 Z");
    ctx.stroke(path1);
</script>
```

显示效果如下：

![image-20221121215016420](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221121215016420.png)



## 3.2 path 元素

path 元素中每一个命令都用一个关键字来表示，<font color="red">大写字母表示绝对定位，小写字母表示相对定位</font>

1. 直线命令

   - M(m)：MoveTo 的缩写，表示移动到指定位置（不能画出线）

     - 例如：`M 10 10` 表示移动到 (10, 10) 这个点

   - L(l)：lineTo 的缩写，画出从起点到指定位置的线段

     - 例如：`L 10 10` 表示画出起点到 (10, 10) 的线段

   - H(h)：绘制水平线

     - 例如：`H 10` 表示画出长度为 10px 的水平线，起点作为起始点

   - V(v)：绘制垂直线

     - 例如：`V 10` 表示画出长度为 10px 的垂直线，起点作为起始点

     > `V ` 和 `H` 分别以右和下为正方向（数值为正），左和上为负方向（数值为负） 
     >
     > <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221121214826537.png" alt="image-20221121214826537" style="zoom:25%;" />

   - Z(z)：用来闭合路径，不区分大小写

2. 曲线命令

   - 贝塞尔曲线，这里以绘制三次贝塞尔曲线为例，三次贝塞尔曲线需要定义一个点和两个控制点，设置如下：

     ```js
      C x1 y1, x2 y2, x y (or c dx1 dy1, dx2 dy2, dx dy)
     ```



## 3.3 颜色样式控制

1. fillSytle：设置图形的填充颜色，参数值为颜色（包括颜色字面量、颜色 16 进制、rgb等）
2. strokeStrle：设置图形路径的颜色，参数值为颜色（包括颜色字面量、颜色 16 进制、rgb等）
3. globalAlpha：设置透明度，参数值的范围为（0-1）
4. color可以是表示css颜色值的字符串，渐变对象或者图案对象，color默认为黑色（这个暂时没有使用过）



## 3.4 线性渐变和径向渐变 

### 3.4.1 createLinearGradient()

`createLinearGradient()` 方法用于创建一个沿指定坐标的直线的线性渐变效果

语法如下：

```js
CanvasGradient ctx.createLinearGradient(x0, y0, x1, y1);
```

- `x0`：起点的 x 轴坐标
- `y0`：起点的 y 轴坐标
- `x1`：终点的 x 轴坐标
- `y1`：终点的 y 轴坐标
- 返回值：一个根据指定线路初始化的线性 `CanvasGradient` 对象



![img](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/mdn-canvas-lineargradient.png)



**示例**：画一个由红色过渡到蓝色的线性渐变效果

```js
<canvas id="c1" width="200px" height="200px"></canvas>
<script>
    var ca = document.getElementById("c1");
    var ctx = ca.getContext("2d");
    var gradient = ctx.createLinearGradient(0, 100, 200, 100);
    gradient.addColorStop(0, 'red');
    gradient.addColorStop(1, 'blue');
    ctx.fillStyle = gradient;
    ctx.fillRect(0, 0, 200, 200);
</script>
```

显示效果如下：





### 3.4.2 addColorStop()

`addColorStop()` 方法用于添加一个**偏移值**和**颜色值**指定的**断点**的渐变效果，语法如下：

```js
void gradient.addColorStop(offset, color);
```

- offset：表示偏移值，范围为：[0, 1]，超出这个返回将抛出`INDEX_SIZE_ERR` 错误
- color：参数为 CSS 的颜色值，如果不能被解析为有效的 CSS 颜色值，将抛出 `SYNTAX_ERR` 错误

> `addColorStop` 函数经常搭配渐变函数一起使用，用于在指定位置添加颜色断点



### 3.4.3 createRadialGradient()

`createRadialGradient()` 用于绘制径向渐变（或称为"辐射渐变"），该参数确定两个圆的坐标，绘制放射性渐变的方法。

```js
CanvasGradient ctx.createRadialGradient(x0, y0, r0, x1, y1, r1);
```

- `x0`：开始圆形的 x 轴坐标
- `y0`：开始圆形的 y 轴坐标
- `r0`：开始圆形的半径
- `x1`：结束圆形的 x 轴坐标
- `y1`：结束圆形的 y 轴坐标
- `r1`：结束圆形的半径
- 返回值是由两个指定的圆初始化的放射性 `CanvasGradient` 对象

> 开始圆我们可以看做显示圆，结束圆我们可以看做转换圆，最终的显示效果是显示圆在转换圆上的投影



**示例**：画一个红色到蓝色的径向渐变

```js
<canvas id="c1" width="200px" height="200px"></canvas>
<script>
    var ca = document.getElementById("c1");
    var ctx = ca.getContext("2d");
    var gradient = ctx.createRadialGradient(100,100,0,100,100,100);
    gradient.addColorStop(0, 'red');
    gradient.addColorStop(1, 'blue');
    ctx.fillStyle = gradient;
    ctx.arc(100, 100, 100, 0, Math.PI*2);
    ctx.fill();
</script>
```

显示效果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/radialgradient.png" alt="radialgradient" style="zoom:50%;" />

### 3.4.4 角度渐变





## 3.5 图案样式 Patterns

Patterns 语法如下：

```js
createPattern(image, type)
```

该方法接收两个参数，image 可以是一个 image 对象的引用，或者另一个 canvas 对象。type 必须是下面的字符串值之一：<u>repeat、repeat-x、repeat-y 和 no-repeat</u>

 

**示例**

```js
<canvas id="c1"></canvas>
<script>
    var c1 = document.getElementById("c1");
    // 获取画笔，上下文对象
    var ctx = c1.getContext("2d");
    // 创建图案样式
    var img = new Image();
    img.src = "../../images/放大镜.png";
    
    img.onload = function() {
        // 创建图案 createPattern(图片方式, 重复方式)
        var pattern = ctx.createPattern(img, "repeat");
        ctx.fillStyle = pattern;
        ctx.fillRect(0, 0, 600, 400);
    }
</script>
```

显示效果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221122111103471.png" alt="image-20221122111103471" style="zoom: 33%;" />



## 3.6 线段和虚线样式设置

我们可以通过一系列属性来设置线的样式

- `lineWidth`：设置线条宽度

  - 描述线段宽度的数字。0、负数、`Infinity` 和 `NaN` 都会被忽略

- `lineCap`：设置线条末端样式

  - `butt`：线段末端以方形结束（默认）
  - `round`：线段末端以圆形结束
  - `square`：线段末端以方形结束，但是增加了一个宽度和线段相同，长度是线段宽度一半的矩形区域

  <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221122142232805.png" alt="image-20221122142232805" style="zoom:25%;" />

- `lineJoin`：设定线条与线条接合处的样式

  - `round`：通过填充一个额外的，圆心在相连部分末端的扇形，绘制拐角的形状

  - `bevel`：在相连部分的末端填充一个额外的以三角形为底的区域，每个部分都有各自独立的矩形拐角

  - `miter`：通过延伸相连的末端填充一个额外的以三角形为底的区域

  <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/canvas_linejoin.png" alt="img" style="zoom: 80%;" />

- `miterLimit`：限制当两条线相交处时交接处的最大长度；所谓交接处长度（斜接长度）是指线条交接处内角顶点到外角顶点的长度

- `getLineDash()`：返回一个包含当前虚线样式，长度为非负偶数的数组

- `setLineDash(segments)`：设置当前虚线样式

  - segments 里面的参数为数组，数组里面的元素分别是线段长和间距交替进行

  <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221122145410935.png" alt="image-20221122145410935" style="zoom:33%;" />

- `lineDashOffset`：设置虚线样式的起始偏移量，即确定一条线从哪里开始是虚线

  - 偏移量是 float 精度的数字， 初始值为 0.0




**示例**：绘制蚂蚁线

> "蚂蚁线" 经常出现在计算机绘图程序的选区工具中，帮助用户根据图片背景动态变化的边界来区分选择的边界

```js
<canvas id="c1" width="400px" height="400px"></canvas>
<script>
    var c1 = document.getElementById("c1");
    var ctx = c1.getContext("2d");
    var offset = 0;
    function draw() {
        // 清除之前绘制的蚂蚁线
        ctx.clearRect(0, 0, c1.width, c1.height);
        // 设置虚线的 segements
        ctx.setLineDash([4, 2]);
        // 偏移量是正数则代表逆时针转动，偏移量是负数则代表顺时针转动
        ctx.lineDashOffset = -offset;
        ctx.strokeRect(10, 10, 100, 100);
    }
    function march() {
        offset++;
        if(offset > 16) {
            offset = 0;
        }
        draw();
        setTimeout(march, 20);
    }
    march();
</script>
```

显示效果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/tutieshi_478x486_2s.gif" alt="tutieshi_478x486_2s" style="zoom:33%;" />



## 3.7 阴影设置

**`shadowOffsetX = float`**

- 用于设定阴影在 X 轴的延伸距离，正值表示往下延伸，赋值表示往上延伸，默认为 0

**`shadowOffsetY = float`**

- 用于设定阴影在 Y 轴的延伸距离，正值表示往右延伸，负值表示往左延伸，默认为 0

**`shadowBlur = color`**

- 用于设定阴影的模糊程度，其数值并不跟像素数量挂钩，也不受变换矩阵的影响，默认为 0

**`shadowColor = color`**

- 是标准的 CSS 颜色值，用于设定阴影颜色效果，默认是全透明的黑色

```js
<canvas id="c1"></canvas>
<script>
    var c1 = document.getElementById("c1");
    var ctx = c1.getContext("2d");
    ctx.shadowColor = "black";
    ctx.shadowOffsetX = 10;
    ctx.shadowOffsetY = 10;
    ctx.shadowBlur  = 10;
    ctx.font = "48px serif";
    ctx.testAlign = "left";
    ctx.strokeText("Hello World", 0, 100);
</script>
```

显示结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221122162827825.png" alt="image-20221122162827825" style="zoom:50%;" />



## 3.8 canvas 渲染图片

```js
drawImage(image, dx, dy)
drawImage(image, dx, dy, dWidth, dHeight)
drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight)
```

- `image`：图像源
- `dx、dy`：放置到画布的相对位置
- `dWidth、dHeight`：将原图像缩放的宽度和高度
- `sx、sy`：截取部分左上角点的 x，y 轴坐标
- `swidth、sHeight`：截取宽度和高度

```js
<canvas id="c1"></canvas>
<script>
    var c1 = document.getElementById("c1");
    var ctx = c1.getContext("2d");
    // 为图像指定特定尺寸大小
    const image = new Image();
    image.src="../../images/new01.jpg";
    image.onload = function() {
        ctx.drawImage(image, 0, 0);
    }
</script>
```

显示效果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221122171231945.png" alt="image-20221122171231945" style="zoom:33%;" />

:warning:注意：<font color="red">这里一定要使用 onload 事件，待图片全部加载完成之后再使用 `drawImage` 进行渲染</font>，因为加载图片显然慢于程序的解释执行，所以我们先得有图片，才能进行渲染



## 3.9 canvas 获取视频并添加水印





## 3.10 渲染文字与对齐

canvas 提供了两种渲染文字的方法

```js
// 1. 在指定的 (x, y) 位置填充的文本，绘制最大宽度是可选
fillText(text x, y, [,maxWidth])

// 2. 在指定的 (x, y) 位置绘制文本边框，绘制最大宽度是可选
strokeText(text, x, y, [,maxWidth])
```

示例：

```js
<canvas id="c1" width="400px" height="400px"></canvas>
<script>
    var c1 = document.getElementById("c1");
    var ctx = c1.getContext("2d");
    ctx.font = "48px serif"
    ctx.fillText("Hello World", 80, 100);
    ctx.strokeText("Hello World", 80, 300);
</script>
```

显示结果如下：

<img src="../../../AppData/Roaming/Typora/typora-user-images/image-20221122183649339.png" alt="image-20221122183649339" style="zoom:25%;" />

**textAlign**

<font color="red">文本水平对齐</font>可以使用 `textAlign` 属性，以基点为对齐点，参数如下：

- left：文本左对齐
- right：文本右对齐
- center：文本居中对齐
- start：文本对齐界限开始的地方
- end：文本对齐界限结束的地方

> 最后两个参数在没有指定方向的情况，right = end，left = strat，如果指定了方向为 rtl，那么就相反（方向通过 ctx.direction 来设置）

```js
<canvas id="c1" width="400px" height="400px"></canvas>
<script>
    var c1 = document.getElementById("c1");
    var ctx = c1.getContext("2d");
    ctx.font = "20px serif"
    ctx.textAlign = "left";
    ctx.fillText("Hello World", 200, 100);
    ctx.textAlign = "center";
    ctx.fillText("Hello World", 200, 200);
    ctx.textAlign = "right";
    ctx.fillText("Hello World", 200, 300);
    ctx.fillStyle = "red";
    ctx.fillRect(200, 100, 5, 5);
    ctx.fillRect(200, 200, 5, 5);
    ctx.fillRect(200, 300, 5, 5)
</script>
```

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221122190718550.png" alt="image-20221122190718550" style="zoom: 25%;" />

**textBaseline**

`textBaseline` 属性是 Canvas 2D API 描述文本时，当前文本基线的属性，即<font color="red">决定文字垂直方向的对齐方式</font>，参数如下，默认是 alphabetic，参数如下：

<u>top</u>

- 文本基线在文本块的顶部。

<u>hanging</u>

- 文本基线是悬挂基线。

<u>middle</u>

- 文本基线在文本块的中间。

<u>alphabetic</u>

- 文本基线是标准的字母基线。

<u>ideographic</u>

- 文字基线是表意字基线；如果字符本身超出了 alphabetic 基线，那么 ideograhpic 基线位置在字符本身的底部。

<u>bottom</u>

- 文本基线在文本块的底部。与 ideographic 基线的区别在于 ideographic 基线不需要考虑下行字母。



## 3.11 位移、缩放、旋转变换

1. 位移：`ctx.translate(X, Y)`
2. 缩放：`ctx.scale(sx, sy)`
3. 旋转：`ctx.rotate(rad)`

使用 transform 方法我们实现对变换矩阵进行





## 3.12 合成

canvas 中提供的 globalCompositeOperation 属性设置了要在绘制新形状是应用的合成操作的类型，其中的 type 是用于标识要使用的合成或混合操模式操作的字符串，语法如下：

```js
ctx.globalCompositeOperation = type;
```

类型参照官网：https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/globalCompositeOperation



**示例**：使用合成图像实现刮刮卡

```css
/* css 样式 */
.content,
.c1 {
    position: absolute;
    width: 300px;
    height: 150px;
    top: 100px;
    left: 50%;
    transform: translateX(-50%);
}

.content {
    text-align: center;
    font-size: 20px;
    line-height: 150px;
    z-index: -1;
}
```

```html
<!-- html 结构 -->
<div class="wrapper">
    <canvas class="c1" width="300px" height="150px"></canvas>
    <div class="content"></div>
</div>
```

```js
// JS 行为
const c1 = document.querySelector('.c1');
const ctx = c1.getContext('2d');
const content = document.querySelector(".content");
ctx.fillStyle = "#ccc";
ctx.fillRect(0, 0, 300, 150);
ctx.beginPath();
ctx.fillStyle = '#fff';
ctx.textAlign = 'center';       // 字体水平居中
ctx.textBaseline = 'middle';    // 字体垂直居中
ctx.font = "40px Arial";        // 设置字号
ctx.fillText("刮刮卡", 150, 75);
let isDraw = false;             // 设置绘制状态
// 1. 设置鼠标放下事件
c1.onmousedown = function () {
    isDraw = true;
}
// 2. 设置鼠标移动事件
c1.onmousemove = function (e) {
    if (isDraw) {
        // 这里需要注意当前刮去的区域是相对于画布的距离，而不是可视区或是页面文档的距离
        const pos=c1.getBoundingClientRect();
        let x = e.clientX - pos.left;
        let y = e.clientY - pos.top;
        ctx.globalCompositeOperation = "destination-out";   
        ctx.arc(x, y, 15, 0, Math.PI * 2);
        ctx.fill();
    }
}
// 3. 设置鼠标抬起事件
c1.onmouseup = function () {
    isDraw = false;
}

// 4. 最后为了控制获奖概率，设置随机函数
if (Math.random() < 0.2) {
    content.innerText = "恭喜您, 中了 1000 w";
} else {
    content.innerText = "谢谢惠顾";
}
```

显示效果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221122214341267.png" alt="image-20221122214341267" style="zoom: 33%;" />



## 3.13 裁剪路径和状态的保存与恢复

`ctx.clip()` 用于设定剪切区域，当我们绘制完一个路径之后使用 `clip()` 就会限制绘图区域在上一个绘制好的路径，所有的绘图只能在这个范围内被显示出来 

**取消剪裁区**

当我们使用 `clip()` 进行绘图之后，为了跳出该剪裁区进行绘制，我们需要使用到 `save()` 函数 和  `restore()` 函数保存绘制状态并防止污染状态栈

示例：

```js
var canvas = document.getElementById("canvas");
var ctx = canvas.getContext("2d");

ctx.save(); // 保存默认的状态

ctx.fillStyle = "green";
ctx.fillRect(10, 10, 100, 100);

ctx.restore(); // 还原到上次保存的默认状态
ctx.fillRect(150, 75, 100, 100);
```

效果图如下：

<img src="../../../AppData/Roaming/Typora/typora-user-images/image-20221122225959873.png" alt="image-20221122225959873" style="zoom:50%;" />

如果我们去除`restore()` 操作，那么绘制出来的将是两个绿色的矩形

:warning:注意：<font color="red">restore 不是撤销，而是恢复到 save 之前的状态</font>



## 3.14 像素操作

**`ImageData()`** 构造函数返回一个新的实例化的 `ImageData` 对象，此对象由给定的类型化数组和指定的宽度和高度组成，自身有如下属性：

- array：Uint8ClampedArray 数组，[0, 1, 2, 3] 分别代表着 r, g, b, a，后面也是依此类推

  <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221122232708546.png" alt="image-20221122232708546" style="zoom:25%;" />

- width：整数
- height：整数

逐个像素遍历算法

```js
for(let i=0;i<arr.length;i+=4){
        let r=data[i+0];
        let g=data[i+1];
        let b=data[i+2];
        let a=data[i+3];
        console.log(r,g,b,a)
}
```

灰度算法公式：
$$
lm = 0.299\times r + 0.587\times g + 0.114 \times b
$$


# 4 类的封装

