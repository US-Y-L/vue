```
内容：组件进阶-插槽、网络请求、路由
讲师：范明剑
日期：2020-11-09
```

# 一、组件进阶

## 4.插槽

在父组件中复用子组件时，子组件中展示内容/结构由父组件来告知。

slot

### (1)匿名插槽

第一步：在子组件中添加一个slot标签，用来展示父组件传递的内容（可以是数据也可以是html结构）

```vue
<template>
<div>
    <!-- 插槽 -->
    <slot></slot>
    <slot></slot>
</div>
</template>
```

只要在子组件中设置了slot标签，就可以在父组件中来分发内容，并且将分发的内容展示在插槽的位置上。

第二步：在父组件中使用子组件时，在子组件内部传递数据或者html结构

```vue
<script>
import index from './components/index'
export default {
	components:{ index }
}
</script>
<template>
	<div class="page">
		<index>
			<!-- 
				给子组件追加额外内容 
				此处内容可以是html代码
			-->
			<h1>这是首页</h1>
			<p>文本内容</p>
		</index>
	</div>
</template>
```

### (2)具名插槽

在子组件中添加slot标签给它设置name属性，用来区分不同的插槽，把父组件分发的不同内容展示在不同的插槽位置上。

```
<slot name="top"></slot>
<slot name="bottom"></slot>
```

在父组件使用子组件时，给追加的内容添加上slot属性，就可以把指定的内容放到指定的插槽上。

```
<index>
    <h1 slot="top">这是首页</h1>
    <p slot="bottom">文本内容</p>
</index>
```

### (3)作用域插槽

希望==父组件控制插槽展示的内容，子组件提供数据==

当==子组件做循环==或者==它的某一部分内容由父组件来分发==，这种场景要用到作用域插槽。

子组件遍历数据

```vue
<template>
    <div>
        <p>child组件</p>
        <ul>
            <slot name="list" v-for="item of arr" :item="item"></slot>
        </ul>
    </div>
</template>
<script>
export default {
    data(){
        return{
            arr:[ 'web前端','ui设计','java开发']
        }
    }
}
</script>
```

父组件分发子组件中要展示的html结构

```vue
<child>
    <!-- 作用域插槽 -->
    <template v-slot:list="props">
    	<li>{{ props.item }}</li>
    </template>
</child>
```

## 5.scoped

给组件的style标签添加scoped属性，可以实现让组件的样式只在当前组件内起作用，可以解决样式污染问题。

```
<style scoped></style>
```

# 二、网络请求

axios

## 1.安装

```
npm i axios
```

## 2.引入

```
import axios from 'axios'
```

## 3.使用

### (1)get请求

```
axios.get("请求地址").then(形参=>{
	...
})
```

### (2)代理转发配置

localhost:8080/关键词/链接地址

反向代理：

后端编写代码允许跨域

Apache/Nginx web服务器软件进行代理转发

webpack配置代理

在confing/index.js文件中修改proxyTable选项（代理映射表）

在module.exports = {

​	dev:{

​		里面修改

}

}	 ==配置文件修改之后要重启服务==

```javascript
proxyTable: {
    // /api是当前vue脚手架链接地址中出现哪个关键词时才进行代理的转发
    '/api':{
        target:'http://suggestion.baidu.com',//目标域名地址（只写域名）
            changeOrigin:true,//允许跨域
                pathRewrite:{
                    //路径重写规则（最终请求的链接地址中没有关键词时才需要进行配置）
                    '^/api':''
                }
    }
}
```

# 三、swiper轮播图插件

## 1.安装

```
npm i vue-awssome-swiper --save
```

## 2.引入

在/src/main.js中全局引入

```
import VueAwesomeSwiper from 'vue-awesome-swiper'
import 'swiper/swiper-bundle.css'
```

css文件一定要引入，否则没有样式效果

在需要使用swiper插件的页面中引入

```javascript
<script>
    import http from "axios";
    import { Swiper, SwiperSlide, directive } from 'vue-awesome-swiper'
    import Swiper2, { Navigation, Pagination, Autoplay } from "swiper"; 
    Swiper2.use([Autoplay, Navigation, Pagination]);
</script>
```

## 3.使用

示例代码：

```vue
<template>
  <div>
    <swiper 
      ref="mySwiper" 
      :options="swiperOptions"
    >
      <swiper-slide v-for="(banneritem,index) of bannerArr" :key="index">
        <img :src="banneritem.imageUrl"/>
      </swiper-slide>
      <div class="swiper-pagination" slot="pagination"></div>
      <div class="swiper-button-prev" slot="button-prev"></div>
      <div class="swiper-button-next" slot="button-next"></div>
    </swiper>
  </div>
</template>
<script>
import http from "axios";
import { Swiper, SwiperSlide, directive } from 'vue-awesome-swiper'
import Swiper2, { Navigation, Pagination, Autoplay } from "swiper"; 
Swiper2.use([Autoplay, Navigation, Pagination]);
export default {
  data(){
    return{
      bannerArr:[],
      swiperOptions: {
        autoplay:true,
        pagination: {
          el: '.swiper-pagination',
          clickable: true
        },
        navigation: {
          nextEl: ".swiper-button-next",
          prevEl: ".swiper-button-prev"
        }
      }
    }
  },
  components: {
    Swiper,
    SwiperSlide
  },
  directives: {
    swiper: directive
  },
  mounted() {
    //发起网络请求，获取轮播图信息
    http.get("/api/banner").then(res => {
      this.bannerArr = res.data.banners;
    });
  }
};
</script>

<style>
.swiper-container{
  width:400px;
  height:300px;
}
.swiper-container img{
  width:100%;height:100%;
}
</style>
```

# 四、路由【重点】

