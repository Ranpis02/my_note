VSCODE 中使用插件 clang-format 出现如下错误提示信息

```
The 'clang-format' command is not available. Please check your clang-format.executable user setting and ensure it is installed.
```

解决方案：下载 [LLVM](http://llvm.org/releases/3.7.0/LLVM-3.7.0-win32.exe)

下载完成之后，将可执行文件 \bin 添加到环境变量中，然后重启 VSCODE 即可





