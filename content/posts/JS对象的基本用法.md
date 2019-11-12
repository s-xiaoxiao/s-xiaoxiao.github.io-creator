---
title: "JS对象的基本用法"
date: 2019-10-18T14:10:36+08:00
draft: false
---

# 前言

> 本文章仅仅自身学习需要,把目前所学已知的用文章记录下来，做一次归纳整理,达到理解和加强记忆的目的，如有不对,请联系: 460046653@qq.com

## 1. 如果声明一个对象(Object)数据类型？

什么是对象呢？对象是属性(由属性名和属性值组成一个属性,:隔开)的集合,用大括号括起来.称之为对象(没有属性名和属性值是空对象(null))

```JavaScript
//以下是规范写法
let obj = new Object({'属性名':'属性值','属性名':'属性值'}) //逗号隔开

//以下是简写写法
let obj2 = {'属性名':'属性值','属性名':'属性值'}
```

<strong>属性名</strong>：遵守标识符的规则,可不加引号。其他例外

```JavaScript
//如果属性名是数值也不可加引号,但自动转换为字符串
let obj = {
  1: 'a',
  3.2: 'b',
  1e2: true,
  1e-2: true,
  .234: true,
  0xFF: true
}
//它们最终都会变成字符串！！
// obj {
//   1: "a",
//   3.2: "b",
//   100: true,
//   0.01: true,
//   0.234: true,
//   255: true
// }

//不符合标识符 报错
let obj = {
  1+2:3,
  1a:'1a'
}

//如果不符合标识符,请加上引号
let obj = {
  '1+2':3,
  '1a':'1a'
}
```

<strong>变量可不可以当成属性名呢？</strong> 答：可以的我们来看代码

```JavaScript
let p1 = 'name'
let obj = {
  [p1]:'xiaoxiao',
}
```

> 可以看到 p1 是一个 string 类型的变量,值为'name'.我们定义对象时,属性名是被中括号包裹起来了,这是当属性名为变量时的一个写法.属性名是变量的值(name)

<strong>属性值</strong>:值遵守该类型规则即可

## 2. 如何删除对象的属性？

```JavaScript
//我们先声明一个对象

let obj = {
  'name':'xiaoxiao',
  'age':18
}

delete obj.name  //name属性就会消失
```

## 3. 怎么查看对象的属性？

分两种情况来看,一个是自身属性,一个是该对象的共有属性

```JavaScript
let obj = {
  name: "xiaoxiao",
  age: 18
};
//查看自身属性
Object.keys(obj);

//查看自身属性和共有属性
console.dir(obj);
```

## 4. 如何修改或增加对象的属性

```javascript
//定义之后增加对象的属性
let obj = {
  name: "xiaoxiao"
};
obj.age = 18;

//定义之后修改对象属性的属性值
obj.name = "xiao";
obj.age = "20";

//或者批量一起修改 可以用Object提供的assign属性
Object.assign(obj, { name: "xiaoxiao", age: 20 });
```

## 5. 'name' in obj 和 obj.hasOwnProperty('name') 的区别

你会问这是什么意思？区别在于自身属性和共有属性,我们在查看和删除的时候

```JavaScript
let obj = {
  name:'xiaoxiao',
  age:18
}
//我们知道obj有个自身属性,它还有一个隐藏的共有属性

'name' in obj //问name是属于obj的自身属性或共有属性吗？答案是true

obj.hasOwnProperty('name') //问name自身属性吗？ 答案是true
```

明白它们之间的区别了吗？
