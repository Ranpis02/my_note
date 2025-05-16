[toc]

# 1 异常处理

## 1.1 基本介绍

1. 默认情况下，Spring Boot 提供 `/error` 处理所有错误的映射，也就是说当出现错误时，Spring  Boot 底层会**请求转发**到 `/error` 这个映射

2. 比如使用浏览器访问不存在的接口（路径映射）时，会响应一个 "Whitelabel Error Page" 的页面，以 HTML 格式呈现给用户，这个页面对应的 `/error` 

   ![image-20230813155305989](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230813155305989.png)

3. Spring Boot 底层默认由 DefaultErrorViewResolver 处理错误

处理流程大致是：当访问 url 没有与之相关的映射时，便会到静态资源目录下寻找 /error/404.html，如果仍然没有找到，就会退而求其次，寻找 /error/4xx.html，如果还没有找到，就会主动创建一个 error 的 ModelAndView 



## 1.2 拦截器和过滤器之间的区别

1. 使用范围不同

   - 过滤器实现的是 javax.servlet.Filter 接口，而这个接口是在 Servlet 规范中定义的，也就是说过滤器的使用要依赖于 Tomcat 等容器，Filter 只能在 web 程序中使用
   - 拦截器（Interceptor）是一个 Spring 组件，并由 Spring 容器管理，并不依赖于 Tomcat 等容器，可以单独使用，不仅能应用在 web 程序中，也可以用于 Application 等程序中

2. 触发时机不同

   ![img](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/c0d9ebd67cd2e22667110755bd0e4b65.png)

   1. 过滤器 Filter 是在请求进入容器后，在进入 servlet 前进行预处理，请求结束是在 servlet 处理完以后
   2. 拦截器 Interceptor 是在请求进入 servlet 后，在进行 Controller 之前进行预处理，Controller 中渲染了对应的视图之后请求结束

   > 过滤器不会处理请求转发，但是拦截器会处理请求转发



## 1.3 自定义异常页面

由于 Spring Boot 支持 Thymeleaf 解析引擎，所以我们可以创建 Thymeleaf 视图，目录结构如下：

```
resources           
├─ static           
├─ templates        
│  └─ error         
│     ├─ 404.html   
│     ├─ 4xx.html: 请求的资源没有找到并且当 400.html 页面没有找到时，就会跑去寻找 4xx.html 页面
│     ├─ 500.html   
│     └─ 5xx.html: 服务器发生错误并且 500.html 页面没有找到时，就会跑去寻找 5xx.html   
└─ application.yml  
```

这里需要注意，我们一定要引入相应的支持 thymeleaf 引擎的依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

 基本流程为：当发生 404 错误的请求时，服务端首先会去寻找是否存在 404.html 视图，如果找到则响应该 404.html 页面，如果没有找到则寻找 4xx.html 的视图，如果找到则返回，如果没有找到，则会创建一个默认的视图用以返回，该页面会将状态码和错误信息打印出来，其他的错误请求，例如 405 method not allowed 和 500 等也是同样的道理



## 1.4 全局异常

说明

1. 在 Spring Boot 中，主要是通过 `@ControllerAdvice + @ExceptionHandler` 处理全局异常
2. 底层是 ExceptionHandlerExceptionResolver

> 全局异常是站在程序异常的角度看待的，例如如果服务器内部抛出空指针异常或者算术异常，那么就会出现 500，如果我们想要更加具体的指明错误可以使用全局异常

创建 exception 包，然后在该包下创建 GlobalExceptionHandler 类，具体实现代码如下：

```java
/**
 * 使用 @ControllerAdvice 注解可以标识一个全局异常处理器，并将下面的 bean 注入到容器中
 */
@Slf4j
@ControllerAdvice
public class GlobalExceptionHandler {
    /**
     * 下面的方法主要用于处理算术异常
     * @param e 表示异常发生之后，传递的异常对象
     * @param model 我们可以将异常信息放入到 model 中，并传递给显示页面
     */
    @ExceptionHandler({ArithmeticException.class})
    public String handleArithmeticException(Exception e, Model model) {
        log.info("异常信息为：{}", e.getMessage());
        model.addAttribute("msg", e.getMessage());
        return "/error/global";
    }

    /**
     * 下面的方法主要用于处理空指针异常

     */
    @ExceptionHandler({NullPointerException.class})
    public String handleNullPointerException(Exception e, Model model) {
        log.info("异常信息为：{}", e.getMessage());
        model.addAttribute("msg", e.getMessage());
        return "/error/global";
    }
}
```

