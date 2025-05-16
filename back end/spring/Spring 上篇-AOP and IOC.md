[toc]

# 1 spring 基础

## 1.1 spring 基本介绍

spring 是一个轻量级容器框架。

> spring 其实是一个 "家族"，目前我们学习的主要是 spring framework



## 1.2 spring 的下载安装

进入 spring 的[官网](https://spring.io/)，选择 Spring Framework，点击右上角的 "小猫" 图标进入到 spring 的 github

![image-20230328121059606](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230328121059606.png)

进入后，下拉找到 【Access to Binaries】

![image-20230328121421139](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230328121421139.png)

![image-20230328121514431](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230328121514431.png)

找到 【libs-release】 → 【org】→ 【springframework】→ 【spring】，找到对应的链接：

![image-20230328122138122](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230328122138122.png)

![image-20230328123215739](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230328123215739.png)

我们将其下载完成后解压缩，得到一组 jar 包（其实所谓 "框架"就是提供了一堆 jar 包（或者称为 "规范"）的供我们使用）



## 1.3 spring 文档

在线文档：https://docs.spring.io/spring-framework/docs/current/reference/html/

离线文档：位于我们下载好的 spring 压缩包中的 javadoc-api 中

离线 API：位于我们下载好的 spring 压缩包中的 kdoc-api 中



## 1.4 核心内容

![image-20230328142353303](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230328142353303.png)

- IOC：**控制反转**，可以管理 Java 对象
- AOP：**切面编程**
- JDBCTemplate：是 spring 提供的一套访问数据库的技术
- 声明式事务：基于  IOC / AOP 实现事务管理

:herb:**几个重要概念**

1. spring 可以整合其他的框架

   > spring 是管理框架的框架

2. spring 有两个核心的概念

   - IOC：Inversion of Control 反转控制
   - AOP：切面编程

3. IOC 的开发模式：

   - spring 根据 <u>配置文件 xml / 注解</u> 创建对象，并将其放入到容器（concurrentHashMap）中，并且可以完成对象之间的依赖
   - 当需要使用某个对象实例时，就直接从容器中获取即可

   <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230328151121084.png" alt="image-20230328151121084" style="zoom: 50%;" />

4. DI —— Dependency Injection 依赖注入，可以理解为是 IOC 的另一种称呼

   - 即通过配置，给程序提供需要使用的 Web 层，例如：Servlet（Action / Controller）、Service、Dao（JavaBean / entity）
   - 依赖注入是 spring 的核心价值所在，也是  IOC 的具体表现，它实现了解耦



## 1.5 spring 快速入门

需求说明：通过 spring 的方式获取 JavaBean（例如 Monster 对象），并给该 对象属性赋值，输出该对象信息

创建一个普通 Java 项目

```
├─.idea
├─lib
└─src
```

 引入 spring5 的核心 jar 包

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230328153253964.png" alt="image-20230328153253964" style="zoom: 67%;" />

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230328154251879.png" alt="image-20230328154251879" style="zoom:50%;" />



演示代码：

```
src                              
├─ com                           
│  └─ spring                     
│     ├─ bean                    
│     │  └─ Monster.java         
│     └─ test                    
│        └─ SpringBeanTest.java  
├─ beans.xml                     
└─ Main.java                     
```

```java
// Monster.java
package com.spring.bean;

public class Monster {
    private Integer id;
    private String name;
    private String skill;

    // 一定需要提供无参构造器，spring 进行反射时需要使用
    // getter 、setter、全参、无参
}
```

```xml
// beans.xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
        <!--
            1. 配置 monster 对象
            2. 在 beans 中可以配置多个 bean
            3. bean 表示就是一个 java 对象
            4. class 属性用来指定类的全路径
            5. id 属性表示java对象在spring容器中的id
        -->
    <bean class="com.spring.bean.Monster" id="monster01">
        <property name="id" value="100"></property>
        <property name="name" value="牛魔王"></property>
        <property name="skill" value="芭蕉扇"></property>
    </bean>

</beans>
```

```java
//SpringBeanTest.java
// 创建容器 ApplicationContext
// 该容器和容器配置文件相关联
ApplicationContext ioc = new ClassPathXmlApplicationContext("beans.xml");
// 3. 通过getBean获取对应的对象
// 默认返回的是 Object，但是运行类型为 Monster
Object o = ioc.getBean("monster01");
// 输入得到对象的运行类型
System.out.println(o.getClass());
// 得到对象的属性
System.out.println(((Monster) o).getId());
System.out.println(((Monster) o).getName());
System.out.println(o);

// 得到 bean 的第二种方式：getBean(String s, Class<T> aClass)，该方法返回的数据类型为泛型T
Monster monster01 = ioc.getBean("monster01", Monster.class);
```



## 1.6 类加载路径

IOC 容器的获取是 spring 获取 javaBean 的关键，即下面代码：

```java
ApplicationContext ioc = new ClassPathXmlApplicationContext("beans.xml");
```

我们需要探讨的是：<u>spring 是如何读取到 beans.xml 的？</u>

说明：我们之前将 beans.xml 放在  src 目录下，类加载也是从该目录开始，当开始加载 beans.xml 后，会在 out 目录下将其编译后的文件存放在指定目录下，我们可以通过如下代码得到执行时使用的资源目录：

```java
File path = new File(this.getClass().getResource("/").getPath());
System.out.println(path);
// 在本机上中的输出结果为：
// C:\Users\Administrator\Desktop\spring5\out\production\spring5
```

out  目录的层级结构：

```
out                                     
└─ production                           
   └─ spring5                           
      ├─ com                            
      │  └─ spring                      
      │     ├─ bean                     
      │     │  └─ Monster.class         
      │     └─ test                     
      │        └─ SpringBeanTest.class  
      ├─ beans.xml                      
      └─ Main.class                                 
```



## 1.7 通过 Debugger 查看容器的内部结构

**配置 Debugger** 

![image-20230328184904476](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230328184904476.png)

![image-20230328185143341](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230328185143341.png)



我们对上面获取容器的代码进行下断点，查看其 IOC 容器的数据结构：

找到 beanFactory → <strong style="color:red">beanDefinitionMap</strong>，该对象中存放的就是 beans.xml 中对应的 bean 对象的结构

![image-20230328191248319](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230328191248319.png)

所使用的数据结构为 concurrentHashMap，其中 table （存放的数据类型为ConcurrentHashMap$Node 类型，结构为多态数组）里面的数据更是关键，包括了 lazyInit（懒加载字段，即是运行时才创建还是编译时就创建），propertyValues 存储 bean 对象相关信息

> table 的初始大小为 512

![image-20230328192414434](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230328192414434.png)

另外，还有一个 <strong style="color:red">singletonObjects</strong> 字段，类型为 ConcurrentHashMap，其中的 table 存放的数据类型也是 ConcurrentHashMap$Node，如果在 beans.xml 文件中配置的对象是单例的就会放在其中

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230328194032066.png" alt="image-20230328194032066" style="zoom:50%;" />

在 beanFactory 中，还有一个 <strong style="color:red">beanDefinitionNames</strong> 用来存放 bean 的 id，例如上面的 "monster01"，目的当然是为了加快 bean 的检索速度

![image-20230328195154677](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230328195154677.png)



## 1.8 实现简单的基于 xml 的配置程序

**思路图解**

![image-20230328195932716](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230328195932716.png)

**实现代码**

```java
public class customApplicationContext {
    private ConcurrentHashMap<String, Object> singletonObjects = new ConcurrentHashMap<>();

    // 构造器
    // 接收一个配置文件，例如 beans.xml(该文件默认应该位于src)

    public customApplicationContext(String iocBeanXmlFile) throws Exception {
        // 1. 得到类加载路径
        String path = this.getClass().getResource("/").getPath();

        // 2. 创建saxReader
        SAXReader saxReader = new SAXReader();

        // 3. 加载文档
        Document document = saxReader.read(new File(path + iocBeanXmlFile));    // 这里会自动拼接（不是很明白）

        // 4. 得到  rootElement
        Element rootElement = document.getRootElement();

        // 5. 得到第一个bean —— monster01
        Element bean = (Element)rootElement.elements("bean").get(0);

        // 6. 得到第一个bean的相关属性
        String id = bean.attributeValue("id");          // 得到id属性值
        String aClass = bean.attributeValue("class");   // 得到class属性值
        List<Element> properties = bean.elements("property");
        Integer monsterId = Integer.parseInt(properties.get(0).attributeValue("value"));
        String name = properties.get(1).attributeValue("value");
        String skill = properties.get(2).attributeValue("value");

        // 7. 使用反射创建对象
        Class<?> rejection = Class.forName(aClass);
        Monster o = (Monster)rejection.newInstance(); // 这里得到的对象即为Monster对象

        /**
         * 原本的流程为：
         * Field id_1 = rejection.getField("id");
         * id_1.set(o, monsterId);
         * 这里我们简化一下
         */

        o.setId(1);
        o.setName("牛魔王");
        o.setSkill("芭蕉扇");

        // 8. 将创建好的对象放入到 singletonObjects 中
        singletonObjects.put(id, o);
    }

    // 向外提供一个获取对象的方法
    public Object getBean(String id) {
        return singletonObjects.get(id);
    }
}
```

**测试代码**

```java
customApplicationContext ac = new customApplicationContext("beans.xml");
Monster monster01 = (Monster)ac.getBean("monster01");
System.out.println(monster01.getName());
```



## 2 Bean — IOC

## 2.1 spring 配置 / 管理 bean

Bean 管理主要包括两方面

- 创建 bean 对象
- 给 bean 注入属性

Bean 配置方式也有两种

- 基于 XML 文件配置方式
- 基于注解方式



## 2.2 通过类型获取 Bean

我们前面已经通过 id 来获取过 Bean ，这里就不做过多介绍，接下来我们来介绍如何直接通过类型来获取 Bean

> 通过 id 获取的 Bean 的类型默认是 Object，我们也可以将 id 与 `.class` 搭配使用，返回的对象就直接是该类型

准备工作：事先在 beans.xml 中准备好 Bean 的配置文件

```xml
<bean class="com.spring.bean.Monster">
    <property name="id" value="100"></property>
    <property name="name" value="牛魔王"></property>
    <property name="skill" value="芭蕉扇"></property>
</bean>
```

**执行程序**

```java
ClassPathXmlApplicationContext ioc = new ClassPathXmlApplicationContext("beans.xml");
Monster m = ioc.getBean(Monster.class); // 直接通过类对象得到Bean
System.out.println(m);
```

:small_red_triangle_down:**注意事项**

1. 按类型来获取 Bean，要求 IOC 容器中的同一个类的 Bean 只能有一个，否则会抛出异常
2. 这种方式的应用场景：<strong style="color:red">在一个线程中只需要一个对象实例的情况</strong>
3. 说明：<u>在容器的配置文件中给属性赋值时，底层是通过 setter 方法完成的</u>，这也是我们需要在 Bean 中提供 setter 的原因



## 2.3 配置 Bean

### 2.3.1 通过指定构造器配置 Bean

在之前，我们提过，在 Bean 中，必须要配置一个无参构造器，因为 getBean 得到的 Bean，默认调用的就是无参构造器，我们也可以指定构造器配置 Bean，主要是通过 **`constructor-arg`** 参数

**Monster 中的全参构造器**

```java
public Monster(Integer id, String name, String skill) {
    this.id = id;
    this.name = name;
    this.skill = skill;
}
```

**示例代码**

```xml
<!--
   通过constructor-arg标签可以指定使用构造器的参数
   index 对应构造器的第几个参数，从0开始
   除了使用index，我们还可以使用 name/type进行指定
-->
 <!--1. 通过 index 指定构造器中的对应参数的索引号-->
<bean class="com.spring.bean.Monster" id="monster_01">
    <constructor-arg value="001" index="0"></constructor-arg>
    <constructor-arg value="白骨精" index="1"></constructor-arg>
    <constructor-arg value="吸人血" index="2"></constructor-arg>

</bean>
<!--2. 通过name直接指定构造器中参数名-->
<bean class="com.spring.bean.Monster" id="monster_02">
    <constructor-arg value="002" name="id"></constructor-arg>
    <constructor-arg value="黑山老妖" name="name"></constructor-arg>
    <constructor-arg value="吸星大法" name="skill"></constructor-arg>
</bean>
<!--3. 通过type对应构造器中参数的类型（由重载规则区分）-->
<bean class="com.spring.bean.Monster" id="monster_03">
    <constructor-arg value="003" type="java.lang.Integer"></constructor-arg>
    <constructor-arg value="白骨精" type="java.lang.String"></constructor-arg>
    <constructor-arg value="吸人血" type="java.lang.String"></constructor-arg>
</bean>
```



### 2.3.2 通过 p 命名空间配置 Bean

通过 Alt + Insert 快速在 beans 中添加对应的命令空间，添加成功后在 beans 标签中会添加属性如下：

```
xmlns:p="http://www.springframework.org/schema/p"
```

```xml
<bean class="com.spring.bean.Monster" id="monster_04"
      p:id = "004"
      p:name = "红孩儿"
      p:skill = "三昧真火"
>
</bean>
```



### 2.3.3 通过 ref 配置 Bean

在 spring 的 IOC 容器中，可以通过 ref  来 实现 bean 对象的相互引用（之前，我们的 property 中字段对应的都是值，但是如果对应的是对象，就需要用到 ref），具体参考代码：

```
spring                                               
├─ dao                                
│  └─ MemberDAOImpl.class             
├─ service                            
│  └─ MemberServiceImpl.class         
└─ test                                       
   └─ SpringBeanTest.class            
```

```java
// MemberDAOImpl
public class MemberDAOImpl {
    public MemberDAOImpl() {
        System.out.println("MemberDAOImpl 被调用...");
    }

    public void add() {
        System.out.println("MemberDAOImpl 的add方法被调用...");
    }
}
```

```java
// MemberServiceImpl
public class MemberServiceImpl {
    private MemberDAOImpl memberDAO;

    public MemberServiceImpl() {
        System.out.println("MemberServiceImpl 被调用...");
    }

    public void add() {
        System.out.printf("MemberServiceImpl 被调用...");
        memberDAO.add();
    }

    public MemberDAOImpl getMemberDAO() {
        return memberDAO;
    }
	// 注意：一定要配置 setter 方法，IOC 底层的实现原理就是通过对象自身的 setter 方法给属性赋值
    public void setMemberDAO(MemberDAOImpl memberDAO) {
        this.memberDAO = memberDAO;
    }
}
```

**核心 xmL 配置**

```xml
<!--
    1. ref="memberDAO" 表示 MemberServiceImpl 对象属性 MemberDAO 引用的对象 id = memberDAO
    2. 在beans.xml中定义顺序可以改变，因为容器中会有"预编译"过程
    3. 这里就体现了容器的依赖注入
-->
<bean class="com.spring.dao.MemberDAOImpl" id="memberDAO"/>

<bean class="com.spring.service.MemberServiceImpl" id="memberService">
    <property name="memberDAO" ref="memberDAO"/>
</bean>
```



### 2.3.4 通过内部 Bean 来配置属性

```xml
<bean class="com.spring.service.MemberServiceImpl" id="memberService2">
    <property name="memberDAO">
        <!--  通过内部的bean设置属性  -->
        <bean class="com.spring.dao.MemberDAOImpl"/>
    </property>
</bean>
```



### 2.3.5 对集合数组属性进行配置

事先准备好 Master 类，有以下属性：

```java
private String name;       // 主人名
private List<Monster> monsterList;  // 管理的妖怪
private Map<String, Monster> monsterMap;
private Set<Monster> monsterSet;

private String[] monsterName;
private Properties props;
```

为其配置好 getter 和 setter 方法

**集合 / 数组类型赋值示例**

1. 对 List 属性进行配置

   ```xml
   <bean class="com.spring.bean.Master" id="master">
       <property name="name" value="太上老君"/>
       <!--给 list 赋值-->
       <property name="monsterList">
           <list>
               <ref bean="monster_01"/>
               <ref bean="monster_02"/>
               <!-- 除了使用引用的方式，我们还可以采取内部注入的方式-->
               <bean class="com.spring.bean.Monster">
                   <property name="id" value="003"/>
                   <property name="name" value="板角青牛"/>
                   <property name="skill" value="水火不侵"/>
               </bean>
           </list>
       </property>
   </bean>
   ```

2. 对 map 属性进行赋值

   ```xml
   <property name="monsterMap">
       <map>
           <entry key="monster_01" value-ref="monster_01"/>
           <entry key="monster_02" value-ref="monster_02"/>
       </map>
   </property>
   ```

3. 对 set 属性进行赋值

   ```xml
   <property name="monsterSet">
       <set>
           <ref bean="monster_01"/>
           <ref bean="monster_02"/>
       </set>
   </property>
   ```

4. 对 array 属性进行赋值

   ```xml
   <property name="monsterName">
       <!--下面以字符串数组为例-->
       <!-- 具体array放的是value、ref还是bean，要根据业务来决定 -->
       <array>
           <value>金角大王</value>
           <value>银角大王</value>
       </array>
   </property>
   ```

5. 给 properties 属性赋值

   ```xml
   <!--对properties中存放的数据都是以k-v形式存在的，无论是k，还是v都是String类型-->
   <property name="props">
       <props>
           <prop key="username">root</prop>
           <prop key="pwd">123456</prop>
           <prop key="ip">127.0.0.1</prop>
       </props>
   </property>
   ```



### 2.3.6 使用 utillist 进行配置

spring 里面可以通过 util 名称空间来创建 list 集合，可以当做创建 bean 对象的工具来使用

```java
// BookStore.java
public class BookStore {
    private List<String> bookList;

    public BookStore() {

    }
    public List<String> getBookList() {
        return bookList;
    }

    public void setBookList(List<String> bookList) {
        this.bookList = bookList;
    }
}
```



```xml
<!-- 通过util这个namespace来指定id以达到数据复用的目的 -->
<util:list id="myBookList">
    <value>醒世恒言</value>
    <value>警世通言</value>
    <value>喻世明言</value>
</util:list>

<bean class="com.spring.bean.BookStore" id="bookStore">
    <property name="bookList" ref="myBookList" />
</bean>
```



### 2.3.7 属性级联赋值配置

即一个类中包含另一个类（即关联关系），当我们设置该类时，同时将该类包含的类成员属性设置好，即属性级联赋值。

准备工作：提前准备好 Dept（部门类）和 Emp（员工类），在员工类中，dept 作为成员属性存在

```java
// Dept
public class Dept {
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Dept{" +
                "name='" + name + '\'' +
                '}';
    }
}
```

```java
// Emp
public class Emp {
    private String name;
    private Dept dept;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Dept getDept() {
        return dept;
    }

    public void setDept(Dept dept) {
        this.dept = dept;
    }

    @Override
    public String toString() {
        return "Emp{" +
                "name='" + name + '\'' +
                ", dept=" + dept +
                '}';
    }
}
```

级联配置如下：

```xml
<!--先配置好相关的dept-->
<bean class="com.spring.bean.Dept" id="dept"/>
<bean class="com.spring.bean.Emp" id="emp">
    <property name="name" value="张三"/>
    <!--引入dept-->
    <property name="dept" ref="dept"/>
    <!--接下来dept的name进行指定赋值-->
    <property name="dept.name" value="人力资源部"/>
</bean>
```



### 2.3.8 通过静态工厂获取 Bean

```java
// MyStaticFactoy
public class MyStaticFactory {
    // 使用Map来预存储创建好的对象
    private static Map<String, Monster> monsterMap;

    // 通过静态代码块预先创建对象
    static {
        monsterMap = new HashMap<>();
        monsterMap.put("monster_01", new Monster(100, "铁扇公主", "芭蕉扇"));
        monsterMap.put("monster_02", new Monster(101, "狐狸精", "魅惑"));
    }

    // 对外提供一个方法，用于
    public static Monster getMonster(String monsterKey) {
        return monsterMap.get(monsterKey);
    }
}
```

**配置参数**

```xml
<!--
    1. 通过静态工厂获取 / 配置bean
    2. class 是静态工程类的全路径
    3. factory-method 表示指定静态工厂类指向的是那个方法
    4. 通过 constructor-arg 来指定返回的对象(只能指定一个)
-->
<bean class="com.spring.factory.MyStaticFactory" factory-method="getMonster" id="myStaticFactory">
    <constructor-arg value="monster_01"/>
</bean>
```



### 2.3.9 通过实例工厂获取 Bean

```java
// MyInstanceFactory
public class MyInstanceFactory {
    private Map<String, Monster> monsterMap;

    {
        monsterMap = new HashMap<>();
        monsterMap.put("monster_01", new Monster(100, "铁扇公主", "芭蕉扇"));
        monsterMap.put("monster_02", new Monster(101, "狐狸精", "魅惑"));
    }

    public Monster getMonster(String key) {
        return monsterMap.get(key);
    }
}
```

**参数配置**

```xml
<!--配置实例工厂对象-->
<bean class="com.spring.factory.MyInstanceFactory" id="myInstanceFactory"/>
<!--
    1. 通过 factory-bean 指定使用哪个实例对象返回 bean
    2. factory-method 指定使用实例的方法
    3. constructor-arg 指定获取到实例工厂中的哪一个 monster
-->
<bean id="my_monster_02" factory-bean="myInstanceFactory"  factory-method="getMonster">
    <constructor-arg value="monster_02"/>
</bean>
```



:vs:**实例工厂和静态工厂的区别**

静态工厂是通过类来获取工厂中的对象，而实例工厂通过实例获取工厂中的对象，由于实例工厂可以有多个，所以从不同的实例工厂中获取的工厂的对象是不同的，我们以下面代码为例：

```xml
<!-- 配置第一个实例工厂 -->
<bean class="com.spring.factory.MyInstanceFactory" id="myInstanceFactory_01"/>
<bean id="my_monster_01" factory-bean="myInstanceFactory_01"  factory-method="getMonster">
    <constructor-arg value="monster_01"/>
</bean>

<!--配置第二个实例工厂-->
<bean class="com.spring.factory.MyInstanceFactory" id="myInstanceFactory_02"/>

<bean id="my_monster_02" factory-bean="myInstanceFactory_02" factory-method="getMonster">
    <!--这里虽然和第一个实例工厂拿的monster属性相同，但不是同一个-->
    <constructor-arg value="monster_01"/>
</bean>
```

**测试程序**

```java
ClassPathXmlApplicationContext ioc = new ClassPathXmlApplicationContext("beans.xml");
Monster myMonster01 = ioc.getBean("my_monster_01", Monster.class);
Monster myMonster02 = ioc.getBean("my_monster_02", Monster.class);

System.out.println(myMonster01);
System.out.println(myMonster02);
System.out.println(myMonster01 == myMonster02); // false
```



### 2.3.10 通过 factoryBean 获取 Bean

```java
public class MyFactoryBean implements FactoryBean<Monster> {
    // 配置要获取对象的 key 和存储 monster 对象的Map数据结构
    private String key;
    private Map<String, Monster> monster_map;

    {
        // 在代码块中完成初始化
        monster_map = new HashMap<>();
        monster_map.put("monster_01", new Monster(1, "牛魔王", "芭蕉扇"));
        monster_map.put("monster_02", new Monster(2, "孙悟空", "七十二变"));
    }

    public String getKey() {
        return key;
    }

    public void setKey(String key) {
        this.key = key;
    }

    @Override
    public Monster getObject() throws Exception {
        return monster_map.get(key);
    }

    @Override
    public Class<?> getObjectType() {
        return Monster.class;
    }

    @Override
    public boolean isSingleton() {
        return true;    // 这里是指定返回的对象是否为单例
    }
}
```



**配置参数**

```xml
<!--
    说明：
    1. class 为指定的factoryBean 的类
    2. name 为 factoryBean 中key属性
    3. value 为要得到工厂中对象的key值
-->
<bean id="myFactoryBean" class="com.spring.factory.MyFactoryBean">
    <property name="key" value="monster_01"/>
</bean>
```



## 2.4 Bean 配置信息重用

```xml
<bean class="com.spring.bean.Monster" id="monster_10">
    <property name="id" value="1"/>
    <property name="name" value="黑山老妖"/>
    <property name="skill" value="吸星大法"/>
</bean>

<!-- 使用 parent 来继承已有的 monster_10 的配置 -->
<bean class="com.spring.bean.Monster" parent="monster_10"/>

<!-- 我们还可以添加 abstract 属性，若为 true,则和类一样，只能被继承，不能被实例化(即不能直接拿过来使用) -->
<!-- 报错信息: Bean definition is abstract -->
<bean class="com.spring.bean.Monster" id="monster_12" abstract="true">
    <property name="id" value="2"/>
    <property name="name" value="蛇精"/>
    <property name="skill" value="吐信子"/>
</bean>
```



## 2.5 Bean 的创建顺序

一般而言，如果两个对象之间没有组合或依赖关系，则按顺序执行，如下：

```xml
<bean class="com.spring.bean.Monster"/>
<bean class="com.spring.bean.Dept"/>
```

如果我们使用 depends-on 指定了对象，就会先出创建依赖对象，之后才是创建本对象，如下：

```xml
<bean class="com.spring.bean.Monster" depends-on="dept002" id="monster002"/>
<bean class="com.spring.bean.Dept" id="dept002"/>
```



---



如果两个对象之间存在相互引用的关系，那么会创建被引用对象，接下来我们来分析两种情况：

**情况一**

```xml
<bean class="com.spring.dao.MemberDAOImpl" id="memberDAOImpl"/>
<bean class="com.spring.service.MemberServiceImpl" id="memberService">
    <property name="memberDAO" ref="memberDAOImpl"/>
</bean>
```

Bean 的创建顺序：

1. 先创建 id = memberDAOImpl
2. 再创建 id = memberServiceImpl
3. 最后调用  `memberServiceImpl.setMemberDAO()`



**情况二**

```xml
<bean class="com.spring.service.MemberServiceImpl" id="memberService">
    <property name="memberDAO" ref="memberDAOImpl"/>
</bean>
<bean class="com.spring.dao.MemberDAOImpl" id="memberDAOImpl"/>
```

**创建顺序**

1. 先创建 id = memberService
2. 再创建 id = memberDAOImpl
3. 最后调用  `memberServiceImpl.setMemberDAO()`



## 2.6 Bean 的单例和多例

在 spring 的 IOC 容器中，默认是按照单例创建的，即配置一个 Bean 对象后，IOC 容器只会创建一个 Bean 实例。如果，我们希望 IOC 容器配置的某个对象，是以多个实例形式创建的则可以通过 `scope = "prototype"` 

```xml
<!--1. 在默认情况下，scope 属性是 singleton-->
<!--2. 在 IOC 容器中，只要有一个这个 bean 对象-->
<!--3. 当我们执行 getBean 时，返回的是同一个对象-->
<!--4. 如果我们希望每次返回的对象不同，可以添加 scope="prototype"-->
<!--<bean class="com.spring.bean.Cat" id="cat" scope="prototype">-->
<bean class="com.spring.bean.Cat" id="cat">
    <property name="name" value="小花"/>
    <property name="age" value="2"/>
</bean>
```



:card_index_dividers:**使用细节**

1. bean 默认是单例 singleton，<u>在启动容器时，默认就会创建，并放入到 singletonObjects 集合中</u>

2. 当 scope 修改为 prototype ，将其设置为多实例机制后，该 bean 是在 `getBean()` 时才会创建

3. 如果是单例 singleton，同时希望在  `getBean()` 时才创建，可以指定懒加载 `lazy-init = true`（注意：默认是 false）

   > 通常情况下，lazy-init 默认值为 false，从开发者的角度看，以空间换时间是值得的

4. 如果将 scope 设置为 prototype ，无论 lazy-init 是 true 还是 false ，都是在 `getBean()` 时才会创建对象



## 2.7 Bean 的生命周期

bean 对象的创建是由 JVM 完成的，然后执行如下方法：

1.  执行构造器
2. 执行 setter
3. 调用 bean 的初始化方法（需要配置）
4. 使用 bean
5. 当容器关闭时，调用 bean 的销毁方法 （需要配置）

**测试程序**

```java
// Dog.java
public class Dog {
    private String name;

    public Dog() {
        System.out.println("Dog 构造方法被调用");
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        System.out.println("setter 方法被调用");
        this.name = name;
    }

    // 配置初始化方法（由自己的业务决定，方法名可以不同）
    public void init() {
        System.out.println("init 方法被调用");
    }

    // 配置销毁方法（由自己的业务决定，方法名可以不同）
    public void destroy() {
        System.out.println("destroy 方法被调用");
    }
}
```

```xml
<!-- 配置文件 -->
<bean class="com.spring.bean.Dog" id="dog" init-method="init" destroy-method="destroy">
    <property name="name" value="旺财"/>
</bean>
```

```java
// 验证程序
// 推荐使用接口接收，使得后续操作更加灵活
ApplicationContext ioc = new ClassPathXmlApplicationContext("beans.xml");

Dog dog = ioc.getBean("dog", Dog.class);

// 关闭容器使用 close 方法，但由于Application中没有该方法，只能将其向下转换为ConfigurableApplicationContext
((ConfigurableApplicationContext)ioc).close();
```

最终的执行结果为：

```
Dog 构造方法被调用
setter 方法被调用
init 方法被调用
destroy 方法被调用
```



## 2.8 配置 Bean 后置处理器

1. 在 spring 的 IOC 容器中，可以配置 bean 的后置处理器
2. 该处理器 / 对象会在 bean 初始化方法调用前和初始化方法调用后被调用
3. 程序员可以在后置处理器中编写自己的代码

> 后置处理器本质上是一个对象

假设我们现在自己配置了一个 Bean 的后置处理器，如下：

```java
// MyBeanPostProcessor
public class MyBeanPostProcessor implements BeanPostProcessor {

    /**
     * 调用时机：在Bean的init方法前被调用
     * @param bean 传入在 IOC 容器中配置的Bean对象
     * @param beanName 传入在 IOC 容器中配置的Bean id
     * @return Object 处理完成后返回的Bean对象
     * @throws BeansException
     */
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("before: " + bean + "," + beanName);
        return BeanPostProcessor.super.postProcessBeforeInitialization(bean, beanName);
    }

    /**
     * 调用时机：在Bean的init方法后被调用
     * @param bean
     * @param beanName
     * @return
     * @throws BeansException
     */
    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("after: " + bean + "," + beanName);
        return BeanPostProcessor.super.postProcessAfterInitialization(bean, beanName);
    }
}
```



在 beans02.xml 中作如下配置：

```xml
<bean class="com.spring.bean.Dog" id="myDog" init-method="init" destroy-method="destroy">
    <property name="name" value="大黄"/>
</bean>

<!--
    当我们在 beans02.xml 容器配置文件中配置了 MyBeanPostProcessor
    后置处理器对象就会作用在该容器中创建的所有 Bean 对象
-->
<bean class="com.spring.bean.MyBeanPostProcessor"/>
```



我们在测试程序中执行，测试结果如下：

```
Dog 构造方法被调用
setter 方法被调用
before: com.spring.bean.Dog@cb0ed20,myDog
init 方法被调用
after: com.spring.bean.Dog@cb0ed20,myDog
destroy 方法被调用
```



:satellite:**说明**

1. 底层原理：AOP（反射 + 动态代理）
2. 作用：<strong style="color:red">对 IOC 容器中所有的对象进行统一处理</strong>
3. 应用范围：<u>日志处理、权限校验、安全验证、事务管理</u>



## 2.9 通过属性文件配置 Bean

在之前，我们都是直接在 xml 文件中直接将属性值输入，这里不是特别灵活，我们可以将其放在配置文件中，在从中取值，以下面为例：

在 src 下创建 myConfig.properties，如下：

```properties
id=10
name=darcula
skill=suckBlood
```

配置 xml

```xml
<!--
说明：
1. 需要将 context 加入到 namespace 中，需要将 highlight 设置为 all problems ，之后通过 alt + enter 即可
2. 对于配置，这里是放在 src 下，所以可以直接在 classPath 后直接接文件名
3. 配置文件全局有效
-->
<context:property-placeholder location="classpath:myConfig.properties"/>

<!--获取配置文件信息通过el（表达式语言）语法取出，即通过 ${} 的形式取值-->
<!--配置 monster 对象-->
<bean class="com.spring.bean.Monster" id="darcula">
<property name="id" value="${id}"/>
<property name="name" value="${name}"/>
<property name="skill" value="${skill}"/>
```

:herb:**扩展：处理 properties 文件中文乱码**

解决方案：<strong style="color:red">直接在配置文件中，将中文转换成 Unicode 编码格式</strong>



## 2.10 自动装配

**示例**

```
spring                               
├─ action                            
│  └─ OrderAction.java                    
├─ dao                                       
│  └─ OrderDAO.java                      
├─ service                              
   └─ OrderService.java              
```



**配置**

```xml
<!--配置 orderDAO 对象-->
<bean class="com.spring.dao.OrderDAO" id="orderDAO"/>
<!--配置 orderService 对象-->
<!--根据类型自动装配，注意：这种方式，在同一个 xml 配置文件中不能存储多个装填类型的对象-->
<bean class="com.spring.service.OrderService" autowire="byType" id="orderService"/>
<bean class="com.spring.action.OrderAction" autowire="byType" id="orderAction"/>
<!--
    说明：
    1. autowire="byType" 表示在创建 orderService 时，通过类型的方式给对象属性自动完成赋值
    2. 例如 OrderService 对象有成员属性 private OrderDAO orderDAO
    3. 就会自动在容器中寻找没有没有 OrderDAO 类型的对象
    4. 如果有，就会自动进行装配，注意：在同一个容器中，不能有两个 OrderDAO 类型的 Bean 对象
-->
```

除了使用 `autowire="byType"` 的方式做到自动装配，我们还可以使用 `byName` 的方式：

> 注意：这种方式是根据 bean 对象 `setXxx()` 后面的 `Xxx` 来寻找 `id=xxx` 对象装配到自身属性中

```xml
<bean class="com.spring.dao.OrderDAO" id="orderDAO"/>
<bean class="com.spring.service.OrderService" autowire="byName" id="orderService"/>
```

OrderService 的 setter 如下：

```java
public void setOrderDAO(OrderDAO orderDAO) {
    this.orderDAO = orderDAO;
}
```

> :warning:注意：一旦 set 后面换成了 OrderDAO2，那么 orderDAO 这个 Bean 的 id 也要换成 orderDAO2



## 2.11 Spring EL 表达式

```xml
<!-- spring  el达式 -->
<bean id="spELBean" class="com.spring.dao.ElDAO">
    <!-- el给字面量 -->
    <property name="name" value="#{'Alice'}"/>
    <!-- el引用其它 bean -->
    <property name="monster" value="#{monster01}"/>
    <!-- el引用其它 bean 的属性值 -->
    <property name="monsterName" value="#{monster02.name}"/>
    <!-- el调用普通方法(返回值)赋值 -->
    <property name="crySound" value="#{spELBean.cry('喵喵的..')}"/>
    <!-- el调用静态方法赋值(注意：全类名前面需要加上T) -->
    <property name="bookName" value="#{T(com.spring.dao.ElDAO).Func(10)}"/>
    <!-- el过运算赋值    -->
    <property name="result" value="#{89*1.2}"/>
</bean>
```



# 3 基于注解配置 Bean

## 3.1 基本使用

基于注解的方式配置 Bean，主要是应用于项目开发中的组件，例如：Controller、Service 和 DAO

组件注解的形式主要有：

- **`@Component`**：表示当前注解标识的是一个组件
- **`@Controller`**：表示当前注解标识的是一个控制器，通常用于 Servlet
- **`@Service`**：表示当前注解标识的是一个处理业务逻辑的类，通常用于 Service 类
- **`@Repository`**：表示当前注解标识的是一个持久化层的类，通常用于 DAO 类



## 3.2 快速入门

首先，我们需要引入需要的 jar 包 —— spring-aop-5.3.8.jar，拷贝在 spring/libs 下即可

我们在 src 下创建 com.spring.component 包，在 component 包下创建如下类：

```java
package com.spring.component;

import org.springframework.stereotype.Component;

@Component
public class MyComponent {
}
```

```java
package com.spring.component;

import org.springframework.stereotype.Controller;

@Controller
public class UserAction {
}
```

```java
package com.spring.component;

import org.springframework.stereotype.Repository;

@Repository
public class UserDAO {
}
```

```java
package com.spring.component;

import org.springframework.stereotype.Service;

@Service
public class UserService {
}
```



**配置 xml**

```xml
<!--
    1. component-scan 要对指定包下的类进行扫描，并创建对象到容器中
    2. base-package 指定要扫描的包
    3. 解析：spring 容器创建 / 初始化时，会扫描 base-package 指定的包，在该包下所有添加有
    @Component | @Repository | @Service | @Controller 注解的类将其实例化，生成对象，放入到IOC容器中
-->
<context:component-scan base-package="com.spring.component"/>
```



**测试程序**

```java
ApplicationContext ioc = new ClassPathXmlApplicationContext("beans03.xml");
// 由于 beans 中对应类型的对象只有一个，我们可以通过 .class 的方式获取
UserAction action = ioc.getBean(UserAction.class);
UserDAO dao = ioc.getBean(UserDAO.class);
// 同时ioc容器在初始化/创建时，会将创建好的单例放在singletonObjects,创建对象的key设置为类名的小驼峰
UserService service = ioc.getBean("userService", UserService.class);
MyComponent myComponent = ioc.getBean("myComponent", MyComponent.class);
```



:label:**小结**

我们可以发现，注解最大的好处就是实现了批量配置，只需要在 xml 配置文件中引入相应的包即可，不需要一个又一个地去添加配置，使得维护起来也更加方便



## 3.3 注解配置的注意事项

1. 需要导入 spring-aop 这个 jar 包

2. 必须在 Spring 配置文件中指定 "自动扫描的包"，IOC 容器才能够检测到当前项目中哪些类被标识了注解，注意导入 context 名称空间

   ```xml
   <context:component-scan base-package="com.spring.component"/>
   ```

3. 可以搭配通配符 `*` 来指定，例如：`com.spring.*`

4. 扫描属于深层扫描，会将指定包下所有的子包也会全部都扫描一遍

5. Spring 的 IOC 容器不能检测一个使用了 `@Controller` 注解的类究竟是不是一个真正的控制器类，注解的名称是用于给程序员做标识看的，方便程序员去区分哪些是 Controller，哪些是 DAO

6. 上面的几种注解在一般环境下是没有区别的，但是如果是在 Spring MVC 下，四者还是存在区别的

7. 我们在 `context:component-scan` 下还可以添加配置 `resource-pattern`来进行过滤

   ```xml
   <context:component-scan base-package="com.spring.component" resource-pattern="User*.class"/>
   <!--在这种情况下，只会匹配component下的以User打头的类-->
   ```

   > 这种形式使用较少，因为如果不想将其添加到 IOC 容器中，完全可以不写注释

8. 排除某些注解的配置，如下：

   ```xml
   <context:component-scan base-package="com.spring.component">
       <!--这种方式就可以直接去除掉 Service 注释打头的类-->
       <!--
           1. context:exclude-filter 指定要排除的类
           2. type 指定排除方式，这里为 annotation，表示以注解方式排除
           3. expression 指定排除注解的全类名
       -->
       <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Service"/>
   </context:component-scan>
   ```

9. 指定自动扫描的类

   ```xml
   <!--
       1.use-default-filters="false": 表示不再使用默认的扫描机制（默认的扫描机制是全扫描）
       2.context:include-filter type="annotation" 以注解的类型，只扫描指定注解的类
       3.expression 后面接注解的全类名
   -->
   <context:component-scan base-package="com.spring.component" use-default-filters="false">
       <context:include-filter type="annotation" expression="org.springframework.stereotype.Service"/>
       <context:include-filter type="annotation" expression="org.springframework.stereotype.Repository"/>
   </context:component-scan>
   ```

10. <strong style="color:red">默认情况下：标记注解后，类名首字母小写作为 id 的值。我们也可以使用注解的 value 属性指定 id 值，并且 value 可以省略，如下：</strong>

    ```java
    @Controller(value="userAction01")
    @Controller("userAction01")
    ```



## 3.4 手动实现 spring 注解配置（重点，日后补充）





## 3.5 自动装配

基于注解配置 Bean ，也可以实现自动装配，常用的注解有：

- `@AutoWired`
- `@Resource`

### 3.5.1 @AutoWired

**演示**

之前我们在 component 包下创建了空的 UserService 和 UserAction 这个类，现在我们将其添加一些内容：

```java
// UserAction
package com.spring.component;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;

@Controller
public class UserAction {
    // 使用 field 注入（当然 spring 团队不推荐这种方式）
    // Field injection is not recommended
    @Autowired
    private UserService userService;

    public void callService() {
        userService.say();
    }

}
```

```java
// UserService
package com.spring.component;

import org.springframework.stereotype.Service;

@Service
public class UserService {
    public void say() {
        System.out.println("UserService 中的say方法被调用...");
    }
}
```

**添加配置如下**

```xml
<context:component-scan base-package="com.spring.component" use-default-filters="false">
    <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    <context:include-filter type="annotation" expression="org.springframework.stereotype.Service"/>
</context:component-scan>
```

**测试程序**

```java
ApplicationContext ioc = new ClassPathXmlApplicationContext("beans04.xml");
UserAction userAction = ioc.getBean("userAction", UserAction.class);
userAction.callService();
```



:incoming_envelope:补充：如果待装配的类型在容器中存在多个，则会自动装配 id 为属性名的 Bean 对象

```java
@Controller
public class UserAction {
    // 这里如果装配的类型对应的bean在IOC容器中有多个，则使用待装配属性的属性名作为id值进行查找（在这里即是以 userService 为 key 进行查找）
    /*
    * 我们再次梳理一下：
    * 1. 如果IOC容器自动装配的类型对象只有一个，则使用该对象
    * 2. 如果IOC容器自动装配的类型对象有多个，则以待装配的属性名作为id值进行查找
    * 3. 如果找不到则会报错
    */
    @Autowired
    private UserService userService;

    public void callService() {
        userService.say();
    }
}
```



### 3.5.2 @Resource

`@Resource` 有两个属性是比较重要的，分别是 name 和 type，Spring 将 `@Resource` 注解的 name 属性解析为 bean 的名字，而 type 属性则解析为 bean 的类型

> 所以，如果使用 name 属性，则使用 byName 的自动注入策略；如果使用 type 属性，则使用 byType 自动注入策略

```java
/**
 * @Resource(name="userService") 表示装配 id=userService 对象
 * 如果没有则会报错
 */
@Resource(name="userService")
private UserService userService;
```

```java
/**
 * @Resource(type="UserService.class") 表示装配类型为 UserService 的对象
 * 如果没有或者存在多个类型为 UserService 的对象则会报错
 */
@Resource(type = UserService.class)
private UserService userService;
```

```java
/**
 * 如果我们使用 @Resource，并且没有指定 name 和 type，则先使用 byName 策略
 * 如果 byName 策略没有找到，那么采用 byType(该类型确保只能有一个)策略
 * 如果仍然没有找到，则会报错
 */
@Resource
private UserService userService;
```



## 3.6 泛型依赖注入

**基本说明**

1. 为了更好地管理有继承和相互依赖的 bean 的自动装配，spring 提供基于泛型依赖的注入机制
2. 在继承关系复杂的情况下，泛型依赖注入就会有很大的优越性

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230331232214210.png" alt="image-20230331232214210" style="zoom:50%;" />

**目录结构**

```
generic                   
├─ dao                    
│  ├─ BaseDao.class       
│  ├─ BookDao.class       @Repository
│  └─ PhoneDao.class      @Repository
├─ domain                 
│  ├─ Book.class          
│  └─ Phone.class         
└─ service                
   ├─ BaseService.class   @Autowired 自动装配 BaseBao
   ├─ BookService.class   @Service
   └─ PhoneService.class  @Service
```

**分析**：现在我们将只要利用 `@Autowired` 将 `Base<T>` 进行自动装配，那么 BookService 和 PhoneService 就能将 BookDao 和 PhoneDao 装配进来，这便是泛型依赖注入

**代码**

```java
// dao

// BaseDao
public abstract class BaseDao<T> {
    public abstract void say();
}

// BookDao
@Repository
public class BookDao extends BaseDao<Book> {
    @Override
    public void say() {
        System.out.println("BookDao 的say方法被调用");
    }
}

// PhoneDao
@Repository
public class PhoneDao extends BaseDao<Phone> {
    @Override
    public void say() {
        System.out.println("PhoneDao 的say方法被调用");
    }
}
```

```java
// Service

// BaseService
public class BaseService<T> {
    // 这里只能使用 Autowired 自动注入，使用 @Resource 会报错（idea工具在这里会提示我们无法注入泛型，但是仍然可以运行）
    @Autowired
    private BaseDao<T> baseDao;
    public void save() {
        baseDao.say();
    }
}

// BookService
@Service
public class BookService extends BaseService<Book> {}

// PhoneService
@Service
public class PhoneService extends BaseService<Phone> {}
```



# 4 AOP 

## 4.1 AOP 文档说明

AOP 的全称为 "Aspect Oriented Programming"

官方文档位置：在下载好的 spring 压缩包解压缩后位于 docs >> reference >> html 中，我们点开 index.html，找到 core，找到 AOP 即可

![image-20230401091638012](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230401091638012.png)

相关的 API ，我们也可以在该参考文档中找到：

![image-20230401091826245](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230401091826245.png)

相关 API ，我们也可以在 docs >> javadoc-api 中找到



## 4.2 动态代理

**为什么要引入动态代理？动态代理又是什么？**

在了解这个之前，我们现引入一个小案例：

现在我们有一个需求，就是提供一个接口 Vehicle，让 Ship 和 Car 这两个类分别实现 `run()` 方法，当 Ship 对象执行 `run()` 方法时，打印内容如下：

```
交通工具开始工作...
轮船在水上游
交通工具停止工作...
```

当 Car 对象执行了 `run()` 方法时，打印内容如下：

```
交通工具开始工作...
汽车在马路上跑
交通工具停止工作...
```

我们传统的实现方法：将 Vehicle 设置为接口，实现子类重写 `run()` 方法，如下：

```java
public class Ship implements Vehicle {
    @Override
    public void run() {
        System.out.println("交通工具开始工作...");
        System.out.println("轮船在水上游");
        System.out.println("交通工具停止工作...");
    }
}
```

但是，这里就出现弊端："交通工具开始工作..." 和 "交通工具停止工作..." 相当于 `run()` 的前置工作和后置工作，如果将其集成在方法体中，那么当实现子类多了的时候就会出现代码冗余，且不利于维护。

接下来，我们将模拟动态代理的实现，其中用到了 `Proxy.newProxyInstance()` 方法，里面需要传入目标对象的类加载器、接口类型数组以及 InvocationHandler 对象，我们需要重写 invocationHandler 对象中的 `invoke()` 方法，示例代码如下：

```java
public class VehicleProxyProvider {
    // 设置一个将来要运行的对象，该对象实现了 Vehicle 接口
    private Vehicle target_vehicle;

    public VehicleProxyProvider(Vehicle target_vehicle) {
        this.target_vehicle = target_vehicle;
    }

    // 核心代码，返回代理对象
    public Vehicle getProxy() {
        // 1. 获取类加载器
        ClassLoader classLoader = target_vehicle.getClass().getClassLoader();

        // 2. 获取接口类型数组
        Class<?>[] interfaces = target_vehicle.getClass().getInterfaces();

        // 3. 创建 InvocationHandler 对象（使用匿名内部类的方式创建）
        InvocationHandler invocationHandler = new InvocationHandler() {
            /**
             *
             * @param proxy the proxy instance that the method was invoked on
             *
             * @param method 该参数即为要为代理对象配置的反射执行的方法
             * @param args 传入方法的参数列表，例如 run(xx, xx, ...)
             *
             * @return  代理对象反射执行方法后返回的结果
             * @throws Throwable
             */
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                System.out.println("交通工具开始工作...");
                Object _invoke = method.invoke(target_vehicle, args);
                System.out.println("交通工具停止工作...");
                return _invoke;
            }
        };
        // 将上面的 loader、interfaces、invocationHandler 构建一个 Vehicle 的代理对象并返回
        return (Vehicle)Proxy.newProxyInstance(classLoader, interfaces, invocationHandler);
    }
}
```

之后，我们创建该类的实例，通过 `getPoxy()` 得到代理对象，用代理对象执行相应的方法：

```java
Vehicle vehicle = new Ship();

// 创建 vehicleProxyProvider 对象并传入要代理的对象
VehicleProxyProvider vehicleProxyProvider = new VehicleProxyProvider(vehicle);
// 得到代理对象，该对象可以代理执行方法
Vehicle proxy = vehicleProxyProvider.getProxy();

proxy.run();
```

我们发现同样可以实现目标效果。

我们再通过 Debug 追到 `proxy.run()` 中，可以发现会直接进入到 `invoke()` 中，其中对象方法的反射实现则是通过 `method.invoke()`，我们在该语句前后可以处理对应的业务



## 4.3 横切关注点

所谓的 "横切关注点"，其实就是**通知**，包括：<u>前置通知、返回通知、异常通知、最终通知</u>

接下来，我们以案例进行说明：

首先，准备的三个类，类图如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230401144354617.png" alt="image-20230401144354617" style="zoom: 33%;" />

当我们在测试程序中，使用得到的 proxy 代理对象执行 `sub()` 和 `sum()` 方法后，打印输出的结果为：

```
日志-方法名-sub-参数 [1.0, 2.0]
方法执行正常结束-日志-方法名-sub-结果 -1.0
方法最终结束-日志-方法名-sub

日志-方法名-sum-参数 [1.0, 2.0]
方法执行正常结束-日志-方法名-sum-结果 3.0
方法最终结束-日志-方法名-sum
```

实现代码：

```java
// SmartAnimal
interface SmartAnimal {
    float sum(float a, float b);
    float sub(float a, float b);

}
```

```java
// SmartDog
public class SmartDog implements SmartAnimal {
    @Override
    public float sum(float a, float b) {
        return a + b;
    }

    @Override
    public float sub(float a, float b) {
        return a - b;
    }
}
```

```java
// SmartAnimalProxyProvider
public class SmartAnimalProxyProvider {
    private SmartAnimal smartAnimal;

    public SmartAnimalProxyProvider(SmartAnimal smartAnimal) {
        this.smartAnimal = smartAnimal;
    }

    public SmartAnimal getProxy() {
        ClassLoader classLoader = smartAnimal.getClass().getClassLoader();
        Class<?>[] interfaces = smartAnimal.getClass().getInterfaces();
        InvocationHandler invocationHandler = new InvocationHandler() {
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                Object res = null;
                try {
                    // method.getName(): 获取方法名
                    // Arrays.asList(): 将数组转换为 list 集合输出
                    // 1、从 AOP 角度看，这里是一个横切关注点——前置通知
                    System.out.println("日志-方法名-" + method.getName() + "-参数 " + Arrays.asList(args));
                    // 使用反射调用方法
                    res = method.invoke(smartAnimal, args);
                    // 2、从 AOP 角度看，这也是一个横切关注点——返回通知
                    System.out.println("方法执行正常结束-日志-方法名-" + method.getName() + "-结果 " + res);

                } catch (Exception e) {
                    e.printStackTrace();
                    // 3. 从 AOP 的角度看，这也是一个横切关注点——异常通知
                    System.out.println("方法执行异常-日志-方法名-" + method.getName() + "-结束");
                } finally {
                    // 4. 从 AOP 的角度看，这也是一个横切关注点——最终通知
                    System.out.println("方法最终结束-日志-方法名-" + method.getName());

                }
                return res;
            }
        };

        return (SmartAnimal)Proxy.newProxyInstance(classLoader, interfaces, invocationHandler);

    }
}
```



## 4.3 AOP 问题提出

![image-20230401145119454](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230401145119454.png)



如果我们解决这个问题其实很简单，就直接新建一个 AOP 类（也可以取其他名），为这个 AOP 类添加 before 和 after 静态方法，里面写入相应的业务处理代码，但是这种方式仍然存在不足，例如：

1. 不够灵活，例如：我们只想让 `sub()` 方法能够执行到 before 和 after 静态方法
2. 本质还是一种硬编码（因为没有用到注解和反射作为支撑）

于是，我们的 Spring AOP 便登场了，<strong style="color:red">AOP 全称为 "Aspect Oriented Programming"，即 "面向切面编程"</strong>

> Spring AOP 底层是 ASPECTJ ，所用的技术还是 反射 + 注解 + 动态代理

下面我们以两张图来进行说明：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230401153926866.png" alt="image-20230401153926866" style="zoom:50%;" />

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230401153938893.png" alt="image-20230401153938893" style="zoom: 67%;" />

所以 "切面编程"，我们可以简单理解为设置一个切面类，该切面类的方法可以直接 "切开" invoke 的执行流程，将自己的方法穿插到横切关注点的位置

**AOP 的实现方式**

1. 基于动态代理的方式（即我们之前实现的方式）
2. 使用框架 aspectj 来实现



## 4.4 AOP 快速入门

**说明**

1. 需要引入核心的 aspect 包

   <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230401155523479.png" alt="image-20230401155523479" style="zoom:50%;" />

2. 在切面类中声明通知方法

   - 前置通知：**`@Before`**
   - 返回通知：**`@AfterReturning`**
   - 异常通知：**`@AfterThrowing`**
   - 后置通知：**`@After`**
   - 环绕通知：**`@Around`**

![image-20230401155014730](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230401155014730.png)



**示例**

```java
interface SmartAnimal {
    float sum(float a, float b);
    float sub(float a, float b);
}     
```

```java
// 自定义的一个切面类 SmartAnimalAspect
@Aspect     // 表示是一个切面类（spring底层会对其进行处理）
@Component  // 会将其注入到容器中
public class SmartAnimalAspect {

    /**
     * Before 表示这是一个前置通知
     * execution 指定了在那个函数下设置通知
     * 注意方法名前要加上全类名指定针对哪一个类而言
     * 方法的参数列表只需要带参数类型，不需要加上参数名
     * 总而言之，切入形式为：访问修饰符 返回类型 全类名+方法名(参数类型列表)
     * f1 这个方法名称自定义，里面的只能填入 JoinPoint （连接点）
     * @param joinPoint 在底层执行时会给切入方法传入连接点对象
     */
    @Before(value = "execution(public float com.aop.SmartDog.sum(float, float))")
    public void f1(JoinPoint joinPoint) {
        // 通过连接点得到签名
        Signature signature = joinPoint.getSignature();
        // 得到切入方法名
        System.out.println(signature.getName());
        // 得到传入方法的参数列表
        System.out.println(Arrays.toString(joinPoint.getArgs()));
    }
}
```

**xml 配置程序**

```xml
<!-- 自动扫描包 -->
<context:component-scan base-package="com.aop"/>
<!-- 开启AOP注解功能 -->
<aop:aspectj-autoproxy/>
```

**测试程序**

```java
ApplicationContext ioc = new ClassPathXmlApplicationContext("beans06.xml");
//SmartDog smartDog = (SmartDog)ioc.getBean("smartDog");
//smartDog.sub(1, 2);

SmartAnimal smartDog = ioc.getBean("smartDog", SmartAnimal.class);
smartDog.sum(1, 2);
System.out.println(smartDog.getClass());
/**
 * 由于我们是给 SmartDog 类前面添加的 @Component 注解，所以创建的bean的key=smartDog
 * 但是，我们在之前其实是通过动态代理的机制实现过，return (SmartAnimal)Proxy.newProxyInstance(classLoader, interfaces, invocationHandler);
 * 最后返回的其实是将 Proxy 向下转型的 SmartAnimal 接口类型，所以获取时只能使用SmartAnimal.class
 * 其运行类型仍然为 Proxy
 */
```



:warning:**注意事项**

1. 规范切面类方法命令
   - 前置通知：**`showBeginLog()`**
   - 返回通知：**`showSuccessEndLog()`**
   - 异常通知：**`showExceptionLog()`**
   - 后置通知：**`showFinallyEndLog()`**

2. 切入表达式还可以搭配使用模糊配置

   ```java
   @AfterReturning(value = "execution(* com.aop.SmartDog.*(..))")
   // 表示匹配 com.aop.SmartDog 下面任意权限、任意方法、任意参数列表
   /**
    * 第一个 "*" 表示访问权限
    * 第二个 "*" 表示类下的方法
    * (..) 表示参数列表可以任意
    */
   public void showSuccessEndLog(JoinPoint joinPoint) {
       // 业务处理代码
   }
   ```

   > 如果我们采用如下通配符的注解配置，那么就会匹配所有包、所有类的所有方法
   >
   > ```java
   > @Before(value = "execution(* *.*(..))")
   > ```

3. 当 spring 容器开启了基于注解的 AOP 功能，即 `<aop:aspect-autoproxy/>` 时，我们获取注入的对象，需要以接口的类型来获取，因为注入的对象已经通过 `Proxy.newProxyInstance` 注入的是代理类型了；除此之外，我们获取注入的对象，也可以通过 id 来获取，但是也要转成接口类型

   > 这里之所以不使用代理类型，因为注入的代理类型是一个匿名内部类，例如 `$proxy14` 的这种形式，我们无法准确得到其类型



## 4.5 切入表达式

![image-20230401184630412](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230401184630412.png)

![image-20230401184637330](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230401184637330.png)

![image-20230401184641928](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230401184641928.png)



:warning:**注意事项**

1. 切入表达式也可以指向接口的方法，这时切入表达式会对实现了接口的类/对象生效

   > 备注：只会对实现类/对象中的实现方法生效，对其特有方法不生效

2. <strong style="color:red">切入表达式也可以对没有实现接口的类，进行切入</strong>

   > 这就涉及到动态代理jdk的 Proxy 和 spring 的 CGlib，参考文章链接：https://www.cnblogs.com/threeAgePie/p/15832586.html

**演示**

```java
// 在原有代码的基础上，添加一个普通的 person 类
@Component
public class Person {
    public void say() {
        System.out.println("hello");
    }
}
```

```java
// 添加前置通知
@Before(value = "execution(public void com.aop.Person.say())")
public void showBeginLog(JoinPoint joinPoint) {
    Signature signature = joinPoint.getSignature();
    System.out.println("method = " + signature.getName());
}
```

**测试程序**

```java
ApplicationContext ioc = new ClassPathXmlApplicationContext("beans06.xml");
Person person = ioc.getBean("person", Person.class);
person.say();
System.out.println(person.getClass());	// class com.aop.Person$$EnhancerBySpringCGLIB$$58c68abe
```

> 不同点：
>
> - 我们可以直接通过 Person.class 类获取 bean，之前我们都是通过该类实现的接口类型来获CGLIB取
> - person 的运行类型不再是 proxy ，而是 CGLIB

这里，我们直接截取推荐文章的结论来说明：

1. JDK动态代理是面向接口的，只能增强实现类中接口中存在的方法。CGlib是面向父类的，可以增强父类的所有方法
2. JDK得到的对象是JDK代理对象实例，而<strong style="color:red">CGlib得到的对象是被代理对象的子类</strong>



## 4.6 JoinPoint

```java
Signature signature = joinPoint.getSignature();
// JoinPoint 常用方法
signature.getName();    // 获取目标方法名
signature.getDeclaringType().getSimpleName();   // 获取简单类名
signature.getDeclaringTypeName();   // 获取全类名
signature.getModifiers();   // 获取目标方法的访问修改符（以数字表示，例如：1 public | 2 private | 4 protected）
Object[] args = joinPoint.getArgs();    // 获取传入目标的方法的参数，返回的是一个Object数组
joinPoint.getTarget();  // 获取被代理的对象
joinPoint.getThis();     // 获取代理本身
```



## 4.7 返回通知获取结果

对于返回通知获取结果当然只只针对于  **`@AfterRetuning`**，我们查看 `@AfterReturning` 底层源码，也可以看到存在一个属性 returning，用来接收方法执行的结果

```java
public @interface AfterReturning {
    String value() default "";

    String pointcut() default "";

    String returning() default "";

    String argNames() default "";
}
```

**示例代码**

```java
@AfterReturning(value = "execution(public float getSum(float, float))", returning="res")
public void showSuccessEndLog(JoinPoint joinPoint, Object res) {
    System.out.println("方法执行返回的结果为: " + res);
}
```

> 注意 returning 中参数名需要和返回通知方法中的 `Object res`，属性名保持一致



## 4.8 异常通知中获取异常信息

异常通知中获取异常信息与返回通知中获取结果信息类似如下：

```java
@AfterThrowing(value = "execution(public int getDivide(int, int))", throwing = "e")
public void showExceptionLog(JoinPoint joinPoint, Throwable e) {
    System.out.println("异常信息为：" + e.getMessage());
}
```

> 这里有点小收获，就是：原来浮点 1 除以 浮点 0 的结果为 infinity ，并不会报错



## 4.9 环绕通知（了解）

**环绕通知 `@Around` 可以完成其它四个通知要做的事情**

```java
/**
 * @Around 表示这个一个环绕通知 —— 完成其他四个通知的功能
 * doRound 表示要切入的方法 —— 调用的结构为 try-catch-finally
 * 使用连接点类型为 ProceedingJoinPoint，是 JoinPoint 的子类
 */
@Around(value = "execution(public float getSum(float, float))")
public Object doRound(ProceedingJoinPoint pJoinPoint) {
    Object result = null;
    try {
        // 1. 完成前置通知的任务
        Object[] args = pJoinPoint.getArgs();   // 得到参数列表
        String name = pJoinPoint.getSignature().getName();  // 得到方法名

        // 执行方法，相当于 result = method.invoke(target_obj, args)
        result = pJoinPoint.proceed();

        // 2. 完成后置通知的任务
        System.out.println("AOP 环绕通知，方法执行的结果为: " + result);
    } catch(Throwable throwable) {
        // 3. 完成异常通知的任务
        System.out.println("异常信息为: " + throwable.getMessage());
    } finally {
        // 4. 完成最终通知的任务
        System.out.println("AOP 环绕通知执行结束...");
    }
    return result;
}
```



## 4.10 切入点表达式重用

为了统一管理切入点表达式，可以使用切入点表达式重用技术。

```java
// 定义一个切入点，给后面的切入点表达式复用; 定义的格式类似于方法的定义
@Pointcut(value = "execution(public float com.aop.SmartDog.sum(float, float))")
public void myPointcut() {
}

@Before(value = "myPointcut()")
public void f1(JoinPoint joinPoint) {}

@AfterReturning(value = "myPointcut()", returning = "res")
public void showSuccessEndLog(JoinPoint joinPoint, Object res) {}

@AfterThrowing(value = "myPointcut()", throwing = "e")
public void showExceptionLog(JoinPoint joinPoint, Throwable e) {}
```



## 4.11 AOP 切面优先级问题（执行顺序）

**测试程序**

```java
// AOP 类1
@Aspect     // 表示是一个切面类（spring底层会对其进行处理）
@Component  // 会将其注入到容器中
public class SmartAnimalAspect_1 {

    @Pointcut(value = "execution(public float com.aop.SmartDog.sum(float, float))")
    public void myPointcut() {
    }

    @Before(value = "myPointcut()")
    public void showBeginLog(JoinPoint joinPoint) {
        System.out.println("SmartAnimalAspect_1 的前置通知被调用");
    }

    @AfterReturning(value = "myPointcut()", returning = "res")
    public void showSuccessEndLog(JoinPoint joinPoint, Object res) {
        System.out.println("SmartAnimalAspect_1 的返回通知被调用");
    }

    @After(value = "myPointcut()")
    public void showExceptionLog(JoinPoint joinPoint) {
        System.out.println("SmartAnimalAspect_1 的最终通知被调用");
    }
}
```

```java
// AOP 类2
@Aspect
@Component
public class SmartAnimalAspect_2 {
    @Pointcut(value = "execution(public float com.aop.SmartDog.sum(float, float))")
    public void myPointcut() {
    }

    @Before(value = "myPointcut()")
    public void showBeginLog(JoinPoint joinPoint) {
        System.out.println("SmartAnimalAspect_2 的前置通知被调用");
    }

    @AfterReturning(value = "myPointcut()", returning = "res")
    public void showSuccessEndLog(JoinPoint joinPoint, Object res) {
        System.out.println("SmartAnimalAspect_2 的返回通知被调用");
    }

    @After(value = "myPointcut()")
    public void showExceptionLog(JoinPoint joinPoint) {
        System.out.println("SmartAnimalAspect_2 的最终通知被调用");
    }
}
```

当我们调用了 smartDog 代理的 sum 方法后，在控制台打印输出的结果为：

```
SmartAnimalAspect_1 的前置通知被调用
SmartAnimalAspect_2 的前置通知被调用
SmartAnimalAspect_2 的返回通知被调用
SmartAnimalAspect_2 的最终通知被调用
SmartAnimalAspect_1 的返回通知被调用
SmartAnimalAspect_1 的最终通知被调用
```

:herb:这里引入一下：其实 AOP 类中还可以通过 Order 注释配置优先级：

```java
public @interface Order {
    int value() default Integer.MAX_VALUE;	// 数值越大，优先级越小，默认是最小的，jdk1.8 中为 2^31-1
}
```

如果我们将 SmartAnimalAspect_2 的 Order 的 value 属性设置为 1，如下：

```java
@Aspect
@Order(value = 1)
@Component
public class SmartAnimalAspect_2 {}
```

执行顺序为：

```
SmartAnimalAspect_2 的前置通知被调用
SmartAnimalAspect_1 的前置通知被调用
SmartAnimalAspect_1 的返回通知被调用
SmartAnimalAspect_1 的最终通知被调用
SmartAnimalAspect_2 的返回通知被调用
SmartAnimalAspect_2 的最终通知被调用
```

这里我们不过多介绍，调用顺序与 filter 的过滤链式调用机制十分像，优先级高的，其前置通知先执行，对应的其他通知最后执行，如下图：

![image-20230401220321481](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230401220321481.png)



## 4.12 基于 xml 配置的 AOP

我们在前面都是基于注解来配置 AOP，接下来，我们希望能够通过 xml 的方式来配置 AOP

AOP 类如下：

```java
public class SmartAnimalAspect {

    public void showBeginLog(JoinPoint joinPoint) {
        System.out.println("SmartAnimalAspect 的前置通知被调用");
    }

    public void showSuccessEndLog(JoinPoint joinPoint, Object res) {
        System.out.println("SmartAnimalAspect 的返回通知被调用");
    }

    public void showExceptionLog(JoinPoint joinPoint, Throwable throwable) {
        System.out.println("SmartAnimalAspect 的异常通知被调用");
    }
    public void showFinallyEndLog(JoinPoint joinPoint) {
        System.out.println("SmartAnimalAspect 的最终通知被调用");
    }
}
```

> 去掉了所有的注解

**xml 配置文件**

```xml
<!--配置AOP类-->
<bean class="com.xml.SmartAnimalAspect" id="smartAnimalAspect"/>
<!--配置切入类-->
<bean class="com.xml.SmartDog" id="smartDog"/>
<!--
    统一配置切入点
    注意：一定要引入 aop 命名空间
-->
<aop:config>
    <!--配置切入点-->
    <aop:pointcut expression="execution(public float com.xml.SmartDog.sum(float, float))" id="myPointcut"/>
    <!--
        引入AOP类, order 决定AOP类的执行顺序
    -->
    <aop:aspect ref="smartAnimalAspect" order="1">
        <!--配置横切关注点（即切入通知）-->
        <!--1. 配置前置通知-->
        <aop:before method="showBeginLog" pointcut-ref="myPointcut"/>
        <!--2. 配置返回通知-->
        <aop:after-returning method="showSuccessEndLog" pointcut-ref="myPointcut" returning="res"/>
        <!--3. 配置异常通知-->
        <aop:after-throwing method="showExceptionLog" pointcut-ref="myPointcut" throwing="throwable"/>
        <!--4. 配置最终通知-->
        <aop:after method="showFinallyEndLog" pointcut-ref="myPointcut"/>
    </aop:aspect>
</aop:config>
```

**测试程序**

```java
ApplicationContext ioc = new ClassPathXmlApplicationContext("beans07.xml");
SmartAnimal smartAnimal = ioc.getBean(com.xml.SmartAnimal.class);
smartAnimal.sum(1, 2);
```





# 5 实现 spring 底层机制

## 5.1 提出

### 5.1.1 抛出问题1

在手动实现 spring 的底层机制之前，我们需要新建一个 maven 项目，目录结构如下:

```
spring-second                             
├─ src                                    
│  ├─ main                                
│  │  ├─ java                             
│  │  │  ├─ com                           
│  │  │  │  └─ xzh                        
│  │  │  │     └─ spring                  
│  │  │  │        └─ component            
│  │  │  │           ├─ UserAction.java   
│  │  │  │           ├─ UserDao.java      
│  │  │  │           └─ UserService.java  
│  │  │  └─ AppMain.java                  
│  │  └─ resources                        
│  │     └─ beans.xml                     
│  └─ test                                
│     └─ java                                                
└─ pom.xml                                
```

>注意：对于普通 java 项目，beans.xml 文件放在 src 目录下，而对于maven项目，beans.xml 文件放在 resources 目录下

在 pom.xml 文件中，我们填入如下依赖：

```xml
<!--加入spring开发的基本包-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.3.7</version>
</dependency>
<!--加入spring切面编程需要的包-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aspects</artifactId>
    <version>5.3.8</version>
</dependency>
```

```java
 // UserAction.java
@Component
public class UserAction {
}
```

```java
// AppMain
public class AppMain {
    public static void main(String[] args) {
        ApplicationContext ioc = new ClassPathXmlApplicationContext("beans.xml");
        UserAction userAction1 = (UserAction)ioc.getBean("userAction");
        UserAction userAction2 = (UserAction)ioc.getBean("userAction");
        System.out.println("UserAction1 = " + userAction1);
        System.out.println("UserAction2 = " + userAction2);
    }
}
```

**xml 配置文件**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
       <!--配置自动扫描的包，同时引入对应的名称空间-->
    <context:component-scan base-package="com.xzh.spring.component" />
</beans>
```



代码说明：现在我们的主要任务是测试装载到 spring 容器中的 bean 是单例的还是多例的，代码运行结果如下：

```
UserAction1 = com.xzh.spring.component.UserAction@45afc369
UserAction2 = com.xzh.spring.component.UserAction@45afc369
```

结论：**在 spring 中装载的 bean 默认是单例的**

如果我们想要让 bean 成为多例的，只需要在类的前面添加注解：`@Scope(value = "prototype")` 注解即可

生成问题：

1. spring 底层是如何实现将 bean 自动组装到另一个 bean 中？
2. spring 底层是如何完成多例和单例的转换？

最后还有我自己的一个疑问：**配置类如果添加了 `@Scope(value = "prototype")` 注解貌似就无法被后置处理器捕获？**

估计是由于配置了多例的 bean，只有在运行时阶段才会被捕获，也就是除非我们手动从容器中获取该多例类，否则在编译阶段时无法被捕获的



### 5.1.2 抛出问题2

上一节我们提出了两个问题，这一节我们将围绕后置处理器 BeanPostProcessor 提出第三个问题，并在后面一一解决：

01 首先，创建一个 BeanPostProcessor 的实现，并重写里面的两个方法：

```java
public class MyBeanPostProcessor implements BeanPostProcessor {
    /**
     * 在 bean 的 init 初始化方法前调用
     * @param bean
     * @param beanName
     * @return
     * @throws BeansException
     */
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("postProcessBeforeInitialization 被调用, 其中 bean = " + bean.getClass() + ", beanName = " + beanName);
        return bean;
    }

    /**
     * 在 bean 的初始化方法后 调用
     * @param bean
     * @param beanName
     * @return
     * @throws BeansException
     */
    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("postProcessAfterInitialization 被调用, 其中 bean = " + bean.getClass() + ", beanName = " + beanName);
        return bean;
    }
}
```

> 通过 `getClass()` 方法可以得到一个实例的运行时类型

02 配置后置处理器

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
       <!--配置自动扫描的包，同时引入对应的名称空间-->
    <context:component-scan base-package="com.xzh.spring.component" />
    <bean class="com.xzh.spring.process.MyBeanPostProcessor" id="myBeanPostProcessor" />
</beans>
```

