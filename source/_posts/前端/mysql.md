---
title: mysql
data: 2023-8-20 15:10:00
description: 
tags: mysql
keywords: 
sticky:  # 数字越大，置顶优先
cover: https://bu.dusays.com/2023/08/13/64d7b65aa1a6e.webp
copyright: true
toc_number: false
abbrlink: 882ea85
# swiper_index: 1 #   置顶轮播图顺序，非负整数，数字越大越靠前
categories: 前端
aside: true # 显示侧边栏 (默认 true)
swiper_index: 

---


# MYSQL

### **1. 数据库的基本概念**

#### **1.1 什么是数据库**

数据库（database）是用来**组织**、**存储**和**管理**数据的仓库。

当今世界是一个充满着数据的互联网世界，充斥着大量的数据。数据的来源有很多，比如出行记录、消费记录、

浏览的网页、发送的消息等等。除了文本类型的数据，图像、音乐、声音都是数据。

为了方便管理互联网世界中的数据，就有了数据库管理系统的概念（简称：数据库）。用户可以对数据库中的数

据进行**新增、查询、更新、删除**等操作。



#### **1.2 常见的数据库及分类**

市面上的数据库有很多种，最常见的数据库有如下几个：

⚫ MySQL 数据库（目前使用最广泛、流行度最高的开源免费数据库；Community + Enterprise）

⚫ Oracle 数据库（收费）

⚫ SQL Server 数据库（收费）

⚫ Mongodb 数据库（Community + Enterprise）

其中，MySQL、Oracle、SQL Server 属于**传统型数据库**（又叫做：关系型数据库 或 SQL 数据库），这三者的

设计理念相同，用法比较类似。

而 **Mongodb** 属于**新型数据库**（又叫做：非关系型数据库 或 NoSQL 数据库），它在一定程度上弥补了传统型

数据库的缺陷。



#### **1.3 传统型数据库的数据组织结构**

数据的组织结构：指的就是数据以什么样的结构进行存储。

传统型数据库的数据组织结构，与 Excel 中数据的组织结构比较类似。

因此，我们可以对比着 Excel 来了解和学习传统型数据库的数据组织结构。



###### **1. Excel 的数据组织结构**

![](images\excel1.png)



###### **2. 传统型数据库的数据组织结构**

![](images\excel2.png)



###### **3. 实际开发中库、表、行、字段的关系**

① 在实际项目开发中，一般情况下，每个项目都对应独立的数据库。

② 不同的数据，要存储到数据库的不同表中，例如：用户数据存储到 users 表中，图书数据存储到 books 表中。

③ 每个表中具体存储哪些信息，由字段来决定，例如：我们可以为 users 表设计 id、username、password 这 3 个字段。

④ 表中的行，代表每一条具体的数据。



###### **4. 安装并配置 MySQL**

对于开发人员来说，只需要安装 MySQL Server 和 MySQL Workbench 这两个软件，就能满足开发的需要了。

⚫ **MySQL Server**：专门用来提供数据存储和服务的软件。

⚫ **MySQL Workbench**：可视化的 MySQL 管理工具，通过它，可以方便的操作存储在 MySQL Server 中的数据。





### **2. MySQL 的基本使用**



#### **2.1 使用 MySQL Workbench 管理数据库**

###### **1.** **连接数据库**

![](images\mysql1.png)



###### **2. 了解主界面的组成部分**

![](images\mysql2.png)



###### **3. 创建数据库**

![](images\mysql3.png)



![](images\mysql5.png)





###### **4. 创建数据表**

![](images\mysql6.png)



###### **5. 向表中写入数据**

![](images\mysql7.png)







####  **2.2 使用 SQL 管理数据库**

**1. 什么是 SQL**

SQL（英文全称：Structured Query Language）是结构化查询语言，专门用来访问和处理数据库的编程语言。能够让我们**以编程的形式**，**操作数据库里面的数据**。

① SQL 是一门**数据库编程语言**

② 使用 SQL 语言编写出来的代码，叫做 **SQL 语句**

③ SQL 语言**只能在关系型数据库中使用**（例如 MySQL、Oracle、SQL Server）。非关系型数据库（例如 Mongodb）                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       不支持 SQL 语言



**2. SQL 能做什么**

① 从数据库中**查询数据**

② 向数据库中**插入新的数据**

③ **更新**数据库中的**数据**

