## uniapp起步

### 1.在HBuilder X中预览程序在微信开发者工具里

```
微信开发者工具: 设置 --> 安全设置 --> 服务端口 --> 开启
HBuilder X: 运行 --> 运行到小程序模拟器 --> 微信开发者工具
```

### 2.HBuilder X中的目录结构

```
manifest.json: 配置APP的应用标识,描述,版本号,图标,启动界面,SDK配置,权限模块的配置;
pages.json: 把每一个页面都配置进来,每个页面都是一个对象,所有页面放进一个数组里;
main.js: 全局js文件,全局交互行为可以写进这个文件里;
pages: 核心文件,遵循单文件组件规范;
```

### 3.uniapp框架

```
文档规范 ---> Vue单文件组件规范
组件规范 ---> 小程序组件规范
js接口能力 ---> 微信小程序规范
```

```
Vue单文件组件规范:
每个.vue文件最多包含一个<template>块
每个.vue文件最多包含一个<script>块
每个.vue文件可以包含多个<style>标签
```

### 4.尺寸和单位

```
uniapp的基准宽度为750px
```

### 5.样式导入

```
<style>
	@import "../common/uni.css"
</style>

重复度较高的样式可写进全局样式文件里;
重复度较低的样式可定义在当前文件里;
```

### 6.配置相关 

`pages.json`偏向微信小程序，可设置导航栏背景颜色、标题颜色、标题文字内容、窗口背景色、下拉`loading`样式、是否开启下拉刷新、页面上拉触发时距页面底部距离、导航栏样式、顶部窗口背景色、底部窗口背景色

```
globalStyle: 设置应用的状态栏、导航栏、标题、窗口背景色等;
```

```
pages: 新建页面必须在pages里进行注册,一个页面就是一个对象;

{
	"path": "pages/about/about", //首项是入口页
	"style": {
		"navigationBarTitleText": "uni about"
	}
}
```

`tabBar`配置底部路由切换，最少配置两个，最多配置5个，按数组顺序进行排序。可设置`tab`上文字默认颜色，文字选中时的颜色，背景色，上边框色等

```
pages.json中

"tabBar": {
	"color": "#000000", //默认是黑色
	"selectedColor": "#0faeff", //激活后的颜色
	”backgroundColor": "#ffffff", //tabBar整体背景颜色

	"list": [
		{
			"pagePath": "pages/index/index", //配置页面路径,首页
			"iconPath": "static/image/icon_conmonent.png", //未激活时图标的样式
			"selectedIconPath": "static/image/icon_component_HL.png", //激活后图标的样式
			"text": "首页" //文字内容
		}
	]
}
```

### 7.生命周期函数

```
onLoad: 监听页面加载,其参数为上一页面传递过来的数据,类型为Object;
onshow: 监听页面显示;
onReady: 监听页面初次渲染完成;
onHide: 监听页面隐藏;
onUnload: 监听页面卸载;
onPullDownRefresh: 监听用户下拉动作(下拉刷新);
onReachBottom: 页面上拉触发事件处理函数(页面触底加载更多);
onShareAppMessage: 用户点击右上角分享(微信小程序);
onPageScroll: 监听页面滚动;
onTabItemTap: 当前是tab页情况下,点击tab触发;
--------------------------------------------------------
<script>
	export default{
		data: {
			title: 'hello...'
		},
		onLoad: function(e) {
			console.log('onLoad');
		},
		onShow: function(e) {
			console.log('onShow')
		},
		onHide: function(e) {
			console.log('onHide')
		}
	}
</script>
```

### 8.基础组件

```
view 视图容器
```

```
swiper 轮播图
```

```
text 文本
```

```
rich-text 富文本
```

```
progress 进度条
```

```
scroll-view 横向滚动和竖向滚动

<scroll-view scroll-x="true"> //开启横向滚动
	<view>1</view>
	<view>2</view>
	<view>3</view>
</scroll-view>
```

