# let 、const 和 var 的区别

在 ES6 出现之前，必须使用 var 声明变量。ES6 添加了 let 和 const。

总结区别：

- **var**声明是全局作用域或函数作用域，而**let**和**const**是块作用域；
- **var**变量可以在其范围内更新和重新声明； **let**变量可以被更新但不能重新声明； **const**变量既不能更新也不能重新声明
- 它们都被提升到其作用域的顶端。 但是，虽然使用变量 undefined 初始化了 **var** 变量，但未初始化 **let** 和 **const** 变量。
- 尽管可以在不初始化的情况下声明 **var** 和 **let**，但是在声明期间必须初始化 **const**
- let **暂时性死区**： 在代码块内，使用 let 命令声明变量之前，该变量都是不可用的。

### var

- var 的作用域： var 可以在全局范围声明或函数/局部范围内声明

  当在最外层函数的外部声明 var 变量时，作用域是全局的。 这意味着在最外层函数的外部用 var 声明的任何变量都可以在 windows 中使用。
  当在函数中声明 var 时，作用域是局部的。 这意味着它只能在函数内访问。

  ```javascript
  var greeter = "hey hi"; // 全局作用域

  function newFunction() {
    var hello = "hello"; // 函数作用域
  }
  ```

- var 变量可以重新声明和修改
- var 的变量提升

  变量提升是 JavaScript 的一种机制:在执行代码之前，变量和函数声明会移至其作用域的顶部

  ```javascript
  console.log(greeter); // undefined
  var greeter = "say hello";
  ```

  相当于

  ```javascript
  var greeter;
  console.log(greeter); // greeter is undefined
  greeter = "say hello";
  // var声明的变量会被提升到其作用域的顶部，并使用 undefined 值对其进行初始化.
  ```

### let

- let 是块级作用域
- let 可以被修改但是不能被重新声明

```javascript
let greeting = "say Hi";
if (true) {
  let greeting = "say Hello instead";
  console.log(greeting); // "say Hello instead"
}
console.log(greeting); // "say Hi"
// 两个实例的作用域不同，因此它们会被视为不同的变量。
```

- let 的变量提升
  就像 var 一样，let 声明也被提升到作用域顶部。
  但不同的是:用 var 声明的变量会被提升到其作用域的顶部，并使用 undefined 值对其进行初始化；用 let 声明的变量会被提升到其作用域的顶部，不会对值进行初始化

### const

- const 也是块级作用域
- const 不能被修改并且不能被重新声明
- 与 let 一样，const 声明也被提升到顶部，但是没有初始化。

## let、const 作用域是什么作用域

块级作用域

## const 为什么不能重新赋值、重新声明

const 指针指向不可以改变，指向地址的内容是可以改变的。 因为 const 只是保证了对象的的指针不变，而对象的内容改变不会影响到指针的改变，所以对象属性是可以修改的

:::tip 提示
const 实际上保证的，并不是变量的值不得改动，而是变量指向的那个内存地址所保存的数据不得改动。对于简单类型的数据（数值、字符串、布尔值），值就保存在变量指向的那个内存地址，因此等同于常量。但对于复合类型的数据（主要是对象和数组），变量指向的内存地址，保存的只是一个指向实际数据的指针，const 只能保证这个指针是固定的（即总是指向另一个固定的地址），至于它指向的数据结构是不是可变的，就完全不能控制了。因此，将一个对象声明为常量必须非常小心。
:::
