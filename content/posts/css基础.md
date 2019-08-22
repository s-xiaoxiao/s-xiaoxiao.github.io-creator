---
title: "css基础"
date: 2019-08-22T14:10:36+08:00
draft: false
---

# css 基础

## 文档流(normal flow)

文档流里有块、内联以及内联块

#### 1. block(块)：可自行设定宽度(width),高度(height)(不设定情况下,由内部文档流决定)并且独占一行

![](/CssBasic_images/block.png)

#### 2. inline(内联)：由内容限定宽度,高度由 line-height 间接确定(跟 height 无关).跟其他内联占满一行(多余另起一行)

![](/CssBasic_images/inline.png)

#### 3. inline-block(内联块)：可自行设定宽度以及高度,但具有内联特性(不会独占一行)又具有块的特性(块不可分离)

![](/CssBasic_images/inline-block.png)

<pre>
注意要点:
        width and height 默认是：auto(自动伸缩);
        width 不可写：100%(会有 bug);
        当内容大于宽度或者高度时,会发生溢出。overflow属性默认:visible(显示溢出部分)。hidden(溢出部分隐藏),auto(根据实际宽度和宽度出现滚动条),scroll(不管是否溢出直接除滚动条).当前是overflow:auto;
        块级元素里没有content,height为0
</pre>

## 脱离文档流

会使当前容器脱离父容器,父容器的宽高度计算则忽略有以下代码的子容器

position:absolute/fixed; or float:left;

## 盒模型

content padding border margin 共有 4 层。内容+内边距+边框+外边距

#### 两种盒模型

#### 1. content box(内容盒) 内容就是盒模型的边界

![](/CssBasic_images/content-box.png)

#### 2. border box(边框盒) 边框就是盒模型的边界

![](/CssBasic_images/border-box.png)

## margin 合并

当上下都有 margin 时(没有 border 等等),外边距会合并。

1. 父子合并(最上父子 margin 和最下父子 margin)
2. 兄弟合并(相邻的 margin 也会合并)

> 以上情况都是在 Margin 和 Margin 之间没有其他属性存在,才会合并

-   消除父子之间合并 padding/border/overflow:hidden/display:flex;
-   兄弟之间合并可以用 inline-block;

## 基本单位

#### 1. 长度单位

-   px 像素
-   em 相对于自身 font-size 大小的倍数
-   百分数
-   整数
-   rem
-   vw vh

#### 2. 颜色单位

-   十六进制(#000000 可缩写#000)
-   rgb/rgba (rgb(0-255,0-255,0-255),a(0-1)透明)
-   hsl/hsla(hsl(1-360,百分比,百分比),a(0-1)透明)

## 彩虹(只供参考)

<div style="overflow:hidden;height:200px;width:500px;">
    <div style="overflow:hidden;height:400px;height:400px;margin:10px;border-radius:50%;background:hsl(0,80%,50%);">
        <div style="overflow:hidden;height:380px;margin:10px;border-radius:50%;background:hsl(60,80%,50%);">
            <div style="overflow:hidden;height:360px;margin:10px;border-radius:50%;background:hsl(120,80%,50%);">
                <div style="overflow:hidden;height:340px;margin:10px;border-radius:50%;background:hsl(180,80%,50%);">
                      <div style="overflow:hidden;height:320px;margin:10px;border-radius:50%;background:hsl(240,80%,50%);">
                        <div style="overflow:hidden;height:300px;margin:10px;border-radius:50%;background:hsl(300,80%,50%);">
                            <div style="overflow:hidden;height:280px;margin:10px;border-radius:50%;background:hsl(330,80%,50%);">
                                <div style="overflow:hidden;height:260px;margin:10px;border-radius:50%;background:hsl(330,80%,100%);">
                                </div>
                            </div> 
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>

<!-- <div style="height:20px;"><div> -->

## 问题

#### 1. 第一问！ CSS 是什么以及是谁发明的？

> css 全称为：Cascading style sheets(层叠样式表).是具有不同样式的一张表可被一调多层叠(如一人穿两件外套)

> 由哈肯·莱提出建议并且与伯特·波斯发布了 CSS 规范的第 1 个版本
