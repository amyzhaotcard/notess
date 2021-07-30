# webpack plugin 怎么理解

Plugin（Plug-in）是一种计算机应用程序，它和主应用程序互相交互，以提供特定的功能

## 特性

其本质是一个具有 apply 方法 javascript 对象

apply 方法会被 webpack compiler 调用，并且在整个编译生命周期都可以访问 compiler 对象

关于整个编译生命周期钩子，有如下：

- entry-option ：初始化 option
- run
- compile： 真正开始的编译，在创建 compilation 对象之前
- compilation ：生成好了 compilation 对象
- make 从 entry 开始递归分析依赖，准备对每个模块进行 build
- after-compile： 编译 build 过程结束
- emit ：在将内存中 assets 内容写到磁盘文件夹之前
- after-emit ：在将内存中 assets 内容写到磁盘文件夹之后
- done： 完成所有的编译过程
- failed： 编译失败的时候

## 常见的 plugin

- HtmlWebpackPlugin: 简单创建 HTML 文件
- clean-webpack-plugin： 删除（清理）构建目录
- mini-css-extract-plugin： 提取 CSS 到一个单独的文件中
- DefinePlugin： 允许在编译时创建配置的全局对象，是一个 webpack 内置的插件，不需要安装
- copy-webpack-plugin： 复制文件或目录到执行区域