除了使用 xml 文件配置后置处理器，我们还可以使用注解的方式，即使用如 `@Component` 的注解自动将处理器注入到容器中

> 这里需要注意：一定不要忘记将后置处理器添加到扫描路径中，否则注入不生效



03 最后配置 init 方法

```java
@Component
public class UserService {
    @Autowired
    private UserDao userDao;

    public void m1() {
        userDao.hi();
    }

    /**
     * 添加 @PostConstruct 注解指明该方法为 init 方法
     */
    @PostConstruct
    public void init() {
        System.out.println("UserService init 方法被调用");
    }
}
```

此时，当我们在获取到 spring 容器之后调用 UserService 的初始化方法时，会在后置处理器中前置方法和后置方法

那么，再次抛出下面的问题：

- spring 是如何实现后置处理器？



### 5.1.3 抛出问题3：探讨 beanPostProcessor 和 AOP 的关系

我们在 spring 包下新建一个 aop 包，目录结构如下：

```
aop                        
├─ SmartAnimal.java         -- 接口类
├─ SmartAnimalAspect.java  	-- 切面类
└─ SmartDog.java            -- 实现类
```

```java
// SmartAnimal 接口类
public interface SmartAnimal {
    /**
     *
     * smartAnimal 中加法运算
     * @param a
     * @param b
     * @return
     */
    float sum(float a, float b);

    /**
     * smartAnimal 中的减法运算
     * @param a
     * @param b
     * @return
     */
    float sub(float a, float b);
}
```

