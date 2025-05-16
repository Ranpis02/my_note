[toc]

# 1 SpringMVC 入门

## 1.1 基本介绍

Spring MVC 的完整的叫法是 "Spring Web MVC"，是 web 层的框架

- SpringMVC 通过注解，让 POJO 成为控制器，不需要实现任何接口
- SpringMVC 支持 REST 格式的 URL 请求
- Spring MVC 是处理 web 层的一个模块 / 组件，Spring MVC 的基石是 Servlet

> SpringMVC 基于 Spring，核心包是：Spring-web-xxx.jar 和 Spring-webmvc-xxx.jar



:herb:**拓展**

Spring、Spring MVC 和 Spring Boot 之间的关系

Spring Boot 是为了简化开发者的会用而提出的**封神框架**（约定优于配置，简化了 Spring 的配置流程），Spring Boot 中包含很多组件 / 框架，其中 Spring 就是最为核心的内容之一，而 Spring 本身就包含了 Spring MVC，因此他们三者之间的关系为：<u>Spring Boot > Spring > Spring MVC</u>



## 1.2 快速入门

需求：使用 Spring MVC 设计一个登录业务，时序图如下：

![image-20230729211339306](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230729211339306.png)



搭建环境，在使用 Spring MVC 之前，我们需要手动搭建环境，如下：



01 引入外部依赖

```xml
<dependencies>
    <!--spring 所需的 jar 包-->
    <dependency>
        <groupId>c3p0</groupId>
        <artifactId>c3p0</artifactId>
        <version>0.9.1.2</version>
    </dependency>

    <dependency>
        <groupId>net.sourceforge.cglib</groupId>
        <artifactId>com.springsource.net.sf.cglib</artifactId>
        <version>2.2.0</version>
    </dependency>

    <dependency>
        <groupId>org.aopalliance</groupId>
        <artifactId>com.springsource.org.aopalliance</artifactId>
        <version>1.0.0</version>
    </dependency>

    <dependency>
        <groupId>org.aspectj</groupId>
        <artifactId>com.springsource.org.aspectj.weaver</artifactId>
        <version>1.6.4.RELEASE</version>
    </dependency>

    <!-- log4j 所需的 jar 包 -->
    <dependency>
        <groupId>commons-logging</groupId>
        <artifactId>commons-logging</artifactId>
        <version>1.1.3</version>
    </dependency>

    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.33</version>
    </dependency>

    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-aop</artifactId>
        <version>5.3.8</version>
    </dependency>

    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-aspects</artifactId>
        <version>5.3.8</version>
    </dependency>

    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-beans</artifactId>
        <version>5.3.8</version>
    </dependency>

    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.3.8</version>
    </dependency>

    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
        <version>5.3.8</version>
    </dependency>

    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-expression</artifactId>
        <version>5.3.8</version>
    </dependency>

    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>5.3.8</version>
    </dependency>

    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-orm</artifactId>
        <version>5.3.8</version>
    </dependency>

    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-tx</artifactId>
        <version>5.3.8</version>
    </dependency>
    
	<!--spring mvc 所需的 jar 包-->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-web</artifactId>
        <version>5.3.8</version>
    </dependency>

    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.3.8</version>
    </dependency>
</dependencies>
```



02 创建 applicationContext-mvc.xml 配置文件（该文件为 spring 的配置文件）

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
    <!--配置自动扫描的包-->
    <context:component-scan base-package="com.xzh.web" />

    <!--配置视图解析器-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!--配置 suffix 和 prefix-->
        <property name="prefix"  value="/WEB-INF/pages/" />
        <property name="suffix" value=".jsp" />
    </bean>
</beans>
```



03 在 web.xml 中配置中央控制器（其中中央控制器需要在加载 spring 的 xml 配置文件）

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">


    <!--配置中央控制器/前端控制器/分发控制器(用户的请求都会经过它的处理)-->
    <servlet>
        <servlet-name>springDispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>

        <!-- 配置属性 contextConfigLocation, 指定 DispatcherServlet 操作的 spring 配置文件 -->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:applicationContext-mvc.xml</param-value>
        </init-param>
        <!--在 web 项目启动时，就自动加载 DispatcherServlet-->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springDispatcherServlet</servlet-name>
        <!--
            这里我们配置的 url-patter 为 /，表示用户的请求都经过 DispatcherServlet 请求,
            同时这样请求也支持 rest 风格
        -->
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```



