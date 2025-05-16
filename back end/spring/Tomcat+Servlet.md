[toc]

# 1 Web 介绍说明

> Tomcat 的官网：https://tomcat.apache.org/

1. Web：在英语中 Web  表示网 / 网络资源(页面，图片，css，js)意思，它用于表示  Web  服务器（主机）供浏览器访问的资源
2. Web 服务器(主机)上供外界访问的  Web  资源分为：
   -   静态 Web 资源（如 html 页面）：指 web 页面中供人们浏览的数据始终是不变。 
   -   动态 Web 资源，比如 Servlet(java)、PHP 等。

3.    静态 Web 资源开发技术：HTML、CSS、JS 等

4.    常用动态 web  资源开发技术：Servlet、SpringBoot、SpringMVC、PHP、ASP.NET 等

---

**JavaWeb 开发技术栈**

![image-20230310182900751](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230310182900751.png)



## 1.1 BS 和  CS 说明

**BS 开发**

1. B: browser(浏览器,    种类太多    FireFox, Chrome, IE, Edge,)
2. S: Server(服务端,    考虑很多)

**BS 特点**

- 兼容性：因为浏览器的种类很多，发现你写的程序，在某个浏览器会出现问题，其它浏览器正常
- 安全性：通常情况下，BS  安全性不如 CS 好控制
- 易用性：BS 好于 CS,    浏览器电脑有扩展性，BS 相对统一，只需要写 Server

![image-20230318000230734](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230318000230734.png)



# 2 JavaWeb 服务器

学习 JavaWeb 开发，需要先安装 JavaWeb 服务软件【我们把安装了 JavaWeb 服务软件主机称为 Web 服务器 / JavaWeb 服务器】，然后在 web 服务器中开发相应的 web 资源。 

> 例如：Javaweb 服务器，Mysql 服务器

像 Tomcat 本质就是一个  Java 程序,    但是这个  Java 程序可以处理来自浏览器的 HTTP  请求



## 2.1 安装 JavaWeb 服务器

> Tomcat 官网：https://tomcat.apache.org/

下载好 apache-tomcat 后解压即可。

**启动**

1. 双击 bin 目录下的 starup.bat 即可
2. 在浏览器中输入 http://localhost:8080/ 可显示界面

> 在开发中，我们可以使用 `netstat -anb` 可查看有哪些端口监听



**启动的第二种方式**

通过 catalina 启动 Tomcat，步骤如下：

1. 进入到 Tomcat 的 bin 目录下
2. 打开 cmd，输入 `catalina  run`



**关闭 Tomcat 服务**

1. 进入 Tomcat 的 bin 目录下的 shutdown.bat，双击即可停止 Tomcat 服务器（推荐）
2. 暴力关闭服务器窗口（意外终止，不会保留日志，不推荐）





## 2.2 Tocat 目录结构

![image-20230318002408706](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230318002408706.png)

1. server.xml 用于配置 tomcat 的基本设置(启动端口，关闭端口, 主机名)
2. wex.xml    用于指定  tomcat 运行时配置(比如  servlet 等..)
3. webapps    目录是存放 web 应用，就是网站



## 2.3 Tomcat 部署 Web  应用

Tomcat 的 Web 资源都会存放到其安装目录下的一个叫做 Webapps 的目录下，里面包括 <u>html 文件、css 文件、js 文件、动态 web 页面、java 程序、支持 jar 包、配置文件</u>等。开发人员在开发 web 应用时，按照规定目录结构存放这些文件。否则，在把 web 应用交给 web 服务器管理时，不仅可能会使 web 应用无法访问，还会导致 web 服务器启动报错。JavaWeb  工程目录结构如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230318004247414.png" alt="image-20230318004247414" style="zoom:50%;" />

- **部署方式1**：将 Web 工程的目录直接拷贝到 Tomcat 的 webapps 目录下

