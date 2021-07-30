# Object 与 Map、Set 的异同及使用场景

## Map

Map 是一种数据结构,在 Map 中，每一对数据的格式都为 key value 的形式。Map 中的 key 和 value 可以是任何数据类型，不仅限于字符串或整数。

## map 的属性和方法

属性： size 属性（返回 Map 结构的成员总数）

方法：

- set()

  - 设置键名 key 对应的键值为 value，然后返回整个 Map 结构
  - 如果 key 已经有值，则键值会被更新，否则就新生成该键
  - 同时返回的是当前 Map 对象，可采用链式写法

  ```javascript
  const m = new Map();
  m.set("edition", 6); // 键是字符串
  m.set(262, "standard"); // 键是数值
  m.set(undefined, "nah"); // 键是 undefined
  m.set(1, "a").set(2, "b").set(3, "c"); // 链式操作
  ```

- get() 读取 key 对应的键值，如果找不到 key，返回 undefined
  ```javascript
  const m = new Map();
  const hello = function () {
    console.log("hello");
  };
  m.set(hello, "Hello ES6!"); // 键是函数
  m.get(hello); // Hello ES6!
  ```
- has() 返回一个布尔值，表示某个键是否在当前 Map 对象之中
  ```javascript
  const m = new Map();
  m.set("edition", 6);
  m.set(262, "standard");
  m.set(undefined, "nah");
  m.has("edition"); // true
  m.has("years"); // false
  m.has(262); // true
  m.has(undefined); // true
  ```
- delete() 删除某个键，返回 true。如果删除失败，返回 false

  ```javascript
  const m = new Map();
  m.set(undefined, "nah");
  m.has(undefined); // true
  m.delete(undefined);
  m.has(undefined); // false
  ```

- clear() 清除所有成员，没有返回值
  ```javascript
  let map = new Map();
  map.set("foo", true);
  map.set("bar", false);
  map.size; // 2
  map.clear();
  map.size; // 0
  ```

## map 的遍历

- keys()：返回键名的遍历器
- values()：返回键值的遍历器
- entries()：返回所有成员的遍历器
- forEach()：遍历 Map 的所有成员

```javascript
const map = new Map([
  ["F", "no"],
  ["T", "yes"],
]);

for (let key of map.keys()) {
  console.log(key);
}
// "F"
// "T"

for (let value of map.values()) {
  console.log(value);
}
// "no"
// "yes"

for (let item of map.entries()) {
  console.log(item[0], item[1]);
}
// "F" "no"
// "T" "yes"

// 或者
for (let [key, value] of map.entries()) {
  console.log(key, value);
}
// "F" "no"
// "T" "yes"

// 等同于使用map.entries()
for (let [key, value] of map) {
  console.log(key, value);
}
// "F" "no"
// "T" "yes"

map.forEach(function (value, key, map) {
  console.log("Key: %s, Value: %s", key, value);
});
```

## Object

Object 遵循与 Map 类型相同 key value 的存储结构。JavaScript 中的 Object 拥有内置原型(prototype)。需要注意的是，JavaScript 中几乎所有对象都是 Object 实例，包括 Map。

## object 和 map 的区别

- key：Object 遵循普通的字典规则，key 必须是单一类型，并且只能是整数、字符串或是 Symbol 类型。但在 Map 中，key 可以为任意数据类型（Object, Array 等）。
- 元素顺序：Map 会保留所有元素的顺序，而 Object 并不会保证属性的顺序。
- 继承：Map 是 Object 的实例对象，而 Object 显然不可能是 Map 的实例对象。
- 构建方式：

object：

- 直接定义（var obj = {}; //空对象 var obj = {id: 1, name: "Test object"}; ）
- 构造方法（new Object）
- Object.prototype.create

map:

- new Map()

## object 和 map 的应用场景

- 如果知道所有的 key，它们都为字符串或整数（或是 Symbol 类型），你需要一个简单的结构去存储这些数据，Object 是一个非常好的选择。构建一个 Object 并通过知道的特定 key 获取元素的性能要优于 Map（字面量 vs 构造函数，直接获取 vs get()方法）。
- 如果需要在对象中保持自己独有的逻辑和属性，只能使用 Object
- JSON 直接支持 Object，但尚未支持 Map。因此，在某些我们必须使用 JSON 的情况下，应将 Object 视为首选。
- Map 是一个纯哈希结构，而 Object 不是（它拥有自己的内部逻辑）。使用 delete 对 Object 的属性进行删除操作存在很多性能问题。所以，针对于存在大量增删操作的场景，使用 Map 更合适。
- 不同于 Object，Map 会保留所有元素的顺序。Map 结构是在基于可迭代的基础上构建的，所以如果考虑到元素迭代或顺序，使用 Map 更好，它能够确保在所有浏览器中的迭代性能。
- Map 在存储大量数据的场景下表现更好，尤其是在 key 为未知状态，并且所有 key 和所有 value 分别为相同类型的情况下。

