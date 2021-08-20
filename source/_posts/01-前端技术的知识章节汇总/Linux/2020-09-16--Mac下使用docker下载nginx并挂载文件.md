---
title: Mac下使用docker下载nginx并挂载文件
categories:
  - Linux
tags:
  - docker
  - nginx
abbrlink: efd0
date: 2020-09-16 00:12:22
updated: 2020-09-16 00:12:22
---

Mac下使用docker下载nginx并挂载文件。
也发到另外csdn：<https://blog.csdn.net/zenkin/article/details/108612594>

<!-- more -->

## 一、docker for macr客户端

- 对于10.10.3以上的用户 推荐使用 Docker for Mac [http://mirrors.aliyun.com/docker-toolbox/mac/docker-for-mac/](http://mirrors.aliyun.com/docker-toolbox/mac/docker-for-mac/)
- 同时，使用阿里云的镜像加速器，[https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors](https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors)  
- 阿里云的上面的地址里里有教程，就不转载了。

![Docker for Mac](https://i.loli.net/2020/09/16/qobDTG3cA4pIy5w.png)

> 若使用阿里云的docker镜像仓库也很不错，个人免费限制有点大，有空研究下；

## 二、安装nginx+静态文件+挂载

### 1、拉取nginx镜像

```bash
docker pull nginx:latest ###最新版本
```

![拉取nginx镜像](https://i.loli.net/2020/09/16/x5NMkuQ2m7wHYTz.png)

### 2、查看镜像

```bash
docker images  ### 查看搜有镜像
```

![查看镜像](https://i.loli.net/2020/09/16/7iE1Yn9mKusprtv.png)

### 3、运行镜像

```bash
docker run -d -p 4000:80 --name zen-nginx  nginx  ### 用4000端口映射80，用zen-nginx作为容器名，对应仓库nginx
```

![运行镜像](https://i.loli.net/2020/09/16/C5nIXgbZ2SEJi6x.png)

### 4、查看容器

```bash
docker ps -a ### 所有容器，包括未运行
```

![查看容器](https://i.loli.net/2020/09/16/rhg7IBx5u4sjRSF.png)

若安装docker mac，可以见：

![docker mac 查看容器](https://i.loli.net/2020/09/16/VKxuaOoYXLskiFn.png)

### 5、访问

```bash
curl 127.0.0.1:4000 ## curl
```

![curl访问](https://i.loli.net/2020/09/16/YqPINMjDk2sfJiR.png)

到浏览器访问：`http://127.0.0.1:4000`

![浏览器访问](https://i.loli.net/2020/09/16/ZwG6VE7N8sXDnmt.png)

### 6、以终端的方式进入nginx容器

如有docker for mac 则按钮进入，自动新建终端

![以终端的方式进入nginx容器](https://i.loli.net/2020/09/16/GjBap9PoQRXd78W.png)

或 以终端方式进入nginx容器

```bash
docker exec -it zen-nginx bash
```

![以终端方式进入nginx容器](https://i.loli.net/2020/09/16/7HDLkeN8ZtIhQ2v.png)

① 容器名； ②容器ID

### 7、查看有关文件夹 `etc/nginx`

> `ls`命令，查看文件文件夹列表、
> `cd`命令

`cd`进入目录，进入`cd etc`目录 或直接进入 `cd /etc/nginx`

```bash
cd / ###进入根目录
cd .. ### 返回上一级目录
```

![进入目录](https://i.loli.net/2020/09/16/zh4BY2u87y5aLm9.png)

### 8、查看文件cat nginx.conf 等文件

> 查看文件命令 `cat`

查看文件 `cat nginx.conf` ，为nginx配置文件

![nginx配置文件](https://i.loli.net/2020/09/16/pnbtehYXAxwa9G5.png)

查看包含了其他文件，继续查看 `cat /etc/nginx/conf.d/*.conf`

![继续查看/etc/nginx/conf.d/*.conf](https://i.loli.net/2020/09/16/erl2EahUFfi46ky.png)

看什么？
看有日志文件、资源文件的配置项。即：
`root /usr/share/nginx/html`，资源文件夹，没有会出现访问nginx服务器会出现403错误，`/usr/share/nginx/html`
`error_log /var/log/nginx/error.log warn`，日志文件夹 `/var/log/nginx`

### 9、退出容器

`exit ### 退出容器命令` 退出容器，或者使用 `docker for mac` 切换。

![退出容器](https://i.loli.net/2020/09/16/a5UQ4nFlGe7sTP3.png)

回到了系统终端。

### 10、创建本地挂载文件

![创建本地挂载文件](https://i.loli.net/2020/09/16/bMxeBJAirWfKTG2.png)

### 11、复制文件到本地

```bash
docker cp zen-nginx:/etc/nginx /Users/zenkin/Documents/00-个人档/11-docker/demo-nginx/config/    ### nginx配置文件
```

使用的是命令 `docker cp`，从容器复制到本地，如此类推：

```bash
docker cp zen-nginx:/var/log/nginx /Users/zenkin/Documents/00-个人档/11-docker/demo-nginx/logs/   ###日志文件，注意路径
docker cp zen-nginx:/usr/share/nginx/html /Users/zenkin/Documents/00-个人档/11-docker/demo-nginx/data/   ### 资源内容文件，注意路径
```

![容器复制到本地](https://i.loli.net/2020/09/16/ZU24nrlMxRNW3Ee.png)

> 有几次错误，复制回来的问题，与容器里的不对应，就删掉文件夹再来了，第二次成功。

### 12、关闭容器

关闭容器，准备重启 `docker rm -f zen-nginx`

### 13、重启并挂载文件

最关键一步，挂件文件，启动容器

```bash
docker run --name zen-nginx -p 4000:80 -v /Users/zenkin/Documents/00-个人档/11-docker/demo-nginx/config/nginx/:/etc/nginx -v /Users/zenkin/Documents/00-个人档/11-docker/demo-nginx/data/html:/usr/share/nginx/html -v /Users/zenkin/Documents/00-个人档/11-docker/demo-nginx/logs:/var/log/nginx -d nginx
```

### 14、修改默认配置文件

修改默认nginx配置文件，配置网站本地域名，注意改host，修改网站文件等等；

![成功](https://i.loli.net/2020/09/16/PzeqMmUCpybLnfS.png)

到此时已成功了。

### 15、docker for mac

在软件上看到挂载mounts

![mounts](https://i.loli.net/2020/09/16/qn7OKHNR9MV4jSF.png)

> 参考不分先后：

- [Nginx开发从入门到精通 — Nginx开发从入门到精通](http://tengine.taobao.org/book/index.html)
- [nginx基本配置与参数说明](http://www.nginx.cn/76.html)
- [Nginx 详解 （二）-天真小同志-51CTO博客](https://blog.51cto.com/dengxi/1747883)
- [windows下nginx的安装及使用方法入门 - 冒雨ing - 博客园](https://www.cnblogs.com/saysmy/p/6609796.html)
- [Windows下安装以及配置nginx - CSDN博客](http://blog.csdn.net/u011192409/article/details/51084831)
- [MacOS Docker 安装 | 菜鸟教程](https://www.runoob.com/docker/macos-docker-install.html)
- [Docker Getting Started_weixin_43162745的博客-CSDN博客](https://blog.csdn.net/weixin_43162745/article/details/83241492)
- [了解【Docker】从这里开始 - 我没有三颗心脏 - 博客园](https://www.cnblogs.com/wmyskxz/p/10943169.html)
- [(1) Docker入门，看这篇就够了 - 个人文章 - SegmentFault 思否](https://segmentfault.com/a/1190000009544565#articleHeader6)
- [Docker(一)：Docker入门教程 - 纯洁的微笑 - 博客园](https://www.cnblogs.com/ityouknow/p/8520296.html)
- [在 Mac 平台下搭建docker - nginx - 简书](https://www.jianshu.com/p/0c86a7580f3a)
- [mac环境下使用docker安装nginx_weixin_30455365的博客-CSDN博客](https://blog.csdn.net/weixin_30455365/article/details/95964381)
- 主要参考：[Mac 下使用docker下载nginx并挂载文件，解决端口问题_weixin_45493633的博客-CSDN博客】](https://blog.csdn.net/weixin_45493633/article/details/103101182?utm_medium=distribute.pc_relevant.none-task-blog-title-1&spm=1001.2101.3001.4242)
- [docker 安装nginx 并部署_ddhsea的博客-CSDN博客](https://blog.csdn.net/ddhsea/article/details/92203713?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param)


