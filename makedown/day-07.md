```
内容：路由进阶
讲师：范明剑
日期：2020-11-11
```

## 一、axios进阶

## 1.批量请求

当一个页面中需要发起多个网络请求时，可以使用批量请求来实现

第一步：先定义好网络请求的相关函数，有几个网络请求就定义几个函数，每个函数都返回请求的结果

```
getBanner(){
	return this.axios.get('/api/banner');
},
getPersonalized(){
	return this.axios.get('/api/personalized?limit=6')
},
getNewSong(){
	return this.axios.get('/api/personalized/newsong')
}
```

第二步：在页面挂载完成时使用all方法来批量发起网络请求

```
this.axios.all([this.getBanner(),this.getPersonalized(),this.getNewSong()]).then(
    this.axios.spread((bannerRes,personalizeRes,newsongRes)=>{
        this.bannerArr = bannerRes.data.banners;
        this.songListArr = personalizeRes.data.result;
        this.newSongArr = newsongRes.data.result;
	})
)
```



## 二、路由进阶

## 1.路由传参

### (1)动态路由

### (2)查询参数

如果可以接收的参数==数量不固定==时，使用动态路由就不合适了，需要使用查询参数方式。

第一步：定义一个固定链接的路由规则

```
// { path:'/playlist/:id',component:playlist },
{ path:'/playlist',component:playlist },
```

第二步：在列表页进行页面跳转时，传递参数时不使用动态路由的方式传递参数

```
playlist(obj){
//   this.$router.push("/playlist/"+id)
    this.$router.push({
        path:'/playlist',
        query:{id:obj.id,name:obj.name}
    })
},
```

使用查询参数的方式，传递的参数会自动以“?参数名=参数值&参数名=参数值”的方式展示在浏览器地址当中，第一个参数是?开头，其他参数均是&开头。

## 2.路由命名

路由规则中可以设置一个可选参数：name

它的作用是给当前路由规则设置一个重复的名称，在进行路由跳转并传递参数时，可以非常方便的实现。

第一步：定义路由规则时设置name选项

```
{ 
    path: "/playlist/:id", 
    component: playlist,
    name:'gedan'
}
```

第二步：在进行页面跳转时，使用name属性

```
playlist(id){
    //   this.$router.push("/playlist/" +id)
    this.$router.push({
        name:'gedan',
        params:{id}
    })
}
```

## 3.路由别名

alias

```
{ path: "recommend", component: recommend,alias:'tuijian' }
```

添加好别名后，原来的路由规则和别名的路由规则都可以正常访问。

## 4.路由模式

在路由配置文件中，和routes选项平级的一个项目

mode:

​	hash 哈希模式，默认

​	history 

hash模式会在浏览器地址中添加一个#，#号后面默认会当成路由规则进行解析，在路由跳转时，不会重新发起http请求

history模式在浏览器地址中没有#号，利用了html5的history.pushState来完成路由跳转，打包之后的项目部署上线后，一定要配合后端（Apache、Nginx等）web服务器进行代理的转发配置，否则会出现404的情况。

## 5.路由守卫【重点】

路由守卫的作用是对路由规则访问之前进行一定的验证，从而实现拦截的效果

大多数路由守卫钩子函数都有三个参数，分别是：

to 目标路由规则信息

from 来源路由规则信息

next 函数，用来执行或者改变路由规则

```
next()//执行默认路由规则
next('/index/recommend')//执行指定的路由规则
next(false);//中止默认的路由规则
```

根据作用范围不同，分成以下几种方式的路由守卫：

### (1)路由独享守卫

只有某一个路由规则才起作用的守卫

/src/router/index.js

```
{ 
    path: "/play/:id", 
    component: play,
    beforeEnter(to,from,next){
        next()//执行默认路由规则
        // next('/index/recommend')//执行指定的路由规则
        // next(false);//中止默认的路由规则
    }
}
```

此时，只会在访问http://localhost/play/xxxx这一个路由时，钩子函数才会触发

### (2)组件守卫

①beforeRouteEnter

路由规则访问到某一个页面组件之前，会自动触发beforeRouteEnter钩子函数

```vue
<script>
export default {
	beforeRouteEnter(to,from,next){
        //验证操作
        next();
    }
}
</script>
```

②beforeRouteLeave

要从某一个路由规则地址，跳转到另外一个路由规则时，这个钩子函数会自动触发

```vue
<script>
export default {
	beforeRouteLeave(to,from,next){
        var flag = window.confirm("确定要离开此页面吗？")
        if(flag){
            next();
        }
    }
}
</script>
```

③beforeRouteUpdate

动态路由参数变化时自动触发

如果路由模式是history时，此钩子函数失效

```vue
<script>
export default {
	beforeRouteUpdate(to,from,next){
		//验证操作
		next();
    }
}
</script>
```

### (3)全局守卫

/src/router/index.js

```
const router = new Router({...})
全局守卫代码....
export default router;
```

①beforeEach	全局前置守卫

在项目中==所有的==路由地址访问之前，会自动触发

②afterEach	全局后置守卫

它只有两个参数，分别是to和from，没有next

在项目中==所有的==路由地址访问之后，会自动触发，它只有两个参数（不能实现拦截操作）

```
//全局前置守卫
router.beforeEach((to,from,next)=>{
	//验证操作
    next();
});
//全局后置守卫
router.afterEach((to,from)=>{
    console.log(to,'to......')
    console.log(from,'from......')
});
export default router;
```

## 6.路由懒加载

```
{
    path:'/login',
    // component:login
    component:()=>import('../components/login')
}
```



# 三、项目打包

```
npm run build
```

项目打包完成后，会在项目根目录下生成一个dist文件夹