---
title: Redirecting to /bin/systemctl restart sshd.service
categories:
  - Linux
tags:
  - linux
  - nginx
abbrlink: 446f
date: 2020-11-12 00:00:53
---

云服务器 ECS Linux CentOS 7 下重启服务不再通过`service`操作，而是通过`systemctl`操作。

查看：`systemctl status sshd.service`

启动：`systemctl start sshd.service`

重启：`systemctl restart sshd.service`

自启：`systemctl enable sshd.service`
