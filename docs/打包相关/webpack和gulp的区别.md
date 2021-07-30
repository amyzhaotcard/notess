# webpack 和 gulp 的区别

webpack 是一个前端模块化打包工具，主要由入口，出口，loader（加载器），plugins（插件）四个部分。前端的打包工具还有一个 gulp，不过 gulp 侧重于前端开发的过程，而 webpack 侧重于模块，例如他会将 css 文件看作一个模块，通过 css-loader 将 css 打包成符合 css 的静态资源。

gulp 强调的是前端开发的工作流程，我们可以通过配置一系列的 task，定义 task 处理的事务（例如文件压缩合并、雪碧图、启动 server、版本控制等），然后定义执行顺序，来让 gulp 执行这些 task，从而构建项目的整个前端开发流程。
