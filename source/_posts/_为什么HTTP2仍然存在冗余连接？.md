---
title: 为什么HTTP2仍然存在冗余连接？
author: Jarrycow
img: /medias/featureimages/xxx.png
cover: false
top: false
mathjax: true
categories:
  - 网络测量
tags:
  - 网络测量
  - 计算机网络
  - 论文
keywords: 为什么HTTP2仍然存在冗余连接？
abbrlink: redundantConnectHTTP2
date: 2023-01-27 01:14:14
---



<!--more-->

本文为阅读论文[Sharding and HTTP/2 Connection Reuse Revisited: Why Are There Still Redundant Connections?](https://arxiv.org/ftp/arxiv/papers/2110/2110.14239.pdf)读书笔记。

HTTP/1.1 需要多个并发链接实现并行传输，但其后继者HTTP/2 和 HTTP/3 可在一个单一链接上复用内容，其在理论上极大节省了开销。但研究表明，浏览器仍然会于向同一个域打开多个 HTTP/2 链接。

# HTTP 多种链接方式

现代网页由众多资源组成，TTTP/1.1 使用单个 TCP 链接发送单个资源。当一个资源延迟会延迟后续资源访问。

## HTTP/1 并行链接

当浏览器打开多个平行链接，运营商采用域名分片将资源分散到更多域名，造成额外链接。但每个链接都有其维护成本，对于服务器开销很大，且延迟很高。

## HTTP/2 和 HTTP/3 的单个链接

为降低延迟，HTTP/2 和 HTTP/3 们使用在单个连接上复用的多个流，以允许资源的平行传输。为此，HTTP/2 在 TCP 上实现流语义；HTTP/3 使用 QUIC 及其集成流。