④ 从数据库**删除数据**

⑤ 可以创建新数据库

⑥ 可在数据库中创建新表

⑦ 可在数据库中创建存储过程、视图

⑧ etc…



**3. SQL 的学习目标**

重点掌握如何使用 SQL 从数据表中：

**查询数据（select） 、插入数据（insert into） 、更新数据（update） 、删除数据（delete）**

额外需要掌握的 4 种 SQL 语法：

**where 条件、and 和 or 运算符、order by 排序、count(*) 函数**





#### **2.3 SELECT **

![](images\select1.png)

![](images\select2.png)

![](images\select3.png)

#### **2.4 INSERT INTO**

![](images\insert1.png)

![](images\insert2.png)



#### **2.5 UPDATE**

![](images\update.png)

![](images\update2.png)

![](images\update3.png)





#### **2.6 DELETE**

![](images\delete.png)

![](images\delete1.png)



#### **2.7 WHERE**



![](images\where1.png)

![](images\where2.png)

![](images\where3.png)



#### **2.8 AND** **和** **OR**

![](images\and1.png)

![](images\and2.png)

![](images\and3.png)





#### **2.9 ORDER BY**

![](images\order1.png)

![](images\order2.png)

![](images\order3.png)

![](images\order4.png)





#### **2.10 COUNT(\*)** 

![](images\count1.png)

![](images\count2.png)



![](images\count3.png)





### **3. 在项目中操作 MySQL**



#### **3.1 在项目中操作数据库的步骤**

① 安装操作 MySQL 数据库的第三方模块（**mysql**）

② 通过 mysql 模块**连接到 MySQL 数据库**

③ 通过 mysql 模块**执行 SQL 语句**

![](images\lian1.png)



#### **3.2 安装与配置 mysql 模块**

###### **1.** **安装** **mysql 模块**

**mysql** 模块是托管于 npm 上的**第三方模块**。它提供了在 Node.js 项目中**连接**和**操作** **MySQL** 数据库的能力。

想要在项目中使用它，需要先运行如下命令，将 mysql 安装为项目的依赖包：

```js
npm install mysql
```



###### **2.** **配置** **mysql 模块**

在使用 mysql 模块操作 MySQL 数据库之前，**必须先对 mysql 模块进行必要的配置**，主要的配置步骤如下：

```js
// 1. 导入 mysql 模块
const mysql = require('mysql')

// 2. 建立与 mysql 数据的链接
const db = mysql.createPool({
    host: '127.0.0.1',      // 数据库的 IP 地址
    user: 'root',           // 登录数据库的账号
    password: 'admin123',   // 登录数据库的密码
    database: 'my_db_01'    // 指定要操作那个数据库
})
```



###### **3. 测试 mysql 模块能否正常工作**

调用 **db.query()** 函数，指定要执行的 SQL 语句，通过回调函数拿到执行的结果：

```js
// 检测 mysql 模块能否正常工作
db.query('select 1' , (err , results) => {
    if(err) return console.log(err.message)
    
    // 只要能打印出 [ RowDataPacket { '1' : 1 } ] 的结果，就证明数据库连接正常
    console.log(results)
})
```



####  **3.3 使用 mysql 模块操作 MySQL 数据库**



###### **1. 查询数据**

查询 users 表中所有的数据：

```js
// 查询 users 表中的所有的用户数据

db.query('select * from users' , (err , results) => {
    // 查询失败
    if(err) return console.log(err.message)
    // 查询成功
    console.log(results)
})
```

**注意：如果执行的是 select 查询语句，则执行的结果是数组！**



###### **2. 插入数据**

向 users 表中新增数据， 其中 **username** 为 Spider-Man，**password** 为 pcc321。示例代码如下：

```js
// 1. 要插入到 users 表中的数据对象
const user = { username: 'Spider-Man', password: 'pcc321', status: 105 }

// 2. 待执行的 sql 语句， 其中英文的 ? 代表占位符
const sqlStr = 'insert into users (username , password, status) values (?,?,?)'

// 3. 使用数组的形式， 依次为 ? 占位符指定具体的值
db.query(sqlStr, [user.username, user.password, user.status], (err, results) => {
    if (err) return console.log(err.message) // 失败
        // 成功
    if (results.affectedRows === 1) {
        console.log('插入数据成功！')
    }
})
```

**注意：如果执行的是 insert into 插入语句，则 results 是一个对象**

