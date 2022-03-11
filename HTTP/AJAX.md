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
      //设置头信息（预定义头信息）
      xhr.setRequestHeader('Content-type','application/x-www-form-urlencoded')
      //设置自定义头部信息,浏览器会报错，需要在server端改为app.all
      xhr.setRequestHeader('name','lpq')
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
// app.post('/server',(req,res)=>{
//   res.setHeader('Access-Control-Allow-Origin','*')
//   res.send('send ajax post!')
// })
//所有请求都会接受
app.all('/server',(req,res)=>{
  res.setHeader('Access-Control-Allow-Origin','*')
  res.setHeader('Access-Control-Allow-Headers','*')
  res.send('send ajax post!')
})
app.listen('8000',()=>{
  console.log("服务已启动！")
})
```

## 3.从服务器接受JSON数据

### 3.1 JsonData.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>接受JSON数据</title>
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
    window.onkeydown=()=>{
      //1.创建xhr对象
      const xhr=new XMLHttpRequest()
      //设置响应数据的格式
      xhr.responseType='json'
      //2.初始化请求方法 url
      xhr.open('POST','http://localhost:8000/json-server')
      //3.发送请求
      xhr.send()//可在send中发送请求体
      //4. 处理服务器响应数据
      xhr.onreadystatechange=()=>{
        if(xhr.readyState===4){
          if(xhr.status>=200&&xhr.status<300){
            console.log(xhr.response.name)
            ele.innerHTML=xhr.response.name
          }
        }
      }
    }
  </script>
</body>
</html>
```

### 3.2 Server.js

```js
const  express=require('express')
const app=express()
app.get('/server',(req,res)=>{
  res.setHeader('Access-Control-Allow-Origin','*')
  res.send('hello express!')
})
// app.post('/server',(req,res)=>{
//   res.setHeader('Access-Control-Allow-Origin','*')
//   res.send('send ajax post!')
// })
//所有请求都会接受
app.all('/server',(req,res)=>{
  res.setHeader('Access-Control-Allow-Origin','*')
  res.setHeader('Access-Control-Allow-Headers','*')
  res.send('send ajax post!')
})

app.all('/json-server',(req,res)=>{
  res.setHeader('Access-Control-Allow-origin','*')
  // res.setHeader('Access-Control-Allow-Headers','*')
  const data={
    name:'lpq'
  }
  // res.send('send ajax json!')
  res.send(JSON.stringify(data))//send方法只能发送字符串
})
app.listen('8000',()=>{
  console.log("服务已启动！")
})
```

## 4.使用nodemon对服务热启动

安装：npm install --save-dev nodemon

执行：nodemon xxx.js

解决在window系统执行报错问题:

![image-20210914232034429](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210914232034429.png)

解决方法：

1. window+r 输入powershell
2. 执行：set-ExecutionPolicy RemoteSigned![image-20210914232145760](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210914232145760.png)

3.查看执行策略：get-ExecutionPolicy,再重新启动即可

![image-20210914232322884](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210914232322884.png)

## 5 解决IE缓存问题

老版本IE会有IE缓存问题，即服务数据改变时，再次请求数据拿到的还是旧数据。

解决：

### 5.1 IECatch.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>IE缓存问题</title>
  <style>
    #result{
      height: 200px;
      width: 200px;
      border: solid 1px green;
    }
  </style>
</head>
<body>
  <button>点我发送请求</button>
  <div id="result"></div>
  <script>
    const btn=document.getElementsByTagName('button')[0]
    const result=document.querySelector('#result')
    btn.addEventListener('click',()=>{
      console.log("aaaaa")
      const xhr=new XMLHttpRequest()
      // xhr.open('GET','http://localhost:8000/ie-catch')
      xhr.open('GET',`http://localhost:8000/ie-catch?t=${Date.now()}`)//用当前时间戳来标记不同标识以解决IE缓存问题
      xhr.responseType='json'
      xhr.send()
      xhr.onreadystatechange=()=>{
        if(xhr.readyState===4){
          if(xhr.status>200&&xhr.status<300){
            result.innerHTML=xhr.response
          }
        }
      }
    })
  </script>
