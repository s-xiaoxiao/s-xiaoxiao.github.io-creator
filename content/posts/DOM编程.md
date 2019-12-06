---
title: "DOM编程"
date: 2019-11-28T15:09:36+08:00
draft: false
------
title: "DOM编程"
date: 2019-11-28T15:09:36+08:00
draft: false
------
title: "DOM编程"
date: 2019-11-28T15:09:36+08:00
draft: false
---

# 前言

```JavaScript
//在进行认识DOM开始之前呢,希望你有如下知识:
1. JS语法,如变量、if else、循环
2. JS的其中数据类型: Number String Bool Symbol Null Undefined Object
3. 五个falsy值: 0  NaN '' null undefined
4. 函数是对象,数组也是对象
5. div和span标签
6. Css布局：flex
```

> 什么是 falsy 值？ 在 Bool 上下文中认定为 false 的值

## 1. 什么是 DOM(Document Object Model 文档对象模型)

1 . 网页里的各种标签按照数据结构来看是一个树的结构.(Document)

```HTML
<!doctype html>
<html>
  <head>
    <title>我是第三层</title>
  </head>
  <body>
    <div>我是第三层</div>
  </body>
</html>

<!-- 是不是很想树的结构？-->
```

2 . JS 如何操作这棵树？(Object)

浏览器往 window 上加一个 document 即可,JS 用 document 操作网页。这就是 Document Object Model 文档对象模型

不过有个事实是自带的 API 有点难用,或者你认为 DOM 有点傻呢？不奇怪,下次文章解答你的疑惑

> 至于模型两字(Model)应该还有其他的模型以作区分.

## 2. 如何获取元素?(标签)

```JavaScript
//标签必须有一个id的属性和属性值,可以通过以下API获取到

1. window.id或者id                     //通过ID值找到标签
//实例(假设id值是kw)：
    window.kw //可以获取到这个元素
    kw        //也可以获取到这个元素

2. document.getElementById()     //通过ID值找到标签
//实例
    document.getElementById('kw')

3. document.getElementsByTagName()   //通过标签name去找,返回是一个数组
//实例
    document.getElementsByTagName('div)

4. document.getElementsByClassName() //通过class属性值来寻找标签,返回是一个数组
//实例
    document.getElementsByClassName('red')document.querySelectorAll()

5. document.querySelector() //放回文档中与指定选择器或选择器组匹配的第一个HTML元素element。找不到返回
//null

//实例
    document.querySelector('#wrapper') //带#是找id的值,(符合有效的CSS选择器字符串即可)

6. document.querySelectorAll() //返回与指定的选择器匹配的文档中的元素列表。返回的是NodeList(节点列表及
//数组)

//实例
    document.querySelectorAll('.red')[0]
```

> 共有 6 个。该怎么用呢？工作上如果要兼容 IE 用 2 3 4.不兼容用 5 6.自己写 demo 可以用 1

## 3. 如何获取特定元素?(标签)

```JavaScript
//获取html元素
document.documentElement

//获取head元素
document.head

//获取body元素
document.body

//获取窗口(窗口不是元素)
window

//获取所有元素
document.all      //它是第六个falsy值. 返回一个数组包含了所有标签.
```

## 4. 追究元素本质

由于我们已知如何获取元素并且已知 DOM,文档对象模型。每个元素都是一个对象吗?是的.我们拿一个来看看它的原型

```JavaScript
let div = document.getElementsByTagName('div')[0]
console.dir(div)     //

//可以看出
div.__proto__ === HTMLDivElement.prototype    //div共有属性

HTMLDivElement.__proto__ === HTMLElement.prototype  //html共有属性

HTMLElement.__proto__ === Element.prototype   //xml html标签的共有属性

Element.__proto__ === Node.prototype          //节点共有属性,该元素是是标签还是文本？

Node.__proto__ === EventTarget.prototype      //一些事件

EventTarget.__proto__ === Object.prototype    //对象的共有属性
```

> 可以看出原型链的终点是 object.prototype 所以 div 是一个对象

什么是节点？元素？标签？

