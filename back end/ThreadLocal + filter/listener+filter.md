[toc]

# 1 Listener

> Listener 在Java EE 文档位于 javax.servlet 下 

## 1.1 Listener 监听器介绍

1. Listener 监听是 JavaWeb 的三大组件之一。JavaWeb 的三大组件分别是：<u>Servlet 程序、Listener 监听器、Filter 过滤器</u>
2. Listener 是 JavaEE 的规范，就是接口
3. 监听器的作用：<strong style="color:red">监听某种变化，触发对应方法完成相应的任务</strong>

4. JavaWeb 中的监听器共有 8 个，目前最常用的是 ServletContextListener



## 1.2 ServletContextListener

1. 作用：监听 ServletContext 创建或销毁，即生命周期

   > 当我们 Web 应用启动时，就会创建 ServletContext

2. 应用场景：

   - 加载初始化的配置文件，例如 Spring 的配置文件
   - 任务调度（配合定时器 Timer / TimerTask）

3. 相关方法

   - `void contextInitialized(ServletContextEvent sce)`：创建 ServletContext 时触发
   - `void contextDestroyed(ServletContextEvent sce)`：销毁 ServletContext 时触发

示例如下：

```java
import javax.servlet.ServletContext;
import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;

/**
 * 1. 当一个类实现了 ServletContextListener 该类就是一个监听器
 * 2. 该类可以监听的事件由该类实现的接口决定，例如实现了 ServletContextListener 的类可以监听 ServletContext 对象的创建和销毁；实现了 HttpServletContextListener 的类可以监听 HttpServletContext 的创建和销毁
 * 3. 这里的 MyServletContextListener 就是一个监听者
 * 4. 当 web 应用启动时就会产生 ServletContextEvent 事件，会调用监听器对应的事件处理方法 contextInitialized ，同时传递事件对象
 * 5. 程序员拿到 ServletContextEvent 后就可以进行一些处理操作
 * 6. Tomcat 如何知道监听者的存在？需要我们在 web.xml 中进行配置
 */
public class MyServletContextListener implements ServletContextListener {
    @Override
    public void contextInitialized(ServletContextEvent servletContextEvent) {
        // 拿到马上创建的 servletContext
        ServletContext servletContext = servletContextEvent.getServletContext();
        System.out.println("MyServletContextListener 监听到" + servletContext + "被创建");


    }

    @Override
    public void contextDestroyed(ServletContextEvent servletContextEvent) {
        // 拿到即将销毁的 servletContext
        ServletContext servletContext = servletContextEvent.getServletContext();
        System.out.println("MyServletContextListener 监听到" + servletContext + "被销毁 ");

    }
}
```



同时，我们需要在 web.xml 中进行相关配置，如下：

```xml
<!--配置监听者 -->
<listener>
    <listener-class>com.xzh.listener.MyServletContextListener</listener-class>
</listener>
```



## 1.3 ServletContextAttribute

1. 作用：监听 ServletContext 属性变化
2. 相关方法
   - `void attributeAdded(ServletContextAttributeEvent event)`：添加属性时调用
   - `void attributeReplaced(ServletContextAttributeEvent event)`：替换属性时调用
   - `void attributeRemoved(ServletContextAttributeEvent event)`：移除属性时调用

**示例**

```java
import javax.servlet.ServletContextAttributeEvent;
import javax.servlet.ServletContextAttributeListener;

public class MyServletContextAttributeListener implements ServletContextAttributeListener {
    @Override
    public void attributeAdded(ServletContextAttributeEvent servletContextAttributeEvent) {
        // 得到新增属性名
        String name = servletContextAttributeEvent.getName();
        // 得到新增属性值
        Object value = servletContextAttributeEvent.getValue();
        System.out.println("MyServletContextAttributeListener监听到属性添加：" + name + '-' + value);
    }

    @Override
    public void attributeRemoved(ServletContextAttributeEvent servletContextAttributeEvent) {
        // 得到移除属性名
        String name = servletContextAttributeEvent.getName();
        // 得到移除属性值
        Object value = servletContextAttributeEvent.getValue();
        System.out.println("MyServletContextAttributeListener监听到属性移除：" + name + '-' + value);
    }

    @Override
    public void attributeReplaced(ServletContextAttributeEvent servletContextAttributeEvent) {
        // 得到替换属性名
        String name = servletContextAttributeEvent.getName();
        // 得到替换属性值（替换之前）
        Object value = servletContextAttributeEvent.getValue();
        System.out.println("MyServletContextAttributeListener监听到属性替换：" +  name + '-' + value);
    }
}
```



在 web.xml 中进行配置

```xml
<listener>
    <listener-class>com.xzh.listener.MyServletContextAttributeListener</listener-class>
</listener>
```





测试 servlet