</body>
</html>
```

5.2 Server.js

```js
const  express=require('express')
const app=express()
app.get('/server',(req,res)=>{
  res.setHeader('Access-Control-Allow-Origin','*')
  res.send('hello express!')
})
// app.post('/server',(req,res)=>{
//   res.setHeader('Access-Control-Allow-Origin','*')
//   res.send('send ajax post!')
// })
//所有请求都会接受
app.all('/server',(req,res)=>{
  res.setHeader('Access-Control-Allow-Origin','*')
  res.setHeader('Access-Control-Allow-Headers','*')
  res.send('send ajax post!')
})

app.all('/json-server',(req,res)=>{
  res.setHeader('Access-Control-Allow-origin','*')
  // res.setHeader('Access-Control-Allow-Headers','*')
  const data={
    name:'lpq'
  }
  // res.send('send ajax json!')
  res.send(JSON.stringify(data))//send方法只能发送字符串
})

//IE浏览器缓存问题
app.get('/ie-catch',(req,res)=>{
  res.setHeader('Access-Control-Allow-origin','*')
  res.send('ie Catch4')

})
app.listen('8000',()=>{
  console.log("服务已启动！")
})
```

## 6.延时设置及友好提醒

### 6.1 Delay.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>IE缓存问题</title>
  <style>
    #result{
      height: 200px;
      width: 200px;
      border: solid 1px green;
    }
  </style>
</head>
<body>
  <button>点我发送请求</button>
  <div id="result"></div>
  <script>
    const btn=document.getElementsByTagName('button')[0]
    const result=document.querySelector('#result')
    btn.addEventListener('click',()=>{
      console.log("aaaaa")
      const xhr=new XMLHttpRequest()
      //添加延迟提示
      xhr.timeout=2000
      xhr.ontimeout=()=>{
        alert("请求超时，请稍后重试")
      }
      xhr.onerror=()=>{
        alert('网络异常，请检查网络是否正常！')
      }
      xhr.open('GET','http://localhost:8000/delay')
      xhr.responseType='json'
      xhr.send()
      xhr.onreadystatechange=()=>{
        if(xhr.readyState===4){
          if(xhr.status>200&&xhr.status<300){
            result.innerHTML=xhr.response
          }
        }
      }
    })
  </script>
</body>

</html>
```

### 6.2 Server.js

```js
const  express=require('express')
const app=express()
app.get('/server',(req,res)=>{
  res.setHeader('Access-Control-Allow-Origin','*')
  res.send('hello express!')
})
// app.post('/server',(req,res)=>{
//   res.setHeader('Access-Control-Allow-Origin','*')
//   res.send('send ajax post!')
// })
//所有请求都会接受
app.all('/server',(req,res)=>{
  res.setHeader('Access-Control-Allow-Origin','*')
  res.setHeader('Access-Control-Allow-Headers','*')
  res.send('send ajax post!')
})

app.all('/json-server',(req,res)=>{
  res.setHeader('Access-Control-Allow-origin','*')
  // res.setHeader('Access-Control-Allow-Headers','*')
  const data={
    name:'lpq'
  }
  // res.send('send ajax json!')
  res.send(JSON.stringify(data))//send方法只能发送字符串
})

//IE浏览器缓存问题
app.get('/ie-catch',(req,res)=>{
  res.setHeader('Access-Control-Allow-origin','*')
  res.send('ie Catch4')

})
//延迟响应
app.get('/delay',(req,res)=>{
  res.setHeader('Access-Control-Allow-origin','*')
  setTimeout(()=>{
    res.send('延时响应测试')
  },3000)
})
app.listen('8000',()=>{
  console.log("服务已启动！")
})
```

## 7.取消发送ajax请求

### 7.1 CancelReq.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>取消ajax请求</title>
</head>
<body>
  <button>发送请求</button>
  <button>取消请求</button>
  <script>
    const btns=document.querySelectorAll('button')
    let xhr
    btns[0].onclick=()=>{
       xhr=new XMLHttpRequest()
      xhr.open('GET','http://localhost:8000/delay')
      xhr.send()
      // ......
    }
    //取消发送请求
    btns[1].onclick=()=>{
      xhr.abort()
    }

  </script>
