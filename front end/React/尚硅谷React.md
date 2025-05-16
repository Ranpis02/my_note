[toc]

# 1 React 基础

## 1.1 React 简介

- 英文官网：[传送门](https://reactjs.org/)
- 中文官网：[传送门](https://react.docschina.org/)

React 用于<font color="red">构建用户的 JS 库，是一个将数据渲染为 HTML 视图的开源  JS 库。</font>

> React 由 Facebook 开发并且开源

我们之所以使用 React 框架替代原生 JS 或 jQuery，就是因为其具有以下缺点：

1. 原生 JS 操作 DOM 繁琐、效率低（<strong style="color:red">DOM-API 操作 UI</strong>）

2. 使用 JS 直接操作 DOM，浏览器会进行大量的<strong style="color:red">重绘重排</strong>

   > - 重绘：元素的基本样式发生改变，例如颜色、背景、字体大小等
   > - 重排：元素的位置发生改变，例如增加、修改和删除某个节点等

3. 原生 JS 没有<strong style="color:red">组件化</strong>编码方案，代码复用率低



## 1.2 React 的特点

1. 采用<strong style="color:red">组件化</strong>模式、<strong style="color:red">声明式编码</strong>，提高开发效率及组件复用率

   > 声明式编码：与命令式编码相对，将具体操作细节黑盒化

2. 在 React Native 中可以使用 React 语法进行<strong style="color:red">移动端开发</strong>

3. 使用<strong style="color:red">虚拟 DOM</strong> + 优秀的 <strong style="color:red">Diffing 算法</strong>，尽量减少与真实 DOM 的交互

   <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230111163856368.png" alt="image-20230111163856368" style="zoom:50%;" />



## 1.3 创建 React 工程项目

我们在创建工程项目之间需要导入下列 JS 文件：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230111172010178.png" alt="image-20230111172010178" style="zoom:50%;" />

其中，1、3、4 为必须要引入的，他们分别的作用如下：

- babel：将 ES6 => ES5，将 JSX => JS 
- react-development：React 的核心库
- react-dom：提供操作 DOM 的 React 的扩展库

<strong style="color:red">并且核心库的引入需要在扩展库引入前引入</strong>

```react
<!-- 准备好容器 -->
<div id="test"></div>

<!-- 引入 React 的核心库 -->
<script src="../js/react.development.js"></script>
<!-- 引入 react-dom，用于支持 React 操作 DOM  -->
<script src="../js/react-dom.development.js"></script>
<!-- 引入 bebel，用于将 jsx 转化为 js -->
<script src="../js/babel.min.js"></script>

<script type="text/babel">
    // 1. 创建虚拟 DOM
    const VDOM = <h1>Hello,React</h1>   // 此处一定不要写引号，因为不是字符串
    // 2. 渲染虚拟 DOM 到页面
    ReactDOM.render(VDOM, document.getElementById('test'))
</script>
```

我们前往浏览器控制台去查看，提示信息如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230111175058879.png" alt="image-20230111175058879" style="zoom:50%;" />

- 提示1：要下载 React 官方的浏览器扩展去搭配工程调试使用
- 提示2：现在正在使用的是浏览器内的 Babel 转换器，需要先预编译再投入使用



:sos:**友情提示**

如果您像我一样在使用嵌入式的 script 脚本时，在控制台报了如下错误：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230122173230885.png" alt="image-20230122173230885" style="zoom:50%;" />

这里我的情况是安装 【FeHelper (前端助手)】这款扩展插件，所以只要在开发者模式中将去关闭或者卸载即可解决问题，当然，这个错误并不会影响到代码的正常运行



## 1.3 对比 react-dom 和原生 JS 写法

其实，上述代码，我们也可以通过原生 JS 实现，如下：

```jsx
// 1. 创建虚拟 DOM
const VDOM = React.createElement('h1', {id:'title'}, 'Hello,React')   // 此处一定不要写引号，因为不是字符串
// 2. 渲染虚拟 DOM 到页面
ReactDOM.render(VDOM, document.getElementById('test'))
```

这些看起来其实问题好像也不大，但是一旦要创建的DOM 结构层级较深，那么此时使用原生 JS 将十分复杂，现在需要在`h1` 中套一层 `span` 对比如下：

```jsx
// react-dom 
const VDOM = (
    <h1 id="title">
        <span id="content">Hello,React</span>
    </h1>
)

// 原生 JS
const VDOM = React.createElement('h1', {id:'title'}, React.createElement('span', {id:'content'}, 'Hello,React'))
```

这还只是两层的情况，一旦出现"嵌套地狱"，使用原生创建 DOM 结构将更加繁琐，所以推荐使用 JSX 的写法



## 1.4 虚拟 DOM 与真实 DOM 

关于虚拟 DOM：

1. 本质是 Object 类型的对象（一般对象）
2. 虚拟 DOM 比较 "轻"，而真实 DOM 比较 "重"，因为虚拟 DOM 是 React 内部使用，无需像真实 DOM 一样携带众多属性参数
3. 虚拟 DOM 最终会被 React 转化为真实 DOM ，呈现在页面上



## 1.5 JSX 语法规则

1. 定义虚拟 DOM 时，外围不能使用引号包括，可以不包裹或者使用小括号`()` 包裹
2. <strong style="color:red">标签中混入 JS 表达式时，需要使用 `{}`</strong>
3. 样式的类名指定不要使用 `class`，而应该使用 `className`
4. 内联样式，要用 `style={{key1:value1, key2:value2, …}}` 这种形式书写
5. 只能具有一个根标签
6. 标签需要闭合，也就是说像 `input`、`br` 这类单标签，应该将其扩展为双标签`<input></input>` 或者自闭合标签 `<input />` 这种写法
7. 对于标签首字母，具有如下讲究：
   - 若小写字母开头，JSX 会将其转化为 HTML 中同名标签，若 HTML 中无对应标签，将报错
   - 若大写字母开头，React 就去渲染对应的组件。若组件没有定义，则报错



```jsx
const myId = 'TiTle'
const content = 'Hello,React'

const VDOM = (
    <div>
        <h1 id={myId.toLowerCase()}>
            <span style={{color:'red', fontSize:'28px'}}>{content.toLowerCase()}</span>
        </h1>
        <br/>
        <input type="text"/>
    </div>
)
```

显示效果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230111223630727.png" alt="image-20230111223630727" style="zoom:50%;" />



:bell:**小案例：使用 JSX 语法完成无序列表的动态渲染**

在渲染之前，我们需要知道，react 会将数组中的元素逐个渲染到页面中，而则不能直接渲染对象，我们如果想要将数组中的数据渲染为无序列表，则需要对其进行处理，如下：

```jsx
const data = ['Angular', 'React', 'Vue']

const VDOM = (
    <ul>
        {
            data.map((item, index) => {
                return <li key={index}>{item}</li>
            })
        }
    </ul>
)
ReactDOM.render(VDOM, document.getElementById('test'))
```

:warning:**注意**

```
需要特别注意，React 中采用了虚拟 DOM + Diffing 算法，所以对于无序列表中每个列表项都应该指定唯一的 key 值
```



## 1.6 组件与模块

### 1.6.1 模块

1. 理解：向外提供特定功能的 JS 程序，一般就是一个 JS 文件
2. 拆成模块的原因：随着业务逻辑的增加，代码越来越多且复杂
3. 作用：复用 JS ，简化 JS 的编写，提高 JS 的运行效率



### 1.6.2 组件

1. 理解：<strong style="color:red">用来实现局部功能效果的代码和资源的集合</strong>（例如：html/css/js/image等等）
2. 拆成组件的原因：一个界面的功能复杂
3. 作用：复用代码，简化项目编码，提高运行效率



### 1.6.3 模块化

当应用的 JS 都以模块来编写，这个应用就是一个模块化的应用



### 1.6.4 组件化

当应用是以多组件的方式实现，这个应用就是一个组件化的应用



# 2 React 面向组件编程

## 2.1 开发工具的安装

前往 Chrome 商店下载安装即可：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230112000456013.png" alt="image-20230112000456013" style="zoom:50%;" />

新增的变化：

<div>
    <ul>
        <li>当工程项目处于开发状态时，图片显示为：<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230112001542688.png" style="width:25px"></li>
        <li>当工程项目处于生产状态时，图片显示为：<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230112001452583.png" style="width:25px"></li>
    </ul>
</div>

> 美团的[官网](https://nc.meituan.com/)就是使用 React 来进行开发的

控制台多了两个面板

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230112002146428.png" alt="image-20230112002146428" style="zoom:50%;" />

- Components：用于组件管理和查看
- Profiler：分析器，分析工程项目的性能，显示具体的参数值



## 2.2 函数式组件（适用于简单组件的定义）

函数式组件用法如下：

```jsx
// 1. 创建函数式组件
function MyComponent() {
    console.log(this);		// undefined
    return <h1>我是通过函数定义的组件（适用于简单组件的定义）</h1>
}

// 2. 渲染组件到页面
ReactDOM.render(<MyComponent/>, document.getElementById("test"))
```

:pushpin:**注意事项**

- 函数式组件的函数定义名需要以大写开头，JSX 语法才会将其识别为组件
- 在 babel 转换器作用下，JSX => JS，同时会开启严格模式，导致自定义的全局函数内 this 为 `undefined`
- 最后一步渲染（即 React.render(…) 执行的过程）
  1. React 解析组件标签，找到了 MyComponent 组件
  2. 解析时发现 MyComponent 是使用函数定义的，随后调用该函数，将返回的虚拟 DOM 转换为真实 DOM，渲染在页面上

:herb:**扩展**

1. 我们可以来到 [babel 官网](https://babel.docschina.org/)进行调试，看看babel 是如何将 JSX 转换为 JS 的
2. 严格模式（Strict Mode）下的语法变化
   - 严格模式下，全局作用域中函数中的 this 是 `undefined`；但是在普通模式下，全局作用中函数的 this 是 `window`
   - 严格模式下，定时器中的 this 指向的是 `window`；而在普通模式下，也是指向 `window`
   - 严格模式下，不允许删除变量，并且使用未声明的变量，会报错；而在普通模式下，允许使用 `delete` 删除变量，而使用未声明的变量，是不会报错的



## 2.3 类式组件

### 2.3.1 类的基本知识复习

**类的基础用法**

```js
// 创建一个 Person 类
class Person {
    // 构造器方法
    constructor(name, age) {
        // 构造器中的 this 指向的类的实例对象
        this.name = name;
        this.age = age;
    }
    // 一般方法
    speak() {
        // speak 方法放到了类的原型对象上，供实例使用
        // 通过 Person 实例调用 speak 时，speak 中的 this 指向的就是实例对象
        console.log(`我叫${this.name}, 今年${this.age}岁`);
    }
}
const p1 = new Person('张三', 18);
```

**继承语法**

```js
// 创建一个 Student 类，继承与 Person 类
class Student extends Person {
    // 创建构造器（如果新类的属性与继承类的属性相同，那么可以省略不写）
    constructor(name, age, grade) {
        // 使用 super 来沿袭继承类的属性，并且要放到最前面且顺序与继承类中属性字段顺序一致
        super(name, age)
        this.grade = grade
    }
    // 重写从父类继承过来的方法
    speak() {
        console.log(`我叫${this.name}, 今年${this.age}, 正在上${this.grade}`);
    }
}
```

:spiral_notepad:**总结**

1. 类中的构造器不是必须写的，要对示例进行一些初始化操作，如添加指定属性时才需要写
2. 如果 A 类继承了 B 类，且 A 类中写了构造器，那么 A 类构造器中的 super 是必须要调用的
3. 类中所定义的方法，都是放在了类的原型对象上，供实例去使用



### 2.3.2 类式组件用法

**类式组件的基本用法**

```jsx
// 1. 创建类式组件
class MyComponent extends React.Component {
    render() {
        return <h2>我是用类定义的组件（适用于复杂组件的定义）</h2>
    }
}

// 2. 渲染组件到页面
ReactDOM.render(<MyComponent/>, document.getElementById("test"))
```

:spiral_notepad:**重点**

- 类中的 `render` 从哪里来? —— 继承于 `React.Component`，引入到 `MyComponent` 的原型对象上，供实例使用
- `render` 中的 `this` 的指向? —— 指向`MyComponent` 的实例对象 <=> `MyComponent` 组件实例对象
- 执行了 `ReactDOM.render(…)` 后发生了什么?
  1. `React` 解析组件标签，找到 `MyComponent` 组件
  2. 发现组件是使用类定义的，随后 `new` 出来该类的实例，并通过该实例调用到原型上的 `render`
  3. 将 `render` 返回的虚拟 DOM 转为真实 DOM，虽然渲染在页面上



## 2.4 组件实例三大核心属性

### 2.4.1 state

在上一节，我们了解到函数式组件和类组件，它们之间除了定义不同之外，还有就是：

- 函数式组件：适用于【简单组件】的定义
- 类式组件：适用于【复杂组件】的定义

而，

- 简单组件：**无状态（`state`）**的组件
- 复杂组件：**有状态（`state`）**的组件

**理解 `state`**

1. `state` 是组件对象最重要的属性，<strong style="color:red">值是对象</strong>
2. 组件被称为"状态机"，<strong style="color:red">通过更新组件的 `state` 来更新对应的页面显示（重新渲染组件）</strong>

示例如下：

```jsx
// 1. 创建虚拟 DOM
class Weather extends React.Component {
    constructor(props) {
        super(props)
        // 初始化状态
        this.state = {
            isHot: true
        }
        // 改变 this 原型对象上 change 的指向，并将其添加到 this 身上，方法名同样为 change
        this.change = this.change.bind(this)
    }
    render() {
        // 解构赋值，将 isHot 字段提取出来
        const {isHot} = this.state
        // 根据状态信息渲染到对应的页面上
        return <h1 onClick={this.change}>今天天气很{isHot?'炎热':'凉爽'}</h1>
    }

    change() {
        const isHot = this.state.isHot
        this.setState({
            isHot: !isHot
        })
    }
}
// 2. 渲染虚拟 DOM 到页面
ReactDOM.render(<Weather/>, document.getElementById('test'))
```

显示效果如下：

![tutieshi_640x117_2s](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/tutieshi_640x117_2s.gif)

:warning:**注意**

- 在 React 中，事件绑定时我们使用 `onClick`，`onBlur`、… 这种小驼峰命名法的形式去定义
- 对于<strong style="color:red">类式组件</strong>而言，除了自身方法中 `this` 指向的是类的实例对象（因为最后是通过实例去调用的），其他自定义方法中的 `this` 由于类自身的方法中开启了<strong style="color:red">局部严格模式</strong>，所以 `this` 的指向为 `undefined`，我们需要在构造器中动态地将方法中 `this` 指向修改为类的实例对象
- 构造器 `constructor` 和 `render` 方法中的 `this` 指向的都是类的实例

:thinking:**思考**

1. 组件渲染过程中 `constructor` 执行的次数？

   ```
   1次。整个渲染过程（包括后面状态发生改变，react 重新调用 render 方法）中，constructor 只执行 1 次
   ```

2. 组件渲染过程中 `render` 执行的次数？

   ```
   1 + n 次。其中初始化时，render 会被执行 1 次，后面每次点击触发事件时，render 都会执行一次，所以 n 代表触发事件的次数
   ```

3. 组件渲染过程中自定义方法——`change` 执行的次数？

   ```
   n 次。其中 n 代表触发事件的次数
   ```

:pushpin:**重点**

1. 代码中改变 `this` 指向这一步（即：**`this.change = this.change.bind(this)`** ）十分重要，首先 `change()` 方法本来是挂载在实例的原型对象上，后来通过这一行代码既改变了 `this` 的指向，又将其挂载在了实例身上，这样根据原型链就会访问实例身上的改变了 `this` 指向的 `change()` 方法，同时不会改变原型上的该方法
2. 改变 `state` 身上的属性，需要调用 **`setState()`** 方法（该方法挂载在 React.Component 的原型上），而不能直接去改变数据，因为直接更改，React 是不会自动帮你调用 `render()` 方法的，如此页面就无法更新



:herb:**扩展：state 的简写形式**

实际开发过程中，我们并不会像上面的代码一样去设置 `state` ，然后如此麻烦地去更改，我们更多地会使用下面要介绍的方式，在了解之前，我们需要知道 ES6 中类的一种写法，即：<strong style="color:red">类中可以直接写赋值语句，并且赋值语句中的变量将直接作为类的实例的属性存在</strong>

简写形式如下：

```jsx
// 1. 创建虚拟 DOM
class Weather extends React.Component {
    // 添加初始化数据
    state = {
        isHot: true
    }
    render() {
        // 解构赋值，将 isHot 字段提取出来
        const { isHot } = this.state
        // 根据状态信息渲染到对应的页面上
        return <h1 onClick={this.change}>今天天气很{isHot ? '炎热' : '凉爽'}</h1>
    }
    // 自定义方法使用匿名函数的形式，匿名函数没有自己的 this，最后会找到外面环境的this，即指向实例
    change = () => {
        const isHot = this.state.isHot
        this.setState({
            isHot: !isHot
        })
    }
}
// 2. 渲染虚拟 DOM 到页面
ReactDOM.render(<Weather />, document.getElementById('test'))
```

> 以后在类式组件中，对于自定义方法，尽量使用<strong style="color:orange">赋值语句形式 + 箭头函数</strong>



:spiral_notepad:**总结**

1. 组件中的 `render()`  方法 `this` 指向的是组件实例对象

2. 组件自定义方法中的 `this` 为 `undefined`，解决方法如下：

   a. 通过 `bind()` 方法强制改变 `this` 指向

   b. 使用箭头函数

3. 状态数据，不能直接修改或更新

### 2.4.2 props

<strong style="color:red">`props` 用来接受组件传递过来的参数</strong>

基本用法如下：

```react
<div id="test"></div>
<div id="test1"></div>
<div id="test2"></div>
<!-- 引入 React 的核心库 -->
<script src="../js/react.development.js"></script>
<!-- 引入 react-dom，用于支持 React 操作 DOM  -->
<script src="../js/react-dom.development.js"></script>
<!-- 引入 bebel，用于将 jsx 转化为 js -->
<script src="../js/babel.min.js"></script>
<script type="text/babel">
    class Person extends React.Component {
        render() {
            console.log(this);
            const { name, age, sex } = this.props;
            return (
                <ul>
                    <li>姓名：{name}</li>
                    <li>年龄：{age}</li>
                    <li>性别：{sex}</li>
                </ul>
            )
        }
    }

    ReactDOM.render(<Person name="jerry" age="3" sex="男" />, document.getElementById('test'))
    ReactDOM.render(<Person name="Tom" age="4" sex="女" />, document.getElementById('test1'))
    ReactDOM.render(<Person name="kerwen" age="5" sex="男" />, document.getElementById('test2'))
</script>
```

显示效果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230119224403905.png" alt="image-20230119224403905" style="zoom:50%;" />



:star2:**props 的高级用法 —— 批量传递 props**

首先，我们需要回顾一下扩展运算符（`...`），基本用法如下：

```js
/*
@语法格式：√
@执行结果：a b c d
@结论：扩展运算符可以将字符串展开 
 */
const str = 'abcd'
console.log(...str);

/*
@语法格式：√
@执行结果：1 2 3 4 
@结论：扩展运算符可以将数组展开 
 */
const arr = [1, 2, 3, 4]
console.log(...arr);    // 扩展运算符可以将数组展开

/*
@语法格式：×
@执行结果：报错 
@结论：扩展运算符不能将对象展开 
 */
const object = {name:'zs', age:18}
console.log(...object);
```

但是，在 ES 2018 中新增一种特性，那就是<strong style="color:red">在构造字面量对象时，可以使用扩展运算符进行克隆或属性拷贝</strong>

```js
const object = {name:'zs', age:18}
const o = {...object}
console.log(o);
```

使用对象的 "属性克隆" 更改数值，如下：

```js
const object = {name:'zs', age:18}
const o = {...object, name:'jack'}
```

而 `props` 中存在使用 `props` 进行组件传参的语法糖，如下：

```jsx
const person = {
    name:'jerry',
    age:3,
    sex:'男'
}
ReactDOM.render(<Person {...person}/>, document.getElementById('test'))
```

> :warning:**注意：**这里虽然和对象的 "属性克隆" 有点像，但是这里的 `{}` 并不是拷贝，而是解析表达式的符号，在 React 中有点不同，可以使用扩展运算符来展开对象，但是仅限在组件传参时使用



#### 对 `props` 进行限制

要对 `props` 属性进行限制，那么我就需要首先引入 `prop-types.js` 脚本文件

```jsx
// 类式组件
class Person extends React.Component {
    render() {
        const { name, age, sex } = this.props;
        return (
            <ul>
                <li>姓名：{name}</li>
                <li>年龄：{age}</li>
                <li>性别：{sex}</li>
            </ul>
        )
    }
}
// 对标签属性进行类型、必要性的限制
Person.propTypes = {
    name:PropTypes.string.isRequired,   // 限制name字段为字符串并且为必选
    sex:PropTypes.string,   // 限制sex字段为字段串并且是可选的
    age:PropTypes.number,   // 限制age字段为数值类型并且可选的
    speak:PropTypes.func,   // 限制speak字段为函数类型并且是可选的
}
// 指定默认标签属性值
Person.defaultProps = {
    sex:'男',   // 规定sex字段默认值为"男"
    age:18      // 规定age字段默认值为18
}
```

:warning:**注意**

1. 组件身上的 `propTypes` 属性为 `React` 为组件提供的，而限制的字段的 `PropTypes` 为脚本 `prop-type.js` 提供的，一定要先引入脚本才能使用
2. 限制字段类型为了避免与 `JS/JSX` 中基本类型冲突，所以 `String => string`、`Number => number`、`function => func`



#### `props` 的简写方式

上面将类的定义与类的限制分离，我们也可以使用简写形式，将限制放在类的里面：<strong style="color:red">考虑使用赋值语句</strong>。如下：

```jsx
// 类式组件
class Person extends React.Component {
    // 对标签属性进行类型、必要性的限制
    static propTypes = {
        name: PropTypes.string.isRequired,   // 限制name字段为字符串并且为必选
        sex: PropTypes.string,   // 限制sex字段为字段串并且是可选的
        age: PropTypes.number,   // 限制age字段为数值类型并且可选的
        speak: PropTypes.func,   // 限制speak字段为函数类型并且是可选的
    }
    // 指定默认标签属性值
    static defaultProps = {
        sex: '男',   // 规定sex字段默认值为"男"
        age: 18      // 规定age字段默认值为18
    }
    render() {
        const { name, age, sex } = this.props;
        return (
            <ul>
                <li>姓名：{name}</li>
                <li>年龄：{age}</li>
                <li>性别：{sex}</li>
            </ul>
        )
    }
}
```

:pushpin:**重点**

- 这里我们在赋值语句前面添加 `static` 字段，表示将属性添加到类身上，而不是类的实例身上
- <strong style="color:red">`props` 是只读的</strong>，我们不能对 `props` 中的属性进行修改



#### 类式组件中的构造器与 `props`

通过前面的学习，我们知道在 `constructor` 中可以接收到 `props`，并且在为 `React.Component` 子类实现构造函数时，应在其他语句之前调用 `super(props)`，如果不调用，那么 `this.props` 在构造函数中可能会出现未定义的 bug

示例如下：

```jsx
// 1. 当构造器中调用super(props)，那么this.props为undefined
constructor() {
    super()
    console.log(this.props)     // undefined
}

// 2. 当构造器中调用super(props),那么this.props为传入参数的对象
constructor(props) {
    super(props)
    console.log(this.props)     // Object
}
```

:spiral_notepad:**总结**

<strong style="color:red">构造器中是否接收props，是否传递给 `super`，取决于：是否希望在构造器中通过 `this` 访问 `props` </strong>



#### 函数式组件使用 `props`

示例如下：

```jsx
function Person(props) {
    const { name, age, sex } = props
    return (
        <ul>
            <li>姓名：{name}</li>
            <li>年龄：{age}</li>
            <li>性别：{sex}</li>
        </ul>
    )
}
const person = {
    name: 'jerry',
    age: 3,
    sex: '男'
}
ReactDOM.render(<Person {...person} />, document.getElementById('test'))
```



### 2.4.3 refs

#### 01 字符串类型的 `ref`

示例代码如下：

```jsx
class Demo extends React.Component {
    showData = () => {
        const {input1} = this.refs
        alert(input1.value)
    }
    render() {
        return (
            <div>
                <input type="text" ref="input1"/>&nbsp;
                <button onClick={this.showData}>点击我弹出输入框中的文字</button>
            </div>
        )
    }
}
ReactDOM.render(<Demo/>, document.getElementById('test'))   
```

- 基本思路：对于字符串形式的 `ref` （上面的 "input1" 为字符串，所以称为字符串形式的 `ref`）,`ref` 用于给标签打标识，类似于 `id`，之后标识会挂载在组件实例的 `refs` 对象上，通过 `.` 来取得属性值（即：DOM 节点）

- 字符串形式的 `ref` 已经不再推荐，以后可能会遗弃（原因：**当我们过度使用字符串类型的 `ref`，代码的运行效率会降低**）

  <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230120220537056.png" alt="image-20230120220537056" style="zoom:33%;" />

  > 位置：【高级索引】=> 【Refs & DOM】



#### 02 回调形式的 `ref`

示例代码如下：

```jsx
class Demo extends React.Component {
    showData = () => {
        alert(this.input1.value);
    }
    render() {
        return (
            <div>
                <input type="text" ref={(currentNode) => {this.input1 = currentNode}}/>&nbsp;
                <button onClick={this.showData}>点击我弹出输入框中的文字</button>
            </div>
        )
    }
}
ReactDOM.render(<Demo/>, document.getElementById('test'))    
```

:pushpin:**备注**

- 在回调形式的 `ref` 中，`ref` 中使用的回调形式的函数，`ref` 中的回调 React 会自动帮助我们调用。回调中的参数为当前 `ref` 所处的 DOM 节点，在回调函数体内直接将其添加到组件实例身上即可
- 回调的究极简写形式：**`c => this.input1 = c`**

:herb:**扩展1：如何将`return` 返回的 DOM 节点给注释掉** 

JSX 语法不同于 JS 语法，我们要想要将 `return` 返回的 DOM 节点给注释掉，需要按如下方式：

```jsx
render() {
    return (
        <div>
            {/*<input type="text" ref={(currentNode) => {this.input1 = currentNode}}/>&nbsp;*/}
            <button onClick={this.showData}>点击我弹出输入框中的文字</button>
        </div>
    )
}
```

> 注释部分使用 `{/*<p>这是被注释的部分</p>*/}` 这种形式



:herb:**扩展2：使用内联函数定义的 `ref` 回调在每次渲染的时候会被执行两次，第一次传入参数 `null`，然后第二次会传入参数 DOM 元素**

演示代码如下：

```jsx
class Demo extends React.Component {
    state = {
        isHot:true
    }
    showData = () => {
        alert(this.input1.value);
    }
    changeWeather = () => {
        const {isHot} = this.state
        this.setState({
            isHot:!isHot
        });
    }
    render() {
        return (
            <div>
                <input type="text" ref={c => { this.input1 = c;console.log('@',c) }} />&nbsp;
                <p>今天天气真{this.state.isHot ? '炎热':'凉爽'}</p>
                <button onClick={this.showData}>点击我弹出输入框中的文字</button>
                <button onClick={this.changeWeather}>点击我改变天气</button>
            </div>
        )
    }
}
ReactDOM.render(<Demo />, document.getElementById('test'))  
```

现象：初始化时，会在控制台打印出 `ref` 所在的节点，之后，通过点击绑定了 `changeWeather` 的按钮，模板更新，`render()` 被重新调用，打印结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230120234956002.png" alt="image-20230120234956002" style="zoom:50%;" />

究其原因在于：<strong style="color:red">在每次渲染时会创建一个新的函数实例，所以 React 清空旧的 `ref` 并且设置新的</strong>

要解决这一问题，就需要将 `ref` 的内联回调函数修改为 `class` 的绑定的函数，如下：

```jsx
callback = (c) => {
    this.input1 = c;console.log('@',c)
}

render() {
    return (
        <div>
            <input type="text" ref={this.callback} />&nbsp;
            <p>今天天气真{this.state.isHot ? '炎热':'凉爽'}</p>
            <button onClick={this.showData}>点击我弹出输入框中的文字</button>
            <button onClick={this.changeWeather}>点击我改变天气</button>
        </div>
    )
}
```



#### 03 `createRef()`的使用

示例代码如下：

```jsx
class Demo extends React.Component {
    myRef = React.createRef()
    showData = () => {
        console.log(this.myRef.current.value)
    }
    render() {
        return (
            <div>
                <input type="text" ref={this.myRef} />&nbsp;
                <button onClick={this.showData}>点击我弹出输入框中的文字</button>
            </div>
        )
    }
}
ReactDOM.render(<Demo />, document.getElementById('test'))  
```

:spiral_notepad:**总结**

- `React.createRef()` 调用后可以返回一个容器，该容器可以存储在被 `ref` 所标识的节点，该容器是 "专人专用" 的
- 该 `ref` 容器其实就是一个对象，对象中存储当前节点的信息，属性规定为 `current`



## 2.5 事件处理

我们在 React 官网上，可以看到下面一段话：<strong style="color:red">勿过度使用 Refs</strong>

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230121111320119.png" alt="image-20230121111320119" style="zoom:50%;" />

所以在处理事件时，如果我们可以不使用 `Refs`，那么就尽量不要使用，上述事件处理可以使用如下方式实现：

```jsx
class Demo extends React.Component {
    myRef = React.createRef()
    showData = (event) => {
        console.log(event.target.value)
    }
    render() {
        return (
            <div>
                {/*失去焦点后打印出输入框中文本*/}
                <input type="text" onBlur={this.showData}/>
            </div>
        )
    }
}
ReactDOM.render(<Demo />, document.getElementById('test'))    	
```

> 上述代码中，`event.target` 指向的也是当前触发事件的对象（在这里即：`input` 节点）



## 2.6 收集表单数据

包含表单的组件分类：

- 受控组件：对于组件中渲染到页面上的输入类的 DOM 节点是<strong style="color:red">现用现取</strong>
- 非受控组件：对于组件中渲染到页面上的输入类的 DOM 节点<strong style="color:red">会先将其存放到 state 中，之后每次调用时会从 state 中取出</strong>

非受控组件示例代码如下：

```jsx
class Login extends React.Component {
    handlerSubmit = (e) => {
        e.preventDefault()
        const {userName, password} = this
        console.log(`用户名为${userName.value}, 密码为${password.value}`)
    }
    render() {
        return (
            <form onSubmit={this.handlerSubmit}>
                用户名：<input ref={c => this.userName = c} name="userName" type="text"/><br/>
                密码：<input ref={c => this.password = c} name="password" type="password"/><br/>  
                <button>登录</button>
            </form>
        )
    }
}

ReactDOM.render(<Login/>, document.getElementById('test'))
```

> :warning:注意：`ref` 中的直接调用不能写成 `c => this.userName = c.value`



受控组件示例代码如下：

```jsx
class Login extends React.Component {
    state = {
        userName: '',
        password: ''
    }

    // 保存用户名到状态中
    saveUserName = (e) => {
        this.setState({
            userName: e.target.value
        })
    }
    // 保存密码到状态中
    savePassword = (e) => {
        this.setState({
            password: e.target.value
        })
    }
    handlerSubmit = (e) => {
        e.preventDefault()
        const {userName, password} = this.state
        console.log(`用户名为${userName}, 密码为${password}`)
    }
    render() {
        return (
            <form onSubmit={this.handlerSubmit}>
                用户名：<input onChange={this.saveUserName} name="userName" type="text"/><br/>
                密码：<input  onChange={this.savePassword} name="password" type="password"/><br/>  
                <button>登录</button>
            </form>
        )
    }
}
```

> 相较于非受控组件，受控组件中 `ref` 出现次数明显减少，因而运行效率较高



## 2.7 高阶函数的柯里化

在之前的受控组件中，有一段代码看起来是比较冗余的，如下：：

```jsx
// 保存用户名到状态中
saveUserName = (e) => {
    this.setState({
        userName: e.target.value
    })
}
// 保存密码到状态中
savePassword = (e) => {
    this.setState({
        password: e.target.value
    })
}
```

如果表单中要保存的数据字段特别多，就会更加冗长，我们可以考虑使用**函数柯里化**的方法去解决这一问题：

```jsx
saveFormData = (dataType) => {
    return (e) => {
        this.setState({
            [dataType]: e.target.value
        })
    }
}
```

```html
<form onSubmit={this.handlerSubmit}>
    用户名：<input onChange={this.saveFormData('userName')} name="userName" type="text"/><br/>
    密码：<input  onChange={this.saveFormData('password')} name="password" type="password"/><br/>  
    <button>登录</button>
</form>
```

:spiral_notepad:**备注**

1. `saveFormData()` 这个方法的返回值会交给 onChange 作为回调，回调里面传入的参数为 event 对象
2. `saveFormatData()` 在每个表单字段中传入的为当前表单的字段，事件触发时，会将该字段的值设为输入框中的值



:herb:**扩展1：高阶函数和函数柯里化**

高阶函数：如果一个函数符合下面 2 个规范中的任何一个，那么该函数就是高阶函数：

1. 若函数 A，接收的参数是一个函数，那么 A 就是高阶函数
2. 若函数 A，调用的返回值依然是一个函数，那么  A 就是高阶函数

> 常见的高阶函数：`Promise()`、`setTimeout()`、`arr.map()` 等



<strong style="color:red">函数的柯里化：通过函数调用继续返回函数的方式，实现多次接收参数最后统一处理的函数编码形式</strong>

示例代码如下：

```js
// 不使用函数柯里化
function add(a, b, c) {
    return a + b + c
}

add(1, 2, 3)

// 使用函数柯里化
function add(a) {
    return (b) => {
        return (c) => {
            return a + b + c
        }
    }
}
console.log(add(1)(2)(3))
```



:herb:**扩展2：不使用函数柯里化的实现**

```jsx
saveFormData = (dataType, e) => {
    this.setState({
        [dataType]: e.target.value
    })
}
```

```html
<form onSubmit={this.handlerSubmit}>
    用户名：<input onChange={(event) => { this.saveFormData('userName', event) }} name="userName" type="text" /><br />
    密码：<input onChange={(event) => { this.saveFormData('password', event) }} name="password" type="password" /><br />
    <button>登录</button>
</form>
```



## 2.8 DOM 的 diffing 算法













# 3 React 生命周期

## 3.1 旧的声明周期

React 旧的声明周期可以用下图表示：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/react%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F(%E6%97%A7).png" alt="react生命周期(旧)" style="zoom:50%;" />

:pencil:**解释说明**

1. 该图我们可以将其分为左右两部分，<strong style="color:red">左边部分代表组件初始化时的生命周期函数，右边部分代表组件更新时的生命周期函数</strong>
2. 组件初始化时钩子的执行顺序：`constructor(构造器)`  => `componentWillMount(组件即将挂载)` => `render(组件渲染到模板时)` => `componentDidMount(组件挂载完毕)` => `componentWillUnmount(组件即将卸载)`
3. 组件更新时钩子的执行顺序可以分为三条路线：
   - 组件使用 `setState` 正常更新状态时，路线为：`setState` => `shouldComponentUpdate` => `componentWillUpdate` => `render` => `componentDidUpdate` => `componentWillUnmount`
   - 组件强制更新时，路线为：`forceUpdate` => `componentWillUpdate` => `render` => `componentDidUpdate` => `componentWillUnmount`
   - 父组件向子组件传递参数时导致的状态改变时，路线为：`componentWillReceiveProps` => `shouldComponentUpdate` => `componentWillUpdate` => `render` => `componentDidUpdate` => `componentWillUnmount`



**示例1**

```jsx
class Demo extends React.Component {
    // 构造器
    constructor(props) {
        super(props)
        console.log('Demo -- constructor')
    }
	
    // 组件即将挂载
    componentWillMount() {
        console.log('Demo -- componentWillMount')
    }
	
    // 组件渲染到页面中
    render() {
        console.log('Demo -- render')
        return (
            <div>
                <h1>我是 Demo 组件</h1>
                <button onClick={this.unloadCompeont}>卸载组件</button>
            </div>
        )
    }
	// 组件挂载完毕
    componentDidMount() {
        console.log('Demo -- componentDidMount');
    }
	
    // 组件即将卸载
    componentWillUnmount() {
        console.log('Demo -- componentWillUnmount');
    }
    
    unloadCompeont = () => {
        ReactDOM.unmountComponentAtNode(document.getElementById('test'))
    }
}

ReactDOM.render(<Demo/>, document.getElementById('test'))
```

执行结果为如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230122121217012.png" alt="image-20230122121217012" style="zoom:50%;" />



**示例2**

```jsx
class Demo extends React.Component {
    
    state = {carName:'奔驰'}

    changeCar = () => {
        this.setState({
            carName: '奥拓'
        })
    }

    // 组件更新的闸门，通过返回的布尔值控制组件更新与否
    shouldComponentUpdate() {
        console.log('Demo -- shouldComponentUpdate')
        return true
    }

    // 组件即将更新
    componentWillUpdate() {
        console.log('Demo -- componentWillUpdate');
    }
    
    // 组件更新完毕
    componentDidUpdate() {
        console.log('Demo -- componentDidUpdate');
    }

    // 渲染组件
    render() {
        console.log('Demo -- render')
        return (
            <div>
                <h1>我是 Demo 组件</h1>
                <h2>今天我开的车是：{this.state.carName}</h2>
                <button onClick={this.changeCar}>换车</button>
            </div>
        )
    }

}

ReactDOM.render(<Demo />, document.getElementById('test'))
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230122130632433.png" alt="image-20230122130632433" style="zoom:50%;" />

> - `shouldComponentUpdate` 为模板更新的 "闸门"，该函数中必须要返回一个布尔值（true or false），表示是否允许更新
> - `componentDidUpdate` 可以接收两个参数，第一个是 `prevProps` （更新之前的 props 值），第二个为 `prevState` （更新之前的 state 值）



**示例3**

在 示例2 的基础上新增的内容

```jsx
// 添加类方法
forceChangeCar = () => {
    this.forceUpdate()
}
```

```html
<!-- 添加渲染内容 -->
<button onClick={this.forceChangeCar}>强制换车</button>
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230122131532794.png" alt="image-20230122131532794" style="zoom: 50%;" />



**示例4**

```jsx
class Parent extends React.Component {
    state = {
        num: 18
    }

    updateData = () => {
        const { num } = this.state
        this.setState({
            num: num + 1
        })
    }

    render() {
        return (
            <div>
                <h1>父组件</h1>
                <button onClick={this.updateData}>点我更新父组件传递给子组件的数据</button>
                <Children num={this.state.num} />
            </div>
        )
    }
}

class Children extends React.Component {
    // 当子组件接受到父组件新的数据执行
    componentWillReceiveProps() {
        console.log('Children -- componentWillRecieveProps')
    }

    // 组件更新的闸门，通过返回的布尔值控制组件更新与否
    shouldComponentUpdate() {
        console.log('Childrem -- shouldComponentUpdate')
        return true
    }

    // 组件即将更新
    componentWillUpdate() {
        console.log('Children -- componentWillUpdate');
    }

    // 组件更新完毕
    componentDidUpdate() {
        console.log('Children -- componentDidUpdate');
    }

    render() {
        console.log('Children -- render')
        const { num } = this.props
        return (
            <h2>从父组件中得到的数目为：{num}</h2>
        )
    }
}

ReactDOM.render(<Parent />, document.getElementById('test'))
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230122134758570.png" alt="image-20230122134758570" style="zoom:50%;" />

> `componentWillRecieveProps` 只有在子组件接受到父组件<strong style="color:red">新</strong>传递过来的参数时才会执行，初始化时传递的参数并不会经过该函数，并且该函数可以接收参数 `props` ，为父组件传递过来的参数，以对象形式存储





## 3.2 新的生命周期

之前我们引入的 React 的 JS 文件是 16.8.4 版本，要探究 React 新的生命周期，我们需要需要用到 17 版本的，这里以 17.0.1 做测试（前往 [React 官网](https://react.docschina.org/) 或 [BootCDN](https://www.bootcdn.cn/) 下载即可），引入下面的 JS 文件

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230122164553224.png" alt="image-20230122164553224" style="zoom:50%;" />

新版本中主要废弃了三个钩子，分别为：

- `componentWillReceiveProps` => `UNSAFE_componentWillReceiveProps`（以后版本可能会被废除）
- `componentWillUpdate()` => `UNSAFE_componentWillUpdate()`（以后版本可能会被废除）
- `componentWillMount` => `UNSAFE_componentWillMount`（以后版本可能会被废除）

新增的两个钩子，分别是：

- `getDerivedStateFormProps`
- `getSnapshotBeforeUpdate`

图示如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230122180616670.png" alt="image-20230122180616670" style="zoom: 50%;" />



### 3.2.1 getDerivedStateFormProps

`getDerivedStateFormProps` 我们可以翻译为 "从 props 中得到一个派生的状态"，该钩子能够接收两个参数，分别是 `props` 和 `state` ，返回值必须为一个 `Object` 或 `Null`

示例代码如下：

```jsx
static getDerivedStateFromProps(props, state) {
    console.log('接收到的 props 为', props, '当前的 state 为', state)
    return props
} 
```

:warning:**注意点**

1. <strong style="color:red">`getDerivedStateFormProps` 应该是挂载在组件的类上，而不是组件实例上</strong>，所以前面需要添加 `static` 进行修饰
2. 当前组件中一定要设置状态，否则该钩子会报错
3. 该钩子的作用：<strong style="color:red">将返回的 props 设置为当前组件的状态，并且后续无法更改</strong>
4. 该钩子适用于[罕见用例](https://zh-hans.reactjs.org/blog/2018/06/07/you-probably-dont-need-derived-state.html#when-to-use-derived-state)，并且派生的状态会导致代码冗余，并且组件难以维护，所以很少使用



### 3.2.2 getSnapShotBeforeUpdate

`snapshot` 在译为 "快照"，该钩子介于 `render` 和 `componentDidUpdate` 之间，用于在更新之前获取快照，语法格式如下：

```jsx
getSnapshotBeforeUpdate(prevProps, prevState)
```

- `prevProps`：更新之前的 props 值
- `prevState`：更新之前的 state 值
- 返回值：`snapshot` 或 `null`，该返回值最终会传递给 `componentDidUpdate`



**案例：模拟看新闻时视图固定的效果**

```css
/* 样式文件 */
* {
    margin: 0;
    padding: 0
}

.list {
    height: 180px;
    width: 200px;
    background: pink;
    overflow: auto;
}

.news {
    height: 30px;
    background: skyblue;
}
```

```react
<!-- 准备好容器 -->
<div id="test"></div>

<!-- 引入 React 的核心库 -->
<script src="../js2/react.development.js"></script>
<!-- 引入 react-dom，用于支持 React 操作 DOM  -->
<script src="../js2/react-dom.development.js"></script>
<!-- 引入 bebel，用于将 jsx 转化为 js -->
<script src="../js2/babel.min.js"></script>

<script type="text/babel">
    class Demo extends React.Component {
        state = { newArr: [] }

        componentDidMount() {
            setInterval(() => {
                // 获取原状态
                const { newArr } = this.state
                // 模拟通过请求获取一条新闻
                const news = '新闻' + (newArr.length + 1)
                // 更新状态, 将最新的新闻置顶
                this.setState({
                    newArr: [news, ...newArr]
                })
            }, 1000)
        }

        // 通过getSnapshotBeforeUpdate 和 componentDidUpdate 来完成视图固定
        getSnapshotBeforeUpdate() {
            return this.refs.list.scrollHeight
        }

        componentDidUpdate(prevProps, prevState, height) {
            this.refs.list.scrollTop += this.refs.list.scrollHeight - height    // 差值为固定值，其中这里完全就可以直接写 30px
        }

        render() {
            return (
                <div className="list" ref="list">
                    {
                        this.state.newArr.map((ls, index) => {
                            return <div key={index} className="news">{ls}</div>
                        })
                    }
                </div>
            )
        }
    }

    ReactDOM.render(<Demo />, document.getElementById('test'))
</script>
```

显示效果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/tutieshi_402x360_10s.gif" alt="tutieshi_402x360_10s" style="zoom:50%;" />



# 4 React 脚手架

我们在之前是通过在 `<script>` 标签上添加 `type=text/babel` 来使得浏览器运行 JSX 文件，但是**这会使得你的网站变慢，并不适合生产环境**，在平时开发过程中，我们更多地是设置 JSX 预处理器来自动转化所有 `<script>` 标签的内容，设置 JSX 预处理器的过程我们也可以称为**搭建脚手架**



## 4.1 概念

1. **xxx 脚手架** 用来帮助程序员快速创建一个基于 xxx 库的模板项目

   - 里面包含了所需要的配置（语法检查、JSX 预处理器、devServer）

   - 下载了了所有相关的依赖

   - 可以直接运行一个简单效果

2. React 提供了一个用于创建 React 项目的脚手架库：`create-react-app`

3. 项目的整体技术架构为：react + webpack + es6 + eslint

4. 使用脚手架开发的项目的特点：模块化、组件化、工程化



## 4.2 初始化脚手架

1. 全局安装 `create-react-app`

   ```bash
   npm install -g create-react-app
   ```

2. 切换指定目录

   ```bash
   cd xxx
   ```

3. 创建项目

   ```bash
   create-react-app hello-react
   ```

4. 启动项目

   ```bash
   npm start
   # OR
   yarn start
   ```



## 4.3 脚手架文件介绍

```
react-test                
├─ public: 静态资源文件夹                 
│  ├─ favicon.ico: 网站页签图标         
│  ├─ index.html: 主页面          
│  ├─ logo192.png: logo 图         
│  ├─ logo512.png: logo 图        
│  ├─ manifest.json: 应用加壳的配置文件       
│  └─ robots.txt: 爬虫协议文件          
├─ src: 源码文件夹                   
│  ├─ App.css: App 组件的样式             
│  ├─ App.js: App 组件              
│  ├─ App.test.js: 用于给 App 做测试         
│  ├─ index.css: 样式           
│  ├─ index.js: 入口文件            
│  ├─ logo.svg: logo 图            
│  ├─ reportWebVitals.js: 页面性能分析文件（需要 web-vitals 库的支持）  
│  └─ setupTests.js       
├─ package-lock.json      
├─ package.json           
└─ README.md              
```

