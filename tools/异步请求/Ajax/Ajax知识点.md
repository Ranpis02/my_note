-- 笔记内容均来自 B 站黑马程序员官方视频，感谢黑马官方的悉心指导

---

# 1 客户机和服务器

- 客户端与服务器之间的通信过程，分为 <font color="red">请求 → 处理 → 响应 </font>三个步骤
- 网页中每一个资源，都是通过 <font color="red">请求 → 处理 → 响应</font> 的方式从服务器获取过来的

如果在网页中请求服务器上的数据资源，则需要用到 XMLHttpRequest 对象

XMLHttpRequest （简称为 xhr）是浏览器提供的 JS 成员，通过它，可以请求服务器上的数据资源

最简单的用法：`var xhrObj = new XMLHttpRequest()`

## 1.1 基于浏览器的开发者工具分析通信过程

1. 打开 Edge 浏览器，按下 F12 打开调试工具
2. 找到 network → doc，之后便可查看相关的数据

如下图：

![image-20220924155542394](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220924155542394.png)

## 1.2 资源的请求方式

客户端请求服务器时，请求方式有很多种，常见的两种请求方式分别为 get 和 post 请求

- get 请求常用与<font color="red">获取服务端资源</font>
  - 例如 ：根据 URL 地址，从服务器获取 HTML 文件、CSS 文件、JS 文件、图片文件、数据资源等
- post 请求通常用于向服务器提交数据（往服务器发送资源）
  - 例如：登录时向服务器提交的登录信息、注册时向服务器提交的注册信息、添加用户时向服务器提交的用户信息等各种数据提交操作

# 2 了解 Ajax

## 2.1 什么是 Ajax

Ajax 的全称是 Asynchronous Javascript And Xml （异步的 JS 和 XML）

通俗的理解：在网页中利用 XMLHttpRequest 对象和服务器进行数据交互的方式就是 Ajax

Ajax 能让我们轻松实现 网页 和 服务器 之间的数据交互

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/Ajax.jpg" alt="Ajax" style="zoom:50%;" />

## 2.2 Ajax 的典型应用场景

- 用户名检测：注册用户时，通过 Ajax 的形式，动态检测用户名是否被占用
- 数据分页显示：当点击页码值的时候，通过 Ajax 的形式。根据页码值动态刷新表格的数据
- 搜索提示：当输入搜索关键字时，通过 Ajax 的形式，动态加载搜索提示列表
- 数据的增删改查：数据的添加、删除、修改、查询操作，都需要通过 Ajax 的形式，来实现数据的交互



## 2.3 了解 jQuery 中的 Ajax

浏览器中提供了 XMLHttpRequest 用法比较复杂，所以 JQ 对 XMLHttpRequest 进行了封装，提供了一系列 Ajax 相关的函数，极大地降低了 Ajax 的使用难度

JQ 中发起 Ajax 请求常用的三个方法如下：

- $.get()
- $.post()
- $.ajax()

### 2.3.1 $.get() 函数的语法

```js
$.get(url,[data],[callback])
```

| 参数名   | 参数类型 | 说明                     |
| -------- | -------- | ------------------------ |
| url      | string   | 要请求的资源地址         |
| data     | object   | 请求资源期间要携带的参数 |
| callback | function | 请求成功时的回调函数     |

**使用 get 发起不带参数的请求**

```js
$.get('http://www.liulongbin.top:3006/api/getbooks', function (res) {
	console.log(res)
}
```

我们打开调试工具，选择 XHR 过滤，之后查看返回的数据，如下：

![image-20220924170811850](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220924170811850.png)

**使用 get 发起带参数的请求**

```js
$.get('http://www.liulongbin.top:3006/api/getbooks', { id: 1 }, function (res) {
  console.log(res)
}
```

![image-20220924190722289](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220924190722289.png)

### 2.3.2 使用 post 发起的 POST 请求