</body>
</html>
```

## 8.解决重复高频向服务器发送ajax请求问题

### 8.1 RepeatRequest.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>解决ajax重复发请求的问题</title>
</head>
<body>
  <button>发送请求</button>
  <script>
    const btns=document.querySelectorAll('button')
    let xhr=null
    let isSending=false//是否正在发送ajax请求
    btns[0].onclick=()=>{
      if(isSending){
        xhr.abort()//如果正在发送ajax请求则取消上次请求 
      }
      xhr=new XMLHttpRequest()
      isSending=true
      xhr.open('GET','http://localhost:8000/delay')
      xhr.send()
      xhr.onreadystatechange=()=>{
        if(xhr.readyState===4){
          isSending=fa
          if(xhr.status>=200&&xhr.status<300){
            console.log(xhr.response)
          }
        }
      }
    }
  </script>
</body>
</html>
```

### 8.2 Server.js

```js
const  express=require('express')
const app=express()
app.get('/server',(req,res)=>{
  res.setHeader('Access-Control-Allow-Origin','*')
  res.send('hello express!')
})
// app.post('/server',(req,res)=>{
//   res.setHeader('Access-Control-Allow-Origin','*')
//   res.send('send ajax post!')
// })
//所有请求都会接受
app.all('/server',(req,res)=>{
  res.setHeader('Access-Control-Allow-Origin','*')
  res.setHeader('Access-Control-Allow-Headers','*')
  res.send('send ajax post!')
})

app.all('/json-server',(req,res)=>{
  res.setHeader('Access-Control-Allow-origin','*')
  // res.setHeader('Access-Control-Allow-Headers','*')
  const data={
    name:'lpq'
  }
  // res.send('send ajax json!')
  res.send(JSON.stringify(data))//send方法只能发送字符串
})

//IE浏览器缓存问题
app.get('/ie-catch',(req,res)=>{
  res.setHeader('Access-Control-Allow-origin','*')
  res.send('ie Catch4')

})
//延迟响应
app.get('/delay',(req,res)=>{
  res.setHeader('Access-Control-Allow-origin','*')
  setTimeout(()=>{
    res.send('延时响应测试')
  },3000)
})
app.listen('8000',()=>{
  console.log("服务已启动！")
})
```

### 8.3 验证结果

![image-20210915001729149](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210915001729149.png)

## 9.axios发送get () post()请求

### 9.1 axios.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <script crossorigin="anonymous"  src="https://cdn.bootcdn.net/ajax/libs/axios/0.21.1/axios.min.js"></script>
  <title>axios</title>
</head>
<body>
  <button>GET</button>
  <button>POST</button>
  <button>AJAX</button>
  <script>
    const btns=document.querySelectorAll('button')
    axios.defaults.baseURL='http://localhost:8000'
    //get
    btns[0].onclick=()=>{
      axios.get('/axios-server',{params:{username:"lpq",age:18},headers:{name:'lpq1',age:19}}).then((value)=>{
        console.log(value)

      })
    }
    //post
    btns[1].onclick=()=>{
      axios.post('/axios-server',{username:"lpq",age:18},{params:{name:'lpq1',age:19},headers:{height:169,weight:60}}).then(value=>console.log(value.data))
    }
    //通用方法
    btns[2].onclick=()=>{
      axios({
        //请求方法
        method:'POST',
        //url
        url:'/axios-server',
        //请求行
        params:{
          level:30,
          vip:0
        },
        //请求头
        headers:{
          a:100,
          b:200
        },
        //请求体
        data:{
          name:'lpq',
          age:18
        }
      }).then(res=>console.log(res))//响应结果
    }
  </script>
</body>
</html>
```

### 9.2 Server.js

```js
const  express=require('express')
const app=express()
app.get('/server',(req,res)=>{
  res.setHeader('Access-Control-Allow-Origin','*')
  res.send('hello express!')
})
// app.post('/server',(req,res)=>{
//   res.setHeader('Access-Control-Allow-Origin','*')
//   res.send('send ajax post!')
// })
//所有请求都会接受
app.all('/server',(req,res)=>{
  res.setHeader('Access-Control-Allow-Origin','*')
  res.setHeader('Access-Control-Allow-Headers','*')
  res.send('send ajax post!')
})

