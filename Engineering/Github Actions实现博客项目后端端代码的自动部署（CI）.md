# Github Actions实现博客项目后端端代码的自动部署（CI）

## 介绍

由于本人个人博客后端基于Node.js+koa2+pm2部署的后端项目，所以只总结node后端代码自动部署流程

## 配置项目私钥：

![image-20220226223024615](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220226223024615.png)

![image-20220226223138179](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220226223138179.png)

## 配置服务器相关信息

![image-20220226223234799](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220226223234799.png)

## 配置Actions

```
name: deploy to aliyun-server
on:
  push:
    branches:
      - main
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      # 切换分支
      - name: Checkout
        uses: actions/checkout@master
      # 下载 git submodule
      - uses: srt32/git-actions@v0.0.3
        with:
          args: git submodule update --init --recursive
      # 使用 根据自己的情况选择node版本，这里用node:12
      - name: use Node.js 12
        uses: actions/setup-node@v1
        with:
          node-version: 12
           # npm install and build
      - name: npm install and build
        run: |
          npm install
        env:
          CI: true
      # Deploy to aliyun
      - name: Deploy to aliyun server
        uses: easingthemes/ssh-deploy@v2.1.5
        env:
          SSH_PRIVATE_KEY: ${{ secrets.ALIYUN_SERVER_ACCESS_TOKEN }}
          ARGS: "-avzr --delete"
          SOURCE: "./node_modules"
          REMOTE_HOST: ${{ secrets.ALIYUN_SERVER_HOST }}
          REMOTE_USER: ${{ secrets.ALIYUN_REMOTE_USER }}
          TARGET: ${{ secrets.ALIYUN_TARGET }}
      # start Server and pm2
      - name: Start Server
        uses: appleboy/ssh-action@master
        with:
         host: ${{ secrets.ALIYUN_SERVER_HOST }}
         username: ${{ secrets.ALIYUN_REMOTE_USER }}
         key: ${{ secrets.ALIYUN_SERVER_ACCESS_TOKEN }}
         script: |
          cd ${{ secrets.ALIYUN_TARGET }}
          ./restart.sh 
```

注：由于node后端项目不需要编译，所以只替换更新node_modules,然后在服务器拉取代码的位置git pull（拉取非node_modules的代码），然后执行shell脚本./restart.sh

restart.sh

```
#!/bin/bash
git pull//拉取代码
pm2 stop www//停止www服务
npm run prd//启动服务
或：
git pull
pm2 restart www
```

## 执行结果

![image-20220226223946923](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220226223946923.png)

![image-20220226224012475](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220226224012475.png)

![image-20220226224211579](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220226224211579.png)