- **部署方式2**：通过配置文件部署（利用配置文件可以将 Web 应用映射到磁盘的某个位置）

  步骤如下：

  1. 在 Tomcat 下的 conf 目录 \Catalina\localhost\ 下，定义配置文件（例如：test.xml）

     ```xml
     <?xml version="1.0" encoding="UTF-8"?>
     <!-- Context 表示一个工程上下文，path 表示工程的访问路径，docType 表示 web 工程目录位置 -->
     <!-- /test 和 test.html 文件名最好保持一致-->
     <Context path="/test" docBase="D:\test" />
     ```

  2. 在对应位置分别创建好工程文件

  3. 在浏览器中访问即可

     <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230318010343565.png" alt="image-20230318010343565" style="zoom:33%;" />

     > 访问 test/index.html 代表访问的是 D 盘下 text 文件中的 index.html 文件

**访问 Web 工程**

1.    在浏览器地址栏中输入访问地址如下：` http://ip[域名]:port`， 没有Web工程/应用名时，默认访问的是   ROOT 工程
2.    在浏览器地址栏中输入的访问地址如下： `  http://ip[域名]:port/工程名/ `   ，没有资源名， 
      默认访问 index.jsp 页面



## 2.4 浏览器请求资源过程

我们使用一张时序图来进行说明：

![image-20230318082201582](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230318082201582.png)

> HOSTS 文件位置：`C:\Windows\System32\drivers\etc`



## 2.5 IDEA 部署 Web 服务器

如何配置 Web  服务器？

1. 选择配置的 Web 目录
2. 在 Run → Edit Configurations → 添加 Tomcat 并部署即可

<center>
    <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230318094721531.png" float="left" style="width:48%">
    <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230318094802502.png" float="left" style="width:48%">
</center>

> 进行部署时，我们选择 var exploded 模式，而不要选择 war 模式，war 模式是直接将 Web 工程以包的形式上传到服务器，而 war exploded 模式则是对项目工程与我们的当前项目文件夹做映射，支持热加载

:warning:**注意事项**

1. **热加载**

   <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230318093326130.png" alt="image-20230318093326130" style="zoom:50%;" />

   - `on 'Update' action`： 表示当我们进行更新操作时 Tomcat 会自动更新类和资源(当 jsp/html 文件修改时，可以生效，但是如果你修改的 Java 文件,    需要 Redepoly 才会生效)
   - `on frame deactivation`：表示 IDEA 失去焦点（比如最小化)，也 会 导 致 jsp/html 发生更新，但是 Java 修改了，还是需要 redeploy

2. **修改端口号**

   <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230318093739836.png" alt="image-20230318093739836" style="zoom:50%;" />

   > 端口的修改不会影响到配置文件，只是当前项目的访问端口号发生改变

3. **out 目录**

   当 Tomcat 启动时，会生成 out 目录，该目录就是原项目资源的映射，我们浏览器访问的资源就是 out 目录

4. 当我们从外部拷贝资源到项目  ，如果出现 404（没有同步到 out 目录中），解决方法：<u>rebuild project → 重启 Tomcat</u>



**对整个项目的目录结构进行分析**

![image-20230318095331674](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230318095331674.png)



# 3 Servlet 

> Tomcat 8.0 版本对应的 Servlet 官方文档地址：https://tomcat.apache.org/tomcat-8.0-doc/servletapi/index.html

## 3.1 Servlet 基本介绍

我们为什么需要 Servlet？

是为了完成**动态网页技术**，即通过 Servlet  可以从 DB 中拿到数据。

下面是 Java Web 技术流图的改进，Tomcat 支持 Servlet ，既充当 Web 服务器，又充当 Servlet 容器

![image-20230318100854573](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230318100854573.png)

什么是 Servlet ？
Servlet 在开发动态 WEB 工程中，得到广泛的应用，掌握好  Servlet 非常重要了，Servlet(基石)是  SpringMVC 的基础，Servlet(Java 服务器小程序)的特点:

1.        他是由服务器端调用和执行的(一句话：是Tomcat解析和执行)
2.        他是用java语言编写的，本质就是 Java 类
3.        他是按照Servlet规范开发的(除了tomcat->Servlet weblogic->Servlet)
4.        功能强大，可以完成几乎所有的网站功能(在以前，我们老程员，使用Servlet开发网站)    技术栈要求高



## 3.2 Servlet 开发说明

Servlet 3.0 前使用 Web.xml，Servlet 3.0 版本后（包括 3.0）<font color="red">支持注解</font>，同时支持 Web.xml 配置

**快速入门**

**需求**：开发一个 HelloServlet，当浏览器访问 `http://localhost:8080/web应用名.helloServelet`  时，后台输出 "Hello，Servlet"

