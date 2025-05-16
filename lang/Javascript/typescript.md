# 1 基础

## 1.1 安装

**全局安装 `typescript`**

```bash
npm i -g typescript
```



**初始化一个 `ts` 工程**

```bash
tsc --init
```



**编译 `ts`(将 `ts` => `js`)**

```bash
tsc fileName.ts
```



**实时编译**

```bash
tsc -w index.ts
```



在 node.js 环境下执行 ts，请安装如下依赖

**全局安装 `ts-node`**

```bash
npm i -g ts-node
```



**安装 `@types/node` 模块**

```bash
npm i @types/node
```



> 官方靶场地址：https://www.typescriptlang.org/zh/play?#code/FAAhQ

## 1.2 TS 简介

TS（全称 "TypeScript"）是以 Javascript 为基础构建的语言，TS 相较于 JS 有如下特点：

1. TS 是 JS 的超集
2. TS 可以在任何支持 JS 平台中执行
3. TS 扩展了 JS 并添加了类型
4. TS 不能直接被 JS 解析器直接执行，需要先进行编译转换



## 1.3 类型声明

ts 中类型有如下几类：

| **类型** | **例子**          | **描述**                       |
| -------- | ----------------- | ------------------------------ |
| number   | 1, -33, 2.5       | 任意数字                       |
| string   | 'hi', "hi", `hi`  | 任意字符串                     |
| boolean  | true、false       | 布尔值true或false              |
| 字面量   | 其本身            | 限制变量的值就是该字面量的值   |
| any      | *                 | 任意类型                       |
| unknown  | *                 | 类型安全的any                  |
| void     | 空值（undefined） | 没有值（或undefined）          |
| never    | 没有值            | 不能是任何值                   |
| object   | {name:'孙悟空'}   | 任意的JS对象                   |
| array    | [1,2,3]           | 任意JS数组                     |
| tuple    | [4,5]             | 元素，TS新增类型，固定长度数组 |
| enum     | enum{A, B}        | 枚举，TS中新增类型             |

> 其中内置的数据类型一共有八种：number、string、boolean、null、undefined、object、symbol、bigint



**字面量声明**

```typescript
/* 
 * 直接使用字面量进行类型声明
 */
// 使用场景1：限制 a 的值只能为字面量 10（不常用）
let a: 10   
a = 10 

// 使用场景2：联合字面量（有点类似于枚举类型）
let b: 'male' | 'female'
b = 'male'
b = 'female'

// 使用场景3：联合字面量（可选数据类型）
let c: boolean | string
c = 'hello'
c = true
```



**any 和 unknown 类型**

`any` 表示任意类型，一个变量设置类型为 `any` 后（显式设置 `any`）相当于**对该变量关闭了 TS 的类型检测****，当我们声明变量如果不指定类型，那么 TS 默认会将该变量解析为 `any` 类型（隐式设置）

```typescript
let a: any
a = 10
a = "ts"
```

但是我们不推荐使用 `any` 类型，因为这即与 JS 并无分别了，并且 `any` 类型存在一个极大的隐患，就是会将其他的变量 "污染"，例如：

```typescript
let a: any
a = 10
let b: boolean
b = a
console.log(typeof b)   // 此时 b 成为了 number 类型
```

取而代之，我们可以如果在不知道某个变量具体的类型时，可以使用 "安全" 的 `unknown` 类型，该类型不会存在变量污染的问题

```typescript
let a: unknown
a = 'hello'
let b: boolean
b = a   // 非法操作
```

> 换言之，`unkonwn` 只能作为父类型，而不能作为子类型，而 `any` 既可以作为父类型，又可以作为子类型

除此之外，`unknown` 的限制比较严格，当我们使用一个 `unknown` 类型的变量去指向一个对象时，是无法获取内部的属性的，如下：

```ts
let obj: unknown = {a: 1, b: ():number => 110}
obj.a   // 非法操作
obj.b() // 非法操作
```





**unknown 配合断言**

JS 中的断言用于<strong style="color:red">告诉解析器变量的实际类型</strong>

语法如下：

```typescript
// 语法1
变量 as 类型

// 语法2
<类型>变量
```

> 这里感觉有点像泛型，与 Java 中 assert 有所不同

如果我们真的希望 `unknown` 类型能够赋给其他类型，那么我们可以使用断言

```typescript
let a: unknown
a = 'hello'
let b: string
let c: string

b = a as string
c = <string>a
```



**void 类型**

`void` 常见于表示函数的返回值为空，当然也可以给一个变量声明为 `void` ，但是一旦我们给某变量声明了 `void` 类型，那么我们就只能为其赋予 `undefined` 

```typescript
// 函数的返回类型为 void
function fn(a:number, b:number): void {
    console.log(a, b)
}

// 为变量声明void类型，赋值 undefined 
let a: void
a = undefined
```



**never 类型**

`never` 类型表示的是<strong style="color:red">永远不存在的值的类型</strong>

而值永远不存在有两种情况：

- 函数执行时抛出异常，无法到达 `return`（即使没有 `return` 语句）
- 函数陷入死循环

```typescript
// 抛出异常
function fn1(): never {
  throw new Error("执行错误")
}

// 死循环
function fn2(): never {
  while (true) {}
}
```

