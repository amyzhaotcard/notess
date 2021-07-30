# vue3 中 Composition Api 了解么

## 设计初衷

- 逻辑组合和复用
- 类型推导：Vue3.0 最核心的点之一就是使用 TS 重构，以实现对 TS 丝滑般的支持。而基于函数 的 API 则天然对类型推导很友好。
- 打包尺寸：每个函数都可作为 named ES export 被单独引入，对 tree-shaking 很友好；其次所有函数名和 setup 函数内部的变量都能被压缩，所以能有更好的压缩效率。

## 用途

- 使用 Composition API 模块化共享属性和方法

## Composition Api 与 Options Api（Vue 2.X）的比较

:::tip
Options Api 是通过定义 methods，computed，watch，data 等属性与方法，共同处理页面逻辑
:::

- 逻辑组织
  - Options Api 理解和维护复杂组件变得困难；在处理单个逻辑关注点时，我们必须不断地“跳转”相关代码的选项块
  - Composition Api 将某个逻辑关注点相关的代码全都放在一个函数里，这样当需要修改一个功能时，就不再需要在文件中跳来跳去
- 逻辑复用

  - Options Api 通过 mixin 引入，会存在命名冲突、数据来源不清晰的问题
  - Composition Api 数据来源清晰，也不会出现命名冲突的问题

总结差异比较：

- 在逻辑组织和逻辑复用方面，Composition API 是优于 Options API
- 因为 Composition API 几乎是函数，会有更好的类型推断。
- Composition API 对 tree-shaking 友好，代码也更容易压缩
- Composition API 中见不到 this 的使用，减少了 this 指向不明的情况
- 如果是小型组件，可以继续使用 Options API，也是十分友好的