04 创建静态 web 资源（jsp）和动态 web 资源（servlet）

```java
// UserServlet： com.xzh.web/UserServlet.java
/**
 * 当我们使用了 spring mvc 框架后，只需要在一个类上添加标识 @Controller
 * 表示将该类视为一个控制器，注入到容器中
 * 比原生的 servlet 开发要简化很多
 */
@Controller
public class UserServlet {
    // 编写方法，响应用户的请求
    /**
     * RequestMapping 里面的value类似于我们以前在原生的 Servlet 上配置 url-pattern
     * 即用户通过 localhost:8080/web_app/login 就可以访问到 login() 方法
     * return "login_ok" 表示返回结果给视图解析器（InternalResourceViewResolver）,视图解析器会根据配置，来决定跳转到哪一个页面
     * 例如我们在视图解析器配置如下：
     * <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
     *   <property name="prefix"  value="/WEB-INF/pages/" />
     *   <property name="suffix" value=".jsp" />
     * </bean>
     * 则表示当我们访问 localhost:8080/web-app/login 这个 url 时
     * springmvc 底层会将返回值与 suffix 和 prefix 进行拼接，例如
     * /WEB-INF/pages/login_ok.jsp 并在容器内进行转发
     */
    @RequestMapping(value = "/login")
    public String login() {
        System.out.println("login ok");
        return "login_ok";
    }
}
```

```jsp
<%-- 登录页面，放在 web 项目工程的根目录 --%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>login</title>
  </head>
  <body>
      <h3>登录页面</h3>
    <form action="login">
      用户名：<input type="text" name="username"><br>
      密码：<input type="password" name="password"><br>
      <input type="submit" value="登录">
    </form>
  </body>
</html>
```

```jsp
<%-- 登录成功页面，登录成功后跳转到该页面 ，路径为 WEB-INF 下的 pages/login_ok.jsp --%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>登录成功</title>
  </head>
  <body>
    <h3>恭喜登录成功</h3>
  </body>
</html>
```

> WEB-INF 目录：该目录是 Java Web 的安全目录，只能服务端访问，而客户端无法访问，如果想要在客户端进行访问的话，就必须通过 web.xml 文件或是采用注解的方式对要访问的文件进行映射



## 1.3 Spring MVC 注意事项

1. 我们之前讲过，在 Sping 中，无论是 `@Component` 还是 `@Repostory`、`@Controller`、`@Service` 其实效果都是一样，都是实例或类对象注入到 Spring 容器中，但是在 SpingMVC 中，`@Controller` 是有具体含义的，就是充当一个 Servlet, 我们称之为一个 Hanlder 处理器

2. 我们在使用 REST 风格的请求配置时，如果只需要指定请求 url， 那么 value 是可以省略的，如下

   ```java
   @ReuqetsMapping(value = "/login") 
   // 等价于下面的形式
   @RequestMapping("/login")
   ```

3. 关于 SpringMVC 的 DispatcherServet 的配置文件，如果不崽 web.xml 指定 applicationContext-mvc.xml ，默认在 /WEB-INF/springDispatcher-servlet.xml

   ```java
   /**
    * Suffix for WebApplicationContext namespaces. If a servlet of this class is
    * given the name "test" in a context, the namespace used by the servlet will
    * resolve to "test-servlet".
    */
   ```

   > 上面的注解便是源码的注解，如果我们没有配置 springDispatcherServlet 这个控制器的 spring 配置文件，那么底层会自动查找 springDispatcherServlet-servlet.xml



## 1.4 SpringMVC 执行流程分析

![image-20230730203837952](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230730203837952.png)



# 2 运用进阶

## 2.1 RequestMapping

### 2.1.1 修饰方法和类

`@RequestMapping` 注解可以修饰方法，还可以修饰类，当同时修饰类和方法时，请求的 url 就是组合 `/类请求值/方法请求值`

**示例**

