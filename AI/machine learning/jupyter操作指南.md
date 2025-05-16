# 1 安装

1. 安装 jupyter notebook

   ```shell
   conda install -y jupyter
   ```

   > [!tip]
   >
   > 其中 `-y` 代表自动确认

2. 将 conda 环境添加到 jupyter 内核中

   ```shell
   conda install ipykernel 
    
   python -m ipykernel install --name ${conda_env_name}
   ```
   



:point_right: **附加内容**​

1. 查看内核环境列表

   ```shell
   jupyter kernelspec list
   ```

2. 删除指定内核环境

   ```shell
   jupyter kernelspec uninstall ${kernel_name}
   ```

   > [!caution]
   >
   > 该命令可行性存疑

   

# 2 快捷键操作

- 删除 cell：`x`
- 拆分 cell：`Ctrl + Shift + -`
- 运行 cell：`Shift + Enter`
- 显示行号：`Shift + L`



# 3 技巧

1. 如何判断那些代码块已经运行完毕？

   > 运行完毕的 cell 最左边会有数字显示

2. 将 .ipynb 转换为 .md

   ```shell
   # 安装 nbconvert
   pip install nbconvert
   # 安装 pandoc
   pip install pandoc
   ```

   ```python
   # 命令行转换
   jupyter nbconvert --to FORMAT notebook.ipynb
   ```

   > [!tip]
   >
   > 其中 `FORMAT` 可以为 `markdown`, `html` 格式

