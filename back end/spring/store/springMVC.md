[toc]

# 1 SpringMVC 基础

## 1.1 springMVC 介绍

官方文档位置：解压 spring-5.3.8-dist.zip 后的 `spring-framework-5.3.8\docs\reference\html\web.html` 中

**特点**

1. SpringMVC 准确叫法应为 "Spring Web MVC"，它是 WEB 层框架，接管了 Web 层组件，比如<u>控制器、视图、视图解析、返回用户的数据格式</u>等，同时支持 MVC 的开发架构
2. MVC：Model-View-Controller
3. SpringMVC 通过注解，让 POJO 成为控制器，不需要继承类或者实现接口
4. SpringMVC 采用低耦合的组件设计方式，具有更好的扩展性和灵活性
5. 支持 REST 格式的 URL 请求
6. SpringMVC 是基于 Spring 的，也就是 SpringMVC 基于Spring 的，核心包为 spring-webmvc-xx.jar 和 spring-web-xx.jar



**Spring、SpringMVC 和 SpringBoot 的关系** 

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230423110835371.png" alt="image-20230423110835371" style="zoom: 50%;" />

1. Spring MVC 只是 Spring 处理 WEB 层请求的一个模块 / 组件，Spring MVC 的基石是 Servlet
2.  Spring Boot 是为了简化开发者的使用，推出的 "封神" 框架（<strong style="color:red">约定优于配置</strong>，简化了 Spring 的配置流程），SpringBoot 包含很多组件 / 框架，Spring 就是最核心的内容之一，也包含 Spring MVC



## 1.2 快速入门

完成一个最基本的登录案例，登录流程分析如下：

![image-20230423113618422](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230423113618422.png)

基本流程如下

1. 添加 web工程项目
2. 创建 lib 目录，导入相关依赖
3. 创建 src/applicationContext-mvc.xml(spring config 文件)

对于 web 目录结构如下：

```
web                                                             
├─ WEB-INF                                                      
│  ├─ lib         
│  ├─ pages                                                     
│  │  └─ login_ok.jsp                                           
│  └─ web.xml                           
└─ login.jsp                                 
```

web.xml 配置文件内容如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!--配置前端控制器/中央控制器/分发控制器-->
    <servlet>
        <servlet-name>springDispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--配置属性 contextConfigLocation, 指定DispatcherServlet操纵spring配置文件 -->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:applicationContext-mvc.xml</param-value>
        </init-param>
        <!--在web项目启动时，就自动加载DispatcherServlet-->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <!--
            1. 这里我们配置的utl-pattern是/，表示用户的请求都经过DispatcherServlet
            2. 这里的配置也符合 REST 风格的 URL 请求
         -->
        <servlet-name>springDispatcherServlet</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```

- 主要配置了前端控制器
- 前端控制器同时解析了 java 工程中的 spring 配置文件



login.jsp 文件内容如下：

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>登录</title>
</head>
<body>
    <h3>登录页面</h3>
    <form action="login">
        用户名: <input type="text" name="username"><br>
        密码: <input type="password" name="password"><br>
        <input type="submit" value="提交">
    </form>
</body>
</html>
```



login_ok.jsp 文件内容如下：

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>登录成功</title>
</head>
<body>
    <h1>恭喜！登录成功</h1>
</body>
</html>
```



src 目录结构如下：

```
src                            
├─ com                         
│  └─ xzh                      
│     └─ web                   
│        └─ UserServlet.java   
├─ applicationContext-mvc.xml  
└─ Main.java                   
```



applicationContext-mvc.xml (spring 的配置文件)如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
    <!--配置自动扫描的包-->
    <context:component-scan base-package="com.xzh.web"/>
    <!--配置视图解析器-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/pages/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
</beans>
```

- 配置了需要扫描的包，将添加了 `@Component`、`@Repository` 等注解的 bean 对象注入 IOC 容器
- 配置视图解析器，其中设置了 `prefix`（前缀）和 `suffix` （后缀），对设置了 `RequstMapping` 的 servlet 中处理后的跳转路由为 prefix + 返回值 + suffix



UserServlet 的代码如下：

