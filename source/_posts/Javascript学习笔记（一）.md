---
title: Javascript学习笔记（一）
date: 2017-10-28 19:10
---

## 1.插入javascript

`<script>`标签用来向html页面中插入js。主要的插入方法有两种：直接在`<script>` `</script>`之间写入；引入外部文件。

**`<script>` 定义了六种属性：**

- async（可选）：在不妨碍页面中的其他操作同时，立即下载脚本；

- charstt（可选）：通过scr指定的代码的字符集（大部分浏览器会忽略该值）；

- defer（可选）：脚本的执行可以延迟到整个页面都解析完毕（只对外部脚本有用）；

- language（已废弃）：表示编写的脚本所使用的语言；

- src（可选）：要执行的代码的外部文件；

type（可选）：脚本语言的内容类型（即MIME类型），一般为text/javascript;(不是必须，如果代码未指定type，默认还是为它)。

`<script>`嵌入js代码时，切记不能在里面任何地方出现`</script>`字符串，会产生错误，如：

``` javascript
<script type = "text/javascript">
	function sayScript(){
	  	alert("</script>");                 //不可这样写，可通过转义字符，如：alert<"\/script">
	}
</script>                   
```

带有scr属性的<script>中不应该再往<script></script>中间嵌入额外代码，否则只会执行下载的外部文件代码，不会执行嵌入的代码。现代的Web应用程序一般都会把全部的js引用放在<body>的最下面，这样做的目的是为了使页面快速呈现内容，不用等到js脚本渲染完毕再呈现，避免了延迟。

## 2.文档模式

通过文档类型（doctype）的切换实现。最初的两种文档类型为：混杂模式和标准模式，两种模式主要影响css内容的呈现。

## 3.基本概念

### 3.1 严格模式

严格模式是js定义的一种解析与执行模式，会对某些不确定和不安全的操作抛出错误。对整个脚本启动严格模式：在顶部输入 `"use strict"`;也可以指定函数启动严格模式，在函数体内第一行输入`"use strict"`。

### 3.2 变量

ECMAScript的变量为松散类型，即变量可以保持任何类型的数据，每个变量仅仅是一个用来保存值的占位符。如果在函数中使用var定义一个变量，那么变量在函数结束后会被销毁。

在定义变量时省略var操作符，则默认定义为全局变量，如：

``` javascript
function test(){
  	kymage = 20;      //kymage这个变量前面未定义，故在这里会默认定义为全局变量
  	/*但不推荐这种做法，会造成代码难以维护，在严格模式下会抛出ReferenceError错误*/
}
```

### 3.3 数据类型

数据类型包括物种基础数据类型（Undefined、Null、Boolean、Number、String）和一种复杂数据类型（object）。下面分别对这几种类型进行介绍和补充。

#### Number 类型

number类型使用IEEE754格式来表示整数和浮点数值。由前导决定位数默认为十进制（如0开头为8进制，0x开头为16进制），但是前导之后的数字如果超出界限，则前导无效，如：（八进制字面量在严格模式下无效）

``` javascript
var num1 = 070;   //为十进制56
var num2 = 079;   //前导无效，为十进制79
```

浮点数的值如果本身表示的是一个整数（如1.0），则该值会被转化为整数。浮点数的精度最高为17位，但是在进行计算事精度相比整数差很多，如：

``` javascript
var a = 0.1;
var b = 0.2;
alert(a + b);                       //结果为0.30000000000000004  
/*
* 0.1+0.2属于比较特殊的例子，这是基于IEEE754数值的浮点数计算的通病
*/
```

如果计算数值超过出了js数值范围（Number.MIN_VALUE ~ Number.MAX_VALUE），那么数值会被自动转换为特殊值Infinity（无穷）或-Infinity（负无穷）。

NaN(not a number)：一个特殊值，用来表示本来要返回一个数值的操作数未返回数值（NaN避免了报错）；有两个特点：1.任何设计NaN的操作都会返回NaN。2.NaN不与任何值相等，包括自身。

（数值转换等在这里暂不做笔记）

#### String 类型

即字符串，字符串用双引号或者单引号括起来（必须匹配，不能出现向'hello"这种），数字加引号做字符串处理，但可强制转换为数值。

#### Boolean 类型

只有两个字面值`true`和`false`

| 数据类型      |  转化为true值  | 转化为false值 |
| --------- | :--------: | :-------: |
| Boolean   |    true    |   false   |
| String    |  所有非空字符串   |  空字符串" "  |
| Number    | 非0数（包括无穷大） |   0、NaN   |
| Undefined |    n/a     | undefined |
| Object    |    任何对象    |   null    |

#### Undefined 类型

Undefined类型只有一个值，即undefined，表示未定义，在var声明变量未初始化的时候会默认为该值。