```JavaScript
//本质上区分元素,标签,节点
<div></div>             //这是一个div标签(tag),
<div>hello world</div>  //如果加上内容等等其他的是一个元素(element)
//节点呢？显然单独无法区别出来,待我们把整个网页整体来看,形成一个树状时,每个点是一个节点,在
//div原型链上个有个Node对象,它是用来干嘛的？区分这个节点它是element(为1)还是text(hello world)(3)和其他

let div = document.getElementsByTagName('div')[1]  //上面第二个标签
console.log(div.nodeType)               //1 是element
console.log(div.firstChild.nodeType)    //3 是text
```

## 5. 节点的增删改查

1. 增

```JavaScript
//创建一个标签节点
let div1 = document.createElement('div)
document.createElement('style')
document.createElement('script')
document.createElement('li')

//创建一个文本节点
text1 = document.createTextNode('你好')

//标签插入文本
div1.appendChild(text1)
div1.innerText = '你好' or div1.textContent = '你好'

//当前创建的标签节点和文本节点处于 JS 线程中,你必须把它放到 head 或 body 里面它才生效
document.body.appendChild(div)
已在页面中的元素.appendChild(div)

//一个节点只能在页面中出现一次如果想出现两次需要复制一份,可以调用cloneNode()API
div2 = div1.cloneNode(true)

```

    Node.cloneNode() 方法 ：接收一个参数,如果为 true 则该节点的所有后代都会克隆,如果 false 则只会克隆该节点本身

2. 删

```JavaScript
//有两种方法删除元素
let div = document.createElement('div')
document.body.appendChild(div)

div.parentNode.removeChild(div)     //旧方法
// Node.parentNode()方法 : 返回该指定节点在DOM树中的父节点
// Node.removeChild()方法：从DOM中删除一个子节点.返回删除的节点

document.body.appendChild(div)

div.remove()                        //新方法
//ChildNode.remove()方法：把对象从它所属的DOM树中删除
```

    > 该删除的方法仅仅只是把元素从页面中删除了,

3. 改

```JavaScript
let div = document.createElement('div')     //创建一个div
document.body.appendChild(div)              //在body里最后添加一个子元素,
//改id
div.id = div

//改class
div.className ='red'  //如果class有属性值需要改成 +=。这是旧方法
div.classList.add('green')    //新方法,直接添加

//改style
div.style = ' color:red'   //这种方法会覆盖原有的,不推荐
div.style.color = 'red'   //即可,其他同理
div.background-color = 'black'    //带有杠的这种变成下面这种
div.backgroundColor = 'black'

//改指定某个属性值
div.setAttribute('data-x','text')
setAttribute() //该方法设置指定元素上的某个属性值。如果属性已经存在,则更新该值;否则,
//使用指定的名称和值添加一个新的属性

//如何得到上面的值？
div.getAttribute('data-x')
div.dataset.x       //该方法的前提属性一定是data-开头才可以
div.dataset.x = 'xiaoxiao'   //也可以直接改

//在读标准属性的的时候有两种方法
div.classList / a.href      //第一种
div.getAttribute('class') / a.getAttribute('href')    //第二种

// 为什么有这种呢？因为有时a的标签不是跳转到其他域名还是跳转其他子页面href="/xxx"
//要是还是用 a.href会得到当前域名+a.href

//改事件处理函数(div.onclick默认为null)
//如果把div.onclick改为一个函数fn,那么点击div的时候,浏览器就会调用这个函数,
//并且是这样调用的fn.call(div,event)  div会被当成this event包含了点击事件的所有信息

div.onclick = function (x){
  console.log(this)           //如果写成匿名函数在点击时浏览器会传进去,this === div
  console.log(x)              // x === event
}
```

```JavaScript
let div = document.createElement('div')
let div2 =div.cloneNode(true)            //深拷贝(当前节点+后代所有节点)false是浅拷贝(当前节点)
document.body.appendChild(div)
// 改内容
div.innerText = 'hi'
div.textContent = 'hi'

//改HTML内容
div.innerHTML = '<string>重要内容</string>`

//改标签
div.innerHTML = ''    //清空
div.appendChild(div2)
```

4. 查

```JavaScript
let div = document.createElement('div')
document.body.appendChild(div)

//查自己
div

//查父节点
div.parentNode      //返回指定的节点在DOM树中的父节点
div.parentElement   //返回当前节点的父元素节点,如果该元素没有父节点,或者父节点不是一个DOM元素,
//则返回null