## object 为什么是无序的（Object 是怎么存储的）

[栈空间和堆空间：数据是如何存储的](https://blog.poetries.top/browser-working-principle/guide/part3/lesson12.html)

对象是以 Hash 结构存储的（hash 表其实就是管理一对对<Key，Value>这样的结构）

在 JavaScript 的执行过程中， 主要有三种类型内存空间，分别是代码空间、栈空间和堆空间。

## Set

- Set 是 es6 新增的数据结构，类似于数组，但是成员的值都是唯一的，没有重复的值，我们一般称为集合
- Set 本身是一个构造函数，用来生成 Set 数据结构

```javascript
const s = new Set();
```

## set 的方法

- add()
  - 添加某个值，返回 Set 结构本身
  - 当添加实例中已经存在的元素，set 不会进行处理添加
  - ```javascript
    s.add(1).add(2).add(2); // 2只被添加了一次
    ```
- delete()

  - 删除某个值，返回一个布尔值，表示删除是否成功
  - ```javascript
    s.delete(1);
    ```

- has()
  - 返回一个布尔值，判断该值是否为 Set 的成员
  - ```javascript
    s.has(2);
    ```
- clear()
  - 清除所有成员，没有返回值
  - ```javascript
    s.clear();
    ```

## set 的遍历

- keys()：返回键名的遍历器
- values()：返回键值的遍历器
- entries()：返回键值对的遍历器
- forEach()：使用回调函数遍历每个成员

```javascript
let set = new Set(["red", "green", "blue"]);

for (let item of set.keys()) {
  console.log(item);
}
// red
// green
// blue

for (let item of set.values()) {
  console.log(item);
}
// red
// green
// blue

for (let item of set.entries()) {
  console.log(item);
}
// ["red", "red"]
// ["green", "green"]
// ["blue", "blue"]
```

## WeakSet 和 WeakMap

#### WeakSet

- 创建 WeakSet 实例
  ```javascript
  const ws = new WeakSet();
  ```
- WeakSet 可以接受一个具有 Iterable 接口的对象作为参数
  ```javascript
  const a = [
    [1, 2],
    [3, 4],
  ];
  const ws = new WeakSet(a);
  // WeakSet {[1, 2], [3, 4]}
  ```
- 在 API 中 WeakSet 与 Set 有两个区别：1)没有遍历操作的 API; 2)没有 size 属性
- WeackSet 只能成员只能是引用类型，而不能是其他类型的值

  ```javascript
  let ws = new WeakSet();

  // 成员不是引用类型
  let weakSet = new WeakSet([2, 3]);
  console.log(weakSet); // 报错

  // 成员为引用类型
  let obj1 = { name: 1 };
  let obj2 = { name: 1 };
  let ws = new WeakSet([obj1, obj2]);
  console.log(ws); //WeakSet {{…}, {…}}
  ```

- WeakSet 里面的引用只要在外部消失，它在 WeakSet 里面的引用就会自动消失

#### WeakMap

- WeakMap 结构与 Map 结构类似，也是用于生成键值对的集合
- 在 API 中 WeakMap 与 Map 有两个区别：1)没有遍历操作的 API;2)没有 clear 清空方法

  ```javascript
  // WeakMap 可以使用 set 方法添加成员
  const wm1 = new WeakMap();
  const key = { foo: 1 };
  wm1.set(key, 2);
  wm1.get(key); // 2

  // WeakMap 也可以接受一个数组，
  // 作为构造函数的参数
  const k1 = [1, 2, 3];
  const k2 = [4, 5, 6];
  const wm2 = new WeakMap([
    [k1, "foo"],
    [k2, "bar"],
  ]);
  wm2.get(k2); // "bar"
  ```

- WeakMap 只接受对象作为键名（null 除外），不接受其他类型的值作为键名
  ```javascript
  const map = new WeakMap();
  map.set(1, 2);
  // TypeError: 1 is not an object!
  map.set(Symbol(), 2);
  // TypeError: Invalid value used as weak map key
  map.set(null, 2);
  // TypeError: Invalid value used as weak map key
  ```
- WeakMap 的键名所指向的对象，一旦不再需要，里面的键名对象和所对应的键值对会自动消失，不用手动删除引用

#### 举个场景例子：

在网页的 DOM 元素上添加数据，就可以使用 WeakMap 结构，当该 DOM 元素被清除，其所对应的 WeakMap 记录就会自动被移除

```javascript
const wm = new WeakMap();

const element = document.getElementById("example");

wm.set(element, "some information");
wm.get(element); // "some information"
```

注意：WeakMap 弱引用的只是键名，而不是键值。键值依然是正常引用

下面代码中，键值 obj 会在 WeakMap 产生新的引用，当你修改 obj 不会影响到内部

```javascript
const wm = new WeakMap();
let key = {};
let obj = { foo: 1 };

wm.set(key, obj);
obj = null;
wm.get(key);
// Object {foo: 1}
```
