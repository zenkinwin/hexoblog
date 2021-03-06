---
title: 笔记-信息化与系统集成技术-云计算的服务形式(IaaS/PaaS/SaaS/DaaS)
categories:
  - 项目管理的培训笔记
  - 信息化与系统集成技术
tags:
  - 云计算
  - IaaS
  - PaaS
  - SaaS
  - DaaS
abbrlink: '5167'
date: 2020-09-19 21:41:36
updated: 2020-09-19 21:41:36
---

云计算的资源是动态扩展且虚拟化的，通过互联网提供，终端用户不需要了解云中基础设施的细节，不必具有专业的云技术知识，也无须直接进行控制，只要关注自身真正需要什么样的资源以及如何通过网络来获得相应的服务即可。

按照服务划分，云计算可以分为**IaaS、PaaS、SaaS、DaaS**四个层次。

<!-- more -->

**IaaS（Infrastructure as a Service，基础架构即服务）是基础层**
在这一层，通过虚拟化、动态化将IT基础资源（计算、网络、存储）聚合形成资源池。资源池即计算能力的集合，终端用户（企业）可以通过网络获得自己需要的计算资源，运行自己的业务系统。这种方式使用户不必自己建设这些基础设施，而是通过付费即可使用这些资源。

**PaaS（Platform as a Service，平台即服务）层**
这一层除了提供基础计算能力，还具备了业务的开发运行环境，提供包括应用代码、SDK、操作系统以及API在内的IT组件，供个人开发者和企业将相应功能模块嵌入软件或硬件，以提高开发效率。对于企业或终端用户而言，这一层的服务可以为业务创新提供快速、低成本的环境。

**SaaS（Software as a Service，软件即服务）**
实际上，SaaS在云计算概念出现之前就已经存在，并随着云计算技术的发展得到了更好的发展。SaaS的软件是“拿来即用”的，不需要用户安装，软件升级与维护也无须终端用户参与。同时，它还是按需使用的软件，与传统软件购买后就无法退货相较具有无可比拟的优势。

**数据沉淀、抽象形成了新的服务——DaaS（Data as a Service，数据即服务）**
数据聚合抽象，把数据转换成通用信息，从而为公众提供公共信息服务。例如，对于天气信息，可能A需要根据天气信息来判断出门穿着，B需要根据天气信息判断是否洗车，C需要根据天气信息判断是否准备防洪防涝设施等。不同用户均可利用DaaS满足自己的诉求。

对企业而言，可根据需要使用上述某一种层次的云服务。大型企业一般需要综合上述四个层次的云服务，即**层次化**的云计算服务，一般也称为`I-P-S&D`云计算，各层可独立提供云服务，下一层的架构为上一层云计算提供支撑。例如，某视频网站采用了上述的I-P-S&D云计算架构。

其中，由阿里云大型服务器群、高速网络、存储系统等组成IaaS架构提供基础服务；将阿里云提供的RDS、MQ等服务构建在IaaS层上；这些RDS、MQ提供的服务组成PaaS，把视频的应用逻辑（如视频编解码、视频流接入等）部署上来，提供在线的视频点播、直播等服务。

这样一个大型的系统对互联网用户而言，就是一个大规模视频类SaaS应用。对日常用户访问日志、视频观看爱好等数据做进一步的聚合分析之后，便可形成千人千面、精准推送等DaaS服务。
