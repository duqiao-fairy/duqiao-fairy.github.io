---
title: 前端知识_javascript基础
---
整理掘金上的文章 一名【合格】前端工程师的自检清单: https://juejin.im/post/5cc1da82f265da036023b628 的答案

## 变量和类型

### 1. JavaScript规定了几种语言类型

```javascript
两类: 基础类型和引用类型;
基础类型: Number, String, Boolean, Null, Undifined, Symbol(ES6新增);
引用类型: Object;
```

More info: [Writing](https://hexo.io/docs/writing.html)

### 2. JavaScript对象的底层数据结构是什么

```javascript
对象数据被存储于堆中(如对象, 数组, 函数等, 它们是通过拷贝和new出来的)
引用类型的数据的地址指针是存储于栈中的, 当我们想要访问引用类型的值的时候, 需要先从栈中获得对象的地址指针, 然后, 再通过地址指针找到对堆中所需要的数据
```

More info: [从Chrome源码看JS Object的实现](https://zhuanlan.zhihu.com/p/26169639)

### 3. Symbol类型在实际开发中的应用、可手动实现一个简单的Symbol

```javascript
ES6 引入了一种新的原始数据类型 Symbol，表示独一无二的值。
symbol类型的 key 不能被 Object.keys 和 for..of 循环枚举。因此可当作私有变量使用。

let mySymbol = Symbol('key')
// 第一种写法
let a = {}
a[mySymbol] = 'Hello'
// 第二种写法
let a = {
  [mySymbol]: 'Hello'
}
```

More info: [手动实现Symbol](https://github.com/mqyqingfeng/Blog/issues/87)

### 4.JavaScript中的变量在内存中的具体存储形式

``` bash
栈内存和堆内存
JavaScript中的变量分为基本类型和引用类型
基本类型是保存在栈内存中的简单数据段, 他们数值都有固定的大小, 保存在栈空间, 通过按值访问
引用类型是保存在堆内存中的对象, 值大小不固定, 栈内存中存放该对象的访问地址指向堆内存中的对象, JavaScript不允许直接访问堆内存中的位置, 因此操作对象时, 实际操作对象的引用
```

More info: [JavaScript中的变量在内存中的具体存储形式](https://www.jianshu.com/p/80bb5a01857a)

### 5.基本类型对应的内置对象，以及他们之间的装箱拆箱操作

String(), Number(), Boolean()

装箱: 就是把基本类型转变为相应的对象, 装箱分为隐式和显示

```javascript
// 隐式装箱: 每当读取一个基本类型的值的时候, 后台会创建一个该基本类型所对应的的独享
// 在这个基本类型上调用方法, 其实是在这个基本类型对象上调用方法
// 这个基本类型的对象是临时的, 它只存在于方法调用那一行代码执行的瞬间, 执行方法后立刻被销毁
let num = 123
num.toFixed(2) // '123.00'
// 上方代码在后台的真正步骤为
var c = new Number(123)
c.toFixed(2)
c = null
// 显示装箱: 通过内置对象 Boolean, Object, String等可以对基本类型显示装箱
var obj = new String('123)
```

拆箱: 拆箱与装箱相反, 把对象转变为基本类型的值

```javascript
Number([1]) // 1
// 转变演变
[1].valueOf() // [1]
[1].toString() // '1'
Number('1') // 1
```

### 6.理解值类型和引用类型

JavaScript中的变量分为基本类型和引用类型
基本类型: 保存在栈内存中的简单数据段, 它们的值都有固定的大小, 保存在栈空间, 通过按值访问

引用类型: 保存在堆内存中的对象, 值大小不固定, 栈内存中存放的该对象的访问地址指向堆内存中的对象, JavaScript不允许直接访问堆内存中的位置, 因此操作对象时, 实际操作对象的引用

### 7.null和undefined的区别

1. Number 转换的值不同，Number(null) 输出为 0, Number(undefined) 输出为 NaN

2. null 表示一个值被定义了，但是这个值是空值

3. undefined 表示缺少值，即此处应该有值，但是还没有定义

### 8.至少可以说出三种判断JavaScript数据类型的方式，以及他们的优缺点，如何准确的判断数组类型

1. typeof —— 返回给定变量的数据类型，可能返回如下字符串：
```javascript
 'undefined'——Undefined
  'boolean'——Boolean
  'string'——String
  'number'——Number
  'symbol'——Symbol
  'object'——Object / Null （Null 为空对象的引用）
  'function'——Function
  // 对于一些如 error() date() array()无法判断，都是显示object类型
```
2. instanceof 检测 constructor.prototype是否存在于参数object的原型链上, 是则返回true, 不是则返回false
```javascript
  alert([1,2,3] instanceof Array) // true
  alert(new Date() instanceof Date) // true
  alert(function(){this.name="22";} instanceof Function) //true
  alert(function(){this.name="22";} instanceof function) //false
  // instanceof 只能用来判断两个对象是否属于实例关系，而不能判断一个对象实例具体属于哪种类型。
```
3. constructor —— 返回对象对应的构造函数。
```javascript
  alert({}.constructor === Object);  =>  true
  alert([].constructor === Array);  =>  true
  alert('abcde'.constructor === String);  =>  true
  alert((1).constructor === Number);  =>  true
  alert(true.constructor === Boolean);  =>  true
  alert(false.constructor === Boolean);  =>  true
  alert(function s(){}.constructor === Function);  =>  true
  alert(new Date().constructor === Date);  =>  true
  alert(new Array().constructor === Array);  =>  true
  alert(new Error().constructor === Error);  =>  true
  alert(document.constructor === HTMLDocument);  =>  true
  alert(window.constructor === Window);  =>  true
  alert(Symbol().constructor);    =>    undefined 
  // null 和 undefined 是无效的对象，没有 constructor，因此无法通过这种方式来判断。
```
4. Object.prototype.toString() 默认返回当前对象的 [[Class]] 。这是一个内部属性，其格式为 [object Xxx] ，是一个字符串，其中 Xxx 就是对象的类型。
```javascript
  Object.prototype.toString.call(new Date);//[object Date]
  Object.prototype.toString.call(new String);//[object String]
  Object.prototype.toString.call(Math);//[object Math]
  Object.prototype.toString.call(undefined);//[object Undefined]
  Object.prototype.toString.call(null);//[object Null]
  Object.prototype.toString.call('') ;   // [object String]
  Object.prototype.toString.call(123) ;    // [object Number]
  Object.prototype.toString.call(true) ; // [object Boolean]
  Object.prototype.toString.call(Symbol()); //[object Symbol]
  Object.prototype.toString.call(new Function()) ; // [object Function]
  Object.prototype.toString.call(new Date()) ; // [object Date]
  Object.prototype.toString.call([]) ; // [object Array]
  Object.prototype.toString.call(new RegExp()) ; // [object RegExp]
  Object.prototype.toString.call(new Error()) ; // [object Error]
  Object.prototype.toString.call(document) ; // [object HTMLDocument]
  Object.prototype.toString.call(window) ; //[object global] window 是全局对象 global 的引用
```

### 9.可能发生隐式类型转换的场景以及转换原则，应如何避免或巧妙应用

隐式转换一般说的是Boolean的转换
在if的语句中, null, "", undefined, 0, false都会被转化为false
一般应用于对接口数据判空时使用

### 10.出现小数精度丢失的原因，JavaScript可以存储的最大数字、最大安全数字，JavaScript处理大数字的方法、避免精度丢失的方法

精度丢失原因，说是 JavaScript 使用了 IEEE 754 规范，二进制储存十进制的小数时不能完整的表示小数

能够表示的最大数字 Number.MAX_VALUE 等于 1.7976931348623157e+308 ,最大安全数字 Number.MAX_SAFE_INTEGER 等于 9007199254740991

避免精度丢失

计算小数时，先乘 100 或 1000，变成整数再运算
如果值超出了安全整数，有一个最新提案，BigInt 大整数，它可以表示任意大小的整数，注意只能表示整数，而不受安全整数的限制


More info: [JavaScript中的变量在内存中的具体存储形式](https://github.com/camsong/blog/issues/9)

## 原型和原型链(12.26)

### 1. 理解原型设计模式以及JavaScript中的原型规则

prototype 和 __ proto__
prototype: 原型
__proto__: 原型链

```
prototype: 是只有函数才有的, 别的对象没有, 每一个函数都有一个原型
如: function Fn () {}
1) 一旦这样声明, 浏览器会创建一个Fn.prototype
   typeof Fn.prototype === 'object'
2) 浏览器会给这个原型对象添加一个方法, 叫做constructor, 而且'默认'这个constructor就是函数自己
   Fn.prototype.constructor = Fn
```
![image](https://note.youdao.com/yws/api/personal/file/WEB3724c00563aa2b07d19766bbff60e2a8?method=download&shareKey=2dab6578a920bb0a1996de247e5ebbae)

```
__prototype__: 原型链
1) 只要是对象就有__prototype__, 该对象的__prototype__指向该对象"构造函数的prototype"
如:
    var arr = [];
    var obj = {};

    arr.__proto__ === Array.prototype // true
    obj.__proto__ === Object.prototype // true
2) __prototype__的作用是: 可以通过__prototype__找到该对象下的方法, 而且可以一层一层的网上找, 直到顶级.
如下图, arr对象下面没有push这个方法, arr.__proto__下面才有push这个方法, 但是arr.push有值, 标识arr是可以调用push方法的, 因为arr.__proto__里面有push, arr.__proto__下面没有hasOwnProperty这个方法, arr.__proto__.__proto__下面有hasOwnProperty这个方法
arr.hasOwnProperty有值, 表示arr可以调用hasOwnProperty这个方法, 就是通过__proto__的__proto__一次原型链网上找的
顶层链是: Object.prototype
```
![image](https://note.youdao.com/yws/api/personal/file/WEB34c2d667a61c046bde75ac757477e80e?method=download&shareKey=5212ce3b4ac3a243eb9e5bf1e2eff086)

![image](https://note.youdao.com/yws/api/personal/file/WEBeacc66ed2c8533fc110a7024e843499d?method=download&shareKey=2adcf144bceae04b92a2775485ab6c66)

### 2.instanceof的底层实现原理，手动实现一个instanceof
简单说就是判断实例对象的__proto__是不是强等于对象的prototype属性, 如果不是继续往原型链上找, 直到__proto__为null为止.

```javascript
function myInstanceOf (obj, object) { // obj表示实例对象, object 表示对象
  var O = object.prototype
  obj = obj.__proto__
  while(true) {
    if (obj === null) {
      return false
    }
    if (O === obj) { // 重点; 当o严格等于obj时, 返回true
      return true
    }
    obj = obj.__proto__
  }
}
```
### 3.创建对象的方法以及他们的优缺点
more info: [JavaScript深入之创建对象的多种方式以及优缺点](https://github.com/mqyqingfeng/Blog/issues/15)
### 4.实现继承的几种方式以及他们的优缺点
为什么要继承, 就是为了展现更多形态, 更多扩展 

1). 拷贝继承

```javascript
// 1. 声明一个函数
var Person = function (options) {
  // 2. 一个私有属性
  this.sex = options.sex
}

// 3. 在公有属性prototype这里加入一个方法
Person.prototype.getSex = function () {
  return this.sex
}

var GoodPerson = function (options) {
  Person.call(this. options)
  this.job = options.job
}

```

### 5.至少说出一种开源项目(如Node)中应用原型继承的案例

### 6.可以描述new一个对象的详细过程，手动实现一个new操作符

### 7.理解es6 class构造以及继承的底层实现原理

## 作用域和闭包(2.6)

### 1.理解词法作用域和动态作用域

词法作用域也称静态作用域, javascript采用静态作用域

静态作用域: 函数的作用域基于函数创建的位置
动态作用域: 函数的作用域基于函数的使用位置

```javascript
  var value = 1

  function foo() {
    console.log(value)
  }
  function bar() {
    var value = 2
    foo()
  }
  bar() // 输出1, Javascript采用的是词法作用域, 也称为静态作用域.
```

### 2.理解JavaScript的作用域和作用域链

作用域(scope): 就是变量访问规则的有效范围.
作用域链: 当查找变量的时候, 会先从当前上下文的变量查找, 如果没有找到, 就从父级(词法层面的父级)执行上下文的变量对象查找, 
一直找到全局上下文的变量对象, 也就是全局对象. 由多个执行上下文的变量对象构成的链, 就是作用域链

### 3.理解JavaScript的执行上下文栈，可以应用堆栈信息快速定位问题

执行上下文: 当 JavaScript 代码执行一段可执行代码(executable code)时，会创建对应的执行上下文(execution context)。
执行上下有三个重要属性(也就是前面 3 题所说的内容):
  变量对象
  作用域链
  this

执行代码:
  全局代码
  函数
  eval

执行上下文栈: 每个函数都会创建执行上下文，执行上下文栈（Execution context stack，ECS）就是 JavaScript 引擎创建出来管理执行上下文的。
执行上下文栈是有全局上下文初始化，由于执行代码首先是全局代码。全局上下文永远在执行上下文栈中的最底下，只有等程序关闭才释放。

```javascript
  function foo(){
    function bar(){}
    bar()
  }
  foo()

  // 假设 ECS 是个数组
  // 第一步 初始化， 全局代码的上下文 globalContext
  ECS = [
    globalContext
  ]

  // 第二步 执行 foo

  ECS = [
    globalContext,
    fooContext
  ]
  // 第三步 执行 bar

  ECS = [
    globalContext,
    fooContext,
    barContext
  ]

  //第四步 bar 执行完成, 释放 barContext
  ECS = [
    globalContext,
    fooContext
  ]

  // 第五步 foo 执行完成, 释放 fooContext
  ECS = [
    globalContext
  ]
```

### 5.闭包的实现原理和作用，可以列举几个开发中闭包的实际应用

原理: 闭包就是能够读取其他函数内部变量的函数. 由于在javascript语言中, 只有函数内部的子函数才能读取局部变量, 
因此可以把闭包简单理解成"定一个在一个函数内部的函数"

所以, 在本质上, 闭包就是将函数内部和函数外部连接起来的一座桥梁.

作用: 闭包可以用在很多地方. 它的最大用处有两个, 一个是前面提到的可以读取函数内部的变量, 拎一个就是让这些变量的
值始终保持在内存中.

应用: 1. 匿名函数的自执行函数. 2. 结果缓存. 3. 封装局部变量

### 6.理解堆栈溢出和内存泄漏的原理，如何防止

堆栈溢出: 由于过多的函数调用, 导致调用栈无法容纳这些调用的返回地址, 一般在递归中产生.
堆栈溢出很可能由无限递归产生, 但也可能仅仅是过多的堆栈层级

内存泄露: 
1. 闭包
2. 意外的全局变量引起的内存泄露
3. 定时器没有及时的被销毁
4. 没有清理的dom元素的引用
5. 监听事件造成的泄露

内存泄露的避免: 
1. 谨慎使用闭包 a、在业务不需要用到的内部函数，可以重构在函数外，实现解除闭包.
b、闭包内，局部变量使用后或不再需要，及时的清除掉

2. 减少不必要的全局变量，如果用了，最好在声明周期钩子中或再函数调用之前，及时的清除掉.
3. 减少生命周期较长的对象，及时对无用的数据进行释放销毁.
4. 避免创建过多的对象，对不用的对象及时的释放.
5. 对注册的事件，再不用的时候，及时的解耦.释放资源.

### 7.如何处理循环的异步操作

1. 将异步操作变同步, 使用async/await
2. 去掉循环, 将循环变成递归

### 8.理解模块化解决的实际问题，可列举几个模块化方案并理解其中原理

[模块加载方案](https://github.com/mqyqingfeng/Blog/issues/108)

## 执行机制(2.7)

### 1.为何try里面放return，finally还会执行，理解其内部机制 


### 2.JavaScript如何实现异步编程，可以详细描述EventLoop机制


### 3.宏任务和微任务分别有哪些


### 4.可以快速分析一个复杂的异步嵌套逻辑，并掌握分析方法


### 5.使用Promise实现串行


### 6.Node与浏览器EventLoop的差异


### 7.如何在保证页面运行流畅的情况下处理海量数据

## 语法和API(2. 8)

### 1.理解ECMAScript和JavaScript的关系


### 2.熟练运用es5、es6提供的语法规范，


### 3.熟练掌握JavaScript提供的全局对象（例如Date、Math）、全局函数（例如decodeURI、isNaN）、全局属性（例如Infinity、undefined）


### 4.熟练应用map、reduce、filter 等高阶函数解决问题


### 5.setInterval需要注意的点，使用settimeout实现setInterval


### 6.JavaScript提供的正则表达式API、可以使用正则表达式（邮箱校验、URL解析、去重等）解决常见问题


### 7.JavaScript异常处理的方式，统一的异常处理方案