```java
@Controller
@RequestMapping("/user")
public class UserHandler {
    /**
     * 对于 UserHandler，如果我们想要调用 buy 这个方法，那么我们需要请求 ip:port/web-app/user/buy
     * method 指明了我们请求的方法，其中常用的有 GET、POST、DELETE、PUT 四种方法
     * SpringMVC 控制器默认支持 GET 和 POST 两种方式
     */
    @RequestMapping(value = "/buy", method = RequestMethod.POST)
    public String buy() {
        System.out.println("购买商品");
        return "success";
    }
}
```



### 2.1.2 指定请求方式 method

1. RequestMapping 还可以指定请求的方式：POST、GET、PUT、DELETE，请求的方式并且要和指定的方式一样，否则会报 405 —— 即 Request method 'xxx' not supported

2. SpringMVC 控制器默认支持 GET 和 POST 两种方式，也就是说当我们不指定 method 时，既可以接收 GET 请求，也可以接收 POST 请求，也就是默认是如下配置

   ```java
   @RequestMapping(value = "login", method = {RequestMethod.GET,RequestMethod.POST})
   ```



### 2.1.3 指定 params 和 header 支持简单的表达式

```java
/**
 * 1. params = "bookId" 表示请求该目标方法时，必须给一个 bookId 参数，值没有限定，请求url: /user/find?bookId=100
 *  如果缺少请求参数，那么就会报 400，提示请求条件不满足
 * 2. params = "bookId=100" 表示必须给一个 bookId 参数，并且值必须为 100
 *  如果缺少请求参数，同样报 400
 * 3. params = "!bookId" 表示请求参数不能携带 bookId
 * 4. params = "bookId!=100" 表示请求参数bookId不能为 100（注意：不等号两边不能留有空格）
 * 5. params = {"bookId=100", "bookPrice"} 表示请求参数需要携带 bookId 和 bookPrice 两个参数，并且 bookId 的值为 100
 */
@RequestMapping(value = "/find", params = "bookId!=100")
public String search(String bookId) {
    return "login_ok";
}
```

 headers 配置也几乎是一样的道理



### 2.1.4 支持 Ant 风格资源地址

1. `?`：匹配文件名中的一个字符
2. `*`：匹配文件名中的任意字符
3. `**`：匹配多层路径

例如：

- `/user/*/create`：匹配 `/user/aaa/create` 、`/user/bbb/create` 等 URL
- `/user/**/create`：匹配 `/user/create`、`/user/aaa/create`、`/user/aaa/bbb/create` 等 URL



### 2.1.5 配合 `@PathVariable`

`@RequestMaping` 还可以配合 `@PathVariable` 映射 URL 绑定的占位符，这样就不需要在 url 地址上带上参数名，更加简介明了

```java
/**
 * PathVariable 正如字面意思是 "路径变量"
 * 请求 url 中的 userId 会与 @PathVariable 后面的参数名相匹配，并将值注入到 id 中
 * username 也是同样的道理
 */
@RequestMapping(value = "/login/{userId}/{username}")
public String register(@PathVariable("userId") String id,
                       @PathVariable("username") String name) {
    System.out.println("userId = " + id);
    System.out.println("username = " + name);
    return "register";
}
```



### 2.1.6 注意事项和使用细节

1. 映射的 URL 是唯一的，不能重复
2. 各种请求的简写形式：
   - `@GetMapping`
   - `@PostMapping`
   - `@PutMapping`
   - `@DeleteMapping`

3. 注入提交参数名和后台方法的形参名应保持一致，否则后端无法接受到参数



## 2.2  REST

### 2.2.1 基本介绍

1. REST：即 Representational State Transfer。（资源）表现层状态转化。是目前比较流行的一种请求方法
2. HTTP 协议中，有四个表示操作方式的动词，分别是 ：
   - GET：获取资源
   - POST：新建资源
   - PUT：更新资源
   - DELETE：删除资源

传统的请求方式：

- getBook?id=1 GET
- delete?id GET
- update POST
- add POST

> 传统的 url 是通过参数来说明 CRUD 的类型，REST 是通过 GET/POST/PUT/DELETE 来说明 CRUD 的类型



**REST 核心过滤器**

