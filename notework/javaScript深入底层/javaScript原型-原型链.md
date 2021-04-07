# JavaScript原型-原型链

------

深入理解javaScript原型和原型链究竟是什么？先从构造函数开始。

## 构造函数创建对象

```js
function Person(){}
var person = new Person();
person.name = 'Bibo';
cosole.log(person.name) // Bibo
```

在这个例子中，Person 就是一个构造函数，我们使用 new 创建了一个实例对象 person。

## prototype

------

**每一个函数都有prototype属性

```js
function Person(){}
Person.prototype.name = 'Bibo';
var person1 = new Person();
var person2 = new Person();
console.log(person1.name) // Bibo
console.log(person2.name) // Bibo
```

其实，函数的 prototype 属性指向了一个对象，这个对象正是调用该构造函数而创建的**实例**的原型，也就是这个例子中的 person1 和 person2 的原型。

那么我们该怎么表示实例与实例原型，也就是 person 和 Person.prototype 之间的关系呢，这时候我们就要讲到第二个属性：

##  __Proto__ 

------

这是**每一个JavaScript对象(除了 null )都具有的一个属性**，叫__proto__，这个属性会指向该对象的原型。

```js
function Person(){}
var person = new Person();
console.log(person.__proto__ === Person.prototype)//true
```

## constructor

------

每个原型都有一个 constructor 属性指向关联的构造函数。这就要讲到第三个属性：constructor﻿

为了验证这一点：

```js
function Person(){}
console.log(Person === Person.prototype.constructor)//true
```

综上：

```js
function Person(){}
var person = new Person();
console.log(Person.prototype === person.__proto__)//true
console.log(Person.prototype.constructor === Person)//true
// ES5专门获取对象原型的方法
console.log(Object.getPrototypeOf(person) === Person.prototype)//true
```

了解了构造函数、实例原型、和实例之间的关系，接下来我们讲讲实例和原型的关系：

## 实例与原型

------

当读取实例的属性时，如果找不到，就会查找与对象关联的原型中的属性，如果还查不到，就去找原型的原型，一直找到最顶层为止。

举个例子：

```js
function Person() {}
Person.prototype.name = 'Kevin';
var person = new Person();

person.name = 'Daisy';
console.log(person.name) // Daisy
delete person.name;
console.log(person.name) // Kevin
```

在这个例子中，我们给实例对象 person 添加了 name 属性，当我们打印 person.name 的时候，结果自然为 Daisy。

但是当我们删除了 person 的 name 属性时，读取 person.name，从 person 对象中找不到 name 属性就会从 person 的原型也就是 person.__proto__ ，也就是 Person.prototype中查找，幸运的是我们找到了 name 属性，结果为 Kevin。

但是万一还没有找到呢？原型的原型又是什么呢？

## 原型的原型

------

在前面，我们已经讲了原型也是一个对象，既然是对象，我们就可以用最原始的方式创建它，那就是：

```js
var obj = new Object();
obj.name = 'Kevin'
console.log(obj.name) // Kevin
```

所以原型对象是通过 Object 构造函数生成的，结合之前所讲，实例的 __proto__ 指向构造函数的 prototype

## 原型链

------

对象之间相互关联的上下文就是原型链



## 补充

最后，补充三点大家可能不会注意的地方：

### constructor

首先是 constructor 属性，我们看个例子：

```js
function Person() {}
var person = new Person();
console.log(person.constructor === Person); // true
```

当获取 person.constructor 时，其实 person 中并没有 constructor 属性,当不能读取到constructor 属性时，会从 person 的原型也就是 Person.prototype 中读取，正好原型中有该属性，所以：

```js
person.constructor === Person.prototype.constructor
```

### __proto__

其次是 __proto__ ，绝大部分浏览器都支持这个非标准的方法访问原型，然而它并不存在于 Person.prototype 中，实际上，它是来自于 Object.prototype ，与其说是一个属性，不如说是一个 getter/setter，当使用 obj.__proto__ 时，可以理解成返回了 Object.getPrototypeOf(obj)。

### 真的是继承吗？

最后是关于继承，前面我们讲到“每一个对象都会从原型‘继承’属性”，实际上，继承是一个十分具有迷惑性的说法，引用《你不知道的JavaScript》中的话，就是：

继承意味着复制操作，然而 JavaScript 默认并不会复制对象的属性，相反，**JavaScript 只是在两个对象之间创建一个关联，这样，一个对象就可以通过委托访问另一个对象的属性和函数，所以与其叫继承，委托的说法反而更准确些。**





来源于 [冴羽](https://github.com/mqyqingfeng) [mqyqingfeng](https://github.com/mqyqingfeng) ，用于笔记学习。



