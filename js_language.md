# JavaScript 语言规范

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
  1. [迭代器](#iterators-and-generators)
  1. [属性](#properties)
  1. [变量](#variables)
  1. [比较运算符和等号](#comparison-operators--equality)
  1. [类型转换](#type-casting--coercion)
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

<a name="arrow-functions"></a>
## 箭头函数

  - 当你必须使用函数表达式（或传递一个匿名函数）时，使用箭头函数符号。

  > 为什么?因为箭头函数创造了新的一个 `this` 执行环境，通常情况下都能满足你的需求，而且这样的写法更为简洁。

  > 为什么不？如果你有一个相当复杂的函数，你或许可以把逻辑部分转移到一个函数声明上。

  ```javascript
    // bad
    [1, 2, 3].map(function (x) {
      return x * x;
    });

    // good
    [1, 2, 3].map((x) => {
      return x * x;
    });
  ```

  - 如果一个函数适合用一行写出并且只有一个参数，那就把花括号、圆括号和 `return` 都省略掉。如果不是，那就不要省略。

  > 语法糖。在链式调用中可读性很高。

  > 为什么不？当你打算回传一个对象的时候。

  ```javascript
    // good
    [1, 2, 3].map(x => x * x);

    // good
    [1, 2, 3].reduce((total, n) => {
      return total + n;
    }, 0);
  ```

**[⬆ 返回目录](#table-of-contents)**

<a name="constructors"></a>
## 构造函数

  - 总是使用 `class`。避免直接操作 `prototype` 。

  > 为什么? 因为 `class` 语法更为简洁更易读。

  ```javascript
    // bad
    function Queue(contents = []) {
      this._queue = [...contents];
    }

    Queue.prototype.pop = function() {
      const value = this._queue[0];
      this._queue.splice(0, 1);
      return value;
    }


    // good
    class Queue {
      constructor(contents = []) {
        this._queue = [...contents];
      }

      pop() {
        const value = this._queue[0];
        this._queue.splice(0, 1);
        return value;
      }
    }
  ```


  - 方法可以返回 `this` 来帮助链式调用。

  ```javascript
    // bad
    Jedi.prototype.jump = function() {
      this.jumping = true;
      return true;
    };

    Jedi.prototype.setHeight = function(height) {
      this.height = height;
    };

    const luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20); // => undefined

    // good
    class Jedi {
      jump() {
        this.jumping = true;
        return this;
      }

      setHeight(height) {
        this.height = height;
        return this;
      }
    }

    const luke = new Jedi();

    luke.jump()
      .setHeight(20);
  ```


**[⬆ 返回目录](#table-of-contents)**

<a name="modules"></a>
## 模块

  - 总是使用模组 (`import`/`export`) 而不是其他非标准模块系统。你可以编译为你喜欢的模块系统。

  > 模块就是未来!

  ```javascript
    // bad
    const MaoyanStyleGuide = require('./MaoyanStyleGuide');
    module.exports = MaoyanStyleGuide.es6;

    // ok
    import MaoyanStyleGuide from './MaoyanStyleGuide';
    export default MaoyanStyleGuide.es6;

    // best
    import { es6 } from './MaoyanStyleGuide';
    export default es6;
  ```

  - 不要使用通配符 import。

  > 这样能确保你只有一个默认 export。

  ```javascript
    // bad
    import * as MaoyanStyleGuide from './MaoyanStyleGuide';

    // good
    import MaoyanStyleGuide from './MaoyanStyleGuide';
  ```

  - 不要从 import 中直接 export。

  > 虽然一行代码简洁明了，但让 import 和 export 各司其职让事情能保持一致。

  ```javascript
    // bad
    // filename es6.js
    export { es6 as default } from './MaoyanStyleGuide';

    // good
    // filename es6.js
    import { es6 } from './MaoyanStyleGuide';
    export default es6;
  ```

**[⬆ 返回目录](#table-of-contents)**

<a name="iterators-and-generators"></a>
## 迭代器

  - 不要使用 iterators。使用高阶函数例如 `map()` 和 `reduce()` 替代 `for-of`。

  > 为什么？这加强了我们不变的规则。处理纯函数的回调值更易读，这比它带来的副作用更重要。

  ```javascript
    const numbers = [1, 2, 3, 4, 5];

    // bad
    let sum = 0;
    for (let num of numbers) {
      sum += num;
    }

    sum === 15;

    // good
    let sum = 0;
    numbers.forEach((num) => sum += num);
    sum === 15;

    // best (use the functional force)
    const sum = numbers.reduce((total, num) => total + num, 0);
    sum === 15;
  ```

**[⬆ 返回目录](#table-of-contents)**

<a name="properties"></a>
## 属性

  - 使用 `.` 来访问对象的属性。

  ```javascript
    const luke = {
      jedi: true,
      age: 28,
    };

    // bad
    const isJedi = luke['jedi'];

    // good
    const isJedi = luke.jedi;
  ```

  - 当通过变量访问属性时使用中括号 `[]`。

  ```javascript
    const luke = {
      jedi: true,
      age: 28,
    };

    function getProp(prop) {
      return luke[prop];
    }

    const isJedi = getProp('jedi');
  ```

**[⬆ 返回目录](#table-of-contents)**

<a name="variables"></a>
## 变量

  - 一直使用 `const` 来声明变量，如果不这样做就会产生全局变量。我们需要避免全局命名空间的污染。

  ```javascript
    // bad
    superPower = new SuperPower();

    // good
    const superPower = new SuperPower();
  ```

  - 使用 `const` 声明每一个变量。

  > 增加新变量将变的更加容易，而且你永远不用再担心调换错 `;` 跟 `,`。

  ```javascript
    // bad
    const items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // good
    const items = getItems();
    const goSportsTeam = true;
    const dragonball = 'z';
  ```

  - 将所有的 `const` 和 `let` 分组

  > 当你需要把已赋值变量赋值给未赋值变量时非常有用。

  ```javascript
    // bad
    let i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // bad
    let i;
    const items = getItems();
    let dragonball;
    const goSportsTeam = true;
    let len;

    // good
    const goSportsTeam = true;
    const items = getItems();
    let dragonball;
    let i;
    let length;
  ```

**[⬆ 返回目录](#table-of-contents)**

<a name="comparison-operators--equality"></a>
## 比较运算符和等号

  - 优先使用 `===` 和 `!==` 而不是 `==` 和 `!=`.
  - 条件表达式例如 `if` 语句通过抽象方法 `ToBoolean` 强制计算它们的表达式时总遵守下面的规则：

    + **对象** 被计算为 **true**
    + **Undefined** 被计算为 **false**
    + **Null** 被计算为 **false**
    + **布尔值** 被计算为 **布尔的值**
    + **数字** 如果是 **+0、-0、或 NaN** 被计算为 **false**, 否则为 **true**
    + **字符串** 如果是空字符串 `''` 被计算为 **false**，否则为 **true**

    ```javascript
    if ([0]) {
      // true
      // An array is an object, objects evaluate to true
    }
    ```

  - 使用简写。

    ```javascript
    // bad
    if (name !== '') {
      // ...stuff...
    }

    // good
    if (name) {
      // ...stuff...
    }

    // bad
    if (collection.length > 0) {
      // ...stuff...
    }

    // good
    if (collection.length) {
      // ...stuff...
    }
    ```

**[⬆ 返回目录](#table-of-contents)**

<a name="type-casting--coercion"></a>
## 类型转换

  - 在语句开始时执行类型转换。
  - String:

    ```javascript
    //  => this.reviewScore = 9;

    // bad
    const totalScore = this.reviewScore + '';

    // good
    const totalScore = String(this.reviewScore);
    ```

  - Number，使用 `parseInt` 转换，并带上类型转换的基数。

    ```javascript
    const inputValue = '4';

    // bad
    const val = new Number(inputValue);

    // bad
    const val = +inputValue;

    // bad
    const val = inputValue >> 0;

    // bad
    const val = parseInt(inputValue);

    // good
    const val = Number(inputValue);

    // good
    const val = parseInt(inputValue, 10);
    ```

  - 如果因为某些原因 parseInt 成为你所做的事的瓶颈而需要使用位操作解决[性能问题](http://jsperf.com/coercion-vs-casting/3)时，留个注释说清楚原因和你的目的。

    ```javascript
    // good
    /**
     * 使用 parseInt 导致我的程序变慢，
     * 改成使用位操作转换数字快多了。
     */
    const val = inputValue >> 0;
    ```

  - 小心使用位操作运算符。数字会被当成 [64 位值](http://es5.github.io/#x4.3.19)，但是位操作运算符总是返回 32 位的整数（[参考](http://es5.github.io/#x11.7)）。位操作处理大于 32 位的整数值时还会导致意料之外的行为。[关于这个问题的讨论](https://github.com/airbnb/javascript/issues/109)。最大的 32 位整数是 2,147,483,647：

    ```javascript
    2147483647 >> 0 //=> 2147483647
    2147483648 >> 0 //=> -2147483648
    2147483649 >> 0 //=> -2147483647
    ```

  - Boolean:

    ```javascript
    const age = 0;

    // bad
    const hasAge = new Boolean(age);

    // good
    const hasAge = Boolean(age);

    // good
    const hasAge = !!age;
    ```

**[⬆ 返回目录](#table-of-contents)**

<a name="events"></a>
## 事件

  - 当给事件附加数据时（无论是 DOM 事件还是私有事件），传入一个哈希而不是原始值。这样可以让后面的贡献者增加更多数据到事件数据而无需找出并更新事件的每一个处理器。例如，不好的写法：

    ```javascript
    // bad
    $(this).trigger('listingUpdated', listing.id);

    ...

    $(this).on('listingUpdated', function(e, listingId) {
      // do something with listingId
    });
    ```

    更好的写法：

    ```javascript
    // good
    $(this).trigger('listingUpdated', { listingId : listing.id });

    ...

    $(this).on('listingUpdated', function(e, data) {
      // do something with data.listingId
    });
    ```

  **[⬆ 返回目录](#table-of-contents)**