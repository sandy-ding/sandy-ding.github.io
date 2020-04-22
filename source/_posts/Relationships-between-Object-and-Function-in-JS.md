---
title: JS中Object和Function的关系
date: 2020-03-26 22:38:19
categories:
    - 前端
    - JavaScript
tags: 
    - 前端
    - JavaScript
    - 原型链
    - Object
    - Function
---

JavaScript中`Object`与`Function`的关系经常让人感到疑惑，希望本文能有助理解。

# 数据类型上

最新的 ECMAScript 标准定义了 8 种数据类型：

7种原始类型：
- Boolean
- Null
- Undefined
- Number
- BigInt
- String
- Symbol
和Object类型：
- Function
- Array
- Date
- RegExp

JavaScript中除了7种基本类型以外，就是`Object`类型，它可以简单理解成“名称-值”对。从数据类型上看，而`Function`是一种特殊的`Object`类型，特点是可以用`()`来调用。

```JavaScript
function Foo() {
    var a = 3; // 函数内的代码块，只能在函数内用
}
Foo.a = 5;   // 函数也是对象的一种，可以添加属性
console.log(Foo.a);  // 5，作为对象的属性
```

# 函数角度上

> `prototype`就是一个普通的对象。`在JavaScript中，所有的对象（除null外）最终都继承自Object.prototype`，它的`__proto__`就指向`Object.prototype`。 
[JavaScript原型链](https://sandy-ding.github.io/2020/03/25/JavaScript-prototype-chain/)

由上可知，`Function.prototype.__proto__`也指向`Object.prototype`。

![图片来自Hursh Jain/mollypages.org，内容有所修改](https://cdn.jsdelivr.net/gh/sandy-ding/imgHosting/blog/20200327000342.jpg)

我们调用`new Object()`，生成新的对象实例，所以`Object`是一个构造函数。`在JavaScript中，所有函数都继承自Function`，都是由`Function`生成的实例，所以`Object.__proto__`指向`Function.prototype`。

![图片来自Hursh Jain/mollypages.org, 内容有所修改](https://cdn.jsdelivr.net/gh/sandy-ding/imgHosting/blog/20200327002554.jpg)

# 存在的先后顺序上

1. 先有`Object.prototype`，它是所有对象的根源。`Object.prototype`只是挂载在`Object`函数对象上。
2. 接着`Function.prototype`继承自`Object.prototype`。`Function.prototype`只是挂载在`Function`函数对象上。
3. 所有函数，包括`Object`函数和`Function`函数继承自`Function.prototype`。

# 结论：

`作为数据类型，Function是一种特殊的Object类型`。
`从函数角度看，Object函数是Function的实例对象。Function.prototype对象是Object的实例对象。`

最后，是时候祭出JS原型链的终级图了，结合[原型链](https://sandy-ding.github.io/2020/03/25/JavaScript-prototype-chain/)这篇，可以看看自己是否彻底掌握了。

![图片来自Hursh Jain/mollypages.org](https://cdn.jsdelivr.net/gh/sandy-ding/imgHosting/blog/20200327002838.jpg)

# 参考链接：
[Relationships between objects in JS](https://medium.com/@ivanilic/relationships-between-objects-in-js-335bdc4f402b)
[Is Object a function in JavaScript?](https://stackoverflow.com/questions/54861385/is-object-a-function-in-javascript)
[js 原型的问题 Object 和 Function 到底是什么关系？](https://segmentfault.com/q/1010000002736664)
