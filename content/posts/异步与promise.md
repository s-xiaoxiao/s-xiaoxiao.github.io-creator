---
title: "异步与promise "
date: 2019-12-19T10:57:36+08:00
draft: false
---

> 前言:本篇文章讲诉异步 回调 promise 一些概念以及在使用 AJAX 时,我们需要 promise 构造函数提供的对象来监听 AJAX 的成功与失败等

## 异步(Async)

<pre>
    异步: 当我需要在1分钟内才能拿到炒饭,在这1分钟内我可以做一些其他的东西.1分钟之后有人告诉我你的炒饭好了.
    前两句话是异步操纵(我可以每隔15秒问一下我的饭好了吗),后一句是回调操作
    同步：当我需要在1分钟内才能拿到炒饭,1分钟就拿到炒饭了(中途无法分心,中途无法过问)
</pre>

## 回调(call back)

<pre>
    回调：当你早上去买热干面时,本该先付钱再给我面就行(好香)。但是师傅问你,弄好了要不要辣(这就是回调)
</pre>

```JavaScript
let 要辣 = ()=>{
    console.log("热干面+加辣椒 好了")
}

let 热干面= (额外配料) => {
    console.log("煮面")
    console.log("捞面装碗")
    console.log("基本配料")
    额外配料()
}

热干面(要辣);

//热干面函数返回undefined  要辣函数输出预期的结果。要辣函数返回的结果比热干面函数要早。
//这是异步的特征。同时要辣函数也是一个回调函数
```

> 异步任务不能直接拿到结果(每个人口味不一样),所以传给一个回调函数给异步。异步完成调用回调函数

问题来了！我不要辣椒,我不要酸豆角,我希望麻酱不要放少了！萝卜多放点等等。。每天那么多客人我怎么我不能每次都问一遍吧？(要写多少个回调函数？或者回调函数的参数得写多少个?)这得多麻烦阿。效率低下！那怎么办?办法就是基本配料师傅来放,其他口味的配料让客人自助 ok 解决了(这就是我们今天说的主角 promise 具有类似得思想)

## Promise

> Promise ：对象用于表示一个异步操作的最终完成(或失败),及其结果值

我们以 AJAX 得封装为例来介绍 不用和用 Promise 的用法

```JavaScript
//不用promise
let ajax =(method,url,options)=>{
  const{success,fail} = options
  const request = new XMLHttpRequest()
  request.open(method,url)
  request.onreadystatechange = ()=>{
      if(request.readyState ===4){
        //成功调用success 失败调用 fail
        if(request.status<400){
          success.call(null,request.response)
        }else if(request.status>=400){
          fail.call(null,request.response)
        }
      }
  }
  request.send()
}

ajax('get','/xxx',{success(response){},fail:(request,status)=>{}})
```

```JavaScript
//用Promise
ajax =(method,url,options)=>{
  return new Promise((resolve,reject)=>{
    const {success,fail}=options
    const request = new XMLHttpRequest()
    request.open(method,url)
    request.onreadystatechange = ()=>{
      if(request.readyState===4){
        if(request.status<400){
          resolve.call(null,request.response)
        }else if(request.status >=400){
          reject.call(null,request)
        }
      }
    }
  })
}
//Promise 对象用于表示一个异步操作的最终完成(或失败),其结果值。接收一个参数该参数为一个函数并且接收两个
//参数名为resolve,reject。Promise 构造函数执行时立即调用该参数,该参数的两个参数resolve、reject作为函数传
//给该参数。resolve和reject执行时会修改Promise的状态改为fulfilled和rejected。

//Promise：并不是立即返回最终执行结果,而是返回一个能代表未来出现结果Promise对象。由于返回的对象我们可以
//进行链式操作。一个Promise有以下几种状态
//pending：初始状态，既不是成功，也不是失败状态
//fulfilled：意味着操作完成
//rejected：意味着操作失败

//初始值为pending状态的promise对象可能会变成fulfilled状态并传递一个值给相应的状态处理方法，也可能变为失败
//状态rejected并传递失败信息。当中其一中情况发生时,promise对象的.then方法绑定的处理方法(handlers)就会被调
//用(then方法包含两个参数：onfulfilled和onrejected,它们都是Function类型。当promise状态为fulfilled时,
//调用then的onfulfilled方法，当Promise状态为rejected时，调用then的onrejected方法，所以在异步操作的完成
//和绑定处理方法之间不存在竞争)因为它们都返回一个promise对象
```

这样有可能一时难以理解的话下面一段代码要记住!因为它返回的是一个 promise 对象可以链式操作

```JavaScript
return new Promise((resolve,reject)=>{})

//最后一步写成功时回调函数和失败时回调函数

.them(success,fail) //传入成功和失败函数

//先到这里promise的其他用法其他本章介绍
```

> 目前最新的 AJAX 的库是 axios