app.all('/axios-server',(req,res)=>{
  //允许跨域
  res.setHeader('Access-Control-Allow-Origin','*')
  res.setHeader('Access-Control-Allow-Headers','*')
  const data={
    name:'lpq',
    age:"18"
  }
  res.send(JSON.stringify(data))
})
app.listen('8000',()=>{
  console.log("服务已启动！")
})
```

## 10.利用fetch()函数发送请求

### 10.1 fetch.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>fetch函数发送请求</title>
</head>
<body>
  <button>fetch函数发送请求</button>
  <script>
    const btn=document.querySelector('button')
    const url='http://localhost:8000/fetch-server?name=lpq'
    btn.onclick=()=>{
      fetch(url,{
        method:'POST',
        headers:{
          a:100,
          b:200
        },
        body:'username=admin&password=admin'
      }).then(response=>{
        console.log(response)
        return response.json()
      }).then(response=>{
        console.log(response)
      })
    }
  </script>
</body>
</html>
```

### 10.2 Server.js

```js
const  express=require('express')
const app=express()
app.get('/server',(req,res)=>{
  res.setHeader('Access-Control-Allow-Origin','*')
  res.send('hello express!')
})
// app.post('/server',(req,res)=>{
//   res.setHeader('Access-Control-Allow-Origin','*')
//   res.send('send ajax post!')
// })
//所有请求都会接受
app.all('/server',(req,res)=>{
  res.setHeader('Access-Control-Allow-Origin','*')
  res.setHeader('Access-Control-Allow-Headers','*')
  res.send('send ajax post!')
})

app.all('/axios-server',(req,res)=>{
  //允许跨域
  res.setHeader('Access-Control-Allow-Origin','*')
  res.setHeader('Access-Control-Allow-Headers','*')
  const data={
    name:'lpq',
    age:"18"
  }
  res.send(JSON.stringify(data))
})

//fetch函数
app.all('/fetch-server',(req,res)=>{
  //允许跨域
  res.setHeader('Access-Control-Allow-Origin','*')
  res.setHeader('Access-Control-Allow-Headers','*')
  const data={
    name:'lpq',
    age:"18"
  }
  res.send(JSON.stringify(data))
})

app.listen('8000',()=>{
  console.log("服务已启动！")
})
```

## 11.跨域问题

条件：请求方与响应方的协议、域名、端口号必须同时一致称为同源策略，否则就会跨域

### 11.1解决跨域---JSONP

利用script标签天然可跨域的优势进行处理跨域问题。

在src中填写服务端url地址，在服务端返回js代码，在浏览器中执行

缺点：只能处理get()请求的跨域问题。

#### 11.1.1 JSONP.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>jsonp解决跨域</title>
  <style>
    #result{
      height: 300px;
      width: 200px;
      border: solid;
    }
  </style>
</head>
<body>
  <!-- <button>点我发送请求</button> -->
  <div id="result"></div>
  <script>
    function handle(data){
      const result=document.getElementById('result')
      result.innerHTML=data.name
    }
  </script>
  <script src="http://localhost:8000/jsonp-server"></script>
</body>
</html>
```

#### 11.1.2 Server.js

```js
const  express=require('express')
const app=express()
app.get('/server',(req,res)=>{
  res.setHeader('Access-Control-Allow-Origin','*')
  res.send('hello express!')
})
// app.post('/server',(req,res)=>{
//   res.setHeader('Access-Control-Allow-Origin','*')
//   res.send('send ajax post!')
// })
//所有请求都会接受
app.all('/server',(req,res)=>{
  res.setHeader('Access-Control-Allow-Origin','*')
  res.setHeader('Access-Control-Allow-Headers','*')
  res.send('send ajax post!')
})