```java
import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet(value="/hi")
public class HiServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext servletContext = req.getServletContext();
        servletContext.setAttribute("name", "Alice");   // 添加属性
        servletContext.setAttribute("name", "Bob");     // 替换属性
        servletContext.removeAttribute("name");            // 删除属性
        System.out.println("hiServlet over...");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```



## 1.4 HttpSessionListener

1. 作用：监听 session 的创建或销毁，即监听生命周期
2. 相关方法：
   - `void sessionCreated(HttpSessionEvent se)`：创建 session 时调用
   - `void sessionDestroyed(HttpSeesionEvent se)`：销毁 session 时调用
3. 该监听器常用来<strong style="color:red">监控用户上线、离线</strong>

**示例代码**

```java
import javax.servlet.http.HttpSession;
import javax.servlet.http.HttpSessionEvent;
import javax.servlet.http.HttpSessionListener;

public class MyHttpSessionListener implements HttpSessionListener {
    @Override
    public void sessionCreated(HttpSessionEvent httpSessionEvent) {
        HttpSession session = httpSessionEvent.getSession();
        // 将创建的 session 的最大存活时间设置为 30s
        session.setMaxInactiveInterval(30);
        System.out.println("MyHttpSessionListener 监听到 session 创建，id 为" + session.getId());
    }

    @Override
    public void sessionDestroyed(HttpSessionEvent httpSessionEvent) {
        HttpSession session = httpSessionEvent.getSession();
        System.out.println("MyHttpSessionListener 监听到 session 销毁，id 为" + session.getId());
    }
}
```



**xml 配置**

```xml
<listener>
    <listener-class>com.xzh.listener.MyHttpSessionListener</listener-class>
</listener>
```



如果我们想要进行测试，那么可以在原来的 HiServlet 的 doGet 方法中添加以下代码用来创建 session

```java
HttpSession session = req.getSession();
```

当我们访问 /hi 后就 Tomcat 就会创建一个新的 session 出来，当然，当我们首次启动主页时，也会有一个 JSESSION 被创建



## 1.5 HttpSessionAttributeListener

1. 作用：监听 Session 属性的变化
2. 相关方法：
   - `void attributeAdded(ServletRequestAttributeEvent srae)`：添加属性时被触发
   - `void attributeReplaced(ServletRequestAttributeEvent srae)`：替换属性时被触发
   - `void attributeRemoved(ServletRequestAttributeEvent srae)`：移除属性时被触发

3. 该监听器和前面的 `ServletContextAttribute` 类似，这里就不详细说明



## 1.6 ServletRequestListener 

1. 作用：监听 Request 创建或销毁，即监听 Request 的生命周期
2. 相关方法：
   - `void requestInitlized(ServletRequestEvent sre)`：创建 request 时被触发
   - `void requestDestoryed(ServletReuqestEvent sre)`：销毁 request 时被触发
3. 可以用来监听某个 IP 访问我们网站的频率

示例代码如下：

```java
import javax.servlet.ServletRequest;
import javax.servlet.ServletRequestEvent;
import javax.servlet.ServletRequestListener;
import javax.servlet.http.HttpServletRequest;

public class MyServletRequestListener implements ServletRequestListener {
    @Override
    public void requestDestroyed(ServletRequestEvent servletRequestEvent) {
        System.out.println("监听到 request 对象被销毁...");
    }

    @Override
    public void requestInitialized(ServletRequestEvent servletRequestEvent) {
        System.out.println("+----------------------------+");
        System.out.println("MyServletRequestListener 监听到 request 对象被创建");
        ServletRequest servletRequest = servletRequestEvent.getServletRequest();
        System.out.println("访问的IP为：" + servletRequest.getRemoteAddr());
        System.out.println("访问的资源为：" + ((HttpServletRequest)servletRequest).getRequestURL());
        System.out.println("+----------------------------+");
    }
}
```



**web.xml 配置**

```xml
<listener>
    <listener-class>com.xzh.listener.MyServletRequestListener</listener-class>
</listener>
```



## 1.7 其他监听器

### 1.7.1 ServletRequestAttributeListener

1. 作用：监听 Reuqest 属性变化
2. 相关方法
   - `void attributeAdded(ServletRequestAttributeEvent srae)`：request 请求对象添加属性时被触发
   - `void attributeReplace(ServletRequestAttributeEvent srae)`：替换属性时
   - `void attributeRemoved(ServletRequestAttributeEvent srae)`：移除属性时

### 1.7.2 HttpSessionBindingListener

感知监听器，使用较少

### 1.7.3 HttpSessionActivationListener 

感知监听器，使用较少



# 2 过滤器

> 过滤器是一个非常重要的知识点，平时使用较多，filter 在 Java EE 文档中位于 javax.servlet 包下

