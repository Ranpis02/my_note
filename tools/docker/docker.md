[toc]

# 1 Docker 基本介绍

概述：docker 是一个使用 go 开发的开源的应用容器引擎

- docker 基于镜像的虚拟化技术，可以妙级启动容器
- 每个容器都是一个完整的运行环境，容器之间可以相互隔离

> docker 官网：https://www.docker.com/
>
> docker 仓库：https://hub.docker.com/



对应 Docker 和 Linux

| 特性       | 容器               | 虚拟机     |
| ---------- | ------------------ | ---------- |
| 启动速度   | 秒级               | 分钟级     |
| 硬盘使用   | 一般为MB           | 一般为GB   |
| 性能       | 接近原生           | 弱于原生   |
| 系统支持量 | 单机支持上千个容器 | 一般几十个 |



# 2 Docker 安装

## 2.1 CentOS 安装 Docker

版本要求：CentOS 7 及以上，内核版本要求在 3.8 以上

> 查看内核版本信息命令：`uname -r`
>
> 安装的参考文档：https://docs.docker.com/engine/install/centos/

使用阿里云的安装脚本自动安装

```bash
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```



docker 更换镜像

登录 https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors，获取阿里云镜像地址

![image-20230903201049677](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230903201049677.png)

在 /etc/docker/daemon.json 中写入以下内容（若没有则新建即可）

```json
{
	"registry-mirrors": ["https://${your aliyun id}.mirror.aliyuncs.com"]
}
```

之后重启服务

```bash
sudo systemctl daemon-reload
sudo systemctl restart docker
```



```bash
# 安装 vim 
yum -y install vim-minimal
yum -y install vim-enhanced

# 编辑文件
vim /etc/docker/daemon.json
```

之后通过 `docker info` 来查看配置是否成功





# 3 Docker 安装软件

## 3.1 总体步骤

安装软件镜像大同小异，这里以 Tomcat 为例，介绍基本步骤

1. 搜索镜像

   ```bash
   docker search tomcat
   ```

2. 拉取镜像

   ```bash
   docker pull tomcat
   ```

3. 查看镜像

   ```bash
   docker images tomcat
   ```

4. 启动容器

   ```bash
   docker run -d tomcat --name tomcat
   ```

5. 停止容器

   ```bash
   docker stop tomcat
   ```

6. 移除容器

   ```bash
   docker rm tomcat
   ```







# 4 Docker 命令



## 4.1 帮助启动类命令

1. 启动 docker

   ```bash
   systemctl start docker
   ```

2. 停止 docker

   ```bash
   systemctl stop docker
   ```

3. 重启 docker

   ```bash
   systemctl restart docker
   ```

4. 查看 docker 的状态

   ```bash
   systemctl status docker
   ```

5. 开启自启动

   ```bash
   systemctl enable docker
   ```

6. 查看 docker 摘要信息

   ```bash
   docker info
   ```

7. 查看 docker 总体帮助文档

   ```bash
   docker --help
   ```

8. 查看 docker 命令帮助文档

   ```bash
   docker 具体命令 --help
   ```



## 4.2 镜像命令

1. 列出本地主机上的镜像

   ```bash
   docker images
   ```

   - `-a`：列出本地所有镜像（含历史映像层）
   - `-q`：只显示镜像 ID

2. 到远程仓库去查找镜像

   ```bash
   docker search [name]
   ```

   - `--limit`：限制镜像列出数量，默认是 25 个

3. 拉取镜像到本地

   ```bash
   docker pull 镜像名:[TAG]
   ```

   > 默认情况下，没有 TAG = latest

4. 删除镜像

   ```bash
   # 删除单个镜像
   docker rmi [name / id]
   
   # 删除多个镜像
   docker rmi 镜像1 镜像2
   
   # 删除全部镜像
   docker rmi -f $(docker images -qa)
   ```

   - `-f`：强制删除

5. 查看镜像 / 容器 / 数据卷所占用的空间

   ```bash
   docker system df
   ```



提问：docker 的虚悬镜像是什么？

docker 的虚悬镜像是指：仓库名、标签名都是 `<none>` 的镜像



## 4.3 容器命令

