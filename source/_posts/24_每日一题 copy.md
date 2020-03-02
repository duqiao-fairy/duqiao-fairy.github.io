---
title: 每日一题
---

[参考](https://juejin.im/post/5ac2329b6fb9a028bf057caf)

### 1. 写React/Vue项目时为什么要在列表组件中写key, 其作用是什么?

key的作用是为了在diff算法执行时更快的找到对应的节点，提高diff速度。

vue和react都是采用diff算法来对比新旧虚拟节点，从而更新节点。在vue的diff函数中（建议先了解一下diff算法过程）。
在交叉对比中，当新节点跟旧节点头尾交叉对比没有结果时，会根据新节点的key去对比旧节点数组中的key，从而找到相应旧节点（这里对应的是一个key => index 的map映射）。如果没找到就认为是一个新增节点。而如果没有key，那么就会采用遍历查找的方式去找到对应的旧节点。一种一个map映射，另一种是遍历查找。相比而言。map映射的速度更快。

就react而言，key是对于列表组件而言，并且无key或者key不唯一会报错提示

### 2. ['1', '2', '3'].map(parseInt) what & why ?
**答案: [1, NaN, NaN]**

  parseInt()传递两个参数: 字符串和基数。
  所以实际执行的的代码是：
```javascript
  ['1', '2', '3'].map((item, index) => {
    return parseInt(item, index)
  })

  // 即返回的值分别为:
  parseInt('1', 0) // 1
  parseInt('2', 1) // NaN
  parseInt('3', 2) // NaN, 3 不是二进制

```

### 3. 什么是防抖和节流? 有什么区别? 如何实现
**1. 防抖**

> 触发高频时间后n秒函数只会执行一次, 如果n秒内高频事件再次被触发, 则重新计算时间

* 思路: 

> 每次触发事件时都取消之前的延时调用方法

```javascript
  function debounce(fn) {
      let timeout = null; // 创建一个标记用来存放定时器的返回值
      return function () {
        clearTimeout(timeout); // 每当用户输入的时候把前一个 setTimeout clear 掉
        timeout = setTimeout(() => { // 然后又创建一个新的 setTimeout, 这样就能保证输入字符后的 interval 间隔内如果还有字符输入的话，就不会执行 fn 函数
          fn.apply(this, arguments);
        }, 500);
      };
    }
    function sayHi() {
      console.log('防抖成功');
    }

    var inp = document.getElementById('inp');
    inp.addEventListener('input', debounce(sayHi)); // 防抖
```
**2. 节流**

> 高频事件触发，但在n秒内只会执行一次，所以节流会稀释函数的执行频率

* 思路: 

> 每次触发事件时都判断当前是否有等待执行的延时函数

```javascript
  function throttle(fn) {
      let canRun = true; // 通过闭包保存一个标记
      return function () {
        if (!canRun) return; // 在函数开头判断标记是否为true，不为true则return
        canRun = false; // 立即设置为false
        setTimeout(() => { // 将外部传入的函数的执行放在setTimeout中
          fn.apply(this, arguments);
          // 最后在setTimeout执行完毕后再把标记设置为true(关键)表示可以执行下一次循环了。当定时器没有执行的时候标记永远是false，在开头被return掉
          canRun = true;
        }, 500);
      };
    }
    function sayHi(e) {
      console.log(e.target.innerWidth, e.target.innerHeight);
    }
    window.addEventListener('resize', throttle(sayHi));
```

### 4. 介绍下 Set、Map、WeakSet 和 WeakMap 的区别？
**1. set: 无重复值的数组**

Set 结构的实例有以下属性: 

* Set.prototype.constructor：构造函数，默认就是Set函数。
* Set.prototype.size：返回Set实例的成员总数。
* Set 实例的方法分为两大类：操作方法（用于操作数据）和遍历方法（用于遍历成员）。下面先介绍四个操作方法。
* add(value)：添加某个值，返回 Set 结构本身。
* delete(value)：删除某个值，返回一个布尔值，表示删除是否成功。
* has(value)：返回一个布尔值，表示该值是否为Set的成员。
* clear()：清除所有成员，没有返回值。
* keys()：返回键名的遍历器
* values()：返回键值的遍历器
* entries()：返回键值对的遍历器
* forEach()：使用回调函数遍历每个成员

**2. map: 无重复值的数组**

JavaScript 的对象（Object），本质上是键值对的集合（Hash 结构），但是传统上只能用字符串当作键。这给它的使用带来了很大的限制。

### 5. 介绍下深度优先遍历和广度优先遍历，如何实现？

**深度优先(DFS): 找到一个节点后, 把它的后背都找出来, 最常用递归法**
**广度优先(BFS): 找到一个节点后, 把他同级的兄弟节点都找出来放在前边, 还在放到后边,最常用while**

![image](https://note.youdao.com/yws/api/personal/file/WEB947603bd74ca243975ec7f20b31a1d30?method=download&shareKey=f61cde0dd64ef496645ac3a0324c304e)

```javascript
/*深度优先遍历三种方式*/
let deepTraversal1 = (node, nodeList = []) => {
  if (node !== null) {
    nodeList.push(node)
    let children = node.children
    for (let i = 0; i < children.length; i++) {
      deepTraversal1(children[i], nodeList)
    }
  }
  return nodeList
}
let deepTraversal2 = (node) => {
    let nodes = []
    if (node !== null) {
      nodes.push(node)
      let children = node.children
      for (let i = 0; i < children.length; i++) {
        nodes = nodes.concat(deepTraversal2(children[i]))
      }
    }
    return nodes
  }
// 非递归
let deepTraversal3 = (node) => {
  let stack = []
  let nodes = []
  if (node) {
    // 推入当前处理的node
    stack.push(node)
    while (stack.length) {
      let item = stack.pop()
      let children = item.children
      nodes.push(item)
      // node = [] stack = [parent]
      // node = [parent] stack = [child3,child2,child1]
      // node = [parent, child1] stack = [child3,child2,child1-2,child1-1]
      // node = [parent, child1-1] stack = [child3,child2,child1-2]
      for (let i = children.length - 1; i >= 0; i--) {
        stack.push(children[i])
      }
    }
  }
  return nodes
}
```
```javascript
// 广度优先遍历
let widthTraversal2 = (node) => {
  let nodes = []
  let stack = []
  if (node) {
    stack.push(node)
    while (stack.length) {
      let item = stack.shift()
      let children = item.children
      nodes.push(item)
        // 队列，先进先出
        // nodes = [] stack = [parent]
        // nodes = [parent] stack = [child1,child2,child3]
        // nodes = [parent, child1] stack = [child2,child3,child1-1,child1-2]
        // nodes = [parent,child1,child2]
      for (let i = 0; i < children.length; i++) {
        stack.push(children[i])
      }
    }
  }
  return nodes
}
```


### 6. 请分别用深度优先思想和广度优先思想实现一个拷贝函数
**DFS: 递归**
**BFS: 队列**

```javascript
  // 
```

### 7. ES5/ES6 的继承除了写法以外还有什么区别？？


### 8. setTimeout、Promise、Async/Await 的区别？

这三者在事件循环的区别, 事件循环中氛围宏任务队列和微任务队列.
其中**settimeout**的回调函数放到宏任务队列里, 等到执行栈清空以后执行;
**promise.then**里的会放到相应微任务队列里, 等宏任务里面的同步代码执行完再执行;
**async**函数表示函数里面可能会有异步方法, await后面跟一个表达式, async方法执行时, 遇到await会立即执行表达式, 然后把表达式后面的代码放到微任务队列里, 让出执行栈让同步代码先执行.

### 9. Async/Await 如何通过同步的方式实现异步



### 10. 异步笔试题

> 请写出下面代码的运行结果

```javascript
  async function async1() {
      console.log('async1 start');
      await async2();
      console.log('async1 end');
  }
  async function async2() {
      console.log('async2');
  }
  console.log('script start');
  setTimeout(function() {
      console.log('setTimeout');
  }, 0)
  async1();
  new Promise(function(resolve) {
      console.log('promise1');
      resolve();
  }).then(function() {
      console.log('promise2');
  });
  console.log('script end');

  // script start
  // async1 start
  // async2
  // promise1
  // script end
  // async1 end
  // promise2
  // setTimeout

```


