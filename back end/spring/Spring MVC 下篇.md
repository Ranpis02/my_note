[toc]

# 1 数据格式化

## 1.1 基本数据类型的相互转换

提出问题：我们在提交数据时，Spring MVC 是如何对提交的数据进行转换和处理？

搭建环境进行演示

01 web.xml 中配置分发器

```xml
<!--配置中央控制器/前端控制器/分发控制器(用户的请求都会经过它的处理)-->
<servlet>
    <servlet-name>springDispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <!-- 配置属性 contextConfigLocation, 指定 DispatcherServlet 操作的 spring 配置文件 -->
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:springDispatcherServlet-servlet.xml</param-value>
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
```



02 spring 的配置文件中添加默认的视图解析器

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">
    <!--配置自动扫描的包-->
    <context:component-scan base-package="com.xzh.web" />

    <!--配置视图解析器-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!--配置 suffix 和 prefix-->
        <property name="prefix"  value="/WEB-INF/pages/" />
        <property name="suffix" value=".jsp" />
        <property name="order" value="10" />
    </bean>

    <!--加入两个常规配置，对应的地址为 http://www.springframework.org/schema/mvc -->
    <!--支持springmvc的高级功能，比如JSR303校验，映射动态请求-->
    <mvc:annotation-driven />
    <!--将 springmvc 不能处理的请求，交给 tomcat 处理，比如 css、js-->
    <mvc:default-servlet-handler />
</beans>
```



03 创建 Monster entity

```java
@NoArgsConstructor
@AllArgsConstructor
@Data
public class Monster {
    /**
     * 怪兽id
     */
    private Integer id;
    /**
     * 怪兽名
     */
    private String name;
    /**
     * 怪兽年龄
     */
    private Integer age;
    /**
     * 怪兽邮箱？
     */
    private String email;
}
```



04 handler 处理器

```java
@Controller
@RequestMapping("/monster")
public class MonsterHandler {
    /**
     * 初始化
     */
    @RequestMapping("/loadMonsterUI")
    public String loadMonsterUI(Map<String, Object> mp) {
        mp.put("monster", new Monster());
        return "monster_add";
    }

    /**
     * 添加怪兽操作
     */
    @RequestMapping("/save")
    public String save(Monster monster) {
        System.out.println(monster);
        return "success";
    }

}
```



05 Jsp 页面

```jsp
<%--当前jsp页面路径：/web/WEB-INF/pages/monster_add.jsp-->
<%--引入 springframework 中的 form 标签库--%>
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>monster 回显信息</title>
  </head>
  <body>
    <%--modelAttribute取的是模型数据（在这里为 request 域）中的 map 容器中的数据monster为key--%>
    <form:form action="save" method="post" modelAttribute="monster">
      妖怪姓名：<form:input path="name" /><br>
      妖怪年龄：<form:input path="age" /><br>
      电子邮件：<form:input path="email" /><br>
      <input type="submit" value="添加妖怪">
    </form:form>
  </body>
</html>
```



实验验证：当我们在浏览器中键入 `ip:port/web_app/monster/loadMonsterUI` 后进入添加妖怪表单页面，输入对应的参数后可以发现 IDEA 控制会打印输入对应语句

观察打印输出的信息，我们会发现

1. 在 Spring MVC 环境下，如果 age 输入的数字则通过，并且自动会将其转化为 Integer/int；同样的道理，对于 name 字段，如果输入的是字符串则会将其转化为 String
2. 如果输入的数据类型不匹配，那么就会报 400 的错误

结论：Spring MVC 底层会帮助我们将基本数据类型和字符串自动转换，但是要求数据匹配，如果不匹配则会抛出 400 的错误



## 1.2 特殊类型和字符串的相互转换

1. 特殊类型和字符之间的转使用注解，例如日期、规定格式的小数，比如货币形式

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Person {
    @DateTimeFormat(pattern = "yyyy-MM-dd")
    private Date birthday;
    @NumberFormat(pattern = "###,###.##")
    private float salary;
}
```

