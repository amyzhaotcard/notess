# new 操作符

## 一句介绍 new

> new 运算符创建一个用户定义的对象类型的实例或具有构造函数的内置对象类型之一

## new 都做了些什么？

- 创建了一个新的实例对象
- 该对象的**proto**属性与构造函数的 prototype 全等
- 改变 this 指向，使其指向新创建的实例对象
- 返回该中间对象,也就是返回了实例对象

也就是说，通过 new 操作符实现的实例对象，即可访问构造函数的属性，也可以访问构造函数原型上的属性。

## 手写一个 new

```javascript
function _new() {
  var obj = new Object(), // 创建一个新的对象，并返回
    Constructor = [].shift.call(arguments); // 截取传入函数的第一个参数
  obj.__proto__ = Constructor.prototype; // 将第一个参数的prototype与要返回的对象建立关联
  var result = Constructor.apply(obj, arguments); // 使用apply，改变构造函数的this指向，使其指向新对象，这样，obj就可以访问到构造函数中的属性了
  return typeof result === "object" ? result : obj; // 如果函数返回值是一个对象，那就返回这个对象，如果不是就返回新的对象
}
```
