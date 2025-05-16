[toc]

# 1 基础入门

## 1.1 基本介绍

Spring Boot 可以轻松创建独立的、生产级的基于 Spring 的应用程序；它直接嵌入 Tomcat、Jetty 或 Undertow，可以直接运行 Spring Boot 应用程序



## 1.2 快速入门

01 搭建一个 maven 项目

02 引入相关依赖

```xml
<!--导入 springboot 父工程项目-->
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.5.3</version>
</parent>

<!--导入 web 场景启动器: 会自动导入和 web 开发相关的所有依赖-->
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```



03 创建 MainApp 类

```java
/**
 * @SpringBootApplication 表示这是一个 springboot 项目
 */
@SpringBootApplication
public class MainApp {
    public static void main(String[] args) {
        // 启动 springboot 应用程序
        SpringApplication.run(MainApp.class, args);
    }
}
```



04 创建 controller 类

```java
@Controller
public class HelloController {
    @RequestMapping("/hello")
    @ResponseBody
    public String hello() {
        return "Hello";
    }
}
```



**小结**

1. SpringBoot 相较传统的 SSM 开发，简化整合步骤，提高了开发效率
2. 简化了  Maven 项目的 pom.xml 依赖

![image-20230810115805895](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230810115805895.png)

## 1.3 梳理 spring、spring mvc、spring boot 三者之间的关系

1. 首先他们的关系大概是：Spring Boot > Spring > Spring MVC
2. Spring MVC 只是 Spring 处理 web 层请求的一个模块 / 组件，Spring MVC 的基石是 Servlet
3. Spring 的核心是 IOC 和 AOP，IOC 提供了依赖注入的容器，AOP 解决了面向切面变成
4. Spring Boot 是为了简化开发而提出的封神框架（约定优于配置，简化了 Spring 项目的配置流程），Spring Boot 包含很多组件 / 框架，而 Spring 就是其最核心的内容之一，也包含 Sppring MVC
5. Spring 家族有众多衍生框架和组件，例如 Spring Boot、Spring Security、Spring JPA 等，他们的基础都是 Spring



## 1.4 约定优于配置

1. 约定优于配置（Convention over Configuration, COC），又称为按约定编程，是一种软件设计规范，本质上是对系统、类库或框架中的一些假定一个大众化合理的默认值
2. 简化来说就是，当配置和我们预期的配置相同时，我们不需要自己手动配置，除非自己想要 DIY 自定义时才需要进行单独配置



## 1.5 依赖管理和自动配置

### 1.5.1 依赖管理

什么是依赖管理？

1. spring-boot-starter-parent 的父项目 spring-boot-dependencies 中声明了开发中常用的依赖的版本号，并且会自动进行==版本仲裁==

   > 所谓的==版本仲裁==其实就是如果程序员没有指定某个依赖 jar 的版本，则以父项目指定的版本为准

2. 如何修改版本仲裁？

   - 方式一：直接在 spring-boot-dependencies 中，找到 properties，并修改对应的依赖的 version 属性
   - 方式二：在当前项目的 pom.xml 中指定想要修改的依赖，由于 maven 的就近原则，会将父项目中默认版本的依赖给覆盖



### 1.5.2 starter 场景启动器

开发中我们引入相关场景的 starter，这个场景中所有的相关依赖都引入进行，例如当我们引入 spring-boot-starter-web，那么该 starter 将导入与 web 开发相关的所有包

- 所有场景启动器最基本的依赖就是 spring-boot-starter，这个依赖也是 Spring Boot 自动配置的核心依赖

starter 分类

- 官方 starter
- 第三方 starter

> 官方的 starter 前缀为 `spring-boot-starter` 后面跟的尾缀代表应用场景；而第三方 starter 通常以 `thirdpartyproject-spring-boot-starter`，也就是说其后缀为 `spring-boot-starter`，前缀代表其应用场景



### 1.5.3 自动配置

Spring Boot 中存在==自动配置机制==，使得我们不需要像以前配置 Tomcat、Spring MVC、扫描宝、视图解析器等等一系列配置

我们可以通过如下程序查看注入的配置有哪些：

```java
ConfigurableApplicationContext ioc = SpringApplication.run(MainApp.class, args);
String[] beanDefinitionNames = ioc.getBeanDefinitionNames();
for (String beanDefinitionName : beanDefinitionNames) {
    System.out.println(beanDefinitionName);
}
```

扫描包的默认位置：**主程序所在的包及其子包下的所有文件都会被扫描**



### 1.5.4 修改默认配置

:question:如何修改默认扫描包结构

假如当前项目的目录结构如下：

```
src                                        
├─ main                                    
│  ├─ java                                 
│  │  └─ com                               
│  │     └─ xzh                            
│  │        └─ springboot                  
│  │           ├─ controller               
│  │           │  └─ HelloController.java  
│  │           └─ MainApp.java             
│  └─ resources                            
└─ test                                    
   └─ java                                 
```

现在我们在 com.xzh 包下创建 ScanTest 类，并要求扫描到该类，那么应该如何处理？

直接在 `@SpringBootApplication` 中指定 scanBasePackages 即可

```java
/**
 * scanBasePackages 的值可以为字符串，表示指定哪一个包，也可以为数组，表示指定那些包
 */
@SpringBootApplication(scanBasePackages = {"com.xzh"})
public class MainApp {
    public static void main(String[] args) {
        // 启动 springboot 应用程序
        ConfigurableApplicationContext ioc = SpringApplication.run(MainApp.class, args);
        String[] beanDefinitionNames = ioc.getBeanDefinitionNames();
        for (String beanDefinitionName : beanDefinitionNames) {
            System.out.println(beanDefinitionName);
        }
    }
}
```



SpringBoot 配置大全