**实现步骤**

1. 创建好 Java Web 工程项目，并配置好 Tomcat

2. 添加 servlet-api.jar（在 tomcat/lib 下）到工程，因为 servlet-api.jar 不是 JDK 自带的，需要引入，添加到 `web/WEB-INF/lib` 下，并添加为 Library

3. 在 src 下创建 com.servlet.helloServlet 类

   ```java
   package com.servlet;
   
   import javax.servlet.*;
   import java.io.IOException;
   
   /**
    * 开发 Servlet 需要实现 Servlet 接口
    */
   public class HelloServlet implements Servlet {
       /**
        * 1.初始化servlet
        * 2. 当创建 HelloServlet 实例时，会调用 init 方法
        * 3. 该方法只会调用一次
        * @param servletConfig
        * @throws ServletException
        */
       @Override
       public void init(ServletConfig servletConfig) throws ServletException {
           System.out.println("init 被调用");
       }
   
       /**
        * 返回servletConfig对象，也就是返回servlet配置
        * @return
        */
       @Override
       public ServletConfig getServletConfig() {
           return null;
       }
   
       /**
        * 1. service 方法处理浏览器的请求（包括get/post）
        * 2. 当浏览器每次请求Servlet，就会调用一个service
        * 3. 当tomcat调用该方法时，会把http请求的数据封装成实现ServletRequest接口的request对象，通过接收ServletRequest对象，可以得到用户提交的数据
        * 4.servletResponse对象可以用于返回数据给tomcat -> 浏览器
        * @param servletRequest
        * @param servletResponse
        * @throws ServletException
        * @throws IOException
        */
       @Override
       public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
           System.out.println("Hello, Servlet");
       }
   
       /**
        * 返回servlet信息，使用较少
        * @return
        */
       @Override
       public String getServletInfo() {
           return null;
       }
   
       /**
        * 该方法在servlet被销毁时被tomcat调用，只能调用一次
        */
       @Overrid
       public void destroy() {
   
       }
   
   }
   ```

4. 在 web.xml 中配置该 web 应用使用到的 Servlet

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
       <!--配置Servlet-->
       <!--
       1. servlet-name 给Servlet取名（由程序员决定）,该名字在整个xml配置中唯一
       2. servlet-class Servlet 的全类名，Tomcat 在反射生成该Servlet时需要用到
       3. url-pattern 访问该servlet的url配置
       4. 在浏览器中访问时，应该输入http://localhost:8080/工程名/helloServlet
       -->
       <servlet>
           <servlet-name>HelloServlet</servlet-name>
           <servlet-class>com.servlet.HelloServlet</servlet-class>
       </servlet>
       <servlet-mapping>
           <servlet-name>HelloServlet</servlet-name>
           <url-pattern>/helloServlet</url-pattern>
       </servlet-mapping>
   </web-app>
   ```



## 3.3 浏览器调用 Servlet 流程分析

![image-20230318111843230](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230318111843230.png)



## 3.4 Servlet 生命周期

Servlet  主要方法：

1. `init()`：初始化阶段
2. `service()`：处理浏览器请求阶段
3. `destory()`：终止阶段

具体的流程图如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230318115522924.png" alt="image-20230318115522924" style="zoom:50%;" />

> 上图还存在不够完善的地方：当在 Tomcat 第一次创建好实例后，会将该实例放到  HashMap 中，第二次就可以直接调用该示例，而不需进行装载和创建

**特别提醒**

1. Servlet 容器（在这里是 Tomcat）启动时自动装载某些 Servlet，实现这个需要在 web.xml 文件中添加`<load-on-startup>1</load-on-startup>` ，其中 `1` 表示装载的顺序
2. 在 Servlet 容器启动后，浏览器首次向 Servlet 发送请求
3. Servlet 重新装载时（例如对 Tomcat 进行 redeploy）时会销毁所有的 Servlet 实例



**对 service 方法的解析**

1. 每接收到一个 HTTP 请求，服务器就会产生一个新的线程进行处理

   > 我们可以通过 `Thread.currentThread().getId()` 进行测试

2. 创建封装 HTTP 请求消息的 ServletRequest 对象和 HTTP 响应消息的 ServletResponse 对象

3. 然后调用 Servlet 的 `service()` 方法将响应对象和请求对象作为参数传入



**对 `destory()` 方法的解析**

当 Web 应用被终止（redeploy）或者 Servlet 容器（Tomcat）运行终止时，会重新调用 `destory()` 方法



## 3.5 GET 和 POST 请求的分发处理

**核心代码**

```java
HttpServletRequest httpServletRequest = (HttpServletRequest) servletRequest;
String method  = httpServletRequest.getMethod();
System.out.println(method);
if(method.equals("GET")) {
    doGet();
} else if(method.equals("POST")) {
    doPost();
}
```

```html
<!--测试前端页面-->
<h1>注册用户</h1>
<form action="http://localhost:8080/fish/helloServlet" method="post">
  用户名: <input type="text">
  <input type="submit" value="注册用户">
