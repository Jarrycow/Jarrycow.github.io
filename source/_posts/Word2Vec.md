---
title: Word2Vec
author: Jarrycow
img: /medias/featureimages/xxx.png
cover: false
top: false
mathjax: true
categories:
  - nlp
tags:
  - nlp
keywords: Word2Vec
abbrlink: Word2Vec
date: 2023-07-18 09:13:06
---



<!--more-->

# Word2Vec

Word2vec 是只有一个隐层的全连接神经网络, 用来预测给定单词的关联度大的单词。

![Word2vec](https://raw.githubusercontent.com/Jarrycow/picHost/main/JavaWeb/v2-66919569386cb606d29a33d7b8b87f57_r.jpg)

## Word2Vec 步骤

1. 在输入层，一个词被转化为 One-Hot 向量。
2. 第一个隐层，输入的是一个 $Wx+b$
3. 输出层可以简单看成一个分类器，用的是Softmax回归，最后输出的是每个词对应的概率

# CBOW

![CBOW](https://raw.githubusercontent.com/Jarrycow/picHost/main/JavaWeb/v2-7519805289ec4a9f1d4bfe04671516bc_r.jpg)

普通的CBOW模型通过上下文来预测中心词，抛弃了词序信息

**输入层：**$ 2m×\left|V\right|$个节点 $2m$ 个词的 one-hot 编码表示

**输入层到投影层到连接边：**输入词矩阵 $V:n×|V|$ 维

**投影层：**$n$ 个节点，上下文共 $2m$ 个词的词向量直接相加后求得的平均值

**投影层到输出层连接边：**输出词矩阵 $U_{\left|V\right|\times n}$

**输出层：**$\left|V\right|$ 个节点

# Skip-Gram

Skip-Gram 模型采取 CBOW 的逆过程的动机在于：CBOW 算法对于很多分布式信息进行了平滑处理<SUB>如将一整段上下文信息视为一个单一观察量</sub>。很多时候，对于小型的数据集，这一处理是有帮助的。相比之下，Skip-Gram模型将每个 “上下文-目标词汇” 的组合视为一个新观察量，这种做法在大型数据集中会更为有效。

![Skip-Gram](https://raw.githubusercontent.com/Jarrycow/picHost/main/JavaWeb/v2-ca81e19caa378cee6d4ba6d867f4fc7c_r.jpg)

# 加速训练技巧

对于原始的模型来说，由于使用的是 softmax 函数，时间复杂度为 $O(|V|)$ ，因此计算代价很大，对大规模的训练语料来说，非常不现实。因此Mikolov 提出2个技巧加速训练。

## Hierarchical Softmax

Hierarchical Softmax是一种对输出层进行优化的策略，输出层从原始模型的利用softmax计算概率值改为了利用**哈夫曼树**计算概率值。

哈夫曼树在叶子节点及叶子节点的权给定的情况下，该树的带权路径长度最短。直观上可以看出，叶子节点的权越大，则该叶子节点就应该离根节点越近。因此对于模型来说就是，词频越高的词，距离根节点就越近。Hierarchical Softmax正是利用这条路径来计算指定词的概率，而非用softmax来计算。

![哈夫曼树](https://raw.githubusercontent.com/Jarrycow/picHost/main/JavaWeb/v2-ea510d96287ec366696f58e44157ef05_1440w.webp)

简单来说，应用Hierarchical Softmax就是把 N 分类问题变成 log(N)次二分类

## Negative Sampling

把语料中的一个词串的中心词替换为别的词，构造语料 D 中不存在的词串作为负样本。本质上就是一个预测全部分类的变成预测总体类别的子集的方法。在这种策略下，优化目标变为了：最大化正样本的概率，同时最小化负样本的概率。

# 参考文献

[evtor-全面理解word2vec](https://zhuanlan.zhihu.com/p/33799633)