```java
package com.xzh.web;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

/**
 * 当我们使用了 SpringMVC 后， 在一个类上标识 @Controller
 * 表示将该类视为一个控制器，注入到容器中
 * 相较于原生的 Servlet ，能简化开发
 */
@Controller
public class UserServlet {
    /**
     * 1. login 方法用于响应用户的登录请求
     * 2. value = "/login" 相当于以前原生 Servlet 中使用注解 webSocket 中 value，用于处理 /login 路由
     * 3. 用户在浏览器中输入 http://localhost:8080/web工程路径/login 时会被该方法拦截处理
     * 4.  return "login_ok 会将返回结果交给    视图解析器 InternalResourceViewResolver，视图解析器会根据配置自动拼接 prefix + login_ok + suffix 得到完整路径
     */
    @RequestMapping(value = "/login")
    public String login() {
        System.out.println("login finished");
        return "login_ok";
    }
}
```



:warning:**注意事项**

1. 这里的 UserServlet <strong style="color:red">只能使用注解 `@Controller`</strong>，我们称之为一个 Handler 处理器

   > 千万不要误用注解 `@Component`，血的教训

2. UserServlet 指定 url 时，可以省略 value

3. 关于 SpringMVC 的 DispatcherServlet 的配置文件，如果不在 web.xml 中指定 applicationContext-mvc.xml，默认在 `/WEB-INF/springDispatcherServlet-servlet.xml` 中找这个配置文件（此时，我们可以不用设置 init-param）

   > 这里要注意：springDispatcherServlet 的前缀即 springDispatcher 是因为这里设置的 sevlet-name 为 springDispatcher ，它并不是固定只能是这个名字，我们查看 Dispatcher 源码，可以看到：
   >
   > ![image-20230424174936568](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230424174936568.png)
   >
   > 如果给一个 servlet 的 name 值为 "test"，那么默认解析的文件 test-servlet.xml 文件



## 1.3 springMVC 的执行流程

![image-20230424204835423](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230424204835423.png)



## 1.4 RequestMapping

`@RequestMapping` 注解可以修饰方法，还可以修饰类，<strong style="color:red">当同时修饰类和方法时，请求的 url 就是 `\类请求值\方法请求值`</strong>，示例如下：

```java
@Controller
@RequestMapping("/user")
public class UserHandler {
    /**
     * 1. method = RequestMethod.POST：表示请求的 buy 方法目标方法必须是 post 方法
     * 2. RequestMethod 四个常用选项分别是：POST、GET、PUT、DELETE
     * 3. SpringMVC 控制器默认支持 GET 和 POST 两种方式
     * 4. buy 方法的请求路径为 http://localhost:port/applicationContext/user/buy
     */
    @RequestMapping(value = "/buy", method = RequestMethod.POST)
    public String buy() {
        System.out.println("购买商品");
        return "success";
    }
}
```

> 如果我们指定了请求方法，那么就必须使用指定的请求方式，否则会报 "405 Method Not Allowed"，客户请求方法被禁止 的错误



### 1.4.1 `@RequestMapping` 指定 params 

`RequestMapping` 中可以通过 params 参数指定要传入哪些参数和不要传入那些参数，以及要传入参数的值必须是什么或不能是什么，接下来我们举例说明：

```java
// 可以通过 get / post 方法传入参数，并且一定要携带参数 bookId，传入的 bookId 又会直接当做方法的参数列表中的一员
@RequestMapping(value = "/search", params = "bookId")
public String  search(String bookId) {
    System.out.println("@@BookId = " + bookId);
    return "success";
}
```

![image-20230424221035840](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230424221035840.png)

而如果当我们缺少 `bookId` 这个参数时，服务器就会返回 400，即 "Bad Request"，表示客户端请求的语法错误，服务器无法理解

我们还可以通过下面的方法指定值，如果传入的参数值不对，则仍然会报 400

```java
// 限制传入的 bookId 的值只能为 100
@RequestMapping(value = "/search", params = "bookId=100 ")	

// 限制传入的参数不能为 bookId
@RequestMapping(value = "/search", params = "!bookId")

// 限制传入的参数必须包含 bookId 和 bookName
@RequestMapping(value = "/search", params = {"bookId", "bookName"})
```



