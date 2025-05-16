> 之前在学校听黑马的node.js由于看的进度比较快，没有完全整明白，所以现在趁着寒假来看看千锋的node.js，希望这一次是带着脑子听的

[toc]

# 1 node.js 基础

## 1.1 认识 node.js

Node.js是一个<strong style="color:orange">javascript运行环境</strong>。它让javascript可以开发后端程序，实现几乎其他后端语言实现的所有功能，可以与PHP、Java、Python、.NET、Ruby等后端语言平起平坐。

Nodejs是<strong style="color:orange">基于V8引擎</strong>，V8是Google发布的开源JavaScript引擎，本身就是用于Chrome浏览器的js解释部分，但是Ryan Dahl 这哥们，鬼才般的，把这个V8搬到了服务器上，用于做服务器的软件。

**浏览器环境 VS node 环境**

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221229204930344.png" alt="image-20221229204930344" style="zoom: 50%;" />

<strong style="color:red">Node.js 可以解析JS代码（没有浏览器安全级别的限制）提供很多系统级别的API</strong>，如：

- 文件的读写 (File System)

  ```js
  const fs = require('fs')
  
  fs.readFile('./ajax.png', 'utf-8', (err, content) => {
    console.log(content)
  })
  ```

- 进程的管理 (Process)

  ```js
  function main(argv) {
    console.log(argv)
  }
  
  main(process.argv.slice(2))
  ```

- 网络通信 (HTTP/HTTPS)

  ```js
  const http = require("http")
  
  http.createServer((req,res) => {
    res.writeHead(200, {
      "content-type": "text/plain"
    })
    res.write("hello nodejs")
    res.end()
  }).listen(3000)
  ```



