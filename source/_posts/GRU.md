---
title: GRU
author: Jarrycow
img: /medias/featureimages/xxx.png
cover: false
top: false
mathjax: true
categories:
  - null
tags:
  - null
keywords: GRU
abbrlink: GRU
date: 2023-08-04 10:38:26
---



<!--more-->

GRU 将 忘记和输入门组合成一个单一的“更新门”。它还将单元格状态（memory cell，即 $C^t$）和隐藏状态（$h^t$）合并。

GRU 参数量降了一些，但是性能和LSTM几乎一样。

GRU 只传递 $h^t$，不再有 $C^t$。

# GRU 结构

**重置门 reset gate：**

**更新门 update gate：**GRU中的遗忘门和输入门是联动的。具体就是说，如果有新的信息进来，才会忘掉之前的信息，而如果没有新信息进来，就不会忘记信息；当你忘记信息的时候，就会有新的信息进来，这个逻辑听上去也是颇为合理的。