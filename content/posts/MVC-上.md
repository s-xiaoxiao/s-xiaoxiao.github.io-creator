---
title: "MVC-上 "
date: 2020-01-06T15:50:36+08:00
draft: false
---

# MVC (Model view controller)

    Model (数据模型) ：负责操作所有数据
    View (视图)：负责 ui
    controller (控制器) 负责操作数据在ui层可视化

简称 MVC 模式，这是一种软件架构模式。该模式的目的是在于对程序的修改和扩展简化。并且某一个部分可重复利用的可能。
本篇博客使用模块化创建四个小功能：
[在线预览](http://sunxiaochuang.top/MVC-demo-1/dist/index.html)

我们先实现这四个功能。
后期使用 MVC 模式重构该小 demo

## 目录

```
index.html       //主页面

main.js          //主页面行为 被index.html引用

global.css       //主页面布局 被main.js引用

reset.css        //主页面重置css 默认属性 被main.js引用

    //以下js文件被main.js引用。css文件被自身js文件引用
    app1.css     //功能1的css和js
    app1.js

    app2.css     //功能2的css和js
    app2.js

    app3.css     //功能3的css和js
    app3.js

    app4.css     //功能4的css和js
    app4.js
```

> 这是实现该 4 个功能全部的文件(这比以前在一个页面里写下全部代码好太多了,你应该学习这种方式)

### 开始前准备

1. 首先创建一个 mvc-demo1 文件里在创建一个 src 文件把上述文件 12 个全部放在里面即可
2. 使用 yarn 命令初始化:yarn init -y;
3. 使用 yarn 命令添加 jquery：yarn add jquery

ok 步骤完成。使用 parcel src/index.html 开启本地服务器在浏览器里查看

### 4 个主页面

1. index.html

```HTML
<div class="page">
  <section id="app1">
    <div class="output">
      <span id="number">100<span>
    </div>
    <div class="actions">
      <button id="add1"></button>
      <button id="minus1"></button>
      <button id="mul2"></button>
      <button id="divide2"></button>
    </div>
  </section>

  <section id="app2">
    <ol class="tab-bar">
      <li class="selected">1111</li>
      <li>2222</li>
    </ol>
    <ol class="tab-content">
      <li class="active">内容1</li>
      <li>内容2</li>
    </ol>
  </section>

  <section id="app3">
    <div class="square"></div>
  </section>

  <section id="app4">
    <div class="circle"></div>
  </section>
</div>
<script src="main.js"> </script>
```

2. main.js

```JavaScript
import './reset.css'
import './global.css'

import './app1.js'
import './app2.js'
import './app3.js'
import './app4.js'

// import:静态的import语句用于由另一个模块导出的绑定
```

3. global.css

```CSS
/*隐藏右侧下拉*/
body {
     overflow: hidden;
}
body > .page{
  display:flex;
  flex-wrap:wrap;
}
body > .page > section{
  width:50vw;
  height:50vh;
}
```

4. reset.css

```CSS
   *,*::before,*::after{
     box-sizing:border-box;
     padding:0;
     margin:0;
   }
   ol,ul{
     list-style:none;
   }
```

### 4 个功能页面

功能 1：加减乘除

1.  app1.css

```CSS
#app1{
  border:1px solid red;
}
```

2. app1.js

```JavaScript
import './app1.css'       //引入app1.css
import $ from 'jquery'    //引入jquery

const $number=$('#number'),
      $button1=$('#add1'),
      $button2=$('#minus1'),
      $button3=$('#mul2'),
      $button4=$('#divide2');

const n=localStorage.getItem('n')
$number.text(n||100)

$button1.on('click',()=>{
  let n = parseInt($number.text())
  $number.text(++n)
  localStorage.setItem('n',n)
})
$button2.on('click',()=>{
  let n = parseInt($number.text())
  $number.text(--n)
  localStorage.setItem('n',n)
})
$button3.on('click',()=>{
  let n = parseInt($number.text())
  $number.text(n*=2)
  localStorage.setItem('n',n)
})
$button4.on('click',()=>{
  let n = parseInt($number.text())
  $number.text(n/=2)
  localStorage.setItem('n',n)
})
//localStorage.getItem()：接受一个key得到本地存储key的value
//localStorage.setItem()：接受一个key和value存储到本地
//parseInt()：将一个字符串转换为radix进制的正数.radix在2-36之间.默认为10
```

功能 2：tab 切换

1.  app2.css

```CSS
#app2{
  border:1px solid green;
}
#app2 .tab-bar{
  display:flex;
  flex-wrap:wrap;
}
#app2 .tab-bar >li {
  border:1px solid grey;
  width:50%;
}
#app2 .tab-content >li{
  display:none;
}
#app2 .tab-content >li.active{
  display:black;
}
#app2 .tab-bar >li.selected{
  background:rgb(253,221,155)
  color:white;
}

```

2.  app2.js

```JavaScript
import './app2.css'
import $ from 'jquery'

const $tabBar = $('#app2 .tab-bar'),
      $tabContent = $('#app2 .tab-content')

$tabBar.on('click',(e)=>{
  const $li = $(e.currenTarget)
  const index = $li.index()

  $li.addClass('selected').siblings().removeClass('selected')

  $tabContent.children().eq(index).addClass('active').siblings().removeClass('active')
})

// e是event对象，事件处理函数在被处理时将event对象作为第一个参数
```

功能 3：移动

1.  app3.css

```CSS
#app3{
  border:1px solid purple;
}
#app3{
  border:1px solid grey;
  margin:10vw 0 0 10vw;
  width:10vw;
  height:10vw;
  transition:transform 1s;
}
#app3 .square.active{
  transform:translateX(15vw)
}
```

2.  app3.js

```JavaScript
import './app3.css'
import $ from 'jquery'

const $square = $('#app3 .square')

$square.on('click',()=>{
  $.square.toggleClass('active')
})
```

功能 4：渐变

1. app4.css

```CSS
#app4{
  border:1px solid pink;
}
@keyframes change{
  0%{
    background:red;
  }
  100%{
    background:blue;
  }
}
#app4 .circle{
  border:1px solid grey;
  width:20vw;
  height:20vw;
  border-radius:50%;
  margin:2vw 0 0 2vw;
}
#app4 .circle.active{
  animation:change 1s infinite alternate liner;
}
```

2. app4.js

```JavaScript
import './app4.css'
import $ from 'jquery'

const $circle = $('#app4 .circle')

$circle.on('mouseenter',()=>{
  $circle.addClass('active');
})

$circle.on('mouseout',()=>{
  $circle.removeClass('active')
})
```

现在所有小功能已经实现,现在你已经发现了模块化的好处。
MVC-中 见