我们到源码中进行查看，大致的处理机制是：先走的 doResolveHandlerMethodException 方法，这个方法是在发生异常时被执行，后面的两个参数 handlerMethod 表示异常所在的方法，exception 是异常信息，会交给 getExceptionHandlerMethod 方法去处理异常，如果没有找到对应的处理器，那么就会执行默认的异常处理机制

**获取异常发生的方法**

这个很简单，只需要在参数中添加 HandlerMethod 参数即可

```java
@ExceptionHandler({ArithmeticException.class})
public String handleArithmeticException(Exception e, Model model, HandlerMethod handlerMethod) {
    Method method = handlerMethod.getMethod();
    System.out.println(method);
    return "/error/global";
}
```



## 1.5 自定义异常

1. 如果 Spring Boot 提供的异常不能满足开发需求，程序员也可以自定义异常
2. `@ResponseStatus+ 自定义异常`
3. 底层是 `ResponseStatusExceptionResolver`，底层是调用的 `response.sendError(statusCode, resolvedReason)`
4. 当抛出自定义异常后，仍然会根据状态码，去匹配使用 xxx.html 显示

模拟使用场景

需求：自定义一个异常 AccessException，当用户访问某个无权访问的路径时，抛出该异常，显示自定义异常状态码

```java
/**
 * value = HttpStatus.FORBIDDEN 表示发生 AccessException 异常时，我们通过 Http 协议返回的状态码 403
 * 这个状态码和自定义异常的对应关系是由程序员指定的
 */
@NoArgsConstructor
@ResponseStatus(HttpStatus.FORBIDDEN)
public class AccessException extends RuntimeException {
    public AccessException(String msg) {
        super(msg);
    }
}
```

然后我们就可以在逻辑代码中通过 throw 抛出该异常

注意事项

1. 如果我们将自定义异常类型，放在全局异常处理器，那么仍然会走全局异常处理机制

   > 全局异常处理优先级 > 默认异常处理机制



# 2 注入 Servlet、Filter、Listener

1. Spring Boot 支持将 Servlet 、Filter、Listener 注入到 Spring 容器中，成为 Spring Bean
2. 也就是说 Spring Boot 开放了和原生 web 组件的兼容



## 2.1 注入 servlet

首先，我们需要编写一个原生的 Servlet

```java
/**
 * 1. 通过继承 HttpServlet 来开发原生的 Servlet
 * 2. 通过注解 @WebServlet 标识 Servlet 对象
 * 3. urlPattern 是对servlet的配置项
 */
@WebServlet(urlPatterns = {"/servlet01", "/servlet02"})
public class MyServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.getWriter().write("hello, servlet");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```



然后，需要在 MainApp 中使用注解 `@ServletComponentScan` 将 servlet 注入到容器中

```java
@ServletComponentScan(basePackages = {"com.example.demo.servlet"})
@SpringBootApplication
public class Demo1Application {
    public static void main(String[] args) {
        SpringApplication.run(Demo1Application.class, args);
    }
}
```



## 2.2 注入 Filter

```java
@Slf4j
@WebFilter(urlPatterns = {"/css/*", "/images/*"})
public class MyFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        log.info("init--");
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        // 获取过滤的 uri
        HttpServletRequest req = (HttpServletRequest)servletRequest;
        String requestURI = req.getRequestURI();
        log.info("过滤的 URI = " + requestURI);
        // 放行
        filterChain.doFilter(servletRequest, servletResponse);
    }

    @Override
    public void destroy() {
        log.info("destroy--");
    }
}
```

> 注意：
>
> 1. servlet 匹配全部都是 `/*` ，而 Spring Boot 是 `/**`
> 2. 同样地，我们也需要使用 `@ServletComponentScan` 将其注入到容器



