---
title: Vue
author: Jarrycow
img: /medias/featureimages/xxx.png
cover: false
top: false
mathjax: true
categories:
  - 前端
tags:
  - 前端
  - Vue
  - JS
keywords: Vue
abbrlink: VueIntroduction
date: 2023-07-23 08:57:17
summary: Vue 是很流行的一套前端框架。
---

Vue 是一套前端框架，免除原生JavaScript中的DOM操作，简化书写。 Vue 基于MVVM(Model-View-ViewModel)思想，实现数据的双向绑定，将编程的关注点放在数据上。

![MVVM 模型](https://raw.githubusercontent.com/Jarrycow/picHost/main/AI/MVVM.png)

一旦数据变化，显示页面会自动变化；一旦显示页面变化，后端数据也会自动更新。

# 简单用法

在 HTML 代码中加入

```html
<script src="js/vue.js"></script>
```

在JS代码区域，创建Vue核心对象，定义数据模型

```html
<script>
    new Vue({
        el: "#app",  // 区域 id 编号
        data: {
            message: "Hello Vue!"
        }
    })
</script>
```

编写视图

```html
<div id="app">
    <input type="text" v-model="message">
    {{ message }}
</div>
```

# 常用指令

| **指令**                          | **作用**                                            |
| --------------------------------- | --------------------------------------------------- |
| v-bind                            | 为HTML标签绑定属性值，如设置 href , css样式等       |
| v-model                           | 在表单元素上创建双向数据绑定                        |
| v-on                              | 为HTML标签绑定事件                                  |
| v-if  <br> v-else-if  <br> v-else | 条件性的渲染某元素，判定为true时渲染,否则不渲染     |
| v-show                            | 根据条件展示某元素，区别在于切换的是display属性的值 |
| v-for                             | 列表渲染，遍历容器的元素或者对象的属性              |

**v-bind**

为HTML标签绑定属性值

```html
<script>
  new Vue({
     el: "#app",
     data: {
        url: "jarrycow.top/"
     }
  })
</script>
```

```html
<a v-bind:href="url">Jarrycow</a>
<a :href="url">Jarrycow</a>  <!-- 简写 -->
```

则超链接自动转为 Vue 中定义的 url

**v-model**

```html
<input type="text" v-model="url">
```

表单数据自动为 v-model

**v-on**

v-on 为HTML标签绑定事件。

```html
<input type="button" value="按钮" v-on:click="handle()">
<input type="button" value="按钮" @click="handle()">  <!-- 简写 -->
```

```html
<script>
    new Vue({
        el: "#app",
        data: {
	//...
        },
        methods: {
            handle:function(){
                alert('我被点击了');
            }
        },
    })
</script>
```

**v-if & v-else-if & v-else**

只有 true 时方才渲染。

```html
年龄{{age}},经判定为:
<span v-if="age <= 35">年轻人</span>
<span v-else-if="age > 35 && age < 60">中年人</span>
<span v-else>老年人</span>
```

**v-show**

v-show 起同样效果，区别主要是修改的是 display

```html
年龄{{age}},经判定为:
<span v-show="age <= 35">年轻人</span>
```

**v-for**

列表渲染，遍历容器的元素或者对象的属性

```html
<div v-for="addr in addrs">{{addr}}</div>
<div v-for="(addr,index) in addrs">{{index + 1}} : {{addr}}</div>


data: {
   . . .
   addrs: ['北京','上海','广州','深圳','成都','杭州']
},
```

# Vue 生命周期

生命周期的八个阶段：每触发一个生命周期事件，会自动执行一个生命周期方法。

| **状态**      | **阶段周期** |
| ------------- | ------------ |
| beforeCreate  | 创建前       |
| created       | 创建后       |
| beforeMount   | 挂载前       |
| mounted       | 挂载完成     |
| beforeUpdate  | 更新前       |
| updated       | 更新后       |
| beforeDestroy | 销毁前       |
| destroyed     | 销毁后       |

![Vue 生命周期](https://raw.githubusercontent.com/Jarrycow/picHost/main/JavaWeb/Vue 生命周期.png)

# Vue 开发流程

Vue-cli 是Vue官方提供的一个脚手架，用于快速生成一个 Vue 的项目模板。

需要 NodeJS 安装 vue-cli

```shell
npm install -g @vue/cli
```

创建 Vue 项目，打开图形化界面

```shell
vue ui
```

![Vue 创建项目](https://raw.githubusercontent.com/Jarrycow/picHost/main/JavaWeb/VueCreate.png)

生成目录结构：

![Vue 项目目录结构](https://raw.githubusercontent.com/Jarrycow/picHost/main/JavaWeb/Vue 项目目录结构.png)

运行命令启动

```shell
npm run serve
```

## 组件文件 .vue

Vue 开发主要是操作 Vue 文件

![Vue 组件文件](https://raw.githubusercontent.com/Jarrycow/picHost/main/JavaWeb/_Vue.png)

# Vue 路由

lVue Router 是 Vue 的官方路由,点击标签切换显示内容。

![Vue 路由](https://raw.githubusercontent.com/Jarrycow/picHost/main/JavaWeb/VueRouter.png)

在 router/index.js 中

```js
const routes = [
    {
        path: '/dept',
        name: 'dept',
        component: () => import('../testExample.vue')
    }
]
```

在点击区域加上 `router-link` 标签,在显示部分加上 `router-view` 标签。

```html
<router-link to="/dept">example</router-link>
<router-view></router-view>
```

