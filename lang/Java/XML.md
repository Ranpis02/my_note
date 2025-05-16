[toc]

# 1 XML 基础

XML 全称为 "Extensible Markup Language"，即可扩展标记语言，XML 主要的作用是用来<strong style="color:red">传输和存储数据</strong>

> XML 作为配置文件用途广泛，例如 Tomcat 中 server.xml，spring 中 ico 配置文件（beans.xml, Maven 中的 pom.xml ）

**应用场景**

1. 解决程序间的数据传输

   例如最早的 QQ 就是使用 XML 格式来传输数据的，具有良好 的可读性、可维护性

2. xml 作为配置文件

   xml 作为配置文件使用十分广泛，例如 tocat 服务器中的 server.xml，web.xml，Maven 中的 pom.xml 等

3. xml 可以充当小型的数据库



## 1.1 创建 XML 文件

在 IDEA 中创建一个新的 Java 项目，之后【新建文件】即可

> 注意：不要新建一个 java class 文件，而是选择新建普通文件

**模板**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!--
    1. xml 表示该文件的类型
    2. version="1.0" 版本号
    3. encoding 文件编码
    4. students root元素，每个文件只能有一个root元素
    5. student 中的id为student的属性
-->
<students>
    <student id="100">
        <name>jack</name>
        <age>10</age>
        <gender>男</gender>
    </student>
</students>
```



## 1.2 xml 语法

xml 可以分为如下几部分：

- 文档声明：放在XML文档的第一行

- 元素：对于元素我们会有如下要求

  - 每个 XML 文档必须有且只有一个根元素
  - 根元素是一个完全包括文档中其他元素的元素
  - xml 元素是指 xml 文档中出现的标签，可以分为包含标签体和不包含标签体的
  - 元素之间不能相互嵌套
  - 元素的还有其他的称呼，例如：节点、标签
  - 空元素（或者叫做自闭合标签）：`<code/>`

- 属性

  - 属性值可以用 `""` 或 `''` 括起来
  - 一个元素可以有多个属性
  - 属性不能重复，特定的属性名称在同一个元素标记中只能出现一次
  - 属性值不能包括 `&` 字符

- 注释

  ```xml-dtd
  <!--这是一个注释-->
  <!--
  	1. 注释不能嵌套
  	2. 注释不能放在元素中
  -->
  ```

- CDATA 节

**重点介绍 CDATA 节**

<strong style="color:red">有些内容不想让解析引擎执行，而是当做原始内容处理（即普通文本）</strong>，可以使用 CDATA 括起来，如下：

```xml
<![CDATA[这里可以放入普通文本]]>
<!-- 注意不能放入 ]]> -->
```

> IDEA 中可以通过 CD + TAB 快速生成

## 1.3 元素命名规则

- 区分大小写

- 不能以数字开头
- 不能包含空格
- 名称中间不能包含冒号
- 建议使用下划线命名方法



## 1.4 转义字符

| 转义符号 |  含义  |
| :------: | :----: |
|  `&lt;`  |  &lt;  |
|  `&gt;`  |  &gt;  |
| `&amp;`  | &amp;  |
| `&quot;` | &quot; |
| `&apos`  | &apos; |



# 2 dom4j

早期 JDK 为我们提供了两种 XML 解析技术 DOM 和 Sax（Simple API for XML），而如今这两种技术都已经过时

**第三方的 XML 解析技术**

1. jdom 在 dom 基础上进行了封装
2. dom4j 又对 jdom 进行了 封装
3. pull 主要用在 Android 手机开发，和 Sax 非常类似，都是事件机制解析 XML 文件

**dom4j 介绍**

1. dom4j 是一个简单、灵活的开放源代码的库（用于解析/处理XML文件）
2. 与 JDOM 不用，dom4j 使用 接口 和 抽象基类，虽然 dom4j 的 API 要相对复杂，但是它提供了比 JDOM 更好的灵活性
3. dom4j 是一个非常优秀的 Java XML API，具有性能优异、功能强大、易于使用等特点

> dom4j 官网地址：https://dom4j.github.io/



## 2.1 获取 document 对象

1. 读取XML文件，获得document对象

   ```java
   SAXReader saxReader = new SAXReader();
   Doucment doucment = saxReader.read(new File("src/input.xml"));
   ```

2. 解析 XML 形式的文本，得到 document 对象

   ```java
   String text = "<members></members>";
   Document document = DocumentHelper.parseText(text);
   ```

3. 主动创建 document 对象

   ```java
   Document document = DocumentHelper.createDocument(); 
   Element root = document.addElement("members");
   ```



## 2.2 遍历 XML 指定元素

在使用 dom4j 之前，我们需要先将其引入到 Java 项目中，在根目录 创建 lib 文件夹，将 jar 包放入，然后将 lib 【Add as Library】（或者将该 jar 包添加到构建路径中）

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230305223634704.png" alt="image-20230305223634704" style="zoom: 50%;" />

**演示文件**

```xml
<?xml version="1.0" encoding="utf-8" ?>
<!--student.xml-->
<students>
    <student id="01">
        <name>小龙女</name>
        <gender>女</gender>
        <age>16</age>
        <resume>古墓派掌门人</resume>
    </student>
    <student id="02">
        <name>欧阳锋</name>
        <gender>男</gender>
        <age>18</age>
        <resume>白驼山KTV</resume>
    </student>
