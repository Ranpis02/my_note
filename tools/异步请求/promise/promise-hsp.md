[toc]

# 1 promise 基础

<strong style="color:red">promise 是异步编程的一种解决方案，可以解决传统的 Ajax 回调函数嵌套（callback hell）问题</strong>



## 1.1 基本介绍

1. 传统的 Ajax 异步调用在需要多个操作的时候，会导致多个回调函数相互嵌套，导致代码不够直观
2. 为了解决上述问题，promise 对象应运而生，在 ES 2015 中已经成为标准
3. promise 是异步编程的一种解决方案
4. 从语法上来说，promise 是一个对象，从它可以获取异步操作的消息



## 1.2 模拟回调地狱

演示目录结构如下：

```
Ajax                      
├─ data                      
│  ├─ monester.json          
│  └─ monster_detail_1.json  
└─ ajax.html                 
```

**准备文件**

```json
// monster.json
{
    "id": 1,
    "name": "黑山老妖"
}
```

```json
// monster_detail_1.json
{
    "name": "黑山老妖",
    "address": "黑山洞",
    "skill": "无法无天-翻江倒海",
    "age": 300
}
```

获取数据的 js 文件

```js
$.ajax({
    url: './data/monster.json',
    success(data) {
        $.ajax({
            url: `./data/monster_detail_${data.id}.json`,
            success(data) {
                console.log(data);
            }
        })
    }
})
```

> 可以看到如果 ajax 回调变多的话，代码会一直向右延伸，十分不美观



使用 promise 改写上述代码

```js
// 将处理操作进行封装到 promise 对象
/**
 *  resolve => .then
 *  reject => .catch
 */
let p = new Promise((resolve, reject) => {
    $.ajax({
        url: './data/monster.json',
        success(data) {
            console.log("第一层成功的回调")
            resolve(data)
        },
        error(err) {
            console.log("第一层失败的回调")
            reject(err)
            console.log("1")
        }
    })
})
p.then((data) => {
    // 将内部的新的 promise 对象作为返回值返回，以便于直接在p后进行then或catch处理
    return new Promise((resolve, reject) => {
        $.ajax({
            url: './data/monster_detail_.json',
            success(data) {
                console.log("第二层成功的回调")
                resolve(data)
            },
            error(data) {
                console.log("第二层失败的回调")
                reject(data)
            }
        })
    })
}).then((data) => {
    console.log(data)
}).catch((err) => {
    // 对异常统一处理
    console.log(err)
})
```



## 1.3 promise 重排优化

上述代码虽然能够解决问题，但是还是存在大量的 error 和 success 处理函数，我们可以对其进行重排优化：

```js
function handler(url, data) {
    return new Promise((resolve, reject) => {
        $.ajax({
            url,
            data,
            success(data) {
                resolve(data)
            },
            error(err) {
                reject(err)
            }
        })
    })
}

handler("./data/monster.json").then((data) => {
    console.log(data)
    return handler("./data/monster_detail_1.json")
}).then((data) => {
    console.log(data)
}).catch((err) => {
    console.log(err)
}) 
```



