---
title: "JavaScript的诞生"
date: 2019-09-03T20:10:36+08:00
draft: false
---

# JavaScript

<br>
### 1. 1994年底一家名为网景公司发布第一个版本的网页浏览器Mosaic Netscape 0.9。 四个月内抢占当时浏览器市场份额四分之三,成为90年代互联网的主要浏览器。当时的网页只是静态页面的浏览，没有交互能力。网景预见到未来网络需要变得更加多态。
<br>
### 2. 1995.4 身为网景的程序员布兰登·艾克目标是把Scheme语言嵌入浏览器内。但在这之前浏览器就已经支持java。内部争论中，网景决定发明一种跟Java在语法上有相似的语言，这个决策导致把Perl、Python、Tcl或Scheme排除在外。
<br>
### 3. 1995.5 艾克受命于此，仅仅花了10天时间就把原型设计出来了。最初命名为Mocha,9月分更名为LiveScript。同年12月改为JavaScript(当时网景公司跟Sun公司组成的开发联盟为了让这门语言搭上Java这个编程语言的热度)当时Sun公司大肆宣传Java"一次编写 到处运行"
<br>
### 4. 由于当时仓促的时间设计，语言的一些细节考虑的不够严谨，后来导致很长一段时间，JavaScript写出来的程序混乱不堪。总的来说，他的设计思路借鉴以下语言:

<pre>
(1) 借鉴C语言的基本语言

(2) 借鉴Java语言的数据类型和内存管理

(3) 借鉴Scheme语言，将函数提升到”第一等公民(first class)“的地位

(4) 借鉴Self语言，使用基于原型(prototype)的继承机制
</pre>

### 5. 标准化

<pre>
  在1996年11月，网景公司向ECMA(欧洲计算机制造协会)提交语言标准。
  1997年6月，ECMA以JavaScript语言为基础制定了ECMAScript标准规范ECMA-262。
  JavaScript成为了ECMAScript最著名的实现之一
</pre>

> 所以，Javascript 语言实际上是两种语言风格的混合产物----（简化的）函数式编程+（简化的）面向对象编程。这是由 Brendan Eich（函数式编程）与网景公司（面向对象编程）共同决定的。

# JavaScript 的十个设计缺陷

<pre>
      由于上文得知,JavaScript是在十天内设计的。所以因为时间的原因造成的一些缺陷.
      上文提到JavaScript语言是函数式+面向对象的混合产物,这是在那个时代是属于先例开创先河了,得自己摸索着
  前进.还有由于竞争的环境,当时微软公司推出自己的脚本语言Jscript,由此网景公司在当时处于不利于自己的
  处境,先下手强,申请JavaScript国际标准,打压微软.1997年6月第一个国际标准ECMA-262正式版权().
      你想象一下这个过程当中从设计到标准,时间太快了.这门语言还没有经过市场的检验国际标准就问世了,当然这
  也是迫不得己,相比之下其他语言将近几十年的市场检验,才发布国际标准.
</pre>

### <strong style="color:black;">1. 不适合开发大型程序</strong>

<pre>
    Javascript没有名称空间（namespace），很难模块化；没有如何将代码分布在多个文件的规范；
    允许同名函数的重复定义，后面的定义可以覆盖前面的定义，很不利于模块化加载。
</pre>

> 相对于 C 语言，一个函数名只能出现一次。

### <strong style="color:black;">2. 非常小的标准库</strong>

<pre>
Javascript 提供的标准函数库非常小，只能完成一些基本操作，很多功能都不具备。
</pre>

> 怎么形容呢，大概就是你活在原始部落(当时 JavaScript),跟现代化城市的对比。

### <strong style="color:black;">3. null 和 undefined</strong>

<pre>
null属于对象（object）的一种，意思是该对象为空；undefined则是一种数据类型，表示未定义。
</pre>

```JavaScript
    typeof null; // object

    typeof undefined; // undefined
```

> 怎么理解这个 null 呢？相当于每个人都会有,但此时此刻没有。你可以理解为空。比如每个人都会有男朋友，但此刻没有。不就是空。
> 那怎么理解 undefined 首先它在 JS 属于一种数据类型，假设你不知道暖男，渣男是什么。你可以先表示为未定义。第一次分手时，是不是就有了一个清晰的定义?

<pre>
但在JS中两者非常容易混淆，但是含义完全不同。
</pre>

```JavaScript
    var foo;

　　alert(foo == null); // true

　　alert(foo == undefined); // true

　　alert(foo === null); // false

　　alert(foo === undefined); // true
```

> 实践当中 null 来说 几乎没人用。为什么呢？写代码自然是写有用的代码，为 null 的话。那还怎么执行

### <strong style="color:black;">4. 全局变量难以控制</strong>

