# 1 Linux 基础

## 1.1 初识 Linux

Linux 的创始人：<u>Linus Benedict Torvalds</u>

Linux 是在 UNIX 的基础之上进行改良的

UNIX 的取自 Multi 和 Uni 两个英文单词前缀，**Multi 代表的是大的意思，大而繁；而 Uni 是小的意思，小而巧**，UNIX 开始是由 C汇编语言编写的，但是移植性差，后来 UNIX 的创始人 肯·汤姆森  的同事 丹尼斯·里奇 在创造了 C 语言后，就将 UNIX 的大部分源码改用了 C 语言进行编写

但是 UNIX 是商业软件，而 Linux 是开源的、免费的、公开的，其 logo 是一只南极洲的企鹅

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221105204957647.png" alt="image-20221105204957647" style="zoom: 25%;" />



## 1.2 Linux 的内核

 Linux 的内核的组成如下：

1. [Linux 系统内核](https://www.kernel.org/)
   - 提供了最核心的功能，如：调度 CPU、调度内存、调度文件系统、调度网络通讯、调度 IO等
2. 系统级应用程序
   - 出厂自带的程序，可供用户快速上手操作系统，如文件管理器、任务管理器、图片查看、音乐播放等



**Linux 发行版**

内核是免费的、开源的，这也就是代表了：

- 任何人都可以获得并修改内核，并且自行集成系统级程序
- 提供了内核 + **系统级程序** 的完整封装，称之为 Linux 发布版

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221105212452720.png" alt="image-20221105212452720" style="zoom:50%;" />

任何人都可以封装 Linux ，目前市面上非常多的 Linux 发行版，最常用、知名的如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221105212616003.png" alt="image-20221105212616003" style="zoom: 25%;" />

不同的发行版：

- 基础命令是完全相同的
- 部分操作不同（如：软件安装）



## 1.3 虚拟机

虚拟机：**通过软件，模拟计算机硬件，并给虚拟硬件安装真实的操作系统**



**安装虚拟机**

市面上，常见的虚拟机有如下几类：

- [VMware WorkStation](https://www.vmware.com/cn/products/workstation-pro/workstation-pro-evaluation.html)
- [Virtual Box](https://www.virtualbox.org/)

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221105214256086.png" alt="image-20221105214256086" style="zoom:33%;" />

安装过程以后补上，这里要特别注意的是，当我们在安装完虚拟机之后，要去观察我们的网卡是否多了 VMnet1 和 VMnet8 两个虚拟网卡，具体查看如下：

通过 win + R 打开 window 运行面板，输入 `ncpa.cpl` 打开网络连接进行查看  

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221105215406849.png" alt="image-20221105215406849" style="zoom:50%;" />



## 1.4 在虚拟机中安装 linux 操作系统

首先，我们需要下载操作系统的安装文件，本次使用 CentOS7.6 版本进行学习：

[下载传送入口](https://vault.centos.org/7.6.1810/isos/x86_64/)

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221105230127135.png" alt="image-20221105230127135" style="zoom:50%;" />

> 这里安装请使用典型模式安装，自定义安装好像会出问题



## 1.5 远程连接 Linux 操作系统

对于操作系统的使用，有两种使用形式：

- 图形化界面：使用操作系统提供的图形化界面，以获得<strong style="color:red">图形化反馈</strong>的形式去使用操作系统
- 命令行界面：使用操作系统提供的各类命令，以获得<strong style="color:red">字符反馈</strong>的的形式去使用操作系统

而在 Linux  操作系统中，无论是企业还是个人开发，多数使用的是：==命令行==

> 在开发中，少使用命令行形式，更加直观，并且资源占用低，程序运行更加稳定



所以我们这时候可以使用 远程登录工具 去操作 Linux 终端，这里我们推荐使用 MobaXterm 去进行操控，连接方式如下：

1. 打开虚拟机，输入命令 `ip addr` 或 `ifconfig`

   <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221106083345633.png" alt="image-20221106083345633" style="zoom:25%;" />

   > 这里需要注意：在 Ubuntu 20.04 中，默认是没有安装 `ifconfig` 的，我们只能使用 `ip addr` 命令查看 ens33

2. 找到虚拟网卡 en33 对应的 inet 地址即可

3. 在 windows 中新建打开 MobaXterm ，选择新建 SSH 会话

   <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221106083602781.png" alt="image-20221106083602781" style="zoom:33%;" />

4. 输入远程的 host 地址即可完成连接



## 1.6 配置 WSL

WSL 作为 windows10 系统带来的全新特性，正在逐步颠覆开发人员既有的选择

- 传统方式获取 Linux 操作系统环境，是安装完整的虚拟机
- 使用 WSL，可以以轻量化的方式，得到 Linux 的系统环境

WSL：Windows Subsystem for Linux，是用于 Windows 系统之上的 Linux 子系统，作用就是在 Windows 系统中获得 Linux 系统环境，并完直连计算机硬件

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221106083813893.png" alt="image-20221106083813893" style="zoom:33%;" />



**WSL 部署**

1. 找到【程序与功能】

   <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221106084725918.png" alt="image-20221106084725918" style="zoom: 33%;" />

2. 选择【启动或关闭 Windows 功能】，勾选【适用于 Linux 的 Windows 子系统】

   <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221106084833522.png" alt="image-20221106084833522" style="zoom:33%;" />

3. 启用后重启计算机

4. 再到微软商店里下载 Ubuntu

   <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221106085004792.png" alt="image-20221106085004792" style="zoom:33%;" />





## 1.7 虚拟机快照

在学习阶段我们无法避免的可能破坏 Linux 操作系统，如果损坏的话，则需要重新安装一个 Linux 操作系统就会十分困难，这时候我们需要拍摄快照来将当前虚拟机的状态保存下来，以后可以通过快照来恢复虚拟机到保存的快照的状态

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221106093113332.png" alt="image-20221106093113332" style="zoom:25%;" />

注意事项：

1. 确保虚拟机关机后，再进行拍摄快照



# 2 Linux 命令

## 2.1 Linux 目录结构

Linux 的目录结构是一个==树型结构==

- 在 Linux 系统中，路径之间的层级关系，使用反斜杆 `/` 表示
- 在 Windows 系统中，路径之间的层级关系，使用斜杆 `\` 表示



Linux 的目录结构（即 FHS，英文全称 Filesystem Hierarchy Standard，文件系统层次化标准）如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221106093924137.png" alt="image-20221106093924137" style="zoom: 50%;" />

最上面的一层目录就是根目录，**根目录下面的子目录我们称之为一级目录，一级目录下面的目录我们称之为二级目录**

**根目录是最为重要的目录**，原因如下两点：

1. 所有的目录都是从根目录衍生出来的
2. 根目录与系统的开机、修复、还原密切相关

**一级目录的重要性仅次于根目录，其中很多的核心文件都在一级目录下**

| 一级目录 | 功能                                                 |
| -------- | ---------------------------------------------------- |
| /bin     | 存放系统命令                                         |
| /boot    | 系统启动目录，保存与系统相关的文件                   |
| /dev     | 设备保存位置                                         |
| /etc     | 配置文件保存位置                                     |
| /home    | 普通用户的家目录                                     |
| /root    | root 用户的家目录                                    |
| /lib     | 系统调用的函数库保存位置                             |
| /mnt     | 挂载目录                                             |
| /sbin    | 保存与系统环境设置相关的命令，只有 root 用户可以使用 |
| /tmp     | 临时目录，系统存放临时文件的地方                     |
| /usr     | 全称为 Unix SoftWare Resource，用于存储系统软件资源  |
| /var     | 用于存储动态数据                                     |



## 2.2 Linux 命令基础

Linux 中命令的通用格式：

```shell
command [-options] [parameter]
```

- command：命令本身
- -options：可选，命令选项，控制命令的行为细节
- parameter：可选，命令的参数，多用于命令的指向目标等



## 2.3 ls 命令

`ls` 命令的语法如下：

```shell
ls [-a -l -h] [url]
```

- 不带选项时，默认使用平铺的形式显示当前工作目录下的内容

- 使用 `-a` ，能显示出隐藏文件 / 文件夹（以 `.` 开头）

- 使用 `-l` ，以列表 list 的形式（竖向排列）展示内容，并展示更多的信息

  <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221106103902407.png" alt="image-20221106103902407" style="zoom: 20%;" />

- 使用 `-h`，表示以易于阅读的形式，列出文件大小，如：K、M、G（`-h`选项需要搭配`-l`选项一起使用）



## 2.4 目录切换相关命令（cd / pwd）

`cd` 命令的作用是<font color="red">切换工作目录</font>，它是 "<font color="red">C</font>hange <font color="red">D</font>irectory" 的缩写，语法格式如下：

```shell
cd [url]
```

- `cd` 命令没有选项，只有参数，表示要切换到那个目录下

- 当我们不加参数时，默认切换到当前用户的家目录

- `cd` 命令后面可以跟特殊符号，主要有如下特殊符号

  | 特殊字符  | 说明                     |
  | --------- | ------------------------ |
  | `.`       | 当前工作目录             |
  | `..`      | 代表上级目录             |
  | `-`       | 代表上次目录             |
  | `~`       | 代表当前登录用户的主目录 |
  | `~用户名` | 代表切换至用户的主目录   |



`pwd` 命令的作用是打印工作目录，是英文 "<font color="red">P</font>rint <font color="red">W</font>orking <font color="red">D</font>irectory" 的缩写，语法如下：

```shell
pwd
```

> `pwd` 命令无选项，也无参数



## 2.5 相对路径和绝对路径

- 相对路径：以==当前目录==为起点，到另一个目录的路径
- 绝对路径：以==根目录==为起点，到另一个目录的路径，路劲以 `/` 开头



## 2.6 创建目录命令

通过 `mkdir` 命令可以创建新的目录，该命令来自英文 "<font color="red">M</font>a<font color="red">k</font>e <font color="red">Dir</font>ectory"，语法如下：

```shell
mkdir [-p] url
```

- 参数必填，表示要创建的文件夹的路径
- `-p` 可选，表示自动创建不存在的父目录，适用于创建连续多层级的目录

:warning:注意：<font color="red">创建文件夹需要修改权限，如果我们在 Home 以外的目录下创建文件夹则会涉及权限问题</font>



## 2.7 文件操作命令 part1（touch、cat、more）

`touch` 创建文件，语法如下：

```shell
touch url
```

> `touch` 命令无选项，参数必填，表示要创建的文件路径



`cat` 命令用于查看文件内容，语法如下：

```shell
cat url
```



`more` 命令查看文件内容，语法如下：

```shell
more url
```



:bell:`more` 与 `cat` 命令的区别

- `more` 支持翻页，如果文件内容过多，可以一页一页地显示，通过空格进行翻页，按`q`退出查看
- `cat` 是直接将内容全部显示出来

:herb:拓展：查看文件常用命令

1. 查看文件类型

   ```shell
   file url
   ```

2. 查看文本内容

   ```shell
   # 1. cat 命令
   cat url
   
   # 2. cat 显示行编号
   cat -n url
   
   # 3. cat 非空行加上编号输出
   cat -b url
   ```

3. 分页查看

   ```shell
   # 1. more 
   more url
   
   # 2. less(升级版的 more)
   less url
   ```

4. 查看部分文件

   ```shell
   # 1. head(默认显示文件前 10 行)
   head -n [number] url
   
   # 2. 显示文件前 10 个字节
   head -c 10 url
   
   # 3. tail(默认显示文件后 10 行)
   tail -n [number] url
   
   # 4. 显示文件后 10 个字节
   tail -c 10 url
   ```

   

## 2.8 文件操作命令 part2(cp、mv、rm)

`cp` 命令用于**复制（拷贝）文件 / 文件夹**，是英文单词 "<font color="red">c</font>o<font color="red">p</font>y" 的缩写，语法如下：

```shell
cp [-r] source_url dist_url
```

- `-r` 选项，用于复制文件夹使用，表示递归
- 参数1，Linux 路径，表示被复制的文件或文件夹
- 参数2，Linux 路径，表示要复制去的地方

:warning:注意：<font color="red">复制文件夹，必须使用 `-r` 选项，否则不生效</font>



`mv` 命令可以用于**移动文件 / 文件夹**，是英文单词 "move" 的缩写，语法如下：

```shell
mv source_url dist_url
```



`rm` 命令可以用于**删除文件 / 文件夹**，是英文单词 "remove" 的缩写，语法如下：

```shell
rm [-r -f] 参数1 参数2 参数3…
```

- `-f` 表示 force，强制删除（不会弹出确认信息）
  - 普通用户删除内容不会弹出提示，只有 root 用户删除内容时才会有提示
  - 一般用户用不到 `-f` 选项
- 参数1、参数2、… 表示要删除的文件或文件夹路径

> - `rm` 可以搭配通配符使用，实现批量删除
> - `rm` 是一个危险的命令，特别是处于 root 模式下，千万不要执行 `rm -rf /` 或 `rm -rf /*`



## 2.9 查看命令（which、find）

**which 命令**

我们在前面学习的 Linux 命令，其实本体就是一个二进制可执行程序，我们通过 `which` 命令<font color="red">查看所使用的一系列命令的程序文件位置</font>

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221106160624372.png" alt="image-20221106160624372" style="zoom:33%;" />



**find 命令**

`find` 命令的作用是<font color="red">按文件名查找文件</font>，语法如下：

```shell
find 起始路径 -name "被查找文件名"
```

例如：我们要从根目录 `/` 下查找文件名为 test 的文件

```shell
find / -name "test"
```

同样地，我们可以将 find 命令搭配通配符 `*` 一起使用

```shell
# 1. 查找任何以 test 开头的文件
find / -name "test*"

# 2. 查找任何以 test 结尾的文件
find / -name "*test"

# 3. 查找任何包含 test 的文件
find / -name "*test*"
```



`find`命令还可以<font color="red">按文件大小查找文件</font>，语法如下：

```shell
find 起始路径 -size +|- [kMG]
```

- `+`、`-` 代表大于和小于
- `n` 代表大小数字
- `kMG` 代表单位

示例：

```shell
# 1. 查找小于 10kb 的文件
find / -size -10k

# 2. 查找大于 100 MB 的文件
find / -size +100M

# 3. 查找大于 1GB 的文件
find / -size +1G
```



## 2.10 grep、wc 和管道符

`grep` 命令<font color="red">用于从文件中通过关键字过滤文件行</font>，语法如下：

```shell
grep [-n] 关键字 文件路径
```

- `-n`：可选，表示在结果中显示匹配的行的行号
- 参数1：表示要过滤的关键字，带有空格或其它特殊符号时，建议使用 " " 将关键字包围起来
- 参数2：文件路径，表示内容的输入端口



`wc` 命令<font color="red">做数量统计</font>，语法如下：

```shell
wc [-c -m -l -w] url
```

- `-c`：统计字节数量
- `-m`：统计字符数量
- `-l`：统计行数
- `-w`：统计单词数量



**管道符**

管道符：`|`，其含义为：<font color="red">将管道符左边命令的结果，作为右边的输入</font>

示例如下：

```shell
# 1. 过滤 ls 的结果
ls | grep Desktop

# 2. 过滤 find 的结果
find / -name "test" | grep "/usr/lib64"

# 3. 过滤 cat 的结果
cat test | grep "itcast"
```



## 2.10 echo、tail 和重定向符

**`echo` 命令**用于在命令行内输出指定内容，语法如下：

```shell
echo 输出内容
```

例如：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221106170311316.png" alt="image-20221106170311316" style="zoom:33%;" />

**反引号**

Linux 中的反引号用于命令替换（或者说解析变量）

例如，有些字符是由特定的含义的，如果我们我们想要用 `echo` 显示当前的工作路径，我们如果直接使用 `echo pwd`，结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221106170727457.png" alt="image-20221106170727457" style="zoom:33%;" />

可以看到，显示的内容是 pwd，而不是我们期望的当前的工作路径，这个时候我们就需要用到反引号，如下：

```shell
echo `pwd`
```

显示结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221106171052201.png" alt="image-20221106171052201" style="zoom:33%;" />

**重定向符**

重定向符主要有：`>` 和 `>>`

- `>` 将左侧命令的结果，<font color="red">覆盖</font>写入右侧指定的文件中
- `>>` 将左侧命令的结果，<font color="red">追加</font>写入到右侧指定的文件中



**tail 命令**

使用 `tail` 命令，可以查看文件尾部内部，跟踪文件的最新更改，语法如下：

```shell
tail [-f -num] url
```

- 参数 url，表示被跟踪的文件路径
- 选项 `-f`，表示持续跟踪
- 选项 `-num`，表示查看尾部多少行，默认是后10 行



## 2.10 vi / vim 编辑器

**vi / vim** 是 "visual interface" 的简称，是 Linux 中最经典的文本编辑器

> vim 是 vi 的加强版本，兼容 vi 的所有指令，不仅能编辑文本，而且还具有 shell 程序编辑的功能，可以用不同颜色的文本来辨别语法的正确性，极大方便了程序的设计和编辑性



**vi / vim 编辑器的三种工作模式** 

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221106181526215.png" alt="image-20221106181526215" style="zoom:33%;" />

1. 命令模式（Command mode）

   命令模式下，所敲的按键编辑器都理解为命令，以命令驱动执行不同的功能。此模式下，不能自由进行文本编辑

2. 输入模式（Insert mode）

   也可以称之为编辑模式、插入模式。此模式下，可以对文件内容进行自由编辑

3. 底线模式（Last line mode）

   以`:`开始，通常用于文件的保存、退出

> 我们通过 vi / vim 编辑器编辑的文本如果存在则直接打开，如果不存在会自动创建一个空文件，再打开文件



**命令模式下的一些快捷键**

| 命令 | 描述                           |
| ---- | ------------------------------ |
| `i`  | 在当前光标位置进入输入模式     |
| `a`  | 在当前光标位置之后进入输入模式 |
|      |                                |
|      |                                |
|      |                                |
|      |                                |
|      |                                |
|      |                                |







## 2.11 查看帮助和手册

`--help` 选项，任何命令都支持 `--help` 这个选项，用来查看命令的帮助

例如：

```shell
# 1. 查看 ls 的帮助文档
ls --help

# 2. 查看 cd 的帮助文档
cd --help
```

如果想看查看命令的详细手册，可以通过 man（manual，手册）命令来进行查看

 例如：

```shell
# 1. 查看 ls 命令的详细手册
man ls

# 2. 查看 cd 命令的详细手册
man cd
```



# 3 Linux 用户和权限

## 3.1 认识 root 用户

**root 用户即超级管理员**

无论是 Windows、MacOS、Linux 均采用多用户的管理模式进行权限管理，在 Linux 系统中，拥有最大权限的账户名为 root



## 3.2 su 和 exit 命令

`su` 命令就是用于**账户切换**的系统命令，是英文单词 "Switch User" 的简写形式，语法如下：

```shell
su [-] [用户名]
```

- `-` 符号是可选的，表示是否在切换用户后加载环境变量（建议带上）
- 参数：用户名，表示要切换的用户，如果不加参数，默认切换到 root 用户
- 切换用户后，可以通过 `exit` 命令退回上一个用户，也可以使用快捷键 Ctrl + D



## 3.3 sudo 命令

在我们得知 root 用户密码后，可以通过 su 命令切换到 root 获得最大权限，但是**不建议长期使用 root 用户，避免对系统造成损坏**，我们可以使用 `sudo` 命令进行替换，表示<font color="red">普通的命令授权，临时以 root 身份执行</font>，语法如下：

```shell
sudo command
```

:warning:注意：

1. `sudo` 命令用于临时为普通命令添加 root 权限
2. 并不是所有的用户都有权利使用 `sudo` ，我们需要为普通用户==配置 sudo 认证==

配置流程如下：

1. 切换 root 用户，执行 `visudo` 命令，也可以使用 vim 命令打开 /etc/sudoers

   ```shell
   # 方式1
   visudo
   
   # 方式2
   vim /etc/sudoers
   ```

2. 在文件末尾添加下面一行语句

   ```shell
   USER_NAME ALL=(ALL)   NOPASSWD:ALL
   
   # USER_NAME 换成自己想要定义的用户名
   ```

3. 通过 wq 进行保存

注意事项：

1. 如果我们不为用户配置 sudo 认证，是无法使用 sudo 命令的

   <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221106214158713.png" alt="image-20221106214158713" style="zoom:33%;" />



## 3.4 用户和用户组

Linux 系统中可以：

- 配置多个用户
- 配置多个用户组
- 用户可以加入多个用户组中

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221106214735898.png" alt="image-20221106214735898" style="zoom:25%;" />

Linux 中关于权限的管控级别有两个级别

- 针对**用户**的权限控制
- 针对**用户组**的权限控制



**用户组管理**

- 创建用户组

  ```shell
  groupadd 用户组名
  ```

- 删除用户组

  ```shell
  groupdel 用户组名
  ```



**用户管理**

**创建用户**

```shell
useradd [-g -d] 用户名
```

- `-g` 指定用户的组，不指定 `-g` ，会创建同名组并自动加入，若指定了 `-g`，则必须要存在该组
- `-d` 指定用户 HOME 路径，不指定，HOME 目录默认在`/home/用户名`

示例如下：

```shell
# 1. 创建一个 test01 的用户,指定其分配在 group01 中
useradd test01 -g group01

# 2. 创建一个 test02 的用户，将其 home 目录放在 /home/newHome 中
useradd test02 -d /home/newHome
```



**删除用户**

```shell
userdel [-r] 用户名
```

- 选项：`-r` 用于表示是否删除用户的 HOME 目录，若不使用该选项则默认会保留其 HOME 目录



**查看用户所属的组**

```shell
id [用户名]
```

- 参数：用户名，被查看的用户，如果不提供则查看自身



**修改用户所属的组**

```shell
usermod -aG 用户组 用户名
```

例如：

```shell
# 将 test03 加入到 group02 这个组中
usermod -aG group02 test03
```



**查看当前系统中所有的用户**

```shell
getent passwd
```

例如：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221106234943231.png" alt="image-20221106234943231" style="zoom:33%;" />

每一行代表一个用户，从左到右一共有 7 份信息，分别是：

1. 用户名
2. 密码（x）
3. 用户 ID
4. 组 ID
5. 描述信息（无用）
6. HOME目录
7. 执行终端（默认 bash）



**查看当前系统所有的用户组**

```bash
getent group
```

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221107000001507.png" alt="image-20221107000001507" style="zoom:33%;" />

从左到右共三份信息：

1. 组名称
2. 组认证（显示为 x）
3. 组 ID



## 3.5 认知权限信息

通过 `ls -l` 命令可以以列表的形式查看内容，并显示权限细节

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221107000446003.png" alt="image-20221107000446003" style="zoom:33%;" />

- 序号1，表示文件、文件夹的权限控制信息
- 序号2，表示文件、文件夹所属用户
- 序号3，表示文件、文件夹所属的用户组

序号1 的权限细节可以分为 10 个槽位，如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221107000619750.png" alt="image-20221107000619750" style="zoom:50%;" />

- r：读权限（权重4）
- w：修改权限（权重2）
- x：执行权限（权重1）
  - 针对文件表示将文件作为程序执行
  - 针对文件夹表示可以更改工作目录到此文件夹



## 3.6 修改权限控制 - chmod

我们可以使用 `chmod` 命令去修改文件、文件夹的权限信息

<font color="red">特别注意：只有文件、文件夹的所属用户或者 root 用户可以修改</font>

语法如下：

```shell
chmod [-R] 权限 文件或文件夹
```

- 选项 `-R` ，对文件夹内的全部内容应用相同的操作

示例如下：

```shell
# 1. 将 hello 文件的所属用户权限设置为可执行，将文件所属组的权限设置为可写，将其他用户的权限设置为可读
chmod u=x,g=w,o=r hello

# 2. 将 test 文件夹中所有文件（包括 test 文件夹）的权限设置上面的权限设置
chmod -R u=x,g=w,o=r test
```

权限的设置和表示我们还可以使用数字权限设定法，即二进制表示法，7（111） 代表可读、可写、可执行，4（100）代表可读，其他依次类推

**修改权限符号**

| 符号 | 含义     |
| ---- | -------- |
| =    | 重置     |
| +    | 增加权限 |
| -    | 删除权限 |



## 3.7 修改权限控制2 - chown

我们前面使用 `chmod` 命令可以修改文件或文件夹的读、写、执行等权限，接下来我们来讲解 `chown` 命令，该命令<font color="red">可以用来修改文件所属的用户和用户组</font>，语法如下：

```shell
chown [-R] [用户] [:] [用户组] 文件或文件夹
```

- `-R` 选项，同 `chmod` 命令，是用来将文件夹内全部内容全都应用相同的规则
- 选项，用户，修改所属的用户
- `:` 用来分割用户和用户组
- 选项，用户组，修改所属的用户组

示例如下：

```shell
# 1. 将 hello 文件所属的用户修改为 root
chown root hello 

# 2. 将 hello 文件所属的用户组修改为 root
chown :root hello

# 3. 将 hello 文件所属的用户组和用户全都修改为 root
chown root:root hello
```



# 4 Linux 实用操作

## 4.1 常用的快捷键

1. `Ctrl + C` 用于强制停止命令的执行

2. `Ctrl + D` 用于退出或登出

   > 不能用于退出 vi / vim 编辑器

3. `history`：历史命令搜索

4. `!命令前缀`：自动执行上一次匹配前缀的命令

5. `Ctrl + R`：输入内容去匹配历史命令

   > 回车键可以直接执行
   >
   > 键盘左右键，可以得到该命令，但不执行

6. 光标移动快捷键

   - `Ctrl + A`：跳到命令开头
   - `Ctrl + E`：跳到命令结尾
   - `Ctrl + 键盘左键`：向左跳一个单词
   - `Ctrl + 键盘右键`：向右跳一个单词

7. 清屏

   ```shell
   # 方式1
   Ctrl + L
   
   # 方式2
   clear
   
   # 方式3
   reset
   ```

   



## 4.2 软件安装

CentOS 中安装软件的命令是 `yum`

Ubuntu 中安装软件的命令是 `apt`

**Linux 系统的应用商店**

- 下载安装包自行安装
  - win 系统中所使用的是.exe 文件、.msi 文件
  - mac 系统中所使用的是 dmg 文件、pkg 文件
- 系统的应用商店
  - win 系统中有 Microsoft Store
  - mac 系统中有 AppS

Linux 中同样支持这两种安装方式

**CentOS 的安装方式 - 通过 yum 命令**

yum：RPM 包软件管理器，用于自动化安装配置 Linux  软件，并可以自动解决依赖问题，语法如下：

```shell
yum [-y] [install | remove | search] 软件命令
```

- 选项 `-y`，自动确认，无需手动确认安装或卸载过程
- `install`：安装
- `remove`：卸载
- `search`：搜索

:warning:注意：

1. `yum` 命令需要 root 权限
2. `yum` 命令需要联网下载

由于 CentOS 或者 Ubuntu 的下载都是从国外的服务器下载的，所以我们要进行换源，这里以 CentOS 7 为例：

```shell
# 方式1
wget -O /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo

# 方式2
curl -o /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo
```



我们拿安装、下载和搜索 wget 程序为例

```shell
# 1. 安装 wget 程序
yum install -y wget

# 2. 卸载 wget 程序
yum remove -y wget

# 3. 搜索 wget 程序源
yum search wget
```



**Ubuntu 的 apt 安装命令**

`apt` 语法如下：

```shell
apt [-y] [install | remove | search] 软件名称
```

- `apt` 命令和 `yum` 命令一样同样需要 root 权限



## 4.3 systemctl 命令

Linux 很多软件（内置的或第三方）均支持使用 systemctl 命令控制：<font color="red">启动、停止、开机自启</font>，能够被 systemctl 管理的软件，一般也称之为：==服务==，语法如下：

```shell
systemctl start | stop | status | enable | disable 服务名
```

- stop ：关闭
- start：启动
- statu：查看状态
- enable：开启开机自启
- disable：关闭开启自启

系统内置的服务比较多，比如：

- NetworkManager：主网络服务
- network：副网络服务
- firewalld：防火墙服务
- sshd：ssh 服务（远程管理工具使用的就是该服务）

除了内置的服务以外，部分第三方软件安装后也可以使用 systemctl 进行控制

```shell
# 1. 安装 ntp 服务
yum install -y ntp

# 2. 安装 apahe 服务器软件
yum install -y httpd
```

> 部分软件安装后并没有集成到 systemctl 中，我们需要手动添加



:herb:拓展：为什么 Linux 中所有的服务都是 `d` 结尾的？

答：`d` 代表 deamon（守护进程），Linux 的大多数服务就是用守护进程实现，<font color="red">守护进程是运行在 Linux 服务器后台的一种服务程序，用于周期性地执行某种任务或等待处理某些事件</font>

## 4.4 软链接和硬链接

`ln` 命令用来<u>创建硬链接和软链接</u>，即为某个文件在另外一个位置建立同步的链接，Linux 中的链接文件有两种形式，一种是硬链接（hard link），另一种是软链接（symbolic link）

### 4.4.1 硬链接

<u>硬链接</u>是通过对文件的 inode 属性块进行赋值，当用户修改 A 或 B 文件，另外一个文件的内容也会发生改变，创建硬链接的语法如下：

```shell
# 为 file1 创建软链接
ln file1 file2
```

特点如下：

1. 硬链接不限于两个文件之间，可以在多个文件之间进行，`ls -l` 显示了文件的硬链接数
2. 不能对目录创建硬链接
3. 不能在不同的文件系统之间做硬链接（Linux 的文件系统：ext4、xfs 等）
4. 所有的硬链接，具备相同的 inode 节点号

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221107160337106.png" alt="image-20221107160337106" style="zoom:33%;" />



### 4.4.2 软链接

创建语法如下：

```shell
# 为 file1 创建软链接文件
ln -s file1 file2
```

<u>软链接</u>相当于 windows 系统中的快捷方式文件，<font color="red">原始文件被移动或删除后，软链接文件也无法使用</font>，特点如下：

1. 如果源文件被删除了，将无法使用该软链接
2. 当我们使用 `ls -l` 后，软链接作为一种特殊的文件类型显示出来，其第一个字母为 `l`
3. 软链接的大小是其链接文件的路径名中的字符数

:bell:获取链接的源文件路径

如果我们为一个文件夹创建了软链接，当我们进入这个软链接后，使用 `pwd` 命令，那么显示的链接的路径，如果我们想要显示链接的源文件的路径，那么需要使用`pwd -P` 命令



**删除软链接**

如果软链接，连接到的是是一个目录，正确的删除方式是：

```shell
rm dir
```

切记不要使用如下命令，否则会将目录中所有的文件删除

```shell
rm -rf dir/
```



## 4.5 日期、时区

通过 `date` 命令可以<font color="red">在命令行中查看系统的时间</font>，语法如下：

```shell
date [-d] [+格式化字符串]
```

- `-d` 按照给定的字符串显示日期，一般用于日期计算

- 格式化字符串：通过**特定的字符串标记来控制显示的日期格式**

  | 符号 | 含义                                                |
  | ---- | --------------------------------------------------- |
  | `%Y` | 年                                                  |
  | `%y` | 年份后两位数字（00…99）                             |
  | `%m` | 月份（01…12）                                       |
  | `%d` | 日（01…31）                                         |
  | `%H` | 小时（00…23）                                       |
  | `%M` | 分钟（00…59）                                       |
  | `%S` | 秒（00…60）                                         |
  | `%s` | 自 1970-01-01 00:00:00 UTC 到现在的秒数（即时间戳） |

示例如下：

```shell
# 1. 获取当前时间
date

# 2. 获取当前时间并且进行格式化
date "+%Y-%m-%d %H:%M:%S"
```



`-d` 选项用于日期计算，其中支持的时间标记为：

1. year
2. month
3. day
4. hour
5. minute
6. second

示例如下：

```shell
# 1. 获取当前时间的后一天，并格式化输出
date -d "+1 day" "+%Y-%m-%d %H:%M:%S"

# 2. 或当前时间的前一年，并格式化输出
date -d "-1 year" "+%Y-%m-%d %H:%M:%S"
```



**修改 Linux 时区**

我们会发现，如果我们没有经过设置，通过 date 查看的时间是不准确的，这是因为：<font color="red">系统默认时区并不是中国的东八区的时间</font>

这时候我们就需要<font color="red">使用 root 权限，执行如下命令</font>，修改时区为东八区时间：

```shell
# 1. 将系统自带的 localtime 文件删除
sudo rm -f /etc/localtime

# 2. 将 /usr/share/zoneinfo/Asia/Shanghai 文件创建软链接为 localtime 文件
sudo ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```



我们还可以<font color="red">通过 ntp 程序自动校准系统时间</font>

1. 安装 ntp 服务

   ```shell
   yum -y install ntp
   ```

2. 启动并设置为开机自启

   ```shell
   # 1. 启动 ntpd 服务
   systemctl start ntpd
   
   # 2. 设置为开机自启
   systemctl enable ntpd
   ```



## 4.6 IP 地址、主机名

**IP 地址**

每一台联网的电脑又会有一个地址，用于和其他计算机进行通讯

IP 地址主要有两个版本，V4 版本和 V6 版本（V6 很少使用，这里暂不涉及）

IPV4 版本的地址格式是：`a:b:c:d`，其中a、b、c、d 表示  0~255 的数字

可以通过 `ifconfig` 命令来查看本机的 IP 地址，也可以通过 `ip addr` 来进行查看，如果无法使用 `ifocnfig` 命令，可以通过如下命令安装 net-tools 来解决：

```shell
yum -y install net-tools
```

 在 CentOS 中，主网卡地址为 ens 中的 inet 

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221107182445795.png" alt="image-20221107182445795" style="zoom:33%;" />

其中，另外的两个网卡：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221107182607992.png" alt="image-20221107182607992" style="zoom:33%;" />

- lo：本地回环网卡
- virbr0：虚拟网卡



**特殊 IP 地址**

除了标准的 IP 地址以外，还有几个特殊的 IP 地址需要去了解：

1. `127.0.0.1`：用于指代本机
2. `0.0.0.0`：特殊 IP 地址
   - 可以用于指代本机
   - 可以在端口绑定中来确定绑定关系
   - 在一些 IP 地址限制中，表示所有 IP 的意思，如放行规则设置为 0.0.0.0，表示允许任意 IP 访问



**主机名**

每一台电脑除了对外联络地址（IP 地址）以外，也可以有一个名字，称之为主机名

Linux 系统主机名可以通过如下命令获取：

```shell
hostname
```

显示效果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221107205357467.png" alt="image-20221107205357467" style="zoom:33%;" />



**修改主机名**

我们可以如下命令实现修改主机名：

```shell
hostnamectl set-hostname centos
```

重启启动之后，即可看到主机名是否正确显示



**域名解析**

由于 IP 地址是一组数字，比较难记，我们可以通过主机名进行替换，<u>域名 和 IP 地址之间的关系</u>类似于人类社会中的 <u>身份证 和 姓名之间的关系</u>，我们在访问百度时主要还是通过 `www.baidu.com` 来进行访问的，而不是通过其 IP 地址 `14.215.177.38` 来进行访问

访问百度的流程如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221107210814409.png" alt="image-20221107210814409" style="zoom:33%;" />

DNS 服务器常见的域名有：`8.8.8.8` 和 `114.114.114.114`



**虚拟机配置固定 IP**

:question:为什么我们需要固定  IP 地址，这是因为：

当前我们虚拟机的 Linux 操作系统，其 IP 地址是用过 DHCP 服务来获取的，如果 IP 地址经常变更，那个我们需要不断变换其主机与 IP 地址的映射关系，所以我们需要将 IP 地址固定下来

> DHCP：Dynamic Host Configuration Protocol，即动态主机配置协议，每次重启设备之后都会获取一次，可能导致 IP 地址频繁变更



**在  VMware Workstation 中配置固定 IP**

配置固定 IP 需要两个大步骤：

1. 在 VMware Workstation（或 Fushion）中配置 IP 地址网关和网段（IP 地址的范围）
2. 在 Linux 系统中手动修改配置文件，固定 IP



**第一步，编辑虚拟网络**

1. 打开 Vware 中 【编辑】 → 【虚拟网络编辑器】

2. 按下图先配置 DHCP

   <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221107215844026.png" alt="image-20221107215844026" style="zoom: 50%;" />

3. 设置 NAT

   <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221107215922508.png" alt="image-20221107215922508" style="zoom: 50%;" />

**第二步，在 Linux 系统中修改固定 IP**

通过如下命令编辑主网卡的文件

```shell
vim /etc/sysconfig/network-scripts/ifcfg-ens33
```

修改内容：将 BOOTPRO 的属性值设置为 `static`

增加内容：

```shell
IPADDR="192.168.88.130"	# IP 地址
NETMASK="255.255.255.0" # 子网掩码地址
GATEWAY="192.168.88.2"  # 网关和 Vware 虚拟编辑器中的网关保持一致
DNS1="192.168.88.2"	# DNS1 设置为网关即可，Vware 会自动帮助我们进行域名解析
```

设置如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221107232404403.png" alt="image-20221107232404403" style="zoom:25%;" />

编辑完成之后重启可以设置成功，或者使用如下命令将网卡重启

```shell
systemctl restart network 
```

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221107232459020.png" alt="image-20221107232459020" style="zoom:25%;" />



## 4.7 网络请求和下载

**ping 命令**

`ping` 命令用于<font color="red">检查网络服务器是否是可连通状态</font>，语法如下：

```shell
ping [-c num] ip或主机名
```

- 选项`-c`代表检查次数，如果不使用 `-c` 命令，将无限次持续检查
- 参数 ip 或主机名地址表示被检查的服务器地址

```shell
# 检查 baidu.com 是否连通
ping baidu.com
```

执行效果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221107233644227.png" alt="image-20221107233644227" style="zoom:33%;" />

可以看到，我这里延迟还是比较高的，平均 40 ms 左右



**wget 命令**

`wget` 是<font color="red">非交互式的文件下载器，可以在命令行内下载网络文件</font>，语法如下：

```shell
wget [-b] url
```

- `-b` ：可选，表示后台下载，会将日志写入到当前工作目录的 wget-log 文件中
- url：下载链接

:warning:<font color="red">注意：无论下载是否完成，都会生成要下载的文件，如果下载未完成，请及时清理未完成的不可用文件</font>

我们可以通过如下命令，对后台下载文件进行监控

```shell
tail -f wget-log
```



**curl 命令**

`curl`可以<font color>发送 http 网络请求</font>，可用于：<u>下载文件、获取信息</u>等，语法如下：

```shell
curl [-o] url
```

- `-o`：用于下载文件，当 url 是下载链接时，可以使用此选项保存文件



## 4.8 端口

端口，是<font color="red">设备与外界通讯交流的出入口</font>，端口可以分为：<u>物理端口和虚拟端口两类</u>

- 物理端口：又可称之为接口，是可见的端口，如 USB 接口、HDMI 接口、RJ45 网口等
- 虚拟端口：是指计算机内部的端口，是不可见的，是用来操作系统和外部进行交互使用的

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221108000707556.png" alt="image-20221108000707556" style="zoom:33%;" />

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221108000715626.png" alt="image-20221108000715626" style="zoom:33%;" />

计算机程序之间的通讯，通过 IP只能锁定计算机，但是无法锁定具体的程序，我们如果想要锁定具体的程序，需要用到端口，这里有一个很形象的比喻：

- "IP地址相当于小区地址，在小区内可以有许多住户（程序），而门牌号（端口）就是各个住户（程序）的联系地址"

  <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221108001011637.png" alt="image-20221108001011637" style="zoom:25%;" />



Linux 系统是可以支持 65535 个端口，这么多的端口可以分为 3 类进行使用：

- 公认端口：1~1023，通常用于一些系统内置或知名程序的预留使用，如 SSH 服务的端口号是 22，HTTPS 服务的端口号是 443

  > 非特殊需要，不要占用这个区间的端口

- 注册端口：1024~49151，通常可以随意使用，用于松散地绑定一些程序 / 服务

- 动态端口：49152~65535，通常不会固定绑定程序，而是当程序对外进行网络连接时，用于临时使用



**查看端口的占用**

使用 `nmap` 命令，可以查看端口号的占用情况

```shell
# 1. 安装 nmap 程序
yum -y install nmap

# 2. 查看 IP 地址
nmap IP地址
```

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221108002302148.png" alt="image-20221108002302148" style="zoom:33%;" />

> 根据上图我们可以看到本地 IP 地址一共有 5 个端口号被占用了，其中端口号 22 一般是给 ssh 服务使用的



**查看端口号占用情况**

可以通过 `netstat` 命令来查看指定的端口占用情况，语法如下：

```shell
# 1. 安装 netstat 服务
yum -y install net-tools

# 2. 查看 xxx 端口号的占用情况
netstat -anp | grep 端口号
```



## 4.9 进程管理

程序是运行在操作系统中，是被操作系统所管理的，为管理运行的程序，每个程序在运行的时候，便被操作系统注册为系统中一个进程，并为其分配一个独有的 ID：==进程ID==

**查看进程**

我们如果想要在 Linux 中查看进程信息，可以通过 `ps` 命令进行查看，该命令是 "process status" 的缩写

```shell
ps [-e -f]
```

- `-e`：显示出全部的进程
- `-f`：以完全格式化的形式展示信息

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221108151749226.png" alt="image-20221108151749226" style="zoom:25%;" />

从左到右，依次代表着：

- UID：进程所属的用户 ID
- PID：进程的进程号 ID
- PPID：进程的父ID（启动此进程的其他进程）
- C：进程的 CPU 占用率百分比
- STIME：进程的启动时间
- TTY：启动的终端序号，如果显示`?`则代表非终端
- TIME：进程占用 CPU 的时间
- CMD：进程对应的名称或启动路径或启动命令



**关闭进程**

关闭进程的语法如下：

```shell
kill [-9] 进程ID
```

- `-9` 表示强制关闭进程，不使用此选项会向进程发送关闭信号要求关闭，但是否关闭取决于进程自身的处理机制



## 4.10 Linux 文件的上传和下载

我们可以直接通过 MobaXterm 进行虚拟机上的文件和本地电脑的文件的上传和下载

这里需要注意的是：我们使用 MobaXterm 默认是以普通用户的身份登录的，这也就代表着我们无法查看需要 root 权限相关的文件了，自然也就无法下载和上传，如果我们想以 root 用户身份去登录，则需要进行如下配置：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221108004500934.png" alt="image-20221108004500934" style="zoom:33%;" />



当然，除了这种简单的拖拽来实现上传下载之外，我们还可以通过 `rz`、`rs` 命令来实现上传和下载

首先，我们需要安装好 `rz`、`rs` 命令相关的服务

```shell
yum -y install lrzsz
```

- `sz` 命令的使用

  ```shell
  sz 文件名
  ```

  > 经过测试，`sz`命令在 MobaXterm 会报错，如下：
  >
  > <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221108005536824.png" alt="image-20221108005536824" style="zoom:33%;" />
  >
  > 这里由于 MobaXterm 不支持 lszrz 包，我们可以换成 ftp 连接或者使用 finalshell 或者 xshell

- `rz` 命令的使用

  ```shell
  rz
  ```

  之后会弹出文件管理器，选择要上传的文件

> 这里不推荐使用这两个命令，不好用而且速度慢，一般情况下，我们直接通过拖拽进行上传和下载



## 4.11 压缩、解压

市面上，有许多格式的压缩，例如：zip、rar、7zip 等，但是在 Linux 中最常见的压缩格式是：<u>tar、gzip、zip</u> 这三种格式

- tar：也称之为 tarball，归档文件，即简单地将文件组装到一个 .tar 的文件内，并没有太多的体积的减少，仅仅是简单的封装，所以<font color="red">我们利用 tar 程序封装文件的过程也叫作打包</font>
- gz：最常见的是 .tar.gz 为后缀的压缩包，gzip 格式压缩的文件，即使用 gzip 压缩算法将文件压缩到一定的体积，<font color="red">利用 gz 程序压缩的过程我们叫做压缩</font>

针对这两种格式，使用 tar 命令均可以使用压缩和解压缩的操作，语法如下：

```shell
tar [-c -V -X -f -z -C] 参数1 参数2 参数3…
```

- `-c`：*创建压缩文件，用于压缩模式
- `-v`：显示压缩、解压缩过程，用于查看进度
- `-x`：*解压模式
- `-f`：要创建的文件，或要解压的文件，`-f`必选放在所有选项的最后
- `-z`：gzip 模式，不使用 `-z` 就是普通的 tarball 格式
- `-C`：选择解压的目的地，用于解压模式

示例如下：

```shell
# 1. 将 1.txt, 2.txt, 3.txt 文件压缩到 test.tar 文件内
tar -cvf test.tar 1.txt 2.txt 3.txt

# 2. 将 1.txt, 2.txt, 3.txt 文件压缩到 test.gz.tar 文件内
tar -zcvf test.tar.gz 1.txt 2.txt 3.txt

# 3. 将 test.tar 解包到 /home/Desktop 目录中
tar -xvf test.tar -C /home/Desktop

# 4. 将test.tar.gz 解压到 /home/Destop 目录中
tar -zxcf test.tar.gz -C /home/Desktop
```



**zip 命令压缩文件**

在 Linux 中也可以使用 zip 命令，压缩文件为 zip 压缩包，语法如下：

```shell
zip [-r] 参数1 参数2…
```

- `-r`：被压缩的包含文件夹的时候，需要使用 `-r` 选项

示例如下：

```shell
# 1. 压缩文件
zip test.zip a.txt b.txt…


# 2. 压缩文件夹
zip -r test.zip dir
```



**uzip 命令解压文件**

使用 uzip 命令，<font color="red">可以方便地解压 zip 压缩包</font>，语法如下：

```shell
uzip [-d] 参数
```

- `-d`，指定要解压去的位置，通 `tar` 命令的 `-C` 选项
- 参数，被解压的 zip 压缩包文件

示例如下：

```shell
# 1. 将 test.zip 解压到当前目录
uzip test.zip 

# 2. 将 test.zip 解压到指定文件夹
uzip test.zip -d /home/xzh
```



## 4.12 环境变量

环境变量是<font color="red">操作系统在运行的时候，记录的一些关键性的信息，用以辅助系统运行</font>

- 在 Linux 和 Mac 系统中，可以通过 `env` 命令还查看当前系统中记录的环境变量
- 在 windows 系统中，通过 `set` 命令来查看环境变量
- 环境变量是一种 key-value 型结构，即名称和值

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221108143235297.png" alt="image-20221108143235297" style="zoom:33%;" />

我们之所以可以在任意地方执行命令，例如：`ls`、`cd` 等等，就是因为 PATH 这个环境变量的作用，我们通过如下命令进行过滤一下：

```shell
env | grep PATH
```

> :warning:注意：grep 对大小写敏感，一定要查找的是大写的 PATH

![image-20221108144223125](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221108144223125.png)

- 在环境变量 PATH 中，我们可以可以看到有一个 `/usr/bin` 的属性值，里面存放的就是我们常用的命令程序



**`$`符号**

在 Linux 系统中，`$`符号被用于取 "变量" 的值

例如，如果我们想要打印 PATH 的值，那么我们可以通过如下命令来实现：

```shell
# 方式1
echo $PATH

# 方式2
echo ${PATH}
```

**自行设置环境变量**

Linux 环境变量可以自行设置，其中分为：

1. 临时设置，语法：`export 变量名 = 变量值`

2. 永久生效

   - 针对当前用户生效，配置在当前用户的 `~/.bashrc` 文件中

   - 针对所有用户生效，配置在系统的 `/etc/profile` 文件中

   - 配置完成之后，通过如下命令进行全局配置，立即生效，或重启生效

     ```shell
     source 配置文件
     ```

**自定义环境变量 PATH**

如果我们自己写了一个程序，当我们想要将其设置为全局有效，我们可以进行如下操作：

1. 在某个 url 中编写一个脚本文件

2. 修改其权限，将其设置为可执行

3. 修改 /etc/profile 文件，在末行添加以下内容：

   ```shell
   export PATH=${PATH}:url
   ```

4. 之后通过 `source` 进行全局配置即可生效

   ```shell
   source /etc/profile
   ```



