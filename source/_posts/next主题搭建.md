---
title: next主题搭建
date: 2022-07-27 11:13:42
categories:
  - 博客
tags:
  - 博客
img: /medias/featureimages/next.jfif
abbrlink: next
---

# 博客页面空白

**问题：**安装好主题之后，预览本地页面正常，但博客显示空白

**原因：**`next`默认使用jsdelivr作为DNS加速服务，但该服务国内备案已下架

**解决：**打开 **`themes\next\_config.yml`**，将`# FancyBox`以下修改为

```yml
# FancyBox
# jquery: //cdn.jsdelivr.net/npm/jquery@3/dist/jquery.min.js
# fancybox: //cdn.jsdelivr.net/gh/fancyapps/fancybox@3/dist/jquery.fancybox.min.js
# fancybox_css: //cdn.jsdelivr.net/gh/fancyapps/fancybox@3/dist/jquery.fancybox.min.css
jquery: https://cdn.bootcdn.net/ajax/libs/jquery/3.6.0/jquery.min.js
fancybox: https://cdn.bootcdn.net/ajax/libs/fancybox/3.5.1/jquery.fancybox.min.js
fancybox_css: https://cdn.bootcdn.net/ajax/libs/fancybox/3.5.1/jquery.fancybox
```

重新部署即可

# 修改主题

在Blog根目录文件夹下，用GitBash下载

```cmd
git clone https://github.com/theme-next/hexo-theme-next themes/next
```

打开根目录下的```_config.yml```，修改`# Extensions`下内容

```yml
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: next   #主题改为next
```

next提供了4种主题，只需将 **`themes\next\_config.yml`** 中`# Schemes`下内容中，想选择的主题前`#`删除即可

```yml
# Schemes
#scheme: Muse
#scheme: Mist
#scheme: Pisces
scheme: Gemini    #这是我选的主题
```

回到根目录打开Git Bash，输入如下三条命令：

```tex
hexo clean
hexo g
hexo d
```

# 博客美化

## 设置菜单

 <!-- more -->

打开`themes/next`下的`_config.yml`

```yml
menu:
  home: / || home                      #首页
  archives: /archives/ || archive      #归档
  categories: /categories/ || th       #分类
  tags: /tags/ || tags                 #标签
  about: /about/ || user               #关于
  #schedule: /schedule/ || calendar    #日历
  #sitemap: /sitemap.xml || sitemap    #站点地图，供搜索引擎爬取
  #commonweal: /404/ || heartbeat      #腾讯公益404
```

```英文名称:  /目标地址/  ||  图标(Font Awesome)```

将对应英文翻译中文，打开`theme/next/languages/zh-CN.yml`，在`menu`下编辑翻译

在根目录下打开Git Bash，添加页面，会在根目录的`sources`文件夹下会生成`categories`、`tags`、`about`子文件夹

```shell
hexo new page "categories"
hexo new page "tags"
hexo new page "about"
```

## 设置建站时间

打开主题配置文件`themes/next/_config.yml`

```yml
footer:
  # Specify the date when the site was setup. If not defined, current year will be used.
  since: 2020-02   #建站时间
```

## 设置头像

打开主题配置文件`themes/next/_config.yml`

```yml
# Sidebar Avatar
avatar:
  url: /images/avatar.gif   #头像图片的位置
  rounded: true   #头像展示在圈里
  rotated: false  #头像随光标旋转
```

## 网站图标设置

打开主题配置文件`themes/next/_config.yml`

```yml
favicon:
  small: /images/sast-16×16.jpg
  medium: /images/sast-32×32.jpg
  apple_touch_icon: /images/apple-touch-icon-next.png
  safari_pinned_tab: /images/logo.svg
  #android_manifest: /images/manifest.json
  #ms_browserconfig: /images/browserconfig.xml
```