1. 当前浏览器只支持 POST/GET 请求，因此为了得到 PUT/DELETE 的请求方式需要使用 Spring 提供的 ==HiddenHttpMethodFilter== 过滤器进行转换
2. **HiddenHttpMethodFilter 是对 POST 请求方法进行转换，对于 GET 请求无法转换**，我们需要特别注意这一点



### 2.2.2 配置

01 在 web.xml 中配置 HiddenHttpMethodFilter 

```xml
<!--
    1. 配置 HiddenHttpMethodFilter，作用是将 POST 提交的 DELETE 和 PUT 请求进行转换
    2. 配置 url-pattern 是 /* 表示所有的请求都将经过 hiddenHttpMethodFilter 过滤
-->
<filter>
    <filter-name>hiddenHttpMethodFilter</filter-name>
    <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>hiddenHttpMethodFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```



02 在 spring 的配置文件中添加相关配置

```xml
<!--加入两个常规配置，对应的地址为 http://www.springframework.org/schema/mvc -->
<!--支持springmvc的高级功能，比如JSR303校验，映射动态请求-->
<mvc:annotation-driven />
<!--将 springmvc 不能处理的请求，交给 tomcat 处理，比如 css、js-->
<mvc:default-servlet-handler />
```



03 新建 book.jsp 和 success.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>rest </title>
    <%--引入jquery--%>
    <script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.6.4/jquery.js"></script>
    <script type="text/javascript">
        $(function () { //当页面加载完成后，就执行=> 如果你完全忘记，去回顾我们讲过的jquery
            //给删除超链接绑定一个点击事件
            $("#deleteBook").click(function (){
                const formDom = $('#hiddenForm')
                //我们自己定义给提交的行为
                formDom.attr("action", this.href);
                $(":hidden").val("DELETE");
                formDom.submit();
                return false; //改变点击超链接的行为, 不再提交
            })
        })
    </script>
  </head>
  <body>
    <h3>Rest风格的crud操作案例</h3>
    <br><hr>
    <h3>rest风格的url 查询书籍[get]</h3>
    <a href="user/book/200">点击查询书籍</a>
    <br><hr>
    <h3>rest风格的url 添加书籍[post]</h3>
    <form action="user/book" method="post">
      name:<input name="bookName" type="text"><br>
      <input type="submit" value="添加书籍">
    </form>
    <br><hr>
    <h3>rest风格的url, 删除一本书</h3>
    <%--
    1. 默认情况下 <a href="user/book/600">删除指定id的书</a> 是get
    2. 怎么样将 get 请求转成 springmvc 可以识别的 delete 就要考虑HiddenHttpMethodFilter机制
       public static final String DEFAULT_METHOD_PARAM = "_method";
       ---------------------------------------------------
       private static final List<String> ALLOWED_METHODS =
                Collections.unmodifiableList(Arrays.asList(HttpMethod.PUT.name(),
                        HttpMethod.DELETE.name(), HttpMethod.PATCH.name()));
      ---------------------------------------------------
       if ("POST".equals(request.getMethod()) && request.getAttribute(WebUtils.ERROR_EXCEPTION_ATTRIBUTE) == null) {
                String paramValue = request.getParameter(this.methodParam);
                if (StringUtils.hasLength(paramValue)) {
                    String method = paramValue.toUpperCase(Locale.ENGLISH);
                    if (ALLOWED_METHODS.contains(method)) {
                        requestToUse = new HttpMethodRequestWrapper(request, method);
                    }
                }
            }
    3. 上面代码可以看到 HiddenHttpMethodFilter 过滤器可以对以Post方式提交的delete,put,patch进行转换,成springmvc
       识别的 RequestMethod.DELETE / RequestMethod.PUT /...
    4. 我们需要将 get <a href="user/book/600">删除指定id的书</a> 以post方式提交给后端handler, 这样过滤器还会生效
    5. 我们可以同jquery来处理-引入jquery
    --%>
    <a href="user/book/600" id="deleteBook">删除指定id的书</a>
    <form action="" method="post" id="hiddenForm">
      <input type="hidden" name="_method"/>
    </form>
    <br><hr>
    <h3>rest风格的url 修改书籍[put]~</h3>
    <form action="user/book/666" method="post">
      <input type="hidden" name="_method" value="PUT">
      <input type="submit" value="修改书籍~">
    </form>
  </body>
