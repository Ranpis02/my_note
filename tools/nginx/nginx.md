[toc]

# 1 基础

## 1.1 基本介绍

概述：Nginx 是一个高性能的 HTTP 和反向代理的 web 服务器

> 介绍文档：https://lnmp.org/nginx.html
>
> 官方文档：https://nginx.org/en/docs/

Nginx 的核心功能：

1. 反向代理
2. 负载均衡
3. 动静分离
4. 高性能集群



## 1.2 反向代理

与==反向代理==相对的一个概念叫做 ==正向代理==

==正向代理==相当于一个 "跳板机"，也就是说目标 URL 是明确的，只是说我们如果正常访问是无法访问的，需要通过代理才能访问目标资源。最常见的例如 VPN

![img](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/305504-20161112124853014-1532060796.png)

正向代理的作用：

1. 帮助我们访问无法访问的资源
2. 做缓存，加速访问资源
3. 对客户端访问授权，上网进行认证
4. 代理可以记录用户访问记录（上网行为管理），对外隐藏用户信息



而反向代理则是**客户端将请求发送到代理服务器，由代理服务器去选择目标服务器获取数据后，返回给客户端**

![img](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/305504-20161112124341280-1435223816.png)

反向代理可以通过反向代理服务器来保证内网安全，通过其提供的 WAF（web 应用防火墙） 功能来阻止 web 攻击，同时还有一个功能就是通过 LB 算法减轻目标服务器压力

两者的主要区别在于：

1. 在正向代理中，<u>proxy 和 client 同属于一个 LAN</u>，部分攻击者喜欢通过代理服务器来隐藏自己真实的 IP 地址防止被纳入黑名单
2. 在反向代理中，<u>proxy 和 server 同属于一个 LAN</u>，大型网站通过反向代理即可以做到保护自己 web 服务器，又可以做到负载均衡



## 1.3 负载均衡

负载均衡：即将 web 请求通过 LB 算法分发到不同的服务器

![image-20230901211737310](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230901211737310.png)



## 1.4 动静分离

为了加快网站的解析速度，可以把动态资源和静态资源由不同的服务器来解析，降低单个服务器的压力

![image-20230901214838108](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230901214838108.png)



## 1.5 下载使用

> 下载地址：https://nginx.org/en/download.html

使用版本：nginx-1.20.2

使用环境：Ubuntu 20.04

具体安装步骤：

1. 安装 gcc 编译器

   ```bash
   sudo apt install gcc
   ```

2. 切换到 /usr/local 目录下，安装 nginx

   ```bash
   cd /usr/local
   wget http://nginx.org/download/nginx-1.20.2.tar.gz
   ```

3. 解压得到 nginx 源码

   ```bash
   tar -zxvf nginx-1.20.2.tar.gz
   ```

4. 使用 nginx 自带的 configure 进行配置命令（注意：一定要确认当前目录下存在 configure文件）

   ```bash
   cd nginx-1.20.2
   ./configure
   ```

5. 安装 make 指令并进行编译和安装（注意：一定要确认当前目录下存在 Makefile 文件）

   ```bash
   # 安装 make 命令
   sudo apt install make
   make&&make install
   ```

6. 进入 /usr/local/nginx/sbin 目录下进行验证安装是否成功，同时检查配置文件

   ```bash
   cd /usr/local/nginx/sbin
   # 检查安装是否成功
   ./nginx -v
   # 检查配置文件
   ./nginx -t
   ```

7. 启动 nginx ，找到 /usr/local/nginx/sbin/nginx 和 /usr/local/nginx/conf/nginx.conf，执行命令

   ```bash
   nginx -c nginx.conf
   ```

   例如：`root@xzh:/usr/local/nginx# ./sbin/nginx -c ./conf/nginx.conf` 这样执行即可

8. 由于启动是不会输出任何信息的，所以我们可以通过查看进程来判断启动是否成功

   ```bash
   ps -ef | grep nginx
   ```

   ![image-20230901234322445](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230901234322445.png)

9. 或者查看对应的端口也行（nginx 的默认端口是 80）

   ```bash
   netstat -anp | more
   ```

   ![image-20230901234553423](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230901234553423.png)





:bug:bug 收集

01 `./configure` 出现如下错误

```
the HTTP rewrite module requires the PCRE library.
```

解决方案：安装相应的库即可

```bash
sudo apt-get install libpcre3 libpcre3-dev
```



02 `./configure` 又出现如下错误

```
the HTTP gzip module requires the zlib library.
```

出现原因：缺少 zlib 库

解决方案：安装对应的库即可

```bash
apt install zlib1g
apt install zlib1g-dev
```





:bulb:小技巧：快速清除终端已输入的命令

<u>按快捷键 Ctrl + U 即可</u>



## 1.6 本地访问

如果我们安装的是 Ubuntu Desktop（即有 GUI），可以直接浏览器中输出 url 进行访问

```
http://localhost
```

> http 服务默认端口号为 80，所以我们可以 不需要单独输端口号

