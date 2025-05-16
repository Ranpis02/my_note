# 1 JSP 基础

## 1.1 为什么需要 JSP

因为 Servlet 做界面非常不方便，而 HTML 又不能动态获取数据，如下：

![image-20230325093442305](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230325093442305.png)

所以引入了 JSP。

```
JSP = HTML + JAVA 片段 + 标签 + JavaScript + CSS
```

> 在手册中，查阅 JSP 相关 API 请参考 javax.servlet.jsp



## 1.2 JSP 基本介绍

1. JSP 全称为 <u>Java Server Pages，Java 的服务器页面，就是服务器渲染技术</u>
2. JSP 这门技术最大的特点在于，写 JSP 就像在写 HTML
   - 相较于 HTML 只能为用户提供静态数据，而 JSP 技术允许在页面中嵌套 Java 代码，为用户提供数据
   - 相较于 Servlet 而言，Servlet 很难对数据进行排版，而 JSP 除了可以用 Java 代码生成动态数据的同时，也很容易对数据进行排版
3. JSP 技术基于 Servlet，可以理解为 <strong style="color:red">JSP 就是对 Servlet 的包装</strong>



## 1.3 JSP 快速入门

创建 WEB 项目，引入相关的 Jar 包，包括 jsp-api.jar 和 servlet-api.jar

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230325100804648.png" alt="image-20230325100804648" style="zoom:50%;" />

在 web 工作目录中添加 jsp 文件，编写如下代码：

```jsp
<%
    int i = 10;
    int j = 20;
    int res = i + j;
    // 在 jsp 中，该标签可以直接书写 java 代码
	// out、response、request 等等都是 jsp 内置的对象，可以直接拿来使用
    out.println(i + " + " + j + " = " + res);
%>
```

:warning:**注意事项**

1. JSP 页面不能像 HTML 页面一样，直接用浏览器运行，只能通过访问浏览器访问 Tomcat 来访问 JSP 页面

2. 设置 JSP 模板：打开 IDEA 的 Settings → File and Code Templates  → Other → JSP files

   ![image-20230326105917779](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230326105917779.png)



## 1.4 JSP 运行原理

JSP 本质上是一个 Servlet 程序，其性能是和 Java 关联的，其运行原理如下：

1. 第一次访问 JSP 页面时，Tomcat 服务器会将 JSP 页面解析为一个 Java 源文件，并将其编译为 .class 字节码程序，如下：

   <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230326112324181.png" alt="image-20230326112324181" style="zoom:50%;" />

   > - sum_jsp.java 是 JSP 对应的 Servlet
   > - sum_jsp.class 是对应的字节码文件

2. 我们打开 sum_jsp.java 可以看到该类继承自 HttpJspBase。 我们可以先引入 jasper.jar（在 tomcat/lib 文件夹下），然后在 JSP 中查看对应类的类图

   <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230326113941384.png" alt="image-20230326113941384" style="zoom:50%;" />

   > 可以看到其实 JSP 本质就是一个 Servlet，其继承自 HttpServlet，同时实现了 HttpJspPage 接口



## 1.5 Page 指令

模板如下：

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
```

```jsp
<%@ page import="org.apache.jasper.runtime.HttpJspBase" %>
```

说明：

1. language 表示 JSP 翻译后是什么语言文件（虽然目前只支持 Java）
2. contentType 表示 JSP 返回的内容类型（文本类型、字符编码等），对应源码中的 `response.setContentType()` 参数值
3. pageEncoding 属性表示当前 JSP 页面文件本身的字符集
4. import 属性和 Java 源代码中一样，用于导包和类



## 1.6 声明脚本

```jsp
<%! 
    // 在 jsp 中，通过该标签声明脚本（声明类）
    // 1. 添加成员属性
    private Integer id;
    private String name;
    private Double sal;
    private static String company;
    // 2. 添加成员方法
    public Integer getId() {
        return id;
    }
    // 3. 添加静态代码块
    static {
        company = "zs";
    }
%>
```



## 1.7 表达式脚本

表达式脚本格式如下：

```jsp
<%=表达式%>
```

- 表达式脚本的作用：在 JSP 页面上输出数据
- 脚本中的表达式不能以分号结束

**示例**

```jsp
<%
  String name = "Alice";