```properties
＃SPRING CONFIG（ConfigFileApplicationListener）  
spring.config.name =＃配置文件名（默认 为  'application' ）  
spring.config.location =＃配置文件的位置  
 
＃ 多环境配置文件激活属性
spring.profiles.active=dev					＃加载application-dev.properties配置文件内容 
application-dev.properties：					＃开发环境
application-test.properties：					＃测试环境
application-prod.properties：					＃生产环境
 
 
＃activemq
spring.activemq.broker-url						＃指定ActiveMQ broker的URL，默认自动生成.
spring.activemq.in-memory						＃是否是内存模式，默认为true.
spring.activemq.password						＃指定broker的密码.
spring.activemq.pooled							＃是否创建PooledConnectionFactory，而非ConnectionFactory，默认false
spring.activemq.user							＃指定broker的用户.
 
 
＃aop
spring.aop.auto									＃是否支持@EnableAspectJAutoProxy，默认为: true
spring.aop.proxy-target-class					＃true为使用CGLIB代理，false为JDK代理，默认为false
 
 
＃application
spring.application.admin.enabled				＃是否启用admin特性，默认为: false
spring.application.admin.jmx-name				＃指定admin MBean的名称，默认为: org.springframework.boot:type=Admin,name=SpringApplication
 
 
＃artemis(HornetQ捐献给apache后的版本)
spring.artemis.embedded.cluster-password		＃指定集群的密码，默认是启动时随机生成.
spring.artemis.embedded.data-directory			＃指定Journal文件的目录.如果不开始持久化则不必要指定.
spring.artemis.embedded.enabled					＃是否开启内嵌模式，默认true
spring.artemis.embedded.persistent				＃是否开启persistent store，默认false.
spring.artemis.embedded.queues					＃指定启动时创建的队列，多个用逗号分隔，默认: []
spring.artemis.embedded.server-id				＃指定Server ID. 默认是一个自增的数字，从0开始.
spring.artemis.embedded.topics					＃指定启动时创建的topic，多个的话逗号分隔，默认: []
spring.artemis.host								＃指定Artemis broker 的host. 默认: localhost
spring.artemis.mode								＃指定Artemis 的部署模式, 默认为auto-detected(也可以为native or embedded).
spring.artemis.port								＃指定Artemis broker 的端口，默认为: 61616
 
 
＃autoconfig
spring.autoconfigure.exclude					＃配置要排除的Auto-configuration classes.
 
 
＃batch
spring.batch.initializer.enabled				＃是否在必要时创建batch表，默认为true
spring.batch.job.enabled						＃是否在启动时开启batch job，默认为true
spring.batch.job.names							＃指定启动时要执行的job的名称，逗号分隔，默认所有job都会被执行
spring.batch.schema								＃指定要初始化的sql语句路径，默认:classpath:org/springframework/batch/core/schema-@@platform@@.sql)
spring.batch.table-prefix						＃指定批量处理的表的前缀.
 
 
＃cookie、session配置
server.session.cookie.comment					＃指定session cookie的comment
server.session.cookie.domain					＃指定session cookie的domain
server.session.cookie.http-only					＃是否开启HttpOnly.
server.session.cookie.max-age					＃设定session cookie的最大age.
server.session.cookie.name						＃设定Session cookie 的名称.
server.session.cookie.path						＃设定session cookie的路径.
server.session.cookie.secure					＃设定session cookie的“Secure” flag.
server.session.persistent						＃重启时是否持久化session，默认false
server.session.timeout							＃session的超时时间
server.session.tracking-modes					＃设定Session的追踪模式(cookie, url, ssl).
 
 
＃datasource 
spring.dao.exceptiontranslation.enabled			＃是否开启PersistenceExceptionTranslationPostProcessor，默认为true
spring.datasource.abandon-when-percentage-full	＃设定超时被废弃的连接占到多少比例时要被关闭或上报
spring.datasource.allow-pool-suspension			＃使用Hikari pool时，是否允许连接池暂停，默认为: false
spring.datasource.alternate-username-allowed	＃是否允许替代的用户名.
spring.datasource.auto-commit					＃指定updates是否自动提交.
spring.datasource.catalog						＃指定默认的catalog.
spring.datasource.commit-on-return				＃设置当连接被归还时，是否要提交所有还未完成的事务
spring.datasource.connection-init-sql			＃指定连接被创建，再被添加到连接池之前执行的sql.
spring.datasource.connection-init-sqls			＃使用DBCP connection pool时，指定初始化时要执行的sql
spring.datasource.connection-properties.[key]	＃在使用DBCP connection pool时指定要配置的属性
spring.datasource.connection-test-query			＃指定校验连接合法性执行的sql语句
spring.datasource.connection-timeout			＃指定连接的超时时间，毫秒单位.
spring.datasource.continue-on-error				＃在初始化数据库时，遇到错误是否继续，默认false
spring.datasource.data							＃指定Data (DML)脚本
spring.datasource.data-source-class-name		＃指定数据源的全限定名.
spring.datasource.data-source-jndi				＃指定jndi的地址
spring.datasource.data-source-properties.[key]	＃使用Hikari connection pool时，指定要设置的属性
spring.datasource.db-properties					＃使用Tomcat connection pool，指定要设置的属性
spring.datasource.default-auto-commit			＃是否自动提交.
spring.datasource.default-catalog				＃指定连接默认的catalog.
spring.datasource.default-read-only				＃是否设置默认连接只读.
spring.datasource.default-transaction-isolation	＃指定连接的事务的默认隔离级别.
spring.datasource.driver-class-name				＃指定driver的类名，默认从jdbc url中自动探测.
spring.datasource.fair-queue					＃是否采用FIFO返回连接.
spring.datasource.health-check-properties.[key]	＃使用Hikari connection pool时，在心跳检查时传递的属性
spring.datasource.idle-timeout					＃指定连接多久没被使用时，被设置为空闲，默认为10ms
spring.datasource.ignore-exception-on-pre-load	＃当初始化连接池时，是否忽略异常.
spring.datasource.init-sql						＃当连接创建时，执行的sql
spring.datasource.initial-size					＃指定启动连接池时，初始建立的连接数量
spring.datasource.initialization-fail-fast		＃当创建连接池时，没法创建指定最小连接数量是否抛异常
spring.datasource.initialize					＃指定初始化数据源，是否用data.sql来初始化，默认: true
spring.datasource.isolate-internal-queries		＃指定内部查询是否要被隔离，默认为false
spring.datasource.jdbc-interceptors				＃使用Tomcat connection pool时，指定jdbc拦截器，分号分隔
spring.datasource.jdbc-url						＃指定JDBC URL.
spring.datasource.jmx-enabled					＃是否开启JMX，默认为: false
spring.datasource.jndi-name						＃指定jndi的名称.
spring.datasource.leak-detection-threshold		＃使用Hikari connection pool时，多少毫秒检测一次连接泄露.
spring.datasource.log-abandoned					＃使用DBCP connection pool，是否追踪废弃statement或连接，默认为: false
spring.datasource.log-validation-errors			＃当使用Tomcat connection pool是否打印校验错误.
spring.datasource.login-timeout					＃指定连接数据库的超时时间.
spring.datasource.max-active					＃指定连接池中最大的活跃连接数.
spring.datasource.max-age						＃指定连接池中连接的最大年龄
spring.datasource.max-idle						＃指定连接池最大的空闲连接数量.
spring.datasource.max-lifetime					＃指定连接池中连接的最大生存时间，毫秒单位.
spring.datasource.max-open-prepared-statements	＃指定最大的打开的prepared statements数量.
spring.datasource.max-wait						＃指定连接池等待连接返回的最大等待时间，毫秒单位.
spring.datasource.maximum-pool-size				＃指定连接池最大的连接数，包括使用中的和空闲的连接.
spring.datasource.min-evictable-idle-time-millis		＃指定一个空闲连接最少空闲多久后可被清除.
spring.datasource.min-idle						＃指定必须保持连接的最小值(For DBCP and Tomcat connection pools)
spring.datasource.minimum-idle					＃指定连接维护的最小空闲连接数，当使用HikariCP时指定.
spring.datasource.name							＃指定数据源名.
spring.datasource.num-tests-per-eviction-run	＃指定运行每个idle object evictor线程时的对象数量
spring.datasource.password						＃指定数据库密码.
spring.datasource.platform						＃指定schema要使用的Platform(schema-${platform}.sql)，默认为: all
spring.datasource.pool-name						＃指定连接池名字.
spring.datasource.pool-prepared-statements		＃指定是否池化statements.
spring.datasource.propagate-interrupt-state		＃在等待连接时，如果线程被中断，是否传播中断状态.
spring.datasource.read-only						＃当使用Hikari connection pool时，是否标记数据源只读
spring.datasource.register-mbeans				＃指定Hikari connection pool是否注册JMX MBeans.
spring.datasource.remove-abandoned				＃指定当连接超过废弃超时时间时，是否立刻删除该连接.
spring.datasource.remove-abandoned-timeout		＃指定连接应该被废弃的时间.
spring.datasource.rollback-on-return			＃在归还连接时，是否回滚等待中的事务.
spring.datasource.schema						＃指定Schema (DDL)脚本.
spring.datasource.separator						＃指定初始化脚本的语句分隔符，默认: ;
spring.datasource.sql-script-encoding			＃指定SQL scripts编码.
spring.datasource.suspect-timeout				＃指定打印废弃连接前的超时时间.
spring.datasource.test-on-borrow				＃当从连接池借用连接时，是否测试该连接.
spring.datasource.test-on-connect				＃创建时，是否测试连接
spring.datasource.test-on-return				＃在连接归还到连接池时是否测试该连接.
spring.datasource.test-while-idle				＃当连接空闲时，是否执行连接测试.
spring.datasource.time-between-eviction-runs-millis    ＃指定空闲连接检查、废弃连接清理、空闲连接池大小调整之间的操作时间间隔
spring.datasource.transaction-isolation			＃指定事务隔离级别，使用Hikari connection pool时指定
spring.datasource.url							＃指定JDBC URL.
spring.datasource.use-disposable-connection-facade		＃是否对连接进行包装，防止连接关闭之后被使用.
spring.datasource.use-equals					＃比较方法名时是否使用String.equals()替换==.
spring.datasource.use-lock						＃是否对连接操作加锁
spring.datasource.username						＃指定数据库名.
spring.datasource.validation-interval			＃指定多少ms执行一次连接校验.
spring.datasource.validation-query				＃指定获取连接时连接校验的sql查询语句.
spring.datasource.validation-query-timeout		＃指定连接校验查询的超时时间.
spring.datasource.validation-timeout			＃设定连接校验的超时时间，当使用Hikari connection pool时指定
spring.datasource.validator-class-name			＃用来测试查询的validator全限定名.
spring.datasource.xa.data-source-class-name		＃指定数据源的全限定名.
spring.datasource.xa.properties					＃指定传递给XA data source的属性
＃data  springdata
spring.data.elasticsearch.cluster-name			＃指定es集群名称，默认: elasticsearch
spring.data.elasticsearch.cluster-nodes			＃指定es的集群，逗号分隔，不指定的话，则启动client node.
spring.data.elasticsearch.properties			＃指定要配置的es属性.
spring.data.elasticsearch.repositories.enabled	＃是否开启es存储，默认为: true
spring.data.jpa.repositories.enabled			＃是否开启JPA支持，默认为: true
spring.data.mongodb.authentication-database		＃指定鉴权的数据库名
spring.data.mongodb.database					＃指定mongodb数据库名
spring.data.mongodb.field-naming-strategy		＃指定要使用的FieldNamingStrategy.
spring.data.mongodb.grid-fs-database			＃指定GridFS database的名称.
spring.data.mongodb.host						＃指定Mongo server host.
spring.data.mongodb.password					＃指定Mongo server的密码.
spring.data.mongodb.port						＃指定Mongo server port.
spring.data.mongodb.repositories.enabled		＃是否开启mongodb存储，默认为true
spring.data.mongodb.uri							＃指定Mongo database URI.默认:mongodb://localhost/test
spring.data.mongodb.username					＃指定登陆mongodb的用户名.
spring.data.rest.base-path						＃指定暴露资源的基准路径.
spring.data.rest.default-page-size				＃指定每页的大小，默认为: 20
spring.data.rest.limit-param-name				＃指定limit的参数名，默认为: size
spring.data.rest.max-page-size					＃指定最大的页数，默认为1000
spring.data.rest.page-param-name				＃指定分页的参数名，默认为: page
spring.data.rest.return-body-on-create			＃当创建完实体之后，是否返回body，默认为false
spring.data.rest.return-body-on-update			＃在更新完实体后，是否返回body，默认为false
spring.data.rest.sort-param-name				＃指定排序使用的key，默认为: sort
spring.data.solr.host							＃指定Solr host，如果有指定了zk的host的话，则忽略。默认为: http://127.0.0.1:8983/solr
spring.data.solr.repositories.enabled			＃是否开启Solr repositories，默认为: true
spring.data.solr.zk-host						＃指定zk的地址，格式为HOST:PORT.
＃----------------------------------------  
＃DEVTOOLS属性  
＃----------------------------------------  
  
＃DEVTOOLS（DevToolsProperties）  
spring.devtools.livereload.enabled = true		＃启用livereload.com兼容的server。  
spring.devtools.livereload.port = 35729			＃服务器端口。  
spring.devtools.restart.additional-exclude =		＃应该从触发完全重新启动时排除的其他模式。  
spring.devtools.restart.additional-paths =		＃额外的路径来观察变化。  
spring.devtools.restart.enabled = true			＃启用自动重启。  
spring.devtools.restart.exclude = META-INF /行家/ **，META-INF /资源/ **，资源/ **，静态/ **，公共/ **，模板/ **，** / * Test.class，** / * Tests.class，git.properties＃应该排除触发完全重启的模式。  
spring.devtools.restart.poll-interval = 1000	＃polling 路径更改之间等待的时间（以毫秒为单位）。  
spring.devtools.restart.quiet-period = 400		＃触发重新启动之前，没有任何类路径变化所需的静默时间（以毫秒为单位）。  
spring.devtools.restart.trigger-file =			＃特定文件的名称，在更改时会触发重新启动检查。如果未指定任何类路径文件更改将触发重新启动。  
  
  
＃DEVTOOLS   REMOTE DEVTOOLS（RemoteDevToolsProperties）  
spring.devtools.remote.context-path =  			＃用于处理远程连接的上下文路径。  
spring.devtools.remote.debug.enabled = true		＃启用远程调试支持。  
spring.devtools.remote.debug.local-port = 8000	＃本地远程调试服务器端口。  
spring.devtools.remote.proxy.host =				＃用于连接远程应用程序的代理主机。  
spring.devtools.remote.proxy.port =				＃用于连接远程应用程序的代理端口。  
spring.devtools.remote.restart.enabled = true	＃启用远程重启。  
spring.devtools.remote.secret =					＃建立连接所需的共享密钥（需要启用远程支持）。  
spring.devtools.remote.secret-header-name = X-AUTH-TOKEN  		＃用于传输共享密钥的HTTP头。  
 
 
＃----------------------------------------  
＃执行器属性  
＃----------------------------------------  
  
＃ENDPOINTS（AbstractEndpoint子类）  
endpoints.enabled = true						＃启用端点。  
endpoints.sensitive =							＃默认的端点敏感设置。  
endpoints.actuator.enabled = true				＃启用端点。  
endpoints.actuator.path =						＃端点URL路径。  
endpoints.actuator.sensitive = false			＃在端点上启用安全性。  
endpoints.autoconfig.enabled =					＃启用端点。  
endpoints.autoconfig.id =						＃端点标识符。  
endpoints.autoconfig.sensitive =				＃标记端点是否暴露敏感信息。  
endpoints.beans.enabled =						＃启用端点。  
endpoints.beans.id =							＃端点标识符。  
endpoints.beans.sensitive =						＃标记端点是否暴露敏感信息。  
endpoints.configprops.enabled =					＃启用端点。  
endpoints.configprops.id =						＃端点标识符。  
endpoints.configprops.keys-to-sanitize			＃应该清理的密钥。键可以是属性以或正则表达式结束的简单字符串。  
endpoints.configprops.sensitive =				＃标记端点是否公开敏感信息。  
endpoints.docs.curies.enabled = false			＃启用居里代。  
endpoints.docs.enabled = true					＃启用执行器文档终结点。  
endpoints.docs.path = / docs					＃  
endpoints.docs.sensitive = false				＃  
 
  
＃终端CORS配置（EndpointCorsProperties）  
endpoints.cors.allow-credentials =				＃设置是否支持凭据。未设置时，不支持凭证。  
endpoints.cors.allowed-headers =				＃在请求中允许使用逗号分隔的标题列表。'*'允许所有标题。  
endpoints.cors.allowed-methods = GET			＃逗号分隔的允许的方法列表。'*'允许所有的方法。  
endpoints.cors.allowed-origins =				＃逗号分隔的起源列表允许。'*'允许所有的来源。未设置时，CORS支持被禁用。  
endpoints.cors.exposed-headers =				＃包含在响应中的逗号分隔的标题列表。  
endpoints.cors.max-age = 1800					＃以秒为单位，客户端可以缓存飞行前请求的响应。  
  
＃JMX ENDPOINT（EndpointMBeanExportProperties）  
endpoints.jmx.domain =							＃JMX域名。如果设置，则用“spring.jmx.default-domain”的值初始化。  
endpoints.jmx.enabled = true					＃启用所有端点的JMX导出。  
endpoints.jmx.static-names =					＃附加到所有表示端点的MBean的ObjectName的静态属性。  
endpoints.jmx.unique-names = false				＃确保ObjectNames在发生冲突时被修改。  
 
 
＃flyway
flyway.baseline-description 					＃对执行迁移时基准版本的描述.
flyway.baseline-on-migrate						＃当迁移时发现目标schema非空，而且带有没有元数据的表时，是否自动执行基准迁移，默认false.
flyway.baseline-version							＃开始执行基准迁移时对现有的schema的版本打标签，默认值为1.
flyway.check-location							＃检查迁移脚本的位置是否存在，默认false.
flyway.clean-on-validation-error				＃当发现校验错误时是否自动调用clean，默认false.
flyway.enabled									＃是否开启flywary，默认true.
flyway.encoding									＃设置迁移时的编码，默认UTF-8.
flyway.ignore-failed-future-migration			＃当读取元数据表时是否忽略错误的迁移，默认false.
flyway.init-sqls								＃当初始化好连接时要执行的SQL.
flyway.locations								＃迁移脚本的位置，默认db/migration.
flyway.out-of-order								＃是否允许无序的迁移，默认false.
flyway.password									＃目标数据库的密码.
flyway.placeholder-prefix						＃设置每个placeholder的前缀，默认${.
flyway.placeholder-replacement					＃placeholders是否要被替换，默认true.
flyway.placeholder-suffix						＃设置每个placeholder的后缀，默认}.
flyway.placeholders.[placeholder name]			＃设置placeholder的value
flyway.schemas									＃设定需要flywary迁移的schema，大小写敏感，默认为连接默认的schema.
flyway.sql-migration-prefix						＃迁移文件的前缀，默认为V.
flyway.sql-migration-separator					＃迁移脚本的文件名分隔符，默认__
flyway.sql-migration-suffix						＃迁移脚本的后缀，默认为.sql
flyway.table									＃flyway使用的元数据表名，默认为schema_version
flyway.target									＃迁移时使用的目标版本，默认为latest version
flyway.url										＃迁移时使用的JDBC URL，如果没有指定的话，将使用配置的主数据源
flyway.user										＃迁移数据库的用户名
flyway.validate-on-migrate						＃迁移时是否校验，默认为true.
 
＃FREEMARKER（FreeMarkerAutoConfiguration）  
spring.freemarker.allowRequestOverride = false   			＃设置HttpServletRequest属性是否允许覆盖(隐藏)控制器生成的相同名称的模型属性。
spring.freemarker.allowSessionOverride = false   			＃设置HttpSession属性是否允许覆盖(隐藏)控制器生成的相同名称的模型属性。	
spring.freemarker.cache = true  				 			＃使用模板缓存。
spring.freemarker.charset=UTF-8 				 			＃ Template encoding.
spring.freemarker.checkTemplateLocation = true   			＃检查模板位置是否存在spring.freemarker.check-template-location=true 
spring.freemarker.contentType = text / html  	 			＃Content-Type  
spring.freemarker.exposeRequestAttributes = false  			＃在与模板合并之前，设置是否应该将所有请求属性添加到模型中。spring.freemarker.expose-request-attributes=false
spring.freemarker.exposeSessionAttributes = false  			＃在与模板合并之前，设置是否应该将所有HttpSession属性添加到模型中。spring.freemarker.expose-session-attributes=false
spring.freemarker.exposeSpringMacroHelpers = false  		＃设定是否以springMacroRequestContext的形式暴露RequestContext给Spring’s macro library使用      spring.freemarker.expose-spring-macro-helpers=true
spring.freemarker.prefer-file-system-access=true 			＃是否优先从文件系统加载template，以支持热加载，默认为true
spring.freemarker.prefix =  								＃在构建URL时，前缀被预先指定以查看名称。
spring.freemarker.requestContextAttribute =  				＃所有视图的RequestContext属性的名称.   freemarker.request-context-attribute= 
spring.freemarker.settings.* =  							＃Well-known FreeMarker keys which will be passed to FreeMarker's Configuration
spring.freemarker.suffix = .ftl  							＃在构建URL时附加到视图名称后面的后缀		
spring.freemarker.templateEncoding = UTF- 8  
spring.freemarker.templateLoaderPath =classpath:/templates  ＃设定ftl文件路径 类路径：/模板/  
spring.freemarker.viewNames =								＃可以解析的视图名称的白名单  
spring.mvc.static-path-pattern=/static/**  					＃设定静态文件路径，js,css等
＃GIT信息  
spring.git.properties =							＃生成的git信息属性文件的资源引用。  
＃GROOVY模板（GroovyTemplateAutoConfiguration）  
spring.groovy.template.allow-request-override 				＃指定HttpServletRequest的属性是否可以覆盖controller的model的同名项
spring.groovy.template.allow-session-override 				＃指定HttpSession的属性是否可以覆盖controller的model的同名项
spring.groovy.template.cache  								＃是否开启模板缓存
.spring.groovy.template.charset 							＃指定Template编码
.spring.groovy.template.check-template-location 			＃是否检查模板的路径是否存在.
spring.groovy.template.configuration.auto-escape 			＃是否在渲染模板时自动排查model的变量，默认为: false
spring.groovy.template.configuration.auto-indent 			＃是否在渲染模板时自动缩进，默认为false
spring.groovy.template.configuration.auto-indent-string 	＃如果自动缩进启用的话，是使用SPACES还是TAB，默认为: SPACES
spring.groovy.template.configuration.auto-new-line 			＃渲染模板时是否要输出换行，默认为false
spring.groovy.template.configuration.base-template-class 	＃指定template base class.
spring.groovy.template.configuration.cache-templates 		＃是否要缓存模板，默认为true
spring.groovy.template.configuration.declaration-encoding 	＃在写入declaration header时使用的编码
spring.groovy.template.configuration.expand-empty-elements 	＃是使用<br/>这种形式，还是<br></br>这种展开模式，默认为: false)
spring.groovy.template.configuration.locale 				＃指定template locale.
spring.groovy.template.configuration.new-line-string 		＃当启用自动换行时，换行的输出，默认为系统的line.separator属性的值
spring.groovy.template.configuration.resource-loader-path 	＃指定groovy的模板路径，默认为classpath:/templates/
spring.groovy.template.configuration.use-double-quotes 		＃指定属性要使用双引号还是单引号，默认为false
spring.groovy.template.content-type 						＃指定Content-Type.
spring.groovy.template.enabled 								＃是否开启groovy模板的支持.
spring.groovy.template.expose-request-attributes 			＃设定所有request的属性在merge到模板的时候，是否要都添加到model中.
spring.groovy.template.expose-session-attributes 			＃设定所有request的属性在merge到模板的时候，是否要都添加到model中.
spring.groovy.template.expose-spring-macro-helpers 			＃设定是否以springMacroRequestContext的形式暴露RequestContext给Spring’s macro library使用
spring.groovy.template.prefix 								＃指定模板的前缀.
spring.groovy.template.request-context-attribute 			＃指定RequestContext属性的名.
spring.groovy.template.resource-loader-path				 	＃指定模板的路径，默认为: classpath:/templates/
spring.groovy.template.suffix								＃指定模板的后缀
spring.groovy.template.view-names 							＃指定要使用模板的视图名称.
＃h2
spring.h2.console.enabled						＃是否开启控制台，默认为false
spring.h2.console.path							＃指定控制台路径，默认为: /h2-console
＃hornetq	(HornetQProperties）  
spring.hornetq.embedded.cluster-password		＃指定集群的密码，默认启动时随机生成.
spring.hornetq.embedded.data-directory			＃指定Journal file 的目录. 如果不开启持久化则不必指定.
spring.hornetq.embedded.enabled					＃是否开启内嵌模式，默认:true
spring.hornetq.embedded.persistent				＃是否开启persistent store，默认: false
spring.hornetq.embedded.queues					＃指定启动是创建的queue，多个以逗号分隔，默认: []
spring.hornetq.embedded.server-id				＃指定Server ID. 默认使用自增数字，从0开始.
spring.hornetq.embedded.topics					＃指定启动时创建的topic，多个以逗号分隔，默认: []
spring.hornetq.host								＃指定HornetQ broker 的host，默认: localhost
spring.hornetq.mode								＃指定HornetQ 的部署模式，默认是auto-detected，也可以指定native 或者 embedded.
spring.hornetq.port								＃指定HornetQ broker 端口，默认: 5445
＃http
spring.hateoas.apply-to-primary-object-mapper   ＃设定是否对object mapper也支持HATEOAS，默认为: true
spring.http.converters.preferred-json-mapper   	＃是否优先使用JSON mapper来转换.
spring.http.encoding.charset 					＃指定http请求和相应的Charset，默认: UTF-8
spring.http.encoding.enabled					＃是否开启http的编码支持，默认为true
spring.http.encoding.force						＃是否强制对http请求和响应进行编码，默认为true
＃jersey
spring.jersey.filter.order 						＃指定Jersey filter的order，默认为: 0
spring.jersey.init								＃指定传递给Jersey的初始化参数.
spring.jersey.type								＃指定Jersey的集成类型，可以是servlet或者filter.
＃jms
spring.jms.jndi-name							＃指定Connection factory JNDI 名称.
spring.jms.listener.acknowledge-mode			＃指定ack模式，默认自动ack.
spring.jms.listener.auto-startup				＃是否启动时自动启动jms，默认为: true
spring.jms.listener.concurrency					＃指定最小的并发消费者数量.
spring.jms.listener.max-concurrency				＃指定最大的并发消费者数量.
spring.jms.pub-sub-domain						＃是否使用默认的destination type来支持 publish/subscribe，默认: false
jmx
spring.jmx.default-domain						＃指定JMX domain name.
spring.jmx.enabled								＃是否暴露jmx，默认为true
spring.jmx.server								＃指定MBeanServer bean name. 默认为: mbeanServer)
＃jooq
spring.jooq.sql-dialect							＃指定JOOQ使用的SQLDialect，比如POSTGRES.
＃Messages
spring.messages.basename		 							＃指定message的basename，多个以逗号分隔，如果不加包名的话，默认从classpath路径开始，默认: messages
spring.messages.cache-seconds	 							＃设定加载的资源文件缓存失效时间，-1的话为永不过期，默认为-1
spring.messages.encoding    			 					＃设定Message bundles的编码，默认: UTF-8
＃JPA
spring.jpa.database								＃指定目标数据库.
spring.jpa.database-platform					＃指定目标数据库的类型.
spring.jpa.generate-ddl							＃是否在启动时初始化schema，默认为false
spring.jpa.hibernate.ddl-auto					＃指定DDL mode (none, validate, update, create, create-drop). 当使用内嵌数据库时，默认是create-drop，否则为none.
spring.jpa.hibernate.naming-strategy			＃指定命名策略.
spring.jpa.open-in-view							＃是否注册OpenEntityManagerInViewInterceptor，绑定JPA EntityManager到请求线程中，默认为: true
spring.jpa.properties							＃添加额外的属性到JPA provider.
spring.jpa.show-sql								＃是否开启sql的log，默认为: false
＃json
spring.jackson.date-format						＃指定日期格式，比如yyyy-MM-dd HH:mm:ss，或者具体的格式化类的全限定名
spring.jackson.deserialization					＃是否开启Jackson的反序列化
spring.jackson.generator						＃是否开启json的generators.
spring.jackson.joda-date-time-format			＃指定Joda date/time的格式，比如yyyy-MM-dd HH:mm:ss). 如果没有配置的话，dateformat会作为backup
spring.jackson.locale							＃指定json使用的Locale.
spring.jackson.mapper							＃是否开启Jackson通用的特性.
spring.jackson.parser							＃是否开启jackson的parser特性.
spring.jackson.property-naming-strategy			＃指定PropertyNamingStrategy (CAMEL_CASE_TO_LOWER_CASE_WITH_UNDERSCORES)或者指定PropertyNamingStrategy子类的全限定类名.
spring.jackson.serialization					＃是否开启jackson的序列化.
spring.jackson.serialization-inclusion			＃指定序列化时属性的inclusion方式，具体查看JsonInclude.Include枚举.
spring.jackson.time-zone						＃指定日期格式化时区，比如America/Los_Angeles或者GMT+10.
＃JTA
spring.jta.allow-multiple-lrc					＃是否允许 multiple LRC，默认为: false
spring.jta.asynchronous2-pc						＃指定两阶段提交是否可以异步，默认为: false
spring.jta.background-recovery-interval			＃指定多少分钟跑一次recovery process，默认为: 1
spring.jta.background-recovery-interval-seconds	＃指定多久跑一次recovery process，默认: 60
spring.jta.current-node-only-recovery			＃是否过滤掉其他非本JVM的recovery，默认为: true
spring.jta.debug-zero-resource-transaction		＃是否追踪没有使用指定资源的事务，默认为: false
spring.jta.default-transaction-timeout			＃设定默认的事务超时时间，默认为60
spring.jta.disable-jmx							＃是否禁用jmx，默认为false
spring.jta.enabled								＃是否开启JTA support，默认为: true
spring.jta.exception-analyzer					＃设置指定的异常分析类
spring.jta.filter-log-status					＃使用Bitronix Transaction Manager时，是否写mandatory logs，开启的话，可以节省磁盘空间，但是调试会复杂写，默认为false
spring.jta.force-batching-enabled				＃使用Bitronix Transaction Manager时，是否批量写磁盘，默认为true.
spring.jta.forced-write-enabled					＃使用Bitronix Transaction Manager时，是否强制写日志到磁盘，默认为true
spring.jta.graceful-shutdown-interval			＃当使用Bitronix Transaction Manager，指定shutdown时等待事务结束的时间，超过则中断，默认为60
spring.jta.jndi-transaction-synchronization-registry-name		＃当使用Bitronix Transaction Manager时，在JNDI下得事务同步registry，默认为: java:comp/TransactionSynchronizationRegistry
spring.jta.jndi-user-transaction-name			＃指定在JNDI使用Bitronix Transaction Manager的名称，默认:java:comp/UserTransaction
spring.jta.journal								＃当使用Bitronix Transaction Manager，指定The journal是否disk还是null还是一个类的全限定名，默认disk
spring.jta.log-dir								＃Transaction logs directory.
spring.jta.log-part1-filename					＃指定The journal fragment文件1的名字，默认: btm1.tlog
spring.jta.log-part2-filename					＃指定The journal fragment文件2的名字，默认: btm2.tlog
spring.jta.max-log-size-in-mb					＃指定journal fragments大小的最大值. 默认: 2M
spring.jta.resource-configuration-filename		＃指定Bitronix Transaction Manager配置文件名.
spring.jta.server-id							＃指定Bitronix Transaction Manager实例的id.
spring.jta.skip-corrupted-logs					＃是否忽略corrupted log files文件，默认为false.
spring.jta.transaction-manager-id				＃指定Transaction manager的唯一标识.
spring.jta.warn-about-zero-resource-transaction	＃当使用Bitronix Transaction Manager时，是否对没有使用指定资源的事务进行警告，默认为: true
＃mail
spring.mail.default-encoding					＃指定默认MimeMessage的编码，默认为: UTF-8
spring.mail.host								＃指定SMTP server host.
spring.mail.jndi-name							＃指定mail的jndi名称
spring.mail.password							＃指定SMTP server登陆密码.
spring.mail.port								＃指定SMTP server port.
spring.mail.properties							＃指定JavaMail session属性.
spring.mail.protocol							＃指定SMTP server使用的协议，默认为: smtp
spring.mail.test-connection						＃指定是否在启动时测试邮件服务器连接，默认为false
spring.mail.username							＃指定SMTP server的用户名.
＃mobile
spring.mobile.devicedelegatingviewresolver.enable-fallback	＃是否支持fallback的解决方案，默认false
spring.mobile.devicedelegatingviewresolver.enabled			＃是否开始device view resolver，默认为: false
spring.mobile.devicedelegatingviewresolver.mobile-prefix	＃设定mobile端视图的前缀，默认为:mobile/
spring.mobile.devicedelegatingviewresolver.mobile-suffix	＃设定mobile视图的后缀
spring.mobile.devicedelegatingviewresolver.normal-prefix	＃设定普通设备的视图前缀
spring.mobile.devicedelegatingviewresolver.normal-suffix	＃设定普通设备视图的后缀
spring.mobile.devicedelegatingviewresolver.tablet-prefix	＃设定平板设备视图前缀，默认:tablet/
spring.mobile.devicedelegatingviewresolver.tablet-suffix	＃设定平板设备视图后缀.
spring.mobile.sitepreference.enabled						＃是否启用SitePreferenceHandler，默认为: true
＃MONGODB（Mongo性能）  
spring.data.mongodb.host =						＃分贝主机  
spring.data.mongodb.port = 27017  				＃连接端口（默认为  27107 ）  
spring.data.mongodb.uri = 						＃连接URL  
spring.mongodb.embedded.features				＃指定要开启的特性，逗号分隔.
spring.mongodb.embedded.version					＃指定要使用的版本，默认: 2.6.10
＃MANAGEMENT HTTP SERVER（ManagementServerProperties）  
management.add-application-context-header = true＃在每个响应中添加“X-Application-Context”HTTP标头。  
management.address =							＃管理端点应该绑定的网络地址。  
management.context-path =						＃管理端点上下文路径。例如`/执行器`  
management.port =								＃管理端点HTTP端口。默认使用与应用程序相同的端口。  
management.security.enabled = true				＃启用
management.security.role = ADMIN				＃访问管理端点所需的角色。  
management.security.sessions 					＃会话创建策略使用（always, never, if_required, stateless）。  
  
＃HEALTH INDICATORS健康指标（以前的健康状况*）  
management.health.db.enabled = true				＃启用数据库运行状况检查。  
management.health.defaults.enabled = true		＃启用默认健康指标。  
management.health.diskspace.enabled = true		＃启用磁盘空间运行状况检查。  
management.health.diskspace.path =				＃用于计算可用磁盘空间的路径。  
management.health.diskspace.threshold = 0		＃应该可用的最小磁盘空间（以字节为单位）。  
management.health.elasticsearch.enabled = true	＃启用elasticsearch运行状况检查。  
management.health.elasticsearch.indices =		＃逗号分隔的索引名称。  
management.health.elasticsearch.response-timeout = 100		＃等待群集响应的时间（以毫秒为单位）。  
management.health.jms.enabled = true			＃启用JMS运行状况检查。  
management.health.mail.enabled = true			＃启用邮件运行状况检查。  
management.health.mongo.enabled = true			＃启用MongoDB运行状况检查。  
management.health.rabbit.enabled = true			＃启用RabbitMQ健康检查。  
management.health.redis.enabled = true			＃启用Redis运行状况检查。  
management.health.solr.enabled = true			＃启用Solr运行状况检查。  
management.health.status.order 					＃以逗号分隔的健康状态列表。  
management.trace.include 						＃要包含在跟踪中的项目。  
＃METRICS EXPORT（MetricExportProperties）  
spring.metrics.export.aggregate.key-pattern =	＃告诉聚合器如何处理源存储库中的密钥的模式。  
spring.metrics.export.aggregate.prefix =		＃全局存储库的前缀（如果处于活动状态）。  
spring.metrics.export.delay-millis = 5000		＃输出滴答之间的延迟（以毫秒为单位）。度量标准按照计划导出到外部源。  
spring.metrics.export.enabled = true			＃启用metric 标准导出的标志（假设MetricWriter可用）。  
spring.metrics.export.excludes =				＃要排除的metric 标准名称的模式列表。包括后应用。  
spring.metrics.export.includes =				＃要包含的metric 标准名称的模式列表。  
spring.metrics.export.redis.key 				＃Redis存储库导出密钥（如果有效）。  
spring.metrics.export.redis.prefix				＃如果处于活动状态，redis存储库的前缀。  
spring.metrics.export.send-latest 				＃根据不导出不变的metric 值的标志关闭所有可用的优化。  
spring.metrics.export.statsd.host =				＃接收导出metric 的statsd server的主机。  
spring.metrics.export.statsd.port = 8125		＃接收导出metric的statsd server的端口。  
spring.metrics.export.statsd.prefix =			＃统计导出metric的前缀。  
spring.metrics.export.triggers。* =				＃每个MetricWriter bean名称的特定触发器属性。  
＃multipart
multipart.enabled								＃是否开启文件上传支持，默认为true
multipart.file-size-threshold					＃设定文件写入磁盘的阈值，单位为MB或KB，默认为0
multipart.location								＃指定文件上传路径.
multipart.max-file-size							＃指定文件大小最大值，默认1MB
multipart.max-request-size						＃指定每次请求的最大值，默认为10MB
＃mustcache
spring.mustache.cache							＃是否Enable template caching.
spring.mustache.charset							＃指定Template的编码.
spring.mustache.check-template-location			＃是否检查默认的路径是否存在.
spring.mustache.content-type					＃指定Content-Type.
spring.mustache.enabled							＃是否开启mustcache的模板支持.
spring.mustache.prefix							＃指定模板的前缀，默认: classpath:/templates/
spring.mustache.suffix							＃指定模板的后缀，默认: .html
spring.mustache.view-names						＃指定要使用模板的视图名.
＃MVC（SPRING MVC相关的一些配置）  
http.mappers.json-pretty-print = false  					＃打印JSON  
http.mappers.json-sort-keys = false  						＃排序键  
spring.mvc.locale =											＃设置固定语言环境，例如en_UK  
spring.mvc.date-format =									＃设置固定的日期格式，例如dd / MM / yyyy  
spring.mvc.async.request-timeout	 						＃设定async请求的超时时间，以毫秒为单位，如果没有设置的话，以具体实现的超时时间为准，比如tomcat的servlet3的话是10秒.
spring.mvc.favicon.enabled 									＃是否支持favicon.ico，默认为: true
spring.mvc.ignore-default-model-on-redirect  				＃在重定向时是否忽略默认model的内容，默认为true
spring.mvc.locale＃指定使用的Locale.
spring.mvc.message-codes-resolver-format 					＃指定message codes的格式化策略(PREFIX_ERROR_CODE,POSTFIX_ERROR_CODE).
spring.view.prefix =   										＃MVC视图前缀  
spring.view.suffix =   										＃...和后缀  	
spring.resources.cache-period =             				＃发送到浏览器的标题缓存超时  
spring.resources.add-mappings = true         				＃如果 应该添加默认映射   
＃liquibase
liquibase.change-log							＃Change log 配置文件的路径，默认值为classpath:/db/changelog/db.changelog-master.yaml
liquibase.check-change-log-location				＃是否坚持change log的位置是否存在，默认为true.
liquibase.contexts								＃逗号分隔的运行时context列表.
liquibase.default-schema						＃默认的schema.
liquibase.drop-first							＃是否首先drop schema，默认为false
liquibase.enabled								＃是否开启liquibase，默认为true.
liquibase.password								＃目标数据库密码
liquibase.url									＃要迁移的JDBC URL，如果没有指定的话，将使用配置的主数据源.
liquibase.user									＃目标数据用户名
＃logging日志
logging.path =  							＃文件路径
logging.file = myapp.log   					＃文件名称
logging.config =							＃如果你即想完全掌控日志配置，但又不想用logback.xml作为Logback配置的名字，可以通过logging.config属性指定自定义的名字
logging.level.root=INFO  					＃日志级别  从控制台打印出来的日志级别只有ERROR, WARN 还有INFO，如果你想要打印debug级别的日志，可以配置debug=true
logging.level.org.springframework.web=DEBUG
logging.level.org.hibernate=ERROR									
＃rabbitmq
spring.rabbitmq.addresses						＃指定client连接到的server的地址，多个以逗号分隔.
spring.rabbitmq.dynamic							＃是否创建AmqpAdmin bean. 默认为: true)
spring.rabbitmq.host							＃指定RabbitMQ host.默认为: localhost)
spring.rabbitmq.listener.acknowledge-mode		＃指定Acknowledge的模式.
spring.rabbitmq.listener.auto-startup			＃是否在启动时就启动mq，默认: true)
spring.rabbitmq.listener.concurrency			＃指定最小的消费者数量.
spring.rabbitmq.listener.max-concurrency		＃指定最大的消费者数量.
spring.rabbitmq.listener.prefetch				＃指定一个请求能处理多少个消息，如果有事务的话，必须大于等于transaction数量.
spring.rabbitmq.listener.transaction-size		＃指定一个事务处理的消息数量，最好是小于等于prefetch的数量.
spring.rabbitmq.password						＃指定broker的密码.
spring.rabbitmq.port							＃指定RabbitMQ 的端口，默认: 5672)
spring.rabbitmq.requested-heartbeat				＃指定心跳超时，0为不指定.
spring.rabbitmq.ssl.enabled						＃是否开始SSL，默认: false)
spring.rabbitmq.ssl.key-store					＃指定持有SSL certificate的key store的路径
spring.rabbitmq.ssl.key-store-password			＃指定访问key store的密码.
spring.rabbitmq.ssl.trust-store					＃指定持有SSL certificates的Trust store.
spring.rabbitmq.ssl.trust-store-password		＃指定访问trust store的密码.
spring.rabbitmq.username						＃指定登陆broker的用户名.
spring.rabbitmq.virtual-host					＃指定连接到broker的Virtual host.
＃redis
spring.redis.database							＃指定连接工厂使用的Database index，默认为: 0
spring.redis.host								＃指定Redis server host，默认为: localhost
spring.redis.password							＃指定Redis server的密码
spring.redis.pool.max-active					＃指定连接池最大的活跃连接数，-1表示无限，默认为8
spring.redis.pool.max-idle						＃指定连接池最大的空闲连接数，-1表示无限，默认为8
spring.redis.pool.max-wait						＃指定当连接池耗尽时，新获取连接需要等待的最大时间，以毫秒单位，-1表示无限等待
spring.redis.pool.min-idle						＃指定连接池中空闲连接的最小数量，默认为0
spring.redis.port								＃指定redis服务端端口，默认: 6379
spring.redis.sentinel.master					＃指定redis server的名称
spring.redis.sentinel.nodes						＃指定sentinel节点，逗号分隔，格式为host:port.
spring.redis.timeout							＃指定连接超时时间，毫秒单位，默认为0
＃resource
spring.resources.add-mappings					＃是否开启默认的资源处理，默认为true
spring.resources.cache-period					＃设定资源的缓存时效，以秒为单位.
spring.resources.chain.cache					＃是否开启缓存，默认为: true
spring.resources.chain.enabled					＃是否开启资源 handling chain，默认为false
spring.resources.chain.html-application-cache	＃是否开启h5应用的cache manifest重写，默认为: false
spring.resources.chain.strategy.content.enabled	＃是否开启内容版本策略，默认为false
spring.resources.chain.strategy.content.paths	＃指定要应用的版本的路径，多个以逗号分隔，默认为:[/**]
spring.resources.chain.strategy.fixed.enabled	＃是否开启固定的版本策略，默认为false
spring.resources.chain.strategy.fixed.paths		＃指定要应用版本策略的路径，多个以逗号分隔
spring.resources.chain.strategy.fixed.version	＃指定版本策略使用的版本号
spring.resources.static-locations				＃指定静态资源路径，默认为classpath:[/META-INF/resources/,/resources/, /static/, /public/]以及context:/
＃security     spring security是springboot支持的权限控制系统。
security.basic.authorize-mode					＃要使用权限控制模式.
security.basic.enabled							＃是否开启基本的鉴权，默认为true
security.basic.path								＃需要鉴权的path，多个的话以逗号分隔，默认为[/**]
security.basic.realm							＃HTTP basic realm 的名字，默认为Spring
security.enable-csrf							＃是否开启cross-site request forgery校验，默认为false.
security.filter-order							＃Security filter chain的order，默认为0
security.headers.cache							＃是否开启http头部的cache控制，默认为false.
security.headers.content-type					＃是否开启X-Content-Type-Options头部，默认为false.
security.headers.frame							＃是否开启X-Frame-Options头部，默认为false.
security.headers.hsts							＃指定HTTP Strict Transport Security (HSTS)模式(none, domain, all).
security.headers.xss							＃是否开启cross-site scripting (XSS) 保护，默认为false.
security.ignored								＃指定不鉴权的路径，多个的话以逗号分隔.
security.oauth2.client.access-token-uri			＃指定获取access token的URI.
security.oauth2.client.access-token-validity-seconds		＃指定access token失效时长.
security.oauth2.client.additional-information.[key]			＃设定要添加的额外信息.
security.oauth2.client.authentication-scheme				＃指定传输不记名令牌(bearer token)的方式(form, header, none,query)，默认为header
security.oauth2.client.authorities				＃指定授予客户端的权限.
security.oauth2.client.authorized-grant-types	＃指定客户端允许的grant types.
security.oauth2.client.auto-approve-scopes		＃对客户端自动授权的scope.
security.oauth2.client.client-authentication-scheme			＃传输authentication credentials的方式(form, header, none, query)，默认为header方式
security.oauth2.client.client-id				＃指定OAuth2 client ID.
security.oauth2.client.client-secret			＃指定OAuth2 client secret. 默认是一个随机的secret.
security.oauth2.client.grant-type				＃指定获取资源的access token的授权类型.
security.oauth2.client.id						＃指定应用的client ID.
security.oauth2.client.pre-established-redirect-uri			＃服务端pre-established的跳转URI.
security.oauth2.client.refresh-token-validity-seconds		＃指定refresh token的有效期.
security.oauth2.client.registered-redirect-uri				＃指定客户端跳转URI，多个以逗号分隔.
security.oauth2.client.resource-ids				＃指定客户端相关的资源id，多个以逗号分隔.
security.oauth2.client.scope					＃client的scope
security.oauth2.client.token-name				＃指定token的名称
security.oauth2.client.use-current-uri			＃是否优先使用请求中URI，再使用pre-established的跳转URI. 默认为true
security.oauth2.client.user-authorization-uri	＃用户跳转去获取access token的URI.
security.oauth2.resource.id						＃指定resource的唯一标识.
security.oauth2.resource.jwt.key-uri			＃JWT token的URI. 当key为公钥时，或者value不指定时指定.
security.oauth2.resource.jwt.key-value			＃JWT token验证的value. 可以是对称加密或者PEMencoded RSA公钥. 可以使用URI作为value.
security.oauth2.resource.prefer-token-info		＃是否使用token info，默认为true
security.oauth2.resource.service-id				＃指定service ID，默认为resource.
security.oauth2.resource.token-info-uri			＃token解码的URI.
security.oauth2.resource.token-type				＃指定当使用userInfoUri时，发送的token类型.
security.oauth2.resource.user-info-uri			＃指定user info的URI
security.oauth2.sso.filter-order				＃如果没有显示提供WebSecurityConfigurerAdapter时指定的Filter order.
security.oauth2.sso.login-path					＃跳转到SSO的登录路径默认为/login.
security.require-ssl							＃是否对所有请求开启SSL，默认为false.
security.sessions								＃指定Session的创建策略(always, never, if_required, stateless).
security.user.name								＃指定默认的用户名，默认为user.
security.user.password							＃默认的用户密码.
security.user.role								＃默认用户的授权角色.
＃sendgrid
spring.sendgrid.password						＃指定SendGrid password.
spring.sendgrid.proxy.host						＃指定SendGrid proxy host.
spring.sendgrid.proxy.port						＃指定SendGrid proxy port.
spring.sendgrid.username						＃指定SendGrid username.
＃server配置
server.address								＃指定server绑定的地址
server.compression.enabled					＃是否开启压缩，默认为false.
server.compression.excluded-user-agents		＃指定不压缩的user-agent，多个以逗号分隔，默认值为:text/html,text/xml,text/plain,text/css
server.compression.mime-types				＃指定要压缩的MIME type，多个以逗号分隔.
server.compression.min-response-size		＃执行压缩的阈值，默认为2048
server.context-parameters.[param name]		＃设置servlet context 参数
server.context-path							＃设定应用的context-path.
server.display-name							＃设定应用的展示名称，默认: application
server.jsp-servlet.class-name				＃设定编译JSP用的servlet，默认: org.apache.jasper.servlet.JspServlet)
server.jsp-servlet.init-parameters.[param name]				＃设置JSP servlet 初始化参数.
server.jsp-servlet.registered				＃设定JSP servlet是否注册到内嵌的servlet容器，默认true
server.port									＃设定http监听端口
server.servlet-path							＃设定dispatcher servlet的监听路径，默认为: /
＃SHELL      REMOTE SHELL  
shell.auth = simple								＃认证类型。根据环境自动检测。  
shell.auth.jaas.domain =my-domain				＃JAAS域。  
shell.auth.key.path =							＃认证密钥的路径。这应该指向一个有效的“.pem”文件。  
shell.auth.simple.user.name = user				＃登录用户。  
shell.auth.simple.user.password =				＃登录密码。  
shell.auth.spring.roles = ADMIN					＃用于登录到CRaSH控制台的所需角色的逗号分隔列表。  
shell.command-path-patterns = classpath *：/ commands / **，classpath *：/ crash / commands / **＃用于查找命令的模式。  
shell.command-refresh-interval = -1				＃扫描更改并在必要时更新命令（以秒为单位）。  
shell.config-path-patterns = 					＃用于查找配置的模式。  
shell.disabled-commands 						＃禁用命令的逗号分隔列表。  
shell.disabled-plugins =						＃禁用逗号分隔的插件列表。根据环境，某些插件默认是禁用的。  
shell.ssh.auth-timeout =						＃用户提示重新登录后的毫秒数。  
shell.ssh.enabled = true						＃启用CRaSH SSH支持。  
shell.ssh.idle-timeout =						＃关闭未使用的连接之后的毫秒数。  
shell.ssh.key-path =							＃SSH服务器密钥的路径。  
shell.ssh.port = 2000							＃SSH端口。  
shell.telnet.enabled = false					＃启用CRaSH telnet支持。如果TelnetPlugin可用，则默认启用。  
shell.telnet.port = 5000						＃Telnet端口。  
＃social
spring.social.auto-connection-views				＃是否开启连接状态的视图，默认为false
spring.social.facebook.app-id					＃指定应用id
spring.social.facebook.app-secret				＃指定应用密码
spring.social.linkedin.app-id					＃指定应用id
spring.social.linkedin.app-secret				＃指定应用密码
spring.social.twitter.app-id					＃指定应用ID.
spring.social.twitter.app-secret				＃指定应用密码
＃ssl配置
server.ssl.ciphers								＃是否支持SSL ciphers.
server.ssl.client-auth							＃设定client authentication是wanted 还是 needed.
server.ssl.enabled								＃是否开启ssl，默认: true
server.ssl.key-alias							＃设定key store中key的别名.
server.ssl.key-password							＃访问key store中key的密码.
server.ssl.key-store							＃设定持有SSL certificate的key store的路径，通常是一个.jks文件.
server.ssl.key-store-password					＃设定访问key store的密码.
server.ssl.key-store-provider					＃设定key store的提供者.
server.ssl.key-store-type						＃设定key store的类型.
server.ssl.protocol								＃使用的SSL协议，默认: TLS
server.ssl.trust-store							＃持有SSL certificates的Trust store.
server.ssl.trust-store-password					＃访问trust store的密码.
server.ssl.trust-store-provider					＃设定trust store的提供者.
server.ssl.trust-store-type						＃指定trust store的类型.
＃tomcat服务器配置（ServerProperties）  
server.port = 8080   										＃端口
server.address =  											＃该服务绑定IP地址，启动服务器时如本机不是该IP地址则抛出异常启动失败，只有特殊需求的情况下才配置
server.session-timeout =									＃会话超时秒数  默认30
server.context-path =   									＃上下文路径，默认为  '/'  
server.servlet-path =										＃servlet路径，默认为  '/'  
server.tomcat.access-log-pattern =							＃访问日志的日志模式  
server.tomcat.access-log-enabled = false  					＃启用访问日志记录  
server.tomcat.protocol-header = x -forwarded-proto 			＃ssl转发标头  
server.tomcat.accesslog.pattern								＃设定access logs的格式，默认: common
server.tomcat.accesslog.prefix								＃设定Log 文件的前缀，默认: access_log
server.tomcat.accesslog.suffix								＃设定Log 文件的后缀，默认: .log
server.tomcat.background-processor-delay = 30 ; 			＃ 后台线程方法的Delay大小: 30
server.tomcat.basedir										＃设定Tomcat的base 目录，如果没有指定则使用临时目录.
server.tomcat.internal-proxies								＃设定信任的正则表达式，默认:“10\.\d{1,3}\.\d{1,3}\.\d{1,3}| 192\.168\.\d{1,3}\.\d{1,3}|
															＃169\.254\.\d{1,3}\.\d{1,3}| 127\.\d{1,3}\.\d{1,3}\.\d{1,3}| 172\.1[6-9]{1}\.\d{1,3}\
															＃.\d{1,3}| 172\.2[0-9]{1}\.\d{1,3}\.\d{1,3}|172\.3[0-1]{1}\.\d{1,3}\.\d{1,3}”
server.tomcat.max-http-header-size							＃设定http header的最小值，默认: 0
server.tomcat.max-threads									＃设定tomcat的最大工作线程数，默认为: 0
server.tomcat.port-header									＃设定http header使用的，用来覆盖原来port的value.
server.tomcat.protocol-header								＃设定Header包含的协议，通常是 X-Forwarded-Proto，如果remoteIpHeader有值，则将设置为RemoteIpValve.
server.tomcat.protocol-header-https-value					＃设定使用SSL的header的值，默认https.
server.tomcat.remote-ip-header								＃设定remote IP的header，如果remoteIpHeader有值，则设置为RemoteIpValve
server.tomcat.uri-encoding									＃设定URI的解码字符集.
＃THYMELEAF（Thymeleaf 模板）  
spring.thymeleaf.prefix =    								＃类路径：/模板/  检查模板位置
spring.thymeleaf.suffix =  
spring.thymeleaf.mode = HTML5  								＃模板的模式
spring.thymeleaf.encoding = UTF- 8  
spring.thymeleaf.content-type = text / html＃; charset = <编码>  
spring.thymeleaf.cache = true  								＃这个开发配置为false，避免改了模板还要重启服务器 
＃undertow
server.undertow.access-log-dir					＃设定Undertow access log 的目录，默认: logs
server.undertow.access-log-enabled				＃是否开启access log，默认: false
server.undertow.access-log-pattern				＃设定access logs的格式，默认: common
server.undertow.accesslog.dir					＃设定access log 的目录.
server.undertow.buffer-size						＃设定buffer的大小.
server.undertow.buffers-per-region				＃设定每个region的buffer数
server.undertow.direct-buffers					＃设定堆外内存
server.undertow.io-threads						＃设定I/O线程数.
server.undertow.worker-threads					＃设定工作线程数
＃velocity
spring.velocity.allow-request-override			＃指定HttpServletRequest的属性是否可以覆盖controller的model的同名项
spring.velocity.allow-session-override			＃指定HttpSession的属性是否可以覆盖controller的model的同名项
spring.velocity.cache							＃是否开启模板缓存
spring.velocity.charset							＃设定模板编码
spring.velocity.check-template-location			＃是否检查模板路径是否存在.
spring.velocity.content-type					＃设定ContentType的值
spring.velocity.date-tool-attribute				＃设定暴露给velocity上下文使用的DateTool的名
spring.velocity.enabled							＃设定是否允许mvc使用velocity
spring.velocity.expose-request-attributes		＃是否在merge模板的时候，将request属性都添加到model中
spring.velocity.expose-session-attributes		＃是否在merge模板的时候，将HttpSession属性都添加到model中
spring.velocity.expose-spring-macro-helpers		＃设定是否以springMacroRequestContext的名来暴露RequestContext给Spring’s macro类库使用
spring.velocity.number-tool-attribute			＃设定暴露给velocity上下文的NumberTool的名
spring.velocity.prefer-file-system-access		＃是否优先从文件系统加载模板以支持热加载，默认为true
spring.velocity.prefix							＃设定velocity模板的前缀.
spring.velocity.properties						＃设置velocity的额外属性.
spring.velocity.request-context-attribute		＃设定RequestContext attribute的名.
spring.velocity.resource-loader-path			＃设定模板路径，默认为: classpath:/templates/
spring.velocity.suffix							＃设定velocity模板的后缀.
spring.velocity.toolbox-config-location			＃设定Velocity Toolbox配置文件的路径，比如 /WEB-INF/toolbox.xml.
spring.velocity.view-names						＃设定需要解析的视图名称.
```

