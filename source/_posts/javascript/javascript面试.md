---
title: javascript面试题
data: 2023-8-05 12:25:00
description: 每天一道面试题。
tags: javascript
keywords: js面试题,前端
sticky: 1 # 数字越大，置顶优先
cover: https://npm.elemecdn.com/akilar-candyassets/image/cover5.webp
copyright: true
toc_number: false
abbrlink: 3927
# swiper_index: 1 #   置顶轮播图顺序，非负整数，数字越大越靠前
categories: javascript 
aside: false # 显示侧边栏 (默认 true)
swiper_index: 2
businesscard: true # 个性名片
---

## 1. 谈谈你对闭包的理解？

这个问题想考察的主要有两个方面：

- 对闭包的基本概念的理解
- 对闭包的作用的了解

**什么是闭包？**

MDN的官方解释：

> `闭包是函数和声明该函数的词法环境的组合`

更通俗一点的解释是：

> `内层函数, 引用外层函数上的变量, 就可以形成闭包`

需求: 定义一个计数器方法, 每次执行一次函数, 就调用一次进行计数

```js
let count = 0      // 全局变量，太容易被别人修改了，希望有些数据是私有的，不让外部随意的访问
function fn () {
  count++
  console.log('fn函数被调用了' + count + '次')
}
fn()
```

这样不好! count 定义成了全局变量, 太容易被别人修改了,  我们可以利用闭包解决

闭包实例:

```jsx
function fn () {
  let count = 0   // 局部变量，外部访问不到

  function add () {
    count++       //  引用外层函数上的变量，形成闭包
    console.log('fn函数被调用了' + count + '次')
  }

  return add   // 注意：需要 return 引用（内存才不会被释放）
}
const addFn = fn()
addFn()
addFn()
addFn()
```

标记清除：从根部，全局出发，访问不到（无法触及）的内存空间，就会被自动回收

`addFn = null`    => 释放内存，断开了对于之前内部函数的引用，对应的缓存变量内容也会被释放掉



**闭包的主要作用是什么？**

`实现数据的私有`

在实际开发中，闭包最大的作用就是用来 **`变量私有`**。

下面再来看一个简单示例：

```js
function Person() {
  // 以 let 声明一个局部变量，而不是 this.name
  // this.name = 'zs'     =>  p.name
  let name = 'hm_programmer' // 数据私有！！！！！
  
  this.getName = function(){ 
    return name
  }
  
  this.setName = function(value){ 
    name = value
  }
}

// new:
// 1. 创建一个新的对象
// 2. 让构造函数的this指向这个新对象
// 3. 执行构造函数
// 4. 返回实例
const p = new Person()
console.log(p.getName()) // hm_programmer

p.setName('Tom')
console.log(p.getName()) // Tom

p.name // 访问不到 name 变量：undefined！！！！！！
```

在此示例中，变量 `name` 只能通过 Person 的实例方法进行访问，外部不能直接通过实例进行访问，形成了一个私有变量。

```js
// 早期，闭包还用于解决for循环中，定时打印内容的问题
for(var i = 1 ; i <= 5 ; i++) {
   (function(num) {
       // 形参也可以理解为函数中的局部变量
       setTimeout(()=> {
      console.log(num)
   }, i * 1000)
   })(i)
}
```