%>
userName: <%=name%><br/>
job: <%="java developer"%><br/>
age: <%=request.getParameter("age")%><br/>
```

当我们在浏览器中输入：`localhost:8080/jsp/exp.jsp?age=20` 后，可以直接在页面中展示相应的信息



## 1.8 代码脚本

代码脚本的格式如下：

```jsp
<% 脚本 %>
```

**示例**

```jsp
<%
    ArrayList<Monster> monsterArrayList = new ArrayList<>();
    monsterArrayList.add(new Monster("铁扇公主", 30, "芭蕉扇"));
    monsterArrayList.add(new Monster("蜘蛛精", 36, "吐丝"));
%>
<table>
    <tr>
        <th>姓名</th>
        <th>年龄</th>
        <th>技能</th>
    </tr>
    <%
        for (Monster m : monsterArrayList) {
    %>
    <tr>
        <td><%=m.getName()%></td>
        <td><%=m.getAge()%></td>
        <td><%=m.getSkill()%></td>
    </tr>
    <%
        }
    %>
</table>
```



## 1.9 JSP 的注释

对于 JSP ，如果是在 Java 片段中，那么我们的注释遵循 Java 的语法，使用 ；如果是在 Java 片段外，那么我们注释有如下两种写法：

```jsp
<!-- 我是一个注释 -->
```

```jsp
<%-- 我是一个注释 --%>
```



# 2 JSP 进阶

## 2.1 JSP 内置对象

JSP 的内置对象（inbuild），是指在 Tomcat 翻译 JSP 页面成为 Servlet 后，内部提供的九大对象，叫做内置对象。

1. `out`：向客户端输出数据，例如：`out.println("")`

   > 数据类型为 JspWriter，与 Servlet 中的 printWriter 一致

2. `request`：客户端的 http 请求

3. `response`：服务端的 http 响应

4. `session`：会话对象

5. `application`：对应 ServletContext

6. `pageContext`：页面的上下文，是一个域对象

7. `exception`：异常对象，可以通过 `getMessage()` 获取异常信息

8. `page`：代表 JSP 这个实例本身，类似于 `this`

9. `config`：对应 ServletConfig

 JSP 内置对象与 Servlet 中对象的映射：

![image-20230326170032273](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230326170032273.png)



## 2.2 域对象

所谓的 "域对象"，即：在某个范围内有效的对象。JSP 中有四大域对象，分别是：

- pageContext：存在的数据只在当前页面中有效

  ![image-20230326165523127](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230326165523127.png)

- request：存放到数据在一次 request 请求中有效

  ![image-20230326165600480](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230326165600480.png)

- session：存放的数据在一次会话中有效

  ![image-20230326165853358](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230326165853358.png)

- application：存放的数据在整个 web 应用运行期间有效，范围更大

  ![image-20230326165940522](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230326165940522.png)



## 2.3 JSP 请求转发标签

**示例**

```jsp
<jsp:forward page="/dest.jsp"></jsp:forward>
```

在 source.jsp 的 body 中添加上述代码，可以将**请求转发**到 dest.jsp 中



## 2.4 习题：使用 JSP 完成一个计算机



# 3 EL 表达式

1. EL 表达式全称：Expression Language，即表达式语言
2. EL 表达式主要是代替 JSP 页面的表达式脚本 `<%=request.getAttribute("xx")%>`
3. EL 表达式的基本语法：`${key}` ，我们可以简单地将其理解为一个语法糖
4. 我们在使用 EL 表达式取属性值时，默认是从小范围开始取，没有则向上取：pageContext → request → session → application

**示例**

```jsp
<%
    pageContext.setAttribute("name", "zs");
%>
<%--  对于传统的表达式脚本，如果 name 为 null，则会以字符串的形式的输出；--%>
<%--  而对于 el 表达式语法，如果 name 为 null，则会以空串 "" 的形式输出--%>
name = <%=pageContext.getAttribute("name")%><br/>
name = ${name}<br/>
```



## 3.1 EL 表达式常见输出形式

**测试对象**

```java
public class Book {
    private String name;
    private String[] author;
    private List<String> reader;
    private HashMap<String, String> topics;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String[] getAuthor() {
        return author;
    }

