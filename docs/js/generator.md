# generator

async / await 是 generator 的语法糖

## Generator 函数

Generator 函数是协程在 ES6 的实现，最大特点就是可以**交出函数的执行权**（即暂停执行）。

Generator 函数可以暂停执行和恢复执行，这是它能封装异步任务的根本原因。

形式上，Generator 函数是一个普通函数，但是有两个特征：

- function 关键字与函数名之间有一个星号
- 函数体内部使用 yield 表达式，定义不同的内部状态

## 异步解决方案

- 回调函数
- Promise 对象
- generator 函数
- async/await

ES6 诞生之前只有以下几种：

- 回调函数
- 事件监听
- 发布/订阅
- Promise 对象

### promise、Generator、async/await 比较

- promise 和 async/await 是专门用于处理异步操作的
- Generator 并不是为异步而设计出来的，它还有其他功能（对象迭代、控制输出、部署 Interator 接口...）
- promise 编写代码相比 Generator、async 更为复杂化，且可读性也稍差
- Generator、async 需要与 promise 对象搭配处理异步情况
- async 实质是 Generator 的语法糖，相当于会自动执行 Generator 函数
- async 使用上更为简洁，将异步代码以同步的形式进行编写，是处理异步编程的最终方案

:::tip
Promise 的写法只是回调函数的改进，使用 then 方法以后，异步任务的两段执行看得更清楚了，除此以外，并无新意
:::
[深入理解 generator](http://www.alloyteam.com/2016/02/generators-in-depth/)

[Generator 函数的异步应用](https://es6.ruanyifeng.com/#docs/generator-async)