</html>
```

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>Title</title>
  </head>
  <body>
  <h1>操作成功</h1>
  </body>
</html>
```



04 新建 BookHanlder

```java
@Controller
@RequestMapping("/user")
public class BookHandler {

    /**
     * 通过订单号查询图书
     */
    @GetMapping(value = "/book/{id}")
    public String getBook(@PathVariable("id") String id) {
        System.out.println("正在查询 id = " + id + "的图书");
        return "success";
    }

    /**
     * 添加图书书籍
     */
    @PostMapping(value = "/book")
    public String addBook(String bookName) {
        System.out.println("添加的书籍名为：" + bookName);
        return "success";
    }

    /**
     * 删除图书书籍
     * 说明：前面的无论是 get 请求还是 post 请求，其实和之前并没有区别
     * 但是对于 put 和 delete 请求，我们才开始真正要使用 HiddenHttpMethodFilter
     */
    /**
     * 源码分析：
     * public static final String DEFAULT_METHOD_PARAM = "_method";
     * private static final List<String> ALLOWED_METHODS =
     * 			Collections.unmodifiableList(Arrays.asList(HttpMethod.PUT.name(),
     * 					HttpMethod.DELETE.name(), HttpMethod.PATCH.name()));
     * 默认请求的参数为 _method ，允许转换的请求方法包括 PUT、DELETE、PATCH
     * 之后会对请求进行处理，其中包括了大小写转换匹配 post
     */
    /**
     * 问题提出：因为请求必须是 POST 请求，并且需要携带参数 _method
     * 那么对于普通的 get 请求，例如 a 标签，我们如何进行 put 或者 delete 请求
     * 解决方案：我们可以通过 jquery 更改点击事件，使其添加 hidden 属性，值为 DELETE / delete
     */
    @DeleteMapping(value = "/book/{id}")
    public String login(@PathVariable("id") String id) {
        System.out.println("删除图书id = " + id);
        /**
         * 如果我们直接 return "success" , 那么将会报错 405 - JSPs only permit GET POST or HEAD
         * 解决方案：使用重定向
         */

        return "redirect:/user/success";
    }

    /**
     * put 请求修改更新数据
     */
    @PutMapping(value  = "/book/{id}")
    public String updateBook(@PathVariable("id") String id) {
        System.out.println("修改图书 id = " + id);
        return "redirect:/user/success";
    }

    /**
     * 重定向通用请求
     */
    @RequestMapping(value = "/success")
    public String successGeneral() {
        return "success";
    }
}
```



**注意事项**

1. HiddenHttpMethodFilter，在将 post 转换为 delete / put 请求时，是按照 `_method` 参数名来读取的
2. 如果 web 项目时运行在 Tomcat 8 及以上，会发现被过滤成 DELETE 和 PUT 请求，到达控制器时能顺利执行，但是返回时会报 HTTP 405 的错误提示：405 - JSPs only permit GET POST or HEAD
   - 解决方案1：使用 Tomcat 7
   - 解决方案2：将请求转发（forward）改为请求重定向（redirect），重定向到一个 Handler ，由 Handler 转发到页面

3. 页面测试时，如果出现点击修改书籍，仍然走的删除 url，是因为浏览器原因（缓存等原因



## 2.3 RequestParam 获取参数值

需求说明：开发过程中有时候我们可能会遇到请求的 url 中请求参数顺序与我们的 handler 中的形参顺序不一致或者名称不对应，那么我们应该如何解决？

解决方案：<u>利用注解 `@RequestParam`</u> 

```java
@RequestMapping("/vote")
@Controller
public class VoteHandler {
    /**
     * 获取到超连接传递的数据
     * 对于 ip:port/web-app/vote/vote01?name=xxx 如果我们直接访问，那么由于我们处理函数的形参为 username，那么肯定是无法接收成功的，所以我们这个时候就需要使用到 @RequestParam 进行参数映射
     * value = "name" 表示匹配请求的参数为 name
     * required = false 表示请求参数是否为必传项，默认为 true
     */
    @RequestMapping(value = "/vote01")
    public String test01(@RequestParam(value = "name", required = false ) String username) {
        System.out.println("传递的参数 username 为：" + username);
        return null;
    }
}
```