## 2.3 注入 Listener

```java
@Slf4j
@WebListener
public class MyListener implements ServletContextListener {

    @Override
    public void contextInitialized(ServletContextEvent sce) {
        log.info("ServletContextListener 初始化完成...");
    }

    @Override
    public void contextDestroyed(ServletContextEvent sce) {
        log.info("ServletContextListener 销毁完成...");
    }
}
```

> Listener 的注入比较简单，只需要添加 `@WebListener` 注解标识，然后使用 `ServletComponentScan` 扫描注入即可



## 2.4 使用 RegistrationBean 方式注入

```java
@Configuration
public class RegisterConfig {
    /**
     * 注入 servlet
     */
    @Bean
    public ServletRegistrationBean<MyServlet> myServlet() {
        MyServlet myServlet = new MyServlet();
        return new ServletRegistrationBean<>(myServlet, "/servlet01", "/servlet02");
    }

    /**
     * 注入 filter
     */
    @Bean
    public FilterRegistrationBean<MyFilter> myFilter() {
        MyFilter myFilter = new MyFilter();
        FilterRegistrationBean<MyFilter> filterBean = new FilterRegistrationBean<>(myFilter);
        // 设置 url-pattern
        filterBean.setUrlPatterns(Arrays.asList("/css/*", "/images/*"));
        return filterBean;
    }

    /**
     * 注入 Listener
     */
    @Bean
    public ServletListenerRegistrationBean<MyListener> myListener() {
        MyListener myListener = new MyListener();
        return new ServletListenerRegistrationBean<>(myListener);
    }
}
```



## 2.5 思考：请求 Servlet 为什么不会到达拦截器？

- 注入的 Servlet 会存在于 Spring 容器
- DispatherServlet 也存在于 Spring 容器

![image-20230814115543797](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230814115543797.png)



# 3 内置 Tomcat 

## 3.1 基本说明

Spring Boot 支持服务器：Tomcat、Jetty、Undertow

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-tomcat</artifactId>
  <version>2.7.14</version>
  <scope>compile</scope>
</dependency>
```



Tomcat 常用配置

对于 Tomcat 的配置，我们可以查看 ServerProperties 

<img src="https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230814150250630.png" alt="image-20230814150250630" style="zoom:50%;" />

常用配置如下：

```yaml
server:
  port: 9999  # 配置端口号
  tomcat:
    threads:
      max: 200  # 最大的工作线程数，默认是 200
      min-spare: 5  # 最小的工作线程，默认是 10
    accept-count: 100 # tomcat 启动的线程达到最大值，接受请求排队的个数，默认 100
    max-connections: 8912 # 最大连接数，默认是 8192
    connection-timeout: 10000 # 建立连接的超时时间，单位是毫秒
```



通过类来修改配置

由于在某些情况下，我们是无法修改 application.yml 文件，这个时候如果我们想要修改配置，那么就需要使用类来修改相应配置

```java
@Component
public class CustomServer implements WebServerFactoryCustomizer<ConfigurableServletWebServerFactory> {
    @Override
    public void customize(ConfigurableServletWebServerFactory factory) {
        // 设置 server 的服务端端口
        factory.setPort(8081);
    }
}
```

> 通过类来进行设置，可设置配置相较于 application.yml 配置文件配置可选项会缺少很多



## 3.2 切换 WebServer

修改 pom.xml 文件，排除 tomcat，之后再引入 undertow 依赖即可

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <exclusions>
            <!--排除 tomcat-->
            <exclusion>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-tomcat</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
    <!--引入 undertow-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-undertow</artifactId>
    </dependency>
</dependencies>
```



# 4 数据库操作

## 4.1 原生 JDBC

Spring Boot 中默认是使用 JDBC + HikariDatasource 完成对 MySQL 的操作

> HikariDataSource 是目前市面上非常优秀的数据源，是 Spring Boot2 的默认数据源

接下来将演示如何通过 JDBC + HikariDataSource 完成对 MySQL 的操作

01 创建家具表

