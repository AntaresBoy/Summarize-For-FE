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



## 13. 封装工具函数

### 13.1 call()、apply()、bind()函数

#### 13.1.1 封装call()函数

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

#### 13.1.2 封装apply()函数

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

#### 13.1.3 封装bind()函数

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

#### 13.1.4 总结

1. apply()、call()、bind()函数都是用来更改this指向的函数；
2. call()函数传入的参数时单个基本数据类型格式，而apply()函数传入的参数时一个数组；
3. call()、apply()函数一旦被调用会立即被执行，而bind()函数会先返回一个函数，等待函数被调用，该函数可以传入参数；
4. bind()函数一旦被调用对象的this指向会永久被改变。
5. bind()函数既可以在调用bind()函数时传入参数也可以在返回函数后在调用返回的函数中传入参数；

### 13.2 函数防抖与节流

作用：限制事件处理函数的频率调用；

#### 13.2.1 函数节流(throttle)

理解：

- 当函数需要频繁触发时，函数触发一次后，下一次触发只有当时间间隔大于设定的时间周期后才能被触发；
- 适合多次事件按照时间做平均分配触发；
- 语法：throttle(callback,wait);
- 功能：创建一个节流函数，在wait时间间隔内只能执行callback回调函数一次；

场景：

- 窗口调整（resize）
- 页面滚动（scroll）
- DOM元素拖拽功能及鼠标移动时间（mousemove）
- 秒杀疯狂点击（click）

实现：

throttle.js:

```javascript
function throttle(callback,wait){
  //定义起始时间
  let start=0
  return function(event){
    let now=Date.now()
    if(now-start>wait){
      callback.call(this,event)//回调函数的this指向事件源
    }
    start=now
  }
}
```

throttle.html:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>节流测试</title>
  <script src="./throttle.js"></script>
  <style>
    body{
      height: 2000px;
      background-color: aqua;
    }
  </style>
</head>
<body>
  <script>
      //监听鼠标滚动事件
window.addEventListener('scroll',throttle((event)=>{
  console.log(Date.now(),event)
},500))
  </script>
</body>
</html>
```

#### 13.2.2 函数防抖(debounce)

语法：deboundce(callback,wait)

功能：创建一个防抖函数，该函数会从上一次被调用后，延迟wait时间间隔后再次调用callback回调函数，如果在wait间隔内事件再次被触发则继续延迟等待一个wait时间间隔，直到wait时间间隔内没有事件被处罚才回去执行回调函数callback;

debounce.js:

```js
function debounce(callback,wait){
  let timeId;
  return function(event){
    if(timeId){
      clearTimeout(timeId)//清除定时器
    }
    timeId=setTimeout(() => {
      callback.call(this,event)
      timeId=null;//防止执行无用的判断代码，第456行
    }, wait);
  }
}
```

debounce.html:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>防抖测试</title>
  <script src="./debounce.js"></script>
</head>
<body>
  <input type="text">
  <script>
    let input=document.querySelector('input')
    //防抖前
    // input.onkeydown=function(e){
    //   console.log(e.keyCode)//频率很高
    // }
    //防抖后
    input.onkeydown=debounce((event)=>{
      console.log(event.keyCode)//返回按键对应的ascII码
    },1000)
  </script>
</body>
</html>
```

### 13.3 数组相关

#### 13.3.1 map()函数

MDN:[javascript数组操作之map](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/map)

map.js:

```js
function map(arr,callback){
  let result=[]
  for(let index=0;index<arr.length;index++){
    result.push(callback(arr[index],index))
  }
  return result;
}

let arr=[1,2,3,4]
const res=map(arr,(curItem,index)=>{
  console.log(index)
  return curItem*10;
})
console.log(res)

```

#### 13.3.2 reduce()函数

reduce.js

```javascript

/**
 * 
 * @param {Array} array 
 * @param {Function} callback 
 * @param {*} initValue 
 * @returns 
 */
function reduce(array,callback,initValue) {
  let result=initValue
  for(let index=0;index<array.length;index++){
    result=callback(result,array[index])
  }
  return result
}
```

reduce.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <script src="./reduce.js"></script>
  <title>封装reduce函数</title>

</head>
<body>
  <script>
    const arr=[1,2,3,4,5]
    let result=reduce(arr,(curResult,value)=>{
      return curResult +value
    },10)
    console.log(result)
  </script>
