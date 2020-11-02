```
内容：vue框架基础
讲师：范明剑
日期：2020-11-02
```

# 一、框架介绍

## 1.什么是框架

框架，framework，它是一个网站的半成品，能够让开发人员能够专注于业务逻辑的处理。

## 2.vue框架

作者：尤雨溪

官网：https://cn.vuejs.org/

渐进式的Javascript框架

## 3.第四阶段主要学习内容

vue框架

​	vue、路由、vue-cli脚手架、axios、stylus、UI库、vuex状态管理、typescript、服务器端渲染

react框架

​	react、jsx语法糖、脚手架、路由

## 4.vue框架入门

vue两大核心：

数据驱动页面

组件化开发

### (1)优点

①体积小

②运行速度快（轻量级框架）

③虚拟DOM机制

④指令系统

⑤组件化开发

⑥生态系统繁荣

### (2)缺点

①兼容性，不兼容ie8

②报错相对来说不是特别的清晰

### (3)vue框架介绍

Vue (读音 /vjuː/，类似于 **view**) 是一套用于==构建用户界面==的**渐进式框架**。与其它大型框架不同的是，Vue 被设计为可以==自底向上==逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与[现代化的工具链](https://cn.vuejs.org/v2/guide/single-file-components.html)以及各种[支持类库](https://github.com/vuejs/awesome-vue#libraries--plugins)结合使用时，Vue 也完全能够为复杂的==单页应用==提供驱动。

### (4)安装vue

①直接引入外部js

```
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```

②npm安装

```
npm i vue
```

安装好之后直接引入

```
<script src="./node_modules/vue/dist/vue.js"></script>
```

使用npm安装或者初始化项目前，一定要确认是否设置淘宝镜像地址（这样下载速度会比较快）

```
npm config set registry https://registry.npm.taobao.org
```

# 二、框架基本使用

## 1.实例化vue

```
new Vue({配置选项})
```

el:设置vue的作用范围，一般使用的id选择器，能够保证唯一性

el的挂载点不能是HTML、body这样的标签，应该是一个普通标签

## 2.设置初始数据

```
new Vue({
	el:"#app",
	data:{
		msg:"这里是初始数据"
	}
})
```

## 3.函数的声明和使用

自定义函数在methods选项中进行声明

```
new Vue({
	el:"",
	data:{},
	methods:{
		函数名称:function(){},
		函数名称N(){}
	}
})
```

使用自定义函数要在vue的挂载点内

```
<div id="app">
    {{ 函数名称() }}
</div>
```

# 三、指令系统

## 1.内容展示

(1)文本插值语法 {{ }}

mustache语法，双花括号内，可以写单行js语法

```
<p>{{ 10 + 20 }}</p>
<p>{{ status ? '已登录' : '未登录' }}</p>
<p>{{ '欢迎：' + name }}</p>
<p>欢迎：{{name}}</p>
```

vue中的指令都是以v-开头，都写在标签的开始标签上，作为一个属性去使用。

```
<标签名 指令=“”></标签名>
```

(2)v-text

v-text会把指定的内容展示到指定标签内部，类似原生JS中的innerText

(3)v-html

v-html会把指定的内容展示到指定标签内部，同时，它可以解析html语法，类似原生js中的innerHTML

文本插值语法和v-text、v-html的区别

1.它们都可以在页面中展示指定的内容

2.不同的是，v-text和v-html用于展示固定内容，如果要展示内容是某段文字中的一部分时，要使用文本插值语法。

```
<p v-text='name'></p>
<p v-html="name"></p>
```

## 2.条件判断

(1)v-if

​	v-if、v-else

​	v-if、v-else-if

单路分支

```
<标签 v-if="布尔值或者条件表达式"></标签>
```

如果布尔值或者条件表达式返回值为true，指定的标签会在页面结构中存在，否则就不存在。

双路分支

```
<标签1 v-if="布尔值或者条件表达式"></标签1>
<标签2 v-else></标签2>
```

多路分支

```
<标签1 v-if="布尔值或者条件表达式"></标签1>
<标签2 v-else-if="布尔值或者条件表达式N"></标签2>
...
<标签2 v-else></标签2>
```

(2)v-show

```
<标签1 v-show="布尔值或者条件表达式"></标签1>
```

不论布尔值或者条件表达式的返回值是什么，标签都会在页面结构中存在

当布尔值或者条件表达式的返回值为true时，标签会在页面结构中显示

当布尔值或者条件表达式的返回值为false时，会给标签添加一个display：none属性

v-if 惰性加载，满足条件了才会在页面结构存在指定的标签

v-show 只是改变了display属性，如果需要频繁的控制元素显示/不显示时，推荐使用v-show

==v-show不能和v-else配合使用==

## 3.事件绑定

```
<标签 v-on:事件名="一行js代码或者自定义函数名称"></标签>
```

可以简写成：

```
<标签 @事件名="一行js代码或者自定义函数名称"></标签>
```

## 4.属性绑定

### (1)普通属性绑定

当需要让标签的属性按照既定的规律进行变化时，可以通过v-bind进行绑定，这样属性绑定的变量值发生变化后，对应的属性也会跟着进行变化。

```
<标签 v-bind:属性名=“属性值”></标签>
```

可以简写成

```
<标签 :属性名=“属性值”></标签>
```

示例代码：

```vue
<div id="app">
    <img v-bind:src="arr[showidx]" />
    <button @click="showidx--">上一张</button>
    <button @click="showidx++">下一张</button>
</div>
<script>
    var vm = new Vue({
        el:"#app",
        data:{
            showidx:0,
            arr:[
                '3.jpg',
                '2.jpeg',
                '1.jpg'
            ]
        },
        methods: {
            //下一张的点击事件
            next(){
                if(this.showidx >= (this.arr.length-1)){
                    return;//不能让showidx数组下标无条件的累加，return之后的代码就不再执行了
                }
                this.showidx++;
            },
            //上一张的点击事件
            prev(){
                if(this.showidx<=0){
                    return;
                }
                this.showidx--;
            }
        }
    })
</script>
```

此案例中，showidx就是要展示图片的下标值，当下标值发生变化时，就会根据下标从数组中获取对应的图片地址并绑定给src属性，数据变化后，页面会自动重新渲染。

### (2)特殊属性绑定（动态样式绑定）

#### style

①变量的写法

```vue
<div id="app">
    <p :style="styleFont">破纪录！袁隆平团队双季稻晚稻亩产911.7公斤</p>
    <button @click="changeStyle('red')">红色</button>
    <button @click="changeStyle('blue')">蓝色</button>
</div>
<script>
	new Vue({
		el:"#app",
		data:{
			styleFont:{color:'red'}
		},
		methods:{
			changeStyle(color){
				this.styleFont = { color }
			}
		}
	})
</script>
```

②对象的写法

```
<p :style="{color:fontc,fontSize:'20px'}">破纪录！袁隆平团队双季稻晚稻亩产911.7公斤</p>
<p :style="{color:fontc,'font-size':'20px'}">破纪录！袁隆平团队双季稻晚稻亩产911.7公斤</p>
```

在对象写法中，属性名如果是多个单词，比如font-size、background-color、margin-left等，vue中需要把横杠和小写字母转换成大写字母：fontSize、backgroundColor、marginLeft。

③数组的写法

```
<p :style="[styleFont,styleSize]">破纪录！袁隆平团队双季稻晚稻亩产911.7公斤</p>
```

style的数组写法中，每一个数组元素都是一个变量，表示此标签可以使用多个行内样式。

#### class

①变量的写法

```vue
<style>
	.red{ color:red }
</style>
<div id="app">
	<p :class="className">港府公报：行政长官林郑月娥明起访问北京、广州和深圳</p>
</div>
<script>
	new Vue({
		el:"#app",
		data:{ className:'red' }
	})
</script>
```

②对象的写法

```vue
<style>
	.big{ font-size:40px; }
    .small{ font-size:15px;}
</style>
<div id="app">
	<p :class="{big:false,small:true}">港府公报：行政长官林郑月娥明起访问北京、广州和深圳</p>
</div>
<script>
	new Vue({
		el:"#app"
	})
</script>
```

布尔值或者表达式的结果为true时，表示使用指定的class属性，否则就不使用指定的class属性

③数组的写法

```vue
<style>
	.big{ font-size:40px; }
    .red{color:red;}
</style>
<div id="app">
	<p :class="['big','red']">港府公报：行政长官林郑月娥明起访问北京、广州和深圳</p>
</div>
<script>
	new Vue({
		el:"#app"
	})
</script>
```

## 5.列表渲染（循环）

v-for

语法格式：

```
<标签名 v-for="每次遍历的元素的变量名 of 数据源" ></标签名>
```

(1)数组

```
<标签名 v-for="(每次遍历的数组元素的变量名,遍历元素的下标) of 数据源" ></标签名>
```

(2)对象

```
<标签名 v-for="(每次遍历键值对中的键值,每次遍历键值对中的键名,每次遍历键值对的下标) of 数据源"></标签名>
```

(3)整数

```
<标签名 v-for="每次遍历键值对中的键值 of 整数值"></标签名>
```

如果想让某个标签遍历指定次数，可以直接遍历一个整数值，默认从1开始，每次递增1

示例代码：

```vue
<div id="app">
    <!-- 遍历数组，最多支持两个参数，第一个是元素，第二个是下标 -->
    <button v-for="(item,index) in arr">{{index}}---{{ item }}新闻</button>
    <!-- 遍历对象，最多支持三个参数，第一个是键名，第二个是键值，第三个是下标  -->
    <p v-for="(index,item,key) of info">{{key}}---{{ index }}---{{ item }}</p>
    <!-- 遍历整数，默认从1开始进行遍历，每次递增1，到指定数值结束 -->
    <p v-for="num in 10">{{ num }}</p>
</div>
<script>
    var vm = new Vue({
        el:"#app",
        data:{
            arr:[ '北京','中国','国际','热点'],
            info:{
                name:'小王',
                age:18,
                address:'北京市朝阳区'
            }
        }
    })
    // 原生js中of获取到是的元素内容，in获取到的是元素下标
    // var arr2 = [ '北京','中国','国际','热点']
    // for(item of arr2){
    //     console.log(item)
    // }
</script>
```



# 常见错误

1.vue.js:634 [Vue warn]: Do not mount Vue to <html> or <body> - mount to normal elements instead.

vue的挂载点不能是html、body标签，应该是一个普通的标签

2.[Vue warn]: Property or method "msg" is not defined on the instance but referenced during render. Make sure that this property is reactive, either in the data option, or for class-based components, by initializing the property.

vue框架是一个声明式框架，变量或者函数要先声明然后再使用

3.[Vue warn]: Error compiling template:

v-else used on element <a> without corresponding v-if.

v-else和v-else-if必须依存v-if才能使用