## 1.2 node 环境的安装

 直接到官网进行下载安装即可：[官网传送门](https://nodejs.org/)

**验证安装**

```js
node -v
```



## 1.3 模块、包和commonJS

:question:**为什么要有模块化开发**

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220210100015768.png" alt="image-20220210100015768"  />



**CommonJS 规范**

![image-20220210101652166](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220210101652166.png)

![image-20220210101720533](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220210101720533.png)



:star:**modules 模块化规范写法**

我们可以把公共的功能抽离为一个单独的 JS 文件，作为一个模块。默认情况下，这个模块里面的方法或属性，外面是没法访问的。如果要让外部可以访问模块里面的方法或者属性，就必须在模块里面通过 `exports` 或者 `module.exports` 暴露属性或方法

示例代码如下：

```js
// m1.js
const customName = 'bob'

const sayName = () => {
    console.log(customName)
}

console.log('module_1')

// 接口暴露方法一
module.exports = {
    say:sayName
}

// 接口暴露方法二
exports.say = sayName

// 错误写法
exports = {
    say: sayName
}
```

```js
// main.js
const m1 = require('./m1')
m1.say()
```



## 1.4 npm 的使用

```bash
# 初始化项目, 同时生成 package.json 文件用来管理包
npm init

# 安装包, -g 代表全局安装
npm install 包名 -g 
# --save 或者 -S 代表记录项目在运行时需要的包，放到 dependencies 节点中, --save-dev 或 -D 代表记录项目在开发过程中过程中需要使用的包, 放到 devDependencies 节点

# 安装指定包名
npm install 包名@版本号

# 卸载包
npm uninstall 包名

# 更新包
npm update 包名

# 列出当前目录下的安装包, 加了 -g 代表查看所有全局安装的包
npm list [-g]

# 列出包的详细信息
npm info 包名

# 列出包的版本号
npm info 包名 version

# 检查包是否已经过时
npm outdated
```

> :warning:日常开发过程中吗，默认局部安装，如果我们使用 `-g` 选项进行全局安装后，将在 package.json 文件不存记录，这就意味着后续开发者就没有办法通过 `npm install` 来下载完整依赖

:sparkles:**小细节**

当我们使用 **`npm outdated`** 命令去查看包是否过时时，我们可以看到下面的信息：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221230121320992.png" alt="image-20221230121320992" style="zoom:50%;" />

这里有一个 Wanted 字段，这个字段代表在 package.lock.json 文件中锁住的包的版本号，这个版本号可能是与 Current 当前工程中所使用的包的版本号不同，这个主要需要取决于：

里面的字符，如下：

```js
"dependencies": {
  "md5": "^1.0.1"		// "^" 代表直接npm install安装依赖时将会安装 md5 1.*.* 的最新版本，即大版本不变，小版本升级到最新
}
```

```js
"dependencies": {
  "md5": "~1.0.1"		// "~" 代表如果npm install 将会保持版本号的前两位，最后一位保持最新 
}
```

```js
"dependencies": {
  "md5": "*"		// "*" 代表如果npm install 将会保持版本最新
}
```



## 1.5 nrm 的使用

项目所依赖的包我们不使用全局安装，而仅对自己本地开发有帮助的包的我们需要使用全局安装，例如：nrm

> NRM（npm registry manager）是 npm 的镜像源管理工具，有时候国外资源下载太慢，使用这个我们就可以快速地在 npm 源间进行切换

**全局安装 nrm**

```bash
npm i nrm -g
```

> 如果 node.js 安装在默认路径 C 盘中，那么我们如果想要全局安装，需要使用管理员身份打开 CMD

**使用 nrm**

```bash
# 查看可选源
nrm ls

# 切换源,以切换淘宝源为例
nrm use taobao
```

**测试镜像速度**

```bash
nrm test
```

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221230134918189.png" alt="image-20221230134918189" style="zoom:50%;" />

> 带 `*` 的为当前所使用的镜像源

:herb:**扩展**

在中国大陆，我们除了使用 npm 命令，还可以使用 cnpm 命令进行替代

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220210114017616.png" alt="image-20220210114017616" style="zoom:50%;" />

```bash
npm install -g cnpm --registry=https://registry.npmmirror.com
```



## 1.6 yarn 的使用

<strong style="color:orange">yarn: 英[jɑːn]</strong>

**安装**

```bash
npm install -g yarn
```

```
对比npm:
	速度超快: Yarn 缓存了每个下载过的包，所以再次使用时无需重复下载。 同时利用并行下载以最大化资源利用率，因此安装速度更快。
    超级安全: 在执行代码之前，Yarn 会通过算法校验每个安装包的完整性。

开始新项目
	yarn init 
添加依赖包
	yarn add [package] 
	yarn add [package]@[version] 
	yarn add [package] --dev 
升级依赖包
	 yarn upgrade [package]@[version] 
移除依赖包
	 yarn remove [package]
	 
安装项目的全部依赖
	 yarn install 
```



## 1.7 nodemon 的使用

在之前，我们每次构建服务器后，都要通过重新开启服务器，服务器才能生效，这样显然是相当麻烦的，不利于测试，为了更好地满足我们平时开发的需求，我们可以借助 nodemon 这个工具:

<strong style="color:red">nodemon 可以检测到目录中的文件更改时自动重新启动节点应用程序来帮助开发基于 node.js 的应用程序</strong>

**安装**

```bash
npm i nodemon -g
```

**启动服务器**

```bash
nodemon fileName
```

:herb:**扩展**

除了 nodemon 之外，我们还有 node-dev 这个工具，安装和使用方法同 nodemon，同样需要全局安装

```bash
npm i node-dev -g
```



# 2 模块化开发和内置模块的使用

## 2.1 ES 模块化写法

在 node.js 中，默认使用的模块化开发方式都是 common.js，使用 `export` 暴露，使用 `require` 接收，当然我们也可以换一种模块化开发，即使用 ES 语法规范，我们需要在 package.json 中添加如下字段：

```js
"type": "module",
```

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221230145053001.png" alt="image-20221230145053001" style="zoom:50%;" />

> type 的值默认是 commonjs，即 common.js 的语法规范

示例代码如下：

```js
// m1.js
const customName = 'bob'

const sayName = () => {
    console.log(customName)
}
// 默认暴露
export default sayName

// 分别暴露 
export {
    sayName
}
```

```js
// main.js
// 默认导入
import sayName from './m1.js';
// 分别导入
import {sayName} from './m1.js';
console.log(sayName);
```

> :warning:注意：使用 ES 模块化导入时，扩展名要补全，否则无法导入成功



## 2.2 内置模块

### 2.2.1 http 模块

> 要使用 HTTP 服务器和客户端，则必须先要导入 HTTP 模块

示例代码如下：

**文件目录结构**

```
server                 
├─ modules             
│  ├─ renderHTML.js    
│  └─ renderStatus.js  
└─ server.js           
```

```js
// server.js

// 导入 http 模块
const http = require('http')
// 导入自定义模块
const renderHTML = require('./modules/renderHTML')
const renderStatus = require('./modules/renderStatus')
// 创建服务器
    /*
    * @intro:接收浏览器传递的参数，返回渲染的内容
    * @req: request 请求体
    * @res: response 响应体
    */
http.createServer((req, res) => {
    /*
    * @res.writeHead(): 写入响应头
    * @res.write(): 写入响应内容
    * @res.end(): 结束响应
    * @req.url: 获取请求的URL地址
    */
   res.writeHead(renderStatus.renderStatus(req.url), {"Content-Type":"text/html;charset=utf-8"})  // 设置响应的文本类型为 html，编码字符集为 utf-8,请求的状态码随请求的 URL 发生改变
   res.write(renderHTML.renderHTML(req.url))
   res.end()
}).listen(3000, () => {
    // 服务器创建成功之后的回调
    console.log('server start');
})
```

```js
// renderHTML.js

function renderHTML(_url) {
    switch(_url) {
        case '/home':
            return `
                <html>
                    <strong>我是home页面</strong>
                </html>
            `
        case '/index':
            return `
                <html>
                    <strong>我是index页面</strong>
                </html>
            `
        default:
            return `
                <html>
                    404 NOT FOUND
                </html>
            `
    }
}

module.exports = {
    renderHTML
}
```

```js
// renderStatus.js

function renderStatus(_url) {
    const normalURL = ['/home', '/index']
    return normalURL.includes(_url) ? 200:404
}

module.exports = {
    renderStatus
}
```



针对上述的 server.js ，我们还有第二种写法

```js
// 导入 http 模块
const http = require('http')
// 创建服务器
const server = http.createServer()
// 事件监听
server.on('request', (req, res) => {
    // 监听处理
})
// 开启服务器
server.listen(3000, () => {
    console.log('server start');
})
```

> 上述代码存在一点问题：就是如果我们请求的 URL 中携带了参数，输入对应的 URL，服务器无法正常识别 URL，而返回了 404 NOT FOUND



### 2.2.2 url 模块

#### 01 parse() 方法

```js
const url = require('url')
const urlString = 'https://www.baidu.com:443/ad/index.html?id=8&name=mouse#tag=110'
const parseStr = url.parse(urlString)
console.log(parseStr);
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221230202725683.png" alt="image-20221230202725683" style="zoom:50%;" />

> 通过 `parse()` 方法，将 URL 解析成为了一个 Url 对象，如果我们想要该 URL 中的 query 参数，可以直接通过 `.query` 来获取，其他的同理

:herb:**补充**

我们通过上述代码可以看到，我们虽然可以得到 URL 的 query 参数，但很可惜，**不是一个对象格式**

其实，<strong style="color:red">该方法是有第二个参数的，第二参数为布尔值，如果为 true，那么则将 query 转为一个对象，默认是 false，表示一般格式</strong>

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221230203720650.png" alt="image-20221230203720650" style="zoom:50%;" />



#### 02 format() 方法

> 该方法已经被弃用，了解即可，与 parse 是一个互逆的操作，用于将对象转化为一个 URL 对象

```js
const url = require('url')
const urlObject = {
  protocol: 'https:',
  slashes: true,
  auth: null,
  host: 'www.baidu.com:443',
  port: '443',
  hostname: 'www.baidu.com',
  hash: '#tag=110',
  search: '?id=8&name=mouse',
  query: { id: '8', name: 'mouse' },
  pathname: '/ad/index.html',
  path: '/ad/index.html?id=8&name=mouse'
}
const parsedObj = url.format(urlObject)
console.log(parsedObj)
```



#### 03 resolve

```js
var a = url.resolve('/one/two/three', 'four')  // 注意最后加'/' ，不加'/'的区别 
var b = url.resolve('http://example.com/', '/one')
var c = url.resolve('http://example.com/one', '/two')
```

打印输入的结果如下：

```
/one/two/four
http://example.com/one
http://example.com/two
```



### 2.2.3 新版url 模块

上一节中，我们学习到的 url 方法都属于旧版，并且目前 `format()` 和 `resolve()` 方法已经被抛弃，我们可以到 node.js 中文网去进行查看

> node.js 中文网：[传送门](https://www.nodeapp.cn/)

对于新版的 URL 的一些方法和特性，我们[参考文档](https://www.nodeapp.cn/url.html)来记录一些比较重要的内容

新版 url 对象中通过构造函数内容来创建，语法如下：

```js
new URL(input[, base])
```

- `input`：要解析的 URL
- `base`：如果 `input` 是相对 URL，那么 base 则为基本 URL

例如：

```js
const { URL } = require('url');
const myURL = new URL('/foo', 'https://example.org/');  
// https://example.org/foo
```

此外，如果 `input`  或 `base` 是无效 URL，将会抛出 `TypeError`

存在于 `input` 主机名中的 Unicode 字符将被使用 Punycode 算法自动转换为 ASCII

我们将创建出来的对象打印一下，看下对象中拥有的参数：

```js
const url = require('url')
const { URL } = require('url');
const myURL = new URL('/foo', 'https://example.org/');
console.log(myURL);
```

打印结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221230225536376.png" alt="image-20221230225536376" style="zoom:50%;" />

- 其中 query 参数可以通过迭代器属性的 searchParams 来获取



新的 URL 对象中也有 `format()` 方法，语法如下：

```js
url.format(URL[, options])
```

- `URL`：一个 WHATWG URL 对象
- `options`：<Object>
  - `auth` ：<boolean>，如果序列化的URL字符串应该包含用户名和密码为`true`，否则为`false`。默认为`true`。
  - `fragment` ：<boolean>，如果序列化的URL字符串应该包含分段为`true`，否则为`false`。默认为`true`。
  - `search` ：<boolean>，如果序列化的URL字符串应该包含搜索查询为`true`，否则为`false`。默认为`true`。
  - `unicode` ：<boolean>， 如果出现在URL字符串主机元素里的Unicode字符应该被直接编码而不是使用Punycode编码为`true`，默认为`false`。



### 2.2.4 querystring 模块

#### 01 parse()

`parse()` 方法用于将表单编码格式（FormUrlEncoded）转换为一个对象

```js
const querystring = require('querystring')
var qs = 'x=3&y=4'
var parsed = querystring.parse(qs)
console.log(parsed)
```

执行结果如下：

```
[Object: null prototype] { x: '3', y: '4' }
```



#### 02 stringify()

`stringify()` 方法用于将对象转换为表单编码格式

```js
const querystring = require('querystring')
var qo = {
  x: 3,
  y: 4
}
var parsed = querystring.stringify(qo)
console.log(parsed)
```

执行结果如下：

```
x=3&y=4
```



#### 03 `escape()` 和 `unescape()`

这两个方法用于将字符串进行编码和解码，默认情况下，`stringify()` 调用了 `escape()`，而 `parse()` 调用了 `unescape()`

这两个函数通常不直接使用，这里不过多介绍



### 2.2.5 event 模块

event 模块用于对事件进行进行**监听和触发**

```js
const EventEmitter = require('events')
const event = new EventEmitter()

event.on('play', (data) => {
    console.log('play事件触发了,得到的数据有：' + data);
})

setTimeout(() => {
    event.emit('play', 123)
})
```



### 2.2.6 fs 模块

fs 模块用于**对文件进行操作**

```js
const fs = require('fs')

// 创建文件夹
fs.mkdir('./logs', (err) => {
  console.log('done.')
})

// 文件夹改名
fs.rename('./logs', './log', () => {
  console.log('done')
})

// 删除文件夹
fs.rmdir('./log', () => {
  console.log('done.')
})

// 写内容到文件里
fs.writeFile(
  './logs/log1.txt',
  'hello',
  // 错误优先的回调函数
  (err) => {
    if (err) {
      console.log(err.message)
    } else {
      console.log('文件创建成功')
    }
  }
)

// 给文件追加内容
fs.appendFile('./logs/log1.txt', '\nworld', () => {
  console.log('done.')
})

// 读取文件内容
fs.readFile('./logs/log1.txt', 'utf-8', (err, data) => {
  console.log(data)
})

// 删除文件
fs.unlink('./logs/log1.txt', (err) => {
  console.log('done.')
})

// 批量写文件
for (var i = 0; i < 10; i++) {
  fs.writeFile(`./logs/log-${i}.txt`, `log-${i}`, (err) => {
    console.log('done.')
  })
}

// 读取文件/目录信息
fs.readdir('./', (err, data) => {
  data.forEach((value, index) => {
    fs.stat(`./${value}`, (err, stats) => {
      // console.log(value + ':' + stats.size)
      console.log(value + ' is ' + (stats.isDirectory() ? 'directory' : 'file'))
    })
  })
})

// 同步读取文件
try {
  const content = fs.readFileSync('./logs/log-1.txt', 'utf-8')
  console.log(content)
  console.log(0)
} catch (e) {
  console.log(e.message)
}

// 同步创建文件
const fs = require('fs')

try {
    fs.mkdirSync('./filefolder')
} catch(e) {
    console.log(e);
}

// 同步删除文件
const fs = require('fs')

try {
    fs.unlinkSync('a.txt')
} catch(e) {
    console.log(e);
}

// 异步读取文件：方法一
fs.readFile('./logs/log-0.txt', 'utf-8', (err, content) => {
  console.log(content)
  console.log(0)
})
console.log(1)

// 异步读取文件：方法二
const fs = require("fs").promises
fs.readFile('./logs/log-0.txt', 'utf-8').then(result => {
  console.log(result)
})

```

:question:**在 `fs` 模块中，提供同步方法是为了方便使用，那我们到底应该是使用异步方法还是同步方法呢**

> 由于 node 环境下执行的 JS 代码是服务器代理，所以，绝大部分需要在服务器运行期反复执行业务逻辑的代码，必须使用异步代码，否则，同步代码在执行时期，服务器将停止响应，因为 JS 只有一个执行线程。
>
> 服务器启动时如果需要读取配置文件，或者结束时需要写入到状态文件，可以使用同步代码，因为这些代码只在启动和结束时执行一次，不影响服务器正常运行时的异步执行。

**:herb:拓展小案例：现在我们有一个需求就是我们想要编写一个函数完成删除一个目录（包括子级文件，不考虑子级目录）**

```js
const fs = require('fs').promises
fs.readdir('./filefolder').then(async (data) => {
    let arr = []
    data.forEach(item => {
        arr.push(fs.unlink(`./filefolder/${item}`))
    })
    await Promise.all(arr)
    await fs.rmdir('./filefolder')
})
```



### 2.2.7 stream 流模块

`stream` 是 Node.js 中提供的一个仅在服务器端可用的模块，目的是<strong style="color:red">支持 "流" 这种数据结构</strong>

在 Node.js 中，流也是一个对象，我们只需要响应流的事件即可：`data` 事件表示流的数据已经可以读取了，`end` 事件表示这个流已经到底了，`error` 事件表示发生错误

```js
const fs = require('fs');
// 创建一个可读流
const rs = fs.createReadStream('./1.txt', 'utf-8')

rs.on('data', (chunk) => {
    console.log('DATA:', chunk);
})

rs.on('end', () => {
    console.log('----------END----------');
})

rs.on('error', err => {
    console.log(err);
})
```

:warning:注意：`data` 事件可能会有多次，每次传递的 `chunk` 是流的一部分数据

要以流的形式写入数据，只需要不断调用 `write()` 方法，最后以 `end()` 结束

```js
var fs = require('fs');

var ws1 = fs.createWriteStream('output1.txt', 'utf-8');
ws1.write('使用Stream写入文本数据...\n');
ws1.write('END.');
ws1.end();
```

`pipe` ，即 "管道"，用于将一个 `Readable` 流和 `Writable` 流串起来，所有的数据自动从 `Readable` 流进入 `Writable` 流，这种操作叫做 `pipe`

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220407085931744.png" alt="image-20220407085931744" style="zoom:50%;" />

```js
const fs = require('fs')

const readStream = fs.createReadStream('./1.txt')
const writeStream = fs.createWriteStream('./2.txt')

readStream.pipe(writeStream)
```



### 2.2.8 zlib 模块

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220407105916114.png" alt="image-20220407105916114" style="zoom:33%;" />



为了加快传输的速度，我们可以在先在服务器端对数据进行压缩，之后在浏览器中进行解压缩即可，这个时候我们就可以用到 zlib 模块

```js
const fs = require('fs')
const http = require('http')
const zlib =require('zlib')
const gzip = zlib.createGzip()
http.createServer((req, res) => {
    const readStream = fs.createReadStream('./1.js')
    res.writeHead(200, {"Content-Type":"application/javascript;charset=utf-8", "Content-Encoding":"gzip"})
    readStream.pipe(gzip).pipe(res)
}).listen(3000, () => {
    console.log('listen at http://localhost:3000');
})
```



### 2.2.9 crypto 模块

crypto 模块的目的是为了<strong style="color:red">提供通用的加密、解密和哈希算法。</strong>

#### 01 MD5 算法

MD5 是一种常用的哈希算法，可以用于数字签名，同时也是一个单向函数。

```
const crypto = require('crypto')
const hash = crypto.createHash('md5')

hash.update('Hello World')
// 将明文转化的 MD5 以十六进制形式显示
console.log(hash.digest('hex'));
```

- `update()` 方法默认字符串编码是 `UTF-8` ，也可以传入 buffer
- 如果想要计算 SHA1，只需要将 `md5` 改成 `sha1` 即可 



#### 02 Hmac 算法

Hmac 也是一种哈希算法，同样可以利用 MD5 或者 SHA1 算法，但是 Hmac 还需要一个秘钥

```js
const crypto = require('crypto')
let secret_key = '1234567890abcd'
const hmac = crypto.createHmac('sha256', secret_key)

hmac.update('hello world')

console.log(hmac.digest('hex'))
```



#### 03 AES 算法

AES 是一种对称加密算法，加解密都是用的同一个秘钥。crypto 模块提供了 AES 支持，但是需要自己封装好函数来调用：

```js
const crypto = require("crypto");

function encrypt (key, iv, data) {
    let decipher = crypto.createCipheriv('aes-128-cbc', key, iv);
    // decipher.setAutoPadding(true);
    return decipher.update(data, 'binary', 'hex') + decipher.final('hex');
}

function decrypt (key, iv, crypted) {
     crypted = Buffer.from(crypted, 'hex').toString('binary');
     let decipher = crypto.createDecipheriv('aes-128-cbc', key, iv);
     return decipher.update(crypted, 'binary', 'utf8') + decipher.final('utf8');
}
```

>key,iv必须是16个字节



## 2.3 http 模块 -- 解决跨域策略

在解决跨域问题之前，我们首先需要了解**同源策略（same-origin policy）**

**同源策略**是浏览器的一个重要的基本的安全策略，用来限制一个 origin 的文档或它加载的脚本与另一个源的资源的通信，<strong style="color:red">如果两个 URL的<u>协议、域名、端口</u>都相同，那么则代表同源；如果其中一项不同则代表不同源</strong>，如果我们在不同源之间进行访问则叫做**跨越访问**

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221231151924142.png" alt="image-20221231151924142" style="zoom:50%;" />

参考图例

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221231154909186.png" alt="image-20221231154909186" style="zoom:50%;" />

同源策略的限制范围：

1. Cookie，localStorage，IndexDB无法读取

2. DOM无法获得

3. AJAX请求不能发送

解决同源策略的方法：

1. jsonp
2. cors
3. 反向代理
4. postMessage



### 2.3.1 JSONP

JSONP解决跨域主要还是利用了<strong style="color:red">在 JS 中，允许 script 标签通过 src 属性引用别的源</strong>，这样一来就为跨源提供了可能

> JSONP 全称为 "JSON with Padding"

实现原理：

- 客户端事先设置好指定的数据处理函数
- 之后通过 script 标签引入服务器中 JSON 格式化的包裹真实数据的函数调用字符串

![image-20221231162829435](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221231162829435.png)

示例代码如下：

```js
// server.js 
const http = require('http')
const url = require('url')
const server = http.createServer((req, res) => {
    let urlObj = url.parse(req.url, true)
    switch(urlObj.pathname) {
        case '/index':
            res.end(`${urlObj.query.callback} (${JSON.stringify({
                name:"Alice",
                age:16
            })})`)
            break;
        default:
            res.end('404 NOT FOUND')
    }
}).listen(3000)
```

```html
<!-- client.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>client</title>
</head>
<body>
    <script>
        // 动态创建 script 标签
        var oscript = document.createElement("script");
        oscript.src = 'http://localhost:3000/index?callback=test'
        document.body.appendChild(oscript);
        function test(obj) {
            console.log(obj);
        }
    </script>
</body>
</html>
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221231181903622.png" alt="image-20221231181903622" style="zoom:50%;" />

> 这里有一个同步和异步的问题需要我们进行考虑，我们留到以后进行讨论

JSONP 本质还是利用了 script 的引入的漏洞，它的兼容性会更好一些，思路简单，同时绕开了浏览器的同源策略，但是存在一些令人诟病的缺点：

1. 只支持 GET 请求，不支持 POST 请求
2. 需要客户端和服务器两方面的相互配合，比较麻烦
3. 支持跨越 HTTP 请求的这种请求，不能解决不同域的两个页面之间如何进行 JS 调用的问题
4. JSONP 如果调用失败，也不会返回各种 HTTP 状态码
5. 最后就是安全性的问题，这点等以后深入开发后补充...



### 2.3.2 CORS

CORS（Cross-Origin Resource Sharing，跨域资源共享）是一个系统，它由一系列 HTTP 头组成，我们可以通过配置 HTTP 头来允许浏览器跨域请求访问到 Web 服务器的资源

现在，我们在客户端使用 fetch 发送请求：

```js
// 通过 fetch 来发送请求
fetch('http://localhost:3000/index')
.then(res => res.json())
.then(res => {
    console.log(res);
})
```

```js
// server.js
const http = require('http')
const url = require('url')
const server = http.createServer((req, res) => {
    let urlObj = url.parse(req.url, true)

    // 配置头部
    res.writeHead(200, {
        "Content-type": "application/json;charset=utf-8",
        // cors 头
        "access-control-allow-origin": "*"	// 允许所有origin进行访问
    })
    switch (urlObj.pathname) {
        case '/index':
            res.end(`${JSON.stringify({
                name:'张三',
                age:18
            })}`)
            break;
        default:
            res.end('404 NOT FOUND')
    }
}).listen(3000)
```

接着，我们就可以在浏览器中看到，控制台打印出收的的对象数据：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221231194354543.png" alt="image-20221231194354543" style="zoom:50%;" />



### 2.3.3 反向代理

定义：<strong style="color:red">反向代理服务器位于客户端与服务器之间，但是对于客户端来说，反向代理就相当于目标服务器，即客户端直接访问反向代理服务器即可获得目标服务器的资源</strong>

:spiral_notepad:**反向代理和正向代理**

- 正向代理：<strong style="color:orange">代理我们的客户端，由内部网络用户向外网发送请求，例如 VPN</strong>
- 反向代理：<strong style="color:orange">代理我们的服务器，由外部网络用户向内网发送请求，例如 Ngnix </strong>

我们以猫眼的接口为例：

```
https://m.maoyan.com/ajax/movieOnInfoList
```

> 该接口表示猫眼中正在热映的电影列表

```js
// proxy.js
const http = require('http')
const https = require('https')
const url = require('url')
const server = http.createServer((req, res) => {
    let urlObj = url.parse(req.url, true)

    // 配置头部
    res.writeHead(200, {
        "Content-type": "application/json;charset=utf-8",
        // cors 头
        "access-control-allow-origin": "*"
    })
    switch (urlObj.pathname) {
        case '/list':
            httpsGet((data) => {
                res.end(data)
            })
            break;
        default:
            res.end('404 NOT FOUND')
    }
}).listen(3000)
// 数据处理函数
function httpsGet(callback) {
    let data = ''
    https.get(`https://m.maoyan.com/ajax/movieOnInfoList`, (res) => {
        res.on('data', (chunk) => {
            data += chunk
        })

        res.on('end', () => {
            console.log(data);
            callback(data);
        })
    })
}
```

> :small_red_triangle:注意点：
>
> 1. 由于现在大部分接口都是 HTTPS，所以我们首先需要引入 https 模块，在数据处理函数中调用 `https.get()`（当然，http 模块也有 get 方法，这要依据接口所使用的的协议来定）
> 2. 在自己所写的数据处理函数（模块）中，添加了回调函数 callback，待函数处理完成之后可以直接在 server 中使用 res 将 data 返回到客户端



客户端发送请求

```js
// 通过 fetch 来发送请求
fetch('http://localhost:3000/list')
.then(res => res.json())
.then(res => {
    console.log(res);
})
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221231231010362.png" alt="image-20221231231010362" style="zoom:50%;" />