> 各种配置都有默认的，如果我们想要修改其默认的配置，那么我们可以在 `resources\application.properties` 中进行修改，application.properties 需要我们手动创建，其中里面的每一条配置都对应一个类



### 1.5.5 常用配置

```properties
# 端口号
server.port=8081
# 应用的上下文路径（项目路径）
server.servlet.context-path=/context

# 在 mybatis 中指定扫描包（在起别名的时候生效）
mybatis.type-aliases-package=com.xzh

# 指定 mapper.xml 的路径
mybatis.mapper-locations=classpath:com/xzh.allModel/mapper

# session 的生效时间（单位：s）
spring.session.timeout=18000

## 数据库连接配置
# 连接的 url
mysql.one.jdbc-url=jdbc:mysql://localhost:8080/test?serverTimezone=Asia/Shanghai&useSSL=false
# mysql 数据库用户名
mysql.one.username=xxx
# 数据库密码
mysql.one.password=xxx
# 线程池允许的最大连接数
mysql.one.maximum-pool-size=15

#日志打印:日志级别    trace<debug<info<warn<error<fatal    默认级别为  info，即默认打印  info  及其以 上级别的日志
#logging.level 设置日志级别，后面跟生效的区域，比如  root  表示整个项目，也可以设置为某个包下， 也可以具体到某个类名（日志级别的值不区分大小写）
logging.level.com.cxs.allmodel.=debug
logging.level.com.cxs.allmodel.mapper=debug logging.level.org.springframework.web=info logging.level.org.springframework.transaction=info logging.level.org.apache.ibatis=info
logging.level.org.mybatis=info
logging.level.com.github.pagehelper = info
logging.level.root=info
#日志输出路径
logging.file.name=/tmp/api/allmodel.log

# mapper 分页插件
pagehelper.helperDialect=mysql
pagehelper.reasonable=true
pagehelper.supportMethodsArguments=true
pagehelper.params=count=countSql

#jackson  时间格式化
spring.jackson.serialization.fail-on-empty-beans=false
#指定日期格式，比如  yyyy-MM-dd HH:mm:ss，或者具体的格式化类的全限定名 spring.jackson.date-format=yyyy-MM-dd HH:mm:ss
#指定日期格式化时区，比如  America/Los_Angeles  或者  GMT+10 spring.jackson.time-zone=GMT+8
#设置统一字符集
server.servlet.encoding.charset=utf8
#redis 连接配置
# redis  所在主机  ip  地址
spring.redis.host=
#redis 服务器密码
spring.redis.password=
#redis 服务器端口号
spring.redis.port=
#redis 数据库的索引编号(0 到  15)
spring.redis.database=14
##    连接池的最大活动连接数量，使用负值无限制
spring.redis.pool.max-active=8
##    连接池的最大空闲连接数量，使用负值表示无限数量的空闲连接 
spring.redis.pool.max-idle=8
##    连接池最大阻塞等待时间，使用负值表示没有限制 
spring.redis.pool.max-wait=-1ms
##    最小空闲连接数量，使用正值才有效果 
spring.redis.pool.min-idle=0
##    是否启用  SSL  连接.
spring.redis.ssl=false

##    连接超时，毫秒为单位
spring.redis.timeout= 18000ms
##    集群模式下，集群最大转发的数量
spring.redis.cluster.max-redirects=
##    集群模式下，逗号分隔的键值对（主机：端口）形式的服务器列表 
spring.redis.cluster.nodes=
##    哨兵模式下，Redis  主服务器地址
spring.redis.sentinel.master=

##    哨兵模式下，逗号分隔的键值对（主机：端口）形式的服务器列表 
spring.redis.sentinel.nodes= 127.0.0.1:5050,127.0.0.1:5060
```

