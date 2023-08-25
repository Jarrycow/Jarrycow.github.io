---
title: Ajax
author: Jarrycow
img: /medias/featureimages/xxx.png
cover: false
top: false
mathjax: true
categories:
  - 前端
tags:
  - 前端
keywords: Ajax
abbrlink: Ajax
date: 2023-07-23 09:22:14
---

Ajax 是异步的JavaScript和XML。

<!--more-->

**作用**：

- 数据交换：通过Ajax可以给服务器发送请求，并获取服务器响应的数据。
- 异步交互：可以在不重新加载整个页面的情况下，与服务器交换数据并更新部分网页的技术，如：搜索联想、用户名是否可用的校验等等。

# Axios

lAxios 对原生的Ajax进行了封装，简化书写，快速开发。

## 开发步骤

引入Axios的js文件

```html
<script src="js/axios-0.18.0.js"></script>
```

使用Axios发送请求，并获取响应结果

```javascript
axios({
    method: "get",
    url: "http://yapi.smart-xwork.cn/mock/169327/emp/list"
}).then((result) => {
    this.emps = result.data;
});
```

```javascript
axios({
    method: "post",
    url: "http://yapi.smart-xwork.cn/mock/169327/emp/deleteById",
    data: "id=1"
}).then((result) => {
    this.emps = result.data;
});
```

可以简化为

```javascript
axios.get("http://yapi.smart-xwork.cn/mock/169327/emp/list").then(result => {
            this.emps = result.data;
        })
```

```javascript
axios.post("http://yapi.smart-xwork.cn/mock/169327/emp/deleteById","id=1").then(result => {
            this.emps = result.data;
        })
```

可以通过 Vue 中定义 emps 获取 Ajax 值

```javascript
new Vue({
       el: "#app",
       data: {
         emps:[]
       },
       mounted () {  // 发送异步请求,加载数据
          axios.get("http://yapi.smart-xwork.cn/mock/169327/emp/list").then(result => {
            this.emps = result.data.data;
          })
       }
    });
```

