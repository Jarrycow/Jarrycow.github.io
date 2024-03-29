---
title: 通过TCP检测DNS延迟
author: Jarrycow
img: /medias/featureimages/DNS_on_TCP_for_RTT.webp
cover: false
top: false
mathjax: true
categories:
  - 网络测量
tags:
  - 计算机网络
  - 论文
  - 网络测量
keywords: 通过TCP检测DNS延迟
abbrlink: DNS_on_TCP_for_RTT
date: 2022-11-03 12:36:44
---

本文验证了可以通过TCP检测DNS的延迟，并通过该方法发现了一系列网络问题

<!--more-->

# 研究背景

DNS延迟被认为是网络运营商一个关键性能指标，例如CDN的存在可以减少用户的服务延迟，但也必须依赖于全球DNS的可达性和负载均衡的情况。DNS的部署经常使用IP广播，一个DNS服务器通常由多个权威服务器<sub>依据上级DNS授权向下分发域名的DNS服务器</sub>提供，而BGP选择哪些用户去哪些站点。一般DNS用户会选择延迟最低的权威服务器。

# 验证DNS/TCP检测RTT的可行性

DNS首选UCP协议作为传输层协议，但也一直需要TCP处理大型回复。首先假设：

1. TCP在空间和时间上的使用有足够的覆盖;
2. TCP和UDP发送查询的RTT相同

## 足够覆盖

首先，文章检测了两个权威服务器的一周流量，发现TCP流量占比不到7%。但是这些7%的流量可以代表21%的DNS解析器以及44%的自治系统。虽然比例仍然不到一半。不过通常情况下，一个TCP可以代表其所在AS（自治系统），对于所有递归解析器在同一个地点的自治系统，到一个的延迟和到其他的是相同的。最终发现，较少的TCP可以覆盖流量的95%~98%.

### 诱导性覆盖

不过，这样TCP的覆盖同样是不完整的。因此可以通过主动的流量管理来诱导TCP查询进而获得完整的覆盖。DNS报文中的标志位中包含TC位：即通过TCP重试的截断回复。DNS接收者可以通过限制速率强制使用这种机制。

### 时间上的覆盖

验证了每个自治系统的流量具有一定时间精度，可以支持一天中接近实时监测。

## DNS/UDP vs DNS/TCP

测量DNS/UDP和DNS/TCP查询响应时间，DNS/UDP中是直接提出请求并得到响应；而在DNS/TCP中，要先建立TCP连接，因此需要2个RTT。

# 发现问题

不管是网络问题的检测还是解决，都是劳动密集型的——检测需要确定具体问题和潜在根源；解决需要新站点部署或路由改变。

因此，通过两种策略分析问题：

- 分析每个广播站点：负责寻找广播服务中显示朝向站点的高延迟的位置
- 分析每个客户AS：虽然客户AS性能不是运营商的问题，但在某些情况下仍然可以解决

考虑因素：

- 中位数延迟：网站整体延迟代表
- 四分位数范围：75%的延迟和25%的延迟之间的差异，反映了AS可能延迟的分布
- 查询量：改进将影响更多用户的位置

## 边陲之地

某地区在当地没有任意广播服务器，并且与世界其他地方的连接有限

**发现：**东京、新加坡、巴黎节点大四分位数延迟，所有75%延迟超过100ms

**解决方案：**在当地建立广播服务器

## 倾向于将客户带到另一个大陆

AS选择路由时，将用户带到另一个大陆的DNS

BGP路由策略并不是RTT优先

**发现：**巴西圣保罗中位延迟增高

**解决方案：**联系运营商，部署BGP社区限制通往南美的路由

## 跨国巨头广播极化

公司使用自己的骨干网络将数据传输到一个数据中心，而非使用更为完善的广播网络

