# keep-alive

keep-alive 是 Vue 内置的一个组件，可以使被包含的组件保留状态，或避免重新渲染。

- include - 字符串或正则表达式，只有名称匹配的组件会被缓存
- exclude - 字符串或正则表达式，任何名称匹配的组件都不会被缓存
- max: 最多可以缓存多少组件实例。一旦这个数字达到了，在新实例被创建之前，已缓存组件中最久没有被访问的实例会被销毁掉。

#### 生命周期

activated（激活）、deacitvated（冻结）

#### 实现原理

- 在它的函数钩子 created 阶段，定义了 caches 对象、keys 数组来缓存已经创建的 vnode
- keep-alive 还有一个 render 函数，渲染的时候，计算一个 key，然后判断是否存在 cache 对象， 如果有就把缓存的 vnode 插入到 dom 树，没有就把 vnode 缓存到 caches 对象
