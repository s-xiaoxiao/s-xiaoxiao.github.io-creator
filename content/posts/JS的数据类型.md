---
title: "JS的数据类型"
date: 2019-10-16T14:10:36+08:00
draft: false
---

# 预知：

在本文章中,作者只是把已知的数据类型做一些归类整理,仅供复习参考。

## 前言
> 在程序的世界里,数据是很重要的.程序的目的就是为了处理数据.我们会一眼区别出数字不是字母.但是程序是怎么知道的呢？是程序员标明的。计算机才知道这个是数字的,那个是字母对不对?接下来我们看看在JavaScript里有哪些数据类型


## 1. 数字(number)
在现实里我们用数字来计算的数在JS里都算数字类型。但是我们常用十进制来计算。在程序还有二进制、八进制、十进制、十六进制。还有正0、负0。它们都是0.还有一些无穷大、正无穷大、负无穷大。以及不是数的数(Not a Number(简称NaN))，在JS里比如0/0它的结果一定不是数但是属于数字类型,接下来我们用代码看看吧

<strong>写法</strong>
``` JavaScript
let integer=10        //整数    表示  10 
let decimals=10.1     //小数    表示  10.1(如果是10.0会自动忽略0)
let binary=0b1010     //二进制  表示  10
let octonary=0o12     //八进制  表示  10
let hex=0xa           //十六进  表示  10
```
<strong>特殊值</strong>
```JavaScript
let positiveNumber = +0       //正0等于0
let negativeNumber = -0       //负0等于0 (有符号意义不同)
let positiveInfinity = 1/0    //等于正无穷大
let negativeInfinity = 1/-0   //等于负无穷大
let NotaNumber = 0/0          //等于NaN(不是数的数(Not a Number)))
```


## 2. 字符串(string)

电脑是怎么理解我现在写的这句话是字符串而不是数字的？来看下面代码吧

```JavaScript

let SingleQuotedString = 'hello world'  //单引号表示字符串(一般单引号表示单个字符))
let DoubleQuotedString = "hello world"  //双引号表示字符串(一般双引号表示字符串)
let BackquteString = `hello world`      //反引号
```
当我们写字符串的时候第一个'或"是字符串的开始第二个'或"代表结束,如果我们需要在''里包含'或""包含"该怎么做?我们可以做一下转义(用斜杠\表示(意思是后面的符号不是原来的意思,改变一下当前环境里的意
思)))

<strong>转义:</strong>

```JavaScript
\'    //表示单引号
\"    //表示双引号
\n    //表示换行
\r    //表示回车
\t    //表示tab制表符
\\    //表示反斜杠
\uFFFF//转义数字形式表示Unicode字符(由16位组成)
\xFF  //转义数字形式表示前Unicode 256个字符
```
注:Unicode(简称:万国码)90年研发,94年发布.是一个字符集.它存储所有国家的文字

    你有可能有点疑惑了,既然单引号能表示字符串为什么还要双引号来甚至是反引号呢?
    因为问题来了呀,如果文中带有单引号或双引号呢?那我要是写多行字符串呢?程序是怎么知道是一行的?

```JavaScript
"it's ok"       //文中带有单引号就用双引号括起来(无需转义)
`"我是个引用"`   //文中带有双引号就用反引号括起来(无需转义)
`我是多行字符串
我是多行字符串
我是多行字符串`  //多行字符串用反引号
```



## 3. 布尔(bool)

布尔只有两个值,用来判断是否为真或为假

true为真

false为假

1是真还是假 ? 0 呢? :1为真 0为假

还有相当于false但又不是false:undefined null 0 NaN ''(空字符串)

怎么知道的呢?用if来判断一下

```JavaScript
if(undefined){
  console.log('true')
}else{
  console.log('false')
}
```
注意:打印出false不代表为false.你可以用它们跟false来===比一下
```JavaScript
undefined === false   //false
null === false        //false
0 === false           //false
NaN === false         //false
'' === false          //false
```
## 4. undefined
字面意义为未定义,我们可以把已知的一些定义在定义前称为未定义(一般是在非对象上))
```JavaScript
var a=1   //数字类型 a为1
var b     //未定义,不知其为何类型
```
## 5. null
空,为什么大家都有对象(此对象不是男女朋友哦),而你没有.(一般null是用在判断是否为空对象的一个解释)

``` JavaScript
var a={
  name:frank;     //这不是一个空对象
}

var b={
                  //啥都没有,空对象一个
}
```
## 6. object

大家好我是对象,我有些复杂.我想特殊一点,用一篇文章来单独介绍我


## 7. 符号(symbol)

先声明一下,这个数据类型真的太不常用了呢. 为什么呢?它会当前环境当中生成一个唯一的一个值!
``` JavaScript
var m = {
  a:Symbol(),
  b:Symbol()
}

m.a === m.b  //false 
```
为什么不一样?因为它自动你生成是唯一的值哦(当前全局环境当中)