如果我们安装的使 Ubuntu Server，希望通过命令行访问，可以安装 w3m / links / lynx

```bash
sudo apt install w3m
w3m localhost
```

如果我们希望直接通过外部的 window 访问 Linux 虚拟机，可以配置防火墙使其可以访问，步骤如下：

1. 安装 ufw

   ```bash
   sudo apt update
   sudo apt install ufw
   ```

2. 开放端口号

   ```bash
   sudo ufw allow 80
   ```

3. 重启防火墙

   ```bash
   ufw reload
   ```

端口开放成功后，通过 `ip addr` 查看虚拟机 IP 地址，之后通过 ip:port 形式访问即可



ufw 更多命令参数

- 查看已定义规则

  ```bash
  ufw status
  ```

- 外来访问默认允许 / 拒绝

  ```bash
  ufw default allow/deny
  ```

- 允许 / 拒绝访问端口号

  ```bash
  ufw allow/deny 20
  ```

- 允许自192.168.0.0/24的tcp封包访问本机的22端口

  ```bash
  sudo ufw allow proto tcp from 192.168.0.0/24 to any port 22
  ```

- 删除以前自定义规则

  ```bash
  ufw delete allow/deny 20
  ```

- 关闭防火墙

  ```bash
  ufw disable
  ```





## 1.7 nginx 命令行参数

> 参考文档：https://nginx.org/en/docs/switches.html

- 开启 nginx 服务

  ```bash
  nginx -c nginx.conf
  ```

- 关闭 nginx 服务

  ```bash
  nginx -s stop
  ```

- 重新加载

  ```bash
  nginx -s reload
  ```

- 查看当前版本

  ```bash
  nginx -v
  nginx -V # 不仅可以看到当前 nginx 版本，还可以看到依赖的 GCC 版本
  ```



## 1.8 nginx.conf

nginx.conf 默认位置在 nginx/conf 文件夹下，用于配置各种参数，我们在启动 nginx 时，也可以直接 `./nginx` 后面不需要带上 `-c nginx.conf` ，会自动加载 nginx.conf 文件

nginx.conf 主要作用包括：<u>完成对 Nginx 的各种配置，包括端口、并发数、重写规则等</u>

```mermaid
graph LR
A("组成") --> B("全局块") & C("events") & D("http块")
```

详细配置如下：