//查父节点的父节点
div.parentNode.parentNode   //以此类推

//查子节点
div.childNodes //返回包含指定节点的子节点的集合,该集合为即时更新的集合有空格会也为一个子节点(text)

div.children   //返回一个Node的子elements(元素)

//查所有相邻节点(没有API可以调)
//思路是:找到父节点所有的子节点,排除自己,得到所有的相邻节点
let siblings = []
let c = div.parentNode.children
for(let i=0;i<c.length;i++>){
    if(c[i]!==div){
      siblings.push(c[i])
    }
}

```

```JavaScript
let div = document.createElement('div')
document.body.appendChild(div)

//查老大
div.parentNode.children[0]
div.firstChild        //返回树中节点的第一个子节点,如果节点是无子节点,则返回null

//查老幺
div.lastChild         //返回当前节点的最后一个子节点
//如果父节点为一个元素节点,则子节点通常为一个元素节点,或一个文本节点,或一个注释节点.如果没有节点
// 则返回null

//查前一个兄弟节点
div.previousSibling   //返回当前节点的前一个兄弟节点,没有返回null

//查后一个兄弟节点
div.nextSibling       //返回其父节点的childNodes列表中紧跟在其后面的节点,如果指定节点是最后一
// 个节点则返回null

//上面两个查前和后一个兄弟节点,如果前后的节点是text节点或其他的话,
//返回的就不是element节点下面两个API是返回element节点的
div.previousElementSibling
//返回当前元素在其父元素的子元素节点中的前一个元素节点,如果该元素已经是第一个元素节点,则返回null

div.nextElementSibling
//返回当前元素在其父元素的子元素节点中的后一个元素节点,如果该元素是最后一个元素节点,则返回null
```

      遍历一个 div 元素里所有元素

      ```JavaScript
      let div = document.createElement('div')
      document.body.appendChild(div)
      let travel = (node,fn) =>{
        fn(node)
        if(node.children){
          for(let i=0;i<node.children.length;i++){
            travel(node.children[i],fn)
          }
        }
      }
      travel(div,(node)=>{console.log(node)})
      ```

## 6. DOM 操作是跨线程的

前言：JS 引擎和渲染引擎不是一个引擎它们是独立工作的,那桥梁是谁？浏览器

```JavaScript
let div = document.createElement('div')
//上述代码由JS引擎去做的,存在内存里

document.body.appendChild(div)
//上述代码是往页面body里添加一个div标签由JS引擎 浏览器 渲染引擎共同工作

//经过上述代码.那它们之间是怎么运行的?
```

> 当浏览器发现 JS 在 body 里面加了个 div 对象,浏览器就会通知渲染引擎在页面里也新增一个 div 元素新增的 div 元素所有属性都照抄上述代码。这就是跨线程的操作

我们直到一个 div 标签不仅仅是个标签,它还有属性呢.我们用 JS 去操作它的属性会怎样呢？

```JavaScript
let div = document.createElement('div')
document.body.appendChild(div)

//我们知道上面是JS引擎和浏览器以及渲染引擎共同工作才能完成,那下面的属性呢？

div.id = 'xiaoxiao'         //标准属性会同步修改
div.title = 'new title'     //标准属性会同步修改
div.x = 'x'                 //(页面标签先有自定义属性)非标准属性不会同步修改
div.data-x = 'xx'           //data-* 属性会同步修改

//已知标准属性和data-*属性会同步修改,自定义属性不会同步修改,如果要自定义属性同步修改加上前缀data-。
//用dataset来访问得到
```

假设我上下文代码同时修改一个元素的 width 会渲染两次吗？答：不会就想我欠你 20.你说两次我也只是还 20

最后一个问题,div 的属性在 JS 里和在标签里,是一样的吗？

```JavaScript
//答:标准属性和前缀data-是一样的,非标准属性就不一样了,
//还有在标签里属性的值只能是字符串,在js里是对象属性值不单单是一个字符串了,还是可以布尔值(boolean)
//我们怎么区分呢?
                //property属性是JS线程中所有属性叫做div的property.
                //attribute也是属性是渲染引擎对应标签的属性,叫做attribute
```

> see you later
