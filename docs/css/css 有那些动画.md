# css 有那些动画

- transition 实现渐变动画
- transform 转变动画
- animation 实现自定义动画

## transition

```css
.box {
  transition: all 2s ease-in 500ms;
}
```

- property:填写需要变化的 css 属性
- duration:完成过渡效果需要的时间单位(s 或者 ms)
- timing-function:完成效果的速度曲线
- delay: 动画效果的延迟触发时间

其中`timing-function`的值有如下:

- linear： 匀速（等于 cubic-bezier(0,0,1,1)）
- ease： 从慢到快再到慢（cubic-bezier(0.25,0.1,0.25,1)）
- ease-in： 慢慢变快（等于 cubic-bezier(0.42,0,1,1)）
- ease-out： 慢慢变慢（等于 cubic-bezier(0,0,0.58,1)）
- ease-in-out： 先变快再到慢（等于 cubic-bezier(0.42,0,0.58,1)），渐显渐隐效果
- cubic-bezier(n,n,n,n)： 在 cubic-bezier 函数中定义自己的值。可能的值是 0 至 1 之间的数值

## transform

- translate：位移
- scale：缩放
- rotate：旋转
- skew：倾斜

## animation

| 属性                      | 描述                                                              | 属性值                                        |
| ------------------------- | ----------------------------------------------------------------- | --------------------------------------------- |
| animation-duration        | 指定动画完成一个周期所需要时间，单位秒（s）或毫秒（ms），默认是 0 |
| animation-timing-function | 指定动画计时函数，即动画的速度曲线，默认是 "ease"                 | linear、ease、ease-in、ease-out、ease-in-out  |
| animation-delay           | 指定动画延迟时间，即动画何时开始，默认是 0                        |
| animation-iteration-count | 指定动画播放的次数，默认是 1                                      |
| animation-direction       | 指定动画播放的方向 默认是 normal                                  | normal、reverse、alternate、alternate-reverse |
| animation-fill-mode       | 指定动画填充模式。默认是 none                                     | forwards、backwards、both                     |
| animation-play-state      | 指定动画播放状态，正在运行或暂停。默认是 running                  | running、pauser                               |
| animation-name            | 指定 @keyframes 动画的名称                                        |
