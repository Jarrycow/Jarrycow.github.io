---
title: 条件随机场 CRF
author: Jarrycow
img: /medias/featureimages/xxx.png
cover: false
top: false
mathjax: true
categories:
  - AI
tags:
  - 机器学习
keywords: 条件随机场 CRF
abbrlink: 条件随机场 CRF
date: 2023-07-28 15:51:11
---



<!--more-->

# CRF 在干啥

为事件构建分类器时，为了增加标注的准确率考虑时间顺序，这就是 CRF 在做的事情。

## CRF 定义

设 $X$ 与 $Y$ 是随机变量，$P(Y|X)$ 是在给定X的条件下Y的条件概率分布。若随机变量 $Y$ 构成一个由无向图 $G=(V, E)$ 表示的马尔科夫随机场对任意结点v成立，则称条件概率分布 $P(Y|X)$ 为**条件随机场**。
$$
P\left(Y_v\left|\right.X,Y_w,w\ne v\right)=P\left(Y_v\left|\right.X,Y_w,w\sim v\right)
$$

- $w\sim v$：在图 $G=(V, E)$ 中与结点 $v$ 有边连接的所有结点 $w$
- $w\ne v$：结点 $v$ 以外的所有结点
- $Y_v$ 与 $Y_w$：结点 $v, u$ 与 $w$ 对应的随机变量即**一个结点仅与它直接相连的结点相关**。

# CRF 中特征函数

如何判断一个标注序列是否靠谱，需要定义一个特征函数集合，用这个特征函数集合来为一个标注序列打分，并据此选出最靠谱的标注序列。也就是说，**每一个特征函数都可以用来为一个标注序列评分，把集合中所有特征函数对同一个标注序列的评分综合起来，就是这个标注序列最终的评分值**。

CRF 特征函数接受四个参数：

- `s`：要标注词性的句子
- `i`：表示句子第 `i` 个单词
- `l_i`：要评分的标注序列给第 `i` 个单词标注的词性
- `l_{i-1}`：要评分的标注序列给第 `i-1` 个单词标注的词性

**输出：**0/1，0 表示评分标注序列不符合该特征；1 表示评分标注序列符合该特征

<font color="blue">注：该特征函数仅仅依靠当前单词的标签和它前面的单词的标签对标注序列进行评判，这样建立的CRF也叫作**线性链CRF**，这是CRF中的一种简单情况。</font>

定义好一组特征函数后，我们要给每个特征函数 $f_j$ 赋予权重 $λ_j$，用句子 $s$ 和标注序列 $l$ ，利用定义特征函数对 $l$ 进行评分：
$$
score\left(l\left|\right.s\right)=\sum_{j=1}^m\sum_{i=1}^n\lambda_jf_j\left(s,i,l_i,l_{i-1}\right)
$$
外面的求和用来求每一个特征函数fj评分值的和，里面的求和用来求句子中每个位置的单词的的特征值的和。

对这个分数进行指数化和标准化，我们就可以得到标注序列l的概率值 $p(l|s)$：

$$
p\left(l\left|\right.s\right)=\dfrac{\exp\left[score\left(l\left|\right.s\right)\right]}{\sum_{l'}\exp\left[score\left(l\left|\right.s\right)\right]}\\=\dfrac{\exp\left[\sum_{j=1}^m\sum_{i=1}^n\lambda_jf_j\left(s,i,l_i,l_{i-1}\right)\right]}{\sum_{l'}\exp\left[\sum_{j=1}^m\sum_{i=1}^n\lambda_jf_j\left(s,i,l'_i,l'_{i-1}\right)\right]}
$$







# CRF 与其他模型比较

## CRF与逻辑回归的比较

条件随机场是逻辑回归的**序列化版本**。逻辑回归是用于分类的对数线性模型，条件随机场是用于序列化标注的对数线性模型。

## CRF与HMM的比较

每一个 HMM 模型都等价于某个 CRF，但是 CRF 要比 HMM 更加强大：

- **CRF 可以定义数量更多，种类更丰富的特征函数：**HMM 天然具有局部性，即 HMM 模型中，当前单词只依赖于当前的标签，当前的标签只依赖于前一个标签。这样的局部性限制了HMM只能定义相应类型的特征函数。但是 CRF 却可以着眼于整个句子 s 定义更具有全局性的特征函数。
- **CRF 可以使用任意的权重：**将对数HMM模型看做CRF时，特征函数的权重由于是log形式的概率，所以都是小于等于0的，而且概率还要满足相应的限制；但在CRF中，每个特征函数的权重可以是任意值，没有这些限制。





# 参考资料

[CRF条件随机场](https://luweikxy.gitbook.io/machine-learning-notes/conditional-random-field#ding-yi-crf-zhong-de-te-zheng-han-shu)