除了这些预置的配置外，我们也可以在 properties 文件中自定义配置，通过 `@Value("${}")` 获取对应的属性值

```properties
# application.properties 文件中
my.website=https://www.baidu.com
```

```java
// 某个bean
@Value("${my.website}")
private String url;
```



:herb:拓展1：探究 Spring Boot 是如何读取 application.properties?

我们打开 IDEA ，快捷键 Ctrl + N 打开类搜索框，输入 ConfigFileApplicationListener，查看其源码，我们主要看其下面两个属性：

```java
private static final String DEFAULT_SEARCH_LOCATIONS = "classpath:/,classpath:/config/,file:./,file:./config/*/,file:./config/";
private static final String DEFAULT_NAMES = "application";
```

- `DEFAULT_SEARCH_LOCATIONS`：默认的搜索路径，其中 `classpath:` 即类路径，`file:` 即项目根路径
- `DEFAULT_NAMES`：默认的文件名



### 1.5.7 按需加载原则

1. 自动配置遵守按需加载原则，也就是说，引入了那个场景 starter 就加载该场景关联的 jar，没有引入则不会加载其关联的 jar
2. Spring Boot 所有自动配置功能都在 spring-boot-autoconfigure 包里面，在 Spring Boot 自动配置包中，一般是 XxxAutoConfiguration.java 对应 XxxProperties.java



