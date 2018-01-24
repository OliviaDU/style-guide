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
   ```javascript
    // bad
    var a = 1;

    // good
    const a = 1;
  ```

  -  如果一定需要可变动的引用，使用 `let` 代替 `var`。

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

  - 如果你的代码在浏览器环境下执行，别使用 [保留字](http://es5.github.io/#x7.6.1) 作为键值。这样的话在 IE8 不会运行。但在 ES6 模块和服务器端中使用没有问题。

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
    }
  };

  // good
  const atom = {
    value: 1,

    addValue(value) {
      return atom.value + value;
    }
  };
```

  <a name="es6-object-concise"></a>
  - 使用对象属性值的简写。

  > 这样更短更有描述性。

  ```javascript
    const job = 'FE';

    // bad
    const obj = {
      job: job
    };

    // good
    const obj = {
      job
    };
  ```

  - 在对象属性声明前把简写的属性分组。

  > 这样能清楚地看出哪些属性使用了简写。

  ```javascript
  const job = 'FE';
  const company = 'maoyan';

  // bad
  const obj = {
    age: 22,
    company,
    sex: 'female',
    job
  };

  // good
  const obj = {
    company,
    job,
    age: 22,
    sex: 'female'
  };
  ```

**[⬆ 返回目录](#table-of-contents)**


<a name="arrays"></a>
## 数组

  - 使用字面值创建数组。

  ```javascript
  // bad
  const items = new Array();

  // good
  const items = [];
  ```

  - 使用拓展运算符 `...` 复制数组。

  ```javascript
  // bad
  const len = items.length;
  const itemsCopy = [];
  let i;

  for (i = 0; i < len; i++) {
    itemsCopy[i] = items[i];
  }

  // good
  const itemsCopy = [...items];
  ```
  - 使用 Array.from 把一个类数组对象转换成数组。
  ```javascript

  const foo = document.querySelectorAll('.foo');
  const nodes = Array.from(foo);

  ```

**[⬆ 返回目录](#table-of-contents)**

<a name="destructuring"></a>
## 解构赋值

  - 使用解构存取和使用多属性对象。

  > 解构能减少临时引用属性。

  ```javascript
    // bad
    function getFullName(user) {
      const firstName = user.firstName;
      const lastName = user.lastName;

      return `${firstName} ${lastName}`;
    }

    // good
    function getFullName(obj) {
      const { firstName, lastName } = obj;
      return `${firstName} ${lastName}`;
    }

    // best
    function getFullName({ firstName, lastName }) {
      return `${firstName} ${lastName}`;
    }
  ```

  - 对数组使用解构赋值。

  ```javascript
    const arr = [1, 2, 3, 4];

    // bad
    const first = arr[0];
    const second = arr[1];

    // good
    const [first, second] = arr;
  ```

  - 需要回传多个值时，使用对象解构，而不是数组解构。
  > 增加属性或者改变排序不会改变调用时的位置。

  ```javascript
    // bad
    function processInput(input) {
      // then a miracle occurs
      return [left, right, top, bottom];
    }

    // 调用时需要考虑回调数据的顺序。
    const [left, __, top] = processInput(input);

    // good
    function processInput(input) {
      // then a miracle occurs
      return { left, right, top, bottom };
    }

    // 调用时只选择需要的数据
    const { left, right } = processInput(input);
  ```


**[⬆ 返回目录](#table-of-contents)**

<a name="strings"></a>
## 字符串

  - 字符串使用单引号 `''` 。

    ```javascript
    // bad
    const name = "Capt. Janeway";

    // good
    const name = 'Capt. Janeway';
    ```

  - 字符串超过 80 个字节应该使用字符串连接号换行。
  - 注：过度使用字串连接符号可能会对性能造成影响。

    ```javascript
    // bad
    const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

    // bad
    const errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // good
    const errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.';
    ```

  - 程序化生成字符串时，使用模板字符串代替字符串连接。

  > 模板字符串更为简洁，更具可读性。

  ```javascript
    // bad
    function sayHi(name) {
      return 'How are you, ' + name + '?';
    }

    // bad
    function sayHi(name) {
      return ['How are you, ', name, '?'].join();
    }

    // good
    function sayHi(name) {
      return `How are you, ${name}?`;
    }
  ```

**[⬆ 返回目录](#table-of-contents)**

<a name="functions"></a>
## 函数

  - 使用函数声明代替函数表达式。

  > 因为函数声明是可命名的，所以他们在调用栈中更容易被识别。此外，函数声明会把整个函数提升（hoisted），而函数表达式只会把函数的引用变量名提升。这条规则使得[箭头函数](#arrow-functions)可以取代函数表达式。

  ```javascript
    // bad
    const foo = function () {
    };

    // good
    function foo() {
    }
  ```

  - 函数表达式:

  ```javascript
    // 立即调用的函数表达式 (IIFE)
    (() => {
      console.log('Welcome to the Internet. Please follow me.');
    })();
  ```

  - 永远不要在一个非函数代码块（`if`、`while` 等）中声明一个函数，把那个函数赋给一个变量。

  ```javascript
    // bad
    if (currentUser) {
      function test() {
        console.log('Nope.');
      }
    }

    // good
    let test;
    if (currentUser) {
      test = () => {
        console.log('Yup.');
      };
    }
  ```

  - 永远不要把参数命名为 `arguments`。这将取代原来函数作用域内的 `arguments` 对象。

  ```javascript
    // bad
    function nope(name, options, arguments) {
      // ...stuff...
    }

    // good
    function yup(name, options, args) {
      // ...stuff...
    }
  ```

  <a name="es6-rest"></a>
  - 不要使用 `arguments`。可以选择 rest 语法 `...` 替代。

  > 使用 `...` 能明确你要传入的参数。另外 rest 参数是一个真正的数组，而 `arguments` 是一个类数组。

  ```javascript
    // bad
    function concatenateAll() {
      const args = Array.prototype.slice.call(arguments);
      return args.join('');
    }

    // good
    function concatenateAll(...args) {
      return args.join('');
    }
  ```

  - 直接给函数的参数指定默认值，不要使用一个变化的函数参数。

  ```javascript
    // really bad
    function handleThings(opts) {
      // 如果参数 opts 是 false 的话，它就会被设定为一个对象。
      opts = opts || {};
    }

    // still bad
    function handleThings(opts) {
      if (opts === void 0) {
        opts = {};
      }
    }

    // good
    function handleThings(opts = {}) {
      // ...
    }
  ```

  - 直接给函数参数赋值时需要避免副作用。

  > 因为这样的写法让人感到很困惑。

  ```javascript
  var b = 1;
  // bad
  function count(a = b++) {
    console.log(a);
  }
  count();  // 1
  count();  // 2
  count(3); // 3
  count();  // 3
  ```


**[⬆ 返回目录](#table-of-contents)**