```ini
 
#Nginx用户及组：用户 组。window下不指定
#user  nobody;
 
#工作进程：数目。根据硬件调整，通常等于CPU数量或者2倍于CPU。
worker_processes  1;
 
#错误日志：存放路径。
#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;
 
#pid(进程标识符)：存放路径
pid       /usr/local/nginx/logs/nginx.pid;
 
#一个进程能打开的文件描述符最大值，理论上该值因该是最多能打开的文件数除以进程数。
#但是由于nginx负载并不是完全均衡的，所以这个值最好等于最多能打开的文件数。
#LINUX系统可以执行 sysctl -a | grep fs.file 可以看到linux文件描述符。
worker_rlimit_nofile 65535;
 
 
events {
	#使用epoll的I/O 模型。linux建议epoll，FreeBSD建议采用kqueue，window下不指定。
	use epoll;
	
	#单个进程最大连接数（最大连接数=连接数*进程数）
    worker_connections  1024;
	
	#客户端请求头部的缓冲区大小。这个可以根据你的系统分页大小来设置，
	#一般一个请求头的大小不会超过1k，不过由于一般系统分页都要大于1k，所以这里设置为分页大小。
	#client_header_buffer_size 4k;
}
 
 
http {
	#设定mime类型,类型由mime.type文件定义
    include       mime.types;
	
    default_type  application/octet-stream;
	
	#日志格式设置
    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';
	
		
	#用了log_format指令设置了日志格式之后，需要用access_log指令指定日志文件的存放路径
	#记录了哪些用户，哪些页面以及用户浏览器、ip和其他的访问信息
	#access_log  logs/host.access.log  main;
	#access_log  logs/host.access.404.log  log404;
	
	#服务器名字的hash表大小
    server_names_hash_bucket_size 128;
	
	#客户端请求头缓冲大小。
    #nginx默认会用client_header_buffer_size这个buffer来读取header值，
    #如果header过大，它会使用large_client_header_buffers来读取。
    #如果设置过小HTTP头/Cookie过大 会报400 错误 nginx 400 bad request
    #如果超过buffer，就会报HTTP 414错误(URI Too Long)
    #nginx接受最长的HTTP头部大小必须比其中一个buffer大
    #否则就会报400的HTTP错误(Bad Request)
    #client_header_buffer_size 32k;
    #large_client_header_buffers 4 32k;
	
	
	#隐藏ngnix版本号
    #server_tokens off;
	
	#忽略不合法的请求头
    #ignore_invalid_headers   on;
	
	#让 nginx 在处理自己内部重定向时不默认使用  server_name设置中的第一个域名
    #server_name_in_redirect off;
	
	
	#客户端请求体的大小
    #client_body_buffer_size    8m;
   
  
    #开启文件传输，一般应用都应设置为on；若是有下载的应用，则可以设置成off来平衡网络I/O和磁盘的I/O来降低系统负载
    sendfile        on;
	
	
	#告诉nginx在一个数据包里发送所有头文件，而不一个接一个的发送。
    #tcp_nopush     on;
	
	#tcp_nodelay off 会增加通信的延时，但是会提高带宽利用率。在高延时、数据量大的通信场景中应该会有不错的效果
    #tcp_nodelay on，会增加小包的数量，但是可以提高响应速度。在及时性高的通信场景中应该会有不错的效果
	tcp_nodelay on;
	
 
    #长连接超时时间，单位是秒
    keepalive_timeout  65;
 
    #gzip模块设置，使用 gzip 压缩可以降低网站带宽消耗，同时提升访问速度。
    #gzip  on;                     #开启gzip
    #gzip_min_length  1k;          #最小压缩大小
    #gzip_buffers     4 16k;       #压缩缓冲区
    #gzip_http_version 1.0;        #压缩版本
    #gzip_comp_level 2;            #压缩等级
    #gzip_types   text/plain text/css text/xml text/javascript application/json application/x-javascript application/xml application/xml+rss;#压缩类型
    
	
	
	
    #负载均衡
	#max_fails为允许请求失败的次数，默认为1
	#weight为轮询权重，根据不同的权重分配可以用来平衡服务器的访问率。
    # upstream myServer{
    #   server  192.168.247.129:8080 max_fails=3 weight=2;
    #   server  192.168.247.129:8081 max_fails=3 weight=4;	
    # }
	
	
	
    #server {
    #    listen       80;
	#	
	#	#IP/域名可以有多个，用空格隔开
	#	server_name  192.168.247.129;
	#	#server_name  www.test.com;
	#
    #    #charset koi8-r;
	#
    #    #access_log  logs/host.access.log  main;
	#	
	#   #反向代理配置，
    #   #将所有请求为www.test.com的请求全部转发到upstream中定义的目标服务器中。
    #   location / {
	#   			
	#	    #此处配置的域名必须与upstream的域名一致，才能转发。
	#	    proxy_pass http://myServer;
	#	    #proxy_pass http://192.168.247.129:8080;
	#		
	#		 proxy_connect_timeout 20;          #nginx跟后端服务器连接超时时间(代理连接超时)
	#		
    #        #client_max_body_size       10m;   #允许客户端请求的最大单文件字节数
    #        #client_body_buffer_size    128k;  #缓冲区代理缓冲用户端请求的最大字节数
	#		 #proxy_send_timeout         300;   #后端服务器数据回传时间(代理发送超时)
    #        #proxy_read_timeout         300;   #连接成功后，后端服务器响应时间(代理接收超时)
    #        #proxy_buffer_size          4k;    #设置代理服务器（nginx）保存用户头信息的缓冲区大小
    #        #proxy_buffers              4 32k; #proxy_buffers缓冲区，网页平均在32k以下的话，这样设置
    #        #proxy_busy_buffers_size    64k;   #高负荷下缓冲大小（proxy_buffers*2）
    #        #proxy_temp_file_write_size 64k;   #设定缓存文件夹大小，大于这个值，将从upstream服务器传    		
	#		
	#		root   html;
	#		
	#		#定义首页索引文件的名称
	#		index  index.html index.htm;
    #    }
	#
    #   #动静分离 静态资源走linux 动态资源走tomcat
    #   # 注意 /source/image/下面寻找资源
    #   location /image/ {
    #       root /source/;
	#       autoindex on;
    #   } 		
	#
	#
	#    # 出现50x错误时，使用/50x.html页返回给客户端
    #    error_page   500 502 503 504  /50x.html;
    #    location = /50x.html {
    #        root   html;
    #    }
    #}
		
		
		
	#下面是配置生产环境中既支持HTTP又支持HTTPS,保证用户在浏览器中输入HTTP也能正常访问
	
	# SSL证书 配置                                 
	ssl_certificate     	cert/yphtoy.com.pem;   #加密证书路径
	ssl_certificate_key	cert/yphtoy.com.key;       #加密私钥路径
	ssl_protocols		TLSv1 TLSv1.1 TLSv1.2;     #加密协议
	ssl_session_cache	shared:SSL:1m;             #加密访问缓存设置,可以大大提高访问速度
	ssl_session_timeout	10m;                       #加密访问缓存过期时间
	ssl_ciphers		HIGH:!aNULL:!MD5;              #加密算法
	ssl_prefer_server_ciphers on;	               #是否由服务器决定采用哪种加密算法
	
	# 负载均衡
	upstream api_upstream
	{
	    server 127.0.0.1:8080 max_fails=3 weight=1;
		server 127.0.0.1:8081 max_fails=3 weight=1;
	}
	
	#api 接口(兼容HTTP)
	server{
	    listen 80;
		server_name api.test.com;
		# 301重定向跳转到HTTPS接口
		return 301 https://$server_name$request_uri;
		error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
	}
	
	#api 接口(兼容HTTPS)
	server{
	    listen 443 ssl;
		server_name api.test.com;
		location / {
		   root html;
		   index  index.html index.htm;
		   proxy_pass http://api_upstream;
		   
		   #语法： proxy_cookie_path oldpath replacepath;
		   #oldpath就是你要替换的路径 replacepath 就是要替换的值
		   #作用：同一个web服务器下面多个应用之间能获取到cookie
		   proxy_cookie_path /api/ /;
		   
		   #服务端接收的请求头Cooke值不变
		   proxy_set_header Cookie $http_cookie;
		}
	}
	
	#管理后台端(兼容HTTP)
	server{
	    listen 80;
		server_name manage.test.com;
		# 301重定向跳转到HTTPS接口
		return 301 https://$server_name/$request_uri;
		error_page 500 502 503 504 /50x.html;
		location = /50x.html{
			 root html	
		}
	}
	
	#管理后台端(兼容HTTPS)
	server{
	    listen 443 ssl;
		server_name manage.test.com;
		location / {
		    root /home/test/web/dist
			
			index /index.html;
			
			
			#语法：try_files 【$uri】 【 $uri/】 【参数】
			#当用户请求https://manage.test.com/login时，
			#一.如果配置了上面的默认index,会依次请求
			#1./home/test/web/dist/login       查找有没有login这个文件，没有的话
			#2./home/test/web/dist/index.html  有就直接返回
			
			#二.如果没有配置了上面的默认index或者配置了没有找到对应的资源,会依次请求
			#1./home/test/web/dist/login        查找有没有login这个文件，没有的话
			#2./home/test/web/dist/login/       查找有没有login这个目录，没有的话
		    #3.请求https://manage.test.com/index.html  nginx内部做了一个子请求
			
			#三.总的来说,index的优先级比try_files高,请求会先去找index配置,这里最后一个参数必须存在
			try_files $uri $uri/ /index.html;	
			
			
			
			#解决跨域问题
            #允许跨域请求地址(*表示全部,但是无法满足带cookie请求,因为cookie只能在当前域请求)
            add_header Access-Control-Allow-Origin $http_origin;
            #允许接收cookie和发送cookie
            add_header Access-Control-Allow-Credentials 'true';
            #允许请求的方法
            add_header Access-Control-Allow-Methods 'GET,POST,DELETE,PUT,OPTIONS';
            #允许请求头（Content-Type:请求数据/媒体类型 x-requested-with:判断请求是异步还是同步 自定义header 比如 token）
            add_header Access-Control-Allow-Headers $http_access_control_request_headers;
            #浏览器缓存请求头信息,1800秒内,只会有1次请求，不会出现"OPTIONS"预请求,节约资源
            #add_header Access-Control-Max-Age '1800';
		    if ($request_method = 'OPTIONS') {
                    return 204;
            }
			
			
			#服务端HttpServletRequest可以获得用户的真实ip
		    proxy_set_header X-Real-IP $remote_addr;
			
			#服务端HttpServletRequest可以获得用户的真实ip和经过的每一层代理服务器的ip
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
			
			#服务端接收的请求头Host值不变
            proxy_set_header Host  $http_host;
			
            proxy_set_header X-Nginx-Proxy true;
		}
	}
}
```



