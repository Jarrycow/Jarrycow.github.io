---
title: Agent体系结构和分层控制
author: Jarrycow
img: /medias/featureimages/xxx.png
cover: false
top: false
mathjax: true
categories:
  - null
tags:
  - null
keywords: Agent体系结构和分层控制
abbrlink: Agent体系结构和分层控制
date: 2022-11-03 13:25:32
---



Agent在环境中如何推理、动作，涉及Agent内部结构

<!--more-->

# Agent系统





Agent应构建实时性，能即时接受传感器信息并作出即时反映。

假定$T$为时间点集合，$P$为所有可能感知点集合；**感知轨迹**则为$T\rightarrow P$的函数，描述了每个时间点观察的事物

**命令轨迹：**所有命令信息序列

**转换：**感知轨迹$\rightarrow$命令轨迹

转换是具有**因果**联系的，可视作从Agent在$t$时刻历史到现在发出的命令函数

**信念状态：**Agent在$t$时间记住的信息综合

离散时间下信念转换函数可表示为
$$
remember: S\times P\rightarrow S\\
s_{t+1}=remember(s_t, p_t)
$$

> $S:$信念状态集
>
> $P:$可能认知集合
>
> 即$s_{t+1}$是在$s_t$状态下观察$p_t$得到的

指令函数可表示为
$$
do:S\times P\rightarrow C\\
c_t=do(s_t,p_t)
$$

> $C：$指令集合

若仅存储有限的可能信念状态，控制器可称为**有限状态控制器**或**有限状态机**

# 分层控制