除此之外，`never` 类型只能赋给 `never` 类型，不能赋给其他类型，同时，像 `null` 和 `undefined` 也不能赋给 `never`



**object 类型**

由于在 JS/TS 中，一切皆对象，因此我们为一个变量类型声明为 `object` 是没有任何意义的，相当于没有给该变量做语法检查

```typescript
let a: object
a = {}
a = function() {}
```



我们更加常用的搭配（或写法）是如下写法：

```typescript
// 表示指定 a 为一个对象，并且必须且只能包含 name 属性，类型为 string
let a: { name: string }
a = { name: "zs" }

// 如果我们在属性名后面添加 "?"，表示该属性是可选的
let b: { name: string, age?: number }
b = { name: 'ls', age: 18}

// 添加可变参数, 其中 propName 名字可自定义，后面分别表示属性类型和属性值类型,unknown 就代表了属性值可以为任意类型
let c: { name: string, [propName: string]: unknown }
c = { name: 'zs', age: 18, gender: '男'}
```



**进阶：限制函数的类型声明**

```typescript
// 使用 Function 定义函数的类型(不建议)
let a: Function

// 使用箭头函数来限制函数的定义
let b: (a: number, b: number) => number // 表示函数 b 中应有两个参数，并且均为 number 类型，返回值也为 number 类型

b = function(a: number) {
    return a
}

b = function(a: number, b: number) {
    return a + b
}

b = function(a: number, b: number, c: number) { // error
    return a + b + c;
}
```



**array 类型**

```typescript
// 声明一个 number 类型的数组
let a: number[]

// 声明一个 string 类型的数组
let b: string[]

// 数组泛型，规则是 Array<类型>
let c: Array<number>
```



**tuple 类型**

`tuple` 是固定长度的数组，属于 TS 的扩展类型

```typescript
// 声明一个 tuple,里面有且只能包含两个 string 类型的元素
let h: [string, string]
h = ['hello', 'world']
```



**enum 类型**

`enum` 也是 TS 的一个扩展类型，和其他语言的 `enum` 所表示的含义相同，用于表示有限集合

```typescript
enum Gender {
    Male,
    Female
}

let person: { name: string, gender: Gender }
person = {
    name: 'Alice',
    gender: Gender.Male
}

console.log(person.gender === Gender.MALE)
```



## 1.4 联合类型 && 交叉类型

**联合类型**

我们在之前已经提到到联合类型，该类型表示<strong style="color:red">取值可以为众多类型的一种</strong>，使用 `|` 分隔

```typescript
// 基本使用
let myInfo: string | number
myInfo = '张三'
myInfo = 18

// 常与 undefined 结合使用
let lng: number | undefined = 12.89

// 限制字面量的选择范围
let season: 'spring' | 'summer' | 'autumn' | 'winter'
season = 'spring' 
```



**交叉类型**

交叉类型刚好和联合类型相反，交叉类型是将多个类型合并为一个类型，使用 `&` 定义，当然交叉类型并不适用于基本数据类型，如下：

```typescript
let i: string & number  // 错误使用
```

> 不可能存在一个变量既是 `string` 类型又是 `number` 类型

正确的使用方法如下：

```typescript
let o: { name: string } & { age: number }
o = { name: 'zs', age: 18}
```



## 1.5 type 关键字

`type` 关键字用于给类型起别名，如下：

```typescript
type MyString = string
let s: MyString = 'zhangsan'
```

> 注：我们通过 `type` 仅仅只是为变量类型起别名，而不是真正地创建了一个新的类型，这点我们可以通过 `typeof` 进行验证



**应用场景（其实编程领域的很多应用场景都源于程序员的 "懒"）**

```typescript
// 当自定义的变量类型特别长时，我们可以为其起别名
type myCustomType = 1 | 2 | 3 | 4 | 5
let m: myCustomType
let n: myCustomType
```



## 1.6 null、undefined && number、bigint

`null` 和 `undefined` 都代表空，但区别在于

-  `null` 是有指向的，对应的地址为 ==0x00000000==，该地址（内存空间）并不指向任何内容，换言之，你有一个垃圾桶，但垃圾桶是空的；
- `undefined` 没有指向，它表示变量没有被初始化，换言之，你连垃圾桶都没有

`null` 和 `undefined` 本身都是数据类型，有人可能发现通过 `typeof null` 得到的明明是一个 `object`，但这其实是一个 "美丽的误会"，是设计者当初由于快速开发产生的 bug 诞生的，具体原因可以自查。

```typescript
typeof null // object
typeof undefined // undefined
```

**严格判断和非严格判断**

```typescript
null == undefined      // true
null === undefined     // false
!!null === !!undefined // true
```

> 第三种双 `!` 代表将其转换为布尔值，等价于 `Boolean(null) === Boolean(undefined)`

当然，`null` 和 `undefined` 类型是不能相互赋值的，但是它们都是所有类型的子类型，我们在关闭了 strictNullChecks 模式后是可以直接赋值给其他类型的，如下：

```ts
let a: number = 1
let b: undefined = undefined
a = b
```



**`bigint` 和 `number`**

