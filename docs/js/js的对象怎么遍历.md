# js 的对象怎么遍历

## for-in

- for … in 循环遍历对象自身的和继承的可枚举属性(循环遍历对象自身的和继承的可枚举属性(不含 Symbol 属性))
- 也可遍历数组
- ```javascript
  var obj = { 0: "a", 1: "b", 2: "c" };
  for (var i in obj) {
    console.log(i, ":", obj[i]);
  }
  // 0 : a
  // 1 : b
  // 2 : c
  ```

## Object.keys

- 返回一个数组,包括对象自身的(不含继承的)所有可枚举属性(不含 Symbol 属性)
- ```javascript
  var obj = { 0: "a", 1: "b", 2: "c" };
  Object.keys(obj).forEach((key) => {
    console.log(key, obj[key]);
  });
  // 0  a
  // 1  b
  // 2  c
  ```

## Object.values

- Object.values()方法返回一个给定对象自身的所有可枚举属性值的数组，值的顺序与使用 for...in 循环的顺序相同 ( 区别在于 for-in 循环枚举原型链中的属性 )
- ```javascript
  var obj = { 0: "a", 1: "b", 2: "c" };
  console.log(Object.values(obj));
  // ["a", "b", "c"]
  ```

## Object.getOwnPropertyNames(obj)

- 返回一个数组,包含对象自身的所有属性(不含 Symbol 属性,但是包括不可枚举属性)
- ```javascript
  var obj = { 0: "a", 1: "b", 2: "c" };
  Object.getOwnPropertyNames(obj).forEach(function (key) {
    console.log(key, obj[key]);
  });
  // 0  a
  // 1  b
  // 2  c
  ```

## Reflect.ownKeys(obj)

- 返回一个数组,包含对象自身的所有属性,不管属性名是 Symbol 或字符串,也不管是否可枚举.
- ```javascript
  var obj = { 0: "a", 1: "b", 2: "c" };
  Reflect.ownKeys(obj).forEach(function (key) {
    console.log(key, obj[key]);
  });
  // 0  a
  // 1  b
  // 2  c
  ```
