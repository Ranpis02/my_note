# 1 Ajax 基本介绍

1. Ajax   即 "Asynchronous Javascript And XML" (异步JavaScript 和 XML)
2. Ajax   是一种浏览器异步发起请求(指定发哪些数据)，局部更新页面的技术



## 1.1 Ajax 应用场景

1.    搜索引擎根据用户输入关键字，自动提示检索关键字
2.    动态加载数据，按需取得数据【树形菜单、联动菜单...】
3.    改善用户体验。【输入内容前提示、带进度条文件上传...】
4.    电子商务应用。    【购物车、邮件订阅...】
5.    访问第三方服务。【访问搜索服务、rss 阅读器】
6.    页面局部刷新, https://piaofang.maoyan.com/dashboard

提炼一下，Ajax 发挥的最大的核心作用有如下几点：

1. 异步请求
2. 发送指定数据
3. 局部刷新



## 1.2 传统的 Web 请求和 Ajxa 请求对比

**传统 Web 请求**

![image-20230317231821932](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230317231821932.png)



**Ajax 请求**

![image-20230317232112382](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230317232112382.png)



# 2 Ajax 的使用

## 2.1 应用实例—验证用户名是否存在

要求：

1. 在输入框中输入用户名
2. 点击验证用户名，使用 Ajax 方式，服务端验证用户名是否已经被占用，如果该用户已经被占用，则以 JSON 格式返回该用户信息
3. 假定用户名为 admin 就不可用，其它用户名可用（后面会通过接入 DB 实现【MySQL + JDBC】）
4. 对页面进行局部刷新，显示返回信息

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230327151831285.png" alt="image-20230327151831285" style="zoom: 50%;" />

**示例**

```java
// CheckUserNameServlet
//接收ajax提交的数据
String userName = request.getParameter("userName");
// 为响应数据设置好类型
response.setContentType("text/html; charset=utf-8");
if ("admin".equals(userName)) {
    // 这里是模拟使用（后面数据都是从数据库中拿到）
    User admin = new User(1, "admin", "123", "xxx@qq.com");
    //  返回响应数据
    response.getWriter().write(new Gson().toJson(admin));
    // 注意：这里使用 Gson ,需要我们在 WEB_INF/lib 中引入 Gson 的 jar 包
} else {
    // 如果用户名不为 admin, 即可用，则返回空串
    response.getWriter().write("");
}

```

```html
<!-- register.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>register</title>
</head>
<script type="text/javascript">
    window.onload = function() {
        let checkUname = document.getElementById("checkUserName");
        checkUname.addEventListener('click', function() {
            // 1. 创建XMLHttpRequest对象（ajax引擎）
            let xhr = new XMLHttpRequest();
            // 获取用户填写的用户名
            let uname = document.getElementById("uname").value;
            // 2.准备发送指定数据
            /**
             * "GET" 表示请求的方式
             * 参数2即发送数据的url
             * 参数3为布尔值，为true代表异步发送，默认为false（同步发送）
             */
            xhr.open("GET", "/ajax/CheckUserServlet?userName=" + uname, true);

            // 3. 真正发送ajax请求（ajax）请求
            xhr.send();
            // 注意：这里由于是 get 请求，所以不需要携带请求体; 如果是 post 请求，则需要传入参数

            // 4. 对响应的数据进行处理
            xhr.onreadystatechange = function() {
                if(xhr.readyState == 4 && xhr.status == 200) {
                    // 其中，我们需要的是 xhr 中的 responseText
                    let text = xhr.responseText;
                    let myRes = document.getElementById("myRes");
                    if(text != "") {
                        myRes.value = "用户名不可用！";
                    } else {
                        myRes.value = "用户名可用！！";
                    }
                }
            }
        })
    }
</script>
    <body>
        <h1>用户注册</h1>
        <form action="/ajax/CheckUserServlet" method="post">
            用户名: <input type="text" name="userName" id="uname">
            <input type="button" value="验证用户名" id="checkUserName">
            <input type="text" style="border-width: 0; color:red" id="myRes"><br><br>
            用户密码: <input type="password" name="password"><br><br>
            电子邮件: <input type="text" name="email"><br><br>
            <input type="submit" value="注册">
        </form>
    </body>
</html>
```

> - responseText：响应数据的字符串形式
> - responseXML：响应数据的 XML 格式



## 2.2 onreadystatechange 事件

当请求被发送到服务器时，我们需要执行一些基于响应的任务。

每当 readyState 改变时，就会触发 onreadystatechange 事件。

readyState 属性存有 XMLHttpRequest 的状态信息。

下面是 XMLHttpRequest 对象的三个重要的属性：

