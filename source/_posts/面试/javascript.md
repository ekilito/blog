---
title: JS面试
data: 2023-8-05 12:25:00
description: 每天一道面试题。
tags: [js面试题,闭包]
keywords: js面试题,前端
sticky:  # 数字越大，置顶优先
cover: https://bu.dusays.com/2023/08/13/64d7b653376ff.webp
copyright: true
toc_number: false
abbrlink: 3927
# swiper_index: 1 #   置顶轮播图顺序，非负整数，数字越大越靠前
categories: 面试 
aside: true # 显示侧边栏 (默认 true)
swiper_index: 2

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

```javascript
// 以下代码有没有闭包
function outer() {
    let age = 18
    function inner() {
        console.log(age)
    }
    inner()  // 闭包 和return 无关 一般用外部函数命名  内部函数访问了外部函数作用域里的变量
}            // 一般会 return 出去。但是闭包 !== return
outer()
```
1. 

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





## 2. async函数阻塞

   1. async函数内部可以使用await 等待promise执行完毕,内部是阻寒的

   2. 但是async函数不会阻塞同级代码的执行,除非用async西数再包一层

   3. 比如,我之前在做导航守卫判断的时候,有一个逻辑

      1. action获取用户信息
      2. 基于用户信息判断是否登录
      3. 最开始没有写 {% label await red %} ,导致第一次判断不通过,第二次才可






