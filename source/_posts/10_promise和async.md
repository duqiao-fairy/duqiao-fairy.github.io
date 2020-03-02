---
title: promise相关知识点
---

## 一, 关于promise的reject和catch的问题

### 1. reject后的东西, 一定会进入then中的第二个回调, 如果then中没有写第二个回调, 则进入catch

```javascript
    var p1 = new Promise((resolve, reject) => {
      // resolve('success')
      reject('fail')
    })

    p1.then((data) => {
      console.log('resolve:', data)
    }, (err) => {
      console.log('reject:', err)
    }).catch((err2) => {
      console.log('catch:', err2)
    })

    // 输出: reject: fail
```
```javascript
    p1.then((data) => {
      console.log('resolve:', data)
    }).catch((err2) => {
      console.log('catch:', err2)
    })

    // 输出 catch: fail
```

如果没有then, 也可以直接进入catch

```javascript
p1.catch((res) => {
      console.log('catch:', res)
    })

    // 输出 catch: fail
```

### 2. resolve的东西, 一定会进入then的第一个回调, 肯定不会进入catch

```javascript
    var p1 = new Promise((resolve, reject) => {
      resolve('success')
      // reject('fail')
    })

    p1.then((res) => {
      console.log('resolve:', res)
    }).catch((err) => {
      console.log('catch:', err)
    })

    // 输出: resolve: success
```

## 二, 必会的十道题

### 题目一

```javascript
    const p1 = new promise((resolve, reject) => {
      console.log(1)
      resolve()
      console.log(2)
    })
    
    p1.then(() => {
      console.log(3)
    })
    console.log(4)
```

运行结果: 
1
2
4
3
解释: Promise构造函数是同步执行的, promise.then中的函数是异步执行的

### 题目二

```javascript
    const p1 = new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve('success')
      }, 1000)
    })
    console.log(p1)
    p1.then(() => {
      console.log(p1)
      throw new Error('error')
    }).catch(() => {
      console.log(p1)
    })
```

运行结果: 
Promise { <pending> }
Promise { <resolve> 'success' }
Promise { <resolve> 'success' }
解释: promise有3种状态: pending. fulfilled和rejected. 改变状态只能是pending > fulfilled或者pending > rejected, 状态一旦改变则不能再变.

### 题目三

```javascript
  const promise = new Promise((resolve, reject) => {
    resolve('success1')
    reject('error')
    resolve('success2')
  })

  promise
    .then((res) => {
      console.log('then: ', res)
    })
    .catch((err) => {
      console.log('catch: ', err)
    })
```

运行结果: 
then: success1
解释: 构造函数中的 resolve 或reject 只有第一次执行有效, 多次调用没有任何作用, 呼应代码二结论: promise状态一旦改变则不能再变.

### 题目四

```javascript
  Promise.resolve(1)
    .then((res) => {
      console.log(res)
      return 2
    })
    .catch((err) => {
      return 3
    })
    .then((res) => {
      console.log(res)
    })
```

运行结果:
1
2
解释: promise可以链式调用, 提起链式调用我们通常会想到通过 return this 实现, 不过 Promise 并不是这样实现的. promise每次调用.then或者.catch都会返回一个新的promise, 从而实现了链式调用

### 题目五

```javascript
  const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
      console.log('once')
      resolve('success')
    }, 1000)
  })

  const start = Date.now()
  promise.then((res) => {
    console.log(res, Date.now() - start)
  })
  promise.then((res) => {
    console.log(res, Date.now() - start)
  })
```

运行结果: 
once
success 1005
success 1007
解释: promise的.then 或者.catch可以被调用多次, 但这里Promise构造函数只执行一次. 或者说promise内部状态一经改变, 并且有了一个值, 那么后续每次调用.then或者.catch都会直接拿到该值.

### 题目六

```javascript
  Promise.resolve()
    .then(() => {
      return new Error('error!!!')
    })
    .then((res) => {
      console.log('then: ', res)
    })
    .catch((err) => {
      console.log('catch: ', err)
    })
```

运行结果: 
then: Error: error!!!
    at Promise.resolve.then (...)
    at ...
解释: .then 或者 .catch中return 一个error对象并不会抛出错误, 所以不会被后续的 .catch捕获, 需要改成其中一种: 
return Promise.reject(new Error(‘error!!!’))
throw new Error(‘error!!!’)
因为返回任意一个非promise的值都会包裹成promise对象, 即 return new Error('error!!!') 等价于 return Promise.resolve(new Error('error!!!'))

### 题目七

```javascript
  const promise = Promise.resolve()
    .then(() => {
      return promise
    })
  promise.catch(console.error)
```

运行结果: 
TypeError: Chaining cycle detected for promise #
    at 
    at process._tickCallback (internal/process/next_tick.js:188:7)
    at Function.Module.runMain (module.js:667:11)
    at startup (bootstrap_node.js:187:16)
    at bootstrap_node.js:607:3

解释: .then 或.catch 返回的值不能是promise本身, 否则会造成死循环. 类似于 
process.nextTick(function tick () {
  console.log('tick')
  process.nextTick(tick)
})

### 题目八

```javascript
  Promise.resolve(1)
    .then(2)
    .then(Promise.resolve(3))
    .then(console.log)
```

运行结果: 
1
解释: .then或者.catch的参数期望是函数, 传入非函数则会发生值穿透

### 题目九
```javascript
  Promise.resolve()
    .then(function success (res) {
      throw new Error('error')
    }, function fail1 (e) {
      console.error('fail1: ', e)
    })
    .catch(function fail2 (e) {
      console.error('fail2: ', e)
    })
```

运行结果: 
fail2: Error: error
    at success (...)
    at ...

解释: .then可以接收两个参数, 第一个是处理成功的函数, 第二个是处理错误的函数, .catch 是 .then 第二个参数的简便写法, 但是它们用法上有一点需要注意: .then 的第二个处理错误的函数捕获不了第一个处理成功的函数抛出的错误, 而后续的 .catch 可以捕获之前的错误

### 题目十

```javascript
  process.nextTick(() => {
    console.log('nextTick')
  })
  Promise.resolve().then(() => {
    console.log('then')
  })
  setImmediate(() => {
    console.log('setImmediate')
  })
  console.log('end')
```

运行结果: 
end
nextTick
then
setImmediate
解释: process.nextTick 和 promise.then 都属于 microtasks, 而 setImmediate 属于 macrotasks, 在事件循环的check阶段执行. 事件循环的每个阶段(macrotasks)之间都会执行 microtasks, 事件循环的开始会先执行一次 microtasks.
