[toc]

# 1 Web 工程路径

在 Web 工程中，路径是一个比较麻烦的问题，例如：如果表单提交的 url 全部都使用绝对路径，如 `http://localhost:8080/xxx/xxx` 就会非常难受，所以我们需要一个方法来解决这一问题。



## 1.1 相对路径解决访问路径问题

一个非常重要的规则：页面所有相对路径，在默认情况下，都会参考当前浏览器地址栏的路径 <strong style="color:orange">`http://ip:port/工程名/`</strong> + <strong style="color:red">资源</strong>，所以如果我们上次访问了 `http://localhost:63342/web_project/web/register.html` （表单注册页面），提交数据后，如果 `action` 字段的值为 `ok`，那么页面就会直接跳转到 `localhost:63342/web_project/web/ok`

所以这就会带来一个问题，如下：

![image-20230319154859814](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230319154859814.png)

所以，我们希望项目工程中所有资源都可以通过 `http://ip:host/[web工程根目录]` 来进行访问，解决方法：<strong style="color:red">base 标签</strong>



## 1.2 base 标签

- base 标签是 HTML 语言中**基准网址标记**，它是一个单标签，位于网页头部文件的 head 标签内
- 一个页面最多只能使用一个 base 元素，用来提供一个指定的默认目标，是一种表达路径和连接网址的标记
- 常见的 url 路径形式分别由相对路径和绝对路径，如果 base 标签指定了目标，浏览器将通过这个目标来解析当前文档中所有的相对路径，包括的标签的有：`a`、`img`、`link`、`form`

> 我们可以理解为：base 标签将工程中所有的资源由相对路径转换为绝对路径（相较完整的绝对路径更加简洁）

**示例**

我们在 head 标签中添加如下配置：

```html
<base href="http://localhost:8080/webpath/">
```

那么，如果页面中使用了绝对路径，例如 `a.html` 就会被解析为 

`http://localhost:8080/webpath/a.html` ，除此之后上面还可以简写为：

```html
<base href="/webpath/">
```

原因：**`base` 标签是由浏览器解析的，当浏览器遇到 `/` 后，会将其解析为 `http://localhost:8080/`**



## 1.3 服务器端转发定位资源

```java
/**
 * 1. 在服务器端，解析第一个 "/" 时，会被解析为 http:ip:port/[Application context]
 * 2. 也就是说 /a/b/b.html => 被解析为http://ip:port/[Application context]/a/b/b.html
 * 3. 其实如果我们写成 a/b/b.html 也可以被正常解析
 * 4. 因为如果没有 "/"，服务器就按照默认的方式参考定位 http://ip:port/[Application context]/
 */
request.getRequestDispatcher("/a/b/b.html").forward(request, response);
```



:warning:**注意事项**

1. 在实际开发中，路径都适用绝对路径，而不是用相对路径
2. 在 web 中，`/` 如果被浏览器解析，转换的地址为 `http://ip[域名]:port/`
3. 在 web 中，`/` 如果被服务器解析，转换的地址为：`http://ip[域名]:port/工程路径/`

**示例**

```xml
<url-pattern>/servlet</url-pattern>
```

```java
request.getRequestDispathcher("/");
String realPath = getServletContext().getRealPath("/");
// 以我的工程为例
// C:\Users\Administrator\Desktop\web_project\out\artifacts\web_project_war_exploded\
// 可以最终得到的是工程在发布之后的文件路径
```

```java
String contextPath = getServletContext().getContextPath();
String contextPath = request.getContextPath();
// 得到的contextPath可以用于动态获取Application context
```

4. 在 JavaWeb 中，路径最后带 `/` 和不带 `/` 含义不同：
   - `<a href="/a/servlet">网址</a>`：servlet 表示资源
   - `<a href="/a/servlet/">网址</a>`：servlet 表示路径

5. 对于重定向 `response.sendRedirect("/")` 这条语句虽然是在服务器执行的，但是，服务器是把 `/` 发送给浏览器解析的



## 1.4 优化 WEB 工程路径

之前，HTML 页面中 base 标签都是硬编码（"写死的"），如果后来我们的 Application context 发生变化，那么所有与该路径有关的资源 url 都无法正常获取，所以我们需要动态获取该路径并进行拼接

这里以 JSP（或 thymeleaf 也可以）为例，直接添加：`<%=request.getContextPath()%>` 即可



# 2 会话技术

**会话**：会话可以简单理解为一个**信息交互**的过程，用户打开一个浏览器窗口，点击多个超链接，访问服务器多个 web 资源，然后关闭浏览器，整个过程称之为一个会话

