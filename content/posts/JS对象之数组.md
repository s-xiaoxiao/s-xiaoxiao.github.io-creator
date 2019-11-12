---
title: "JS对象的基本用法"
date: 2019-10-21T09:10:36+08:00
draft: false
---

> 前言：本文章仅仅适合刚入门的 JS 人员.本文章仅仅自身学习需要,把目前所学已知的用文章记录下来，做一次归纳整理,达到自己理解和加强记忆的目的，如有不对,请联系: 460046653@qq.com

# 数组(Array)

在 JavaScript 里 Array 对象是构造数组的全局对象.在 JS 里数组也是对象

## 1. 创建一个数组

### 1.1 新建

```JavaScript
let arr = new Array('apple','xiaomi') //规范写法
let arr1 = ['apple','xiaomi']         //简写
```

<strong>规范写法</strong>：需要用到 new 操作符 调用 Array 对象函数来构造一个数组.接收的参数用逗号隔开。

<strong>简写</strong>：只需要用中括号即可,用逗号隔开每一项

注：当用规范写法时,且参数为数字并参数为一位时,例外

```JavaScript
let arr = new Array(3)  //这是声明数组的长度为3,内容为empty(空)
console.log(arr.length) //3   (length是构造时创造给arr自身的属性)
```

### 1.2 转化成数组

#### 1.2.1 split():方法使用指定的分隔符将字符串(string)对象分割成字符串数组。

```JavaScript
let arr ='1,2,3'.split(',')
console.log(arr)            //["1","2","3"]

let arr1='123'.split('')
console.log(arr1)           //["1","2","3"]
```

#### 1.2.2 Array.from():方法从一个类似数组或可迭代对象创建一个新的,浅拷贝的数组实例

```JavaScript

console.log(Array.from('123'))  //类似数组创建一个新的数组。
```

### 1.3 合并多个数组.concat()方法

怎么把多个数组合在一起? concat()方法合并多个数组。不改变现有数组,返回一个新数组

```JavaScript
let arr1 = ['a','b','c']
let arr2 = [1,2,3]
let arr3 = [4,5,6]
console.log(arr1.concat(arr2)) //["a","b","c",1,2,3]

console.log(arr1.concat(arr2,arr3)) //["a","b","c",1,2,3,4,5,6]
```

### 1.4 拷贝一个数组.slice()方法

slice() 方法返回一个新的数组对象,不会改变原数组.接收两个参数 begin 和 end(包括 begin,不包括 end))。

```JavaScript
let arr = [1,2,3,4,5,6]

console.log(arr.slice(2,4)) //[3,4]
console.log(arr.slice(0))   //[1,2,3,4,5,6]
```

第一个参数是开始,第二个是结束。当只有一个参数时,end 被省略,slice 会一直提取到原数组末尾

## 2. 删除数组中的元素

在一个数组当中,要么删开头要么删尾部,和数组的中间.

<strong>shift()</strong>:方法删除数组的第一个元素,返回被删除的值。如果数组为空返回 undefined。并且改变原数组的长度

```JavaScript
let arr = [1,2,3,4,5]
console.log(arr)           //[1,2,3,4,5]
console.log(arr.shift());  // 1
console.log(arr)           //[2.3.4.5]
```

<strong>pop()</strong>:方法删除数组最后一个元素,返回被删除的值.如果数组为空返回 undefined.并且改变原数组的长度

```JavaScript
let arr = [1,2,3,4,5]

console.log(arr)        //[1,2,3,4,5]
console.log(arr.pop())  //5
console.log(arr)        //[1,2,3,4]
```

<strong>splice()</strong>:方法通过删除或替换现有元素或者原地添加新的元素来修改数组。此方法会改变原数组的长度(当前实例只是删除)

```JavaScript
let arr = [1,2,3,4,5]

console.log(arr)              //[1,2,3,4,5]
console.log(arr.splice(2,1)); //[3] 参数1是开始.参数2是删几个
console.log(arr)              //[1,2,4,5]
```

## 3. 查看元素

### 3.1 查看所有元素

#### 3.1.1 for 语句

for 是一个循环,数组的下标有规律可循。可逐步循环出数组中的每个元素

```JavaScript
let arr = [1,2,3,4,5]

for(let i=0;i<arr.length;i++){
  console.log(`${i}:${arr[i]}`)     // ${会认为是一个变量}
}
/*输出：
0:1
1:2
2:3
3:4
4:5
undefined
*/

```

#### 3.1.2 forEach()方法

forEach():方法对数组的每个元素执行一次提供的函数

```JavaScript
let arr = [1,2,3,4,5]
arr.forEach(function(value,index){
  console.log(`${index}:${value}`);
})
/*输出
0:1
1:2
2:3
3:4
4:5
undefined
*/
```

forEach()方法会循环数组并且执行接收的函数,函数第一个参数是当前元素,第二个参数是当前元素的索引(下标)

<strong>自己写个 forEach</strong>

输出元素 value

```JavaScript

function forEach(array,fn){
  for(let i=0;i<array.length;i++){
    fn(array[i])                    //调用参数二函数
  }
}

forEach([1,2,3,4,5],function(arrayValue){
  console.log(arrayValue);          //输出元素的value
})
```

输出元素 index 和 value

