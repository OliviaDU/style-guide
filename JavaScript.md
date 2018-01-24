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

  - 对所有的引用使用 `const` ；不要使用 `var`。

  > 这能确保你无法对引用重新赋值，也不会导致出现 bug 或难以理解。

  -  如果一定需要可变动的引用，使用 `let` 代替 `var`。

  > 因为  `let` 是块级作用域，而 `var` 是函数作用域。

  - 注意 `let` 和 `const` 都是块级作用域。

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


<a name="objects"></a>
## 对象

  - 使用字面值创建对象。

  ```javascript
  // bad
  const item = new Object();

  // good
  const item = {};
  ```

  - 如果你的代码在浏览器环境下执行，别使用 [保留字](http://es5.github.io/#x7.6.1) 作为键值。这样的话在 IE8 不会运行。 [更多信息](https://github.com/airbnb/javascript/issues/61)。 但在 ES6 模块和服务器端中使用没有问题。

  ```javascript
  // bad
  const superman = {
    default: { clark: 'kent' },
    private: true,
  };

  // good
  const superman = {
    defaults: { clark: 'kent' },
    hidden: true,
  };
  ```

  - 使用同义词替换需要使用的保留字。

  ```javascript
  // bad
  const superman = {
    class: 'alien',
  };

  // bad
  const superman = {
    klass: 'alien',
  };

  // good
  const superman = {
    type: 'alien',
  };
  ```

  - 创建有动态属性名的对象时，使用可被计算的属性名称。

  > 这样可以在一个地方定义所有的对象属性。

  ```javascript
    function getKey(k) {
      return `a key named ${k}`;
    }

    // bad
    const obj = {
      id: 5,
      name: 'San Francisco',
    };
    obj[getKey('enabled')] = true;

    // good
    const obj = {
      id: 5,
      name: 'San Francisco',
      [getKey('enabled')]: true,
    };
  ```

  <a name="es6-object-shorthand"></a>
  - 使用对象方法的简写。

  ```javascript
  // bad
  const atom = {
    value: 1,

    addValue: function (value) {
      return atom.value + value;
    },
  };

  // good
  const atom = {
    value: 1,

    addValue(value) {
      return atom.value + value;
    },
  };
  ```

  <a name="es6-object-concise"></a>
  - 使用对象属性值的简写。

  > 这样更短更有描述性。

    ```javascript
    const lukeSkywalker = 'Luke Skywalker';

    // bad
    const obj = {
      lukeSkywalker: lukeSkywalker,
    };

    // good
    const obj = {
      lukeSkywalker,
    };
    ```

  - 在对象属性声明前把简写的属性分组。

  > 这样能清楚地看出哪些属性使用了简写。

  ```javascript
  const anakinSkywalker = 'Anakin Skywalker';
  const lukeSkywalker = 'Luke Skywalker';

  // bad
  const obj = {
    episodeOne: 1,
    twoJedisWalkIntoACantina: 2,
    lukeSkywalker,
    episodeThree: 3,
    mayTheFourth: 4,
    anakinSkywalker,
  };

  // good
  const obj = {
    lukeSkywalker,
    anakinSkywalker,
    episodeOne: 1,
    twoJedisWalkIntoACantina: 2,
    episodeThree: 3,
    mayTheFourth: 4,
  };
  ```

**[⬆ 返回目录](#table-of-contents)**