## 2.1 基本介绍

**过滤器的必要性**

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230425185116722.png" alt="image-20230425185116722" style="zoom:50%;" />

**介绍**

1. Filter 过滤器是 JavaWeb 的三大组件之一

2. Filter 过滤器是 JavaEE 的规范，是一个接口

   <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230425185827561.png" alt="image-20230425185827561" style="zoom:50%;" />

3. Filter 过滤器的作用：

   - 拦截请求
   - 过滤响应

4. 应用场景：权限检查、日记操作、事务管理



## 2.2 filter 基本原理

![image-20230425194003328](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230425194003328.png)



## 2.3 快速入门

要求：编写一个 login 页面和展示主页，当用户成功登录后才能访问主页

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230425200518562.png" alt="image-20230425200518562" style="zoom:50%;" />

**提前引入对应的 Jar 包**

![image-20230425200900035](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230425200900035.png)

**添加 filter 过滤器**

```java
import javax.servlet.*;
import java.io.IOException;

public class ManageFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        // 当 filter 创建后会调用该方法完成初始化
        System.out.println("filter 的 init 方法被调用");
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        /*
           1. 每当调用该 filter 时就会执行该方法;
           2. 如果这里没有继续调用请求方法，则就停止如果继续访问目标资源;
           3. 如果想要往下请求资源，则需要调用 filterChain.doFilter(servletRequest, servletResponse) 方法
        */
        System.out.println("doFilter 方法被调用");
        // 放行请求
        filterChain.doFilter(servletRequest, servletResponse);
    }

    @Override
    public void destroy() {
        // 当 filter 对象被销毁时会调用该方法
        System.out.println("filter 的 destroy 方法被调用");
    }
}
```

> 注意：filter 所导入的包在 javax 中，filter 过滤器中一共有三个方法，标志其生命周期，分别是 
>
> - init：tomcat 启动时调用的方法
> - doFiter：被调用时的方法
> - destroy：tomcat 销毁时调用的方法

**在 web.xml 配置过滤器**

```xml
<!--
    1. filter 一般配置在 web.xml 的最前面
    2. url-pattern 就是当请求和 url 与配置的 url 相匹配的时候就会调用该 filter
-->
<filter>
    <filter-name>ManageFilter</filter-name>
    <filter-class>com.xzh.filter.ManageFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>ManageFilter</filter-name>
    <url-pattern>/manage/*</url-pattern>
</filter-mapping>
```

> filter 的配置方式基本上与 servlet 相同



## 2.4 对过滤器的说明

1. 对于请求转发，即 `request.getRequestDispatcher` 是不会走过滤器
2. 如果页面中存在某些静态资源，例如 `<img src="xxx">` ，在请求这些静态资源时是会经过过滤器判断的
3. 过滤器 filter 中的 servletReuqest 与放行的 web 资源，例如 jsp 页面中的 request对象是同一个



## 2.5 url-pattern 

过滤器的 url-pattern 是拦截路径，即浏览器在请求什么位置的资源时，过滤器会进行过滤拦截，常见的 url-pattern 拦截格式有如下几种写法：

1. 精准匹配：`/a.jsp` 表示拦截请求地址为 `http://ip[域名]:port/工程路径/a.jsp` 的资源
2. 目录匹配：`manage/*` 表示拦截请求地址为 `http://ip[域名]:port/工程路径/manage` 目录下的所有资源
3. 后缀名匹配：`*.jsp` 表示拦截 webapp 工程目录下所有后缀一 jsp 结尾的资源



## 2.6 filter 生命周期

1. filter 在 web 项目启动时，由 tomcat 创建 filter 实例，只会创建一个

2. 创建 filter 实例会调用其默认的无参构造器，同时会调用 init 方法，总会调用一次

3. 在创建 filter 实例时，同时会创建一个 filterConfig 对象，并通过 init 方法传入

   > 通过 filterConfig 对象，程序员可以获取该 filter 的相关配置信息

4. 当一个 HTTP 请求与该 filter url-pattern 对应上后，会调用 doFilter 方法

5. 当 tomcat 关闭时，filter 实例会被自动销毁



## 2.7 过滤器链 filterChain

在处理某些业务时，一个过滤器是远远不够的，我们需要需要多个过滤器，而这些过滤器串起来的对象就是 filterChain 

<img src="https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230726162145204.png" alt="image-20230726162145204" style="zoom:50%;" />

当一个资源的请求 url 与多个 filter 匹配上了之后，会按照一条链的方式执行后又回来，就像一条流水线，加工处理顺序与 web.xml 中配置的顺序保持一致，当一条链上所有的过滤器的前置任务通过 doFilter 传递执行完毕后会回过头来执行后置任务

> 多个 filter 和目标资源在一次 HTTP 请求，在同一个线程中

