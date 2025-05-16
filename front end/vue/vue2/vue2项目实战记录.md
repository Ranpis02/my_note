# 1 配置工作

## 1.1 初始配置

**01 运行项目时自动打开**

在 `package.json` 的 "scripts" 的 serve 命令后面添加 `--open` 即可



**02 关闭代码校验工具**

在 `vue.config.js` 中暴露配置覆盖 "隐藏vue.config.js"

```js
module.exports = {
  // 关闭eslint校验工具
  lintOnSave: false,
}
```



**03 让 `@` 符号替代常访问的 `src` 目录**

在 `jsconfig.js` 中添加如下配置

```js
{
    "compilerOptions": {
        "baseUrl": "./",
        "paths": {
            "@/*": [
                "src/*"
            ]
        }
    },
    "exclude": [
        "node_modules",
        "dist"
    ]
}
```



**04 配置 less 环境**

```bash
npm install less --save
npm install less-loader@6.1.0 --save
```



**05 允许脚手架中 script 中使用 js 语法**

配置项中设置 `"allowJs": true`

```json
{
  "compilerOptions": {
    "allowJs": false,
  }
}
```

这样可以避免脚手架中出现如下错误提示信息：

```
Virtual script not found, may missing ＜script lang=“ts“＞
```



## 1.2 区分路由组件和非路由组件

路由组件和非路由组件本质都是组件，它们存在的区别如下：

- 放置位置：路由组件一般放置在 `pages/views` 文件夹下，而非路由组件一般放置在 `components` 文件夹下

- 使用方式：路由组件需要配置在 router 文件夹下，注册方式有以下两种：

  - **先导入再注册**

    ```js
    import Home from '@/pages/Home'
    export default new VueRoute(
    	{
    		path: '/home',
    		component: Home
    	}
    )
    ```

  - **注册的同时动态导入**

    ```js
    export default new VueRouter({
    	{
    		path: '/home',
    		component: () => import('@/pages/Home')
    	}
    })
    ```

    使用的方式就是我们需要为路由组件定义一个出口，通常配置一级路由配置 `router-view` 在 App.vue 中，二级路由出口配置在其被嵌套的一级路由中；

    非路由组件又称为公共组件，使用时直接配置在 components 配置项下，然后使用双标签或自闭和标签进行引入渲染即可

除此之外，还有特别重要的一点就是当我们将 router 挂载到 Vue 身上后，所有组件都配置了以下两个属性：

- `$route`：用于获取路由信息

  > 例如：路径、query、params 等

- `$router`：用于编程式路由导航，进行路由跳转，例如 push,replace

  > 另外还有一种声明式路由导航，即利用 `router-link` 进行跳转，底层用到了 `a` 标签，但提醒一点：router-link 也是一个组件，如果存在大量的 router-link 组件是非常消耗内存的



## 1.3 路由传参

**01 路由传参方式一：字符串形式传参**

==query 传参== or ==params 传参==

路由对象写法:

```js
{
    path: '/search/:keyword',
    component: Search,
    meta: {
        show: true,
    }
}
```

编程式路由导航跳转的同时携带参数传入：

```js
routerToSearch() {
  this.$router.push('/search/' + this.keyword + "?k=" + this.keyword.toLowerCase())
}
```

在 Vue devtools 中查看信息：

![image-20230425235621763](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230425235621763.png)

**02 路由传参方式二：模板字符串**

```js
this.$router.push(`/search/${this.keyword}?k=${this.keyword.toLowerCase()}`)
```



**03 路由传参方式三：对象传参（推荐）**

使用这种方式的前提：<strong style="color:orange">需要我们在路由对象中添加 name 参数</strong>

```js
{
    name: 'search',
    path: '/search/:keyword',
    component: Search,
    meta: {
        show: true,
    }
}
```

路由跳转写法：

```js
this.$router.push({
    name: 'search',
    params: {
      keyword: this.keyword
    },
    query: {
      k: this.keyword.toLowerCase()
    }
})
```



**相关面试题**

1. 路由传递参数（对象写法），path 是否可以结合 params 使用？

   > 答：不可以

2. 如何指定 params 参数可传可不传？

   > 在配置路由的时候进行，path 的 param 参数后面带上一个问号即可，例如：`path: '/search/:keyword?`

