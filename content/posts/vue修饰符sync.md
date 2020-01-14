---
title: "Vue修饰符sync "
date: 2020-01-14T23:03:36+08:00
draft: false
---

预知知识：

```JavaScript
new Vue({
  data(){return {}},
  components:{},
  props:[]
})
```

1. data(){}：保存数据的对象
2. components：保存组件的对象
3. props：保存父组件传来的数据
4. vue.文件是一个组件

# sync

sync 同步：这个同步说的是数据之间的同步。

<pre>
需求：用Vue模拟 使用余额宝支付完,让余额宝更新到最新金额

1. 余额宝是一个父组件,有money一个数据
2. 付款是一个子组件,引用余额宝父组件的money数据

.sync修饰符主要目的是子组件引用父组件数据,由子组件让父组件数组产生了变化要告诉父组件,父组件会修改自身数据。
由于子组件引用了父组件的数据,所引用的数据自然会更改
</pre>

代码逻辑：

1. 父组件：余额宝.vue

```JavaScript
<template>
      <div>
        余额宝: {{money}}
        <hr />
        <Pay :payMoney.sync="money"/>
      </div>
</template>

<script>
      import Pay from "./Pay.vue";
      export default {
        data() {
          return { money: 1000 };
        },
        components: { Pay }
};
</script>

<style>
</style>
```

    Pay 是子组件。payMoney是在子组件定义的引用父组件money。

2. 子组件：付款框.vue

```JavaScript
<template>
      <div>
        {{payMoney}}
        <button @click="$emit('update:payMoney', payMoney-100)">
        <span>花钱</span>
        </button>
      </div>
</template>

<script>
export default {
      props: ["payMoney"]
};
</script>

<style>
</style>
```

    update:money是一个事件,点击该button会发布.payMoney-100的结果会告诉父组件,父组件产生数据修改.会返回给子组件

总结：.sync 主要目的是在子组件引用父组件数据产生了修改同步到父组件上
