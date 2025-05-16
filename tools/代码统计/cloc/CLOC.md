# 1 下载安装

Cloc 的官网：[传送门](https://cloc.sourceforge.net/)

Cloc 的 Github：[传送门](https://github.com/AlDanial/cloc)

如果电脑里面安装了 node.js ，可以使用 npm 快速安装，当然这也是最为推荐的一种方式，window 用户的安装命令如下：

```bash
npm install cloc -g
```

:warning:**`npm` 无法下载原因**

1. **使用国外的服务器**

   当我们使用国外的服务器时，由于国家之间的 Internet 使用海底光缆连接，所以速度缓慢，这个时候需要更换镜像源

   - 更换方式1

     ```bash
     # 1. 查看当前代理
     npm get registry
     
     # 2. 更换镜像源(这里以国内淘宝镜像为例)
     npm config set registry https://registry.npm.taobao.org
     ```

   - 更换方式2

     ```bash
     # 1. 列出当前可用镜像源
     nrm ls
     
     # 2. nrm 更换镜像
     nrm use xxx
     ```

     <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221219185609046.png" alt="image-20221219185609046" style="zoom: 67%;" />

2. `node` 和 `npm` 版本不匹配

   如果 `node` 和 `npm` 版本不匹配也会出现一些问题，这里列出官网查看映射关系：[传送门](https://nodejs.org/zh-cn/download/releases/)

   ```bash
   # 查看node版本号
   node -v
   
   # 查看npm版本号
   npm -v
   ```

   ![image-20221219185831149](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221219185831149.png)

3. 使用了代理（例如：如果你使用了 `devsider` 这个工具进行代理）

   ```bash
   # 1. 查看代理（如果不是 null 则说明配置了代理，需要删除）
   npm config get proxy
   
   # 2. 删除代理
   npm config rm proxy 
   npm config rm https-proxy
   ```

   > 有些时候，需要我们手动清除 npm 缓存：`npm cache clean --force`（当然，node 版本太高时这个命令会被禁止，需要降低版本后使用）

当以上方法都无效时，我们可以考虑使用 `cnpm` 命令，该命令为中国 `npm` 镜像客户端的命令，只要在国内都可以使用**，但是不推荐，在做项目的时候可能会出现些许问题**



# 2 使用方法

1. 配置检测工作目录路径

   ```bash
   cd xxx
   ```

2. 统计工作目录中的代码量

   ```bash
   cloc .
   ```

如果出现和我一样的小伙伴，那么请老老实实安装exe程序版本的吧，报错提示信息：

```
Error: EPERM: operation not permitted, mkdir 'C:\Program Files\nodejs\node_modules\.cloc-GAftn4Ib'
```

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221219192220301.png" alt="image-20221219192220301" style="zoom:50%;" />



---



# 3 下载可执行文件

exe 程序下载：[官网传送门](https://github.com/AlDanial/cloc/releases)

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221219192314769.png" alt="image-20221219192314769" style="zoom:50%;" />



下载后之后，<strong style="color:red">重命名为 `cloc`，并将程序放到工作目录的 src 文件夹下</strong>

![image-20221219192920547](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221219192920547.png)

打开 cmd 命令窗口，输入以下命令进行统计

```bash
cloc .
```

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221219193041077.png" alt="image-20221219193041077" style="zoom:50%;" />