```sql
create table furn
(
    id       int auto_increment comment '主键' primary key,
    name     varchar(64)    not null comment '家具名',
    maker    varchar(64)    not null comment '制作商',
    price    decimal(11, 2) not null comment '价格',
    sales    int            not null comment '销量',
    stock    int            not null comment '库存',
    img_path varchar(256)   not null comment '图片路径'
)comment '家具表' collate = utf8mb4_unicode_ci;
```

02 插入一些预置数据

```sql
INSERT INTO `furn`(`id`, `name`, `maker`, `price`, `sales`, `stock`, `img_path`) VALUES(NULL, '北欧风格小椅子', '熊猫家具', 200, 100, 188, 'images/1.png');
INSERT INTO `furn`(`id`, `name`, `maker`, `price`, `sales`, `stock`, `img_path`) VALUES(NULL, '简约风格小桌子', '蚂蚁家具', 180, 66, 88, 'images/2.png');
INSERT INTO `furn`(`id`, `name`, `maker`, `price`, `sales`, `stock`, `img_path`) VALUES(NULL, '温馨风格盆景架', '海豚家具', 300, 166, 288, 'images/3.png');
```



03 在 pom.xml 中引入 data-jdbc starter

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jdbc</artifactId>
</dependency>
```



04 选择数据库驱动为 MySQL

> 由于 Spring Boot 不知道项目需要操作的是 MySQL 还是 Oracle，所以我们需要引入相关的数据库驱动，并且这个数据库驱动的版本需要与我们本地开启的 MySQL 服务的版本相同

```xmnl
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.33</version>
</dependency>
```

> 注意：这里的 version 也可以使用 ${mysql.version} 替代



05 在 application.yml 中进行配置数据源

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/spring_boot?useUnicode=true&characterEncoding=UTF-8&useSSL=true
    username: root
    password: 123456
    driver-class-name: com.mysql.cj.jdbc.Driver
```



06 创建实体类

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Furn {
    /*主键*/
    private int id;
    /*家具名*/
    private String name;
    /*制造商*/
    private String maker;
    /*价格*/
    private BigDecimal price;
    /*销量*/
    private Integer sales;
    /*库存*/
    private Integer stock;
    /*图片url*/
    private String imgPath = "images/1.png";
}
```



07 接下来，我们需要在 Spring Boot 环境下进行测试，测试前需要引入相关依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```

测试程序

```java
@SpringBootTest
@Slf4j
class Demo1ApplicationTests {
    // 注入 jdbcTemplate
    @Resource
    private JdbcTemplate jdbcTemplate;

    @Test
    void contextLoads() {
        RowMapper<Furn> mapper = new BeanPropertyRowMapper<>(Furn.class);
        String sql = "SELECT * FROM `furn`";
        List<Furn> furnList = jdbcTemplate.query(sql, mapper);
        for(Furn furn : furnList) {
            log.info(furn.toString());
        }
        // 查看底层所使用的数据源类型
        log.info(Objects.requireNonNull(jdbcTemplate.getDataSource()).getClass().toString());	// class com.zaxxer.hikari.HikariDataSource
    }
}
```

>`Objects.requireNonNull` 用于进行非空判断，如果为空则会抛出空指针异常，源码如下
>
>```java
>public static <T> T requireNonNull(T obj) {
>    if (obj == null)
>        throw new NullPointerException();
>    return obj;
>}
>```



## 4.2 整合 Druid

> Druid github 网址：https://github.com/alibaba/druid

基本介绍

- Druid 作为数据库连接池，性能优秀，且集成了 SQL 监控，黑名单拦截等功能
- 整合 Druid 到 Spring Boot 方法
  - 自定义方式
  - 引入 starter 方式

接下来，我们来完成一下自定义方式

