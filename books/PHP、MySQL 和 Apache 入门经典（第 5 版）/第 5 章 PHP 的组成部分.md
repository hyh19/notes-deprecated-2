[TOC]

# 第 5 章 PHP 的组成部分

## 5.1 变量

一个变量由你所选取的一个名字以及前面的一个美元符（$）组成。

一个分号（`;`）（又叫做指令终止符，instruction terminator）用来结束一条 PHP 语句。

**注意：每一条语句都要以分号结束，不然会报错。**

### 5.1.1 全局变量

赋给一个变量的值只在它所在的函数或脚本中有效。变量作用域也可以为全局的，脚本之间共享一个变量的值，第 7 章会详细说明。

### 5.1.2 超全局变量

[Predefined Variables](http://php.net/manual/en/reserved.variables.php)

在脚本中使用超全局变量对于创建安全的应用程序很重要，因为超全局变量减少了用户注入式攻击进入到脚本的可能性。

## 5.2 数据类型

http://php.net/manual/en/language.types.php

PHP 是类型宽松的语言，这意味着它将在数据被赋给每个变量的时候才确定数据类型。

查询 API 可以直接把关键词放到 PHP 网址的后面，例如查询 `is_null` 的用法：http://php.net/is_null

连接操作符用一个句点（.）表示。它把两个操作数都当做是字符串，把右操作数附加到左操作数上。后面章节有详细说明。

**程序清单 5.1 测试一个变量的类型**
```php
<?php
$testing;
echo "is null? ".is_null($testing);
echo "<br/>";

$testing = 5;
echo "is an integer? ".is_int($testing);
echo "<br/>";

$testing = "five";
echo "is a string? ".is_string($testing);
echo "<br/>";

$testing = 5.024;
echo "is a double? ".is_double($testing);
echo "<br/>";

$testing = true;
echo "is boolean? ".is_bool($testing);
echo "<br/>";

$testing = array('apple', 'orange', 'pear');
echo "is an array? ".is_array($testing);
echo "<br/>";
echo "is numeric? ".is_numeric($testing);
echo "<br/>";
?>
```

输出结果：

is null? 1<br/>is an integer? 1<br/>is a string? 1<br/>is a double? 1<br/>is boolean? 1<br/>is an array? 1<br/>is numeric? <br/>

### 5.2.1 使用 `settype()` 来改变变量的数据类型

http://php.net/settype

**程序清单 5.2 使用 `settype()` 修改一个变量的类型**
```php
<?php
$undecided = 3.14;
echo "is ".$undecided." a double? ".is_double($undecided)."<br/>";

settype($undecided, 'string');
echo "is ".$undecided." a string? ".is_string($undecided)."<br/>";

settype($undecided, 'integer');
echo "is ".$undecided." an integer? ".is_integer($undecided)."<br/>";

settype($undecided, 'double');
echo "is ".$undecided." a double? ".is_double($undecided)."<br/>";

settype($undecided, 'bool');
echo "is ".$undecided." a boolean? ".is_bool($undecided)."<br/>";
?>
```

输出结果：

is 3.14 a double? 1<br/>is 3.14 a string? 1<br/>is 3 an integer? 1<br/>is 3 a double? 1<br/>is 1 a boolean? 1<br/>

### 5.2.2 通过类型转换改变变量的数据类型

http://php.net/gettype

使用 `settype()` 来改变一个已有变量的类型和使用类型转换改变变量类型的主要区别在于：**类型转换会生成一个拷贝**，而保持原来的变量不动。

**程序清单 5.3 对一个变量进行类型转换**
```php
<?php
$undecided = 3.14;
$holder = (double)$undecided;
echo "is ".$holder." a double? ".is_double($holder)."<br/>";

$holder = (string)$undecided;
echo "is ".$holder." a string? ".is_string($holder)."<br/>";

$holder = (integer)$undecided;
echo "is ".$holder." an integer? ".is_integer($holder)."<br/>";

$holder = (double)$undecided;
echo "is ".$holder." a double? ".is_double($holder)."<br/>";

$holder = (boolean)$undecided;
echo "is ".$holder." a boolean? ".is_bool($holder)."<br/>";

$holder = (integer)$undecided;
echo "is ".$holder." an integer? ".is_integer($holder)."<br/>";

echo "<hr/>";
echo "original variable type of $undecided: ";
echo gettype($undecided);
?>
```

输出结果：

is 3.14 a double? 1<br/>is 3.14 a string? 1<br/>is 3 an integer? 1<br/>is 3.14 a double? 1<br/>is 1 a boolean? 1<br/>is 3 an integer? 1<br/>original variable type of 3.14: double

### 5.2.3 为何测试类型

n/a

## 5.3 操作符和表达式

### 5.3.1 赋值操作符

使用赋值操作符的语句总是求得右操作数的值的一个副本。

### 5.3.2 算术操作符

n/a

### 5.3.3 连接操作符

连接操作符用一个句点（`.`）表示。它把两个操作数都当做是字符串，把右操作数附加到左操作数上。如
```php
"hello"." world"
```
返回
```
hello world
```

**不管和连接操作符一起使用的操作数是什么数据类型，它们都会被当做字符串对待，并且结果也总是字符串类型。**

### 5.3.4 复合赋值操作符

n/a

### 5.3.5 自动增加和减少一个整型变量

n/a

### 5.3.6 比较操作符

n/a

### 5.3.7 使用逻辑操作符创建复杂的测试表达式

n/a

### 5.3.8 操作符优先级

n/a

## 5.4 常量

[Magic constants](http://php.net/manual/en/language.constants.predefined.php)

[Predefined Constants](http://php.net/manual/en/reserved.constants.php)

常量必须使用 PHP 内建的 `define()` 函数来创建，随后这个常量是不能改变的，**除非再次明确地 `define()` 它**。

http://php.net/define

**按照惯例，常量的名字应该是大写的，常量只能使用常量名访问，不需要美元符号。**

**程序清单 5.4 定义和访问一个常量**
```php
<?php
define("THE_YEAR", "2012");
echo "It is the year ".THE_YEAR.".";
?>
```

输出结果：

It is the year 2012.

学习结束

复习 2018.2.25