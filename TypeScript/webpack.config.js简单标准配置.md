## 1.webpack.config.js

```js
// 引入一个包
const path = require('path');
// 引入html插件
const HTMLWebpackPlugin = require('html-webpack-plugin');
// 引入clean插件
const { CleanWebpackPlugin } = require('clean-webpack-plugin');

// webpack中的所有的配置信息都应该写在module.exports中
module.exports = {

    // 指定入口文件
    entry: "./src/index.ts",

    // 指定打包文件所在目录
    output: {
        // 指定打包文件的目录
        path: path.resolve(__dirname, 'dist'),
        // 打包后文件的文件
        filename: "bundle.js",

        // 告诉webpack不使用箭头
        environment:{
            arrowFunction: false
        }
    },

    // 指定webpack打包时要使用模块
    module: {
        // 指定要加载的规则
        rules: [
            {
                // test指定的是规则生效的文件
                test: /\.ts$/,
                // 要使用的loader
                use: [
                     // 配置babel
                     {
                         // 指定加载器
                         loader:"babel-loader",
                         // 设置babel
                         options: {
                             // 设置预定义的环境
                             presets:[
                                 [
                                     // 指定环境的插件
                                     "@babel/preset-env",
                                     // 配置信息
                                     {
                                         // 要兼容的目标浏览器
                                         targets:{
                                             "chrome":"58",
                                             "ie":"11"
                                         },
                                         // 指定corejs的版本
                                         "corejs":"3",
                                         // 使用corejs的方式 "usage" 表示按需加载
                                         "useBuiltIns":"usage"
                                     }
                                 ]
                             ]
                         }
                     },
                    'ts-loader'
                ],
                // 要排除的文件
                exclude: /node-modules/
            }
        ]
    },

    // 配置Webpack插件
    plugins: [
        new CleanWebpackPlugin(),
        new HTMLWebpackPlugin({
            // title: "这是一个自定义的title"
            template: "./src/index.html"
        }),
    ],

    // 用来设置引用模块
    resolve: {
        extensions: ['.ts', '.js']
    }

};

```

## 2.tsconfig.json

```json
{
  "compilerOptions": {
    "module": "ES2015",
    "target": "ES2015",
    "strict": true
  }
}

```

## 3.package.json

```json
{
  "name": "part3",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack",
    "start": "webpack serve --open chrome.exe"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "@babel/core": "^7.12.9",
    "@babel/preset-env": "^7.12.7",
    "babel-loader": "^8.2.2",
    "clean-webpack-plugin": "^3.0.0",
    "core-js": "^3.8.0",
    "html-webpack-plugin": "^4.5.0",
    "ts-loader": "^8.0.11",
    "typescript": "^4.1.2",
    "webpack": "^5.6.0",
    "webpack-cli": "^4.2.0",
    "webpack-dev-server": "^3.11.0"
  }
}


```