01 引入 druid 依赖

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.2.18</version>
</dependency>
```



02 创建配置类

```java
@Configuration
public class DruidDataSourceConfig {
    /**
     * 编写方法，注入 dataSource
     * 同时要使用 @ConfigurationProperties 这个注解将 druid 域默认的 HikariDataSource 关联起来
     */
    @Bean
    @ConfigurationProperties("spring.datasource")
    public DataSource dataSource() {
        return new DruidDataSource();
    }
}
```

在这里提出两个问题：

1. 为什么使用 `@ConfigurationProperties` 后，DruidDataSource 中 url 、diverclass-name 等一系列配置就自动注入了？

   >底层还是用到 setter 进行装配，我们可以到 DruidAbstractDataSource 这个类中进行查看

2. 为什么当我们配置了 ruidDataSource 后，原来的 HikariDataSource 就失效了？

   > 到 DataSourceAutoConfiguration 这个类中进行查看，我们可以看到下面的注解
   >
   > ```java
   > @ConditionalOnMissingBean({ DataSource.class, XADataSource.class })
   > ```
   >
   > 一看到该注解就一目了然，当存在 DataSource 时就会使用已有的 DataSource，如果不存在 DataSource ，就会使用注入默认的 HikariDataSource 



## 4.3 Druid 监控功能

官方文档说明：

Druid内置提供了一个StatViewServlet用于展示Druid的统计信息。

这个StatViewServlet的用途包括：

- 提供监控信息展示的html页面
- 提供监控信息的JSON API

> 注意：使用StatViewServlet，建议使用druid 0.2.6以上版本。

添加相应的配置类

```java
@Configuration
public class DruidDataSourceConfig {
    /**
     * 编写方法，注入 dataSource
     * 同时要使用 @ConfigurationProperties 这个注解将 druid 域默认的 HikariDataSource 关联起来
     */
    @Bean
    @ConfigurationProperties("spring.datasource")
    public DataSource dataSource() {
        return new DruidDataSource();
    }

    /**
     * 配置 druid 的监控页
     */
    @Bean
    public ServletRegistrationBean<StatViewServlet> statViewServlet() {
        StatViewServlet statViewServlet = new StatViewServlet();
        ServletRegistrationBean<StatViewServlet> reg = new ServletRegistrationBean<>(statViewServlet, "/druid/*");
        // 允许清空统计数据
        reg.addInitParameter("resetEnable", "true");
        // 设置用户名
        reg.addInitParameter("loginUsername", "admin");
        // 设置密码
        reg.addInitParameter("loginPassword", "admin");
        return reg;
    }
}
```

然后我们通过浏览器，访问 http://localhost:8080/druid/login.html 便可来到其登录页面



### 4.3.1 SQL 监控

配置 StatFilter，开启监控功能，参考文档：https://github.com/alibaba/druid/wiki/%E9%85%8D%E7%BD%AE_StatFilter，参考代码如下：

```java
@Configuration
public class DruidDataSourceConfig {
    @Bean
    @ConfigurationProperties("spring.datasource")
    public DataSource dataSource() throws SQLException {
        DruidDataSource dataSource = new DruidDataSource();
        // 加入监控功能
        dataSource.setFilters("stat");
        return dataSource;
    }
}
```

然后我们再添加一个控制层，用来进行 sql 监控的测试，代码如下：

```java
/**
 * 注意：下面代码仅是为了演示用，真实开发过程中不会再 controller 层直接操作数据库
 */
@Controller
public class SqlMonitor {
    @Resource
    private JdbcTemplate jdbcTemplate;