---

上面我们测试了 GET 请求，接下来也来测试一下 POST 请求

拿小米有品的官网中的一个 POST 请求为例：

```
https://m.xiaomiyoupin.com/mtop/market/search/placeHolder
```

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221231235740045.png" alt="image-20221231235740045" style="zoom:50%;" />

![image-20230101011544751](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230101011544751.png)

> 当然，以上写的请求官网随时都可能发生改变，所以我们需要自己到官网上去抓取

```js
// server.js
const http = require('http')
const https = require('https')
const url = require('url')
const server = http.createServer((req, res) => {
    let urlObj = url.parse(req.url, true)

    // 配置头部
    res.writeHead(200, {
        "Content-type": "application/json;charset=utf-8",
        // cors 头
        "access-control-allow-origin": "*"
    })
    switch (urlObj.pathname) {
        case '/list':
            httpsPost((data) => {
                res.end(data)
            })
            break;
        default:
            res.end('404 NOT FOUND')
    }
}).listen(3000)
// 数据处理函数
function httpsPost(callback) {
    let data = ''

    let options = {
        hostname:"m.xiaomiyoupin.com",
        port:"443",
        path:"/mtop/market/search/placeHolder",
        method:"POST",
        headers:{
            "Content-Type":"application/json"
        }
    }
    // 配置请求的 options
    let req = https.request(options, (res) => {
        res.on('data', chunk => {
            data += chunk
        })

        res.on('end', () => {
            callback(data)
        })
    })
    // 请求体里面写入请求的内容，并通过 end 方法结束写入
    req.write(JSON.stringify([{}, {"baseParam": {"ypClient": 1}}]))
    req.end()
}
```