</body>
</html>
```

#### 13.3.3 数组去重

```js
/**
 * @description 利用indexOf()与forEach()方法数组去重
 * @param {Array} array 
 * @returns 
 */
function unique(array) {
  const result=[]
  array.forEach(element => {
    if(result.indexOf(element)===-1){
      result.push(element)
    }
  });
  return result
}

function unique1(array) {
  const result=array.filter((curItem,index,array)=>{
    if(array.indexOf(curItem)===index){
      return curItem
    }
  })
  return result
}

/**
 * @description 利用es6的Array.from()与 Set集合去重或展开运算符与Set()
 * @param {Array} array 
 * @returns 
 */
function unique2(array) {
  // const result=Array.from(new Set(array))
  // return result
  return [...new Set(array)]
}

/**
 * @description 使用对象去重
 * @param {Array} array 
 * @returns 
 */
function unique3(array) {
  let result=[]
  const obj={}
  array.forEach((curItem)=>{
    if(!obj.hasOwnProperty(curItem)){
      obj[curItem]=true
      result.push(curItem)
    }
  })
  return result;
}

const a=[1,2,3,2,3,4,5,5,6]
console.log(unique1(a))

```

#### 13.3.4 对象、数组拷贝之浅拷贝

```js
function shallowClone(target) {
  if(typeof target==='object' && target!==null) {
    if(Array.isArray(target)){
      return [...target]
    }else{
      return {...target}
    }
  }else{
    return target
  }
}

/**
 * @description 使用ES5进行浅拷贝
 * @param {Array  || Object} target 
 * @returns 
 */
function shallowClone1(target) {
  if(typeof target==='object' && target!==null){
    let result=Array.isArray(target)?[]:{}
    for(let key in target){
      if(target.hasOwnProperty(key)){
        result[key]=target[key]
      }
    }
    return result
  }else{
    return target
  }
  
}

let obj={a:1,b:3,c:{d:6}}
const obj1=shallowClone1(obj)
console.log(obj1)
obj1.c.d=8
console.log(obj1,obj)
```

#### 13.3.5 对象、数组拷贝之深拷贝

```js
/**
 * @description 乞丐版
 * @description 存在问题：1对象中有函数无法进行拷贝 2、无法对循环引用进行深拷贝
 * @param {Array || Object} target 
 * @returns 
 */
function deepClone1(target) {
  return JSON.parse(JSON.stringify(target))
}

/**
 * @description 高级版。利用递归 可解决无法拷贝对象中函数的问题。
 * @param {Array || Object} target 
 * @returns 
 */
function deepClone2(target) {
  if(typeof target==='object'  && target!==null){
    let result= Array.isArray(target)?[]:{}
    for(let key in target){
      if(target.hasOwnProperty(key)){
        result[key]=deepClone2(target[key])
      }
    }
    return result
  }else{
    return target
  }
  
}

/**
 * @description 终极版  解决无法拷贝函数即无法深拷贝循环引用的问题
 * @param {Array || Object} target 
 * @param {*} map 
 * @returns 
 */
function deepClone3(target,map=new Map()) {
  if(typeof target==='object' && target!==null){
    const catchTarget=map.get(target)
    if(catchTarget){
      return catchTarget
    }
    let result=Array.isArray(target)?[]:{}
    map.set(target,result)
    for(let key in target){
      if(target.hasOwnProperty(key)){
        result[key]=deepClone3(target[key],map)
      }
    }
    return result
  }else{
    return target
  }
}


/**
 * @description 优化终极版 for in 循环会遍历原型数据，性能较差
 * @param {Array || Object} target 
 * @param {*} map 
 * @returns 
 */
 function deepClone4(target,map=new Map()) {
  if(typeof target==='object' && target!==null){
    const catchTarget=map.get(target)
    if(catchTarget){
      return catchTarget
    }
    let isArray=Array.isArray(target)
    let result=isArray?[]:{}
    map.set(target,result)
    if(isArray){
      target.forEach((curItem,index) => {
        result[index]=deepClone4(curItem,map)
      });
    }else{
      Object.keys(target).forEach((key)=>{
        result[key]=deepClone4(target[key],map)

      })
    }
    return result
  }else{
    return target
  }
}

