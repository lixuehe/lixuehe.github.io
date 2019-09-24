---
layout: post
section-type: post
category: tech
tags: [ 'tutorial' ]
---

# ECMAScript 6 简介

### 一.ECMAScript 6.0（以下简称 ES6）

* 是 JavaScript 语言的下一代标准;

* ECMAScript 和 JavaScript 的关系是:ECMAScript 6.0（以下简称 ES6）是 JavaScript 语言的下一代标准，前者是后者的规格，后者是前者的一种实现（另外的 ECMAScript 方言还有 JScript 和 ActionScript）。日常场合，这两个词是可以互换的。

### 二.查看部署进度

* 查看 Node 已经实现的 ES6 特性:

  ```bash
  $ node --v8-options | grep harmony
  ```

* [ruanyf.github.io/es-checker](http://ruanyf.github.io/es-checker)，查看浏览器支持 ES6 的程度；

* 查看你正在使用的 Node 环境对 ES6 的支持程度

  ```bash
  $ sudo npm install -g es-checker
  $ es-checker
  ```

### 三.Babel转码器：将 ES6 代码转为 ES5 代码

1. 在项目目录中，安装 Babel

   ```bash
   $ npm install --save-dev @babel/core
   ```

2. 配置文件.babelrc,存放在项目的根目录下，用来设置转码规则和插件，基本格式如下:

   ```javascript
   {
     "presets": [],
     "plugins": []
   }
   ```

   `presets`字段设定转码规则，官方提供以下的规则集，你可以根据需要安装:

   ```bash
   # 最新转码规则
   $ npm install --save-dev @babel/preset-env
   
   # react 转码规则
   $ npm install --save-dev @babel/preset-react
   ```

   然后，将这些规则加入`.babelrc`。

   ```javascript
    {
       "presets": [
         "@babel/env",
         "@babel/preset-react"
       ],
       "plugins": []
     }
   ```

   ### 四.命令行转码--命令行工具`@babel/cli`

   1. 安装命令：

      ```bash
      $ npm install --save-dev @babel/cli
      ```

   2. 基本用法

      ```bash
      # 转码结果输出到标准输出
      $ npx babel example.js
      
      # 转码结果写入一个文件
      # --out-file 或 -o 参数指定输出文件
      $ npx babel example.js --out-file compiled.js
      # 或者
      $ npx babel example.js -o compiled.js
      
      # 整个目录转码
      # --out-dir 或 -d 参数指定输出目录
      $ npx babel src --out-dir lib
      # 或者
      $ npx babel src -d lib
      
      # -s 参数生成source map文件
      $ npx babel src -d lib -s
      ```

   

## 一. let 和 const 命令

### 1.let 命令

* 用于声明变量,所声明的变量，只在`let`命令所在的代码块内有效；

  案例：

  ```
  var a = [];
  for (var i = 0; i < 10; i++) {
    a[i] = function () {
      console.log(i);
    };
  }
  a[6](); // 10
  -----------------------------
  var a = [];
  for (let i = 0; i < 10; i++) {
    a[i] = function () {
      console.log(i);
    };
  }
  a[6](); // 6
  ```

  **`for`循环还有一个特别之处，就是设置循环变量的那部分是一个父作用域，而循环体内部是一个单独的子作用域。**

  ```
  for (let i = 0; i < 3; i++) {
    let i = 'abc';
    console.log(i);
  }
  // abc  abc abc
  //输出了 3 次abc,这表明函数内部的变量i与循环变量i不在同一个作用域，有各自单独的作用域。
  ```

* 不存在变量提升：所声明的变量一定要在声明后使用，否则报错；

* 暂时性死区：

  * 在代码块内，使用`let`命令声明变量之前，该变量都是不可用的。这在语法上，称为“暂时性死区”（temporal dead zone，简称 TDZ）。

  * 只要块级作用域内存在`let`命令，它所声明的变量就“绑定”（binding）这个区域，不再受外部的影响。

  *  ES6 明确规定，如果区块中存在`let`和`const`命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，就会报错。

  * 暂时性死区的本质就是，只要一进入当前作用域，所要使用的变量就已经存在了，但是不可获取，只有等到声明变量的那一行代码出现，才可以获取和使用该变量。

  ````
  var tmp =123;
  if(true){
      // tmp='abc';        //ReferenceError: tmp is not defined
      // console.log(tmp); //ReferenceError: tmp is not defined
      let tmp;
      console.log(tmp);    //undefined
      tmp=123;
      console.log(tmp);    //123
  }
  ````

* 不允许重复声明：`let`不允许在相同作用域内，重复声明同一个变量，不能在函数内部重新声明参数。

  ```
  // 报错
  function func() {
    let a = 10;
    let a = 1;
  }
  -----------------------
  function func(arg) {
    let arg;
  }
  func() // 报错
  
  function func(arg) {
    {
      let arg;
    }
  }
  func() // 不报错
  ```

### 2.块级作用域

* 为什么需要块级作用域？

  * 第一种场景，内层变量可能会覆盖外层变量。

    ```javascript
    var tmp = new Date();
    
    function f() {
      console.log(tmp);
      if (false) {
        var tmp = 'hello world';
      }
    }
    f(); // undefined  变量提升，导致内层的tmp变量覆盖了外层的tmp变量。
    ```

  * 第二种场景，用来计数的循环变量泄露为全局变量。

    ```javascript
    var s = 'hello';
    
    for (var i = 0; i < s.length; i++) {
      console.log(s[i]);
    }
    console.log(i); // 5
    ```

* ES6的块级作用域：

  ```
  function f1(){
      let n=5;
      if(true){
          let n=10;
          console.log(n);//10
      }
      console.log(n);//5
  }
  f1();  // 10  5
  -----------------------------
  f1();  // 10  10
  function f1(){
      var n=5;
      if(true){
          var n=10;
          console.log(n);//10
      }
      console.log(n);//10
  }
  f1();  // 10  10
  ```

* let 的块级作用域，代替了匿名函数表达式（匿名 IIFE）

  ```
  // IIFE 写法
  (function () {
      var tmp1 = 123;
      console.log(tmp1);    //123
  }());
      // console.log(tmp1); //报错
  // 块级作用域写法
  {
      let tmp2 = 456;
      console.log(tmp2);  //456
  }
      console.log(tmp2);  //报错
  {
      var tmp3 = 789;
      console.log(tmp3); //789
  }
      console.log(tmp3);  //789
  ```

* 块级作用域与函数声明

  * 允许在块级作用域之中声明函数;
  * 块级作用域之中，函数声明语句的行为类似于`let`，在块级作用域之外不可引用。

* 规则：

  * 允许在块级作用域内声明函数;
  * 函数声明类似于`var`，即会提升到全局作用域或函数作用域的头部;
  * 函数声明还会提升到所在的块级作用域的头部。

  案例：

  ```
  {
      let a = 'secret';
      console.log(f());  //secret，函数被提升
      function f() {
        return a;
      }
      console.log(f()); //secret
  }
      console.log(f());  //secret  
    
    // 块级作用域内部，优先使用函数表达式
    {
      let a = 'secret';
      let f = function () {
        return a;
      };
      console.log(a);  //secret
    }
      // console.log(a);//报错
  ```

* 块级作用域必须使用大括号：{}