    public void setAuthor(String[] author) {
        this.author = author;
    }

    public List<String> getReader() {
        return reader;
    }

    public void setReader(List<String> reader) {
        this.reader = reader;
    }

    public Map<String, String> getTopics() {
        return topics;
    }

    public void setTopics(HashMap<String, String> topics) {
        this.topics = topics;
    }

    @Override
    public String toString() {
        return "Book{" +
                "name='" + name + '\'' +
                ", author=" + Arrays.toString(author) +
                ", reader=" + reader +
                ", topics=" + topics +
                '}';
    }
}
```



**测试输出**

```jsp
<%
    Book book = new Book();
    book.setName("红楼梦");
    book.setAuthor(new String[]{"曹雪芹", "高鹗"});
    ArrayList<String> reader = new ArrayList<>();
    reader.add("张三");
    reader.add("李四");
    reader.add("王五");
    HashMap<String, String> topics = new HashMap<String, String>();
    topics.put("NO 1", "喜欢你的作品");
    topics.put("NO 2", "看完后意犹未尽");
    book.setReader(reader);
    book.setTopics(topics);

    // 将 book 放入 request 域对象中
    request.setAttribute("book_key", book);
%>

book对象: ${book_key}<br/>    <%-- 注意： book_key 取出的是request域对象中的属性 --%>
书名: ${book_key.name}<br/>
第一作者: ${book_key.author[0]}<br/>
读者1: ${book_key.reader[0]}<br/>
第一条评论: ${book_key.topics["NO 1"]}<br/>
```



## 3.2 EL 运算操作

**基本语法**

```jsp
${ 运算表达式 }
```

**关系运算**

| 关系运算符   | 范例                      |
| ------------ | ------------------------- |
| `==` 或 `eq` | `${5==5}` 或 `${5 eq 5}`  |
| `!=` 或 `ne` | `${5!=5}` 或 `${5 ne 5}`  |
| `>` 或 `gt`  | `${5>3}` 或 `${5 gt 3}`   |
| `<` 或 `lt`  | `${3 < 5}` 或 `${3 lt 5}` |
| `>=` 或 `ge` | `${5>=3}` 或 `${5 ge 3}`  |
| `<=` 或 `le` | `${3<=5}` 或 `${3 le 5}`  |

**逻辑运算**

| 逻辑运算符    | 说明   |
| ------------- | ------ |
| `&&` 或 `and` | 与运算 |
| `||` 或 `or`  | 或运算 |
| `!` 或 `not`  | 非运算 |

**算术运算符**



## 3.3 Empty 运算

empty 运算可以判断一个数据是否为空，如果为空，则返回 true，否则返回 false

以下几种情况为空：

- 值为 null
- 值为空串
- 值为 Object 类型数组，但长度为零
- list 集合，元素个数为0
- map 集合，元素个数为0

**示例**

```jsp
<%
    request.setAttribute("k1", null);
    request.setAttribute("k2", "");
    request.setAttribute("k3", new Object[]{});
    request.setAttribute("k4", new ArrayList<>());
    request.setAttribute("k5", new HashMap<>());
%>
k1是否为空：${empty k1}<br>
k2是否为空：${empty k2}<br>
k3是否为空：${empty k3}<br>
k4是否为空：${empty k4}<br>
k5是否为空：${empty k5}<br>
```



## 3.4 EL 获取域对象

EL 中隐含的 11 个隐含对象，可以直接使用：

![image-20230326193759220](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230326193759220.png)



其中，我们只需要关注四个域对象

![image-20230326193850297](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230326193850297.png)



**示例**

```jsp
<%
    pageContext.setAttribute("k1", "我是pageContext作用域下的对象");
    request.setAttribute("k2", "我是request作用域下的对象");
    session.setAttribute("k3", "我是session作用域下的对象");
    application.setAttribute("k4", "我是application作用域下的对象");
%>

