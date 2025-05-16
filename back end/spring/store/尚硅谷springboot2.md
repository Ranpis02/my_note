[toc]

# 1 基础入门

## 1.1 SpringBoot 官方文档架构

我们如果想要阅读 SpringBoot 官方文档，那么首选得需要进入 spring 的[官网](https://spring.io/)，之后在 Projects 中的导航栏找到 SpringBoot ，找到 【Learn】 -> 【current】-> 【[Reference Doc](https://docs.spring.io/spring-boot/docs/current/reference/html/)】版本即可

<img src="https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230719092237413.png" alt="image-20230719092237413" style="zoom:50%;" />



## 1.2 基础入门 —— SpringBoot HelloWorld

**系统要求**

- Java 8
- Maven 3.3+

**Maven 配置文件**

```xml
<mirrors>
	<mirror>
		<id>nexus-aliyun</id>
		<mirrorOf>central</mirrorOf>
		<name>Nexus aliyun</name>
		<url>http://maven.aliyun.com/nexus/content/groups/public</url>
	</mirror>
</mirrors>

<profiles>
	<profile>
		<id>jdk-1.8</id>

		<activation>
			<activeByDefault>true</activeByDefault>
			<jdk>1.8</jdk>
		</activation>

		<properties>
			<maven.compiler.source>1.8</maven.compiler.source>
			<maven.compiler.target>1.8</maven.compiler.target>
			<maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
		</properties>
	</profile>
</profiles>

```



**在 pom.xml 中引入依赖**

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.3.4.RELEASE</version>
</parent>

<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```



**创建主程序类**

```java
/**
 * 主程序类
 * @author Administrator
 * @SpringBootApplication：表示这是一个 SpringBoot 应用
 */
@SpringBootApplication
public class MainApplication {
    public static void main(String[] args) {
        SpringApplication.run(MainApplication.class);
    }
}
```



**编写业务**

```java
/**
 * 业务代码编写，其中 @RestController 是作为 @ResponseBody 和 @Controller 的合体
 */
@RestController
public class HelloController {
    
    @RequestMapping("/hello")
    public String handle01() {
        return "Hello, Spring Boot 2";
    }
}
```

> `@Controller` 注解用于将类注入容器，`@ResponseBody` 注解用于转换返回值并自动将其写入 HTTP 响应



**测试**

我们直接在 postman 或者 浏览器中输入 `http://localhost:8080/hello` 直接发送请求即可，最后查看响应结果



:star:**添加配置**

在 springboot 项目中，我们如果想要添加配置选项，可以直接在 resource 中新建 application.properties 配置文件，然后写入相关的配置选项。目录结构如下：

```
main                                    
├─ java                                 
│  └─ com                               
│     └─ atguigu                        
│        └─ boot                        
│           ├─ controller               
│           │  └─ HelloController.java  
│           └─ MainApplication.java     
└─ resources                            
   └─ application.properties            
```

添加端口号配置

```properties
server.port=8888
```

> 更多具体的配置项，我们还可以参照官方文档的 Application Properties 章节



springboot 中专门提供给我们 maven 项目中打包部署的插件==spring-boot-maven-plugin==，我们直接在 pom.xml 中添加如下依赖即可

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <version>2.3.4.RELEASE</version>
        </plugin>
    </plugins>
</build>
```

>注意事项：这里官方文档没有指定版本号，我自己的 maven 版本是 3.6.3 ，如果不加版本号则无法引入，后面导入成功之后再删除则没有问题
>
>> 如果是 Gradle ，请参照[官方文档](https://docs.spring.io/spring-boot/docs/current/reference/html/getting-started.html#getting-started.first-application.executable-jar)



打包项目，我们在打包好了项目之后，可以直接在 target 文件夹下找到对应的 jar 包，打开终端，键入 `java -jar xxx.jar` 即可开启服务

> 注意细节：关闭 cmd 的==快速编辑== 模式，不过其实也不一定需要关



## 1.3 spring 的依赖管理特性

### 1.3.1 依赖管理

- 父项目做依赖管理

  ```xml
  <parent>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-parent</artifactId>
      <version>2.3.4.RELEASE</version>
  </parent>
  
  <!-- 该项目上面还有父项目 -->
  <parent>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-dependencies</artifactId>
      <version>2.3.4.RELEASE</version>
  </parent>
  <!-- 在该父项目中里面已经默认配置了许多版本控制管理，最典型的例如 mysql 驱动选用了 8.0.21，junit 选择的是 4.13 -->
  ```

  > 如果我们希望自定义依赖版本号，可以在当前项目中手动配置 `properties` 属性，在里面我们可以重写配置，例如我们希望使用的 mysql 5.1.43 版本的，那么我们可以进行如下配置
  >
  > ```xml
  > <properties>
  >     <mysql.version>5.1.43</mysql.version>
  > </properties>
  > ```

- 开发导入 starter 场景启动器

  1. spring-boot-starter-* 就是官方给我们提供的启动器

     > 其中 * 代表某种场景

  2. *-spring-boot-starter 就是第三方给我们提供的启动器 

- 使用 springboot，我们无需关注版本号，自动版本仲裁
  - 引入依赖默认可以不写版本号
  - 如果一旦引入非版本仲裁的 jar，我们则需要自己手动配置版本号

> IDEA 快键键
>
> - `Ctrl + Shift + Alt + U`：以图的形式显示项目中依赖之间的关系
> - `Alt + Ins`：创建新的类或者新包



### 1.3.2 自动配置

- 自动配置 Tomcat

  > 这个需要我们引入了 web 开发场景，里面自动引入 Tomcat

- 自动引入 SpringMVC

  > 同样是位于 web 开发场景中

- 自动配置好 Java web 的一些配置信息，例如字符编码拦截器 characterEncodingFilter 以及视图解析器 ViewResolver 和文件上传解析器 MultipartResolver

- 默认的包结构，主程序所在的包及其下面的所有子包都会被默认扫描进来

  > 如果我们希望扫描不是在主程序所在的包的外面，那么我们需要手动配置，在主程序的注解中添加扫描包：`@SpringBootApplication(scanBasePackages = "com.atguigu")`
  >
  > 或者配置 `@ComponentScan` 也可以指定扫描路径

  ```java
  @SpringBootConfiguration
  @EnableAutoConfiguration
  @ComponentScan
  // 等同于
  @SpringBootApplication
  ```

- 各种配置拥有默认值
  - 默认配置最终都是映射到 MultipartProperties
  - 配置文件的值最终会绑定每个类上，这个类会在容器中创建对象
- 按需加载所有自动配置项
  - springboot 所有自动配置功能都在 spring-boot-test-autoconfigure 中
  - 在该配置项中大部分都是飘红的，表示这些不生效，如果想要生效则需要手动在当前项目的 pom.xml 中引入





## 1.4 底层注解

### 1.4.1 @Configuration 配置类

```java
/**
 * 1. configuration 用于表示这是一个配置类
 * 2. 配置类里面使用 @Bean 标注在方法上面给容器注册组件，默认是单实例的
 * 3. 在配置类里面使用 @Bean 标注在方法上给容器注册组件，默认也是单例的
 * 4. proxyBeanMethods: 代理 bean 的方法
 *  Full(proxyBeanMethods = true) 重量级，每次在返回实例之前都会检查 IOC 容器中是否存在查找的 bean 对象，保证每个 @Bean 方法被调用多少次返回的组件都是单实例的
 *  Lite(proxyBeanMethods = false) 轻量级，每次会返回的 bean 对象都是新创建的
 * 最典型的应用场景便是组件依赖
 *
 */
@Configuration(proxyBeanMethods = false)
public class MyConfig {

    @Bean  // 给容器中添加组件。以方法名作为组件的id。返回类型就是组件类型。返回值，就是组件在容器中的实例
    public User user01() {
        return new User("zhangSan", 18);
    }

    @Bean("tomcat")	// 为该 bean 对象打标识
    public Pet tomcatPet()
    {
        return new Pet("tomcat");
    }
}
```



我们可以在 Application 类或者 test 中进行验证，具体思路是：首先得到 IOC 容器，之后通过类名或者类对象获取对应的 Bean 对象，再进行比较

```java
// 1. 返回 IOS 容器
ConfigurableApplicationContext run = SpringApplication.run(MainApplication.class, args);

// 2. 查看容器里面的组件
String[] names = run.getBeanDefinitionNames();

for(String name: names) {
    System.out.println(name);
}

// 3. 从容器中获取实例对象组件
User zhangSan = run.getBean("zhangSan", User.class);
Pet tomcat = run.getBean("tomcat", Pet.class);

// 4. 从容器中获取配置类组件
MyConfig bean = run.getBean(MyConfig.class);
```



### 1.4.2 @Import 导入组件

@Bean、@Component、@Controller、@Service、@Repository都是 Spring 表示基本标签，用于注入容器，在 SpringBoot 中并未改变他们原来的功能。我们这一章主要讲解 @Import 

我们在类前面加入对应的如下注解

```java
@Import({User.class, DBHelper.class})
@Configuration(proxyBeanMethods = false)
public class MyConfig {
}
```

那么 IOC 容器中就会自动创建出这两个类型的组件，默认组件的名字就是全类名



### 1.4.3 @Conditional 条件装配

如果条件满足 `@Conditional` 指定的条件，则进行组件注入

![image-20230719214516422](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230719214516422.png)

示例如下：我们首先在 config 配置类中加入组件依赖，如下：

```java
@Configuration(proxyBeanMethods = true)
public class MyConfig {

    @Bean  // 给容器中添加组件。以方法名作为组件的id。返回类型就是组件类型。返回值，就是组件在容器中的实例
    @ConditionalOnBean(name = "tomcat") // 如果 IOC 容器中存在 tomcat 组件，那么则将 user01 的返回值注入，否则不注入
    public User user01() {
        User zhangSan = new User("zhangSan", 18);
        // 注意：这里是由于由于要将 tomcat函数的的返回值（即 Pet 对象）装载到 zhangSan 这个 user 中，所以我们需要开启 Full 模式
        zhangSan.setPet(tomcatPet());
        return zhangSan;
    }

    @Bean
    public Pet tomcatPet()
    {
        return new Pet("tomcat");
    }
}
```

之后，我们来到 application 类中，进行如下首先获取容器，然后再从容器中取出验证组件，验证组件是否存在

```java
@SpringBootApplication(scanBasePackages = "com.atguigu")
public class MainApplication {
    public static void main(String[] args) {
        // 拿到 IOC 容器
        ConfigurableApplicationContext run = SpringApplication.run(MainApplication.class, args);

        boolean user01 = run.containsBean("user01");
        System.out.println("容器最终user01组件是否存在：" + user01);   // false
    }
}
```



### 1.4.4 @ImportResource 原生配置文件引入

在一些老的项目中仍然使用的使 xml 配置文件的方式来注入 IOC 容器，如果我们里面的 bean 对象配置较多，而我们又不想要将其转换为注解的配置方式，那么我们就可以使用 `@ImportResource` 来引入原生配置文件

```xml
<!-- resources/beans.xml --> 
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

        <bean id="haha" class="com.atguigu.boot.bean.User">
            <property name="name" value="zhangsan" />
            <property name="age" value="18" />
        </bean>
</beans>
```

之后，我们在 config 类中直接添加注释 

```java
@ImportResource("classpath:beans.xml")
```

即可注入成功



## 1.5 配置绑定

在以前，如果我们希望能够获得 properties 配置文件中的某些属性值，那么如果按照传统方法，那么具体操作如下：

```java
public class getProperties {
     public static void main(String[] args) throws FileNotFoundException, IOException {
         Properties pps = new Properties();
         pps.load(new FileInputStream("a.properties"));
         Enumeration enum1 = pps.propertyNames();//得到配置文件的名字
         while(enum1.hasMoreElements()) {
             String strKey = (String) enum1.nextElement();
             String strValue = pps.getProperty(strKey);
             System.out.println(strKey + "=" + strValue);
             //封装到JavaBean。
         }
     }
 }
```

而 SpringBoot 中提供了一种配置绑定

我们使用 `@ConfigurationProperties` 注解即可匹配对应的配置，例如，我们在 application.properties 配置文件中添加如下配置：

```properties
mycar.brand=BYD
mycar.price=1000000
```

然后，我们在 bean 文件夹下创建 Car 类

```java
@Component
@ConfigurationProperties(prefix = "mycar")
public class Car {
    private String brand;
    private Integer price;
    /**
     *	getter 
     *	setter
     * 	construcotr
     *  noconstructor
     * /
}
```

> 这里有几点需要我们注意的：
>
> 1. 我们在使用配置绑定的时候，需要添加 `@Component` 将其添加到 SpringBoot 的 IOC 容器中，否则配置项目无法生效
>
> 2. `@ConfigurationProperties`的==prefix==书写规范
>
>    1. prefix值应该用小写字母、数字、中划线“-”区分单词
>    2. 不能用大写字母、特殊字符，区分单词不能用下划线。
>
>    > 所以我们在配置文件中请不要使用 myCar 或者 my_car 这种形式，可以使用 my-car 或者直接 mycar
>
> 3. 我们在 bean 中无参构造器是一定要配置的 

最后，就让我们来耍一耍，在 controller 层中自动注入 Car，同时使用创建请求映射

```java
@RestController
public class HelloController {
    @Resource
    Car car;
        
    @RequestMapping("/car")
    public Car car() {
        return car;
    }
}
```

最后，我们直接在浏览器中键入 URL 访问，看看能不能得到返回结果

除了 `@ConfigurationProperties + @Component` 组合，我们还可以使用

`@EnableConfigurationProperties + @ConfigurationProperties` 的方式进行配置绑定

1. 开启Car配置绑定功能
2. 把这个Car这个组件自动注册到容器中

```java
@EnableConfigurationProperties(Car.class)
public class MyConfig {
...
}

@ConfigurationProperties(prefix = "mycar")
public class Car {
...
}
```



## 1.6 自动配置【源码分析】

SpringBoot 的启动类:

```java
@SpringBootApplication(scanBasePackages = "com.atguigu")
public class MainApplication {
    public static void main(String[] args) {
		SpringApplication.run(MainApplication.class, args);
    }
}
```

分析 `@SpringBootApplication` 

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
public @interface SpringBootApplication { }
```

这里最重要的是 `@SpringBootConfiguration`，`@EnableAutoConfiguration`，`@ComponentScan` 三个注解

### `@SpringBootConfiguration`

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Configuration
public @interface SpringBootConfiguration {
}
```

其中 `@Configuration` 用于声明配置类



### `@ComponentScan`

该注解也比较简单，表示用于扫描那些包



### `@EnableAutoConfiguration`

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@AutoConfigurationPackage
@Import({AutoConfigurationImportSelector.class})
public @interface EnableAutoConfiguration { }
```

其中 `@AutoConfigurationPackage` 注解的作用如下：

```java
public void registerBeanDefinitions(AnnotationMetadata metadata, BeanDefinitionRegistry registry) {
    AutoConfigurationPackages.register(registry, (String[])(new PackageImports(metadata)).getPackageNames().toArray(new String[0]));
}
```

1. 利用 Register 给容器中导入一系列组件
2. 在 MainApplication 同级的包下面所有的组件注入





### 最佳实践

- 引入场景依赖
  - [官方文档](https://docs.spring.io/spring-boot/docs/current/reference/html/using-spring-boot.html#using-boot-starter)
- 查看自动配置了哪些（选做）
  - 自己分析，引入场景对应的自动配置一般都生效了
  - 配置文件中debug=true开启自动配置报告。
    - Negative（不生效）
    - Positive（生效）
- 是否需要修改
  - 参照文档修改配置项
    - [官方文档](https://docs.spring.io/spring-boot/docs/current/reference/html/appendix-application-properties.html#common-application-properties)
    - 自己分析。xxxxProperties绑定了配置文件的哪些。
  - 自定义加入或者替换组件
    - @Bean、@Component…
  - 自定义器 XXXXXCustomizer；
  - …

> 我们可以通过配置 sping.banner.image.location=classpath:xxx 来指定启动时的 banner 样式

# 2 最佳实践

## 2.1 lombok 简化开发

Lombok用标签方式代替构造器、getter/setter、toString()等鸡肋代码。

spring boot已经管理Lombok。引入依赖：

```xml
 <dependency>
     <groupId>org.projectlombok</groupId>
     <artifactId>lombok</artifactId>
</dependency>
```



如果是老版本的 IDEA，那么我们需要手动在 settings -> plugins 中安装 lombok 插件，如果是新版本的 IDEA 中，那么 lombok 已经内置，我们不需要手动配置



**lombok 中常用注解**

- `@Data`：相当于 @Setter + Getter + @ToString + @EqualsAndHashCode
- `@Setter` 和 `@Getter`：自动生成 getter 和 setter 方法
- `@ToString`：生成 toString() 方法
- `@NoArgsConstructor`：生成无参构造器
- `@AllArgsConstructor`：生成全参构造器
- `@Slf4j`：日志管理工具，我们可以使用 log.info 替代 System.out.println，除此之外，还可以使用 `log.warn()`、`log.debug()`、`log.error()` 等等



## 2.2 Dev tools 

SpringBoot 官方给我们提供了一个热更新依赖，引入依赖如下：

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
        <optional>true</optional>
    </dependency>
</dependencies>
```

后续，如果我们修改了配置文件或者页面，那么我们可以直接 ==Build Project== 即可重新加载，或者在 IDEA 中默认快捷键为 Ctrl + F9

但是上面的依赖实质还是做的 ==restart== ，如果我们想要实现 ==reload==，那么需要使用付费的 JRebel



## 2.3 Spring Initailizr 

Spring Initailizr 是 IDEA 中创建Spring Boot工程向导。

在IDEA中，菜单栏New -> Project -> Spring Initailizr。



# 3 核心功能

## 3.1 配置文件

### 3.1.1 yaml 的用法

同以前的properties用法

YAML 是 “YAML Ain’t Markup Language”（YAML 不是一种标记语言）的递归缩写。在开发的这种语言时，YAML 的意思其实是：“Yet Another Markup Language”（仍是一种标记语言）。

**非常适合用来做以数据为中心的配置文件**。

#### 基本语法

- key: value；kv之间有空格
- 大小写敏感
- 使用缩进表示层级关系
- 缩进不允许使用tab，只允许空格
- 缩进的空格数不重要，只要相同层级的元素左对齐即可
- '#'表示注释
- 字符串无需加引号，如果要加，单引号’'、双引号""表示字符串内容会被 转义、不转义

#### 数据类型

- 字面量：单个的、不可再分的值。date、boolean、string、number、null

```yaml
k: v
```

- 对象：键值对的集合。map、hash、set、object

```yaml
#行内写法：  

k: {k1:v1,k2:v2,k3:v3}

#或

k: 
  k1: v1
  k2: v2
  k3: v3
# 注意：缩进和 py 一样，严格控制位两个空格，每个 k 后面需要添加一个空格
```

- 数组：一组按次序排列的值。array、list、queue

```yaml
#行内写法：  

k: [v1,v2,v3]

#或者

k:
 - v1
 - v2
 - v3
```

#### 实例

```java
@Data
public class Person {
    private String userName;
    private Boolean boss;
    private Date birth;
    private Integer age;
    private Pet pet;
    private String[] interests;
    private List<String> animal;
    private Map<String, Object> score;
    private Set<Double> salarys;
    private Map<String, List<Pet>> allPets;
}

@Data
public class Pet {
    private String name;
    private Double weight;
}
```

用yaml表示以上对象

```yaml
person:
  userName: zhangsan
  boss: false
  birth: 2019/12/12 20:12:33
  age: 18
  pet: 
    name: tomcat
    weight: 23.4
  interests: [篮球,游泳]
  animal: 
    - jerry
    - mario
  score:
    english: 
      first: 30
      second: 40
      third: 50
    math: [131,140,148]
    chinese: {first: 128,second: 136}
  salarys: [3999,4999.98,5999.99]
  allPets:
    sick:
      - {name: tom}
      - {name: jerry,weight: 47}
    health: [{name: mario,weight: 47}]
```

> 注意事项：这里需要特别注意的是 value 中的值，我们是可以添加单引号或者双引号的，但是单引号会转义，而双引号不会转义，例如对于 \n, 单引号会当做普通字符串输出，而双引号会将其作为换行输出



### 3.1.2 自定义类绑定的配置提示

自定义的类和配置文件绑定一般没有提示。若要提示，添加如下依赖：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>

<!-- 下面插件作用是工程打包时，不将spring-boot-configuration-processor打进包内，让其只在开发阶段有用 -->
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <configuration>
                <excludes>
                    <exclude>
                        <groupId>org.springframework.boot</groupId>
                        <artifactId>spring-boot-configuration-processor</artifactId>
                    </exclude>
                </excludes>
            </configuration>
        </plugin>
    </plugins>
</build>
```



## 3.2 web 开发

### 3.2.1 静态资源规则与定制化

**静态资源目录**，只要静态资源放在类路径下: called `/static` (or `/public` or `/resources` or `/META-INF/resources`

- 访问： 当前项目根路径/ + 静态资源名
- 原理： 静态映射/**

我们可以做一个简单的测试，我们添加一个 TestController，在里面加入 mapping 类，如下：

```java
@RestController
public class TestController {
    @RequestMapping("/bug.jpg")
    public String func() {
        return "xxx";
    }
}
```

与此同时，我们在静态资源下面放置一张 bug.jpg 图片，然后在浏览器中访问资源，观察到最终访问的是 controller 层管理的资源

访问步骤：<u>先去找Controller看能不能处理。不能处理的所有请求又都交给静态资源处理器。静态资源也找不到则响应404页面。</u>



**静态资源访问前缀**

默认情况下，我们直接通过==当前项目根路径/ + 静态资源名== 可以得到对应的静态资源，而如果我们希望添加访问前缀，可以在 ==resources/application.yaml== 里面添加如下配置即可

```yaml
spring:
  mvc:
    static-path-pattern: /res/**
```

这样，我们每次访问静态资源都需要加上 res 前缀



**更改默认的静态资源路径**

```yaml
# 默认的静态资源路径: "classpath:/META-INF/resources/", "classpath:/resources/", "classpath:/static/", "classpath:/public/"

spring:
  resources:
    static-locations: [classpath:/haha/]
```



**映射 webjars** 

> webjars 即是一些将 js、css 打包的 jar 包，不过使用的不是特别多，了解即可

添加如下依赖

```xml
<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>jquery</artifactId>
    <version>3.5.1</version>
</dependency>
```

刷新 maven 后，重启项目，访问 `http://localhost:8080/webjars/jquery/3.5.1/jquery.js` 即可

![image-20230720153514130](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230720153514130.png)



### 3.2.2 web 场景 —— welcome 与 favicon 功能

> [官方文档](https://docs.spring.io/spring-boot/docs/2.3.8.RELEASE/reference/htmlsingle/#boot-features-spring-mvc-welcome-page)

**欢迎页支持**

我们在静态资源存放目录下创建 index.html，那么我们可以直接通过访问 localhost:8080 来访问欢迎页

- 静态资源路径 index.html
  - 可以配置静态资源路径，其实就是仍然可以通过 localhost:8080 直接访问
  - 不可以配置静态资源的访问前缀，否则 index.html 不能被默认访问，即使通过 localhost:8080/presfix/ 进行访问也不行



**favicon.ico 页签图标支持**



## 3.3 源码分析

### 3.3.1 静态资源原理

- springboot 启动默认加载 xxxAutoConfiguration（自动配置类）
- springMVC 功能的自动配置类 WebMvcAutoConfiguration，该配置类位于 autoconfigure 下面的 web/servlet 里



### 3.3.2 请求参数处理

**请求映射**

- @xxxMapping;
  - @GetMapping
  - @PostMapping
  - @PutMapping
  - @DeleteMapping
- Rest风格支持（使用**HTTP**请求方式动词来表示对资源的操作）
  - 以前：
    - /getUser 获取用户
    - /deleteUser 删除用户
    - /editUser 修改用户
    - /saveUser保存用户
  - 现在： /user
    - GET-获取用户
    - DELETE-删除用户
    - PUT-修改用户
    - POST-保存用户
  - 核心Filter；HiddenHttpMethodFilter

对于 REST 的请求，我们需要分两种情况进行考虑：

- 如果是表单发送的请求，由于表单只能发送 GET 和 POST 请求，**而对于 PUT 和 DELETE 请求，会自动将转发为 GET请求**
- 而如果是使用的客户端工具，例如 postman, 那么我们可以直接 PUT、DELETE 请求

**原理探讨**

我们可以在 IDEA 中使用 Ctrl + Shift + N 全局搜索 WebMvcAutoConfiguration ，找到 HiddenHttpMethodFilter

我们截取其中的部分代码：

```java
private static final List<String> ALLOWED_METHODS;
public static final String DEFAULT_METHOD_PARAM = "_method";
private String methodParam = "_method";

public HiddenHttpMethodFilter() {
}

public void setMethodParam(String methodParam) {
    Assert.hasText(methodParam, "'methodParam' must not be empty");
    this.methodParam = methodParam;
}

protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain) throws ServletException, IOException {
HttpServletRequest requestToUse = request;
    if ("POST".equals(request.getMethod()) && request.getAttribute("javax.servlet.error.exception") == null) {
        String paramValue = request.getParameter(this.methodParam);
        if (StringUtils.hasLength(paramValue)) {
            String method = paramValue.toUpperCase(Locale.ENGLISH);
            if (ALLOWED_METHODS.contains(method)) {
                requestToUse = new HttpMethodRequestWrapper(request, method);
            }
        }
    }
}
```

如果我们希望在表单中使用 PUT 和 DELETE 请求，那么我们只需要添加一个隐藏属性，设置属性值 name 为 `_method`，并且表单的请求方式设置为 POST 请求，同时在 yaml 配置文件中

```html
<form action="/user" method="get">
    <input value="REST-GET提交" type="submit"/>
</form>

<form action="/user" method="post">
    <input value="REST-POST提交" type="submit"/>
</form>

<form action="/user" method="post">
    <!--设置真正想要发送的请求为 put-->
    <input name="_method" value="put" type="hidden">
    <input value="REST-PUT 提交" type="submit"/>
</form>

<form action="/user" method="post">
    <!--设置真正想要发送的请求为 delete-->
    <input name="_method" value="delete" type="hidden">
    <input value="REST-delete提交" type="submit"/>
</form>
```

```yaml
spring:
  mvc:
    hidden-method:
      filter:
        enabled: true
```

**Rest 原理**

- 表单提交会带上 `_method=PUT`

- 请求过来会被 HiddenHttpMethodFilter 拦截

  - 请求是否正常，并且是 POST 

    - 获取到 _method 的值
    - 兼容以下请求：PUT、DELETE、PATCH

    - 原生 request(post)，包装器 requestWrapper 重写了 getMethod 方法，返回的使传入的值，即 value 对应的真正的方法
    - 过滤器放行的时候用 wrapper 。以后的方法调用 getMethod 是调用 requestWrapper 的返回值

<u>如果我们使用客户端工具，例如 postman 发送请求，那么可以直接发送 PUT、DELETE 等方式发送请求，而无需 filter</u>

```java
@GetMapping("/user")
//@RequestMapping(value = "/user",method = RequestMethod.GET)
public String getUser(){
    return "GET-张三";
}

@PostMapping("/user")
//@RequestMapping(value = "/user",method = RequestMethod.POST)
public String saveUser(){
    return "POST-张三";
}

@PutMapping("/user")
//@RequestMapping(value = "/user",method = RequestMethod.PUT)
public String putUser(){
    return "PUT-张三";
}

@DeleteMapping("/user")
//@RequestMapping(value = "/user",method = RequestMethod.DELETE)
public String deleteUser(){
    return "DELETE-张三";
}
```

> 以上就是 REST 风格的请求映射，其中 `@RequestMapping` 就等同于  GET / POST



### 3.3.3 更改默认的 _method

在 `HiddenHttpMethodFilter` 类中，虽然 *DEFAULT_METHOD_PARAM* 是 final ，不能改变的，但是该类提供给我们 setMethodParam 方法去修改 methodParam

所以我们可以自定义 filter ，去重写 setter 方法，改变默认的 `_method`

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.filter.reactive.HiddenHttpMethodFilter;

/**
 * @author zhiHong Xie
 * @date 2023-07-20
 * @Description 这里千万要注意，不要导错了依赖
 */
@Configuration(proxyBeanMethods = false)
public class WebConfig{
    //自定义filter
    @Bean
    public HiddenHttpMethodFilter hiddenHttpMethodFilter(){
        HiddenHttpMethodFilter methodFilter = new HiddenHttpMethodFilter();
        methodFilter.setMethodParamName("_m");
        return methodFilter;
    }
}
```

同时需要在 yaml 配置文件中设置可重覆盖

```yaml
spring:
  main:
    allow-bean-definition-overriding: true
```

最终在 html 真的 form 表单中设置即可

```html
<form action="/user" method="post">
    <!--设置真正想要发送的请求为 put-->
    <input name="_method" value="put" type="hidden">
    <input name="_m" value="put" type="hidden">
    <input value="REST-PUT 提交" type="submit"/>
</form>
```



### 3.3.4 请求映射原理

太难了，理解不了。。。





## 3.4 请求处理——常用参数注解使用

- `@PathVaribale`：路径变量
- `@RequestHeader`： 获取请求头
- `@RequstParam`：获取请求参数
- `@CookieValue`：获取Cookie 的值
- `@RequestAttribute` 获取request域属性
- `@RequestBody` 获取请求体[POST]
- `@MatrixVariable` 矩阵变量
- `@ModelAttribute`

```java
@RestController
public class ParameterTestController {

    @GetMapping("/car/{id}/owner/{username}")
    public Map<String, Object> getCar(@PathVariable("id") Integer id,   // 获取路径变量 id 的值
                                      @PathVariable("username") String name, // 获取路径变量 username 的值
                                      @PathVariable Map<String, String> pv, // 获取路径变量上所有的值，并且以键值对的形式存储在pv中
                                      @RequestHeader("User-Agent") String userAgent,  // 获取 user-agent 的信息，是一长串的字符串
                                      @RequestHeader Map<String, String> header,    // 获取请求头header的信息
                                      @RequestParam("age") Integer age, // 获取请求参数 age 的值，
                                      @RequestParam("inters") List<String> inters,  // 获取请求参数 inters 的值，其中用 list存储，例如发送的请求为localhost:8080/car/3/owner/xzh?inters=game&inters=eat
                                      @CookieValue("_ga") String _ga,   // 直接拿到 _ga 这个 cookie 的值
                                      @CookieValue("_ga") Cookie cookie   // 拿到 _ga 这个 Cookie 对象
                                      ) {
        Map<String, Object> map = new HashMap<>();
        map.put("id", id);
        map.put("username", name);  //
        map.put("pv", pv);
        map.put("user-agent", userAgent);
        map.put("header", header);
        map.put("age", age);

        return map;
    }
}
```



**`@RequestBody` 的使用**

```java
@Controller
public class RequestController {
    @GetMapping("/goto")
    public String goToPage(HttpServletRequest request) {

        request.setAttribute("msg", "GOAT");
        request.setAttribute("code", 200);

        return "forward:/success";  // 转发到 /success 请求
        /**
         * forward 标识转发操作
         * redirect 标识重定向操作
         */
    }

    @ResponseBody
    @GetMapping("/success")
    public Map<String, Object> success(@RequestAttribute("msg") String msg,
                          @RequestAttribute("code") String code,
                          HttpServletRequest request) {
        Map<String, Object> map = new HashMap<String, Object>();
        // 直接从请求映射的注解中获取数据
        map.put("annotation_msg", msg);
        // 从 request 请求拿到参数信息
        Object msg1 = request.getAttribute("msg");
        map.put("reqMethod_msg", msg1);
        return map;
    }
}
```



**@MatrixVariable 的使用**

`@MatrixVariable` 有别于以往的 queryString 请求方式和 rest 风格请求。请求参数使用分号 `;` 分隔，第一个 `;` 前面的参数是路径中的参数，后面的都是携带的参数

```
queryString请求方式
/request?username=admin&password=123456&age=20

rest风格请求
/request/admin/123456/20

MatrixVariable矩阵变量
/request;username=admin;password=123456;age=20
```

在 Controller 层中进行测试：

```java
// /cars/sell;low=34;brand=byd,audi,yd
@ResponseBody
@GetMapping("/cars/sell")
public Map carsSell(@MatrixVariable("low") Integer low,
                    @MatrixVariable("brand") List<String> brand) {
    Map<String, Object> map = new HashMap<>();
    map.put("low", low);
    map.put("brand", brand);
    return map;
}
```

> :warning:**注意：**
>
> 1. springboot 默认是禁用了矩阵变量的功能，我们需要手动开启
> 2. 开启原理：将 UrlPathHelper 的 removeSemicolonContent 设置为 false
> 3. 矩阵变量必须要有 url 路径变量才能被解析

**开启方式1**

```java
@Controller
@Configuration(proxyBeanMethods = false)
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void configurePathMatch(PathMatchConfigurer configurer) {
        UrlPathHelper urlPathHelper = new UrlPathHelper();
        // 重新设置 urlPathHelper ，将其中的去除分号内容的配置关闭
        urlPathHelper.setRemoveSemicolonContent(false);
        configurer.setUrlPathHelper(urlPathHelper);
    }
}
```

**请求访问配置**

```java
// /cars/sell;low=34;brand=byd,audi,yd
@ResponseBody
@GetMapping("/cars/{path}") // 注意：这里的路径参数要使用 path 变量替代，而不能直接写死
public Map carsSell(@MatrixVariable("low") Integer low,
                    @MatrixVariable("brand") List<String> brand,
                    @PathVariable("path") String path) {
    Map<String, Object> map = new HashMap<>();
    map.put("path", path);
    map.put("low", low);
    map.put("brand", brand);
    return map;
}
```

最终返回结果为：

```json
{
    "path": "sell",
    "low": 34,
    "brand": [
        "byd",
        "audi",
        "yd"
    ]
}
```



**开启方式2：创建返回 `WebMvcConfigurer` Bean**

```java
@Configuration(proxyBeanMethods = false)
public class WebConfig{
    @Bean
    public WebMvcConfigurer webMvcConfigurer(){
        return new WebMvcConfigurer() {
                        @Override
            public void configurePathMatch(PathMatchConfigurer configurer) {
                UrlPathHelper urlPathHelper = new UrlPathHelper();
                // 不移除；后面的内容。矩阵变量功能就可以生效
                urlPathHelper.setRemoveSemicolonContent(false);
                configurer.setUrlPathHelper(urlPathHelper);
            }
        }
    }
}
```



其他配置：当出现路径变量冲突的请求，可以使用 `pathVar` 单独指定使用哪一个路径参数下的矩阵变量

```java
// /cars/1;age=18;name=zs/2;age=19;name=ls
@ResponseBody
@GetMapping("/cars/{bossId}/{empId}")
public Map boss(@MatrixVariable(value = "age", pathVar="bossId") Integer bossAge,
                @MatrixVariable(value = "age", pathVar="empId") Integer empAge) {
    HashMap<String, Object> mp = new HashMap<>();
    mp.put("bossAge", bossAge);
    mp.put("empAge", empAge);
    return mp;
}
```



==源码解析==



## 3.5 servlet API



4 拦截器
