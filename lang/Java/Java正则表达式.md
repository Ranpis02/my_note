# 1 Java 正则表达式

## 1.1 基础入门

使用的基本步骤：

1. 创建 Pattern 对象
2. 创建 matcher 匹配器
3. 循环获取匹配对象



```java
String content = "1985年2月18日中共中央、国务院批转《长江、珠江三角洲和闽南厦漳泉三角地区座谈会纪要》，决定在长江三角洲、珠江三角洲abc地区开ccc经济开放区。1985年，中Hello市改革全面展开，重点是国有企业改革。";
// 1. 创建一个 Pattern 对象, 里面放入正则表达式，其中 [a-zA-Z]+ 表示匹配一个或多个由英文字母组成的单词
Pattern pattern = Pattern.compile("[a-zA-Z]+");
// 2. 创建匹配器，里面需要传入扫描的内容
Matcher matcher = pattern.matcher(content);
// 3. 开始循环匹配
while(matcher.find()) {
    // 4. 匹配的文本内容会放到 m.group(0) 中
    System.out.println("找到："+ matcher.group(0));
}
```



## 1.2 底层实现

```java
// \\d 表示的使 0-9 的数字， \\d\\d\\d\\d 就表示 4 位数字
Pattern pattern = Pattern.compile("\\d\\d\\d\\d");
Matcher matcher = pattern.matcher(content);
/**
 * 对 find 方法的详细解析，其中查找的过程如下：
 *  1.根据指定的规则，定位满足规则的子字符串
 *  2.找到后，将字符串的索引记录到 matcher 的 int[] groups 里，group[0] = startIndex ,
 *  group[1] = endIndex + 1, 同时将 endIndex + 1 存放到 oldLast 中
 *  3. 字符串截取底层如下，我们传 0 到 group 方法中，那么就按 groups[0] ~ groups[1] 进行截取字符串
 *  4. 如果没找到则返回 null , 否则继续向后查找
 *
 *  public String group(String name) {
 *         int group = getMatchedGroupIndex(name);
 *         if ((groups[group*2] == -1) || (groups[group*2+1] == -1))
 *             return null;
 *         return getSubSequence(groups[group * 2], groups[group * 2 + 1]).toString();
 *     }
 */
while(matcher.find()) {
    System.out.println("找到："+ matcher.group(0));
}
```



:thinking:**思考**

group() 方法中可以传入 0 ，那么应该也可以传入 1、2，……。其实这就涉及分组的概念

在正则表达式中的分组其实就是使用括号`()`将其包括，然后在索引的时候，记录括号里面匹配项的起始索引和结束索引，例如，我们上面的匹配四位数字的正则进行分组，如`(\\d\\\d)(\\d\\d)`，那么第一组对应的起始索引和结束索引会被放在 groups[2] 和 groups[3] 中，第二组会被放在 groups[4] 和 groups[5] 中，我们使用 find 遍历的时候就可以直接通过 group(1) 和 group(2) 得到分组的值



## 1.3 正则表达式语法

我们如果想要灵活运用正则表达式，那么我们需要了解 Java 正则里的各种元字符的功能，元字符从功能上可以大致分为如下几类：

1. 限定符
2. 选择匹配符
3. 分组组合和反向引用符
4. 特殊字符
5. 字符匹配符
6. 定位符



### 1.3.1 转义符号

转义符号，我们都非常熟悉了，主要是为了解决有些符号在编程语言中具有特殊含义，所以我们需要将其转义，变为普通字符

- 在其它编程语言中，转义符号为：`\`
- 在 Java 中，转义符号为：`\\`

常见的需要进行转义的符号包括：`. * + () $ / \ ? [ ] ^ { }`



### 1.3.2 字符匹配符

- `[]`：表示可接收的字符列表，例如 `[abc]` 就代表可以匹配 a、b、c
- `[^]`：表示不接受的字符列表，例如 `[^abc]` 就代表匹配除 a、b、c 以外的任意一个字符
- `-`：连字符，`[A-Z]` 代表匹配所有的大写字母，`[a-z]` 代表匹配所有的小写字母

![image-20230721161856773](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230721161856773.png)

> 除此之外，还有 `\\s` 匹配任何空白字符，例如空格、制表符等；`\\S` 匹配任何非空白字符。这里需要注意我们在使用字符匹配符时，例如 `\\d{3,4}`,数字之间千万不要加空格，例如`\\d{3, 4}` 这种写法是错误的



### 1.3.3 选择匹配符

匹配某个字符串是选择性的，我们就可以使用 `|` ，例如 `ab|cd` 就表示可以匹配 ab 或者 cd



### 1.3.4 正则限定符

![image-20230721162705519](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230721162705519.png)



### 1.3.5 正则定位符

![image-20230721163006018](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230721163006018.png)