# 1 img 里的 src 属性中字符串拼接

在 Vue 中，而且是在脚手架环境下，如果需要对 img 里的 src 属性值进行字符串拼接，需要借助 common.js 的语法，如下：

```js
<img :src="require(`../../assets/food/pic${index}.jpg`)" class="image">
```

:herb:**拓展：class 和 style 属性值的字符串拼接**

```js
// class 属性值的字符串拼接，以下示例是在vue cli搭配element实现不同的值更换不同的图标
:class="scope.row.pass?'el-icon-circle-check':'el-icon-circle-close'"

// style 属性值的字符串拼接，以下示例是在vue cli搭配element实现不同的值使用不同的颜色
:style="{color:(scope.row.pass == 1)?'#67C23A':'#F56C6C'}"
```



# 2 Vue-Element-Admin 项目启动报错

报错原因：**node.js 版本问题**

>node.js V17版本中最近发布的 OpenSSL3.0, 而OpenSSL3.0对允许算法和密钥大小增加了严格的限制，可能会对生态系统造成一些影响。故此以前的项目在升级 node.js 版本后会报错。

解决方法：

<strong style="color:red">修改 package.json，在启动构建相关命令之前添加前缀：`SET NODE_OPTIONS=--openssl-legacy-provider &&`</strong>

例如：

```json
"scripts": {
   "serve": "SET NODE_OPTIONS=--openssl-legacy-provider && vue-cli-service serve",
   "build": "SET NODE_OPTIONS=--openssl-legacy-provider && vue-cli-service build"
},
```

```json
"scripts": {
   "dev": "SET NODE_OPTIONS=--openssl-legacy-provider && vue-cli-service serve",
 },
```



<strong style="color:red">第二种解决方案就是：将 node.js 回退到 V16 版本</strong>



# 3 解决Refused to apply style xxx 

完整的代码错误提示信息为：

```
Refused to apply style from 'http://localhost:8080/iconfont.css' because its MIME type ('text/html') is not a supported stylesheet MIME type, and strict MIME checking is enabled
```

出现原因，在 Vue 脚手架中，直接在 index.html 中引入外部 css，如下：

```html
<link rel="stylesheet" href="<%= BASE_URL %>reset.css">
```

解决方法：**使用 `type = "text/html"` 指明文件类型即可**

```html
<link rel="stylesheet" type="text/html" href="<%= BASE_URL %>reset.css">
```

推荐写法：<strong style="color:orange">在 public 文件夹下，我们一般将一些静态资源放在 static 文件夹下，然后引入</strong>

