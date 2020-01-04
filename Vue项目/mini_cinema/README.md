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

## 一级路由设计

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



# 移动端的适配

```html
<meta name="viewport" content="width=device-width,initial-scale=1.0,user-scalable=no">
```



# 资源请求的路径

```html
<!-- 在index.html中的href属性中 <%= BASE_URL %> ,和路由中的base字段相对应 -->
<link rel="icon" href="<%= BASE_URL %>favicon.ico">
```

```js
const router = new VueRouter({  
    routes: [
    	{path: '/movie',component: Movie},
  		{path: '/cinema',component: Cinema},
  		{path: '/mine',component: Mine}
    ],
    mode: 'history',
    base: process.env.BASE_URL
})
```



# 将静态资源放入public文件夹下

在`public`文件夹下，新建一个`css`文件夹，用以存放静态样式文件。

将`iconfont`图标样式和初始化项目的样式引入进去：

```html
<link rel="stylesheet" href="<%= BASE_URL %>css/common.css">
<link rel="stylesheet" href="<%= BASE_URL %>css/iconfont.css">
```

打开浏览器控制台，在`network`中可以发现上述两个`css`已经全部加载成功了。



# 创建头部和尾部组件

 ```vue
// 头部组件

<template>
   <header id="header">
      <h1>mini_cinema</h1>
   </header>
</template>

<script>
export default {
   name: 'Header' // 添加上name属性之后,方便调试
}
</script>

<style lang="scss" scoped>
   #header{
      width: 100%;
      height: 100%;
      color: #fff;
      background-color: #e54847;
      border-bottom: 1px solid #e54847;
      position: relative;
      h1{
         font-size: 18px; 
         font-weight: normal;
         text-align: center;
         line-height: 50px;
      }
      // 这里还有一个i样式没有暂时没有写入
   }
</style>
 ```

```vue
// 底部一级路由切换组件

<template>
  <footer id="footer">
     <ul>
         <li>
            <i class="iconfont icon-dianying"></i>
            <p>电影</p>
         </li>
         <li>
            <i class="iconfont icon-yingyuan"></i>
            <p>影院</p>
         </li>
         <li>
            <i class="iconfont icon-wode"></i>
            <p>我的</p>
         </li> 
     </ul>
  </footer>
</template>

<script>
export default {

}
</script>

<style lang="scss" scoped>
   #footer{
      width: 100%;
      height: 50px;
      background-color: #fff;
      border-top: 2px solid #ebe8e3;
      position: fixed;
      left: 0;
      bottom: 0; 
      ul{
         text-align: center;
         height: 50px;
         align-items: center;
         display: flex;
         li{
            height: 40px;
            flex: 1;
            .active{
               color: #f03d37;
            }
         }
         i{
            font-size: 20px;
         }
         p{
            font-size: 12px;
            line-height: 18px;
         }
      }
   }
</style>
```

## 在Movie页面中引入上述两个组件

```vue
<template>
  <div id="main">
    <!-- 使用组件 -->
    <Header />
    <TabBar />
  </div>
</template>

<script>
// 引入头部、底部组件
import Header from '@/components/Header'
import TabBar from '@/components/TabBar'
export default {
  name: 'Movie',
  components: {
    // 注册局部组件
    Header,
    TabBar
  }
}
</script>

<style>

</style>
```



# 缓存处理

在`App.vue`中，我们使用`<keep-alive>`将路由展示给包裹起来，这样在进行路由切换的时候，就会有缓存的效果。

```vue
<template>
  <keep-alive>
    <!-- 这是根组件 -->
    <router-view />  
  </keep-alive>
</template>
```



# router-link实现页面路由切换

```vue
 <router-link tag="li" to="/movie">
    <i class="iconfont icon-dianying"></i>
    <p>电影</p>
 </router-link>
```

## 当前选中路由的高亮设置

```css
.router-link-active{
   color: #f03d37;
}
```



# 同样在cinema和mine页面也引入以上两个组件

引入，注册，使用。



# 切换路由时实现头部标题的切换

在我们的头部组件中，标题是写死的，我们需要在调用头部组件的时候，实现父子通信，将标题写成一个动态信息

