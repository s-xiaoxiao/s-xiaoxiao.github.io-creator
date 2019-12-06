---
title: "jQuery基本功能 "
date: 2019-12-06T20:20:36+08:00
draft: false
---

> 前言:本文章的目的是介绍 jQuery 的基本功能,如果你想了解 jQuery 基本的思路可以去看 jQuery 的设计模式上和下的 blog。希望能帮助到你

目录

1. <a href="#getEl">jQuery 如何获取元素</a>
2. <a href="#chain">jQuery 链式操作是怎样的</a>
3. <a href="#createEl">jQuery 如何创造元素</a>
4. <a href="#moveEl">jQuery 如何移动元素</a>
5. <a href="#alterAttr">jQuery 如何修改元素的属性</a>

# jQuery

jQuery 是一个高效、精简并且功能丰富的 JavaScript 工具库,jQuery 是目前使用最广泛的 JavaScript 库

## <h2 id="getEl">1. jQuery 如何获取元素</h2>

```HTML
<!doctype html>
<html>
  <head>
    <title>jQuery之获取元素</title>
  </head>
  <body>
    <div id="test"></div>
  </body>
</html>
```

```JavaScript
$('#test')        //利用id值来获取元素

//上述代码用ID的值去获取元素,$()函数执行完后,会返回一个对象。这个对象包含了对元素进行各种操作的方法
```

> jQuery 基本思想和主要用法：就是当我获取一个元素之后,我可以直接进行各种操作。jQuery 的构造函数简写为\$

## <h2 id="chain">2. jQuery 链式操作是怎样的</h2>

```HTML
<!doctype html>
<html>
  <head>
    <title>jQuery之链式操作</title>
  </head>
  <body>
    <div id="test">
      <div class="child">child1</div>
      <div class="child">child2</div>
      <div class="child">child3</div>
    </div>
  </body>
</html>
```

```JavaScript
$("#test").find(".child")   //在id为test里找到类名为child
```

> 这是主要思想和主要用法的体现之处.当我获取到一个元素时,可以直接进行操作。这就是链式操作

## <h2 id="createEl">3. jQuery 如何创造元素</h2>

```JavaScript
$(`<div id="test2"></div>`)      //你想创建什么元素,写开始标签结束标签以及属性内容。就会创造一个元素.
```

执行上述代码之后,创建一个 div 元素 ID 为 test2.并且返回一个对象

## <h2 id="moveEl">4. jQuery 如何移动元素</h2>

```HTML
<!doctype html>
<html>
  <head>
    <title>jQuery之移动元素</title>
  </head>
  <body>
    <div id="test">
      test1
      <div class="child">child1</div>
      test1
      <div class="child">child2</div>
      test1
      <div class="child">child3</div>
    </div>
  </body>
</html>
```

```JavaScript
$("#test").after($(".child"))

//.after():在匹配元素集合中的每个元素后面插入参数所指定的内容,作为其兄弟节点
```

执行上述代码之后类名为.child 跟 ID 名为 test 的作为兄弟元素

## <h2 id="alterAttr">5. jQuery 如何修改元素的属性</h2>

```HTML
<!doctype html>
<html>
  <head>
    <title>jQuery之修改元素的属性</title>
  </head>
  <body>
      <div class="child">child1</div>
      <div class="child">child2</div>
      <div class="child">child3</div>
    </div>
  </body>
</html>
```

```JavaScript
$(".child").attr('title','red');
//.attr():获取匹配的元素集合中的第一个元素的属性值。设置每一个匹配元素的一个或多个属性。
```

执行上述代码之后 child 都变成 red

> 在当前来说,jQuery 已经不是主流行了。但依然有很多老项目在使用。这个初步的认识有助于我们未雨绸缪.更多 API 请看<a href="https://www.jquery123.com/" target="_blank">jQuery 中文文档</a>

再见！