JQ 中提供的 `$.post()`函数的功能单一，专门用来发起 POST 请求，从而向服务器提交数据

`$.post()`函数的语法如下：

```js
$.post(url,[data],[callback])
```

| 参数名   | 参数类型 | 说明                   |
| -------- | -------- | ---------------------- |
| url      | string   | 提交的数据地址         |
| data     | object   | 要提交的数据           |
| callback | function | 数据提交成功的回调函数 |

**使用 `$.post()` 向服务器提交数据示例**

```js
$.post('http://www.liulongbin.top:3006/api/addbook', { bookname: '水浒传', author: '施耐庵', publisher: '天津图书出版社' }, function (res) {
  console.log(res)
})
```

### 2.3.3 $.ajax() 函数的语法

相比于 `$.get()`  和  `$.post()` 函数，JQ 中提供的 `$.ajax()` 函数，是一个比较综合的函数，它允许我们对 Ajax 请求进行更详细的配置

`$.ajax()`函数的基本语法如下：

```js
$.ajax(){
	type: '',	// 请求的方式，例如 post 和 get
	url: '',	// 请求的 URL 地址
	data: {},	// 携带的数据
	success: function(){} 	// 请求成功之后的回调函数
}
```



# 3 接口

使用 Ajax 请求数据时，被请求的 <font color="red">URL 地址</font>，就叫做<font color="red">数据接口</font>(简称为接口)，同时，每个接口必须有请求方式

## 3.1 接口测试工具

接口测试工具：为了验证接口能否被正常访问，我们常常需要使用接口测试工具来对数据接口进行检测

好处：接口测试工具可以让我们在不写任何代码的情况下，对接口进行调用和测试

这里介绍接口测试工具 ：[postman](https://www.postman.com/downloads/)

**使用 postman 测试 GET 接口**

步骤：

1. 选择请求的方式：GET
2. 填写请求的 URL 地址
3. 填写请求的参数
4. 点击 Send 按钮发起 GET 请求
5. 查看服务响应的结果

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220924202437842.png" alt="image-20220924202437842" style="zoom: 25%;" />

**使用 postman 测试 post 接口**

步骤：

1. 选择请求的方式
2. 填写请求的 URL 地址
3. 选择 Body 面板并勾选数据格式
4. 填写要发送到服务器的数据
5. 点击 Send 按钮发起 POST 请求
6. 查看服务器的响应结果

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220924204323313.png" alt="image-20220924204323313" style="zoom:25%;" />

## 3.2 什么是接口文档

接口文档，顾名思义就是<font color="red">接口的说明文档，它是我们调用接口的依据</font>。好的接口文档包含了对<font color="blue">接口 URL、参数以及输出内容</font>的说明，我们参照接口文档就能方便地知道接口的作用，以及接口如何调用

### 3.2.1 接口文档的组成部分

一个合格的接口文档，应该包含一下六项内容，从而为接口的调用提供依据：

1. <font color="red">接口名称</font>：用来标识各个接口的简单说明，如：<font color="blue">登录接口，获取图书列表接口</font>
2. <font color="red">接口 URL</font>：接口的调用地址
3. <font color="red">调用方式</font>：接口的调用方式，如 GET 或 POST
4. <font color="red">参数格式</font>：接口需要传递的参数，每个参数必须包含<font color="blue">参数名称、参数类型、是否必选、参数说明</font>这4项内容
5. <font color="red">响应格式</font>：接口的返回值的详细描述，一般包含<font color="blue">数据名称、数据类型、说明</font>3项内容
6. 返回示例（可选）：通过对象的形式，列举服务器的返回数据的结构

接口文档示例：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220924212036276.png" alt="image-20220924212036276" style="zoom:50%;" />

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220924212151418.png" alt="image-20220924212151418" style="zoom:50%;" />

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220924212218959.png" alt="image-20220924212218959" style="zoom:50%;" />



# 案例：图书管理

