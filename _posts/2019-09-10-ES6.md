---
layout: post
section-type: post
category: tech
tags: [ 'tutorial' ]
---

# ECMAScript 6 简介

### 2.const命令

* `const`声明一个只读的常量。一旦声明，常量的值就不能改变;
* `const`的作用域：只在声明所在的块级作用域内有效;
* `const`命令声明的常量不提升，存在暂时性死区，只能在声明的位置后面使用;
* 不可重复声明。

**本质:**

* `const`实际上保证的，并不是变量的值不得改动，而是变量指向的那个内存地址所保存的数据不得改动;

* 对于简单类型的数据（数值、字符串、布尔值），值就保存在变量指向的那个内存地址，因此等同于常量;

* 对于复合类型的数据（主要是对象和数组），变量指向的内存地址，保存的只是一个指向实际数据的指针，`const`只能保证这个指针是固定的（即总是指向另一个固定的地址），至于它指向的数据结构是不是可变的，就完全不能控制了。

* 可以改变属性，但是不可以直接赋值

  案例1：

  ```
  const foo = {};//声明一个对象
  // 为 foo 添加属性，可以成功
  foo.prop = 123;
  foo.name="aaa";
  console.log(foo.prop)// 123
  console.log(foo.name); //aaa
  // 将 foo 指向另一个对象，就会报错
  //  foo = {}; // TypeError: "foo" is read-only
  ```

  案例2：

  ```
  const a = [];
  a.push('Hello'); // 可执行
  a.length = 0;    // 可执行
  a = ['Dave'];    // 报错
  ```

* 如果不想让对象的属性被更改，就将对象冻结，应该使用`Object.freeze`方法。

  ```
  const foo = Object.freeze({});
  // 常规模式时，下面一行不起作用；
  // 严格模式时，该行会报错
  foo.prop = 123;
  ```
  
  **除了将对象本身冻结，对象的属性也应该冻结。下面是一个将对象彻底冻结的函数。**
  
  ```
  var obj={"name":"liming","age":{
      age1:"11",
      age2:'22',
  }};
  var constantize = (obj) => {
      Object.freeze(obj);
      //遍历对象，如果对象中的属性值也是一个对象的话，也让其冻结
      Object.keys(obj).forEach( (key, i) => {
        if ( typeof obj[key] === 'object' ) {
          constantize( obj[key] );
        }
      });
  };
  constantize(obj);
  obj.age.age2="88";
  console.log(obj.age);  //Object {age1: "11", age2: "22"}
  ```
  
  

**内容补充：Object.keys()与枚举：**

**<1>枚举：**

在JavaScript中，对象的属性分为可枚举和不可枚举之分，它们是由属性的enumerable值决定的。可枚举性决定了这个属性能否被for…in查找遍历到。

一、怎么判断属性是否可枚举

1. js中基本包装类型的原型属性是不可枚举的，如Object, Array, Number等，如果你写出这样的代码遍历其中的属性： 

2. Object对象的propertyIsEnumerable()方法可以判断此对象是否包含某个属性，并且这个属性是否可枚举。
3. 需要注意的是：如果判断的属性存在于Object对象的原型内，不管它是否可枚举都会返回false。

二、枚举性的作用

属性的枚举性会影响以下三个函数的结果：

for…in        Object.keys()         JSON.stringify

**<2>Object.keys()：**

**Object.keys() **方法会返回一个由一个给定对象的自身可枚举属性组成的数组，数组中属性名的排列顺序和使用 **[for...in]**循环遍历该对象时返回的顺序一致 ,如果对象的键-值都不可枚举，那么将返回由键组成的数组。

1. 语法：

   ```
   Object.keys(obj)                     //obj要返回其枚举自身属性的对象。
   ```

2. 一个表示给定对象的所有可枚举属性的字符串数组.

3. 描述：

   `Object.keys` 返回一个所有元素为字符串的数组，其元素来自于从给定的`object`上面可直接枚举的属性。这些属性的顺序与手动遍历该对象属性时的一致。

---

#### ES6 声明变量的六种方法：

var    function     let      const      import      class

### 3.顶层对象的属性

JavaScript 语言存在一个顶层对象，它提供全局环境（即全局作用域），所有代码都是在这个环境中运行

* 浏览器里面，顶层对象是`window`，但 Node 和 Web Worker 没有`window`;
* 浏览器和 Web Worker 里面，`self`也指向顶层对象，但是 Node 没有`self`;
* Node 里面，顶层对象是`global`，但其他环境都不支持。

为了能够在各种环境，都能取到顶层对象，现在一般是使用`this`变量，但是有局限性:

* 全局环境中，`this`会返回顶层对象。但是，Node 模块和 ES6 模块中，`this`返回的是当前模块;
* 函数里面的`this`，如果函数不是作为对象的方法运行，而是单纯作为函数运行，`this`会指向顶层对象。但是，严格模式下，这时`this`会返回`undefined`;
* 不管是严格模式，还是普通模式，`new Function('return this')()`，总是会返回全局对象。但是，如果浏览器用了 CSP（Content Security Policy，内容安全策略），那么`eval`、`new Function`这些方法都可能无法使用。