**可以通过 results.affectedRows 属性，来判断是否插入数据成功**



###### **3. 插入数据的便捷方式**

向表中新增数据时，如果**数据对象的每个属性**和**数据表的字段****一一对应**，则可以通过如下方式快速插入数据：

```js
// 1. 要插入到 users 表中的数据对象
const user = { username: 'dapeng', password: 'aao321', status: 0 }

// 2. 待执行的 sql 语句， 其中英文的 ? 代表占位符
const sqlStr = ' insert into users set ? '


// 3. 直接将数据对象当作占位符的值
db.query(sqlStr, user, (err, results) => {
    if (err) return console.log(err.message) // 失败
        // 成功
    if (results.affectedRows === 1) {
        console.log('插入数据成功！')
    }
})
```



###### **4. 更新数据**

```js
// 1. 要更新的数据对象 (更新 id 为 105的数据)
const user = { id: 105, username: 'xiaohei', password: '123abc', status: 1 }

// 2. 要执行的 sql 语句
const sqlStr = 'update users set username = ? , password = ? , status = ? where id = ?'

// 3. 调用 db.query() 执行 sql 语句的同时，使用数组依次为占位符指定具体的值
db.query(sqlStr, [user.username, user.password, user.status, user.id], (err, results) => {
    if (err) return console.log(err.message) // 失败
        // 成功
    if (results.affectedRows === 1) {
        console.log('更新数据成功!')
    }
})
```

**注意： 执行了 update 语句之后，执行的结果，也是一个对象，可以通过 affectedRows 判断是否更新成功！**



###### **5. 更新数据的便捷方式**

更新表数据时，如果**数据对象的每个属性**和**数据表的字段**一一对应，则可以通过如下方式快速更新表数据：

```js
// 1. 要更新的数据对象 
const user = { id: 105, username: 'xiaobai', password: '123456', status: 1 }

// 2. 要执行的 sql 语句
const sqlStr = 'update users set ? where id = ?'

// 3. 调用 db.query() 执行 sql 语句的同时，使用数组依次为占位符指定具体的值
db.query(sqlStr, [user, user.id], (err, results) => {
    if (err) return console.log(err.message) // 失败
        // 成功
    if (results.affectedRows === 1) {
        console.log('更新数据成功!')
    }
})
```



###### **6. 删除数据**

在删除数据时，推荐根据 id 这样的**唯一标识**，来删除对应的数据。示例如下：

```js
// 1. 要执行的 sql 语句
const sqlStr = 'delete from users where id = ?'

// 2. 调用 db.query() 执行 sql 语句的同时，为占位符指定具体的值
// 注意： 如果 sql 语句中有多个占位符，则必须使用数组为，为每个占位符指定具体的值
//       如果 sql 语句中只有一个占位符，则可以省略数组
db.query(sqlStr, 106 , (err , results) => {
     if (err) return console.log(err.message) // 失败
        // 成功
    if (results.affectedRows === 1) {
        console.log('删除数据成功!')
    }
})
```



###### **7.** **标记删除**

使用 DELETE 语句，会把真正的把数据从表中删除掉。为了保险起见，**推荐使用**标记删除的形式，来**模拟删除的动作**。

所谓的标记删除，就是在表中设置类似于 **status** 这样的**状态字段**，来**标记**当前这条数据是否被删除。

当用户执行了删除的动作时，我们并没有执行 DELETE 语句把数据删除掉，而是执行了 UPDATE 语句，将这条数据对应

的 status 字段标记为删除即可。

```js
// 标记删除
// 使用 update 语句替代 delete 语句 ， 只更新数据的状态，并没有真正的删除
db.query('update users set status =? where id = ?' , [1 , 101] , (err, results) => {
    if (err) return console.log(err.message) // 失败
        // 成功
    if (results.affectedRows === 1) {
        console.log('标记删除数据成功!')
    }
})
```



### **4. 前后端的身份认证**



##### **1. Web 开发模式**

目前主流的 Web 开发模式有两种，分别是：

① 基于**服务端渲染**的传统 Web 开发模式

② 基于**前后端分离**的新型 Web 开发模式



前后端分离的概念：前后端分离的开发模式，**依赖于 Ajax 技术的广泛应用**。简而言之，前后端分离的 Web 开发模式，

就是**后端只负责提供 API 接口，前端使用 Ajax 调用接口**的开发模式。

