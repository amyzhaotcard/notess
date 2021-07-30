# BFC

BFC 即 Block Formatting Contexts (块级格式化上下文)， 是 W3C CSS2.1 规范中的一个概念。它是页面中的一块渲染区域，并且有一套渲染规则，它决定了其子元素将如何定位，以及和其他元素的关系和相互作用。

具有 BFC 特性的元素可以看作是隔离了的独立容器，容器里面的元素不会在布局上影响到外面的元素，并且 BFC 具有普通容器所没有的一些特性。

#### css 中的 BFC（格式化上下文）有什么作用？

- 可以清除浮动
- 解决 margin 塌陷
- 可以阻止元素被浮动元素覆盖（一个正常文档流的 block 元素可能被一个 float 元素覆盖，挤占正常文档流，因此可以设置一个元素的 float、display、position 值等方式触发 BFC，以阻止被浮动盒子覆盖。）
- 阻止因为浏览器四舍五入造成的多列布局换行的情况

#### 如何创建一个 BFC？

- float:left 或 right
- overflow:hidden,auto,scoll
- display:inline-block;
- position:fixed,absolute
