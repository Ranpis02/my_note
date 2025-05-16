pom 的全称：Project Object Model（项目对象模型）

作用：用于管理源代码、配置文件、开发者的信息和角色、问题追踪系统、组织信息、项目授权、项目的url、项目的依赖关系等等

pom 之前的关系：

- 继承：子项目继承父项目的 pom.xml，在子项目中定义父项目
- 依赖：一个dependency元素定义一个依赖关系
- 聚合：聚合和关联多个项目中相同的配置，在被聚合项目中定义其子模块以使用 `${propertyName}` 的形式引用属性

pom.xml 中各文件的作用

### 01 modelVersion

```xml
<modelVersion>4.0.0</modelVersion>
```

>指定了当前Maven模型的版本号，对于Maven2和Maven3来说，它只能是4.0.0，pom必须遵守一个模型，它总是设置为4.0.0在Maven 2和3，因为，目前，没有其他模型



### 02 依赖相关

```xml
<groupId>com.example</groupId>
```

>是项目组织唯一的标识符，实际对应JAVA的包的结构，

```xml
 <artifactId>demo</artifactId>
```

>是项目的唯一的标识符，实际对应项目的名称，就是项目根目录的名称

```xml
<version>0.0.1-SNAPSHOT</version>
```

>指定了项目的当前版本，SNAPSHOT意为快照，说明该项目还处于开发中，是不稳定的版本

除了上面的三个主要标签，还有下面的标签

- scope：用于指定依赖的作用范围

  | scope          | 主代码 | 测试代码 | 打包 | 示例        |
  | -------------- | ------ | -------- | ---- | ----------- |
  | complier(默认) | Y      | Y        | Y    | log4j       |
  | test           |        | Y        |      | junit       |
  | provided       | Y      | Y        |      | servlet-api |
  | runtime        |        |          | Y    | jdbc        |

- 



### 03 parent

```xml
<parent>
</parent>
```

> 



### 04 properties

```
<properties>
    <!--指定当前的 jdk 版本-->
    <java.version>1.8</java.version>
    <!--在maven中设置 java 版本，通常与jdk版本保持一致-->
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
    <!--设置项目工程的编码-->
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
</properties>
```

> 该标签用于配置版本号，通常与依赖中的 version 标签配合使用，锁定依赖的版本



### 05 dependencyManagement

```xml
<dependencyManagement>
    <dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-dependencies</artifactId>
        <version>2.2.2.RELEASE</version>
        <!--type::pom 和 scope::import 配合使用解决 maven 单继承机制的问题-->
        <type>pom</type>
        <scope>import</scope> </dependency>
    </dependencies>
</dependencyManagement>
```

> 该标签用于提供一种管理依赖版本号的方式，在父项目中声明所依赖的 jar 包的版本号信息，那么在所有子项目中再次依赖该 jar 时无需显示列出版本号。Maven会沿着父子层级向上寻找拥有dependencyManagement 元素的项目，然后使用它指定的版本号

:warning:**注意**

<strong style="color:red">dependencyManagement 标签本身只是指定版本，而不引入相关依赖</strong>



### 06 其他标签

- name: 开发们常用的简称 
- description: 描述 
- url: 链接 
- inceptionYear: 成立年份