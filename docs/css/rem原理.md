# rem 原理

rem 是相对于根元素 html 的 font-size 属性，默认情况下浏览器字体大小为 16px，此时 1rem = 16px

可以利用媒体查询，针对不同设备分辨率改变 font-size 的值，如下：

```css
@media screen and (max-width: 414px) {
  html {
    font-size: 18px;
  }
}

@media screen and (max-width: 375px) {
  html {
    font-size: 16px;
  }
}

@media screen and (max-width: 320px) {
  html {
    font-size: 12px;
  }
}
```

为了更准确监听设备可视窗口变化，我们可以在 css 之前插入 script 标签，内容如下:

```javascript
//动态为根元素设置字体大小
function init() {
  // 获取屏幕宽度
  var width = document.documentElement.clientWidth;
  // 设置根元素字体大小。此时为宽的10等分
  document.documentElement.style.fontSize = width / 10 + "px";
}

//首次加载应用，设置一次
init();
// 监听手机旋转的事件的时机，重新设置
window.addEventListener("orientationchange", init);
// 监听手机窗口变化，重新设置
window.addEventListener("resize", init);
```

无论设备可视窗口如何变化，始终设置 rem 为 width 的 1/10，实现了百分比布局