# 2 容器功能

## 2.1 @Congiguration 

前面在学习 Spring Framework 的时候，我们都是通过配置 beans.xml 来实现添加/注入组件，在 Spring Boot 中，我们可以使用注解 `@Configuration` 来添加和注入组件

```java
/**
 * 1. @Configuration 标识这是一个配置类，等价于配置文件
 * 2. 程序员可以通过 @Bean 注解注入 bean 对象到容器中
 */
@Configuration
public class BeanConfig {
    /**
     * 1. @Bean: 给容器添加组件，就是 Monster bean
     * 2. monster01: 这个即是方法名，又是 id 名
     * 3. Monster: 返回值代表了注入类型，注入 bean 的类型是 Monster
     * 4. new Monster(200, "牛魔王", 500, "封魔拳") 反应了注入到容器中具体的 Bean 信息
     * 5. 如果我们想要指定 id 的名，而不是使用其方法名，那么我们可以使用 @Bean(name = "xxx") 这种形式
     * 6. 默认是单例注入
     * 7. 通过
     */
    @Bean
    @Scope("prototype")
    public Monster monster01() {
        return new Monster(200, "牛魔王", 500, "封魔拳");
    }
}
```

> `@Bean` 注解中参数 value 和 name 是相同的，设置的时候，两个参数只能选择一个，原因是@AliasFor导致的，其中传入的参数类型为 `String[]`，第一个值作为 bean 名，其他值作为别名



