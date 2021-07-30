# 地址栏输入 URL 敲下回车后发生了什么

[在浏览器输入 URL 回车之后发生了什么（超详细版）](https://zhuanlan.zhihu.com/p/80551769)

- URL 解析
- DNS 查询
- TCP 连接
- HTTP 请求
- 响应请求
- 页面渲染
  - 解析 HTML，构建 DOM 树
  - 解析 CSS ，生成 CSS 规则树
  - 合并 DOM 树和 CSS 规则，生成 render 树
  - 布局 render 树（ Layout / reflow ），负责各元素尺寸、位置的计算
  - 绘制 render 树（ paint ），绘制页面像素信息
  - 浏览器会将各层的信息发送给 GPU，GPU 会将各层合成（ composite ），显示在屏幕上

## template 编译成 render 函数过程

总的来说，在 beforeMount 之前执行编译过程，

- 第一步通过 html-parser 将 template 解析成 ast 抽象语法树
- 第二步通过 optimize 优化 ast 并标记静态节点和静态根节点
- 第三步通过 generate 将 ast 抽象语法树编译成 render 字符串并将静态部分放到 staticRenderFns 中
- 最后通过 new Function(render)生成 render 函数。

在 beforeMount 和 mounted 之间执行 render 函数生成 VNode，然后通过 patch(VNode)生成 dom 树并挂载，调用 mounted。

[说说地址栏输入 URL 敲下回车后发生了什么?](https://vue3js.cn/interview/http/after_url.html#%E4%B8%80%E3%80%81%E7%AE%80%E5%8D%95%E5%88%86%E6%9E%90)