3. params 参数可以传递也可以不传递，但是传递的如果是空串，如何解决？

   > 使用 undefined 即可，例如：
   >
   > ```js
   > params: {
   >   keyword: this.keyword || undefined
   > },
   > ```

4. 路由组件能不能传递 props 数据

   > 可以，并且有三种写法



**路由组件传递 props 数据的三种写法**

**01 方式1：通过 props: true 配置传递（这种方式只能传递 params）**

首先在路由配置项中添加 `props: true` 的配置参数，如下：

```js
{
    name: 'search',
    path: '/search/:keyword?',
    component: Search,
    meta: {
        show: true,
    },
    props: true	// 添加的 props 
}
```

之后直接在 search 组件中接收即可：

```js
export default {
    props: ['keyword']
}
```

![image-20230426002914434](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230426002914434.png)



**02 方式2：对象写法（不常用）**

配置项中的 props 使用对象形式：

```js
{
    name: 'search',
    path: '/search/:keyword?',
    component: Search,
    meta: {
        show: true,
    },
    // props: {a: 1, b: 2}
    props: {a, b}
}
```



**03 方式3：函数式写法，可以将 params 、query 通过 props 传入（推荐写法）**

```js
{
    name: 'search',
    path: '/search/:keyword?',
    component: Search,
    meta: {
        show: true,
    },
    props: ($route) => {
        return {
            keyword: $route.params.keyword,
            k: $route.query.k
        }
    }
}
```



## 1.3 编程式路由跳转抛出 "vigationDuplicated" 错误

场景如下：push 路由跳转方式可以记录历史页面，但是如果当我们多次跳转到同一个路由时，就会抛出如下错误信息：

![image-20230426004037555](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230426004037555.png)

原因：push 和 replace 方法<strong style="color:red">底层使用了 promise</strong> ，当跳转不成功时，需要对错误进行处理

解决方法：重写 VueRouter 原型对象上的 `push` 和 `replace()`

```js
// 重写 push 和 replace 方法
// 1. 保留一份原来的 push 和 replace 方法
let originPush = VueRouter.prototype.push
let originReplace  = VueRouter.prototype.replace

// 2. 对回调进行重写
VueRouter.prototype.push = function(location, resolve, reject) {
    if(resolve && reject) {
        originPush.call(this, location, resolve, reject)
    } else {
        originPush.call(this, location, () => {}, () => {})
    }
}
VueRouter.prototype.replace = function(location, resolve, reject) {
    if(resolve && reject) {
        originReplace.call(this, location, resolve, reject)
    } else {
        originReplace.call(this, location, () => {}, () => {})
    }
}
```

> 提醒一点：`call()` 和 `apply()` 的区别与联系
>
> - 相同点：都可以调用函数一次，都可以改变函数的上下文指向
> - 不同点：call 传递的参数使用逗号隔开，而 apply 传递的参数以数组形式传入
>
> 最后，需要注意一点就是，这里不要使用箭头函数



## 1.4 注册全局组件

为了使得一些频繁被使用的组件能够直接拿来使用，而不需要通过 `import` 导入后再使用，可以直接将其注册到全局中，如下：

```js
// 引入三级联动组件 -- 全局组件
import TypeNav from '@/pages/Home/TypeNav'
Vue.component(TypeNav.name, TypeNav)
```

> name 属性则是组件自身在 script 脚本中配置的属性



## 1.5 axios 二次封装

axios 二次封装的原因：<u>添加请求拦截器、响应拦截器</u>

```js
import axios from 'axios'

// 1. 利用 axios 对象的create方法，创建axios实例
const request = axios.create({
    // 添加基础路径
    baseURL: '/api',
    // 配置超时时间
    timeout: 5000
})
// 配置请求拦截器：在发送请求之前，
request.interceptors.request.use(config => {
    // 返回响应的配置对象,对象里面包含一个重要的属性：header请求头
    return config
})
// 配置响应拦截器
request.interceptors.response.use(res => {
    // 成功的回调，直接返回封装的响应数据报
    return res.data
}, err => {
    // 失败的回调直接抛出 Error 终结
    return Promise.reject(new Error('whoops'))
})


export default request
```



## 1.6 noprogress 进度条

如果想要实现加载时的进度条的效果，我们可以借用 noprogress 插件

```bash
npm i nprogress 
```

之后在 request.js 中引入相应包和样式，并通过 `start()` 和  `done()` 方法控制进度条

