## 1.利用express向服务器发送ajax-get请求

### 1.1 GET.html

[Express基本使用](https://www.expressjs.com.cn/starter/hello-world.html)

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>测试AJAX的get请求</title>
  <style>
    #result{
      height: 200px;
      width: 200px;
      border: solid;
    }
  </style>
</head>
<body>
  <button class="button">点击我发送请求</button>
  <div id="result">
  </div>
  <script>
    const btn =document.getElementsByTagName('button')[0]
    const id=document.getElementById('result')
    btn.onclick=()=>{
      //1.创建xhr对象
     const xhr=new XMLHttpRequest()
     //2.初始化  请求方法 url
     xhr.open('GET','http://localhost:8000/server')
     //3.发送请求
     xhr.send()
     //4.绑定事件，获取响应结果
     xhr.onreadystatechange=()=>{
//xhr.readyState是xhr中的属性表示状态：
//0(未初始化) 1(已经初始化-open()方法调用完毕) 2(事件绑定完毕-send方法调用完毕) 3(服务端返回部分结果) 4(服务端响应完全返回)
       if(xhr.readyState===4){
       if(xhr.status>=200 && xhr.status<300){
         //响应结果：行 头 空行 体
         console.log(xhr.status,xhr.statusText,xhr.readyState)
         console.log(xhr.getAllResponseHeaders(),xhr.response)
        id.innerHTML=xhr.statusText
       }
      }
     }
    }
  </script>
</body>
</html>
```

注：xhr.readyState是xhr中的属性表示状态（5个状态码）：

- 0。未初始化，正在创建xhr：new XMLHttpRequest()；
- 1。已经初始化-open()方法调用完毕；
- 2。事件绑定完毕-send方法调用完毕；
- 3。服务端返回部分结果；
- 4。服务端响应完全返回；

### 1.2  Server.js

```js
const  express=require('express')
const app=express()
app.get('/server',(req,res)=>{
  res.setHeader('Access-Control-Allow-Origin','*')
  res.send('hello express!')
})
app.listen('8000',()=>{
  console.log("服务已启动！")
})
```

## 2.利用express向服务器发送ajax-post请求

### 2.1 POST.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>AJAX发送POST请求</title>
  <style>
    #result{
      height: 200px;
      width: 200px;
      border: solid;
    }
  </style>
</head>
<body>
  <div id="result">

  </div>
  <script>
    const ele=document.getElementById('result')
    ele.addEventListener('mouseover',()=>{
      console.log("1111")
      //1.创建xhr对象
      const xhr=new XMLHttpRequest()
      //2.初始化 请求方法 url
      xhr.open('POST','http://localhost:8000/server')
      //3.绑定事件 发送请求
      xhr.send()
      //4.处理服务器返回的数据
      xhr.onreadystatechange=()=>{
        if(xhr.readyState===4){
          if(xhr.status>=200&&xhr.status<300){
            ele.innerHTML=xhr.response
          }
        }
      }
    })
    ele.addEventListener('mouseout',()=>{
      console.log("sss")
      ele.innerHTML=''
    })
  </script>
</body>
</html>
```

### 2.2 Server.js

```js
const  express=require('express')
const app=express()
app.get('/server',(req,res)=>{
  res.setHeader('Access-Control-Allow-Origin','*')
  res.send('hello express!')
})
app.post('/server',(req,res)=>{
  res.setHeader('Access-Control-Allow-Origin','*')
  res.send('send ajax post!')
})
app.listen('8000',()=>{
  console.log("服务已启动！")
})
```

