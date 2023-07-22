# JavaScript var 作用域


## 作用域提升

var 声明变量，会将变量提升到全局作用域或函数作用域的头部。

## 暂时性死区

ES6明确规定，如果区块中存在let和const命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，就会报错。

在代码块内，使用let命令声明变量之前，该变量都是不可用的。这在语法上，称为“暂时性死区”（temporal dead zone，简称 TDZ）。

例如：

```js
// var 的情况
console.log(foo); // 输出undefined
var foo = 2;

// let 的情况
console.log(bar); // 报错ReferenceError
let bar = 2;

// 情况二

var tmp = 123;

if (true) {
  tmp = 'abc'; // ReferenceError
  let tmp;
}

// 情况三

if (true) {
  // TDZ开始
  tmp = 'abc'; // ReferenceError
  console.log(tmp); // ReferenceError

  let tmp; // TDZ结束
  console.log(tmp); // undefined

  tmp = 123;
  console.log(tmp); // 123
}
```

## 块级作用域的写法

```js
// IIFE 写法
(function () {
  var tmp = ...;
  ...
}());

// 块级作用域写法
{
  let tmp = ...;
  ...
}
```

## 块级作用域中的函数声明，ES6 支持

在 ES5 中，是不允许在块级作用域中声明函数的，但是为了兼容以前的旧代码，浏览器厂商没有遵守 ES5 的这一标准。

> ES6 引入了块级作用域，明确允许在块级作用域之中声明函数。ES6 规定，块级作用域之中，函数声明语句的行为类似于let，在块级作用域之外不可引用。

## 块级作用域是语句

本质上，块级作用域是一个语句，将多个操作封装在一起，没有返回值。

例如：我们无法从下方代码块中获得计算返回值

```js
{
  let t = f();
  t = t * t + 1;
}
```

那么如何获得上述块级作用域的计算结果，这时需要我们引入 **do**

```js
let x = do {
  let t = f();
  t * t + 1;
};
```

## const 声明常量

const声明一个只读的常量。一旦声明，常量的值就不能改变。

```js
const PI = 3.1415;
PI // 3.1415

PI = 3;
// TypeError: Assignment to constant variable.
```

const声明的变量不得改变值，这意味着，const一旦声明变量，就必须立即初始化，不能留到以后赋值。

```js
const foo;
// SyntaxError: Missing initializer in const declaration
```

## codePointAt()

JavaScript内部，字符以UTF-16的格式储存，每个字符固定为2个字节。对于那些需要4个字节储存的字符（Unicode码点大于0xFFFF的字符），JavaScript会认为它们是两个字符。
