---
title: 基于RPL的物联网中缓解和隔离内部Rank攻击者的轻量高能效算法
author: Jarrycow
cover: false
top: false
mathjax: true
categories:
  - 无线网络
tags:
  - 论文
  - 无线网络
  - 物联网安全
keywords: 基于RPL的物联网中缓解和隔离内部Rank攻击者的轻量高能效算法
abbrlink: A_Lightweight_Energy-Efficient_Algorithm_for_mitigation_and_isolation_of_Internal_Rank_Attackers_in_RPL_based_Internet_of_Things
date: 2023-02-02 23:27:25 
summary: 本文设计轻量高效算法，在基于 RPL 物联网中检测隔离 Rank 攻击 
---
这是阅读论文[《A Lightweight Energy-Efficient Algorithm for mitigation and isolation of Internal Rank Attackers in RPL based Internet of Things》](https://www.sciencedirect.com/science/article/abs/pii/S138912862200425X)读书笔记

# 研究背景

物联网终端互联成为必然趋势。低功耗有损网络 LLN 是一类内部链接质量和路由器受限网络。但该网络下路由器处理功能、内存、功耗限制较大，也具有高丢包率、低数据传输率和不稳定特性，因此传统通信技术和协议不适用。RPL 是为 LLN 涉及的距离矢量路由协议，但目前的安全措施消耗较多资源。

**LLN：**低功耗有损网络，是一类内部链接和路由器都受限的网络。该网络下的路由器的处理器功能、内存、功耗都可能受到较大的限制，而里面的网络连接也具有高丢包率、低数据传输率及不稳定的特性。

**RPL：**为低功耗有损网络设计的一种距离矢量路由协议<sub>原有的 OSPF、RIP等都不再适用于LLN</sub>

# 相关技术

### RPL 协议

**网络结构：**以目的地为导向有向无环图，树状拓扑

![采用RPL协议的网络示例](https://raw.githubusercontent.com/Jarrycow/picHost/main/article/%E9%87%87%E7%94%A8RPL%E5%8D%8F%E8%AE%AE%E7%9A%84%E7%BD%91%E7%BB%9C%E7%A4%BA%E4%BE%8B.png)

RPL 根据应用的具体目标优化 DODAG 拓扑结构，一组 DODAG 构成一个 RPL 实例。RPL 使用信息对象消息（DIO）、目的地通告消息（DAO）和请求消息（DIS）等控制包。

**构建过程：**

1. 从根开始，根节点通过传送 DIO 消息广播 DODAG 信息<sub>包含实例 ID、目标函数等路由度量标准</sub>
2. 收到 DIO 节点计算其在 DODAG 中的 rank 值
3. 以 DAO 消息进行回复，告知其参与
4. 经过验证，在 DODAG 根的路由表中为被确认的节点创建一个条目

### Rank 攻击

根节点通过广播实现和其他节点交互信息，并在交互过程中通过目标函数将多个度量约束转成 Rank 值，然后根据 Rank 值确定路由选择。RPL 中 rank 值从根节点逐渐向下增加。若 rank 被篡改，攻击者可通过子节点吸引实现父节点更新或改变拓扑。

![Rank 攻击](https://raw.githubusercontent.com/Jarrycow/picHost/main/article/image-20230203005543671.png)

### Rank 减少攻击

在 DIO 消息中宣称 rank 下降。由于 rank 降低，节点更接近根，大多数节点选择攻击者作为首选父节点达到根。

![Rank 减少攻击](https://raw.githubusercontent.com/Jarrycow/picHost/main/article/image-20230203005753389.png)

# 算法

本文算法通过附加本节点 rank、父节点 rank 和哈希值等字段改变 DAO 消息，以检测 rank 攻击者。收到 DAO 消息后，根节点将这些字段值与信息表中的值比较，若存在不匹配则产生警报。

1. **修改 DAO 消息：**DAO 消息通过增加额外字段，如 rank、父节点 rank 和哈希值，增加 96 比特

2. **哈希值生成：**作为轻量级协议使用整数哈希函数。函数接受 64 位<sub>Rank + Parent rank + DAO</sub>作为输入，并返回 64 位输出。因为发送者 IP 对每个节点唯一，因此哈希函数输出也唯一

   ```C
   x < ---------^ x >> 32
   x < --------x * 0xd6e8feb86659fd93U
   x < ---------^ x >> 32
   x < x * 0xd6e8feb86659fd93U
   x < -----------^ x >> 32
   ```

3. **检查是否不一致：**收到 DAO 消息，根部将收到的数值与信息表中的数值进行比较，若产生变化通过 ICMP 错误信息通知

4. **产生警报：**警报包含在 DIO 消息本身以减少开销













