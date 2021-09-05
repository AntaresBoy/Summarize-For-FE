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

### 13.1 封装call()函数

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

### 13.2 封装apply()函数

apply.js:

```javascript
function apply(Fn,obj,args){
  if(obj===undefined|| obj===null){
    obj=globalThis
  }
  obj.tmp=Fn
  const result=obj.tmp(...args)
  delete obj.tmp
  return result
}
```

apply.html:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <script src="./apply.js"></script>
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
    console.log('obj',apply(add,obj,[120,340]))
    console.log('window',apply(add,null,[120,340]))
  </script>
</body>
</html>
```

### 13.3 封装bind()函数

bind.js:

```javascript
function bind(Fn,obj,...args){
  //返回一个新函数
  return function(...args2){
    //执行call()方法
    return call(Fn,obj,...args,...args2)
  }
}
```

bind.html:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <script src="./bind.js"></script>
  <!-- 引入call(函数) -->
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
   let fn1=bind(add,obj,10,20)//返回一个函数是传入参数，等待被调用
   console.log('fn1',fn1())
   let fn2=bind(add,obj)//返回一个函数
   console.log('fn2',fn2(10,20))//调用时传参
   let fn3=bind(add,obj,10,20)//返回一个函数是传入参数，等待被调用
   console.log('fn3',fn3(30,40))//调用时传入参数。
  </script>
</body>
</html>
```

### 13.4 总结

1. apply()、call()、bind()函数都是用来更改this指向的函数；
2. call()函数传入的参数时单个基本数据类型格式，而apply()函数传入的参数时一个数组；
3. call()、apply()函数一旦被调用会立即被执行，而bind()函数会先返回一个函数，等待函数被调用，该函数可以传入参数；
4. bind()函数一旦被调用对象的this指向会永久被改变。
5. bind()函数既可以在调用bind()函数时传入参数也可以在返回函数后在调用返回的函数中传入参数；