会话过程中需要解决的**问题**：每个用户在使用浏览器与服务器进行会话的过程中，不可避免地会产生一些数据，服务器要想办法为每个用户保存这些数据。

会话的两种技术：

- Session
- Cookie



# 3 Cookie

cookie 是 <u>servlet 发送到 Web 浏览器的少量信息，这些信息由浏览器保存，然后发送回服务器</u>。cookie 的值可以唯一地标识客户端，因此  cookie 常用于**会话管理**。一个 cookie 拥有一个名称、一个值和一些可选属性，比如<u>注释、路径和域限定符、最大生存时间和版本号</u>。

Cookie 的简单示意图：

![image-20230319203249472](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230319203249472.png)

1. Cookie 是服务器在客户端保存用户的信息，比如登录名、浏览历史等就可以使用 Cookie 方式保存

2. Cookie 信息就像是 "小甜饼" 一样，数据量不大，服务器端在需要的时候可以从客户端 / 浏览器读取（HTTP 协议）

   <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230319204746000.png" alt="image-20230319204746000" style="zoom:50%;" />

3. 最后说明：Cookie 数据是保存在浏览器上的



Cookie 的作用：

1. 保存上次登录的时间等信息
2. 保存用户名、密码，在一定时间内不用重新登录
3. 网站的个性化，例如定制网站的服务和内容



## 3.1 Cookie 的常见方法

1. Cookie 的构造方法：

   ```java
   Cookie(String name, String value) 
   ```

   > Cookie 的存储机制也是一组键值对

2. 将 Cookie 添加到客户端

   ```java
   response.addCookie(c)
   ```

3. 读取 Cookie

   ```java
   request.getCookies()
   ```

4. 设置 Cookie 的保存时间

   ```java
   c.setMaxAge()
   ```



## 3.2 创建 Cookie 及底层机制

我们以下面代码为例：

```java
// 创建cookie对象
Cookie cookie = new Cookie("k1", "v1");

response.setContentType("text/html; charset=utf-8");
// 把从服务器创建的cookie对象返回给浏览器
response.addCookie(cookie);

PrintWriter writer = response.getWriter();
writer.println("<h1>创建cookie成功</h1>");

writer.flush();
writer.close();
```

访问 Servlet，在浏览器中进行抓包：

![image-20230319213024275](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230319213024275.png)

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230319213305397.png" alt="image-20230319213305397" style="zoom:50%;" />



## 3.3 读取 Cookie 及底层机制

```java
Cookie[] cookies = request.getCookies();
if(cookies != null && cookies.length != 0) {
    for (Cookie cookie : cookies) {
        System.out.println("name = " + cookie.getName() + ", value = " + cookie.getValue());
    }
}
```

同时我们进行抓包查看：

![image-20230319215416639](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230319215416639.png)

 

## 3.4 JSESSIONID 说明

JSEESIONID 用于作为服务器与浏览器之间会话的标识，其本质是一个 Cookie，需要注意的是：只有首次访问服务器的时候才会产生 JSESSIONID 

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230319231809695.png" alt="image-20230319231809695" style="zoom:50%;" />

> JSEESIONID 会建立在每一个服务器与浏览器之间的会话，也就是说后续我们访问该服务器其他页面时不会再产生 JSEESIONID，除非另开一个浏览器或重启服务器



## 3.5 对Cookie 的CRUD

Cookie 类中没有获得指定 Cookie 的方法，所以我们需要自己手写一个工具类，用来获取指定Cookie

```java
public class CookieUtils {
    public static Cookie getCookieByName(String name, Cookie[] cookies) {
        if(name == null ||  "".equals(name) || cookies == null || cookies.length == 0) {
            return null;
        }
        for (Cookie cookie : cookies) {
            if(name.equals(cookie.getName())) {
                return cookie;
            }
        }
        return null;
    }
}
```



如果我们想要修改 Cookie 流程如下：

1. 先查找到需要修改的 Cookie 对象
2. 调用 `setValue()` 方法赋予新的 Cookie 值
3. 调用 `response.addCookie()` 通知客户端保存修改

如果我们创建了一个同名的 cookie，则相当于覆盖原来的 cookie



## 3.6 Cookie 的生命周期

Cookie 的生命周期是指 Cookie 从创建到销毁的整个流程，我们在之前已经介绍过 Cookie 的创建过程，接下来，我们会对 Cookie 在什么时候销毁做说明：

Cookie 类中有一个私有属性 `MaxAge`，该字段用于指明 Cookie 的最大生存时间，默认值是 -1

```java
private int maxAge = -1;
```

同时在 Cookie 类中也有一个 `setMaxAge(int expiry)` 方法，以秒为单位

