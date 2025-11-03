# 1 The Basics

## 1.1 Overall Architecture

![image-20250507145614544](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250507145614544.png)



The version of dependency in project:

| Name                 | Version    |
| -------------------- | ---------- |
| Java                 | 8          |
| Spring Boot          | 2.6.13     |
| Spring Cloud         | 2021.0.5   |
| Spring Cloud Alibaba | 2021.0.5.0 |
| MySQL                | 8.4.0      |
| MyBatis-Plus         | 3.5.1      |



## 1.2 E-commerce Model

1. B2B Model

   B2B(Business to Business), 是指商家与商家之间建立的商业关系，代表平台：阿里巴巴

2. B2C Model

   B2C(Business to Customer), 即商家直接将商品卖给顾客，例如京东、天猫

3. C2B Model

   C2B(Customer to Business), 即消费者先提出需求，后有生产企业按照需求组织生产

4. C2C Model

   C2C(Customer to Customer), 客户把自己的东西放在平台上进行售卖，例如：淘宝、闲鱼

5. O2O Model

   O2O(Online to Offline), 即将线下商务和互联网结合起来，让互联网成为线下交易的前台，例如：饿了么，美团 





## 1.3 Concept of Microservice

**Technical Term**

- *Microservice*: an architectural approach to developing software applications as a collection of small, independent services that communicate with each other over a network.
- *Cluster*: a group of independent computers that works together closely, often appearing as a single more powerful system to the outside world.
- *Distributed System*: a system whose components are located on different networked computers, which then communicate and coordinate their actions by passing messages to one another.
- *Node*: a fundamental unit or a single machine within a large network, cluster, or distributed system.
- *RPC(Remote Procedure Call)*: a protocol that allows a program to execute a function or procedure located on another machine(or service), as if it were a local function.
- *Load Balance*: a technique used in computing to distribute work evenly across multiple resources.



# 2 Environment Configuration

## 2.1 VirtualBox + Vagrant

01 Initialize the VM 

```bash
vagrant init ubuntu/jammy64
```

