---
title: matery主题的配置
date: 2022-08-30 17:43:58
categories:
  - 博客
tags:
  - 博客
img: /medias/featureimages/matery.jpg
abbrlink: matery
toc: true
top: true
summary: matery主题感觉看上去蛮好看的，跟cxy要过来搞一波配置
---

matery主题感觉看上去蛮好看的，跟cxy要过来搞一波配置

[toc]

<!-- more -->

# 主题下载与配置

## 下载主题

1. 在根目录下执行命令

   ```cmd
   git clone https://gitee.com/yafine66/hexo-theme-matery themes/matery
   ```

2. 将站点配置文件中的`theme`值修改为`theme: matery`

## 基础配置

1. 修改网站图标

修改`\themes\source\favicon.png`文件

修改`\themes\source\medias`文件



## 修改网站背景图

修改`themes\_config.yml` > `background:`

将`enable`修改为`true`，`url`修改

将`indexbtn:`下`url`修改

# 增加功能

## 增加页面

增加页面的步骤基本类似，创建`tags`页面、`categories`页面，`about`页面、` contact`页面、`friends`页面、均可以使用这个步骤

1. 在根目录执行命令

   ```cmd
   hexo new page "页面英文"  # 如：hexo new page "tags"
   ```

2. 编辑刚刚创建的页面文件夹中的```index.md```，路径为```\source\"pages"\index.md```

   ```markdown
   ---
   title: tags                # 页面英文
   date: 2020-06-19 16:23:38  # 页面创建时间
   type: "tags"               # 如：type: "tags"
   layout: "tags"             # 如：layout: "tags"
   ---
   ```

## 菜单的设置

在`\themes\matery\_config.yml`中，对`menu:`进行修改

```yml
menu:
  Index:
    url: /
    icon: fas fa-home
```

