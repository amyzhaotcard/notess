# call 、apply 、 bind

共同点：

- 都是改变 this 指向的
- 三者的第一个参数都是 this 指向的对象

区别：语法不同

## call

```javascript
fun.call(thisArg, arg1, arg2, ...)
```

- 调用 `call` 的对象，必须是个函数 `Function`
- 第一个参数：`thisArg` 是 `this` 的指向，如果不传默认为全局对象 `window`
- 第二个参数开始，可以接受任意参数，会映射到相应位置的 `Function` 的参数上面

## apply

```javascript
fun.apply(thisArg, [argsArray]);
```

- 调用 apply 的对象，必须是个函数 `Functin`
- 第一个参数：`thisArg` 是 `this` 的指向，如果不传默认为全局对象 `window` 。和 `call` 一致。
- 第二个参数：**必须是数组或者类数组，它们会被转成类数组**，传到调用的函数中，并且映射到 function 对应的参数上。call 和 apply 之间很重要的一个区别

## bind

```javascript
fun.bind(thisArg[, arg1[, arg2[, ...]]])
```

- `bind` 方法的返回值是函数，并且需要调用后，才会执行。而 `apply` 和 `call` 是立即调用的

bind() 方法会创建一个新函数。当这个新函数被调用时，bind() 的第一个参数将作为它运行时的 this，之后的一序列参数将会在传递的实参前传入作为它的参数。

## bind 的实现

bind 函数有 2 个特点：

- 返回一个函数
- 可以传入参数

代码实现

```javascript
Function.prototype.bind = function (context) {
  if (typeof this !== "function") {
    throw new Error("调用 bind 的不是函数");
  }

  var self = this;
  // 获取bind函数从第二个参数到最后一个参数
  var args = Array.prototype.slice.call(arguments, 1);

  var fNOP = function () {};

  var fBound = function () {
    // 这个时候的arguments是指bind返回的函数传入的参数
    var bindArgs = Array.prototype.slice.call(arguments);
    // 当作为构造函数时，this 指向实例，此时结果为 true，将绑定函数的 this 指向该实例，可以让实例获得来自绑定函数的值
    // 当作为普通函数时，this 指向 window，此时结果为 false，将绑定函数的 this 指向 context
    return self.apply(
      this instanceof fNOP ? this : context,
      args.concat(bindArgs)
    );
  };

  // 修改返回函数的 prototype 为绑定函数的 prototype，实例就可以继承绑定函数的原型中的值
  fNOP.prototype = this.prototype;
  fBound.prototype = new fNOP();
  return fBound;
};
```

## this 的指向

this 永远指向最后调用它的那个对象

#### 怎么解决由于 this 指向产生的问题

- 使用 ES6 的箭头函数
- 在函数内部使用 \_this = this
- 使用 apply、call、bind （改变 this 指向）
- new 实例化一个对象 （改变 this 指向）