```java
// SmartDog 是 SmartAnimal 的实现类
@Component
public class SmartDog implements SmartAnimal {
    @Override
    public float sum(float a, float b) {
        float res = a + b;
        System.out.println("smartDog 中的 a + b 的值为：" + res);
        return res;
    }

    @Override
    public float sub(float a, float b) {
        float res = a - b;
        System.out.println("smartDog 中的 a - b 的值为：" + res);
        return res;
    }
}
```

```java
// 切面类
@Component
@Aspect
public class SmartAnimalAspect {
    @Pointcut(value = "execution(public float com.xzh.spring.aop.SmartDog.*(float, float))")
    public void myPointcut() {}

    /**
     * 前置通知
     */
    @Before(value = "myPointcut()")
    public void showBeginLog(JoinPoint joinPoint) {
        System.out.println("前置通知，" + "调用方法为：" + joinPoint.getSignature().getName() + ", 方法的参数为：" + Arrays.asList(joinPoint.getArgs()));
    }

    /**
     * 返回通知
     */
    @AfterReturning(value = "myPointcut()")
    public void showSuccessEndLog(JoinPoint joinPoint) {
        System.out.println("返回通知，" + "调用方法为：" + joinPoint.getSignature().getName() + ", 方法的参数为：" + Arrays.asList(joinPoint.getArgs()));
    }

    /**
     * 最终通知
     */
    @After(value = "myPointcut()")
    public void finalEndLog(JoinPoint joinPoint) {
        System.out.println("最终通知，" + "调用方法为：" + joinPoint.getSignature().getName() + ", 方法的参数为：" + Arrays.asList(joinPoint.getArgs()));
    }
}
```