## 2.4 获取 HTTP 请求消息头

说明：本节将讲解如何获取到 HTTP 请求的消息头信息

> 该功能日常开发使用较少

在 SpringMVC 中，如果我们想要获取请求头，那么我们可以直接通过 `@RequestHeader` 来获取，具体使用方法如下：

```java
@RequestMapping(value = "/vote02")
    public String test02(@RequestHeader("Accept-Encoding") String ae,
                         @RequestHeader("Host") String host) {
        System.out.println("客户端可接收的压缩算法为：" + ae);
        System.out.println("请求服务器的 ip+port：" + host);
        return null;
    }
```

> :information_source:区分 Content-Encoding 和 Accepting-Encoding
>
> - Acceptig-Encoding 是位于请求头，用于告诉服务器自己可以接受那些算法
> - Content-Encoding 是位于响应头，用于告诉客户端，例如浏览器当前传送的文件所使用的是哪种压缩算法



## 2.5 获取 JavaBean 对象

使用场景说明：前端通过表单发送过来的请求参数将其进行封装为一个 JavaBean 对象

使用方法如下：

准备好 entity

```java
/**
  *	主人实体
  */
@Getter
@Setter
public class Master {
    /**
     * 主人 id
     */
    private Integer id;
    /**
     * 主人名
     */
    String name;
    /**
     * 主人养的宠物，该类由于 pet 属性与 Pet 类产生级联
     */
    private Pet pet;
}
```

```java
@Getter
@Setter
public class Pet {
    /**
     * 宠物 ID
     */
    private Integer id;
    /**
     * 宠物名
     */
    private String name;
}
```

handler 处理器/controller 控制器

```java
/**
 * 将提交的数据封装为 java bean
 *  1. 方法的形参用对应的类型来指定即可，SpringMVC 会自动进行封装
 *  2. 如果自动完成封装，要求提交的数据，参数名与对应的字段名保持一致
 *  3. 如果属性是对象，这里就是仍然通过 字段名.下一级字段名 的形式，例如  pet 是 monster 的字段，那么
 *  传入的参数名应为 pet.id, pet.name 这种形式
 *  4. 如果提交的参数名和对象的字段名不匹配则对象的属性值就是空
 */
@RequestMapping("/vote")
@Controller
public class VoteHandler {
    @RequestMapping(value = "/vote03")
    public String test03(Master master) {
        System.out.println(master);
        return "success";
    }
}
```

我们使用 postman 直接验证

![image-20230731192310238](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230731192310238.png)

![image-20230731192626582](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230731192626582.png)



## 2.6 使用 servlet api

开发过程中，我们可能会用到原生的 servlet，那么我们如何在 spring 环境下使用原生的 servlet 呢？

如果我们想要使用原生的 servlet，那么我们首先需要引入 servlet-api.jar

```xml
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>4.0.1</version>
    <scope>provided</scope>
</dependency>
```

之后我们使用原生的 servlet 对请求进行处理

```java
@RequestMapping("/vote")
@Controller
public class VoteHandler {
    /**
     * 使用原生的 servlet-api 来获取提交的数据
     */
    @RequestMapping(value = "/vote04")
    public void test04(HttpServletRequest req, HttpServletResponse resp) {
        String username = req.getParameter("username");
        System.out.println("username = " + username);
    }
}
```

测试请求的 url：`http://localhost:8080/spring_mvc/vote/vote04?username=xzh`



:warning:**注意事项**

1. 除了 HttpServletRequest、HttpServletResponse 还有其他的对象也可以直接通过参数形式获取，例如：`HttpSession`、`java.security.Principal`、`InputStream`、`OutputStream`、`Reader`、`Writer`
2. 其中有一些对象除了通过从参数列表中获取外还可以通过其他对象间接获取，例如 Session 对象，需要注意的是通过这种方式间接获取的对象和直接通过参数形式获取的对象是同一个



# 3 高阶用法

