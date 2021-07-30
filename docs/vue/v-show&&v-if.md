# v-show && v-if

## 共同点

都能控制元素在页面是否显示

## 不同点

- 控制手段不同： `v-show`隐藏是为该元素添加`css--display:none`，dom 元素依旧还在。`v-if`显示隐藏是将 dom 元素整个添加或删除.
- 编译过程不同： `v-if`切换有一个局部编译/卸载的过程，切换过程中合适地销毁和重建内部的事件监听和子组件；`v-show`只是简单的基于 css 切换.
- 编译条件不同: `v-if`是真正的条件渲染，它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。只有渲染条件为假时，并不做操作，直到为真才渲染.
- `v-show` 由 false 变为 true 的时候不会触发组件的生命周期
- `v-if` 由 false 变为 true 的时候，触发组件的 beforeCreate、create、beforeMount、mounted 钩子，由 true 变为 false 的时候触发组件的 beforeDestory、destoryed 方法
- 性能消耗：`v-if` 有更高的切换消耗；`v-show` 有更高的初始渲染消耗；

## 使用场景

- 如果需要非常频繁地切换，则使用 v-show 较好
- 如果在运行时条件很少改变，则使用 v-if 较好

## 原理

- v-show： 有 transition 就执行 transition，没有就直接设置 display 属性
- v-if: 返回一个 node 节点，render 函数通过表达式的值来决定是否生成 DOM
