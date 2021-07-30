# 如何提高 webpack 的构建速度

**优化 webpack 构建的方式有很多，主要可以从优化搜索时间、缩小文件搜索范围、减少不必要的编译等方面入手**

- 优化 loader 配置: 可以通过配置 include、exclude、test 属性来匹配文件
- 合理使用 resolve.extensions： 通过 resolve.extensions 是解析到文件时自动添加拓展名，当我们引入文件的时候，若没有文件后缀名，则会根据数组内的值依次查找；当我们配置的时候，则不要随便把所有后缀都写在里面，这会调用多次文件的查找，这样就会减慢打包速度
- 优化 resolve.modules： 可以指明存放第三方模块的绝对路径，以减少寻找
- 优化 resolve.alias： 给一些常用的路径起一个别名，减少查找过程
- 使用 DLLPlugin 插件： 不经常改变的代码，抽成一个共享的库
- 使用 cache-loader： 缓存
- terser 启动多线程： 使用多进程并行运行来提高构建速度
- 合理使用 sourceMap： 打包生成 sourceMap 的时候，如果信息越详细，打包速度就会越慢