## 3.1 模型数据

介绍：所谓模型数据即是从 model 中获取的数据

需求：我们需要对模型数据进行处理，将其渲染到页面中，可以使用 jsp 或 vue 等等，这个过程需要我们在 controller 层完成



### 3.1.1 方式1：通过 HttpServletRequest 放入 request 域中

**小试牛刀**

```java
@RequestMapping("/vote")
@Controller
public class VoteHandler {
    /**
     * 1. springmvc 会自动把获取的 model 模型，放入到  request 域中，key 就是参数名
     * 2. 我们也可以手动将参数放入到 request 域中
     */
    @RequestMapping(value = "/vote05")
    public String test05(Master master) {
        return "vote_ok";
    }
}
```

```jsp
<%-- 回显页面：vote_ok.jsp --%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>回显页面</title>
  </head>
  <body>
    <h1>获取的数据显示页面</h1>
    <ul>
      <li>主人名字：${requestScope.master.name}</li>
      <li>宠物名字：${requestScope.master.pet.name}</li>
    </ul>
  </body>
</html>
```

测试请求 url: `http://localhost:8080/spring_mvc/vote/vote05?name=xzh&pet.name=smith` 

结果分析：我们会发现页面确实将请求域 requestScope 中的数据拿到了，说明 springmvc 中不仅会自动将请求参数自动封装到 javabean 中，还会自动帮助我们插入到请求域中，作为一个对象存储到里面，对象名为 handler 参数的属性的类型的小驼峰名



### 3.1.2 方式2：通过 HttpServletRequest 放入到request域中

我们第一种方式只能将请求的参数封装起来放入到 request 域中，可是真实的业务场景中，大部分都是我们从 service 中获取数据手动放入到 request 域中，那么在这种情况下，我们就需要思考如何手动放入参数到 request 域中，下面就是我们以前的原始做法

```java
@RequestMapping("/vote")
@Controller
public class VoteHandler {
    /**
     * 通过 HttpServletRequest 来完成
     */
    @RequestMapping(value = "/vote06")
    public String test06(Master master, HttpServletRequest req) {
        // 添加属性值
        req.setAttribute("hobby", "rap");
        // 修改封装好的 master 中的属性值(这里由于对象是以引用的方式传递，所以这里直接修改即可)
        master.setName("jack");
        return "vote_ok";
    }
}
```



### 3.1.3 通过请求的方法参数 Map<String, Object> 放入到 request 域中

```java
@RequestMapping("/vote")
@Controller
public class VoteHandler {
    /**
     * 演示通过 Map<String, Object> 设置数据到 request 域中
     * 前面的我们提到 springmvc 会将参数自动封装到 request 域中，其实就是借助了这个 "map"
     * 它将参数列表中的 master 放入到 map 容器中，其中 key = "master"（参数类型的小驼峰 ），value = master
     */
    @RequestMapping(value = "/vote07")
    public String test07(Master master, Map<String, Object> map) {
        // 将 address 放入到 map 中，相当于间接将其放入到 request 域中
        map.put("address", "jiangxi");
        Iterator<Map.Entry<String, Object>> iterator = map.entrySet().iterator();
        while(iterator.hasNext()) {
            Map.Entry<String, Object> next = iterator.next();
            System.out.println("key = " + next.getKey() + ", value = " + next.getValue());
        }
        /**
         * key = master, value = com.xzh.web.requestparam.entity.Master@5024b378
         * key = org.springframework.validation.BindingResult.master, value = org.springframework.validation.BeanPropertyBindingResult: 0 errors
         * key = address, value = jiangxi
         * 结论：对于遍历的结果我们可以看到其实不只有 master 和 address 两个参数
         */
        return "vote_ok";
    }
}
```



### 3.1.4 方法4：通过返回 ModelAndView 获取 request 域中数据

```java
@RequestMapping("/vote")
@Controller
public class VoteHandler {
    /**
     * 在 springmvc 中 modelAndView 是一个非常重要的对象，我们利用它可以做很多事情
     */
    @RequestMapping(value = "/vote08")
    public ModelAndView test08(Master master) {
        ModelAndView modelAndView = new ModelAndView();
        // 添加属性和属性值到 request 域中
        modelAndView.addObject("address", "上海");
        // 替换原先的属性值 master
        modelAndView.addObject("master", null);
        // 指定跳转的视图名称
        modelAndView.setViewName("vote_ok");
        return modelAndView;
    }
}
```

