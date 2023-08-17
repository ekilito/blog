---
title: JS原理二
data: 2023-8-17 21:10:00
description: 
tags: [柯里化,Promise]
keywords: 
sticky:  # 数字越大，置顶优先
cover: https://bu.dusays.com/2023/08/13/64d7b658096f0.webp
copyright: true
toc_number: false
abbrlink: 882eae5
# swiper_index: 1 #   置顶轮播图顺序，非负整数，数字越大越靠前
categories: 前端
aside: true # 显示侧边栏 (默认 true)
swiper_index: 

---

## 函数柯里化

Currying 又译为卡瑞化或加里化，是把接受多个参数的函数变换成接受一个单一参数（最初函数的第一个参数）的函数，并且返回接受余下的参数而且返回结果的新函数的技术。

 柯里化 作为一种高阶技术, 可以提升函数的复用性和灵活性。

### 什么是函数柯里化

函数柯里化 (Currying) 是一种**将多个参数的函数转换为单个参数函数**的技术

转换完毕之后的函数:**只传递函数的一部分参数来调用，让他返回一个新的函数去处理剩下的参数。**

**例子:**

```javascript
// 调整函数 sum
function sum(num1, num2) {
  return num1 + num2
}

// 改写为 可以实现如下效果
console.log(sum(1)(2))// 
```

**核心步骤:**

1. `sum`改为接收一个参数，返回一个新函数
2. 新函数内部将**参数1**，**参数2**累加并返回

```javascript
function sum(num1) {
  return function (num2) {
    return num1 + num2
  }
}
```

### 柯里化面试题-全局变量

柯里化在面试的时候一般以笔试题出现,比如

**需求:**

```javascript
function sum(a, b, c, d, e) {
  return a + b + c + d + e
}
// 改写函数sum实现:参数传递到5个即可实现累加
// sum(1)(2)(3)(4)(5)
// sum(1)(2,3)(4)(5)
// sum(1)(2,3,4)(5)
// sum(1)(2,3)(4,5)
```

**核心步骤:**

1. 接收不定长参数
2. 存储已传递的参数
3. 判断长度
   1. 满足5:累加
   2. 不满足:继续返回**函数本身**

```javascript
// 保存参数
let nums = []
function currySum(...args) {
    // 将传入的参数 保存到数组中
  nums.push(...args)
  if (nums.length >= 5) {
      // 累加
    return nums.reduce((prev, curv) => prev + curv, 0)
  } else {
    return currySum
  }
}
```

### 柯里化面试题-使用闭包

**需求:**

1. 使用**闭包**将上一节代码中的全局变量,保护起来
2. 支持自定义累加的参数个数

```javascript
function sumMaker(length){
    // 逻辑略
}
// 支持5个累加
const sum5 = sumMaker(5)
// 支持7个累加
const sum7 = sumMaker(7)
sum7(1,2,3)(4,5,6,7)
```



**核心步骤:**

1. 定义外层函数:
   1. 定义参数`length`
   2. 将全局变量迁移到函数内
2. 定义内层函数:
   1. 参数长度判断,使用传入的参数`length`
   2. 直接复用上一节的逻辑,并返回

```javascript
function sumMaker(length) {
  let nums = []
  function inner(...args) {
      // 将传递的参数保存到数组中
    nums.push(...args)
    if (nums.length >= length) {
        // 累加 并返回
      return nums.reduce((prev, curv) => prev + curv, 0)
    } else {
      return inner
    }
  }
  return inner
}

// 支持5个累加
const sum5 = sumMaker(5)
console.log(sum5(1,2,3)(4)(5))
// 支持7个累加
const sum7 = sumMaker(7)
sum7(1,2,3)(4,5,6,7)
```

### 柯里化实际应用-类型判断

通过**参数复用**,实现一个**类型判断生成器函数**

**需求:**

1. 将下列4个类型判断函数,改写为通过函数`typeOfTest`动态生成

