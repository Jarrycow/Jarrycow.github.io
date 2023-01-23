---
title: 在博客中写latex矩阵
author: Jarrycow
img: /medias/featureimages/xxx.png
cover: false
top: false
mathjax: true
categories:
  - 博客
tags:
  - 博客
  - latex
keywords: 在博客中写latex矩阵
abbrlink: matrix_in_blog
date: 2022-12-07 11:19:53
---

博客中的 mathjax 公式一旦遇到单边大括号即无法加载

<!--more-->

# 问题

1. `\left\{` 的单边大括号无法加载

   $\left\{\right.$

2. `\\` 不换行

   $A\\B$

# 原因和解决

1. `\{` 会被转义，因此要修改为 `\\{`

   $\\{$

2. `\\` 会被转义，要修改为 `\\\\`

   $A\\\\B$
