# 1 Maven 简介

## 1.1 Maven 的概念与作用

传统项目管理状态分析：

- jar 包不统一，jar 包不兼容
- 工程维护过程中操作繁琐
- …



**Maven 是什么？**

- Maven 的本质是一个==项目管理工具==，将项目开发和管理过程抽象成一个项目对象模型（POM）
- POM（Project Object Model）：项目对象模型（存放在一个 POM.xml 的文件中）

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221030230356897.png" alt="image-20221030230356897" style="zoom:50%;" />

- 统一开发结构：提供标准的、统一的项目标准

  <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221030230600981.png" alt="image-20221030230600981" style="zoom:50%;" />



**使用 Maven 的流程**

- 项目构建
- 依赖管理

- 信息管理



## 1.2 Maven 的下载与安装

我们在下载 Maven 前需要先检查 JDK 的安装（ Maven 3 需要在 JDK 1.7 版本及以上）

1. 进入 Maven  的[官网目录](https://maven.apache.org/download.cgi)，进行下载相关的安装文件

   <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221030232513872.png" alt="image-20221030232513872" style="zoom:50%;" />

2. 将框中第二个文件（即：-bin.zip 文件）解压到本地目录中

   <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221030233232884.png" alt="image-20221030233232884" style="zoom:50%;" />

3. Maven 属于绿色版软件，解压即安装，解压后一共产生 4 个目录
   - bin：核心的可执行文件
   - boot：加载器
   - conf：核心配置
   - lib：依赖的 jar 包

4. Maven 环境变量的配置

   1. 打开环境编辑系统环境变量

   2. 在系统变量中选择新建系统变量，将 Maven 的解压路径添加到 MAVEN_HOME 环境变量的值中

      <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221030233747459.png" alt="image-20221030233747459" style="zoom:50%;" />

   3. 在 path 中添加 `%MAVEN_HOME%\bin` 即可
   
   4. 输入命令 `mvn -v` 检查配置是否成功，如果出现以下提示信息则表示安装成功
   
      <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221031075237510.png" alt="image-20221031075237510" style="zoom:50%;" />

:warning:配置环境报错！！！

如果配置环境后报错如下：

```
The JAVA_HOME environment variable is not defined correctly
```

<font color="red">那么请重新配置 JAVA_HOME 环境，或者直接写在 Java JDK 再重新配置即可</font>



## 1.3 镜像设置

**为什么要更换镜像？**

安装完 Maven 之后，默认是从国外 Maven 中央仓库下载内容，下载速度极慢，国内阿里巴巴为了方便国人使用方便，所以将中央仓库中所有内容复制一份 Maven  镜像放到阿里云中，加快下载速度



**配置阿里云镜像**

1. 找到 Maven 安装目录

   <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221030233232884.png" alt="image-20221030233232884" style="zoom:50%;" />

2. 在 conf 文件夹中找到 settings.xml

   <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221031080614017.png" alt="image-20221031080614017" style="zoom:50%;" />

3. 对文件内容中的 mirrors  标签使用下面代码进行替换

   ```xml
   <mirror>
     <!-- 此镜像的唯一标识符，用来区分不同的 mirror 元素 -->
     <id>nexus-aliyun</id>
     <!-- 替代中央仓库 -->
     <mirrorOf>central</mirrorOf>
     <!-- 镜像名称 -->
     <name>Nexus aiyun</name>
     <!-- 镜像 url -->
     <url>http://maven.aliyun.com/nexus/content/groups/public</url> 
   </mirror>
   ```




**Eclipse和IDEA中如何查找 settings.xml 文件路径**

可能 Eclipse 或 IDEA 工具本身已集成安装了 Maven，那么可能找不到 settings.xml 文件，像 IDEA 默认使用的就是自己的 Maven 插件

**IDEA**

打开 IDEA，然后 File => Settings，搜索框输入maven，在Build Tools中选中 Maven，如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221031085118562.png" alt="image-20221031085118562" style="zoom:50%;" />

> 注意：如果从 settings.xml 文件路径中没找到这个配置文件，别担心，从网上下载一个 maven 安装包，解压后从里面拷贝一个 setting.xml 文件过来即可。



# 2 Maven 的基本概念 — 仓库

仓库：<u>用于存储资源，包含各种 jar 包</u>

![Snipaste_2023-03-10_15-06-05](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/Snipaste_2023-03-10_15-06-05.png)

仓库分类如上：

- 本地仓库：自己本机上存储资源的仓库，连接远程仓库获取资源

- 远程仓库：非本机上的仓库，为本地仓库提供资源

  - 中央仓库：Maven 团队维护，存储所有资源的仓库
  - 私服：部门 / 公司范围内存储资源的仓库，从中央仓库获取资源

- 私服的作用：

  - 保存具有版权的资源，包含购买或者自主研发的 jar

    > 中央仓库中的 jar 都是开源的，不能存储具有版权的资源

  - 一定范围内共享资源，仅对内部开放，不对外共享
  
    > 一旦对外开发，那么其他用户就可以根据 groupId 和 artifactId 来进行定位
  



## 2.1坐标

:question:什么是坐标

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221112192258074.png" alt="image-20221112192258074" style="zoom:33%;" />

- Maven 中的坐标用于描述仓库中资源的位置，maven 仓库：[传送入口](https://repo1.maven.org/maven2/)
- Maven 中的坐标主要组成
  - groupId：定义当前 Maven 项目隶属组织名称（通常是域名反写 + 公司名）
  - artifactId：定义当前的 Maven 项目名称（通常是模块名称，例如：CRM、SMS）
  - version：定义当前项目版本号（可省，默认是最新版本）
  - scope:    表示作用域，也就是你引入的  jar  包的作用范围
    - provided：在编译，测试使用，但是在打包发布就不会带上
    - runtime：不作用在编译，但作用在运行和测试
  - <font color="grey">packaging：定义该项目的打包方式，用于区分生产环境和开发环境</font>
  - <font color="grey">classifiter：构建输出的一些附属文件</font>



我们进入 Maven 仓库官网：https://mvnrepository.com/

我们拿 JUnit 包进行举例：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221112111423442.png" alt="image-20221112111423442" style="zoom:33%;" />

可以看到里面的组成，我们使用坐标，唯一定位资源位置，通过该标识可以将资源的识别与下载操作交由机器完成，下载完成后，在仓库中 groupId + artifactId + version 就形成了该 jar 包的文件存放位置

**推荐查找 jar 包坐标的网址：**

```
http://mvnrepository.com/
http://search.maven.org/
```



## 2.2 仓库配置

（我们在命令行中输入 `mvn`，Maven 就会在当前工作目录中创建一个 .m2 的文件，里面存放着 Maven 仓库和管理文件，为了更好的管理，我们需要自己手动配置本地仓库）？

Maven 启动后，会自动保存下载的资源到本地仓库，我们 Maven 的安装路径，conf --> settings.xml，打开后手动配置

- 默认位置

  ```xml
  <localRepository> ${user.home}/.m2/repository</localRepository>
  ```

- 自定义位置

  ```xml
  <localRepository>D:\maven\repository</localRepository>
  ```
  
  > 在 IDEA 中，当我们引入别人的项目时，会默认使用 IDEA 自带的 setting.xml 配置，为了使得能够使用自己的本地仓库，我们需要修改其 maven 仓库位置

由于中央仓库采用的是国外的服务器，下载速度慢，所以我们需要进行镜像仓库配置

- 在 setting.xml 文件中配置阿里云镜像仓库

  ```xml
  <mirror>
    <!-- 此镜像的唯一标识符，用来区分不同的 mirror 元素 -->
    <id>nexus-aliyun</id>
    <!-- 替代中央仓库 -->
    <mirrorOf>central</mirrorOf>
    <!-- 镜像名称 -->
    <name>Nexus aiyun</name>
    <!-- 镜像 url -->
    <url>http://maven.aliyun.com/nexus/content/groups/public</url> 
  </mirror>
  ```

  该配置放在 `mirrors` 标签中

- 全局 setting 与用户 setting 区别
  - 全局 setting 是全局有效
  - 用户 setting 是在当前用户或当前工程有效，具体配置就是在自己设置的 repository 同级建立一个 setting.xml ，在该文件中进行具体配置

# 3 Maven 工程

## 3.1 Maven 工程结构

Maven 的工程结构如下：



<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221112191639998.png" alt="image-20221112191639998" style="zoom: 25%;" />

## 3.2 Maven 项目构建命令

Maven  构建命令使用 mvn 开头，后面添加功能参数，可以一次执行多条命令，使用空格分隔

```shell
mvn compile				# 编译
mvn clean				# 清理
mvn test				# 测试
mvn package				# 打包
mvn install 			# 安装到本地仓库
```



## 3.3 IDEA 中创建 Maven 工程

配置 Maven

- <u>IDEA 对 3.6.2 及以上版本存在兼容性问题，为了避免冲突，IDEA 中安装使用 3.6.1 版本</u>

我们到 Maven 的[官网](https://maven.apache.org/download.cgi)中去下载 Maven 3.6.1 版本

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221112162549394.png" alt="image-20221112162549394" style="zoom:50%;" />

选择 3.6.1 版本

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221112162637691.png" alt="image-20221112162637691" style="zoom:50%;" />

进入到 binaries 文件夹中

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221112162748227.png" alt="image-20221112162748227" style="zoom:50%;" />

window 系统中选择 .zip 结尾的压缩包

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221112162904334.png" alt="image-20221112162904334" style="zoom:50%;" />

下载完成之后解压到安装路径中，这里要注意一件事，就是我们需要先卸载之前安装的 Maven，同时删除之前配置的环境变量再进行重新配置

上述操作完成之后，接下来来对 IDEA 中 Maven 进行配置

在【设置】中搜索【Maven】，找到如下设置选项进行配置

![image-20230310170533529](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230310170533529.png)



**新建一个带有 Maven 管理的 Java 工程项目**

接下来让我们新建一个工程，再在工程里建立模块，按如下配置操作

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230310172110138.png" alt="image-20230310172110138" style="zoom:50%;" />

我们现在去 Maven 仓库的官网上去下载一个 JUnit 依赖来进行测试，在 pom.xml 中添加 `dependency` 标签，将该标签放入 `dependencies` 标签中 （如果没有该标签，就自己添加）

```xml
<dependencies>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.13.1</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

接下来，我们如果想要进行加载，可以选择手动加载或自动加载，默认是手动加载，如下：

![image-20230310171248764](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230310171248764.png)

IDEA 2022 取消了自动加载，我们如果想要配置自动加载，可以手动修改配置文件，下载好的 jar 包存放在 external library 中

![image-20230310172014251](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230310172014251.png)



接下来，我们来解决 IDEA 2022 中，<strong style="color:red">每次新建项目，仓库配置回到默认的问题</strong>

1. 在 System Settings 中关闭 【Reopen projects on startup】

   ![image-20230310175410028](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230310175410028.png)

2. 退出 IDEA，重新打开，会出现下图界面

   ![image-20230310175535549](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230310175535549.png)

3. 再次修改 Maven 仓库配置即可

   ![image-20230310175608086](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230310175608086.png)

