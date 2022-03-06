# koa2+jwt实现登录

## 1.jwt

jwt：json web token

- token:用户认证成功后服务器返回给客户端的秘钥（加密的token）

******

- 客户端后续每次都会在请求头中携带token,以示用户信息

- 服务端反解加密的token校验用户信息**

## **2.模拟登陆-不带token明文传输**

- 使用koa2 -e test生成一个koa2项目

- 在路由中编写登陆代码：

  ```
  const router = require('koa-router')()
  
  router.prefix('/users')
  
  router.get('/', function (ctx, next) {
    ctx.body = 'this is a users response!'
  })
  
  router.get('/bar', function (ctx, next) {
    ctx.body = 'this is a users/bar response'
  })
  
  //模拟登陆
  router.post('/login', async (ctx, next) => {
    let userInfo
    const {
      username,
      password
    } = ctx.request.body
    if (username === 'lpq' && password === '123456') {
      //校验成功
      userInfo = {
        userId: 1,
        username: 'lpq',
        nickName: "李培强",
        gender: 1 //男
      }
    }
    if (!userInfo) {
      //校验失败
      ctx.body = {
        errno: -1,
        msg: "登录失败！"
      }
      return
    }
    ctx.body = {
      errno: 0,
      msg: "登陆成功！"
    }
  })
  
  module.exports = router
  
  ```

- 启动：npm run dev