# 3 路由

示例项目的工程结果如下：

```
router            
├─ static         
│  ├─ 404.html    
│  ├─ home.html   
│  └─ index.html  
└─ server.js      
```

```js
// server.js
const http = require('http')
// const route = require('./router')
const fs = require('fs')
http.createServer((req, res) => {
    const myURL = new URL(req.url, 'http://127.0.0.1:3000')
    // console.log(myURL);
    route(res, myURL.pathname)
    res.end()
}).listen(3000, () => {
    console.log('server start');
})

function route(res, pathname) {
    switch (pathname) {
        case '/index':
            res.writeHead(200, { 'Content-Type': 'text/html;charset=utf8' })
            res.write(fs.readFileSync('./static/index.html'), 'utf-8')
            break
        case '/home':
            res.writeHead(200, { 'Content-Type': 'text/html;charset=utf8' })
            res.write(fs.readFileSync('./static/home.html'), 'utf-8')
            break
        default:
            res.writeHead(404, { 'Content-Type': 'text/html;charset=utf8' })
            res.write(fs.readFileSync('./static/404.html'), 'utf-8')
    }
}
```

上述代码仅仅只完成了对静态资源的路由的配置，但是像接口和一些解耦操作都还没完成，完善版本的工程结构如下：

```
router            
├─ static         
│  ├─ 404.html    
│  ├─ home.html   
│  ├─ index.html  
│  └─ login.html  
├─ apiRounter.js  
├─ index.js       
├─ router.js      
└─ server.js      
```

