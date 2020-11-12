```
第08讲
内容：vuex、stylus
讲师：范明剑
日期：2020-11-12
```

# 一、状态管理-vuex【重点】

组件通信：

​	父子组件、子父组件、非父子组件

​	ref

​	localStorage、sessionStorage

​	vuex

## 1.介绍

Vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式**。它采用==集中式==存储管理应用的所有组件的状态，并以相应的规则保证状态以一种==可预测==的方式发生变化。Vuex 也集成到 Vue 的官方调试工具 [devtools extension](https://github.com/vuejs/vue-devtools)，提供了诸如零配置的 time-travel 调试、状态快照导入导出等高级调试功能。

## 2.安装

```
npm i vuex
```

## 3.引入

```
import Vuex from 'vuex'
Vue.use(Vuex)
```

## 4.基本使用

核心概念：

### (1)state

Vuex 使用单一状态树——是的，用一个对象就包含了全部的应用层级状态。至此它便作为一个“唯一数据源 (SSOT)”而存在。这也意味着，每个应用将仅仅包含一个 store 实例。

类似于组件中的data

/src/main.js

```javascript
import Vuex from 'vuex'
Vue.use(Vuex)
//初始化仓库
const store = new Vuex.Store({
    //初始数据
    state: {
        name: 'vuex name'
    }
})
new Vue({
    el: '#app',
    router,
    components: { App },
    template: '<App/>',
    //store: store 
    store
})
```

在任意页面组件中就可以直接通过$store.state来获取仓库中的数据

```
<p>仓库中的name为：{{ $store.state.name }}</p>
```

助手函数

mapState，从把仓库中的所有数据全部获取到，并根据需要在页面组件中使用指定的数据

```vue
<template>
	<div>
    	<p>仓库中的newage为:{{ age }}</p>
    </div>
</template>
<script>
import { mapState } from 'vuex'
export default {
    //从仓库中获取到数据，并存放到当前页面的计算属性中
    //变量名和仓库中的一致时的写法
    computed:{
        ...mapState(['name','age'])
    },
    //变量名和仓库中的不一致时的写法
    // computed:mapState({
    //     username:state=>state.name
    // })
}
</script>
```

### (2)getters

根据state中已有的数据，派生出新的数据，类似于组件中的computed

第一步：在仓库中定义好getter规则

```javascript
const store = new Vuex.Store({
    //初始数据
    state: {
        uname: 'vuex name',
        age:18
    },
    //计算属性
    getters:{
        newage(state){
            return state.age + 5;
        }
    }
})
```

第二步：在页面组件中使用getters中的数据

```
<p>仓库中的newage为：{{ $store.getters.newage }}</p>
```

注意：不要从state里获取数据，vuex的计算属性并没有把派生出的数据放到state中。

助手函数

```javascript
import { mapState,mapGetters } from 'vuex'
computed:{
    ...mapState(['uname','age']),
    ...mapGetters(['newage'])//从所有的getters获取指定的计算属性结果
}
```

### (3)mutations

更改 Vuex 的 store 中的状态的==唯一方法==是提交 mutation。

第一步：先在仓库中定义好改变state的方法

```javascript
const store = new Vuex.Store({
    ...
    //修改state的事件函数
    mutations:{
        changeAge(state){
            state.age+=2;
        },
        changeName(state,data){
            state.uname = data;
        }
    }
})
```

第二步：在页面组件中使用定义好的修改state的方法

这样在页面组件中就不用再定义修改数据的方法了，能够实现数据的集中式可预测性变化

```
<button @click="$store.commit('changeAge')">改变仓库中的age</button>
<!-- 传递额外参数 -->
<button @click="$store.commit('changeName','home name')">改变仓库中的name</button>
```

助手函数

如果改变state的函数有很多，而某一个页面组件只需要使用其中的一部分时，可以在当前页面组件中使用助手函数把对应的mutations方法引出来

```js
<template>
	<div>
    	<p>仓库中的newage为:{{ age }}</p>
        <button @click="changeAge">改变age</button>
    </div>
</template>
<script>
import { mapState,mapGetters,mapMutations } from 'vuex'
export default {
    methods:{
        ...mapMutations(['changeAge'])
    }
}
</script>
```

使用mapMutations助手函数后，就可以在页面中像使用自定义函数的方式来使用仓库中定义好的mutations了。

mutations只能执行同步操作

### (4)actions

Action 类似于 mutation，不同在于：

Action 提交的是 mutation，而不是直接变更状态。
Action 可以包含任意异步操作。

第一步：在仓库中定义好actions

```
//初始化仓库
const store = new Vuex.Store({
    ...
    //定义可以执行异步操作的action
    actions:{
        // context 上下文
        CHANGEAGE(context){
            // console.log(context)
            setTimeout(function(){
                context.commit("changeAge")
            },3000);
        }
    }
})
```

第二步：在页面组件中通过dispatch来触发actions

```
<button @click="$store.dispatch('CHANGEAGE')">改变仓库中的age-action方式</button>
传递参数
<button @click="$store.dispatch('CHANGEAGE',10)">改变仓库中的age-action方式</button>
```

助手函数

```vue
<template>
    <div>
        <h1>child组件</h1>
        <button @click="changeAge">改变age</button>
        <button @click="CHANGEAGE">改变age-action</button>
    </div>
</template>
<script>
import { mapMutations,mapActions } from 'vuex'
export default {
    methods:{
        ...mapMutations(['changeAge']),
        ...mapActions(['CHANGEAGE'])
    }
</script>
```

此时，在当前页面组件中就可以直接调用CHANGEAGE这个action，来执行一些异步的操作。

### (5)modules

由于使用单一状态树，应用的所有状态会集中到一个比较大的对象。当应用变得非常复杂时，store 对象就有可能变得相当臃肿。

为了解决以上问题，Vuex 允许我们将 store 分割成模块（module）。每个模块拥有自己的 state、mutation、action、getter

第一步：创建一个shop模块，并在其中定义好state和mutations等

/src/store/shop/index.js

```js
export default{
    state:{ num:200 },
    mutations:{
        changeNum(state){
            state.num+=1;
        }
    }
}
```

第二步：在根模块中设置一个子模块

/src/store/index.js

```js
export default new Vuex.Store({
    ...
    modules:{ shop }
})
```

第三步：在页面中调用根模块和shop模块的数据及改变数据的方法

```vue
<p>仓库中根模块的num为：{{ $store.state.num }}</p>
<p>仓库中shop模块的num为：{{ $store.state.shop.num }}</p>
<button @click="$store.commit('changeNum')">修改num</button>
```

此时会发现点击按钮后根模块和shop模块的num变量都发生了变化

为了解决这个问题，可以给模块启用命名空间、

/src/store/shop/index.js

```
export default{
    namespaced:true,//启用命名空间
    state:{
        num:200
    },
    mutations:{
        changeNum(state){
            state.num+=1;
        }
    }
}
```

namespaced:true 表示启用命名空间

然后在页面中调用shop模块中的mutation

```
<button @click="$store.commit('shop/changeNum')">修改shop模块的num</button>
```

# 二、stylus样式预处理器

## 1.介绍

- 冒号可有可无
- 分号可有可无
- 逗号可有可无
- 括号可有可无
- 变量
- 插值（Interpolation）
- 混合（Mixin）
- 数学计算
- 强制类型转换
- 动态引入
- 条件表达式
- 迭代
- 嵌套选择器
- 父级引用
- Variable function calls
- 词法作用域
- 内置函数（超过 60 个）
- 语法内函数（In-language functions）
- 压缩可选
- 图像内联可选
- Stylus 可执行程序
- 健壮的错误报告
- 单行和多行注释
- CSS 字面量
- 字符转义
- TextMate 捆绑

## 2.安装

```
npm i stylus-loader@3.0.2 stylus --save
```

## 3.引入

```
<style lang="stylus">
```

## 4.基本使用

(1)基础语法

```
<style lang="stylus">
    .page
        width 100vw
        height 100vh
        display flex
        flex-direction column
        .top
            height 50px
            background-color skyblue
        .middle
            flex 1
            display flex
            .menu
                width 100px
                background-color:#000
            .right
                flex 1
                background-color #fff
            
</style>
```

(2)变量的使用

可以在预先定义好一些项目页面中会使用到的样式属性值，并编写到一个styl后缀的文件中，当成这些属性值的配置文件使用，这样需要调整相关属性值时就不需要改页面组件了，直接修改这个配置文件即可，

第一步：创建一个color.styl文件，并在里面定义好几个变量

/src/assets/css/color.styl

```
$bgcolor1 = #8B8B00
$bgcolor2 = #8B6508
$bgcolor3 = #ffffff
```

第二步：在页面组件中使用变量

```
<style lang="stylus">
// 引入样式文件
@import('../assets/css/color.styl')
    .page
        width 100vw
        height 100vh
        display flex
        flex-direction column
        .top
            height 50px
            // 使用预定义好的变量
            background-color $bgcolor1
        .middle
            flex 1
            display flex
            .menu
                width 100px
                background-color:$bgcolor2
            .right
                flex 1
                background-color $bgcolor3
            
</style>
```

这样做的好处是，只需要改变变量对应的值，而页面组件中只需要使用变量名即可。

(3)函数的使用

第一步：创建函数文件并编写重复使用的样式代码

创建一个fn.styl文件，里面封装一些函数，把会重复使用到的样式作为函数的内容

/src/assets/css/fn.styl

```
fullscreen(){
    width 100vw
    height 100vh
}
```

第二步：在页面组件中使用函数

```
<style lang="stylus">
// 引入样式文件
@import('../assets/css/fn.styl')
    .page
        fullscreen()
</style>
```

这样在其他页面组件中如果需要使用相同的样式代码时，只需要调用函数名称即可。