其中，`url`指目录指向链接，该链接生成需在`git bash`中用`hexo new page "..."`生成文件夹；`icon`使用[Font Awesome](https://fontawesome.com/icons)

若想要生成二级目录，可如下使用`children`	

```yml
Index:
	children:
      - name: 关于我  
        url: /about
        icon: fas fa-user-circle
```

其中，```-```引领具体的二次目录；```name```为二级目录名称

最后，在`\themes\languages\zh-CN.yml`下替换翻译



# 视觉美化

## 副标题

修改`themes\_config.yml` > `subtitle:`

在`sub`下添加语句

## 自定义字体（未完成）

1. 在站点目录下 `source` 文件夹创建 `font` 文件夹存放字体

2. 将字体存放于文件夹

3. 在 `/themes/matery/source/css/my.css` 下填入

   ```css
   @font-face{
       font-family: 'myFont';
       src: url('../font/myFont.ttf');
   }
   
   body{
       font-family: 'myFont';
   }
   ```

   我选择了[六个蛋老师](https://space.bilibili.com/38053181)设计的[得意黑](atelier-anchor.com/typefaces/smiley-sans/)字体，这是一套完全开源免费的字体。不过 u1s1，用在正文似乎因为都是斜体，不甚美观。尤其因为诸如公式、行内代码还是原来的字体，显得不是很协调![使用了得意黑的文章](https://raw.githubusercontent.com/Jarrycow/picHost/main/advancedMath/使用了得意黑的文章.png)

   因此，考虑只对部分内容使用得意黑字体

   1. 将  `/themes/matery/source/css/my.css` 修改为

      ```css
      @font-face{
          font-family: 'myFont';
          src: url('../font/myFont.ttf');
      }
      
      .fontSmileySans{
          font-family: 'myFont';
      }
      ```

   2. 然后对着想修改的地方加入该 class 即可

## 自定义鼠标样式

# 博客优化

## gulp 代码压缩(未实现)

hexo 生成的  html、css、js等都有很多的空格或者换行，而空格和换行也是占用字节的，所以需要将空格换行去掉也就是我要进行的“压缩”，减小一点资源文件的大小也是对访问速度有那么一点提升

1. 

## CDN 加速

因为博客部署到了国外的 Github上，国内访问速度很慢，可以使用 CDN 来实现全站加速

在主题配置文件_config.yml中搜索`jsDelivr`，填写url为自己的博客仓库即可

```yml
jsDelivr:
  url: https://cdn.jsdelivr.net/gh/"用户名"/"用户名".github.io
```

重新`hexo cl && hexo g && hexo d`部署

> 注意:
>
> 1. 配置了此项，就代表着 `hexo s`本地调试的时候，网站依然会去 GitHub 请求原来的资源，所以本地调试的时候将此项配置注释掉
> 2. 在 `hexo s` 本地调试好之后，需要 `hexo d` 部署到网上，要先配置到 `url`，之后再`hexo cl && hexo g && hexo d` 进行部署，否则不生效
> 3. 使用了 matery 提供的全局CDN加速，有可能一些特效会消失
> 4. jsDelivr 似乎国内备案到期了, 有失效风险

## 部署到 Coding (还没做)

博客部署到国内Coding仓库上，可以提高一些网站访问的速度

## 图床设置

本来担心图床的稳定性，试图将所有图片保存至本地，但太麻烦了。因此使用 PicGo+Github 构建图床。

详见：[图床创建](https://jarrycow.top/posts/picHostCreation.html)

## 新建文章自动打开本地 Markdown 编辑器

每次使用 ```hexo new "文章" ```时都需要进入 `_post` 文件夹手动打开，非常麻烦可以设置在生成之后自动打开

在根目录下新建 `scripts` 目录，然后在新建`auto_open.js`：

```js
var spawn = require('child_process').exec;

// Hexo 2.x 用户复制这段
//hexo.on('new', function(path){
  //spawn('start  "markdown编辑器绝对路径.exe" ' + path);
//});

// Hexo 3 用户复制这段
hexo.on('new', function(data){
  spawn('start  "D:\Program Files\Typora\Typora.exe" ' + data.path);
});
```

其中`D:\Program Files\Typora\Typora.exe` 为本地 Typora 编辑器的路径

# 基础修改



## 修改个人联系

在`\themes\_config.yml`中，对`socialLink:`进行修改

若想添加B站，在在`\themes\_config.yml` > `socialLink:`下添加`bilibili: #https://space.bilibili.com/xxx`，并在`\themes\layout\_partial\social-link.ejs`添加

```yaml
<% if (theme.socialLink.bilibili) { %>
    <a href="<%= theme.socialLink.bilibili %>" class="tooltipped" target="_blank" data-tooltip="在B站上关注我" data-position="top" data-delay="50">
        <i class="fas fa-play-circle"></i>
    </a>
<% } %>
```

## 添加友链

在`source\_data\friend.json`中添加

```json
{
    "avatar": "图片链接",
    "name": "友链名",
    "introduction": "介绍",
    "url": "友情链接",
    "title": "按钮标题"
}
```

# 主界面的修改

## 取消蒙版

在`theme\source\css\matery.css`，注释

```css
.bg-cover:after {
    -webkit-animation: rainbow 60s infinite;
    animation: rainbow 60s infinite;
}
```

## 更改图片

在`\themes\source\medias\banner`下替换图片(命名为0~7)

## 主界面视频

主界面除了图片之外，还可以使用视频

在`\themes\_config.yml`的`cover:`下的`video:`下的`enable:`改为`true`，并在其下的`src:`给出视频链接



# 404界面更改(未完成)

在`source`文件夹下新建`404.md`，内容开头包括

```tex
title: 404
date: 20xx-xx-xx xx:xx:xx
type: "404"
layout: "404"
description: "随便啥404界面的描述"
---
```
```html
<style type="text/css"> 
    /* don't remove. */ 
    .about-cover { 
        height: 90.2vh; 
    } 
</style> 
<div class="bg-cover pd-header about-cover"> 
    <div class="container"> 
        <div class="row"> 
            <div class="col s10 offset-s1 m8 offset-m2 l8 offset-l2"> 
                <div class="brand"> 
                    <div class="title center-align"> 
                        404 
                    </div> 
                    <div class="description center-align"> 
                        <%= page.description %> 
                            </div> 
                    </div> 
                </div> 
            </div> 
        </div> 
    </div> 
    <script> 
        // 每天切换 banner 图. Switch banner image every day. 
        $('.bg-cover').css('background-image', 'url(/medias/banner/' + new Date().getDay() + '.jpg)'); </script>
```

# 代码高亮

下载hexo-prism-plugin 插件

```yaml
npm i -S hexo-prism-plugin
```

修改根目录下`_config.yml`

```yaml

highlight: # 代码块的设置 
    enable: false # 关闭自带代码高亮 
    line_number: true # 自动检测编程语言 
    auto_detect: false # 显示行数 
    tab_replace: 4 # 用空格替换tabs
    tabs wrap: true 
    hljs: false 

# 关闭原有的代码高亮，使用自己的 
prism_plugin: 
    mode: 'preprocess' # realtime/preprocess 
    theme: 'tomorrow' 
    line_number: false # default false 
    custom_css:

```

# 文章生成永久链接

在博客目录执行

```shell
cnpm install hexo-abbrlink --save
```

在`\_congig.yml`中添加

```shell
abbrlink: 
    alg: crc16 #算法： 
    crc16(default) and crc32 rep: hex #进制： dec(default) and hex: dec #输出进制：十进制和十六进制，默认为10进制。丨dec为十进制，hex为十六进制

```

其中，`permalink: `更改为

```shell
permalink: posts/:abbrlink.html
```



**<font color="red">注：之前所有博客必须在开头添加</font>**

# 动态标题栏

在`theme/matery/layout/layout.ejs`

```ejs
<!--动态标题-->
<script type="text/javascript"> var OriginTitile = document.title, st; 
    document.addEventListener("visibilitychange", function () { 
        document.hidden ? 
        (document.title = "Σ(っ °Д °;)っ鳖走啊！", clearTimeout(st)) :
        (document.title = "φ(゜▽゜*)♪爱你么么哒！", st = setTimeout(function () { 
            document.title = OriginTitile 
        }, 3e3)) 
        }) 
</script>
```

# 博客支持Mathjax数学公式

1. 使用Kramed代替Marked

   在工程目录下执行命令

   ```shell
   npm uninstall hexo-renderer-marked --save
   npm install hexo-renderer-kramed --save
   ```

2. 在`/node_modules/hexo-renderer-kramed/lib/renderer.js`下

   将`function formatText(text)`改为

   ```shell
   // Change inline math rule
   function formatText(text) {
       return text;
   }
   ```

3. 卸载`hexo-math`

   ```shell
   npm uninstall hexo-math --save
   ```

   安装`hexo-renderer-mathjax`

   ```shell
   npm install hexo-renderer-mathjax --save
   ```

4. 更新 Mathjax 的 CDN 链接，打开`/node_modules/hexo-renderer-mathjax/mathjax.html`，将`<script>`更改为

   ```shell
   <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML"></script>
   ```

5. 更改默认转义规则，将`/node_modules/kramed/lib/rules/inline.js`下的`escape:`更改为

   ```shell
   escape: /^\\([`*\[\]()# +\-.!_>])/,
   ```

   `em:`更改为

   ```shell
   em: /^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,
   ```

6. 在`theme\_config.yml`下的`mathjax:`中，将`enable`改为`true`

7. 在博文开头加入`mathjax: true`

**<font color="red">注：如果第四步更改了CDN，那么在挂梯子情况下，查看博客时极大可能导致公式无法加载</font>**

# 界面加载动画

1. 在 Matery 主题配置文件 `_config.yml` 中新增配置属性 `preloader`

   ```yaml
    # 是否开启页面加载动画 true 开启，false 关闭
    preloader:
      enable: true
   ```

2. 在 Matery 主题目录 `/layout/_widget` 下新增文件 `loading.ejs`，并写入内容。
   ```js
    <% if (theme.preloader.enable) { %>
    <div id="loading-box">
        <div class="loading-left-bg"></div>
        <div class="loading-right-bg"></div>
        <div class="spinner-box">
            <div class="configure-border-1">
                <div class="configure-core"></div>
            </div>
            <div class="configure-border-2">
                <div class="configure-core"></div>
            </div>
            <div class="loading-word">加载中...</div>
        </div>
    </div>
    <script>
        window.addEventListener('load', function(){
            document.body.style.overflow = 'auto';
            document.getElementById('loading-box').classList.add("loaded")
        }, false)
    </script>
    <% } %>
   ```

3. 在主题目录 `/css` 下新增 `loading.css` ，并写入内容
   ```css
    #loading-box .loading-left-bg,
    #loading-box .loading-right-bg {
      position: fixed;
      z-index: 1000;
      width: 50%;
      height: 100%;
      background-color: #37474f;
      transition: all 0.5s;
    }
    
    #loading-box .loading-right-bg {
      right: 0;
    }
    
    #loading-box > .spinner-box {
      position: fixed;
      z-index: 1001;
      display: flex;
      justify-content: center;
      align-items: center;
      width: 100%;
      height: 100vh;
    }
    
    #loading-box .spinner-box .configure-border-1 {
      position: absolute;
      padding: 3px;
      width: 115px;
      height: 115px;
      background: #ffab91;
      animation: configure-clockwise 3s ease-in-out 0s infinite alternate;
    }
    
    #loading-box .spinner-box .configure-border-2 {
      left: -115px;
      padding: 3px;
      width: 115px;
      height: 115px;
      background: rgb(63, 249, 220);
      transform: rotate(45deg);
      animation: configure-xclockwise 3s ease-in-out 0s infinite alternate;
    }
    
    #loading-box .spinner-box .loading-word {
      position: absolute;
      color: #ffffff;
      font-size: 0.8rem;
    }
    
    #loading-box .spinner-box .configure-core {
      width: 100%;
      height: 100%;
      background-color: #37474f;
    }
    
    div.loaded div.loading-left-bg {
      transform: translate(-100%, 0);
    }
    
    div.loaded div.loading-right-bg {
      transform: translate(100%, 0);
    }
    
    div.loaded div.spinner-box {
      display: none !important; 
    }
    
    @keyframes configure-clockwise {
      0% {
        transform: rotate(0);
      }
    
      25% {
        transform: rotate(90deg);
      }
    
      50% {
        transform: rotate(180deg);
      }
    
      75% {
        transform: rotate(270deg);
      }
    
      100% {
        transform: rotate(360deg);
      }
    }
    
    @keyframes configure-xclockwise {
      0% {
        transform: rotate(45deg);
      }
    
      25% {
        transform: rotate(-45deg);
      }
    
      50% {
        transform: rotate(-135deg);
      }
    
      75% {
        transform: rotate(-225deg);
      }
    
      100% {
        transform: rotate(-315deg);
      }
    }
   ```

4. 在主题目录 `/layout/_partial/head.ejs`，在 `<head>` 标签中添加以下内容引入 `loading.css` 文件。
   ```html
    <link rel="stylesheet" type="text/css" href="<%- theme.jsDelivr.url %><%- url_for('/css/loading.css') %>">
   ```
5. 在主题目录 `/layout` 下找到 `layout.ejs`，然后在 `<body>` 标签下第一行引入 `loading.ejs`。

   ```js
    <%- partial('_widget/loading') %>
   ```

# 参考文献

[matery官方文档](https://github.com/blinkfox/hexo-theme-matery/blob/develop/README_CN.md)

[闪烁之狐博客](https://blinkfox.github.io/)

[cxy621的博客](https://hexo.cxy621.top/)

[MaosongRan简书](https://www.jianshu.com/p/e8d433a2c5b7?u_atoken=aab04d52-42a7-41a5-9ec1-2c34ec6d6f69&u_asession=018WKUdNnG6_0l5OUhdElYtB7eJNBsUu7y7hj0U4L8BdkJbHf4doxuopxnUSqptHk4X0KNBwm7Lovlpxjd_P_q4JsKWYrT3W_NKPr8w6oU7K9MNM_qjuacQ3mkmp1gywqfPIF6hypDqzN_tz02ZDuS7GBkFo3NEHBv0PZUm6pbxQU&u_asig=05GMhfeFGOEHiWf9nfeNuARXt-zBngi3Xqw0sW5R3YoPYfZVa2HJZISxVGhSPTjBzj4ABaWKZppfMpfBkxRD7FOXFKfCOrzNIxpptHDEVD4UvqsmpCDfMuq0Jv8fzxkL2FAocjyJqZg4HnC2gvSikzcpxZdCI5Yuxh3KtEGeMHlL39JS7q8ZD7Xtz2Ly-b0kmuyAKRFSVJkkdwVUnyHAIJzbhcXECjnUb6gL2kRcEGNiinyW_M0vYYgvPfUuB1znrFHLC90DffcRgc58NhmjbgM-3h9VXwMyh6PgyDIVSG1W9ZaGEhvt5pul5kydz9r3VL4b7QbEBqQMpA6Yx2M9XqKdOAH52o8OX-2f2q1nKQ_uK4QcDqR514V8mTbtKIw_0SmWspDxyAEEo4kbsryBKb9Q&u_aref=N6e3Ir7eFMD4IMGCTOu7xEtpyKg%3D)

[白马金羁侠少年CSDN](https://blog.csdn.net/qq_43827595/article/details/104324443)

[小苏的博客](https://fenghen0918.github.io/)

# 
