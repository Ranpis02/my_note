# 1 控制台花式打印数据

`console.log` 其实和 `prinf` 一样也可以格式化打印数据，方法如下：

1. `%c` 表示 css 样式

   ```js
   console.log('%c' + 'hello world', 'color: red;font-size: 24px;font-weight: bold;text-decoration: underline;')
   ```

2. `%d` 表示数字

   ```js
   console.log('%d', 123)
   ```

3. `%i` 表示整型数字

   ```js
   console.log('%i', 123)
   ```

4. `%o` 表示 DOM 元素

   ```js
   console.log('%o', document.body)
   ```

5. `%O` 表示 JS 对象

   ```js
   console.log('%O', new Date())
   ```



在 JS 中，其实 console 也是一个对象，其常用的方法如下：

```js
console.log() 		// 打印日志
console.debug()		// 打印调试
console.error()		// 打印错误
console.info()		// 打印信息
console.warn()		// 打印警告
console.assert()	// 打印断言
```

