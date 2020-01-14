---
title: "Vue数据响应式之data "
date: 2020-01-12T21:35:36+08:00
draft: false
---

前置概念：

    数据响应式：数据在后台或者在内存里发生了改变要更新到 UI 视图层里。这是数据的响应式

预知知识：

    getter：get 语法将对象属性绑定到查询该属性时将被调用的函数
    setter：set 语法当尝试设置属性时,set语法将对象属性绑定到要调用的函数
    Object.defineProperty：该方法会直接在一个对象上定义一个新属性,或者修改一个对象的现有属性,并返回这个对象。

# 实现简易版的 vue 数据响应式之 data

需求：实现监听 data 里的数据是否发生了变化

```JavaScript
new Vue({
  data:{
    n:0
  },
  template:`
    <div>
      {{n}}<button @click="add">+1</button>
    </div>
  `,
  methods:{
    add(){
      this.n+=1
    }
  }
}).$mount('#app')

//我们知道在ui视图会显示一个数据0和一个按钮。按一下按钮数据会＋1.这是Vue的数据响应式
//接下来我们粗糙的看看原理是如何监听到数据的变化并且响应到视图里的
```

## getter 和 setter

我们来看看代码

```JavaScript
let obj = {
  h:'hello',
  w:'world',
  get hw(){
    return this.h+`,`+this.w
  },
  set hw(hw){
    this.h=hw.h
    this.w=hw.w
  }
}

console.log(obj.hw) //hello,world 注:属性前面有get 该属性的值是函数的返回值。该函数不接受参数;
obj.hw = {h:'h',w:'w'}  //注：set 所绑定的函数只能有一个形式参数
console.log(obj.hw) //h,w,你看当我给obj.hw赋值之后.h .w发生了改变。这点很重要
```

通过代码我们了解的 get 语法是得到该对象属性值,set 语法是设置该对象属性值,这是它们不同的地方。相同的是它们都会为该属性绑定一个被调用的函数

注意的是:

```
hw 是被getter创造的,属于伪属性,不是函数！可以理解为具有动态计算的属性
hw 没有setter的话，该值是不会发生改变的。相反没有getter则访问不到该属性,返回undefined
hw 在控制台用console.dir(obj),会显示hw(...) 这是为什么？因为它还没有调用getter。点击之后就会调用了
get hw(){} 和 get hw(){}是不允许的,set同理。
```

## Object.defineProperty

该方法是在对象已定义外定义或修改一个属性

```JavaScript
let data = {n:0}
Object.defineProperty(data,'$n',{
  get(){
    return this.n
  },
  set(value){
    this.n = value
  }
})

//第三个参数是属性描述符;这次我们只用value(JavaScript值)、get(getter方法)、set(setter方法)
```

目前我们知道 getter 是读一个伪属性值,getter 是设置一个伪属性值.defineProperty 是定义或修改一个对象的属性

## 数据响应式原理之 data

1. 把 data.n 的数据渲染到 ui 层做到响应式

   ```JavaScript
   let data = {
     n:0,
     get $n(){
       return this.n
     },
     set $n(value){
       if(value!==this.n){
         this.n = value
         console.log('我要响应到页面去')
         // 渲染到ui层函数(this.n)
       }
     }
   }
   data.$n     //0
   data.$n = 5 //首先if判断是不是不相等？如果不相等就保存该值,调用console.log,并且调用渲染到UI层函数
   data.$n     //5
   //但是有个BUG
   data.n=10   //n的值是被修改了。你会想我修改了值。咋没响应呢到页面去呢？
   data.$n     //10 完了.这咋没响应呢？

   //继续优化代码
   ```

2. 改进可以直接修改 data.n 的 bug
   <pre>这咋改?如何使得让别人访问不到 data 这个对象！
          你还记得匿名吗？你能从匿名对象里拿到属性吗？显然不能！
          使用匿名对象岂不是谁都拿不到了？
          你可以使用函数包裹它,然后返回一个新的对象包含getter、setter和数据
   </pre>

```JavaScript

function proxy({data}){  //注解：{data}是解构赋值。传进去的匿名函数data属性给形参data
      let obj = {}
      Object.defineProperty(obj,'n',{
        get(){
          return data.n
        },
        set(value){
          if(data.n!==value){
            data.n=value
            console.log('我要响应到页面去')
            // 渲染到ui层函数(this.n)
          }
        }
      })
      return obj
}
let obj = proxy({
      data:{
        n:0
      }
})      //是不是很new Vue({data:{n:0}}) 一样？
obj.n  //0
obj.n = 5 //判断改变值。然后可以响应到页面
obj.n  //5

//proxy这个函数我们称之为代理。因为外部现在无法绕过setter修改数据
```

      此代码还是有 bug,如果匿名对象 data 属性我引用了外部对象。我修改外部对象照样跟版本 1 一样我就可以直接,而做不到响应了

1. 改进匿名对象引用外部对象的 BUG
   <pre>在内部复制外部对象的值?失去对外部对象的依赖。这样是不是可以的？</pre>

```JavaScript
function proxy({data}){ //注解：{data}是解构赋值。传进去的匿名函数 data 属性给形参 data
      let obj = {}
      let value = data.n
      Object.defineProperty(obj,'n',{
        get(){
          return value
        },
        set(newValue){
          if(value!==newValue){
          value = newValue
          console.log('我要响应到页面去')
          // 渲染到 ui 层函数(this.n)
        }
      }
      })
      return obj
}
let mydata = {n:0}
let obj = proxy({
      data:mydata
})
obj.n //0
mydata.n = 10
obj.n   //还是等于0

//目前只是单个值有效。跟vue实际代码是不同的。因为它可以处理很多个数据。而这个只是单一的值
```

## Vue.set and this.\$set

      前面的一切前提是有个原始数据 n
      如果一开始没这个 n 还可以代理一个数据和监听它吗？
      可以的.使用Vue.set 或者 this.$set  (因为它们两者相等)

```JavaScript
// 例：
new Vue({
  data:{

  },
  methods:{
    addB(){
      this.$set(this.data,'a',10)
    }
  }
})
//添加一个a属性
//自动创建代理和监听
//异步触发UI更新
```

## vue 的 bug

```JavaScript
new Vue({
  data:{
    data:{
      a:0d
    }
  },
  template:`<div>{{b}}</div>`

}).$mount('#app')
//注意,data.data.a 但是没有数据b。模板引用了数据b。vue不会爆出错误。
//这个使用你就可以用vue.set
//在methods里面设置
```

总结：该文章只是简易版所以只对一个数据的响应

1. 数据响应式是因为 getter、setter 对对象属性绑定所调用函数的特性,而触发了渲染函数,
2. 数据应该被一个匿名对象所包裹起来传给函数。因为这样除了该函数其他无法直接修改
3. Object.defineProperty 提供了在对象外创建属性的功能
4. Vue 带有 Vue.set 和 this.\$set 可以创建数据和代理监听并且异步更新
5. Vue 在检测 data 时,只会检测 data 一层属性。小心,别写出 bug