</form>
```



## 3.6 通过继承 HttpServlet 开发 Servlet 

上面我们发现，如果每次开发一个 Servlet，我们都要实现五个抽象方法，但实际上我们用到的可能就只有 `service()` 这一个方法，这样显得代码十分冗余，所以实际开发中我们使用 HttpServlet 替代 Servlet，类图如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230318150544080.png" alt="image-20230318150544080" style="zoom:50%;" />

**示例代码**

```java
public class HelloHttpServlet extends HttpServlet {
    /**
     * HttpServlet 中已经提前写好了 doGet 和 doPost 方法 ，所以我们只需要重写其方法即可
     */
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("Hi, doGet");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("Hi, doPost");
    }
}
```



## 3.7 使用 IDEA 快速开发 Servlet

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230318152353163.png" alt="image-20230318152353163" style="zoom:50%;" />

在新建一个 Servlet 之后，我们会发现，web.xml 中也多出了一个 Servlet 的配置（但 mapping 仍然需要我们自己配置）



## 3.8 Servlet 注意事项

1. Servlet 是一个供其他 Java 程序（Servlet 引擎）调用的 Java 类，不能独立运行
2. 针对浏览器的多次 Servlet 请求，通常情况下，服务器只会创建一个 Servlet 实例对象，也就是说 Servlet 实例对象一旦创建，它就会常驻内存，为后续的其他请求提供服务，直至 Web 容器退出或 redeploy 该 Web 应用，Servlet 实例对象才会被销毁。
3. 在 Servlet 的整个生命周期内，init 方法只被调用一次，而对每次请求都导致 Servlet 引擎调用一次Servlet 的 service 方法
4. 对于每次访问请求，Servlet 引擎都会创建一个新的 HttpServletRequest 请求对象和一个新的HttpServletResponse 响应对象，然后将这两个对象作为参数传递给它调用的 Servlet 的 `service()` 方法，service 方法再根据请求方式分别调用 doXXX 方法



## 3.9 Servlet 注解开发

**常规配置**

```java
@WebServlet(name = "OKServlet", value = "/OKServlet")	// 即servlet-name = "OKServlet",url-pattern = "/OKServllet"
public class OKServlet extends HttpServlet {}
```



**可配置项**

```java
public @interface WebServlet {
    String name() default "";

    String[] value() default {};

    String[] urlPatterns() default {};

    int loadOnStartup() default -1;

    WebInitParam[] initParams() default {};

    boolean asyncSupported() default false;

    String smallIcon() default "";

    String largeIcon() default "";

    String description() default "";