`bigint` 这种数据类型所对应的是内置对象 `BigInt` ，在介绍之前请注意以下几点：

1. `BigInt` != `Number`，两者还是具有比较大的区别的，`BigInt` 解决了 `Number` 整数溢出问题，<u>但是不能使用 `Math` 内置对象进行运行，也不能和 `Number` 混合运算</u>，但是两者可以强转（注意精度丢失问题）
2. `BigInt` 支持基本运算，如 `+`、`-`、`*`、`/`、`%`、`**` 和除 `>>>` 外的位运算，因为 `BigInt` 是有符号的，并且不支持单目运算符
3. `BigInt` 结果带小数的运算会被取整
4. `BigInt` 和 `Number` 严格不相等（很显然，两者类型都不同），但宽松相等

**定义 `BigInt`**

```typescript
// 定义方式1：末尾捎带 n
const m = 1n

// 定义方式2：使用 BigInt() 构造器
const n: bigint = BigInt(1)
```



## 1.7 symbol 

`symbol` 是一个非常有意思的数据类型，它是的类型正如其名 ，代表一个 "标识"，同时它是一个基本数据类型（primitive data type）。`symbol` 类型的值通过 `Symbol()` 得到，无法通过 `new` 创建而得。

`symbol` 的应用场景：用于解决**变量同名覆盖**问题

```typescript
// 定义方式1：通过 Symbol 定义，括号内为描述信息（可不写，但推荐写，方便后期调试使用）
let a: symbol = Symbol('变量a')
let b: symbol = Symbol('变量b')
console.log(a, b)

// 定义方式2：通过 Symbol.for
let c: symbol = Symbol.for('symbol变量')
let d: symbol = Symbol.for('symbol变量')
console.log(c === d)        // true
```

> 上述两者使用有差别，`Symbol()` 每次会返回一个新的 symbol，而 `Symbol.for()` 每次会根据 for 中的描述信息放在一个全局 symbol 注册表，key 为 description，value 为 symbol 值，如果 description 相同则直接返回从注册表中返回

我们取出上述 symbol 的描述的方法有如下方式，对于全局注册，可以使用 `.description` 或 `Symbol.for(key)` 的方式获取；对于局部注册，只能使用 `.description` 的方式获取

```typescript
console.log(a.description)      // 变量a
console.log(c.description)      // symbol变量

console.log(Symbol.for(a))      // error
console.log(Symbol.keyFor(c))   // symbol变量
```



**应用场景：字符串解耦**

我们日常生活中，经常会出现这么一种情况，一个班级可能出现同名的情况的，那么我们在登分的时候就会出现困扰（当然，我们在管理系统中肯定要用学号作为 key 以示区分），解决方式如下：

```js
const stu_1 = {
    key: Symbol(),
    name: '张三'
}

const stu_2 = {
    key: Symbol(),
    name: '张三'
}

const grade = {
    [stu_1.key]: {css: 99, js: 100},
    [stu_2.key]: {css: 20, js: 30}
}

console.log(grade[stu_1.key])
```



## 1.8 包装类型和原始类型

这个概念不仅存在于 Java，在 JS/TS 中同样也有，例如：

| 原始类型 | 包装类型 |
| -------- | -------- |
| number   | Number   |
| string   | String   |
| boolean  | Boolean  |
| symbol   | Symbol   |

**示例**

```typescript
const a: number = 1
const b: Number = a // 装箱操作
const c: number = b.valueOf() // 拆箱操作
```



## 1.9 断言

TS 无法动态进行类型检测，所有的检查都是在编译阶段进行的，因此会出现有时候自己清楚使用的变量的类型，但是编译器不清楚的情况，在这时就需要用到==断言==，==断言==类似于告诉编译器 "相信自己就是这个类型"，应用如下：

```typescript
// as 断言
const arr: number[] = [1, 2, 3, 4]
const greater2: number = arr.find(v => v > 2) as number

// 尖括号写法
const str: unknown = 'Hello World'
console.log((<string>str).length)
```

> 类型断言只能够 "欺骗" TS 编译器，而无法避免运行时错误

### 1.9.1 非空断言

当编译器根据上下文无法判断变量的类型，我们可以使用后缀操作符 `!` 表示该变量不是 `null` 或 `undefined` 类型

```typescript
let str: null | undefined | string = 'abc'
console.log(str!.toString())
```

```typescript
let x: number
init()
console.log(x!) // 告诉编译器 x 会被明确赋值
function init() {
    x = 10
}
```



### 1.9.2 any 临时断言

由于 `any` 类型的变量上可以访问任何属性，所以我们可以使用 `any` 断言使编译器认为该对象上有该属性

```ts
const obj = {
    a: 1,
    b: 2
};
obj.c   // 报错
(obj as any).c  // 通过
```



### 1.9.3 as const 断言

`as const` 断言是对字面量的断言，与 `const` 声明块级变量是有去区别的，如下：

- 如果是基本类型，那么两者无区别

  ```ts
  const myName: string = 'zs'
  myName = 'ls'   // 无法修改
  
  
  let myName = 'zs' as const
  myName = 'ls'   // 无法修改
  ```

