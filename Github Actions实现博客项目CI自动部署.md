# Github Actions实现博客项目前端代码的自动部署（CI）

## Actions简介

官网定义：在 GitHub Actions 的仓库中自动化、自定义和执行软件开发工作流程。 您可以发现、创建和共享操作以执行您喜欢的任何作业（包括 CI/CD），并将操作合并到完全自定义的工作流程中

## Actions使用

### 配置公钥/私钥

```md
1.在服务器中使用命令： ssh-keygen -m PEM -t rsa -b 4096
生成pem格式的公钥和私钥
2.进入到服务器的根目录进入.ssh下，获取rsa码
    cd ~ /.ssh
    //将公钥写入.ssh/authorized_keys
    cat id_rsa.pub >> ~/.ssh/authorized_keys
    //获取私钥
    cat id_rsa
    //获取公钥
    cat id_rsa.pub

```

### Github配置公钥

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/efc449f251314d5bbfa7e8c5dea32dfc~tplv-k3u1fbpfcp-watermark.image?)

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9fde4906c67f45bcaa38d65a063bc8b1~tplv-k3u1fbpfcp-watermark.image?)

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/53664e7d699d4ec1bd4d07f9a08e4c02~tplv-k3u1fbpfcp-watermark.image?)

按图示操作即可，最后将服务器的公钥拷贝到ssh key中即可

### Github配置私钥-secret

1. 进入项目所在setting

   ![image-20220226221305718](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220226221305718.png)

  2.进入Secrets->Actions

![image-20220226221350111](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220226221350111.png)

3.点击 new repository secret

![image-20220226221540625](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220226221540625.png)

4.取名ACCESS-TOKEN（或其它命名）并将服务器私钥拷贝到value

![image-20220226221719771](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220226221719771.png)

5.同理可设置服务器账号、密码、IP、端口号、资源所在路径等变量（最后在actions脚本中连接服务器使用）

![image-20220226221858663](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220226221858663.png)

```
ALIYUN_SERVER_HOST = 阿里云服务器的地址
ALIYUN_REMOTE_USER = 阿里云user，一般是root
ALIYUN_TARGET = 目标路径
```

6.配置Actions

```
name: deploy to aliyun-ui
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
          npm run build
        env:
          CI: true
      # Deploy to aliyun
      - name: Deploy to aliyun server
        uses: easingthemes/ssh-deploy@v2.1.5
        env:
          SSH_PRIVATE_KEY: ${{ secrets.ALIYUN_SERVER_ACCESS_TOKEN }}
          ARGS: "-avzr --delete"
          SOURCE: "dist/"
          REMOTE_HOST: ${{ secrets.ALIYUN_SERVER_HOST }}
          REMOTE_USER: ${{ secrets.ALIYUN_REMOTE_USER }}
          TARGET: ${{ secrets.ALIYUN_TARGET }}
```

![image-20220226222204998](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220226222204998.png)

![image-20220226222236749](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220226222236749.png)

**当后面提交代码时会自动触发Actions去编译打包代码，并通过ssh到服务器部署编译后的代码到指定服务器发布地址**