pageContext作用域下的k1: ${pageScope.k1}<br>
request作用域下的k2: ${requestScope.k2}<br>
session作用域下的k3: ${sessionScope.k3}<br>
application作用域下的k4: ${applicationScope.k4}<br>
```



:herb:**对于 PageContext 这个隐含对象我们需要进行单独说明**

```jsp
<%--    通过pageContext可以获取request和response对象上的相关的属性--%>
<%--接下来，我们以获取request对象下的http协议相关的数据为例--%>
<%
    pageContext.setAttribute("req", request);
    // 将 request 取别名 req，挂载在 pageContext 上，以后获取pageContext.request.scheme 只需要通过req.scheme 即可
%>
<hr>
协议：${req.scheme}<br>
服务器IP：${req.serverName}<br>
服务器端口：${req.serverPort}<br>
工程路径：${req.contextPath}<br>
请求方法：${req.method}<br>
客户端IP地址：${req.remoteHost}<br>
会话ID：${req.session.id}<br>
请求远程主机的IP地址：${req.remoteAddr}<br>
```



# 4 JSTL

JSTL 标签库是指 "**JSP Standard Tag Library**"，即JSP 标准标签库

1. EL 表达式是为了替换 JSP 中的表达式脚本，JSTL 是为了替换代码脚本，这样 JSP 页面会变得更加简洁

2. JSTL 由五个标签库组成

   ![image-20230326203332709](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230326203332709.png)

:warning:**注意事项**

在使用 JSTL 之前，我们需要引入相关的 Jar 包

![image-20230326203550005](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230326203550005.png)

**入门案例**

```jsp
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>jstl快速入门</title>
</head>
<body>
    <c:if test="${10 > 2}">
        <h1>10 > 2 为真</h1>
    </c:if>
</body>
</html>
```



## 4.1 set 标签

set 标签的作用：<strong style="color:red">用于保存域中的数据</strong>

基本用法：

```jsp
<c:set scope="request" var="userName" value="zs"></c:set>
```

- 等价于 `域对象.setAttribute(key, value)`
- scope 表示属性保存那个域中
  - page 表示 PageContext 域（默认值）
  - request 表示 Request 域
  - session 表示 Session 域
  - application 表示 ServletContext 域
- var 属性设置 【键】
- value 属性设置 【值】

**示例**

```jsp
<c:set scope="request" var="userName" value="zs"></c:set>
用户名: ${requestScope.userName}
```



## 4.2 if 标签

if 标签的作用：<strong style="color:red">控制标签体的内容显示与否</strong>

**示例**

```jsp
<c:set scope="request" var="num1" value="10"></c:set>
<c:set scope="request" var="num2" value="20"></c:set>
<c:if test="${ num1 < num2 }">
    <h1>${num1} < ${num2}</h1>
</c:if>
```



## 4.3 choose 标签

choose 搭配 when 和 otherwise 进行多分支选择

```jsp
<c:set var="score" value="90"></c:set>
<c:choose>
    <c:when test="${score >= 80}">
        <h1>成绩优秀</h1>
    </c:when>
    <c:when test="${score >=70}">
        <h1>成绩优良</h1>
    </c:when>
    <c:when test="${score >= 60}">
        <h1>成绩及格</h1>
    </c:when>
    <c:otherwise>
        <h1>没有及格</h1>
    </c:otherwise>
</c:choose>
```



## 4.4 forEach 标签

forEach 标签主要用来遍历输出，主要有以下四种形式：

1. 普通遍历输出 `i` 到 `j` 
2. 遍历数组
3. 遍历 Map
4. 遍历 List

**示例**

```jsp
<%
    request.setAttribute("sports", new String[]{"篮球", "羽毛球"});

    HashMap<Integer, String> rank = new HashMap<Integer, String>();
    rank.put(1, "项目经理");
    rank.put(2, "产品经理");
    rank.put(3, "运维");
    request.setAttribute("rank", rank);
%>
<ul>
    <c:forEach begin="1" end="5" step="1" var="i">
        <li>${i}</li>
    </c:forEach>
</ul>

<c:forEach items="${sports}" var="item">
    ${item}<br>
</c:forEach>
<c:forEach items="${rank}" var="item">
    ${item.key}-${item.value}<br>
</c:forEach>
```