- 如果是引用类型，那么 `as const` 由于断言的是字面量，所以是无法修改的

  ```ts
  const obj = {
      a: 1,
      b: 2
  }
  
  obj.a = 1   // 可以修改，因为 obj 是引用类型，只要不是修改其引用地址即可
  
  let obj = {
      a: 1,
      b: 2
  } as const
  
  obj.a = 1   // 无法修改， as const 修改字面量 { a: 1, b: 2}
  ```





## 1.10 函数相关

### 1.10.1 函数定义

```typescript
/**
 * @param a number 类型
 * @param b number 类型
 * @returns number 类型
 */
function add(a: number, b: number): number {
    return a + b
}
```





### 1.10.2 函数表达式

```typescript
let sub = (a:number, b: number): number => a - b
```



### 1.10.3 可选参数

```typescript
function buildName(firstName: string, lastName?: string): string {
    if(lastName) {
        return lastName + '-' + firstName
    } else {
        return firstName
    }
}
```



### 1.10.4 剩余参数

```typescript
function flat(arr: any[], ...items: any[]) {
    items.forEach(v => arr.push(v))
    return arr
}

const a = [1, 2, 3]
const res = flat(a, 'js', 'ts')
console.log(res)    // [ 1, 2, 3, 'js', 'ts' ]
console.log(a)      // [ 1, 2, 3, 'js', 'ts' ]
```



### 1.10.5 参数默认值

```typescript
function concat(a: string, b: string = 'cat'): string {
    return a + b
}
```



### 1.10.6 函数重载

```typescript
type myType = string | number | undefined
function plus(a: myType, b: myType): myType {
    if(typeof a === 'number' && typeof b === 'number') return a + b
    if(typeof a === 'string' && typeof b === 'string') return a + b
}

console.log(plus(1, 2))     // 3
console.log(plus('a', 'b')) // ab
```



### 1.10.7 接口定义函数

```ts
// 使用接口定义函数
interface Add {
  (param1: number, param2: number): number
}

const add: Add = (a: number, b: number) => {
  return a + b
}

console.log(add(1, 2))
```





## 1.11 元组相关

元组和数组相似，但是不同的是：

1. 元组有固定的长度，而 JS 中的数组可以动态扩展
2. 元组中可以存放不同的数据类型，而数组中元素的类型相同

```typescript
let myTuple: [number, string] = [1, '张三']
myTuple = [2, '李四']
```



### 1.11.1 元组解构

```typescript
let stu: [number, string] = [1, '张三']
const [id, username] = stu
console.log(`id: ${id}`)
console.log(`username: ${username}`)
```



### 1.11.2 可选元组

```typescript
let person: [number, string, string?]
person = [1, 'Alice']
person = [2, 'Bob', 'male']
```



### 1.11.3 剩余元组

```typescript
let person: [number, ...string[]]
```



### 1.11.4 只读元组

```typescript
const point: readonly [number, number] = [10, 20]
```



### 1.11.5 元组应用场景

示例，可以用于导出 excel

```ts
let excel: [string, string, number, boolean][] = [
    ['张三', '唱', 18, true],
    ['李四', '跳', 19, false],
    ['王五', 'rap', 20, true],
    ['赵六', '篮球', 21, false]
]
```











# 2 TS 编译选项

## 2.1 编译选项

**自动编译文件**：使用 `tsc -w` 命令，TS 编译器会自动监视文件的变化

**自动编译整个项目**

如果我们想要在直接通过一个命令 `tsc` 或 `tsc -w` 编译项目中的所有文件，需要在项目根目录下创建 tsconfig.json 



## 2.2 配置选项

**重要配置项**

- files - 设置要编译的文件的名称；
- include - 设置需要进行编译的文件，支持路径模式匹配；
- exclude - 设置无需进行编译的文件，支持路径模式匹配；
- compilerOptions - 设置与编译流程相关的选项



**compilerOptions 中相关配置**

