```
内容：组件通信和进阶
讲师：范明剑
日期：2020-11-06
```

## 一、组件通信【重点】

场景：由于组件与组件数据不共享，当需要复用组件并让组件中的内容不一样时，可以使用组件通信来实现。

## 1.父子组件

自定义属性和props

第一步：先创建两个组件，一个父组件parent.vue，一个子组件child.vue

在App.vue中引入父组件，并使用它

```vue
<script>
import vParent from './components/parent'
export default {
	components: {
        vParent
    }
}
</script>
<template>
	<div class="page">
        <v-parent></v-parent>
	</div>
</template>
```

第二步：在父组件中引入子组件，构建父子组件的关系，并通过自定义属性传递数据

/src/components/parent.vue

```vue
<template>
    <div>
        <h1>父组件</h1>
        <!-- 在父组件使用子组件时，通过自定义属性来传递数据 -->
        <v-child v-for="(item,index) of newsarr" :key="index"
            :newstitle="item"
        ></v-child>
    </div>
</template>
<script>
import vChild from './child'
export default {
    components:{ vChild },
    data(){
        return{
            newsarr:['国内','北京','国际','娱乐']
        }
    }
}
</script>
```

第三步：在子组件child.vue中通过props选项来接收父组件传递数据，并在子组件中使用这些数据

/src/components/child.vue

```vue
<template>
    <div class="box">
        <h1>{{ newstitle }}新闻</h1>
    </div>
</template>
<script>
export default {
    // 在子组件中通过props来接收父组件传递的数据
    // 子组件接收到数据后，就可以直接在子组件的模板中使用该数据了
    props:['newstitle']
}
</script>
<style>
    .box{
        border:1px solid #000;
        margin:20px;
        padding:20px;
    }
</style>
```

### props验证

子组件可以对父组件传递的数据进行一些验证：数据类型、默认值、自定义验证。。。

#### (1)验证数据类型

```js
<script>
export default {
    props:{
        // 验证数据类型，可以验证常见的数据类型：
        //字符串String 数字Number 布尔值 Boolean 对象 Object 函数 Function等
        newstitle:String
    }
}
</script>
```

验证多个数据类型

如果要接收的变量是多个数据类型，则需要这样去进行验证

```js
<script>
export default {
    props:{
        newstitle:[String,Number]//此时父组件传递的数据既可以是字符串，也可以是数字
    }
}
</script>
```

#### (2)验证必填

```js
<script>
export default {
    props:{
        viewnum:{
        	required:true,
            type:Number//验证必填的同时还可以验证数据类型
        }
    }
}
</script>
```

如果子组件中有一个父组件必须传递的数据，可以通过required来进行验证。

#### (3)默认值

```vue
<script>
export default {
    props:{
        viewnum:{
        	required:true,
            type:Number//验证必填的同时还可以验证数据类型
        },
        first:{
            // default:"什么也没有"
            //如果数据类型是对象或者数组时，默认值是一个函数，返回对应的数据格式
            default:function(){
                return{
                    title:'什么也么有'
                }
            }
        }
    }
}
</script>
```

#### (4)自定义验证规则

```vue
<script>
export default {
    props:{
        viewnum:{
        	required:true,
            type:Number,//验证必填的同时还可以验证数据类型
            validator:function(val){
                //自定义验证规则，只要返回值为false就会出现警告
                return val > 1 && val <= 999;
            }
        }
    }
}
</script>
```

props验证给出只是警告，不影响程序的运行。

## 2.子父组件

自定义事件、$emit

第一步：在父组件使用子组件的时候，传递一个自定义函数，并在父组件中定义好对应的函数操作

```vue
<v-child v-for="(item,index) of newsarr" :key="index"
    :newstitle="item.title" :num="item.num"
    :newsidx="index"
    @addbychild="addnum"
></v-child>
<script>
export default {
    components:{ vChild },
    methods:{
        addnum(n){
            this.newsarr[n].num++;
        }
    },
    data(){
        return{...}
    }
</srcipt>
    
```

第二步：在子组件中通过vue实例上的$emit方法，来触发父组件传递的事件函数

```vue
<template>
    <div class="box">
        <span>{{ newstitle }}</span>
        <span>访问量：{{ num }}</span>
        <button @click="add">访问</button>
    </div>
</template>

<script>
export default {
    props:['newstitle','num','newsidx',"arr"],
    methods:{
        add(){
            //触发父组件传递的事件函数，并传递参数
            this.$emit("addbychild",this.newsidx)
        }
    }
}
</script>
```