    String displayName() default "";
}
```



**匹配模式**

**01 精确匹配**

```java
@WebServlet("/ok/zs")
```

访问servlet： 

- `localhost:8080/servlet/ok/zs`

**02 目录匹配**

```java
@WebServlet("/ok/*")
```

访问文件: 

- `localhost:8080/servlet/ok/aaa`        
- `localhost:8080/servlet/ok/bbb`



**03 扩展名匹配**

```java
@WebServlet("*.action")
```

> 注意：不能写成 `@WebServlet("/*.action")`，即不能带 `/ `, 否则 tomcat 报错



**04 任意匹配**

```java
@WebServlet("/")        
// 或者
@WebServlet("/*")
```

> 注意：`/` 和 `/*` 的配置会匹配所有的请求，我们需要尽量避免



:warning:**注意事项**

1. 不建议使用**任意匹配**，建议尽量使用**精确匹配**

2. Tomcat 有一个 DefaultServlet，当其他的 url-pattern 都匹配不上时，会走这个 Servlet，返回默认的静态资源

   ![image-20230318164944247](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230318164944247.png)

3. 匹配优先级：精确匹配 > 目录匹配 > 扩展名匹配 > `/*` > `/` > default



# 4 HTTP 协议

注意：接下来，我们可能经常用到浏览器的网络监视台进行抓包

>打开浏览器网络的快捷键： Ctrl + Shift + I

问：一个 HTML 页面中有 2 张图片，问浏览器会发出几次 HTTP 请求？

答：3次，过程分析如下：

1. 第一次请求 HTML 页面
2. 当浏览器在解析过程中发现 HTML 页面中有 `<img src="pic1.png">` 和 `<img src="pic2.png">`
3. 会继续向服务器发送请求，要 pic1.png 和 pic2.png



## 4.1 GET 请求返回数据

```java
/**
 * 为了让浏览器显示中文，需要告诉浏览器我们的编码格式为UTF-8
 * text/html 是 MIME(多用途因特网邮件协议扩展，用于扩展除ASCII字符外的其他字符和二进制无法正常在页面中显示的问题)，为 text 下的 html 格式数据
 */
response.setContentType("text/html;charset=utf-8");
PrintWriter writer = response.getWriter();
writer.print("<h1>登录成功</h1>");
// 为了确保数据返回,可以添加如下代码
writer.flush();
writer.close();
```

**对 HTTP GET 请求包分析**

![image-20230318211859863](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230318211859863.png)



## 4.2 POST 请求

**对 POST 请求包进行分析**

![image-20230318212933521](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230318212933521.png)

> 注意区分 host 和 origin：
>
> - host 表示发送的目的主机
> - origin 表示发送的源主机

<strong style="color:red">POST 请求报文除了请求行、请求头外还有请求体，表单发送的数据使永 url 编码存放到请求体中</strong>



## 4.3 总结 POST 和 GET 请求有哪些

GET 请求的类别如下：

1. from 指定 `method = get`
2. a 标签
3. link 标签引入 css
4. script 标签引入 js
5. img 标签引入图片
6. iframe 引入  html 页面
7. 在浏览器地址栏中输入地址后回车

POST 请求类别如下：

1. from 指定 `method = post`



**什么使用 GET 请求，什么时候使用 POST 请求?**

在大部分情况下，我们是不需要考虑这个问题的，因为业务本身就会自动区别，例如<u>显示图片时，引入 CSS / JS 时，默认就是 GET 请求，当你需要登录 、发帖、上传文件时，就会使用 POST</u> 

POST 和 GET 请求区别如下：

1. GET 传送的数据量较小，不能大于 2 KB（不同浏览器存在区别）；POST 传送的数据量大，一般默认不受限制

2. GET 安全性较 POST 差，所以对于查询操作一般使用 GET，而对于 增删改 操作，一般使用 POST

   > 在用户登录注册向服务器传送数据时使用 POST 请求，在做分页、前台展示、页面共享等时使用 GET 请求



## 4.4 HTTP 响应报文

![image-20230318221354849](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230318221354849.png)



## 4.5 常见状态码说明

对于 **302 资源重定向**的说明示例

```java
@Override
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    // 重定向到baidu.com页面
    /**
     * (1) 返回 302 状态码
     * (2) 返回响应头 location: https://baidu.com
     */
    response.sendRedirect("https://baidu.com");
}

