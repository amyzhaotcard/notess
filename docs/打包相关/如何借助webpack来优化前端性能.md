# 如何借助 webpack 来优化前端性能

**关于 webpack 对前端性能的优化，可以通过文件体积大小入手，其次还可通过分包的形式、减少 http 请求次数等方式，实现对前端性能的优化**

- JS 代码压缩: `TerserPlugin`
- CSS 代码压缩: `css-minimizer-webpack-plugin`
- Html 文件代码压缩: 使用 HtmlWebpackPlugin 插件来生成 HTML 的模板时候，通过配置属性 minify 进行 html 优化,设置了 minify，实际会使用另一个插件 `html-minifier-terser`
- 文件大小压缩: `compression-webpack-plugin`
- 图片压缩
- Tree Shaking:消除死代码，依赖于 ES Module 的静态语法分析（不执行任何的代码，可以明确知道模块的依赖关系）
  - usedExports：通过标记某些函数是否被使用，之后通过 Terser 来进行优化的
  - sideEffects：跳过整个模块/文件，直接查看该文件是否有副作用
- 代码分离: `splitChunksPlugin`,将代码分离到不同的 bundle 中，之后我们可以按需加载，或者并行加载这些文件.
- 内联 chunk: `InlineChunkHtmlPlugin`, 将一些 chunk 的模块内联到 html，如 runtime 的代码（对模块进行解析、加载、模块信息相关的代码），代码量并不大，但是必须加载的
