1

# hugo 快速创建 blog 教程

## 第一步：下载 hugo

[下载地址](https://github.com/gohugoio/hugo/releases)

#### 1. 选择文件名为：hugo-版本号-系统-32bit/64/.zip

根据系统选择对应的

例：windows-64 位：hugo_0.57.2_Windows-64bit.zip

#### 2. 解压文件到你所知道的目录(路径你要知道,等下要用)。你会得到一个 hugo.exe 文件

#### 3. 把执行命令放到 path 变量,先复制 hugo.exe 的路径;右键桌面我的电脑-属性-高级系统设置-环境变量-下面框框里(系统变量)-双击 Path-新建-粘贴-有确定就点确定。

![](/images/3.png)

自检：重新打开 cmder(相关命令行工具)输入 hugo version(你会得到一个版本号)
![](/images/4.png)

> 如果没有成功重新检查以上步骤!

恭喜你，下载完成！
<br/><br/>

## 第二步：利用 hugo 快速搭建 blog 框架

#### 1. 创建建站根一个目录()

![](/images/6.png)

#### 2. 该目录在 cmder()打开

![](/images/5.png)

#### 3. 地址栏输入网址,回车进入;点击 quick start：

[hugo 官网](https://gohugo.io/)
![](/images/1.png)
![](/images/2.png)

#### 4. 从 step2 开始 复制-打开 cmder(一定要在第一步创建的目录里)-粘贴-回车

![](/images/7.png)

#### 5. step3 复制-打开(cmder)-粘贴-回车

本地仓库初始化和下载自带模板
![](/images/8.png)

#### 6. step4 复制-打开(cmder)-粘贴-回车

![](/images/9.png)
创建 blog 目录(posts 目录存放 blog)和第一个 blog 文件以.md 为后缀
![](/images/14.png)可以自己写点什么，注意时间不要来自未来！

#### 7. step5 复制-打开(cmder)-粘贴-回车

启动服务器
![](/images/10.png)
如果一样则成功启动.按住 Ctrl+左键就可以进入啦
![](/images/11.jpg)

如果出现以下情况
![](/images/13.png)
继续往下做 ctrl+C 关闭掉配置一下再启动

#### 8. step6

这个是要需要配置以下:新建一个 config.toml 文件，把 step 的内容复制进来，保存关闭
![](/images/15.png)

#### 8. step7

构建静态页面,命令是 hugo
![](/images/16.png)
弄完之后。再次启动 hugo server -D 返回 step6 看到http://localhost:54316; 主要是前缀，后面的数字无所谓。因为我打开了两个所以这次是 54316.ctrl+左键就可以进入了静态页面啦。具体页面见 step5。

恭喜你,hugo 静态站点已经创建完毕。接下来是写 blog 了。

## 第三步：写博客

#### 1. 在页面里，如果你的画面里跟我一样(你第一次是一个,我创建了两个.md 文件)则成功啦。点击 read more 可进入阅读

![](/images/12.png)
如果页面里,没有的话,你需要创建一个 content 目录(创建在根目录,我的是 hugo-demo),把 posts 目录放到 content 里。刷新一下就有啦
如图：![](/images/17.png)
如果还没有怎么办!!.以上配置好
你再重启服务器。就 ok 啦。

hugo server -D (启动 hugo 服务器命令)

<hr style="width:100%">
## 分割线
在 content/posts/目录下所有以.md 文件都是一篇博客。

写多篇用 ctrl+c 复制 ctrl+v 粘贴,修改时间即可


