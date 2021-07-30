# vue2 - patch && diff

组件化的实现过程就是一个递归 new Vue 的过程， new Vue 后就是一个 init -> render -> patch 的过程， 而 patch 就是把 render 生成的 vnode 转换成真实 DOM 的过程，vnode 又分普通的 vnode 和组件 vnode，patch 过程中遇到了组件 vnode， 就会根据这个组件 vnode 再次执行 new Vue 的过程。

patch 的核心：**diff 算法**

在 Vue 中，把 DOM-Diff 过程叫做 patch 过程：以新的 VNode 为基准，改造旧的 oldVNode 使之成为跟新的 VNode 一样。

整个 patch 过程其实就是做 3 件事：

- 创建节点：新的 VNode 中有而旧的 oldVNode 中没有，就在旧的 oldVNode 中创建。
- 删除节点：新的 VNode 中没有而旧的 oldVNode 中有，就从旧的 oldVNode 中删除。
- 更新节点：新的 VNode 和旧的 oldVNode 中都有，就以新的 VNode 为准，更新旧的 oldVNode

**diff 算法是通过同层的树节点进行比较而非对树进行逐层搜索遍历的方式，所以时间复杂度只有 O(n)**

## 创建节点

只有 3 种类型的节点能够被创建并插入到 DOM 中，它们分别是：元素节点、文本节点、注释节点。

```javascript
function createElm(vnode, parentElm, refElm) {
  const data = vnode.data;
  const children = vnode.children;
  const tag = vnode.tag;
  if (isDef(tag)) {
    vnode.elm = nodeOps.createElement(tag, vnode); // 创建元素节点
    createChildren(vnode, children, insertedVnodeQueue); // 创建元素节点的子节点
    insert(parentElm, vnode.elm, refElm); // 插入到DOM中
  } else if (isTrue(vnode.isComment)) {
    vnode.elm = nodeOps.createComment(vnode.text); // 创建注释节点
    insert(parentElm, vnode.elm, refElm); // 插入到DOM中
  } else {
    vnode.elm = nodeOps.createTextNode(vnode.text); // 创建文本节点
    insert(parentElm, vnode.elm, refElm); // 插入到DOM中
  }
}
```

## 删除节点

如果某些节点在新的 VNode 中没有而在旧的 oldVNode 中有，那么就需要把这些节点从旧的 oldVNode 中删除：在要删除节点的父元素上调用 removeChild 方法

```javascript
function removeNode(el) {
  const parent = nodeOps.parentNode(el); // 获取父节点
  if (isDef(parent)) {
    nodeOps.removeChild(parent, el); // 调用父节点的removeChild方法
  }
}
```

## 更新节点

```javascript
function patchVnode(oldVnode, vnode) {
  /* 新老VNode相同不做处理，return掉 */
  if (oldVnode **= vnode) {
    return;
  }

  /* 如果新老节点都是静态的，并且key相同时，直接取老节点的ele及componentInstance就可 */
  /* 是否是静态的节点，在编译（optimize）的过程会直接标记出来*/
  if (vnode.isStatic && oldVnode.isStatic && vnode.key **= oldVnode.key) {
    vnode.elm = oldVnode.elm;
    vnode.componentInstance = oldVnode.componentInstance;
    return;
  }

  const elm = (vnode.elm = oldVnode.elm);
  const oldCh = oldVnode.children;
  const ch = vnode.children;

  /* 如果新的VNode节点是文本节点，直接用setTextContent */
  if (vnode.text) {
    /* nodeOps是一个适配层，根据不同平台提供不同的操作平台 DOM 的方法，实现跨平台 */
    nodeOps.setTextContent(elm, vnode.text);
  } else {
    if (oldCh && ch && oldCh !** ch) {
      /* 都存在且不相同 */
      updateChildren(elm, oldCh, ch);
    } else if (ch) {
      /* 只有ch存在*/
      if (oldVnode.text) {
        /* 如果老节点是文本节点，需先将节点的文本清除 */
        nodeOps.setTextContent(elm, "");
      }
      addVnodes(elm, null, ch, 0, ch.length - 1);
    } else if (oldCh) {
      /* 如果只有oldCh,需要清除掉所有的老节点 */
      removeVnodes(elm, oldCh, 0, oldCh.length - 1);
    } else if (oldVnode.text) {
      /* 只有老节点是文本节点时，清除节点文本内容*/
      nodeOps.setTextContent(elm, "");
    }
  }
}
```

## 更新子节点

当新的 VNode 与旧的 oldVNode 都是元素节点并且都包含子节点时，那么这两个节点的 VNode 实例上的 children 属性就是所包含的子节点数组。我们把新的 VNode 上的子节点数组记为 newChildren，把旧的 oldVNode 上的子节点数组记为 oldChildren。

同样是有四种情况：

- 创建子节点：如果 newChildren 里面的某个子节点在 oldChildren 里找不到与之相同的子节点，那么说明 newChildren 里面的这个子节点是之前没有的，是需要此次新增的节点，那么就创建子节点。
- 删除子节点：如果把 newChildren 里面的每一个子节点都循环完毕后，发现在 oldChildren 还有未处理的子节点，那就说明这些未处理的子节点是需要被废弃的，那么就将这些节点删除。
- 移动子节点：如果 newChildren 里面的某个子节点在 oldChildren 里找到了与之相同的子节点，但是所处的位置不同，这说明此次变化需要调整该子节点的位置，那就以 newChildren 里子节点的位置为基准，调整 oldChildren 里该节点的位置，使之与在 newChildren 里的位置相同。
- 更新节点：如果 newChildren 里面的某个子节点在 oldChildren 里找到了与之相同的子节点，并且所处的位置也相同，那么就更新 oldChildren 里该节点，使之与 newChildren 里的该节点相同。

## 优化更新子节点

外层循环新节点的 children 数组，内层循环旧节点的 children 数组，每循环外层的新节点的 children 数组中的一个子节点，就去内层老节点的 children 数组中去找有没有相同的节点，再根据不同的情况去做操作。当包含的子节点数量过多时，这样循环算法的时间复杂度就会变的很大，不利于性能提升。

优化：

- 先把 newChildren 数组里的所有**未处理**子节点的**第一个子节点**和 oldChildren 数组里所有**未处理**子节点的**第一个子节点**做比对，如果相同，那就直接进入更新节点的操作；
- 如果不同，再把 newChildren 数组里所有**未处理**子节点的**最后一个子节点**和 oldChildren 数组里所有**未处**理子节点的**最后一个子节点**做比对，如果相同，那就直接进入更新节点的操作；
- 如果不同，再把 newChildren 数组里所有**未处理**子节点的**最后一个子节点**和 oldChildren 数组里所有**未处理**子节点的**第一个子节点**做比对，如果相同，那就直接进入更新节点的操作，更新完后再将 oldChildren 数组里的该节点移动到与 newChildren 数组里节点相同的位置；
- 如果不同，再把 newChildren 数组里所有**未处理**子节点的**第一个子节点**和 oldChildren 数组里所有**未处理**子节点的**最后一个子节点**做比对，如果相同，那就直接进入更新节点的操作，更新完后再将 oldChildren 数组里的该节点移动到与 newChildren 数组里节点相同的位置；
- 最后四种情况都试完如果还不同，那就按照之前循环的方式来查找节点。