```js
// 头部组件中通过props字段,结构部分渲染title字段
export default {
   name: 'Header',
   props: {
      title: {
         type: String,
         default: 'mini_cinema'
      }
   }
}
```

回到`cinema`和`mine`组件中，将`title`字段值传递出去：

```vue
<template>
  <div id="main">
    <!-- 通过在Header标签头部添加上一个 title 属性,进行传值 -->
    <Header title="电影院" />
    <TabBar />
  </div>
</template>

<script>
import Header from '@/components/Header'
import TabBar from '@/components/TabBar'
export default {
  name: 'Cinema',
  components: {
    Header,
    TabBar
  }
}
</script>
```



# 二级路由切换

 ![二级路由](D:\notes\Vue项目\mini_cinema\img\二级路由.jpg)

在`components`组件文件夹中，新建4个二级子路由，分别是`City`、`NowPlaying`、`ComingSoon`、`Search`

## 配置路由

通过`children`字段进行配置。

```js
const router = new VueRouter({  
    routes: [
        {path: '/movie',component: Movie,
            children: [
                {path: 'city',component: () => import('@/components/City')},
                {path: 'nowPlaying',component: () => import('@/components/NowPlaying')},
                {path: 'comingSoon',component: () => import('@/components/ComingSoon')},
                {path: 'search',component: () => import('@/components/Search')}
            ]
        },
  		{path: '/cinema',component: Cinema},
        {path: '/mine',component: Mine},
        {path: '/*',redirect: '/movie'} // 路由重定向,上述路由都不匹配的情况下
    ],
    mode: 'history'
})
```

## 将内容结构写入组件中

`Movie`组件：

```vue
<template>
  <div id="main">
    <!-- 使用组件 -->

    <!-- 头部 -->
    <Header />
    <!-- 二级路由切换部分 -->
    <div id="content">
      <div class="movie_menu">
        <div class="city_name">
          <span>大连</span>
          <i class="iconfont icon-lower-triangle"></i>
        </div>
        <div class="hot_swtich">
          <div class="hot_item active">正在热映</div>
          <div class="hot_item">即将上映</div>
        </div>
        <div class="search_entry">
          <i class="iconfont icon-sousuo"></i>
        </div>
      </div>
      <!-- 二级路由展示区 -->
      <keep-alive>
        <router-view></router-view>
      </keep-alive>
    </div>
 
    <!-- 底部 -->
    <TabBar />
  </div>
</template>
```

## 编辑二级路由内容的结构

总共有4个二级页面，这四个页面是位于`movie`一级路由下的，分别是：

- `City`
- `NowPlaying`
- `ComingSoon`
- `Search`

  ![二级路由切换](D:\notes\Vue项目\mini_cinema\img\二级路由切换.jpg)

```vue
<!-- 二级路由切换部分 -->
<div id="content">
  <div class="movie_menu">
    <router-link tag="div" to="/movie/city" class="city_name">
      <span>大连</span>
      <i class="iconfont icon-lower-triangle"></i>
    </router-link>
    <div class="hot_swtich">
      <router-link tag="div" to="/movie/nowPlaying" class="hot_item">正在热映</router-link>
      <router-link tag="div" to="/movie/comingSoon" class="hot_item">即将上映</router-link>
    </div>
    <router-link tag="div" to="/movie/search" class="search_entry">
      <i class="iconfont icon-sousuo"></i>
    </router-link>
  </div>
  <!-- 二级路由展示区 -->
  <keep-alive>
    <router-view></router-view>
  </keep-alive>
</div>
```

使用`router-link`进行路由的跳转。

## 选中的二级组件高亮显示

```scss
.router-link-active{
  color: #ef4238;
  border-bottom: 2px solid #ef4238;
}
```

## 重定向二级路由

```js
const router = new VueRouter({  
    routes: [
        {path: '/movie',component: Movie,
            children: [
                {path: 'city',component: () => import('@/components/City')},
                {path: 'nowPlaying',component: () => import('@/components/NowPlaying')},
                {path: 'comingSoon',component: () => import('@/components/ComingSoon')},
                {path: 'search',component: () => import('@/components/Search')},
                {path: '/movie',redirect: '/movie/nowPlaying'} // 二级路由重定向
            ]
        },
  		{path: '/cinema',component: Cinema},
        {path: '/mine',component: Mine},
        {path: '/*',redirect: '/movie'} // 路由重定向,上述路由都不匹配的情况下
    ],
    mode: 'history'
})
```