```JavaScript
function forEach(array,fn){
  for(let i=0;i<array.length;i++){
    fn(array[i],i)                   //调用参数二函数
  }
}

forEach([1,2,3,4,5],function(arrayValue,arrayIndex){
  console.log(`${arrayIndex}:${arrayValue}`)//输出元素的value和index
})
```

### 3.2 查看单个元素

<strong>通过下标<strong>

```JavaScript
let arr = [1,2,3,4,5]

console.log(arr[0])  //1
```

> 注:下标从 0 开始

#### 3.2.1 indexOf():方法查看某个元素是否在数组里,存在返回索引,否则返回-1

```JavaScript
let arr = [1,2,3,4,5]

console.log(arr.indexOf(2))  //1
```

#### 3.2.2 find():方法返回数组中的元素满足测试函数的第一个元素的值,否返回 undefined

```JavaScript
let arr = [1,2,3,4,5]
// 元素的值为偶数返回值
console.log(arr.find(function(element){
  return element%2===0          //2
}))

//函数简写为箭头函数
console.log(arr.find(element => element%2===0))   //2
```

#### 3.2.3 findIdenx:方法返回数组中的元素满足测试函数的第一个元素的索引,否返回 undefined

```JavaScript
let arr = [1,2,3,4,5]
//元素的值为偶数返回索引
console.log(arr.findIndex(function(element){
  return element%2===0          //1
}))
//函数简写为箭头函数
console.log(arr.findIndex(element => element%2===0))  //1
```

## 4. 增加数组中的元素

跟删除大致相同,要么在开头添加元素,要么在尾部添加元素,或者在中间添加元素

### 4.1 unshift():方法将一个或多个元素添加到数组的开头,并返回该数组的新长度

```JavaScript
let arr = [3,4,5]

console.log(arr)              //[3,4,5]
console.log(arr.unshift(1,2)) //5
console.log(arr)              //[1,2,3,4,5]
```

### 4.2 push(): 方法将一个或多个元素添加到数组的尾部,并返回数组的新长度

```JavaScript
let arr = [1,2,3]

console.log(arr)            //[1,2,3]
console.log(arr.push(4,5))  //5
console.log(arr)            //[1,2,3,4,5]
```

### 4.3 splice():方法通过删除或替换现有元素或者原地添加新的元素来修改数组,并以数组形式返会被修改的内容。会改变原数组

```JavaScript
let arr = [1,2,5]

console.log(arr)                  //[1,2,5]
console.log(arr.splice(2,0,3,4))
//[].在索引2处删除0个元素并且添加3和4.没有修改内容,因此返回空数组

console.log(arr)                  //[1,2,3,4,5]
```

## 5. 修改数组元素的顺序

### 5.1 reverse():方法会把元素的位置颠倒,第一个变成最后一个,最后一个变成第一个。并返回该数组，该方法会改变原数组

```JavaScript
let arr = [1,2,3,4,5]

console.log(arr)            //[1,2,3,4,5]
console.log(arr.reverse())  //[5,4,3,2,1]
```

### 5.2 sort():方法默认排序是在将元素转换为字符串,然后比较每个字符串的第一个大小(你可以理解第一个字符,谁小谁在前).会改变原数组元素的顺序

```JavaScript
let arr = ['c','b','a','d']

console.log(arr.sort())   //[a,b,c,d]


let arr = [10,20,100,300,2000]

console.log(arr.sort())   //[10,100,20,2000,300]
```

不过呢,它可以接收一个函数,改变升序还是降序,我们来提供一个函数。

函数接收两个参数：如果

(a,b) 小于 0 a 在 b 前面,

(a,b) 等于 0 位置不变,

(a,b) 大于 0 b 在 a 的前面

```JavaScript
let arr = [10,20,1000,30000,200]

console.log(arr.sort((a,b) => a>b?-1:1))  //[30000,1000,200,20,10]
console.log(arr.sort((a,b) => a>b?1:-1))  //[10,20,200,1000,30000]
```

## 6. 数组变换

### 6.1 改变数组每个元素

<strong>map()</strong>:方法创建一个新数组,其结果是该数组每个元素都调用一个提供的函数返回的结果。不会改变原数组

```JavaScript
let arr =[1,2,4,6]

console.log(arr.map(function (x){
  return x*2;
}))   //[2,4,8,12]

console.log(arr)  //[1,2,4,6]
```

### 6.2 提取数组中满足条件的元素

<strong>filter()</strong>:方法创建一个新数组,其包含通过了所提供函数实现的测试的所有元素
.不会改变原数组

```JavaScript
let arr =[1,2,3,4,5,6,7,8,9,10]


console.log(arr.filter(function(x){
  return x%2===0;
}))           //[2,4,6,8,10]
console.log(arr)  //[1,2,3,4,5,6,7,8,9,10]
```

### 6.3 对整体数组中的元素做某些操作

<strong>reduce()</strong>:方法对数组中每个元素执行一个由你提供的 reduce 函数(升序执行),将其结果汇总为单个返回值.不会改变原数组

```JavaScript
let arr = [5,5,5,5]

console.log(arr.reduce(function (arrSum,arrValue){
  return arrSum+=arrValue
},0))     //20
console.log(arr)      //[5,5,5,5]
```

> reduce()方法接收两个参数：函数(函数接送四个参数：累加器,当前元素值,当前元素索引,自身数组),初始值。上面例子初始值为 0.函数值只调用了两个参数