```json
{
  "compilerOptions": {
  
    /* 基本选项 */
    "target": "es5",                       // 指定 ECMAScript 目标版本: 'ES3' (default), 'ES5', 'ES6'/'ES2015', 'ES2016', 'ES2017', or 'ESNEXT'
    "module": "commonjs",                  // 指定使用模块: 'commonjs', 'amd', 'system', 'umd' or 'es2015'
    "lib": [],                             // 指定要包含在编译中的库文件
    "allowJs": true,                       // 允许编译 javascript 文件
    "checkJs": true,                       // 报告 javascript 文件中的错误
    "jsx": "preserve",                     // 指定 jsx 代码的生成: 'preserve', 'react-native', or 'react'
    "declaration": true,                   // 生成相应的 '.d.ts' 文件
    "sourceMap": true,                     // 生成相应的 '.map' 文件
    "outFile": "./",                       // 将输出文件合并为一个文件
    "outDir": "./",                        // 指定输出目录
    "rootDir": "./",                       // 用来控制输出目录结构 --outDir.
    "removeComments": true,                // 删除编译后的所有的注释
    "noEmit": true,                        // 不生成输出文件
    "importHelpers": true,                 // 从 tslib 导入辅助工具函数
    "isolatedModules": true,               // 将每个文件做为单独的模块 （与 'ts.transpileModule' 类似）.

    /* 严格的类型检查选项 */
    "strict": true,                        // 启用所有严格类型检查选项
    "noImplicitAny": true,                 // 在表达式和声明上有隐含的 any 类型时报错
    "strictNullChecks": true,              // 启用严格的 null 检查
    "noImplicitThis": true,                // 当 this 表达式值为 any 类型的时候，生成一个错误
    "alwaysStrict": true,                  // 以严格模式检查每个模块，并在每个文件里加入 'use strict'

    /* 额外的检查 */
    "noUnusedLocals": true,                // 有未使用的变量时，抛出错误
    "noUnusedParameters": true,            // 有未使用的参数时，抛出错误
    "noImplicitReturns": true,             // 并不是所有函数里的代码都有返回值时，抛出错误
    "noFallthroughCasesInSwitch": true,    // 报告 switch 语句的 fallthrough 错误。（即，不允许 switch 的 case 语句贯穿）

    /* 模块解析选项 */
    "moduleResolution": "node",            // 选择模块解析策略： 'node' (Node.js) or 'classic' (TypeScript pre-1.6)
    "baseUrl": "./",                       // 用于解析非相对模块名称的基目录
    "paths": {},                           // 模块名到基于 baseUrl 的路径映射的列表
    "rootDirs": [],                        // 根文件夹列表，其组合内容表示项目运行时的结构内容
    "typeRoots": [],                       // 包含类型声明的文件列表
    "types": [],                           // 需要包含的类型声明文件名列表
    "allowSyntheticDefaultImports": true,  // 允许从没有设置默认导出的模块中默认导入。

    /* Source Map Options */
    "sourceRoot": "./",                    // 指定调试器应该找到 TypeScript 文件而不是源文件的位置
    "mapRoot": "./",                       // 指定调试器应该找到映射文件而不是生成文件的位置
    "inlineSourceMap": true,               // 生成单个 soucemaps 文件，而不是将 sourcemaps 生成不同的文件
    "inlineSources": true,                 // 将代码与 sourcemaps 生成到一个文件中，要求同时设置了 --inlineSourceMap 或 --sourceMap 属性

    /* 其他选项 */
    "experimentalDecorators": true,        // 启用装饰器
    "emitDecoratorMetadata": true          // 为装饰器提供元数据的支持
  }
}
```



# 3 使用 webpack 打包 ts 代码

## 3.1 环境搭建

如果我们想要实现将我们的 ts 项目使用 webpack，那么就必须要使用 `npm init -y` 进行项目初始化

安装必要的依赖：

```bash
npm i -D webpack webpack-cli typescript ts-loader
```

通过 `tsc --init` 快速生成 tsconfig.json 配置文件

同时在项目根目录下新建一个 webpack.config.js 文件



# 4 面向对象（重要）

## 4.1 类基础

**类的定义**

```typescript
class Person {
  private name: string
  private age: number

  constructor(name: string, age: number) {
    this.name = name
    this.age = age
  }

  public getName(): string {
    return this.name
  }

  public getAge(): number {
    return this.age
  }

  public setName(name: string): void {
    this.name = name
  }

  public setAge(age: number): void {
    this.age = age
  }

  static sayHello() {
    console.log('Hello, every!!!')
  }
}
```

>默认可见性为 `public`



**类的使用**

```typescript
const p = new Person("张三", 18)
Person.sayHello()
p.getName() // 张三
```



## 4.2 构造器

在类中使用 `constructor` 关键字定义一个构造器方法

> 特别注意：在 JS/TS 中，一个类仅能有一个构造器方法

```typescript
class Cat {
  name: string
  age: number

  constructor(name: string, age: number) {
    this.name = name
    this.age = age
  }
}
```

上述代码等同于下面的代码：

```typescript
class Cat {
  constructor(public name: string, public age: number) {}
}
```

我们将上述代码与相同功能的 js 代码做对比，如下：

```js
class Cat {
    constructor(name, age) {
        this.name = name
        this.age = age
    }
}
```

> 相较之下，js 代码就十分简洁类，我们不需要在类单独声明成员变量









## 4.3 封装

所谓 "封装"，实质上就是将属性和方法都封装到一个对象容器中以供使用

默认情况下，对象中成员属性和成员方法的可见性修饰符都是 `public`，也就是说可以直接进行修改的，成员的修饰符有如下几类：

1. **`static`**：使用 **`static`** 修饰的属性或方法在挂载在类中，当类被加载完成后静态成员也就随之被加载，使用方法：**`类名.属性`** 或 **`类名.方法`**
2. **`readonly`**：修改成员属性，使用该修饰符后，成员属性无法被修改
3. 可见性修饰符：
   - `public(default)`：在类的内部和外部都可以访问
   - `protected`：在类的内部和继承子类的内部可以访问
   - `private`：只在本类中可以访问



## 4.4 继承

```typescript
class Animal {
    name: string
    age: number

    constructor(name: string, age: number) {
        this.name = name
        this.age = age
    }
}

class Dog extends Animal {
    /**
     * 子类继承父类
     *  1. 如果子类没有构造器，那么创建子类对象时调用的父类的构造器
     *  2. 如果子类中有构造器，且父类中不存在无参构造器，那么子类中必须通过super显式调用父类构造器
     */
    skill: string
    constructor(name: string, age: number, skill: string) {
        super(name, age)
        this.skill = skill
    }
}
```