优点：

① **开发体验好。**前端专注于 UI 页面的开发，后端专注于api 的开发，且前端有更多的选择性。

② **用户体验好。**Ajax 技术的广泛应用，极大的提高了用户的体验，可以轻松实现页面的局部刷新。

③ **减轻了服务器端的渲染压力。**因为页面最终是在每个用户的浏览器中生成的。

缺点：

① **不利于 SEO。**因为完整的 HTML 页面需要在客户端动态拼接完成，所以爬虫对无法爬取页面的有效信息。（解决方

案：利用 Vue、React 等前端框架的 **SSR** （server side render）技术能够很好的解决 SEO 问题！）





##### **2. 身份认证**



**1. 什么是身份认证**

**身份认证**（Authentication）又称“**身份验证**”、“**鉴权**”，是指**通过一定的手段，完成对用户身份的确认**。

⚫ 日常生活中的身份认证随处可见，例如：高铁的验票乘车，手机的密码或指纹解锁，支付宝或微信的支付密码等。

⚫ 在 Web 开发中，也涉及到用户身份的认证，例如：各大网站的**手机验证码登录**、**邮箱密码登录**、**二维码登录**等。



**2. 为什么需要身份认证**

身份认证的目的，是为了**确认当前所声称为某种身份的用户，确实是所声称的用户**。

在互联网项目开发中，如何对用户的身份进行认证，是一个值得深入探讨的问题。



**3. 不同开发模式下的身份认证**

对于服务端渲染和前后端分离这两种开发模式来说，分别有着不同的身份认证方案：

① 服务端渲染推荐使用 **Session 认证机制**

② 前后端分离推荐使用 **JWT 认证机制**



##### **3. Session 认证机制**



###### **1. HTTP 协议的无状态性**

HTTP 协议的无状态性，指的是客户端**的每次 HTTP 请求都是独立的**，连续多个请求之间没有直接的关系，**服务器不会**

**主动保留每次 HTTP 请求的状态**。



###### **2.** **如何突破 HTTP 无状态的限制**

![](images\session1.png)



###### **3. 什么是 Cookie**

Cookie 是**存储在用户浏览器中的一段不超过 4 KB 的字符串**。它由一个名称（Name）、一个值（Value）和其它几个用

于控制 Cookie 有效期、安全性、使用范围的可选属性组成。

不同域名下的 Cookie 各自独立，每当客户端发起请求时，会**自动**把**当前域名下**所有**未过期的 Cookie** 一同发送到服务器。

**Cookie的几大特性：**

① 自动发送

② 域名独立

③ 过期时限

④ 4KB 限制



###### **4. Cookie 在身份认证中的作用**

![](images\cookies.png)



###### **5. Cookie不具有安全性**

![](images\cookie2.png)

###### **6.** **提高身份认证的安全性**

![](images\cookie3.png)



###### **7.** **Session的工作原理**

![](images\session.png)





### **5. 在 Express 中使用 Session 认证**



##### **1.** **安装** **express-session 中间件**

在 Express 项目中，只需要安装 **express-session** 中间件，即可在项目中使用 Session 认证：

```
npm install express-session
```



##### **2.** **配置** **express-session 中间件**

express-session 中间件安装成功后，需要通过 **app.use()** 来注册 **session 中间件**，示例代码如下

```js
// 1. 导入 session 中间件
const session = require('express-session')

// 2. 配置 session 中间件
app.use(session({
    secret: 'keyboard cat',  // secret 属性的值可以为任意字符串
    resave: false,           // 固定写法
    saveUninitialized: true  // 固定写法
}))
```



##### **3. 向 session 中存数据**

当 **express-session** 中间件配置成功后，即可通过 **req.session** 来访问和使用 session 对象，从而存储用户的关键信息：

```js
app.post('.api/login' , (req,res) => {
    // 判断用户提交的登录信息是否正确
    if(req.body.username !== 'admin' || req.body.password !== '000000') {
        return res.send({ status: 1 , msg:'登陆失败' })
    }
    req.session.user = req.body  // 将用户的信息，存储到 session 中
    req.session.islogin = true   // 将用户的登录状态， 存储到 session 中
    
    res.send({ status: 0 , msg:'登录成功
})
```



##### **4. 从 session 中取数据**

可以直接从 **req.session** 对象上获取之前存储的数据，示例代码如下：

