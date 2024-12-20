---
title: maven
author: Jarrycow
img: /medias/featureimages/xxx.png
cover: false
top: false
mathjax: true
categories:
  - 后端
tags:
  - 后端
  - java
  - 环境配置
keywords: maven
abbrlink: maven
date: 2023-07-24 09:29:38
---

Maven是apache旗下的一个开源项目，是一款用于管理和构建java项目的工具。

<!--more-->

# Maven 

## Maven 作用

**依赖管理：**方便快捷的管理项目依赖的资源(jar包)，避免版本冲突问题<sub>不用手动导入 jar 包</sub>

**统一项目结构：**提供标准、统一的项目结构

![Maven 项目结构](https://raw.githubusercontent.com/Jarrycow/picHost/main/JavaWeb/Maven_项目结构.png)

**项目构建：**标准跨平台（Linux、Windows、MacOS）的自动化项目构建方式

## Maven 模型

![Maven 模型](https://raw.githubusercontent.com/Jarrycow/picHost/main/JavaWeb/Maven 架构.png)

通过插件完成项目标准化构建

POM 是通过 pom.xml 的信息描述整个工程

通过依赖管理模型，从仓库中寻找模型需要的依赖

# IDEA 集成 Maven

## 配置 Maven 环境

**配置Maven环境(当前工程)**

- 选择 IDEA中 File --> Settings --> Build,Execution,Deployment --> Build Tools --> Maven
- 设置 IDEA 使用本地安装的 Maven，并修改配置文件及本地仓库路径

![配置当前工程 Maven 环境](https://raw.githubusercontent.com/Jarrycow/picHost/main/JavaWeb/配置当前工程 Maven 环境.png)

**配置Maven环境(全局)**

![配置全局 Maven 环境](https://raw.githubusercontent.com/Jarrycow/picHost/main/JavaWeb/配置全局 Maven 环境.png)

## 创建 Maven 项目

1. 创建模块，选择Maven，点击Next
2. 填写模块名称，坐标信息，点击finish，创建完成
3. 编写 HelloWorld，并运行

![IDEA 创建 Maevn](https://raw.githubusercontent.com/Jarrycow/picHost/main/JavaWeb/IDEA 创建 Maevn.png)

## 导入 Maven 项目

打开IDEA，选择右侧Maven面板，点击 + 号，选中对应项目的pom.xml文件，双击即可。

![IDEA 导入 Maven](https://raw.githubusercontent.com/Jarrycow/picHost/main/JavaWeb/IDEA 导入 Maven.png)

# 依赖管理

依赖：指当前项目运行所需要的 jar 包，一个项目中可以引入多个依赖。

## 依赖配置

1.在 pom.xml 中编写 `<dependencies>` 标签

2.在 `<dependencies>` 标签中 使用 `<dependency>` 引入坐标

3.定义坐标的 groupId，artifactId，version

4.点击刷新按钮，引入最新加入的坐标

## 依赖传递

依赖具有传递性

- 直接依赖：在当前项目中通过依赖配置建立的依赖关系

- 间接依赖：被依赖的资源如果依赖其他资源，当前项目间接依赖其他资源

![Maven 依赖传递](https://raw.githubusercontent.com/Jarrycow/picHost/main/JavaWeb/Maven 依赖.png)

**依赖范围**

可以通过 `<scope>test</scope>` 控制依赖范围

| **scope****值** | **主程序** | **测试程序** | **打包（运行）** | **范例**    |
| --------------- | ---------- | ------------ | ---------------- | ----------- |
| compile（默认） | Y          | Y            | Y                | log4j       |
| test            | -          | Y            | -                | junit       |
| provided        | Y          | Y            | -                | servlet-api |
| runtime         | -          | Y            | Y                | jdbc驱动    |

## 生命周期

Maven 的生命周期就是为了对所有的maven项目构建过程进行抽象和统一。

Maven 中有3套相互独立的生命周期：

- clean：清理工作——移除上一次构建生成的文件

- default：核心工作，如：编译、测试、打包、安装、部署等。
  - compile：编译项目源代码
  - test：使用合适的单元测试框架运行测试(junit)
  - package：将编译后的文件打包，如：jar、war等
  - install：安装项目到本地仓库

- site：生成报告、发布站点等。

在同一套生命周期中，当运行后面的阶段时，前面的阶段都会运行。