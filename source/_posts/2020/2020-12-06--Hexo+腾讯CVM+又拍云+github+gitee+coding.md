---
title: Hexo+腾讯CVM+又拍云+github+gitee+coding
categories:
  - 前端技术的知识章节
  - Hexo
tags:
  - Hexo
  - 腾讯CVM
  - 又拍云
abbrlink: 298c
date: 2020-12-06 20:53:39
updated: 2020-12-06 20:53:39
---


自从考完高项后，博客的一堆笔记就这样放着了（[信息系统项目管理师](/categories/信息系统项目管理师/index.html)），但我想，博客还是想继续长期用用的，于是计划：

- 公司内网的学习笔记的转录
- Vue学习内容
- Linux学习内容
- 历史文章

---

目前博客是部署到 `github+gitee+coding`，双11入了腾讯CVM，同时兼顾学习Centos和Nginx，于是博客也部署到CVM，就成了 `github+gitee+coding+CVM` 的部署。

- hexo部署到 `github+gitee+coding`
- hexo部署到CVM
- hexo的图片使用又拍云的云储存和CDN（别问我为什么不是要腾讯OSS，我也不知道）
- 博客和图片都使用SSL

<!-- more -->

#### 1. _config.yml设置

<img title="_config.yml设置" src="https://cdn.zenwu.site/upload/pic/2020/20201206221003.png" style="width:450px"/>

如何部署到 `github+gitee+coding` 等仓库就先不说了，在一切准备好之后，在博客目录输入以下命令完成部署：

```yaml
hexo c #一般都先清本地再g
hexo g -d #s
```

#### 2. 又拍云

又拍云注册链接：<https://console.upyun.com/register/?invite=BJIwBP9jP>

**为什么使用又拍云？**
又拍云为开发者**提供免费的10G存储空间和15G的流量**，只要在网站加入又拍云logo然后申请即可。通过下面的链接注册，然后打开又拍云联盟网站 <https://www.upyun.com/league> 进行注册申请。

*对比七牛、腾讯云、阿里云等，又拍云最最最大区别是https流量和https都是免费额度内，免费，免费！！！*

![又拍云联盟](https://cdn.zenwu.site/upload/pic/2020/20201206221727.png)

又拍云的控制台管理挺简单和方便的，没多少套路，个人喜欢。
