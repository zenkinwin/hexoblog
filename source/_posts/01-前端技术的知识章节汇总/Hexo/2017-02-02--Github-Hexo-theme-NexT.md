---
title: Github-Hexo-theme-NexT
categories:
  - Hexo
tags:
  - Github
  - hexo
  - NextT
abbrlink: 1be4
date: 2017-02-02 00:00:00
updated: 2017-02-02 00:00:00
---

上月介绍了Git+jekyll，托管在码云（git@osc）上，然而并不尽兴，事实上Git+Hexo会更便捷（对window用户而言），jekyll是ruby编写，而hexo是nodejs 。  

Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。
jekyll是把原文上传github（md文件），可以直接生成博客，文件也可以用在线编辑修改，而hexo 是本地生成 html 再上传。
借着假期，纠结了2天，搭建个人博客，以下记录下这次搭建过程：

而这次用到的模板是大名鼎鼎的`nexT`，教程：http://theme-next.iissnan.com/ 

 <!-- more --> 

## 准备工作

1. 安装 node，https://nodejs.org/en/download/ 在NodeJs官网下载对应版本进行安装。
2. 安装 git，在Git官网下载对应版本进行安装。步骤如下（借图）：
![](http://wp-00-wp.qiniudn.com/0.183261890062568.png)
![](http://wp-00-wp.qiniudn.com/0.12367357255081979.png)
![](http://wp-00-wp.qiniudn.com/0.03426800610440184.png)
![](http://wp-00-wp.qiniudn.com/0.03366818130083549.png)
![](http://wp-00-wp.qiniudn.com/0.8714599287577482.png)

可以通过以下命令测试是否安装成功：
```
$ git --version #git版本
$ node --version #NodeJs版本
```
![](http://wp-00-wp.qiniudn.com/33fde566-d2b3-4ea5-9c1d-5f7eac68e64e.png)

## 注册Github账号

去 Github 官网进行注册即可，注册完之后记得添加 SSH Key，这个 SSH Key是一个认证，让github识别绑定这台机器，允许这台机器提交。S
SH找了个教程，传送门：http://blog.csdn.net/hustpzb/article/details/8230454/
检查通过以下命令测试是否安装成功：
```
$ ssh git@github.com
```
See :
![](http://wp-00-wp.qiniudn.com/f2e2bab6-160a-4982-ad3e-f08f6cf09f78.png)

## 安装Hexo

安装好`git`和`node`之后，就可以安装`hexo`了，简单一句命令搞定:
```
$ npm install -g hexo-cli
```
检查通过以下命令测试是否安装成功：
```
$ hexo version #简写hexo v
```
![](http://wp-00-wp.qiniudn.com/0a776216-23d4-48d8-b98a-96f938e78096.png)

## 创建Hexo本地博客

安装完成后，执行以下命令，**Hexo**将在指定目录下新建需要的文件
```
$ hexo init <folder> # 用命令创建一个目录并且初始化目录hexo文件
```
PS：
1. 可以是手动新建文件夹，而后再 `$ hexo init` 初始化目录；
2. hexo会自动下载一套默认皮肤：`landscape`，整个安装下载过程可能有点慢……
3. 我是直接在 `git bash` 里命令的，也可以在其他命令符状态下安装；
初始化后需要再 `$ npm install`  进行npm的依赖插件。

![](http://wp-00-wp.qiniudn.com/759463df-5e24-406b-ae9b-476a3484341b.png)


## 部署形成文件

```
$ hexo generate # 简写hexo g
```

最后剩下运行server，跑起hexo服务：

```
$ hexo server # 简写hexo s
```

![](http://wp-00-wp.qiniudn.com/27968e2b-d3fa-440c-8e6d-57c9bfb91ab5.png)

浏览器运行 http://http://localhost:4000/ 就能成功看到了。 

![](http://wp-00-wp.qiniudn.com/8cf4be10-5e83-4872-ba98-c03f0a8c8b80.jpg)

### 将本地hexo项目托管到Github

打开网站配置文件 *_config.yml* （根目录的文件），网站的配置文件，你可以在这里配置一些基本信息，这里列举部分关键配置：
![](http://wp-00-wp.qiniudn.com/5c55c6ac-2638-435e-a0b5-def4955cd1e1.png)

```html 
# Hexo Configuration
## Docs: https://hexo.io/docs/configuration.html
## Source: https://github.com/hexojs/hexo/
 
# Site
title: Zenkin.Win ’s Blog #网站的标题
subtitle: 世事如棋，乾坤莫测 #副标题
description: 
author: Zen.Woo #作者信息
avatar: /images/avatar.png #头像，图片位置在相应主题目录下的images
language: zh-Hans #中文简体
email: 43002111@qq.com
timezone:
 
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: next #配置主题，这里使用next主题
stylus:
  compress: true #自适应布局
 
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git #部署环境，基于hexo+githubpage,所以这里使用git。注意：不同版本的hexo，type有可能不同，3.x以后应使用git,具体参看官方文档
  repository: git@github.com:username/username.github.io.git #git仓库地址，替换成你的username即可，其他保持不变，后面会提到如何创建git仓库
  branch: master
```
编辑最后面的 `deploy` 属性，加入代码：
```
type: git
repository: git@github.com:xxxxxx/xxxxxx.github.io.git #替换github地址，可https可ssh
branch: master #分支branch填写master
```

### 安装hexo-deployer-git插件

```
npm install hexo-deployer-git --save
```
经常遇到问题是忘记安装，找不到git、

### 发布过程遇到问题

过程中遇到提示，查到是这样：
``` html
Deployer not found: github
```
遇错情景： 最近从next主题切换到新主题的时候部署时遇到这个问题。
报错原因： 没有 *hexo-deployer-git* 这个东西，所以需要重新安装一次。
注意事项： Hexo 3.0以前是使用的是hexo-deployer-github，3.0之后官方更改为hexo-deployer-git
解决方法： 重新安装
```
$ npm install hexo-deployer-git –save
```

### 部署你本地的主题到github上

代码如下，每次修改本地主题，都需要执行以下代码
```
$ hexo clean
$ hexo generator #简写 hexo g
$ hexo deploy #简写 hexo d #启动本地服务，进行文章预览调试，执行如下命令
$ hexo server #简写 hexo s
```
## 参考

next主题说明：http://theme-next.iissnan.com/getting-started.html
https://hexo.io/zh-cn/api/
http://www.jianshu.com/p/858ecf233db9  
配置SSH http://jingyan.baidu.com/article/d8072ac47aca0fec95cefd2d.html 

