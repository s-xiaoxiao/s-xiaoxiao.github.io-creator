---
title: "css知识总结"
date: 2019-09-09T10:10:36+08:00
draft: false
---

## <strong style="color:black;">1.浏览器渲染原理</strong>

<pre>
    浏览器在得到服务器的响应之后,会先根据HTML去渲染成一个结构图HTML树(DOM)所有的标签具有父子关系。
HTML标签是最大的,其次是head和body两个大儿子,依次渲染。
    CSS会构建成CSS树(CSSOM)。CSS也是根据标签所具备的属性。在这里body是最大的。如果body所带的属性
如果标签是共有的属性,儿子会继承父子所带的属性
    这时会将HTML CSS 合并在一起成为渲染树(render tree)
    Layout布局然后是计算标签的大小以及位置渲染布局。(文档流、盒模型、计算大小和位置)
    paint绘制接下来是绘制标签的背景颜色、边框、字体颜色、阴影的(所有美化属性在渲染)
    compose最后一步是层叠的合成,(例如拍照：就是把背景和人合成跟这个概念是一样的)
    
</pre>

## <strong style="color:black;">CSS 动画的两种做法（transition 和 animatioin）</strong>

### <strong style="black">1. transition 过渡</strong>

作用:补充中间帧

<pre>
语法：

    transition:属性名 时长 过渡方式 延迟
    transition：left 200ms linear
    可以用逗号分隔两个不同属性
    transition:left 200ms,top 400ms
    可以用 all 代表所有属性
    transition:all 200ms
    时长单位只有 s 和 ms
    过渡方式有：linear|ease|ease-in|ease-out|ease-in-out|cubic-bezier|step-start|step-end|steps
</pre>

### <strong style="black">2. animation 动画</strong>

<pre>
缩写语法：
    animation:时长|过渡方式|延迟|次数|方向|填充模式|是否暂停|动画名；
    时长：1s 或者 1000ms
    过渡方式：跟 transition 取值一样，如 linear
    次数：3 或者 2.4 或者 infinite
    方向：reverse|alternate|alternate-reverse
    填充模式：none|forwards|backwards|both
    是否暂停：paused|running
    以上所有属性都有对应的单独属性。
</pre>
