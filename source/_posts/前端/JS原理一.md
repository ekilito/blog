---
title: JS原理一
data: 2023-8-16 09:59:00
description: 
tags: [this,手写,继承,fetch,Generato]
keywords: 
sticky:  # 数字越大，置顶优先
cover: https://bu.dusays.com/2023/08/13/64d7b658096f0.webp
copyright: true
toc_number: false

# swiper_index: 1 #   置顶轮播图顺序，非负整数，数字越大越靠前
categories: 前端
aside: true # 显示侧边栏 (默认 true)
swiper_index: 

---

# JS原理

### 知识点自测

1. 函数的`call`方法-[文档链接](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/call)

```javascript
// 以指定的this调用函数，并通过 从第二个参数开始依次传递参数
function func(name,drink){
  console.log(this) // 指向obj  {name: 'kilito'}
  console.log(name)
  console.log(drink)
}
const obj = {
  name:'kilito'
}
// call 参数1: this  
//      参数2: 2-n函数的参数
func.call(obj,'kilito','咖啡')
```

2. 函数的`apply`方法-[文档链接](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)

```javascript
// 以指定的this调用函数，并通过 数组的形式 传递参数
function func(name,drink){
  console.log(this) // 指向obj  {name: 'kilito'}
  console.log(name)
  console.log(drink)
}
const obj = {
  name:'kilito'
}
// apply 参数1: this
//       参数2: 以数组的形式传入参数
func.apply(obj,['xiaoqing','咖啡'])
```

3. 函数的`bind`方法-[文档链接](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)

```javascript
function func(food, drink) {
  console.log(this) // {name: 'kilito'}
  console.log(food)
  console.log(drink)
}
const obj = {
  name: 'kilito'
}
// 返回一个绑定了this的新函数！
const bindFunc = func.bind(obj, '花菜')
bindFunc('可乐')
// const bindFunc = func.bind(obj)
// bindFunc('花菜',可乐')
```