@Override
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    // 对 doPost 和 doGet 合并处理
    doGet(request, response);
}
```



对于 **304 请求资源未修改**

该状态的生成取决于浏览器的缓存机制、if-modified-since、last-modified 字段，当所请求资源未修改时，服务器返回该状态码，并且不会返回任何资源。

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230318224938073.png" alt="image-20230318224938073" style="zoom:50%;" />



## 4.6 MIME 介绍

MIME 是 HTTP 协议中的数据字段，中文名为 "多功能邮件扩展协议"。MIME 类型的格式是 "大类型 / 小类型"，并与某一种文件的扩展名相对应。

在响应包中的 Content-Type 中就有 MIME 类型，如下：

![image-20230318225307749](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230318225307749.png)

**常见的MIME 类型**

![image-20230318225333953](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230318225333953.png)



# 5 Servlet 相关对象 

## 5.1 ServletConfig

1. ServletConfig 类是为 Servlet 程序的配置信息的类
2. Servlet 程序和 ServletConfig 对象都是由 Tomcat 负责创建的
3. Servlet 程序默认是第一次访问的时候创建，ServletConfig 在 Servlet 程序创建时，就创建一个对应的 ServletConfig 对象

**ServletConfig 的作用**

1. 获取 Servlet 程序的 servlet-name 的值
2. 获取初始化参数 init-param
3. 获取 ServletContext 对象

## 5.2 获取 ServletConfig

在 HttpServlet 的父类 GenericServlet 中，有一个方法 `getServletConfig()` 用于获取 Servlet 配置信息 

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230318232042479.png" alt="image-20230318232042479" style="zoom:50%;" />

**测试**

在 web.xml 中配置相关信息

```xml
<!--配置信息，而不是硬编码到程序中-->
<init-param>
    <param-name>userName</param-name>
    <param-value>Alice</param-value>
</init-param>
<init-param>
    <param-name>password</param-name>
    <param-value>123</param-value>
</init-param>
```

Servlet 中获取配置信息

```java
ServletConfig servletConfig = getServletConfig();
String userName = servletConfig.getInitParameter("userName");
String pwd = servletConfig.getInitParameter("password");
System.out.println(userName);
System.out.println(pwd);
```



在 Servlet 中 init 方法也可以获取 config 对象，如下：

```java
public void init(ServletConfig config) throws ServletException {
    System.out.println("@" + config);	// 该config对象与我们通过getServletConfig是同一个对象
    super.init(config);
}
```

**注意**

这里在初始化时，必须要加载 config，否则后续的 Service 方法中无法正确得到 config 对象，我们可以看到 `super.init()` 源码如下：

```java
public void init(ServletConfig config) throws ServletException {
    this.config = config;
    this.init();
}
```

如果我们在Servlet 的 `init()` 方法中调用了 `super.init()`，但没有传入 config 配置对象，那么 `GenericServlet` 的成员变量 `private transient ServletConfig config` 中 config 就会为空，后续操作就更不用说，都为空。



## 5.3 ServletContext

1. ServletContext 是一个接口，表示 Servlet 上下文对象

2. 一个 Web 工程，只能有一个 ServletContext 对象实例

3. ServletContext 对象，是在 Web 工程启动的时候创建的，在 Web  工程停止时销毁

4. ServletContext 对象可以通过 `getServletConfig().getServletContext()`，也可以直接通过 `getServletContext()` 获取因为其底层源码如下：

   ```java
   public ServletContext getServletContext() {
       return this.getServletConfig().getServletContext();
   }
   ```

5. 由于一个 Web 应用中所有的 Servlet 共享一个 ServletContext 对象，因此 Servlet 对象之间也可以通过 ServletContext 对象来实现对象之间的通信。ServlletContext 对象通常也被称之为**域对象**

   <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230319001539925.png" alt="image-20230319001539925" style="zoom:50%;" />



**ServletContext 用途**

1.    获取 web.xml 中配置的上下文参数 context-param（信息和整个  web  应用相关，而不是属于某个  Servlet）
2.    获取当前的工程路径，格式: /工程路径  => 比如：/servlet
3.    获取工程部署后在服务器在磁盘上的绝对路径 (例如D:\hspedu_javaweb\servlet\out\artifacts\servlet_war_exploded)
4.    像 Map 一样存取数据，多个 Servlet 共享数据

**示例**

```java
// 1. 获取上下文参数
ServletContext servletContext = getServletContext();
String website = servletContext.getInitParameter("website");
System.out.println("website=" + website);

// 2. 获取当前 web 项目的工程路径
String project_root = servletContext.getContextPath();

