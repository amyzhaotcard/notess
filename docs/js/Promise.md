# Promise

## 介绍

Promise，译为承诺，是异步编程的一种解决方案，解决了回调地狱的问题

## 优点

- 链式操作减低了编码难度
- 代码可读性明显增强

## 状态

promise 对象仅有三种状态

- pending（进行中）
- fulfilled（已成功）
- rejected（已失败）

## 特点

- 对象的状态不受外界影响，只有异步操作的结果，可以决定当前是哪一种状态
- 一旦状态改变（从 pending 变为 fulfilled 和从 pending 变为 rejected），就不会再变，任何时候都可以得到这个结果

## 用法

```javascript
const promise = new Promise(function (resolve, reject) {});
```

Promise 构造函数接受一个函数作为参数，该函数的两个参数分别是 resolve 和 reject

- resolve 函数的作用是，将 Promise 对象的状态从“未完成”变为“成功”
- reject 函数的作用是，将 Promise 对象的状态从“未完成”变为“失败”

## 实例方法(then、catch、finally)

- then()
  - then 方法返回的是一个新的 Promise 实例，也就是 promise 能链式书写的原因
  - then 是实例状态发生改变时的回调函数，第一个参数是 resolved 状态的回调函数，第二个参数是 rejected 状态的回调函数
- catch()
  - 是`.then(null, rejection)`或`.then(undefined, rejection)`的别名，用于指定发生错误时的回调函数
  - 一般来说，使用 catch 方法代替 then()第二个参数
  - Promise 对象抛出的错误不会传递到外层代码，不会退出进程
- finally()
  - finally()方法用于指定不管 Promise 对象最后状态如何，都会执行的操作

## 手写一个 promise

```javascript
// 三个常量表示状态
const PENDING = "pending";
const FULFILLED = "fulfilled";
const REJECTED = "rejected";

class Promise {
  /**
   * 在 new Promise 的时候会传入一个执行器 (executor) 同时这个执行器是立即执行的
   * state      Promise的状态，初始化为等待
   * value      成功的值
   * reason     错误的原因
   */
  constructor(executor) {
    this.state = PENDING;
    this.value = undefined;
    this.reason = undefined;

    /**
     * resolve 和 reject 函数中
     * 只有在等待状态（）下的 Promise 才会修改状态
     */
    // 成功函数
    const resolve = (value) => {
      if (this.state === PENDING) {
        this.state = FULFILLED;
        this.value = value;
      }
    };

    // 失败函数
    const reject = (reason) => {
      if (this.state === PENDING) {
        this.state = REJECTED;
        this.reason = reason;
      }
    };

    /**
     * 执行器（executor）接受两个参数，分别是 resolve, reject
     * 防止执行器报错，需要捕获，并传入 reject 函数
     */

    try {
      executor(resolve, reject);
    } catch (e) {
      reject(e);
    }
  }

  // then 方法
  then(onFulfilled, onRejected) {
    if (this.state === FULFILLED) {
      // 成功后的回调，并把值返回
      onFulfilled(this.value);
    }

    if (this.state === REJECTED) {
      // 失败的回调，并把失败原因返回
      onRejected(this.reason);
    }
  }
}
```

## 构造函数方法

- all()
- race()
- allSettled()
- resolve()
- reject()
- try()

### all()

`const p = Promise.all([p1, p2, p3]);`

- 只有所有的 Promise 返回成功，才会成功，
- 如果有一个失败，就是失败状态，并且不会返回其他已经成功的状态
- **成功和失败的返回值是不同的，成功的时候返回的是一个结果数组，而失败的时候则返回最先被 reject 失败状态的值**

```javascript
  static all(promises) {
    return new Promise((resolve, reject) => {
      // 参数不是数组 直接 reject
      if (!Array.isArray(promises)) {
        reject(new TypeError('参数必须是数组'))
        return
      }

      const result = []

      // 空数组直接返回
      if (promises.length === 0) {
        resolve(result)
      }

      // 记录成功 promise 数量
      let num = 0

      function check(i, data) {
        result[i] = data

        num++

        // 成功数量等于传入的数量 resolve
        if (num === promises.length) {
          resolve(result)
        }
      }

      for (let i = 0; i < promises.length; i++) {
        Promise.resolve(promises[i]).then(
          v => {
            check(i, v)
          },
          e => {
            reject(e)
            return
          })
      }
    })
  }

```

### race()

`const p = Promise.race([p1, p2, p3]);`

- 新的 Promise 实例状态会根据 最先更改状态的 Promise 而更改状态
- 只要有一个 Promise 状态发生改变，就调用其状态对应的回调方法
- 哪个结果获得的快，就返回那个结果，不管结果本身是成功状态还是失败状态