> `@DateTimeFormat` 和 `@NumberFormat` 用于告诉 spring MVC 在封装参数时按指定格式进行转换，如果没有按照注解指定的格式进行封装数据则会给出 400 的页面



# 2 验证以及国际化

## 2.1 表单数据验证

概述

1. 对于输入的数据（比如表单数据），进行必要的验证，并给出相应的提示信息
2. 对于验证表单数据，Spring MVC 同提供了很多实用的注解，这些注解由 JSR 303 验证框架提供



JSR 303 验证框架

1. JSR 303 是 Java 为 Bean 数据合法性校验提供的标准框架，它已经在 JavaEE 中

2. JSR 303 通过在 Bean 属性上标注类似于 `@NotNull`、`@Max` 等标准的注解指定校验规则，并通过标准的验证接口对 Bean 进行验证

3. JSR 303 提供的基本验证注解包括：

   ![image-20230801184956308](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230801184956308.png)

4. Hibernate Validator 扩展注解。Hibernate Validator 是 JSR 303 的一个参考实现，除支持所有标准的校验注解外，它还支 持以下的扩展注解

   ![image-20230801185312293](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230801185312293.png)



实战演示

01 引入验证和国际化相关的 jar 包

```xml
<dependency>
    <groupId>com.fasterxml</groupId>
    <artifactId>classmate</artifactId>
    <version>0.8.0</version>
</dependency>

<dependency>
    <groupId>org.hibernate.validator</groupId>
    <artifactId>hibernate-validator</artifactId>
    <version>6.0.0.Final</version>
</dependency>

<dependency>
    <groupId>org.hibernate.validator</groupId>
    <artifactId>hibernate-validator-annotation-processor</artifactId>
    <version>6.0.0.Final</version>
</dependency>

<!-- hibernate 特定的日志门面 -->
<dependency>
    <groupId>org.jboss.logging</groupId>
    <artifactId>jboss-logging</artifactId>
    <version>3.1.1.GA</version>
</dependency>

<dependency>
    <groupId>javax.validation</groupId>
    <artifactId>validation-api</artifactId>
    <version>1.1.0.Final</version>
</dependency>
```



02 使用

```java
/**
 * @NotEmpty 该注解用于标识属性不能为空
 */
@NotEmpty
private Integer id;
/**
 * @Range 用于限制取值返回，如下就限制age的取值范围为[1, 100]
 */
@Range(min = 1, max = 100)
private Integer age;
```



实战演示

01 限制 entity 的取值

```java
@NoArgsConstructor
@AllArgsConstructor
@Data
public class Monster {
    /**
     * 怪兽id
     */
    private Integer id;
    /**
     * 怪兽名
     */
    /**
     * 非空验证的注解信息如下：支持类型包括字符集（串）、集合类型、map 类型和数组类型
     *The annotated element must not be {@code null} nor empty. Supported types are:
     * {@code CharSequence} (length of character sequence is evaluated)</li>
     * {@code Collection} (collection size is evaluated)</li>
     * {@code Map} (map size is evaluated)</li>
     * Array (array length is evaluated)</li>
     */
    @NotEmpty
    private String name;
    /**
     * 对于 @Range 注解，如果我们不指定 min 和 max ，其默认值分别为 0 和 Long.MAX_VALUE
     */
    @Range(min = 1, max = 100)
    private Integer age;
    /**
     * 怪兽邮箱？
     */
    private String email;
}
```



02 在 handler 中作相应的处理

