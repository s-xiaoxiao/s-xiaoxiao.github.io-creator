---
title: "浅析Vue "
date: 2020-01-10T22:19:36+08:00
draft: false
---

前言：请在 PC 浏览器下获得最佳阅读体验

# Vue

本文主要讲诉 Vue 两个版本的区别

<h2>1. 两个版本对应的名字</h2>

```
          完整版      只包含运行时版(非完整版 or 不完整版。文章中不完整版意思是只包含运行时版)
名字：    vue.js      vue.runtime.js

//注:生产环境的版本带min如:vue.min.js和vue.runtime.min.js
//带min的意思是该代码被压缩了。体积会小(加速加载代码),方便用户体验。
```

- 完整版：包含编译器和运行时的版本
- 编译器：用来将模板字符串编译成为 JavaScript 渲染函数的代码
- 运行时：用来创建 Vue 实例、渲染并处理虚拟 DOM 等的代码。基本上除去编译器的其他一切

<h2>2. template 和 render 使用方法</h2>
我们知道完整版是带有编译器的,不完整版就是不带有编译器。我们知道编译器就是把模板转译JavaScript渲染函数的代码
这是本小节最主要的区别所在

如下例：

```JavaScript
//template(模板)和 render用法 ---→ vue.js(完整版) VS vue.runtime.js(非完整版)

//完整版：需要编译器
new Vue({
  template:"<div>{{hi}}</div"
})

//不完整版：不需要编译器
new Vue({
  render(h){
    return h("div",this.hi)
  }
})

//template 的 value 是编译器所需要的模板
//h 函数是由 render 创建时由 vue 传进来的。最主要的目的时创建整个元素本身(是不是很像 createElement)
```

注： vue 支持单页面组件使用 vue-loader 把.vue 文件(包含 template、script、style )翻译成 h 构建方法.如下:

```JavaScript
improt Demo from "./Demo.vue"

new Vue({
  render(h){
    return h(Demo)
  }
})
```

<h2>3. codesandbox.io 写Vue</h2>
codesandbox.io 是什么？答:是一个在线的代码编辑器,主要聚焦于创建 Web 应用项目。当前已经进化为可以同时
支持浏览器端以及服务器的 Web 应用。

<a href="https://codesandbox.io/" target="_blank">
打开 codesandbox.io
</a>

1. → 点击右上角的 Create Sandbox 按钮
2. → 在 Official Templates 中点击 Vue 即可创建 Vue 项目

请勿登录该网站(登录后创建项目限制 在 50 个 以内),由于资源有限优先使用文字说明

## 总结：

1. Vue 两个版本代码上一个带有编译器一个没有。完整版对于开发者友好在写模板即可一目了然。非完整版对用户体验教好在加载性能。带 min 的是压缩后的文件
2. template 和 render。一个需要编译器一个不需要;
3. codesandbox.io 是一个在线代码编辑器。可以构建 web 应用项目。支持很多框架一键构建
4. 使用非完整版+单页面组件+vue-loader 来开发应用(最大好处是模块化)