通过 docker 镜像可以创建相应容器，在演示之前，我们提前下载一个 CentOS 或者 Ubuntu 的 docker 镜像进行演示

```bash
docker pull ubuntu
```



### 4.3.1 启动容器

```bash
docker run [options] [image_name]
```

- `--name`：为容器指定名称

- `-d`：启动守护式容器（后台运行）

- `-i`：以交互模式运行容器，通常与 `-t` 配合使用

- `-t`：为容器分配一个伪输入终端，通常与 `-i` 配合使用

  > `-it` 一起使用时即启动交互式容器（前台有伪终端，等待交互）

- `-P`（大写的 P）：随机端口映射

- `-p`（小写的 p）：指定端口映射，其中

  ```bash
  # 如下表示会将容器的 80 端口映射到宿主机的 8000 端口上，并且在启动容器的同时会打开一个 shell 终端
  docker run -p 8000:80 -it ubuntu /bin/bash
  ```

  > 退出容器：`exit`



如果我们想要列出当前所有正在运行的容器该如何做？

```bash
docker ps [options]
```

- -a :列出当前所有正在运行的容器+历史上运行过的
- -l :显示最近创建的容器
- -n：显示最近n个创建的容器
- -q :静默模式，只显示容器编号



### 4.3.2 停止、退出、重启、删除容器

docker 中如果想要退出容器有两种方式：

- exit：通过 run 进入容器，通过 exit 退出后，<u>容器停止</u>
- Ctrl + p + q：通过 run 进入容器，通过快捷键退出后，<u>容器不停止</u>



**停止容器**

```bash
docker stop 容器ID或者容器名

# 强制停止容器
docker kill 容器ID或者容器名
```



**重启容器**

```bash
docker restart 容器ID或者容器名

# 启动已停止运行的容器
docker start 容器ID或者容器名
```



**删除已停止的容器**

```bash
docker rm 容器ID

# 全部删除
docker rm -f $(docker ps -aq)
```



### 4.3.3 进阶容器命令

**启动守护式容器进程**

在大部分场景下，我们是希望 docker 的服务是在后台运行的，这时我们可以通过 `-d` 指定容器的后台运行模式

```bash
docker run -d 容器名
```



查看 docker 容器的日志

```bash
docker logs 容器ID
```



查看容器内运行的进程

```bash
docker top 容器ID
```



查看容器内部细节

```
docker inspect 容器ID
```



**进入正在运行的容器并以命令行的形式进行交互**

```bash
# 方式1
docker exec -it 容器ID bash

# 方式2
docker attach 容器ID
```

> 推荐使用方式1，通过方式1重新进入后，使用 exit 命令退出时不会关闭容器，而方式2会



**从容器中拷贝文件到主机**

```bash
docker cp 容器ID:容器内路径 目的主机路径
```

演示

```bash
# 在容器的 home 目录下创建文件 a
root@1c5eff44ac5a:/home# touch a
# 从容器中退出
root@1c5eff44ac5a:/home# exit

# 将容器中文件移到当前主机上
[root@localhost xzh]# docker cp 1c5eff44ac5a:/home/a /home
```



**导出和导入容器**

- `export` 用于导出容器的内容留作为一个 tar 归档文件
- `import` 从 tar 包中的内容创建一个新的文件系统再导入为镜像

```bash
# 默认是当前目录下
docker export 容器ID > 文件名.tar

cat 文件名.tar | docker import - 镜像用户/镜像名:镜像版本号
```



# 5 Docker 镜像发布

## 5.1 commit

```bash
docker commit -m="描述信息" -a="作者" 容器ID 要创建的目标镜像名:[标签名]
```

**演示**

```bash
[root@localhost xzh]# docker run -it ubuntu bash
root@2ce4b4e7f354:/# apt-get install vim
root@2ce4b4e7f354:/# apt-get install vim
root@2ce4b4e7f354:/# exit

# 通过上述操作将 ubuntu 容器中添加了 vim 命令，下面将进行提交容器副本使之成为一个新的镜像
[root@localhost xzh]# docker commit -m="add vim command" -a="zhihong" 2ce4b4e7f354 zhihong/ubuntu:bear

# 然后对创建的 docker 镜像进行查看
[root@localhost xzh]# docker images
```



## 5.2 本地镜像发布到阿里云

