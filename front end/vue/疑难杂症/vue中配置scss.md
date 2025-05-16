在 Vue 项目中引入 scss 的方法

1. 安装 sass 的依赖包

   ```bash
   //在开发环境安装依赖
   npm install --save-dev sass-loader
   ```

2. 安装 loader 解析器

   ```bash
   //在开发环境安装依赖
   npm install --save-dev node-sass
   ```

3. 在 vue.vonfig.js 中配置 sass

   ```js
   module.exports = {
     css: {
       loaderOptions: {
         sass: {
           data: `@import "@/styles/main.scss";`
         }
       }
     }
   };
   //由于sass-loader的版本不同，不同的版本对应的关键字：  
   //sass-loader v8-中，关键字为 “ data ”  
   //sass-loader v8中，关键字为 “ prependData ”  
   //sass-loader v10+中，关键字为 “ additionalData ”
   ```

   > 这个代码将把`main.scss`文件导入到你Vue3项目的所有组件中