### 1.4.2 `@RequestMapping` 支持 Ant 路径风格

1. `?`：匹配文件名中的一个字符
2. `*`：匹配文件名中任意字符
3. `**`：匹配多层路径

示例如下：

- `/user/*/createUser`: 匹配 `/user/aaa/createUser`、`/user/bbb/createUser` 等 URL 
- `/user/**/createUser`: 匹配 `/user/createUser`、`/user/aaa/bbb/createUser` 等 URL 
- `/user/createUser??`: 匹配 `/user/createUseraa`、`/user/createUserbb` 等 URL

例如：对于下面的匹配规则表示可以匹配 `/search`、`/search/aa`、`/search/aa/bb` 等路径

```java
@RequestMapping(value = "/search/**", params = "bookId")
```



### 1.4.3 `@RequestMapping` 可配合 `@PathVariable` 绑定映射 URL 绑定的占位符

`@RequestMapping` 还可以配合 `@PathVariable` 映射 URL 绑定的占位符，这样就不需要在 URL 地址上带参数名，显得更加简洁明了

**例如：我们想要从请求或获取 username 和 gender**

```java
/**
 * 客户端可以直接通过 /reg/xxx/xxx 来访问并传入参数 username 和 gender(这里的username和gender我们称为路径变量)
 * handler 这里使用时需要在参数列表前加上注解 @PathVariable
 */
@RequestMapping(value = "/reg/{username}/{gender}")
public String register(@PathVariable String username, @PathVariable String gender) {
    System.out.println("username = " + username + " ," + "gender = " + gender);
    return "success";
}
// 第二种写法
public String register(@PathVariable("username") String name, @PathVariable("gender") String sex) {
    System.out.println("name = " + name + " ," + "sex = " + sex);
    return "success";
}
```



### 1.4.4 `@RequestMapping` 的注意事项

1. 映射的 URL ，不能重复。以下为错误示例

   ```java
   @RequestMapping(value = "/ref")
   public void f1() {}
   
   @RequestMapping(value = "/ref")
   public void f2() {}
   ```

2. 各种请求也有对应的简写形式

   - `@RequestMapping(value = "/buy", method = RequestMethod.POST)` 等价于 `@PostMapping(value = "/buy")`
   - 除此之外，还有：`GetMapping`、`PostMapping`、`@PutMapping`、`@DeleteMapping`、`PatchMapping`

3. 如果我们确定表单或者超链接会提交某个字段数据例如 `email` ，那么要求提交的参数名和目标方法的参数名保持一致

   ```java
   @RequestMapping(value = "/sell")
   public String sellGoods(String goodsId) {
       System.out.println("将要被卖出去的商品ID = " + goodsId);
       return "success";
   }
   ```

   ![image-20230424235931708](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230424235931708.png)

   > 这里要注意的是：即使参数名不保持一致，在项目工程中也不会报错，只是会被当做 null 值处理



## 1.5 Postman

### 1.5.1 Postman 介绍

Postman 是一款由 Google 打造的功能强大的用于<strong style="color:red">发送 HTTP 请求的测试工具</strong>

> 也可以使用国内的 ApiPost



### 1.5.2 使用方法

一些常用快捷键：

- `Ctrl + =`：放大界面
- `Ctrl + -`：缩小界面
- `Ctrl + Enter`：发送请求

Postman 将所有的请求（request）都放在集合（collection）中，其实就是一个文件夹，用于管理一类请求

> 注意：只有在联网的条件下才能创建 collection

工作区如下：

![image-20230425001752783](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230425001752783.png)

![image-20230425002324221](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230425002324221.png)



# 2 Rest

## 2.1 Rest 基本介绍

1. REST：英文全称为 "Representational State Transfer"，（资源）表现层状态转化。是目前流行的请求方式，结构清晰，被用于很多网站
2. HTTP 协议中，最常用的四个请求方法：
   - GET：获取资源
   - POST：新建资源
   - PUT：更新资源
   - DELETE：删除资源
3. 
