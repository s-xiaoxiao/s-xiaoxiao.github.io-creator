---
title: "jQuery的设计模式——下 "
date: 2019-12-07T14:36:36+08:00
draft: false
---

> 前言本文是 jQuery 的设计模式——上的续篇.在上篇我们实现 jQuery 函数的实现.还有很多细节的问题需要我们来完善。

问题

1. 我觉得 jQuery 这个名字太长了,我可不可以换一个符号来代替?可以.
2. 我应该如何创建元素?用重载.
3. 如果我调用了两次以及多次 jQuery 的返回的对象。是不是太占用内容了？是的,可以使用原型.
4. 如果我把 jQuery 构造函数的实例对象当作实参传进去,该怎么区别它是一个 jQuery 实例？我们可以给它添加一个属性,值为 true.这样的话我们只需要判断这个属性是不是为 true。

注：整体大概思路是这样的,不过跟 jQuery 相比我们只是阐述了调用 jQuery()函数之后的链式操作和一些节约内存的问题以及如何重载的问题.学习 jQuery 的 DOM 操作有助于我们学习 vue react 等框架,如果看完你有所疑惑请看 jQuery 中文文档

## 1. 用\$代替 jQuery

```JavaScript
window.$ = window.jQuery= function() = {
}

//这样声明之后呢,我们就可以用$()来调用了。
```

## 2. 创建元素(用重载)

```JavaScript
window.$ = window.jQuery = function(selectorOrArrayOrTemplate){
  let elements
  if(selectorOrArrayOrTemplate === 'string'){
    if(selectorOrArrayOrTemplate.trim()[0] === '<'){
      elements = createElement(selectorOrArrayOrTemplate)   //创建一个元素
    }else{
      elements = document.queryElementAll(selectorOrArrayOrTemplate)  //查找一个元素
    }
  }else if(selectorOrArrayOrTemplate instanceof Array){
    elements = selectorOrArrayOrTemplate                    //元素数组
  }
  //创建元素
  function createElement(string){
    const container = document.createElement("template")
    container.innerHTML = string.trim()
    return container.content.firstChild()
  }
}

//重载：大概的意思是针对不同的参数执行不同的代码(上述代码只是方法其一)
```

## 3. 原型 and 区分 jQuery 实例对象的属性

```JavaScript
window.$ = window.jQuery = function(selectorOrArrayOrTemplate){
  let elements
  if(selectorOrArrayOrTemplate === 'string'){
    if(selectorOrArrayOrTemplate.trim()[0] === '<'){
      elements = createElement(selectorOrArrayOrTemplate)   //创建一个元素
    }else{
      elements = document.queryElementAll(selectorOrArrayOrTemplate)  //查找一个元素
    }
  }else if(selectorOrArrayOrTemplate instanceof Array){
    elements = selectorOrArrayOrTemplate                    //元素数组
  }
  //创建元素
  function createElement(string){
    const container = document.createElement("template")
    container.innerHTML = string.trim()
    return container.content.firstChild()
  }

  const api = Object.create(jQuery.prototype)
  Object.assign(api,{
    elements:elements,
    oldApi:selectorOrArrayOrTemplate.oldApi
  })

  return api
  //Object.create():方法创建一个对象,使用现有的对象来提供新创建的对象的__proto__.
  //Object.create():方法用于将所有可枚举属性的值从一个或多个源对象赋值到目标对象。它将返回目标对象
}
```

```JavaScript
jQuery.prototype = {
  //保存构造函数
  constructor: jQuery,
  //这个属性主要目标是在调用jQuery.prototype.方法时区分它是不是jQuery构造函数的实例对象。
  //实例见 append and appendTo
  jquery: true,
  get(index) {
    return this.elements[index];
  },
  //添加到node元素的子元素
  appendTo(node) {
    if (node instanceof Element) {
      this.each(el => node.appendChild(el));
    } else if (node.jquery === true) {
      this.each(el => node.get(0).appendChild(el));
    }
  },
  //添加子元素
  append(children) {
    if (children instanceof Element) {
      this.get(0).appendChild(children);
    } else if (children instanceof HTMLCollection) {
      for (let i = 0; i < children.length; i++) {
        this.get(0).appendChild(children[i]);
      }
    } else if (children.jquery === true) {
      children.each(node => this.get(0).appendChild(node));
    }
  },
};
```

> 目前来说 1-4 的问题已经解决了.下次我们用 jQuery 做一个前端导航项目

再见
