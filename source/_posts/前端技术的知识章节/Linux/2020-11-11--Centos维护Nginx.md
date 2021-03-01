---
title: Centos维护Nginx
categories:
  - 前端技术的知识章节
  - Linux
tags:
  - linux
  - nginx
abbrlink: 47c0
date: 2020-11-11 23:47:13
updated: 2020-11-11 23:47:13
---


进入维护目录：`cd /usr/local/nginx/sbin`

```
./nginx -v 查看当前nginx版本号
./nginx -s stop 关闭
./nginx 开启
./nginx -s reload 重新加载配置文件（修改配置文件后进行）
```

安装目录：`/usr/local/nginx/sbinx`