```java
@Controller
@RequestMapping("/monster")
public class MonsterHandler {
    /**
     * @Valid 用于对 entity 中指定的注解进行验证，例如@NotEmpty用于验证怪兽名非空，@Range(min = 1, max = 100)用于限制怪兽的年龄在 1-100  之间
     * Error 用于存储验证过程中出现的错误信息
     * Map<String, Object> map 表示如果校验出现错误，将校验的错误信息保存到 map 中同时保存 monster 对象
     * 校验发生的时机：在 springmvc 底层，反射调用目标方法时，会接收到 Http 请求的数据，然后根据注解进行验证，在验证的过程中，如果出现了错误，就把错误信息填充到  errors 和 map
     */
    @RequestMapping("/save")
    public String save(@Valid Monster monster, Errors errors, Map<String, Object> map) {
        // 为了更加直观地看到，我们看以将信息输出打印出来

        System.out.println("map 中信息如下：--------------------");
        for(Map.Entry<String, Object> m : map.entrySet()) {
            System.out.println("key = " + m.getKey() + ", value = " + m.getValue());
        }
        System.out.println("---------------------------");

        System.out.println("错误信息如下：-----------------------");
        List<ObjectError> allErrors = errors.getAllErrors();
        for(ObjectError allError : allErrors) {
            System.out.println("error = " + allError);
        }
        System.out.println("------------------------------");
        return "success";
    }
}
```

当我们传入的 age 不在 [1, 100] 范围内或者 name 传入的值为空时，会被收集到 errors 中

> 补充：区分 `@NotEmpty`、`@NotBlank`、`@NotNull`
>
> - `@NotEmpty`：适用于 String、Collection、Map、Array，要求不能为 null 或者长度不能为 0。一般用于集合类或者数组上
> - `@NotBlank`：只能作用在 String 类型，不能为 null 且需要有实际字符
> - `@NotNull`：不能为 null，但是可以为 empty，也就是说可以wield空字符，一般用于 Integer 类型，搭配 `@size`、`@Max`、`@Min` 对字段的值进行大小的控制



如果我们希望能够将错误信息回显，那么我们可以考虑使用 spring framework 自带的 Jsp tag 标签 `form:errors` 来进行接收

```jsp
<form:form action="save" method="post" modelAttribute="monster">
  妖怪姓名：<form:input path="name" /> <form:errors path="name" /><br>
  妖怪年龄：<form:input path="age" /> <form:errors path="age" /><br>
  电子邮件：<form:input path="email" /> <form:errors path="email" />< br>
  <input type="submit" value="添加妖怪">
</form:form>
```

但是回显的错误信息非常多，那么我们就需要考虑如何自定义错误信息？

 首先，我们需要在 spring 的配置文件中专门配置国际化信息处理的 bean

```xml
<!--在 spring 的配置文件中配置国际化错误信息的资源处理bean-->
<bean id="messageSource" class="org.springframework.context.support.ResourceBundleMessageSource">
    <!--
        配置国际化文件名字，如果是普通的 java 项目，那么下面的路径就是 src/i18.properties,
        如果是 maven 项目，那么就是位于 resources 下
    -->
    <property name="basename" value="i18n" />
</bean>
```

接下来，我们配置 i18n.properties，如下：

```properties
# 年龄要求在 1-100 之间
Range.monster.age=\u5e74\u9f84\u8981\u6c42\u57281-100\u4e4b\u95f4
# 怪兽名字不能为空
NotBlank.monster.name=\u602a\u517d\u540d\u5b57\u4e0d\u80fd\u4e3a\u7a7a
```

> key 为 验证规则.modelAttribute值.属性名，我们也可以在 idea 开发工具的控制台看到专门的打印信息：
>
> ![image-20230801232027980](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230801232027980.png)
>
> value 的值为其他字符转成的 unicode 编码

最终，我们使用如下 Jsp 代码作为回显页面

```jsp
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>Title</title>
  </head>
  <body>
    <form:form action="save" method="post" modelAttribute="monster">
      妖怪姓名：<form:input path="name" /> <form:errors path="name" /><br>
      妖怪年龄：<form:input path="age" /> <form:errors path="age" /><br>
      电子邮件：<form:input path="email" /> <form:errors path="email" /><br>
      <input type="submit" value="添加妖怪">
    </form:form>
  </body>
</html>
```

可以看到错误的提示信息此时已经变成了我们在 properties 中的配置信息