    @GetMapping("/monitor")
    @ResponseBody
    public List<Furn> monitor() {
        RowMapper<Furn> mapper = new BeanPropertyRowMapper<>(Furn.class);
        String sql = "SELECT * FROM `furn`";
        return jdbcTemplate.query(sql, mapper);
    }
}
```

当我们发送相关请求后，看以看到 sql 监控面板就会有记录产生

![image-20230814220611907](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230814220611907.png)

各参数所代表的含义请参考：https://www.bookstack.cn/read/Druid/2fa0c5cdf8a9e77e.md#3.7%20%E5%8C%BA%E9%97%B4%E5%88%86%E5%B8%83

对于区间分布，文档上的描述是：

SQL监控项上，执行时间、读取行数、更新行数都有区间分布，将耗时分布成8个区间：

- 0 - 1 耗时0到1毫秒的次数
- 1 - 10 耗时1到10毫秒的次数
- 10 - 100 耗时10到100毫秒的次数
- 100 - 1,000 耗时100到1000毫秒的次数
- 1,000 - 10,000 耗时1到10秒的次数
- 10,000 - 100,000 耗时10到100秒的次数
- 100,000 - 1,000,000 耗时100到1000秒的次数
- 1,000,000 - 耗时1000秒以上的次数
  记录耗时区间的发生次数，通过区分分布，可以很方便看出SQL运行的极好、普通和极差的分布。耗时区分分布提供了“执行+RS时分布”，是将执行时间+ResultSet持有时间合并监控，这个能方便诊断返回行数过多的查询。



### 4.3.2 web 关联监控

> web 监控配置参考：https://github.com/alibaba/druid/wiki/%E9%85%8D%E7%BD%AE_%E9%85%8D%E7%BD%AEWebStatFilter

```java
@Configuration
public class DruidDataSourceConfig {
    /**
     * 编写方法，注入 dataSource
     * 同时要使用 @ConfigurationProperties 这个注解将 druid 域默认的 HikariDataSource 关联起来
     */
    @Bean
    @ConfigurationProperties("spring.datasource")
    public DataSource dataSource() throws SQLException {
        DruidDataSource dataSource = new DruidDataSource();
        // 加入监控功能
        dataSource.setFilters("stat");
        return dataSource;
    }

    /**
     * 配置 druid 的监控页
     */
    @Bean
    public ServletRegistrationBean<StatViewServlet> statViewServlet() {
        StatViewServlet statViewServlet = new StatViewServlet();
        ServletRegistrationBean<StatViewServlet> reg = new ServletRegistrationBean<>(statViewServlet, "/druid/*");
        // 允许清空统计数据
        reg.addInitParameter("resetEnable", "true");
        // 设置用户名
        reg.addInitParameter("loginUsername", "admin");
        // 设置密码
        reg.addInitParameter("loginPassword", "admin");
        return reg;
    }

    /**
     * 配置 web 监控
     */
    @Bean
    public FilterRegistrationBean<WebStatFilter> webStatFilter() {
        WebStatFilter webStatFilter = new WebStatFilter();
        FilterRegistrationBean<WebStatFilter> reg = new FilterRegistrationBean<>(webStatFilter);
        // 添加过滤的 url
        reg.setUrlPatterns(Collections.singletonList("/*"));
        // 排除指定的 url
        reg.addInitParameter("exclusions", "*.js,*.gif,*.jpg,*.png,*.css,*.ico,/druid/*");
        return reg;
    }
}
```

![image-20230814223737585](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230814223737585.png)



### 4.3.3 sql 防火墙

> 防火墙配置参考文档：https://github.com/alibaba/druid/wiki/%E9%85%8D%E7%BD%AE-wallfilter

```java
@Configuration
public class DruidDataSourceConfig {
    /**
     * 编写方法，注入 dataSource
     * 同时要使用 @ConfigurationProperties 这个注解将 druid 域默认的 HikariDataSource 关联起来
     */
    @Bean
    @ConfigurationProperties("spring.datasource")
    public DataSource dataSource() throws SQLException {
        DruidDataSource dataSource = new DruidDataSource();
        // 加入监控功能和防火墙
        dataSource.setFilters("stat,wall");
        return dataSource;
    }
}
```



### 4.3.4 session 监控

session 监控不需要我们进行配置，我们只要一登录就能看到相应的 session 监控记录



## 4.4 引入 starter 整合 Druid

引入 ==Druid Spring Boot Starter== 依赖

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.2.18</version>
</dependency>
```

在 application.yml 文件中进行相应的配置

```yaml
spring:
  # 当我们使用了 druid 的 starter 后，可以直接配置数据源
  datasource:
    url: jdbc:mysql://localhost:3306/spring_boot?useUnicode=true&characterEncoding=UTF-8&useSSL=true
    username: root
    password: 123456
    driver-class-name: com.mysql.cj.jdbc.Driver
    # 配置 druid 和监控功能
    druid:
      stat-view-servlet:
        enabled: true
        login-username: admin
        login-password: admin
        reset-enable: true
```

经过上述操作后，我们的 Druid 就已经整合完成了，接下来我们只需要配置相应的监控（过滤器）即可，完整配置如下：

