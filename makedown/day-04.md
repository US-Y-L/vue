```
内容：过渡动画、组件、脚手架
讲师：范明剑
日期：2020-11-05
```

# 一、过渡动画

使用场景：v-if、v-show、动态组件、组件根节点

## 1.内置类名

```
<transition>
	<要使用过渡动画效果的标签></要使用过渡动画效果的标签>
</transtion>
```

### (1).匿名动画

进入

​	进入开始			v-enter

​	进入进行中		v-enter-active

​	进入结束			v-enter-to

离开

​	离开开始			v-leave

​	离开进行中		v-leave-active

​	离开结束			v-leave-to

示例代码：

```vue
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <style>
        .box{
            width: 300px;
            height: 300px;
            background-color: red;
        }
        .v-enter,.v-leave-to{ opacity: 0; }
        .v-enter-active,.v-leave-active{ transition:1.5s linear; }
        .v-enter-to,.v-leave{ opacity: 1; }
    </style>
</head>
<body>
    <div id="app">
        <transition>
            <div class="box" v-if="show"></div>
        </transition>
        <button @click="show=!show">切换</button>
    </div>
    <script>
        new Vue({
            el:"#app",
            data:{
                show:true
            }
        })
    </script>
</body>
</html>
```

### (2)具名动画

页面中有多个标签要设置不同的过渡动画效果时，需要给transition标签设置name属性，设置name属性之后，内置类名的前缀就要改为对应的name属性。

```vue
<style>
	/* 具名过渡动画 */
    .move-enter,.move-leave-to{left:0;}
    .move-enter-active,.move-leave-active{ transition:1.5s linear;background-color: blue; }
     .move-enter-to,.move-leave{ left: 600px; }
    </style>
<transition name="move">
	<div class="box2" v-if="show"></div>
</transition>
```

## 2.animate.css动画库

(1)安装

```
npm i animate.css
```

(2)引入

```
<link rel="stylesheet" href="./node_modules/animate.css/animate.css">
```

(3)使用

需要给transition标签设置两个属性

enter-active-class	进入的动画

leave-active-class   离开的动画

示例代码：

```vue
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>过渡动画-动画库</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <link rel="stylesheet" href="./node_modules/animate.css/animate.css">
    <style>
        .box{
            width: 300px;
            height: 300px;
            background-color: red;
        }
    </style>
</head>
<body>
    <div id="app">
        <button @click="show=!show">切换</button>
        <transition 
            enter-active-class="animate__animated animate__bounceInUp"
            leave-active-class="animate__animated animate__bounceOutLeft"
        >
            <div class="box" v-if="show"></div>
        </transition>
        
    </div>
    <script>
        new Vue({
            el:"#app",
            data:{
                show:true
            }
        })
    </script>
</body>
</html>
```

## 二、组件【重点】

## 1.介绍

组件即零件，组件系统是 Vue 的另一个重要概念，因为它是一种抽象，允许我们使用小型、独立和通常可复用的组件构建大型应用

是一个局部的页面功能模块

组件是==可复用==的 ==Vue 实例==

因为组件是可复用的 Vue 实例，所以它们与 new Vue 接收相同的选项，例如 data、computed、watch、methods 以及生命周期钩子等。仅有的例外是像 el 这样根实例特有的选项。

## 2.注册

### (1)局部注册

```
new Vue({
	components:{
		组件名称1:{
			template:"组件模板内容"
		},
		组件名称N:{
			template:"组件模板内容"
		}
	}
})
```

### (2)全局注册

```
Vue.component("组件名称1",{ template:"组件模板内容" })
Vue.component("组件名称N",{ template:"组件模板内容" })
```

## 3.使用

在vue的挂载点内，以标签的方式来使用组件

在vue的挂载点内使用组件后，组件名称的所在位置会被组件的template内容进行替换。

示例代码：

```vue
<div id="app">
    <!-- 使用组件 -->
    <mycom></mycom>
    <mycom></mycom>
    <mycom></mycom>
</div>
<script>
    new Vue({
        el:"#app",
        components:{
            mycom:{
                // 组件的template中只能有一个根标签
                template:"<div><h1>这是一个自定义组件</h1><h2>标题2</h2></div>"
            }
        }
    })
</script>
```

## 4.注册组件注意事项

(1)组件名称不能是系统内置的标签名

(2)组件名称不能是系统内置的大写的标签名

(3)如果组件名称中包含了大写字母，在使用要把大写字母转换成-小写字母

(4)template内容中只能有一个根标签

## 5.template的使用

### (1)vue实例的template选项

组件中可以通过template属性来设置该组件的模板内容

vue实例上也可以设置一个template属性，用来把指定的template对应的内容或者组件，替换到vue实例的挂载点内。

示例代码：

```vue
<script>
	new Vue({
        el:"#con",
        // template:"<h2>vue实例的template属性</h2>"
        template:"<my-div />",
        components:{
            myDiv:{
                template:"<h1>自定义组件</h1>"
            }
        }
    });
</script>
```

### (2)template标签

如果组件中的模板内容比较多时，使用字符串的方式编写html代码会很麻烦，且没有任何语法提示，如果代码编写不够熟练，非常容易出现错误。

