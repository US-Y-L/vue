```
内容：表单元素双向绑定、修饰符
讲师：范明剑
日期：2020-11-03
```

# 一、表单元素双向绑定

修改字体样式：

```html
<span style="color:darkorange"></span>
```



## 1.设计模式

### (1)MVC

smalltalk，把用户的输入、输出、处理分开

m model 数据模型层

v view 视图层（html、css、js）

c controller 控制器层

### (2)MVVM

m model 数据模型层

v view 视图层

vm viewmodel 视图模型层

## v-model

 你可以用 `v-model` 指令在表单 ``、`` 及 元素上 创建<span style="color:red">双向数据绑定</span>。它会根据控件类型自动选取正确的方法来更新元素。尽管有些神奇，但 `v-model` 本质上不过是语法糖。它<span style="background-color:darkorange;color:#000">负责监听用户的输入事件以更新数据</span>，并对一些极端场景进行一些特殊处理 

 `v-model` 会忽略所有表单元素的 `value`、`checked`、`selected` attribute 的初始值而总是将 **Vue 实例的数据作为数据来源**。你应该通过 JavaScript 在组件的 **`data` 选项中声明初始值** 

## 2.文本框

 在输入框上使用时，输入的内容会实时映射到绑定的数据上 

```html
<div id="app">
         <input type="text" v-model="message" placeholder="输入...">
         <p>输入的内容：{{message}}</p>
</div>
<script>
         var app = new Vue({
                 el:'#app',
                 data:{
                       message:""
                   }
         })
</script>
```

 使用v-model 后，表单控件显示的值==只依赖所绑定的数据，不再关心初始化时的value属性==，对于在＜textarea></textarea> 之间插入的值，也不会生效．使用v-model 时，如果是用中文输入法输入中文，一般在没有选定词组前，也就是在拼音阶段， Vue 是不会更新数据的，当敲下汉字时才会触发史新。如果希望总是实时更新，可以用＠ input 来替代v-model . 事实上， v-model 也是一个特殊的语法糖，只不过它会在不同的表单上智能处理。 

```
<input type="text" v-model="msg">
```

数据响应式原理：

<img src="./data.png" />

原生JS示例代码：

```javascript
<script>
    let person = {
        wh:"燕人",
        get wh(){
            console.log("获取外号....")
            return "张飞";
        },
        set wh(val){
            console.log("设置外号....",val)
        }
    }
    console.log(person.wh)//读取数据，就会自动触发get方法
    person.wh = "关羽"//设置数据，就会自动地触发set方法
</script>
```

## 3.文本域

textarea

```
<textarea cols="30" rows="10" v-model="content"></textarea>
```

## 4.复选框

checkbox

(1)多选

```vue
<div id="app">
    <div>
        <label>兴趣爱好:</label>
        <input type="checkbox" v-model="hobbys" value="打游戏">打游戏
        <input type="checkbox" v-model="hobbys" value="看电影">看电影
        <input type="checkbox" v-model="hobbys" value="逛吃">逛吃
    </div>
    <div>
        选择的是：{{ hobbys }}
    </div>
    
</div>
<script>
	new Vue({
		el:"#app",
		data:{
			hobbys:[]
		}
	})
</script>
```

复选框的初始数据类型，一定是数组，这样才能够实现多选的情况

 组合使用时需要v-model和value一起使用，多个数值绑定到同一种类型的数组数据中，value值在数组中，==选择过程是双向的==，选中时，数值也会push到数组中。 

多选的情况下，checkbox一定要设置value属性

(2)单选

```vue
<div id="app">
    <div>
    <!-- 是否同意协议：<input type="checkbox" :checked="isagree"> -->
    是否同意协议：<input type="checkbox" v-model="isagree">
    </div>
</div>
<script>
	new Vue({
        el:"#app",
        data:{
            isagree:true
        }
    })
</script>
```

单选的情况下，checkbox不需要设置value属性,只需要通过v-model绑定一个布尔值

## 5.单选框

 单选按钮在==单独使用==时，不需要v-model ，直接使用v-bind 绑定一个布尔类型的值， 为真时选中， 为否时不选，例如： 

```html
<div id="app">
         <input type="radio" :checked="picked">
         <label>单选按钮</label>
</div>
<script>
         var app = new Vue({
                   el:'#app',
                   data:{
                           picked:true
                   },
         })
</script>
```



radio

```vue
<div id="app">
	<div>
        <label>状态：</label>
        <!-- <input type="radio" name="status">正常
<input type="radio" name="status">禁用 -->
        <input type="radio" value="1" v-model="status">正常
        <input type="radio" value="2" v-model="status">禁用
    </div>
    <div>
        选择的是：{{ status }}
    </div>
</div>
<script>
	new Vue({
        el:"#app",
        data:{
            status:1
        }
    })
</script>
```

radio和checkbox（多选）一样，都是要设置value属性，这样在进行数据绑定时，才能够根据指定的数据，控制元素的选中效果。

## 6.下拉菜单

select > option

```vue
<div id="app">
    <div>
        <label>所学专业</label>
        <select v-model="course">
            <option value="">--请选择--</option>
            <option value="1">web前端</option>
            <option value="2">java开发</option>
            <option value="3">ui设计</option>
        </select>
    </div>
    <div>
        选择的是：{{ course }}
    </div>
</div>
<script>
	new Vue({
        el:"#app",
        data:{course:""}
    })
</script>
```

原生html中想让某个选项选中，需要在这个选项中添加selected属性

```
<option selected="selected">web前端</option>
```

在vue中直接给select标签添加v-model双向绑定，就可以给相应的选项设置选中效果