:warning: 注意事项

1. 当一个类被 `@Configuration` 标识时，该类也会被当做一个 bean 注入到容器中

2. Spring Boot2 中新增一个特性 —— proxyBeanMethod 可以 指定 Full 模式和 Lite 模式

   - `Full(proxyBeanMethods = true)`：保证每个 `@Bean` 方法被调用多少次返回的组件都是单实例的，是代理方式
   - `Lite(proxyBeanMethods = false)`：每个 `@Bean` 方法被调用多少次返回的组件都是新创建的，是非代理方式

   > 注意：proxyBeanMethods 是在调用 `@Bean` 方法才生效，因此，需要先获取 BeanConfig 组件，再调用 proxyBeanMethods，如果直接通过 `ioc.getBean()` 的方式获取 Bean，proxyBeanMethods 值并没有生效
   >
   > > 选择依据：组件依赖必须要使用 Full 模式；如果不存在组件依赖，那么我们可以使用 Lite 模式提高性能

3. 和 beans.xml 可以配置多个是同样的道理，配置类也可以配置多个，但是要保证方法名不同，因为默认情况下，方法名即为 id 名



示例

```java
@Configuration(proxyBeanMethods = false)
public class BeanConfig {
    @Bean
    public Monster monster01() {
        return new Monster(200, "牛魔王", 500, "封魔拳");
    }
}
```

```java
@SpringBootApplication
public class MainApp {
    public static void main(String[] args) {
        // 启动 springboot 应用程序
        ConfigurableApplicationContext ioc = SpringApplication.run(MainApp.class, args);

        /**
         * 先拿到 beanConfig ，再通过调用方法得到bean
         */
        // 1. 先得到 beanConfig 对象
        BeanConfig beanConfig = ioc.getBean(BeanConfig.class);
        // 2. 再通过 beanConfig 得到 Bean 对象
        Monster monster_01 = beanConfig.monster01();
        Monster monster_02 = beanConfig.monster01();
        System.out.println("monster_01 = " + monster_01);
        System.out.println("monster_02 = " + monster_02);

        System.out.println("==============================");

        /**
         * 直接通过 ioc 容器的 getBean 方法拿 bean
         * 注意：这里没有给 bean 添加上 @Scope("prototype")
         */
        Monster monster_03 = ioc.getBean("monster01", Monster.class);
        Monster monster_04 = ioc.getBean("monster01", Monster.class);
        System.out.println("monster_03  = " + monster_03);
        System.out.println("monster_04 = " + monster_04);
    }
}
```

最终打印结果

```
monster_01 = com.xzh.springboot.bean.Monster@5300f14a
monster_02 = com.xzh.springboot.bean.Monster@1f86099a
==============================
monster_03  = com.xzh.springboot.bean.Monster@77bb0ab5
monster_04 = com.xzh.springboot.bean.Monster@77bb0ab5
```

可以看到，在 Lite 模式下，如果我们通过 beanConfig 拿 bean ，每次返回的结果不同；而如果直接使用 ioc 获取 bean ，proxyBeanMethods 就会失效



## 2.2 @Import

`@Import` 注解可以用来注入组件，例如：

```java
@Import({Monster.class})
@Configuration(proxyBeanMethods = false)
public class BeanConfig {
}
```

> 注意：java 中注解默认都会有 value 属性，用来配置该注解的主要功能，如果配置属性只有这一个，那么 value 可以省略不写，上面的 `@Import({Monster.class})` 的完整写法为 `@Import(value = {Monster.class})`



## 2.3 @Conditional

`@Conditional` 是 spring4 提供的一个新的注解，它的作用是按照一定的条件进行判断，满足条件的将会给 IOC 容器注入 bean，而在 Spring Boot 中又提供了一系列 `@ConditionalOnXxx` 的注解用来拓展

| @Conditional扩展注解           | 作用（判断是否满足当前指定条件）                 |
| ------------------------------ | ------------------------------------------------ |
| @ConditionOnJava               | 系统的Java版本是否符合要求；                     |
| @ConditionOnBean               | 容器中存在指定Bean；                             |
| @ConditionOnMissingBean        | 容器中不存在指定Bean；                           |
| @ConditionOnExpression         | 满足SpEL表达式指定；                             |
| @ConditionOnClass              | 系统中有指定的类；                               |
| @ConditionOnMissingClass       | 系统中没有指定的类；                             |
| @ConditionOnSingleCandidate    | 容器中只有一个指定的bean，或者这个bean是首选bean |
| @ConditionOnProperty           | 系统中指定的属性是否有指定的值                   |
| @ConditionOnResource           | 类路径下是否存在指定资源文件                     |
| @ConditionalOnWebApplication   | 当前是web环境                                    |
| @ConditionalOnNoWebApplication | 当前不是web环境                                  |
| @ConditionalOnJndi             | JNDI存在指定项                                   |

示例

```java
@Import({Monster.class})
@Configuration(proxyBeanMethods = false)
public class BeanConfig {

    @Bean(name = "master01")
    public Master master01() {
        return new Master();
    }

    /**
     * 注解 @ConditionalOnBean(name = "master01") 表示当容器中存在 id 为  master01
     * 的 bean 时才会将下面的 bean 注入
     * 注意：该注解对返回值类型不做要求
     */
    @Bean
    @ConditionalOnBean(name = "master01")
    public Monster monster01() {
        return new Monster(200, "牛魔王", 500, "封魔拳");
    }
}
```



## 2.4 @ImportResource

`@ImportResource` 注解用于<u>导入 Spring 的配置文件</u>

其源码如下：

```java
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.TYPE})
@Documented
public @interface ImportResource {
    @AliasFor("locations")
    String[] value() default {};

    @AliasFor("value")
    String[] locations() default {};

    Class<? extends BeanDefinitionReader> reader() default BeanDefinitionReader.class;
}
```

> 这里就比较有意思了，上述的代码表明，在 `@ImportResource` 注解中，locations 参数和 value 参数其实就是一回事，两这通过别名别名相互替代

```java
@ImportResource(locations = "classpath:beans.xml")
public class BeanConfig {}
```



## 2.5 配置绑定

配置绑定：使用 Java 读取到 Spring Boot 核心配置文件 application.properties 的内容，并且将它封装到 JavaBean 中

在 application.properties 中添加如下配置

```properties
# 前缀用于区别，. 后面的为配置的 bean 的属性名
monster01.id=100
monster01.name=zs
monster01.age=20
monster01.skill=dance
```

之后，来到 Monster 这个实体类中添加 `@ConfigurationProperties` 这个注解用来读取配置文件相应属性值并封装为 bean 注入到容器中

```java
@Component
@ConfigurationProperties(prefix = "monster01")
@Data
@NoArgsConstructor
@AllArgsConstructor
public class Monster {
    /*主键id*/
    private int id;
    /*姓名*/
    private String name;
    /*年龄*/
    private Integer age;
    /*技能*/
    private String skill;
}
```

最后，我们将其装配到控制层的类中，通过发送网络请求来进行测试

```java
@Controller
public class TestController {
    /**
     * 将前面的 bean 装配到属性中
     */
    @Resource
    private Monster monster;

    @RequestMapping("/test")
    @ResponseBody
    public String test() {
        return monster.toString();
    }
}
```

:warning:注意事项

01 上述代码，如果我们没有在 Monster 类上添加 `@Component` 注解将其添加到容器中，也可以使用 config 类中导入，如下：

```java
/**
 * 注解 @EnableConfigurationProperties 的功能
 * 1. 开启配置绑定功能
 * 2. 将 Monster 组件自动注入到容器中
 */
@Configuration
@EnableConfigurationProperties({Monster.class})
public class BeanConfig {}
```

> 其实即使我们不添加 `@EnableConfigurationProperties({Monster.class})` 也可以正常运行，应为在主程序中已经自动帮助我们添加了注解 `@EnableAutoConfiguration`



02 解决配置文件中文乱码

如果 application.properties 中存在中文，那么会发生乱码，解决方案：使用 unicode 编码即可



# 3 Spring Boot 机制手动实现



# 4 Spring Boot 中使用 lombok

## 4.1 lombok 常用注解

| 注解                | 含义                                                         |
| ------------------- | ------------------------------------------------------------ |
| @Data               | 注解在类上；提供类所有属性的 getter 和 setter 方法，此外还提供了 equals、canEqual、hashCode、toString 等方法 |
| @Setter             | 注解在属性上；为属性提供 setter 方法                         |
| @Getter             | 注解在属性上；为属性提供 getter 方法                         |
| @Log4j              | 注解在类上；为类提供一个属性名为 log 的 log4j 的日志对象     |
| @NoArgsConstructor  | 注解在类上；为类提供一个无参的构造方法                       |
| @AllArgsConstructor | 注解在类上；为类提供一个全参的构造方法                       |
| @Cleanup            | 关闭流                                                       |
| @Builder            | 生成构造器                                                   |
| @Synchronized       | 加同步锁                                                     |
| @SneakyThrows       | 等同于添加 try/catch 捕获异常                                |
| @NonNull            | 如果给参数添加这个注解，当注解为 null 时会抛出空指针异常     |
| @Value              | 和 @Data 类似，但是会把所有成员变量默认定义为 private final 修饰，并且不会生成 setter 方法 |



