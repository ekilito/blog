---
title: JS原理二
data: 2023-8-17 21:10:00
description: 手写Promise
tags: [柯里化,手写Promise]
keywords: 
sticky:  # 数字越大，置顶优先
cover: https://bu.dusays.com/2023/08/13/64d7b65c57bdc.webp
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

### 手写Promise-then方法支持异步和多次调用（非链式）

**需求:**

1. 实例化传入的回调函数,内部支持异步操作
2. then方法支持多次调用

```javascript
const p = new MyPromise((resolve, reject) => {
  setTimeout(() => {
    // resolve('成功结果')
    reject('失败原因')
  }, 2000)
})

p.then(res => {
  console.log('success1:', res)
}, err => {
  console.log('error1:', err)
})
p.then(res => {
  console.log('success2:', res)
}, err => {
  console.log('error2:', err)
})
p.then(res => {
  console.log('success3:', res)
}, err => {
  console.log('error3:', err)
})
```

**核心步骤:**

1. 定义属性,保存传入的回调函数:[]
2. 调用`then`方法并且状态为`pending`时保存传入的成功/失败回调函数
3. 调用`resolve`和`reject`时执行上一步保存的回调函数

```javascript
const PENDING = 'pending'
const FULFILLED = 'fulfilled'
const REJECTED = 'rejected'
class MyPromise {
  state = PENDING
  result = undefined
  // 1. 添加handlers属性保存then方法添加的回调函数
  handlers = []

  constructor(executor) {
    const resolve = (result) => {
      this.#changeState(FULFILLED, result)
      // 调用保存在handlers中的回调函数
      // 从开头部分取出回调函数执行
      // while(this.handlers.length > 0) {
      //    通过解构获取对应的回调函数
      //    const { onFulfilled } = this.handlers.shift()
      //    onFulfilled(this.result)
      // }
      
      // 4. 调用runHandlers 执行回调函数
      this.#runHandlers()
    }
    const reject = (result) => {
      this.#changeState(REJECTED, result)
      // 4. 调用runHandlers 执行回调函数
      this.#runHandlers()
    }
    executor(resolve, reject)
  }

  // 3. 抽取方法 执行 fulfilled/rejected状态时的回调函数
  #runHandlers() {
    while (this.handlers.length > 0) {
      const { onFulfilled, onRejected } = this.handlers.shift()
      if (this.state === FULFILLED) {
          // 成功
         onFulfilled(this.result)
      } else {
          // 失败
         onRejected(this.result)
      }
    }
  }


  #changeState(state, result) {
    if (this.state !== PENDING) {
      return
    }
    this.state = state
    this.result = result
  }
  
  then(onFulfilled, onRejected) {
    onFulfilled = typeof onFulfilled === 'function' ? onFulfilled : value => value
    onRejected = typeof onRejected === 'function' ? onRejected : reason => {throw reason}
    if (this.state === FULFILLED) {
      onFulfilled(this.result)
    } else if (this.state === REJECTED) {
      onRejected(this.result)
    } else {
      // 2. 状态为 pending 时,状态还没改变，回调函数还不能执行，将回调函数添加到数组中
      this.handlers.push({
        onFulfilled, onRejected
      })
    }
  }
}
```





### 手写Promise-链式编程-成功状态+普通返回值

**需求:**

1. `then`的链式编程
2. 目前只考虑`resolve`内部返回普通值的情况

```javascript
const p = new MyPromise((resolve, reject) => {
  resolve(1)
})
p.then(res => {
  console.log(res)
  return 2
}).then(res => {
  console.log(res)
  return 3
}).then(res => {
  console.log(res)
  return 4
})
```

**核心步骤:**

1. 调整`then`方法，返回一个新的`MyPromise`对象
2. 内部获取`onFulfilled`的执行结果,传入`resolve`方法继续执行