```js
import axios from 'axios'
// 引入进度条
import nprogress from 'nprogress'
// 引入进度条相关样式
import 'nprogress/nprogress.css'

// 1. 利用 axios 对象的create方法，创建axios实例
const request = axios.create({
    // 添加基础路径
    baseURL: '/api',
    // 配置超时时间
    timeout: 5000
})
// 配置请求拦截器：在发送请求之前，
request.interceptors.request.use(config => {
    // 返回响应的配置对象,对象里面包含一个重要的属性：header请求头
    nprogress.start()   // 进度条开始执行
    return config
})
// 配置响应拦截器
request.interceptors.response.use(res => {
    // 成功的回调，直接返回封装的响应数据报
    nprogress.done()    // 进度条结束执行
    return res.data
}, err => {
    // 失败的回调直接抛出 Error 终结
    return Promise.reject(new Error('whoops'))
})

export default request
```

我们也可以在其对应的 nprogress.css 修改其对应的样式：

![image-20230426150919501](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230426150919501.png)



## 1.7 Vuex 的开发

```js
import Vue from 'vue'
import Vuex from 'vuex'
// 注入插件
Vue.use(Vuex)

// 处理 action, 可以书写自己的业务逻辑，也可以处理异步
const actions = {
    add(context) {
        context.commit('ADD')
    }
}

// 修改 state 的唯一手段
const mutations = {
    ADD(state) {
        state.count++
    }
}
// 存储数据的仓库
const state = {
    count: 1
}

// 计算属性，用于简化仓库数据，让组件获取仓库的数据更加方便
const getters = {}

export default new Vuex.Store({
    state,
    actions,
    mutations,
    getters,
})
```



**Vuex 的模块化开发**

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

// 引入小仓库
import home from '/home'
import search from '/search'

// 对外暴露 Store 的一个实例
export default new Vuex.Store({
    // 实现 Vuex 仓库模块式开发
    modules: {
        home,
        search
    }
})
```



# 2 业务处理

## 2.1 节流与防抖

当用户的行为过快，而当前回调函数中具有一些大量业务，那么由可能会出现卡顿现象（由于快速触发而来不及响应），此时我们就需要通过节流与防抖来组织用户操作过快的行为

- 防抖：<strong style="color:red">前面的所有触发都被取消，最后一次执行在规定的时间之后才会触发</strong>，也就是说如果连续快速地触发，只会执行一次
- 节流：<strong style="color:red">在规定的时间范围内不会重复触发回调，只有这个时间间隔才会触发回调</strong>，把频繁触发变为少量触发

如果想要完成防抖与节流，我们可以使用 lodash 库，里面提供了专门用于==防抖的函数 —— `_.debounce`()== 和专门用于 ==节流的函数 —— `_.throttle()`==

> 官方文档：https://www.lodashjs.com
>
> > 注意：在 Vue 的脚手架中已经提前帮我准备好了 lodash，不需要自己手动安装



```js
// 全部引入
import _ from 'lodash'

