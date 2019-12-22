---
title: "AJAX的原理 "
date: 2019-12-18T5:32:36+08:00
draft: false
---

> 前言 : AJAX (Asynchronous JavaScript And Xml)(非同步的 JavaScript 与 XML 技术):主要功能是存取数据用的.

# AJAX 的原理

<pre>
    用JS发送请求和收响应(在早期用户登录时,服务器会返回全部页面信息,页面会全部刷新.AJAX出来后当我登录时,
    只需要局部刷新即可,无需服务器返回全面页面.因为登录前后有些HTML CSS是一样的)
</pre>

## 用 AJAX 请求 2.js 文件

<pre>
    既然我们知道AJAX的目的,那我们需要一个本地服务器做出响应。main.js做出请求
    index.html 视图化 2.js做出响应正文
    本地服务器代码：<a href="https://github.com/s-xiaoxiao/ajax-1/blob/master/server.js">服务器源码</a> 复制代码创建一个server.js文件粘贴

</pre>

### index.html

```HTML
<!doctype html>
<html>
  <head>
    <title>AJAX应用——请求JS文件
  </head>
  <body>
    <button id="getJs">Get 2.js</button>
  </body>
</html>
```

### 2.js

```JavaScript
console.log("hello world")
```

### main.js

```JavaScript
//在JS里由构造函数XMLHttpRequest创建的实例可以实现与服务器交互
getJs.onclick = ()=>{
  const request = new XMLHttpRequest()  //创建该构造函数的实例

  request.open("GET","/2.js")       //
  request.onreadystatechange=()=>{
    if(request.readyState === 4 && request.status ===200){
      const script = document.createElement("script")
      script.innerHTML = request.response
      document.body.appendChild(script)
    }
  }
  request.send()
}


//XMLHttpRequest 对象可以与服务器交互。你可以从url获取数据，而无需让整个页面刷新.这允许网页在不影响用户
//的操作情况下更新页面的局部更新。
//XMLHttpRequest.open():方法初始化一个请求。该方法要从JavaScript代码使用;
//XMLHttpRequest.onreadystatechange:该属性只要readyState属性发生变化,就会处理相应的处理函数。
//XMLHttpRequest.readyState: 属性返回一个XMLHttpRequest代理当前所处的状态。
//4代表下载操作已完成 0.代理创建 1.open已经被调用 2.send已经被调用 3.下载中 4.完成
//XMLHttpRequest.status: 属性返回XMLHttpRequest响应中的数字状态码。在请求完成前值为0出错也为0.
//200代表请求成功
//XMLHttpRequest.response:属性返回响应的正文.返回的类型可以是ArrayBuffer、blob、document、
//javaScript object或DOMString。
//XMLHttpRequest.send():方法用于发送HTTP请求。如果是异步请求(默认)。则此方法会在请求发送后立即返回;
//如果是同步请求,则此方法直到响应到达后才会返回
```

> 当你点击过后控制台会输出 hello world

以上只是讲诉了请求 js 文件的应用

以下源代码:<a href="https://github.com/s-xiaoxiao/ajax-1">code</a> 包含了请求 html、xml、json、css、script 文件以及应用。核心代码相同.

> 总结：一共分四步。创建构造函数 XMLHttpRequest 实例、调用实例的 open 方法、监听对象的 onreadystatechange 事件、调用实例的 send()方法
