---
title: Transformer
author: Jarrycow
img: /medias/featureimages/xxx.png
cover: false
top: false
mathjax: true
categories:
  - null
tags:
  - null
keywords: Transformer
abbrlink: Transformer
date: 2023-08-31 12:23:33
---



<!--more-->

![Transformer 架构](https://raw.githubusercontent.com/Jarrycow/picHost/main/JavaWeb/Transformer 架构.png)

# Transformer 架构

Transformer 模型利用 Attention，去掉 RNN。Transformer 架构和 Encoder-Decode r模型有些相似，左边是 Encoder 部分，右边是 Decoder 部分。

**超参数：**

- $N$：
- $D_{model}$：词向量长度 `d_model`，默认 512
- $d_{ff}$：
- $h$：heads 数目 $h$，默认为 8 
- $d_k$：
- $d_v$：
- $P_{drop}$：`dropOut`，默认 0.1
- $\epsilon _{ls}$：
- $\text{train steps}$：

```make_model(src_vocal, tgt_vocab, N=6, d_model=512, d_ff = 2048, h=8, dropout=0.1) ```

## 输入 Input Embedding

经过 word embedding 和 positional embedding 后可以得到一个句子的 representation。即每个向量包含 word 特征和 word 位置信息。输入的 size 为 `[nbatches, L, 512]`。

- nbatches ：batch_size
- L ：sequence 的长度,(比如“我爱你”，L = 3)
- d_model：embedding 的 dimension

### Word Embedding

对词进行 embedding，用较短的向量表达这个 word 的属性。

在 Pytorch 中使用 `nn.Embedding` 或用用 one-hot vector 与权重矩阵 W 相乘得到。nn.Embedding 包含一个权重矩阵 W。其 shape 为 ( num_embeddings，embedding_dim )。num_embeddings 指的是词汇量，即想要翻译的 vocabulary 的长度。embedding_dim 指的是想用多长的 vector 来表达一个词，可以任意选择，比如64，128，256，512等。在 Transformer 论文中选择的是512(即 `d_model =512`)。

### Positional Embedding

embedding 本身不包含在句子中的相对位置信息，且在 Transformer 中，输入句子的所有 word 是同时处理的，没有考虑词的排序和位置信息。positional encoding“ 使得 Transformer 可以衡量 word 位置有关的信息
$$
PE_{\left(pos,2i\right)}=\sin\left(pos/10000^{\frac{2i}{d_{\text{model}}}}\right)
\\
PE_{\left(pos,2i+1\right)}=\cos\left(pos/10000^{\frac{2i}{d_{\text{model}}}}\right)
$$

- $pos$：该 word 在这个句子中的位置
- $i$：embedding 维度。比如选择 d_model=512，那么i就从1数到512

## Encoder 模块

输入单词的 Embedding，再加上位置编码，然后进入统一的结构，循环 N 次。每一层又可以分成Attention层和全连接层，再额外加了一些处理，比如Skip Connection，做跳跃连接，然后还加了Normalization层。

![Encoder 层](https://raw.githubusercontent.com/Jarrycow/picHost/main/JavaWeb/v2-e7e0a2f384b39efb8ff9d5065ecf0a71_r.jpg)

每一层包含了2部分

### Multi-Head Attention Mechanism

attention 机制的输入定义为 x

- Encoder 开始，x 为句子的 representation
- 各层中间，x 代表前一层 EncoderLayer 的输出

使用不同的 linear layers 基于 x 来计算 keys，queries和values，三者相互独立，权重不同

- key = linear_k(x)
- query = linear_q(x)
- value = linear_v(x)

计算 Attention：
$$
Attention\left(Q,K,V\right)=\text{softmax}\left(\dfrac{QK^T}{\sqrt{d_k}}\right)V
$$

- $\sqrt{d_k}$：为防止 $d_k$ 增大，$QK^T$ 过大

  $d_k=\dfrac{d_{model}}k$

### Position-Wise fully connected feed-forward network

feed-forward network
$$
FFN(X)=\max\left(0,xW_1+b_1\right)W_2+b_2
$$

## Decoder 模块

第一次输入是前缀信息，之后的就是上一次产出的Embedding，加入位置编码，然后进入一个可以循环 N 次的模块。该模块可以分成三块来看，第一块也是Attention层，第二块是cross Attention，不是Self-Attention，第三块是全连接层。也用了跳跃连接和Normalization。

### “masked” Multi-Headed Attention

使用"masked" 机制，防止为了模型看到要预测的数据，防止泄露

### encoder-decoder multi-head attention

### Linear and Softmax to Produce Output Probabilities

最后的输出要通过全连接层将 decoder 的输出扩展到与 vocabulary size 一样的维度上。经过 softmax 后，选择概率最高的一个 word 作为预测结果。



# 参考文献

[产品经理的人工智能学习库《Transformer》](https://easyai.tech/ai-definition/transformer/)

[知乎《10分钟带你深入理解Transformer原理及实现》](https://zhuanlan.zhihu.com/p/80986272)