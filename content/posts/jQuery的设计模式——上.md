---
title: "jQuery的设计模式——上 "
date: 2019-12-05T20:05:36+08:00
draft: false
---

> 前言：上篇 blog 我们用对象的风格进行了 DOM 操作的封装,跟原生 DOM API 操作相比很明显在调用时极大的减少开发者的时间,这次我们用著名 jQuery 设计模式来重新封装 DOM 操作(称之为链式风格)。

在你阅读本篇之前谨记三点：

1. 第一次出现的 API 会注解,当第二次时还不明白不要回头看。去 MDN 里搜索相关 API
2. 整个代码运行过程在最后有注释,不过先不要去看.思考这个部分谁都不能代替,莫要自欺欺人。最好能形成一个逻辑写一遍
3. 如果你看了代码运行过程的注释,记住一定要要自己照着自己的逻辑写一遍,没写顺在看注释.再形成逻辑.第三遍还没顺,再看代码

## 1. 添加全局函数

```JavaScript
//下面时一个函数,请注意
window.jQuery = (selectorOrArray){      //参数有可能是单个选择器或多个元素(数组形式)
  let elements                          //保存元素or多个元素(数组)
  if(typeof selectorOrArray === 'string'){
    elements = document.querySelectorAll(selectorOrArray)
  }else if(selectorOrArray instanceof Array){
    elements = selectorOrArray
  }

  return {

  }
  //注意了！本函数返回一个对象！我们会在对象实现addClass、find、each、parent、child、print、end

  //document.querySelectorAll() :查找指定选择器的全部元素
  //typeof ：返回其类型
  //instanceof:判断是否属于该构造函数的实例
}
```

> 当我们在调用 jQuery 时,会返回一个对象！并且根据指定选择器来寻找元素并且赋值给 elements 变量。

## 2. addClass():添加类名

```html
<!DOCTYPE html>
<html>
  <head>
    <title>jQuery</title>
  </head>
  <body>
    <div class="test"></div>
  </body>
</html>
```

需求：给 div 添加一个 red 的类名

```JavaScript
jQuery(".test")
//调用时返回一个对象,elements变量保存类名为.test的元素 我们只要在返回的对象里实现一个函数即可

window.jQuery = (selectorOrArray){
  let elements
  if(typeof selectorOrArray === 'string'){
    elements = document.querySelectorAll(selectorOrArray)
  }else if(selectorOrArray instanceof Array){
    elements = selectorOrArray
  }

  return {
    //添加一个类名
    addClass(className){
      for(let i=0;i<elements.length;i++){
        elements[i].classList.add(className)
      }
      return this;
    }
    //Element.classList:是一个只读属性,返回一个元素的类属性的实时DOMTokenList集合
    //Element.classList.add :添加指定的类值.如果这些类已经存在于元素的属性中,那么它们将被忽略
    //this ：在函数里简单调用来讲谁调用了这个函数this就指向了谁.由此可见是addClass是无名对象的属性
    //那么在函数用this时,实际是一个对象.
  }
}

jQuery(".test").addClass('red');  //ok 完成！我们给每个类名里有test的在添加一个类名为red

//我们来解释一下,我们知道第一次调用jQuery时返回一个对象,这个对象里有个addClass的方法,
//用.(属性访问器)来访问这个方法给元素添加类名最后return this. 这个this是包裹这个addClass属性的对象
//为什么返回一个对象？就跟第一次调用jQuery一样,返回一个对象是让我们再次调用addClass属性

```

## 3. find():在某元素下找到指定子元素

```html
<!DOCTYPE html>
<html>
  <head>
    <title>jQuery</title>
  </head>
  <body>
    <div class="test">
      <div class="child">child1</div>
      <div class="child">child2</div>
      <div class="child">child3</div>
    </div>
  </body>
</html>
```

> 需求:找到 .test 的子节点类名为 .child 并且添加一个类名为 red

```javaScript
jQuery('.test').find('.child').addClass('red')
//上述代码写好,现在我们来看看find()函数实际代码是怎么写的

window.jQuery = (selectorOrArray){
  let elements
  if(typeof selectorOrArray === 'string'){
    elements = document.querySelectorAll(selectorOrArray)
  }else if(selectorOrArray instanceof Array){
    elements = selectorOrArray
  }

  return {
    //添加一个类名
    addClass(className){
      for(let i=0;i<elements.length;i++){
        elements[i].classList.add(className)
      }
      return this;
    },
    // 在某元素下找到指定子元素
    find(selector){
      let array = []
      for(let i=0;i<elements.length;i++){
        array = array.concat(Array.from(elements[i].querySelectorAll(selector)))
      }
      array.oldApi = this
      return jQuery(array)
    }
    //Array.concat : 方法用于合并两个或多个数组。此方法不会改变现在数组，而是返回一个新数组
    //Array.from() : 方法从一个类似数组或可迭代对象创建一个新的,浅拷贝的数组实例
    //array.oldApi = this ;此时我们已经知道this 是什么了,那这一步是什么意思呢？我们知道在调
    //用jQuery并且实参为array后,elements会等于array这个数组.就不再是.test了。如果我们想让
    //elements为.test怎么办？调用一个函数(后面会写一个end函数)里elements.oldApi即可
  }
}

jQuery('.test').child('.child').addClass('red');
//第一次调用jQuery时返回一个对象(包含find()方法和addClass()方法),我们再次调用child寻找含有
//.child的类名.返回一个对象(还是含有find()方法和addClass()方法),最后调用addClass给类名为child
//的添加一个类名为red的
```