<pre>
Javascript的全局变量,在所有模块中都是可见的;任何一个函数内部都可以生成全局变量,这大大加剧了程序的复杂性。
</pre>

```JavaScript
    a = 1;
　　(function(){
　　　　b=2;
　　　　alert(a);
　　})(); // 1
　　alert(b); //2

```

> 如果把 function 理解一个人家的话。那随便一个人就可以进出你家，这是不是让你非常没有安全感?让你时刻保护自身安全以及私有财产

### <strong style="color:black;">5. 自动插入行尾分号</strong>

<pre>
    Javascript的所有语句，都必须以分号结尾。但是，如果你忘记加分号，解释器并不报错，而是为你自动加上分号。
有时候，这会导致一些难以发现的错误。
</pre>

比如，下面这个函数根本无法达到预期的结果，返回值不是一个对象，而是 undefined。

```JavaScript
function(){

　　　　return
　　　　　　{
　　　　　　　　i=1
　　　　　　};

　　}
```

原因是解释器自动在 return 语句后面加上了分号。

```JavaScript
function(){

　　　　return;
　　　　　　{
　　　　　　　　i=1
　　　　　　};

　　}
```

> 相对于这个设计而言,如果仅仅是因为书写格式就判为缺陷,这不太妥当.我觉得只需要按照格式来编程就好

### <strong style="color:black;">6. 加号运算符</strong>

<pre>
+号作为运算符，有两个含义，可以表示数字与数字的和，也可以表示字符与字符的连接。
</pre>

```JavaScript
    alert(1+10); // 11

　　alert("1"+"10"); // 110
```

<pre>
如果一个操作项是字符，另一个操作项是数字，则数字自动转化为字符。
</pre>

```JavaScript
    alert(1+"10"); // 110

    alert("10"+1); // 101
```

这样的设计，不必要地加剧了运算的复杂性，完全可以另行设置一个字符连接的运算符。

> 态度中立,不过增加了运算的复杂性(必然有相对的判断操作项是数字还是字符的代码)。我觉得这个也不算缺陷。我们所认知的加号运算符两边都应该为数才可以计算。或许有人认为如果这样写应该报错就好了，这样符合我们所认知的。我并没有觉得这样本身是错的。不这样写就不会增加运算的复杂性

### <strong style="color:black;">7. NaN</strong>

<pre>
NaN是一种数字，表示超出了解释器的极限。它有一些很奇怪的特性：
</pre>

```JavaScript
NaN === NaN; //false

NaN !== NaN; //true
alert( 1 + NaN ); // NaN
```

> 与其设计 NaN，不如解释器直接报错，反而有利于简化程序。s

### <strong style="color:black;">8. 数组和对象的区分</strong>

<pre>
由于Javascript的数组也属于对象（object），所以要区分一个对象到底是不是数组，相当麻烦。Douglas Crockford
的代码是这样的：
</pre>

```JavaScript
if ( arr && typeof arr === 'object' && typeof arr.length === 'number' && !arr.propertyIsEnumerable
('length')){

　　alert("arr is an array");

}
```

### <strong style="color:black;">9. == 和 ===</strong>

<pre>
==用来判断两个值是否相等。当两个值类型不同时，会发生自动转换，得到的结果非常不符合直觉。
</pre>

```JavaScript
    "" == "0" // false

　　0 == "" // true

　　0 == "0" // true

　　false == "false" // false

　　false == "0" // true

　　false == undefined // false

　　false == null // false

　　null == undefined // true

　　" \t\r\n" == 0 // true
```

> 因此，推荐任何时候都使用"==="（精确判断）比较符。

### <strong style="color:black;">10. 基本类型的包装对象</strong>

<pre>
Javascript有三种基本数据类型：字符串、数字和布尔值。它们都有相应的建构函数，可以生成字符串对象、数字对象和
布尔值对象。
</pre>

```JavaScript
    new Boolean(false);

　　new Number(1234);

　　new String("Hello World");
```

与基本数据类型对应的对象类型，作用很小，造成的混淆却很大。

```JavaScript
    alert( typeof 1234); // number

　　alert( typeof new Number(1234)); // object
```

完！

> 不过，有些设计确实为缺陷，从 JavaScript 诞生到现在为止，依然 JavaScript 语言依然保持活跃，受开发者青睐,经过市场的检验会之后修复这些缺陷。如果你不去改变,那就享受现在吧,毕竟未来须改变,而不是一味独醉

文章引用：

[JavaScript 的历史](https://zh.wikipedia.org/wiki/JavaScript#%E5%8E%86%E5%8F%B2)

[JavaScript 诞生记](http://www.ruanyifeng.com/blog/2011/06/birth_of_javascript.html)

[JavaScript 的 10 个设计缺陷](http://www.ruanyifeng.com/blog/2011/06/10_design_defects_in_javascript.html)
