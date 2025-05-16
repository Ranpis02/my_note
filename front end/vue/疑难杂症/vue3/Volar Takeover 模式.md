> 参考文档：https://juejin.cn/post/7231944822100803642

引入：为了让 Vue 单文件组件和 TypeScript 一起工作，Volar 创建了一个针对 Vue 的 TS 语言服务实例，将其用于 Vue 单文件组件。同时，普通的 TS 文件依然由 VSCode 内置的 TS 语言服务来处理。这也是为什么我们需要安装 TypeScript Vue Plugin 来支持在 TS 文件中引入 Vue 单文件组件。

这套默认设置能够工作，但在每个项目里我们都运行了两个语言服务实例：<u>一个来自 Volar，一个来自 VSCode 的内置服务</u>。这在大型项目里可能会带来一些性能问题。



为了优化性能，Volar 提供了一个叫做 "Takeover 模式" 的功能。在这个模式下，Volar 能够使用一个 TS 语言服务实例同时为 Vue 和 TS 文件提供支持。



开启 volar 插件的 "Takeover 模式"，步骤如下：

01 首先，我们需要在项目的工作空间中禁用 VSCODE 的内置 TS 语言服务

1. 在当前项目的工作空间下，用 `Ctrl + Shift + P` (macOS：`Cmd + Shift + P`) 唤起命令面板。
2. 输入 `built`，然后选择 "Extensions：Show Built-in Extensions"。
3. 在插件搜索框内输入 `typescript` (不要删除 `@builtin` 前缀)。
4. 点击 "TypeScript and JavaScript Language Features" 右下角的小齿轮，然后选择 "Disable (Workspace)"。
5. 重新加载工作空间。Takeover 模式将会在你打开一个 Vue 或者 TS 文件时自动启用。

> 或者直接扩展市场搜索 `@builtin typescript`



02 配置当项目的 ts 检测工具，点击右下角的带有查号的 `{}` 图标，然后再进行配置，选择 volar 即可

![image-20230904232915765](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230904232915765.png)

![image-20230904233118399](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230904233118399.png)