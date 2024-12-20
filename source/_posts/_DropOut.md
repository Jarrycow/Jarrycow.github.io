---
title: DropOut
author: Jarrycow
img: /medias/featureimages/xxx.png
cover: false
top: false
mathjax: true
categories:
  - AI
tags:
  - AI
  - CNN
keywords: DropOut
abbrlink: DropOut
date: 2023-07-17 20:48:53
---



<!--more-->

# 为什么要有 DropOut

机器学习模型如果**参数太多，样本太少**，则训练结果很容易过拟合。为了解决过拟合问题，一般会采用模型集成的方法，即训练多个模型进行组合，但这样训练耗时就是大问题。2012年，Hinton 提出 DropOut——通过阻止特征检测器的共同作用来提高神经网络的性能。

DropOut 简单来说，就是每个训练批次中让某个神经元的激活值以一定的概率p停止工作，这样可以使模型泛化性更强，因为它不会太依赖某些局部的特征.

![](https://raw.githubusercontent.com/Jarrycow/picHost/main/javaGuli/v2-5530bdc5d49f9e261975521f8afd35e9_r.jpg)

## DropOut 为什么可以解决过拟合

- **取平均**

  对于标准模型，用相同训练数据训练 5 个不同的神经网络，一般会得到 5 个不同结果，此时采用“5 个结果取均值”。因为不同的网络可能产生不同的过拟合，取平均则有可能让一些“相反的”拟合互相抵消。DropOut掉不同的隐藏神经元就类似在训练不同的网络。

- **减少神经元之间复杂共适应关系**

  DropOut 导致两个神经元不一定每次都在一个网络中出现，因此权值的更新不再依赖于有固定关系的隐含节点的共同作用，阻止了某些特征仅仅在其它特定特征下才有效果的情况 。即模型不再专注于某些特定的线索片段。





# DropOut 工作流程

一般的神经网络，把输入通过网络向前传播，然后把误差反向传播以决定如何更新参数让网络进行学习。使用 Dropout 之后，过程变成如下：

1. 随机（临时）删除网络中一部分神经元，输入层和输出层的神经元不变

   ![临时删除的神经元](https://raw.githubusercontent.com/Jarrycow/picHost/main/javaGuli/v2-24f1ffc4ef118948501eb713685c068a_1440w.webp)

2. 输入 $x$ 通过修改后的网络向前传播，然后将得到损失结果反向传播，更新参数 $w$，$b$

3. 重复该过程

   - 恢复被删除的神经元——被删除的神经元保持原样，未被删除的神经元更新

# Dropout 的使用

上面说的对，但是怎么用呢

![](https://raw.githubusercontent.com/Jarrycow/picHost/main/javaGuli/v2-543a000fcfe9778cd64c898c01743aae_r.jpg)

**训练阶段**

对于普通神经网络


$$
z_{i}^{\left( l+1 \right)}=w_{i}^{\left( l+1 \right)}y^l+b_{i}^{\left( l+1 \right)}
\\
y_i^\left(l+1\right)=f\left(z_i^\left(l+1\right)\right)
$$
采用 DropOut 之后
$$
\tilde{y}^{\left( l \right)}=r^{\left( l \right)}\times y^{\left( l \right)}
\\
z_{i}^{\left( l+1 \right)}=w^{\left(l+1\right)}_i\tilde {y} ^l+b_i^{\left(l+1\right)}
\\
y_i^\left(l+1\right)=f\left(z_i^\left(l+1\right)\right)
$$
$r^\left(l\right)$ 是生成概率 $p$ 的向量。代码层面即让某个神经元以一定概率停止工作，即其激活函数以概率变 0。

<font color = "blue">**注意：** 经过上面屏蔽掉某些神经元，使其激活值为 0 以后，还需要对向量进行缩放，即 $\times \dfrac1{1-p}$；否则测试时需要对权重进行缩放</font>

**测试阶段**

预测模型的时候，每一个神经单元的权重参数要乘以概率 $p$。因此测试阶段的 DropOut：

$w_{test}^{\left(l\right)}=pW^{\left(l\right)}$

## Keras 中的源码

```python
# coding:utf-8
import numpy as np


def dropout(x, level):  # dropout函数的实现
    if level < 0. or level >= 1: #level是概率值，必须在0~1之间
        raise ValueError('Dropout level must be in interval [0, 1[.')
    retain_prob = 1. - level

    # 我们通过binomial函数，生成与x一样的维数向量。binomial函数就像抛硬币一样，我们可以把每个神经元当做抛硬币一样
    # 硬币 正面的概率为p，n表示每个神经元试验的次数
    # 因为我们每个神经元只需要抛一次就可以了所以n=1，size参数是我们有多少个硬币。
    random_tensor = np.random.binomial(n=1, p=retain_prob, size=x.shape) #即将生成一个0、1分布的向量，0表示这个神经元被屏蔽，不工作了，也就是dropout了
    print(random_tensor)

    x *= random_tensor
    print(x)
    x /= retain_prob

    return x
```

# 参考文献

[深度学习中Dropout原理解析](https://zhuanlan.zhihu.com/p/38200980)