- 正值表示该 Cookie 在经过指定时间后过期

  > 注意：该值是 cookie 过期的最大生存时间，而不是当前生存时间

- 负值表示 cookie 并不会被持久存储，将在 Web 浏览器退出时删除

  > expire 在浏览器中被设置为 "session"，即当前会话，当我们退出会话，cookie 将被浏览器删除

- 0 值会导致删除 cookie

> 注意：区分 "过期" 和 "删除" ，"过期" 的 cookie 在浏览器中并不会立马被删除，仅是指浏览器在发送 HTTP 请求的时候不会将过期的 cookie 携带待请求报文中；而当我们将 cookie 的 `MaxAge` 值设置为 0 时，该 cookie 会立即被删除

当我们将 cookie 的 `MaxAge` 设置为 0，并且通过 `response.addCookie()` 将该 cookie 作为响应返回时，我们进行抓包处理，会发现：

![image-20230320002609439](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230320002609439.png)

浏览器自动将其有效期设置为 1970 年，显然早就过期了



## 3.7 Cookie  的有效路径

Cookie 中有一个 path 属性，用来过滤哪些 cookie 可以发送到服务器，哪些不可以发送到服务器。该属性通过请求的地址来进行过滤

**示例**

```
cookie1.setPath = /工程路径
cookie2.setPath = /工程路径/aaa

请求地址: http://ip:端口/工程路径/资源 
cookie1   会发给服务器
cookie2   不会发给服务器

请求地址: http://ip:端口/工程路径/aaa/资源 
cookie1   会发给服务器
cookie2   会发给服务器
```



```java
Cookie cookie1 = new Cookie("salary", "3000");
Cookie cookie2 = new Cookie("gender", "male");
cookie1.setPath(request.getContextPath() + "/aaa");	// 只能访问请求包含 /web_project/aaa 的资源 
cookie2.setPath(request.getContextPath());
response.addCookie(cookie1);
response.addCookie(cookie2);
```

> cookie 设置的默认路径是当前 web工程的路径



## 3.8 cookie 的注意事项

1. 一个 cookie 只能标识一种信息
2. 一个 web 站点可以给一个浏览器发送多个 cookie，一个浏览器也可以存储多个 web 站点提供的 cookie
3. cookie 的总数没有限制，但是每个域名的 cookie 数量和大小有限制，cookie 不适合存放数据量大的数据
4. 删除 cookie 时，path 必须保持一致，否则无法删除成功
5. cookie 中是不能够存储中文字符的，但是我们可以通过 `URLEncoder` 工具类将其转换为 UTF-8 编码后存储

**示例**

当我们直接使用 cookie 保存中文字符，会出现下面的情况：

![image-20230320091555729](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230320091555729.png)

**解决方案**

```java
// 先使用URLEncoder将中文字符转码为utf-8
String baidu = URLEncoder.encode("百度", "utf8");
Cookie cookie = new Cookie("company", baidu);
response.addCookie(cookie);
```

```java
// 解码就可以直接使用 URLDecoder 工具类进行解码
Cookie[] cookies = request.getCookies();
Cookie company = CookieUtils.getCookieByName("company", cookies);	// CookieUtils 为自己封装的工具类
System.out.println(URLDecoder.decode(company.getValue(), "utf8"));
```



# 4 Session

session 是服务器端技术，服务器在运行时为每一个用户的浏览器创建一个独享的 session 对象 / 集合。

由于 session 为各个**用户浏览器**独享，所以用户在访问服务器的不同页面时 ，可以从各自的 session 中读取 / 添加数据，从而完成相应任务。



**用途**

1. 网上商城中的购物车
2. 保存登录用户的信息
3. 将数据放入到 session 中，供用户在访问不同页面时，实现跨页面访问数据
4. 防止用户非法登录到某个页面



## 4.1 session 原理图

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230320103309907.png" alt="image-20230320103309907" style="zoom: 50%;" />

1. 当用户浏览器，访问某个网站时，操作 session 时，服务器就会在内存（在服务端中）为每一个用户浏览器创建一个独立的 session 对象，该 session 对象被浏览器独占

2. 该 session 对象也可以看做是一个容器 / 集合，默认的存活时间是 30 min（在 Tomcat 的 web.xml 配置中可以看到）

   ```xml
   <session-config>
       <session-timeout>30</session-timeout>
   </session-config>
   ```

3. 服务器通过浏览器客户端的 JSESSIONID 来确定为浏览器分配那一块内存



## 4.2 session 的存储结构

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230320104844596.png" alt="image-20230320104844596" style="zoom:50%;" />