# 完成影院和我的一级路由组件内容

![影院](D:\notes\Vue项目\mini_cinema\img\影院.jpg)

创建两个组件`CiList`和`Login`。

## 回到Cinema一级路由组件

完善结构。

同样的完成`Login.vue`组件，然后回到`Mine`组件页面，里面原本是有两个组件了已经，分别是头部组件和底部组件，所以我们讲新完成的组件`Login.vue`组件导入到`Mine`组件里，注册，使用。那么这个`Mine`页面组件就完成了。

至此，基本上的组件页面拆分就已经完成了。



# 使用`git`管理阶段项目代码

```shell
git status // 查看项目的修改代码
git add .
git commit -m "" // 创建为一个版本
git checkout dev // 切换到开发分支上
git merge createComponents --no-ff // 参数-no-ff方便后期我们来进行查看日志
git log // 打印日志

git push ck dev // push到远程
git branch // 查看当前有多少分支
git branch -d createComponents // 删除分支
```

 

# 调接口，请求真实数据

```shell
git checkout -b setData // 创建一个新分支
```

## 接口文档

```
正在热映
http://39.97.33.178/api/movieOnInfoList?cityId=10

即将上映
http://39.97.33.178/api/movieComingList?cityId=10

搜索
http://39.97.33.178/api/searchList?cityId=10&kw=a

城市
http://39.97.33.178/api/cityList

电影详情
http://39.97.33.178/api/detailmovie?movieId=345808

影院
http://39.97.33.178/api/cinemaList?cityId=10

城市定位
http://39.97.33.178/api/getLocation
```

## 反向代理解决跨域问题

让脚手架能够代理到数据，在脚手架中新建一个`vue.config.js`文件，用以配置相关的环境，内容如下：

```js
module.exports = {
    devServer: {
        proxy: {
            '/api': {
                target: 'http://39.97.33.178',
                changeOrigin: true
            }
        }
    }
}
```

重启项目，下载`axios`

```shell
npm install axios --save
```

## `city`一级路由中获取真实数据

我们先获取入口函数模块`main.js`中，引用加载`axios`：

```js
// vue是面向对象的程序,vue就是一个类,我们可以对其添加一些方法
import axios from 'axios'
Vue.prototype.axios = axios // 这样完成添加之后,我们就可以通过this.axios来使用了
```

`vue`中我们都是在`mounted`生命周期中去获取我们的数据的：

```js
// 在City页面

export default {
   name: 'City',
   data() {
      return {
         // 将请求到的数据保存在这里,以做到响应式
         cityList: [],
         hotList: []
      }
   },
   mounted() {
      // 发起网络请求,请求数据
      this.axios.get('/api/cityList').then((res) => {
         // console.log(res)
         let msg = res.data.msg
         if(msg === 'ok') {
            // 获取到的所有数据
            let cities = res.data.data.cities 
            // [ { index: 'A',list: [{ name: '安徽',id: 1 },{ name: '埃及',id: 2 } ]} ] 将数据整理成这种结构
            // 封装一个函数,将原始数据传递进去,以达到格式化的效果
            var { cityList,hotList } = this.formatCityList(cities) // 对象的解构赋值
            this.cityList = cityList; // 将数据进行存储
            this.hotList = hotList;
         }
      })
   },
   methods: {
      formatCityList(cities) {
         let cityList = [] // 最后整理完成的数据
         let hotList = [] // 筛选出id为1的热门城市

         // 整理热门城市,一层判断就完成了
         for(var i = 0;i < cities.length;i ++) {
            if(cities[i].isHot === 1) {
               hotList.push(cities[i])
            }
         }
		 
         // 整理全部城市,并按首字母顺序进行排序
         for(var i = 0;i < cities.length;i ++) {
            // 截取第一个英文字母并转换为大写形式
            var firstLetter = cities[i].py.substring(0,1).toUpperCase()
            if(toCom(firstLetter)) { // 新添加index
               cityList.push({ index: firstLetter,list: [{ nm: cities[i].nm, id: cities[i].id }]})
            }else { // 累加到已有index索引中
               for(var j = 0;j < cityList.length;j ++) {
                  if(cityList[j].index === firstLetter) {
                     cityList[j].list.push({ nm: cities[i].nm, id: cities[i].id })
                  }
               }
            }
         }
		 // 仅判断是否有相同数据存在
         function toCom(firstLetter) {
            for(var i = 0;i < cityList.length;i ++) {
               if(cityList[i].index === firstLetter) {
                  return false
               }
            }
            return true
         }

         // console.log(cityList)
		
         // 将整理完成的数据按字母顺序进行整理排列
         cityList.sort((n1,n2) => {
            if(n1.index > n2.index) {
               return 1
            }else if(n1.index < n2.index) {
               return -1
            }else {
               return 0
            }
         })

         console.log(cityList)
         console.log(hotList)
          
         // 将最终数据return出去
         return {
            cityList,
            hotList
         }
      }
   }
}
```

