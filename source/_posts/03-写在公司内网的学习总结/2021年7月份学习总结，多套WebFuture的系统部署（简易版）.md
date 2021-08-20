---
title: 2021年7月份学习总结，多套WebFuture的系统部署（简易版）
categories:
  - 写在公司内网的学习总结
tags:
  - WebFuture
  - Linux
  - 系统部署
  - 中标麒麟
  - 达梦数据库
date: '2021年08月03日 15:37:15'
updated: '2021年08月03日 15:37:15'
abbrlink: '9524'
---

> 本文摘录2021年7月份学习总结，创建日期:2021年08月03日 15:37:15，有修改。
> 在*Linux（中标麒麟）+达梦数据库+WebFuture*搭配下部署。

「Linux（中标麒麟）+达梦数据库+WebFuture」的环境安装过程就略了，可参考软件包内的部署文档。在一台服务器下的多套 `WebFuture` 的部署对项目实施部门就很重要了，先试先行。

<!--more-->

#### 1. 网站文件上存

这里注意点是：不同的网站文件夹，包括 `/root` 下和 `/var/www/` 下的。

压缩包xftp传文件到 `/root` 下，解压：

```
cd ~
sudo unzip -q webfuture_linux_10.0.0.0_20210726.zip -d ~/webfuture-RC
```

创建位于 `/var/www` 下的网站文件夹，复制文件。

```
sudo mkdir -p /var/www/webfuture-RC/
sudo cp -rfT ~/webfuture-RC/WebSite /var/www/webfuture-RC
```

#### 2. 数据库安装

进入数据库目录，先执行 ` cd /home/dmdba/dmdbms/bin `

```
# 连接本地数据库，账户SYSDBA、密码SYSDBA均在环境部署时候创建了。
./disql SYSDBA/SYSDBA@localhost //进入disql命令环境

# 创建一个数据库登录名WEBFUTURE-RC
create user "WEBFUTURE-RC" identified by "WEBFUTURE-RC";    // 注意这里是区分大小写的

# 给WEBFUTURE-RC设置权限
grant "DBA","PUBLIC","VTI","SOI" to "WEBFUTURE-RC";

# 输入exit退出
exit  // 退出disql环境
```

#### 3. 配置数据库连接字符串

WebFuture的链接字符串文档在网站的 `/Configuration` 目录下的 connectionstrings.json 文件，将 `connectionstrings.json` 文件中的 `"CurrentProvider": "SqlServer"`，及数据库、账号密码改成对应所使用数据库的配置。

`CurrentProvider` 在这里就应该是 `DaMeng` ，这里共3处需要修改：
*DmConnection、ConnectionMonitorConnection、DataBaseOutputCacheConnection*

![connectionstrings.json配置](https://cdn.zenwu.site/upload/pic/2021/20210803145605.jpg)

#### 4. 监测应用

创建服务文件，举例创建服务名 `webfuture-RC.service`，我是复制和修改的，下面是 `WebFuture 10.0.0.0 rc` 版，用的是 `7500` 端口：

```
[Unit]
Description=————————WebFuture-RC-Website-Servic————————
[Service]
WorkingDirectory=/var/www/webfuture-RC
ExecStart=/usr/share/dotnet/dotnet /var/www/webfuture-RC/PowerEasy.WebSite.Government.dll
Restart=always
RestartSec=10
KillSignal=SIGINT
SyslogIdentifier=webfuture-10-RC
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=ASPNETCORE_URLS=http://*:7500
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false
Environment=DatabaseProvider__CurrentProvider=DaMeng 
[Install]
WantedBy=multi-user.target
```

设置权限用户 `www-data` 对这个服务的权限：

```
sudo chown www-data -R /var/www/webfuture-RC
```

再来就是对服务必备节奏：

```
# 启动服务
systemctl start webfuture-RC.service

# 设置服务自启动，打开服务后会自动开启网站运行
systemctl enable webfuture-RC.service

# 查看服务的运行状态
systemctl status webfuture-RC.service

# 修改完服务文件后，需要重启服务
sudo systemctl daemon-reload
sudo systemctl restart webfuture-RC.service 

# 查看日志
sudo journalctl -fu webfuture-RC
```

#### 5. 端口开放

防火墙管理命令：<https://wangchujiang.com/linux-command/c/firewall-cmd.html>。

```
# 首先开放防火墙端口
firewall-cmd --permanent --add-port=7500/tcp
# 重启防火墙
firewall-cmd --reload
```

端口监测，可以用 `netstat` 显示网络状态，如没有就安装：

```
# 安装netstat
yum -y install net-tools
[root@localhost ~]# netstat -anlp | grep 7500
tcp6       0      0 :::7500                 :::*                    LISTEN      6213/dotnet         
[root@localhost ~]# netstat -anlp | grep 7500
tcp6       0      0 :::7500                 :::*                    LISTEN      6213/dotnet         
tcp6       0      0 192.168.8.40:7500       183.27.96.61:1801       ESTABLISHED 6213/dotnet         
tcp6       0      0 192.168.8.40:7500       183.27.96.61:1802       ESTABLISHED 6213/dotnet
```

#### 6. 运行网站

运行网站，数据库生成，等待，期间密切监控服务。

```
# 查看服务的运行状态
systemctl status webfuture-RC.service
```