## 4. each():遍历每个元素,并且执行一个函数

```html
<!DOCTYPE html>
<html>
  <head>
    <title>jQuery</title>
  </head>
  <body>
    <div class="test">
      <div class="child">child1</div>
      <div class="child">child2</div>
      <div class="child">child3</div>
    </div>
  </body>
</html>
```

> 需求：遍历.test 里的.child 元素

```JavaScript
jQuery('.test').find('.child').addClass('red').each((n)=>{console.log(n)})
//上述代码写好,现在我们来看看each()函数实际代码是怎么写的

window.jQuery = (selectorOrArray){
  let elements
  if(typeof selectorOrArray === 'string'){
    elements = document.querySelectorAll(selectorOrArray)
  }else if(selectorOrArray instanceof Array){
    elements = selectorOrArray
  }

  return {
    //添加一个类名
    addClass(className){
      for(let i=0;i<elements.length;i++){
        elements[i].classList.add(className)
      }
      return this;
    },
    // 在某元素下找到指定子元素
    find(selector){
      let array = []
      for(let i=0;i<elements.length;i++){
        array = array.concat(Array.from(elements[i].querySelectorAll(selector)))
      }
      array.oldApi = this
      return jQuery(array)
    },
    //遍历每个元素,并且执行一个函数
    each(fn){
      for(let i=0;i<elements.length;i++){
        fn.call(null,elements[i],i)
      }
      return this
    }
    //Function.prototype.call :方法使用一个指定的this值给单独给出的一个或多个参数来调用一个函数
  }
}

jQuery('.test').find('.child').addClass('red').each((n)=>{console.log(n)})
//调用each时,实参是一个函数,在each()函数内被调用时传进来什么就用console.log()打印到控制台里

```

## 5. parent():找到父节点

```html
<!DOCTYPE html>
<html>
  <head>
    <title>jQuery</title>
  </head>
  <body>
    <div class="test">
      <div class="child">child1</div>
      <div class="child">child2</div>
      <div class="child">child3</div>
    </div>
  </body>
</html>
```

```JavaScript
jQuery('.test').find('.child').addClass('red').each((n)=>{console.log(n)}).parent()
//上述代码写好,现在我们来看看parent()函数实际代码是怎么写的

window.jQuery = (selectorOrArray){
  let elements
  if(typeof selectorOrArray === 'string'){
    elements = document.querySelectorAll(selectorOrArray)
  }else if(selectorOrArray instanceof Array){
    elements = selectorOrArray
  }

  return {
    //添加一个类名
    addClass(className){
      for(let i=0;i<elements.length;i++){
        elements[i].classList.add(className)
      }
      return this;
    },
    // 在某元素下找到指定子元素
    find(selector){
      let array = []
      for(let i=0;i<elements.length;i++){
        array = array.concat(Array.from(elements[i].querySelectorAll(selector)))
      }
      array.oldApi = this
      return jQuery(array)
    },
    //遍历每个元素,并且执行一个函数
    each(fn){
      for(let i=0;i<elements.length;i++){
        fn.call(null,elements[i],i)
      }
      return this
    },
    //找父节点
    parent(){
      const array = [] //存放父节点
      this.each(node =>{
        if(array.indexOf(node.parentNode) === -1){
          array.push(node.parentNode)
        }
      })
      array.oldApi=this
      return jQuery(array)
    }
  //Array.prototype.indexOf(): 方法返回在数组中可以找到一个给定元素的第一个索引,如果不存在，则返回-1
  //Array.prototype.push() :方法将一个或多个元素添加到数组的末尾,并返回改数组的新长度
  //Node.parentNode:返回指定的节点在DOM树中的父节点
}

jQuery('.test').find('.child').addClass('red').each((n)=>{console.log(n)}).parent()
//调用parent时用array存当前所有元素的父节点,并且用array数组为实参去调用jQuery().再次返回一个对象
```

## 6. child():获取儿子