修改`small`和`medium`到图片路径，我在[制作图](https://www.zhizuotu.com/msize)网站将手边的图片裁剪为$16\times 16$像素以及$32\times 32$像素

## 设置动态背景

### canvas nest 风格

在`themes/next`目录下打开Git Bash

```shell
git clone https://github.com/theme-next/theme-next-canvas-nest source/lib/canvas-nest
```

打开主题配置文件`themes/next/_config.yml`

```yml
# Canvas-nest
# Dependencies: https://github.com/theme-next/theme-next-canvas-nest
# For more information: https://github.com/hustcc/canvas-nest.js
canvas_nest:
  enable: true
  onmobile: true # Display on mobile or not
  color: "255,255,255" # RGB values, use `,` to separate
  opacity: 0.5 # The opacity of line: 0~1
  zIndex: -1 # z-index property of the background
  count: 99 # The number of lines
```

### JavaScript 3D library风格

在`themes/next`目录下打开Git Bash

```shell
git clone https://github.com/theme-next/theme-next-three source/lib/three
```

打开主题配置文件`themes/next/_config.yml`

将想要的风格后的`false`改为`true`

```yml
# JavaScript 3D library.
# Dependencies: https://github.com/theme-next/theme-next-three
three:
  enable: true
  three_waves: false
  canvas_lines: true
  canvas_sphere: false
```

## 设置背景图片

打开主题配置文件`themes/next/_config.yml`，取消`style`前的注释

```yml
custom_file_path:
  style: source/_data/styles.styl
```

## 添加顶部加载条

在`themes/next`目录下打开Git Bash

```shell
git clone https://github.com/theme-next/theme-next-pace source/lib/pace
```

打开主题配置文件`themes/next/_config.yml`，将`pace`下`enable`修改为`true`，并在`theme`后选择合适加载条

```yml
pace:
  enable: true
  # Themes list:
  # big-counter | bounce | barber-shop | center-atom | center-circle | center-radar | center-simple
  # corner-indicator | fill-left | flat-top | flash | loading-bar | mac-osx | material | minimal
  theme: bounce
```

## 设置预览摘要

在正文摘要部分撰写

```markdown
 <!-- more -->
```

## 设置侧边栏显示效果

打开主题配置文件`themes/next/_config.yml`，修改`sidebar`下设置

```yml
sidebar:
  # Sidebar Position. #设置侧边栏位置
  position: left
  #position: right

  #  - post    默认显示模式
  #  - always  一直显示
  #  - hide    初始隐藏
  #  - remove  移除侧边栏
  display: post
```

## 侧边栏推荐链接

打开主题配置文件`themes/next/_config.yml`，修改`links`下设置

```yml
links_settings:
  icon: link
  title: 链接网站  #修改名称

links:
  #Title: http://yoursite.com
  我的网站: http://网站链接.com
```

## 添加社交链接

这一块就是放置自己其他社交账号地方

打开主题配置文件`themes/next/_config.yml`，修改`social`下设置

```yml
social:
  GitHub: https://github.com/yourname || fab fa-github
  #E-Mail: mailto:yourname@gmail.com || fa fa-envelope
  #Weibo: https://weibo.com/yourname || fab fa-weibo
  #Google: https://plus.google.com/yourname || fab fa-google
  #Twitter: https://twitter.com/yourname || fab fa-twitter
  #FB Page: https://www.facebook.com/yourname || fab fa-facebook
  #StackOverflow: https://stackoverflow.com/yourname || fab fa-stack-overflow
  #YouTube: https://youtube.com/yourname || fab fa-youtube
  #Instagram: https://instagram.com/yourname || fab fa-instagram
  #Skype: skype:yourname?call|chat || fab fa-skype
```

## 文章末尾添加版权说明

打开主题配置文件`themes/next/_config.yml`，修改`creative_commons`下设置

```yml
creative_commons:
  license: by-nc-sa
  sidebar: false
  post: true  # 将post改为true
  language:
```

## 添加评论功能

在[来必利网站](http://livere.com/)注册登录，并获得uid

打开主题配置文件`themes/next/_config.yml`，将```livere_uid```改为所获得uid

