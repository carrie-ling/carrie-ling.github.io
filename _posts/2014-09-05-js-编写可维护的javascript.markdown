---
layout: postjs
title:  "编写可维护的javascript!"
date:   2014-09-25 17:46:13
tags: [postjs]
categories: 读"编写可维护的javascript!"一书有感

---
# 编写可维护的javascript #

## 第一部分 ##

### 1、编程风格 ###

1、代码质量检查工具，JsLint和JsHint

jsLint只是简单查找不符合Javascritpt模式的，错误的小工具。经过数年的变化，jslint已经成为一个有用的工具，不仅仅可以找出代码中潜在的错误，而且能针对你的代码贵出编码风格上的警告。

2、Crockford将他对javascript风格的观点分成了三个不同的部分。

>1、“javascript风格的组成部分（第一部分）”（http://javascritp.crockford.com/style1/html），包含基本的模式和语法。
>
>2、”javascript风格的组成部分（第二部分）“（http://javascript.crockfrod.com/style2.html），包含一般性的javascript惯用法。
>
>3、”javascript编程语言的编码规范“（http://javascript.crockford.com/code.html）这个规范更加全面，从前两部分中提炼出了编程风格的精华部分，同时增补了少量的编程风格的指引；

### 2、基本的格式化 ###

2.1 缩进层级

使用制表符进行缩进：每个缩进层级都用单独的制表符表示。所以一个缩进层级是一个制表符；

第一		制表符和缩进层级之间是一对一的关系，这是符合逻辑的。

第二		文本编辑器可以配置字表符的展现长度；

使用制表符缩进的弊端：

各个编辑器的制表符长度有一定的差异性，因此会对开发者造成一定的困惑。

使用空格符进行缩进：每个缩进由多个空格字符组成。在这种观点中有三种具体的做法：2个空格表示一个缩进，4个空格表示一个缩进，8个空格表示一个缩进。这三种做法在其他很多编程语言中都能找到渊源。

**很多团队选择4个空格的缩进；**

尽管是悬着制表符还是空格做缩进只是一种个人偏好，但绝对不要将两者混用，这非常重要；

2.2 语句结尾

javascript的语句结尾有两种形式

一是用分号结束例如下代码;另一种是空格

//合法的代码

	var name = "Nicholas";
	
	function sayName(){
		alert (name);
	}

//合法的代码但不推荐

	var name = "Nicholas"
	
	function sayName(){
		alert (name);
	}

2.3 行的长度

单行代码最长不超多80个字符，这个数值是来源于很久之前文本编辑器的单行最多字符限制；

2.4 换行

当一行长度达到单行最大字符数限制时，就需要手动将一行拆成两行。通常我们会在运算符后换行，下一行会政教两个层级的缩进；

	//好的做法：在运算符后换行，第二行追加两个缩进
	callAFunction(document,element,windwo."som string value",true,123,
			naviagator);
	//不好的做法，第二行只有一个缩进
	callAFunction(document,element,windwo."som string value",true,123,
		naviagator);
	//不好的做法，在运算符之前换行了
	callAFunction(document,element,windwo."som string value",true,123
		，naviagator);
当给变量赋值时，第二行的位置应该和赋值运算符的位置保持对齐，例如：

	var result = something +anotherThing +yetAnotherThing+somthingElse+
				anotherSomethingEllse;

2.5 空行

一般来讲，在下面这些场景中添加空行也是不错的主意。

1、在方法之间。

2、在方法中的局部变量（local variable）和第一条语句之间。

3、在多行或单行注释之前

4、在方法内的逻辑片段之间插入空行，提高可读性。

2.6 命名

“计算机科学只存在两个难题：缓存失效和命名”---PhilKarlton.

只要写代码，都会涉及变量和函数，因此变量和函数命名对于增强代码可读性至关重要，javasript是遵照了驼峰式大小写（Camel case）命名法，是有小写字母开始的，后续每个单词首字母大写，比如：

	var thisIsMyName;
	var anotherVariable;
	var aVeryLongVariableName;

大部分javascript程序员使用驼峰命名法来给变量和函数命名；

2.6.1 变量和函数

