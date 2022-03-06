# inspect & debugger调试

## 1.介绍

借助inspect和debugger可以有效在chrome浏览器下对代码进行调试。

## 2.使用

**1.在package.json中配置script**

```
"dev": "cross-env NODE_ENV=dev ./node_modules/.bin/nodemon --inspect=9229 bin/www"

```

**注：--inspect=9229使用9229这个端口号进行调试，当多个人使用inspect调试时需要保证端口号不一样**

2.**运行npm run dev**

出现信息如下：

```
[nodemon] starting `node --inspect=9229 bin/www`
Debugger listening on ws://127.0.0.1:9229/73abece8-1ca7-4dc8-97cb-5df535ecff2a
For help, see: https://nodejs.org/en/docs/inspector
```

![img](https://uploader.shimo.im/f/jrcTYSBflh6KABO6.png!thumbnail?accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJhdWQiOiJhY2Nlc3NfcmVzb3VyY2UiLCJleHAiOjE2NDY1MzU0MzcsImciOiJtNGtNTE9hZ3lsVE1vUnFEIiwiaWF0IjoxNjQ2NTM1MTM3LCJ1c2VySWQiOjIyNTg4NjIzfQ.faSFIof2QDJ_kSt0YvGOSkS_tjBbHRirHc_NZJ6RJYQ)

**3.在chrome浏览器中输入：chrome://inspect**

出现页面信息如下：

![img](https://uploader.shimo.im/f/LrAWRtVSOP2fveSv.png!thumbnail?accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJhdWQiOiJhY2Nlc3NfcmVzb3VyY2UiLCJleHAiOjE2NDY1MzU0MzcsImciOiJtNGtNTE9hZ3lsVE1vUnFEIiwiaWF0IjoxNjQ2NTM1MTM3LCJ1c2VySWQiOjIyNTg4NjIzfQ.faSFIof2QDJ_kSt0YvGOSkS_tjBbHRirHc_NZJ6RJYQ)

**4.点击inspect调出控制台**

![img](https://uploader.shimo.im/f/1tt7HLKPB40WBjOc.png!thumbnail?accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJhdWQiOiJhY2Nlc3NfcmVzb3VyY2UiLCJleHAiOjE2NDY1MzU0MzcsImciOiJtNGtNTE9hZ3lsVE1vUnFEIiwiaWF0IjoxNjQ2NTM1MTM3LCJ1c2VySWQiOjIyNTg4NjIzfQ.faSFIof2QDJ_kSt0YvGOSkS_tjBbHRirHc_NZJ6RJYQ)

![img](https://uploader.shimo.im/f/oq0aMgdb0MtVK4RM.png!thumbnail?accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJhdWQiOiJhY2Nlc3NfcmVzb3VyY2UiLCJleHAiOjE2NDY1MzU0MzcsImciOiJtNGtNTE9hZ3lsVE1vUnFEIiwiaWF0IjoxNjQ2NTM1MTM3LCJ1c2VySWQiOjIyNTg4NjIzfQ.faSFIof2QDJ_kSt0YvGOSkS_tjBbHRirHc_NZJ6RJYQ)

**5.当访问接口时会在控制台打印请求信息**

![img](https://uploader.shimo.im/f/1nKmmrMdskhGTPwS.png!thumbnail?accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJhdWQiOiJhY2Nlc3NfcmVzb3VyY2UiLCJleHAiOjE2NDY1MzU0MzcsImciOiJtNGtNTE9hZ3lsVE1vUnFEIiwiaWF0IjoxNjQ2NTM1MTM3LCJ1c2VySWQiOjIyNTg4NjIzfQ.faSFIof2QDJ_kSt0YvGOSkS_tjBbHRirHc_NZJ6RJYQ)

![img](https://uploader.shimo.im/f/zeWnG1ozLG8gORsU.png!thumbnail?accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJhdWQiOiJhY2Nlc3NfcmVzb3VyY2UiLCJleHAiOjE2NDY1MzU0MzcsImciOiJtNGtNTE9hZ3lsVE1vUnFEIiwiaWF0IjoxNjQ2NTM1MTM3LCJ1c2VySWQiOjIyNTg4NjIzfQ.faSFIof2QDJ_kSt0YvGOSkS_tjBbHRirHc_NZJ6RJYQ)

**6.代码加断点debugger**

![img](https://uploader.shimo.im/f/Ij2iCT8A69ptWRqG.png!thumbnail?accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJhdWQiOiJhY2Nlc3NfcmVzb3VyY2UiLCJleHAiOjE2NDY1MzU0MzcsImciOiJtNGtNTE9hZ3lsVE1vUnFEIiwiaWF0IjoxNjQ2NTM1MTM3LCJ1c2VySWQiOjIyNTg4NjIzfQ.faSFIof2QDJ_kSt0YvGOSkS_tjBbHRirHc_NZJ6RJYQ)



**7.重新启动**

![img](https://uploader.shimo.im/f/1CypIkBCBUvGwCM3.png!thumbnail?accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJhdWQiOiJhY2Nlc3NfcmVzb3VyY2UiLCJleHAiOjE2NDY1MzU0MzcsImciOiJtNGtNTE9hZ3lsVE1vUnFEIiwiaWF0IjoxNjQ2NTM1MTM3LCJ1c2VySWQiOjIyNTg4NjIzfQ.faSFIof2QDJ_kSt0YvGOSkS_tjBbHRirHc_NZJ6RJYQ)

**注：****重启后需要重新输入chrome://inspect否则不生效**

**8.点击源代码进入代码调试界面**

![img](https://uploader.shimo.im/f/7wjBEdtC2xqnS6ur.png!thumbnail?accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJhdWQiOiJhY2Nlc3NfcmVzb3VyY2UiLCJleHAiOjE2NDY1MzU0MzcsImciOiJtNGtNTE9hZ3lsVE1vUnFEIiwiaWF0IjoxNjQ2NTM1MTM3LCJ1c2VySWQiOjIyNTg4NjIzfQ.faSFIof2QDJ_kSt0YvGOSkS_tjBbHRirHc_NZJ6RJYQ)

![img](https://uploader.shimo.im/f/xlePTbCw7GKOLnrj.png!thumbnail?accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJhdWQiOiJhY2Nlc3NfcmVzb3VyY2UiLCJleHAiOjE2NDY1MzU0MzcsImciOiJtNGtNTE9hZ3lsVE1vUnFEIiwiaWF0IjoxNjQ2NTM1MTM3LCJ1c2VySWQiOjIyNTg4NjIzfQ.faSFIof2QDJ_kSt0YvGOSkS_tjBbHRirHc_NZJ6RJYQ)