// 按需引入
import throttle from 'lodash/throttle'
```

**使用方法示例**

```js
changeBg: throttle(function(index) {
  this.currentIndex = index;
}, 50)
```

> 注意点：
>
> 1. method 使用 k-v 的形式
> 2. throttle 中的回调函数不要使用箭头函数，不然会出现 this 为 undeinfed 的情况



## 2.2 过渡动画

过渡动画的前提：组件 | 元素要有 `v-if` 或 `v-show` 指令才能进行过渡动画效果



## 2.3 mock 模拟数据

安装 mockjs 依赖

```bash
npm i mockjs
```

使用方法：

1. 引入对应的 mockjs 模块

   ```bash
   import Mock from 'mockjs'
   ```

2. 引入 JSON 数据

   ```js
   import banner from './banner.json'
   import floor from './floor.json'
   ```

   > 这里有人可能会很奇怪，json 数据文件没有对外暴露，怎么能直接引入？
   >
   > 其实像 图片、JSON 数据都是 webpack 默认对外暴露的，只有 js 文件需要我们首先需要通过 export 暴露，之后再引入

3. mock 数据

   ```js
   // Mock.mock(拦截URL, 封装数据的对象)
   Mock.mock('/mock/banner', {code: 200, data: banner})
   Mock.mock('/mock/floor', {code: 200, data: floor})
   ```

4. 执行 mock 程序，直接在 main.js 中引入即可，不需要对外暴露对象



## 2.4 登录注册页面



# 3 组件间通信高阶

## 3.1 event 深入

事件：可以分为 【系统事件】 和 【自定义事件】

> 系统事件又称为原生事件，例如 click, dbclick，鼠标事件等

事件三要素：<u>事件源（给谁绑定的事件？）、事件类型（触发事件的类型？）、事件回调（触发事件后的处理程序）</u>

我们对对 Vue 中绑定的事件进行分析：

1. 原生 DOM 绑定的原生事件

   ```js
   <button @click="hanldeClick">button</button>
   ```

2. 组件绑定的是自定义事件，由于没有办法触发 `$emit`，所以回调不会被执行

   ```js
   <Detail @click="handleClick" />
   ```

   > 如果想要让点击事件生效，那么可以通过 `.native` 修饰符将点击事件转换为原生事件

3. 如果我们想要在组件中添加自定义事件 xxx，并在组件中某个原生 DOM 触发时执行回调，需要在该 DOM 中添加原生事件，并在原生事件中绑定 `$emit`


此外，在原生 JS 中有一个 event 事件对象，同样地，在 Vue 的事件回调中也有 `$event` ，不过这个一个 `$event` 参数，我们需要在咋回调中引入才能使用

```js
<input type="text" :value="msg" @input="handle">
```

```js
handle($event) {
  console.log($event)		
  console.log($event.target)	// 获取事件源的 DOM
  this.msg = $event.target.value	// 由于事件源为 input 框，这里的 value 表示输入框的值
}
```



## 3.2 v-model 深入

`v-model` 其实是 `v-model:value` 的语法糖，应用在表单元素中完成双向绑定，对于 `v-model` ，我们还有另外一种实现方式，即通过原生的 `@input` 事件完成

```js
<input type="text" :value="msg" @input="msg = $event.target.value">
```



## 3.3 sync 修饰符

sync 修饰符主要用于完成<strong style="color:red">父子组件之间的数据同步</strong>

如果我们不使用 sync 修饰符，那么我们如果想要实现父子组件间数据同步，得按如下操作完成：

**父组件**

```js
<!-- $event 为事件触发传递过来的参数 -->
<ChildOne :money="money" @update:money="money = $event" /> 
```

```js
data() {
    return {
      money: 10000
    }
}
```

> 其中 第一个 `money` 为传递到子组件的 props，第二个 `money` 为父组件中 data 中数据，`update:money` 为自定义事件

**子组件**

```js
<h1>child_1当前还剩余：{{ money }} 元</h1>
<button @click="$emit('update:money', money - 100)">花费100元</button>
```

```js
props: {
  money: {
    type: Number,
    default: 0
  }
}
```



而如果我们使用了 **`sync`** 修饰符，那么前面的 **`<ChildOne :money="money" @update:money="money = $event" /> `** 完全可以写成如下形式：

```
<ChildOne :money.sync="money" />
```



## 3.4 `$attrs` 和 `$listeners`

- `$attrs`：组件实例身上的一个属性，可以用于获取父组件传递到子组件的 props 数据
- `$listeners`：同样是组件实例身上的一个属性，可以用于获取父组件传递到子组件中的自定义事件

将传递过来的参数信息（包括属性和方法）都 "化为己用"

```js
<el-button v-bind="$attrs" v-on="$listeners" ></el-button>
```



## 3.5 `$children` 和 `$parent`

- `$children`：用于获取当前组件的全部子组件，返回的结果形式为数组，我们可以像 `$refs` 一样，在得到子组件后直接拿到子组件身上的 data 和 methods
- `$parent`：用于获取当前组件的全部父组件



## 3.6 深度选择器

在解释深度选择器之前，我们需要了解 style 中的 `scoped` 属性，该属性的作用：<strong style="color:red">给当前组件中所有的标签（包括子组件中的最外层的标签（例如：div、ul）等添加上一个 `data-v-xxx` 的自定义属性，之后在 style 中设置的所有样式，vue 底层都会将其利用属性选择器进行封装，以达到 "样式隔离" 的作用</strong>

如何在添加了 scoped 属性的 style 中将样式应用到子组件？

利用深度选择器：

- 原生 CSS：`>>>`
- less：`/deep`
- scss：`::v-deep`