```javascript
  static race(promises) {
    return new Promise((resolve, reject) => {
      // 参数不为数组时直接 reject
      if (!Array.isArray(promises)) {
        reject(new TypeError('参数必须为数组'))
        return
      }

      // 如果传入一个空数组则直接返回
      if (promises.length === 0) {
        resolve()
        return
      }

      for (let i = 0; i < promises.length; i++) {
        // 只要有一个 Promise 状态发生改变，就调用其状态对应的回调方法
        Promise.resolve(promises[i]).then(resolve, reject)
      }
    })
  }
```

### allSettled()

- 只有等到所有这些参数实例都返回结果，不管是 resolved（成功） 还是 rejected（失败），包装实例才会结束，一旦结束，状态总是 resolved（成功），并且把所有状态返回
- 该方法由 ES2020 引入

```javascript
static allSettled(promises) {
    return new Promise((resolve, reject) => {
      // 参数不为数组时直接 reject
      if (!Array.isArray(promises)) {
        reject(new TypeError('参数必须为数组'))
        return
      }

      const result = []

      // 如果传入一个空数组则直接返回
      if (promises.length === 0) {
        resolve(result)
        return
      }

      // 记录当前已返回结果的 Promise 数量
      let num = 0

      // resolve 验证函数
      function check(i, data) {
        result[i] = data
        num++
        // 只有已返回结果的 Promise 数量等于传入的数组长度时才调用 resolve
        if (num === promises.length) {
          resolve(result)
        }
      }

      for (let i = 0; i < promises.length; i++) {
        Promise.resolve(promises[i]).then(
          (value) => {
            check(i, {
              status: FULFILLED,
              value,
            })
          },
          (reason) => {
            check(i, {
              status: REJECTED,
              reason,
            })
          }
        )
      }
    })
  }
```

### resolve()

- 将现有对象转为 Promise 对象

```javascript
Promise.resolve("foo");
// 等价于
new Promise((resolve) => resolve("foo"));
```

- 参数是一个 Promise 实例，promise.resolve 将不做任何修改、原封不动地返回这个实例
- 参数是一个 thenable 对象，promise.resolve 会将这个对象转为 Promise 对象，然后就立即执行 thenable 对象的 then()方法
- 参数不是具有 then()方法的对象，或根本就不是对象，Promise.resolve()会返回一个新的 Promise 对象，状态为 resolved
- 没有参数时，直接返回一个 resolved 状态的 Promise 对象

### reject()

- Promise.reject(reason)方法也会返回一个新的 Promise 实例，该实例的状态为 rejected
- Promise.reject()方法的参数，会原封不动地变成后续方法的参数

```javascript
const p = Promise.reject("出错了");
// 等同于
const p = new Promise((resolve, reject) => reject("出错了"));

p.then(null, function (s) {
  console.log(s);
});
// 出错了
```

## 多个 promise 实现串行

[面试官： 来说一下如何串行执行多个 Promise](https://www.manman.io/2019/03/21/promise_series/)

```javascript
function delay(time) {
  return new Promise((resolve, reject) => {
    console.log(`wait ${time}s`);
    setTimeout(() => {
      console.log("execute");
      resolve();
    }, time * 1000);
  });
}

const arr = [3, 4, 5];
```

方法一：reduce

```javascript
arr.reduce((s, v) => {
  return s.then(() => delay(v));
}, Promise.resolve());
```

方法二： async + 循环 + await

```javascript
(async function () {
  for (const v of arr) {
    await delay(v);
  }
})();
```

方法三：普通循环

```javascript
let p = Promise.resolve();
for (const i of arr) {
  p = p.then(() => delay(i));
}
```

方法四：递归

```javascript
function dispatch(i, p = Promise.resolve()) {
  if (!arr[i]) return Promise.resolve();
  return p.then(() => dispatch(i + 1, delay(arr[i])));
}
dispatch(0);
```

方法五：for await of

```javascript
function createAsyncIterable(arr) {
  return {
    [Symbol.asyncIterator]() {
      return {
        i: 0,
        next() {
          if (this.i < arr.length) {
            return delay(arr[this.i]).then(() => ({
              value: this.i++,
              done: false,
            }));
          }

          return Promise.resolve({ done: true });
        },
      };
    },
  };
}

(async function () {
  for await (i of createAsyncIterable(arr)) {
  }
})();
```

方法六：generator + co
只知道这样也可以实现，后续补充

## Promise.then 为什么可以链式调用

then 方法返回的是一个新的 Promise 实例，也就是 promise 能链式书写的原因

## Promise.then 为什么可以写多个
