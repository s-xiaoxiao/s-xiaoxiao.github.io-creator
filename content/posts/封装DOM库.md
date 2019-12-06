---
title: "手写DOM库 "
date: 2019-11-30T16:17:36+08:00
draft: false
---

# 前言

上篇 blog,我们学习了 DOM (Document Object model)操作.但是每个 API 的名字实在是有点长.这次我们把哪些 JS 本身的 API 给封装成一个库供大家便利。,

## 全局对象

```JavaScript
window.dom = {}   //我们提供一个全局对象供我们调用
```

## 增(Create)

```JavaScript
dom.create = (node){   //新增节点
  const container = document.createElement('template');
  container.innerHTML = node.trim()
  return template.content.firstChild
}
//Document.createElement() 方法 :创建由tagName指定的HTML元素
//<template></template> 内容模板元素,改内容在加载页面时不会呈现,但随后可以在运行时使用JavaScript实例化
//Element.innerHTML 属性：设置或获取HTML语法表示的元素的后代
//String.prototype.trim() 方法：会从一个字符串的两端删除空白字符.上下文中的空白字符是所有的空白字符(space,tab,no-break space等)
//HTMLTemplateElement.content 属性 :返回<template>元素的模板内容
//Node.firstChild : 只读属性返回树中节点的第一个子节点,如果节点是无子节点,则返回null


dom.after = (node,node2){ //新增弟弟
  node.parentNode.insertBefore(node2,node.nextSibling)
}
//Node.parentNode 属性 : 返回指定的节点在DOM树中的父节点
//Node.insertBefore() 方法 ：方法在参考节点之前插入一个拥有指定父节点的子节点.如果给定的子节点是对文档中现有节点的引用,insertBefore()会将其从当前位置移动到新位置
//Node.nextSibling : 返回与该节点同级的下一个节点Node,如果没有返回null

dom.before=(node,node2){  //新增哥哥
  node.parentNode.insertBefore(node2,node);
}
//Node.parentNode : 返回一个当前节点的父节点.如果没有这个结点,返回null
//Node.insertBefore() : 在当前节点下增加一个子节点的Node,并使该子节点位于参考节点的前面

dom.append(parent,child){ //新增儿子
  parent.appendChild(child)
}
//Node.appendChild() 方法 ：将指定的childNode参数作为最后一个子节点添加到当前节点,如果参数引用了DOM树中的现有节点,则节点将从当前位置分离,并附加到新位置

dom.wrap(node,parent){     //新增爸爸
  dom.before(node,parent)
  dom.append(parent,node)
}

```

## 删(Delete)

```JavaScript
dom.remove(node){   //用于删除节点
  return node.parentNode.removeChild();
}
//Node.parentNode ：返回一个当前节点Node的父节点。如果没有这样的节点返回Null
//Node.removeChild : 从当前元素中删除一个子节点,该子节点必须是当前节点的子节点

dom.empty(parent){  //用于删除后代
  const array = []
  let x=parent.firstChild
  while(x){
    array.push(dom.remove(parent.firstChild))
    x=parent.firstChild
  }
  return array
}
//Node.firstChild()：返回该节点的第一个子节点Node,如果该节点没有子节点则返回null
//Array.prototype.push() 方法：将一个或多个元素添加到数组的末尾,并返回该数组的长度
```

## 改(Update)