4. 剩余参数-[文档链接](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Functions#%E5%89%A9%E4%BD%99%E5%8F%82%E6%95%B0)

```javascript
function func(...args){
  console.log(args)// 以数组的形式获取传入的所有参数
}
func('西蓝花','西葫芦','西洋参','西芹')
```

5. Promise核心用法-[文档链接](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Using_promises)

```javascript
const p = new Promise((resolve, reject) => {
  setTimeout(() => {
    const num = parseInt(Math.random() * 10)
    if (num > 5) {
      resolve(`成功啦--${num}`)
    } else {
      reject(`失败啦--${num}`)
    }
  }, 1000)
})
p.then(res => {
  console.log(res)
}, err => {
  console.log(err)
})
```

6. URLSearchParams核心用法-[文档链接](https://developer.mozilla.org/zh-CN/docs/Web/API/URLSearchParams)

```javascript
// 实例化时支持传入JS对象
const params = new URLSearchParams({ name: 'jack', age: 18 })
// toString方法 返回搜索参数组成的字符串，可直接使用在 URL 上。
console.log(params.toString())
```

7. Object.create核心用法-[文档链接](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/create)

```javascript
  const person = {
    name: 'kilito',
    foods: ['西蓝花', '西红柿', '西葫芦']
  }
  // 将传入的对象作为原型，创建一个新对象（浅拷贝）
  const clone = Object.create(person)
  clone.name = 'itheima'
  clone.foods.push('西北风')
  console.log(clone.foods === person.foods)// true
```

7. Object.assign核心用法-[文档链接](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)

```javascript
  const person = {
    name: 'kilito',
    foods: ['西蓝花', '西红柿', '西葫芦']
  }
  const son = {
    name: 'rose',
  }
  // 参数1 目标对象
  // 参数2 源对象
  // 将源对象的自身属性复制到目标对象，并返回目标对象
  const returnTarget = Object.assign(son, person)

  console.log(returnTarget === son)// true
  console.log(son.name)// itheima
  console.log(son.foods === person.foods)// true
```





## JS中的this

> [传送门：MDN-this](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/this)
>
> [传送门：MDN-call](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/call)
>
> [传送门：MDN-apply](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)
>
> [传送门：MDN-bind](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)
>
> [传送门：MDN-箭头函数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
>
> [传送门：MDN-剩余参数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/rest_parameters)
>
> [传送门：MDN-Symbol](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol)



### 如何确认this指向：

在绝大多数情况下，函数的调用方式决定了 `this` 的值（运行时绑定）

**谁调用就是谁，直接调用就是window**

```javascript
// 案例1
function func() {
  console.log(this) // window
}
func()

// 案例2
const person = {
  name: 'jack',
  sayHi: function () {
    console.log(this)// person
    function inner() {
      console.log(this)// 直接调用就是 window
    }
    inner()
  },
  sayHello(){
    console.log(this)// person
    setTimeout(function(){
        console.log(this)// window
    })
  }
}
person.sayHi()
person.sayHello()
```





### 如何改变this指向

主要有2类改变函数内部`this`指向的方法：

1. 调用函数并传入具体的`this`:
   1. `call`:
      1. 参数1:`this`() （希望this指向谁就传哪个）
      2. 参数2-n:传递给函数的参数

   2. `apply`-数组作为参数
      1. 参数1:`this`
      2. 参数2:以数组的形式,传递给函数的参数

2. 创建绑定`this`的函数:
   1. bind:返回一个绑定了`this`的新函数
   2. 箭头函数:最近的this是谁,就是谁




**调用函数并传入具体的this：**

```javascript
function funcA(p1, p2) {
  console.log('funcA-调用')
  console.log(this)
  console.log('p1:', p1)
  console.log('p2:', p2)
}
const obj = {
  name: 'kilito'
}
// call参数
// 参数1 this值 
// 参数2-参数n 挨个传入函数的参数  
funcA.call(obj, 1, 2)
// apply参数
// 参数1 this值
// 参数2 以数组的形式传入函数的参数
funcA.apply(obj, [3, 4])
```



**创建绑定this的函数：**

```javascript
function funcB(p1, p2) {
  console.log('funcB-调用')
  console.log(this)
  console.log('p1:', p1)
  console.log('p2:', p2)
}
const person = {
  name: 'kilito'
}
// bind参数
// 参数1 this值
// 参数2-参数n 绑定的参数
const bindFuncB = funcB.bind(person, 123)
bindFuncB(666)


// 箭头函数
const student = {
  name: 'kilito',
  sayHi: function () {
    console.log(this) // student
    // 箭头会从自己作用域链的上一层继承this
    const inner = () => {
      console.log('inner-调用了')
      console.log(this)  // student 箭头函数中的this，指向所在作用域中的this 沿用上一层
    }
    inner()
  }
}
student.sayHi()


// 箭头函数
const person = { // 这个大括号没有创建作用域
  name: 'kilito',
  sayHi() {
    console.log(this) // person
    // 箭头会从自己作用域链的上一层继承this
    setTimeout(()=> {
        console.log(this) // person
    },1000)
  },
     // 易混淆情况（不要这样写）
  sayHello:()=> {
      console.log(this) // window
   }
}
//person.sayHi()
    person.sayHello()
```






### 手写call方法

这一节咱们来实现`myCall`方法，实际用法和`call`方法一致，核心步骤有4步

```javascript
//  实现myCall 可以实现如下的调用效果
const obj2 = {
  name: 'kilito'
}
function func2(drink, food) {
  console.log(`我叫${this.name},我喜欢喝${drink},我爱吃${food}`)
}
// 参数1：this
// 参数2-参数n：参数列表 
func2.myCall(obj2, '咖啡', '西兰花炒蛋')

```

1. 如何定义`myCall`?
2. 如何让函数内部的`this`为某个对象？
3. 如何让`myCall`接收参数2-参数n?
4. 使用[Symbol](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol)调优`myCall`？
   1. 添加到原型上，所有函数均可调用
   2. 通过给对象动态添加属性的方式来指定 this
   3. ...args 剩余参数 实现参数传递
   4. 通过 Symbo 解决了和默认属性重名的问题

```javascript
// 1. 如何定义`myCall`
Function.prototype.myCall = function () {
  // 逻辑
}

// 2 如何让函数内部的`this`为某个对象                 // thisArg => { name:'kilito' }
Function.prototype.myCall = function (thisArg) { // 1. 接收原函数的this要指向的对象 thisArg
  // this 是调用myCall的 函数
  // thisArg 指定的this
  // 2.为他添加一个自定义属性，让函数成为他的该属性  { name:'kilito',func: 原函数 } 
  thisArg.func = this // 这个this 是原函数（谁调用，this就指向谁）
  // 3.调用并获取结果
  const res = thisArg.func()
  // 移除添加的自定义属性
  delete thisArg.func
  // 返回调用结果
  return res
}

// func.myCall(obj)

// 3 如何让`myCall`接收参数2-参数n
Function.prototype.myCall = function (thisArg, ...args) { // ...args接受传过来的参数
  thisArg.func = this
  // 调用并获取结果
  const res = thisArg.func(...args)  // 使用展开运算符传入原函数
  // 移除添加的自定义属性
  delete thisArg.func
  // 返回调用结果
  return res
}
// func.myCall(obj,1,2,3,4)


// 4 使用`Symbol`调优`myCall`
Function.prototype.myCall = function (thisArg, ...args) {
  // 使用Symbol生成唯一标记，避免和原属性冲突
  const fn = Symbol()
  // 给对象动态添加方法 指定为 this
  thisArg[fn] = this
  // 调用
  const res = thisArg[fn](...args)
  // 移除添加的自定义属性
  delete thisArg[fn]
  // 返回调用结果
  return res
}


// 测试代码
const obj2 = {
  name: '我是小小黑'
}
function func2(drink, food) {
  console.log(`我叫${this.name},我喜欢喝${drink},我爱吃${food}`)
}
func2.myCall(obj2, '咖啡', '西兰花炒蛋')
```

```javascript
// symbol
// 调用全局函数 Symbol 可以传入标记（可选）
const s1 = Symbol()
const s2 = Symbol()
const s3 = Symbol('kilito')
console.log(s1 === s2) // false
```

```javascript
Function.prototype.myCall = function (thisArg, ...args) {
  const fn = Symbol()
  thisArg[fn] = this
  const res = thisArg[fn](...args)
  delete thisArg[fn]
  return res
}
```

![手写call.png](https://bu.dusays.com/2023/08/16/64dcb002560a2.png)



### 手写apply方法

这一节咱们来实现`myApply`方法，实际用法和`apply`方法一致，核心步骤依旧`4`步

```javascript
//  实现myApply 可以实现如下的调用效果
const obj2 = {
  name: '我是小小黑'
}
function func2(drink, food) {
  console.log(`我叫${this.name},我喜欢喝${drink},我爱吃${food}`)
}
// 参数1：this
// 参数2：数组形式传入的参数列表
func2.myApply(obj2, ['咖啡', '西兰花炒蛋'])

```

1. 如何定义`myApply`? 函数Function的原型上
2. 如何让函数内部的`this`为某个对象？给对象动态增加方法,方法为原函数,通过对象调用即可
3. 如何让`myApply`接收数组形式的参数列表? 定义一个参数接收数组即可 形参: args,调用时,...args
4. 使用[Symbol](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol)调优`myApply`？和原对象的属性重名

```javascript
// 1. 如何定义`myApply`
Function.prototype.myApply = function () {
  // 逻辑略
}

// 2 如何让函数内部的`this`为某个对象
Function.prototype.myApply = function (thisArg) {
  // 为他添加一个自定义属性，让函数成为他的该属性
  thisArg['fn'] = this
  // 调用并获取结果
  const res = thisArg['fn']()
  // 移除添加的自定义属性
  delete thisArg['fn']
  // 返回调用结果
  return res
}


// 3 如何让`myApply`接收参数2-参数n
Function.prototype.myApply = function (thisArg, args) {
  thisArg['fn'] = this
  // 调用并获取结果
  // 用... 将args展开传入
  const res = thisArg['fn'](...args)
  // 移除添加的自定义属性
  delete thisArg['fn']
  // 返回调用结果
  return res
}

// 4 使用`Symbol`调优`myApply`
Function.prototype.myApply = function (thisArg, args) {
  // 使用Symbol生成唯一标记，避免和原属性冲突
  const fn = Symbol()
  thisArg[fn] = this
  const res = thisArg[fn](...args)
  // 移除添加的自定义属性
  delete thisArg[fn]
  // 返回调用结果
  return res
}


// 测试代码
const obj2 = {
  name: '我是小小黑'
}
function func2(drink, food) {
  console.log(`我叫${this.name},我喜欢喝${drink},我爱吃${food}`)
}
func2.myApply(obj2, ['咖啡', '西兰花炒蛋'])
```

小结：手写apply方法

1. 如何定义`myApply`? 函数的原型上
2. 如何让函数内部的`this`为某个对象？ 动态给对象添加方法,通过对象的方式调用方法
3. 如何让`myApply`接收数组形式的参数列表?   形参: args,调用时,...args
4. 使用[Symbol](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol)调优`myApply`？避免和默认属性重名

```javascript
Function.prototype.myApply = function (thisArg, args) {
  const fn = Symbol()
  thisArg[fn] = this
  const res = thisArg[fn](...args)
  delete thisArg[fn]
  return res
}
```







### 手写bind方法

这一节咱们来实现`myBind`方法，实际用法和`bind`方法一致，核心步骤为2步

```javascript
const obj = {
  name: '我是小小黑'
}
function func(drink, food) {
  console.log(`我叫${this.name},我喜欢喝${drink},我爱吃${food}`)
}
// 调用bind获取绑定this的新函数，参数1为可乐
const bindFunc = func.bind(obj, '可乐')
// 调用函数，只需要传递参数2即可
bindFunc('西蓝花炒蛋')
```

1. 如何返回一个绑定了`this`的函数？
2. 如何实现绑定的参数，及传入的参数合并?

```javascript
// 1 如何返回一个绑定了`this`的函数
Function.prototype.myBind = function (thisArg) {
  // myBind函数调用时，this就是函数本身 
  return () => {
    // 通过call方法将传入的 thisArg 作为this进行调用
    this.call(thisArg)  // this 指向 func
  }
}

// 2 如何实现绑定的参数，及传入的参数合并
// ...args 接收绑定参数
Function.prototype.myBind = function (thisArg, ...args) {
  // ...args2 接收调用时的参数
  return (...args2) => {
    // thisArg 需要指定的this
    // args 调用myBind时传入的参数
    // args2 调用新函数时传入的参数
    this.call(thisArg, ...args, ...args2)
  }
}

const obj = {
  name: '我是小小黑'
}
function func(drink, food) {
  console.log(`我叫${this.name},我喜欢喝${drink},我爱吃${food}`)
}
// 调用bind获取绑定this的新函数，参数1为可乐
const bindFunc = func.bind(obj, '可乐')
// 调用函数，只需要传递参数2即可
bindFunc('西蓝花炒蛋')
```

![手写bind.png](https://bu.dusays.com/2023/08/16/64dcb004b93cc.png)

小结：手写bind方法

1. 如何返回一个绑定了`this`的函数？
2. 如何实现绑定的参数，及传入的参数合并?

```javascript
Function.prototype.myBind = function (thisArg, ...args) {
  return (...args2) => {
    this.call(thisArg, ...args, ...args2)
  }
}
```



## JS继承-ES5

> 这一节咱们来学习如何在JS中实现**继承**，首先看看在ES6之前可以如何实现继承
>
> [传送门:继承与原型链](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)
>
> [传送门:继承（计算机科学）](https://zh.wikipedia.org/wiki/%E7%BB%A7%E6%89%BF_(%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6))
>
> [传送门:JavaScript高级程序设计](https://www.ituring.com.cn/book/2472)
>
> [传送门:MDN-Object.create](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/create)
>
> [传送门:MDN-Object.assign](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)

**继承：**继承可以使子类具有父类的各种属性和方法，而不需要再次编写相同的代码

这一节咱们会学习ES5中常见的继承写法(命令来源于 **《JavaScript高级程序设计》**)

1. 原型链实现继承
2. 构造函数继承
3. 组合继承
4. 原型式继承
5. 寄生式继承
6. 寄生组合式继承

```javascript
// 父类
function Parent(){
  this.name = name
  this.foods = ['西蓝花', '西红柿']
  this.sayFoods = function () {
    console.log(this.foods)
  }
}
```





### ES5-原型链实现继承

**核心步骤：**希望继承谁，就将谁作为原型

**缺点：**父类中的引用数据类型，会被所有子类共享

```javascript
// 父类构造函数
function Parent(name) {
  this.name = name
  this.foods = ['西蓝花', '西红柿']
  this.sayFoods = function () {
    console.log(this.foods)
  }
}
// 子类构造函数 
function Son() {
}

// 将父类的实例 作为子类的原型
Son.prototype = new Parent('jack')
const s1 = new Son()
s1.sayFoods()// ['西蓝花', '西红柿']

const s2 = new Son()
s2.sayFoods() // ['西蓝花', '西红柿']

console.log(s1 === s2) // false
// 引用数据类型是同一个
console.log(s1.foods === s2.foods) // true

s2.foods.push('西葫芦')  // 会影响到 s1

s2.sayFoods()// ['西蓝花', '西红柿', '西葫芦']
s1.sayFoods()// ['西蓝花', '西红柿', '西葫芦']
```



### ES5-构造函数继承

**核心步骤：**在子类的构造函数中通过`call`或`apply`父类的构造函数

**缺点：**子类没法使用父类原型上的属性/方法

```javascript
// 父类
function Parent(name) {
  this.name = name           // 3. 给传入的 this 设置属性/方法
}
Parent.prototype.sayHi = function () {
  console.log('你好,我叫:', this.name)
}

// 子类
function Son(name) {
  Parent.call(this, name)  // 2. this 指向 son 的实例化对象
}

const s1 = new Son('lucy')  // 1. 调用子类构造函数
const s2 = new Son('rose')
s1.sayHi() // 报错 子类没法用到父类原型上的属性/方法
```



### ES5-组合继承

通过组合继承,结合原型链继承和构造函数继承2种方法的优点

**核心步骤：**

1. 通过原型链继承公共的属性和方法
2. 通过构造函数继承实例独有的属性和方法

**特点：**调用了2次构造函数

```javascript
// 父类
function Person(name) {
  this.name = name
}
// 公共的属性和方法加父类原型上
Person.prototype.sayHi = function () {
  console.log(`你好，我叫${this.name}`)
}
// 子类构造函数
function Student(name, age) {
  // 调用父类构造函数传入this
  Person.call(this, name)
  // 子类独有的属性和方法单独设置
  this.age = age
}
// 设置子类的原型为 父类实例
Student.prototype = new Person()
// 调用子类的构造函数
const s = new Student('李雷', 18)

// 可以使用原型链上的 属性和方法 也可以使用 通过构造函数获取的父类的属性和方法
```





### ES5-原型式继承

直接基于对象实现继承

**核心步骤:**对某个对象进行浅拷贝(工厂函数或[Object.create](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/create)),实现继承

**缺点:**父类中的引用数据类型，会被所有子类共享

```javascript
const parent = {
  name: 'parent',
  age: 25,
  friend: ['rose', 'ice', 'robot'],
  sayHi() {
    console.log(this.name, this.age)
  }
}

// 可以用 Object.create替代

// 工厂函数：返回一个新对象
function objectFactory(obj) {
    // 定义构造函数
  function Fun() { }
    // 传入父类的对象，设置给构造函数的原型
  Fun.prototype = obj
    // 返回了实例化对象
  return new Fun()
}

const son1 = objectFactory(parent)
const son2 = objectFactory(parent)
son1.friend.push('lucy')

// 父类中的引用数据类型，会被所有子类共享
console.log(son1.friend) // ['rose', 'ice', 'robot','lucy']
console.log(son2.friend) // ['rose', 'ice', 'robot','lucy']

// 原型链继承，基于构造函数
// 原型继承，基于实例
```



### ES5-寄生式继承

**核心步骤:**

定义工厂函数,并在内部:

1. 对传入的对象进行浅拷贝(公共属性/方法)
2. 为浅拷贝对象增加属性/方法(独有属性/方法)

```javascript
// 解决引用类型共享问题
const parent = {
  name: 'parent',
  foods: ['西蓝花', '炒蛋', '花菜']
}

function createAnother(origin) {
  // Object.create基于原型创建新对象，对属性进行浅拷贝
  const clone = Object.create(origin)
  // 为对象增加属性/方法
  clone.sayHi = function () {
    console.log('你好')
  }
  return clone
}

const son1 = createAnother(parent)
const son2 = createAnother(parent)
```

```javascript
// Object.create() 静态方法以一个现有对象作为原型，创建一个新对象。
// 基于一个对象作为原型，创建一个新对象
// 对传入的对象进行浅拷贝
const father = {
    name: 'kilito',
    age: 25,
    foods: ['西瓜', '西兰花' ,'西葫芦']
}
const newF = Object.create(father)
console.log(newF === father) // false
console.log(newF.foods === father.foods) // true
```

寄生式继承

1. 寄生式继承的核心步骤是?
   1. 基于对象,创建新对象

   2. 增加新的**属性和方法**

2. 寄生式继承和原型式原型式继承的区别是?
   1. 创建出来的新对象,会额外的增加新的**属性/方法**




### ES5-寄生组合式继承

**核心步骤:**

1. 通过构造函数来继承属性
2. 通过原型链来继承方法

```javascript
// 继承原型函数
function inheritPrototype(son, parent){
    //  基于父类的原型 创建新的对象
    const prototype = object.create(parent.prototype)
    // 保证原型三角的关系
    prototype.constructor = son
    // 设置给子类的类型
    son.prototype = prototype
}

// 父类构造函数
function Parent(name) {
  this.name = name
  this.foods = ['西蓝花', '西葫芦', '西红柿']     // 实例属性，写构造函数内
}

Parent.prototype.sayHi = function () {        // 公共的方法写在原型上
  console.log(this.name, `我喜欢吃,${this.foods}`)
}

// 子类借用父类的构造函数
function Son(name, age) {
    // 继承实例属性
  Parent.call(this, name)
  this.age = age
}

// 完成原型继承
inheritPrototype(Son,Parent)
// 可以继续在原型上添加属性/方法
Son.prototype.sayAge = function () {
  console.log('我的年龄是', this.age)
}

const son1 = new Son('jack', 18)
const son2 = new Son('rose', 16)
```



## JS继承-ES6

> [传送门:mdn类](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/class)
>
> [传送门:阮一峰ES6-class](https://wangdoc.com/es6/class)
>
> [传送门:mdn-super](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/super)

ES6中推出了`class`类,是用来创建对象的模板.`class`可以看作是一个语法糖,它的绝大部分功能，ES5 都可以做到，新的`class`写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已。



### class核心语法

**核心语法:**

1. 如何定义及使用[类](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes#%E7%B1%BB%E5%A3%B0%E6%98%8E):
2. 如何定义实例属性/方法:

```javascript
// 注意：class本质还是基于原型的
// 属性，在实例上
// 方法，在原型上

// 定义类
class Person {
  // 实例属性，方便一眼确认有哪些（直接写，并且可以设置值）
  // 可以不写，构造函数中可以通过 this 动态添加
  // 建议写上
  name
  food
  // 构造方法，类似于构造函数，new的时候会调用，内部的this就是实例化的对象
  constructor(name, food) {
    this.name = name
    this.food = food
  }

  // 实例方法
  sayHi() {
    console.log(`你好，我叫${this.name},我喜欢吃${this.food}`)
  }
}
 
const p = new Person('小黑', '西蓝花')
p.sayHi()
```





### class实现继承

**关键语法:**

1. **子类**通过[extends](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes/extends)继承**父类**
2. 子类构造函数中通过[super](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/super)调用父类构造函数

```javascript
// 在上一份代码的基础上继续编写下面代码
class Student extends Person {
  song
  constructor(name, food, song) {
    // 子类构造函数使用this以前必须最开始调用super调用父类的构造函数！！！
    super(name, food)
    this.song = song
  }
  // 添加方法
  sing() {
    console.log(`我叫${this.name},我喜欢唱${this.song}`)
  }
}
const s = new Student('李雷', '花菜', '孤勇者')
s.sayHi()
s.sing()
```



### class私有,静态属性和方法

**补充语法:**

1. [私有](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes/Private_class_fields)属性/方法的定义及使用(内部调用)
2. [静态](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes/static)属性/方法的定义及使用(类直接访问)

```javascript
class Person {
  constructor(name) {
    this.name = name
  }
  // 通过#作为前缀添加的属性会变为私有
  // 私有属性
  #secret = '我有一个小秘密，就不告诉你'
  // 私有方法
  #say() {
    // 私有属性可以在
    console.log('私有的say方法')
  }
  info() {
    // 在类的内部可以访问私有属性调用私有方法
    console.log(this.#secret)
    this.#say()
  }

  // 通过 static定义静态属性/方法
  // 访问的时候 通过 类 直接访问
  static staticMethod() {
    console.log('这是一个静态方法')
    console.log(this)
  }
  static info = '直立行走，双手双脚'
}

const p = new Person('jack')
console.log(p)
// 外部无法访问 点语法访问直接报错，通过[]无法动态获取
console.log(p['#secret'])
p.info()
// 通过类访问静态属性/方法
Person.staticMethod()
console.log(Person.info)
```





## fetch

> 这一节咱们来学习内置函数`fetch`
>
> [传送门-fetch](https://developer.mozilla.org/zh-CN/docs/Web/API/fetch)
>
> [传送门-Response](https://developer.mozilla.org/zh-CN/docs/Web/API/Response)
>
> [传送门-Headers](https://developer.mozilla.org/zh-CN/docs/Web/API/Headers)

全局的`fetch`函数用来发起获取资源请求.他返回一个`promise`,这个`promise`会在请求响应后被`resolve`,并传回Response对象



1. `fetch`核心语法

2. `fetch`结合`URLSearchParams`发送get请求:

   1. ```javascript
      const obj = {
          name:'jack',
          age:18
      }
      name=jack&age=17
      ```

3. `fetch`发送post请求,提交`JSON`数据

4. `fetch`发送post请求,提交`FormData`数据

### fetch核心语法

**核心语法:**

1. 如何[发请求](https://developer.mozilla.org/zh-CN/docs/Web/API/fetch):
2. 如何处理[响应](https://developer.mozilla.org/zh-CN/docs/Web/API/Response):
3. 注:[测试用接口](https://apifox.com/apidoc/project-1937884/api-49760223)

```javascript
document.querySelector('.request').addEventListener('click',() => {
    // 1. fetch(url地址) ===> Promise对象
    fetch('http://hmajax.itheima.net/api/news').then(response => {
        // console.log(response)
        // 2. 请求成功之后， resolve  --> then 获取 response
        // 3. 调用 json 方法，获取解析之后的结果，返回Promise
        response.json().then(res => {
            console.log(res)
        })
    })
})

// async await 改写
document.querySelector('.request').addEventListener('click', async() => {
   const response =  await fetch('http://hmajax.itheima.net/api/news')
   const res = response.json()
   console.log(res)
})
```





### fetch结合URLSearchParams发送get请求:

**需求:**

1. 使用`fetch`结合`URLSearchParams`调用地区查询[接口](https://apifox.com/apidoc/project-1937884/api-49760217)

```javascript
;(async function () {
  const params = new URLSearchParams({
    pname: '安徽省',
    cname: '合肥市'
  })
  // console.log(params) URLSearchParams { size: 2 } 2组键值对
  // 传入的对象，转为查询字符串
  // key=value&key=value
  // 中文会编码
  // params.toString() 
  
  const url = `http://hmajax.itheima.net/api/area?${params.toString()}`
  // fetch函数返回的是 Promise对象，通过await等待获取response对象
  const res = await fetch(url)
  // .json方法返回的是Promise对象 继续通过await等待
  const data = await res.json()
})()
```





### post请求-提交JSON

**需求:**

1. `fetch`发送post请求,提交`JSON`数据
2. [测试接口-用户注册](https://apifox.com/apidoc/project-1937884/api-49760218)

**核心步骤:**

1. 根据文档设置请求头
2. 通过配置项设置,请求方法,请求头,请求体

```javascript
    ; (async function () {
      // 通过headers设置请求头
      const headers = new Headers()
      // 通过 content-type指定请求体数据格式
      headers.append('content-type', 'application/json')
        
      // 参数1 url
      // 参数2 请求配置
      const res = await fetch('http://hmajax.itheima.net/api/register', {
        method: 'post',// 请求方法
        headers, // 请求头
        // 请求体
        body: JSON.stringify({ username: 'itheima9876', password: '123456' })
      })
      const json = await res.json()
      console.log(json)
    })()
```

post请求-提交JSON

1. `fetch`函数的第二个参数可以设置请求头,请求方法,请求体

   

### post请求-提交FormData

**需求:**

1. `fetch`发送post请求,提交`FormData`数据(上传+回显)
2. [测试接口-上传图片](https://apifox.com/apidoc/project-1937884/api-49760221)

**核心步骤:**

1. 通过`FormData`添加文件
2. 通过配置项设置,请求方法,请求体(`FormData`不需要设置请求头)

```javascript
  <input type="file" class="file" accept="image/*">
  <script>
    document.querySelector('.file').addEventListener('change', async function (e) {
      // 生成FormData对象并添加数据
      const data = new FormData()
      data.append('img', this.files[0])
      const res = await fetch('http://hmajax.itheima.net/api/uploadimg', {
        method: 'post',
        body: data
      })
      const json = await res.json()
      console.log(json)
    })
  </script>
```

#### 



## Generator

> [传送门-Generator](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Generator)

`Generator`对象由[生成器函数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/function*)返回并且它符合[可迭代协议](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Iteration_protocols#可迭代协议)和[迭代器协议](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Iteration_protocols#迭代器协议).他可以用来**控制流程**,语法行为和之前学习的函数不一样

### Generator-核心语法

**核心语法:**

1. 如何定义生成器函数:
2. 如何获取`generator`对象
3. `yield`表达式的使用
4. 通过`for of`获取每一个`yield`的值

```javascript
// 1. 通过function* 创建生成器函数 
function* foo() {
  // 生成器函数内部的逻辑，不会自动调用，调用 Generator 对象的 next() 方法  
  // 遇到yield表达式时会暂停后续的操作 （*对应async  yield对应await）
  yield 'a'
  yield 'b'
  yield 'c'
  return 'd'
}
// 2. 调用函数获取生成器
const f = foo()
// 3. 通过next方法获取yield 之后的表达式结果，会被包装到一个对象中
// 执行一次next 即可获取一次 yield之后的表达式结果
const res1 = f.next()
console.log(res1)// {value: 'a', done: false}
const res2 = f.next()
console.log(res2)// {value: 'b', done: false}
const res3 = f.next()
console.log(res3)// {value: 'c', done: false}
// 最后一次可以拿到return的结果
const res4 = f.next()
console.log(res4)// {value: 'd', done: true} 
// done 为true之后，获取到的value为undefined
const res5 = f.next()
console.log(res5)// {value: undefined, done: true} 

// 生成器，函数创建之后，代码不执行
// 每调用一次next执行到yield，获取结果
// 如果执行next之后无法获取结果， done: false


// 4. 通过for of 获取每一个yield之后的值，
// 迭代器协议 可以自定义 for of 的时候的行为
// iterator迭代（循环的每一项）
// f2 循环的内容
const f2 = foo()
for (const iterator of f2) {
  console.log(iterator)  // a b c d 获取每一个 yeild 之后的结果
}

// for of
// 可以用来遍历一些符合 迭代器协议的数据 比如数组

```





### Generator-id生成器

**需求:**使用`Generator`实现一个id生成器id

```javascript
function* idGenerator() {
    // 逻辑略
}
const idMaker = idGenerator()

// 调用next方法,获取id(每次累加1)
const { value: id1 } = idMaker.next()
console.log(id1)
const { value: id2 } = idMaker.next()
console.log(id2)
```

**核心步骤:**

1. 定义生成器函数
2. 内部使用循环,通过`yield`返回`id`并累加

```javascript
// 1. 通过function* 创建生成器函数 
function* generator() {
  let id = 0
  // 无限循环
  while (true) {
    // id累加并返回
    yield id++
  }
}
// 2. 调用函数获取生成器
const idMaker = generator()
// 3. 需要id的时候 通过next获取即可
const { value: id1 } = idMaker.next()
console.log(id1)
const { value: id2 } = idMaker.next()
console.log(id2)
```



### Generator-流程控制

遇到`yield`表达式时会**暂停**后续的操作

**需求:**使用`Generator`实现流程控制

```javascript
function* weatherGenerator() {
	// 逻辑略
    yield axios()
}
// 获取Generator实例
const weather = weatherGenerator()
// 依次获取 北上广深的天气 (axios)
weather.next()
```

**核心步骤:**

1. `yield`后面跟上天气查询逻辑
2. [接口文档-天气预报](https://apifox.com/apidoc/project-1937884/api-49760220)
3. 参考`code`:北京 110100  上海 310100  广州 440100 深圳 440300

```html
  <button class="getWeather">天气查询</button>
  <script src="https://cdn.bootcdn.net/ajax/libs/axios/1.3.6/axios.js"></script>
  <script>
    /**
     * 需求：流程控制，依次查询，北上广深的天气预报
     * 参考code: 北京 110100  上海 310100  广州 440100 深圳 440300
     * 接口文档: https://apifox.com/apidoc/project-1937884/api-49760220
     * */
    function* weatherGenerator() {
      // yield 会暂停代码的执行
      // 北京
      yield axios('http://hmajax.itheima.net/api/weather?city=110100')
      // 上海
      yield axios('http://hmajax.itheima.net/api/weather?city=310100')
      // 广州
      yield axios('http://hmajax.itheima.net/api/weather?city=440100')
      // 深圳
      yield axios('http://hmajax.itheima.net/api/weather?city=440300')
    }

    const cityWeather = weatherGenerator()
    
    //const response = weather.next()
    // 继续 .then
    //response.value.then(res => {
    //   console.log(res)
    //})
    
    document.querySelector('.getWeather').addEventListener('click', async () => {
      const res = await genCity.next()
      console.log(res)
    })
  </script>
```