- 在postman中模拟登陆信息

  ![img](https://uploader.shimo.im/f/wOW3DHhoxrIWhU4r.png!thumbnail?accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJhdWQiOiJhY2Nlc3NfcmVzb3VyY2UiLCJleHAiOjE2NDY1NjI0NDgsImciOiJLcmtFVk93eHp6Rnd3OUFKIiwiaWF0IjoxNjQ2NTYyMTQ4LCJ1c2VySWQiOjIyNTg4NjIzfQ.I7WPJBWsyvfXJM-eCarOqX2-_GOTP2sDTnbsEuQ4enc)

  

  ## 3.模拟登陆-加密token

  ### 3.1.安装依赖

  ```
  npm i koa-jwt -D
  ```

  ### 3.2.在app.js中引入使用jwt

  1.引入

  ```
  const jwtKoa=require('koa-jwt')
  
  ```

  2.jwt验证

  ```
  //jwt对返回的用户信息进行加密同时对指定请求外的接口进行校验
  app.use(jwtKoa({
    secret:"Yu199*&_lpq"
  }).unless(
    {
      path:[/^\/users\/login/]//自定义哪些目录不需要token验证（jwt验证--对/users/login取消校验）
    }
  ))
  
  ```

  **完整app.js:**

  ```
  const Koa = require('koa')
  const app = new Koa()
  const views = require('koa-views')
  const json = require('koa-json')
  const onerror = require('koa-onerror')
  const bodyparser = require('koa-bodyparser')
  const logger = require('koa-logger')
  const jwtKoa=require('koa-jwt')
  const {SECRET} =require('./conf/const')
  
  const index = require('./routes/index')
  const users = require('./routes/users')
  
  // error handler
  onerror(app)
  
  //jwt对返回的用户信息进行加密同时对指定请求外的接口进行校验
  app.use(jwtKoa({
    secret:"Yu199*&_lpq"
  }).unless(
    {
      path:[/^\/users\/login/]//自定义哪些目录不需要token验证（jwt验证）
    }
  ))
  // middlewares
  app.use(bodyparser({
    enableTypes:['json', 'form', 'text']
  }))
  app.use(json())
  app.use(logger())
  app.use(require('koa-static')(__dirname + '/public'))
  
  app.use(views(__dirname + '/views', {
    extension: 'ejs'
  }))
  
  // logger
  app.use(async (ctx, next) => {
    const start = new Date()
    await next()
    const ms = new Date() - start
    console.log(`${ctx.method} ${ctx.url} - ${ms}ms`)
  })
  
  // routes
  app.use(index.routes(), index.allowedMethods())
  app.use(users.routes(), users.allowedMethods())
  
  // error-handling
  app.on('error', (err, ctx) => {
    console.error('server error', err, ctx)
  });
  
  module.exports = app
  ```

  3.token加密

  ```
  npm i jsonwebtoken -D
  ```

  登陆接口中对登陆成功返回的结果进行加密：

  ```
    let token
    if(userInfo){
      token=jwt.sign(userInfo,"Yu199*&_lpq",{expiresIn:"1h"})//expiresIng--token过期时间
    }
  
  ```

  完整代码如下：

  ```
  const router = require('koa-router')()
  const jwt =require('jsonwebtoken')
  router.prefix('/users')
  
  router.get('/', function (ctx, next) {
    ctx.body = 'this is a users response!'
  })
  
  router.get('/bar', function (ctx, next) {
    ctx.body = 'this is a users/bar response'
  })
  
  //模拟登陆
  router.post('/login', async (ctx, next) => {
    let userInfo
    const {
      username,
      password
    } = ctx.request.body
    if (username === 'lpq' && password === '123456') {
      //校验成功
      userInfo = {
        userId: 1,
        username: 'lpq',
        nickName: "李培强",
        gender: 1 //男
      }
    }
    //加密userInfo
    let token
    if(userInfo){
      token=jwt.sign(userInfo,"Yu199*&_lpq",{expiresIn:"1h"})//expiresIng--token过期时间
    }
    if (!userInfo) {
      //校验失败
      ctx.body = {
        errno: -1,
        msg: "登录失败！"
      }
      return
    }
    ctx.body = {
      errno: 0,
      data:token//登陆成功后返回加密的token
    }
  })
  
  module.exports = router
  
  ```

  这样返回登陆成功的用户信息都是经过jwt加密后的，结果如下图所示：

  ![img](https://uploader.shimo.im/f/y2dHPczBTgaAqSMz.png!thumbnail?accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJhdWQiOiJhY2Nlc3NfcmVzb3VyY2UiLCJleHAiOjE2NDY1NjI3OTksImciOiJLcmtFVk93eHp6Rnd3OUFKIiwiaWF0IjoxNjQ2NTYyNDk5LCJ1c2VySWQiOjIyNTg4NjIzfQ.D-zOUqZaL0a9s4uHtoxYuNy4LPDVS8Yyjkc2FkMCsB8)

## 4.客户端携带token给服务端-获取加密token(用户信息)

**服务端将登陆的用户信息加密成token后返回给客户端，下次客户端需要在请求头中将该token带给服务端，服务端会对客户端发送来的请求进行jwt校验，校验通过才能进行后续操作，否则返回401（权限不足）

1.客户端获取token通过jwt校验客户端需要在请求头携带token,携带方式：Authorization=Bearer token(Bearer 是koa2的限制)

![img](https://uploader.shimo.im/f/Q4B83uTSiP7eKZ2X.png!thumbnail?accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJhdWQiOiJhY2Nlc3NfcmVzb3VyY2UiLCJleHAiOjE2NDY1NjMwOTUsImciOiJLcmtFVk93eHp6Rnd3OUFKIiwiaWF0IjoxNjQ2NTYyNzk1LCJ1c2VySWQiOjIyNTg4NjIzfQ.65WOCyIfhKEH-HAUesbLwPJCGKb6fFZaYPYTnHJiReM)

![img](https://uploader.shimo.im/f/Q4B83uTSiP7eKZ2X.png!thumbnail?accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJhdWQiOiJhY2Nlc3NfcmVzb3VyY2UiLCJleHAiOjE2NDY1NjMwOTUsImciOiJLcmtFVk93eHp6Rnd3OUFKIiwiaWF0IjoxNjQ2NTYyNzk1LCJ1c2VySWQiOjIyNTg4NjIzfQ.65WOCyIfhKEH-HAUesbLwPJCGKb6fFZaYPYTnHJiReM)**2.服务端解密token**

```
//获取用户信息--解密token用户信息
router.get('/userInfo', async (ctx, next) => {
  const token = ctx.header.authorization
  try {
    const payload = await verify(token.split(' ')[1], SECRET)//解密
    ctx.body = {
      errno: 0,
      userInfo: payload
    }
  } catch (err) {
    ctx.body = {
      errno: -1,
      msg: `校验失败：${err}`
    }
  }
})

```

完整代码如下：

```
const router = require('koa-router')()
const jwt = require('jsonwebtoken')
const util = require('util')
const verify = util.promisify(jwt.verify) //编程异步

const SECRET = 'Yu199*&_lpq'

router.prefix('/users')
router.get('/', function (ctx, next) {
  ctx.body = 'this is a users response!'
})

router.get('/bar', function (ctx, next) {
  ctx.body = 'this is a users/bar response'
})

//模拟登陆
router.post('/login', async (ctx, next) => {
  let userInfo
  const {
    username,
    password
  } = ctx.request.body
  if (username === 'lpq' && password === '123456') {
    //校验成功
    userInfo = {
      userId: 1,
      username: 'lpq',
      nickName: "李培强",
      gender: 1 //男
    }
  }
  //加密userInfo
  let token
  if (userInfo) {
    token = jwt.sign(userInfo, SECRET, {
      expiresIn: "1h"
    }) //expiresIng--token过期时间
  }
  if (!userInfo) {
    //校验失败
    ctx.body = {
      errno: -1,
      msg: "登录失败！"
    }
    return
  }
  ctx.body = {
    errno: 0,
    data: token //登陆成功后返回加密的token
  }
})

//获取用户信息--解密token用户信息
router.get('/userInfo', async (ctx, next) => {
  const token = ctx.header.authorization
  try {
    const payload = await verify(token.split(' ')[1], SECRET)//解密
    ctx.body = {
      errno: 0,
      userInfo: payload
    }
  } catch (err) {
    ctx.body = {
      errno: -1,
      msg: `校验失败：${err}`
    }
  }
})
module.exports = router

```

![img](https://uploader.shimo.im/f/sHtQ7WSXyN4176WH.png!thumbnail?accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJhdWQiOiJhY2Nlc3NfcmVzb3VyY2UiLCJleHAiOjE2NDY1NjMwOTUsImciOiJLcmtFVk93eHp6Rnd3OUFKIiwiaWF0IjoxNjQ2NTYyNzk1LCJ1c2VySWQiOjIyNTg4NjIzfQ.65WOCyIfhKEH-HAUesbLwPJCGKb6fFZaYPYTnHJiReM)

**过程：当客户端登陆时，服务端首先将登陆的用户信息通过jwt加密成token并返回给客户端,当下次用户做其他操作时，客户端需要在请求头中携带加密的token,服务端会校验解析token，如果解析成功则通过解析为明文的用户信息（如userId/username等）去数据库中查询处理数据，解析失败返回错误信息并终止操作。**

## **5.jwt与session的登录的区别**

- 都可以处理登录和存储用户信息
- jwt用户信息加密后的token存储在客户端，不依赖cookie，可跨域，session存储在服务端，依赖redis
- session存储在服务端，强依赖cookie，默认不跨域（跨域可使用代理....）
- 一般情况下两者都可以满足登录，大型系统可以共用
- jwt更适合服务节点较多，跨域较多的情况
- session更适合统一的web服务，server要严格管理用户信息