1. session 看做容器类似于 HashMap，有两列（K-V），每一行就是 session 的一个属性
2. 每个属性包含有两个部分，一个是该属性的名字（String），另一个是它的值（Object）



## 4.3 session 常用方法

1. 创建和获取 session

   ```java
   request.getSeesion();
   // Returns the current session associated with this request, or if the request does not have a session, creates one.
   // 第一次是创建 session,后面就会直接返回session
   ```

2. 向 session 中添加属性

   ```java
   hs.setAttribute(String name, Object value)
   ```

3. 从 session 中得到某个属性

   ```java
   hs.getAttribute(String name)
   ```

4. 从 session 中删除某个属性

   ```java
   hs.removeAttribute(String name)
   ```

5. 判断是否是刚创建出来的 session

   ```java
   isNew()
   ```

6. 获取每个 session 的唯一标识 id 的值

   ```java
   getId()
   ```



## 4.4 session 底层机制

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230320125337660.png" alt="image-20230320125337660" style="zoom:50%;" />

> 当在首次会话中创建 session ，后续将不再返回 Set-Cookie 响应头；另外，如果根据之前残留 JESSIONID 在内存中查询不到分配内存，分配 JESSIONID 并返回创建关联的 session 对象后，也会返回包含新的 JESSIONID 的 Set-Cookie 字段



## 4.5 session 的生命周期

1. `public void setMaxInactiveInterval(int interval)` 用于设置 session  的超时时间（以秒为单位），超过指定时长，session  就会被销毁

   - 值为正数，表示 session 的超时时间
   - 值为负数，表示 session 永不过期

2. `public int getMaxInactiveInterval()` 返回 servlet 容器的在客户端访问的会话保持开启的最大时间间隔 

   - 正数表示超时时间，以秒为单位
   - 负数表示永不过期

3. ` public void invalidate()` 使会话失效

5. 如果没有调用 `setMaxInactiveInterval()` 来指定 session 的生命时长，Tomcat 会以 session 默认时长为准，默认的超时为 30 分钟，同样在 Tomcat 的 web.xml 可以看到

   ```xml
   <session-config>
       <session-timeout>30</session-timeout>
   </session-config>
   ```

6. session 的生命周期指的是：<strong style="color:red">客户端 / 浏览器两次请求最长间隔时长，而不是累积时长</strong>，即当客户端访问了自己的 session ，session 的生命周期将从 0 开始重新计算

7. 底层：Tomcat 用一个线程来轮询会话状态，如果某个会话的空闲时间超过设定的最大值，则该会话将会被销毁

**测试程序**

```java
/* CreateSession */
// 创建 session
HttpSession session = request.getSession();
// 得到 session id
System.out.println("当前会话的session id = " + session.getId());
// 设置 session 的生命周期为 60s
session.setMaxInactiveInterval(60);
// 添加属性到 session 中用作测试使用
session.setAttribute("name", "jack");
```

```java
/* ReadSession */
HttpSession session = request.getSession();
System.out.println("当前会话的session id = " + session.getId());
// 得到 CreateSession 中设置的属性
Object name = session.getAttribute("name");
if(name != null) {
    System.out.println("读取到的属性值为：" + name);
} else {
    System.out.println("session 被销毁");
}
```

> 如果我们想要使当前 session 失效可以使用 `invalidate()` 方法；如果想要删除 session 中的属性可以使用 `removeAttribute()`



# 5 防止非法进入管理页面

要求：

1. 用户通过 login.html 进行登录
2. 输入正确的用户名和密码（Alice && 666666），登录成功，Manager.java 对页面进行动态渲染；否则登录失败，转发到 error.html
3. 用户如果直接通过网址登录访问 Manager，则会被重定向到 login.html 页面

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230320183518977.png" alt="image-20230320183518977" style="zoom: 50%;" />

核心代码如下：

```java
// loginCheck
String userName = request.getParameter("userName");
String pwd = request.getParameter("pwd");
if("Alice".equals(userName) && "666666".equals(pwd)) {
    HttpSession session = request.getSession();
    session.setAttribute("loginUser", userName);

    request.getRequestDispatcher("/ManageServlet").forward(request, response);
} else {
    request.getRequestDispatcher("/error.html").forward(request, response);
}
```

```java
// ManagerServlet
HttpSession session = request.getSession();
if(session.getAttribute("loginUser") == null) {
response.sendRedirect("/web_project/login.html");
} else {
response.setContentType("text/html;charset=utf8");
PrintWriter writer = response.getWriter();
writer.println("<h1>欢迎来到登录管理页面</h1>");
writer.println("<h2>欢迎您，管理员" + session.getAttribute("loginUser") + "</h2>");

writer.flush();
writer.close();
```

