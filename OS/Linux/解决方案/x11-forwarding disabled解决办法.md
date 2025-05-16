> 【摘要】 Linux系统也是有图像界面的。它的方式和Windows不一样，叫做 X Window，采用的是X11协议。X11 中的 X 指的就是 X 协议；11 指的是采用 X 协议的第 11 个版本。原理图如上。那么我们使用客户端来登陆到一个服务器时，做图像界面展示时，哪个是X client，哪个是X server呢？答案是客户端是X server，比如我们用MobaXterm登录服务器，MobaX...

Linux系统也是有图像界面的。它的方式和Windows不一样，叫做 X Window，采用的是X11协议。X11 中的 X 指的就是 X 协议；11 指的是采用 X 协议的第 11 个版本。

![image-20230904140634413](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230904140634413.png)

原理图如上。那么我们使用客户端来登陆到一个服务器时，做图像界面展示时，哪个是X client，哪个是X server呢？

答案是客户端是X server，比如我们用MobaXterm登录服务器，MobaXterm就是X Server，而Linux服务器是X Client哦~



现在开始解决 ==CentOS 7== 的 Server 版本中出现 X11-forwarding disbaled 的问题

![](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230904140417503.png)



01 安装xorg-x11-xauth

```bash
yum update
yum install xorg-x11-xauth
```



02 修改 sshd 的配置文件

```bash
vim /etc/ssh/sshd_config
```

修改内容如下：

```bash
#AllowAgentForwarding yes
AllowTcpForwarding yes   #这里打开
#GatewayPorts no
X11Forwarding yes      #这里原本就是打开的
#X11DisplayOffset 10
#X11UseLocalhost yes
#PermitTTY yes
```



03 重启 sshd 服务

```bash
systemctl reload sshd
systemctl restart sshd
```

