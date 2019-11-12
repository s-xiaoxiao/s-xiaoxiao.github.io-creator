---
title: "JS 函数的执行时机"
date: 2019-11-03T11:10:36+08:00
draft: false
---
## 1. 为何以下代码打印出6个6？
```JavaScript
let i=0

for(i=0;i<6;i++){
  setTimeout(()=>{console.log(i)},0)
}
/*
  输出：
        6
        6
        6
        6
        6
        6
*/
```
1. 首先 let(块级作用域)定义一个i变量初始值为0。在for里循环,setTimeout()方法是一个定时器，该定时器到期后会执行指定的代码.延迟以毫秒计算1000毫秒=1秒,默认值是0(为0时也不会在第一代码执行。)).


> 我们首先了解到有一个循环,和一个定时器。在整个环境中i在全局范围内.我们该如何理解最后输出6个6。首先分两个部分,第一代码执行,第二执行其余代码.for循环在第一代码执行但是到包含了setTimeout()，发现它是一个定时器而且延迟为0毫秒在执行,那就放到第二代码执行去,现在剩下的代码执行完毕再说.setTimeout()在第二代码执行.因为在第一代码执行后 i因for值变成6。所以,在第二代码执行时setTimeout函数去调用i时,i的值为6

什么是第一代码执行：预先先写的,不需要交互<button>点击我没反应</button>

什么是第二代码执行：当我需要交互时(点击时)
<button onclick="javascript:{setTimeout(function(){alert('你好,我是setTimeout')},3000)}">点击后3秒有弹窗</button>

所有 你理解了吗？为什么当在第二执行setTimeout时会输出6而不是0 1 2 3 4 5 呢？



## 2. 当let配合for一起时,结果令人感到正常？
```JavaScript
for(let i=0;i<6;i++){
  setTimeout(()=>{console.log(i)},0)
}

/*
  输出：
        0
        1
        2
        3
        4
        5
*/
```

> 为什么let写在for里面结果就不一样呢？主要有个细节,当在第一代码执行setTimeout()时把它放到第二代码执行时i的当前值被保存在setTimeout()函数里了。


## 3. 使用递归配合setTimeout()输出0-5

```JavaScript

function fn(i){
    return i<6?setTimeout(()=>{console.log(i);fn(++i),0)}:i
}

fn(0)
/*
    输出：
          0
          1
          2
          3
          4
          5
*/

```

> 如果你不明白递归先自行了解。如果你知道递归,你就明白每次递时i的值都是不同的。