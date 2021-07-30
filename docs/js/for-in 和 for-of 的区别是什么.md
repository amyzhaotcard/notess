# for-in 和 for-of 的区别是什么

for in 是遍历（object）key，for of 是遍历（array）value

- 推荐在循环对象属性的时候，使用 for...in,在遍历数组的时候的时候使用 for...of。
- for...in 循环出的是 key，for...of 循环出的是 value
- 注意，for...of 是 ES6 新引入的特性。修复了 ES5 引入的 for...in 的不足

  - 为什么说 for...of 修复了 for...in 的缺陷和不足?
    往数组中添加一个属性之后看两个方法的不同

    ```javascript
    let aArray = ["a", 123, { a: "1", b: "2" }];
    aArray.name = "demo";
    for (let index in aArray) {
      console.log(`${aArray[index]}`);
    }
    // a
    // 123
    //  [object Object]
    // demo

    for (var value of aArray) {
      console.log(value);
    }
    // a
    // 123
    // {a: "1", b: "2"}
    ```

    **作用于数组的 for-in 循环除了遍历数组元素以外,还会遍历自定义属性**

- for...of 不能循环普通的对象，需要通过和 Object.keys()搭配使用
  - for...of 循环不会循环对象的 key，只会循环出数组的 value，因此 for...of 不能循环遍历普通对象,对普通对象的属性遍历推荐使用 for...in
  - 如果实在想用 for...of 来遍历普通对象的属性的话，可以通过和 Object.keys()搭配使用，先获取对象的所有 key 的数组,然后遍历：
  ```javascript
  var student = {
    name: "wujunchuan",
    age: 22,
    locate: {
      country: "china",
      city: "xiamen",
      school: "XMUT",
    },
  };
  for (var key of Object.keys(student)) {
    //使用Object.keys()方法获取对象key的数组
    console.log(key + ": " + student[key]);
  }
  // name: wujunchuan
  // age: 22
  // locate: [object Object]
  ```

```javascript
var obj = { a: 1, b: 2, c: 3 };

for (let key in obj) {
  console.log(key);
}

// a
// b
// c

const array2 = ["a", "b", "c"];

for (const val in array2) {
  console.log(val);
}
// 0
// 1
// 2

const array1 = ["a", "b", "c"];

for (const val of array1) {
  console.log(val);
}

// a
// b
// c
```
