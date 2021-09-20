## SCMAScript语法汇总

## 1. 私有属性(ES11)

code:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>私有属性</title>
</head>
<body>
    <script>
        class Person{
            //公有属性
            name;
            //私有属性
            #age;
            #weight;
            //构造方法
            constructor(name, age, weight){
                this.name = name;
                this.#age = age;
                this.#weight = weight;
            }

            intro(){
                console.log(this.name);
                console.log(this.#age);
                console.log(this.#weight);
            }
        }

        //实例化
        const girl = new Person('晓红', 18, '45kg');

        // console.log(girl.name);
        // console.log(girl.#age);
        // console.log(girl.#weight);

        girl.intro();
    </script>
</body>
</html>
```

## 2. Promise.allSettled(ES11)

code:

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Promise.allSettled</title>
</head>
<body>
    <script>
        //声明两个promise对象
        const p1 = new Promise((resolve, reject)=>{
            setTimeout(()=>{
                resolve('商品数据 - 1');
            },1000)
        });

        const p2 = new Promise((resolve, reject)=>{
            setTimeout(()=>{
                resolve('商品数据 - 2');
                // reject('出错啦!');
            },1000)
        });

        //调用 allsettled 方法
        // const result = Promise.allSettled([p1, p2]);//返回的promise的状态都是成功的，包含成功和失败的值
        
        // const res = Promise.all([p1, p2]);//当所有返回的promise回调都成功时才返回成功的结果，否则就是失败

        console.log(res);

    </script>
</body>
</html>
```

## 3. 批量提取字符串String.prototype.matchAll(ES11)

code:

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>String.prototype.matchAll</title>
</head>
<body>
    <script>
        let str = `<ul>
            <li>
                <a>肖生克的救赎</a>
                <p>上映日期: 1994-09-10</p>
            </li>
            <li>
                <a>阿甘正传</a>
                <p>上映日期: 1994-07-06</p>
            </li>
        </ul>`;

        //声明正则
        const reg = /<li>.*?<a>(.*?)<\/a>.*?<p>(.*?)<\/p>/sg

        //调用方法
        const result = str.matchAll(reg);//返回是一个可迭代对象

        // for(let v of result){
        //     console.log(v);
        // }

        const arr = [...result];

        console.log(arr);
        
    </script>
</body>
</html>
```

## 4.可选链操作符?.(ES11)

code:

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>可选链操作符</title>
</head>
<body>
  <script>
    function test(config) {
  // const dbHost=config&&config.db&&config.db.host//常规写法
  const dbHost=config?.db?.host//可选链操作符，等同于：const dbHost=config&&config.db&&config.db.host
  console.log(config.db.host)
}

const config={
  // db:{
  //   host:"192.168.1.1",
  //   username:'lpq',
  //   password:"admin"
  // },
  catch:{
    host:"190.168.2.1",
    username:"admin",
    password:"admin"
  }
}
test(config)
  </script>
</body>
</html>
```



## 5.动态导入(ES11)import(path).then(module=>{xxxxx})

code:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>动态导入</title>
  <script src="./hello.js" type="module"></script>
</head>
<body>
  <button id="btn">点击</button>
  <script>
    // import * as module from './hello'//静态导入
    const btn=document.querySelector('button')
    btn.onclick= function alertHello(){
      // console.log(module)
      //动态导入
      import("./hello").then(module=>{
        module.alertHello()
      })
    }
   

  </script>
</body>
</html>
```

## 6.大整型(ES11)

code:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>BigInt</title>
</head>
<body>
    <script>
        //大整形
        // let n = 521n;
        // console.log(n, typeof(n));

        //函数
        // let n = 123;
        // console.log(BigInt(n));
        // console.log(BigInt(1.2));

        //大数值运算
        let max = Number.MAX_SAFE_INTEGER;//最大安全整数
        console.log(max);
        console.log(max + 1);
        console.log(max + 2);

        console.log(BigInt(max))
        console.log(BigInt(max) + BigInt(1))//大整型不能与其它类型运算
        console.log(BigInt(max) + BigInt(2))
    </script>
</body>
</html>
```

## 7.globalThis(ES11)

code:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>globalThis</title>
</head>
<body>
    <script>
        console.log(globalThis);//始终指向全局对象---无论是浏览器还是node.js
    </script>
</body>
</html>
```

## 8.rest参数(ES6)

code:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>rest参数</title>
</head>
<body>
    <script>
        // ES6 引入 rest 参数，用于获取函数的实参，用来代替 arguments
        // ES5 获取实参的方式
        // function date(){
        //     console.log(arguments);
        // }
        // date('白芷','阿娇','思慧');

        // rest 参数
        // function date(...args){
        //     console.log(args);// filter some every map 
        // }
        // date('阿娇','柏芝','思慧');

        // rest 参数必须要放到参数最后
        // function fn(a,b,...args){
        //     console.log(a);
        //     console.log(b);
        //     console.log(args);
        // }
        // fn(1,2,3,4,5,6);

    </script>
</body>
</html>
```

## 9.symbol(ES6)

### 9.1 symbol创建

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>symbol</title>
</head>
<body>
    <script>
        //创建Symbol
        let s = Symbol();
        // console.log(s, typeof s);
        let s2 = Symbol('尚硅谷');
        let s3 = Symbol('尚硅谷');
        //Symbol.for 创建
        let s4 = Symbol.for('尚硅谷');
        let s5 = Symbol.for('尚硅谷');

        //不能与其他数据进行运算
        //    let result = s + 100;
        //    let result = s > 100;
        //    let result = s + s;

        // USONB  you are so niubility 
        // u  undefined
        // s  string  symbol
        // o  object
        // n  null number
        // b  boolean

    </script>
</body>
</html> 
```

### 9.2symbol创建对象属性

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Symbol 创建对象属性</title>
</head>
<body>
    <script>
        //向对象中添加方法 up down
        let game = {
            name:'俄罗斯方块',
            up: function(){},
            down: function(){}
        };
        
        //声明一个对象
        // let methods = {
        //     up: Symbol(),
        //     down: Symbol()
        // };

        // game[methods.up] = function(){
        //     console.log("我可以改变形状");
        // }

        // game[methods.down] = function(){
        //     console.log("我可以快速下降!!");
        // }

        // console.log(game);

        //
        let youxi = {
            name:"狼人杀",
            [Symbol('say')]: function(){
                console.log("我可以发言")
            },
            [Symbol('zibao')]: function(){
                console.log('我可以自爆');
            }
        }

        console.log(youxi)

        
    </script>
</body>
</html>
```

### 9.3 Symbol内置属性

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Symbol内置属性</title>
</head>
<body>
    <script>
        // class Person{
        //     static [Symbol.hasInstance](param){
        //         console.log(param);
        //         console.log("我被用来检测类型了");
        //         return false;
        //     }
        // }

        // let o = {};

        // console.log(o instanceof Person);

        // const arr = [1,2,3];
        // const arr2 = [4,5,6];
        // arr2[Symbol.isConcatSpreadable] = false;
        // console.log(arr.concat(arr2));
    </script>
</body>
</html>
```



## 10. 默认参数(ES6)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>函数参数默认值</title>
</head>
<body>
    <script>
        //ES6 允许给函数参数赋值初始值
        //1. 形参初始值 具有默认值的参数, 一般位置要靠后(潜规则)
        // function add(a,c=10,b) {
        //     return a + b + c;
        // }
        // let result = add(1,2);
        // console.log(result);

        //2. 与解构赋值结合
        function connect({host="127.0.0.1", username,password, port}){
            console.log(host)
            console.log(username)
            console.log(password)
            console.log(port)
        }
        connect({
            host: 'atguigu.com',
            username: 'root',
            password: 'root',
            port: 3306
        })
    </script>
</body>
</html>
```

## 11. 迭代器(ES6)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>迭代器</title>
</head>
<body>
    <script>
        //声明一个数组
        const xiyou = ['唐僧','孙悟空','猪八戒','沙僧'];

        //使用 for...of 遍历数组
        // for(let v of xiyou){
        //     console.log(v);
        // }

        let iterator = xiyou[Symbol.iterator]();

        //调用对象的next方法
        console.log(iterator.next());
        console.log(iterator.next());
        console.log(iterator.next());
        console.log(iterator.next());
        console.log(iterator.next());
    </script>
</body>
</html>
```

### 11.1迭代器自定义遍历对象

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>自定义遍历数据</title>
</head>

<body>
    <script>
        //声明一个对象
        const banji = {
            name: "终极一班",
            stus: [
                'xiaoming',
                'xiaoning',
                'xiaotian',
                'knight'
            ],
            [Symbol.iterator]() {
                //索引变量
                let index = 0;
                //
                let _this = this;
                return {
                    next: function () {
                        if (index < _this.stus.length) {
                            const result = { value: _this.stus[index], done: false };
                            //下标自增
                            index++;
                            //返回结果
                            return result;
                        }else{
                            return {value: undefined, done: true};
                        }
                    }
                };
            }
        }

        //遍历这个对象 
        for (let v of banji) {
            console.log(v);
        }
    </script>
</body>

</html>
```



## 12.对象方法扩展(ES6)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>对象方法扩展</title>
</head>
<body>
    <script>
        //1. Object.is 判断两个值是否完全相等 
        // console.log(Object.is(120, 120));// === 
        // console.log(Object.is(NaN, NaN));// === 
        // console.log(NaN === NaN);// === 

        //2. Object.assign 对象的合并
        // const config1 = {
        //     host: 'localhost',
        //     port: 3306,
        //     name: 'root',
        //     pass: 'root',
        //     test: 'test'
        // };
        // const config2 = {
        //     host: 'http://atguigu.com',
        //     port: 33060,
        //     name: 'atguigu.com',
        //     pass: 'iloveyou',
        //     test2: 'test2'
        // }
        // console.log(Object.assign(config1, config2));

        //3. Object.setPrototypeOf 设置原型对象  Object.getPrototypeof
        const school = {
            name: '尚硅谷'
        }
        const cities = {
            xiaoqu: ['北京','上海','深圳']
        }
        Object.setPrototypeOf(school, cities);
        console.log(Object.getPrototypeOf(school));
        console.log(school)
    </script>
</body>
</html>
```

## 13.箭头函数(ES6)

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>箭头函数</title>
</head>

<body>
    <script>
        // ES6 允许使用「箭头」（=>）定义函数。
        //声明一个函数
        // let fn = function(){

        // }
        // let fn = (a,b) => {
        //     return a + b;
        // }
        //调用函数
        // let result = fn(1, 2);
        // console.log(result);


        //1. this 是静态的. this 始终指向函数声明时所在作用域下的 this 的值
        function getName(){
            console.log(this.name);
        }
        let getName2 = () => {
            console.log(this.name);
        }

        //设置 window 对象的 name 属性
        window.name = '尚硅谷';
        const school = {
            name: "ATGUIGU"
        }

        //直接调用
        // getName();
        // getName2();

        //call 方法调用
        // getName.call(school);
        // getName2.call(school);

        //2. 不能作为构造实例化对象
        // let Person = (name, age) => {
        //     this.name = name;
        //     this.age = age;
        // }
        // let me = new Person('xiao',30);
        // console.log(me);

        //3. 不能使用 arguments 变量
        // let fn = () => {
        //     console.log(arguments);
        // }
        // fn(1,2,3);

        //4. 箭头函数的简写
            //1) 省略小括号, 当形参有且只有一个的时候
            // let add = n => {
            //     return n + n;
            // }
            // console.log(add(9));
            //2) 省略花括号, 当代码体只有一条语句的时候, 此时 return 必须省略
            // 而且语句的执行结果就是函数的返回值
            let pow = n => n * n;
                
            console.log(pow(8));

    </script>
</body>

</html>
```

### 13.1箭头函数实践

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>箭头函数实践</title>
    <style>
        div {
            width: 200px;
            height: 200px;
            background: #58a;
        }
    </style>
</head>
<body>
    <div id="ad"></div>
    <script>
        //需求-1  点击 div 2s 后颜色变成『粉色』
        //获取元素
        let ad = document.getElementById('ad');
        //绑定事件
        ad.addEventListener("click", function(){
            //保存 this 的值
            // let _this = this;
            //定时器
            setTimeout(() => {
                //修改背景颜色 this
                // console.log(this);
                // _this.style.background = 'pink';
                this.style.background = 'pink';
            }, 2000);
        });

        //需求-2  从数组中返回偶数的元素
        const arr = [1,6,9,10,100,25];
        // const result = arr.filter(function(item){
        //     if(item % 2 === 0){
        //         return true;
        //     }else{
        //         return false;
        //     }
        // });
        
        const result = arr.filter(item => item % 2 === 0);

        console.log(result);

        // 箭头函数适合与 this 无关的回调. 定时器, 数组的方法回调
        // 箭头函数不适合与 this 有关的回调.  事件回调, 对象的方法

    </script>
</body>

</html>
```

## 14.解构赋值(ES6)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>变量的解构赋值</title>
</head>
<body>
    <script>
        //ES6 允许按照一定模式从数组和对象中提取值，对变量进行赋值，
        //这被称为解构赋值。
        //1. 数组的结构
        // const F4 = ['小沈阳','刘能','赵四','宋小宝'];
        // let [xiao, liu, zhao, song] = F4;
        // console.log(xiao);
        // console.log(liu);
        // console.log(zhao);
        // console.log(song);

        //2. 对象的解构
        // const zhao = {
        //     name: '赵本山',
        //     age: '不详',
        //     xiaopin: function(){
        //         console.log("我可以演小品");
        //     }
        // };

        // let {name, age, xiaopin} = zhao;
        // console.log(name);
        // console.log(age);
        // console.log(xiaopin);
        // xiaopin();

        let {xiaopin} = zhao;
        xiaopin();


    </script>
</body>

</html>
```

## 15. 生成器函数(ES6)

```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>生成器</title>
</head>
<body>
    <script>    
        //生成器其实就是一个特殊的函数
        //异步编程  纯回调函数  node fs  ajax mongodb
        //函数代码的分隔符
        function * gen(){
            // console.log(111);
            yield '一只没有耳朵';
            // console.log(222);
            yield '一只没有尾部';
            // console.log(333);
            yield '真奇怪';
            // console.log(444);
        }

        let iterator = gen();
        console.log(iterator.next());
        console.log(iterator.next());
        console.log(iterator.next());
        console.log(iterator.next());

        //遍历
        // for(let v of gen()){
        //     console.log(v);
        // }

    </script>
</body>
</html>
```

## 16.生成器函数参数(ES6)

```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>生成器函数参数</title>
</head>
<body>
    <script>
        function * gen(arg){
            console.log(arg);
            let one = yield 111;
            console.log(one);
            let two = yield 222;
            console.log(two);
            let three = yield 333;
            console.log(three);
        }

        //执行获取迭代器对象
        let iterator = gen('AAA');
        console.log(iterator.next());
        //next方法可以传入实参
        console.log(iterator.next('BBB'));
        console.log(iterator.next('CCC'));
        console.log(iterator.next('DDD'));
        
    </script>
</body>
</html>
```

### 16.1生成器函数实例

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>生成器函数实例</title>
</head>

<body>
    <script>
        // 异步编程  文件操作 网络操作(ajax, request) 数据库操作
        // 1s 后控制台输出 111  2s后输出 222  3s后输出 333 
        // 回调地狱
        // setTimeout(() => {
        //     console.log(111);
        //     setTimeout(() => {
        //         console.log(222);
        //         setTimeout(() => {
        //             console.log(333);
        //         }, 3000);
        //     }, 2000);
        // }, 1000);

        function one(){
            setTimeout(()=>{
                console.log(111);
                iterator.next();
            },1000)
        }

        function two(){
            setTimeout(()=>{
                console.log(222);
                iterator.next();
            },2000)
        }

        function three(){
            setTimeout(()=>{
                console.log(333);
                iterator.next();
            },3000)
        }

        function * gen(){
            yield one();
            yield two();
            yield three();
        }

        //调用生成器函数
        let iterator = gen();
        iterator.next();

    </script>
</body>

</html>
```

### 16.2 生成器函数实例

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>生成器函数</title>
</head>
<body>
    <script>
        //模拟获取  用户数据  订单数据  商品数据 
        function getUsers(){
            setTimeout(()=>{
                let data = '用户数据';
                //调用 next 方法, 并且将数据传入
                iterator.next(data);
            }, 1000);
        }

        function getOrders(){
            setTimeout(()=>{
                let data = '订单数据';
                iterator.next(data);
            }, 1000)
        }

        function getGoods(){
            setTimeout(()=>{
                let data = '商品数据';
                iterator.next(data);
            }, 1000)
        }

        function * gen(){
            let users = yield getUsers();
            let orders = yield getOrders();
            let goods = yield getGoods();
        }

        //调用生成器函数
        let iterator = gen();
        iterator.next();
    </script>
</body>
</html>
```

## 17.数值扩展

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>数值扩展</title>
</head>
<body>
    <script>
        //0. Number.EPSILON 是 JavaScript 表示的最小精度
        //EPSILON 属性的值接近于 2.2204460492503130808472633361816E-16
        // function equal(a, b){
        //     if(Math.abs(a-b) < Number.EPSILON){
        //         return true;
        //     }else{
        //         return false;
        //     }
        // }
        // console.log(0.1 + 0.2 === 0.3);
        // console.log(equal(0.1 + 0.2, 0.3))

        //1. 二进制和八进制
        // let b = 0b1010;
        // let o = 0o777;
        // let d = 100;
        // let x = 0xff;
        // console.log(x);

        //2. Number.isFinite  检测一个数值是否为有限数
        // console.log(Number.isFinite(100));
        // console.log(Number.isFinite(100/0));
        // console.log(Number.isFinite(Infinity));
        
        //3. Number.isNaN 检测一个数值是否为 NaN 
        // console.log(Number.isNaN(123)); 

        //4. Number.parseInt Number.parseFloat字符串转整数
        // console.log(Number.parseInt('5211314love'));
        // console.log(Number.parseFloat('3.1415926神奇'));

        //5. Number.isInteger 判断一个数是否为整数
        // console.log(Number.isInteger(5));
        // console.log(Number.isInteger(2.5));

        //6. Math.trunc 将数字的小数部分抹掉  
        // console.log(Math.trunc(3.5));

        //7. Math.sign 判断一个数到底为正数 负数 还是零
        console.log(Math.sign(100));
        console.log(Math.sign(0));
        console.log(Math.sign(-20000));

    </script>
</body>
</html>
```

## 18.扩展运算符(ES6)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>扩展运算符</title>
</head>
<body>
    <script>
        // 『...』 扩展运算符能将『数组』转换为逗号分隔的『参数序列』
        //声明一个数组 ...
        const tfboys = ['易烊千玺','王源','王俊凯'];
        // => '易烊千玺','王源','王俊凯'

        // 声明一个函数
        function chunwan(){
            console.log(arguments);
        }

        chunwan(...tfboys);// chunwan('易烊千玺','王源','王俊凯')
    </script>
</body>
</html>
```

## 19.Map(ES6)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Map</title>
</head>
<body>
    <script>
        //声明 Map
        let m = new Map();

        //添加元素
        m.set('name','尚硅谷');
        m.set('change', function(){
            console.log("我们可以改变你!!");
        });
        let key = {
            school : 'ATGUIGU'
        };
        m.set(key, ['北京','上海','深圳']);

        //size
        // console.log(m.size);

        //删除
        // m.delete('name');

        //获取
        // console.log(m.get('change'));
        // console.log(m.get(key));

        //清空
        // m.clear();

        //遍历
        for(let v of m){
            console.log(v);
        }

        // console.log(m);

    </script>
</body>
</html>
```

## 20.Set()集合(ES6)

```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>集合</title>
</head>
<body>
    <script>
        //声明一个 set
        let s = new Set();
        let s2 = new Set(['大事儿','小事儿','好事儿','坏事儿','小事儿']);

        //元素个数
        // console.log(s2.size);
        //添加新的元素
        // s2.add('喜事儿');
        //删除元素
        // s2.delete('坏事儿');
        //检测
        // console.log(s2.has('糟心事'));
        //清空
        // s2.clear();
        // console.log(s2);

        for(let v of s2){
            console.log(v);
        }
        
    </script>
</body>
</html>
```

### 20.1 set实践

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Set 实践</title>
</head>
<body>
    <script>
        let arr = [1,2,3,4,5,4,3,2,1];
        //1. 数组去重
        // let result = [...new Set(arr)];
        // console.log(result);
        //2. 交集
        let arr2 = [4,5,6,5,6];
        // let result = [...new Set(arr)].filter(item => {
        //     let s2 = new Set(arr2);// 4 5 6
        //     if(s2.has(item)){
        //         return true;
        //     }else{
        //         return false;
        //     }
        // });
        // let result = [...new Set(arr)].filter(item => new Set(arr2).has(item));
        // console.log(result);

        //3. 并集
        // let union = [...new Set([...arr, ...arr2])];
        // console.log(union);

        //4. 差集
        let diff = [...new Set(arr)].filter(item => !(new Set(arr2).has(item)));
        console.log(diff);

    </script>
</body>

</html>
```

## 21 class的set()、get()---(ES6)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>get 和 set</title>
</head>
<body>
    <script>
        // get 和 set  
        class Phone{
            get price(){
                console.log("价格属性被读取了");
                return 'iloveyou';
            }

            set price(newVal){
                console.log('价格属性被修改了');
            }
        }

        //实例化对象
        let s = new Phone();

        // console.log(s.price);
        s.price = 'free';
    </script>
</body>
</html>
```

## 22.class类(ES6)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>类声明</title>
</head>
<body>
    <script>
        //手机
        function Phone(brand, price){
            this.brand = brand;
            this.price = price;
        }

        //添加方法
        Phone.prototype.call = function(){
            console.log("我可以打电话!!");
        }

        //实例化对象
        let Huawei = new Phone('华为', 5999);
        Huawei.call();
        console.log(Huawei);

        //class
        class Shouji{
            //构造方法 名字不能修改
            constructor(brand, price){
                this.brand = brand;
                this.price = price;
            }

            //方法必须使用该语法, 不能使用 ES5 的对象完整形式
            call(){
                console.log("我可以打电话!!");
            }
        }

        let onePlus = new Shouji("1+", 1999);

        console.log(onePlus);
    </script>
</body>
</html>
```

## 23.promise语法(ES6)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Promise基本语法</title>
</head>
<body>
    <script>
        //实例化 Promise 对象
        const p = new Promise(function(resolve, reject){
            setTimeout(function(){
                //
                // let data = '数据库中的用户数据';
                // resolve
                // resolve(data);

                let err = '数据读取失败';
                reject(err);
            }, 1000);
        });

        //调用 promise 对象的 then 方法
        p.then(function(value){
            console.log(value);
        }, function(reason){
            console.error(reason);
        })
    </script>
</body>
</html>
```

### 23.1promise封装ajax请求

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>发送 AJAX 请求</title>
</head>

<body>
    <script>
        // 接口地址: https://api.apiopen.top/getJoke
        const p = new Promise((resolve, reject) => {
            //1. 创建对象
            const xhr = new XMLHttpRequest();

            //2. 初始化
            xhr.open("GET", "https://api.apiopen.top/getJ");

            //3. 发送
            xhr.send();

            //4. 绑定事件, 处理响应结果
            xhr.onreadystatechange = function () {
                //判断
                if (xhr.readyState === 4) {
                    //判断响应状态码 200-299
                    if (xhr.status >= 200 && xhr.status < 300) {
                        //表示成功
                        resolve(xhr.response);
                    } else {
                        //如果失败
                        reject(xhr.status);
                    }
                }
            }
        })
        
        //指定回调
        p.then(function(value){
            console.log(value);
        }, function(reason){
            console.error(reason);
        });
    </script>
</body>

</html>
```

### 23.2 promise的then()方法

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Promise.prototype.then</title>
</head>
<body>
    <script>
        //创建 promise 对象
        const p = new Promise((resolve, reject)=>{
            setTimeout(()=>{
                resolve('用户数据');
                // reject('出错啦');
            }, 1000)
        });

        //调用 then 方法  then方法的返回结果是 Promise 对象, 对象状态由回调函数的执行结果决定
        //1. 如果回调函数中返回的结果是 非 promise 类型的属性, 状态为成功, 返回值为对象的成功的值

        // const result = p.then(value => {
        //     console.log(value);
        //     //1. 非 promise 类型的属性
        //     // return 'iloveyou';
        //     //2. 是 promise 对象
        //     // return new Promise((resolve, reject)=>{
        //     //     // resolve('ok');
        //     //     reject('error');
        //     // });
        //     //3. 抛出错误
        //     // throw new Error('出错啦!');
        //     throw '出错啦!';
        // }, reason=>{
        //     console.warn(reason);
        // });

        //链式调用
        p.then(value=>{

        }).then(value=>{

        });


    </script>
</body>
</html>
```

### 23.3 promise的catch方法

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>catch方法</title>
</head>
<body>
    <script>
        const p = new Promise((resolve, reject)=>{
            setTimeout(()=>{
                //设置 p 对象的状态为失败, 并设置失败的值
                reject("出错啦!");
            }, 1000)
        });

        // p.then(function(value){}, function(reason){
        //     console.error(reason);
        // });

        p.catch(function(reason){
            console.warn(reason);
        });
    </script>
</body>
</html>
```

### 23.4 promise封装-读取文件

```js
//1. 引入 fs 模块
const fs = require('fs');

//2. 调用方法读取文件
// fs.readFile('./resources/为学.md', (err, data)=>{
//     //如果失败, 则抛出错误
//     if(err) throw err;
//     //如果没有出错, 则输出内容
//     console.log(data.toString());
// });

//3. 使用 Promise 封装
const p = new Promise(function(resolve, reject){
    fs.readFile("./resources/为学.mda", (err, data)=>{
        //判断如果失败
        if(err) reject(err);
        //如果成功
        resolve(data);
    });
});

p.then(function(value){
    console.log(value.toString());
}, function(reason){
    console.log("读取失败!!");
});

```



### 23.5 promise实践-读取多个文件

```js
//引入 fs 模块
const fs = require("fs");

// fs.readFile('./resources/为学.md', (err, data1)=>{
//     fs.readFile('./resources/插秧诗.md', (err, data2)=>{
//         fs.readFile('./resources/观书有感.md', (err, data3)=>{
//             let result = data1 + '\r\n' +data2  +'\r\n'+ data3;
//             console.log(result);
//         });
//     });
// });

//使用 promise 实现
const p = new Promise((resolve, reject) => {
    fs.readFile("./resources/为学.md", (err, data) => {
        resolve(data);
    });
});

p.then(value => {
    return new Promise((resolve, reject) => {
        fs.readFile("./resources/插秧诗.md", (err, data) => {
            resolve([value, data]);
        });
    });
}).then(value => {
    return new Promise((resolve, reject) => {
        fs.readFile("./resources/观书有感.md", (err, data) => {
            //压入
            value.push(data);
            resolve(value);
        });
    })
}).then(value => {
    console.log(value.join('\r\n'));
});
```

