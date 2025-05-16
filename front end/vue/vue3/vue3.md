[toc]

# 1 vue3 基础

## 1.1 vue3 的新特性

1. Options API -> Composition API
2.  重写了双向数据绑定：由原来的 defineProperty -> proxy 实现
3. 引入 Tree shaking 特性，即将全局 API 进行分块，如果我们不使用某功能，那么该功能将不会包含在基础包中，例如 watch 就需要使用 `import { watch } from 'vue'` 引入
4. 支持 Fragment，同时新增 Suspense teleport 和 多 v-model 用法
5. 使用 patch flag（打标识）的方式优化 VDOM



## 1.2 安装脚手架

01 通过下面的命令创建一个 vite 项目

```bash
npm init vite@latest
```

> 选择 vue -> 选择 typescript



02 之后使用 `npm i` 命令下载好相关依赖



03 再使用 `npm run dev` 启动 vite 项目



OR



还有一种更为简单的创建 vue-vite 项目的方法，直接通过如下命令：

```bash
npm init vue@latest
```

我们在这种情况下会拥有更多的配置选择

![image-20230516195129746](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230516195129746.png)



## 1.3 VSCode 插件配置

在 VSCode 中安装如下两个插件

![image-20230516195937848](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230516195937848.png)

![image-20230516200009162](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230516200009162.png)

同时将 Vetur 插件禁用

![image-20230516200057861](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230516200057861.png)

> Vetur 主要是为 Vue2 打造的，而 Volar 是为 Vue3 打造的，两者不能同时使用，否则会发生冲突



## 1.4 模板语法 && vue 指令

可以使用 Options API，也即 Vue2 的形式，也可以使用 setup 函数模式，所有成员属性和函数通过 return  

暴露出去

```js
<script>
export default {
  setup() {
    const a = 1;
    return {
      a
    }
  }
}
</script>
```

但是每次都这样通过 return 将其暴露是十分繁琐的，因此 Vue 提供了 setup 语法糖，我们可以将其转换为如下形式

```ts
<script setup lang="ts">
const a: number = 1;
</script>
```



vue 指令示例如下：

1. v-if … v-else-if … v-else

2. 动态改变事件

   ```html
   <template>
       <button @[event]="fn">change event's button</button>
   </template>
   ```

   ```ts
   <script setup lang="ts">
   import { ref } from 'vue'
   let event = ref('click')
   const eventList: string[] = ['click', 'dblclick']
   const fn: () => void = function() {
     const index: number = Math.floor(Math.random() * eventList.length)
     event.value = eventList[index]
     console.log('current event is ', event)
   }
   </script>
   ```

3. v-bind：单向数据绑定，v-model：双向数据绑定

   ```html
   <input type="text" v-model="data">
   <h3>{{ data }}</h3>
   ```

   ```ts
   import { ref } from 'vue'
   const data = ref()
   ```

4. v-once 和 v-memo



## 1.5 虚拟 DOM 和 diff 算法

虚拟 DOM 即通过 JS 生成的一个 AST 节点树

> AST："abstract syntax tree"，抽象语法树

