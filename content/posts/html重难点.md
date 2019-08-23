---
title: "HTML 重难点"
date: 2019-08-19T12:58:36+08:00
draft: false
---

## a 标签

#### 1. a 标签属性

-   <strong>href</strong>
    -   网址：href="https://www.google.com" or href="http://www.google.com" or //google.com
    -   路径：href="/a/b/c.html"
    -   伪协议：href="javascript:alert(1);" (value 要小写,点击会执行 javascript 代码) or javascript:;相对于页面来说,不会有视觉上的刷新
    -   ID 当某个标签有 id 之后.例 :id="xx" href="#xx".会定位到 id 为 xx
    -   mailto：href="mailto:460046653@qq.com" ;
    -   tel：href="tel:11 位电话号" ; 点击时会打电话

> 总结：可跳转到其他页面,可跳转到子页面，可跳转到邮箱、电话.

-   <strong>target</strong>

    -   \_blank :<a href="//baidu.com" target="_blank">target 取值为:\_blank</a>

        空白页面打开

    -   \_top ：<iframe src=""></iframe>在 iframe src 属性链接的页面里,a 标签 target 设置\_top 会在最顶页面打开
    -   \_parent：在父级页面打开跳转页面
    -   \_self：<a href="//baidu.com" target="_self">target 取值为:\_blank</a>当前页面打开
    -   window.name:target="xx" 指定窗口(如果没有会新建一个窗口(在空白页))为打开
    -   iframe.name:指定在 iframe 标签打开页面：当 iframe 属性 name 为：xx。 a 属性 target="xx" 则会在 iframe 页面中打开跳转页面

*   <strong>download</strong>:下载当前页面(浏览器支持度低)
*   <strong>rel=noopener</strong>：以后补上。

## iframe 标签

    主要作用于在页面里,加载一个页面。会有上下关系

## table 标签

1. <strong>相关标签</strong>

```html
<table>
    <thead>
        <tr>
            <!--table row (代表一行) -->
            <th></th>
            <!--table head (表头)-->
            <th>1</th>
            <th>2</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <th></th>
            <td>3</td>
            <!--table data (表数据)-->
            <td>4</td>
        </tr>
    </tbody>
    <tfoot></tfoot>
</table>
```

<table>
    <thead>
        <tr>
            <th></th>
            <th>1</th>
            <th>2</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <th></th>
            <td>3</td>
            <td>4</td>
        </tr>
    </tbody>
    <tfoot></tfoot>
</table>

table thead tbody tfoot tr th td

## img 标签

-   作用

    发送 get 请求展示一张图片

-   属性

    <strong>alt:</strong>文字替换(当图片加载失败,会显示文字)

    <strong>height：</strong>设置高度

    <strong>width：</strong>设置宽度

    <strong>src：</strong>设置图片来源

-   事件

    <strong>onload：</strong>在 js 里,判单是否加载成功

    <strong>onerror:</strong>在 js 里,判断是否加载失败

-   响应式

    max-width:100%; 适应不同的宽高度

## form 标签

-   作用

    发送 get 或 post 请求,然后刷新页面

-   属性

    <strong>action:</strong>把数据发送到页面

    <strong>autocomplete:</strong>自动填充用户名等等

    <strong>method:</strong>使用 get/post 方法提交

    <strong>target:</strong>当提交时,页面在哪里显示(例如：PC 页面用支付宝付款时,会跳转到支付宝页面)

-   事件

    <strong>onsubmit</strong> 当点击 submit 时会触发 onsubmit

## input 标签

-   <strong>作用</strong>:

    让用户输入内容

-   <strong>属性</strong>

    1. 类型 type

        <strong>button：</strong> 按钮(用于提交)

        <strong>color：</strong>颜色选择器

        <strong>checkbox：</strong>多选框

        <strong>file：</strong>上传文件/multiple 属性可多文件上床

        <strong>password：</strong>密码框

        <strong>submit：</strong>每个 form 必须有,

        <strong>text：</strong>文本

        <strong>textarea：</strong>多行文本输入

        <strong>radio:</strong>单选按钮

        <strong>select+option：</strong>多项选择

    2. 其他属性

        <strong>name</strong> 名字

        <strong>value</strong> 值

*   事件

    <strong>onchange：</strong>当改变时,触发

    <strong>onfocus：</strong>当聚焦时,触发

    <strong>onblur：</strong>当离开时,触发

-   验证器

    属性 required 必需填写

> form 里面的 input 要有 name
>
> form 里面放一个 type="submit"才能触发 submit 事件