let obj={a:1,b:3,c:{d:[1,2,3]},f:function (params) {
},j:{h:20}}
obj.c.d.push(obj.j)//循环引用
obj.j.k=obj.c.d//循环引用
const result=deepClone3(obj)
result.c.d.push(1000)
result.j.m=10000
console.log(obj)
console.log(result)
```

#### 13.3.6 数组扁平化

```js
/**
 * @description 使用递归，对数组进行扁平化
 * @param {Array} array 
 * @returns 
 */
function flatten(array) {
  let result=[]
  array.forEach(curItem => {
    if(Array.isArray(curItem)){
      result=result.concat(flatten(curItem))
    }else{
      result=result.concat(curItem)
    }
    
  });
  return result
}


/**
 * @description 使用some和concat对数组进行扁平化
 * @param {Array} array 
 * @returns 
 */
function flatten1(array) {
  let result=[...array]  
  while(result.some(curItem=>Array.isArray(curItem))){
    result=[].concat(...result)
  }
  return result
}

const arr=[1,2,3,[4,5,6,[7,8],9],10]
console.log(flatten1(arr))
```

#### 13.3.7 数组分块

```js

/**
 * @description 数组分块
 * @param {Array} array 
 * @param {Number} size 
 * @returns 
 */
function chunk(array,size=1) {
  if(array.length===0){
    return []

  }
  let result=[]
  let tmp=[]
  array.forEach(curItem => {
    if(tmp.length===0){
      result.push(tmp)
    }
    tmp.push(curItem)
    if(tmp.length===size){
      tmp=[]
    }
  });
  return result;
}

const arr=[1,2,3,4,5,6,7]
console.log(chunk(arr,3))
```

#### 13.3.8 数组差集

```js

/**
 * @description 数组差集
 * @param {Array} arr1 
 * @param {Array} arr2 
 * @returns 
 */
function arrayDifference(arr1,arr2=[]) {
  if(arr1.length===0){
    return []
  }
  if(arr2.length===0){
    return arr1.slice()
  }
  let result = arr1.filter(curItem=> !arr2.includes(curItem))
  return result
}

const arr=[1,2,4,5,6]
const arr2=[2,3,5,0]
console.log(arrayDifference(arr,arr2))
```

#### 13.3.9 删除指定数组中部分元素

```js
function pull(arr,...args) {
  const result=[]
  for(let index=0;index<arr.length;index++){
    if(args.includes(arr[index])){
      result.push(arr[index])
      arr.splice(index,1)
      index--
    }
  }
  return result
}

function pullAll(arr,values) {
  return pull(arr,...values)
}
const arr =[1,3,5,3,7]
const arr1 =[1,3,5,3,7]
console.log(pull(arr,1,7,3,7))
console.log(pullAll(arr1,[1,7,3,7]))

```

#### 13.3.10 获取数组指定位置之前或之后的元素数组值

```js
/**
 * @description 从左边开始返回原数组索引为size之后的值的数组，不改变原数组
 * @param {Array} array 
 * @param {Number} size 
 * @returns 
 */
function drop(array,size=0) {
  if(size===0){
    return array
  }
  // return array.filter((curItem,index)=>{
  //   return index<size
  // })
  return array.filter((curItem,index)=>index>=size)//简化
}

/**
 * @description 从右边开始返回原数组索引为size之前的值的数组，不改变原数组
 * @param {Array} array 
 * @param {Number} size 
 * @returns 
 */
function dropRight(array,size=0) {
  if(size===0){
    return array
  }
  return array.filter((curItem,index)=>index<array.length-index)
}
const arr=[1,2,3,45,6]
console.log(drop(arr,3))
console.log(dropRight(arr,3))

```

#### 13.3.11 实现instanceOf()关键字

instanceof 在MDN上的解释：用于检测构造函数的原型是否出现在了实例对象的原型链

```js
/**
 * 
 * MDN解释:instanceof 用于检测构造函数的原型属性prototype是否出现在实例对象的原型链上_proto_                                                                                                                                                                                                                                                                                                                                                                                                                                
 * 
 * 手写实现instanceof:第一个参数：实例对象；第二个参数：构造函数
 * 如果实例对象的原型等于构造函数的原型返回true
 */

