> 来源：https://zhuanlan.zhihu.com/p/409693332

# 1. 介绍

## 1.1 介绍

Nginx是一个反向代理软件，大部分的网站都采用Nginx作为网站/平台的服务器软件。Nginx除了可以直接作为web服务器使用外，更多的情况是通过反向代理将请求转发给上游服务器。

配置上游服务器可以使用upstream进行设置，通过upstream可以实现服务的负载均衡规则，可以提高服务器的高可用性。

# 2. 语法

## 2.1 基本语法

upstream的基本语法如下，一个upstream需要设置一个名称，这个名称可以在server里面当作proxy主机使用。

```ini
upstream default {
    server  php-fpm-tfphp:9000;
}123
```

一个upstream可以设置多个server，通常情况下Nginx会轮询每一个server，从而达到最基本的负载循环效果。

```ini
upstream default {
    server  tflinux_php-fpm-tfphp_1:9000;
    server  tflinux_php-fpm-tfphp_2:9000;
}1234
```

## 2.2 max_fails

max_fails是最多出错数量，可以为每一个server设置一个max_fails，如果请求server发生了错误则max_fails会加一，如果请求server错误次数达到了max_fails后，Nginx会标记这个server为故障状态，后面就不会再去请求它了。

默认情况下，max_fails的次数是1次。

```ini
upstream default {
    server  tflinux_php-fpm-tfphp_1:9000 max_fails=5;
    server  tflinux_php-fpm-tfphp_2:9000 max_fails=3;
}1234
```

## 2.3 fail_timeout

fail_timeout是故障等待超时时间，前面说过了max_fails是请求server错误次数，如果达到了max_fails次数之后server会被标记为故障状态，那么多长时间会重新尝试呢？这个fail_timeout就是这个时间了，在达到max_fails次数之后server进入故障状态，而后在fail_timeout时间之后会被重新标记为正常状态。

默认情况下，fail_timeout的时间是10秒。

```ini
upstream default {
    server  tflinux_php-fpm-tfphp_1:9000 max_fails=5 fail_timeout=100;
    server  tflinux_php-fpm-tfphp_2:9000 max_fails=3 fail_timeout=60;
}1234
```

## 2.4 proxy_connect_timeout 

这个proxy_connect_timeout是连接超时时间，如果连接不到就会报错了。

```ini
proxy_connect_timeout 3s;1
```

## 2.5 proxy_next_upstream_tries

这个proxy_next_upstream_tries是一个upstream反向代理的重试次数，简单说就是如果请求server出错的次数达到了proxy_next_upstream_tries的次数的话，即使没有达到max_fails的次数，即使后面还有没有尝试过的server，都不会再继续尝试了，而是直接报错。

```ini
proxy_next_upstream_tries 3;1
```

## 2.6 proxy_next_upstream_timeout

这个proxy_next_upstream_timeout是一个upstream反向代理的故障等待时间，简单说就是无论upstream内部如何进行重试，所有花费的时间加在一起达到了proxy_next_upstream_timeout时间的话，就会直接报错，不会再继续尝试了。

```ini
proxy_next_upstream_timeout 60s;1
```

## 2.7 backup 

这个是备用服务器参数，可以为一个upstream设置一个backup的server，在生产server全部都出问题之后，可以自动切换到备用server上，为回复服务争取时间。

backup的server不同于其他server，平时是不承载请求的，所以它应该是比较空闲的状态，应急再合适不过了~~

```ini
upstream default {
    server  tflinux_php-fpm-tfphp_1:9000 max_fails=5 fail_timeout=100;
    server  tflinux_php-fpm-tfphp_2:9000 max_fails=3 fail_timeout=60;
    server  tflinux_php-fpm-tfphp_3:9000 backup;
}12345
```

# 3. 示例

福哥给出一个示例，这是TFLinux的TFPHP的配置参数，包括了今天学习的全部知识。

```ini
proxy_connect_timeout 3s;
proxy_next_upstream_timeout 60s;
proxy_next_upstream_tries 3;

upstream default {
    server  tflinux_php-fpm-tfphp_1:9000 max_fails=5 fail_timeout=100;
    server  tflinux_php-fpm-tfphp_2:9000 max_fails=3 fail_timeout=60;
    server  tflinux_php-fpm-tfphp_3:9000 backup;
}123456789
```