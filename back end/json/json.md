[toc]

# 1 JSON 介绍

JSON 官方文档：[传送门](https://www.w3school.com.cn/js/js_json_intro.asp)

- JSON 指的是 JavaScript 对象表示法（ **J**ava**S**cript **O**bject **N**otation）
- JSON 是轻量级的文本数据交换格式
- JSON 独立于语言（即 Java、Php、Go）都可以使用使用 JDON）
- JSON 具有自由描述性，更易理解（好用）



# 2 JSON 入门

**定义格式**

```json
// 第一种形式
{
    "k1": value,    // Number 类型 
    "k2": "value",  // 字符串类型
    "k3": [],       // 数组类型
    "k4": {},       // 对象类型 
    "k5": [{}, {}]  // 对象数组 
};
```

```json
//  第二种形式
[
    {},
    {},
    {}
]
```

**注意事项**

- 映射用冒号 `:` 表示，注意名称是字符串，一定要用双引号引起来
- 并列的数据之间使用逗号 `,` 分隔，最后一个元素尾部不能留有 `,` 
- 映射的集合（对象）用大括号 `{}` 表示，`{"名称1":值,"名称2":值}`
- 并列的集合（数组）用方括号 `[]` 表示，`[{"名称1":值,"名称2":值}, {"名称1":值,"名称2":值}]`
- 元素值类型范围：String、Number、Object、Array、Boolean、null



# 3 JSON 对象与字符串转换

1. `JSON.stringify(json)`：将一个 json 对象转换成 json 字符串
2. `JSON.parse(jsonString)`：将一个 json 字符串转换成 json 对象

**示例**

```js
let obj = {
    "name":"tom",
    "age":20
};
// 将obj转换为json字符串
let jsonStr = JSON.stringify(obj);


// 将json字符串解析为obj
let objDeepCopy = JSON.parse(jsonStr);

console.log(objDeepCopy == obj);    // false
```

> :triangular_flag_on_post:小技巧：如果我们想要实现深拷贝，可以用 JSON 这两个方法进行序列化和反序列化得到深拷贝对象

**注意事项**

在定义对象时，建议使用 `""` 包裹属性，如果使用 `''`，在定义 JSON 对象时没有问题，但是将原生字符串转成 JSON 对象时则会报错：

```js
let flower = "{'name': 'rose', 'color': 'red'}";
let o = JSON.parse(flower); // Uncaught SyntaxError
```



# 4 JSON对象 与 Java 对象进行相互转换

1. Java 中使用 JSON，需要引入第三方的包 gson.jar
2. gson 是 Google 提供的用来在 Java 对象和 JSON 数据之间进行映射的 Java 类库
3. 可以对 JSON 字符串和 Java 对象进行相互转换



JSON 在 Java 中的应用场景：

1. JavaBean 对象和 JSON 字符串的转换
2. List 对象和 JSON 字符串的转换
3. Map 对象和 JSON 字符串的转换

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230326215605859.png" alt="image-20230326215605859" style="zoom:50%;" />

## 4.1 JSON 与 JavaBean 的相互转换

```java
// 创建 gson 对象
Gson gson = new Gson();
Book book = new Book(433, "红楼梦");
// 1. 使用 toJson(),将Book对象转换为JSON字符串
String bookToJson = gson.toJson(book);

// 2. 使用 fromJson(), 将JSON字符串转换为原本的对象
/**
 * 参数1：表示JSON字符串
 * 参数2：表示该JSON字符串需要转换成的对象类
 * 说明：fromJson底层运用到反射机制
 */
Book JsonToBook = gson.fromJson(bookToJson, Book.class);
```



## 4.2 List 对象与 JSON 字符串的相互转换

```java
ArrayList<Book> book = new ArrayList<Book>();
book.add(new Book(442, "西游记"));
book.add(new Book(443, "红楼梦"));
Gson gson = new Gson();
String bookToJson = gson.toJson(book);
/**
 * 由于TypeToken类的无参构造器为protected,只有同包或子类能够访问
 * 解决方法：通过创建匿名内部类，调用里面的无参构造器，由于无参构造中默认会调用super()，于是TypeToken无参构造也会被调用，之后再通过public的getType()方法返回List<Book>全类名，例如java.util.List<com.json.JavaBean.Book>
 */
Type type = new TypeToken<List<Book>>(){}.getType();
List<Book> JsonToBook = gson.fromJson(bookToJson, type);
```



## 4.3 Map 对象与 JSON 字符串的转换

```json
HashMap<String, Book> book_map = new HashMap<String, Book>();
book_map.put("001", new Book(445, "射雕英雄传"));
book_map.put("002", new Book(446, "笑傲江湖"));
Gson gson = new Gson();
String bookToJson = gson.toJson(book_map);
gson.fromJson(bookToJson, new TypeToken<HashMap<String, Book>>() {
}.getType());
```