**结构图**

![image-20230902091436624](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230902091436624.png)





# 2 进阶

## 2.1 反向代理案例

需求：现在要求使用 nginx 完成一个反向代理，即在浏览器访问当前主机的 1000 端口时会重定向到 www.baidu.com

**01 搭建 JDK 和 Tomcat 环境**

> jdk 安装的参考文档：https://developer.aliyun.com/article/704959

这里选择相对简单的 open-jdk

1. 找到合适的版本

   ```bash
   apt-cache search openjdk
   ```

2. 安装 jdk（这里选择 openjdk 8）

   ```bash
   sudo apt-get install openjdk-8-jdk
   ```

3. 配置环境变量，编辑 .bashrc 文件

   ```bash
   vim ~/.bashrc
   ```

4. 添加如下配置

   ```
   export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
   export PATH=$JAVA_HOME/bin:$PATH
   export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
   ```



安装 Tomcat8

> Tomcat 9 安装参考文档：https://developer.aliyun.com/article/1016140?spm=a2c6h.14164896.0.0.35e51316G5Be4z

1. 上传 tomcat 安装文件

2. 进入到解压目录 /bin，启动 tomcat

   ```bash
   ./startup.sh
   ```

3. 开放端口 8080

   ```bash
   ufw allow 8080
   ```



**02 配置反向代理**

