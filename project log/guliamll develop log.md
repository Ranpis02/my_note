# 1 The Basics

## 1.1 Overall Architecture

![image-20250507145614544](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20250507145614544.png)



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



The databases that need to be created includes `gulimall_admin`, `gulimall_oms`, `gulimall_pms`, `gulimall_sms`, `gulimall_ums`, `gulimall_ums`, `gulimall_wms`, which mean respectively:

- `gulimall_admin`: Stores **administrative user data** and system configuration.
- `gulimall_oms`:  Handles **order management data**.
- `gulimall_pms`: Manage **product-related data**.
- `gulimall_sms`: **Stores sales and marketing service** data.
- `gulimall_ums`: contains **user membership data**.
- `gulimall_wms`: Manages **warehouse data**.



## 2.6 Deploy Renren-Admin System

> Detailed informat Refer: [renrenio repository](https://github.com/renrenio/)