function myInstanceof(instance, constructor) {
  if (typeof instance !== 'object') {
    throw 'the instance must be Object!'
  }
  if (typeof constructor !== 'function') {
    throw 'the constructor must be Function!'
  }
  let prototype = instance.__proto__
  while (prototype) {
    if (!prototype.__proto__) {
      return false
    } else if (prototype === constructor.prototype) {
      return true
    } else {
      prototype = prototype.__proto__
    }
  }
}
function supType() {}
function subType() {}
subType.prototype = new supType()
const instance = new subType()
console.log(myInstanceof(instance, supType)) //true

```

#### 13.3.12 多个对象合并

```js
function mergeObject(...args) {
  const result={}
  args.forEach((obj)=>{
    Object.keys(obj).forEach(key=>{
      if(result.hasOwnProperty(key)){
        result[key]=[].concat(result[key],obj[key])
      }else{
        result[key]=obj[key]
      }
    })
  })
  return result
}


const obj1={
  a:{a:1,b:2},
  b:[1,2,3],
  c:"lpq"
}

const obj2={
  a:{c:3},
  b:4
}
console.log(mergeObject(obj1,obj2))

```

#### 13.3.13 字符传操作（翻转、回文、截取）

```js
/**
 * @description 字符串翻转
 * @param {String} str 
 * @returns 
 */
function reverseString(str) {
  const chartList=str.split("")
  chartList.reverse()
  const result=chartList.join("")
  return result
}

/**
 * @description 判断字符串是否为回文字符串
 * @param {String} str 
 * @returns 
 */
function palindrome(str) {
  return reverseString(str)===str
}

/**
 * @description 截取字符串
 * @param {String} str 
 * @param {Number} size 
 * @returns 
 */
function truncate(str,size) {
  return str.slice(0,size)+'...'
}


const str1='i love you!'
const str2='lol'
console.log(reverseString(str1))
console.log(palindrome(str2))
console.log(truncate(str1,2))


```

#### 13.3.14 实现事件委托

##### [JavaScript 事件流模型及事件委托详解](https://segmentfault.com/a/1190000015719043)

实现：

addEventListener.js

```js
/**
 * @description 实现事件委托
 * @param {any} el 
 * @param {String} type 
 * @param {Function} callback 
 * @param {String} selector 
 */
function addEventListener(el,type,callback,selector) {
  //判断元素类型
  if(typeof el==='string'){
    el=document.querySelector(el)
  }
  //如果没有传递子元素选择器，给父元素el绑定事件
  if(!selector){
    el.addEventListener(type,callback)
  } else{
    el.addEventListener(type,function (e) {
      //获取点击目标事件源
      const target=e.target
      //判断选择器与目标元素类型是否相等
      if(target.matches(selector)){
        callback.call(target,e)
      }
    })
  }
}
```

addEventListener.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <script src="./addeventListener.js"></script>
  <title>事件委托</title>
</head>
<body>
  <ul id="bubbleEvent">
    <li>AAAA</li>
    <li>BBBB</li>
    <li>CCCC</li>
    <li>DDDD</li>
    <div>EEEE</div>
  </ul>
<script>
addEventListener('#bubbleEvent','click',function(e){
  console.log(this.innerHTML)
},'li')
</script>
</body>
</html>
```

#### 13.3.15 手写全局事件总线eventBus

```js
const eventBus={
  callbacks:{}
}

/**
 * @description 绑定事件
 * @param {String} eventName 
 * @param {Function} callback 
 */
eventBus.on=function (eventName,callback) {
  if(this.callbacks[eventName]){
    this.callbacks[eventName].push(callback)
  }else{
    this.callbacks[eventName]=[callback]
  }
}

/**
 * @description 触发事件
 * @param {String} eventName 
 * @param {...any} data 
 */
eventBus.emit=function (eventName,data) {
  if(this.callbacks[eventName]&&this.callbacks[eventName].length>0){
    this.callbacks[eventName].forEach(callback=> {
      callback(data)
    });
  }
}

/**
 * @description 解绑事件
 * @param {String | Array} eventName 
 */
eventBus.off=function (eventName) {
  if(!eventName){
    this.callbacks={}
   }else if(Array.isArray(eventName)){
    eventName.forEach(item=>delete this.callbacks[item])
   }else{
    delete this.callbacks[eventName]
  }
}

eventBus.on('login',(data)=>{
  console.log(data+'登录了')
})
eventBus.on('login',(data)=>{
  console.log(data+'写入了')
})
eventBus.on('logout',(data)=>{
  console.log(data+'登出了')
})

eventBus.emit('login','lpq')
eventBus.off('login')
eventBus.off(['login','logout'])
console.log(eventBus)
```

