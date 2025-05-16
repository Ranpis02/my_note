# 1 将虚拟机连上远程连接工具

这里使用 Mobaxterm （一款增强远程连接工具）连接 Ubuntu live server 20.04.5

## 1.1 出现问题和解决方案

### 1.1.1 Connection refused

```
Network error: Connection refused
```

![image-20221018094829214](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221018094829214.png)

:information_source:问题原因

- 没有安装 ssh 相关服务软件
- 没有启动 sshd 服务

1. 检查是否安装了 sshd 服务相关软件

   ```bash
   sudo ps -e |grep ssh
   ```

2. 没有则进行安装提供 sshd 服务的软件

   ```bash
   apt-get install openssh-server
   systemctl restart ssh     # 重启 openssh server
   ```

3. 切换 root 用户

   ```bash
   su
   ```

4. 重启 openssh 服务

   ```bash
   systemctl restart ssh
   ```

一般到这里就已经可以配置成功



### 1.1.2 切换 root 用户

第一次使用 root 用户时，需要配置用户密码：

```bash
sudo passwd [root]
```

 

## 1.2 使用 Mobaxterm

### 1.2.1 使用 SSH session

1. 获取虚拟机的 IP 地址

   ```bash
   ip addr
   ```



# 2 CentOS SSH 远程连接

前面是 Ubuntu 20.04 通过 SSH 连接到 Mobaxterm 的方法，接下来是 CentOS 的解决方案，如果我们发现通过无法通过 `ip addr` 获取到当前主机的 IP 地址，查看网络适配器的配置文件

```bash
vi /etc/sysconfig/network-scripts/ifcfg-ens33
```

修改 ONBOOT = yes

```bash
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=dhcp
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=ens33
UUID=da64e0bf-0cfc-4ace-854c-45d24142ed09
DEVICE=ens33
ONBOOT=yes # 在这里将其从 no -> yes
```

之后再重启网络服务

```bash
sudo service network restart
```

剩下的操作和前面类似



:taco:补充：修改 CentOS 7 镜像

1. 备份源文件

   ```bash
   mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
   ```

2. 下载更改源

   ```bash
   curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
   ```

3. 清空缓存并生成 cache 文件

   ```bash
   yum clean all
   yum makecache
   ```