## 4.2 使用方法

我们在使用 lombok 之前，需要在父工程项目中添加如下依赖配置：

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.24</version>
    <scope>provided</scope>
</dependency>
```

不过，现在的 idea 版本中已经自带 lombok 插件，我们可以直接使用



`@Data` 注解解释

```
Equivalent to {@code @Getter @Setter @RequiredArgsConstructor @ToString @EqualsAndHashCode}.
```

>`@RequiredArgsConstructor` 用于替代 `@Autowired` 自动装配的，要求装配的属性使用 `final` 修饰



lombok 中还可以通通过 `@Slf4j` 注解来使用日志门面输出，该注解其实对下面代码的简化

```java
private static final Logger logger = LoggerFactory.getLogger(xxx.class);
```

需要引入的依赖

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.16.10</version>
</dependency>

<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-log4j12</artifactId>
    <version>1.7.28</version>
</dependency>
```

基本使用，如下：

```java
int a = 1;
// 日志打印：字符串拼接方式
log.info("a = " + a);
// 日志打印：占位符打印方式
log.info("a = {}", a);
// 除此之外还可以打印其他信息，例如错误日志打印，调试日志打印、警告日志打印等一共会有 8 个等级
log.error("xxx");
log.debug("xxx");
log.warn("xxx");
log.trace("xxx");
```



:warning:注意事项

1. 当我们在使用 `@AllArgsConstructor` 注解时会覆盖掉原来的无参构造器，但是在 Spring Boot 中 bean 都是通过反射注入到容器中，因此需要使用 `NoArgsConstructor` 来生成无参构造器
2. 我们如果使用 application.properties 或者 beans.xml 来配置 bean，那么需要提供 setter 方法进行设置，因为 spring 属性的配置依靠 setter 方法完成，我们可以借用 `@Setter` 来完成
3. `@Response` 注解需要将返回值转换为 JSON 格式，底层用到了 getter，我们可以借用 `@Getter` 来完成



# 5 工程创建与配置文件

## 5.1 Spring Initailizr

1. Spring 官方提供了 Spring Initializr 来构建项目，能完美支持 IDEA 和 Eclipse，让程序员来选择需要的开发场景（starter），同时还能自动生成启动类和单元测试代码
2. Spring Initailizr 对 IDEA 版本有要求，同时会走网络，所以不太稳定，可能会出现创建失败的情况



创建方式

1. 通过 IDEA 集成工具，找到 Spring Initailizr（如果没有该选项，我们需要安装插件）创建即可
2. 通过 https://start.spring.io/ 选择好相关的配置后直接下载下来打开即可



## 5.2 yaml 文件

基本说明

1. YAML 是 "YAML Ain't a Markup Language"(yaml 不是一种标记语言) 的缩写。在开发这种语言时，YAML 的意思其实是 "Yet Another Markup Language"(仍是一种标记语言)
2. YAML 以数据作为中心，而不是以标记语言为重点
3. YAML 和传统和标记语言不同，以缩进来表示从属关系

> 官方文档：https://yaml.org/



### 5.2.1 基本语法

1. 形式为 `key: value` 的形式

   > 注意：这里冒号后面必须要带一个空格

2. 区分大小写

3. 使用缩颈表示层级关系

4. 缩进不允许使用 tab，只允许使用空格

5. 缩进的空格数不重要，只要相同层级的元素左对齐即可

6. 字符串无需加引号

7. yaml 中注释使用 `#`



### 5.2.2 数据类型

yaml 中数据类型分为如下几种：

- 字面量
- 数组
- 对象

**01 字面量**

字面量即是单个的，不可再分的值，主要有 null、number、string、boolean、date

```yaml
monster:
  id: 100 # number
  name: 牛魔王 # string
  isMarried: true # boolean
  birthday: 2000/10/10 # date
```



02 数组

例如 array、list、queue

```yaml
# 行内写法
k: [v1, v2, v3]
# 缩进展开写法
z: 
  - v1
  - v2
  - v3
```





03 对象

键值对的集合，例如 map、hash、set、object

```yaml
# 行内写法
k: {k1: v1, k2: v2, k3: v3}
# 缩进展开写法
z:
  k1: v1
  k2: v2
  k3: v3
```



### 5.2.3 注意事项

1. 如果 application.properties 和 application.yaml 有相同的前缀值绑定，则 application.properties 优先级更高，应当避免

2. 在 yaml 中字符串无需使用 `''` 或者 `""` 包裹起来，如果裹起来其实也没有任何问题

3. 如果我们在 bean 中已经配置了注入的注解，那么在 spring boot 中配置 yaml 是会有自动提示的，如果发现自动提示不生效，那么我们需要手动添加如下依赖

   ```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-configuration-processor</artifactId>
       <optional>true</optional>
   </dependency>
   ```

   如果仍然不提示，我们可以在 IDEA 中安装一个插件 YAML/Ansible support



# 6 静态资源访问说明

## 6.1 基本介绍

1. 只要静态资源放在类路径下：/static、/public、/resources、/META-INF/resources 可以被直接访问

   > 我们可以通过访问 WebProperties 类查看源码

2. 常见静态资源：JS、CSS、图片、字体文件等

3. 访问方式：默认路径为 "根路径/ + 静态资源名"

   > 我们可以在 WebMVCProperties 类中进行查看
   >
   > ```java
   > private String staticPathPattern = "/**";
   > ```



## 6.2  注意事项

1. 静态资源访问原理：静态映射是 `/**`，也就是对所有请求进行拦截，请求进来，先看 Controller 层能不能处理，不能处理的话再交给静态资源处理器，如果静态资源找不到则响应 404 页面

2. 当发生 controller 请求路径与静态资源路径冲突时，我们可以通过修改静态资源前缀来避免

   ```yaml
   # 注意：后面的 /** 一定不能漏掉，这个表示拦截所有请求
   spring:
     mvc:
       static-path-pattern: /static/**
   ```

3. 默认的静态资源存放位置为

   ```java
   private static final String[] CLASSPATH_RESOURCE_LOCATIONS = { "classpath:/META-INF/resources/",
   				"classpath:/resources/", "classpath:/static/", "classpath:/public/" };
   ```

   如果我们想要修改静态资源的存放路径，可以如下修改：

   ```yaml
   spring:
     web:
       resources:
         static-locations: [classpath:/xzh/] # 修改当前路径的访问路径
   ```



# 7 REST 风格请求处理

REST 风格：使用 HTTP 请求方式动词来表示对资源的操作

示例如下：

```java
@RestController
public class MonsterController {

    @GetMapping("/monster")
    public String getMonster() {
        return "GET-查询妖怪";
    }


    @PostMapping("/monster")
    public String saveMonster() {
        return "POST-添加妖怪";
    }

    @PutMapping("/monster")
    public String putMonster() {
        return "PUT-修改妖怪";
    }

    @DeleteMapping("/monster")
    public String delMonster() {
        return "DELETE-删除妖怪";
    }
}
```

> `@RestController` 是 `@Controller` 和 `@ResponseBody` 的组合



**细节说明**

1. 客户端是 postman 可以这直接发送 put、delete 等方式的请求，可以不用设置 filter

2. 如果需要 Spring Boot 支持页面表单的 Rest 功能，则需要注意如下细节

   1. Rest 风格请求核心 filter：HiddenHttpMethodFilter，表单会被 HiddenHttpMethodFilter 拦截，获取到表单的 _method 的值，则判断是 PUT/DELETE/PATCH 

      > PATCH 请求是对 PUT 请求的补充，用于对已知资源进行局部更新

   2. 如果要 Spring Boot 支持页面表单的 Rest 功能，需要在 application.yaml 启用 filter 功能，否则无效

      ```yaml
      spring:
        mvc:
          hiddenmethod:
            filter:
              enabled: true
      ```



# 8 请求接收参数注解

主要包括：`@PathVariable`、`@RequestHeader`、`@RequestParam`、`@CookieValue`、`@RequestBody`、`@RequestAttribute`、`@SessionAttribute`

## 8.1 RequestAttribute

该注解用于获取 request 域的属性

```java
/*
 * required 表示 request 域中该属性的存在是否为必选，如果为 true，但是 request 域中没有改属性就会报错
 */
public String ok(@RequestAttribute(value = "username", required = false) String username) {
    return "success";
}
```



## 8.2 @SessionArrtibue

```java
public String ok(@SessionArrtibue(value = "username", required = false) String username) {
    return "success";
}
```



## 8.3 复杂参数

在开发中，Spring Boot 在响应客户端请求时，也支持复杂参数

1. Map、Model、Errors / BindingResult、RedirectAttributes、ServletResponse、SessionStatus、UriComponentsBuilder、ServletUriComponentsBuilder、HttpSession
2. Map、Model 数据会被放在 request 域中
3. RedirectAttributes 重定向携带数据

```java
@Controller
@Slf4j
public class MyController {

    @GetMapping("/register")
    public String register(Map<String, Object> map, Model model, HttpServletResponse response) {
        // 封装数据到 map 和 model 中
        map.put("username", "Susan");
        map.put("password", "123456");
        model.addAttribute("job", "系统架构师");
        model.addAttribute("salary", 1000);
        // 创建 cookie，并通过 response 添加 cookie 到浏览器中
        Cookie email = new Cookie("email", "xxxx@souhu.com");
        response.addCookie(email);
        return "forward:registerOk";
    }

    @ResponseBody
    @GetMapping("/registerOk")
    public String registerOk(HttpServletRequest req) {
        log.info("username = {}", req.getAttribute("username"));
        log.info("password = {}", req.getAttribute("password"));
        log.info("job = {}", req.getAttribute("job"));
        log.info("salary = {}", req.getAttribute("salary"));
        return "success";
    }
}
```



## 8.4 自定义对象参数——自动封装

基本介绍：在开发过程中，Spring Boot 在响应客户端 / 浏览器请求时，也支持自定义对象参数完成**自动类型转换和格式化**，同时**支持级联封装**

01 定义两个 java bean

```java
@Data
public class Monster {
    /*主键 id*/
    private int id;

    /*姓名*/
    private String name;

    /*年龄*/
    private Integer age;

    /*是否已婚(false-未婚;true-已婚)*/
    private Boolean isMarried;

    /*生日*/
    private Date birthday;

    /*开的车的信息*/
    private Car car;
}
```

```java
@Data
public class Car {
    /*车名*/
    private String name;

    /*车的价格*/
    private String price;
}
```



02 创建 controller 

```java
@RestController
@Slf4j
public class ParameterController {
    @PostMapping("/saveMonster")
    public String saveMonster(Monster monster) {
        log.info("monster = " + monster);
        return "success";
    }
}
```



在 postman 中发送如下请求

![image-20230812170525478](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230812170525478.png)

我们在控制台便可以看到打印的 Monster 信息



## 8.5 自定义转换器

1. Spring Boot 在响应客户端请求时，将提交的数据封装承兑行，使用了内置的转换
2. Spring Boot 也支持自定义转换器，本身提供了 124 个内置转换器，我们可以查看 GenericConverter - ConvertiblePair

现在，我们来自定义一个转换器，前面我们使用 car.name 和 car.price 的形式完成转换，现在我们自定义一个转换器，可以直接将 car 进行转换

```java
@Configuration(proxyBeanMethods = false)
public class WebConfig {
    // 注入 WebMvcConfigurer 到容器中
    @Bean
    public WebMvcConfigurer webMvcConfigurer() {
        return new WebMvcConfigurer() {
            @Override
            public void addFormatters(FormatterRegistry registry) {
                /**
                 * 1. 在 addFormatters 方法中，增加一个自定义的转换器
                 * 2. 增加的自定义转换器会注册到 converters 容器中
                 * 3. converters 底层结构是 ConcurrentHashMap，内置有 124 个转换器
                 * 4.registry 有一个 addConverter 方法，里面的 Converter 指定泛型为 String, Car 表示将 String 转换为 Car
                 */
                registry.addConverter(new Converter<String, Car>() {

                    @Override
                    public Car convert(String s) {  // s 是 source 的缩写，表示源对象
                        if(!ObjectUtils.isEmpty(s)) {
                            Car car = new Car();
                            String[] split = s.split(",");
                            car.setName(split[0]);
                            // 这里需要将 String 类型转换为 Double 类型
                            car.setPrice(Double.parseDouble(split[1]));
                            return car;
                        }
                        return null;
                    }
                });

            }
        };
    }
}
```

之后我们在 postman 中发送请求

![image-20230812183938153](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230812183938153.png)



**注意事项**

1. 转换器的标识其实 k-v，也就是说 source->target 就标识一个转换器，当我们定义了多个相同转换类型的转换器，后面定义的转换器会将前面定义的转换器覆盖



# 9 进阶内容

## 9.1 处理 json

Spring Boot 支持返回 JSON  格式的数据，在启动 web starter 时，就已经引入了相关依赖

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-json</artifactId>
  <version>2.5.3</version>
  <scope>compile</scope>
