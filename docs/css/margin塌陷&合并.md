# margin 塌陷&&合并

## margin 塌陷

出现：对于两个嵌套关系（父子关系）的块元素，子元素设置了 margin-top，会表现在父元素上；若子元素和父元素都设置了 margin-top，也只表现在父元素上，取最大的 margin-top 值。

解决：

- 为父盒子设置 border，为外层添加 border 后父子盒子就不是真正意义上的贴合。
- 为父盒子添加 overflow：hidden；
- 为父盒子设定 padding 值。

## margin 合并

出现：当上下相邻的两个块元素（兄弟关系）相遇时，如果上面的元素有下外边距 margin-bottom，下面的元素有
上外边距 margin-top ，则他们之间的垂直间距不是 margin-bottom 与 margin-top 之和。取两个值中的
较大者这种现象被称为相邻块元素垂直外边距的合并

解决：在两容器外都套上相同容器（class/id 相同的），并在容器中触发 BFC 规则，如设定 overflow：hidden。
