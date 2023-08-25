---
title: Lisp 语法
author: Jarrycow
img: /medias/featureimages/xxx.png
cover: false
top: false
mathjax: true
categories:
  - null
tags:
  - null
keywords: Lisp 语法
abbrlink: Lisp 语法
date: 2023-05-05 21:05:50
---

Lisp 编程语言的基本语法。

<!--more-->

# 表达式

Lisp 采用前缀表达式

```lisp
(操作 a b)
```

**前缀表达式优点**

- 完全适用于可能带有任意个实参的过程
- 可以直接扩充，允许出现组合式嵌套

**前缀表达式缺点**

- 与常规数学表示差别很大

# 命名 & 环境

