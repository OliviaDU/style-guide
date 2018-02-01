# JavaScript 格式规范

<a name="table-of-contents"></a>
## 目录

  1. [命名规则](#naming-conventions)
  1. [逗号](#commas)
  1. [分号](#semicolons)
  1. [空格](#whitespace)
  1. [空行](#blank-lines)
  1. [代码块](#blocks)
  1. [注释](#comments)

<a name="naming-conventions"></a>
## 命名规则

  - 避免单字母命名。命名应具备描述性。
  - 尽量在变量名中体现出值的数据类型。

    ```javascript
    // bad
    function q() {
      // ...stuff...
    }

    // good
    function query() {
      // ..stuff..
    }
    ```
  - 变量名前缀应该是名词，函数名前缀是动词。
    | 动词 | 含义 |
    | - | - |
    | can | 函数返回一个布尔值 |
    | has | 函数返回一个布尔值 |
    | is | 函数返回一个布尔值 |
    | get | 函数返回一个非布尔值 |
    | set | 函数用来保存一个值 |

  - 使用驼峰式命名对象、函数和实例。

    ```javascript
    // bad
    const OBJEcttsssss = {};
    const this_is_my_object = {};
    function c() {}

    // good
    const thisIsMyObject = {};
    function thisIsMyFunction() {}
    ```

  - 使用帕斯卡式（每一个单字的首字母都采用大写字母）命名构造函数或类。

    ```javascript
    // bad
    function user(options) {
      this.name = options.name;
    }

    const bad = new user({
      name: 'nope',
    });

    // good
    class User {
      constructor(options) {
        this.name = options.name;
      }
    }

    const good = new User({
      name: 'yup',
    });
    ```

  - 不要使用下划线 `_` 结尾或开头来命名属性和方法。

    ```javascript
    // bad
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';
    this._firstName = 'Panda';

    // good
    this.firstName = 'Panda';
    ```

  - 别保存 `this` 的引用。使用箭头函数或 Function#bind。

    ```javascript
    // bad
    function foo() {
      const self = this;
      return function() {
        console.log(self);
      };
    }

    // bad
    function foo() {
      const that = this;
      return function() {
        console.log(that);
      };
    }

    // good
    function foo() {
      return () => {
        console.log(this);
      };
    }
    ```

  - 如果你的文件只输出一个类，那你的文件名必须和类名完全保持一致。

    ```javascript
    // file contents
    class CheckBox {
      // ...
    }
    export default CheckBox;

    // in some other file
    // bad
    import CheckBox from './checkBox';

    // bad
    import CheckBox from './check_box';

    // good
    import CheckBox from './CheckBox';
    ```

  - 当你导出默认的函数时使用驼峰式命名。你的文件名必须和函数名完全保持一致。

    ```javascript
    function makeStyleGuide() {
    }

    export default makeStyleGuide;
    ```

  - 当你导出单例、函数库、空对象时使用帕斯卡式命名。

    ```javascript
    const AirbnbStyleGuide = {
      es6: {
      }
    };

    export default AirbnbStyleGuide;
    ```

  - 如果属性是布尔值，使用 `isVal()` 或 `hasVal()`。

    ```javascript
    // bad
    if (!dragon.age()) {
      return false;
    }

    // good
    if (!dragon.hasAge()) {
      return false;
    }
    ```

**[⬆ 返回目录](#table-of-contents)**

<a name="commas"></a>
## 逗号

  - 增加结尾的逗号: **需要**。

  > 这会让 git diffs 更干净。另外，像 babel 这样的转译器会移除结尾多余的逗号，也就是说你不必担心老旧浏览器的[尾逗号问题](es5/README.md#commas)。

  ```javascript
    // bad - git diff without trailing comma
    const hero = {
          firstName: 'Florence',
    -    lastName: 'Nightingale'
    +    lastName: 'Nightingale',
    +    inventorOf: ['coxcomb graph', 'modern nursing']
    }

    // good - git diff with trailing comma
    const hero = {
          firstName: 'Florence',
          lastName: 'Nightingale',
    +    inventorOf: ['coxcomb chart', 'modern nursing'],
    }
  ```

**[⬆ 返回目录](#table-of-contents)**

<a name="semicolons"></a>
## 分号

  - **使用分号**

    ```javascript
    // bad
    (function() {
      const name = 'Skywalker'
      return name
    })()

    // good
    (() => {
      const name = 'Skywalker';
      return name;
    })();

    // good (防止函数在两个 IIFE 合并时被当成一个参数)
    ;(() => {
      const name = 'Skywalker';
      return name;
    })();
    ```

**[⬆ 返回目录](#table-of-contents)**


<a name="whitespace"></a>
## 空格

  - 用空格而不是tab进行缩进，使用 2 个空格作为缩进。

    ```javascript
    // bad
    function() {
    ∙∙∙∙const name;
    }

    // bad
    function() {
    ∙const name;
    }

    // good
    function() {
    ∙∙const name;
    }
    ```

- 在花括号前放一个空格。

  ```javascript
  // bad
  function test(){
    console.log('test');
  }

  // good
  function test() {
    console.log('test');
  }

  // bad
  dog.set('attr',{
    age: '1 year',
    breed: 'Bernese Mountain Dog',
  });

  // good
  dog.set('attr', {
    age: '1 year',
    breed: 'Bernese Mountain Dog',
  });
  ```

- 在控制语句（`if`、`while` 等）的小括号前放一个空格。在函数调用及声明中，不在函数的参数列表前加空格。

  ```javascript
  // bad
  if(isJedi) {
    fight ();
  }

  // good
  if (isJedi) {
    fight();
  }

  // bad
  function fight () {
    console.log ('Swooosh!');
  }

  // good
  function fight() {
    console.log('Swooosh!');
  }
  ```

- 使用空格把运算符隔开。

  ```javascript
  // bad
  const x=y+5;

  // good
  const x = y + 5;
  ```

- 在对象的计算属性内，禁止使用空格。冒号后需要插入一个空格。

  ```javascript
  //bad
  obj['foo' ]
  obj[ 'foo']
  obj[ 'foo' ]

  //good
  obj['foo']

  {
    foo: 1,
    bar: 2,
    baz: 3
  }
  ```

- 条件表达式 if/while/switch 后插入一个空格。

  ```javascript
  if (true) {
    //...
  }

  while (true) {
    //...
  }

  switch (v) {
    //...
  }
  ```
- 函数参数括号前后不需要空格，逗号后需插入一个空格。

  ```javascript
  function fn(arg1, arg2) {
    \\todo:...
  }
  ```

<a name="blank-lines"></a>
## 空行

- 在文件末尾插入一个空行。

  ```javascript
  // bad
  (function(global) {
    // ...stuff...
  })(this);
  ```

  ```javascript
  // bad
  (function(global) {
    // ...stuff...
  })(this);↵
  ↵
  ```

  ```javascript
  // good
  (function(global) {
    // ...stuff...
  })(this);↵
  ```

- 在使用长方法链时进行缩进。使用前面的点 `.` 强调这是方法调用而不是新语句。

  ```javascript
  // bad
  $('#items').find('.selected').highlight().end().find('.open').updateCount();

  // bad
  $('#items').
    find('.selected').
      highlight().
      end().
    find('.open').
      updateCount();

  // good
  $('#items')
    .find('.selected')
    .highlight()
    .end()
    .find('.open')
    .updateCount();

  // bad
  const leds = stage.selectAll('.led').data(data).enter().append('svg:svg').class('led', true)
      .attr('width', (radius + margin) * 2).append('svg:g')
      .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
      .call(tron.led);

  // good
  const leds = stage.selectAll('.led')
    .data(data)
    .enter()
    .append('svg:svg')
    .classed('led', true)
    .attr('width', (radius + margin) * 2)
    .append('svg:g')
    .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
    .call(tron.led);
  ```

- 在块末和新语句前插入空行。

  ```javascript
  // bad
  if (foo) {
    return bar;
  }
  return baz;

  // good
  if (foo) {
    return bar;
  }

  return baz;

  // bad
  const obj = {
    foo() {
    },
    bar() {
    },
  };
  return obj;

  // good
  const obj = {
    foo() {
    },

    bar() {
    },
  };

  return obj;
  ```

- 让语义有关联的代码展现在一起，不相关的用空行分隔。例如：
  - 在方法之间。
  - 在方法中的局部变量和第一条语句之间。
  - 在方法内的逻辑片段之间。
  - 在多行或单行注释之前。

- 一行的长度不应该超过80个字符。
  - 通常在运算符后换行，下一行增加两个层级的缩进。
  - 当给变量赋值时，第二行的位置应当和赋值运算符的位置保持对齐。

```javascript
  let result = something + anotherThing + somethingElse + yetAnotherThing +
               anotherSomethingElse;
```


**[⬆ 返回目录](#table-of-contents)**



<a name="blocks"></a>
## 代码块

  - 使用大括号包裹所有的多行代码块。

    ```javascript
    // bad
    if (test)
      return false;

    // good
    if (test) return false;

    // good
    if (test) {
      return false;
    }

    // bad
    function() { return false; }

    // good
    function() {
      return false;
    }
    ```

  - 如果通过 `if` 和 `else` 使用多行代码块，把 `else` 放在 `if` 代码块关闭括号的同一行。

    ```javascript
    // bad
    if (test) {
      thing1();
      thing2();
    }
    else {
      thing3();
    }

    // good
    if (test) {
      thing1();
      thing2();
    } else {
      thing3();
    }
    ```

**[⬆ 返回目录](#table-of-contents)**

<a name="comments"></a>
## 注释

  - 使用 `/** ... */` 作为多行注释。包含描述、指定所有参数和返回值的类型和值。

    ```javascript
    // bad
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

      // ...stuff...

      return element;
    }

    // good
    /**
     * make() returns a new element
     * based on the passed in tag name
     *
     * @param {String} tag
     * @return {Element} element
     */
    function make(tag) {

      // ...stuff...

      return element;
    }
    ```

  - 使用 `//` 作为单行注释。在评论对象上面另起一行使用单行注释。在注释前插入空行。

    ```javascript
    // bad
    const active = true;  // is current tab

    // good
    // is current tab
    const active = true;

    // bad
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      const type = this._type || 'no type';

      return type;
    }

    // good
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      const type = this._type || 'no type';

      return type;
    }
    ```

**[⬆ 返回目录](#table-of-contents)**

