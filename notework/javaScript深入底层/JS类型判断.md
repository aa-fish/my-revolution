# JS数据类型判断

------

判断数据常用到的方法有typeof、Object.prototype.toString、instanceof

## 1、typeof

------

可以利用 typeof 判断 **number, string, object, boolean, function, undefined, symbol** 这七种基本数据类型。

```js
typeof 1 //'number'
typeof '1' //'string'
typeof false //'boolean'
typeof function(){} //'function'
typeof undefined //'undefined'
typeof null // 'object'
typeof [] // 'object'
typeof {} // 'object'
typeof Symbol(1) //'symbol'
```



## 2、instanceof

------

instanceof运算符 用来检测 `constructor.prototype `是否存在于参数 `object` 的原型链上。

```js
function Fn(){}
Object instanceof Object // true
Function instanceof Function // true
Fn instanceof Function // true
Fn instanceof Object // true
```

**instanceof** 实现原理

instanceof 主要原理：右边变量的 prototype 在左边的原型链上即可。

```js
function new_instanceof(leftValue,rightValue){
    let rightProto = rightValue.prototype; // 取右边表达式的prototype值
    let leftProto = leftValue.__proto__; // 取表达式的__proto__值
    while(true){
    	if(leftProto===null){
        	return false   
        }
        if(leftProto===rightProto){
        	return true   
        }
        leftProto = leftProto.__proto__
    }
 }
```



## 3、**Object.prototype.toString**  

------

Object.prototype.toString是判断数据类型最准确的方法

```js
Object.prototype.toString.call(1); // "[object Number]"
Object.prototype.toString.call('1'); // "[object String]"
Object.prototype.toString.call({}); // "[object Object]"
Object.prototype.toString.call([]); // "[object Array]"
Object.prototype.toString.call(true); // "[object Boolean]"
Object.prototype.toString.call(() => {}); // "[object Function]"
Object.prototype.toString.call(null); // "[object Null]"
Object.prototype.toString.call(undefined); // "[object Undefined]"
Object.prototype.toString.call(Symbol(1)); // "[object Symbol]"
```



## 拓展

------

isArray() 判断是否为数组

isNaN() 判断是否为非数  (传入参数会先被隐式类型转换)

hasOwnProperty() 检测对象自身是否存在 某个属性

```js
Array.isArray([]); //true
!isNaN(1) // true

let obj = {}
obj.one = '1'
obj.hasOwnProerty('one') // true
```

isPrototypeOf() 检测一个对象是否在另一个对象的原型链上。

```js
function Foo() {}
function Bar() {}
function Baz() {}

Bar.prototype = Object.create(Foo.prototype);
Baz.prototype = Object.create(Bar.prototype);

var baz = new Baz();

console.log(Baz.prototype.isPrototypeOf(baz)); // true
console.log(Bar.prototype.isPrototypeOf(baz)); // true
console.log(Foo.prototype.isPrototypeOf(baz)); // true
console.log(Object.prototype.isPrototypeOf(baz)); // true
```

如果你有段代码只在需要操作继承自一个特定的原型链的对象的情况下执行，同 [`instanceof`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/instanceof) 操作符一样 `isPrototypeOf()` 方法就会派上用场，例如，为了确保某些方法或属性将位于对象上。

例如，检查 `baz` 对象是否继承自 `Foo.prototype`：

```js
if (Foo.prototype.isPrototypeOf(baz)) {
  // do something safe
}
```

