[toc]

# 1 three.js 基础

## 1.1 本地搭建three.js官网网站

> [three.js 中文文档](https://www.three3d.cn/docs/index.html#manual/zh/introduction/Installation)

进入 three.js 的 github 仓库，然后将 dev 分支 `clone` 到本地，之后下载安装好依赖，通过 `npm run dev` 命令启动，将看到如下界面：

![image-20230423205400397](../../../../AppData/Roaming/Typora/typora-user-images/image-20230423205400397.png)

完成上述步骤后，那么恭喜你，成功搭建 three.js 官网网站



## 1.2 使用 parcel 搭建 three.js 开发环境

> [parcel 官网](https://www.parceljs.cn/)
>
> parcel 和 webpack 一样，都属于 web 应用的打包工具

我们新建一个空的项目，然后初始化项目，再安装 parcel

```bash
yarn add parcel-bundler --dev
```

替换 package.json 中的脚本命令，如下：

```json
{
  "scripts": {
    "dev": "parcel <your entry file>",
    "build": "parcel build <your entry file>"
  }
}
```

> 这里的入口文件，我们可以新建 src/index.html，将其设置为入口文件，即
>
> ```json
> {
>   "scripts": {
>     "dev": "parcel src/index.html",
>     "build": "parcel build src/index.html"
>   }
> }
> ```

我们初步搭建的源文件目录结构如下：

```
src                 
├─ assets           
│  ├─ css           
│  │  └─ style.css  
│  └─ images        
├─ main             
│  └─ main.js       
└─ index.html       
```

在 index.html 中引入 style.css 和 main.js 进行测试，测试成功即搭建完成



