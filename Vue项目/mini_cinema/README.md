# 脚手架创建项目

```shell
vue create mini_cinema
```

## 安装依赖

- `Babel`
- `CSS Pre-processors`
- `Sass/SCSS (with dart-sass)`

## 初始化项目

将脚手架搭建的项目中不需要的部分进行删除，初始化项目。

```js
// main.js - 项目入口模块

import Vue from 'vue'
import App from './App' // 导入根组件

Vue.config.productionTip = false

new Vue({
   el: '#app',
   render: h => h(App)
})
```

```vue
// App.vue - 根组件

<template>
  <div>这是根组件</div>
</template>
```



# 项目开发流程

## 路由设计

![路由设计](D:\notes\Vue项目\mini_cinema\img\路由设计.jpg)

先设计最底下的电影、影院、我的三大路由展示页组件，结构如下：

![底部三个大路由](D:\notes\Vue项目\mini_cinema\img\底部三个大路由.jpg)

```shell
// 路由下载
npm install vue-router --save
```

在`src`开发文件下新建`router`文件夹，里面再新建一个`index.js`路由入口模块。

```js
// 路由入口模块

import VueRouter from 'vue-router' // 导入路由
import Vue from 'vue' // 导入vue,路由依赖于vue

const Movie = () => import('@/views/Movie') // 使用懒加载,导入子组件
const Cinema = () => import('@/views/Cinema')
const Mine = () => import('@/views/Mine')

Vue.use(VueRouter) // 使用中间介注册路由

const router = new VueRouter({ // 实例化一个路由
    routes: [
    	{path: '/movie',component: Movie},
  		{path: '/cinema',component: Cinema},
  		{path: '/mine',component: Mine}
    ],
    mode: 'history'
})

export default router // 导出router,引入的时候注意一下
```

```js
// main.js - 项目入口模块中导入并注册路由

import router from './router' // 导入
new Vue({
   el: '#app',
   router, // 注册
   render: h => h(App)
})
```

```vue
// 回到App.vue根组件中,放置用以展示路由内容的坑

<template>
  <div>
    这是根组件
    <router-view />  
  </div>
</template>
```

完成上述路由设计之后，我们通过手动更改`url`以验证路由是否设计成功！



# `git`管理项目

## 创建分支

```shell
创建并且连接到分支dev上
git checkout -b dev
```

```shell
将第一阶段的代码push到开发分支dev上
git push ck dev
```



