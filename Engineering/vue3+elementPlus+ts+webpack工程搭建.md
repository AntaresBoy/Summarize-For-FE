# vue3+elementPlus+ts+webpack工程搭建

## 1.创建VUE3项目

- 安装脚手架

  ```json
  npm install -g @vue/cli
  ```

- 使用脚手架vue-cli([vue-cli@4.5.4](https://link.juejin.cn/?target=mailto%3Avue-cli%404.5.4)已经支持直接创建vue3.x项目了)

  ```js
  vue create vue3-test
  ```

- 选择预设配置，这里我们选择人工选择（Manually select features）

  ![img](https://uploader.shimo.im/f/LndfKjdstTpEgDjn.png!thumbnail?accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJhdWQiOiJhY2Nlc3NfcmVzb3VyY2UiLCJleHAiOjE2NDcyNjQ4NjMsImciOiIyNXE1TW1ibTVXVU9aZ3FEIiwiaWF0IjoxNjQ3MjY0NTYzLCJ1c2VySWQiOjIyNTg4NjIzfQ.xOCdFzGxgDuaNmD0Eeo6OmVZRGGuWDxFm_FWjhX06p8)

- 根据自己需要选择对应选项（如：typescript等）

  ![img](https://uploader.shimo.im/f/1BE5YBKDyb1P1W6m.png!thumbnail?accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJhdWQiOiJhY2Nlc3NfcmVzb3VyY2UiLCJleHAiOjE2NDcyNjQ4NjMsImciOiIyNXE1TW1ibTVXVU9aZ3FEIiwiaWF0IjoxNjQ3MjY0NTYzLCJ1c2VySWQiOjIyNTg4NjIzfQ.xOCdFzGxgDuaNmD0Eeo6OmVZRGGuWDxFm_FWjhX06p8)

- 选择：3.x

  ![img](https://uploader.shimo.im/f/J8GwjIo0sHDbiReX.png!thumbnail?accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJhdWQiOiJhY2Nlc3NfcmVzb3VyY2UiLCJleHAiOjE2NDcyNjQ4NjMsImciOiIyNXE1TW1ibTVXVU9aZ3FEIiwiaWF0IjoxNjQ3MjY0NTYzLCJ1c2VySWQiOjIyNTg4NjIzfQ.xOCdFzGxgDuaNmD0Eeo6OmVZRGGuWDxFm_FWjhX06p8)

- 一路回车即可

  ![img](https://uploader.shimo.im/f/dvidMrkQyvI3t88C.png!thumbnail?accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJhdWQiOiJhY2Nlc3NfcmVzb3VyY2UiLCJleHAiOjE2NDcyNjQ4NjMsImciOiIyNXE1TW1ibTVXVU9aZ3FEIiwiaWF0IjoxNjQ3MjY0NTYzLCJ1c2VySWQiOjIyNTg4NjIzfQ.xOCdFzGxgDuaNmD0Eeo6OmVZRGGuWDxFm_FWjhX06p8)

  ![img](https://uploader.shimo.im/f/0tqHZ5Rvgm3qBnZJ.png!thumbnail?accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJhdWQiOiJhY2Nlc3NfcmVzb3VyY2UiLCJleHAiOjE2NDcyNjQ4NjMsImciOiIyNXE1TW1ibTVXVU9aZ3FEIiwiaWF0IjoxNjQ3MjY0NTYzLCJ1c2VySWQiOjIyNTg4NjIzfQ.xOCdFzGxgDuaNmD0Eeo6OmVZRGGuWDxFm_FWjhX06p8)

  ![img](https://uploader.shimo.im/f/QduPMPxHmmZqYPcb.png!thumbnail?accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJhdWQiOiJhY2Nlc3NfcmVzb3VyY2UiLCJleHAiOjE2NDcyNjQ4NjMsImciOiIyNXE1TW1ibTVXVU9aZ3FEIiwiaWF0IjoxNjQ3MjY0NTYzLCJ1c2VySWQiOjIyNTg4NjIzfQ.xOCdFzGxgDuaNmD0Eeo6OmVZRGGuWDxFm_FWjhX06p8)

- 安装完后目录结构如下：

  ![img](https://uploader.shimo.im/f/TCiWcMnmGhxNSlFl.png!thumbnail?accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJhdWQiOiJhY2Nlc3NfcmVzb3VyY2UiLCJleHAiOjE2NDcyNjQ4NjMsImciOiIyNXE1TW1ibTVXVU9aZ3FEIiwiaWF0IjoxNjQ3MjY0NTYzLCJ1c2VySWQiOjIyNTg4NjIzfQ.xOCdFzGxgDuaNmD0Eeo6OmVZRGGuWDxFm_FWjhX06p8)

  此时就是vue3+typescript+webpack的项目了

- 运行npm run serve,启动项目

  #### ![img](https://uploader.shimo.im/f/jlDeMxamknvAKTf4.png!thumbnail?accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJhdWQiOiJhY2Nlc3NfcmVzb3VyY2UiLCJleHAiOjE2NDcyNjQ4NjMsImciOiIyNXE1TW1ibTVXVU9aZ3FEIiwiaWF0IjoxNjQ3MjY0NTYzLCJ1c2VySWQiOjIyNTg4NjIzfQ.xOCdFzGxgDuaNmD0Eeo6OmVZRGGuWDxFm_FWjhX06p8)

  注：之前安装过可使用：npm update -g @vue/cli对脚手架进行更新

  脚手架官网：<https://cli.vuejs.org/zh/guide/creating-a-project.html#vue-create>

  ​

### 1.1 VUE3的有点（相比vue2）

#### 1.性能的提升

- 打包大小减少41%
- 初次渲染快55%
-  更新渲染快133%
- 内存减少54%
- ......

#### 2.源码的升级

- 使用Proxy代替defineProperty实现响应式
- 重写虚拟DOM的实现和Tree-Shaking
- ......

#### 3.拥抱TypeScript

Vue3可以更好的支持TypeScript

#### 4.新的特性

- Composition API（组合API）
- setup配置
- ref与reactive
- watch与watchEffect
- provide与inject
- 新的生命周期钩子
- 移除keyCode支持作为 v-on 的修饰符
- ......

#### 5.易于维护

Composition API可以更加优雅的组织我们的代码，函数。让相关功能的代码更加有序的组织在一起。

![img](https://uploader.shimo.im/f/AM0zxmXzEfHGkoZ0.png!thumbnail?accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJhdWQiOiJhY2Nlc3NfcmVzb3VyY2UiLCJleHAiOjE2NDcyNjUxNTQsImciOiIyNXE1TW1ibTVXVU9aZ3FEIiwiaWF0IjoxNjQ3MjY0ODU0LCJ1c2VySWQiOjIyNTg4NjIzfQ.C51SBQTvFwGCinzcujlrRctd6W49N6bPe17H-wqJ51o)

具体优势见官网:<https://blog.vuejs.org/posts/vue-3-one-piece.html>

## 2.修改工程文件配置

### 2.1 .eslintrc.js

```js
module.exports = {
  root: true,
  env: {
    node: true
  },
  'extends': [
    'plugin:vue/vue3-essential',
    'eslint:recommended',
    '@vue/typescript/recommended'
  ],
  parserOptions: {
    ecmaVersion: 2020
  },
  rules: {
    'no-console': process.env.NODE_ENV === 'production' ? 'warn' : 'off',
    'no-debugger': process.env.NODE_ENV === 'production' ? 'warn' : 'off',
    '@typescript-eslint/no-explicit-any': 'off',
    '@typescript-eslint/explicit-module-boundary-types': 'off'
  }
}

```

### 2.2 .gitignore

```
.DS_Store
node_modules

# local env files
.env.local
.env.*.local

# Log files
npm-debug.log*
yarn-debug.log*
yarn-error.log*
pnpm-debug.log*

# Editor directories and files
.idea
.vscode
*.suo
*.ntvs*
*.njsproj
*.sln
*.sw?

```

### 2.3 vue.config.js

```js
/* eslint-disable @typescript-eslint/no-var-requires */
const path = require('path')
require('events').EventEmitter.defaultMaxListeners = 0
const webpack = require('webpack')

function resolve(dir) {
  return path.resolve(__dirname, dir)
}
module.exports = {
  lintOnSave: false,
  chainWebpack: config => {
    config.resolve.alias.set("@", resolve("src")) //解决eslint不使用变量报错问题
    // config.plugin('webpack-bundle-analyzer').use(require('webpack-bundle-analyzer').BundleAnalyzerPlugin)
    //忽略monent/locale下的所有文件
    config.plugin('ignore').use(new webpack.IgnorePlugin(/^\.\/locale$/, /monment$/))
  },
  // publicPath: './',
  outputDir: 'dist',
  productionSourceMap: false,
  filenameHashing: true,
  assetsDir: 'src/assets',
  indexPath: 'index.html',
  configureWebpack: {
    devtool: "source-map",
    module: {
      rules: [{
        test: /\.mjs$/,
        include: /node_modules/,
        type: 'javascript/auto',
      }, ],
    },
  },
  devServer: {
    port: 9090,
    open: true,
    overlay: {
      warnings: true,
      errors: true
    },
    hotOnly: true,
    https: false,
    proxy: {
      "/auth/users": {
        target: "http://xxxxxx.com.cn",
        secure: false,
        changeOrigin: true
      },
      "/users": {
        target: "http://vvvvvvv.com.cn:8686",
        secure: false,
        ws: true,
      },
      "/ids/manager": {
        target: "http://10.7.235.231:8080",
        secure: true,
        changeOrigin: true,
      },
      "/portal": {
        target: 'https://ssss.com.cn',
        secure: false,
        changeOrigin: true
      },
      "/api": {
        // target: "http://10.9.183.183:8000",
        target: "http://39.108.119.239:8000",
        secure: true,
        changeOrigin: true,
      }
    }
  }
}

```

### 2.4 tsconfig.json

```json
{
  "compilerOptions": {
    "target": "esnext",
    "module": "esnext",
    "strict": true,
    "jsx": "preserve",
    "importHelpers": true,
    "moduleResolution": "node",
    "skipLibCheck": true,
    "esModuleInterop": true,
    "resolveJsonModule": true,
    "allowSyntheticDefaultImports": true,
    "sourceMap": true,
    "baseUrl": "./",
    "types": [
      "webpack-env"
    ],
    "paths": {
      "@/*": [
        "src/*"
      ]
    },
    "lib": [
      "esnext",
      "dom",
      "dom.iterable",
      "scripthost"
    ]
  },
  "include": [
    "src/**/*.ts",
    "src/**/*.tsx",
    "src/**/*.vue",
    "tests/**/*.ts",
    "tests/**/*.tsx"
  ],
  "exclude": [
    "node_modules"
  ]
}

```

### 2.5 pakage.json

```json
{
  "name": "vue3-demo",
  "version": "1.2.0",
  "private": true,
  "scripts": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build",
    "start": "vue-cli-service serve",
    "lint": "eslint --fix --ext .js,.vue src"
  },
  "dependencies": {
    "@element-plus/icons-vue": "^0.2.7",
    "@kangc/v-md-editor": "^2.3.13",
    "@tinymce/tinymce-vue": "^4.0.5",
    "axios": "^0.25.0",
    "core-js": "^3.6.5",
    "dayjs": "^1.10.6",
    "element-plus": "^1.3.0-beta.5",
    "tinymce": "^5.10.3",
    "vue": "^3.2.0",
    "vue-router": "^4.0.10",
    "vuex": "^4.0.2"
  },
  "devDependencies": {
    "@types/lodash-es": "^4.17.5",
    "@typescript-eslint/eslint-plugin": "^4.18.0",
    "@typescript-eslint/parser": "^4.18.0",
    "@vue/cli-plugin-babel": "~4.5.0",
    "@vue/cli-plugin-eslint": "~4.5.0",
    "@vue/cli-plugin-router": "~4.5.0",
    "@vue/cli-plugin-typescript": "~4.5.0",
    "@vue/cli-plugin-vuex": "~4.5.0",
    "@vue/cli-service": "~4.5.0",
    "@vue/compiler-sfc": "^3.2.0",
    "@vue/eslint-config-typescript": "^7.0.0",
    "babel-plugin-import": "^1.13.3",
    "eslint": "^6.7.2",
    "eslint-plugin-vue": "^7.0.0",
    "highlight.js": "^11.4.0",
    "less": "^3.9.0",
    "less-loader": "^5.0.0",
    "lodash-es": "^4.17.21",
    "postcss-px2rem": "^0.3.0",
    "prismjs": "^1.26.0",
    "sass-loader": "^12.4.0",
    "stylus": "^0.54.7",
    "stylus-loader": "^3.0.2",
    "typescript": "^4.4.4"
  },
  "eslintConfig": {
    "root": true,
    "env": {
      "node": true
    },
    "extends": [
      "plugin:vue/essential",
      "eslint:recommended",
      "@vue/typescript/recommended"
    ],
    "parserOptions": {
      "ecmaVersion": 2020
    },
    "rules": {
      "@typescript-eslint/camelcase": 0
    },
    "overrides": [
      {
        "files": [
          "**/__tests__/*.{j,t}s?(x)",
          "**/tests/unit/**/*.spec.{j,t}s?(x)"
        ],
        "env": {
          "jest": true
        }
      }
    ]
  }
}


```

### 2.6 babel.config.js

```js
module.exports = {
  presets: [
    '@vue/cli-plugin-babel/preset'
  ],
  plugins: [
    ['import', {
      libraryName: 'vant',
      libraryDirectory: 'es',
      style: true
    }, 'vant']
  ]
}

```

### 2.7 .browserslistrc

```
> 1%
last 2 versions
not dead

```

### 2.8 ts类型声明shims-vue.d.ts

```
declare module '*.vue' {
  import { defineComponent } from 'vue'
  const component: ReturnType<typeof defineComponent>
  export default component
}

declare module '@kangc/v-md-editor/lib/theme/vuepress.js'
declare module '@kangc/v-md-editor'
declare module 'prismjs'
declare module 'pubsub-js'

```

## 3.引入elementPlus

elementPlus官网：<https://element-plus.gitee.io/zh-CN/>

### 3.1新建plugins文件夹并创建lib.js

lib.js:

```js
//引入ElementPlus 
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'
import * as ElIcons from '@element-plus/icons-vue'

//引入v-md-editor
import VueMarkdownEditor from '@kangc/v-md-editor'
import '@kangc/v-md-editor/lib/style/base-editor.css'
import vuepressTheme from '@kangc/v-md-editor/lib/theme/vuepress.js'
import '@kangc/v-md-editor/lib/theme/style/vuepress.css'
import Prism from 'prismjs'

VueMarkdownEditor.use(vuepressTheme, {
  Prism,
})

export default (app: any) => {
  //全局注册图标
  for (const name in ElIcons) {
    app.component(name, (ElIcons as any)[name])
  }
  app.use(ElementPlus)
  app.use(VueMarkdownEditor)
  app.component('mavon-editor', VueMarkdownEditor)
}

```

### 3.2 修改main.ts

```typescript
import { createApp } from 'vue'
import App from './App.vue'
//引入router
import router from './router'
//引入store
import store from './store'
//引入基本样式
import './assets/css/base.css'
import './assets/css/main.styl'
import BaseDirective from './directive'
//引入外部组件
import installElementPlus from './plugins/lib'

const app = createApp(App)
//全局挂载ElementPlus
installElementPlus(app)
//注册
app.use(BaseDirective)
app.use(store)
app.use(router)
app.mount('#app')

```

### 3.3 设置全局样式-postcss.config.js

```js
module.exports = {
  plugins: {
    autoprefixer: {},
    // 'postcss-px2rem': {
    //   remUnit: 50
    // }
  }
}
```

## 4.引入router

### 4.1 安装VueRouter并创建index.ts

vue2.0使用的是vue-router@3.x,vue3.0使用的是`vue-router@4.x` 

```
npm i vue-router@4.0 -D
```

创建router文件夹，并在文件夹中创建index.ts文件，文件内容如下:

```js
import { createRouter, createWebHashHistory, RouteRecordRaw } from 'vue-router'

const routes: Array<RouteRecordRaw> = [
  {
    path: '/login',
    name: 'Login',
    component: () =>
      import(/* webpackChunkName: "login" */ '@/pages/login/Login.vue'),
  },
  {
    path: '/',
    redirect: '/login',
  },
  {
    path: '/home',
    name: 'Home',
    component: () => import(/* webpackChunkName: 'home'*/ '@/pages/Home.vue'),
  },
  {
    path: '/article/:contentId/:type',
    name: 'Article',
    component: () =>
      import(/* webpackChunkName: 'article' */ '@/pages/content/Article.vue'),
  },
  {
    path: '/register',
    name: 'Register',
    component: () =>
      import(
        /* webpackChunkName: "register" */ '@/pages/register/Register.vue'
      ),
  },
  {
    path: '/my-blog/:username',
    name: 'MyBlogs',
    component: () =>
      import(/* webpackChunkName: "Layout" */ '@/pages/Home.vue'),
  },
]

const router = createRouter({
  history: createWebHashHistory(),
  routes,
})
//路由前置守卫
router.beforeEach((to, from, next) => {
  const isLogin = sessionStorage.getItem('isLogin')
  if (!isLogin) {
    if (to.path === '/login' || to.path === '/register') {
      next()
    } else {
      next('/login')
    }
  } else {
    next()
  }
})

export default router

```

### 4.2 main.ts

main.ts引入路由：

```js
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'
const app = createApp(App)
app.use(router)
app.mount('#app')

```

## 5.引入store

### 5.1安装Vuex并创建index.ts

vue2.0使用的是vuex@3.x，vue3.0使用的是`vuex@4.x` 所以安装`vuex@4.x`

```
npm i vuex @4.0 -D
```

### 5.2创建计数器案例

创建counter文件夹并创建counter.ts

- 创建defaultState

  ```js
  export default {
    count: 0
  }

  ```

- 创建actions

  ```js
  export default {
    inrement(context: any) {
      return context.commit('increment')
    }
  }

  ```

- 创建getter

  ```js
  import defaultState from './defaultState'
  export default {
    double(state: typeof defaultState) {
      return 2 * state.count
    }
  }

  ```

- 创建mutations

  ```js
  import defaultState from './defaultState'
  export default {
    increment(state: typeof defaultState) {
      state.count++
    }
  }

  ```

- 创建state

  ```js
  import defaultState from './defaultState'
  export default {
    count: defaultState.count
  }

  ```

- 创建counter入口文件counter.ts

  ```js
  import state from './state'
  import actions from './actions'
  import mutations from './mutations'
  import getters from './getters'
  export default {
    namespace: true,
    state,
    actions,
    mutations,
    getters
  }

  ```

- 创建store的入口index.ts

  ```js
  import { createStore } from 'vuex'
  import counter from './counter/counter'
  export default createStore(counter)
  ```

## 6.其它结构目录创建

其它文件结构可依次创建utils、hooks、types、config、components、assets.、api、pages、layouts、directive目录如下：

![img](https://uploader.shimo.im/f/gRVPqjGt2x0y6uW1.png!thumbnail?accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJhdWQiOiJhY2Nlc3NfcmVzb3VyY2UiLCJleHAiOjE2NDcyNjYwMjksImciOiIyNXE1TW1ibTVXVU9aZ3FEIiwiaWF0IjoxNjQ3MjY1NzI5LCJ1c2VySWQiOjIyNTg4NjIzfQ.sE_MIt-zfCB4zqFWEvepMPBBMk62Jk8hbtXfvPuQiqE)

## 7.自动化部署CI

参考：https://github.com/AntaresBoy/Summarize-For-FE/blob/master/Engineering/Github%20Actions%E5%AE%9E%E7%8E%B0%E5%8D%9A%E5%AE%A2%E9%A1%B9%E7%9B%AECI%E8%87%AA%E5%8A%A8%E9%83%A8%E7%BD%B2.md

## 8.Github主页

<https://antaresboy.github.io/BlogUI/>

![img](https://uploader.shimo.im/f/h9J4XnF1oGku1BJJ.png!thumbnail?accessToken=eyJhbGciOiJIUzI1NiIsImtpZCI6ImRlZmF1bHQiLCJ0eXAiOiJKV1QifQ.eyJhdWQiOiJhY2Nlc3NfcmVzb3VyY2UiLCJleHAiOjE2NDcyNjYzNTEsImciOiIyNXE1TW1ibTVXVU9aZ3FEIiwiaWF0IjoxNjQ3MjY2MDUxLCJ1c2VySWQiOjIyNTg4NjIzfQ.JQV5e3MmJvnP5sSP8GA94O3mKeer9ttdaJ6o_MgI__Y)

