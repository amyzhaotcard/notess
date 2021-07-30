# Vue 中 props、data、computed 加载顺序

加载顺序：props->methods->data->computed->watch

- computed 在 data 之后，所以不要在 data 中引用 computed 中的属性，只能得到 undefined。
- data 可以调用 前面的 props、methods 的属性
- computed 中可以调用 props，methods，data 中的属性
