---
title: vue学习：v-text，v-html, v-model, {{}}之间的异同
categories:
  - 前端技术的知识章节
  - vue
tags:
  - v-text
  - v-html
  - v-model
  - vue
abbrlink: vue/224e
date: 2020-11-22 23:16:41
updated: 2020-11-22 23:16:41
---

### 1. v-text

`v-text`是用于操作纯文本，它会替代显示对应的数据对象上的值。当绑定的数据对象上的值发生改变，**插值处**的内容也会随之更新。注意：此处为**单向绑定**，数据对象上的值改变，插值会发生变化；但是当插值发生变化并不会影响数据对象的值。

```
其中：v-text可以简写为{{}}，并且支持逻辑运算。
```

```
<div id="app">
  {{ msg }}
</div>
```

```
var app = new Vue({
    el : '#app',
    data : {
        msg : 'hello world'
    }
})
```

注：`vue`中有个指令叫做 `v-once` 可以通过`v-once`与`v-text`结合，实现仅执行一次性的插值。

```
<span v-once>这个将不会随msg属性的改变而改变: {{ msg }}</span>
```

### 2. v-html

`v-html`用于输出`html`，它与`v-text`区别在于`v-text`输出的是**纯文本**，浏览器不会对其再进行html解析，但`v-html`会将其当**html标签解析后输出**。

```
<div id="app">
    <p v-html="html"></p>
</div>
```

```
let app = new Vue({
        el: "#app",
        data: {
            html: "<b style='color:red'>v-html</b>"
        }
    });
```

### 3. v-model

`v-model`通常用于**表单组件**的绑定，例如`input，select`等。它与`v-text`的区别在于它**实现的表单组件的双向绑定**，如果用于表单控件以外标签是没有用的。

```
<div id="app">
  <input v-model="message " />
</div>
```

```
var app = new Vue({
   el : '#app',
   data : {
    message : 'hello world'
 }
})
```

---

本文链接：<https://blog.csdn.net/u014541501/article/details/78181729>
