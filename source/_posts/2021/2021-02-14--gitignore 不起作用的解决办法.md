---
title: gitignore 不起作用的解决办法
categories:
  - 前端
tags:
  - git
  - gitignore
abbrlink: 81d1
date: 2021-02-14 13:22:21
updated: 2021-02-14 13:22:21
---

创建了.gitignore文件，在多个仓库中初始化和切换，发现过滤规则不生效，真是醉了。翻查百度，解决核心在于*清除本地缓存*，方法如下：

```
git rm -r --cached .
git add .
git commit -m 'update .gitignore'   //windows 使用的命令是  
git commit -m "update .gitignore"  //需要使用双引号
```
