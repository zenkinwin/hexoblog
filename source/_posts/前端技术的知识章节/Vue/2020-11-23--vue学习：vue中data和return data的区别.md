---
title: vue学习：vue中data和return data的区别
categories:
  - 前端技术的知识章节
  - vue
tags:
  - data
  - vue
abbrlink: 'vue/7e25'
updated: 2020-11-23 23:14:12
---

在简单的vue实例中看到的Vue实例中data属性是如下方式展示的：

```
let app= newVue({
    el:"#app",
    data:{
        msg:''
    },
    methods:{
    }
})
```

在使用组件化的项目中使用的是如下形式：

```
export default{
    data(){
        return {
        }
    },
    methods:{
    }
}
```

### 为什么要写return返回

因为不使用return包裹的数据会在项目的**全局可见**，会造成变量污染
使用**return包裹后数据中变量只在当前组件中生效**，不会影响其他组件。