```js
// 获取用户姓名的接口
app.get('/api/username' , (req,res) => {
    // 判断用户是否登录
    if(!req.session.islogin) return res.send({ status: 1 , msg:'fail' })
    
    res.send( { status: 0 , msg: 'success' , username: req.session.user.username } )
})
```



##### **5. 清空 session**

调用 **req.session.destroy()** 函数，即可清空服务器保存的 session 信息。

```js
// 退出登录的接口
app.post('/api/logout' , (req,res) => {
    // 清空当前客户端对应的 session 信息
    req.session.destroy()
    res.send({
        status: 0,
        msg:'退出登录成功'
    })
})
```



##### **6. 了解 Session 认证的局限性**

Session 认证机制需要配合 Cookie 才能实现。由于 Cookie 默认不支持跨域访问，所以，当涉及到前端跨域请求后端接

口的时候，**需要做很多额外的配置**，才能实现跨域 Session 认证。

注意：

⚫ 当前端请求后端接口**不存在跨域问题**的时候，**推荐使用 Session** 身份认证机制。

⚫ 当前端需要跨域请求后端接口的时候，不推荐使用 Session 身份认证机制，推荐使用 JWT 认证机制。



### **6. JWT 认证机制**

JWT（英文全称：JSON Web Token）是目前**最流行**的**跨域认证解决方案**

##### **1. JWT 的工作原理**

![](images\jwt.png)



##### **2. JWT 的组成部分**

![](images\jwt1.png)



##### **3. JWT 的三个部分各自代表的含义**

![](images\jwt2.png)



##### **4. JWT 的使用方式**

![](images\jwt3.png)



###  **7. 在 Express 中使用 JWT**



##### **1.** **安装** **JWT 相关的包**

运行如下命令，安装如下两个 JWT 相关的包：

```
npm install jsonwebtoken express-jwt
```

其中：

⚫ **jsonwebtoken** 用于**生成 JWT 字符串**

⚫ **express-jwt** 用于**将 JWT 字符串解析还原成 JSON 对象**



##### **2.** **导入** **JWT 相关的包**

使用 **require()** 函数，分别导入 JWT 相关的两个包：

```js
// 1. 导入用于生成 jwt 字符串的包
const jwt = require('jsonwebtoken')

// 2. 导入用于将客户端发送过来的 jwt 字符串，解析还原成 json 对象的包
const expressJWT = require('express-jwt')
```



##### **3. 定义 secret 密钥**

为了**保证 JWT 字符串的安全性**，防止 JWT 字符串在网络传输过程中被别人破解，我们需要专门定义一个用于**加密**和**解密**

的 secret 密钥：

① 当生成 JWT 字符串的时候，需要使用 **secret 密钥**对用户的信息进行**加密**，最终得到加密好的 JWT 字符串

② 当把 JWT 字符串解析还原成 JSON 对象的时候，需要使用 secret 密钥**进行解密**

```js
// secret 密钥的本质： 就是一个字符串
const secretKey = 'xiaoheizi No1 ++_++'
```



##### **4. 在登录成功后生成 JWT 字符串**

调用 **jsonwebtoken** 包提供的 **sign()** 方法，**将用户的信息加密成 JWT 字符串**，**响应给客户端：**

```js
// 登录接口
app.post('/api/login' , function(req,res) => {
         // ...省略登录失败情况下的代码
         // 用户登录成功后， 生成 JWT 字符串， 通过 token 属性响应给客户端
         
         res.send({
            status: 200,
            message: '登录成功!',
            // 调用 jwt.sign() 生成 JWT 字符串， 三个参数分别是：用户信息对象、加密密钥、配置对象
            token: jwt.sign({ username: userinfo.username } , secretKey , { expiresIn : '30s'})
         })
})
```



##### **5. 将** **JWT 字符串还原为JSON 对象**

客户端每次在访问那些有权限接口的时候，都需要主动通过**请求头中的 Authorization 字段**，将 Token 字符串发

送到服务器进行身份认证。

此时，服务器可以通过 **express-jwt** 这个中间件，自动将客户端发送过来的 Token 解析还原成 JSON 对象：

```js
// 使用 app.use() 来注册中间件
// expressJWT({ secret: secretKey })  就是用来解析 Token 的中间件
// .unless({ path: [/^\/api\//] })  用来指定哪些接口不需要访问权限  /api开头的接口不需要访问权限
app.use(expressJWT({ secret: secretKey }).unless({ path: [/^\/api\//] }))
```



