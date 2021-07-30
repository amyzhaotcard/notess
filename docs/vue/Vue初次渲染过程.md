# Vue 初次渲染过程（new Vue 发生了什么）

`new vue` => `init` => `$mount` => `compile(编译)` => `render` => `vnode` => `patch` => `dom`

简单流程描述：

- new Vue 的时候调用会调用\_init 方法
  - 定义 $set、$get 、$delete、$watch 等方法
  - 定义 $on、$off、$emit、$off 等事件
  - 定义 \_update、$forceUpdate、$destroy 生命周期
- 调用$mount 进行页面的挂载
- 挂载的时候主要是通过 mountComponent 方法
- 定义 updateComponent 更新函数
- 执行 render 生成虚拟 DOM
- \_update 将虚拟 DOM 生成真实 DOM 结构，并且渲染到页面中

---

较详细流程：

1. 初始化一个 vue 实例，以及一系列的相关环境（合并配置、生命周期、watcher 等等）

2. $mount：

- 挂载元素会被替换了，vue 不能在 `body` 和 `html` 标签上
- Vue 的组件的渲染最终都需要 render 方法，是一个“在线编译”的过程；

3. compile(编译)：

- 如果有 template 会将它编译成 render 函数
- vue 在挂载的时候会判断是否有 render 函数，如果有就不编译 template
- **普通的 .vue 文件中最上面的 template 为什么优先于 render 函数 ？**

  .vue 文件的会经过处理的，在 webpack 编译阶段 .vue 文件经过 vue-loader 处理， 把 template 标签在最终后被编译成 render 函数，然后添加在对象组件上，所以你运行组件时候其实只有 render 函数，并没有 template

4. 渲染 Watcher （挂载的时候添加的）

- 给实例注册一个渲染 Watcher，渲染 watcher 拥有一个回调，该回调函数会在初始化和每次 vm 实例更新时触发
- 初始化的时候会执行回调函数;
- 当 vm 实例中的监测的数据发生变化的时候执行回调函数

5. render 过程

- 利用 render 函数创建 vnode（在 render 过程中每一个模板节点（DOM 节点）都会生成对应的 \_c，也就是执行 createElement 函数来创建 vnode）
- 从根 vnode （root vnode）开始创建（处理一些边界值 textVnode（文本节点），emptyVnode（空节点）、注释节点 等等）
- 摊平所有 children vnode 成一维数组（children 拍平成一维数组就是为了建立好 tree 的数据结构，因为对于 tree 来说，每个节点的 children 就是一维数组）
- 最终生成一个 vnode tree

6. 开始执行 patch 过程

- 调用 createElement() 判断当前节点是否是 tag（参数：vnode、parentElm(父节点) 等）
- 如果不是 tag：就直接用 createTextNode() 创建 文本 DOM ，通过 insert 插入父 vnode
- 如果是 tag：生成 当前 vnode 的占位符，然后调用 createChildren() 创建子节点
- 每个节点的 children 就是一般是一维数组，然后循环调用 createElement()，也就是 步骤 1，递归
- 也有元素的文本节点，通过 appendChild 直接插入节点
  createChildren() 执行完后，创建当前 vnode 占位符 的对应的 DOM 并把它插入父 vnode
- 最后递归完成到该 vnode 占位符 的渲染 vnode（组件的 root vnode 根节点，template 标签下面的一级），并完成它的 patch。

**patch 的递归过程是一个自上而下的过程，但是插入到 DOM 节点的顺序是自下而上，也就是子节点先插入，父节点后插入。**

---

createElement（render 函数的参数）：

- children 的规范化：遍历把 children 的节点变成一个一维的 vnode 数组
- 把他们都标准化为数组，为了后续 patch 过程中统一处理遍历用的。
- 对 tag 进行判断，如果是字符串类型，继续判断是否系统保留标签，如果是则直接创建一个普通 VNode
- 如果是为已注册的组件名，则通过 createComponent 函数，创建一个组件类型的 VNode
- 最终返回一个 VNode
- render 函数是从最内层开始执行，函数的执行先对参数取值，也就是先执行 children