```javascript
const PENDING = 'pending'
const FULFILLED = 'fulfilled'
const REJECTED = 'rejected'
class MyPromise {
  state = PENDING
  result = undefined
  handlers = []

  constructor(executor) {
    const resolve = (result) => {
      this.#changeState(FULFILLED, result)
      this.#runHandlers()
    }
    const reject = (result) => {
      this.#changeState(REJECTED, result)
      this.#runHandlers()
    }
    executor(resolve, reject)
  }

  #runHandlers() {
    while (this.handlers.length > 0) {
      const { onFulfilled, onRejected } = this.handlers.shift()
      if (this.state === FULFILLED) {
         onFulfilled(this.result)
      } else {
         onRejected(this.result)
      }
    }
  }


  #changeState(state, result) {
    if (this.state !== PENDING) {
      return
    }
    this.state = state
    this.result = result
  }
  then(onFulfilled, onRejected) {
    onFulfilled = typeof onFulfilled === 'function' ? onFulfilled : value => value
    onRejected = typeof onRejected === 'function' ? onRejected : reason => {throw reason}
    // 1. 创建并返回新的Promise对象
    const p2 = new MyPromise((resolve, reject) => {
      if (this.state === FULFILLED) {
          // 成功状态
        const res = onFulfilled(this.result)
        // 2. 继续调用resolve方法 then方法返回的Promise对象的resolve
        // 传递成功的结果给下一个 then
        resolve(res)
      } else if (this.state === REJECTED) {
        onRejected(this.result)
      } else {
        this.handlers.push({
          onFulfilled, onRejected
        })
      }
    })
    return p2
  }
}
```