```html
<!DOCTYPE html>
<html>
  <head>
    <title>jQuery</title>
  </head>
  <body>
    <div class="test">
      <div class="child">child1</div>
      <div class="child">child2</div>
      <div class="child">child3</div>
    </div>
  </body>
</html>
```

> 需求:找到某元素的子节点

```JavaScript
jQuery('.test').find('.child').addClass('red').each((n)=>{console.log(n)}).parent().child()
//上述代码写好,现在我们来看看child()函数实际代码是怎么写的

window.jQuery = (selectorOrArray){
  let elements
  if(typeof selectorOrArray === 'string'){
    elements = document.querySelectorAll(selectorOrArray)
  }else if(selectorOrArray instanceof Array){
    elements = selectorOrArray
  }

  return {
    //添加一个类名
    addClass(className){
      for(let i=0;i<elements.length;i++){
        elements[i].classList.add(className)
      }
      return this;
    },
    // 在某元素下找到指定子元素
    find(selector){
      let array = []
      for(let i=0;i<elements.length;i++){
        array = array.concat(Array.from(elements[i].querySelectorAll(selector)))
      }
      array.oldApi = this
      return jQuery(array)
    },
    //遍历每个元素,并且执行一个函数
    each(fn){
      for(let i=0;i<elements.length;i++){
        fn.call(null,elements[i],i)
      }
      return this
    },
    //找父节点
    parent(){
      const array = [] //存放父节点
      this.each(node =>{
        if(array.indexOf(node.parentNode) === -1){
          array.push(node.parentNode)
        }
      })
      array.oldApi=this
      return jQuery(array)
    },
    //找到子节点
    child(){
      let array = []
      this.each((node)=>{
        array.push(...node.children)
      })
      array.oldApi = this
      return jQuery(array)
    }
    //... 扩展运算符：该运算主要用于函数调用,该运算符将一个数组变为参数序列
}

jQuery('.test').find('.child').addClass('red').each((n)=>{console.log(n)}).parent().child()
//是的,这次跟上次差不多.child()函数把子节点放在array里之后再次调用jQuery返回一个对象
```

> 我们目前就差 print 和 end 没写了,不过主要功能已经写完了下面我们把两个函数一起写完

## 7. print():输出 elements and end():返回上一次 oldApi

```html
<!DOCTYPE html>
<html>
  <head>
    <title>jQuery</title>
  </head>
  <body>
    <div class="test">
      <div class="child">child1</div>
      <div class="child">child2</div>
      <div class="child">child3</div>
    </div>
  </body>
</html>
```

> 需求:打印当前的节点,和返回上一次的 oldApi 环境

```JavaScript
window.jQuery = (selectorOrArray){
  let elements
  if(typeof selectorOrArray === 'string'){
    elements = document.querySelectorAll(selectorOrArray)
  }else if(selectorOrArray instanceof Array){
    elements = selectorOrArray
  }

  return {
    //添加一个类名
    addClass(className){
      for(let i=0;i<elements.length;i++){
        elements[i].classList.add(className)
      }
      return this;
    },
    // 在某元素下找到指定子元素
    find(selector){
      let array = []
      for(let i=0;i<elements.length;i++){
        array = array.concat(Array.from(elements[i].querySelectorAll(selector)))
      }
      array.oldApi = this
      return jQuery(array)
    },
    //遍历每个元素,并且执行一个函数
    each(fn){
      for(let i=0;i<elements.length;i++){
        fn.call(null,elements[i],i)
      }
      return this
    }
  },
  //找父节点
  parent(){
    const array = [] //存放父节点
    this.each(node =>{
      if(array.indexOf(node.parentNode) === -1){
        array.push(node.parentNode)
      }
    })
    array.oldApi=this
    return jQuery(array)
  },
  //找到子节点
  child(){
    let array = []
    this.each((node)=>{
      array.push(...node.children)
    })
    array.oldApi = this
    return jQuery(array)
  },
  //输出当前节点
  print(){
    console.log(elements)
    return this
  },
  //返回上一次oldApi
  end(){
    return elements.oldApi
  }
}

jQuery('.test').find('.child').addClass('red').each((n)=>{console.log(n)}).parent().child().print()
//print()会把当前child数组打印出来。
jQuery('.test').find('.child').addClass('red').each((n)=>{console.log(n)}).parent().child().print().end()
//end() return elements.oldApi;我们为什么保存array.oldApi = this？？就为了我们可以找到父节点、子节点、
// 之后对它们进行操作,并且操作完后可以返回上一次环境
```

> 相关函数已经写完了,每次调用之后再次调用......这就是链式风格。在 jQuery 设计模式下我们使用\$符号代替 jQuery。切近！莫让 ta 人帮你思考