```

```



## 3.2 express

### 3.2.1 介绍与安装

> express 中文网：[官网传送门](https://www.expressjs.com.cn/)

**安装**

```bash
npm install express --save
```

**基本使用**

```js
const express = require('express')

const app = express()

app.get('/', (req, res) => {
    res.send('<b>加粗字体</b>')
})

app.listen(3000, () => {
    console.log('server starting');
})
```

> `res.send()` 方法集成了 `res.write()` 和 `res.end()`，同时还会根据发送给客户端的内容来自动配置响应头



### 3.2.2 express -- 基本路由

路由指定了应用的端点（URIs）和如何响应客户端的请求。

路由是由一个 URI、HTTP 请求（GET 、POST 等）和若干个句柄组成，基本结构如下：

```js
app.method(path, [callback...], callback)
```

> - app 是 express 的一个实例对象
> - method 是指请求的方法
> - path 是指服务器上的对应的端点的路径
> - callback 是路由匹配时回调

路由路径和请求方法一起定义了请求的端点，它可以是字符串、字符串模式或者正则表达式

```js
// 匹配根路径的请求
app.get('/', function (req, res) {
  res.send('root');
});

// 匹配 /about 路径的请求
app.get('/about', function (req, res) {
  res.send('about');
});

// 匹配 /random.text 路径的请求
app.get('/random.text', function (req, res) {
  res.send('random.text');
});
```

**使用字符串模式的路由示例**

```js
// 匹配 acd 和 abcd
app.get('/ab?cd', function(req, res) {
  res.send('ab?cd');
});

// 匹配 /ab/******
app.get('/ab/:id', function(req, res) {
  res.send('aaaaaaa');
});

// 匹配 abcd、abbcd、abbbcd等
app.get('/ab+cd', function(req, res) {
  res.send('ab+cd');
});

// 匹配 abcd、abxcd、abRABDOMcd、ab123cd等
app.get('/ab*cd', function(req, res) {
  res.send('ab*cd');
});

// 匹配 /abe 和 /abcde
app.get('/ab(cd)?e', function(req, res) {
 res.send('ab(cd)?e');
});
```

**使用正则表达式的路由路径示例**

```js
// 匹配任何路径中含有 a 的路径：
app.get(/a/, function(req, res) {
  res.send('/a/');
});

// 匹配 butterfly、dragonfly，不匹配 butterflyman、dragonfly man等
app.get(/.*fly$/, function(req, res) {
  res.send('/.*fly$/');
});
```

> :star:补充
>
> 1. 字符串模式和正则匹配模式是有所不同，最典型的就是字符串模式属于精确模式，也就是说 `/a*b` 匹配的路由路径必须是以 a 开头，b 结尾的，另外还有 `*` 所使用的场景也略有所差异
> 2. `.*` 表示单个字符匹配任意次，开启的是一个贪婪模式，它会尽可能多地匹配多个字符，而如果在后面添加一个 `?` 即可将贪婪模式转换为最小匹配模式

可以为请求处理提供多个回调函数，其行为类似中间件。唯一的区别就是这些回调函数可能调用 `next('route')` 方法而略过其他路由回调函数。可以利用该机制为路由定义前提条件，如果在现有路径上继续执行没有意义，则可将控制权交给剩下的路径。

```js
app.get('/index', (req, res, next) => {
    console.log('response will be sent by the nect function...')
    next()
}, (req, res) => {
    res.send('Hello World')
})
```

使用回调函数数组处理路由

```js
var cb0 = function (req, res, next) {
  console.log('CB0')
  next()
}

var cb1 = function (req, res, next) {
  console.log('CB1')
  next()
}

var cb2 = function (req, res) {
  res.send('Hello from C!')
}

app.get('/example/c', [cb0, cb1, cb2])
```

混合使用函数和函数数据处理路由

```js
var cb0 = function (req, res, next) {
  console.log('CB0')
  next()
}

var cb1 = function (req, res, next) {
  console.log('CB1')
  next()
}

app.get('/example/d', [cb0, cb1], function (req, res, next) {
  console.log('response will be sent by the next function ...')
  next()
}, function (req, res) {
  res.send('Hello from D!')
})
```

:herb:**扩展：思考一下如何栈里面的回调函数如何相互传递参数？**

<u>答：可以放到 res 响应体或者 req 请求体中</u>

```js
app.get('/index', (req, res, next) => {
    console.log('response will be sent by the nect function...')
    res.myName = 'bob'	// 在 res 中添加一个 myName 属性
    next()
}, (req, res) => {
    console.log(res.myName);	// 在下一个 next 回调中使用
    res.send('Hello World')
})
```



### 3.2.3 express -- 中间件

express 是一个自身功能极简，完全是由路由和中间件构成的一个 web 开发框架。从本质上来说，一个 express 应用就是在调用各种中间件。

> 中间件（Middleware）是一个函数，它可以访问请求对象（request），响应对象（res），和 web 应用中处于请求 - 响应循环流程中的中间件，一般被命名为 next 的变量。

中间件的功能包括：

- 执行任何代码
- 修改请求和响应对象
- 终结请求 -- 响应循环
- 调用堆栈中的下一个中间件

如果当前中间件没有终结请求 -- 响应循环，则必须调用 `next()` 方法将控制权交给下一个中间件，否则请求就会挂起。

express 应用可使用如下几种中间件：

- 应用级中间件
- 路由级中间件
- 错误处理中间件
- 内置中间件
- 第三方中间件

#### （1）应用级中间件

```js
const express = require('express')

const app = express()
let fn1 = (req, res, next) => {
    // 验证用户token是否过期
    console.log('验证token');
    let isValid = true
    if(isValid) {
        req.name = 'Alice'
        next()
    } else {
        res.send('error')
    }
}
let fn2 = (req, res) => {
    // 查询数据库
    // 返回内容
    res.send(req.name);
}

app.get('/login', (req, res) => {
    res.send('login')
})
// 挂载应用级别中间件
app.use(fn1)

app.get('/home', [fn2])
app.get('/list', (req, res) => {
    res.send('list')
})

app.listen(3000, () => {
    console.log('server start');
})
```

> :warning:注意：
>
> 1. `app.use()` 只对后面的路由生效，而对前面的路由不可见
> 2. `app.use()` 不止有处理函数，还可以指定路由路径，表示对指定路由生效，如果缺省就全局生效，例如：`app.use('/home', fn1)`，那么 fn1 这个验证规则只对 `/home` 这个路由有效

#### （2）路由级别中间件

路由级别中间件本质上和应用级别中间件，它经常通过挂载到应用级别中间件上，以便更好地管理，我们也可以将其看成是"二级中间件"

**简单示例**

```
express               
├─ apiRouter.js 
└─ index.js               
```

```js
// index.js
const express = require('express')
const apiRouter = require('./apiRouter')
const app = express()

app.use('/api', apiRouter)

app.listen(3000, () => {
    console.log('server start');
})
```

```js
// apiRouter.js
const express = require('express')
const router = express.Router()

router.get('/home', (req, res) => {
    res.send('home')
})

router.get('/index', (req, res) => {
    res.send('index')
})

module.exports = router
```

通过 `/api/home` 或者 `/api/index` ，我们可以访问到对应的路由



#### （3）错误中间件

错误中间件和其他中间定义类似，只不过要用到四个参数，分别是：err、req、res、next

**示例一**

```js
const express = require('express')
const app = express()
app.get('/index', (req, res) => {
    res.send('index')
})

app.use((req, res) => {
    res.status(404).send('something broken')
})

app.listen(3000, () => {
    console.log('server start');
})
```

**示例二**

```js
const express = require('express')
const app = express()
app.get('/index', (req, res) => {
    throw new Error('something broken')
})

app.use((err, req, res, next) => {
    res.status(500).send(err.message)
})

app.listen(3000, () => {
    console.log('server start');
})
```

> 注意：错误中间件的注册所有的中间件的后面，当其他中间件都无法匹配时将匹配到错误中间件

#### （4）内置中间件

`express.static()` 是 express 唯一内置的中间件，它基于 serve-static，负责在 express 应用中托管静态资源。每个应用可有多个静态目录。

```
app.use(express.static('public'))
app.use(express.static('uploads'))
app.use(express.static('files'))
```

#### （5）第三方中间件

安装所需功能的 node 模块，并在应用中加载，可以在应用级加载，也可以在路由级加载。

下面的例子安装并加载了一个解析 cookie 的中间件： cookie-parser

```bash
npm install cookie-parser
```

```js
var express = require('express')
var app = express()
var cookieParser = require('cookie-parser')

// 加载用于解析 cookie 的中间件
app.use(cookieParser())
```



### 3.2.4 express -- 获取请求的参数

**路由设置**

```js
// 响应前端的 get 请求
router.get('/home', (req, res) => {
    console.log(req.query);
    res.send('home')
})

// 响应前端的 post 请求
router.post('/index', (req, res) => {
    console.log(req.body);
    res.send({'ok':1})
})
```

**配置解析post模块**

```js
// 配置解析 post 参数的模块
app.use(express.urlencoded({extended:false}))   // 解析前端传来的 x-www-form-urlencoded格式的数据
app.use(express.json())     // 解析前端传来的 json 格式的数据
```



### 3.2.5 express -- 托管静态资源

通过 express 内置的 `static()` 方法可以方便地托管静态资源吗，例如：image、html、css、js 文件等

将 static 文件夹下的静态资源进行托管，语法如下：

```js
app.use(express.static('static'))
```

如果我们想要为静态资源指定一个虚拟目录，那么使用如下语法：

```js
app.use('/static', express.static('static'))
```

现在，我们就可以通过带有 `/static` 前端的地址来访问 public 目录下面的文件了



### 3.2.6 服务器渲染与客户端渲染