##### **6. 使用 req.user 获取用户信息**

当 express-jwt 这个中间件配置成功之后，即可在那些有权限的接口中，使用 **req.user** 对象，来访问从 JWT 字符串

中解析出来的用户信息了，示例代码如下：

```js
// 这是一个有权限的 api 接口
app.get('admin/getinfo' , function(req,res) {
    console.log(req.user)
    
    res.send({
        status: 200,
        message: '获取用户信息成功',
        data: req.user
    })
})
```



##### **7. 捕获解析 JWT 失败后产生的错误**

当使用 express-jwt 解析 Token 字符串时，如果客户端发送过来的 Token 字符串**过期**或**不合法**，会产生一个**解析失败**

的错误，影响项目的正常运行。我们可以通过 **Express 的错误中间件**，捕获这个错误并进行相关的处理，示例代码如下：

```js
app.use((err,req,res,next) => {
    // token 解析失败导致的错误
    if(err.name === 'UnauthorizedError') {
        return res.send({ status: 401 , message: '无效的token' })
    }
    // 其他原因导致的错误
    re.send({ status: 500, message: '未知错误' })
})
```



app.js

```js
// 导入 express 模块
const express = require('express')
// 创建 express 的服务器实例
const app = express()

// TODO_01：安装并导入 JWT 相关的两个包，分别是 jsonwebtoken 和 express-jwt
const jwt = require('jsonwebtoken')
const expressJWT = require('express-jwt')

// 允许跨域资源共享
const cors = require('cors')
app.use(cors())

// 解析 post 表单数据的中间件
const bodyParser = require('body-parser')
app.use(bodyParser.urlencoded({ extended: false }))

// TODO_02：定义 secret 密钥，建议将密钥命名为 secretKey
const secretKey = 'xiaoheizi No1 ++_++'

// TODO_04：注册将 JWT 字符串解析还原成 JSON 对象的中间件
// 注意： 只要这个express-jwt中间件配置成功之后，就可以把解析出来的用户信息，挂载到 req.user 属性上!!!!
// .unless({ path: [/^\/api\//] })  用来指定哪些接口不需要访问权限  /api开头的接口不需要访问权限
app.use(expressJWT({ secret: secretKey }).unless({ path: [/^\/api\//] }))

// 登录接口
app.post('/api/login', function(req, res) {
    // 将 req.body 请求体中的数据，转存为 userinfo 常量
    const userinfo = req.body
        // 登录失败
    if (userinfo.username !== 'admin' || userinfo.password !== '000000') {
        return res.send({
            status: 400,
            message: '登录失败！'
        })
    }
    // 登录成功
    // TODO_03：在登录成功之后，调用 jwt.sign() 方法生成 JWT 字符串。并通过 token 属性发送给客户端
    // 注意： 千万不要把密码加密到 token 字符串中
    // 调用 jwt.sign() 生成 JWT 字符串， 三个参数分别是：用户信息对象、加密密钥、配置对象,可以配置 token 有效期(30s后作废)
    const tokenStr = jwt.sign({ username: userinfo.username }, secretKey, { expiresIn: '300s' })
    res.send({
        status: 200,
        message: '登录成功！',
        // 要发送给客户端的 token 字符串
        token: tokenStr
    })
})

// 这是一个有权限的 API 接口
app.get('/admin/getinfo', function(req, res) {
    // TODO_05：使用 req.user 获取用户信息，并使用 data 属性将用户信息发送给客户端
    console.log(req.user)
    res.send({
        status: 200,
        message: '获取用户信息成功！',
        data: req.user // 要发送给客户端的用户信息
    })
})

// 项目最后注册错误的中间件
// TODO_06：使用全局错误处理中间件，捕获解析 JWT 失败后产生的错误
app.use((err, req, res, next) => {
    // token 解析失败导致的错误
    if (err.name === 'UnauthorizedError') {
        return res.send({ status: 401, message: '无效的token' })
    }
    // 其他原因导致的错误
    re.send({ status: 500, message: '未知错误' })
})

// 调用 app.listen 方法，指定端口号并启动web服务器
app.listen(8888, function() {
    console.log('Express server running at http://127.0.0.1:8888')
})
```

![](images\token1.png)

![](images\token2.png)





