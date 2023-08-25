---
title: LSTM
author: Jarrycow
img: /medias/featureimages/xxx.png
cover: false
top: false
mathjax: true
categories:
  - AI
tags:
  - AI
  - LSTM
keywords: LSTM
abbrlink: LSTM
date: 2023-08-04 09:42:10
---



<!--more-->

# LSTM 结构

## Memory

![Memory](https://4143056590-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-LpO5sn2FY1C9esHFJmo%2F-M1DtQY-ei6hXfaisjwq%2F-M1DtSffa2TchE2rFm-c%2FLSTM.jpg?generation=1582945406632519&alt=media)

LSTM 有 3 个 Gate：

- **输入门 Input Gate：**当外界的输出值想要写入到LSTM里面的时候，必须先通过一个闸门 Input Gate，并且只有当 Input Gate 打开的时候，才能把值写入到 memory 中去；当 Input Gate 关起来的时候，就无法写入值<sub>至于什么时候把Input Gate打开还是关闭，这是网络自己学到的</sub>

- **输出门 Output Gate：**输出的地方有一个 Output Gate，可以决定外界是否可以把值从 memory 里面读出。只有当 Output Gate 打开的时候，外界才能读取。<sub>Output Gate什么时候打开还是关闭，也是Output Gate自己学到的</sub>

- **遗忘门 Forget Gate：**Forget Gate 决定什么时候把过去记得的东西（存在memory中的值）忘掉，什么时候记住过去学的东西。<sub>Output Gate什么时候把存在memory中的值清除掉或者保留下来，也是Forget Gate自己学到的</sub>

## LSTM 流程

![LSTM 单元格](https://4143056590-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-LpO5sn2FY1C9esHFJmo%2F-M1DtQY-ei6hXfaisjwq%2F-M1DtSfiDaBbHeBymgzw%2FLSTM-2.jpg?generation=1582945410569345&alt=media)

**输入：**

- 输入 $z$ 
- 通过激活函数得到 $g(z)$
- 输入门控制信号 $z_i$经过激活函数得到 $f\left(z_i\right)$
- 两者相乘得到 $g(z)f\left(z_i\right)$

**Memory 更新：**

- Memory 中原有值 $c$

- 遗忘门控制信号 $z_f$ 经过激活函数得到 $f\left(z_f\right)$

  $f\left(z_f\right)$ 为 1 代表记忆；$f\left(z_f\right)$ 为 0 代表遗忘

- 加上输入值得到新的 Memory 中要存储的值

  $c'=g(z)f\left(z_i\right)+cf\left(z_f\right)$

**输出：**

- Memory 中值 $c'$ 经过激活函数得 $h\left(c'\right)$
- 输出门的控制信号 $z_0$ 经过激活函数得到的 $f\left(z_0\right)$

- 两者相乘得到输出值 $a=h\left(c'\right)f\left(z_0\right)$

## LSTM 神经网络

上述为 LSTM 单个神经元结构。LSTM 神经网络就是把一般神经网络中神经元替换为 LSTM 神经元。

<img src="https://4143056590-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-LpO5sn2FY1C9esHFJmo%2F-M1DtQY-ei6hXfaisjwq%2F-M1DtSg2x-U_EV63Ay3u%2Fneurons.jpg?generation=1582945397507431&alt=media" alt="一般神经网络" style="zoom:33%;" /><img src="https://4143056590-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-LpO5sn2FY1C9esHFJmo%2F-M1DtQY-ei6hXfaisjwq%2F-M1DtSg4F7pcInUGFh0M%2Fneurons-1.jpg?generation=1582945405658712&alt=media" alt="LSTM 神经网络" style="zoom:33%;" />

可以看到，LSTM 参数为一般的 4 倍。**三个门的输入值，都是序列x乘上一个权值矩阵得到的结果**。

# 参考文献

[芦苇的机器学习笔记](https://luweikxy.gitbook.io/machine-learning-notes/long-short-term-memory-networks)