```yaml
spring:
  # 当我们使用了 druid 的 starter 后，可以直接配置数据源
  datasource:
    url: jdbc:mysql://localhost:3306/spring_boot?useUnicode=true&characterEncoding=UTF-8&useSSL=true
    username: root
    password: 123456
    driver-class-name: com.mysql.cj.jdbc.Driver
    # 配置 druid 和监控功能
    druid:
      stat-view-servlet:
        enabled: true
        login-username: admin
        login-password: admin
        reset-enable: true
      # 配置 web 监控
      web-stat-filter:
        enabled: true
        url-pattern: /*
        exclusions: "*.js,*.gif,*.jpg,*.png,*.css,*.ico,/druid/*"
      # 配置 sql 监控
      filter:
        stat:
          enabled: true
          # 设置慢查询为 1s
          slow-sql-millis: 1000
          log-slow-sql: true
        # 配置sql防火墙
        wall:
          enabled: true
          config:
            # 不允许出现删除表的操作
            drop-table-allow: false
            # 不允许全查询
            select-all-column-allow: false
```



# 5 Spring Boot 整合 Mybatis

## 5.1 环境搭建

```xml
<!--web 场景-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>

<!--mybatis 场景-->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.2.2</version>
</dependency>

<!--mysql 驱动-->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.33</version>
</dependency>

<!--配置处理器（用来解决yaml配置文件提示问题）-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
</dependency>
```



编写 mapper 接口和对应的 xml 文件

> `xxxMapper.xml` 最好放在 resources/mapper 下

```java
/**
 * 利用 @Mapper 注解直接将其注入到容器中
 */
@Mapper
public interface FurnMapper {
    /**
     * 根据 id 返回 Monster 对象
     */
    public Furn getFurnById(int id);
}
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.example.demo.mapper.FurnMapper">
    <select id="getFurnById" resultType="com.example.demo.entity.Furn">
        select * from furn where id = #{id}
    </select>
</mapper>
```



然后到 application.yaml 中指定扫描的 xml 路径

```yaml
mybatis:
  # 指定要扫描的 XxxMapper.xml 文件
  mapper-locations: classpath:mapper/*.xml
```

我们在以前使用 mybatis 时，需要我们自己创建一个 mybatis-config.xml 文件，在 Spring Boot 项目中仍然可以这么使用

```yaml
mybatis:
  # 指定要扫描的 XxxMapper.xml 文件
  mapper-locations: classpath:mapper/*.xml
  # 指定 mybatis-config.xml 文件
  config-location: classpath:mybatis-config.xml
  # 也可以直接在 application.yml 中进行配置
  # 例1：配置类型别名
  type-aliases-package: com.example.demo.entity
  # 例2：配置日志门面
  configuration:
    log-impl: org.apache.ibatis.logging.slf4j.Slf4jImpl
    # 例3：启用自动驼峰配置
    map-underscore-to-camel-case: true
```



## 5.2 @JsonFormat

我们在前面会发现，日期字段在显示过程中会出现日期问题，由于北京位于东八区，所以会与默认的 GMT 相差 8 个小时，此时我们可以通过 `@JsonFormat` 注解即可

```java
@NoArgsConstructor
@AllArgsConstructor
@Data
public class Monster {
    @JsonFormat(pattern = "yyyy-MM-dd", timezone = "GMT+8")
    private Date birthday;
}
```



# 6 整合 mybatis-plus

## 6.1 Mybatis-plus 的基本介绍

1. Mybatis-plus，简称为 MP，是一个 MyBatis 增强工具，在 MyBatis 基础上只做增强，不做改变，为简化开发、提高效率而生
2. Mybatis-plus 拥有强大的 CRUD 操作：内置通用 Mapper、通用 Service，通过少量配置即可实现单表大部分 CRUD 操作，更有强大的条件构造器，满足各类使用需求

## 6.2 mp 的使用

在使用之前，我们需要先引入相关的依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.xzh</groupId>
    <artifactId>untitled</artifactId>
    <version>1.0-SNAPSHOT</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.7.14</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <dependencies>
        <!-- web starter -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!--mysql 驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>${mysql.version}</version>
        </dependency>

        <!--spring boot 环境下的测试依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

        <!--配置处理器（用来解决yaml配置文件提示问题）-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
        </dependency>

        <!--druid 场景依赖-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.2.18</version>
        </dependency>

        <!--mybatis-plus 场景依赖-->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.5.3.2</version>
        </dependency>
        
        <!--lombok-->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
    </dependencies>