![promise4.png](https://bu.dusays.com/2023/08/18/64df4c7cc60ec.png)


![promise5.png](https://bu.dusays.com/2023/08/18/64df4c7da4b18.png)

![promise6.png](https://bu.dusays.com/2023/08/18/64df4c7569adf.png)

### 手写Promise-链式编程-成功状态+返回Promise

**需求:**

1. `then`的链式编程
2. 目前考虑`resolve`内部返回`MyPromise`的情况

```javascript
const p = new MyPromise((resolve, reject) => {
  resolve(1)
})
p.then(res => {
  console.log(res)
  return new MyPromise((resolve, reject) => {
    resolve(2)
  })
}).then(res => {
  console.log(res)
  return new MyPromise((resolve, reject) => {
    resolve(3)
  })
}).then(res => {
  console.log(res)
})
```

**核心步骤:**

1. 内部获取`onFulfilled`的执行结果:
2. 如果是`MyPromise`实例，继续`then`下去并传入`resolve`和`reject`

```javascript
const PENDING = 'pending'
const FULFILLED = 'fulfilled'
const REJECTED = 'rejected'
class MyPromise {
  state = PENDING
  result = undefined
  handlers = []

  constructor(executor) {
    const resolve = (result) => {
      this.#changeState(FULFILLED, result)
      this.#runHandlers()
    }
    const reject = (result) => {
      this.#changeState(REJECTED, result)
      this.#runHandlers()
    }
    executor(resolve, reject)
  }

  #runHandlers() {
    while (this.handlers.length > 0) {
      const { onFulfilled, onRejected } = this.handlers.shift()
      if (this.state === FULFILLED) {
         onFulfilled(this.result)
      } else {
         onRejected(this.result)
      }
    }
  }


  #changeState(state, result) {
    if (this.state !== PENDING) {
      return
    }
    this.state = state
    this.result = result
  }
  then(onFulfilled, onRejected) {
    onFulfilled = typeof onFulfilled === 'function' ? onFulfilled : value => value
    onRejected = typeof onRejected === 'function' ? onRejected : reason => {throw reason}
    const p2 = new MyPromise((resolve, reject) => {
      if (this.state === FULFILLED) {
        const res = onFulfilled(this.result)
        // 1. 判断是否为MyPromise的实例
        if (res instanceof MyPromise) {
          // 2. 继续调用then方法 传入 resolve 和 reject
          res.then(resolve, reject)
        } else {
          resolve(res)
        }
      } else if (this.state === REJECTED) {
        onRejected(this.result)
      } else {
        this.handlers.push({
          onFulfilled, onRejected
        })
      }
    })
    return p2
  }
}
```

![promise7.png](https://bu.dusays.com/2023/08/18/64df4c7a1e191.png)

![promise8.png](https://bu.dusays.com/2023/08/18/64df4c7b1b34c.png)

![promise9.png](https://bu.dusays.com/2023/08/18/64df4c7da8e76.png)

### 手写Promise-链式编程-失败状态

**需求:**

1. `then`的第二个回调函数，执行`reject`时的链式编程

```javascript
const p = new MyPromise((resolve, reject) => {
  resolve(1)
})

p.then(res => {
  console.log(res)
  return new MyPromise((resolve, reject) => {
    reject(2)
  })
}).then(undefined, err => {
  console.log('err:', err)
})
```

**核心步骤:**

1. 参考`resolve`的逻辑
2. 先实现功能,再抽取为函数直接调用

```javascript
const PENDING = 'pending'
const FULFILLED = 'fulfilled'
const REJECTED = 'rejected'
class MyPromise {
  state = PENDING
  result = undefined
  handlers = []

  constructor(executor) {
    const resolve = (result) => {
      this.#changeState(FULFILLED, result)
      this.#runHandlers()
    }
    const reject = (result) => {
      this.#changeState(REJECTED, result)
      this.#runHandlers()
    }
    executor(resolve, reject)
  }

  #runHandlers() {
    while (this.handlers.length > 0) {
      const { onFulfilled, onRejected } = this.handlers.shift()
      if (this.state === FULFILLED) {
         onFulfilled(this.result)
      } else {
         onRejected(this.result)
      }
    }
  }


  #changeState(state, result) {
    if (this.state !== PENDING) {
      return
    }
    this.state = state
    this.result = result
  }
  then(onFulfilled, onRejected) {
    onFulfilled = typeof onFulfilled === 'function' ? onFulfilled : value => value
    onRejected = typeof onRejected === 'function' ? onRejected : reason => {
      throw reason;
    };
    const p2 = new MyPromise((resolve, reject) => {
      if (this.state === FULFILLED) {
        // const res = onFulfilled(this.result)
        // if (res instanceof MyPromise) {
        //   res.then(resolve, reject)
        // } else {
        //   resolve(res)
        // }
        this.#runPromise(onFulfilled, resolve, reject)
      } else if (this.state === REJECTED) {
        // 1. 参考成功状态的逻辑实现 失败状态
        // const res = onRejected(this.result)
        // if (res instanceof MyPromise) {
        //   res.then(resolve, reject)
        // } else {
        //   reject(res)
        // }
        this.#runPromise(onRejected, resolve, reject)
      } else {
        this.handlers.push({
          onFulfilled, onRejected
        })
      }
    })
    return p2
  }
  // 2. 抽取 then中的逻辑，并替换掉原本代码
  #runPromise(callback, resolve, reject) {
     // 调用回调函数 获取执行的结果
    const res = callback(this.result) 
    if (res instanceof MyPromise) {
        // res 是promise对象 then 方法
      res.then(resolve, reject)
    } else {
      this.state === FULFILLED ? resolve(res) : reject(res)
    }
  }
}
```





### 手写Promise-链式编程-支持异步

**需求:**

1. 执行异步操作时，支持链式编程

```javascript
const p = new MyPromise((resolve, reject) => {
  setTimeout(() => {
    resolve(1)
  }, 2000)
})

p.then(res => {
  console.log(res)
  return new MyPromise((resolve, reject) => {
    setTimeout(() => {
      reject(2)
    }, 2000)
  })
})
    
    .then(undefined, err => {
  console.log('err:', err)
})
```

**核心步骤:**

1. then的内部将`resolve`,`reject`也推送到数组中
2. 调整`runHandlers`函数，内部直接调用`runPromise`函数即可

```javascript
const PENDING = 'pending'
const FULFILLED = 'fulfilled'
const REJECTED = 'rejected'
class MyPromise {
  state = PENDING
  result = undefined
  handlers = []

  constructor(executor) {
    const resolve = (result) => {
      this.#changeState(FULFILLED, result)
      this.#runHandlers()
    }
    const reject = (result) => {
      this.#changeState(REJECTED, result)
      this.#runHandlers()
    }
    executor(resolve, reject)
  }

  #runHandlers() {
    while (this.handlers.length > 0) {
      // 2. 解构出resolve,reject执行和上一步一样的逻辑
      const { onFulfilled, onRejected, resolve, reject } = this.handlers.shift()
      if (this.state === FULFILLED) {
        this.#runPromise(onFulfilled, resolve, reject)
      } else {
        this.#runPromise(onRejected, resolve, reject)
      }
    }
  }


  #changeState(state, result) {
    if (this.state !== PENDING) {
      return
    }
    this.state = state
    this.result = result
  }
  then(onFulfilled, onRejected) {
    onFulfilled = typeof onFulfilled === 'function' ? onFulfilled : value => value
    onRejected = typeof onRejected === 'function' ? onRejected : reason => {
      throw reason;
    };
    const p2 = new MyPromise((resolve, reject) => {
      if (this.state === FULFILLED) {
        this.#runPromise(onFulfilled, resolve, reject)
      } else if (this.state === REJECTED) {
        this.#runPromise(onRejected, resolve, reject)
      } else {
        // 1. 将 resolve和reject也推送到数组中
        this.handlers.push({
          onFulfilled, onRejected, resolve, reject
        })
      }
    })
    return p2
  }
  #runPromise(callback, resolve, reject) {
    const res = callback(this.result)
    if (res instanceof MyPromise) {
      res.then(resolve, reject)
    } else {
      this.state === FULFILLED ? resolve(res) : reject(res)
    }
  }
}
```

### 手写Promise-使用微任务

**需求:**

1. 如下代码打印结果为`1,2,4,3`

```javascript
console.log(1)
const p = new MyPromise((resolve, reject) => {
  console.log(2)
  resolve(3)
})
p.then(res => {
  console.log(res)
})
console.log(4)
```

**核心步骤:**

1. 使用`queueMicrotask`包裹`runPromise`的内部逻辑即可
2. [传送门:MDN-queueMicrotask](https://developer.mozilla.org/zh-CN/docs/Web/API/queueMicrotask)
3. [传送门:MDN-queueMicrotask使用指南](https://developer.mozilla.org/zh-CN/docs/Web/API/HTML_DOM_API/Microtask_guide)

```javascript
  #runPromise(callback, resolve, reject) {
    // 1. 使用queueMicrotask 包裹内部逻辑即可
    queueMicrotask(() => {
      const res = callback(this.result)
      if (res instanceof MyPromise) {
        res.then(resolve, reject)
      } else {
        this.state === FULFILLED ? resolve(res) : reject(res)
      }
    })
  }
```

### 小结:

手写`Promise`的核心代码:

```javascript
// 保存状态的常量 避免后续 硬编码（代码中写死某个值）
    const PENDING = 'pending'
    const FULFILLED = 'fulfilled'
    const REJECTED = 'rejected'

    // 定义类 后续new实例化
    class MyPromise {
      // 状态
      state = PENDING
      // 成功、失败原因 结果
      result = undefined // 成功或失败的原因 默认不知道
      // 待执行的回调函数 异步的回调函数 [{ onFulfilled,onRejected,resolve,reject }]
      handlers = []

      // 构造函数 resolve定义 reject定义 执行传入的回调函数
      constructor(executor) {  // 接收 new MyPromise((resolve,reject)=> {console.log('立刻执行')} ) 传进来的回调函数，然后 resolve，reject 传给回调函数executor
        // 定义 resolve和reject
        const resolve = (result) => {
        // 抽取封装 #changeState
        // if(this.state !== PENDING) {
        //     return 如果状态不是等待，后面不执行 状态确定就不能改变
        //   }
        // this.state = FULFILLED
        // this.result = result
          this.#changeState(FULFILLED, result)

          // 调用保存在handlers中的回调函数
          // 从开头部分取出回调函数执行
          // while(this.handlers.length > 0) {
          //    通过解构获取对应的回调函数
          //    const { onFulfilled } = this.handlers.shift()
          //    onFulfilled(this.result)
          // }
      
          // 调用runHandlers 执行回调函数
          this.#runHandlers()
        }
        const reject = (result) => {
          this.#changeState(REJECTED, result)
          this.#runHandlers()
        }
        // 接收传入的执行器，接收定义的resolve和reject
        executor(resolve, reject)
      }

      // 根据状态执行回调函数的 私有方法
      // 执行回调函数，取出数组中的回调函数，执行到没有为止 用shift()开头弹出
      #runHandlers() {
        // 循环执行到数组长度为0为止
        while (this.handlers.length > 0) {
          // 2. 解构出resolve,reject执行和上一步一样的逻辑
          const { onFulfilled, onRejected, resolve, reject } = this.handlers.shift()
          if (this.state === FULFILLED) {  // this.state = 'fulfilled'硬编码
          // 成功 执行 和 then 中类似的逻辑
          // 获取结果，根据是否为Promise以及状态调用对应的逻辑
          // onFulfilled(this.result)
            this.#runPromise(onFulfilled, resolve, reject)
          } else {
            // 失败
            // onRejected(this.result)
            this.#runPromise(onRejected, resolve, reject)
          }
        }
      }

      //提取resolve，reject内部公共逻辑  修改状态的（pending时才可以修改，执行到没有为止） 私有方法
      #changeState(state, result) {
        if (this.state !== PENDING) {
          return
        }
        this.state = state
        this.result = result
      }

      // then方法，接收成功和失败的回调函数 根据不同的状态执行对应的回调函数
      // 链式编程 promise对象.then(xxx).then(xxx)
      then(onFulfilled, onRejected) {
        // onFulfilled 和 onRejected 的非空判断
        // 处理未传入回调函数的特殊情况
        // 如果不是函数，设置为一个 接受一个参数，直接返回该参数的函数
        onFulfilled = typeof onFulfilled === 'function' ? onFulfilled : value => value
        // 不是函数，设置一个为 接收一个参数，使用 throw 抛出的函数
        onRejected = typeof onRejected === 'function' ? onRejected : reason => {
          throw reason;
        };

        // 保证链式编程，返回Promise
        // 创建一个新的Promise对象 并返回
        const p2 = new MyPromise((resolve, reject) => {
            //  根据状态，调用不同的回调函数
          if (this.state === FULFILLED) {
            // const res = onFulfilled(this.result)
            // if (res instanceof MyPromise) {
            //   res.then(resolve, reject)
            // } else {
            //   resolve(res)
            // }
            // 抽取封装逻辑
            this.#runPromise(onFulfilled, resolve, reject)
          } else if (this.state === REJECTED) {
             //  参考成功状态的逻辑实现 失败状态
             // const res = onRejected(this.result)
             // if (res instanceof MyPromise) {
             //   res.then(resolve, reject)
             // } else {
             //   reject(res)
             // }
            this.#runPromise(onRejected, resolve, reject)
          } else {
             // 状态为 pending 时,状态还没改变，回调函数还不能执行，将回调函数添加到数组中
            this.handlers.push({
              //  将 resolve和reject也推送到数组中
              onFulfilled, onRejected, resolve, reject
            })
          }
        })
        return p2
      }

      // 满足执行条件，执行回调函数的 私有方法
      #runPromise(callback, resolve, reject) {
        // 使用微任务队列
        queueMicrotask(() => {
          // 调用回调函数 获取执行的结果
          const res = callback(this.result)
          // 判断是否为MyPromise的实例
          if (res instanceof MyPromise) {
            // res 是Promise对象 then 方法
            // 继续调用then方法 传入 resolve 和 reject
            res.then(resolve, reject)
          } else {
            // 如果是普通的值，直接resolve
            this.state === FULFILLED ? resolve(res) : reject(res)
          }
        })
      }
    }
```

![promise10.png](https://bu.dusays.com/2023/08/18/64df78c8b6ed3.png)
![promise11.png](https://bu.dusays.com/2023/08/18/64df78c8e2db3.png)

### 手写Promise-实例方法catch

**需求:**

1. 实现实例方法`catch`,可以实现如下调用

```javascript
const p = new MyPromise((resolve, reject) => {
  reject(1)
})
p.then(res => {
  console.log(res)
}).catch(err => {
  console.log('err:', err)
})
```

**核心步骤:**

1. 参考[文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch),catch等同于:`then(undefined,onRjected)`
2. 直接添加`catch`方法，内部调用`then`
3. 使用`try-catch`包裹`runPromise`,出错时,调用`reject`

```javascript
// 添加catch方法，内部参考文档的方式调用then即可 
// 实例方法：catch 本质  then(undefined, onRjected)
catch(onRjected) {
  return this.then(undefined, onRjected)
}

  // 处理回调函数执行结果
  #runPromise(callBack, resolve, reject) {
    queueMicrotask(() => {
        // 捕获异常，通过 reject 继续传递
      try {
        // 调用回调函数 获取执行的结果
        const res = callBack(this.result)
        if (res instanceof MyPromise) {
          // res是Promise对象 then方法
          res.then(resolve, reject)
        } else {
          // 如果是普通的值,直接resolve
          if (this.state === FULFILLED) {
            resolve(res)
          } else if (this.state === REJECTED) {
            reject(res)
          }
        }
      } catch (error) {
        return reject(error)
      }
    })
```



### 手写Promise-实例方法finally

**需求:**

1. 无论成功失败都会执行`finally`的回调函数
2. 回调函数不接受任何参数

```javascript
const p = new Promise((resolve, reject) => {
  reject('error')
})

p.then(res => {
  console.log(res)
}).catch(err => {
  console.log(err)
}).finally(() => {
  console.log('finally执行啦')
})
```

**核心步骤:**

1. 参考[文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/finally):finally方法类似于调用`then(onFinally,onFinally)`,且不接受任何回调函数

```javascript
// 实例方法：finally
finally(onFinally) {
    // 将传入的回调函数，作为成功/失败的回调函数
    // 成功/失败都会执行
    return this.then(onFinally,onFinally)
}
```



### 手写Promise-静态方法resolve

**需求:**

1. 返回一个带有成功原因的`Promise`对象

```javascript
// 返回一个值为2的Promise对象
MyPromise.resolve(2).then(res => {
  console.log(res)
})
const p = new MyPromise((resolve, reject) => {
  setTimeout(() => {
    resolve('success')
  }, 1000)
})
// 直接返回传入的p
MyPromise.resolve(p).then(res => {
  console.log(res)
})
```

**核心步骤:**

1. 增加静态方法`resolve`，根据传入的值返回不同的结果即可

```javascript
  // 静态方法 resolve
  // 根据传入的值，返回不同的结果即可
static resolve(value) {
    // 如果是Promise 返回Promise，后续即可链式调用
    if (value instanceof MyPromise) {
      return value
    }
    // 如果都不是的话，直接返回一个新的Promise对象 将value传递给resolve
    return new MyPromise((resolve, reject) => {
      resolve(value)
    })
  }
```



### 手写Promise-静态方法reject

**需求:**

1. 返回一个带有拒绝原因的`Promise`对象

```javascript
MyPromise.reject('error').catch(err => {
  console.log(err)
})
```

**核心步骤:**

1. 添加静态方法内部返回有拒绝原因的`Promise`对象即可

```javascript
  static reject(err) {
    return new MyPromise((resolve, reject) => {
      reject(err)
    })
  }
```

这个两个方法在axios拦截器里可以发现到。



### 手写Promise-静态方法race

**需求:**

1. 接收Promise数组
   1. 第一个Promise成功或失败时，返回一个该Promise对象及原因

```javascript
const promise1 = new MyPromise((resolve, reject) => {
  setTimeout(() => {
    resolve('one')
  }, 500)
})

const promise2 = new MyPromise((resolve, reject) => {
  setTimeout(() => {
    reject('two')
  }, 100)
})

MyPromise.race([promise1, promise2]).then((value) => {
  console.log('value:',value)
}, err => {
  console.log('err:', err)
})
```



**核心步骤:**

1. 内部返回新的Promise对象:
   1. 参数判断:
      1. 不是数组:报错
      2. 是数组:挨个解析
         1. 任意一个Promise对象成功或失败，直接resolve或reject即可

```javascript
  static race(promises) {
      // race的后面需要 .then.catch
    return new MyPromise((resolve, reject) => {
      // 参数校验 传入的是数组才继续执行
      if (Array.isArray(promises)) {
        // 如果传入的promises是空数组，则返回的promise就将永远等待
        promises.forEach(item => {
          // 通过 Promise.resolve进行处理，只要有任何一个为 成功/拒绝 即可响应结果
          // MyPromise.resolve 传入的无论是不是Promise-->都变成Promise
          // 如果不处理的话，传入普通的值，会直接报错
          MyPromise.resolve(item).then(resolve, reject)
        })
      } else {
        // 参数错误
        return reject(new TypeError('Argument is not iterable'))
      }
    })
  }
```



### 手写Promise-静态方法all

**需求:**

1. 接收Promise数组，
   1. 所有Promise都成功时，返回一个成功的Promise对象及成功数组
   2. 任何一个Promise失败，返回一个失败的Promise对象及第一个失败原因

```javascript
const promise1 = MyPromise.resolve(3);
const promise2 = 42;
const promise3 = new MyPromise((resolve, reject) => {
  setTimeout(() => {
    resolve('success')
  }, 1000);
});

MyPromise.all([promise1, promise2, promise3]).then((values) => {
  console.log(values);
});
```

**核心步骤:**

1. 包裹一个新的Promise并返回，内部进行参数校验

   1. 非数组:报错

   2. 数组:循环挨个解析

      1. 长度为0:直接返回成功状态的Promise

      2. 长度不为0:挨个解析:forEach

         1. 不是Promise对象:直接记录结果并判断是否解析完毕

         2. 是Promise对象:调用then

            1. 成功:记录结果并判断是否解析完毕
            2. 失败:直接reject

            

```javascript
 static all(promises) {
     // 本质：外部可以 then catch
    return new MyPromise((resolve, reject) => {
      // 参数校验
      if (Array.isArray(promises)) {
        // 是数组再继续执行
        // 存储结果
        const result = []
        let count = 0 // 记录结果的个数，判断是否完结
        // 如果长度为0 直接返回 fulfilled状态的Promise即可
        if (promises.length === 0) {
          return resolve(promises)
        }
        // 挨个处理
        promises.forEach((item, index) => {
          if (item instanceof MyPromise) {
            // 如果是Promise
            item.then(res => {
              count++
              // 这么做的目的是保证 结果的顺序 和 promise每一项的一致
              result[index] = res
              count === promises.length && resolve(result)
            }, err => {
              // 任何一个失败 无视其他的promise直接 reject即可
              reject(err)
            })
          } else {
            // 如果不是Promise 原样添加在数组中
            count++
            result[index] = item
            // 全部处理完毕时，响应结果
            count === promises.length && resolve(result)
          }
        })
      } else {
        // 
        // 错误提示
        return reject(new TypeError('Argument is not iterable'))
      }
    })
  }
```



### 手写Promise-静态方法allSettled

**需求:**

1. 传入Promise数组，当所有对象都已敲定时
2. 返回一个新的Promise对象及以数组形式保存的结果

```javascript
// 获取传入的Promise数组 的 敲定状态 和结果
// 包装到对象中 { value：'成功的值' , reason: '失败原因' , status: '状态' }

const promise1 = Promise.resolve('1');
const promise2 = new Promise((resolve, reject) => setTimeout(() => {
  reject('two')
}, 1000));
const promises = [promise1, promise2];

Promise.allSettled(promises).
  then((results) => { console.log(results) })
```



**核心步骤:**

1. 增加静态方法`allSettled`
2. 内部逻辑和`all`类似，需要特别注意的地方:
   1. 成功和失败的原因都会通过对象记录起来
   2. 返回一个记录了成功`{state:FULFILLED,value:'xxx'}`失败`{state:REJECTED,reason:'xxx'}`的结果数组

```javascript
   static allSettled(promises) {
        return new MyPromise((resolve, reject) => {
          // 参数校验
          if (Array.isArray(promises)) {
            let result = []// 结果数组
            let count = 0 // 计数器
            // 空数组直接返回
            if (promises.length === 0) return resolve(promises)

            // 挨个处理内部的Promise对象
            promises.forEach((item, index) => {
              // 使用resolve转为promise统一处理
              MyPromise.resolve(item).then(value => {
                // 成功状态
                count++
                result[index] = {
                  state: FULFILLED,
                  value
                }
                // 处理完毕之后 resolve
                count === promises.length && resolve(result)
              }, reason => {
                // 失败状态
                count++
                // 失败状态 值为 reason
                result[index] = {
                  state: REJECTED,
                  reason
                }
                // 成功和失败最终都对应到 resolve
                count === promises.length && resolve(result)
              })
            })
          } else {
              // 不是数组，报错
            return reject(new TypeError('Argument is not iterable'))
          }
        })
      }
```

### 手写Promise-静态方法any

**需求:**-[传送门](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/any)

1. 传入`Promise`数组，
   1. 任何一个`Promise`对象敲定时，返回一个新的`Promise`对象，及对应的结果
   2. 所有Promise都被拒绝时，返回一个包含所有拒绝原因的`AggregateError`错误数组

```javascript
    const promise1 = new MyPromise((resolve, reject) => {
      setTimeout(() => {
        reject('error1')
      }, 2000);
    });
    const promise2 = new MyPromise((resolve, reject) => {
      setTimeout(() => {
        reject('error2')
      }, 3000);
    });
    const promise3 = new MyPromise((resolve, reject) => {
      setTimeout(() => {
        // resolve('success1')
        reject('error3')
      }, 1000);
    });

    MyPromise.any([promise1, promise2, promise3]).then((values) => {
      console.log(values);
    }, err => {
      console.log('err:', err)
    })
```



**核心步骤:**

1. 类似于`all`核心区别
   1. 数组长度为0，直接返回错误数组
   2. 任何一个成功，直接成功
   3. 通过数组记录失败原因，都失败时响应错误

```javascript
  static any(promises) {
    return new MyPromise((resolve, reject) => {
      // 参数校验
      if (Array.isArray(promises)) {
        let errors = []
        let count = 0
        // AggregateError包含多个错误对象的 单个错误对象（错误对象容器）
        if (promises.length === 0) return reject(new AggregateError('All promises were rejected'))

        // 挨个处理
        promises.forEach(item => {
          item.then(value => {
            // 只要一个成功 就成功
            resolve(value)
          }, reason => {
            count++
            errors.push(reason)
            // 如果没有一个promise成功 就把所有的错误原因合并到一起 一起抛出
            count++ === promises.length && reject(new AggregateError(errors))

          })
        })
      } else {
        // 参数格式有误
        return reject(new TypeError('Argument is not iterable'))
      }
    })
  }
```

### promise需要掌握的点：

1. 组织异步，回调函数 => 链式编程

2. async await : await会等待后面Promise成功，并获取结果，try-catch捕获异常

3. 多个异步管理：

   1. all ：都成功，第一个失败

   2. race：第一个成功或失败

   3. allSettled: 所有都敲定（成功/失败），以对象数组的形式获取结果

      ```javascript
      [
          {
              value: '成功原因',
              status:'fulfilled'
          },
          {
              reason: '失败原因'
              status:'rejected'
          }
      ]
      ```

   4. any : 第一个成功，或者都失败

   5. **被追问**：用在哪里：

      1. all：多个接口数据，获取完毕再渲染

      2. race: 多个服务器的相同接口，都可以获取同一份数据，为了让用户尽可能的拿到结果，race调用相同的多个接口，只要拿到就渲染。

         1.服务器1--新闻接口

         2.服务器2--新闻接口

         3.同时调用，哪个先获取到，就直接渲染

      3. allSettled,any 了解过代码

   **手写promise**

   1. 构造函数：传入回调函数，并接收resolve和reject
   2. 状态和成功/失败结果：
      1. 定义常量保存状态，定义实例属性保存状态和结果
      2. resolve和reject中修改状态记录结果
   3. then方法
      1. 多次调用：用数组来保存回调函数
      2. 链式调用：内部返回Promise
   4. 实例方法：
      1. catch: 本质就是 then(undefined,onRejected)
      2. finally: 本质 then(onFinally,onFinally)
   5. 静态方法

   

   

   