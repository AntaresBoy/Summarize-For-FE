# KOA2项目搭建

## 概述

技术栈：UI：vue3、elementPlus、TypeScript、webpack

Server:KOA2、Nodejs、Nginx、redis、Mysql、PM2

服务器：阿里云ubuntu系统 1核2G

## 1.安装并创建

```
1.先全局安装koa-generator
npm i koa-generato -g
2.生成koa项目
koa2 -e 项目名(-e指的是使用模板引擎ejs来开发项目)
或：koa2 项目名
项目生成后会包含以下目录：
  1.app.js
  2.bin
  3.package.json
  4.public
  5.routes
  6.views
3.运行命令npm i安装依赖
4.运行npm run dev 启动，端口在bin/www下默认是3000

```

注：node版本要大于等于8.0.0

详见：https://shimo.im/docs/XKq4MzrKoBHy4rkN

## 2.修改package.json

```json
{
  "name": "blog-koa2",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "start": "node bin/www",
    "dev": "cross-env NODE_ENV=dev ./node_modules/.bin/nodemon bin/www",
    "prd": "cross-env NODE_ENV=production pm2 start bin/www",
    "pm2": "cross-env NODE_ENV=production pm2 start pm2.conf.json",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "dependencies": {
    "debug": "^4.1.1",
    "koa": "^2.7.0",
    "koa-bodyparser": "^4.2.1",
    "koa-convert": "^1.2.0",
    "koa-json": "^2.0.2",
    "koa-logger": "^3.2.0",
    "koa-onerror": "^4.1.0",
    "koa-router": "^7.4.0",
    "koa-static": "^5.0.0",
    "koa-views": "^6.2.0",
    "pug": "^2.0.3"
  },
  "devDependencies": {
    "cross-env": "^7.0.3",
    "koa-generic-session": "^2.0.1",
    "koa-redis": "^3.1.3",
    "mysql": "^2.18.1",
    "nodemon": "^2.0.15",
    "redis": "^2.8.0",
    "xss": "^1.0.10"
  }
}

```

## 3.创建log、controllor、db、model...

![img](https://uploader.shimo.im/f/MyI8fUn01LXxzOlE.png!thumbnail?accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJhdWQiOiJhY2Nlc3NfcmVzb3VyY2UiLCJleHAiOjE2NDcyNjYyNTMsImciOiJLbGtLVk9LNmV5dWRMWnFkIiwiaWF0IjoxNjQ3MjY1OTUzLCJ1c2VySWQiOjIyNTg4NjIzfQ._THx56pT-4Nct80lz8WufUMHRc5UJF-R2XF78ClFm7s)

## 4.服务部署

项目根目录下创建：pm2.conf.json

```
{
  "apps": {
    "name": "blog-server",
    "script": "bin/www",
    "watch": true,
    "ignore_watch": ["node_modules", "logs"],
    "instances": 1,
    "error_file": "logs/err.log",
    "out_file": "logs/out.log",
    "pid_file": "pids/node-geo-api.pid",
    "max_restarts": 10,
    "log_date_format": "YYYY-MM-DD HH:mm:ss",
    "merge_logs": true,
    "exec_interpreter": "node",
    "exec_mode": "fork",
    "autorestart": false,
    "vizion": false
  }
}

```

pm2具体使用详见：https://shimo.im/docs/rYCv9g99rHH69pkQ

## 5.Github部署CI

详见：https://shimo.im/docs/ZzkLVERePbtNMl3Q

## 6.Github地址

<https://github.com/AntaresBoy/BlogServer-Koa2>