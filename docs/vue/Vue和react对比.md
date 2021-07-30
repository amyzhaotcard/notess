# vue 和 react 对比

## 相同点

- 都有组件化思想
- 都支持服务器端渲染
- 都有 Virtual DOM（虚拟 dom）
- 数据驱动视图
- 都有支持 native 的方案：Vue 的 weex、React 的 React native
- 都有自己的构建工具：Vue 的 vue-cli、React 的 Create React App

## 区别

- 数据流向的不同。react 从诞生开始就推崇单向数据流，而 Vue 是双向数据流
- 数据变化的实现原理不同。react 使用的是不可变数据，而 Vue 使用的是可变的数据
- 组件化通信的不同。react 中我们通过使用回调函数来进行通信的，而 Vue 中子组件向父组件传递消息有两种方式：事件和回调函数
- diff 算法不同。react 主要使用 diff 队列保存需要更新哪些 DOM，得到 patch 树，再统一操作批量更新 DOM。Vue 使用双向指针，边对比，边更新 DOM