```ini
http {
    include       mime.types;
    default_type  application/octet-stream;

    sendfile        on;

    keepalive_timeout  65;

    server {
        listen       80;
        server_name  localhost;

        location / {
            root   html;
            index  index.html index.htm;
        }
    }

    server {
   		# 添加监听端口为 1000
        listen       1000;
        # 这里的 server_name 使用虚拟机的 ip 地址，不能添加 http:// 前缀
        server_name  192.168.88.129;

        location / {
            root        html;
            # 指定重定向的 url
            proxy_pass  http://www.baidu.com;
            index       index.html index.htm;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
}
```



:sparkles:**小技巧**

01 如何查看 nginx.conf 配置错误

```bash
# 检测默认配置文件
nginx -t

# 检测指定配置文件
nginx -tc xxx.conf
```



**:bug: bug 收集**

01 不支持 ssl 协议

```
nginx: [emerg] https protocol requires SSL support in /usr/local/nginx/./conf/nginx.conf:96
```

解决方案：为 nginx 安装 SSL 模块

> 参考文档：https://www.cnblogs.com/aerfazhe/p/15773667.html

先关闭 nginx 服务，然后开始重新配置 SSL 模块，步骤如下：

1. 进入 nginx 安装目录

   ```bash
   root@xzh:/usr/local# cd /usr/local/nginx-1.20.2
   ```

2. 重新配置 SSL 模块

   ```bash
   root@xzh:/usr/local/nginx-1.20.2# ./configure --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module
   ```

3. 报错：error: SSL modules require the OpenSSL library，下载缺少 <头文件.h>和<动态库文件.so及静态库文件.a>，只需要补回头文件和库文件即可

   ```bash
   root@xzh:/usr/local/nginx-1.20.2# sudo apt-get install libssl-dev
   ```

4. 再回到第 2 步重新配置 SSL 模块

5. 重新编译

   ```bash
   root@xzh:/usr/local/nginx-1.20.2# make
   ```

6. 在替换之前先备份

   ```bash
   root@xzh:/usr/local/nginx-1.20.2# cp /usr/local/nginx/sbin/nginx /usr/local/nginx/sbin/nginx.bak
   ```

7. 将刚编译好的 nginx 覆盖原有的 nginx 

   ```bash
   root@xzh:/usr/local/nginx-1.20.2# cp ./objs/nginx /usr/local/nginx/sbin
   ```

8. 通过查看版本命令判断 SSL 模块是否配置成功

   ```bash
   root@xzh:/usr/local/nginx-1.20.2# /usr/local/nginx/sbin/nginx -V
   ```

   > 显示如下内容即代表配置成功
   >
   > ```
   > configure arguments: --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module
   > ```



## 2.2 localtion 配置

> locatio 配置详细内容参考其他笔记和工具包中的文档





## 2.3 负载均衡

示意图

![image-20230902173024096](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230902173024096.png)



负载均衡：即将负载分摊到不同的服务单元，即保证服务的可用性，又提高了系统整体的性能

Linux 下有 Nginx、LVC、Haproxy 等服务可以提供负载均衡服务，Nginx 提供了下面几种分配策略：

1. 轮询（默认）：每个请求按照时间顺序逐一分配到不同的后端服务器，如果后端服务器 down 掉，能够自动剔除

2. weight 权重：weight 代表权重，默认为 1，权重越高被分配的客户端概率越大

   ```ini
   upstream cluster {
       server  192.168.88.129:8080 weight=10;
       server  192.168.88.129:8081 weight=1;
   }
   ```

3. ip_hash：每个请求按访问的 IP 的 hash 结果进行分配，这样每个访问固定一个后端服务器，可以解决在分布式系统中 session 共享的问题

   ```ini
   upstream cluster {
   	ip_hash;
       server  192.168.88.129:8080 weight=10;
       server  192.168.88.129:8081 weight=1;
   }
   ```

4. fair(第三方)：按后端服务器的响应时间来分配请求，响应时间短的优先分配

   ```ini
   upstream cluster {
       server  192.168.88.129:8080 weight=10;
       server  192.168.88.129:8081 weight=1;
       fair;
   }
   ```

   



**测试**

我们按照示意图进行测试程序，首先我们要在开启两个 Tomcat Server，一个端口咋 8080，一个在 8081

1. 复制一份 Tomcat 解压后的文件夹

   ```bash
   root@xzh:/home/xzh/apps# cp apache-tomcat-8.5.93 apache-tomcat-8.5.93-new -r
   ```

2. 修改 config/server.xml

   ```bash
   root@xzh:/home/xzh/apps/apache-tomcat-8.5.93-new/conf# vim server.xml
   ```

3. 需要修改的 xml 内容包括：默认的启动服务端口号，重定向端口号和关闭端口号

   ```xml
   <!-- 改动内容：port、redirectPort -->
   <Connector port="8081" protocol="HTTP/1.1"
                  connectionTimeout="20000"
                  redirectPort="8444"
                  maxParameterCount="1000"
                  />
   <!-- 改动内容：shutdown 指令对应的 port-->
   <Server port="8006" shutdown="SHUTDOWN">
   ```

