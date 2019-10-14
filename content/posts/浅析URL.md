---
title: "浅析 URL"
date: 2019-09-07T14:10:36+08:00
draft: false
---

# <strong>URL (统一资源定位符)</strong>

<br>
## <strong style="color:black;">1. URL 包含哪几部分，每部分分别有什么作用</strong>

![22](/hugo_images/URL.png)

<pre>
<strong >1. 协议：</strong><span style="color:black">规定请求以及响应具体过程的实现带有s则数据会被加密</span></strong>
<strong >2. 域名：</strong><span style="color:black">由域名管理系统(Domain Name System)管理主要方便于人类记忆(相对于IP),跟公网IP绑定</span></strong>
<strong >3. 路径：</strong><span style="color:black">当前路径页面</span></strong>
<strong >4. 参数：</strong><span style="color:black">搜索关键字</span></strong>
<strong >5. 锚点：</strong><span style="color:black">当有锚点时,浏览器可视窗口到元素ID为vue</span></strong>

</pre>

> 上方图中的意思是：你我遵循 https 协议,我要访问 xiedaimala.com 去路径为 search 提供的搜索功能,搜索关键字“前端”。这是请求的部分。
> xiedaimala.com 服务器会遵循 https 协议来解析请求。并且响应给我,然后浏览器会根据锚点去搜哪个元素的 ID 相等锚点

## <strong style="color:black;">2. DNS 的作用是什么，nslookup 命令怎么用</strong>

<pre>
    DNS的作用在于,当用户输入地址 xiedaimala.com,会先到DNS服务器询问xiedaimala.com 这个域名相对的IP
是多少? DNS解析后响应一个的IP。
    nslookup命令是查询域名相对的IP是什么例如：在命令行输入nslookup xiedaimala.com 会得到这个域名的IP 
</pre>

## <strong style="color:black;">3. IP 的作用是什么，ping 命令怎么用</strong>

<pre>
    IP分公网IP和内网IP。公网IP是你能上网的凭证，公网IP的作用是链接其他公网IP的身份凭证,例如：你要访问
baidu.com。首先你得有一个公网IP(由网络服务商提供服务)。baidu.com就知道是谁来访问啦。内网IP由路由
器分配的,管理终端一般由198.168.x.x开头
    ping 命令要在命令行内配合域名/IP使用.例如： ping baidu.com 。主要目的是否能通过IP链接到主机。过程
中会发送数据包,主机是否由响应
    
</pre>

## <strong style="color:black;">4. 域名是什么，分别哪几类域名</strong>

<pre>
    要说域名是什么，得先说起IP。IP的存在是给每个终端分配的一个名字，IP是一串数字。但是一串数字不利于人类
记忆以及传播更不容易发生联想。域名是由字符组成显然优于数字。域名对应着IP。当我们访问某个域名时,也可以说访
问某个IP。
    分别有顶级域名、一级域名以及二级域名例如：.com 是顶级域名 xiedaimala.com 是一级域名。 www.xiedaima
la.com 是二级域名 
    以后缀分类有：主要分为国家地区顶级域(例如：中国是 .CN)以及通用顶级域名
    通用顶级域名：
                1. .com - 供商业机构使用
                2. .net - 供网络服务商
                3. .org - 供非盈利组织
                4. .edu - 供教育机构
                5. .gov - 供政府
                6. .mil - 供军事
                7. .int - 供国际组织 等 
</pre>
