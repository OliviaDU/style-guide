# JavaScript Style Guide

<a name="table-of-contents"></a>
## 目录

  1. [引用](#references)
  1. [对象](#objects)
  1. [数组](#arrays)
  1. [解构](#destructuring)
  1. [字符串](#strings)
  1. [函数](#functions)
  1. [箭头函数](#arrow-functions)
  1. [构造函数](#constructors)
  1. [模块](#modules)
  1. [迭代器和生成器](#iterators-and-generators)
  1. [属性](#properties)
  1. [变量](#variables)
  1. [提升](#hoisting)
  1. [比较运算符和等号](#comparison-operators--equality)
  1. [代码块](#blocks)
  1. [注释](#comments)
  1. [空白](#whitespace)
  1. [逗号](#commas)
  1. [分号](#semicolons)
  1. [类型转换](#type-casting--coercion)
  1. [命名规则](#naming-conventions)
  1. [存取器](#accessors)
  1. [事件](#events)

<a name="references"></a>
## 引用

  - [1.1](#1.1) <a name='1.1'></a> 对所有的引用使用 `const` ；不要使用 `var`。

  > 这能确保你无法对引用重新赋值，也不会导致出现 bug 或难以理解。

   ```javascript
    // bad
    var a = 1;
    var b = 2;

    // good
    const a = 1;
    const b = 2;
  ```

  - [1.2](#1.2) <a name='1.2'></a> 如果你一定需要可变动的引用，使用 `let` 代替 `var`。

  > 因为  `let` 是块级作用域，而 `var` 是函数作用域。

  ```javascript
    // bad
    var count = 1;
    if (true) {
      count += 1;
    }

    // good, use the let.
    let count = 1;
    if (true) {
      count += 1;
    }
  ```

  - [1.3](#1.3) <a name='1.3'></a> 注意 `let` 和 `const` 都是块级作用域。

  ```javascript
    // const 和 let 只存在于它们被定义的区块内。
    {
      let a = 1;
      const b = 1;
    }
    console.log(a); // ReferenceError
    console.log(b); // ReferenceError
  ```

**[⬆ 返回目录](#table-of-contents)**