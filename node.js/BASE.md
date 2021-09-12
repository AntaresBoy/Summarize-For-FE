# 1.node.js处理get请求

## 1.1 初始化npm

npm init -y得到package.json

```json
{
  "name": "node_demo",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}

```

## 1.2  index.js

```js
const http=require('http')
const querystring=require('querystring')
const server=http.createServer((req,res)=>{
  console.log('method:',req.method)
  const url=req.url
  console.log('url',url)
  req.query=querystring.parse(url.split('?')[1])
  console.log('query',req.query)
  res.end(JSON.stringify(req.query))
})
server.listen(8000)
console.log('ok')
```

