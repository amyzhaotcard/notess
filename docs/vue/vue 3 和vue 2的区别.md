# vue 3 和 vue 2 的区别

## vue3 的重构背景（目的）

- 利用新的语言特性(es6)
- 解决架构问题

## vue 3 与 vue 2 的对比

- 速度更快
- 体积减少 : `tree-shaking`
- 更易维护
- 更接近原生 : 可以自定义渲染 API
- 更易使用 : 响应式 Api 暴露出来

### 速度更快

vue3 相比 vue2

- 重写了虚拟 Dom 实现
- 编译模板的优化
- 更高效的组件初始化
- undate 性能提高 1.3~2 倍
- SSR 速度提高了 2~3 倍

### 更易维护

- compositon Api: 可与现有的 Options API 一起使用;灵活的逻辑组合与复用;Vue3 模块可以和其他框架搭配使用
- **更好的 Typescript 支持**: Vue.js 3.0 抛弃 Flow 后，**使用 TypeScript 重构了整个项目**。 TypeScript 提供了更好的类型检查，能支持复杂的类型推导；由于源码就使用 TypeScript 编写，也省去了单独维护 d.ts 文件的麻烦；就整个 TypeScript 的生态来看，TypeScript 团队也是越做越好，TypeScript 本身保持着一定频率的迭代和更新，支持的 feature 也越来越多。
- 编译器重写

## Vue3 新增特性

- framents
- Teleport
- composition Api
- createRenderer

### framents： 支持有多个根节点

```xml
<!-- Layout.vue -->
<template>
  <header>...</header>
  <main v-bind="$attrs">...</main>
  <footer>...</footer>
</template>
```

### Teleport

- 能够将我们的模板移动到 DOM 中 Vue app 之外的其他位置
- 在 vue2 中，像 modals,toast 等这样的元素，如果我们嵌套在 Vue 的某个组件内部，那么处理嵌套组件的定位、z-index 和样式就会变得很困难
- 通过 Teleport，我们可以在组件的逻辑位置写模板代码，然后在 Vue 应用范围之外渲染它

```xml
<button @click="showToast" class="btn">打开 toast</button>
<!-- to 属性就是目标位置 -->
<teleport to="#teleport-target">
    <div v-if="visible" class="toast-wrap">
        <div class="toast-msg">我是一个 Toast 文案</div>
    </div>
</teleport>
```

### composition Api：将相同功能的变量进行一个集中式的管理

[vue3 中 Composition Api 了解么](./vue3中CompositionApi了解么.md)

### createRenderer

构建自定义渲染器，我们能够将 vue 的开发模型扩展到其他平台，比如说我们可以将其生成在 canvas 画布上

## 参考

[vue3 有了解过吗？能说说跟 vue2 的区别吗？](https://vue3js.cn/interview/vue/vue3_vue2.html)
