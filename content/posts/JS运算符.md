---
title: "JS运算符"
date: 2019-11-09T10:50:36+08:00
draft: false
---

# JavaScript运算符
<strong>目录</strong>

* <strong><a href="#算术运算符">算术运算符</a></strong>
* <strong><a href="#比较运算符">比较运算符</a></strong>
* <strong><a href="#布尔运算符">布尔运算符</a></strong>
* <strong><a href="#二进制运算符">二进制运算符</a></strong>
* <strong><a href="#其他运算符">其他运算符</a></strong>
  
  * <strong><a href="#点运算符">点运算符</a></strong>
  * <strong><a href="#void运算符">void运算符</a></strong>
  * <strong><a href="#逗号运算符">逗号运算符</a></strong>



## <h2 id="算术运算符">算术运算符</h2>
```JavaScript
加法( + ) 减法( - ) 除法( / ) 乘法( * ) 求余( % ) 幂( ** )  

递增( ++) 递减( --) 一元负号( - ) 一元正号( - )
```

> 注：第一行是二元操作符,第二行是一元操作符。区别在于是有一个还是有两个操作数。例：一元的意思只有一个操作数 +2。递增和递减分前置和后置。区别在于先加减还是后加减

```JavaScript
// 二元操作符
    // 例：  
          1+1

// 一元操作符
    // 例:   
          +1

// 前置递增/递减(递增/递减只能用于变量,常量不可用)
    // 例：
          let a = 1
          ++a
          --a
    
// 后置递增/递减(递增/递减只能用于变量,常量不可用)
    // 例：
          let a = 2 
          a--
          a++

```

#### Number 类型运算
```JavaScript
let a = 1
let b = 1

console.log(a+b)    //加  2
console.log(a-b)    //减  0
console.log(a*b)    //乘  1
console.log(a/b)    //除  1
console.log(a%b)    //求余 0
console.log(2**2)   //幂 4
console.log(a++)    //1   a等于？
console.log(a--)    //2   a等于？
console.log(++b)    //2   b等于？
console.log(--b)    //1   b等于？ 
let c = - 1
console.log(-c)     //1  负负得正
console.log(+c)     //-1 正号可以把不是数值转换成数值的最快方法
```

#### String 类型运算

```JavaScript
console.log("hello" + " " + "world")    //hello world
```

> 字符串的类型可以用加号( + )  是把两个字符串连接起来

## <h2 id="比较运算符">比较运算符</h2>


```JavaScript
 大于( > )  小于( < )   大于等于( >= )  小于等于( <= )

 等于( == )  不等于( != )  全等于( === )  不全等( !== )
```

第一行比较运算符去理解字面意思即可

第二行在JavaScript里会有点些不一样.不过我现在告诉你等于(==)和不等于(!=)应该抛弃,拥抱全等于(===)和不全等(!==)接下来会说为什么这样

``` JavaScript
console.log('1' == 1)   //true 
//这是为什么是true？？？字符串1 和 数值1是相等的吗？ 按照常理肯定不是的但是这是为什么答案是true呢？
console.log("1" != 1)   //false
// 按照逻辑应该是true 但是结果确实false   
//我们来看 全等于( === )


console.log( "1" === 1)   //false 
console.log( "1" !==1 )   //true
//现在你会觉得 ok 这很正常了。因为它们类型不同 一个number 一个string 本该不相同


//这是因为你在使用 等于( == )或不等于( != ) 时  类型会发生自动转换.而 全等于 ( === )则不会这样
```
## <h2 id="布尔运算符">布尔运算符</h2>

```javaScript

与(&&)  : expr1 && expr2  如果expr1可转换为true 则返回expr2。否则返回expr1
或(||)  : expr1 || expr2  如果expr1可转换为true 则返回expr1。否则返回expr2
非(!)   ：!expr           如果expr可转换为true  则false  否则返回true    

console.log(true && false)   //false

console.log(false || true)   //true

console.log(!true)            //false

// 还有一中双重非和多重非

console.log(!true)    //false
console.log(!!true)      //true
console.log(!!!true)        //false
console.log(!!!!true)           //true 
```

短路逻辑 

```
console && console.log  &&  console.log('hi')

由于逻辑表达式是从左到右并且expr都会去判断是不是true或者false.
防止代码报错我们先判断有没有console。有就在判断console.log有就执行console.log('hi')
```


## <h2 id="按位运算符">二进制运算符</h2>
按位运算符呢是将其操作数当作32位的比特序列。但是返回值依然是标准的JavaScript数值

> 前言: 当十进制转换为二进制数如下(缩写成了8位。在js里是32位)

