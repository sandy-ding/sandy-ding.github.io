---
title: JavaScript原型链
date: 2020-03-25 21:14:04
categories:
    - 前端
    - JavaScript
tags: 
    - 前端
    - JavaScript
    - 原型链
---

JavaScript是通过原型链实现继承的。虽然`ES6`引入了`class`关键字，但那只是语法糖，JavaScript仍然是基于原型继承的。本文将会深入介绍原型链。

# 原型链

## 什么是原型链

* 在JavaScript中，每个函数都有`prototype`属性，`prototype`对象有一个默认的`constructor`属性指回这个函数。
* 每个对象（除`null`外）都会有`__proto__`私有属性。
* `实例对象的__proto__指向其构造函数的prototype`（划重点!!!），由此实现继承。

``` JavaScript
function Foo() {
}

var f1 = new Foo();

f1.__proto__ === Foo.prototype;   //true
```

通过`new`操作符作用于普通函数时，此函数就是构造函数，生成了实例对象。 以上代码中，`f1`是由`Foo`生成的实例，它们的原型关系如下图。

![图片来自Hursh Jain/mollypages.org，内容有所修改](https://cdn.jsdelivr.net/gh/sandy-ding/imgHosting/blog/20200325223328.jpg)

Q：`Foo.prototype`又继承自什么呢？
A：`prototype`就是一个普通的对象。所有JavaScript中的对象（除`null`外）最终都继承自`Object.prototype`，它的`__proto__`就指向`Object.prototype`。再往上找，`Object.prototype.__proto__`为`null`，而`null`没有原型。

![图片来自Hursh Jain/mollypages.org, 内容有所修改](https://cdn.jsdelivr.net/gh/sandy-ding/imgHosting/blog/20200325235155.jpg)

沿着上图中的虚线箭头，递归查找`__proto__`属性，直到`null`，这就是整条`原型链`。当试图访问一个对象的属性时，它不仅仅在该对象上搜寻，还会沿着原型链一直找下去，直到找到一个名字匹配的属性，或找到原型链的终点也没找到。

## 要注意的是

通过原型链实现继承时，不能使用对象字面量创建原型，这将会打破原型链。这样写的`prototype`会丢失`constructor、__proto__`等默认属性。

``` JavaScript
// 不能这样写，将打破原型链
Foo.prototype = {
    age: 10;
};

// 应该这样写，因为prototype里还有constructor、__proto__等属性
Foo.prototype.age = 10;
```

## 有意思的是

定义在构造函数`prototype`上的属性，会被所有实例对象继承，但是构造函数上却没有此属性，因为查找属性只会沿着`__proto__`找。

``` JavaScript
Foo.prototype.age = 10;

// f1上找不到，会接着找f1.__proto__，即Foo.prototype
f1.age   // 10
// 原型链只会沿着__proto__找，不会沿着prototype
Foo.age  // undefined
```

# new做了什么

使用`new`创建出实例对象的时候，实际上执行了以下的操作。

``` JavaScript
// 1. 创建一个空对象
var f1 = new Object();
// 2. 将空对象的__proto__指向构造函数的prototype
f1.__proto__ = Foo.prototype;
// 3. 将构造函数内部this绑定到新创建的空对象
Foo.call(f1);
// 4. 如果该函数没有设置返回对象，则返回新创建的对象
return f1;
```

# 属性遮蔽

沿着原型链向上找某个属性，先找到就会返回结果，因此会遮蔽更深层的同名属性，这就叫`属性遮蔽`。

``` JavaScript
function Foo() {
    this.name = "Tom";
}
Foo.prototype.name = "Bob";

var f1 = new Foo();
f1.name  // Tom
// 在f1上已经找到了name，不会接着向上查找原型链
```

# Object.create

使用`Object.create`方法创建的对象，新对象的原型会指向调用方法时创建的第一个对象。

![图片来自Hursh Jain/mollypages.org， 内容有所修改](https://cdn.jsdelivr.net/gh/sandy-ding/imgHosting/blog/20200326004045.jpg)

`Object.create(null)`可以创建一个没有原型的对象。

# 参考链接：
[继承与原型链](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)
[Javascript Object Layout](http://mollypages.org/tutorials/js.mp)
[JavaScript深入之从原型到原型链](https://github.com/mqyqingfeng/blog/issues/2)
