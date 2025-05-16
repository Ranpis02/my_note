[toc]

# 1 基本入门

## 1.1 搭建环境

官方文档：https://nuxt.com.cn/

- node 环境：16.20

- nuxt 环境：3.6.5

命令

```shell
npx nuxi@latest init <project-name>
```

> `npx` 是一个 npm 包的执行者，相较于 npm，npx 会自动安装依赖包并执行某个命令，也就是说上述命令等价于
>
> ```shell
> npm i nuxi@latest -g
> nuxi init <project-name>
> ```

或者

```shell
pnpm dlx nuxi@latest init <project-name>
```



:warning:注意

上面安装可能会报错，如果报错，那么应该是 DNS 污染的问题，我们可以修改自己的 hosts 文件

访问网址：https://sites.ipaddress.com/raw.githubusercontent.com/，得到映射的 DNS

![image-20230816165213767](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230816165213767.png)

之后到 hosts 文件中粘贴如下内容即可（windows 中 hosts 文件位置：C:\Windows\System32\drivers\etc\hosts）

```
# nuxt Start
185.199.108.133 raw.githubusercontent.com
185.199.109.133 raw.githubusercontent.com
185.199.110.133 raw.githubusercontent.com
185.199.111.133 raw.githubusercontent.com
# nuxt End
```

如果仍然不行，可以直接下载脚手架的安装包，下载地址如下：

https://codeload.github.com/nuxt/starter/tar.gz/refs/heads/v3



---



安装完成之后，我们安装相关依赖

```bash
# yarn(recommand)
yarn install

# npm
npm install

# pnpm 
pnpm install
```

> 事先确定镜像
>
> ```bash
> # yarn 
> yarn config get registry
> yarn config set registry https://registry.npmmirror.com/
> 
> # npm & pnpm 
> npm config get registry
> npm config set registry https://registry.npmmirror.com/
> ```
>
> 

启动调试

```bash
yarn dev -o
```

> `-o` 指令可以帮助我们打开默认浏览器

安装 Volar 插件，该插件是为 Vue3 定制的

> 同时如果之前安装过 Vetur 插件，请将其禁用，该插件为 Vue2 定制的，会与 Volar 产生冲突



## 1.2 UI 库选型