**阿里云ECS Docker生态如下图所示**：

<img src="https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230904100515441.png" alt="image-20230904100515441" style="zoom:50%;" />

如果我们想要将自己制作的镜像发布出来，首先得生成一个镜像，生成的镜像的方式有一种：

1. 前面提过的 commit 方法
2. Dockerfile



之后需要将本地镜像推到云平台

![image-20230904101846832](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230904101846832.png)

进入 ==个人实例== 创建命令空间

![image-20230904102405077](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230904102405077.png)

创建镜像仓库

![image-20230904102611607](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230904102611607.png)

代码源选择==本地仓库==即可，之后根据下面的操作指南进行配置即可



这里需要注意的是：当我们登录成功后，docker 会在 CentOS 7 保存登录密码，存放在 */root/.docker/config.json*，我们可以直接查看到对应的密码

```bash
cat /root/.docker/config.json
echo "Y1W2R3t4a5W64768T9W99u0d0G85lgdfHdNdAsMjAyMA==" | base64 --decode
```

再次登录和删除命令

```bash
# 表示以最近一次有效的用户名和密码进行登录
docker login $domain

# 选择最近一次有效的用户名和密码将 config.json 中删除
docker logout $domain

# 备注：$domain 也可以直接替换为 host
```

为了保证安全，可以更换其他安全系数高的产品将其保存下来



## 5.3 推送到私服

