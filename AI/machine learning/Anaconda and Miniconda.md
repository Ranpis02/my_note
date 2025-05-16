# 1 Anaconda是什么？

Anaconda 是一个数据科学和机器学习的软件套装，它包含了许多工具和库。

Anaconda 包及其依赖项和环境的管理工具为 `conda` 命令。

与传统的 `Python pip` 工具相比 Anaconda 的 `conda` 可以更方便地在不同环境之间进行切换，环境管理较为简单，类似于 nvm 管理 node 和 vagrant 管理 linux。



# 2 如何安装配置 Miniconda?

我们在这里就不使用 Anaconda, 换它的命令行版本 Miniconda.

1. 下载地址：https://docs.anaconda.com/miniconda/

2. 勾选上添加 Miniconda3 到环境变量以及将 Miniconda3 作为我们默认的 python

   ![image-20240911175752795](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20240911175752795.png)

3. 更换镜像，参考文档：https://mirrors.tuna.tsinghua.edu.cn/help/anaconda/



# 3 conda 常用操作

## 3.1 操作环境

1. 查看已创建的环境

   ```shell
   conda info --env
   # OR
   conda env list
   ```


2. 删除环境

   ```shell
   conda remove -n <env_name> --all
   ```

3. 激活环境

   ```python
   conda activate <env_name>
   ```



## 3.2 换源

1. 查看源

   ```shell
   conda config --show
   ```

2. 更换源

   ```shell
   # 更换中科大源
   conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/main/
   conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/free/
   conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/conda-forge/
   conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/msys2/
   conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/bioconda/
   conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/menpo/
   conda config --set show_channel_urls yes
   
   # 更换清华源
   conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free
   conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
   conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/ 
   conda config --set show_channel_urls yes
   ```

3. 删除源

   ```shell
   conda config --remove-key channels
   ```

   > [!warning]
   >
   > 如果没有删除成功，可以到 `C:\Users\${user_name}` 下查看 `.condarc` 文件，可以将文件或内容删除



# 4 Miniforge

Miniforce 和 Miniconda 类似，最主要的区别在于

1. Miniforce 是由社区驱动，完全开源，并且将 `conda-forge` 设置为默认并且唯一的 channel
2. Miniconda 是由 Anaconda 公司提供，属于商业产品，存在版权问题

官方仓库：https://github.com/conda-forge/miniforge

在安装完成之后，我们可以使用如下命令查看使用的 channel

```shell
conda config --get channels
```