**测试程序**

```java
ApplicationContext ioc = new ClassPathXmlApplicationContext("beans.xml");
SmartAnimal smartDog = ioc.getBean(SmartAnimal.class);
smartDog.sum(1, 2);
```

**执行结果**

```
postProcessBeforeInitialization 被调用, 其中 bean = class com.xzh.spring.aop.SmartAnimalAspect, beanName = smartAnimalAspect
postProcessAfterInitialization 被调用, 其中 bean = class com.xzh.spring.aop.SmartAnimalAspect, beanName = smartAnimalAspect
postProcessBeforeInitialization 被调用, 其中 bean = class com.xzh.spring.aop.SmartDog, beanName = smartDog
postProcessAfterInitialization 被调用, 其中 bean = class com.sun.proxy.$Proxy14, beanName = smartDog
前置通知，调用方法为：sum, 方法的参数为：[1.0, 2.0]
smartDog 中的 a + b 的值为：3.0
返回通知，调用方法为：sum, 方法的参数为：[1.0, 2.0]
最终通知，调用方法为：sum, 方法的参数为：[1.0, 2.0]
```

现在，我们抛出两个问题：

1. 后置处理器与 AOP 之间的关系如何？
2. 我们为什么在拿被切面的类的时候，拿的是其接口类，而不是真正的实现类？
3. 我们会发现被切面捕获的类在初始化后会变成为一个