</students>
```



**实现代码**

```java
public void dom4jTest() throws DocumentException {
    // 创建一个解析器
    SAXReader saxReader = new SAXReader();
    // 加载文档
    Document document = saxReader.read(new File("Demo/src/student.xml"));   // content path

    // 遍历所有学生信息
    // 1. 获取 root 节点，类型为 Element
    Element rootElement = document.getRootElement();
    // 2. 得到rootElement的student元素
    List<Element> student = rootElement.elements("student");
    System.out.println("student元素个数=" + student.size()); // 获取student元素的个数
    // 3. 增强 for 循环遍历
    for (Element stu : student) {
        // 获取student元素里面的子元素
        Element name = stu.element("name");
        Element gender = stu.element("gender");
        Element age = stu.element("age");
        Element resume = stu.element("resume");
        System.out.println("name=" + name.getText() + " ," + "gender=" + gender.getText() + " ," + age.getText() + " ," + resume.getText());
    }
```



## 2.3 指定读取 XML 元素

```java
// 通过get方法可以获取第一个学生元素
Element element = student.get(0);
System.out.println(element.element("name").getText());

// 通过attributeValue获取id
System.out.println("id=" + element.attributeValue("id"));
```

如果说我们想要直接获取某个元素，可以使用 【dom4j + xpath】



## 2.4 增删改操作

```java
public void Dom4jTest() throws DocumentException, IOException {
    SAXReader saxReader = new SAXReader();
    Document document = saxReader.read(new File("Demo/src/student.xml"));
    // 创建一个学生节点对象
    Element newStu = DocumentHelper.createElement("student");
    // 为节点添加属性
    newStu.addAttribute("id", "04");

    // 创建name节点，并添加文本
    Element name = DocumentHelper.createElement("name");
    name.setText("宋江");

    // 创建age节点，并添加文本
    Element age = DocumentHelper.createElement("age");
    age.setText("30");

    // 将name和age节点加入到newStu上
    newStu.add(name);
    newStu.add(age);

    // 将newStu加入到root节点上
    document.getRootElement().add(newStu);

    OutputFormat output = OutputFormat.createPrettyPrint();
    output.setEncoding("utf-8");//输出的编码  utf-8
    //把我们的  xml  文件更新
    //  lets  write  to  a  file
    //new  FileOutputStream(new  File("src/myClass.xml"))
    //使用到 io 编程    FileOutputStream    就是文件字节输出流
    XMLWriter writer = new XMLWriter(new FileOutputStream(new File("src/students.xml")), output);
    writer.write(document);
    writer.close();
}
```

删除和更新操作后续补上

