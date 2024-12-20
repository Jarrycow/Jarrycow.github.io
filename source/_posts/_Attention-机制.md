---
title: Attention 机制
author: Jarrycow
img: /medias/featureimages/xxx.png
cover: false
top: false
mathjax: true
categories:
  - null
tags:
  - null
keywords: Attention 机制
abbrlink: Attention 机制
date: 2023-08-31 10:11:44
---

面试被问到了，不会......

<!--more-->

# 为啥要 Attention

### Encoder-Decoder 模型

Encoder-Decoder 模型并非特定具体算法，而是一个统称：「将现实问题转换为数学问题，求解数学问题，进而解决现实问题」

![Encoder-Decoder 模型](https://raw.githubusercontent.com/Jarrycow/picHost/main/JavaWeb/954a9-2019-10-28-attention.png)

无论输入、输出长度，中间的「向量 c」 长度都是固定的。因此，RNN 存在长程梯度消失的问题，对于较长的句子，很难将输入的序列转化为定长的向量而保存所有的有效信息，限制模型理解能力。

Attention 机制核心逻辑就是「**从关注全部到关注重点**」。

# Attention 机制

## Attention 原理

![attention 原理图](https://raw.githubusercontent.com/Jarrycow/picHost/main/JavaWeb/efa5b-2019-11-13-3step.png.webp)

1. query 和 key 进行相似度计算 $S\left(Q,K\right)$，得到权值 $s$
2. 将权值归一化 $\text{SoftMax}()$，得到可用注意力分布权值 $\alpha$
3. 将权重和 value 进行加权求和

$$
\alpha_i=softmax\left(s\left(X_i,q\right)\right)
\\
att\left(q,X\right)=\sum_{i=1}^N\alpha_iX_i
$$

> 以想了解圆神为例：
>
> 图书馆（source）里有很多资料（value），为方便查找对资料编号（key）。当我们想了解圆神（query）时，可以看相关资料。为提高效率，针对圆神相关设定集看得仔细一点（权重高），但是尼卡哲学书简单看下（权重低）。

## 相似度计算

在做 Attention 的时候，我们需要计算 query 和某个 key 的相似度，常用方法有：

- **点乘：**$s\left(q,k\right)=q^T\cdot k$
- **矩阵相乘：**$s\left(q,k\right)=q^Tk$
- **$\cos$ 相似度：**$s\left(q,k\right)=\dfrac{q^Tk}{\lVert \left. q \rVert \right.\cdot \lVert \left. k \rVert \right.}$
- **串联方式：**$s\left(q,k\right)=W\left[q;k\right]$
- **多层感知机：**$s\left(q,k\right)=v^T_a\tan h\left(Wq+Uk\right)$

## Attention 优点

- **参数少：**模型参数相较 CNN、RNN 相比，参数更少，复杂度更少，对算力要求更小
- **速度快：**模型解决 RNN 不能并行计算的问题，计算不依赖上一步计算结果，可以和 CNN 一样并行处理
- **效果好：**从较长文本中挑选重点，不丢失重要的信息

# Attention 类型

![Attention 的种类](https://raw.githubusercontent.com/Jarrycow/picHost/main/JavaWeb/7fc4f-2019-11-13-types.png)

# 模型方面

从模型上看，Attention 一般用在 CNN 和 LSTM 上，也可以直接进行纯 Attention 计算。

## CNN + Attention

CNN 卷积可以提取操作特征，也算 Attention 思想，但是 CNN 卷积视野局部，需要叠加多层卷积扩大视野。

- 在卷积前增加 Attention：文本蕴含任务需要处理两段文本，同时对两段输入的序列向量进行attention，计算出特征向量，再拼接到原始向量中，作为卷积层的输入
- 卷积操作后做 Attention：对两段文本的卷积层的输出做attention，作为pooling层的输入
- 在 pooling 层做 attention，代替 max pooling：首先我们用LSTM学到一个比较好的句向量，作为query，然后用CNN先学习到一个特征矩阵作为key，再用query对key产生权重，进行attention，得到最后的句向量

## LSTM + Attention

LSTM通常需要得到一个向量，再去做任务，常用方式有：

a. 直接使用最后的hidden state（可能会损失一定的前文信息，难以表达全文）

b. 对所有step下的hidden state进行等权平均（对所有step一视同仁）。

c. Attention机制，对所有step的hidden state进行加权，把注意力集中到整段文本中比较重要的hidden state信息。性能比前面两种要好一点，而方便可视化观察哪些step是重要的，但是要小心过拟合，而且也增加了计算量。

# 参考文献

[产品经理的人工智能学习库《Encoder-Decoder 和 Seq2Seq》](https://easyai.tech/ai-definition/encoder-decoder-seq2seq/)

[产品经理的人工智能学习库《Attention 机制》](https://easyai.tech/ai-definition/attention/)