#### 13.3.16 实现消息订阅与发布PubSub

```js
const PubSub={
  id:1,
  callbacks:{
    // chanelName:{
    //   token:fn
    // }
  }
}

/**
 * @description 订阅消息
 * @param {String} chanelName 
 * @param {Function} callback 
 */
PubSub.subscribe=function (chanelName,callback) {
  const token='token_'+this.id++
  if(this.callbacks[chanelName]){
    this.callbacks[chanelName][token]=callback
  }else{
    this.callbacks[chanelName]={
      [token]:callback
    }
  }
  return token//返回唯一pid
}

/**
 * @description 发布消息
 * @param {String} chanelName 
 * @param {...any} data 
 */
PubSub.publish=function (chanelName,data) {
if(this.callbacks[chanelName]){
  Object.values( this.callbacks[chanelName]).forEach(callback=>{
    callback(data)//执行回调
  })
}
}

/**
 * @description 1.如果没有传值，则清除所有订阅 2、传入pid清除pid的订阅 3 传入chanelName 将chanelName下的所有订阅清除
 * @param {undefined | String } flag 
 */
PubSub.unsubscribe=function (flag) {
  if(!flag){
    this.callbacks={}
  }else if(typeof flag==='string'){
    if(!flag.indexOf('token_')){
    const callbackObj= Object.values(this.callbacks).find(curObj=> curObj.hasOwnProperty(flag))
    //可能为null,有则删除
    if(callbackObj){
      delete callbackObj[flag]
    }
    }else{
      delete this.callbacks[flag]
    }
  }
}

//先订阅后发布
const pid1=PubSub.subscribe('login',(data)=>{
  console.log(data)
})
const pid2=PubSub.subscribe('login',(data)=>{
  console.log(data)
})
const pid3=PubSub.subscribe('logout',(data)=>{
  console.log(data)
})
// PubSub.unsubscribe('login')
PubSub.publish('login',"lpq-login")
PubSub.publish('logout',"lpq - logout")

```

#### 13.3.17 封装axios()函数

axios.js:

```js

  function axios({method, url, params, data}){
  //方法转化大写
  method = method.toUpperCase();
  //返回值
  return new Promise((resolve, reject) => {
      //四步骤
      //1. 创建对象
      const xhr = new XMLHttpRequest();
      //2. 初始化
      //处理 params 对象 a=100&b=200
      let str = '';
      for(let k in params){
          str += `${k}=${params[k]}&`;
      }
      str = str.slice(0, -1);
      xhr.open(method, url+'?'+str);
      //3. 发送
      if(method === 'POST' || method === 'PUT' || method === 'DELETE'){
          //Content-type mime类型设置
          xhr.setRequestHeader('Content-type','application/json');
          //设置请求体
          xhr.send(JSON.stringify(data));
      }else{
          xhr.send();
      }
      //设置响应结果的类型为 JSON
      xhr.responseType = 'json';
      //4. 处理结果
      xhr.onreadystatechange = function(){
          //
          if(xhr.readyState === 4){
              //判断响应状态码 2xx
              if(xhr.status >= 200 && xhr.status < 300){
                  //成功的状态
                  resolve({
                      status: xhr.status,
                      message: xhr.statusText,
                      body: xhr.response
                  });
              }else{
                  reject(new Error('请求失败, 失败的状态码为' + xhr.status));
              }
          }
      }

  });
}

axios.get = function(url, options){
  //发送 AJAX 请求 GET
  let config = Object.assign(options, {method:'GET', url: url});
 console.log(config)
  return axios(config);
}

axios.post = function(url, options){
  //发送 AJAX 请求 POST
  let config = Object.assign(options, {method:'POST', url: url});
  return axios(config);
}

axios.put = function(url, options){
  //发送 AJAX 请求 PUT
  let config = Object.assign(options, {method:'PUT', url: url});
  return axios(config);
}

axios.delete = function(url, options){
  //发送 AJAX 请求 DELETE
  let config = Object.assign(options, {method:'delete', url: url});
  return axios(config);
}


```

