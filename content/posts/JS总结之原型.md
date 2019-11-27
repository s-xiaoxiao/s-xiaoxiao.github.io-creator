---
title: "JS总结之原型"
date: 2019-11-13T13:51:36+08:00
draft: false
---

# 原型

```JavaScript
今天我们谈谈原型,当我们使用对象时,该对象有一个__proto__的属性问的就是它的属性值来自哪里?
我们目前只举例三种 Function  Object Array 带你了解过后可以自行推出其他

基于这三种公式

1. 对象.__proto__ === 其构造函数.prototype
  
    例1： let a = {}
        a的原型就是构造对象函数的prototype
        a.__proto__ === Object.prototype

2. Object.prototype是所有对象的的直接或间接原型

    例2: 
        Function.prototype.__proto__ === Object.prototype 

3. 任何函数.__proto__ === Function.prototype

    例3：任何函数包含构造函数和被构造出来的函数。
        构造函数:
                  Object.__proto__ === Function.prototype
                  Function.__proto__ === Function.prototype
                  Array.__proto__ === Function.prototype

        被构造出来的函数：

                  function fn(){}

              fn.__proto__ === Function.prototype
```

根据以上的概念和公式,我们可以推出Object,Function,Array的原型以及被构造函数构造出来的对象的原型
## 1. Object

```JavaScript
注意：Object.prototype是所有对象的直接或间接原型。那Object.prototype的原型是什么？是null(是没有的意思)

console.log(Object.prototype.__proto__ === null )  //true 


//对象的构造函数
Object.__proto__ === Function.prototype   //公式3
Object.__proto__.__proto__ === Object.prototype //公式3 再看公式2
Object.__proto__.__proto__.__proto__ === null //公式3 再看公式2  看看开头的注意

//对象实例

let a = {}

a.__proto__ === Object.prototype    //公式1
a.__proto__.__proto__ === Object.prototype.__proto__  //先看公式1,再看开头的注意

```


## 2. Function

```JavaScript
//函数的构造函数 Function
Function.__proto__ === Function.prototype   //由于公式3 任何函数的原型是构造函数Function.prototype
Function.__proto__.__proto__ === Object.prototype //由于公式3.再看公式2
Function.__proto__.__proto__.__proto__ === Object.prototype.__proto__ //公式3 2 再看1. Object 开头的注意
Function.prototype.__proto__ === Object.prototype //由于公式2对象的prototype是所有对象间接的原型的典范
Function.prototype.__proto__.__proto__ === Object.prototype.__proto__  //由于公式2 再看1. Object 的开头注意

//函数实例 fn
function fn(){}

fn.__proto__ === Function.prototype //由于公式3任何函数的原型是Function.prototype

fn.__proto__.__proto__ === Function.prototype.__proto__ === Object.prototype //由于公式3再看公式2
fn.__proto__.__proto__.__proto__ === Function.prototype.__proto__.__proto__ === Object.prototype.__proto__
//由于公式3再看公式2 再看1. Object 开头的注意
fn.prototype.__proto__ === Function.prototype.__proto__ === Object.prototype //由于公式2
fn.prototype.__proto__.__proto__ === Function.prototype.__proto__.__proto__ === Object.prototype.__proto__
//由于公式2 再看1. Object 开头的注意
```

## Array

```JavaScript
//数组的构造函数
Array.__proto__ === Function.prototype  //公式3
Array.__proto__.__proto__ === Function.prototype.__proto__ 
//再次公式1和3都满足在看公式2
Array.__proto__.__proto__.__proto__ === Object.prototype.__proto__ 
//再次公式1和3都满足在看公式2。再看1. Object开头的注意


//数组的实例
[].__proto__ === Array.prototype  //公式1
[].__proto__.__proto__ === Object.prototype //公式1 再看公式2
[].__proto__.__proto__.__proto__ === Object.prototype.__proto__ 
//公式1 再看公式2 再看1.Object 开头的注意

```