### 5.1.4 简述 Spring AOP 与 BeanPost

- AOP 底层的实现就依靠于 BeanPostProcessor 机制
- Bean 在创建好后，根据是否需要 AOP 处理，决定返回的是代理对象
- <u>在返回代理对象时，就可以根据需要代理的类和方法来返回</u>
- AOP 的底层机制本质上就是 BeanPostProcessor + 动态代理



## 5.2 spring 整体架构分析

![image-20230728140214263](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230728140214263.png)



## 5.3 对类加载器和 classPath 的再说明

**三种类加载器**

- Bootstrap 类加载器：对应路径 `jre/lib`

- Ext 类加载器：对应路径 `jre/lib/ext`

- App 类加载器：对应路径 `classpath`

  > App 类加载器也是我们最长使用的类加载器

更详细的解释：

- **Bootstrap ClassLoader** 最顶层的加载类，主要加载核心类库，%JRE_HOME%\lib下的rt.jar、resources.jar、charsets.jar和class等。另外需要注意的是可以通过启动jvm时指定-Xbootclasspath和路径来改变Bootstrap ClassLoader的加载目录。比如`java -Xbootclasspath/a:path`被指定的文件追加到默认的bootstrap路径中。我们可以打开我的电脑，在上面的目录下查看，看看这些jar包是不是存在于这个目录。
- **Extention ClassLoader** 扩展的类加载器，加载目录%JRE_HOME%\lib\ext目录下的jar包和class文件。还可以加载`-D java.ext.dirs`选项指定的目录。
- **Appclass Loader也称为SystemAppClass** 加载当前应用的classpath的所有类。

加载顺序

1. Bootstrap CLassloder
2. Extention ClassLoader
3. AppClassLoader



classPath 类路径，其实就是 java.exe 执行时，指定的路径，我们可以从 idea 控制台中截取查看

![image-20230728150204574](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230728150204574.png)

![image-20230728150312528](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230728150312528.png)

从上面两张图，我们可以看到所谓的类路径其实就是指 idea 中 target 文件夹下的 classes 部分，程序在运行时也是加载的这部分代码，所以如果有时我们直接从别人的项目中拷贝文件到我们自己 src 文件夹下有时候需要 rebuild 重构项目，否则类路径中无法检测到我们新移入的文件



## 5.4 实现 spring 注解配置 bean 机制

![image-20230728153046474](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230728153046474.png)