axios.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <script src="./axios.js"></script>
  <title>封装axios</title>
</head>
<body>
  <script>
    // axios({
    //   //请求类型
    //   method:"POST",
    //   //请求对的url
    //   url:'https://api.apiopen.top/getJoke',
    //   //请求参数
    //   params:{
    //     a:100,
    //     b:200
    //   },
    //   data:{
    //     c:300,
    //     d:400
    //   }
    // }).then((response) => {
    //   console.log(response)
    // }).catch((errReason) => {
    //   console.log(errReason)
    // });
   
  axios.get('https://api.apiopen.top/getJoke',{params: {
  a:100,
  b:200
}
}).then((response) => {
  console.log(response)
}).catch((errReason) => {
  console.log(errReason)
});
  </script>
</body>
</html>
```

#### 13.3.18 制作npm包

1. 创建工具包项目，在项目中新建webpack.config.js（指定打包位置极其文件名，后续引入打包dist文件时使用）

   ```js
   //引入 nodeJS 内置模块 path
   const path = require('path');
   
   module.exports = {
     // 模式
     mode: 'development', // 也可以使用 production
     // 入口
     entry: './src/index.js', 
     // 出口
     output: {
       // 打包文件夹
       path: path.resolve(__dirname, 'dist'),
       // 打包文件
       filename: 'atguigu-utils.js', 
       // 向外暴露的对象的名称
       library: 'utils',
       // 打包生成库可以通过esm/commonjs/reqirejs的语法引入
       libraryTarget: 'umd', 
     }
   }
   ```

   

2. 完善package.json

   ```json
   {
     "name": "mine-own-util",
     "version": "1.0.0",
     "description": "",
     "main": "./dist/atguigu-utils.js",
     "directories": {
       "test": "test"
     },
     "scripts": {
       "test": "echo \"Error: no test specified\" && exit 1",
        "build:watch": "webpack --watch"
     },
     "keywords": [
       "atguigu",
       "utils",
       "array",
       "object",
       "function",
       "string",
       "axios",
       "event-bus",
       "pub-sub"
     ],
     "author": "",
     "license": "ISC",
     "dependencies": {
       "webpack": "^5.16.0",
       "webpack-cli": "^4.4.0"
     }
   }
   
   ```

   - name:必须是在npm仓库中唯一的名称
   - main:b必须指定为打包生成的js文件
   - keywords:用于可以在npm仓库中能搜到的关键字

3. npm配置

   ```json
   npm配置中央仓库不能是淘宝仓库
   发布前必须执行：npm config set registry https://registry.npmjs.org/
   不用发布时：npm config set registry https://registry.npm.taobao.org/
   查看配置：npm config list
   ```

4. 注册npm中央仓库账号

   ```
   注册地址：https://www.npmjs.com/
   关键信息：用户名、密码、邮箱验证
   发布：npm publish
   修改代码后，更新npm包需要更改package.jsson中的版本号： "version": "1.0.0"---> "version": "1.0.1"
   ```

5. 删除npm包

   ```json
   删除npm包：执行npm unpublish --force
   注：必须在72h内删除，否则不能再删除
   ```

### 13.4 利用Set()集合处理数组

```js
/**
 * @description 数组去重
 * @param {Array} array 
 */
function unique(array) {
  return [...new Set(array)]
}

/**
 * @description 数组交集
 * @param {Array} arr1 
 * @param {Array} arr2 
 */
function mixedArray(arr1, arr2) {
  return [...new Set(arr1)].filter(item => arr2.includes(item))
}

/**
 * @description 两数组取并集--数据不重复
 * @param {Array} arr1 
 * @param {Array} arr2 
 */
function unionArray(arr1, arr2) {
  return [...new Set([...new Set(arr1), ...new Set(arr2)])]
}

/**
 * @description 数组差集
 * @param {Array} arr1 
 * @param {Array} arr2 
 */
function diffArray(arr1, arr2) {
  return [...new Set(arr1)].filter(item => !arr2.includes(item))

}

const arr2 = [1, 2, 2, 4]
const arr1 = [1, 2, 3, 4, 4, 10, 11]
// console.log(unique(arr1))
// console.log(mixedArray(arr1, arr2))
// console.log(unionArray(arr1, arr2))
console.log(diffArray(arr1, arr2))

```