## 4.5 多态

多态，涉及动态绑定，即父类引用指向子类，当调用子类时，表现子类的特性

```typescript
interface Person {
  name: string
  say(): void
}

class Student implements Person {
  constructor(public name: string) {}

  say() {
    console.log('Hello, I am a student')
  }
}

class Teacher implements Person {
  constructor(public name: string) {}

  say() {
    console.log('Hello, I am a teacher')
  }
}

let p: Person = new Student('Alice') 
p.say()
p = new Teacher('Bob')
p.say()
```



## 4.6 重写

重写：即在继承过程中，子类覆盖父类的方法



## 4.7 抽象类

抽象类是专门用来被其他类所继承的，抽象类没有实例，使用关键字 `abstract` 定义

和 Java 类似，<strong style="color:red">拥有抽象方法的类一定是抽象类，但抽象类中不一定有抽象方法</strong>

```typescript
abstract class Animal {
    abstract say(): void

    hunt() {
        console.log('hunt food');
    }
}
```



## 4.8 接口（interface）

接口是**对单继承机制的扩展，主要用来扩展类的行为或者提供一个规范**

```typescript
interface Person {
  name: string
  say(): void
}

class student implements Person {
  constructor(public name: string) {}

  say() {
    console.log('Hello')
  }
}
```



使用接口约束时，属性不能多也不能少，如下：

```ts
interface Person {
    name: string,
    age: number
}

const person: Person = {
    name: 'zs',
    age: 18
}
```



重名的 interface，会进行合并，如果重名的 interface 中存在字段冲突，会直接报错

```ts
interface Person { name: string }
interface Person { age: number }

let a: Person = { 
    name: 'Alice',
    age: 18
}
```



**使用 interface 搭配可选操作符 `?`，使得该属性可选**，

```typescript
interface Person {
    name: string,
    age?: number
}

const person_1: Person = {
    name: 'zs'
}

const person_2: Person = {
    name: 'ls',
    age: 20
}
```



**任意属性 `[propName: string]`**

```ts
interface Person {
    name: string,
    age?: number,
    [propName: string]: any
}

const person_1: Person = {
    name: 'Alice',
    age: 21,
    gender: 'female'
}
```

> 一旦使用了任意属性，那么确定属性和可选属性的类型都必须是其子集



**interface 搭配只读修饰符 `readonly`**

```ts
interface Person {
    name: string,
    age?: number,
    readonly gender?: number,
    [propName: string]: any
}

const Bob: Person = {
    name: 'Bob',
    age: 20,
    gender: 1
}

Bob.gender = 2  // gender 是只读属性，修改属于非法操作
```









# 5 泛型（Generic）

泛型的出现是为了制造一个模板类或者一个模板函数

```typescript
// 泛型函数示例
function add<T>(a: T, b: T): T | undefined {
    if(typeof a === 'string' && typeof b === 'string') return a + b as T
    else if (typeof a === 'number' && typeof b === 'number') return a + b as T
    else return undefined
}

console.log(add<number>(1, 2))
console.log(add<string>('1', '2'))
```

```typescript
// 泛型类示例
class Person<T, K> {
    name: T
    age: K
    
    constructor(name: T, age: K) {
        this.name = name
        this.age = age
    }
}

let p: Person<string, number> = new Person('zs', 19)
```



**泛型约束**

泛型约束即限制具有某一属性的类型键入，如下：

```ts
interface Len {
    length: number
}

// T extends Len 限制输入的类型必须要具有 length 属性，且 length 类型为 number
function getLength<T extends Len>(arr: T): number {
    return arr.length   
}
```













# 6 补充

## 6.1 数组补充

数组的表示方式除了前面的 `类型[]` 和 `Array<类型>` 的方式，我们还可以使用接口表示数组

```ts
interface NumberArray {
    [index: number]: number // 当索引类型为数字时，值的类型也必须是数字
}

const arr: NumberArray = [1, 2, 3]

interface StringArray {
    [index: string]: string // 当索引类型为字符串时，值的类型也必须是字符串
}

const arr1: StringArray = {
    name: 'foo',
    gender: 'male'
}
```

> 其实上述的 `[xxx:xxx]` 在 ts 中有一个专门的术语叫做 "索引签名"，通过索引签名限制索引的类型，同时索引签名类型必须是 string 、number 或者 symbols 



**多维数组表示**

```ts
let data: number[][] = [[1, 2], [3, 4]]
```



**arguments 类数组**

我们在接收 arguments 数组时得用 ts 内置对象 `IArguments` 定义，其底层类型定义为

```ts
interface IArguments {
    [index: number]: any
    length: number
    callee: Function
}
```

示例

```ts
function fn(...args: any[]): void {
  let arr: IArguments = arguments
  console.log(arr)
  
  console.log(args[0])
  console.log(args[1])
  console.log(args[2])

}

fn(1, 2, 3)
```



## 6.2 内置对象

**ES 内置对象**

Boolean、Number、String、RegExp、Date、Error

