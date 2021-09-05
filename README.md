## 1. 前言

主要对前端学习进行记录与总结，方便后续复习学习。主要包括前端：JavaScript、CSS、HTML、TypeScript、Node.js、express、Koa、webpack、性能优化、可视化、Vus.js、React.js、工具函；后端：go、Node.js、数据库(mysql、monogoDB、redis)、nginx、等知识点。

## 2. JavaScript



## 3. CSS



## 4. HTML



## 5. TypeScript



## 6. Node.js



## 7. Express



## 8.  Koa



## 9.  Webapack



## 10. 可视化



## 11. Vue.js



## 12. React.JS



## 13. 工具函数封装

### 13.1 手写call函数

**call.js**

```javascript
function call(FN,obj,...args){
  //传入obj为null或undefined时this指向全局对象
  if(obj===undefined|| obj===null){
    obj=globalThis
  }
  //为obj添加FN临时方法
  obj.tmp=FN
  //执行临时方法
  let result=obj.tmp(...args)
  //删除临时方法
  delete obj.tmp
  return result
}
```

**call.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <script src="./call.js"></script>
</head>
<body>
  <script>
    function add(a,b){
      console.log("this",this)
      return a+b+this.c

    }
    let obj={
      c:521
    }
    window.c=1314
    console.log('obj',call(add,obj,120,340))
    console.log('window',call(add,null,120,340))

  </script>
</body>
</html>
```