Vue3 靶场：[传送门](https://template-explorer.vuejs.org/)

下面有一张 diff 算法的简略图供参考

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/1fe57a274d8644bfacf44526e79d57bc.png" alt="img" style="zoom:50%;" />



## 1.6 ref 全家桶

### 1.6.1 ref、isRef、Ref

```html
<template>
  <h1>我的名字是：{{ name }}</h1>
  <button @click="modifyName">修改名字</button>
</template>
```

```ts
<script setup lang="ts">
/**
 * ref 用来给一个值创建一个响应式引用
 * isRef 用来判断一个值是否是一个 ref 创建的响应式引用
 * Ref 用来定义一个 ref 创建的响应式引用
 */
import { ref, isRef } from 'vue' 
// import { Ref } from 'vue'
// type M = { 
//   name: string
// }
// const man:Ref<M> = ref({name: 'jack'})
let name = ref<string>('John')

const modifyName = () => {
  console.log('name是否是一个 ref 对象?', isRef(name));
  if(name.value === 'john') {
    name.value = 'jack'
  } else {
    name.value = 'john'
  }
}
</script>
```



### 1.6.2 shallowRef vs ref

```html
<template>
  <h1>我是一个ref对象，名字是：{{ person.name }}</h1>
  <button @click="changeRef">改变ref对象的名字</button>
  <hr>
  <h1>我是一个shallowRef对象，名字是：{{ human.name }}</h1>
  <button @click="changeShallowRef">改变shallowRef对象的名字</button>
  <hr>
  <button @click="changeAll">一起修改</button>
</template>
```

```js
<script lang="ts" setup>
/**
* ref 用创建一个深层次的响应式引用
* shallowRef 用来创建一个浅层次的响应式引用
* triggerRef 用来触发一个响应式引用的更新
*/
import { ref, shallowRef } from 'vue'
type M = {
  name: string
}
const person = ref<M>({ name: 'jack' })

const human = shallowRef<M>({ name: 'smith'})

const changeRef = () => {
  person.value.name = 'smith'
}
const changeShallowRef = () => {
  // human.value.name = 'jack' // 修改无效
  human.value = { name: 'jack' } // 修改有效
}

// 警告：不要在模板中同时修改 ref 和 shallowRef，会导致无法更新异常
// 修改 ref 时，底层会调用 triggerRef 方法，而 triggerRef 方法会触发 shallowRef 的更新
const changeAll = () => {
  person.value.name = 'smith'
  human.value.name = 'jack'
}
</script>
```

>shallowRef 只会监听它的值的第一层属性，所以当我们修改它的值时，它的第一层属性并没有发生变化，所以不会触发更新



### 1.6.3 customRef

customRef 帮助我们自定义响应式，例如如果我们想要在指定时间后触发更新，那么我们可以通过如下方式实现：

```ts
<script lang="ts" setup>
import { customRef } from 'vue'
// 定制一个自己专属的 ref
function myRef<T>(value: T) {
  let timer: any
  return customRef((track, trigger) => {
      return {
      get() {
          track()	// 追踪依赖
          return value
      },
      set(newValue: T) {
          // 如果 timer 之前没有清除，就清除 timer
          clearTimeout(timer)
          timer = setTimeout(() => {
              console.log('触发更新了...');
              timer = null
              value = newValue
              trigger()	// 触发更新
          }, 2000)
      }
      }
  })
}
const name = myRef('jack')
const changeName = () => {
  name.value = 'bob'
}
</script>
```



### 1.6.4 补充

我们每次在控制打印输出 ref 对象，想要看到其修改的值，需要点击两次才能看到，如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230517003111452.png" alt="image-20230517003111452"  />

vue 给我们提供了一种类似于 Java 中的 `toString()` 方法，这需要我们将浏览器的【自定义格式化配置】打开

![image-20230517003528748](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230517003528748.png)

然后就是通过 ref ，我们是可以得到 DOM 的

```html
<div ref="dom">我的名字</div>
```

```ts
<script lang="ts" setup>
import { ref, onMounted } from 'vue'
const dom = ref(null)
onMounted(() => {
  console.log(dom.value)
})
</script>
```



## 1.7 reactive 全家桶

`reactive` 和 `ref` 类似，都是用来给数据做响应式，但是他们之间最大的区别在于：<strong style="color:red">`reactive` 只能用来给引用类型做响应式，而 `ref` 可以给任意类型做响应式</strong>

```html
<template>
  <input type="text" v-model="person.name">
  <hr>
  <input type="text" v-model="person.age">
  <hr>
  <button @click="changePerson">改变person属性</button>
</template>
```

```ts
<script lang="ts" setup>
import { ref, reactive } from 'vue'
// 1. ref 支持所有类型，而 reactive 仅支持引用类型，例如 Object、Array、Set、WeakSet、Map、WeakMap 等等
const person = reactive({
  name: 'John',
  age: 18
})

const changePerson = () => {
  // 2. ref 取值和赋值都需要添加 .value，reactive 不需要使用 .value
  person.name = 'jack'
  person.age = 20
}
</script>
```



**reactive 给数组做响应式示例**

```html
<template>
  <ul>
    <li v-for="(item, index) in list" :key="index">{{ item }}</li>
  </ul>
  <button @click="add">添加元素</button>
</template>
```

```ts
<script lang="ts" setup>
import { reactive } from 'vue'
let list: string[] = reactive([])
let t: string[] = ['a', 'b', 'c', 'd', 'e']
const add = () => {
  list.push(...t)
  // 注意：不能直接将 t 赋值给 list,因为 list 是 proxy 对象，直接赋 t,会破坏 list 原有的结构
}
</script>
```

:star2:对于某些做了响应式对象，如果我们希望将其设置为只读的，可以使用 `readonly`

```html
<template>
  个人信息：{{ readObj.name }}
  <button @click="modify">修改信息</button>
  <button @click="change">修改源</button>
</template>
```

```ts
<script lang="ts" setup>
import { reactive, readonly } from 'vue'
const person = reactive({ name: 'John', age: 18})
const readObj = readonly(person)
const modify = () => {
  readObj.name = 'Alice'  // 无法修改，因为 readObj 是只读的
}
const change = () => {
  person.name = 'Bob'   // 但是如果修改源，那么做只读处理的对象，属性就会发生改变
}
</script>
```



**shallowReactive**

```html
<template>
  <h1>obj: {{ obj }}</h1>
  <button @click="edit">编辑</button>
</template>
```

```ts
<script lang="ts" setup>
import { shallowReactive } from 'vue'
const obj = shallowReactive({
  foo: { 
    bar: { 
      num: 1
    }
  }
})
const edit = () => {
  // obj.foo.bar.num = 100  // 无法深层次进行修改
  obj.foo = { bar: { num: 100 } } // 对于 shallowReactive，我们只能做浅层次的修改操作
}
</script>
```

> 和 `ref` 和 `shallowRef` 类似，请不要在同一个作用域中同时修改 `reactive` 对象和 `shallowReactive` 对象 



## 1.8 toRef、toRefs、toRaw、markRow

### 1.8.1 toRef

`toRef` 应用于【响应式对象】，基于响应式对象的一个属性，创建一个对应的 ref，在数据源和该 ref 对象之间为引用关系，两者保持同步。

```html
<template>
  <h1>姓名为：{{ person.name }}</h1>
  <button @click="modify">修改姓名</button>
</template>
```

```ts
<script lang="ts" setup>
import { reactive, toRef } from 'vue'
const person = reactive({
  name: 'Alice',
  age: 20
})

const name = toRef(person, 'name')  // 将响应式数据 person的name属性单独拿出来做成一个 ref 响应式数据

const modify = () => {
  name.value = 'Bob'
}
```

> `toRef` 的作用类似于解构



### 1.8.2 toRefs

```ts
// 手动实现 toRefs
const toRefs = <T extends object>(object: T) => { // 传入一个 object 对象，该对象的类型为 T
  const map: any = {} // 定义一个 map 容器

  for (let key in object) {
    map[key] = toRef(object, key) // 遍历 object ，将 key-value 存储到map容器中，其中value是经过加工的value
  }

  return map
}
```

**使用示例**

```ts
const { name, age} = toRefs(person) // 将 name 和 age 全部解构出来并单独做一个响应式引用
/**
 * 我们如果不使用 toRefs ，那么得按如下写法
 * const name = toRef(person, 'name')
 * consst age = toRef(person, 'age')
 */
```



### 1.8.3 toRaw

`toRaw`，顾名思义，就是将做了响应式的对象或值去除响应响应式，返回原始数据

```ts
import { reactive, ref, toRaw, toRef } from 'vue'
const person = reactive({ name: 'John', age: 18})
const friend = ref('John')
const name = toRef(person, 'name')
console.log('person 原始数据为：', toRaw(person))   // 可以将reactive做的响应式数据转换为原始数据
console.log('friend 原始数据为： ', toRaw(friend))  // 不能够将ref做的响应式数据转换原始数据
console.log('person 中的name的原始数据为：', toRaw(name))   // 不能够将toRef解构出来的响应式数据转换为原始数据
```

> `toRaw` 的底层原理就是取出响应式数据的 `_v_raw` 属性



### 1.8.4 markRaw

markRaw 的作用：<strong style="color:red">标记一个对象，使其永远不会再成为响应式对象。</strong>



## 1.9 响应式源码实现

这里先放一放



## 1.10 computed 计算属性

计算属性，即<strong style="color:red">当依赖的属性的值发生变化时，才会触发他的更改（即官网所说的 "自动追踪响应式依赖"）</strong>，如果依赖的值不发生变化时，使用的是缓存中的属性值。

**计算属性经典案例**

```html
<template>
  <div>
    姓：<input type="text" v-model="firstName"><br>
    名：<input type="text" v-model="lastName"><br>
    全名：{{ fullName }} <br>
    <button @click="changeName">修改全名从而影响姓和名的值</button>
  </div>
</template>
```

```ts
<script lang="ts" setup>
import { ref, computed } from 'vue'
let firstName = ref('张')
let lastName = ref('三')
// 1. 选项式写法
let fullName = computed<string>({
    get() {
        return firstName.value + '-' + lastName.value
    },
    set(newValue) {
        [firstName.value, lastName.value] = newValue.split('-')
    }
})
const changeName = () => {
    fullName.value = '你-好'
}
</script>
```

**计算属性的第二种写法：函数式写法**

```ts
// 2. 函数式写法（只支持 getter 方法，不允许修改值）
let fullName = computed<string>(() => firstName.value + '-' + lastName.value)
```



计算属性的应用场景：适用于**数据联动**的场景，在这种场景下，如果我们某个数据的值与其他数据的值是相关联的，那么如果我们不使用计算属性，我们每次在修改数据的时候需要重新调用一个更新函数，这无疑并不是一个优雅的解决方案，而如果我们使用 computed 计算属性，那么就能 "化繁为简"，接下来，我们将以实现一个购物车的 "单价-数量-总价" 之间的联动为例进行演示：

```html
<template>
  <table border cellspacing="0" cellpadding="0" width="500px">
    <tr>
        <th>物品名称</th>
        <th>物品单价</th>
        <th>物品数量</th>
        <th>物品总价</th>
        <th>操作</th>
    </tr>
    <tr v-for="(item, index) in info" :key="item.id">
        <td align="center">{{ item.name }}</td>
        <td align="center">{{ item.price }}</td>
        <td align="center"><button @click="(item.num > 1? item.num-- : null)">-</button> {{ item.num }} <button @click="(item.num < 100 ? item.num++ : null)">+</button></td>
        <td align="center">{{ item.price * item.num }}</td>
        <td align="center"><button @click="handleDel(index)">删除</button></td>
    </tr>
    <tr>
        <td align="center">总计</td>
        <td colspan="4" align="center">{{ total }}</td>
    </tr>
  </table>
</template>
```

```ts
<script lang="ts" setup>
import { reactive, computed } from 'vue'
interface Data {
    id: number
    name: string, 
    price: number,
    num: number
}

const info = reactive<Data[]>([
    { 
        id: 12819,
        name: '红帽子',
        price: 88,
        num: 1
    },
    { 
        id: 77121,
        name: '皮鞋',
        price: 220,
        num: 1
    },
    { 
        id: 12121,
        name: '围巾',
        price: 33,
        num: 1
    }
])

const handleDel = (index: number) => {
    info.splice(index, 1)
}

// 重点：计算所有商品总价的 total 字段
let total = computed<number>(() => {
    return info.reduce((prev, next) => {
        return prev + next.price * next.num
    }, 0)
})
</script>
```

效果图如下：

![image-20230517205727128](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230517205727128.png)



## 1.11 watch 侦听器

1. 对基础类型的监听：单个数值

   ```ts
   const num = ref(null)
   watch(num, (newVal, oldVal) => {
       console.log('num的值发生改变, 旧值为：', oldVal, ', 新值为： ', newVal)
   })
   ```

2. 对基础类型的监听：多个数值

   ```ts
   const a = ref(null), b = ref(null)
   watch([a, b], (newVal, oldVal) => {
       // newVal 和 oldVal 都为数组
       console.log('旧值为：', oldVal)
       console.log('新值为：', newVal)
   })
   ```

3. 对引用类型的监听（使用 reactive 装饰）：对整个对象进行监听

   ```js
   const person = reactive({
       name: 'John',
       address: {
           country: 'China',
           province: 'jiangxi '
       },
       other: {
           userInfo: {
               username: 'Dio',
               password: 123
           }
       }
   })
   
   watch(person, (newVal, oldVal) => {
       // 由于 person 为引用类型，所以会导致这里的 newVal 和 oldVal 是相同的
       console.log('旧值为：', oldVal)
       console.log('新值为：', newVal)
   })
   ```

   >值得注意的是：使用 reacitve 做的响应式对象，当我们使用 watch 监听时，默认开启的是深度监听
   >
   >当对象中的任何属性发生改变时，都会触发监听器

4. 对引用类型的监听（使用 reactive 装饰）：对单个属性进行监听

   ```ts
   const person = reactive({
       name: 'John',
       address: {
           country: 'China',
           province: 'jiangxi '
       },
       other: {
           userInfo: {
               username: 'Dio',
               password: 123
           }
       }
   })
   
   watch(() => person.name, (newVal, oldVal) => {
       console.log('#旧值为：', oldVal)
       console.log('#新值为：', newVal)
   })
   ```

   >这里解释一下为什么我们需要使用回调函数（准确来说是一个 getter）而不能直接使用 person.name, 因为 person.name 只是一个字符串，并不是一个 proxy 代理对象

5. 对引用类型的监听（使用 ref 装饰）：对整个对象进行监听

   ```ts
   const animal = ref({
       name: 'blob',
       category: 'dog'
   })
   
   watch(animal, (newVal, oldVal) => {
       console.log('@旧值为：', oldVal)
       console.log('@新值为：', newVal)
   }, { deep: true, immediate: true })
   ```

   >ref 做的响应式对象当我们使用 watch 进行监听时，默认是不开启深度监听的，需要我们手动配置;
   >
   >除此之外，vue3 保留了 vue2 的监视属性的特性，还可以配置 immediate ，用来表示是否在初始化时就侦听一次

vue3 中 watch 的配置项新增了一个 flush，用来控制监听器执行顺序，配置项的值有如下三个：

- **`pre`**：组件更新之前调用
- **`sync`**：同步执行
- **`post`**：组件更新之后调用



## 1.12 watchEffect（高级侦听器）

watchEffect 是一个高级侦听器，它会侦听所有响应式数据，一旦运行就会立即开始监听，组件卸载时才会停止监听，官网对其的描述如下：

```ts
function watchEffect(
  effect: (onCleanup: OnCleanup) => void,
  options?: WatchEffectOptions
): StopHandle
```

- effect：该参数为一个副作用函数，用来注册清除回调，该回调会在副作用下一次执行被调用，可以用来节流和防抖或者请求等待中的异步请求
- StopHandle：返回值为一个函数变量名，通过执行该函数手动关闭侦听

**示例代码**

```html
<template>
    姓名：<input type="text" v-model="name"> <br>
    个人年龄：<input type="text" v-model="person.age">
    <button @click="clearWatch">关闭 watchEffect 侦听器</button>
</template>
```

```ts
<script lang="ts" setup>
import { ref, reactive, watchEffect } from 'vue'
const name = ref('zhangsan')
const person = reactive({
    name: 'zhangsan',
    age: 18
})

const stop = watchEffect((onCleanup) => {
    console.log('name=', name.value)
    console.log('persson.age = ', person.age);
    onCleanup(() => {
        console.log('before handle')
    })
})

const clearWatch = () => {
    stop()
}
</script>
```



对于 watchOptions ，vue 官方给我们提供了如下配置项：

```ts
interface WatchEffectOptions {
  flush?: 'pre' | 'post' | 'sync' // 默认：'pre'
  onTrack?: (event: DebuggerEvent) => void
  onTrigger?: (event: DebuggerEvent) => void
}
```

> 其中 onTrigger 可以帮助我们在书写代码时进行调试



## 1.13 Vue3 的生命周期（重要）

> vue3 生命周期官方文档：[在线传送门](https://cn.vuejs.org/api/options-lifecycle.html)

vue3 生命周期图：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/vue3_lifecycle.png" alt="vue3_lifecycle" style="zoom:50%;" />

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/vue3_lifecycle_compare.png" alt="vue3_lifecycle_compare" style="zoom: 80%;" />

示例如下：

```ts
<script lang="ts" setup>
import { onBeforeMount, onMounted, onBeforeUpdate, onUpdated, onBeforeUnmount, onUnmounted} from 'vue'
// vue3 中的 beforeCreate和 created 被 setup 方法本身所替代，在 setup 中可以做 beforeCreate 和 created 的任何操作

console.log('setup...')
// onBeforeMount: 在组件挂载之前被调用
onBeforeMount(() => {
    console.log('onBeforeMount...')
})

// onMounted: 在组件挂载之后被调用
onMounted(() => {
    console.log('onMounted...')
})

// onBeforeUpdate: 在组件更新前被调用
// 组件更新的方法有很多，但是这里推荐使用 :key = "flag" , 通过更新 flag 将组件强制更新
onBeforeUpdate(() => {
    console.log('onBeforeUpdate...')
})

// onUpdated: 在组件更新之后被调用
onUpdated(() => {
    console.log('onUpdated...')
})

// onBeforeUnmount: 在组件卸载之前被调用
onBeforeUnmount(()=> {
    console.log('onBeforeUnmount...')
})

// onMounted: 在组件卸载之后被调用
onUnmounted(() => {
    console.log('onMounted...')
})
</script>
```



# 2 进阶篇

## 2.1 BEM 架构 && scss 语法

BEM 架构其实是一种 CSS 命名方法论，它将网站样式的布局进行分层，其中 BEM 对应三个英文单词：

- Block：块层，即命令空间，显示样式的范围

  > 例如：element plus 中的 `el-button` 中的 `el` 就是一个块层 
- Element：元素层，表示该块内的元素

  >例如：element plus 中的 el-input__inner 中的 inner
- Modifier：修饰符层，用于修改现有元素的外层样式

  >例如：element plus 中的 `el-button--primary`，中的 primary 

对于 BEM 架构，我们只需要遵循如下规则：

- 使用 `__` 将块与元素名分隔
- 使用 `--` 将元素名与修饰符分隔
- 一切样式都是一个类，不能嵌套



**Sass 语法介绍**

常用功能如下：

- 嵌套

- 父级选择器 `&`

- 变量：`$variable`

- 插值：`#{}`

- 跳出嵌套：`@at-root`

- 混入和引入：`@mixin` 、`@include`

  ```scss
  @mixin large-text {
    font: {
      family: Arial;
      size: 20px;
      weight: bold;
    }
    color: #ff0000;
  }
  
  .page-title {
    @include large-text;
    padding: 4px;
    margin-top: 10px;
  }
  ```

- 继承样式：使用 `%` 定义继承样式，使用 `@extend` 完成继承

  ```scss
  .error {
    border: 1px #f00;
    background-color: #fdd;
  }
  .seriousError {
    @extend .error;
    border-width: 3px;
  }
  ```

- 外部样式文件导入：`@import`
- 注释：`/**/` 、`//`



**vue 中安装 sass**

```bash
npm i sass
```

我们自己手动来完成一个 BEM 架构的生成文件，如下：

```scss
$namespace: 'xm' !default;
$block-sel: '-' !default;   // 块层分隔符
$elem-sel: '__' !default;   // 元素层分隔符
$mod-sel: '--' !default;    // 修饰层分隔符

// 下面函数用于生成块， xm-block
@mixin b($block) {
    $B:#{$namespace + $block-sel + $block};
    .#{$B} {
        @content;   // @content 相当于占位符，当变量 $B 的类名有样式内容时会进行替换
    }
}

// 下面函数用于生成元素, xm-block__inner
@mixin e($elem) {
    $selector: &;   // & 用于得到父级选择器名
    @at-root {
        #{$selector + $elem-sel + $elem} {
            @content;
        }
    }
}

// 下面函数用于生成修饰，xm-block--primary
@mixin m($mod) {
    $selector: &;
    @at-root {
        #{$selector + $mo-sel + $mod} {
            @content;
        }
    }
}
```

之后我们需要在 vite.config.ts 将其配置为全局文件

```ts
export default defineConfig({
  plugins: [vue()],
  css: {
    preprocessorOptions: {
      scss: {
        additionalData: `@import "./src/bem.scss";`
      }
    }
  }
})
```



**测试程序**

```html
<template>
    <h1 class="xm-rf">测试使用1</h1>
    <h1 class="xm-rf__inner">测试使用2</h1>
    <h1 class="xm-rf--primary">测试使用3</h1>
</template>
```

```scss
<style scoped lang="scss">
@include b(rf) {
    color: red;
    @include e(inner) {
        color: blue;
    }
    @include m(primary) {
        color: green;
    }
}
</style>
```



## 2.2 父子组件传参

父组件通过 `v-bind` 绑定数据，子组件通过 `definrProps` 接收传递过来的值

示例代码如下：

- 父组件传入数据

  ```vue
  <template>
      <Child title="我是一个标题" :data="data"/>
  </template>
  
  <script lang="ts" setup>
  import Child from './components/Child.vue'
  const data = [1, 2, 3, 4, 5]
  
  </script>
  ```

  > 对于字符串字面量，可以直接通过 "k-v" 的形式传入，而对于非字符串字面量（即可解析的表达式），我们需要通过 `v-bind` 解析后传入

- 子组件接受数据

  ```vue
  <template>
      标题：{{ title }}
      data数组：{{ data }}
  </template>
  
  <script lang="ts" setup>
  defineProps<{
      title: string,
      data: number[]
  }>()
  </script>
  ```

  对于 js 语法，我们可以改为下面写法：

  ```js
  defineProps({
      title: {
          type: String,
          default: ''
      },
      data: {
          type: Array,
          default: []
      }
  })
  ```



如果我们想要让在 ts 环境设置默认值，那么我们可以按如下配置：

```ts
withDefaults(defineProps<{
    title: string, 
    data: number[]
}>(), {
    title: 'zs',
    data: () => [666]
})
```

> withDefaults 接收两个参数，第一个参数为用来定义接收参数的默认值，第二个参数用来定义接收参数的默认值，我们在给引用类型（例如数组、对象）设置默认值时需要使用回调返回



**子组件向父组件通信**

使用 `defineEmits` 定义触发自定义事件，通过自定义事件给父组件传值

**<u>Child.vue</u>**

```vue
<template>
    <button @click="send">给父组件传值</button>
</template>

<script lang="ts" setup>
const emit = defineEmits(['on-click'])  // 以数组形式定义自定义事件
const send = () => {
    emit('on-click', 'Hello World!!!')  // 当点击按钮时触发自定义事件
}
</script>
```

<u>**App.vue**</u>

```vue
<template>
    <Child @on-click="getMsg"/> 
</template>

<script lang="ts" setup>
import Child from './components/Child.vue'

const getMsg = (data: string) => {
    console.log(data) 
}
</script>
```

在 ts 环境下，我们还可以将自定义事件、传入参数和返回值作为泛型传入，如下：

```ts
const emit = defineEmits<{
    (e: 'on-click', msg: string): void
}>()
```



**子组件保留数据给父组件引用**

在 vue2 中，通过配置 `ref`，我们在父组件中拿到子组件中的属性和方法，在 vue3 中同样可以，但是如果我们在子组件中使用 `<script setup>` ，那么该组件默认是私有的，此时我们就需要使用 `defineExpose` 这个宏显示暴露

<u>**Child.vue**</u>

```vue
<template>
    
</template>

<script lang="ts" setup>
const a = 1
const b = () => {
    console.log('Hello World')
}

defineExpose({
    a, 
    b
})
</script>
```

<u>**App.vue**</u>

```vue
<template>
    <Child ref="child"/> 
</template>

<script lang="ts" setup>
import Child from './components/Child.vue'
import { onMounted, ref }  from 'vue'
// 注意：这里typeof后面是组件名，而不是 ref 值
const child = ref<InstanceType<typeof Child>>()
onMounted(() => { 
   console.log(child.value?.a)
   console.log(child.value?.b)
})
</script>
```



## 2.3 全局组件、递归组件、局部组件

局部组件的注册很简单，和 vue2 基本一致，我们只需要通过 **`import`** 导入即可，<u>但是 vue3 的导入遵循 ES6 语法，需求写全后缀名</u>，而 vue2 的后缀是可以省略的



全局组件的注册，需要按如下方法：

```ts
// main.ts
import { createApp } from 'vue'
import App from './App.vue'

// 导入需要注册的全局组件
import Card from './components/Card.vue'
export const app = createApp(App)

// 挂载全局组件
app.component('Card', Card)
app.mount('#app')
```

> 全局组件的应用：在整个页面应用中需要频繁被引用的组件



**递归组件**

递归组件最为典型的应用就是树形控件，简单模拟如下：

<u>**App.vue**</u>

```vue
<template>
    <Tree :list="treeList" />
</template>

<script lang="ts" setup>
import Tree from './components/Tree.vue'
import { reactive } from 'vue'
interface Tree {
    name: string,
    isChecked: boolean,
    children?: Tree[]
}

const treeList = reactive<Tree[]>([
    {
        name: '1',
        isChecked: false,
        children: [
            {
                name: '1-1',
                isChecked: true
            }
        ]
    }, {
        name: '2',
        isChecked: false
    }, {
        name: '3',
        isChecked: false,
        children: [
            {
                name: '3-1',
                isChecked: false
            }, {
                name: '3-2',
                isChecked: false,
                children: [
                    {
                        name: '3-2-1',
                        isChecked: true,
                    }
                ]
            }
        ]
    }
])

</script>

<style scoped>

</style>
```

 

<u>**Tree.vue**</u>

```vue
<template>
    <div v-for="item in list" :key="item.name" class="tree">
        <input type="checkbox" v-model="item.isChecked"> <span>{{ item.name }}</span>
        <!-- 这里需要注意，为了防止内存泄漏，一定要判断还有没有子项，没有则通过v-if销毁 -->
        <Tree :list="item.children" v-if="item.children?.length"/>
    </div>
</template>

<script lang="ts" setup>
interface Tree {
    name: string,
    isChecked: boolean,
    children?: Tree[]
}
defineProps<{
    list?: Tree[]
}>()
</script>

<style scoped lang="scss">
.tree {
    margin-left: 20px;
}
</style>
```



:thought_balloon:**如果我们想要更改递归组件的名字，如上，将其改为 "MyTree"，那么我们有两种方式解决：**

1. 单独再开一个 script 标签，在里面定义组件的 name 即可

   ```ts
   <script lang="ts" setup>
   // ……
   </script>
   
   <script lang="ts">
   export default {
       name: 'MyTree'
   }
   </script>
   ```

2. 引入  unplugin-vue-define-options 插件，然后在配置文件中进行注册

   ```ts
   // vite.config.ts
   import DefineOptions from 'unplugin-vue-define-options/vite'
   import Vue from '@vitejs/plugin-vue'
    
   export default defineConfig({
     plugins: [Vue(), DefineOptions()],
   })
   ```

   ```json
   // tsconfig.json
   {
   	"compilerOptions": {
   		 "types": ["unplugin-vue-define-options/macros-global"]
   	}
   }
   ```

   最后使用宏定义

   ```ts
   defineOptions({
   	name: 'MyTree'
   })
   ```

   > 目前 vue3.2 已经支持这个宏，不需要我们再手动配置



给递归组件添加事件时，一定要注意使用 `stop` 修饰符阻止其冒泡

```html
<div v-for="item in list" :key="item.name" class="tree" @click.stop="handle(item, $event)">
    <input type="checkbox" v-model="item.isChecked"> <span>{{ item.name }}</span>
    <!-- 这里需要注意，为了防止内存泄漏，一定要判断还有没有子项，没有则通过v-if销毁 -->
    <Tree :list="item.children" v-if="item.children?.length" />
</div>
```



:herb:**可选操作符双向问号表达式使用**

我们在日常编程的时候，无论是后端（像是 java），还是前端（js）都经常会出现 ==NullPointerException==  或是==TypeError: Cannot read properties of undefined ...== 的错误

ts 中提供给我们一种非常的解决方案，那就是使用可选操作符，例如：

```ts
const a = {}	

console.log(a.children.length)	
// TypeError: Cannot read properties of undefined (reading 'length')

console.log(a.children?.length)	// undefined

console.log(a.children?.length?.a ?? [])	// []
// 当左边的值为 undefined 或 null 时，返回 ?? 右边的 []
```

> 双问号表达式对于 0 和 false 不做处理，会原样返回



## 2.4 动态组件

vue 中提供了 `<component :is="xxx"></component>` 的方式帮助我们实现了动态组件，示例代码如下：

```vue
<template>
    <div class="dynamic">
        <div class="tabs" :class="[activeIndex === index ? 'active' : '']" v-for="(item, index) in data" @click="switchTab(item, index)">
            <span>{{ item.name }}</span>
        </div>
    </div>
    <component :is="comId"></component>
</template>

<script lang="ts" setup>
import { ref, reactive, shallowRef, markRaw } from 'vue'
import A from './components/A.vue'
import B from './components/B.vue'
import C from './components/C.vue'
const comId = shallowRef(A)
const activeIndex = ref(0)

const data = reactive([
    { 
        name: 'A组件',
        com: markRaw(A)
    },
    { 
        name: 'B组件',
        com: markRaw(B)
    },
    { 
        name: 'C组件',
        com: markRaw(C)
    }
])

const switchTab = (item: any, index: number) => {
    comId.value = item.com
    activeIndex.value = index
}

</script>

<style scoped lang='scss'>
.dynamic {
    display: flex;
    .tabs {
        padding: 5px 10px;
        margin: 5px;
        border: 1px solid #ccc;
    }
    .active {
        background-color: skyblue;
    }
}
</style>
```

> shallowRef 是为了使其浅层次监听 A，给 data 中的 A、B、C 都用 markRaw 包括是为了告诉 vue 不需要给组件做响应式，两者都是为了节省性能

我们除了使用这种对象风格的写法，还可延续 vue2 的写法，使用字符串来实现动态路由切换，如下：

```ts
<script lang="ts">
export default {
    components: {
        A, 
        B,
        C
    }
}
</script>
```

```ts
// 组件的表达全部替换成字符串的形式
const comId = shallowRef('A')

const data = reactive([
    { 
        name: 'A组件',
        com: 'A'
    },
    { 
        name: 'B组件',
        com: 'B'
    },
    { 
        name: 'C组件',
        com: 'C'
    }
])
```



## 2.5 插槽全家桶

插槽：其实就是父组件在子组件标签内插入内容，插入的内容将被子组件接受并渲染（子组件使用 "占位符" `<slot></slot>`，和 sass 中的 `@content` 是一个道理）



### 2.5.1 匿名插槽

**父组件：准备给子组件的 "礼物"，使用 template 将其包裹**

```vue
<template>
<Child>
    <template v-slot>
        <div style="color:red;font-size:10px">
            <span>我是一个匿名插槽，我想要插入到子组件的下面</span>
        </div>
    </template>
</Child>
</template>

<script lang="ts" setup>
import Child from './components/Child.vue'
</script>
```

子组件：使用 `<slot></slot>` 接收即可

```vue
<template>
  <div>
    <h1>我是头部</h1>
    <slot></slot>
  </div>
</template>
```



这里要注意一件事：就是 "匿名插槽" 这个概念在官网并没有说明，这只是我们的说法，我们可以看[官网](https://cn.vuejs.org/api/built-in-directives.html#v-slot)对  `v-slot` 指令的描述

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230518201604984.png" alt="image-20230518201604984"  />

作为 "匿名插槽"，`v-slot` 并不能丢掉，"匿名" 仅仅只是只我们没有单独给其指定 name, vue 底层会给一个默认的插槽名 `default`（父组件的 `v-slot` 和子组件的 `slot` 都会赋 "deufault"）

> 同时，正如官网所说，`v-slot` 是由简写的，为 `#`，正如 `v-bind` 的简写是 `:` 或 `.` 和 `v-on` 简写为 `@`

### 2.5.2 具名插槽

这个就不过多介绍，即给插槽一个名字，用来指定将 "礼物" 插入到子组件固定位置

**父组件**

```vue
<template>
<Child>
    <template v-slot:bottom>
        <div style="color:red;font-size:10px">
            <span>我是一个bottom具名插槽，我想要插入到子组件的下面</span>
        </div>
    </template>
    <template v-slot:default>
        <div style="color:red;font-size:10px">
            我也是一个具名插槽，但我的名字是 default, 能不能把我插入到子组件上面？
        </div>
    </template>
</Child>
</template>
```

**子组件**

```vue
<template>
    <div class="header">
        <slot></slot>
        我是上面
    </div>
    <div class="body">我是中间</div>
    <div class="footer">
        我是下面
        <slot name="bottom"></slot>
    </div>
</template>
```



### 2.5.3 作用域插槽

作用域插槽有点 "相互作用" 的意味，父组件再给子组件 "礼物" 之前，子组件会进行 "回馈"，然后父组件加工处理后返回给子组件

**子组件**

```vue
<template>
    <slot name="gift" :data="data"></slot>
</template>

<script lang="ts" setup>
const data: number[] = [1, 2, 3]
</script>
```



**父组件**

```vue
<template>
<Child>
    <!-- 利用解构方式将 data 解构出来 -->
    <template #gift="{ data }"> 
        <div>
            这是我从子组件中拿到的数据：{{ data }}，现在我要拿它插入到子组件中
        </div>
    </template>
</Child>
</template>

<script lang="ts" setup>
import Child from './components/Child.vue'
</script>
```



### 2.5.4 动态插槽

【动态插槽】这个概念同样在官网是没有的，只是为了表示我们可以通过改变 name 的值动态改变插槽插入的位置，如下：

**父组件**

```vue
<template>
    <Child>
        <!-- <template :v-slot="slotName"> -->
        <template #[slotName]>
            <div>
                我想要插入的位置是：{{ slotName }}
            </div>
        </template>
    </Child>

    <button @click="changePos">改变插入位置</button>
</template>

<script lang="ts" setup>
import { ref } from 'vue'
import Child from './components/Child.vue'
const slotName = ref('top')

const changePos = () => {
    slotName.value = 'bottom'
}
</script>

<style scoped>

</style>
```



**子组件**

```vue
<template>
    <div class="header">
        <h1>我是上面</h1>
        <slot name="top" />
    </div>
    <div class="body">
        <h1>我是中间</h1>
        <slot name="middle" />
    </div>
    <div class="footer">
        <h1>我是下面</h1>
        <slot name="bottom" />
    </div>
</template>

<script lang="ts" setup>

</script>

<style scoped>

</style>
```



## 2.6 异步组件 && 代码分包 && suspense



## 2.7 Teleport 传送组件

Teleport 组件可以帮助我们将某个 DOM 挂载到其他位置

```html
<button @click="open = true">Open Modal</button>

<Teleport to="body" :disabled="isShow">
  <div v-if="open" class="modal">
    <p>Hello from the modal!</p>
    <button @click="open = false">Close</button>
  </div>
</Teleport>
```

> 该组件可以传入两个参数，第一个参数 `to` 后面跟随 CSS 选择器，表示传入到那个 DOM 身上；第二个参数 `disabled` 表示传送是否禁用



## 2.7 keepAlive 缓存组件

`KeepAlive` 用于在多个组件间动态切换时对组件进行缓存，不过一般前端页面的数据大部分都是通过后端拿到的，如果想要做 "伪持久化" 可以放到 webStorage 或 store 状态管理中，使用 keepAlive 的场景比较少，不过使用 KeepAlive 可以提高性能

示例代码：

**<u>App.vue</u>**

```vue
<template>
    <form>
       <label :for="item.name" v-for="(item, index) in list" :key="index" @click="handleClick(index)">
            <input type="radio" :value="item.value" name="demo" :checked="item.isChecked" :id="item.name">
            {{ item.name }}
        </label>
    </form>
    <div class="show-body">
        <component :is="componentId"></component>
    </div>
</template>

<script lang="ts" setup>
import { ref, reactive } from 'vue' 
import A from './components/A.vue'
import B from './components/B.vue'
const componentId = ref(A)
const list = reactive([
    {
       name: 'A',
       value: 0,
       isChecked: true,
       com: A 
    }, {
        name: 'B',
        value: 1,
        isChecked: false,
        com: B 
        
    }
])
const handleClick = (index: number) => {
    componentId.value = list[index].com
}

</script>

<style scoped lang="scss">
.show-body {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 200px;
    height: 250px;
    border: 1px solid #ccc;
}

</style>
```

<u>**A.vue**</u>

```vue
<template>
    <input type="text" v-model="data">
</template>

<script lang="ts" setup>
import { ref } from 'vue'
const data = ref('')

</script>

<style scoped>

</style>
```

<u>**B.vue**</u>

```vue
<template>
    <span style="margin-right: 10px">num: {{ num }} </span>
    <button @click="num++">+</button>
</template>

<script lang="ts" setup>
import { ref } from 'vue'
const num = ref(0)

</script>

<style scoped>

</style>
```

由于组件 A 和组件 B 的数据没有做持久化，因此，当组件被卸载时，数据会复原，而这对用户显然是不友好的，此时我们就可以用到 keepAlive 做缓存，如下：

```html
<keep-alive>
    <component :is="componentId"></component>
</keep-alive>
```

在 `keep-alive` 标签中可以传入三个 prop：

- `include`：指定缓存的组件

  ```html
  <!-- 以英文逗号分隔的字符串 -->
  <KeepAlive include="A,B">
    <component :is="view" />
  </KeepAlive>
  
  <!-- 正则表达式 (需使用 `v-bind`) -->
  <KeepAlive :include="/A|B/">
    <component :is="view" />
  </KeepAlive>
  
  <!-- 数组 (需使用 `v-bind`) -->
  <KeepAlive :include="['A', 'B']">
    <component :is="view" />
  </KeepAlive>
  ```

- `exclude`：与 `include` 是相反的操作，表示排除哪些组件不做缓存

- `max`：最大缓存实例数，采用的是 LRU 算法（操作系统中内存块的分配算法一种）

**缓存实例的生命周期**

- `onActivated()`：组价激活时的回调
- `onDeactivated()`：组件挂载时的回调

```ts
onActivated(() => {
  // 调用时机为首次挂载
  // 以及每次从缓存中被重新插入时
})

onDeactivated(() => {
  // 在从 DOM 上移除、进入缓存
  // 以及组件卸载时调用
})
```



## 2.8 Transition 动画组件

`Transition` 触发条件如下：

- 由 `v-if` 所触发的切换
- 由 `v-show` 所触发的切换
- 由特殊元素 `<component>` 切换的动态组件
- 改变特殊的 `key` 属性

Transition 中涉及的内容非常多，我们重点关注如下几点即可：

进入与离开的 CSS class

![过渡图示](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/transition-classes.f0f7b3c9.png)

以及使用 namespace 和 Animation.css



## 2.9 依赖注入 Provide / Inject

provide 和 inject 的应用场景：<strong style="color:red">组件间通信时，组件嵌套层次较深</strong>

![img](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/5f4ea9e16eda6e37ab336075a788ae15.png)

示例如下：

**<u>App.vue</u>**

```vue
<template>
    <div class="options">
        <label :for="item.name" v-for="(item, index) in optionsList" :key="index">
            <input type="radio" name="demo" v-model="colorVal" :value="item.value" :id="item.name">
            {{ item.name }}
        </label>
    </div>
    <hr>
    <Son></Son>
    <hr>
    <Grandson></Grandson>
</template>

<script lang="ts" setup>
import { ref, reactive } from 'vue'
import { provide } from 'vue'
import Son from './components/Son.vue'
import Grandson from './components/Grandson.vue'
const optionsList = reactive([
    { 
        value: 'red',
        name: 'red'
    }, { 
        value: 'blue',
        name: 'blue'
    }, {
        value: 'green',
        name: 'green'
    }
])

const colorVal = ref('red')
provide('color', colorVal)
</script>

<style scoped>
.options {
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 300px;

}
</style>
```



<u>**Son.vue**</u>

```vue
<template>
    <h1>我是子组件</h1>
    <div class="son"></div>
    <button @click="changeColor">从儿子这里修改inject得到的值</button>
</template>

<script lang="ts" setup>
import { inject } from 'vue'
import type { Ref } from 'vue'
const color = inject<Ref<string>>('color')

const changeColor = () => {
    color!.value = 'yellow'     // 这里在 color 后面添加的 ! 表示非空断言
}
</script>

<style scoped>
.son {
    width: 100px;
    height: 100px;
    background-color: v-bind(color);
}
</style>
```



<u>**Grandson.vue**</u>

```vue
<template>
<h1>我是孙子组件</h1>
<div class="grandson"></div>
</template>

<script lang="ts" setup>
import { inject } from 'vue'
import type { Ref }  from 'vue'
const color = inject<Ref<string>>('color')
</script>

<style scoped>
.grandson {
    width: 100px;
    height: 100px;
    background-color: v-bind(color);
}
</style>
```



**注意事项**

1. inject 和 provide 通常成对使用，无论组件之间的层级有多深，只要组件之间在一条组件链上都可以注入成功（底层用到了 `Object.create()`）
2. `provide` 只能在 `setup` 函数模式或者 `setup` 语法糖中使用，无法在 Options API 中使用



## 2.10 兄弟组件传参



## 2.11 Mitt

在 Vue3 中， Vue2 的 `$on`、`$off` 和 `$once` 实例方法已经被移除，组件实例不再实现事件触发接口，因此 EventBus 现在就无法使用，对于这种情况，我们可以使用 Mitt 库

**安装**

```bash
npm install mitt -S
```



**配置文件**

```ts
// main.ts
import { createApp } from 'vue'
import App from './App.vue'

import mitt from 'mitt'
const Mit = mitt()

// 拓展 ComponentCustomProperties 类型获得类型提示
declare module "vue" {
    export interface ComponentCustomProperties {
        $Bus: typeof Mit
    }
}
export const app = createApp(App)

// Vue3 挂载全局 API
app.config.globalProperties.$Bus = Mit
app.mount('#app')
```



**组件 A 发布消息(emit)**

```vue
<template>
    <h1>我是组件A</h1>
    <button  @click="sendMsg">发布</button>
</template>

<script lang="ts" setup>
import { getCurrentInstance } from 'vue'
const instance = getCurrentInstance()
const sendMsg = () => {
    instance?.proxy?.$Bus.emit('getMsg', 100)
}
</script>
```



**组件 B 订阅消息(on)**

```vue
<template>
    <h1>我是组件B</h1>
</template>

<script lang="ts" setup>
import { getCurrentInstance } from 'vue'
const instance =  getCurrentInstance()
instance?.proxy?.$Bus.on('getMsg', (data:any) => {
    console.log('从A组件中接收到的数据为: ', data)
})
</script>
```



**移除监听事件（off）**

```ts
const Fn = (num: any) => {
    console.log(num, '===========>B')
}
instance?.proxy?.$Bus.on('on-num',Fn)//listen
instance?.proxy?.$Bus.off('on-num',Fn)//unListen
```



**清空所有监听事件（clear）**

```ts
instance?.proxy?.$Bus.all.clear() 
```



## 2.12 tsx & vite 插件

我们之前是使用 Template 的方式去写模版，Vue3 可以扩展另一种 TSX 风格。在使用之前，我们需要安装一个插件

01 安装插件

```bash
npm install @vitejs/plugin-vue-jsx -D
```

> 如果报错，请尝试使用 pnpm 安装

02 在 vite.config.ts 中添加插件配置

```ts
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

import vueJsx from '@vitejs/plugin-vue-jsx'
// https://vitejs.dev/config/
export default defineConfig({
  plugins: [vue(),vueJsx()],
})
```

03 配置 tsconfig.json 

```json
{
  "compilerOptions": {
    "jsx": "preserve",
    "jsxFactory": "h",
    "jsxFragmentFactory": "Fragment",
  }
}
```

> 这里我没有配置好像也可以使用 tsx 语法风格



tsx 书写风格

```tsx
// 写法1：返回一个渲染函数
export default function() {
  return (<div>学习 Vue3</div>)
}
```

```tsx
// 写法2：option api
import { defineComponent } from "vue"
export default defineComponent({
  data() {
    return {
      age: 23,
    }
  },
  render() {
    return <div>{this.age}</div>
  }
})
```

```tsx
// 写法3：setup 函数模式
import { defineComponent } from "vue"
export default defineComponent({
  setup() {
    return () => (<div>学习vue3</div>)
  }
})
```

:warning:注意事项

1. tsx 风格的使用的是单花括号取值，而不是像 vue 中使用双花括号

2. tsx 不支持 ref 自动解包，需要我们手动 `.value`

   ```tsx
   import { defineComponent, ref } from 'vue'
   export default defineComponent({
     setup() {
       const name = ref('Alice')
       return () => <div>{name.value}</div>
     }
   })
   ```



### 2.12.1 v-model

tsx 支持 `v-model` 的使用

```tsx
import { defineComponent, ref, Ref } from 'vue'
export default defineComponent({
  setup() {
    const name:Ref<string> = ref<string>('Alice')
    const change = () => {
      name.value = 'Bob'
    }
    return () =>
      (
        <>
          <input v-model={name.value}></input><br />
          <button onClick={() => {change()}}>点击切换人名</button>
        </>
      )
  }
})
```



### 2.12.2 v-show

tsx 支持 `v-show` 的使用

```tsx
import { defineComponent, ref, Ref } from 'vue'
export default defineComponent({
  setup() {
    const flag:Ref<boolean> = ref<boolean>(true)
    return () =>
      (
        <>
          <h1 v-show={flag.value}>好好学习，天天向上</h1>
        </>
      )
  }
})
```



### 2.12.3 v-if

tsx 不支持 `v-if` 的使用，但是可以转换 `v-if`

```tsx
import { defineComponent, ref, Ref } from 'vue'
export default defineComponent({
  setup() {
    const flag:Ref<boolean> = ref<boolean>(true)
    return () =>
      (
        <>
          {
            flag.value ? <div>雪见</div> : <div>景天</div>
          }
        </>
      )
  }
})
```



### 2.13.4 v-for

tsx 语法不支持 `v-for`，我们可以使用 `arr.map()` 实现

```tsx
import { defineComponent, reactive } from 'vue'
export default defineComponent({
  setup() {
    const authors: string[] = reactive(['Alice', 'Bob', 'Mike', 'John'])
    const change = () => {
      authors.push('xxx')
    }
    return () =>
      (
        <>
          {
            authors.map(author => <div>{author}</div>)
          }
          <button onClick={() => {change()}}>Click me</button>
        </>
      )
  }
})
```



### 2.13.5 v-bind

tsx 中不支持 v-bind，但是可以直接赋值

```tsx
import { defineComponent, ref, Ref } from 'vue'
export default defineComponent({
  setup() {
    const serial: Ref<number> = ref(1)

    return () =>
      (
        <>
          {
            <div data-v={serial.value} class="test">卧龙凤雏</div>
          }
        </>
      )
  }
})
```



### 2.13.6 v-on 绑定事件

- tsx 语法中所有事件以 `on` 开头
- 所有事件名称首字母大写

例如：

- `@click` => `onClick`

- `@change` => `onChange`

- `@drop` => `onDrop`

  > `onDrop` 是拖动事件



### 2.13.7 props

<u>App.tsx</u>

```tsx
import { defineComponent } from 'vue'
import Child from './components/Child'
export default defineComponent({
  setup() {
    return () =>
      (
        <>
          <Child name={'张三'} />
        </>
      )
  }
})
```

<u>Child.tsx</u>

```tsx
import { defineComponent } from 'vue'
interface Props {
  name?: string
}
export default defineComponent({
  props: {
    name: String,
  },
  setup(props: Props) {
    return (() => (
        <>
          <div>{props.name}</div>
        </>
      )
    )
  },
})
```



### 2.13.8 emit 派发

<u>App.tsx</u>

```tsx
import { defineComponent, Ref, ref } from 'vue'
import Child from './components/Child'
export default defineComponent({
  setup() {
    const name: Ref<string> = ref('')
    const getMsg = (data: string) => {
      name.value = data
    }
    return () =>
      (
        <>
          <Child onSendMsg={() => {getMsg('张三')}} /><br />
          父组件从子组件中收到的信息为：{name.value}
        </>
      )
  }
})
```



<u>Child.tsx</u>

```tsx
import { defineComponent } from 'vue'

export default defineComponent({
  emits: ['sendMsg'],
  /**
   * setup 里面接收的两个参数
   * props 和 context { slots, emit, expose }
   */
  setup(_ , {emit}) {
    return (() => (
        <>
          <button onClick={() => { emit('sendMsg', '张三')}}>点击我发送消息到父组件</button>
        </>
      )
    )
  }
})
```



### 2.13.9 插槽

在 TSX 语法中不支持使用 `<template v-slot>` 和 `<slot>` 实现插槽的渲染与传递，需要我们自行实现



非响应式的+有状态的 插槽对象

- 在 `setup()` 函数的第二个参数也就是 `Setup 上下文对象` 中会暴露出一个**非响应式的插槽对象 slots**，我们可以从中获取到所有传递过来的插槽，如上图所示。
- `有状态` 是指 `slots` 对象总是会随着组件自身的更新而更新。这意味着你**应当避免解构它们**，并始终通过 `slots.x` 的形式使用其中的属性。
- `非响应式`则是指，如果你想要基于 `slots` 的改变来执行副作用，那么你应该在 `onBeforeUpdate` 生命周期钩子中编写相关逻辑。



01 直接使用 `v-slots` 指令进行渲染

我们需要在子组件中，在 setup 入参中解构出 slots ，然后通过 `slots.xxx` 的形式渲染插槽 

<u>App.tsx</u>

```tsx
import { defineComponent } from 'vue'
import Child from './components/Child'
export default defineComponent({
  setup() {
    const slot = {
      default: () => (<div>好好学习</div>),
      foo: () => (<div>天天向上</div>)
    }
    return () =>
      (
        <>
          <Child v-slots={slot}/>
        </>
      )
  }
})
```

<u>Child.tsx</u>

```tsx
import { defineComponent } from 'vue'

export default defineComponent({
  emits: ['sendMsg'],
  setup(_, {slots}) {
    return (() => (
        <>
        <h1>{slots.default ? slots.default() : '默认插槽'}</h1>
        <h1>{slots.foo ? slots.foo() : "自定义插槽"}</h1>
        </>
      )
    )
  }
})
```



02 沿用 template 模板的具名插槽原理

<u>App.tsx</u>

```tsx
import { defineComponent } from 'vue'
import Child from './components/Child'
export default defineComponent({
  setup() {
    return () =>
      (
        <>
          <Child>
            {{
              default: () => (<span>男</span>),
              foo: () => [<span>drama</span>, <span>opera</span>]
            }}
          </Child>
        </>
      )
  }
})
```



<u>Child.tsx</u>

```tsx
import { defineComponent } from 'vue'

export default defineComponent({
  emits: ['sendMsg'],
  setup(_, {slots}) {
    return (() => (
        <>
        <h1>{slots.default ? slots.default() : '默认插槽'}</h1>
        <h1>{slots.foo ? slots.foo() : "自定义插槽"}</h1>
        </>
      )
    )
  }
})
```



03 作用域插槽传参





# 3 高级篇

## 3.1 自动导入

我们之前每次导入 Vue 的 API 时都需要手动导入，现在我们可以使用 `unplugin-auto-import` 插件来实现 API 的自动导入

使用方法

01 安装

```bash
npm i -D unplugin-auto-import
```

> 当我们安装完成之后，会发现在项目的 src 目录下会出现一个 auto-import.d.ts 声明文件



02 在 vite.config.ts 中添加如下配置

```ts
import AutoImport from 'unplugin-auto-import/vite'

export default defineConfig({
  plugins: [
    AutoImport({
      imports: ['vue'],
      dts: 'src/auto-import.d.ts'
    })
  ]
})
```



03 编写测试用例

```vue
<template>
  <button @click="flag = !flag">切换显示与否</button>
  <h1 v-show="flag">Hello World!</h1>
</template>

<script setup lang="ts">
const flag = ref<boolean>(true)
</script>
```

> 此时，我们会发现，我们不需要手动引入 `ref` 就可以使用，说明配置成功



## 3.2 v-model

在 Vue3 中，v-model 是破坏式更新的，并且它其实是一个语法糖，通过 props 和 emit 组合而成

1. 默认值的改变
   - prop：value -> modelValue
   - 事件：input -> update:modelValue
   - 移除 `.sync` 修饰符
   - 新增 支持多个 v-model
   - 新增 支持自定义修饰符 modifiers
2. 

 