虽然未初始化的变量会默认undefined，但并不建议这样做。因为用typerof()方法操作变量显示undefined时我们就知道这个操作的变量并未声明，而不是尚未初始化。

####  Null 类型

同上面一样只有一个值，即null。从逻辑上讲，null表示一个空对象指针。

``` javascript
var objtest = null;
alert(typeof objtest);           //结果为"object"
```

如果定义的变量以后用来保存对象，那么最好将其初始化为null。实际上，undefined是派生自null，故`undefined == null`结果为true（全等时为false）。

#### Object 类型

可以理解为一组数据和功能的集合。 `var o = new object();`

### 3.4 操作符

用来操作数据值，下面列出几个比较容易混淆的几个。（位操作符等暂不做笔记）

**++的前置与后置：**声明一个变量`var a = 0;`；++a表示先递增再运行包含它的语句，a++表示先运行包含它的语句然后再递增。（--同理）

``` javascript
var a1 = 2;
var a2 = 2;
var b = 3;
alert(b = ++a1 + b);   //6
alert(a1);             //3   此时b = 6；
alert(b = a2++ + b);   //8
alert(a2);             //3
```

**乘性操作符的几个点：**Infinity * 0结果为NaN；Infinity / Infinity结果为NaN。

**关系操作符：**`var result = "23" < "3";    //true`出现这种情况的原因是因为此时两个数字是作为字符串来比较了，故比较的是字符编码。而`var result = "23" > 3;    //true`这是因为3此时为数值，故"23"会被转化为数值来进行操作。

**条件操作符：**`var max = (num1 > num2)? num1 : num2;`表示max的值为num1和num2中较大的。如果num1大，返回true，max = num1；如果num2大，返回false，max = num2；

### 3.5 语句

ECMA-262规定的一组流控制语句（基本与其他编程语言的差不多），挑选一些不常用和不熟练的做笔记。

**for-in 语句：**是一种精准的迭代语句，可以用来枚举对象的属性。

``` JavaScript
for (var propName in window) {
	document.write(propName);
}
/*
* 循环显示window对象的所有属性，每次循环都会将存在的属性名赋值给propName，过程会持续* 到将所有属性枚举一遍为止。
*/
```

如果迭代的对象值出现undefined或null，for-in语句会报错。

**label语句：**添加标签，以便将来使用（如在下面的break和continue语句中使用）。`label: statement`。

**break和continue语句：**用于在循环中精准的控制代码执行，`break;`会立即跳出当前循环，执行循环后边的语句；`continue;`也会立即跳出当前循环，但是会从循环的顶部继续进行。而`break label;` `continue label;`则会通过标签来控制跳出。如下所示：

``` javascript
var num = 0;
outermost:
for(var i = 0; i < 10; i++){
  for(var j = 0; j < 10; j++){
    if(i == 5 && j == 5)(
    	break outermost;
    )
    num++;
  }
}
alert(num);     //55
/*
* break outermost不仅跳出了嵌套循环，还跳出了上一层循环，故循环从i和j同是为5的时侯
* 结束了。如果只有break,num为95。
*/
```

``` javascript
var num = 0;
outermost:
for(var i = 0; i < 10; i++){
  for(var j = 0; j < 10; j++){
    if(i == 5 && j == 5)(
    	continue outermost;
    )
    num++;
  }
}
alert(num);     //95
/*
* continue outermost不仅跳出了嵌套循环，还跳出了上一层循环，循环继续从最上层的循环
* 继续开始。如果只有continue,num为99。
*/
```

**几点注意：**
- 严格模式下不能使用with语句，大量使用with会造成性能低下；
- switch语句在比较值时使用的是全等操作，因此不会发生类型转换；

### 3.6 函数

如同其它编程语言，函数是个核心概念，js中函数可以封装任意多条语句，并且可以在任何地方任何时候调用。函数可以有返回值（return），也可以没有。函数会在执行return语句之后立即结束，不会继续执行函数中下面的语句。
比较推荐的做法是让函数始终返回一个值或者永远都不要返回，否则函数有时候有返回值有时候没有，会给调用带来麻烦。
js函数没有重载，定义的两个函数如果名称相同，那么名字只属于后定义的函数。
ECMAScript中所有参数传递的都是值，不可能通过引用传递。（下章会涉及到）。

## 4.小结

本篇主要讲了JavaScript的很多基础知识，内容有所删减，写上去的大多是一些不太熟或者和其他编程语言略有区别的内容。很多方法如toString()、valueof()等都没有涉及，将在接下来的章节略微详细的讲解。知识点比较零碎，通过博客整理，记忆有所加强。写的自认为比较乱，以后有时间继续完善。