4. 分别在 Tomcat8081 和 Tomcat8080 的 webapp 下创建

   ```bash
   root@xzh:/home/xzh/apps/apache-tomcat-8.5.93-new/webapps# mkdir search
   root@xzh:/home/xzh/apps/apache-tomcat-8.5.93-new/webapps# touch search/look.html
   ```

   添加内容

   ```html
   <h1>Tomcat 8081: look.html</h1>
   ```

   > Tomcat 8080 也是同样的道理





完成上述 Tomcat 的配置后，我们接下来在 nginx.conf 进行负载均衡的配置

1. 编辑 nginx.conf

   ```bash
   root@xzh:/usr/local/nginx# vim /usr/local/nginx/conf/nginx.conf
   ```

2. 编辑内容如下：

   ```ini
   http {
   	# 配置集群
       upstream cluster {
           server  192.168.88.129:8080;
           server  192.168.88.129:8081;
       }
   
       server {
           listen       80;
           server_name  localhost;
   
           location / {
               root        /search/;
               index       look.html look.htm;
               # 当请求 url 为当前虚拟机IP:80时，转发到 http://192.168.88.129:8080/search/look.html 或 http://192.168.88.129:8081/search/look.html
               proxy_pass  http://cluster;
           }
   	}
   }
   ```



:japanese_goblin:**注意事项**

1. nginx.conf 的 upstream 名不能带下划线，虽然语法检测不到，但是会匹配失败



:candy:**upstream 相关配置**

nginx 是一个反向代理软件，现在大部分网站都采用 nginx 作为网站 / 平台的服务器软件，nginx 除了可以直接作为 web 服务器之外，更多的情况是通过反向代理将请求转发给上游服务器

参考文档：https://zhuanlan.zhihu.com/p/409693332



## 2.4 动静分离

nginx 动静分离其实就是普通服务器的存放功能，只不过将静态资源放置在了 nginx 服务器上，示意图如下：

![image-20230902210011115](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230902210011115.png)

具体配置就是在 nginx/html 目录下创建 css、js、image 等存放静态资源的文件夹，然后将静态资源分门别类的存放，之后配置 nginx.conf

```ini
http {
    server {
        listen       80;
        server_name  localhost;

        location / {
            root   html;
            index  index.html index.htm;
        }
        
        location /css/ {
        	root /usr/local/nginx/html;
        }
        
        location /js/ {
        	root /usr/local/nginx/html;
        }
        
        location /image/ {
        	root /usr/local/nginx/html;
        }
    }
}
```



## 2.5 nginx 的 master-worker 机制

![image-20230902215737662](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230902215737662.png)

说明

- Nginx 采用的使 Master-worker 机制，一个 Master 管理多个 worker
- 每个 worker 都是一个单独的进程，其中单个 worker 与多个 clent 通过 IO 多路复用实现连接，从而做到高并发



**争夺机制**

![image-20230902221501307](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230902221501307.png)

- 一个 master process 管理多个 worker process，也就是说 Nginx 采用的是<strong style="color:red">多进程结构</strong>，而不是多线程结构

- 当 client 发出请求（任务）时，master process 会通过其下管理的 worker process 

- 于是 worker process 开始争抢任务，争抢到的 worker process 建立连接，从而完成任务

  > 每一个 worker 都是一个独立的进程，每个进程里只有一个独立的线程

- Nginx 采用了 IO 多路复用机制（需要在 Linux 环境），使用 IO 多路复用机制是  Nginx 在使用为数不多的 worker process 就可以实现高并发的关键



**"惊群现象" 的解决**

![image-20230902222125039](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230902222125039.png)

虽然 Nginx 的想法很好，希望通过多个 worker 争夺任务来实现快速响应，但是有一个严重的问题需要解决，那就是多个 worker 相互争夺时，只能有一个进程争夺成功，其他进程都要处于 "挂起" 状态，这对性能的消耗十分巨大，那么 Nginx 是如何解决这一问题的？

解决方案：<u>使用 accept_mutex 来解决</u>

1. 所有的子进程会继承父进程的 sockfd，当连接时，父进程会通知所有的子进程有新的任务进来，子进程开始争抢连接
2. 大量进程被激活后又被挂起，只有一个进程可以通过 `accept()` 这个方法得到连接
3. Nginx 在 `accept()` 这个方法上添加一把共享锁 ==accept_mutex==，每个 worker 在执行 `accept()` 之前都需要先获取锁，获取不到就放弃，如此便解决了 "惊群现象"



**使用 "多进程" 而不使用 "多线程" 的好处**

- 节省锁带来的开销，每个 worker 进程都是独立的进程，不共享资源，不需要加锁。并且在编程及问题的排查上会轻松很多。
- 独立进程，减少风险。可用独立的进程，可以让各个进程之间不会相互影响，一个进程中断，不会影响到其他进程，不会影响到服务





**IO 多路复用**

> 参考：http://dgrt.cn/news/5585930.html?action=onClick