从 2023 年 5 月份开始，Docker 的[仓库](https://hub.docker.com/)已经无法访问，但是 Dockerhub、阿里云这样的公共镜像仓库可能保密性不好，所以我们需要创建一个本地私人仓库提供给团队使用

<u>Docker Registry 是官方提供的工具，可用于构建私有镜像仓库</u>

1. 下载镜像 Docker registry

   ```bash
   docker pull registry
   ```

2. 运行容器，创建仓库

   ```bash
   docker run -d -p 5000:5000  -v /xzh/myregistry/:/tmp/registry --privileged=true registry
   ```

   > 默认情况下，仓库被创建在容器的 `/var/lib/registry` 目录下，建议自行用容器卷映射，方便宿主机联调

3. 验证私服仓库

   ```bash
   curl -XGET http://192.168.88.131:5000/v2/_catalog
   ```

   > 注意：这里的 IP 地址是宿主机的 IP 地址

4. 将自己创建的新镜像修改为符合私服规范的 tag

   ```bash
   # 将 ubuntu 这个镜像修改为 192.168.88.131:5000/ubuntu 保留一份（相当于更名克隆一份）
   docker tag ubuntu 192.168.88.131:5000/ubuntu
   ```

5. 修改配置文件 `/etc/docker/daemon.json` 使之支持 http

   ```json
   {
     "registry-mirrors": ["https://iaa77hd4.mirror.aliyuncs.com"],
     "insecure-registries": ["192.168.88.131:5000"]
   }
   ```

   > 重启 docker：`systemctl restart docker`（这步可省略）

6. 最后 push 上去即可

   ```bash
   docker push 192.168.88.131:5000/ubuntu
   ```

7. 从私服上拉取

   ```bash
   docker pull 192.168.88.131:5000/ubuntu
   ```



# 6 容器数据卷

## 6.1 基本使用

容器数据卷：数据卷即存容器中存放数据的文件或目录，主要用来实现容器与外部系统之间数据的共享的问题，实现了数据的持久化。

> 与 redis 里面的 rdb 和 aof 类似，或者我们可以联想电脑本地文件与云端备份，我们使用容器数据卷可以保证当容器销毁时，容器内的数据仍然在外部系统有一份备份，与前面使用 `docker cp` 手动备份不同，容器数据卷可以自动完成备份，并且可以实现**双向同步**



命令如下

```bash
docker run -it --privileged=true -v /宿主机绝对路径目录:/容器内目录 镜像名
```

> 由于 CentOS 7  的安全模块，目录挂载被认为是一种不安全的行为，我们如果使用 `--privileged=true` 解决 cannot open directory .: Permission denied 的问题



## 6.2 容器卷 ro 和 rw 读写规则

如果我们想要限制**容器内部对数据卷的读写权限**，可以使用 ro 和 rw 规则

1. 默认情况下，读写时默认规则，即我们前面的命令等价于下面

   ```bash
   docker run -it --privileged=true -v /宿主机绝对路径目录:/容器内目录:rw 镜像名
   ```

2. 限制容器内部，只能读，不能写

   ```bash
   docker run -it --privileged=true -v /宿主机绝对路径目录:/容器内目录:ro 镜像名
   ```



## 6.3 容器卷的继承

以 Ubuntu 作为继承案例

```bash
docker run -it --privileged=true --volumes-from 父类 --name u2 ubuntu
```



**示例**

```bash
# 启动镜像，保持宿主机的 /home/xzh/sync 与容器的 /home/xzh/sync 数据同步，指定别名为 u1
docker run -it --privileged=true -v /home/xzh/sync:/home/xzh/sync --name u1 ubuntu

# 在容器内写入数据
root@ecb62fb6efbc:/home/xzh/sync# echo "Hello World" >> ./a

# 退出容器
root@ecb62fb6efbc:/home/xzh/sync# exit

# 回到宿主机进行查看
[root@localhost xzh]# cat /home/xzh/sync/a
Hello World

# 创建第二个容器 u2 继承 u1
[root@localhost xzh]# docker run -it --privileged=true --volumes-from u1 --name u2 ubuntu

# 查看是否有 u1 的容器卷同步过来
root@50aef24eccee:/# cat /home/xzh/sync/a
Hello World
```



# 7 Dockerfile

我们回到下面这张图

![image-20230904165343731](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230904165343731.png)



**dockerfile 是用来构建 Docker 镜像的文本文件，是由一条条构建镜像所需的指令和参数构成的脚本**

> 官方文档：https://docs.docker.com/engine/reference/builder/



## 7.1 Dockerfile 基本构建过程

Dockerfile 基础知识

1. 每条保留字指令都必须为**大写字母**且后面至少要跟随一个参数
2. 指令按照从上到下，顺序执行
3. `#` 表示注释
4. 每条指令都会创建一个新的镜像层并对镜像进行提交



执行 dockerfile 的基本流程

1. docker 从基础镜像运行一个容器
2. 执行一条指令并对容器修改
3. 执行类似 docker commit 的操作提交一个新的镜像层
4. docker 再基于刚提交的镜像运行一个新容器
5. 执行 dockerfile 中的下一条指令直到所有指令都执行完成



## 7.2 Docker 的保留字指令

> 参考 Tomcat8 的 dockerfile 入门：https://github.com/docker-library/tomcat



Docker 的保留字指令

1. `FROM`：基础镜像，当前新镜像是基于那个镜像的，指定一个已经存在的镜像作为模块，dockerfile 的第一条必须是 FROM

2. `MAINTAINER`：镜像维护者的姓名和邮箱地址

3. `RUN`：容器构建时所需要的命令

   该命令式在 `docker build` 时运行的命令，主要有如下两种形式：

   1. shell 格式：`RUN <命令行命令>`，等同于在终端操作的 shell 命令
   2. exec 格式：`RUN ["可执行文件", "参数1", "参数2"]`

4. `EXPOSE`：当前容器对外暴露出的端口

5. `WORKDIR`：指定在创建容器后，终端默认登录的工作目录，即一个落脚点

6. `USER`：指定该镜像以什么用户身份去执行，默认是 root

7. `ENV`：用来在构建镜像过程中设置环境变量

8. `ADD`：将宿主机目录下的文件拷贝进镜像且会自动处理 URL 和解压 tar 压缩包

9. `COPY`：类似于 ADD，仅拷贝文件和目录到镜像中

10. `VOLUME`：容器数据卷，用于数据保存和持久化工作

11. `CMD`：指定容器启动后要做的事

    > `dockerfile` 中可以多个 CMD  指令，但是只有最后一个会生效，CMD 会被 `docker run ` 后面的参数替换

12. `ENTRYPOINT`：类似于 CMD 命令，也是用来指定一个容器启动时要运行的命令，但是该命令不会被 `docker run` 后面的命令覆盖，而且这些命令行参数会被当做参数送给 `ENTRYPOINT` 指令指定的程序

    > 注意：如果 Dockerfile 中存在多个 ENTRYPOINT 指令，仅最后一个生效

