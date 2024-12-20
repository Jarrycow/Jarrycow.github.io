---
title: CSS 布局
author: Jarrycow
img: /medias/featureimages/CSS_box.webp
cover: false
top: false
mathjax: true
categories:
  - 前端
tags:
  - CSS
keywords: CSS 盒子模型
abbrlink: CSS_box
date: 2024-06-20 21:39:13
summary: 世界就是一个个盒子组成的——海绵宝宝
---



**清除默认样式**

```css
* {
    margin: 0;
    padding: 0;
}

// 左浮动
.leftfix {
    float: left;
}

// 右浮动
.rightfix {
    float: right;
}

// 清除浮动影响
.clearfix::after {
    content: '';
    display: block;
    clear: both;
}
```





# 盒子模型

![盒子模型组成](https://raw.githubusercontent.com/Jarrycow/picHost/main/JavaWeb/boxModel.png)

- **外边距 margin：**盒子与外界距离
- **边框 border：**盒子边框<sub>压在 content 上</sub>
- **内边距 padding：**内容和外框距离
- **内容 content：**元素中的文本与后代

盒子大小 = `content` + `padding` + `border`

盒子相关属性均**不能继承**

## 内容 content

| 属性名       | 含义                                                         |
| ------------ | ------------------------------------------------------------ |
| `width`      | 内容区宽度<br />默认 = 父属性`content` - `margin` - `border` - `padding` |
| `max-width`  | 内容区最大宽度                                               |
| `min-width`  | 内容区最小宽度                                               |
| `height`     | 内容区高度                                                   |
| `max-height` | 内容区最大高度                                               |
| `min-height` | 内容区最小高度                                               |

## 内边距 padding

| 属性 | 含义 | 值   |
| ---- | ---- | ---- |
| `padding-top` | 上内边距 | 长度 |
| `padding-right` | 右内边距 | 长度 |
| `padding-bottom` | 下内边距 | 长度 |
| `padding-left` | 左内边距 | 长度 |
| `padding` | 边距 | · `padding: 值一px;`：四个方向边距都是值一<br /> · `padding: 值一px 值二px;`：上下，左右<br /> · `padding: 值一px 值二px 值三px;`：上，左右，下<br /> · `padding: 值一px 值二px 值三px 值四px;`：上，右，下，左 |

- 行内元素左右内边距没问题，上下内边距不能完美设置

## 边框 border

| 属性名         | 含义     | 值                                                           |
| -------------- | -------- | ------------------------------------------------------------ |
| `border-width` | 边框宽度 | 长度值                                                       |
| `border-color` | 边框颜色 | 颜色值                                                       |
| `border-style` | 边框样式 | ·  `none`：默认值<br />·  `solid`：实线<br />·  `dashed`：虚线<br />·  `dotted`：点线<br />·  `double`：双实线 |
| `border`       | 边框     |                                                              |

## 外边距 margin

| 属性 | 内容 |
| ---- | ---- |
| `margin-left` |   上外边距   |
| `margin-right` |   右外边距   |
| `margin-top` |   左外边距   |
| `margin-bottom` |   下外边距   |
| `margin`    |   外边距   |

- 子元素 `margin` 参考父元素 `content` 计算

- 上左 `margin` 影响自己位置

- 下右 `margin` 影响后面兄弟元素位置

- `margin` 值为 `auto` 为居中

- **margin 塌陷：**首个子元素的**上外边距**会应用在父元素；首个子元素**下外边距**会应用在父元素

  解决方案：给父元素设置 css：`overflow: hidden`

- **margin 合并：**上面兄弟元素的**下外边距**和下面兄弟**上外边距**合并，取最大值

## 内容溢出

溢出就是内容里的要素太多啦，超出了 `content` 框架

| 属性         | 内容                 | 值                                                           |
| ------------ | -------------------- | ------------------------------------------------------------ |
| `overflow`   | 处理溢出内容方式     | · `visible`：显示，默认<br /> · `hidden`：隐藏<br /> · `scroll`：显示滚动条，不论内容是否溢出<br /> · `auto`：自动显示滚动条，内容溢出不溢出 |
| `overflow-x` | 横向处理溢出内容方式 | 同上                                                         |
| `overflow-y` | 纵向处理溢出内容方式 | 同上                                                         |

## 元素隐藏

| 属性 | 内容 | 值   |
| ---- | ---- | ---- |
| `visibility` | 隐藏但占位 | · `show`：默认显示<br />· `hidden`：隐藏 |
| `display` | 隐藏且不占位 | · `none`：隐藏 |

## 子元素在父元素居中

1. 行内块元素、行内元素可以被父元素当文本处理<sub>可以用` text-align`、`line-height`、`text-indent` 等文本属性</sub>
2. 子元素在父元素水平居中

  - 若子元素为块元素，父元素加上：`margin:0 auto`
  - 若子元素为行内元素、行内块元素，给父元素加上：`text-aligin:center`

3. 子元素在父元素垂直居中

  - 若子元素为块元素，子元素加上：`margin-top: 值;`，值为 `(父元素 content - 子元素盒子总高度) / 2`
  - 若子元素为行内元素、行内块元素，给父元素的：`height`=`line-heigh;`、` font-size=0;`，每个子元素加上：`vertical-align:middle;`

## 元素之间空白间隔

| 空白类型 | 产生原因 | 解决方案 |
| -------- | -------- | -------- |
| 元素之间空白间隔 | 行内元素与行内块元素彼此之间换行会被解析为空白字符 | 给父元素设置 `font-size: 0;`，再给要显示文字元素单独设置 |
| 行内块幽灵空白 | 行内块元素与文本基线对齐，但是基线和底部有距离 | · 给行内块设置 `vertical`，值不是 `baseline` 就好<br /> · 若父元素只有图片，更改图片显示模式 `display:block`<sub>但如果有文字，会被挤下去</sub><br /> · 父元素设置 `font-size:0`<sub>还有其他文字，需要单独设置文字大小</sub><br /> |

# 浮动

原本是做*文字环绕图片*的工作

## 浮动特点

- 脱离文档流
- 浮动后均可设置宽高
- 不会独占一行
- 不会 `margin` 合并和 `margin` 塌陷
- 不会被当作文本处理

## 浮动影响

- 对兄弟元素影响：后面兄弟元素会占据浮动元素之前的位置，在浮动元素下面

- 对父元素影响：不能撑起父元素高度，导致父元素高度塌陷；父元素宽度依然束缚浮动元素

- 解决方案：给浮动元素父元素设置伪元素；兄弟元素要么浮动要么都不浮动

  ```css
  .选择器:affter {
      content: "";
      display: block;
      clear: both;
  }
  ```

# 定位

可以通过 `z-index` 调整定位层级

## 相对定位

以原定位置为参考点移动

```css
position: relative;  /* 开启相对位置 */
left: 值px;
right: 值px;
top: 值px;
bottom: 值px;
```

- `left` 和 `right` 不能一起设置
- `top` 和 `bottom` 不能一起设置

只是视觉位置变化，不会挤压其他元素，但会盖在其他元素身上

如果都进行相对定位，后面盖前面的s

## 绝对定位

递归查询父元素直到查询到设置定位的元素，以该元素为参考点定位

```css
position: absolute;
```

绝对定位和浮动不能同时设置，否则浮动失效

## 固定定位

一直会固定在那儿，不管怎么滑动

```css
position: fixed;
```

固定定位和浮动不能同时设置，否则浮动失效

## 粘性定位

在元素到达某个位置时候固定，以递归查询具有“滚动”机制的祖先为准

```css 
position: sticky;
```

## 定位元素位置

### 定位元素充满包含块

- 宽与包含块一致：

  ```css
  left: 0;
  right: 0;
  ```

- 高与包含块一致：

  ```css
  top: 0;
  bottom: 0;
  ```

### 定位元素在包含块居中

```css
left: 0;
right: 0;
top: 0;
bottom: 0;
margin: auto;
height: 值px;
width: 值px;
```