| 属性               | 描述                                                         |
| ------------------ | ------------------------------------------------------------ |
| onreadystatechange | 存储函数（或函数名），每当 readyState 属性改变时，就会调用该函数。 |
| readyState         | 存有 XMLHttpRequest 的状态。从 0 到 4 发生变化。0: 请求未初始化；1: 服务器连接已建立；2: 请求已接收；3: 请求处理中；4: 请求已完成，且响应已就绪 |
| status             | 200: "OK"404: 未找到页面                                     |

在 onreadystatechange 事件中，我们规定当服务器响应已做好被处理的准备时所执行的任务。

当 readyState 等于 4 且状态为 200 时，表示响应已就绪：

```js
xmlhttp.onreadystatechange=function()
{
    if (xmlhttp.readyState==4 && xmlhttp.status==200)
    {
    	document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
    }
}
```



# 3 jquery 编写 Ajax

## 3.1 $.ajax 

该方法的常用参数：

1. `url`：请求的地址

2. `type`：请求方式，例如：`get` 或 `post`

3. `data`：发送到服务器的数据。将自动转换为请求字符串的格式

   > 如 {foo:["bar1", "bar2"]} 转换为 '&foo=bar1&foo=bar2'

4. `success`：成功的回调函数

5. `error`：失败的回调函数

6. `dataType`：返回的数据类型

   - `xml`：返回 XML 文档
   - `json`：返回 json 数据
   - `text`：返回纯文本字符串
   - `html`：返回纯文本 HTML 信息



## 3.2 get 和 post

`$.get` 和 `$.post` 方法底层其实还是用到了 `$.ajax` 方法，不过相比于 `$.ajax`，简化了请求方法等一些配置信息 

`$.get` 和 `$.post` 方法常用参数：

1. `url`：请求的 URL 地址
2. `data`：请求发送到服务器的数据
3. `success`：成功时的回调函数
4. `type`：返回的内容格式，例如：xml、html、script、json、text

`$.get` 语法如下：

```js
$(selector).get(url,data,success(response,status,xhr),dataType)
```

完整写法：

```js
$.ajax({
  url: url,
  data: data,
  success: success,
  dataType: dataType
});
```

> 其中，url 为必选项，post 与 get 用法基本一致



## 3.3 $.getJSON

`$.getJson()` 语法如下：

```js
jQuery.getJSON(url,data,success(data,status,xhr))
```

同样地，该函数也是简写的 Ajax 函数，等价于：

```js
$.ajax({
  url: url,
  data: data,
  success: callback,
  dataType: json
});
```



## 3.4 演示使用 jquery 发送 Ajax 请求

首先，我们需要在 web 目录下创建 script 文件夹，将我们的 jquery.min.js 文件放入其中，然后 【rebuild project】

> 需要 rebuild 的原因为：整个 web 工程的真正工作目录实际上是在 out 目录下，所以当我们直接拷贝文件到 web 目录中时，需要重新编译，在 out 中生成对应的脚本文件

**示例**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./script/jquery.min.js"></script>
    <script type="text/javascript">
        $(function () {
            $('#test').on('click', () => {
                $.ajax({
                    url: '/ajax/CheckUserServlet2',
                    type: 'post',
                    data: {
                        userName: $('#uname').val(),
                        date: new Date(),
                    },
                    error: () => {
                        console.log("fail...")
                    },
                    /**
                     *
                     * @param data 成功时返回的数据
                     * @param status 返回内容的状态码
                     * @param xhr   XMLHttpRequest对象
                     */
                    success: (data, status, xhr) => {
                        console.log("success...");
                        console.log(data)
                        console.log(status)
                        console.log(xhr)
                    },
                    dataType: 'json'
                })
            })
        })
    </script>
</head>
<body>
<h1>用户登录</h1>
<form action="#">
    用户名: <input type="text" name="uname" id="uname"><br><br>
    密码: <input type="text" name="pwd" id="pwd"><br><br>
<!--    <input type="submit" value="登录"><br><br>-->
    <button id="test">测试</button>
</form>
</body>
</html>
```

```java
//CheckUserServlet.java
String userName = request.getParameter("userName");
System.out.println(userName);
response.setContentType("text/json; charset=utf-8");	// 注意设置MIME为text/json，使得返回的数据可以被正常解析
response.getWriter().write(new Gson().toJson(new User(1, "张三", "666", "xxx@gmail.com")));
```



**get、post、getJSON 演示代码**

```js
$.get('/ajax/CheckUserServlet2', {
    userName: '张三',
    date: new Date()
}, function(response, status, xhr) {
    console.log(status)
    console.log(xhr)
}, 'json')
```

> get 和 post 方法中去除了失败的回调函数，我们可以通过 status 进行判断（如果 status 的值为 success，则执行成功，否则失败）



```js
$.getJSON('/ajax/CheckUserServlet2', {
    userName: '张三',
    date: new Date()
}, function(response, status, xhr) {
    console.log(status)
    console.log(xhr)
})
```

> getJson 相较于 get 和 post 请求，少了 dataType 属性的添加

