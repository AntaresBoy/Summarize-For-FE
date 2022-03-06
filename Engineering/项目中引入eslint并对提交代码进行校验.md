# 项目中引入eslint并对提交代码进行校验

## 1.引入eslint在项目根目录下新建.eslintignore和.eslintrc.json

**.eslintignore**

```
node_modules
test
src/public
```

**.eslintrc.json**

```
{
    "parser": "babel-eslint",
    "env": {
        "es6": true,
        "commonjs": true,
        "node": true
    },
    "rules": {
        "indent": ["error", 4],
        "quotes": [
            "error",
            "single",
            {
              "allowTemplateLiterals": true
            }
        ],
        "semi": [
            "error",
            "never"
        ]
    }
}
```

## 2.安装eslint

```
npm i eslint babel-eslint -D
```

## 3.配置package.json的lint命令

```
{
  "name": "koa2-weibo-code",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "cross-env NODE_ENV=dev ./node_modules/.bin/nodemon --inspect=9229 bin/www",
    "prd": "cross-env NODE_ENV=production pm2 start pm2.conf.json",
    "lint": "eslint --ext .js ./src",
    "test": "cross-env NODE_ENV=test jest --runInBand --forceExit --colors"
  },
  "dependencies": {
    "ajv": "^6.10.2",
    "date-fns": "^2.1.0",
    "debug": "^4.1.1",
    "ejs": "^2.3.4",
    "formidable-upload-koa": "^1.0.1",
    "fs-extra": "^8.1.0",
    "koa": "^2.7.0",
    "koa-bodyparser": "^4.2.1",
    "koa-convert": "^1.2.0",
    "koa-generic-session": "^2.0.1",
    "koa-json": "^2.0.2",
    "koa-logger": "^3.2.0",
    "koa-onerror": "^4.1.0",
    "koa-redis": "^4.0.0",
    "koa-router": "^7.4.0",
    "koa-static": "^5.0.0",
    "koa-views": "^6.2.0",
    "mysql2": "^1.7.0",
    "redis": "^2.8.0",
    "sequelize": "^5.18.0",
    "xss": "^1.0.6"
  },
  "devDependencies": {
    "babel-eslint": "^10.0.3",
    "cross-env": "^5.2.0",
    "eslint": "^6.3.0",
    "jest": "^24.9.0",
    "nodemon": "^1.19.1",
    "pre-commit": "^1.2.2",
    "supertest": "^4.0.2"
  },
  "pre-commit": [
    "lint"
  ]
}


```

## 4.git commit 之前对代码检查

### 4.1.安装pre-commit

```
npm i pre-commit -D

```

### 4.2.配置package.json中的pre-commit

```
 "pre-commit": [
    "lint"//代码commit前运行npm run lint
  ]

```

配置完成后只要代码在commit之前会执行npm run lint命令对提交代码进行检查，如果符合要求则运行通过，否则会报错组织git 提交