vue中给提供了一个template标签，用来存放组件的模板内容

示例代码：

```vue
<div id="app">
    <mycom></mycom>
</div>
<template id="mycom">
	<div>
    	<h1>自定义组件</h1>
    </div>
</template>
<script>
    new Vue({
        el:"#app",
        components:{
            mycom:{
                // 让组件的template属性和template标签关联起来
                template:"#mycom"
            }
        }
    })
</script>
```

## 6.组件的key属性（重要）

组件遍历循环时，需要给组件添加一个唯一的标识，能够加快页面的渲染速度，页面根据key属性去找到改变的那个组件，直接重新渲染

## 7.组件中的data应该是一个函数

由于对象是引用类型，所以在data中以对象的方式定义初始数据，然后在组件中使用数据并复用这个组件时，会产生数据上的冲突（也就是一个数据改变了之后，其他的也跟着进行变化）

所以，在==组件中定义初始数据，需要以函数的方式返回一个新的对象来定义数据==，这样每复用一次组件，就会调用一次data的函数并返回一个新的数据存储空间，这样组件与组件之间的数据就不会互相影响了。

实例代码：

```vue
<div id="app">
    <v-box></v-box>
    <v-box></v-box>
    <v-box></v-box>
</div>
<template id="box">
<div class="box">
    <p>数量：{{ num }}</p>
    <button @click="num++">增加数量</button>
    </div>
</template>
<script>
    new Vue({
        el:"#app",
        components:{
            vBox:{
                template:"#box",
                data:function(){
                    return{
                        num:100
                    }
                }
            }
        }
    })
</script>
```

注意：在组件中，不能直接使用vue根实例上的data数据，因为组件也是一个vue实例，实例与实例之间的数据是不共享的。

# 三、vue-cli脚手架

最新版本：4.X

稳定版本：2.9.6

## 1.安装

node环境

(1)webpack

```
npm i webpack -g
```

(2)vue-cli

```
npm i vue-cli -g
```

## 2.初始化项目

进入到一个指定的目录中，然后进入命令行

```
vue init webpack mydemo
```

安装过程：

```
Project name (mydemo) 	项目名称，如果不需要修改直接回车
Project description (A Vue.js project) 项目描述，如果不需要修改直接回车
Author (fan <fanmingjian@offcn.com>) 项目作者，如果不需要修改直接回车
Vue build (Use arrow keys) vue构建方式，选择默认的热编译
> Runtime + Compiler: recommended for most users
  Runtime-only: about 6KB lighter min+gzip, but templates (or any Vue-specific HTML) are ONLY allowed in .vue files - re
nder functions are required elsewhere
Install vue-router? (Y/n) 是否安装vue路由，输入n
Use ESLint to lint your code? (Y/n) 是否安装eslint代码验证工具，输入n
Set up unit tests (Y/n) 是否创建单元测试，输入n
Setup e2e tests with Nightwatch? (Y/n) 是否创建端对端测试，输入n
Should we run `npm install` for you after the project has been created? (recommended) (Use arrow keys) 选择依赖安装方式，选择npm
> Yes, use NPM
  Yes, use Yarn
  No, I will handle that myself
```

## 3.运行项目

在命令行中，进入项目根目录，执行如下命令：

```
npm run dev
```

localhost:8080

## 4.目录结构

```
mydemo
	build		项目打包主程序目录
	config		项目配置文件目录
	node_modules 项目依赖目录
	src			项目源码目录
		assets	项目静态资源目录（参与打包）
		components 项目组件目录
		App.vue	项目根组件
		main.js	项目启动文件
	static		项目静态资源目录
	index.html	项目首页
	package.json项目依赖配置文件
```

main.js:

入口js，创建vue实例

项目运行流程：

index.html

/src/main.js

/src/App.vue

## 5.vue组件构成

在脚手架中，组件以单独的.vue后缀的文件存在，组件由三部分组成：

template模板，只有一个，所以不需要设置id属性

script 组件逻辑代码【非必须】

style 组件样式代码【非必须】

所有的组件都要先从App.vue开始，它是脚手架的根组件，在App.vue要引用其他组件的话需要使用import关键词

示例代码：

创建一个Home组件

/src/components/Home.vue

```vue
<template>
    <div>
        <h1>home组件</h1>
        <v-top></v-top>
    </div>
</template>
<script>
import vTop from './Top'
export default { //用于配置对象 与Vue一致
    //组件中的data一定要是一个函数 为了不被多个组件使用所影响
    components:{ vTop}
}
</script>
```

/src/App.vue

```vue
<script>
// 引入Home组件
import Home from './components/Home'
export default {
	components: {
		Home
	}
}
</script>
<template>
	<div id="app">
		<Home />
	</div>
</template>

<style>
/* 全局样式 */
h1 { color: red;}
</style>
```



# 常见错误

1.[Vue warn]: Unknown custom element: <mycom> - did you register the component correctly? For recursive components, make sure to provide the "name" option.

组件没有正确的注册就使用了

2.vue.js:634 [Vue warn]: Do not use built-in or reserved HTML elements as component id: div

不使用系统内置标签作为组件名称



3.[Vue warn]: The "data" option should be a function that returns a per-instance value in component definitions.

