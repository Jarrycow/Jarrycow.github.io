---
title: DHCP
date: 2021-02-22 10:07:33
abbrlink: DHCP
---

[toc]

## DHCP概念
**作用**：自动分配IP地址
**地址池/作用域**：（IP、子网掩码、网关、DNS、租期）DHCP协议端口是 67/68
**优点**
- 减少工作量
- 避免IP冲突
- 提高地址利用率
## DHCP原理

也称为DHCP租约过程，分为4步
1、发送DHCP Discovery广播包

> 客户机广播请求IP地址（携带着自己的MAC地址一起发出）

2、响应DHCP Offer广播包

> 服务器响应提供的IP地址（但无子网掩码、DNS、网关等参数）

3、客户机发送DHCP Request广播包

> 客户机选择IP（确认用哪个服务器响应的IP）

4、发送DHCP ACK广播包

> 服务器确定租约，并提供网卡详细参数IP、子网掩码、网关、DNS、租期等

**<font color="red">注：如果有多台DHCP服务器，谁先响应就用谁</font>**

## DHCP续约

- 当租期到50%后，客户机会再次发送DHCP Request广播包，进行续约

- 若服务器无响应，则继续使用并在87.5%再次请求

- 若仍然无响应就释放IP地址，重新发送DHCP Discovery广播包来获取IP地址。

**<font color="red">注：当无任何服务器响应时，给自己分配一个169.254.x.x/16 。该地址只能在内网使用，无法访问外网</font>**

## 部署DHCP服务器 (段号：67、68)

注意：配置DHCP服务的前提是客户机和服务机在同一个网段内
1、点击“我的电脑”中的光驱， 安装DHCP服务$\rightarrow$安装组件$\rightarrow$网络服务$\rightarrow$DHCP
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020032921015092.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDAzMjIzMg==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200329210215761.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDAzMjIzMg==,size_16,color_FFFFFF,t_70)
**<font color="red">服务器必须使用静态IP</font>**
2、配置DHCP服务（windows$\rightarrow$管理工具$\rightarrow$DHCP）
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020032921043266.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDAzMjIzMg==,size_16,color_FFFFFF,t_70)
3、右键$\rightarrow$ 新建作用域
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200329210612170.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDAzMjIzMg==,size_16,color_FFFFFF,t_70)
4、配置可用IP及子网掩码

<font color="red">**分配IP一定要预留一部分IP，备用（添加服务器时用）**</font>
5、添加排除IP（不会分配的IP）
6、设置租期
7、添加公司网关（提供外网服务）
8、添加DNS服务器IP（父域以及WINS服务过于古老）（可在后续服务器选项里统一配置，作用域选项优先级 > 服务器选项优先级）
9、检查配置好的DHCP服务再激活

## 命令
**释放当前IP**

```powershell
ipconfig /release  # 释放IP（取消租约，或者改为手动配置IP，也可以释放租约）
```

**发送Discovery广播获取新的IP**

```powershell
ipconfig /renew    # 有IP时，发送Request续约;无IP时发送Discovery重新获取IP
```

## 地址保留

动态保留，来自同一个客户机请求分配固定的IP地址

保留$\rightarrow$右键 新建保留$\rightarrow$填写保留IP和主机MAC地址、类型填两者

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200329223950627.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDAzMjIzMg==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200329224702614.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDAzMjIzMg==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200329224713973.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDAzMjIzMg==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020032922494650.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDAzMjIzMg==,size_16,color_FFFFFF,t_70)
## DHCP备份

右键$\rightarrow$备份$\rightarrow$选择文件夹$\rightarrow$确定

**还原**

右键$\rightarrow$还原$\rightarrow$选择文件夹$\rightarrow$确定

**恢复服务器**

右键$\rightarrow$浏览$\rightarrow$高级$\rightarrow$查找

## DHCP攻击与防御

1）攻击DHCP服务器：

- 攻击：频繁的发送伪装DHCP请求，直到将DHCP地址池中的资源耗尽。
- 防御：在交换机（管理型）的端口上做动态MAC地址绑定。

2）伪装DHCP服务器

- 攻击：hack通过将自己部署为DHCP服务器，为客户机提供非法IP地址
- 防御：在交换机上，除合法的DHCP服务器所在接口外，全部设置为禁止发送DHCP Offer广播包。