</project>
```

配置 application.yml 

```yaml
server:
  port: 8081
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/spring_boot?useUnicode=true&characterEncoding=UTF-8&useSSL=true&serverTimezone=Asia/Shanghai
    username: root
    password: 123456
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```



我们拿以前的一张表来做演示使用

```sql
create table monster
(
    id       int auto_increment
        primary key,
    name     varchar(255)     not null,
    age      tinyint unsigned not null,
    gender   char             not null comment '0=female,1=male',
    birthday date             null,
    email    varchar(255)     not null,
    salary   double           not null
);
```

对应的 entity

```java
/**
 * 怪兽实体
 */
@Data
@NoArgsConstructor
@AllArgsConstructor
public class Monster {
    /*怪兽 ID */
    private int id;
    /*怪兽名*/
    private String name;
    /*怪兽年龄*/
    private Integer age;
    /*怪兽性别。0=female,1=male*/
    private String gender;
    /*怪兽生日*/
    @JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss", timezone = "GMT+8")
    private Date birthday;
    /*怪兽电子邮件*/
    private String email;
    /*怪兽薪水*/
    private Double salary;
}
```

添加对应的 mapper 接口

```java
/**
 * 当继承了 BaseMapper 之后就可以直接使用内置的众多方法
 */
@Mapper
public interface MonsterMapper extends BaseMapper<Monster> {
}
```

添加 service 层以及对应的实现类

```java
/**
 * 1. 在传统方法中，我们需要在接口中定义方法，然后在实现类中进行实现
 * 2.在 mybatis-plus 中，我们可以继承父接口 IService
 * 3. 这个 IService 中声明了很多方法，足够我们日常开发使用
 */
public interface MonsterService extends IService<Monster> {
}
```

```java
/**
 * 1. 在传统方式中，我们需要实现 service 接口
 * 2. 在 mp 中，我们开发 service 只需要继承 serviceImpl
 * 3. 我们以 MonsterService 为例，MonsterService 继承 IService ，IService 接口内置了我们常用的 CRUD 操作
 * 之后我们在 MonsterServiceImpl 这个实现类实现 MonsterService 同时继承了 ServiceImpl 该实现类，ServiceImpl 已经
 * 实现了 IService 的方法，所以我们自定义的 MonsterServiceImpl 就不需要再去实现
 */
@Service
public class MonsterServiceImpl extends ServiceImpl<MonsterMapper, Monster> implements MonsterService {

}
```

添加 controller 层

```java
@Controller
public class MonsterController {
    @Resource
    private MonsterService monsterService;

    /**
     * 根据 id 返回对应的对象
     */
    @GetMapping("/monster")
    @ResponseBody
    public Monster getMonsterById(@RequestParam("id") int id) {
        return monsterService.getById(id);
    }

    /**
     * 返回所有的 monster 记录
     */
    @GetMapping("/list")
    @ResponseBody
    public List<Monster> getAllMonster() {
        return monsterService.list();
    }
}
```



## 6.3 @MapperScan

我们前面如果想要让一个 mapper 注入到容器中需要使用 `@Mapper` 注解或者在 application.yml 中配置 mybatis-config.xml，我们可以直接在启动类上添加 `@MapperScan` 这个注解即可

```java
@MapperScan(basePackages = {"com.xzh.mp.mapper"})
@SpringBootApplication
public class Main {
    public static void main(String[] args) {
        SpringApplication.run(Main.class, args);
    }
}
```



## 6.4 @TableName

`@TableName` 主要是用来解决数据库表名和 entity 类名映射的，一般情况下，库表名为类名的首字母小写，如果对不上则需要使用 `@TableName` 进行映射



## 6.5 MyBatisX 插件

具体详情可以参考 mybatis-plus 官网，这个插件可以快速完成 mapper 接口到 mapper 配置文件之间的映射，可以极大提高开发效率



