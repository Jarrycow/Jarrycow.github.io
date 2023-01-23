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

## 修改副标题

修改`themes\_config.yml` > `subtitle:`

在`sub`下添加语句

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

# 博客优化





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

# 参考文献

[matery官方文档](https://github.com/blinkfox/hexo-theme-matery/blob/develop/README_CN.md)

[闪烁之狐博客](https://blinkfox.github.io/)

[cxy621的博客](https://hexo.cxy621.top/)

[MaosongRan简书](https://www.jianshu.com/p/e8d433a2c5b7?u_atoken=aab04d52-42a7-41a5-9ec1-2c34ec6d6f69&u_asession=018WKUdNnG6_0l5OUhdElYtB7eJNBsUu7y7hj0U4L8BdkJbHf4doxuopxnUSqptHk4X0KNBwm7Lovlpxjd_P_q4JsKWYrT3W_NKPr8w6oU7K9MNM_qjuacQ3mkmp1gywqfPIF6hypDqzN_tz02ZDuS7GBkFo3NEHBv0PZUm6pbxQU&u_asig=05GMhfeFGOEHiWf9nfeNuARXt-zBngi3Xqw0sW5R3YoPYfZVa2HJZISxVGhSPTjBzj4ABaWKZppfMpfBkxRD7FOXFKfCOrzNIxpptHDEVD4UvqsmpCDfMuq0Jv8fzxkL2FAocjyJqZg4HnC2gvSikzcpxZdCI5Yuxh3KtEGeMHlL39JS7q8ZD7Xtz2Ly-b0kmuyAKRFSVJkkdwVUnyHAIJzbhcXECjnUb6gL2kRcEGNiinyW_M0vYYgvPfUuB1znrFHLC90DffcRgc58NhmjbgM-3h9VXwMyh6PgyDIVSG1W9ZaGEhvt5pul5kydz9r3VL4b7QbEBqQMpA6Yx2M9XqKdOAH52o8OX-2f2q1nKQ_uK4QcDqR514V8mTbtKIw_0SmWspDxyAEEo4kbsryBKb9Q&u_aref=N6e3Ir7eFMD4IMGCTOu7xEtpyKg%3D)

[白马金羁侠少年CSDN](https://blog.csdn.net/qq_43827595/article/details/104324443)

[小苏的博客](https://fenghen0918.github.io/)

# 文章开头format格式

| 配置选项        | 默认值                     | 描述                                                         |
| :-------------- | :------------------------- | :----------------------------------------------------------- |
| `title`         | 文件标题                   | 文章标题                                                     |
| `date`          | 创建时间                   | 发布时间                                                     |
| `author`        | `_config.yml`  > `author`  | 文章作者                                                     |
| `img`           | `featureImages` 中的某个值 | 文章封面图(默认在`/medias/featureimages/xxx.png`)            |
| `top`           | `true`                     | 首页推荐文章                                                 |
| `cover`         | `false`                    | 文章加入首页轮播                                             |
| `coverImg`      | `featureImages`            | 首页轮播封面显示图片                                         |
| `password`      |                            | 文章阅读密码                                                 |
| `toc`           | `true`                     | 开启 TOC                                                     |
| `mathjax`       | `false`                    | 开启数学公式                                                 |
| `summary`       |                            | 文章摘要(我喜欢在正文用`<!-- more -->`)                      |
| `categories`    |                            | 文章分类                                                     |
| `tags`          |                            | 文章标签                                                     |
| `keywords`      | 文章标题                   | SEO关键字                                                    |
| `reprintPolicy` | `cc_by`                    | 文章转载规则， 可以是`cc_by`、`cc_by_nd`、`cc_by_sa`、`cc_by_nc`、 `cc_by_nc_nd`、`cc_by_nc_sa`、`cc0`、`noreprint`、`pay` |

可以在`\scaffolds\post.md`中修改模板