</dependency>
```

而我们如果想要返回的使 JSON 数据格式，而不是一个 ModelAndView 视图，我们需要只需要在其方法上面添加一个 `@ResponseBody` 注解即可

示例（首先，我们需要在 Monster 和 Car 上添加 `@NoArgsConstructor` 和 `@AllArgsConstructor` 生成对应的全参构造器和无参构造器）

```java
@RestController
public class ParameterController {
    @GetMapping("/get/monster")
    public Monster getMonster() {
        Car car = new Car("劳斯莱斯", 100010.01);
        return new Monster(1, "黑山老妖", 200, false, new Date(), car);
    }
}
```

之后，我们输入 `localhost:8080/get/monster` 发送 get 请求便会发现最终返回的结果为 JSON 数据格式

我们可以查看 AbstractJackson2HttpMessageConverter 类进行源码分析，在 writeInternal 方法中



## 9.2 内容协商

内容协商：根据客户端接收能力的不同，Spring Boot 返回不同类型的数据，例如 application/json，application/pdf、application/gzip、application/xml 等

我们可以直接在 postman 中发送请求进行测试，将 Accept 从 `*/*` 修改为 `application/xml`，但是我们会发现会报错 406 Not Accepttable，解决方法也很简单，我们只需要引入如下依赖：

```xml
<dependency>
    <groupId>com.fasterxml.jackson.dataformat</groupId>
    <artifactId>jackson-dataformat-xml</artifactId>
</dependency>
```

> 注意：这里版本号就不需要添加了，直接使用 Spring Boot 的版本仲裁

之后，我们输入访问 url 到浏览器中会发现，返回的数据格式为 xml ，而不是 json？

我们到浏览器中抓包进行查看

![image-20230812205907450](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230812205907450.png)

我们可以看到浏览器是对处理数据的格式做了权重的（后面的 q 就代表该数据格式的权重值，越靠近 1 权重越大），application/xml 格式的权重还比较高，为 0.9 ，所以当服务器具有处理 xml 格式的能力时，便会传输到客户端

**注意事项和使用细节**

1. 对于浏览器，我们无法修改其 Accept 的值，那么我们应该如何处理？

   > 解决方案：开启支持基于请求参数的内容协商功能。修改 application.yaml 如下：
   >
   > ```yaml
   > spring:
   >   mvc:
   >     content negotiation:
   >       favor-parameter: true
   > ```
   >
   > 之后，我们到浏览器中指定响应格式即可：`url?format=json`，我们也可以指定内容协商的参数名，即不使用 format
   >
   > ```yaml
   > spring:
   >   mvc:
   >     content negotiation:
   >       favor-parameter: true
   >       parameter-name: xzh
   > ```



# 10 Thymeleaf

> 中文教程：https://fanlychie.github.io/post/thymeleaf.html

## 10.1 基本介绍

1. Thymeleaf 是一个 Velocity、FreeMarker 类似的模版引擎，可以完全替代 JSP
2. tHYmeleaf 是一个 Java 类库，设计一个 xml / xhtml / html5 的模版引擎，可以作为 mvc 的 web 应用的 view 层



优点

- 类似于 JSTL、OGNL 风格，语法相似，上手快
- Thymeleaf 模版页面无需服务器渲染，也可以被浏览器运行，页面简洁
- Spring Boot 支持 FreeMarker、Thymeleaf、veocity

缺点

- 不是一个高性能的引擎，只适合单体应用



## 10.2 基本语法

表达式

| 表达式名   | 语法  | 用途                               |
| ---------- | ----- | ---------------------------------- |
| 变量取值   | `${}` | 获取请求域、session 域             |
| 选择变量   | `*{}` | 获取上下文对象值                   |
| 消息       | `#{}` | 获取国际化等值                     |
| 链接       | `@{}` | 生成链接                           |
| 片段表达式 | `~{}` | jsp:include 作用，引入公共页面片段 |



字面量

- 字符串、数字、布尔值
- 空值：null

文本操作

- 字符串拼接：`+`
- 变量替换：`|age = ${age}|`

布尔运算

- and、or、not(!)

th 属性

| **编号** | **属性**                                                     | **描述**                                               | **示例**                                                     |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------ | ------------------------------------------------------------ |
| 1        | th:text                                                      | 计算其值表达式并将结果设置为标签的标签体               | <p th:text="${userName}">中国</p>，值为 null 为空时，整个标签不显示任何内容。 |
| 2        | th:utext                                                     | th:text 会对结果中的特殊字符转义，th:utext 不会转义    | <p th:utext="${userInfo}">中国</p>,值为 null 为空时，整个标签不显示任何内容。 |
| 3        | th:attr                                                      | 为标签中的任意属性设置，可以一次设置多个属性           | <a href="" th:attr="title='前往百度',href='http://baidu.com'">前往百度</a> |
| 4        | th:*                                                         | 为 html 指定的属性设值，一次设置一个                   | <a href="" th:title='前往百度' th:href="'http://baidu.com'">前往百度</a> |
| 5        | th:alt-title                                                 | 同时为 alt 与 title 属性赋值                           | <a href="#" th:alt-title="'th:A-B'">th:A-B</a>               |
| 6        | th:lang-xmllang                                              | 同时为 lang 、xmllang 属性赋值                         | <head lang="en" th:lang-xmllang="en">                        |
| 7        | [th:fragment](https://blog.csdn.net/wangmx1993328/article/details/84747497) | 定义模板片段                                           | <div th:fragment="copy">                                     |
| 8        | [ th:insert](https://blog.csdn.net/wangmx1993328/article/details/84747497) | 将被引用的模板片段插⼊到自己的标签体中                 | <div th:insert="~{footer :: copy}"></div>                    |
| 9        | [th:replace](https://blog.csdn.net/wangmx1993328/article/details/84747497) | 将被引用的模板片段替换掉自己                           | <div th:replace="footer :: copy"></div>                      |
| 10       | [th:include](https://blog.csdn.net/wangmx1993328/article/details/84747497) | 类似于 th:insert，⽽不是插⼊⽚段，它只插⼊此⽚段的内容 | <div th:include="footer :: copy"></div>                      |
| 11       | [th:remove](https://blog.csdn.net/wangmx1993328/article/details/84747497) | 删除模板中的某些代码片段                               | <tr th:remove="all">...                                      |
| 12       | [th:each](https://blog.csdn.net/wangmx1993328/article/details/84702386) | 迭代数据，如 数组、List、Map 等                        | <tr th:each="user : ${userList}">...                         |
| 13       | [th:if](https://blog.csdn.net/wangmx1993328/article/details/84702386) | 条件为 true 时,显示模板⽚段，否则不显示                | <p th:if="${isMarry}">已婚1</p>                              |
| 14       | [th:unless](https://blog.csdn.net/wangmx1993328/article/details/84702386) | 条件为 false 时,显示模板⽚段，否则不显示               | <p th:unless="!${isMarry}">已婚2</p>                         |
| 15       | [th:switch ](https://blog.csdn.net/wangmx1993328/article/details/84702386) | 与 Java 中的 switch 语句等效，有条件地显示匹配的内容   | <div th:switch="1">                                          |
| 16       | [th:case](https://blog.csdn.net/wangmx1993328/article/details/84702386) | 配合 th:switch 使用                                    | <div th:switch="1">   <p th:case="0">管理员</p>   <p th:case="1">操作员</p>   <p th:case="*">未知用户</p> </div> |
| 17       | [th:with](https://blog.csdn.net/wangmx1993328/article/details/84766806) | 定义局部变量                                           | <div th:with="userFirst=${userList[0]}">                     |
| 18       | [th:inline](https://blog.csdn.net/wangmx1993328/article/details/84783202) | 禁用内联表达式，内联js,内联css                         | <script type="text/javascript" th:inline="javascript">       |

下面是 th 常用的属性，属性执行的优先级从 1~8，数字越低优先级越高

- th:text :设置当前元素的文本内容，相同功能的还有th:utext，两者的区别在于前者不会转义html标签，后者会。优先级不高:order=7
- th:value:设置当前元素的value值，类似修改指定属性的还有 th:src，th:href。优先级不高: order=6
- th:each:遍历循环元素，和th:text或th:value一起使用。注意该属性修饰的标签位置，详细往后看。优先级很高: order=2
- th:if:条件判断，类似的还有th:unless，th:switch，th:case。优先级较高: order=3
- th:insert:代码块引入，类似的还有th:replace，th:include，三者的区别较大，若使用不恰当会破坏html结构，常用于公共代码块提取的场景。优先级最高: order=1
- th:fragment:定义代码块，方便被th:insert引用。优先级最低: order=8
- th:object:声明变量，一般和*{一起配合使用，达到偷懒的效果。优先级一般: order=4o 
- th:attr:修改任意属性，实际开发中用的较少，因为有丰富的其他th属性帮忙，类似的还有th:attrappend，th:attrprepend。优先级一般: order=5



注意事项

1. 若要使用 Thymeleaf 语法，首先需要声明名称空间

   ```html
   <html lang="en" xmlns:th="http://www.thymeleaf.org" 
   				xmlns:sec="http://www.thymeleaf.org/extras/spring-security"
   				xmlns:shiro="http://www.pollix.at/thymeleaf/shiro">
   ```

2. 设置文本内容使用 `th:text`，设置 input 值使用 `th:value`，循环输出 `th:each`，条件判断 `th:if`，插入代码块 `th:insert`，定义代码块 `th:frament`，声明变量 `th:object`

3. 变量表达式中提供了很多的内置方法，该内置方法是使用 `#` 开头，不要与 `#{}` 消息表达式混淆



## 10.3 使用

1. 如果要支持 Thymeleaf ，需要加入 thymeleaf-starter

   ```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-thymeleaf</artifactId>
   </dependency>
   ```

2. 使用 Thymeleaf 编写的页面默认需要放在 /resources/templates



# 11 拦截器

## 11.1 基本介绍

1. 在 Spring Boot 项目中，拦截器是开发中常用手段，主要用来做登录验证、性能检查、日志记录等
2. 使用的基本步骤
   - 编写一个拦截器实现 HanlderInterceptor 接口
   - 拦截器注册到配置类中
   - 指定拦截规则

![image-20230813092822709](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230813092822709.png)

01 编写拦截器

```java
@Slf4j
public class LoginInterceptor implements HandlerInterceptor {
    /**
     * preHandle 在目标方法执行前辈调用
     */
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        // 1. 获取 uri
        String requestURI = request.getRequestURI();
        log.info("访问的 URI = {}", requestURI);

        // 2. 进行鉴权判断操作，return true -- 放行; return false -- 拦截
        return false;
    }

    /**
     * 在目标方法执行后被调用
     */
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        HandlerInterceptor.super.postHandle(request, response, handler, modelAndView);
    }

    /**
     * 在视图渲染完成之后被调用
     */
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        HandlerInterceptor.super.afterCompletion(request, response, handler, ex);
    }
}
```



02 编写配置类

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        // 注册自定义拦截器
        registry.addInterceptor(new LoginInterceptor()).addPathPatterns("/**").excludePathPatterns("/", "/login", "/images/**");   // addPathPatterns 指定拦截路径，excludePathPatterns 指定放行的路径
    }
}
```



# 11 文件上传

多文件上传：只需要在 input 标签中添加 `multiplie` 属性即可

无论是单文件上传还是多文件上传，我们需要在 form 表单上添加 `enctype="multipart/form-data"` 

```java
@RestController
@Slf4j
public class ParameterController {
    @PostMapping("/upload")
    public String upload(@RequestParam("avatar") MultipartFile avatar,
                         @RequestParam("photos") MultipartFile[] photos) throws IOException {
        log.info("上传的头像图片大小为 {}", avatar.getSize());
        log.info("上传的照片墙的照片个数为 {} 个", photos.length);
        // 得到类路径
        String path = ResourceUtils.getURL("classpath:").getPath();
        File file = new File(path + "static/images/upload/");
        if(!file.exists()) {
            file.mkdirs();
        }

        // 处理头像
        if(!avatar.isEmpty()) {
            // 得到图片的文件名
            String originalFilename = avatar.getOriginalFilename();
            avatar.transferTo(new File(file.getAbsolutePath() + "/" + originalFilename));
        }

        // 处理照片墙
        for (MultipartFile photo : photos) {
            if (!photo.isEmpty()) {
                String originalFilename = photo.getOriginalFilename();
                photo.transferTo(new File(file.getAbsolutePath() + "/" + originalFilename));
            }
        }
        return "";
    }
}
```

处理文件大小限制，直接在 application.yaml 中设置即可

```yaml
spring:
  servlet:
    multipart:
      max-file-size: 10MB # 单个文件限制10MB
      max-request-size: 50MB # 上传的总文件大小限制为50MB
```

解决文件覆盖问题

解决方案：使用 UUID + 系统当前的时间 + 文件名

```java
String fileName = UUID.randomUUID().toString() + "_" + System.currentTimeMillis() + "_" + name;
```

解决文件分目录 + 限制目录中文件个数

解决方案：根据年月日生成分级目录

```java
/*
 * 编写一个工具类，用来生成日期目录
 */
public class WebUtils {
    // 1. 定义一个文件上传的路径
    private final static String UPLOAD_FILE_DIRECTORY = "static/images/upload/";

    // 2. 编写方法，生成一个根据日期而定的目录
    public static String getUploadFileDirectory() {
        return UPLOAD_FILE_DIRECTORY + new SimpleDateFormat("yyyy-MM-dd").format(new Date());
    }
}
```

之后，修改原来的生成文件目录即可

```java
File file = new File(path + WebUtils.getUploadFileDirectory());
```

