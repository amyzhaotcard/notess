# SPA 首屏加载速度慢的怎么解决?

## 加载慢的原因

- 网络延时问题
- 资源文件体积是否过大
- 资源是否重复发送请求去加载了
- 加载脚本的时候，渲染内容堵塞了

## 解决方案

- 减小入口文件积：懒加载，在 vue-router 配置路由的时候，采用动态加载路由的形式
- 静态资源本地缓存：后端返回资源问题（采用 HTTP 缓存，设置 `Cache-Control`，`Last-Modified`，`Etag` 等响应头 或 采用 `Service Worker` 离线缓存）；前端合理利用 `localStorage`
- 避免组件重复打包：在 `webpack` 的 `config` 文件中，修改 `CommonsChunkPlugin` 的配置，把 `minChunks` 设置为 3，`minChunks` 为 3 表示会把使用 3 次及以上的包抽离出来，放进公共依赖文件，避免了重复加载组件
- UI 框架按需加载
- 图片资源的压缩: webp、雪碧图
- 开启 GZip 压缩：webpack 配置
- 使用 SSR：服务端渲染，组件或页面通过服务器生成 html 字符串，再发送到浏览器