![城市数据](D:\notes\Vue项目\mini_cinema\img\城市数据.jpg)

## 将真实数据渲染到页面上

```html
<div class="city_body">
  <div class="city_list">
     <!-- <div class="city_hot">
        <h2>热门城市</h2>
        <ul class="clearfix">
           <li>上海</li>
        </ul>
     </div>
     <div class="city_sort">
        <div>
           <h2>A</h2>
           <ul>
              <li>阿拉善盟</li>
              <li>鞍山</li>
              <li>安庆</li>
              <li>安阳</li>
           </ul>
        </div>
     </div> -->
     <div class="city_hot">
        <h2>热门城市</h2>
        <ul class="clearfix">
           <li v-for="item in hotList" :key="item.id">{{ item.nm }}</li>
        </ul>
     </div>
     <div class="city_sort">
        <div v-for="item in cityList" :key="item.index">
           <h2>{{ item.index }}</h2>
           <ul>
              <li v-for="itemList in item.list" :key="itemList.id">{{ itemList.nm }}</li>
           </ul>
        </div>
     </div>
  </div>
  <div class="city_index">
     <ul>
        <!-- <li>A</li>
        <li>B</li>
        <li>C</li>
        <li>D</li>
        <li>E</li> -->
        <li v-for="item in cityList" :key="item.index">{{ item.index }}</li>
     </ul>
  </div> 
</div>
```

## 点击右侧字母改变滚动条位置

![城市渲染完毕图](D:\notes\Vue项目\mini_cinema\img\城市渲染完毕图.jpg)

通过移动端触摸事件`touchstart`事件。

```vue
<div class="city_index">
    <ul>
        <!-- <li>A</li>
        <li>B</li>
        <li>C</li>
        <li>D</li>
        <li>E</li> -->
        // 将每一项的索引传递给事件handleToIndex
        <li v-for="(item,index) in cityList" :key="item.index" @touchstart="handleToIndex(index)">{{ item.index }}</li>
    </ul>
</div> 
methods: {
	handleToIndex(index) {
		
	}
}
```

```html
 <div class="city_sort" ref="city_sort"> // 我们在最外层的div处添加一个ref属性
    <div v-for="item in cityList" :key="item.index">
       <h2>{{ item.index }}</h2> // 这里是展示左侧 A B C D...的地方
       <ul>
          <li v-for="itemList in item.list" :key="itemList.id">{{ itemList.nm }}</li>
       </ul>
    </div>
 </div>
methods: {
	handleToIndex(index) {
		var h2 = this.$refs.city_sort.getElementsByTagName('h2') 
		// 找到h2的DOM元素的父元素,并将对应点击的目标元素距离顶部距离赋值给父元素
		this.$refs.city_sort.parentNode.scrollTop = h2[index].offsetTop
	}
}
```



# 完成正在热映和即将上映二级页面的数据

```
正在热映 - 接口文档
http://39.97.33.178/api/movieOnInfoList?cityId=10
```