// 3. 获取项目发布后真正的工作路径
String deploy_root = servletContext.getRealPath("/");
```

```java
// 4. 网站访问计数器
ServletContext servletContext = getServletContext();
Object visitCount = servletContext.getAttribute("visit_count");
if(visitCount == null) {
    visitCount = 1;
} else {
    visitCount = Integer.parseInt(visitCount + "") + 1;
}
// 放回visitCount
servletContext.setAttribute("visit_count", visitCount);
response.setContentType("text/html; charset=utf8");
PrintWriter writer = response.getWriter();
writer.print("网站访问次数为: " + visitCount);
writer.flush();
writer.close();
```



## 5.4 HttpServletRequest

1. HttpServletRequest 对象代表客户端的请求
2. 当客户端 / 浏览器通过 HTTP 协议访问服务器时，HTTP 请求头中所有信息都封装到该对象中
3. 通过该对象的方法，可以获得客户这些信息

 **常见方法**

1. `getRequestURI()`：获取请求的资源路径

2. `getRequestURL()`：获取请求的统一资源定位符（绝对路径）

   > 例如：对于 `http://localhost:8080/servlet/loginServlet` ，URI 获取的就是 `/servlet/loginServlet` 部分,URL 获取的就是完整的部分

3. `getRemoteHost()`：获取客户端的主机

   > `getRmoteAddr()` 能够获取到客户端的 IP 地址

4. `getHeader()`：获取请求头

5. `getParameter()`：获取请求的参数

6. `getParameterValues()`：获取请求的参数（多个值的时候使用）

7. `getMethod()`：获取请求的方式是 GET 还是 POST

8. `setAttribute(key, value)`：设置域数据

9. <strong style="color:red">`getRequestDispatcher()`</strong>：获取请求转发对象

**示例**

```java
String requestURI = request.getRequestURI();    // 获取请求资源的URI
System.out.println("URI=" + requestURI);

StringBuffer requestURL = request.getRequestURL();  // 获取请求资源的URL
System.out.println("URL=" + requestURL);

String remoteHost = request.getRemoteHost();    // 得到请求的客户端的主机
System.out.println("HOST=" +  remoteHost);  // 127.0.0.1
System.out.println("RemoteAddr=" + request.getRemoteAddr());    // 127.0.0.1

String headerHost = request.getHeader("Host");    // 获取请求头数据中的Host
System.out.println("Host=" + headerHost);   // localhost:8080
/**
 * 我们通过 getHeader 方法可以得到请求头中的任何数据
 */
// 获取客户机所使用的浏览器
String[] split = request.getHeader("User-Agent").split(" ");
System.out.println("browser=" + split[split.length-1].split("\\/")[0]);

/**
 * 获取表单数据
 */
String userName = request.getParameter("userName"); // 获取用户名
String password = request.getParameter("password"); // 获取密码
// 获取复选框中的多选数据
String[] hobbies = request.getParameterValues("hobby"); // 获取爱好信息
for (String s : hobbies) {
    System.out.println(s);
}
```

**对应的 HTML 页面**

```html
<form action="http://localhost:8080/fish/Register" method="post">
  用户名: <input type="text" name="useName"><br>
  密码: <input type="password" name="password"><br>
  爱好: <input type="checkbox" name="hobby" value="eating">吃饭
  <input type="checkbox" name="hobby" value="sleeping">睡觉<br>
  <input type="submit" value="提交">
</form>
```



:alarm_clock:**注意事项**

1. 解决接收参数乱码问题，只在接收数据前添加下面一段话即可：

   ```java
   request.setCharacterEncoding("utf-8");
   ```

2. 处理响应数据中文乱码问题，在写回数据前添加如下代码：

   ```java
   response.setContentType("text/html; charset=utf-8");
   // 本质就是在HTTP响应头前添加Content-Type
   ```



## 5.5 HttpServletResponse

1. 每次 HTTP 请求，Tomcat  会创建一个 HttpServletResponse 对象传递给 Servllet 程序使用
2. HttpServletRequest 表示请求过来的信息，HttpServletResponse 表示所有响应的信息，如果要设置返回给客户端的信息，通过 HttpServletResponse 对象来设置即可

**常见方法**

![image-20230319140606081](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230319140606081.png)

除此之外，还有：

- `getOutPutStream()`：常用与下载、处理二进制数据文件
- `getWriter()`：常用与回传字符串

> 两个流只能使用一个







# 6 请求转发

我们为什么需要请求转发？

1. 目前我们学习的都是一次请求，对应一个 Servlet，如下：

   ![image-20230319105836885](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230319105836885.png)

2. 但是在实际开发中，往往业务比较复杂，需要在一次请求中，使用到多个 Servlet 完成一个任务（Servlet 链，流水作业）

   ![image-20230319105935229](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230319105935229.png)