```ts
let a: Boolean = new Boolean(1)
let b: Number = new Number(1)
let c: String = new String(1)
let d: Date = new Date()
let e: RegExp = /^a/
let f: Error = new Error('error')
```



**DOM 和 BOM 内置对象**

元素映射表如下：

```ts
//dom元素的映射表
interface HTMLElementTagNameMap {
    "a": HTMLAnchorElement;
    "abbr": HTMLElement;
    "address": HTMLElement;
    "applet": HTMLAppletElement;
    "area": HTMLAreaElement;
    "article": HTMLElement;
    "aside": HTMLElement;
    "audio": HTMLAudioElement;
    "b": HTMLElement;
    "base": HTMLBaseElement;
    "bdi": HTMLElement;
    "bdo": HTMLElement;
    "blockquote": HTMLQuoteElement;
    "body": HTMLBodyElement;
    "br": HTMLBRElement;
    "button": HTMLButtonElement;
    "canvas": HTMLCanvasElement;
    "caption": HTMLTableCaptionElement;
    "cite": HTMLElement;
    "code": HTMLElement;
    "col": HTMLTableColElement;
    "colgroup": HTMLTableColElement;
    "data": HTMLDataElement;
    "datalist": HTMLDataListElement;
    "dd": HTMLElement;
    "del": HTMLModElement;
    "details": HTMLDetailsElement;
    "dfn": HTMLElement;
    "dialog": HTMLDialogElement;
    "dir": HTMLDirectoryElement;
    "div": HTMLDivElement;
    "dl": HTMLDListElement;
    "dt": HTMLElement;
    "em": HTMLElement;
    "embed": HTMLEmbedElement;
    "fieldset": HTMLFieldSetElement;
    "figcaption": HTMLElement;
    "figure": HTMLElement;
    "font": HTMLFontElement;
    "footer": HTMLElement;
    "form": HTMLFormElement;
    "frame": HTMLFrameElement;
    "frameset": HTMLFrameSetElement;
    "h1": HTMLHeadingElement;
    "h2": HTMLHeadingElement;
    "h3": HTMLHeadingElement;
    "h4": HTMLHeadingElement;
    "h5": HTMLHeadingElement;
    "h6": HTMLHeadingElement;
    "head": HTMLHeadElement;
    "header": HTMLElement;
    "hgroup": HTMLElement;
    "hr": HTMLHRElement;
    "html": HTMLHtmlElement;
    "i": HTMLElement;
    "iframe": HTMLIFrameElement;
    "img": HTMLImageElement;
    "input": HTMLInputElement;
    "ins": HTMLModElement;
    "kbd": HTMLElement;
    "label": HTMLLabelElement;
    "legend": HTMLLegendElement;
    "li": HTMLLIElement;
    "link": HTMLLinkElement;
    "main": HTMLElement;
    "map": HTMLMapElement;
    "mark": HTMLElement;
    "marquee": HTMLMarqueeElement;
    "menu": HTMLMenuElement;
    "meta": HTMLMetaElement;
    "meter": HTMLMeterElement;
    "nav": HTMLElement;
    "noscript": HTMLElement;
    "object": HTMLObjectElement;
    "ol": HTMLOListElement;
    "optgroup": HTMLOptGroupElement;
    "option": HTMLOptionElement;
    "output": HTMLOutputElement;
    "p": HTMLParagraphElement;
    "param": HTMLParamElement;
    "picture": HTMLPictureElement;
    "pre": HTMLPreElement;
    "progress": HTMLProgressElement;
    "q": HTMLQuoteElement;
    "rp": HTMLElement;
    "rt": HTMLElement;
    "ruby": HTMLElement;
    "s": HTMLElement;
    "samp": HTMLElement;
    "script": HTMLScriptElement;
    "section": HTMLElement;
    "select": HTMLSelectElement;
    "slot": HTMLSlotElement;
    "small": HTMLElement;
    "source": HTMLSourceElement;
    "span": HTMLSpanElement;
    "strong": HTMLElement;
    "style": HTMLStyleElement;
    "sub": HTMLElement;
    "summary": HTMLElement;
    "sup": HTMLElement;
    "table": HTMLTableElement;
    "tbody": HTMLTableSectionElement;
    "td": HTMLTableDataCellElement;
    "template": HTMLTemplateElement;
    "textarea": HTMLTextAreaElement;
    "tfoot": HTMLTableSectionElement;
    "th": HTMLTableHeaderCellElement;
    "thead": HTMLTableSectionElement;
    "time": HTMLTimeElement;
    "title": HTMLTitleElement;
    "tr": HTMLTableRowElement;
    "track": HTMLTrackElement;
    "u": HTMLElement;
    "ul": HTMLUListElement;
    "var": HTMLElement;
    "video": HTMLVideoElement;
    "wbr": HTMLElement;
}
```



## 6.3 静态属性和静态方法

静态成员可以访问其他静态成员，但无法访问非静态成员；静态成员可以直接通过 `类名.xxx` 访问

```ts
class Person {
    static category: string = 'human'
    private name: string
    private age: number
    
    constructor(name: string, age: number) {
        this.name = name
        this.age = age
    }

    static say() {
        // 静态成员函数可以访问静态成员变量或其他静态成员函数
        console.log(this.category)  
        this.talk()
    }

    static talk() {
        console.log('你好，我是静态成员函数talk...')
    }
}

Person.talk()
```



