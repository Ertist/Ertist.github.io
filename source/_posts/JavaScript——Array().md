---
title: JacaScript——Array()
date: 2018-3-28
---

数组是js中使用频率非常高的一种引用类型，有两种创建方式：

- 使用构造函数：
```javascript
var newob = new Array();    //括号中可包含数字（自动识别为length的长度），也可直接输入几段字符串。（new操作符可省略）
```
- 使用数组字面量表示法:
```javascript
var newob = ["a", "b", "c"];
```

## 检测数组

如何判断一个对象是不是数组？可以使用 ` instanceof() ` 操作符进行检验，但在多框架的网页中，数组的全局作用环境不一定只有一个，故在ES5新增加一个方法，使用Array.isArray()：
```javascript
if(Array.isArray(value)){
	//对数组进行操作
}
```

## 转换方法

