---
title: JavaScript学习笔记（三）——引用类型
date: 2017-10-31
---

在ECMAScript中引用类型是一种数据结构，用于将数据和功能组织在一起，也可称为做**类**,但ECMAScript**不具备像传统面向对象语言所支持的类和接口等基本结构**，故可以称引用类型为**对象定义**，因为它们描述的是一类对象具有的方法和属性。

## 1.Object类型

### 1.1 创建Object实例有两种方法：
``` javascript
/*通过构造函数创建*/
var person = new Object();
person.name = "kym";
person.age = 22;

/*通过对象字面量表示法创建*/
var person = {
  "name": "kym",         //属性名可以使用
  age: 22
};                       //注意不要遗漏分号

var person = {};         //不会调用构造函数
person.name = "kym";
person.age = 22
```

### 1.2 访问对象属性：
``` javascript
alert(person.name);      //kym
alert(person["age"]);    //22

/*方括号能通过变量来访问或属性明中含有会导致语法错误的字符*/
var propertyName = "name";
alert(person[propertyName]);      //kym
preson["first name"] = "kym";
```

## 2.Array类型

ECMAScript中的Array类型（数组）每一项都可以保存任何类型的数据。它的大小可以动态调整。

### 2.1 创建Array类型有两种方法：
``` javascript
/*构造函数法*/
var color1 = new Array();  

//20会自动转换为length值，即声明了一个长度为20的数组
var color2 = new Array(20);
color2.length = 19;               //length值可以调整

//因为数组最后一项的索引始终是length-1，故可以通过length添加数据
color2[color2.length] = "green"   //在第20个位置添加一个数据"green"
var color3 = new Array("blue","red","white");

/*数组字面量表示法（不会调用构造函数）*/
var test1 = ["a","b","c"];
var test2 = [];                 //空数组
```

### 2.2 Array.isArray()方法

Array.isArray()方法用来检测最终某值是不是数组，而不管它是在哪个全局环境中创建的。
``` JavaScript
if(Array.isArray(value)){
  //执行某些数组操作
}
```

### 2.3 转换方法

除所有对象都具有的toLocaleString()、toString()、valueof()方法外。数组还具有一个join()方法，可以用不同的分隔符来构建这个字符串，为空或者值为undefined时，会默认为逗号。
``` JavaScript
var colors = ["blue", "green", "red"];
alert(colors.join("|"));    //blue|green|red|
```

### 2.4 栈方法

ECMAScript提供了两个方法：push()和pop()来实现类似栈的方法。

- push()向末尾添加数据；
- pop()取走末尾的最后一项数据。
``` JavaScript
var test = ["a", "b", "c"];
var count1 = test.push("d", "e");        
alert(count1);    //5
count1 = test.push("f");
alert(count1);    //6
var count2 = test.pop()
alert(count2);    //f
```

### 2.5队列方法

通过push()方法和一个shift()方法来实现。

- shift()：取走开头的第一项数据；
- unshift()：向数组前端添加任意个项并返回新数组的长度；
``` javascript
var colors = new Array();
var count = colors.unshift("red", "green");
alert(count);          //2
```

### 2.6重排序方法

reverse()方法会反转数组项的顺序。
``` javascript
var test = [1,2,3,4,5];
test.reverse();
alert(test);        //5,4,3,2,1
```
sort()方法按照升序来排列数组项，它会调用每个数组项的toString()方法，然后比较得到的字符串；但按照字符比较，10的值位于5的前面，这时候sort()方法可以接受一个比较函数作为参数以便我们指定哪个值在哪个值的前面。
``` javascript
function compare(value1, value2){
  if(value1 < value2){
    return -1;
  }else if(value1 > value2){
    return 1;
  }else{
    return 0;
  }
}

var values = [0, 10, 5, 15, 8];
values.sort(compare);
alert(values);            //0, 5, 8, 10, 15
```
reverse()与sort()方法的返回值是经过排序之后的数组。

### 2.7 操作方法

concat()方法可以基于当前数据中所有的项创建一个新数组（即它只是创建一个原有数组的副本，不会改变原有数组的数据），向方法中传递的参数可以为一个数据也可以为数组。

slice()方法有两个参数（一个也可以，表示起始位置），分别表示返回项在原数组中的位置，不会改变原有数组的数据。如果传入的值是负数，则位置为数组项数加该值。

``` javascript
var colors = ["red", "blue", "black"];
var colors1 = colors.concat("yellow", ["green", "brown"]);
var colors2 = colors.slice(0, 2);
alert(colors1);                 //red,blue,black,yellow,green,brown
alert(colors2);                 //red,blue
```

splice()方法用来向数组中插入、删除、替换数据项。该方法始终返回一个删除项组成的数组，如果没有删除项，则返回一个空数组。
- 删除：两个参数，第一个参数代表要删除的起始位置，第二个参数代表删除项的个数；
- 插入：三个参数，前两个和删除相同，不过第二个参数值为0，第三个参数为要插入的项；
- 替换：三个参数，与插入相同，不过第二个参数为要删除的项的个数，插入的项与删除的项不必相同。

``` javascript
var numbers = [1, 2, 3, 4];
var number1 = numbers.splice(0, 1);
alert(number1);                  //1
alert(numbers);                  //2,3,4
var number2 = numbers.splice(1, 0, 5, 6);
alert(number2);                  //
alert(numbers);                  //2,5,6,3,4
var number3 = numbers.splice(2, 1, 8, 9);
alert(number3);                 //6
alert(numbers);                 //2,5,8,9,3,4
```

### 2.8 位置方法

indexOf()方法和lastIndexOf()方法，前者有两个参数值，第一个参数值为要查找的项，第二个为（可选），查询索引的起点。后者从末尾开始向前查找。返回值为数组中的位置，没有找到会返回-1.第一个参数和数组中的每一项进行比较时采用的是全等。

### 2.9 迭代方法

数组定义了五种迭代方法，里面有两个参数：第一个参数为要在每一项上运行的函数（该函数包括三个参数：数组项的值，数组项在数组中的位置，数组对象本身），第二个值为（可选）运行该函数的作用域对象。

``` javascript
var numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];
//every方法表示必须所有项都满足条件才返回true;
var everyResult = numbers.every(function(item, index, array){
  return item > 2;
});
alert(everyResult);     //false     

//some方法表示只要有一项满足就返回true;
var someResult = numbers.some(function(item, index, array){
  return item > 2;
});
alert(someResult);      //true

//filter方法利用指定的函数返回函数会返回true的项；
var filterResult = numbers.filter(function(item, index, array){
  return item > 2;
});
alert(filterResult);   //3,4,5,4,3

//map方法的返回值为每个项通过函数之后的结果；
var mapResult = numbers.map(function(item, index, array){
  return item * 2;
})
alert(mapResult);       //2,4,6,8,10,8,6,4,2

//forEach方法只是对数组作用函数，没有返回值
numbers.forEach(function(item, index, array){
  //某些操作
})

```

### 2.10 归并方法

reduce()方法和reduceRight()方法，迭代数组的所有项，构建一个最终的返回值，前者从数组的第一项开始，后者从最后一项开始往后。有两个参数，第一个参数为在每一项上调用的函数（函数具有四个参数，分别为：前一个值，当前值，想的索引，数组对象），第二项为（可选）归并的基础值。

``` javascript
//用reduce方法实现一个数组求和；用reduceRight同样可以实现
var values = [1, 2, 3, 4, 5];
var sum = values.reduce(function(prev, cur, index, array){
  return prev + cur；
})
alert(sum);             //15
```