> [!note]
>
> Run this command will generate a `Vagrantfile`
>
> For Detailed information, please refer to the [official document](https://docs.docker.com/desktop/setup/install/linux/ubuntu/). 
>
> > [!warning]
> >
> > In the latest document, `centos` is no longer supported. Therefore, `ubuntu22.04` is as an alternative.



02 Run the VM

```bash
vagrant up
```

> [!warning]
>
> Before starting the VM, ensure that the terminal is in the specified directory. 



03 Connect the VM

```
vagrant ssh
```

> [!warning]
>
> Before connecting the VM, ensure that the terminal is in the specified directory. 



04 Shut down the VM

```bash
vagrant halt [ID]
```



:question:How to navigate to the user home directory?

```bash
cd %USERPROFILE%
```



Check status of VM defined in current directory

```bash
vagrant status
```



Check status of all VMs

```bash
vagrant global-status
```



:question:How to remove or destroy the VM?

1. Use `vagrant global-status` to find VM ID that needs to be destroyed 

2. Use the below command to destroy the specified VM ID

   ```bash
   vagrant destroy ID
   ```



## 2.2 VM network configuration

To change the VM' IP address, modify the `config.vm.network "private_network"` directive in the `Vagrantfile`, such as:

```
config.vm.network "private_network", ip: "192.168.56.10"
```

After modifying the configuration, the VM needs to be reloaded with following directive.

```bash
vagrant reload
```

Then, run `vagrant ssh` to access the VM and use `ip addr` to check its IP address.

Finally, use the `ping` command to test the connectivity in both directions between the host and the guest.

> [!note]
>
> It is worth noting that the command used to check the IP address on Windows is `ipconfig`



## 2.3 Install and configure Docker

> [!tip]
>
> Ref to https://docs.docker.com/desktop/setup/install/linux/ubuntu/



:question:How to enable Docker to start automatically on boot?

```bash
sudo /lib/systemd/systemd-sysv-install enable docker
```



Configure Alibaba Cloud image acceleration for Docker?

Access [aliyun.com](https://www.aliyun.com/)， select `Container Registry`, and follow the guide to configure the registry mirror.



### 2.3.1 Configure MySQL

1. Pull the MySQL

   ```shell
   $ sudo docker pull mysql:8.4
   ```

2. Create the MySQL instance, map MySQL's port to the host and mount the MySQL's files to host

   ```shell
   $ sudo docker run -p 3306:3306 --name mysql \
   -v /mydata/mysql/log:/var/log/mysql \
   -v /mydata/mysql/data:/var/lib/mysql \
   -v /mydata/mysql/conf:/etc/mysql/conf.d \
   -e MYSQL_ROOT_PASSWORD=root \
   -d mysql:8.4
   ```

   > [!note]
   >
   > The option `-e` specifies the password of MySQL

3. Configure automatic setup

   ```shell
   $ sudo docker update --restart=always mysql
   ```





> [!tip]
>
> The way to switch to root user in a Vagrant-created Linux system
>
> ```shell
> $ sudo -i
> ```
>
> , and the way to switch to normal user is as follow:
>
> ```shell
> $ exit
> ```



:question:How to enter the MySQL container in interactive mode?

```shell
$ docker exec -it ${contain_name} /bin/bash 
```



### 2.3.2 Configure Redis

1. Pull the Redis images

   ```shell
   $ sudo docker pull docker
   ```

2. Use the follow commands to run the Redis container

   1. Pre-create the Redis configuration file

      ```bash
      $ mkdir -p /mydata/redis/conf
      $ touch /mydata/redis/conf/redis.conf
      ```
   
   2. Create and run the Redis container
   
      ```bash
      $ docker run -p 6379:6379 --name redis -v /mydata/redis/data:/data \
      -v /mydata/redis/conf/redis.conf:/etc/redis/redis.conf \
      -d redis redis-server /etc/redis/redis.conf
      ```
   
   3. Configure automatic setup
   
      ```shell
      $ sudo docker update --restart=always redis	
      ```
   
   



Enter the Redis container in interactive mode

```shell
$ docker exec -it redis redis-cli
```

> [!tip]
>
> - `redis-cli`: this is the Redis command-line client used to interactive with Redis server inside the container



In the latest version of Redis, data persistence is enabled by default using `RDB` option.

> [!tip]
>
> For more detailed information on data persistence, ref to [Redis official documentation](https://redis.io/docs/latest/operate/oss_and_stack/management/persistence/)



## 2.4 Unified development environment

1. Maven configuration: set Java 1.8 as the compiler version. In conf -> setting.xml, add the following configuration:

   ```xml
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

2. Install Develop Tools: Visual Studio Code, IDEA, and some IDEA Plugins, i.e., Lombok, MyBatisX

3. Configure the Git

   - Set the username and email

     ```shell
     git config --global user.name ${name}
     git config --global user.email ${email}
     ```

   - Generate an SSH Key

     ```shell
     ssh-keygen -t rsa -C ${email}
     ```

   - Get the content of the key

     ```shell
     $ cd ~/.ssh
     $ cat id_rsa.pub
     ```

   - Add a new SSH key

     ![image-20250716175039032](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250716175039032.png)

   - Check whether the setup was successful

     ```shell
     $ ssh -T git@github.com
     ```




## 2.5 Deploy Database

> [!tip]
>
> Common dataset character sets:
>
> 1. `utf8`: the maximum character length is 3 bytes and it supports basic multilingual characters but has limited support for complex characters
> 2. `utf8mb4`(recommended): `utf8mb4` stands for UTF-8 Multi-Byte 4, which supports the full UTF-8 encoding, including 4-byte characters.



The databases that need to be created include `gulimall_admin`, `gulimall_oms`, `gulimall_pms`, `gulimall_sms`, `gulimall_ums`, `gulimall_wms`, which mean respectively:

- `gulimall_admin`: Stores **administrative user data** and system configuration.
- `gulimall_oms`:  Handles **order management data**.
- `gulimall_pms`: Manage **product-related data**.
- `gulimall_sms`: **Stores sales and marketing service** data.
- `gulimall_ums`: contains **user membership data**.
- `gulimall_wms`: Manages **warehouse data**.



## 2.6 Deploy Renren-Admin System

> Detailed informat Refer: [renrenio repository](https://github.com/renrenio/)

Use the [IntelliJ IDEA](https://www.jetbrains.com/idea/) to create basic structure of project and take the creation of the `gulimall-product` as an example:

<img src="https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250812223510437.png" alt="image-20250812223510437" style="zoom:50%;" />

> [!tip]
>
> IF the ==Java 8== is unable to select, replace the Server URL from `https://start.spring.io/` to `https://start.aliyun.com/`  



The main modules need to be created include `gulimall_oms`, `gulimall_pms`, `gulimall_sms`, `gulimall_ums`, `gulimall_wms`, and all these modules have the following in common:

1. Set the group name to `com.atguigu.gulimall`
2. Set the Java version to 8 for all modules
3. Add dependencies for `Spring Web` and `OpenFeign`



Next, Integrate the various modules in the Maven project:

1. Copy the `pom.xml` from any module to the project root
2. Remove all configuration elements from the `pom.xml` except for `<modelVersion>`, `<groupId>`, `<artifactId>`, `<version>`, `<name>`, and `<description>` and add the `<packaging>` for aggregation
3. Add `<module>` configuration element for managing the aggregation modules



For reference, the `pom.xml` in the project root can be written as follows:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.atguigu.gulimall</groupId>
    <artifactId>gulimall</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>gulimall-ware</name>
    <description>GuliMall parent project for aggregating microservice modules</description>
    <packaging>pom</packaging>

    <modules>
        <module>gulimall-coupon</module>
        <module>gulimall-member</module>
        <module>gulimall-order</module>
        <module>gulimall-product</module>
        <module>gulimall-ware</module>
    </modules>
</project>
```



And then, add `.gitignore` file in the project root to exclude unnecessary files from version control:

```txt
.idea
**/target/
```

  

Next, clone [renren-fast](https://github.com/renrenio/renren-fast) and [renren-fast-vue](https://github.com/renrenio/renren-fast-vue) projects to root directory, then create `gulimall-admin` schema and execute the `mysql.sql` file provided by renren-fast to build the tables.

Next, modify the `spring.datasource.druid.url`, `spring.datasource.druid.username` and `spring.datasource.druid.password` in `application-dev.yml` to connect to the local database.

```yml
spring:
    datasource:
        druid:
            url: jdbc:mysql://192.168.56.10:3306/gulimall_admin?useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/Shanghai
            username: root
            password: root
```

Finally, run the `io.renren.RenrenApplication` main class to verify that the database configuration is right.



---



The next phase: Install Node.js LTS version 12 or 14, then run the `renrenfast-vue` project.



## 2.7 Reverse Engineering Setup

Integrate [renren-generator](https://github.com/renrenio/renren-generator) into the project, then modify the `application.yml` and `generator.properties` files to generate the corresponding Mapper, Controller, DAO, Entity, and Service code for the target database.

In the following, `gulimall_pms` will be used as an example:

```yaml
# application.yml
spring:
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    driverClassName: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://192.168.56.:3306/gulimall_pms?useUnicode=true&characterEncoding=UTF-8&useSSL=false&serverTimezone=Asia/Shanghai
    username: root
    password: root
```

```properties
# generator.properties
mainPath=com.atguigu
#\u5305\u540D
package=com.atguigu.gulimall
moduleName=product
#\u4F5C\u8005
author=zhiHong
#Email
email=ranpis02@gmail.com
#\u8868\u524D\u7F00(\u7C7B\u540D\u4E0D\u4F1A\u5305\u542B\u8868\u524D\u7F00)
tablePrefix=pms_
```



After starting the `renren-generator` project, go to the frontend interface, select all the tables, and export the generated file.

Then, decompress the file and copy the entire `main` folder into the `src` directory of `gulimall_product`.



The same operation applies to other databases.



> [!note]
>
> It is worth noting that, in order to make the generated code usable, we need to migrate the required code from the common folder in the `renren-fast` project to the `gulimall-common` module.



## 2.8 Configure & test CRUD functionality

1. Add MySQL Driver dependency to the `gulimall-common` project

   ```xml
   <dependency>
       <groupId>com.mysql</groupId>
       <artifactId>mysql-connector-j</artifactId>
       <version>${mysql.version}</version>
   </dependency>
   ```

2. In `gulimall-project`(or other modules), use the annotation `@MapperScan` to automatically scan all Mapper interfaces in the specified package and register them as Spring beans.

3. Configure the mapper path, the `datasource`, and set the auto-increment primary key in the `application.yml` file of `gulimall-product`

   ```yaml
   server:
     port: 8080
   spring:
     datasource:
       username: root
       password: root
       url: jdbc:mysql://192.168.56.10:3306/gulimall_pms
       driver-class-name: com.mysql.jdbc.Driver
   mybatis-plus:
     mapper-locations: classpath*:/mapper/**/*.xml
     global-config:
       db-config:
         id-type: auto
   ```

   > [!note]
   >
   > :dart:Differences between `classpath` and `classpath*`
   >
   > - `classpath`: loads the first matching resource from classpath.
   > - `classpath*`: loads all matching resource across the entire classpath, including those in nested JARs or directories.



> [!tip]
>
> To ensure the configuration is correct, it's necessary to write test code.



# 3 Establishing a Distributed Environment

## 3.1 Introduction

The architecture diagram of distributed Environment i shown below:

![image-20250825222249023](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250825222249023.png) 

Spring Cloud Alibaba is adopted as the component suite for our distributed architecture. 

Next, I will explain how each component is utilized in the distributed system. With the integration of Spring Cloud Alibaba, our final technology stack is:

- `Nacos`: acts as both a service registry for service discovery and registration, and a configuration center for dynamic configuration management.
- `Ribbon`: acts as a load balancer.
- `OpenFeign`: acts as a declarative HTTP client for remote procedure call.
- `Sentinel`: provides a service fault-tolerance mechanism, including rate limiting, degradation, and circuit breaking
- `Gateway`: services as API gateway.
- `Sleuth`: provides call chain monitoring.
- `Seata`: an open source distributed transaction solution.



To unify the versions of all components from Spring Cloud Alibaba suite, the `<dependencyManagement>` is utilized to manage them.

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-alibaba-dependencies</artifactId>
            <version>2021.0.5.0</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```



> [!tip]
>
> What is difference between `<dependencyManagement>` and `<dependency>`?
>
> - `<dependency>`: directly adds a dependency to current module.
> - `<dependencyManagement>`: centralizes dependency version and scope management without automatically including the dependency.



To make it easy to manage multiple Spring Boot projects simultaneously, `Intellil IDEA` provides the `Run Dashboard` tool. The way to   open it is below:

1. From the top menu of `Intellij IDEA`, select `View` -> `Tool Windows` -> `Services`
2. Click "+" in the bottom panel, and choose `Spring Boot` to add a new configuration



## 3.2 Registry center - Nacos

Follow the steps below to use `Nacos`:

1. Download the corresponding version of [Nacos](https://github.com/alibaba/nacos/releases)

2. Extract the archive, navigate to the `bin` directory, and start `Nacos` using the command `startup.cmd -m standalone`

   > [!tip]
   >
   > After starting the `Nacos` service, we can access its console to perform service registration and configuration management.

3. Register the service in `Nacos` by configuring the `Nacos Server ` address

   ```yaml
   spring:
     cloud:
       nacos:
           discovery:
               server-addr: 192.168.56.1:8848
     application:
       name: gulimall-coupon
   ```

   > [!warning]
   >
   > 1. The property `spring.application.name` is required.
   > 2. The value of `spring.application.name` will be used to construct part of the `dataId` in `Nacos configuration management`. 

4. Use the Spring Cloud native annotation of `@EnableDiscoveryClient` to enable service discovery

   ```java
   @EnableDiscoveryClient
   @SpringBootApplication
   public class GulimallCouponApplication {
   
       public static void main(String[] args) {
           SpringApplication.run(GulimallCouponApplication.class, args);
       }
   
   }
   ```

5. Finally, after starting the `Nacos` and microservice, accessing the console, the service is listed in the `Service List`.



Next, repeat the same operation for the other services.



## 3.3 Remote procedure call - OpenFeign

`OpenFeign` is a declarative HTTP client that simplifies inter-service communication in microservices architecture.

Overall, `OpenFeign` wraps a service with an interface that we can use directly, without the need to manually concatenate URLs or perform other low-level operations.

Next, I will use a demo to show how to use it.

Scene: Query the discount information from the `Coupon` service in the `Member` service

1. Add the `OpenFeign` and `LoadBalancer` dependency

   ```xml
   <!--OpenFeign-->
   <dependency>
       <groupId>org.springframework.cloud</groupId>
       <artifactId>spring-cloud-starter-openfeign</artifactId>
   </dependency>
   
   <!--LoadBalancer-->
   <dependency>
       <groupId>org.springframework.cloud</groupId>
       <artifactId>spring-cloud-starter-loadbalancer</artifactId>
   </dependency>
   ```

2. Create a handler method in `Coupon` service 

   ```java
   @RequestMapping("/member/list")
   public R memberCouponsListDemo() {
       CouponEntity couponEntity = new CouponEntity();
       couponEntity.setCouponName("Get $10 off for every $100 spent");
   
       return R.ok().put("coupons", Collections.singletonList(couponEntity));
   }
   ```

3. Create an interface for invoking remote procedures in the `member` service

   ```java
   @FeignClient("gulimall-coupon")
   public interface CouponClient {
       @RequestMapping("/coupon/coupon/member/list")
       R memberCouponsListDemo();
   }
   ```

   > [!note]
   >
   > - The `Coupon` service provides the `/coupon/coupon/member/list` endpoint.
   > - The `Member` service can call this endpoint via `OpenFeign` to retrieve the list of coupons.
   >
   > And the function of `@OpenFeign` in `Spring Cloud OpenFeign` is to :
   >
   > 1. Declare a Feign Client: When placed on an interface, Spring automatically generates an implementation for that interface.
   > 2. Specifies the target service name. In this code, `gulimall-coupon` refers to the name of remote microservice registered in the service discovery list.

4. Add the `@EnableFeignClients` annotation in the startup class to specify the location of Feign clients.

   ```java
   @EnableFeignClients(basePackages = "com.atguigu.gulimall.member.feign")
   @EnableDiscoveryClient
   @SpringBootApplication
   public class GulimallMemberApplication {
   
       public static void main(String[] args) {
           SpringApplication.run(GulimallMemberApplication.class, args);
       }
   
   }
   ```

5. Test the feign client

   ```java
   @SpringBootTest
   public class FeignClientTest {
   
       @Autowired
       CouponClient couponClient;
   
       @Test
       void getCouponListByFeign() {
           R r = couponClient.getMemberCouponsListDemo();
           Object coupons = r.get("coupons");
           System.out.println(coupons);
   
       }
   }
   ```



## 3.4 Configuration Management - Nacos

> [!tip]
>
> official document: https://nacos.io/en/docs/latest/ecology/use-nacos-with-spring-cloud/

1. Add the `Nacos config` dependency

   ```xml
   <dependency>
       <groupId>com.alibaba.cloud</groupId>
       <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
   </dependency>
   ```

2. Configure the Nacos Server address, specify the application name and the configuration file path in `application.yml`

   ```yaml
   spring:
     cloud:
       nacos:
         config:
           server-addr: 192.168.56.1:8848
     application:
       name: gulimall-coupon
     config:
       import: optional:nacos:application.yml
   ```

3. Add a configuration file in the `ConfigManagement` of `Nacos`. In Nacos Spring Cloud, the format of `dataId` is as follows:

   ```txt
   ${prefix}-${spring.profiles.active}.${file-extension}
   ```

   - `prefix`: by default, its value is `application.name`, and we can configure this value in `spring.cloud.nacos.config.prefix`

   - `spring.profile.active`: the profile of current environment.

     > [!warning]
     >
     > If the value if empty, the hyphen will be deleted and the `dataId` will be `${prefix}.${file-extension}`

   - `file-extension`: only `properties` and `yaml` type is supported.

4. Add the native `@RefreshScope` annotation of Spring Cloud to enable automatic fresh of configuration updates.

   ```java
   @RefreshScope
   @EnableDiscoveryClient
   @SpringBootApplication
   public class GulimallProductApplication {
   
       public static void main(String[] args) {
           SpringApplication.run(GulimallProductApplication.class, args);
       }
   
   }
   ```

5. (Optional)If there is a requirement for isolation between microservices, a separate namespace can be created for each one. In the case, we need to specify the namespace via `spring.cloud.nacos.config.namespace`.

6. (Optional)If you want to logically separate configurations within the same namespace(e.g., by module or environment), we can define different groups via `spring.cloud.nacos.config.group`



Sometimes, the configuration needs to be divides into multiple configuration files. In such cases, the `spring.config.import` property can be used to specify them.

```yaml
spring:
  cloud:
    nacos:
      discovery:
        server-addr: 192.168.56.1:8848
      config:
        server-addr: 192.168.56.1:8848
        namespace: f0edc21e-a3d1-4594-b4bd-29f54d0b6f6a
        group: DEV
        file-extension: yaml
  config:
    import:
      - nacos:datasource
      - nacos:mybatis-plus
```

> [!warning]
>
> The full name is `data-id` + `.${file-extension}`

datasource.yaml

```yaml
spring:
  datasource:
    username: root
    password: root
    url: jdbc:mysql://192.168.56.10:3306/gulimall_sms
    driver-class-name: com.mysql.cj.jdbc.Driver
```

mybatis-plus.yaml

```yaml
mybatis-plus:
  mapper-locations: classpath*:/mapper/**/*.xml
  global-config:
    db-config:
      id-type: auto
```





## 3.5 Gateway

A gateway is typically used to handle requests from clients and direct them to the appropriate service or backend.

It performs several key functions, such as request routing, load balancing, API aggregation, rate limiting & monitoring.

![image-20250829202655179](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250829202655179.png)



Then, Follow the instructions below step by step:

1. Create the `gulimall-gateway` module

2. Add the corresponding dependencies 

   ```xml
   <dependency>
       <groupId>org.springframework.cloud</groupId>
       <artifactId>spring-cloud-starter-gateway</artifactId>
   </dependency>
   
   <!--Loader Balancer-->
   <dependency>
       <groupId>org.springframework.cloud</groupId>
       <artifactId>spring-cloud-starter-loadbalancer</artifactId>
   </dependency>
   
   <!--Caffeine Cache-->
   <dependency>
       <groupId>com.github.ben-manes.caffeine</groupId>
       <artifactId>caffeine</artifactId>
   </dependency>
   ```

3. Configure the service registration and discovery

   ```yaml
   spring:
     application:
       name: gulimall-gateway
     cloud:
       nacos:
         discovery:
           server-addr: 192.168.56.1:8848
         config:
           server-addr: 192.168.56.1:8848
           namespace: 5061d394-85ce-4a0b-93a9-88669ad3348e
           file-extension: yaml
       loadbalancer:
         cache:
           caffeine:
             spec: maximumSize=500,expireAfterWrite=10m
     config:
       import: optional:nacos:application.yml
   server:
     port: 9030
   ```



After completing the above configuration, refer to the [official documentation](https://docs.spring.io/spring-cloud-gateway/reference/spring-cloud-gateway-server-webmvc/gateway-request-predicates.html) to configure the routing rules.

Here are some examples:

1. Set the predicates as `Query=url, google`. I the value of query parameter `url` is `google`, the request will be routed to `www.google.com`

   ```yaml
   spring:
     cloud:
       gateway:
         routes:
           - id: google-route
             uri: https://www.google.com
             predicates:
               - Query=url, google
   ```



# 4 Front-end fundamentals

## 4.1 Basic tools

- VSCode: A lightweight, extensible code editor for writing and managing code efficiently.
- Node.js: A JavaScript runtime that allows running JS on the server and managing project dependencies vis npm.
- Vue: A progressive JavaScipt framework for building interactive user interfaces and singl-page applications.
- Babel: A JavaScipt complier that convert modern JS(ES6+) into backward-compatible code for older browsers.
- Webpack: A module bunlder that packages JavaScipt, CSS, and other assets for efficient web deployment.





