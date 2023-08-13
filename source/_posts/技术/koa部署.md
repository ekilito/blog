---
title: 在nodejs环境中应用并代理跨域
date: 2023-08-07 22:19:00
updated:             	 # 文章更新日期
tags: 技术                # 文章标签
categories: 技术             # 文章分类
keywords: koa,项目部署    # 文章关键字
description:             # 页面描述
cover: https://npm.elemecdn.com/akilar-candyassets/image/cover4.webp  # 文章缩略图

aside: true # 显示侧边栏 (默认 true)
toc_style_simple: # 显示 toc 简洁模式
highlight_shrink: # 配置代码框是否展开(true/false)
copyright_author: # 文章作者
swiper_index: 1
abbrlink: 882eae4
---

## 在nodejs环境中应用并代理跨域

**`目标`**将打包好的代码打包上线，并在nodejs中代理跨域

### 使用koa框架部署项目

> 到现在为止，我们已经完成了一个前端工程师的开发流程，按照常规的做法，此时，运维会将我们的代码部署到阿里云的ngix服务上，对于我们而言，我们可以将其部署到本机的nodejs环境中

部署 自动化部署 /手动部署

第一步，建立web服务文件夹  **`hrServer`**

```bash 
$ mkdir hrServer #建立hrServer文件夹 
```

第二步，在该文件夹下，初始化npm

```bash
$ npm init -y
```

第三步，安装服务端框架koa(也可以采用express或者egg)

```bash
$ npm i koa koa-static
```

第四步，拷贝上小节打包的dist目录到**`hrServer/public`**下

第五步，在根目录下创建app.js，代码如下

```js
const Koa  = require('koa')  // 引入Koa包
const serve = require('koa-static');

const app = new Koa();  // 实例化一个web服务
app.use(serve(__dirname + "/public")); //将public下的代码静态化
app.listen(3333, () => {
     console.log('人资项目启动')
})
```

> node app
>
> 此时，我们可以访问，http://localhost:3333

页面出来了

### 解决history页面404问题

但是，此时存在两个问题，

1. **当我们刷新页面，发现404**

>   这是因为我们采用了history的模式，地址的变化会引起服务器的刷新，我们只需要在app.js对所有的地址进行一下处理即可

安装 koa中间件 

```bash 
$ npm i koa2-connect-history-api-fallback #专门处理history模式的中间件
```

**注册中间件**

```js
const Koa  = require('koa')
const serve = require('koa-static');
const  { historyApiFallback } = require('koa2-connect-history-api-fallback');
const path = require('path')
const app = new Koa();
// 这句话 的意思是除接口之外所有的请求都发送给了 index.html
app.use(historyApiFallback({      //应该先使用 处理访问的中间件 再使用静态化服务
     whiteList: ['/prod-api']  //prod-api代理跨域的问题  表示不要帮我处理 /prod-api 由自己处理
 }));  // 这里的whiteList是 白名单的意思
app.use(serve(__dirname + "/public")); //将public下的代码静态化

app.listen(3333, () => {
      console.log('人资项目启动http://localhost:3333')
})
```

### 解决生产环境跨域问题

1. 当点击登录时，发现接口404

>   前面我们讲过，vue-cli的代理只存在于开发期，当我们上线到node环境或者ngix环境时，需要我们再次在环境中代理

在nodejs中代理

安装跨域代理中间件

```bash
$ npm i koa2-proxy-middleware
```

配置跨越代理

```js
const Koa = require('koa')
const serve = require('koa-static');
const { historyApiFallback } = require('koa2-connect-history-api-fallback');
const proxy = require('koa2-proxy-middleware')
const path = require('path')
const app = new Koa();

//注册跨域代理的中间件
app.use(proxy({
    targets: {
        // 代理哪个地址  代理以 '/prod-api'为开头的地址
        '/prod-api/(.*)': {
            target: 'https://heimahr.itheima.net/api', // 将以prod/api开头的内容代理到该地址  后端服务器地址
            changeOrigin: true,
            pathRewrite: {
                '/prod-api': ""
            }
        }
    }
}))

// 这句话 的意思是除接口之外所有的请求都发送给了 index.html
app.use(historyApiFallback({ //应该先使用 处理访问的中间件 再使用静态化服务
    whiteList: ['/prod-api'] // prod-api代理跨域的问题  表示不要帮我处理 /prod-api 由自己处理
})); // 这里的whiteList是 白名单的意思
app.use(serve(__dirname + "/public")); //将public下的代码静态化

app.listen(3333, () => {
    console.log('人资项目启动http://localhost:3333')
})
```

注意：这里之所以用了**pathRewrite**，是因为生产环境的请求基础地址是 **/prod-api**，需要将该地址去掉

此时，我们的项目就可以跨域访问了！