## 6.1 请求转发说明

1. 实现请求转发：请求转发指一个 web 资源收到客户端请求后，通知服务器去调用另一个 web 资源进行处理
2. HttpServletRequest 对象（也叫作 Request 对象）提供了一个 `getRequestDispatcher()` 方法，该方法返回一个 RequestDispatcher 对象，调用该对象的 `forward()` 方法可以实现请求转发
3. request 对象同时也是一个域对象，开发人员通过 request 对象实现转发时，把数据通过 request 对象带给其他 web 资源处理
   - setAttribute 方法
   - getAttribute 方法
   - removeAttribute 方法
   - getAttributeNames 方法

请求转发示意图如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230319111451447.png" alt="image-20230319111451447" style="zoom:33%;" />

> 分组转发过程在图上属于简略画法，实际上，转发调用仍然是需要 Tomcat 参与



## 6.2 转发实例

**示例代码**

```java
// SourceServlet.java
String userName = request.getParameter("userName");
if("tom".equals(userName)) {    // 这里之所以不写成 userName.equals("tom")是为了防止抛出空指针异常
    request.setAttribute("role", "管理员");
} else {
    request.setAttribute("role", "普通用户");
}
/**
 * 进行分发
 */
// 1. 获取分发器，注意传入的url在本地最终会被解析为http://localhost:8080/[Application Context]/Next
RequestDispatcher requestDispatcher = request.getRequestDispatcher("/Next");
// 2. 通过forward将request和response传递给下一个servlet
requestDispatcher.forward(request, response);
```

```java
// NextServlet
String userName = request.getParameter("userName");
String role = (String)request.getAttribute("role");

response.setContentType("text/html; charset=utf-8");
PrintWriter writer = response.getWriter();
writer.print(userName + "是"+ role);
writer.flush();
writer.close();
```



## 6.2 分组转发注意事项

1. 浏览器地址不会发生变化（地址仍然保留在第一个 Servlet 的 url）
2. 在同一次 HTTP 请求中，进行多次转发，仍然是一次 HTTP 请求
3. 在同一次 HTTP 请求中，进行多次转发，多个 Servlet 可以共享给 request 域 / 对象的数据
4. 可以转发到 WEB-INF 目录下
5. 不能访问除当前 WEB 工程外的资源
6. <strong style="color:red">因为浏览器地址栏会停在第一个 Servlet，如果刷新页面，会再次发送请求，所以在支付业务场景下会造成重复支付</strong>，解决方法可以使用重定向或在数据库中进行业务判断



# 7 请求重定向

请求重定向：一个 web 资源收到客户端请求后，通知客户端去访问另一个 web 资源，这称之为请求重定向。

示意图如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230319142527231.png" alt="image-20230319142527231" style="zoom:50%;" />

> - 除了第一次的 HTTP 请求外，后面在重定向过程中，浏览器也参与其中
> - 在第二次浏览器发出请求后，浏览器搜索框的 url 地址会变成返回重定向的 location

重定向实现方法很简单，只需要在 Servlet 业务中使用 response 对象调用 sendRedirect 即可：

```java
/**
 * sendRedirect 本质就会返回 302 状态码和 location
 * 并且 302 和 location 是由浏览器解析的，而不是服务器
 * 所以重定向需要带上完整路径，而转发只需要添加相对 application context 的路径即可
 * /servlet/next 在返回浏览器时会被解析为 http://localhost:8080/servlet/next
 */
response.sendRedirect("/servlet/next");
```

**注意事项**

1. 重定向的最佳应用场景：网站迁移

2. 浏览器地址会发生变化，本质是两次 HTTP 请求

3. 不能共享 Request 域中的数据

4. 不能重定向到 /WEB-INF 下的资源

5. 可以重定向到 Web 工程以外的资源

6. 重定向还可以像下面这么写：

   ```java
   response.setStatus(302);
   response.setHeader("Location", "/servlet/next");
   ```



## 7.1 动态获取 Application Context

之前的重定向由于是绝对路径，所以每次如果 Application Context 发生变化，我们都需要手动修改，所以我们需要动态拼接返回 location

```java
String contextPath = getServletContext().getContextPath();  // 获取 Application Context
response.sendRedirect(contextPath + "/next");
```





