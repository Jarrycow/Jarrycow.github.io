---
title: DNS服务器
date: 2021-03-01 20:26:18
abbrlink: DNS
---

[toc]

## 域名组成

如下图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200407230358734.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDAzMjIzMg==,size_16,color_FFFFFF,t_70)
各类顶级域

- 国家顶级域 `cn jp hk uk`
- 商业顶级域
  - com 商业机构
  - gov 政府机构
  - mil 军事机构
  - edu 教育机构
  - org 民间组织架构
  - net 互联网

> QDN = 主机名.DNS后缀
FQDN（完整的域名）= 主机名.域名

**监听端口**

>TCP  53
UDP 53

### DNS解析种类

#### 按照查询分类

1. 递归查询：客户机与本地DNS服务器之间
2. 迭代查询：本地DNS服务器与根等其他DNS服务器的解析过程

全世界目前一共13个根域名服务器

### 解析步骤

1. 查询本地主机缓存以及host

   若有，解析

   若无，像本地DNS服务器发送请求

2. 本地DNS服务器查询缓存，

   若有，发送给客户机

   若无，像根域发送请求

3. 根域发送二级域名服务器地址，本地DNS向二级域名服务器发送请求

4. 二级域名发送三级域名服务器地址，本地DNS向三级域名服务器发送请求

5. 三级域名向本地DNS发送解析，本地DNS收到并缓存

6. 本地DNS向客户机发送解析，客户机收到并缓存

## DNS服务器搭建过程

**注意：配置服务器前一定要配置静态IP**

1. 插入光驱双击打开，安装可选的Windows组件- 网络服务 – 域名系统DNS

2. 开始 – 管理工具 打开DNS服务

   此时已经可以作为DNS服务器（缓存服务器）

3. 正向查询区域 – 右键新建区域
   ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200408221536893.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDAzMjIzMg==,size_16,color_FFFFFF,t_70)

   - 主要区域就是主服务器；
   - 辅助区域是备份区域，如果公司已经有一台DNS服务器了，想要做一台备份服务器，就选辅助区域；
   - 存根区域就是根域，一般由国家级的来创建；

4. 区域名称写待解析域名调音

5. ```域名.dns```这个文件是区域解析文件，以后```任何主机.域名```都会写入这个文件；
   
6. 不允许动态更新

     ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200408222258358.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDAzMjIzMg==,size_16,color_FFFFFF,t_70)
     直接下一步

7. 名称：baidu.com
     类型：主要区域（本服务器是baidu.com的主DNS服务器）
     查找类型：正向（正向解析）
     文件名：baidu.com.dns
     ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200408223217828.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDAzMjIzMg==,size_16,color_FFFFFF,t_70)

8. 右击-新建主机
     ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200408223731524.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDAzMjIzMg==,size_16,color_FFFFFF,t_70)
     ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200408223855825.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDAzMjIzMg==,size_16,color_FFFFFF,t_70)

```powershell
ipconfig /flushdns    # 刷新本地DNS缓存
ipconfig /displaydns  # 显示DNS缓存
```

### 反向解析

给服务器命名为dns.baidu.com
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200408232047898.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDAzMjIzMg==,size_16,color_FFFFFF,t_70)
反向区域配置
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200408232143182.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDAzMjIzMg==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200408232201401.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDAzMjIzMg==,size_16,color_FFFFFF,t_70)
新建指针 – 配置反向解析（通过访问IP可以解析到服务器名）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200408232224250.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDAzMjIzMg==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200408232413337.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDAzMjIzMg==,size_16,color_FFFFFF,t_70)
### 同步备份
**备份**：在域名右键-属性-区域复制-只允许到下列服务器
**同步**：右键-新建区域-辅助区域-IP地址填写主DNS服务器

### DNS服务器分类

- 主要名称服务器
- 辅助名称服务器
- 根名称服务器
- 高速缓存名称服务器


### 客户机域名请求解析顺序

1、dns缓存
2、本地hosts文件
3、本地dns服务器

### 服务器对域名请求的处理顺序

1、dns高速缓存
2、本地域名解析文件
3、转发器
4、根rt
