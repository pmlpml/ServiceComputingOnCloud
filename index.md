---
layout: default
title: 课程简介
---

## 服务计算 - 云应用开发方法、技术与架构
{:.no_toc}

* 目录
{:toc}

## 1、概述

云计算技术的飞速发展，越来越多企业借助“云”提供创新服务或改进企业生产力，云服务创新企业不断涌现，形成万亿级别的企业云服务应用开发市场。云提供商、云应用企业大量渴求了解云计算知识，能够在云上快速开发“**高性能、高可靠、高可用、可伸缩**”的企业级原生应用的人才，以满足日益增长的云服务管理、云应用开发与运维的需要。

**面向服务**是云应用软件的基本特征。云计算平台管理服务、零售在线服务、第三方认证服务、移动应用服务端、在线编程服务、到超算云服务 、IoT云服务、AI云服务等等，它们通常以 web service 的形式提供。以 [Docker](https://www.docker.com/) 为代表的容器技术出现，面向服务的方法正在逐步改变软件的开发过程与开发方法。 pivotal 提出“[云原生应用/cloud-native Apps](https://pivotal.io/cn/cloud-native)” 的概念，逐步将“容器”、“微服务”、“持续集成与交付”、“DevOps”等概念串联起来。 Adam Wiggins 提出 [12 factor App](https://12factor.net/) 企业云应用宣言【[中文](http://www.infoq.com/cn/news/2012/09/12-factor-app)】，并创建了 [Heroku](https://www.heroku.com/) 云应用开发管理平台。Martin Fowler 在 2014 给出了“[微服务](http://martinfowler.com/articles/microservices.html)”架构（microservice archtecture）定义【[中文](http://mp.weixin.qq.com/s?__biz=MjM5MjEwNTEzOQ==&mid=401500724&idx=1&sn=4e42fa2ffcd5732ae044fe6a387a1cc3#rd)】，给出云应用构建的架构方法与准则。2015年，Google 发起了[云原生计算基金（CNCF）](https://www.cncf.io/)，围绕“云原生”服务云计算，服务于云服务计算[社区生态与技术框架](https://github.com/cncf/landscape)【注1】，推动相关开源项目的发展与演化，支持编排容器化微服务架构应用。

![](https://pmlpml.github.io/unity3d-learning/images/drf/exclamation.png) 本课程关注面向服务软件的开发、部署与运维技术，紧随 CNCF 技术框架，培养云原生应用生态需要的开发技能。云基础设施与管理软件的核心多数采用 go 语言，课程包含 go 语言初中级知识、微服务应用架构、服务开发流水线、服务运行平台等内容，帮助学员尽快融入云开发生态环境。

【注1】该领域技术与工具变化太快，CNCF 技术栈和内容是动态变化的。

## 2、课程的定位与目标

### 2.1 课程的定位

![](images/position.png)

### 2.2 课程目标

* 掌握 go 语言知识，建立面向对象的编程思想，具有初步程序框架开发能力
* 掌握 Restful 服务开发的知识，了解前后端分离开发技术，能够按CI/CD流程交付一个简单服务项目
* 了解微服务架构，了解容器云如 Docker Swarm、Kebenetes的基本原理，使用容器云部署项目

## 3、课程组织与内容

### 3.1 课程组织

Part I: Golang 基础

> 本部分学习 Go 语言编程相关的基础知识，设计模式，以及命令行程序编程技巧。由于系统工程师的工作环境需要，课程的设定都是Linux操作系统，尽管在Windows下一切都能完美工作。包括内容：Go语言基本语法、函数、结构体与接口，并发、异常处理，面向对象程序编程、工作空间与包，CLI 程序开发等。

Part II: Web 服务编程与框架

> Web 服务端的工作原理与程序结构。web 静态文件服务、模板输出、输入路由、表单处理、过滤器框架与设计等 Request 和 Response 相关的处理。 database/sql 包的工作原理、mysql 服务器访问、 基于 entity - dao - service 三层结构层次模型、 sql template 的设计、 xorm 简介。 CI/CD？客户端的编程模型？

Part III: 微服务架构与服务管理

> 微服务架构；应用在容器云（Docker Swarm 或 kubernetes）中的部署；云服务的运维与优化

### 3.2 课程内容

| 周/次 | 课程内容 |  课后阅读 与 作业 |
|:--:|  ---- | ---- |
| 1 | 云计算与服务计算 | 阅读：《云计算 概念、技术与架构》 <br> 实验：[安装配置你的私有云](ex-install-cloud) |
| 2 | 服务计算与Go语言 | 阅读：[服务面向的架构](http://ptgmedia.pearsoncmg.com/images/9780133858587/samplepages/9780133858587.pdf) <br> 了解：访问 AWS 产品页面，了解你感兴趣的一些产品，它们属于 IaaS，PaaS or SaaS? |
| 3 | Go 语言基础 - 语法、控制、函数、包、结构、集合、工作空间组织 <br> [Go 在线之旅](https://tour.go-zh.org) | 阅读：[《学习GO语言》](https://mikespook.com/learning-go/)中文版 <br> 实验：[安装 Golang 开发环境](ex-install-go) |
| 4-5 | Go 语言基础 - 方法、接口、go程、Posix Cli| 作业：[开发简单 CLI 程序](ex-cli-basic) <br> 了解：利用 [sourcegraph](https://sourcegraph.com/github.com/golang/go/-/blob/src/time/tick.go) 阅读源码 Tick 函数实现 <br> 验证：[使用接口与接口断言会产生性能损失吗？](https://stackoverflow.com/questions/28024884/does-a-type-assertion-type-switch-have-bad-performance-is-slow-in-go)| 
| 6| 面向对象编程 - [接口抽象与多态，Corba 实现原理](oo-thinking)|阅读：[Interfaces / OOP](https://github.com/golang/go/wiki/Articles#interfaces--oop)  |
| 7 | 面向对象编程 - [IO包流抽象及应用，包设计](oo-thinking-abstract) |  作业：[CLI 命令行实用程序开发实战 - Agenda](ex-cli-agenda) |
| 8 | web 技术 - [HTTP 协议 与 golang web 应用服务](https://blog.csdn.net/pmlpml/article/details/78404838) | 阅读：[《Golang web 应用开发》](https://github.com/astaxie/build-web-application-with-golang) <br> 了解：context 包，[Go语言并发模型：使用 context](https://segmentfault.com/a/1190000006744213)，注：现在已是正式库 [context](https://godoc.org/context#pkg-examples) |
| 9 | web 技术 - [处理 Request 与 Response](https://blog.csdn.net/pmlpml/article/details/78539261) | 验证：[Go HTTP Router Benchmark](https://github.com/julienschmidt/go-http-routing-benchmark) <br> 作业： 选择一个任务作为web开发练习：<br> 写一个简单 web 程序 或 原代码阅读与分析 或 写一个中间件 <br> [作业提示1](ex-cloudgo-start) [作业提示2](ex-cloudgo-inout) |
| 10 | web 服务 - [RESTful 基础](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)与[实践](https://www.infoq.cn/article/designing-restful-http-apps-roth) | 阅读：[RESTful 架构详解](http://www.runoob.com/w3cnote/restful-architecture.html) |
| 11 | web 服务 - go RESTful 服务端与客户端开发 |了解：REST API 设计 [Github API v3 overview](https://developer.github.com/v3/) | 
| 12 | web 服务 - 基于 RPC、REST、GraphQL 的服务 | 作业：服务构建与前后端分离的开发，[具体要求](ex-services) |
| 13 | 云应用 - 微服务基本概念与云原生应用实践 | 实验：**选做**[部署一个云原生本地实验环境](https://jimmysong.io/kubernetes-handbook/cloud-native/cloud-native-local-quick-start.html)。提交要求：博客 <br> 阅读：[8 Steps to Becoming Awesome with Kubernetes](https://github.com/rootsongjc/cloud-native-slides-share/blob/master/kubernetes/8-Steps-to-Becoming-Awesome-with-Kubernetes-readhat-burrsutter.pdf)|
| 14 | 云应用 - 基础设施，容器作为服务（CaaS）| |
| 15 | 云应用 - 容器化实践、云原生应用开发与过程 | 作业：[应用容器化](ex-containerization) |
| 16-17 | 云应用 - 架构与模式 | 实践：[容器云实验环境](ex-install-k8s-rancher) <br> 作业：【可选】cncf 技术栈自由实践博客 [cncf landscape](https://github.com/cncf/landscape) |
| 18 | 云应用 - 服务管理与治理综述| |

 
### 3.3 大作业要求

无！

## 4、课程其他信息

### 4.1 必备知识

前置或并行学习知识：

* 操作系统
* 计算机网络
* c 或 python 语言
* 最好了解 Vue，React，AngularJS，或其他 JaveScript 前端开发框架

建议后续学习课程：

* 云计算

### 4.2 教材与参考书

参考书：

* Thomas Erl. _Service-Oriented Architecture: Analysis & Design for Services and Microservices_, 2nd Edition. Prentice Hall, 2016
* Kevin Hoffman，《Cloud Native Go：构建基于Go和React的云原生Web应用与微服务》，电子工业出版社，2017
* Kubernetes中文指南/云原生应用架构实践手册，https://jimmysong.io/kubernetes-handbook/

### 4.3 课程支持

* 教师邮箱 panml@mail.sysu.edu.cn
* 支持 QQ 群： 851549217


## 5、作业提交与考核方法

### 5.1 作业提交

Github 和 博客（务必设置分类，以便于检查和批改）

**什么内容可以写博客？**

1. 帮助小白类。系统操作指南、个人经验分享、技术科普。 例如：
    - [在Github上创建Organization](https://chun-ge.github.io/How-to-establish-an-organization-on-Github/)
    - [Vue极简快速入门手册](https://www.jianshu.com/p/b5cb9b30ffd8)
    - [关于golang使用mysql以及docker的一些坑](https://blog.csdn.net/weimumu0515/article/details/78733919)
2. 框架设计类。介绍自己的设计与实现（必须有Github实现链接）。例如：
    - [ORM-Engine](https://github.com/smallGum/service-computing/tree/master/orm-engine)
3. 技术研究类及其他。例如：
    - [云端服务器部署 denyhosts 抵御 ssh brute force 攻击](https://www.zybuluo.com/longj/note/1197554)

### 5.2 考核内容

|项目 | 分数 | 备注 |
| --- |:---:| -- |
| 平时作业 | 80 | 见作业栏 |
| 博客分享 | 20 | 按博客贡献度。贡献度标准质量+访问量 |

&nbsp;

[Markdown 语法演示页](demo).