## 3.非父子组件

公用的容器（eventbus）、$emit、$on

第一步，创建一个公用的容器

/src/main.js实例化vue之前

```
Vue.prototype.$ev = new Vue();
```

第二步：在数据发送的组件中，通过公用容器进行数据发送

```
this.$ev.$emit("事件名称","要传递的数据")
```

第三步：在数据接收的组件的挂载完成生命周期中实现数据的接收

```
mounted() {
    //通过容器，一直监听数据，只要有数据的发送，就可以收到数据
    this.$ev.$on("时间名称",(str)=>{
    	...
    })
}
```

# 二、组件进阶

## 1.is属性

改变html标签默认结构约束

动态组件

示例代码：

menu.vue

```vue
<template>
    <div class="menu">
        <a @click="tab('setting')">系统设置</a>
        <a @click="tab('student')">学生管理</a>
        <a @click="tab('course')">课程管理</a>
    </div>
</template>

<script>
export default {
    methods:{
        tab(tag){
            console.log(tag,'发送')
            this.$ev.$emit("changeTag",tag)
        }
    }
}
</script>

<style>
    .menu{
        text-align: center;
    }
    .menu a{
        display: block;
        color:#fff;
        font-size: 20px;
        margin:10px;
        cursor: pointer;
    }
</style>
```

main.vue

```vue
<template>
    <div class="main">
        <v-menu></v-menu>
        <div class="right">
            <!-- 动态组件 -->
            <div :is="tagname"></div>
        </div>
    </div>
</template>
<script>
import vMenu from './menu'
import setting from './setting'
import student from './student'
import course from './course'
export default {
    components:{
        vMenu,setting,student,course
    },
    mounted() {
        this.$ev.$on("changeTag",(t)=>{
            console.log(t,'接收')
            this.tagname = t;
        })
    },
    data(){
        return{
            msg:'hello',
            tagname:'setting'
        }
    }
}
</script>
<style>
    .main{
        flex:1;
        display: flex;
    }
    .menu{
        width:120px;
        background-color: #000;
    }
    .right{
        flex:1;
    }
</style>
```



## 2.ref

vue中提供了一个ref属性，可以给标签添加ref属性，来实现原生JS的一些操作或者父子组件通信的效果。

(1)字符串

```
<h1 ref="myh1">系统设置页面</h1>
```

此时在$refs中就会增加了一个myh1的内容，它的值就是DOM元素。

(2)数组

```
<ul>
	<li ref="lis" v-for="item of numarr" :key="item">{{ item }}</li>
</ul>
```

如果ref应该在一个遍历循环上，则会在$refs中增加一个数组，数组的每个元素都是一个DOM元素

(3)组件【常用】

给自定义组件添加ref属性后，可以利用DOM操作的方式，来给组件进行数据的传递

```vue
<template>
	<div>
		<v-item ref="myitem"></v-item>
	</div>
</template>
<script>
import vItem from './item'
export default {
    components:{ vItem },
    mounted(){
        //直接在父组件中通过ref来给子组件进行数据的传递
        this.$refs.myitem.msg = '系统设置-数据'
    }
}
</script>
```

## 3.jquery

如果有一些页面效果，不知道用vue如何实现，但是知道用jquery去实现，那么在vue中可以用jquery来进行操作。

### (1)安装

```
npm i jquery
```

### (2)使用

```vue
<script>
//引入jquery
import $ from 'jquery'
export default {
    methods:{
        toggle(){
            this.jQuery(".box").slideUp(2000)
        }
    }
</script>
<template>
	<div>
        <button @click="toggle">toggle</button>
        <div class="box"></div>
    </div>
</template>
<style>
    .box{
        width: 200px;
        height: 200px;
        background-color: #f00;
    }
</style>
```

未完待续。。。

# 作业：

封装一个表格组件，根据提供的表头数据和表格数据来展示内容

例如：

数据

```
var titles = ['姓名','年龄','手机号']
var datas = [
	{
		name:'小飞',
		age:18,
		phone:18811112222
	},
	{
		name:'小飞2',
		age:19,
		phone:18811113333
	}
	...
]
```