```JavaScript
前8位
                      128 64 32 16  8 4 2 1(每次都*2)
例如：数字9二进制表示   0  0  0  0  1 0 0 1    
对照表依次相加9== 0000 1001
9的      原码       反码       补码
正数     00001001   00001001   00001001
负数     10001001   11110110   11110111

可以看出正数的原码反码补码都一样.唯有负数不一样。左边第一位是符号位(0是正数,1是负数)

负数的反码是：除符号位其余取反。

补码是在反码的基础上+1. 

负数的补码怎么回到原码? 在补码的基础上除符号位其余取反再+1即使负数的原码
```

例：按位运算符实例

```JavaScript

// 9 的二进制形式 00000000 00000000 00000000 00001001
// 6 的二进制形式 00000000 00000000 00000000 00000110

1. 按位与(AND) a & b ：对比每个比特位,只有两个操作数相应的比特位都为1时则为1.否则为0

例 (9 & 6):
        console.log(9 & 6)    //0  
    二进制形式：   1001   //9
                   0110   //6
    &都为1时则为1  0000   //0


2. 按位或(OR) a | b ：对比每个比特位,当两个操作数相应的比特为至少有一个为1则为1,否则为0

例(9 | 6):
        console.log(9 | 6)    //15
    二进制形式:    1001   //9
                   0110   //6
    |至少1个则为1  1111   //15

3. 按位异或(XOR) a ^ b : 对比每个比特位,当两个操作数相应的比特位有且只有一个1时则为1,否则为0

例(9 ^ 6):
        console.log(9 ^ 6)    //15
    二进制形式：   1001   //9
                   0110   //6
    ^有且1个1则为1 1111   //15

4. 按位非(NOT) ~ b   ：反转操作数的比特位,即1变0 即0变1

例(~9):
        console.log( ~9 )     //-10
    二进制形式：   00000000 00000000 00000000 00001001      //9
    ~              11111111 11111111 11111111 11110110      //-10

    对任一数值x进行按位非操作的结果为 -(x+1) 例如：~9  结果为-10
    
5. 左移(Left shift) a << b :将a的二进制形式向左移b(<32),右边用0填充

例( 2 << 2 ):
            console.log( 2 << 2)    //8
    二进制形式：    0010    //2
    左移两位:       1000     //8
    
    x << y   等于 x * (2**y)    例如： 2 << 2  结果为 2*(2**2) = 8

6. 有符号右移 a >> b : 将a的二进制形式向右移动b(<32),丢弃被移出的位

例( 4/-4 >> 1 ):
            console.log(4 >> 1)     //2
    二进制形式：     0100     //4
    >>1              0010     //2
            console.log(-4 >> 1)    //-2符号位保留
    二进制形式:    1 1100     //-4
    >>1            1 1110     //-2
                    

7. 无符号右移 a >>> b : 将a的二进制形式向右移动b(<32),丢弃被移出的位,并使用0在左侧填充

例( 4/-4 >>> 1 ):
          console.log( 4 >>> 1)   //2
    二进制形式：      0100    //4
    >>>1              0010    //2
          console.log( -4 >>> 1 ) //2147483646  注意是无符号右移
    二进制形式：11111111 11111111 11111111 11111100
    >>>1        01111111 11111111 11111111 11111110 

```




## <h2 id="其他运算符">其他运算符</h2>
### <h3 id="点运算符">1. 点运算符</h3>
点运算符一般是调用对象的属性值或者给对象的属性名赋值

```JavaScript
    对象.属性名 = 属性值

let name={
  name:'xiaoxiao'
}


console.log(name.name)    //xiaoxiao

console.log((name.name='nihao',name.name))    //nihao


但有个例外是。比如点运算符前面不是对象为什么也有属性？

let a = 1

console.log(a.toString())   //"1"

这是因为JS在解析时,发现点运算符前面不是对象会把它封装成一个对象。但是呢用完之后就没了回归到原来

我们来看看下面的代码,这次我们自定义一个属性
let b = 1

console.log(b.name = 'xiaoxiao')    //"xiaoxiao" 

console.log(b.name)   //undefined 
因为变量b本身不是一个对象,当定义属性时只有当时有效,再次调用就属于未定义了
```

### <h3 id="void运算符">2. void运算符</h3>

```JavaScript
void 运算符对给定的表达式进行求值,然后返回undefined

void 1    //undefined

void (1+1)    //undefined

void console.log('hi')    //hi undefined
```

### <h3 id="逗号运算符">3. 逗号运算符</h3>
对它的每个操作数求值(从左到右),并且返回最后一个操作数的值

```JavaScript
let a = 1

console.log(a++,a)    //1  2  (这相当于一个函数接收了两个参数)

//以下是作为逗号运算符的一个结果。只会返回最后一个操作数的值
a = 1 
console.log((a++,a))  //2  
```
