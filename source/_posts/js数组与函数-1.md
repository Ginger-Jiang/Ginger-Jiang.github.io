---
title: js数组与函数
date: 2020-01-06 18:34:26
tags:
  - JavaScript
categories:
  - JavaScript
---

```javascript
知识点 - 数组 - 函数 - 作用域 - 作用域链
```

### 数组

```javascript
// 数组:一组有序的数据
// 数组的作用:可以一次性储存多个数据
// 数组元素:数组中存储的每个数据,都可以叫数组的元素,比如:存储了3个数据,数组中3个元素
// 数组长度:就是数组的元素的个数,比如有3个元素,就说,这个数组的长度是3
// 数组索引(下标):用来存储或者访问数组中的数据的,索引从0开始,到长度减1结束
// 数组的索引和数组的长度的关系:长度减1就是最大的索引值

// 1、构造函数创建数组
var arr = new Array()   // 定义了一个叫arr的空数组
var arr = new Array(10)  // 定义了一个长度为10的数组，数组中每个数据为undefiend

// 2、字面量方式创建数组
var arr = []  // 创建了一个空数组
var arr = [1, 2, 3, 4]  // 创建了一个数组并赋值

// 获取数组中所有值
console.Log(arr)  // 打印数组与值
for(var i=0; i<arr.length; i++){ // 循环遍历获取数组中所有元素
    console.log(arr[i])
}
// 设置数组的值
arr[1] = 100  // 设置数组中下标为1的元素为100
// 获取数组中某个位置的值
var result = arr[1]  // 获取数组中下标为1的元素
// 获取数组的长度
console.log(arr.length）  // 打印arr数组的长度

// 总结数组
// 数组:存储一组有序的数据
// 数组的作用:一次性存储多个数据
// 数组的定义方式:
// 1.构造函数定义数组: var 数组名=new Array();
// 2.字面量方式定义数组: var 数组名=[];
// var 数组名=new Array();空数组
// var 数组名=new Array(值);数组定义了,有长度
// var 数组名=new Array(值1,值2,值3....);定义数组并且有多个数据
// var 数组名=[];空数组
// var 数组名=[值1,值2,值3];有三个数据
// 数组元素:就是数组中存储的数据
// 数组长度:就是数组中元素的个数
// 数组索引(下标):从0开始,到数组的长度减1结束
// 通过下标设置数组的元素值: 数组名[索引]=值
// 通过下标访问数组的元素值: 数组名[索引]
```

### 冒泡排序

```javascript
// 把数据按照一定的顺序排列(从小到大或从大到小)
var arr = [1, 3, 5, 7, 9, 2, 4, 6, 8]
for (var i = 0; i < arr.length - 1; i++) {
  for (var j = 0; j < arr.length - 1 - i; j++) {
    if (arr[j] > arr[j + 1]) {
      var tmp = arr[j]
      arr[j] = arr[j + 1]
      arr[j + 1] = tmp
    }
  }
}
```

### 函数

```javascript
// 函数：把重复的代码进行封装，在需要的时候直接调用函数即可。
// 函数作用：代码的重用(重复使用)
// 函数也是一种数据类型 --> function 类型

// 命名函数(有名字的函数叫命名函数)
function f1(){  // 使用function关键字定义了一个叫fn的函数 --> 函数声明
    console.log('函数体-->重复的代码')
}
f1() // 函数的调用-->执行函数体中的代码

// 匿名函数(函数如果没名字，就是匿名函数)
var f2 = function(){ // 函数表达式
    console.log('匿名函数')
}
f2() // 匿名函数的调用

// 函数的自调用
(function(){
    console.log('函数的自调用')
})()； // 函数声明的同时就调用

// 回调函数
// 如果一个函数作为参数，那么这个参数(函数)，可以叫做回调函数
function f2(fn){
    fn()
}
function f3(){
    console.log('f3')
}
f2(f3) // 将 f3 函数作为参数传入 f2 函数，在 f2 函数内部执行 f3 函数


// 形参与实参
// 函数在定义时，函数名后面的小括号里面的变量叫形参
// 函数在调用时，函数名后面的小括号里面的变量或值叫实参
// 函数返回值，在函数内部有return关键字,并且在关键字后面有内容,这个内容被返回了
// 例子
function sum(num1, num2){ // 形参
    var sum = num1 + num2
    console.log(sum)  // 打印结果 30
    return sum	// return 关键字 函数的返回值
}
var result = sum(10, 20) // (10,20)->实参 result->接受函数返回值

// arguments对象伪数组
// 如果一个函数不确定用户是否传入了参数，或者不知道传入了几个参数，可以使用 arguments 进行处理
function fn(){
	// arguments对象可以获取传入的每个参数的值
    console.log(arguments.length) // 5
    for(var i=0;i<arguments.length; i++){
        console.log(arguments[i]) // 输出每个实参的值
    }
}
fn(1,2,3,4,5)

// 小结函数
// 如果一个函数中有return ,那么这个函数就有返回值
// 如果一个函数中没有return,那么这个函数就没有返回值
// 如果一个函数中没有明确的返回值,那么调用的时候接收了,结果就是undefined
// 没有明确返回值:函数中没有return,函数中有return,但是return后面没有任何内容
// 函数没有返回值,但是在调用的时候接收了,那么结果就是undefined
// 变量声明了,没有赋值,结果也是undefined
// 如果一个函数有参数,有参数的函数
// 如果一个函数没有参数,没有参数的函数
// 形参的个数和实参的个数可以不一致
// return 下面的代码是不会执行的
```

### 作用域

```javascript
// 作用域：使用范围

// 全局变量：在函数外部使用 var 关键字定义的变量。
// 局部变量：在函数内部定义的变量是局部变量，外面不能使用。
// 隐式全局变量：在函数内部没有使用 var 定义的变量

// 全局作用域：全局变量的使用范围
// 局部作用域：局部变量的使用范围

// 示例
var num1 = 100 // 全局变量
function num() {
  var num2 = 200 // 局部变量
  num3 = 300 // 隐式全局变量
}
console.log(num1) // 100
console.log(num2) // undefiend
console.log(num3) // 300
```

### 作用域链

```javascript
var num = 10
function f1() {
  function f2() {
    function f3() {
      console.log(num)
    }
    f3()
  }
  f2()
}
f1() // 打印输出 10 一层一层往上寻找 num 的值
```

### 预解析(变量(函数)提升)

```javascript
// 预解析:提前解析代码，提升变量的声明与函数的声明
// 预解析做什么事?
// 把变量的声明提前了----提前到当前所在的作用域的最上面
// 函数的声明也会被提前---提前到当前所在的作用域的最上面

console.log(num) // 打印 undefined 不报错
var num = 10

f1() // 打印 '函数预解析'
function f1() {
  console.log('函数预解析')
}

var num1 = 100
function f1() {
  // var num1 等价于增加了此行代码
  console.log(num1) // 变量提升到了作用域最上面 --> 输出 undefined
  var num1 = 200
}

// 预解析小结
// 预解析中,变量的提升,只会在当前的作用域中提升,提前到当前的作用域的最上面
// 函数中的变量只会提前到函数的作用域中的最前面,不会出去
// 预解析会分段(多对的script标签中函数重名,预解析的时候不会冲突)
```