关键：每进来一个request，会有一个worker进程去处理。但不是全程的处理，处理到什么程度呢？处理到可能发生阻塞的地方，比如向上游（后端）服务器转发request，并等待请求返回。那么，这个处理的worker不会这么傻等着，他会在发送完请求后，注册一个事件：“如果upstream返回了，告诉我一声，我再接着干”。于是他就休息去了。此时，如果再有request 进来，他就可以很快再按这种方式处理。而一旦上游服务器返回了，就会触发这个事件，worker才会来接手，这个request才会接着往下走。



## 2.6 work_process 

**worker 设置的数量**

每一个 worker 的线程可以将一个 CPU 的性能发挥到极致，所以结论是：<strong style='color:red'>worker 数和服务器的 CPU 数相等最为适宜</strong>

同时 Nginx 默认没有开启多核 CPU，可以通过增加 worker_cpu_affinity 配置参数来充分利用多 CPU 的性能

```ini
# 2个CPU内核
worker_processes 2;
worker_cpu_affinity 01 10;

# 4个CPU内核
worker_processes 4;
worker_cpu_affinity 1000 0100 0010 0001;

# 8个CUP内核
worker_processes 8;
worker_cpu_affinity 00000001 00000010 00000100 00001000 00010000 00100000 01000000 10000000;
```



worker_cpu_affinity 的理解


![image-20230902225421859](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230902225421859.png)



**worker_connection**

work_connection 表示 **每个 worker 进程能够建立连接的最大值**，因此一个 nginx 能够建立的最大连接数：worker_connections * worker_processes

> worker_connections 默认值是 1024，可以将其调大到 60000，同时要根据系统的最大打开文件数来调整 $系统的最大打开文件数>= worker\_connections*worker\_process$
>
> 查看系统最大打开文件数
>
> ```bash
> ulimit -a|grep "open files"
> ```



**最大连接数和并发的关系**

如果是支持 http1.1 的浏览器每次访问要占两个连接，所以普通的静态访问最大并发数是： worker_connections * worker_processes /2，而如果是 HTTP 作为反向代理来说，最大并发数量应该是 worker_connections * worker_processes/4。因为作为反向代理服务器，每个并发会建立与客户端的连接和与后端服务的连接，会占用两个连接。

![image-20230902230257985](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230902230257985.png)



## 2.7 配置 Linux 最大打开文件数

如果我们想要提高最大连接数，那么我们需要同时提高 Linux 的最大打开文件数

```bash
ulimit -a # 查看系统所有限制
```

示例结果如下：

```
core file size          (blocks, -c) 0
data seg size           (kbytes, -d) unlimited
scheduling priority             (-e) 0
file size               (blocks, -f) unlimited
pending signals                 (-i) 15196
max locked memory       (kbytes, -l) 65536
max memory size         (kbytes, -m) unlimited
open files                      (-n) 1024
pipe size            (512 bytes, -p) 8
POSIX message queues     (bytes, -q) 819200
real-time priority              (-r) 0
stack size              (kbytes, -s) 8192
cpu time               (seconds, -t) unlimited
max user processes              (-u) 15196
virtual memory          (kbytes, -v) unlimited
file locks                      (-x) unlimited
```

可以看到，如果我们想要知道系统打开文件最大限制数，可以用如下命令进行查看

```bash
ulimit -n
```



临时修改可打开的最大文件数

```bash
ulimit -n 65535
```

永久修改

1) 在 /etc/rc.local 中增加一行 

   ```bash
   ulimit -SHn 65535
   ```

2) 在 /etc/profile 中增加一行

   ```bash
   ulimit -SHn 65535
   ```

3) 在 /etc/security/limits.conf 最后增加如下两行记录

   ```bash
   soft nofile 65535
   hard nofile 65535
   ```

> CentOS 适用方式 3，Debian 适用方式 2（Ubuntu 是 Debian 的衍生版，也可以使用该方法）



## 2.8 搭建高可用集群

**示意图**

![image-20230903094519889](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230903094519889.png)

解析

1. 准备两台 nginx 服务器，一台做主服务器，另一台做从服务器
2. 两台 nginx 服务器之间通过 keepalived 保证通讯，同时对外提供一个统一访问的虚拟 IP 



首先，我们需要将原来的 vm 复制一份，然后按如下步骤修改 IP 地址（适用于 Ubuntu 20.04）

> 参考文档：https://cloud.tencent.com/developer/article/1933335

1. 查看当前主机的 IP 地址

   ```bash
   ifconfig
   ```

   > 如果 `ifconfig` 命令无法使用，那么需要我们使用 `sudo apt install net-tools -y` 命令安装 net-tools 工具

   ![image-20230903105107126](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230903105107126.png)

2. 再通过如下命令得到网关地址

   ```bash
   route -n
   ```

   ![](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230903105249807.png)



得到上述信息后，我们开始修改配置文件

1. 进入配置文件夹

   ```bash
   xzh@xzh:~$ cd /etc/netplan/
   ```

