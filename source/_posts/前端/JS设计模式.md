---
title: 设计模式
data: 2023-8-20 16:57:00
description: 
tags: [设计模式]
keywords: 
sticky:  # 数字越大，置顶优先
cover: https://bu.dusays.com/2023/08/13/64d7b65aa1a6e.webp
copyright: true
toc_number: false
# swiper_index: 1 #   置顶轮播图顺序，非负整数，数字越大越靠前
categories: 前端
aside: true # 显示侧边栏 (默认 true)
swiper_index: 

---


## JS设计模式

> [传送门:wiki-设计模式](https://zh.wikipedia.org/wiki/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%EF%BC%9A%E5%8F%AF%E5%A4%8D%E7%94%A8%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E8%BD%AF%E4%BB%B6%E7%9A%84%E5%9F%BA%E7%A1%80)
>
> [传送门:JavaScript设计模式与开发实践](https://www.ituring.com.cn/book/1632)

设计模式的指的是：在**面向对象软件**设计过程中针对特定问题的简洁而优雅的解决方案。通俗一点说，设计模式就是给面向对象软件开发中的一些好的设计取个名字。

目前说到设计模式，一般指的是《设计模式：可复用面向对象软件的基础》一书中提到的**23种**常见的软件开发设计模式。

JavaScript中不需要生搬硬套这些模式，咱们结合实际前端开发中的具体应用场景，来看看有哪些常用的设计模式

这一节咱们会学习:

1. JS中的常用设计模式
2. 设计模式在开发/框架中的应用场景

### 工厂模式

在JavaScript中，工厂模式的表现形式就是一个**直接调用即可返回新对象的函数**

```javascript
// 定义构造函数并实例化
function Dog(name){
    this.name=name
}
const dog = new Dog('柯基')

// 工厂模式
function ToyFactory(name,price){
    return {
        name,
        price
    }
}
const toy1 = ToyFactory('布娃娃',10)
const toy2 = ToyFactory('玩具车',15)
```

**应用场景**

1. Vue2->Vue3: 

   1. 启用了`new Vue`,改成了工厂函数`createApp`-[传送门](https://v3-migration.vuejs.org/zh/breaking-changes/global-api.html)
   2. ***任何全局改变 Vue 行为的 API(vue2) 现在都会移动到应用实例上(vue3)***
   3. 就不会出现,Vue2中多个Vue实例共享,相同的全局设置,可以**实现隔离**
   4. 避免vue2中全局设置的东西，比如组件，影响后续实例

   ```html
   <!DOCTYPE html>
   <html lang="zh-CN">
   
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>Document</title>
     <style>
       #app1,
       #app2 {
         border: 1px solid #000;
       }
     </style>
   </head>
   
   <body>
     <h2>vue2-全局注册组件</h2>
     <div id="app1">
       实例1 组件
       <my-title></my-title>
     </div>
     <div id="app2">
       实例2
       <my-title></my-title>
     </div>
     <script src="https://cdn.bootcdn.net/ajax/libs/vue/2.7.9/vue.js"></script>
     <script>
         // 全局注册组件
       Vue.component('my-title', {
           // 组件的结构
         template: '<h2 style="color:orange">标题组件</h2>'
       })
       const app1 = new Vue({
         el: "#app1"
       })
   
       const app2 = new Vue({
         el: "#app2"
       })
   
     </script>
   </body>
   
   </html>
   ```

2. axios.create:

   1. 基于传入的配置创建一个新的`axios`实例,[传送门](https://www.axios-http.cn/docs/instance)
   2. 项目中有2个请求基地址如何设置?
   3. 创建出多个请求不同的对象，比如设置多个基地址

```javascript
// 1. 基于不同基地址创建多个 请求对象
const request1 = axios.create({
  baseURL: "基地址1"
})
const request2 = axios.create({
  baseURL: "基地址2"
})
const request3 = axios.create({
  baseURL: "基地址3"
})

// 2. 通过对应的请求对象,调用接口即可
request1({
  url: '基地址1的接口'
})
request2({
  url: '基地址2的接口'
})
request3({
  url: '基地址3的接口'
})
```

小结:

1. 工厂模式:JS中的表现形式,**返回新对象的函数(方法)**

```javascript
      function sayHi(){} // 函数
      const obj ={
          name:'jack',
          sayHello(){} // 方法
      }
```

2. 日常开发中,有2个很经典的场景

   1. `vue3`中创建实例的api改为`createApp`,`vue2`中是`new Vue`
      1. Vue3中,没有影响所有Vue实例的api了,全都变成了影响某个app对象的api,比如`Vue.component-->app.component`
   2. `axios.create`基于传入的配置,创建一个新的请求对象,可以用来设置多个基地址



### 单例模式

**单例模式指的是，在使用这个模式时，单例对象整个系统需要保证只有一个存在。**

**需求:**

1. 通过静态方法`getInstance`获取**唯一实例**

```javascript
const s1 = SingleTon.getInstance()
const s2 = SingleTon.getInstance()
console.log(s1===s2)//true
```

**核心步骤:**

1. 定义类
2. 私有静态属性:`#instance`
3. 提供静态方法`getInstance`:
   1. 调用时判断`#instance`是否存在:
   2. 存在:直接返回
   3. 不存在:实例化,保存,并返回

```javascript
class SingleTon {
   constructor() { }
   // 私有属性，保存唯一实例
   static #instance

  // 获取单例的方法
  static getInstance() {
      // console.log(this) // SingleTon类
    if (SingleTon.#instance === undefined) {
      // 内部可以调用构造函数
      SingleTon.#instance = new SingleTon()
    }
    return SingleTon.#instance
  }
}

const s1 = SingleTon.getInstance()
const s2 = SingleTon.getInstance()
console.log(s1===s2)//true
```

**实际应用:**

1. vant组件库中的弹框组件,保证弹框是单例
   1. toast组件:[传送门](https://gitee.com/vant-contrib/vant/blob/main/packages/vant/src/toast/index.ts)
   2. notify组件:[传送门](https://gitee.com/vant-contrib/vant/blob/main/packages/vant/src/notify/index.ts)
   3. 如果弹框对象
      1. 不存在,-->创建一个新的
      2. 存在,直接用
2. vue中注册插件,用到了单例的思想(只能注册一次)
   1. vue2:[传送门](https://gitee.com/vuejs/vue/blob/main/src/core/global-api/use.ts)
   2. vue3:[传送门](https://gitee.com/vuejs/core/blob/main/packages/runtime-core/src/apiCreateApp.ts#L256)

**小结:**

1. 单例模式:

   1. 保证,应用程序中,某个对象,只能有一个

2. 自己实现:

   1. getInstance方法,
      1. 实例存在->返回
      2. 实例不存在->创建,保存->返回

3. 应用场景:

   1. 我在看源码的时候,发现,vant的toast和notify组件都用到了单例
      1. 多次弹框,不会创建多个弹框,复用唯一的弹框对象
   2. vue中注册插件,vue3和vue2都会判断插件是否已经注册,已注册,直接提示用户



### 观察者模式

在对象之间定义一个**一对多**的依赖，当一个对象状态改变的时候，所有依赖的对象都会自动收到通知。

**举个例子:**

1. `dom`事件绑定，比如

```javascript
window.addEventListener('load', () => {
  console.log('load触发1')
})
window.addEventListener('load', () => {
  console.log('load触发2')
})
window.addEventListener('load', () => {
  console.log('load触发3')
})
```

2. Vue的生命周期钩子:

   1. vue框架,提供给开发者,在Vue实例特定时期,添加自定义逻辑的,一种机制.
   2. 一共有：
      1. beforeCreated 
      2. created
      3.  beforeMount 
      4. Mounted 
      5. beforeUpdate 
      6. Updated 
      7. beforeDestory 
      8. destoryed
      9. 缓存组件（keep-alive）：activated  deactivated 

3. Vue的响应式原理:

   1. [传送门](https://v2.cn.vuejs.org/v2/guide/reactivity.html)

   2. **自己描述：响应式原理**

      1.  创建Vue实例时，会通过Object.definedProperty将data中的数据的每个属性都转为get和set

      2. 就可以监测到对数据的，取值get和赋值set

      3. 只要数据发生了变更，就会去通知所有使用数据为止，更新

         1. 页面
         2. 侦听器
         3. ...

         ![vue响应式原理.png](https://bu.dusays.com/2023/08/19/64e0ac0f284d5.png)

   3. 自己描述2-涉及到虚拟dom：

      1. 可能被追问：
         1. 为什么需要使用**虚拟dom**？ 虚拟dom内存中，速度快
         2. 新旧dom比较，如何比较的？ 
            1. **diff算法** 找不同
            2. vue中同级比较
               1. 有id，id不同，直接不同
               2. 没有id，比元素，在比属性
         3. vue的diff算法和react的diff算法有什么区别？

      ![vue响应式原理2.png](https://bu.dusays.com/2023/08/19/64e0ac9e5e0c8.png)

   4. 自己回答：

      1. vue2中使用的是 Object.definedProperty，动态新增的属性，没有响应式，this.$set
      2. vue3中是Proxy
         1. 没有这个问题，Proxy可以检测到所有属性的改变
         2. vue3中只用了 Proxy 吗？不是，引用了`Object.definedProperty`





### 发布订阅模式01-应用场景

发布订阅模式可以实现的效果类似观察者模式,但是两者略有差异,一句话描述:一个有中间商(**发布订阅模式**)一个没中间商(**观察者模式**)

![image-20230626153656768.png](https://bu.dusays.com/2023/08/19/64e0ca7198385.png)

![image-20230706002933258.png](https://bu.dusays.com/2023/08/19/64e0ca7198385.png)

**应用场景:**

1. `vue2`中的`EventBus`:[传送门](https://v2.cn.vuejs.org/v2/api/#%E5%AE%9E%E4%BE%8B%E6%96%B9%E6%B3%95-%E4%BA%8B%E4%BB%B6)
2. `vue3`中因为移除了实例上对应方法，可以使用替代方案:[传送门](https://v3-migration.vuejs.org/zh/breaking-changes/events-api.html)
   1. 官方推荐,用插件
   2. 微微一笑:直接写


### 发布订阅模式02-自己写一个事件总线

**需求:**

```javascript
const bus = new HMEmitter()
// 注册事件
bus.$on('事件名1',回调函数)
bus.$on('事件名1',回调函数)

// 触发事件
bus.$emit('事件名',参数1,...,参数n)

// 移除事件
bus.$off('事件名')

// 一次性事件
bus.$once('事件名',回调函数)
```

**核心步骤:**

1. 定义类
2. 私有属性:`#handlers={事件1:[f1,f2],事件2:[f3,f4]}`
3. 实例方法:
   1. $on(事件名,回调函数):注册事件
   2. $emit(事件名,参数列表):触发事件
   3. $off(事件名):移除事件
   4. $once(事件名,回调函数):注册一次性事件

**基础模板:**

```html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>
  <h2>自己实现事件总线</h2>
  <button class="on">注册事件</button>
  <button class="emit">触发事件</button>
  <button class="off">移除事件</button>
  <button class="once-on">一次性事件注册</button>
  <button class="once-emit">一次性事件触发</button>
  <script>
    class HMEmmiter {
    	// 逻辑略
    }

    // 简化 querySelector调用
    function qs(selector) {
      return document.querySelector(selector)
    }
    // 注册事件
    qs('.on').addEventListener('click', () => {
    
    })
    // 触发事件
    qs('.emit').addEventListener('click', () => {
   
    })
    // 移除事件
    qs('.off').addEventListener('click', () => {
      
    })
    // 一次性事件注册
    qs('.once-on').addEventListener('click', () => {
     
    })
    // 一次性事件触发
    qs('.once-emit').addEventListener('click', () => {
     
    })
  </script>
</body>

</html>
```



```javascript
class HMEmmiter {
  #handlers = {}
  // 注册事件
  $on(event, callback) {
    if (!this.#handlers[event]) {
      this.#handlers[event] = []
    }
    // 注册继续push
    this.#handlers[event].push(callback)
  }
  // 触发事件
  $emit(event, ...args) {
      // 取出保存的时间 []
    const funcs = this.#handlers[event] || []
      // 挨个触发，并传入参数
    funcs.forEach(func => {
      func(...args)
    })
  }
  // 移除事件
  $off(event) {
      // event 对应的回调函数数组设置空即可
    this.#handlers[event] = undefined
  }
  // 一次性事件:注册了以后，只能触发一次
  $once(event, callback) {
      // 触发之后，清空，移除
    this.$on(event, (...args) => {
        // 执行 callback
      callback(...args)
        // 移除注册的event事件
      this.$off(event)
    })
  }
}
```



### 原型模式

在原型模式下，当我们想要创建一个对象时，会先找到一个对象作为原型，然后通过**克隆原型**的方式来创建出一个与原型一样（共享一套数据/方法）的对象。在`JavaScript`中,`Object.create`就是实现原型模式的内置`api`

**应用场景:**

`vue2`中重写数组方法:

1. 调用方法时(`push`,`pop`,`shift`,`unshift`,`splice`,`sort`,`reverse`)可以触发视图更新:[传送门](https://v2.cn.vuejs.org/v2/guide/list.html#%E5%8F%98%E6%9B%B4%E6%96%B9%E6%B3%95)
2. 源代码:[传送门](https://gitee.com/vuejs/vue/blob/main/src/core/observer/array.ts)`
3. 测试一下:

```html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>
  <h2>原型模式</h2>
  <div id="app"></div>
  <script src="https://cdn.bootcdn.net/ajax/libs/vue/2.7.9/vue.js"></script>
  <script>
    const app = new Vue({
      el: "#app", data: {
        foods: ['西瓜', '西葫芦', '西红柿']
      }
    })
    console.log(app.foods.push === Array.prototype.push)
  </script>

</body>

</html>
```

![原型模式.png](https://bu.dusays.com/2023/08/19/64e0bad8f3abf.png)

**自己描述:**

1. vue2中数组重写了7个方法,内部基于数组的原型`Array.prototype`创建了一个新对象
2. `Object.create`浅拷贝
3. 内部
   1. 调用数组的原方法,获取结果并返回---方法的功能和之前一致
   2. 通知了所有的观察者去更新视图

```javascript
const app = new Vue({
    el:"#app",
    data:{
        arr:[1,2,3]
    }
})
app.arr.push === Array.prototype.push //false
```

4. 原型模式,基于某个对象,创建一个新的对象,JS中,通过Object.create即可实现,Vue中重写数组方法就是这么做的 ↑



### 代理模式

代理模式指的是**拦截和控制**与目标对象的交互,在`JavaScript`中通过`Proxy`,即可实现对象的代理,[传送门](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy)



**核心语法:**

1. 初始对象可以直接修改任意属性
2. 通过`Proxy`生成代理对象，限制访问

```javascript
// 目前obj对象的name和age属性可以被随意修改
const obj = {
  name: 'jack',
}
// 通过Proxy创建代理对象 
// 最大的特点就是，无论操作的属性，在对象上是否存在 都会触发 get set
const objProxy = new Proxy(obj, {
  // []语法进行对象的取值和赋值
    // target: 源对象
    // key： 属性名
  get(target, key) {
    console.log('get触发')
    // return target[key]
  },
  // 结合Reflect的静态方法替换[]语法
    // target: 源对象
    // key： 属性名
    // value：设置的值
  set(target, key, value) {
     console.log('set触发')
   	// target[key]=value
  }
})
// 代理对象属性赋值，触发set
objProxy.name = 'rose'
// 代理对象属性取值，触发get
console.log(objProxy.name)
```

**需求:**

基于上一份代码实现:

1. 属性取值和赋值时,如果属性不存在,报错
2. 修改name时,只能设置字符串,否则报错

**关键步骤:**

1. 在`get`中添加取值判断逻辑
2. 在`set`中添加赋值判断逻辑

```javascript
// 目前obj对象的name和age属性可以被随意修改
const obj = {
  name: 'jack',
  age: 18
}
// 通过Proxy创建代理对象 
const objProxy = new Proxy(obj, {
  // []语法进行对象的取值和赋值
  get(target, key) {
    if (!target[key]) {
      throw new Error('属性不存在')
    }
    return target[key]
  },
  set(target, key, value) {
    if (!target[key]){
      throw new Error('属性不存在')
    }
    if (key === 'name') {
      // 判断类型
      if (typeof value === 'string') {
        target[key]=value
      } else {
        throw new Error('name属性只能设置字符串')
      }
    }
  }
  // 
})
// 代理对象属性赋值，触发set
objProxy.name = 'rose'
// 不存在friend 报错
objProxy.friend = 'rose'
```

**实际应用:**

`Vue3`的响应式原理-[传送门](https://cn.vuejs.org/guide/extras/reactivity-in-depth.html#how-reactivity-works-in-vue)

1. 通过`Proxy`创建响应式对象
2. `getter/setter`用于`ref`
3. Vue2考虑兼容,用的是兼容性好的`Object.defineProperty`,但是无法跟踪动态增加的属性
4. `Vue3`中用了`Proxy`,他对于动态增加的属性,也可以检测到,但是Vue3中也用了`Object.defineProperty`
   1. `reactive`用的是`Proxy`
      1. 注意点:解构之后会丢失响应性,需要用`toRefs`

   2. `ref`用的是`Object.defineProperty`

5. 观察者模式-->虚拟dom->diff算法





### 迭代器模式

迭代器模式提供一种方法顺序访问一个聚合对象中的各个元素，而又不暴露该对象的内部表示.简而言之就是:**遍历**

遍历作为日常开发中的**高频**操作,JavaScript中有大量的默认实现:**比如**

1. `Array.prototype.forEach`:遍历数组
2. `NodeList.prototype.forEach`:遍历`dom`,`document.querySelectorAll`
3. `for in`
4. `for of`



**面试题**:

1. `for in` 和`for of` 的区别?

   1. **`for...in`** **语句**以任意顺序迭代一个对象的除[Symbol](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol)以外的[可枚举](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Enumerability_and_ownership_of_properties)属性，包括继承的可枚举属性。

      1. 对象默认的属性以及动态增加的属性都是**可枚举**属性
      2. 遍历出来的是**属性名**
      3. 继承而来的属性也会遍历

      ```javascript
          <script>
              // 原型上默认的属性和方法,都是不可枚举(for in不出来)
              // 动态添加的,默认是可枚举(可以for in出来)
              Object.prototype.run = function() {
                  console.log('奔跑')
              }
              Array.prototype.swim = '游泳'
      
      
              const arr = ['小鸡', '小鸭', '小鱼']
                  // 遍历的是key,继承而来的属性也可以遍历出来
              for (const key in arr) {
                  console.log('key:', key) // key: 0  key: 1  key: 2  key: swim  key: run  
              }
      
              // 遍历的值,继承而来的遍历不出来
              for (const iterator of arr) {
                  console.log('iterator:', iterator) // key: 小鸡 key: 小鸭 key: 小鱼
              }
          </script>
      ```

      

   2. **`for...of`语句**在[可迭代对象](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Iteration_protocols)（包括 [`Array`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array)，[`Map`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Map)，[`Set`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Set)，[`String`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String)，[`TypedArray`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/TypedArray)，[arguments](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/arguments) 对象等等）上创建一个迭代循环

      1. for of不会遍历**继承**而来的属性
      2. 遍历出来的是**属性值**

```javascript
    const arr = ['小鸡', '小鸭', '小鱼']

    // 让 obj可以 for of for of出来的是他内部的 数组
    // 自定义 forof执行的行为
    const obj = {
      name: 'jack',
      friend: 'rose',
      skill: 'jump together',
      foods: ['沙县', '猪脚饭', '手撕鸡'],
      // 属性名表达式
      [Symbol.iterator]() {
        let index = 0
        // console.log(this)
        // 返回一个对象
        return {
          next: () => {
            if (index < 3) {
              return {
                done: false,
                value: this.foods[index++]
              }
            } else {
              return {
                done: true
              }
            }
          }
        }
      }
    }
    // 遍历的值,继承而来的遍历不出来
    // for (const iterator of arr) {
    //   console.log('iterator:', iterator)
    // }

    // for of 并不能遍历所有的东西,比如 object无法遍历
    // 直接遍历对象: obj is not iterable,obj不可迭代
    // [Symbol.iterator] 添加之后,可以迭代,要求返回特定格式的对象
    for (const iterator of obj) {
      console.log('iterator:', iterator)
    }

```



**可迭代协议和迭代器协议:**

1. 可迭代协议:[传送门](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Iteration_protocols#%E5%8F%AF%E8%BF%AD%E4%BB%A3%E5%8D%8F%E8%AE%AE)
   1. 给对象增加属方法` [Symbol.iterator](){}`
   2. 返回一个符合迭代器协议的对象

2. 迭代器协议:[传送门](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Iteration_protocols#%E8%BF%AD%E4%BB%A3%E5%99%A8%E5%8D%8F%E8%AE%AE)
   1. next方法,返回对象:
      1. `{done:true}`,迭代结束
      2. `{done:false,value:'xx'}`,获取解析并接续迭代


3. 面试问及:
   1. for of可以遍历一部分的类型,比如数组,map
   2. 对象无法遍历,因为对象没有实现 可迭代协议,迭代器协议
   3. 可迭代协议,迭代器协议,约定了:
      1. 可迭代协议:对象上要有一个指定属性的函数,返回 满足迭代器要求的对象
      2. 迭代器协议: `next`方法,返回`{done:true},{done:false,value:'x'}`
      3. 我自己尝试写过一下,但是仅针对语法
      4. 可以和面试官讨论一下,可以用在哪?

4. 直接打印对象,看到**Symbol(Symbol.iterator)**,说明可以使用`for of`





## JS调用栈

1. 执行上下文和调用栈
2. 栈溢出



### 执行上下文和调用栈

[执行上下文](https://262.ecma-international.org/6.0/):是指在代码执行时，JavaScript引擎创建的一种数据结构，它包含了函数执行时的状态信息，例如变量、函数参数、函数返回值等。

在以下三种情况下会创建执行上下文

1. JavaScript执行全局代码时，创建**全局执行上下文**

2. 调用函数时，创建**函数执行上下文**

3. 使用 eval 函数时，创建**执行上下文**
   1. 给他一个字符串,解析为js并执行

我们通过调试工具确认一下

```javascript
const num = 0
function funA(a, b) {
  return a + b
}
function funcB(c) {
  const res = funA(1, 2)
  return res + c
}
num = funcB(3)
// 执行函数时，创建对应执行上下文，内部保存变量，代码等一系列执行函数需要的东西
// 进入JS调用堆栈，执行 ==> 执行完毕之后 => 出栈
// 所有代码执行完毕为止
```

**调用栈:**

1. 执行上下文会存在JS调用栈中,栈的结构特点是:**先进后出**





![image-20230706133635838.png](https://bu.dusays.com/2023/08/19/64e0ca6fa8242.png)



### 栈溢出

栈的容量是有限的,如果内部的内容一直得不到释放,就会出现栈溢出,比如

```javascript
// 栈溢出，JS调用栈有容量大小，太大了，会溢出
// JS调用堆栈装满了之后，就会出现
// Maximum call stack size exceeded
// 日常开发常见的：
// 1. 死递归
// 2. 导航守卫

function sum() {
  let i = 0;
  i++
  sum()
}

sum()

```

![image-20230708104801552.png](https://bu.dusays.com/2023/08/19/64e0ca7057592.png)







## 总结:

![设计模式1.png](https://bu.dusays.com/2023/08/19/64e0a7aa72c29.png)
![设计模式2.png](https://bu.dusays.com/2023/08/19/64e0de1ab6975.png)
![设计模式3.png](https://bu.dusays.com/2023/08/19/64e0de18eb2b5.png)

## 参考资料

1. [阮一峰-《ECMAScript 6 教程》](https://wangdoc.com/es6/)
2. [图灵社区-JavaScript高级程序设计](https://www.ituring.com.cn/book/2472)


   