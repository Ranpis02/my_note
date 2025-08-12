# 生命周期

C++ 的生命周期主要分为如下几个阶段：

1. 预处理（Preprocessing）：处理宏(#define)、头文件(#include)等，**生成预处理后的源代码**

   > [!note]
   >
   > 该阶段，会处理以 `#` 开头的预处理指令，例如 `#include`、`#define`、`#ifdef`、`#ifndef`，具体操作如下：
   >
   > - 头文件展开：将 `#include <iostream>` 等替换为对应的头文件内容
   > - 宏替换：对于 `#define PI 3.14` 而言，会将代码中所有的 `PI` 替换为 3.14
   > - 条件编译：根据 `#ifdef`、`endif` 等决定保留代码
   > - 去除注释：删除代码中的注释

2. 编译（Complication）：将预处理后的代码翻译为汇编代码，再生成目标文件（机器码）。

   ```mermaid
   graph LR
   A("source code") --> B("assembly code")
   B --> C("object file")
   ```

   - `source code`: 即源码，是程序员使用的高级编程语言，例如 main.c 
   - `assembly code`: 指使用汇编语言编写的程序，通常是 `.s`
   - `object file`: 是指编译器将源码编译成的机器代码文件，通常是 `.o` 或者 `.obj` 

   编译的过程中涉及的步骤很多，主要包括词法分析、语法分析、语义分析、中间代码生成、优化（constant folding, loop optimization, function inling）等等

3. 链接（Linking）：将多个目标文件和库合并为可执行文件 `.exe`

4. 加载和运行（Loading and Execution）：操作系统加载可执行文件到内存并执行