```JavaScript
dom.attr(node,name,value){    //get attr or set attr
  if(arguments.length === 3){
    node.setAttribute(name,value)
  }else if(arguments.length === 2){
    return node.getAttribute(name)
  }
}
//Function.arguments 属性代表传入函数的实参,它是一个类数组对象
//Function.arguments.length :本次函数调用时传入函数的实参数量
//Element.setAttribute：设置指定元素上的某个属性值.如果该属性已经存在,则更新该值;否则使用指定的名称和值添加一个新的属性
//Element.getAttribute:返回元素上一个指定的属性值.如果指定的属性不存在,则返回null或""(空字符串)

dom.text(node,string){    //读写text内容
  if(arguments.length === 2){
    if('innerText' in node){
      node.innerText = string
    }else{
      node.textContent = string
    }
  }else if(arguments.length === 1){
    if('innerText' in node){
      return node.innerText
    }else{
      return node.textContent
    }
  }
}
//Function.arguments : 代表传入函数的实参,它是一个类数组对象
//Function.arguments.length：本次函数调用时传入函数的实参数量
//HTMLElement.innerText :属性表示一个节点及其后代的“渲染”文本内容
//Node.textContent : 属性表示一个节点及其后代的文本内容
//innerText源起ie,textContent是标准.

dom.html(node,string){        //读写HTML内容
  if(arguments.length === 2){
    node.innerHTML = string
  }else{
    return node.innerHTml
  }
}
//arguments.length：本次函数调用时传入函数的实参数量
//Element.innerHTML :属性设置或获取HTML语法表示的元素的后代

dom.style(node,name,value){   //修改style
  if(arguments.length === 3){
    node.style[name] = value
  }else if(arguments.length === 2){
    if(typeof name === 'string'){
      return node.style[name]
    }else if(name instanceof Obejct){
      for(key in name){
        node.style[key] = name[key]
      }
    }
  }
}
//arguments.length :本次函数调用时传入函数的实参数量
//typeof 操作符返回一个字符串,表示未经计算的操作数的类型
//instanceof运算符用于检测构造函数的prototype属性是否出现某个实例对象的原型链上
//for...in 语句：任意顺序遍历一个对象的除symbol以外的可枚举属性

dom.class ={}
dom.class.add(node,className){      //添加 className
  node.classList.add(className)
}
//Element.classList.add : 添加指定的类值.如果这些类已经存在于元素的属性中,那么它们将被忽略

dom.class.remove(node,className){   //删除 className
  node.classList.remove(className)
}
//Element.classList.remove :删除指定的类值,删除不存在的类值会导致抛出异常

dom.class.has(node,className){  //className是否存在
  return node.classList.contains(className)
}
//Element.classList.contains :检测元素的类class属性中是否存在指定的类值
//

dom.on(node,eventName,fn){    //添加事件监听
  node.addEventListener(eventName,fn)
}
//EventTarget.addEventListener 方法：在eventTarget上注册特定事件类型的事件处理程序

dom.off(node,eventName,fn){   //删除事件监听
  node.removeEventListener(eventName,fn)
}
//EventTarget.removeEventListener 方法: 在eventTarget中删除事件侦听器


```

## 查(Read)

```JavaScript
dom.find(selector,scope){   //获取元素
  return (scope||document).querySelectorAll(selector);
}
//Document.querySelectorAll 方法:返回与指定的选择器组匹配的文档中的元素列表,返回的是NodeList

dom.parent(node){     //用于获取父元素
  return node.parentNode
}
//Node.parentNode 返回指定在DOM树中的父节点

dom.children(node){   //用于获取子元素
  return node.children
}
//是一个只读属性,返回一个Node的子elements,是一个动态的更新的HTMLCollection

dom.siblings(node){   //用于获取兄弟姐妹
  return Array.from(node.parentNode.children).filter(n=>n!==node)
}
//Array.from :方法从一个类似数组或可迭代对象创建一个新的,浅拷贝的数组实例
//Node.parentNode:返回当前节点的父节点
//ParentNode.children:是一个只读属性,返回一个Node的子elements,是一个动态更新的HTMLCollection
//Array.prototype.filter :方法创建一个新数组,其包含通过所提供函数实现的测试的所有元素

dom.next(node){     //查找下一个
  return node.nextSibling
}
//Node.nextSibling :是一个只读属性,返回其父节点的childNodes列表中的紧跟在其后面的节点,如果指定的节点为最后一个节点,则返回null

dom.previous(node){   //查找上一个
  return node.previousSibling
}
//Node.previousSibling：返回当前的节点前一个兄弟节点,没有则返回null

dom.each(nodes,fn){   //遍历所有节点
  if(nodes.length){
    for(let i=0;i<nodes.length;i++){
      fn.call(null,nodes[i])
    }
  }
}
//Function.prototype.call()方法使用一个指定的this值和单独给出的一个或多个参数来调用一个函数

dom.index(node){  //查询排行第几
  const list = dom.children(node.parentNode)
  let i=0;
  for(;i<list.length;i++){
    if(list[i] === node){
      break
    }
  }
  return i
}
//Node.parentNode : 返回当前节点的父节点
//ParentNode.children : 返回当前节点的子elements.是一个动态更新的HTMLCollection
```

> 我们现在写完了增删改查的 DOM 库,下次我们以 jQuery 的形式再写一遍,你会发现 jQuery 的好处了。see you later
