[toc]

# 1 Spring 基础

## 1.1 初识 Spring

> Spring 官网：spring.io

企业级开发必备的 spring 全家桶中的三个技术栈：

- Spring Framework
- Spring Boot
- Sping Cloud





# 2 Spring Framework

## 2.1 Spring Framework 系统架构

- Spring Framework 是 Spring 生态圈中最基础的项目，是其他项目的根基

![image-20230712190352468](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230712190352468.png)

**核心架构**

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230712191959584.png" alt="image-20230712191959584" style="zoom:50%;" />

- AOP：面向切面编程
- Aspects：AOP 思想实现
- Core Container：核心容器
- Data Access：数据访问
- Data Intergration：数据集成
- Web：web 开发
- Test：单元测试与集成测试



## 2.2 Spring 核心概念

- IOC：Inversion of Control，控制反转，对象的创建控制权由程序转到外部

  例如下面的代码，如果我们在 DAO 层创建了一个新的类，要求业务层转变使用该层的类去访问数据库，那么如果依赖改 DAO 层的 Service 类过多，那么显然是比较麻烦的，所以提出 IOC 的概念，将对象的创建过程转移到外部

  ![image-20230712194127987](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230712194127987.png)

- Spring 技术对 IOC 思想进行实现

  - Spring 提供了一个容器，称为 IOC 容器，用来充当 IOC 思想中的 "外部"

- 



