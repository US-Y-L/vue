> 第09讲 
>
> 内容：vuex、ui库和版本控制软件git
>
> 讲师：范明剑
>
> 日期：2020-11-13

# 一、vuex和本地存储

vuex存储和本地存储(localstorage、sessionstorage)的区别

## 1.区别

vuex存储在内存，localstorage（本地存储）则以文件的方式存储在本地,永久保存；sessionstorage( 会话存储 ) ,临时保存。localStorage和sessionStorage只能存储字符串类型，对于复杂的对象可以使用ECMAScript提供的JSON对象的stringify和parse来处理。

## 2.应用场景

vuex用于组件之间的传值，localstorage则主要用于不同页面之间的传值。

## 3.永久性

当刷新页面时vuex存储的值会丢失，localstorage不会。注：很多同学觉得用localstorage可以代替vuex, 对于不变的数据确实可以，但是当两个组件共用一个数据源（对象或数组）时，如果其中一个组件改变了该数据源，希望另一个组件响应该变化时，localstorage无法做到响应式，vuex可以绑定数据响应式。
Vuex数据状态持久化的使用场景
(1)购物车
比如你把商品加入购物车后，没有保存到后台的情况下，前端来存，就可以通过这种方式vuex+localStorage(sessionStorage)。

(2)会话状态
授权登录后，token就可以用Vuex+localStorage(sessionStorage)来存储。

(3)一些不会经常改变的数据
比如城市列表等（当前也要留下可以更新的入口，比如版本号）

数据持久化插件：

安装

```
npm install  vuex-persistedstate
```

使用

```
import persistedState from 'vuex-persistedstate'
export default new Vuex.Store({
  getters,
  state,
  actions,
  mutations,
  plugins: [persistedState()]
})
```

# 二、ui库

## 1.element-ui

注意：不要使用element-ui的同时使用bootstrap

### (1)安装

```
npm i element-ui
```

### (2)引入

```
import ElementUI from 'element-ui'
import 'element-ui/lib/theme-chalk/index.css';
Vue.use(ElementUI)
```



## 2.mint-ui

官网：

```
http://mint-ui.github.io/#!/zh-cn
```

# 三、版本控制软件git

cvs、svn

## 1.下载安装

官网：https://git-scm.com/

## 2.安装

没有什么特殊要的话直接下一步即可

## 3.使用

### (1)概念

工作区、暂存区、版本库

### (2)基本配置

查看配置

```
git config --list
```

配置必要信息

```
git config --global user.name 用户名
git config --global user.email 邮箱
```

### (3)初始化仓库

进入一个空的目录下或者已有项目的根目录下，执行命令

```
git init
```

创建文件

```
touch 文件名.后缀名
```

创建文件夹

```
mkdir 目录名称
```

git status 查看暂存区状态

### (4)添加文件并提交版本

添加文件到暂存区

```
git add 文件名 添加一个文件
git add *.后缀名 添加一类文件
git add . 添加所有文件和目录
```

提交文件到版本库

```
git commit -m "备注信息"
```

### (5)查看日志

```
git log
git reflog 所有版本记录
```

### (6)版本切换

版本回退到上一个版本

```
git reset --hard HEAD^ 
```

版本回退到上两个版本

```
git reset --hard HEAD^^
```

版本回退到上100个版本

```
git reset --hard HEAD~100
```

版本切换

```
git reset --hard 版本号
```

### (7)对比差异

```
git diff 文件名
```

### (8)撤销修改

```
git checkout 文件名
```

### (9)分支管理

master		主分支，可以对外发布的版本

develop 	开发分支

release		预发布分支

fixed		 修复

...

①查看分支

```
git branch
```

②创建分支

```
git branch 分支名称
```

③切换分支

```
git checkout 分支名称
```

④分支合并

```
git merge 要合并的分支名称
```

# 四、远程协作

github.com

gitee.com 

## 1.注册账号

## 2.创建远程仓库

## 3.添加远程仓库

```
git remote add origin https://github.com/用户名/远程仓库名.git
```

## 4.本地推送到远程仓库

```
git push -u origin master
```

## 5.克隆远程资源

（仅第一次下载项目时执行）

```
git clone https://github.com/用户名/远程仓库名.git
```

## 6.获取远程仓库更新的资源

```
git pull 获取远程仓库的资源，自动的合并到本地
```

```
git fetch 仅获取远程仓库的资源，需要手动的合并
```