<img src="https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230801233520112.png" alt="image-20230801233520112" style="zoom: 67%;" />

:warning: **细节和注意事项**

1. 如果我们要对传入的参数进行验证，那么需要在 Java bean 的属性上添加相应的注解，在目标方法参数字段上添加 `@Valid` 或 `@Validated ` 注解

   > `@Validated` 这个注解好像有毒，使用完一次后 entity 就会找不到

2. 如果我们想要获取到错误列表信息，那么我们可以在 handler 的方法的参数列表中添加 `Errors` 或 `BindingResult` 类型的参数，可以获取到验证的错误信息

3. 回显错误信息需要使用 `<form:errors path="email"><form:errors>` 标签来显示错误消息，这标签需要卸载 `<form::form>` 标签内才会生效

4. 错误消息的国际化文件 `i18n.properties`，中文需要的是 Unicode 编码，需要使用工具进行转码

   - 格式：验证规则.表单 modelAttribute值.属性名 = 消息信息



## 2.2 注解的结合使用

有时候，单个注解并没有很好的满足我们的业务，例如对于表单的 age 字段，我们希望其不为空，并且范围在 0-120 之间，那么我们就可以考虑使用 `@NotNull` 和 `@Range` 搭配使用

如下：

```java
@NoArgsConstructor
@AllArgsConstructor
@Data
public class Monster {
    @NotEmpty(message = "姓名不能为空")
    private String name;

    @NotNull(message = "年龄不能为空")
    @Range(min = 1, max = 100)
    private Integer age;
    
    @NotNull(message = "生日不能为空")
    @DateTimeFormat(pattern = "yyyy-MM-dd")
    private String birthday;
}
```

> 注意：`@NotEmpty` 不能用来修改 Integer/int 类型，所以这里我们需要使用 `@NotNull`



## 2.3 数据类型转换校验核心类 - DataBinder

Spring MVC 通过反射机制对目标方法进行解析，将请求消息绑定到处理方法的入参中，数据绑定的核心部件是 DataBinder

![image-20230802091130927](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230802091130927.png)

1. ServletReuqest 参数传入到目标方法

2. DataBinder 对目标方法中的入参对象进行封装

3. DataBinder 内部的 ConversionService 对象对数据进行转换和格式化

4. 数据校验

   > 注意：并不是所有的验证注解都会经过这一步，例如 `@DateTimeFormat` 和 `@NumberFormat` 就会直接跳过这一步，从 ConversionService 处理 => 放入 BindingResult

5. 最后将校验所得到的错误信息放入到 `BindingResult` 中

   > 其实 `Errors` 接口就是 `BindingResult` 的实现类 



## 2.4 取消属性绑定

在默认情况下，表单提交的数据都会和 JavaBean 中属性绑定，如果在开发过程中，希望取消某个属性的绑定，换言之，就是不希望接收到某个表单对应的属性的值，则可以通过 `@InitBinder` 注解取消绑定

```java
@Controller
@RequestMapping("/monster")
public class MonsterHandler {
    // 取消属性绑定 monster 的 name 属性绑定
    @InitBinder
    public void initBinder(WebDataBinder webDataBinder) {
        /**
         * 1. 方法上需要标注 @InitBinder，Spring MVC 底层会初始化 WebDataBinder
         * 2. 调用 webDataBinder.setDisallowedFields 方法表示取消指定属性的绑定，即表单提交字段为 name 时
         * 就不会把接收到的 name 值，填充到 model 数据 monster 的 name 属性中
         * 3. setDisallowedFields 支持传入可变参数，取消多个属性的绑定
         * 4. 如果我们取消了数据的绑定，那么我们同时也需要取消数据的验证，因此此时数据的验证也没有意义
         */
        webDataBinder.setDisallowedFields("name");
    }
}
```



## 2.5 中文乱码处理

不知道为什么，这个中文乱码的问题我没有复现出来

这里如果以后出现这个问题再回过头来看看