**注意事项**

1. 从本质上看，请求响应的方法 `return "xxx"` 是返回了一个字符串，其实本质是返回了一个 ModelAndiew 对象，只是默认被封装起来了
2. ModelAndView 既可以包含 model 数据，也可以包含视图信息
3. ModelAndView 对象的 addObject 方法可以添加 key-val 数据，默认在 request 域中
4. modelAndView 对象的 setView 方法可以指定视图名称



:herb:拓展：将 modelAndView 的数据放入到 Session 域中

利用 HttpSession 即可



## 3.2 @ModelAttribute 

需求：如果我们想要在调用 handler 的方法时有一个前置方法，在  SpringMVC 中应该如何实现？

解决方案：在 SpringMVC 下，我们可以直接添加注解 `@ModelAttribute` 将 handler 中的某个方法标识为一个前置方法

> 类似于 AOP 中的前置通知

```java
@RequestMapping("/vote")
@Controller
public class VoteHandler {
    @ModelAttribute
    public void prepareModel() {
        System.out.println("prepareModel()-----完成准备工作");
    }
}
```



**最佳实践**

1. 修改用户信息之前，在前置方法中先查出要修改的用户
2. 对要修改的表单的数据与源数据进行比对，如果未发生变化则不进行修改



## 3.3 视图和视图解析器

基本介绍

1. 在 springmvc 中的目标方法最终返回的都是一个视图（有各种视图）
2. 返回的视图都会由一个视图解析器处理（视图解析器由很多种）



自定义视图

1. 在默认的情况下，我们都是返回默认的视图，然后返回的视图交给 springmvc 的 InternalResourceViewResolver 视图解析器来处理

   ```xml
   <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
       <!--配置 suffix 和 prefix-->
       <property name="prefix"  value="/WEB-INF/pages/" />
       <property name="suffix" value=".jsp" />
   </bean>
   ```

2. 在实际开发中，有时我们呢需要自定义视图，这样可以满足更多更复杂的需求

使用步骤

01 首先在 spring 的配置文件中配置自定义视图解析器

```xml
<!--
    1. 配置自定义的视图解析器 BeanNameViewResolver
    2. BeanNameViewResolver 可以去解析我们自定义的视图
    3. 配置属性 order，表示视图解析器执行的顺序，值越小，优先级越高
    4. 属性 order 的默认值是最低优先级，值为 Integer.MAX_VALUE
-->
<bean class="org.springframework.web.servlet.view.BeanNameViewResolver">
    <property name="order" value="99" />
</bean>
```



02 添加 handler 和自定义的视图解析器

```java
// GoodsHanlder
@RequestMapping("/goods")
@Controller
public class GoodsHandler {
    @RequestMapping(value = "/buy")
    public String buy() {
        System.out.println("-----buy-----");
        // 返回自定义的视图解析器的 id
        return "customView";
    }
}
```

```java
// 自定义视图解析器
@Component(value = "customView")
public class MyView extends AbstractView {
    @Override
    protected void renderMergedOutputModel(Map<String, Object> model, HttpServletRequest request, HttpServletResponse response) throws Exception {
        // 完成视图渲染，并且我们可以确定我们要跳转的页面
        request.getRequestDispatcher("/WEB-INF/pages/my_view.jsp").forward(request, response);
    }
}
```



## 3.4 指定转发和重定向

1. 默认返回的方式的请求转发，然后再用视图解析器进行处理

2. 我们也可以在目标方法中直接指定重定向或者转发的 url 地址

   > 注意：如果是 指定重定向，那么将不能定向到 `/WEB-INF` 目录中

**示例**

```java
// 请求转发
return "foward:xxx";

// 重定向
return "redirect:xxx";
```

> 特别提醒：请求转发是交给服务器处理的，根路径  `/` 为 web 工程路径；而重定向则是交给浏览器，根目录为 `ip:port`



# 4 简单实现 SpringMVC