```js
export default {
   name: 'NowPlaying',
   data() {
      return {
         movieList: []
      }
   },
   mounted() {
      this.axios.get('/api/movieOnInfoList?cityId=476').then((res) => {
         var msg = res.data.msg;
         if(msg === 'ok') {
            this.movieList = res.data.data.movieList;
         }
      })
   }
}
```

## 使用过滤器设置图片大小

```js
// 在入口模块main.js中
// 过滤url中,replace其w.h,替换为arg
Vue.filter('setWH',(url,arg) => {
    return url.replace(/w\.h/,arg)
})
```

```html
 <li v-for="item in movieList" :key="item.id">
    <div class="pic_show">
       <img :src="item.img | setWH('128.180')" alt=""> 
    </div>
    <div class="info_list">
       <h2>{{ item.nm }} <img v-if="item.version" src="@/assets/maxs.png" alt=""></h2> 
       <p>观众评 <span class="grade">{{ item.sc }}</span></p>
       <p>主演：{{ item.star }}</p>
       <p>{{ item.showInfo }}</p>
    </div>
    <div class="btn_mall">
       购票
    </div>
 </li>
```

以上就是正在热映真实数据渲染完毕。

即将到来页面与正在热映差不多。



# 搜索页面数据的渲染

数据的渲染是当我们在输入框中输入内容之后，才检索出数据的，所以数据的请求不是和之前一样在`mouted`里面进行了。

给`input`输入框添加双向数据绑定指令，用以获取到输入的值。

如何根据我们输入的值去获取数据，而且`axios`是异步进行获取数据的，所以`computed`计算属性不能使用，应该使用`watch`去监听。

```js
export default {
   name: 'Search',
   data() {
      return {
         message: '',
         moviesList: []
      }
   },
   watch: {
      // 我们需要实时去监听message字段的变化,所以下面只需要写上就能够监听到
      message(newValue) {
         this.axios.get('/api/searchList?cityId=10&kw=' + newValue).then(res => {
            var msg = res.data.msg;
            var movies = res.data.data.movies;
            if(msg && movies) {
               // 符合搜索要求的数据赋值给moviesList进行保存
               this.moviesList = res.data.data.movies.list;
            }
         })
      } 
   }
}
```

```html
<li v-for="item in moviesList" :key="item.id">
   <div class="img">
      <img :src="item.img | setWH('128.180')">
   </div>
   <div class="info"> 
      <p>
         <span>{{ item.nm }}</span>
         <span>{{ item.sc }}</span>
      </p>
      <p>{{ item.enm }}</p>
      <p>{{ item.cat }}</p>
      <p>{{ item.rt }}</p>
   </div>
</li>
```

## 函数防抖

当我们在上述输入框中频繁地输入多次内容的时候，`watch`就会触发多次。而我们所希望的是，当我们快速输入内容的时候，并不希望其发起请求，而当我们输入内容完毕之后，才发起请求。

- 方法1，使用`clearTimeout()`和`setTimeout()`每次发起请求的时候都会清除上次发起请求，已达到效果
- 方法2，使用`axios`终止多次请求

```js
methods: {
  // 1
  cancelRequest() {
     if(typeof this.source === 'function') {
        this.source('终止请求')
     }
  }
},
watch: {
  // 我们需要去监听message字段,所以下面只需要写上就能够监听到message数据的改变了
  message(newValue) {
     // 2
     var that = this;
     // 3
     this.cancelRequest();
     this.axios.get('/api/searchList?cityId=10&kw=' + newValue,{ // 4,再添加一个参数
        cancelToken: new this.axios.CancelToken(function(c) {
           that.source = c;
        })
     }).then(res => {
        var msg = res.data.msg;
        var movies = res.data.data.movies;
        if(msg && movies) {
           // 符合搜索要求的数据赋值给moviesList进行保存
           this.moviesList = res.data.data.movies.list;
        }
     }).catch(err => { // 5
        if(this.axios.isCancel(err)) {
           console.log('Rquest canceled', err.message); // 请求如果被取消,这里是返回取消的message
        }else {
           // handle error
           console.log(err);
        }
     })
  } 
}
```

![函数防抖](D:\notes\Vue项目\mini_cinema\img\函数防抖.jpg)