```html
<div id="app">
         <select v-model="selected">
                   <option>html</option>
                   <option value='js'>JavaScript</option>
                   <option>css</option>
         </select>
         <p>选择的项是：{{selected}}</p>
</div>
<script>
         var app = new Vue({
                   el:'#app',
                   data:{
                            selected:'html'
                   },
         })
</script>
```



 < option＞是备选项，如果含有value 属性， v-model 就会优先匹配value 的值： 如果没有， 就会直接匹配＜ option＞的text ，比如选中第二项时， selected 的值是js ， 而不是JavaScript 。给＜selected＞添加属性multiple 就可以多选了， 此时v -model 绑定的是一个数组， 与复选框用法类似 

```html
<select v-model = ”selected" multiple>
```

 在业务中，＜ option＞经常用v -for 动态输出， value 和text 也是用v-bind 来动态输出的， 例如： 

```html
 
         <select v-model="selected">
                  <option v-for="option in options" 
                    :value="option.value">{{option.text}}						</option>
         </select>
         <p>选择的项是：{{selected}}</p>
 
```

## 7、绑定值

 上一节介绍的单选按钮、复选框和选择列表在单独使用或单选的模式下， v-model 绑定的值是一个静态字符串或布尔值， 但在业务中，有时需要绑定一个动态的数据， 这时可以用v-bind 来实现。 

### 单选按钮

```html
<div id="app">
         <input type="radio" v-model="picked" :value="value">
         <label>单选按钮</label>
         <p>{{picked}}</p>
         <p>{{value}}</p>
</div>
<script>
         var app = new Vue({
                   el:'#app',
                   data:{
                            picked:false,
                            value:123
                   },
         })
</script>
```

 在选中时，app.picked===app.value，值都是123 

### 复选框

```html
<div id="app">
         <input type="checkbox" v-model="toggle" :true-value:"value1" 
            :false-value:"value2">
         <label>复选框</label>
         <p>{{toggle}}</p>
         <p>{{value1}}</p>
         <p>{{value2}}</p>
</div>
<script>
         var app = new Vue({
                   el:'#app',
                   data:{
                            toggle:false,
                            value1:'a',
                            value2:'b',
                   },
         })
</script>
```

 勾选时， app.toggle =\=\= app .valuel ; 未勾选时， app.toggle ＝\=\=app.value2. 

### 选择列表

```html
<div id="app">
     <select v-model="selected">
          <option :value="{number:123}">123</option>
     </select>
     {{selected.number}}
</div>
<script>
     var app = new Vue({
           el:'#app',
           data:{
                selected:''
           },
    })
</script>
```

 当选中时， app.selected 是一个Object ，所以app.selected.number === 123 

# 二、自定义指令

```
Vue.directive("指令名称",{
	//inserted:function(el)
	inserted(el){
		//el就是使用自定义指令的标签元素
		//js操作。。。
	}
})
```

指令名称不需要添加v-前缀，但是在使用指令的时候需要添加v-前缀

inserted 被绑定元素插入父节点时就会调用该函数

bind 指定绑定到对应的元素后就会调用该函数

示例代码：

```vue
<div id="app">
    <input type="text" v-focus>
</div>
<script>
    Vue.directive("focus",{
        inserted(el){
            //el 就是使用该指令的标签元素
            el.focus();
        }
    })
    new Vue({
        el:"#app"
    })
</script>
```

示例代码2：

```vue
<div id="app">
    <h1 v-color="'red'">{{ msg }}</h1>
    <h1 v-color="'green'">{{ msg }}</h1>
</div>
<script>
    Vue.directive("color",{
        bind:function(el,binding){
            //el 是使用自定义指令的元素
            //binding 是自定义指令绑定的相关数据信息
            el.style.color = binding.value;
        }
    })
    new Vue({
        el:"#app",
        data:{
            msg:'hello 自定义指令'
        }
    })
</script>
```

# 三、修饰符

## 1.事件修饰符

(1).prevent

阻止默认事件

(2).stop

阻止事件冒泡

(3).capture

捕获事件冒泡，影响事件冒泡的顺序

(4).self

事件触发者是元素本身时，对应的事件函数才会执行

(5).once

修饰指定事件、修饰符只执行一次

示例代码：

```vue
<div id="app">
    <!-- 阻止默认事件 -->
    <!-- 修饰符可以连贯起来使用，也就是给一个事件添加多个修饰符 -->
    <button @contextmenu.prevent.once="menu">按钮</button>
    <!-- 阻止事件冒泡 -->
    <div class="outer" @click="outer">
        <div class="inner" @click.stop="inner"></div>
    </div>
    <hr>
    <!-- 捕获事件冒泡 -->
    <div class="outer" @click.capture="outer">
        <div class="inner" @click="inner"></div>
    </div>
    <!-- self -->
    <hr>
    <div class="outer" @click.self="outer">
        <div class="inner" @click.once="inner"></div>
    </div>
</div>
<script>
    new Vue({
        el:"#app",
        methods: {
            menu(){
                console.log('右键被点击了....');
            },
            outer(){
                console.log("outer....")
            },
            inner(){
                console.log("inner....")
            }
        },
    })
</script>
```

## 2.表单元素修饰符

(1).lazy

不再对表单元素进行实时的数据双向绑定，只有遇到change事件时，才会进行数据的双向绑定

(2).number

number修饰符可以保持数据类型为number，如果输入的内容中是以数字开头，非数字结尾，可以把非数字内容进行过滤。

(3).trim

过滤输入内容左右两边的空格

## 3.其他修饰符

(1)鼠标修饰符

.left

.middle

.right

(2)键盘修饰符

keydown、keyup

.esc

.enter

.space

.backspace

.left

.right 

.down

.up 

.delete