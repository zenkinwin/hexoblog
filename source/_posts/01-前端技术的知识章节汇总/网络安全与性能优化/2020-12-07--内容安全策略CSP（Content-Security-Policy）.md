---
title: 内容安全策略CSP（Content-Security-Policy）
categories:
  - 网络安全与性能优化
tags:
  - 内容安全策略
  - csp
abbrlink: '6137'
date: 2020-12-07 23:43:16
updated: 2020-12-07 23:43:16
---

内容安全策略，Content-Security-Policy，缩写 CSP，其核心思想十分简单：网站通过发送一个 CSP 头部，来告诉浏览器**什么是被授权执行的与什么是需要被禁止的**。其被誉为专门**为解决XSS攻击而生的神器**。

在[SiteAzure](http://www.powereasy.net/products/siteazure)的3.0版本配置文件`web.config`中，增加了这个防XSS的神器。

``` yaml
<add name="Content-Security-Policy" value="
script-src 'self' 'unsafe-inline' 'unsafe-eval' blob: *.map.baidu.com 
*.bdimg.com bdimg.share.baidu.com res.wx.qq.com pucha.kaipuyun.cn 
dcs.conac.cn webservice.coolwei.com www.gov.cn;
object-src 'self'"/>
```

下面详细介绍如何使用 CSP 防止 XSS 攻击。

<!-- more -->

### 1. CSP是什么

> 跨域脚本攻击 XSS 是最常见、危害最大的网页安全漏洞。为了防止它们，要采取很多编程措施，非常麻烦。很多人提出，能不能根本上解决问题，浏览器自动禁止外部注入恶意脚本？这就是"网页安全政策"（Content Security Policy，缩写 CSP）的来历。
> 摘自<http://www.ruanyifeng.com/blog/2016/09/csp.html>

CSP 的实质就是**白名单制度**，开发者明确告诉客户端，哪些外部资源可以加载和执行，等同于提供白名单。它的实现和执行全部由浏览器完成，开发者只需提供配置。

### 2. CSP的作用

- 限制资源获取
- 报告资源获取越权

CSP 大大增强了网页的安全性。攻击者即使发现了漏洞，也没法注入脚本，除非还控制了一台列入了白名单的可信主机。

### 3. CSP的使用

两种方法可以启用 CSP，一种是通过 **HTTP 头信息**的`Content-Security-Policy`的字段，另一种是通过网页的`<meta>`标签。

**通过 HTTP 头信息的Content-Security-Policy的字段**

```
"Content-Security-Policy:" 策略
"Content-Security-Policy-Report-Only:" 策略
```

**通过网页的`<meta>`标签**

``` html
<meta http-equiv="content-security-policy" content="策略">
<meta http-equiv="content-security-policy-report-only" content="策略">
```

### 4. CSP的实例

```
Content-Security-Policy: 
script-src 'self'; 
object-src 'none';
style-src cdn.zenwu.site zenkin.win; 
child-src https:
```

这段代码中，CSP 做了如下配置。

- 脚本（`script-src`）：只信任当前域名；
-`<object>`标签（`object-src`）：不信任任何URL，即不加载任何资源；
- 样式表：只信任`cdn.zenwu.site`和`zenkin.win`；
- 框架（frame）（`child-src`）：必须使用`HTTPS`协议加载；
- 其他资源：没有限制

1、一个网站管理者想要所有内容均来自站点的同一个源 (不包括其子域名)
`Content-Security-Policy: default-src 'self'`

2、一个网站管理者允许内容来自信任的域名及其子域名 (域名不必须与CSP设置所在的域名相同)
`Content-Security-Policy: default-src 'self' *.trusted.com`

3、一个网站管理者允许网页应用的用户在他们自己的内容中包含来自任何源的图片, 但是限制音频或视频需从信任的资源提供者(获得)，所有脚本必须从特定主机服务器获取可信的代码.
`
Content-Security-Policy: default-src 'self'; img-src *; media-src media1.com media2.com; script-src userscripts.example.com`

在这里，各种内容默认仅允许从文档所在的源获取, 但存在如下例外:

- 图片可以从任何地方加载(注意 `“*”` 通配符)。
- 多媒体文件仅允许从 media1.com 和 media2.com 加载(不允许从这些站点的子域名)。
- 可运行脚本仅允许来自于 userscripts.example.com。

4、一个线上银行网站的管理者想要确保网站的所有内容都要通过SSL方式获取，以避免攻击者窃听用户发出的请求。
`Content-Security-Policy: default-src https://onlinebanking.jumbobank.com`

该服务器仅允许通过 `HTTPS` 方式并仅从 onlinebanking.jumbobank.com 域名来访问文档。

5.一个在线邮箱的管理者想要允许在邮件里包含HTML，同样图片允许从任何地方加载，但不允许JavaScript或者其他潜在的危险内容(从任意位置加载)。
`Content-Security-Policy: default-src 'self' *.mailsite.com; img-src *`

注意这个示例并未指定 `script-src`。在此CSP示例中，站点通过 `default-src` 指令的对其进行配置，这也同样意味着脚本文件仅允许从原始服务器获取。▲▲▲

### 5. 常见CSP指令

``` txt
script-src：外部脚本
style-src：样式表
img-src：图像
media-src：媒体文件（音频和视频）
font-src：字体文件
object-src：插件（比如 Flash）
child-src：框架
frame-ancestors：嵌入的外部资源（比如<frame>、<iframe>、<embed>和<applet>）
connect-src：HTTP 连接（通过 XHR、WebSockets、EventSource等）
worker-src：worker脚本
manifest-src：manifest 文件
```

<div class="table-box"><table><thead><tr><th style="width:120px">指令</th><th align="left" style="width:200px">指令和指令值示例</th><th align="left">指令说明</th></tr></thead><tbody><tr><td>default-src</td><td align="left">‘self’ cdn.zenwu.site</td><td align="left">默认加载策略</td></tr><tr><td>script-src</td><td align="left">‘self’ js.zenwu.site</td><td align="left">对 JavaScript 的加载策略。</td></tr><tr><td>style-src</td><td align="left">‘self’ css.zenwu.site</td><td align="left">对样式的加载策略。</td></tr><tr><td>img-src</td><td align="left">‘self’ img.zenwu.site</td><td align="left">对图片的加载策略。</td></tr><tr><td>connect-src</td><td align="left">‘self’</td><td align="left">对 Ajax、WebSocket 等请求的加载策略。不允许的情况下，浏览器会模拟一个状态为 400 的响应。</td></tr><tr><td>font-src</td><td align="left">font.cdn.zenwu.site</td><td align="left">针对 WebFont 的加载策略。</td></tr><tr><td>object-src</td><td align="left">‘self’</td><td align="left">针对 、 或 等标签引入的 flash 等插件的加载策略。</td></tr><tr><td>media-src</td><td align="left">media.cdn.zenwu.site</td><td align="left">针对媒体引入的 HTML 多媒体的加载策略。</td></tr><tr><td>frame-src</td><td align="left">‘self’</td><td align="left">针对 frame 的加载策略。</td></tr><tr><td>report-uri</td><td align="left">/report-uri</td><td align="left">告诉浏览器如果请求的资源不被策略允许时，往哪个地址提交日志信息。 特别的：如果想让浏览器只汇报日志，不阻止任何内容，可以改用 Content-Security-Policy-Report-Only 头。</td></tr></tbody></table></div>

### 6. 其他的CSP指令

<div class="table-box"><table><thead><tr><th style="width:120px">指令</th><th align="left" style="width:200px">指令和指令值示例</th><th align="left">指令说明</th></tr></thead><tbody><tr><td>sandbox</td><td align="left"></td><td align="left">设置沙盒环境</td></tr><tr><td>child-src</td><td align="left"></td><td align="left">主要防御 <code>&lt;frame&gt;</code>,<code>&lt;iframe&gt;</code></td></tr><tr><td>form-action</td><td align="left"></td><td align="left">主要防御 <code>&lt;form&gt;</code></td></tr><tr><td>frame-ancestors</td><td align="left"></td><td align="left">主要防御 <code>&lt;frame&gt;</code>,<code>&lt;iframe&gt;</code>,<code>&lt;object&gt;</code>,<code>&lt;embed&gt;</code>,<code>&lt;applet&gt;</code></td></tr><tr><td>plugin-types</td><td align="left"></td><td align="left">主要防御 <code>&lt;object&gt;</code>,<code>&lt;embed&gt;</code>,<code>&lt;applet&gt;</code></td></tr></tbody></table></div>

### 7. CSP指令值

<div class="table-box"><table><thead><tr><th style="width:120px">指令值</th><th align="left" style="width:200px">指令和指令值示例</th><th align="left">指令值说明</th></tr></thead><tbody><tr><td>*</td><td align="left">img-src *</td><td align="left">允许任何内容。</td></tr><tr><td>‘none’</td><td align="left">img-src ‘none’</td><td align="left">不允许任何内容。</td></tr><tr><td>‘self’</td><td align="left">img-src ‘self’</td><td align="left">允许来自相同来源的内容（相同的协议、域名和端口）。</td></tr><tr><td>data:</td><td align="left">img-src data:</td><td align="left">允许 data: 协议（如 base64 编码的图片）。</td></tr><tr><td>全域名</td><td align="left">img-src img.zenwu.site</td><td align="left">允许加载指定域名的资源。</td></tr><tr><td>*.zenwu.site</td><td align="left">img-src *.zenwu.site</td><td align="left">允许加载 zenwu.site 任何子域的资源。</td></tr><tr><td>‘unsafe-inline’</td><td align="left">script-src ‘unsafe-inline’</td><td align="left">允许加载 inline 资源（例如常见的 style 属性，onclick，inline js 和 inline css 等等）。</td></tr><tr><td>‘unsafe-eval’</td><td align="left">script-src ‘unsafe-eval’</td><td align="left">允许加载动态 js 代码，例如 eval()。</td></tr></tbody></table></div>

---
参考文章：
<http://www.ruanyifeng.com/blog/2016/09/csp.html>
<https://blog.csdn.net/u014465934/article/details/84199171>
<https://blog.csdn.net/qq_37943295/article/details/79978761>