app.all('/axios-server',(req,res)=>{
  //允许跨域
  res.setHeader('Access-Control-Allow-Origin','*')
  res.setHeader('Access-Control-Allow-Headers','*')
  const data={
    name:'lpq',
    age:"18"
  }
  res.send(JSON.stringify(data))
})

//fetch函数
app.all('/fetch-server',(req,res)=>{
  //允许跨域
  res.setHeader('Access-Control-Allow-Origin','*')
  res.setHeader('Access-Control-Allow-Headers','*')
  const data={
    name:'lpq',
    age:"18"
  }
  res.send(JSON.stringify(data))
})

//jsonp服务
app.all('/jsonp-server',(req,res)=>{
  //允许跨域
  res.setHeader('Access-Control-Allow-Origin','*')
  res.setHeader('Access-Control-Allow-Headers','*')
  const data={
    name:'lpq',
    age:"18"
  }
  const str=JSON.stringify(data)
  res.end(`handle(${str})`)//返回函数调用(数据作为参数)
})

app.listen('8000',()=>{
  console.log("服务已启动！")
})
```

#### 11.1.3 结果展示

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210915230152967.png" alt="image-20210915230152967" style="zoom:150%;" />

### 11.2  CORS

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210915231234040.png" alt="image-20210915231234040" style="zoom:150%;" />

#### 11.2.1 CORS.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>CORS解决跨域</title>
  <style>
    #result{
      height: 200px;
      width: 200px;
      border: solid;
    }
  </style>
</head>
<body>
  <div id="result"></div>
  <button>发送请求</button>
  <script>
    const result=document.getElementById('result')
    const  btn=document.querySelector('button')
    btn.onclick=()=>{
      const xhr=new XMLHttpRequest()
      xhr.open('POST','http://localhost:8000/cors-server')
      xhr.send()
      xhr.responseType='json'
      xhr.onreadystatechange=()=>{
        if(xhr.readyState===4){
          if(xhr.status>=200&&xhr.status<300){
           result.innerHTML= xhr.response.name
           console.log(xhr.response)
          }
        }
      }
    }
  </script>
</body>
</html>
```

#### 11.2.2  服务端设置头部信息-Server.js

```js
const  express=require('express')
const app=express()
app.get('/server',(req,res)=>{
  res.setHeader('Access-Control-Allow-Origin','*')
  res.send('hello express!')
})
// app.post('/server',(req,res)=>{
//   res.setHeader('Access-Control-Allow-Origin','*')
//   res.send('send ajax post!')
// })
//所有请求都会接受
app.all('/server',(req,res)=>{
  res.setHeader('Access-Control-Allow-Origin','*')
  res.setHeader('Access-Control-Allow-Headers','*')
  res.send('send ajax post!')
})

app.all('/axios-server',(req,res)=>{
  //允许跨域
  res.setHeader('Access-Control-Allow-Origin','*')
  res.setHeader('Access-Control-Allow-Headers','*')
  const data={
    name:'lpq',
    age:"18"
  }
  res.send(JSON.stringify(data))
})

//fetch函数
app.all('/fetch-server',(req,res)=>{
  //允许跨域
  res.setHeader('Access-Control-Allow-Origin','*')
  res.setHeader('Access-Control-Allow-Headers','*')
  const data={
    name:'lpq',
    age:"18"
  }
  res.send(JSON.stringify(data))
})

//jsonp服务
app.all('/jsonp-server',(req,res)=>{
  //允许跨域
  res.setHeader('Access-Control-Allow-Origin','*')
  res.setHeader('Access-Control-Allow-Headers','*')
  const data={
    name:'lpq',
    age:"18"
  }
  const str=JSON.stringify(data)
  res.end(`handle(${str})`)
})

//CORS跨域
app.all('/cors-server',(req,res)=>{
  //允许跨域
  res.setHeader('Access-Control-Allow-Origin','*')//允许所有网页
  res.setHeader('Access-Control-Allow-Headers','*')//允许所有头部信息
  res.setHeader('Access-Control-Allow-Method','*')//允许所有请求方法
  const data={
    name:'lpq',
    age:"18"
  }
  res.send(JSON.stringify(data))
})
app.listen('8000',()=>{
  console.log("服务已启动！")
})
```

