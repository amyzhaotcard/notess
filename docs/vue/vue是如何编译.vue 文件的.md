# vue 是如何编译.vue 文件的,如何区分 template、js、style

Vue 的官方提供了 **`vue-loader`** ，它会解析文件，提取每个语言块，如有必要会通过其它 loader 处理，最后将他们组装成一个 ES Module，它的默认导出是一个 Vue.js

官方解释：

- 允许为 vue 组件的每个部分使用他的 webpack loader，例如在 style 中使用 sass，在 template 中使用 Pug
- 允许一个.vue 文件中使用自定义块，并对其运用自定义的 loader 链
- 使用 webpack loader 将 style，template 中引用的资源当作模块依赖处理
- 为每个组件模拟出 scoped css
- 在开发过程中使用热重载来保持状态
