我们一直知道 `forEach` 是没有返回值并且不直接改变原数组的，今天发现是不直接改变，实际上里面是包含着改变的。

1. forEach(item, index, arr)，三个参数，如果直接用 item=xxx 是无法改变原数组的，但是如果用arr[index]就可以改变原数组。

```javascript
var s = [1,2,3,4];
s.forEach(item=>{
	item = 'a'
});

console.log(s);// ["1", "2", "3", "4"] 未改变原数组

s.forEach((item, index, arr)=>{
    arr[index] = 'b'
});

console.log(s);// ["b", "b", "b", "b"] 改变了原数组
```

2. 数组里面的子元素是对象，那么是可以改变对应属性的

```javascript
var s = [{a:1}, {a:1}];
s.forEach(item=>{
    item = null;
});

console.log(s);//[{a: 1} ,{a: 1}] 未改变原数组

s.forEach(item=>{
    item.a = 666;
});

console.log(s); // [{a: 666}, {a: 666}] //改变的原数组里面的对象属性
```