2. 配置旧配置文件

   ```bash
   sudo cp 00-installer-config.yaml  00-installer-config.yaml_before
   ```

3. 修改配置文件

   ```bash
   xzh@xzh:/etc/netplan$ sudo vim 00-installer-config.yaml
   ```

   更改后的文件内容如下：

   ```yaml
   network:
     ethernets:
       ens33:                             			# 网卡名称
         dhcp4: no						   			# 关闭动态主机配置协议
         dhcp6: no
         addresses: [192.168.88.130/24]   			# 静态ip
         gateway4: 192.168.88.2		   			# 网关
         nameservers:
           addresses: [8.8.8.8, 114.114.114.114] 	# dns
     version: 2
   ```

4. 最后一步：使配置生效

   ```bash
   sudo netplan apply
   ```



之后，我们再对 Tomcat 和 Nginx 的配置文件做相应修改即可，接下来是关键核心：在两台 Linxu 主机上部署 keepalived 2.0.20

第一步，到[官网](https://www.keepalived.org/download.html)找到对应的下载源



第二步，将压缩包移入到 /home/usr/apps/ 下，并进行解压到 /usr/local/ 下

```bash
root@xzh:/home/xzh/apps# tar -zxvf keepalived-2.0.20.tar.gz -C /usr/local/
```



第三步，切换目录并进行配置相关参数

```bash
root@xzh:/home/xzh/apps# cd /usr/local/keepalived-2.0.20/
# 尾缀参数说明：将配置文件放在 /etc 下，将安装文件放在 /usr/local 下
root@xzh:/usr/local/keepalived-2.0.20# ./configure --sysconf=/etc --prefix=/usr/local
```



第四步，编译并安装

```bash
root@xzh:/usr/local/keepalived-2.0.20# make && make install
```



说明，两台机子都必须安装 keepalived，同时需要说明的是

- keepalived 的配置目录在 /etc/keepalived/keepalived.conf
- keepalived 的启动指令在 /usr/local/sbin/keepalived



安装完成后，接下来我们需要完成高可用集群配置

1. 将其中一台主机（例如 192.168.88.129）指定为 Master

   ```bash
   root@xzh:/usr/local/keepalived-2.0.20# vim /etc/keepalived/keepalived.conf
   ```

2. 修改相应的配置

   ```ini
   global_defs {
      notification_email {
        acassen@firewall.loc
        failover@firewall.loc
        sysadmin@firewall.loc
      }
      notification_email_from Alexandre.Cassen@firewall.loc
      smtp_server 192.168.200.1
      smtp_connect_timeout 30
      # router_id 指定当前机器主机名，由程序员指定，不重复即可
      router_id MASTER_VM
      vrrp_skip_check_adv_addr
      # vrrp_strict 是严格执行 vrrp 协议，需要注销，否则后面不能访问虚拟 IP：vip
      # vrrp_strict
      vrrp_garp_interval 0
      vrrp_gna_interval 0
   }
   
   vrrp_instance VI_1 {
   	# 由于这里设置的是主服务器，所以 state 使用 MASTER 即可
       state MASTER
       # interface 指定网卡，这里需要修改为自己的虚拟网卡
       interface ens33
       virtual_router_id 51
       # 下面的 priority 是关键，表示优先级，是决定主服务器还是从服务器的关键，主服务器 priority 的值设置高些
       priority 100
       advert_int 1
       authentication {
           auth_type PASS
           auth_pass 1111
       }
       virtual_ipaddress {
       	# virtual_ipaddress 就是 vip，对外统一提供的 ip 地址
           192.168.88.10
       }
   }
   ```

3. 修改另一台服务器作为从服务器（例如 192.168.88.130）

   ```ini
   global_defs {
      notification_email {
        acassen@firewall.loc
        failover@firewall.loc
        sysadmin@firewall.loc
      }
      notification_email_from Alexandre.Cassen@firewall.loc
      smtp_server 192.168.200.1
      smtp_connect_timeout 30
      # 修改 router_id
      router_id BACKUP_VM
      vrrp_skip_check_adv_addr
      # vrrp_strict
      vrrp_garp_interval 0
      vrrp_gna_interval 0
   }
   
   vrrp_instance VI_1 {
   	# 由于是从服务器，所以这里 state 修改为 BACKUP
       state BACKUP
       # 依然指定网卡名
       interface ens33
       virtual_router_id 51
       # 调整优先级
       priority 50
       advert_int 1
       authentication {
           auth_type PASS
           auth_pass 1111
       }
       virtual_ipaddress {
       	# vip 仍然是 192.168.88.10
           192.168.88.10
       }
   }
   ```

4. 开启启动两台 Linux 的 keepalived，执行指令

   ```bash
   root@xzh:/usr/local# ./sbin/keepalived
   ```

5. 最后一步：验证是否绑定成功

   ```bash
   ip a
   ```

   效果图如下表示绑定成功

   ![image-20230903153344745](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230903153344745.png)
