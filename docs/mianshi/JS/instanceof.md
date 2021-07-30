# instanceof 和 typeof 实现原理

## typeof

`typeof` 一般被用于判断一个变量的基础类型

**js 在底层储存变量的时候，会在变量的机器吗的低位 `1-3位` 储存其类型信息**

- 000：对象
- 010：浮点数
- 100：字符串
- 110：布尔值
- 1：整数

但是，对于 `undefined` 和 `null` 来说，这两个值的信息存储是特殊的

`null`：所有机器码均为 0 

`undefined`：用 −2^30 整数来表示


## instanceof

`instanceof` 主要的作用就是判断一个实例是否属于某种类型

```javascript
let person = function () {}

let p1 = new person()

p1 instanceof person  // true
```


- `instanceof` 也可以判断一个实例是否是其父类或者祖先类型的实例

```javascript
let person = function () {}

let p = function () {}

p.prototype = new person()

let p1 = new p()

p1 instanceof person  // true

p1 instanceof p       // true

```


### 实现 instanceof 方法

```javascript
function myInstanceof(leftV, rightV) {
  
  // 取右表达式的 prototype 的值
  let rigthProto = rightV.prototype
  
  // 取左表达式的 __proto__ 值
  leftV = leftV.__proto__
  
  while (true) {
    if (leftV === null) {
      return false
    }
    
    // 严格相等时候 返回 true
    if (leftV === rigthProto) {
      return true
    }
    
    leftV = leftV.__proto__
  }
}
```

## 参考

[浅谈 instanceof 和 typeof 的实现原理](https://juejin.cn/post/6844903613584654344)

