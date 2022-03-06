# 单元测试-jest使用

## 1.介绍

​		单个功能或接口，给定输入，得到输出，看输出是否符合要求。需要手动编写用例代码然后统一执行。能一次性执行所以单元测试，短时间内验证所有功能是否正常

## 2.jest

### 2.1.要点

- .test.js

- 常用的断言

- 测试http接口

### 2.2.使用

1. 安装jest

   ```
   npm i jest -D
   ```

​     2.配置package.json中test命令

```
 "scripts": {
    "dev": "cross-env NODE_ENV=dev ./node_modules/.bin/nodemon --inspect=9229 bin/www",
    "prd": "cross-env NODE_ENV=production pm2 start pm2.conf.json",
    "lint": "eslint --ext .js ./src",
    "test": "cross-env NODE_ENV=test jest --runInBand --forceExit --colors"
  }
# --runInBand顺序执行
# --forceExit强制退出
# --colors按颜色区分

```

​     3.在根目录下创建test文件夹

**demo.test.js:**

```
/**
 * @description 单元测试demo
 * @author AntaresLpq
 */

function twoSum(a,b){
  return a+b;
}

//单元测试
test("10+20 should be equal 30",()=>{
  const res=twoSum(10,20)
  expect(res).toBe(30)
})

```

![img](https://uploader.shimo.im/f/IuZ3f4vxpVrtbyEd.png!thumbnail?accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJhdWQiOiJhY2Nlc3NfcmVzb3VyY2UiLCJleHAiOjE2NDY1MzM1MTMsImciOiJlMUF6NHhHWkthSEJwTXFXIiwiaWF0IjoxNjQ2NTMzMjEzLCJ1c2VySWQiOjIyNTg4NjIzfQ.nYaoJ9f0xHedoQkIEeTn4UZ9yhL0McM1iWIVnViV-c4)

​       4.运行npm run test

![img](https://uploader.shimo.im/f/sOG9uJUOtkChiEtj.png!thumbnail?accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJhdWQiOiJhY2Nlc3NfcmVzb3VyY2UiLCJleHAiOjE2NDY1MzM1MTMsImciOiJlMUF6NHhHWkthSEJwTXFXIiwiaWF0IjoxNjQ2NTMzMjEzLCJ1c2VySWQiOjIyNTg4NjIzfQ.nYaoJ9f0xHedoQkIEeTn4UZ9yhL0McM1iWIVnViV-c4)

## 3.测试http接口

1.安装supertest

```
npm i supertest -D
```

2.test目录下新建server.js

```
/**
 * @description 测试http接口
 * @author AntaresLpq
 */

const request=require('supertest')
const server=require('../src/app').callback()
module.exports=request(server)

```

3.测试json接口

```
/**
 * @description 测试json接口
 * @author AntaresLpq
 */

const server = require('./server')

//get 请求
test('should (json 接口数据返回格式正确)', async () => {
  const res = await server.get('/json')
  expect(res.body).toEqual({
    title: 'koa2 json'
  })
  expect(res.body.title).toBe('koa2 json')
})

//post 请求
test('should (json 接口数据返回格式正确)', async () => {
  const res = await (await server.post('/login')).send({
    username:"admin",
    password:"123456"
  })
  expect(res.body).toEqual({
    title: 'koa2 json'
  })
  expect(res.body.title).toBe('koa2 json')
})

```

