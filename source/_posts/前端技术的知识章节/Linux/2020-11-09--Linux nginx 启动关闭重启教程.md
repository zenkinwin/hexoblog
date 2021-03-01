---
title: Linux nginx 启动关闭重启教程
categories:
  - 前端技术的知识章节
  - Linux
tags:
  - linux
abbrlink: '7144'
date: 2020-11-09 23:57:30
updated: 2020-11-09 23:57:30
---

centos环境，nginx目录`/usr/local/nginx/sbin/`

``` bash
#进入sbin目录文件
`cd /usr/local/nginx/sbin/`

#启动
./nginx

#关闭
./nginx -s stop

#更改配置后重启  
./nginx -s reload

#查看进程
ps -ef | grep nginx

#杀死进程
pkill nginx
```