## 1.介绍

Vue Router 是 Vue.js 官方的路由管理器。它和 Vue.js 的核心深度集成，让构建==单页面应用==变得易如反掌。包含的功能有：

嵌套的路由/视图表
模块化的、基于组件的路由配置
路由参数、查询、通配符
基于 Vue.js 过渡系统的视图过渡效果
细粒度的导航控制
带有自动激活的 CSS class 的链接
HTML5 历史模式或 hash 模式，在 IE9 中自动降级
自定义的滚动条行为

SPA single page application

MPA multi page application【传统】

## 2.安装

### (1)初始化项目选择安装路由

### (2)手动安装

```
npm i vue-router --save
```

## 3.基本使用

(1)引入

/src/main.js

```
import VueRouter from 'vue-router';
Vue.use(VueRouter)
```

此时，vue实例上会多两个变量，一个$route，一个$router，但是它们的值是undefined

(2)配置路由映射表

```
import home from './components/home'
import music from './components/music'
const router = new VueRouter({
    // 路由映射配置表
    routes:[
        { path:'/home',component:home },
        { path:'/music',component:music}
    ]
})
```

(3)把vuerouter实例，挂在到vue实例上

```
new Vue({
    el: '#app',
    // router:router,
    router,
    components: { App },
    template: '<App/>'
})
```

把vuerouter实例挂到vue上之后，浏览器地址中会自动增加一个#

#号后面的内容是路由规则，当路由映射表中的path属性匹配成功之后，对应的页面内容就可以展示

(4)设置路由出口

App.vue

```
<router-view />
```

## 4.路由导航

### (1)内置标签

router-link 会在页面上生成一个a标签，并可以设置导航按钮的激活状态

to ==必须要设置==的属性，给导航设置==目标路由地址==

active-class 可选属性，给导航设置激活状态

### (2)编程式导航

$router是vuerouter的实例，它的原型上提供一系列的方法供我们使用

①push

```
$router.push(目标路由地址)
```

②replace

```
$router.replace(目标路由地址)
```

push方法会在进行页面跳转之前，记录跳转之前访问的路由地址

replace方法用指定的路由地址替换当前访问的路由地址

③go

回退到上一次访问的页面，一般给一个-1的实参

```
$router.go(-1)
```

## 5.路由重定向

redirect

```vue
export default new Router({
  routes: [
    { path:'/recommend',component:recommend },
    { path:'/songlist',component:songlist },
    { path:'/search',component:search },
    { path:'*',redirect: '/recommend'}
    // { path:'*',component: recommend}  不要写成这样，错的！！！
  ]
})
```

## 6.路由嵌套

如果在某个页面中还要展示不同的子级页面，那么需要用到路由嵌套

第一步：定义子级页面

第二步：定义子级页面对应的路由规则

```javascript
import Vue from 'vue'
import Router from 'vue-router'
Vue.use(Router)
import index from '../components/index'
import play from '../components/play'
import recommend from '@/components/recommend'
import songlist from '../components/songlist'
import search from '../components/search'
import recomlist from '../components/recomlist'
export default new Router({
  routes: [
    {
      path:'/index',component:index,
      children:[
        { path:'recommend',component:recommend },
        { path:'songlist',component:songlist },
        { path:'search',component:search },
        { path:'',redirect:'recommend' }
      ]
    },
    { path:'/play',component:play },
    { path:'/recomlist/:id',component:recomlist },
    { path:'*',redirect: '/index'}
  ]
})

```

在首页中再添加一个router-view标签，用来展示子级页面内容。

```vue
<template>
  <div>
    <!-- 路由导航 -->
    <div class="nav">
        <!-- router-link必须要设置to属性，类似a标签的href属性 -->
        <router-link to="/index/recommend" active-class="active">推荐音乐</router-link>
        <router-link to="/index/songlist" active-class="active">热歌榜</router-link>
        <router-link to="/index/search" active-class="active">搜索</router-link>
    </div>
    <!-- 展示二级路由对应的页面组件 -->
    <router-view></router-view>
  </div>
</template>
```

## 7.路由传参

### (1)动态路由

第一步：定义一个动态路由规则，让不同参数的路由地址指向到同一个页面组件

/src/router/index.js

```
{ path:'/recomlist/:id',component:recomlist }
```

$route是当前路由地址的详细信息

$router是vuerouter的实例，包含内置属性和方法（页面跳转等）

第二步：获取动态路由的参数

```
this.$route.params.动态路由参数名称
```

```javascript
//$route和$router的区别
//1.$router是VueRouter的实例方法,可以认为是全局的路由对象，包含了所有路由的对象和属性。
//2.$route是一个跳转的路由对象，可以认为是当前组件的路由管理，指当前激活的路由对象，包含当前url解析得到的数据，可以从对象里获取一些数据。如name,path等。

//参考网址 https://blog.csdn.net/m0_46403582/article/details/104501106?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522160493209619725222463537%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=160493209619725222463537&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_click~default-1-104501106.pc_first_rank_v2_rank_v28&utm_term=vue%E4%B8%ADrouter%E4%B8%8Eroute%E7%9A%84%E5%8C%BA%E5%88%AB&spm=1018.2118.3001.4449
```

## vue中route和router的区别（在上面）

# 作业：网易云音乐案例

接口地址：

1.推荐音乐页面

(1)轮播图

/banner

(2)推荐歌单

/personalized

(3)最新音乐

/personalized/newsong

2.歌单详情页面

/playlist/detail?id=歌单编号

3.热歌榜

top/list?id=3778678

4.歌曲详情页面

/song/detail?ids=1481164987

5.搜索页面

(1)热搜词

/search/hot

(2)搜索

/search/?keywords=关键词