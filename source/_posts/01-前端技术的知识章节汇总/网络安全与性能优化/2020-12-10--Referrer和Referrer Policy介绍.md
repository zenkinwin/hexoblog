---
title: Referrer和Referrer Policy介绍
categories:
  - 网络安全与性能优化
tags:
  - Referrer
  - Referrer Policy
abbrlink: b690
date: 2020-12-10 23:03:06
updated: 2020-12-15 23:05:45
photos: https://cdn.zenwu.site/upload/pic/2020/20201213000022.png
---
### 1. Referrer

`referrer`是HTTP请求`header`的报文头，用于**指明当前流量的来源参考页面**。通过这个信息，我们可以知道访客是怎么来到当前页面的。这对于网站分析（Web Analytics）非常重要，可以用于分析不同渠道流量分布、用户搜索的关键词等。

但是，这个字段同时会造成用户敏感信息泄漏（如：带有敏感信息的重置密码URL，若被Web Analytics收集，则存在密码被重置的危险）。

### 2. Referrer-Policy

The Referrer-Policy HTTP header governs which referrer information, sent in the Refererheader, should be included with requests made.

通俗点就是**Referrer的策略**， **Referrer 就是 referrer 属性可返回载入当前文档的文档的 URL。**

<!-- more -->

``` yaml
Referrer-Policy: no-referrer
Referrer-Policy: no-referrer-when-downgrade
Referrer-Policy: origin
Referrer-Policy: origin-when-cross-origin
Referrer-Policy: same-origin
Referrer-Policy: strict-origin
Referrer-Policy: strict-origin-when-cross-origin
Referrer-Policy: unsafe-url
```

如果值无效就是默认值。

### 3. Referrer-Policy值

**no-referrer**
整个`Referer`首部会被移除。访问来源信息不随着请求一起发送

**no-referrer-when-downgrade （默认值）**
在没有指定任何策略的情况下用户代理的默认行为。在同等安全级别的情况下，引用页面的地址会被发送(HTTPS->HTTPS)，但是在降级的情况下不会被发送 (HTTPS->HTTP)。

**origin**
在任何情况下，仅发送文件的源作为引用地址。例如 `https://example.com/page.html` 会将 `https://example.com/` 作为引用地址。

**origin-when-cross-origin**
对于同源的请求，会发送完整的URL作为引用地址，但是对于非同源请求仅发送文件的源。

**same-origin**
对于同源的请求会发送引用地址，但是对于非同源请求则不发送引用地址信息

**strict-origin**
在同等安全级别的情况下，发送文件的源作为引用地址(HTTPS->HTTPS)，但是在降级的情况下不会发送 (HTTPS->HTTP)。

**strict-origin-when-cross-origin**
对于同源的请求，会发送完整的URL作为引用地址；在同等安全级别的情况下，发送文件的源作为引用地址(HTTPS->HTTPS)；在降级的情况下不发送此首部 (HTTPS->HTTP)。

**unsafe-url**
无论是同源请求还是非同源请求，都发送完整的 URL（移除参数信息之后）作为引用地址。（最不安全的策略了）

### 4. referrer具体设置

#### 4.1. CSP响应头

CSP（Content Security Policy）

```
Content-Security-Policy: referrer no-referrer|no-referrer-when-downgrade|origin|origin-when-cross-origin|unsafe-url;
```

指令和指令值之间以空格分割，多个指令之间用英文分号分割。

#### 4.2. 标签

html页面的meta标签指定。如果content属性不是合法的取值，浏览器会自动选择no-referer策略。

```
<meta name="referrer" content="no-referrer|no-referrer-when-downgrade|origin|origin-when-crossorigin|unsafe-url">
```

#### 4.3. 标签的referer属性

- 作用的只是当前标签。
- 策略只有三中：不传、只host、都传（即完整URL）。
-针对单个链接设置的策略优先级比CSP和要高。

```
<a href="http://example.com" referrer="no-referrer|origin|unsafe-url">xxx</a>
```

---

参考：
<https://w3c.github.io/webappsec-referrer-policy/#referrer-policy-header>
<https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Referrer-Policy>
<https://www.jianshu.com/p/92bd520c0f8f>
<https://www.cnblogs.com/amyzhu/p/9716493.html>