```javascript
// 有如下4个函数
function isUndefined(thing) {
  return typeof thing === 'undefined'
}
function isNumber(thing) {
  return typeof thing === 'number'
}
function isString(thing) {
  return typeof thing === 'string'
}
function isFunction(thing) {
  return typeof thing === 'function'
}

// 改为通过 typeOfTest 生成:
const typeOfTest =function(){
   // 参数 和 逻辑略

}
const isUndefined = typeOfTest('undefined')
const isNumber = typeOfTest('number')
const isString = typeOfTest('string')
const isFunction = typeOfTest('function')

// 可以通过 isUndefined,isNumber,isString,isFunction 来判断类型:

isUndefined(undefined) // true
isNumber('123') // false
isString('memeda') // true
isFunction(() => { }) // true
```

**核心步骤:**

1. `typeOfTest`接收参数`type`用来接收判断的类型
2. 内部返回新函数,接收需要判断的值,并基于`type`进行判断
3. 使用箭头函数改写为最简形式~~[传送门](https://github.com/axios/axios/blob/v1.x/lib/utils.js#L20)

```javascript
const typeOfTest = type => thing => typeof thing === type  
```

### 柯里化实际应用-固定参数

依旧是一个**参数复用**的实际应用

**需求:**

1. 将如下3个请求的函数(都是**post**请求),变为通过`axiosPost`函数动态生成
2. 实现函数`axiosPost`

```javascript
// 将如下3个请求,改写为调用 axiosPost函数即可实现
axios({
  url: 'url1',
  method: 'post',
  data: {}
})
axios({
  url: 'url2',
  method: 'post',
  data: {}
})
axios({
  url: 'url3',
  method: 'post',
  data: {}
})

const axiosPost = () => {
    // 参数,逻辑略
}

axiosPost('url1', data1)
axiosPost('url2', data2)
axiosPost('url3', data3)
```

**核心步骤:**

1. 函数内部固定请求方法,post
2. 函数内部调用`axios`发请求即可
3. `axios`内部就是这样实现的[传送门:](https://github.com/axios/axios/blob/v1.x/dist/axios.js#L2667)

```javascript
const axiosPost = (url, data) => {
  return axios({
    url, data,
    method: 'post'
  })
}
```

小结:

1. 函数柯里化是一种函数式编程思想:**将多个参数的函数转换为单个参数函数,调用时返回新的函数接收剩余参数**

2. 常见面试题

   ```javascript
   function sum(a, b, c, d, e) {
     return a + b + c + d + e
   }
   // 改写函数sum实现:参数传递到5个即可实现累加
   // sum(1)(2)(3)(4)(5)
   // sum(1)(2,3)(4)(5)
   // sum(1)(2,3,4)(5)
   // sum(1)(2,3)(4,5)
   ```

3. 常见应用:固定参数,比如`axios`中的:

   1. [类型判断函数](https://github.com/axios/axios/blob/v1.x/lib/utils.js#L20)
   2. [get,post,put等别名方法](https://github.com/axios/axios/blob/v1.x/dist/axios.js#L2667)



## 手写Promise

1. 实现Promise的核心用法
2. Promise的静态方法
3. 实现Promise的静态方法

首先明确Promise的核心用法

```javascript
const p = new Promise((resolve, reject) => {
    resolve('success')
    // 或者
    // reject('error')
})

// then方法的参数1: 状态为成功的回调函数
// then方法的参数2: 状态为失败的回调函数
p.then(res => {
  console.log(res)
}, err => {
  console.log(err)
})
```



### 手写Promise-构造函数

**需求:**

1. 实现MyPromise类，可以用如下的方式实例化
2. 实例化时传入回调函数
   1. 回调函数立刻执行
   2. 回调函数接收函数`resolve`和`reject`

```javascript
const p = new MyPromise((resolve, reject) => {
  // resolve() 
  // reject() 
})
```

**核心步骤:**

1. 定义类`MyPromise`
2. 实现构造函数，接收`executor`--传入的回调函数
3. 构造函数中定义`resolve`和`reject`并传入`executor`

```javascript
// 1. 定义类
class MyPromise {
  // 2. 构造函数 
  // executor 执行器，实例化时立刻调用
  constructor(executor) {
    // 3. 定义 resolve reject 传入executor
    const resolve = () => {
      console.log('resolve-call')
    }
    const reject = () => {
      console.log('reject-call')
    }
    executor(resolve, reject)
  }
}
```

![promise1.png](https://bu.dusays.com/2023/08/17/64de3335713d9.png)



### 手写Promise-状态、成功or失败原因

**需求:**

1. `MyPromise`增加`state`属性，只能是如下3个值
   1. `pending`:待定，默认状态
   2. `fulfilled`:已兑现，操作成功
   3. `rejected`:已拒绝，操作失败
2. `MyPromise`增加`result`属性，记录成功/失败原因
3. 调用`resolve`或`reject`,修改状态,并记录成功/失败原因

```javascript
const p = new MyPromise((resolve, reject) => {
  // resolve('成功结果')
  reject('失败原因')
})
console.log(p)
```

**核心步骤:**

1. 定义常量保存状态，避免**硬编码**
2. `MyPromise`中定义
   1. 属性:`state`保存状态，`result`成功/失败原因
   2. 修改`state`的私有方法，修改状态并记录`result`
   3. 注意:`state`只有在`pending`时，才可以修改，且不可逆

```javascript
// 1. 定义常量保存状态
const PENDING = 'pending'
const FULFILLED = 'fulfilled'
const REJECTED = 'rejected'
class MyPromise {
  // 2. 定义属性 state（状态） reason（成功/失败原因）
  state = PENDING  // 默认状态
  result = undefined  // 成功或失败的原因 默认不知道

  constructor(executor) {
      
    // 3. 实现 resolve和reject内部逻辑
    const resolve = (result) => {
        // if(this.state !== PENDING) {
        //     return 如果状态不是等待，后面不执行 状态确定就不能改变
        //   }
        // this.state = FULFILLED
        // this.result = result
      this.#changeState(FULFILLED, result)
    }
    
    const reject = (result) => {
      this.#changeState(REJECTED, result)
    }
    
    executor(resolve, reject)
  }

  // 4. 提取resolve和reject内部公共逻辑
  #changeState(state, result) {
    if (this.state !== PENDING) {
      return
    }
    this.state = state
    this.result = result
  }
}
```

![promise2.png](https://bu.dusays.com/2023/08/17/64de3335a27f7.png)





### 手写Promise-then方法的核心功能

**需求:**

1. then方法的回调函数1: 状态变为`fulfilled`时触发，并获取成功结果
2. then方法的回调函数2: 状态变为`rejected`时触发，并获取失败原因
3. then方法的回调函数1或2没有传递的特殊情况处理，[参考:then方法的参数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/then#%E5%8F%82%E6%95%B0)

```javascript
const p = new MyPromise((resolve, reject) => {
  // resolve('成功结果')
  reject('失败原因')
})

p.then(res => {
  console.log('success:', res)
}, err => {
  console.log('error:', err)
})

```

**核心步骤:**

1. 增加`then`方法，根据不同的状态执行对应的回调函数，并传入`result`
   1. 参数1:成功的回调函数
   2. 参数2:失败的回调函数
2. 没有传递`onFulfilled`,`onRejected`时，设置默认值(参考文档)

```javascript
const PENDING = 'pending'
const FULFILLED = 'fulfilled'
const REJECTED = 'rejected'
class MyPromise {
  state = PENDING
  result = undefined

  constructor(executor) {
    const resolve = (result) => {
      this.#changeState(FULFILLED, result)
    }
    const reject = (result) => {
      this.#changeState(REJECTED, result)
    }
    executor(resolve, reject)
  }

  #changeState(state, result) {
    if (this.state !== PENDING) {
      return
    }
    this.state = state
    this.result = result
  }
  
  // 1. 增加then方法，根据不同的状态执行对应的回调函数
  then(onFulfilled, onRejected) {
    // 2. 处理未传入回调函数的特殊情况
      // 如果不是函数，设置为一个 接受一个参数，直接返回该参数的函数
    onFulfilled = typeof onFulfilled === 'function' ? onFulfilled : (value) => value
      // 不是函数，设置一个为 接收一个参数，使用 throw 抛出的函数
    onRejected = typeof onRejected === 'function' ? onRejected : reason => {
      throw reason
    }
    // 3. 根据状态，调用不同的回调函数
    if (this.state === FULFILLED) {
        // 成功的状态
        // 调用对应的回调函数，并传递结果
      onFulfilled(this.result)
    } else if (this.state === REJECTED) {
      onRejected(this.result)
    }
  }
}
```

![promise3.png](https://bu.dusays.com/2023/08/17/64de33344950c.png)