## 6.4 getter 和 setter

TS 中 getter 和 setter 要求成员变量以 `_` 开头，提供的 getter 和 setter 格式如下：

```ts
private _name

// getter
get name() {}

// setter
set name(name: string) { this._name = name }
```

示例如下：

```ts
abstract class Animal {
    protected _name: string
    protected _age: number

    constructor(name: string, age: number) {
        this._name = name
        this._age = age
    }
    get name() {
        return this._name
    }

    set name(name: string) {
        this._name = name
    }
    
    abstract say(): void
    abstract hunt(): void
}

class Dog extends Animal {
    constructor(name: string, age: number) {
        super(name, age)
    }
    say(): void {
        console.log('汪汪汪...')
    }    
    hunt(): void {
        console.log('挖骨头...')
    }
}

class Cat extends Animal {

    constructor(name: string, age: number) {
        super(name, age)
    }

    say(): void {
        console.log('喵喵喵...')
    }
    hunt(): void {
       console.log('抓鱼...')
    }


}
let dog: Dog = new Dog('大黄', 18)
// 调用 getter
console.log(dog.name)
// 调用 setter 
dog.name = '小白'
console.log(dog.name)
console.log('============')
let cat: Cat = new Cat('小黄', 3)
console.log(cat.name)
```



## 6.6 `keyof` 操作符

`keyof` 操作符可以帮助我们获取某种类型的所有键，返回类型为联合类型，如下：

```ts
interface Person {
  name: string;
  age: number;
  location: string;
}

type K1 = keyof Person; // "name" | "age" | "location"
type K2 = keyof Person[];  // number | "length" | "push" | "concat" | ...
type K3 = keyof { [x: string]: Person };  // string | number
```



**应用场景**

解决函数中传入的参数类型未知问题，如下：

```ts
function fn(obj: object, key: any) {
    return obj[key]
}
// Element implicitly has an 'any' type because expression of type 'any' can't be used to index type '{}'
```

对于函数 fn，传入的 obj 索引类型未知，我们不能确定 key 的类型是 string、number 还是其他更为复杂的类型，但是如果我们直接类型定义为 any，ts　编译器又会报错，所以解决方法如下：

```ｔｓ
function fn<T extends object, K extends keyof T>(obj: T, key: K) {
    return obj[key]
}
```



## ６.７　命名空间　namespace

无论是在什么语言中，都会存在变量污染的问题，解决方案也有很多，例如　c++　中提供 `namespaced`，在 TS 中则是通过 `namespace` 声明命令空间，有如下特点：

1. 通过 `namespace` 定义，通过 `export` 暴露出去
2. 命名空间内的类默认私有

> TS 与 ES2015 类似，如果文件中包含 `import` 或者 `export` 那么该文件就会被当做一个模块，而如果没有，里面内容则被被视为全局可见

**示例**

```ts
namespace a {
    export const person = { name: 'zs', age: 18}
    export const fn = <T>(param: T): T => {
        return param
    }
}

console.log(a.person.name)  // zs
console.log(a.fn('zs'))     // zs
```

> 在命名空间中需要将需要暴露的部分通过 `epxort` 导出



**嵌套变量命名空间**

```ts
namespace a {
    export namespace b {
        export const x: number | boolean | never = false
    }
}

console.log(a.b.x)
```



**命名空间合并**

```ts
namespace a {
    export const name = 'zs'
}

namespace a {
    export const age = 18
}

console.log(a.name)
console.log(a.age)
```



# 7 高级语法

## 7.1 三斜线指令

三斜线指令是包含<strong style="color:red">单个 XML 标签</strong>的单行注释。注释的内容会作为编译器指令使用。

三斜线指令放置的位置：==文件最顶端==，如果没有放在文件最顶端（指令前面有其他非注释代码）则会被当做普通注释。

三斜线指令最常见的用途有两个：

1. 导入声明文件
2. 导入第三方库

我们可以看到作用其实类似于 `import`，如果做对比的话，那么他们之间的关系如下：

- `/// <reference path="...">` => `import ./xxx`
- `/// <reference types="...">` => `import xxx from 'xxx'`



## 7.2 declare 声明文件

当我们在 TS 项目中，当我们用到第三方库时，需要引用它的声明文件，才能获得对应的代码补全、接口提示等功能

但是并不是所有库都配置了自己的声明文件，即 `xxx.d.ts`，对没有配置声明文件的库，当我们通过 `import` 导入时就会报错，我们以 axios 和 express 为例

axios 中拥有自己的声明文件 axios.d.ts，所以导入时不会有问题，但是 express 由于没有声明文件，在导入时会出现 ==无法找到模块 xxx 的声明文件== 这种错误，我们可以通过下面的命令来解决这一问题：

```bash
npm i --save-dev @types/express
```

> 该声明文件是由非官方的其他编程人员书写的，像这些第三方包都已收录在 https://www.npmjs.com/~types?activeTab=packages

当然，对于一些冷门的库，如果官方和第三方都没有提供其声明文件，我们就需要自己手写其对应的声明文件

