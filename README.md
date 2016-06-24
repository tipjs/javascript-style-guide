
[원문:https://github.com/airbnb/javascript](https://github.com/airbnb/javascript)

# Airbnb JavaScript 스타일 가이드() {

*JavaScript에 대한 대부분 합리적인 접근 방법*

다른 스타일 가이드들
 - [ES5](https://github.com/tipjs/javascript-style-guide/blob/master/es5.md)
 - [React](https://github.com/airbnb/javascript/tree/master/react)
 - [CSS & Sass](https://github.com/airbnb/css)
 - [Ruby](https://github.com/airbnb/ruby)

## 목차

  1. [형(Types)](#형types)
  1. [참조(References)](#참조references)
  1. [오브젝트(Objects)](#오브젝트objects)
  1. [배열(Arrays)](#배열arrays)
  1. [구조화대입(Destructuring)](#구조화대입destructuring)
  1. [문자열(Strings)](#문자열strings)
  1. [함수(Functions)](#함수functions)
  1. [Arrow함수(Arrow Functions)](#arrow함수arrow-functions)
  1. [Classes & Constructors](#classes--constructors)
  1. [모듈(Modules)](#모듈modules)
  1. [이터레이터와 제너레이터(Iterators and Generators)](#이터레이터와-제너레이터iterators-and-generators)
  1. [프로퍼티(Properties)](#프로퍼티properties)
  1. [변수(Variables)](#변수variables)
  1. [Hoisting](#hoisting)
  1. [조건식과 등가식(Comparison Operators & Equality)](#조건식과-등가식comparison-operators--equality)
  1. [블록(Blocks)](#블록blocks)
  1. [코멘트(Comments)](#코멘트comments)
  1. [공백(Whitespace)](#공백whitespace)
  1. [콤마(Commas)](#콤마commas)
  1. [세미콜론(Semicolons)](#세미콜론semicolons)
  1. [형변환과 강제(Type Casting & Coercion)](#형변환과-강제type-casting--coercion)
  1. [명명규칙(Naming Conventions)](#명명규칙naming-conventions)
  1. [억세서(Accessors)](#억세서accessors)
  1. [이벤트(Events)](#이벤트events)
  1. [jQuery](#jquery)
  1. [ECMAScript 5 Compatibility](#ecmascript-5-compatibility)
  1. [ECMAScript 6 Styles](#ecmascript-6-styles)
  1. [Testing](#testing)
  1. [Performance](#performance)
  1. [Resources](#resources)
  1. [In the Wild](#in-the-wild)
  1. [Translation](#translation)
  1. [The JavaScript Style Guide Guide](#the-javascript-style-guide-guide)
  1. [Chat With Us About JavaScript](#chat-with-us-about-javascript)
  1. [Contributors](#contributors)
  1. [License](#license)

## 형(Types)

  - [1.1](#1.1) <a name='1.1'></a> **Primitives**: When you access a primitive type you work directly on its value.
  - [1.1](#1.1) <a name='1.1'></a> **Primitives**: primitive type은 그 값을 직접 조작합니다.
    + `string`
    + `number`
    + `boolean`
    + `null`
    + `undefined`

    ```javascript
    const foo = 1;
    let bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```
  - [1.2](#1.2) <a name='1.2'></a> **Complex**: When you access a complex type you work on a reference to its value.
  - [1.2](#1.2) <a name='1.2'></a> **참조형**: 참조형(Complex)은 참조를 통해 값을 조작합니다.

    + `object`
    + `array`
    + `function`

    ```javascript
    const foo = [1, 2];
    const bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

**[⬆ back to top](#목차)**

## 참조(References)

  - [2.1](#2.1) <a name='2.1'></a> Use `const` for all of your references; avoid using `var`.
  - [2.1](#2.1) <a name='2.1'></a> 모든 참조는 `const` 를 사용하고, `var` 를 사용하지 마십시오.

  > Why? This ensures that you can't reassign your references, which can lead to bugs and difficult to comprehend code.

  > 왜? 참조를 재할당 할 수 없으므로, 버그로 이어지고 이해하기 어려운 코드가 되는것을 방지합니다.

    ```javascript
    // bad
    var a = 1;
    var b = 2;

    // good
    const a = 1;
    const b = 2;
    ```

  - [2.2](#2.2) <a name='2.2'></a> If you must reassign references, use `let` instead of `var`.
  - [2.2](#2.2) <a name='2.2'></a> 참조를 재할당 해야한다면 `var` 대신 `let` 을 사용하십시오.

  > Why? `let` is block-scoped rather than function-scoped like `var`.

  > 왜? `var` 같은 함수스코프 보다는 오히려 블록스코프의 `let`

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

  - [2.3](#2.3) <a name='2.3'></a> Note that both `let` and `const` are block-scoped.
  - [2.3](#2.3) <a name='2.3'></a> `let` 과 `const` 는 같이 블록스코프라는것을 유의하십시오.

    ```javascript
    // const and let only exist in the blocks they are defined in.
    // const 와 let 은 선언된 블록의 안에서만 존재합니다.
    {
      let a = 1;
      const b = 1;
    }
    console.log(a); // ReferenceError
    console.log(b); // ReferenceError
    ```

**[⬆ back to top](#목차)**

## 오브젝트(Objects)

  - [3.1](#3.1) <a name='3.1'></a> Use the literal syntax for object creation.
  - [3.1](#3.1) <a name='3.1'></a> 오브젝트를 작성할때는, 리터럴 구문을 사용하십시오.

    ```javascript
    // bad
    const item = new Object();

    // good
    const item = {};
    ```

  - [3.2](#3.2) <a name='3.2'></a> If your code will be executed in browsers in script context, don't use [reserved words](http://es5.github.io/#x7.6.1) as keys. It won't work in IE8. [More info](https://github.com/airbnb/javascript/issues/61). It’s OK to use them in ES6 modules and server-side code.
  - [3.2](#3.2) <a name='3.2'></a> 코드가 브라우저상의 스크립트로 실행될때 [예약어](http://es5.github.io/#x7.6.1)를 키로 이용하지 마십시오. IE8에서 작동하지 않습니다. [More info](https://github.com/airbnb/javascript/issues/61) 하지만 ES6 모듈안이나 서버사이드에서는 이용가능합니다.

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

  - [3.3](#3.3) <a name='3.3'></a> Use readable synonyms in place of reserved words.
  - [3.3](#3.3) <a name='3.3'></a> 예약어 대신 알기쉬운 동의어를 사용해 주십시오.

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

  <a name="es6-computed-properties"></a>
  - [3.4](#3.4) <a name='3.4'></a> Use computed property names when creating objects with dynamic property names.
  - [3.4](#3.4) <a name='3.4'></a> 동적 프로퍼티명을 갖는 오브젝트를 작성할때, 계산된 프로퍼티명(computed property names)을 이용해 주십시오.

  > Why? They allow you to define all the properties of an object in one place.

  > 왜? 오브젝트의 모든 프로퍼티를 한 장소에서 정의 할 수 있습니다.

    ```javascript
    function getKey(k) {
      return a `key named ${k}`;
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
      ［getKey('enabled')]: true
    };
    ```

  <a name="es6-object-shorthand"></a>
  - [3.5](#3.5) <a name='3.5'></a> Use object method shorthand.
  - [3.5](#3.5) <a name='3.5'></a> 메소드의 단축구문을 이용해 주십시오.

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
  - [3.6](#3.6) <a name='3.6'></a> Use property value shorthand.
  - [3.6](#3.6) <a name='3.6'></a> 프로퍼티의 단축구문을 이용해 주십시오.

  > Why? It is shorter to write and descriptive.

  > 왜? 기술과 설명이 간결해지기 때문입니다.

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

  - [3.7](#3.7) <a name='3.7'></a> Group your shorthand properties at the beginning of your object declaration.
  - [3.7](#3.7) <a name='3.7'></a> 프로퍼티의 단축구문은 오브젝트 선언의 시작부분에 그룹화 해주십시오.

  > Why? It's easier to tell which properties are using the shorthand.

  > 왜? 어떤 프로퍼티가 단축구문을 이용하고 있는지가 알기쉽기 때문입니다.

    ```javascript
    const anakinSkywalker = 'Anakin Skywalker';
    const lukeSkywalker = 'Luke Skywalker';

    // bad
    const obj = {
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
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
      twoJediWalkIntoACantina: 2,
      episodeThree: 3,
      mayTheFourth: 4,
    };
    ```

**[⬆ back to top](#목차)**

## 배열(Arrays)

  - [4.1](#4.1) <a name='4.1'></a> Use the literal syntax for array creation.
  - [4.1](#4.1) <a name='4.1'></a> 배열을 작성 할 때는 리터럴 구문을 사용해 주십시오.

    ```javascript
    // bad
    const items = new Array();

    // good
    const items = [];
    ```

  - [4.2](#4.2) <a name='4.2'></a> Use Array#push instead of direct assignment to add items to an array.
  - [4.2](#4.2) <a name='4.2'></a> 직접 배열에 항목을 대입하지 말고, Array#push를 이용해 주십시오.

    ```javascript
    const someStack = [];

    // bad
    someStack[someStack.length] = 'abracadabra';

    // good
    someStack.push('abracadabra');
    ```

  <a name="es6-array-spreads"></a>
  - [4.3](#4.3) <a name='4.3'></a> Use array spreads `...` to copy arrays.
  <a name="es6-array-spreads"></a>
  - [4.3](#4.3) <a name='4.3'></a> 배열을 복사할때는 배열의 확장연산자 `...` 를 이용해 주십시오.

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

  - [4.4](#4.4) <a name='4.4'></a> To convert an array-like object to an array, use Array#from.
  - [4.4](#4.4) <a name='4.4'></a> array-like 오브젝트를 배열로 변환하는 경우는 Array#from을 이용해 주십시오.

    ```javascript
    const foo = document.querySelectorAll('.foo');
    const nodes = Array.from(foo);
    ```

**[⬆ back to top](#목차)**

## 구조화대입(Destructuring)

  - [5.1](#5.1) <a name='5.1'></a> Use object destructuring when accessing and using multiple properties of an object.
  - [5.1](#5.1) <a name='5.1'></a> 하나의 오브젝트에서 복수의 프로퍼티를 억세스 할 때는 오브젝트 구조화대입을 이용해 주십시오.

  > Why? Destructuring saves you from creating temporary references for those properties.

  > 왜? 구조화대입을 이용하는 것으로 프로퍼티를 위한 임시적인 참조의 작성을 줄일 수 있습니다.

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

  - [5.2](#5.2) <a name='5.2'></a> Use array destructuring.
  - [5.2](#5.2) <a name='5.2'></a> 배열의 구조화대입을 이용해 주십시오.

    ```javascript
    const arr = [1, 2, 3, 4];

    // bad
    const first = arr[0];
    const second = arr[1];

    // good
    const [first, second] = arr;
    ```

  - [5.3](#5.3) <a name='5.3'></a> Use object destructuring for multiple return values, not array destructuring.
  - [5.3](#5.3) <a name='5.3'></a> 복수의 값을 반환하는 경우는 배열의 구조화대입이 아닌 오브젝트의 구조화대입을 이용해 주십시오.

  > Why? You can add new properties over time or change the order of things without breaking call sites.

  > 왜? 이렇게 함으로써 나중에 호출처에 영향을 주지않고 새로운 프로퍼티를 추가하거나 순서를 변경할수 있습니다.

    ```javascript
    // bad
    function processInput(input) {
      // then a miracle occurs
      // 그리고 기적이 일어납니다.
      return [left, right, top, bottom];
    }

    // the caller needs to think about the order of return data
    // 호출처에서 반환된 데이터의 순서를 고려할 필요가 있습니다.
    const [left, __, top] = processInput(input);

    // good
    function processInput(input) {
      // then a miracle occurs
      // 그리고 기적이 일어납니다.
      return { left, right, top, bottom };
    }

    // the caller selects only the data they need
    // 호출처에서는 필요한 데이터만 선택하면 됩니다.
    const { left, right } = processInput(input);
    ```


**[⬆ back to top](#목차)**

## 문자열(Strings)

  - [6.1](#6.1) <a name='6.1'></a> Use single quotes `''` for strings.
  - [6.1](#6.1) <a name='6.1'></a> 문자열에는 싱크쿼트 `''` 를 사용해 주십시오.

    ```javascript
    // bad
    const name = "Capt. Janeway";

    // good
    const name = 'Capt. Janeway';
    ```

  - [6.2](#6.2) <a name='6.2'></a> Strings longer than 100 characters should be written across multiple lines using string concatenation.
  - [6.2](#6.2) <a name='6.2'></a> 100문자 이상의 문자열은 문자열연결을 사용해서 복수행에 걸쳐 기술할 필요가 있습니다.

  - [6.3](#6.3) <a name='6.3'></a> Note: If overused, long strings with concatenation could impact performance. [jsPerf](http://jsperf.com/ya-string-concat) & [Discussion](https://github.com/airbnb/javascript/issues/40).
  - [6.3](#6.3) <a name='6.3'></a> 주의: 문자연결을 과용하면 성능에 영향을 미칠 수 있습니다. [jsPerf](http://jsperf.com/ya-string-concat) & [Discussion](https://github.com/airbnb/javascript/issues/40).

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

  <a name="es6-template-literals"></a>
  - [6.4](#6.4) <a name='6.4'></a> When programmatically building up strings, use template strings instead of concatenation.
  - [6.4](#6.4) <a name='6.4'></a> 프로그램에서 문자열을 생성하는 경우는 문자열 연결이 아닌 template strings를 이용해 주십시오.

  > Why? Template strings give you a readable, concise syntax with proper newlines and string interpolation features.

  > 왜? Template strings 는 문자열 보간기능과 적절한 줄바꿈 기능을 갖는 간결한 구문으로 가독성이 좋기 때문입니다.

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
  - [6.5](#6.5) <a name='6.5'></a> Never use `eval()` on a string, it opens too many vulnerabilities.
  - [6.5](#6.5) <a name='6.5'></a> 절대로 `eval()` 을 이용하지 마십시오. 이것은 많은 취약점을 만들기 때문입니다.

**[⬆ back to top](#목차)**


## 함수(Functions)

  - [7.1](#7.1) <a name='7.1'></a> Use function declarations instead of function expressions.
  - [7.1](#7.1) <a name='7.1'></a> 함수식 보다 함수선언을 이용해 주십시오.

  > Why? Function declarations are named, so they're easier to identify in call stacks. Also, the whole body of a function declaration is hoisted, whereas only the reference of a function expression is hoisted. This rule makes it possible to always use [Arrow Functions](#arrow-함수arrow-functions) in place of function expressions.

  > 왜? 이름이 부여된 함수선언은 콜스택에서 간단하게 확인하는 것이 가능합니다. 또한 함수선언은 함수의 본체가 hoist 되어집니다. 그에 반해 함수식은 참조만이 hoist 되어집니다.
  이 룰에 의해 함수식의 부분을 항상 [Arrow함수](#arrow함수arrow-functions)에서 이용하는것이 가능합니다.

    ```javascript
    // bad
    const foo = function () {
    };

    // good
    function foo() {
    }
    ```

  - [7.2](#7.2) <a name='7.2'></a> Function expressions:
  - [7.2](#7.2) <a name='7.2'></a> 함수식

    ```javascript
    // immediately-invoked function expression (IIFE)
    // 즉시 실행 함수식(IIFE)
    (() => {
      console.log('Welcome to the Internet. Please follow me.');
    })();
    ```

  - [7.3](#7.3) <a name='7.3'></a> Never declare a function in a non-function block (if, while, etc). Assign the function to a variable instead. Browsers will allow you to do it, but they all interpret it differently, which is bad news bears.
  - [7.3](#7.3) <a name='7.3'></a> 함수이외의 블록 (if나 while같은) 안에서 함수를 선언하지 마십시오. 변수에 함수를 대입하는 대신 브라우저들은 그것을 허용하지만 모두가 다르게 해석합니다.

  - [7.4](#7.4) <a name='7.4'></a> **Note:** ECMA-262 defines a `block` as a list of statements. A function declaration is not a statement. [Read ECMA-262's note on this issue](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97).
  - [7.4](#7.4) <a name='7.4'></a> **주의:** ECMA-262 사양에서는 `block` 은 statements의 일람으로 정의되어 있지만 함수선언은 statements 가 아닙니다. [Read ECMA-262's note on this issue](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97).

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

  - [7.5](#7.5) <a name='7.5'></a> Never name a parameter `arguments`. This will take precedence over the `arguments` object that is given to every function scope.
  - [7.5](#7.5) <a name='7.5'></a> 절대 파라메터에 `arguments` 를 지정하지 마십시오. 이것은 함수스코프에 전해지는  `arguments` 오브젝트의 참조를 덮어써 버립니다.

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
  - [7.6](#7.6) <a name='7.6'></a> Never use `arguments`, opt to use rest syntax `...` instead.
  - [7.6](#7.6) <a name='7.6'></a> 절대 `arguments` 를 이용하지 마십시오. 대신에 rest syntax `...` 를 이용해 주십시오.

  > Why? `...` is explicit about which arguments you want pulled. Plus rest arguments are a real Array and not Array-like like `arguments`.

  > 왜? `...` 을 이용하는것으로 몇개의 파라메터를 이용하고 싶은가를 확실하게 할 수 있습니다. 더해서 rest 파라메터는 `arguments` 와 같은 Array-like 오브젝트가 아닌 진짜 Array 입니다.

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

  <a name="es6-default-parameters"></a>
  - [7.7](#7.7) <a name='7.7'></a> Use default parameter syntax rather than mutating function arguments.
  - [7.7](#7.7) <a name='7.7'></a> 함수의 파라메터를 변이시키는 것보다 default 파라메터를 이용해 주십시오.

    ```javascript
    // really bad
    function handleThings(opts) {
      // No! We shouldn't mutate function arguments.
      // Double bad: if opts is falsy it'll be set to an object which may
      // be what you want but it can introduce subtle bugs.

      // 안돼！함수의 파라메터를 변이시키면 안됩니다.
      // 만약 opts가 falsy 하다면 바라는데로 오브젝트가 설정됩니다.
      // 하지만 미묘한 버그를 일으킬지도 모릅니다.
      opts = opts || {};
      // ...
    }

    // still bad
    function handleThings(opts) {
      if (opts === void 0) {
        opts = {};
      }
      // ...
    }

    // good
    function handleThings(opts = {}) {
      // ...
    }
    ```

  - [7.8](#7.8) <a name='7.8'></a> Avoid side effects with default parameters.
  - [7.8](#7.8) <a name='7.8'></a> side effect가 있을 default 파라메터의 이용은 피해 주십시오.

  > Why? They are confusing to reason about.

  > 왜? 혼란을 야기하기 때문입니다.

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

  - [7.9](#7.9) <a name='7.9'></a> Always put default parameters last.
  - [7.9](#7.9) <a name='7.9'></a> 항상 default 파라메터는 뒤쪽에 두십시오.

    ```javascript
    // bad
    function handleThings(opts = {}, name) {
      // ...
    }

    // good
    function handleThings(name, opts = {}) {
      // ...
    }
    ```

- [7.10](#7.10) <a name='7.10'></a> Never use the Function constructor to create a new function.
- [7.10](#7.10) <a name='7.10'></a> 절대 새 함수를 작성하기 위해 Function constructor를 이용하지 마십시오.

  > Why? Creating a function in this way evaluates a string similarly to eval(), which opens vulnerabilities.

  > 왜? 이 방법으로 문자열을 평가시켜 새 함수를 작성하는것은 eval() 과 같은 수준의 취약점을 일으킬 수 있습니다.

  ```javascript
  // bad
  var add = new Function('a', 'b', 'return a + b');

  // still bad
  var subtract = Function('a', 'b', 'return a - b');
  ```

**[⬆ back to top](#목차)**

## Arrow함수(Arrow Functions)

  - [8.1](#8.1) <a name='8.1'></a> When you must use function expressions (as when passing an anonymous function), use arrow function notation.
  - [8.1](#8.1) <a name='8.1'></a> (무명함수를 전달하는 듯한)함수식을 이용하는 경우 arrow함수 표기를 이용해 주십시오.

  > Why? It creates a version of the function that executes in the context of `this`, which is usually what you want, and is a more concise syntax.

  > 왜? arrow함수는 그 context의 `this` 에서 실행하는 버전의 함수를 작성합니다. 이것은 통상 기대대로의 동작을 하고, 보다 간결한 구문이기 때문입니다.

  > Why not? If you have a fairly complicated function, you might move that logic out into its own function declaration.

  > 이용해야만 하지 않나？ 복잡한 함수에서 로직을 정의한 함수의 바깥으로 이동하고 싶을때.

    ```javascript
    // bad
    [1, 2, 3].map(function (x) {
      const y = x + 1;
      return x * y;
    });

    // good
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });
    ```

  - [8.2](#8.2) <a name='8.2'></a> If the function body consists of a single expression, feel free to omit the braces and use the implicit return. Otherwise use a `return` statement.
  - [8.2](#8.2) <a name='8.2'></a> 함수의 본체가 하나의 식으로 구성된 경우에는 중괄호({})를 생략하고 암시적 return을 이용하는것이 가능합니다. 그 외에는 `return` 문을 이용해 주십시오.

  > Why? Syntactic sugar. It reads well when multiple functions are chained together.

  > 왜? 문법 설탕이니까요. 복수의 함수가 연결된 경우에 읽기 쉬워집니다.

  > Why not? If you plan on returning an object.

  > 사용해야만 하지 않아? 오브젝트를 반환할때.

    ```javascript
    // good
    [1, 2, 3].map(number => `A string containing the ${number}.`);

    // bad
    [1, 2, 3].map(number => {
      const nextNumber = number + 1;
      `A string containing the ${nextNumber}.`;
    });

    // good
    [1, 2, 3].map(number => {
      const nextNumber = number + 1;
      return `A string containing the ${nextNumber}.`;
    });
    ```

  - [8.3](#8.3) <a name='8.3'></a> In case the expression spans over multiple lines, wrap it in parentheses for better readability.
  - [8.3](#8.3) <a name='8.3'></a> 식이 복수행에 걸쳐있을 경우는 가독성을 더욱 좋게하기 위해 소괄호()로 감싸 주십시오.

  > Why? It shows clearly where the function starts and ends.

  > 왜? 함수의 개시와 종료부분이 알기쉽게 보이기 때문입니다.

    ```js
    // bad
    [1, 2, 3].map(number => 'As time went by, the string containing the ' +
      `${number} became much longer. So we needed to break it over multiple ` +
      'lines.'
    );

    // good
    [1, 2, 3].map(number => (
      `As time went by, the string containing the ${number} became much ` +
      'longer. So we needed to break it over multiple lines.'
    ));
    ```

  - [8.4](#8.4) <a name='8.4'></a> If your function only takes a single argument, feel free to omit the parentheses.
  - [8.4](#8.4) <a name='8.4'></a> 함수의 인수가 하나인 경우 소괄호()를 생략하는게 가능합니다.

  > Why? Less visual clutter.

  > 왜? 별로 보기 어렵지 않기 때문입니다.

    ```javascript
    // good
    [1, 2, 3].map(x => x * x);

    // good
    [1, 2, 3].reduce((y, x) => x + y);
    ```

**[⬆ back to top](#목차)**


## Classes & Constructors

  - [9.1](#9.1) <a name='9.1'></a> Always use `class`. Avoid manipulating `prototype` directly.
  - [9.1](#9.1) <a name='9.1'></a> `prototype` 을 직접 조작하는것을 피하고 항상 `class` 를 이용해 주십시오.

  > Why? `class` syntax is more concise and easier to reason about.

  > 왜? `class` 구문은 간결하고 의미를 알기 쉽기 때문입니다.

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

  - [9.2](#9.2) <a name='9.2'></a> Use `extends` for inheritance.
  - [9.2](#9.2) <a name='9.2'></a> 상속은 `extends` 를 이용해 주십시오.

  > Why? It is a built-in way to inherit prototype functionality without breaking `instanceof`.

  > 왜? `instanceof` 를 파괴하지 않고 프로토타입 상속을 하기 위해 빌트인 된 방법이기 때문입니다.

    ```javascript
    // bad
    const inherits = require('inherits');
    function PeekableQueue(contents) {
      Queue.apply(this, contents);
    }
    inherits(PeekableQueue, Queue);
    PeekableQueue.prototype.peek = function() {
      return this._queue[0];
    }

    // good
    class PeekableQueue extends Queue {
      peek() {
        return this._queue[0];
      }
    }
    ```

  - [9.3](#9.3) <a name='9.3'></a> Methods can return `this` to help with method chaining.
  - [9.3](#9.3) <a name='9.3'></a> 메소드의 반환값으로 `this` 를 반환하는 것으로 메소드채이닝을 할 수 있습니다.

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


  - [9.4](#9.4) <a name='9.4'></a> It's okay to write a custom toString() method, just make sure it works successfully and causes no side effects.
  - [9.4](#9.4) <a name='9.4'></a> 독자의 toString()을 작성하는것을 허용하지만 올바르게 동작하는지와 side effect 가 없는지만 확인해 주십시오.

    ```javascript
    class Jedi {
      constructor(options = {}) {
        this.name = options.name || 'no name';
      }

      getName() {
        return this.name;
      }

      toString() {
        return `Jedi - ${this.getName()}`;
      }
    }
    ```

**[⬆ back to top](#목차)**


## 모듈(Modules)

  - [10.1](#10.1) <a name='10.1'></a> Always use modules (`import`/`export`) over a non-standard module system. You can always transpile to your preferred module system.
  - [10.1](#10.1) <a name='10.1'></a> 비표준 모듈시스템이 아닌 항상 (`import`/`export`) 를 이용해 주십시오. 이렇게 함으로써 선호하는 모듈시스템에 언제라도 옮겨가는게 가능해 집니다.

  > Why? Modules are the future, let's start using the future now.

  > 왜? 모듈은 미래가 있습니다. 지금 그 미래를 사용하여 시작합시다.

    ```javascript
    // bad
    const AirbnbStyleGuide = require('./AirbnbStyleGuide');
    module.exports = AirbnbStyleGuide.es6;

    // ok
    import AirbnbStyleGuide from './AirbnbStyleGuide';
    export default AirbnbStyleGuide.es6;

    // best
    import { es6 } from './AirbnbStyleGuide';
    export default es6;
    ```

  - [10.2](#10.2) <a name='10.2'></a> Do not use wildcard imports.
  - [10.2](#10.2) <a name='10.2'></a> wildcard import 는 이용하지 마십시오.

  > Why? This makes sure you have a single default export.

  > 왜? single default export 임을 주의할 필요가 있습니다.

    ```javascript
    // bad
    import * as AirbnbStyleGuide from './AirbnbStyleGuide';

    // good
    import AirbnbStyleGuide from './AirbnbStyleGuide';
    ```

  - [10.3](#10.3) <a name='10.3'></a>And do not export directly from an import.
  - [10.3](#10.3) <a name='10.3'></a> import 문으로부터 직접 export 하는것은 하지말아 주십시오.

  > Why? Although the one-liner is concise, having one clear way to import and one clear way to export makes things consistent.

  > 왜? 한줄짜리는 간결하지만 import 와 export 방법을 명확히 한가지로 해서 일관성을 갖는 것이 가능합니다.

    ```javascript
    // bad
    // filename es6.js
    export { es6 as default } from './airbnbStyleGuide';

    // good
    // filename es6.js
    import { es6 } from './AirbnbStyleGuide';
    export default es6;
    ```

**[⬆ back to top](#목차)**

## 이터레이터와 제너레이터(Iterators and Generators)

  - [11.1](#11.1) <a name='11.1'></a> Don't use iterators. Prefer JavaScript's higher-order functions like `map()` and `reduce()` instead of loops like `for-of`.
  - [11.1](#11.1) <a name='11.1'></a> iterators를 이용하지 마십시오. `for-of` 루프 대신에 `map()` 과 `reduce()` 와 같은 JavaScript 고급함수(higher-order functions)를 이용해 주십시오.

  > Why? This enforces our immutable rule. Dealing with pure functions that return values is easier to reason about than side-effects.

  > 왜? 고급함수는 immutable(불변)룰을 적용합니다. side effect에 대해 추측하는거보다 값을 반환하는 순수 함수를 다루는게 간단하기 때문입니다.

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

  - [11.2](#11.2) <a name='11.2'></a> Don't use generators for now.
  - [11.2](#11.2) <a name='11.2'></a> 현시점에서는 generators는 이용하지 마십시오.

  > Why? They don't transpile well to ES5.

  > 왜? ES5로 잘 transpile 하지 않기 때문입니다.

**[⬆ back to top](#목차)**


## 프로퍼티(Properties)

  - [12.1](#12.1) <a name='12.1'></a> Use dot notation when accessing properties.
  - [12.1](#12.1) <a name='12.1'></a> 프로퍼티에 억세스하는 경우는 점 `.` 을 사용해 주십시오.

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

  - [12.2](#12.2) <a name='12.2'></a> Use subscript notation `[]` when accessing properties with a variable.
  - [12.2](#12.2) <a name='12.2'></a> 변수를 사용해 프로퍼티에 억세스하는 경우는 대괄호 `[]` 를 사용해 주십시오.

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

**[⬆ back to top](#목차)**


## 변수(Variables)

  - [13.1](#13.1) <a name='13.1'></a> Always use `const` to declare variables. Not doing so will result in global variables. We want to avoid polluting the global namespace. Captain Planet warned us of that.
  - [13.1](#13.1) <a name='13.1'></a> 변수를 선언 할 때는 항상 `const` 를 사용해 주십시오. 그렇게 하지 않으면 글로벌 변수로 선언됩니다. 글로벌 namespace 를 오염시키지 않도록 캡틴플래닛도 경고하고 있습니다.

    ```javascript
    // bad
    superPower = new SuperPower();

    // good
    const superPower = new SuperPower();
    ```

  - [13.2](#13.2) <a name='13.2'></a> Use one `const` declaration per variable.
  - [13.2](#13.2) <a name='13.2'></a> 하나의 변수선언에 대해 하나의 `const` 를 이용해 주십시오.

    > Why? It's easier to add new variable declarations this way, and you never have to worry about swapping out a `;` for a `,` or introducing punctuation-only diffs.

    > 왜? 이 방법의 경우, 간단히 새 변수를 추가하는게 가능합니다. 또한 `,` 를 `;` 로 바꿔버리는 것에 대해 걱정할 필요가 없습니다.

    ```javascript
    // bad
    const items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // bad
    // (compare to above, and try to spot the mistake)
    const items = getItems(),
        goSportsTeam = true;
        dragonball = 'z';

    // good
    const items = getItems();
    const goSportsTeam = true;
    const dragonball = 'z';
    ```

  - [13.3](#13.3) <a name='13.3'></a> Group all your `const`s and then group all your `let`s.
  - [13.3](#13.3) <a name='13.3'></a> 우선 `const` 를 그룹화하고 다음에 `let` 을 그룹화 해주십시오.

  > Why? This is helpful when later on you might need to assign a variable depending on one of the previous assigned variables.

  > 왜? 이전에 할당한 변수에 대해 나중에 새 변수를 추가하는 경우에 유용하기 때문입니다.

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

  - [13.4](#13.4) <a name='13.4'></a> Assign variables where you need them, but place them in a reasonable place.
  - [13.4](#13.4) <a name='13.4'></a> 변수를 할당할때는 필요하고 합리적인 장소에 두시기 바랍니다.

  > Why? `let` and `const` are block scoped and not function scoped.

  > 왜? `let` 과 `const` 는 블록스코프이기 때문입니다. 함수스코프가 아닙니다.

    ```javascript
    // good
    function() {
      test();
      console.log('doing stuff..');

      //..other stuff..

      const name = getName();

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // bad - unnecessary function call
    // 필요없는 함수 호출
    function(hasName) {
      const name = getName();

      if (!hasName) {
        return false;
      }

      this.setFirstName(name);

      return true;
    }

    // good
    function(hasName) {
      if (!hasName) {
        return false;
      }

      const name = getName();
      this.setFirstName(name);

      return true;
    }
    ```

**[⬆ back to top](#목차)**


## Hoisting

  - [14.1](#14.1) <a name='14.1'></a> `var` declarations get hoisted to the top of their scope, their assignment does not. `const` and `let` declarations are blessed with a new concept called [Temporal Dead Zones (TDZ)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#Temporal_dead_zone_and_errors_with_let). It's important to know why [typeof is no longer safe](http://es-discourse.com/t/why-typeof-is-no-longer-safe/15).
  - [14.1](#14.1) <a name='14.1'></a> `var` 선언은 할당이 없이 스코프의 선두에 hoist 됩니다. `const` 와 `let` 선언은[Temporal Dead Zones (TDZ)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#Temporal_dead_zone_and_errors_with_let) 라고 불리는 새로운 컨셉의 혜택을 받고 있습니다. 이것은 [왜 typeof 는 더이상 안전하지 않은가](http://es-discourse.com/t/why-typeof-is-no-longer-safe/15)를  알고있는 것이 중요합니다.

    ```javascript
    // we know this wouldn't work (assuming there
    // is no notDefined global variable)
    // (notDefined 가 글로벌변수에 존재하지 않는다고 판정한 경우.)
    // 잘 동작하지 않습니다.
    function example() {
      console.log(notDefined); // => throws a ReferenceError
    }

    // creating a variable declaration after you
    // reference the variable will work due to
    // variable hoisting. Note: the assignment
    // value of `true` is not hoisted.
    // 그 변수를 참조하는 코드의 뒤에서 그 변수를 선언한 경우
    // 변수가 hoist 된 상태에서 동작합니다..
    // 주의：`true` 라는 값 자체는 hoist 되지 않습니다.
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

    // The interpreter is hoisting the variable
    // declaration to the top of the scope,
    // which means our example could be rewritten as:
    // 인터프리터는 변수선언을 스코프의 선두에 hoist 합니다.
    // 위의 예는 다음과 같이 다시 쓸수 있습니다.
    function example() {
      let declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }

    // using const and let
    // const 와 let 을 이용한 경우
    function example() {
      console.log(declaredButNotAssigned); // => throws a ReferenceError
      console.log(typeof declaredButNotAssigned); // => throws a ReferenceError
      const declaredButNotAssigned = true;
    }
    ```

  - [14.2](#14.2) <a name='14.2'></a> Anonymous function expressions hoist their variable name, but not the function assignment.
  - [14.2](#14.2) <a name='14.2'></a> 무명함수의 경우 함수가 할당되기 전의 변수가 hoist 됩니다.

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function() {
        console.log('anonymous function expression');
      };
    }
    ```

  - [14.3](#14.3) <a name='14.3'></a> Named function expressions hoist the variable name, not the function name or the function body.
  - [14.3](#14.3) <a name='14.3'></a> 명명함수의 경우도 똑같이 변수가 hoist 됩니다. 함수명이나 함수본체는 hoist 되지 않습니다.

    ```javascript
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      superPower(); // => ReferenceError superPower is not defined

      var named = function superPower() {
        console.log('Flying');
      };
    }

    // the same is true when the function name
    // is the same as the variable name.
    // 함수명과 변수명이 같은 경우도 같은 현상이 발생합니다.
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      var named = function named() {
        console.log('named');
      }
    }
    ```

  - [14.4](#14.4) <a name='14.4'></a> Function declarations hoist their name and the function body.
  - [14.4](#14.4) <a name='14.4'></a> 함수선언은 함수명과 함수본체가 hoist 됩니다.

    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }
    ```

  - For more information refer to [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting/) by [Ben Cherry](http://www.adequatelygood.com/).
  - 더 자세한건 이쪽을 참고해 주십시오. [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting/) by [Ben Cherry](http://www.adequatelygood.com/).

**[⬆ back to top](#목차)**


## 조건식과 등가식(Comparison Operators & Equality)

  - [15.1](#15.1) <a name='15.1'></a> Use `===` and `!==` over `==` and `!=`.
  - [15.1](#15.1) <a name='15.1'></a> `==` 이나 `!=` 보다 `===` 와 `!==` 을 사용해 주십시오.

  - [15.2](#15.2) <a name='15.2'></a> Conditional statements such as the `if` statement evaluate their expression using coercion with the `ToBoolean` abstract method and always follow these simple rules:
    + **Objects** evaluate to **true**
    + **Undefined** evaluates to **false**
    + **Null** evaluates to **false**
    + **Booleans** evaluate to **the value of the boolean**
    + **Numbers** evaluate to **false** if **+0, -0, or NaN**, otherwise **true**
    + **Strings** evaluate to **false** if an empty string `''`, otherwise **true**
  - [15.2](#15.2) <a name='15.2'></a> `if` 문과 같은 조건식은 `ToBoolean` 메소드에 의한 강제형변환으로 평가되어 항상 다음과 같은 심플한 룰을 따릅니다.
    + **오브젝트** 는 **true** 로 평가됩니다.
    + **undefined** 는 **false** 로 평가됩니다.
    + **null** 은 **false** 로 평가됩니다.
    + **부울값** 은 **boolean형의 값** 으로 평가됩니다.
    + **수치** 는 **true** 로 평가됩니다. 하지만 **+0, -0, or NaN** 의 경우는 **false** 입니다.
    + **문자열** 은 **true** 로 평가됩니다. 하지만 빈문자 `''` 의 경우는 **false** 입니다.

    ```javascript
    if ([0]) {
      // true
      // An array is an object, objects evaluate to true
      // 배열은 오브젝트이므로 true 로 평가됩니다.
    }
    ```

  - [15.3](#15.3) <a name='15.3'></a> Use shortcuts.
  - [15.3](#15.3) <a name='15.3'></a> 단축형을 사용해 주십시오.

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

  - [15.4](#15.4) <a name='15.4'></a> For more information see [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll.
  - [15.4](#15.4) <a name='15.4'></a> 더 자세한건 이쪽을 참고해 주십시오. [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll.

**[⬆ back to top](#목차)**


## 블록(Blocks)

  - [16.1](#16.1) <a name='16.1'></a> Use braces with all multi-line blocks.
  - [16.1](#16.1) <a name='16.1'></a> 복수행의 블록에는 중괄호 ({}) 를 사용해 주십시오.

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

  - [16.2](#16.2) <a name='16.2'></a> If you're using multi-line blocks with `if` and `else`, put `else` on the same line as your
    `if` block's closing brace.
  - [16.2](#16.2) <a name='16.2'></a> 복수행 블록의 `if` 와 `else` 를 이용하는 경우 `else` 는 `if` 블록 끝의 중괄호(})와 같은 행에 위치시켜 주십시오.

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


**[⬆ back to top](#목차)**


## 코멘트(Comments)

  - [17.1](#17.1) <a name='17.1'></a> Use `/** ... */` for multi-line comments. Include a description, specify types and values for all parameters and return values.
  - [17.1](#17.1) <a name='17.1'></a> 복수행의 코멘트는 `/** ... */` 을 사용해 주십시오. 그 안에는 설명과 모든 파라메터, 반환값에 대해 형이나 값을 기술해 주십시오.

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

  - [17.2](#17.2) <a name='17.2'></a> Use `//` for single line comments. Place single line comments on a newline above the subject of the comment. Put an empty line before the comment unless it's on the first line of a block.
  - [17.2](#17.2) <a name='17.2'></a> 단일행 코멘트에는 `//` 을 사용해 주십시오. 코멘트를 추가하고 싶은 코드의 상부에 배치해 주십시오. 또한, 코멘트의 앞에 빈행을 넣어 주십시오.

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

    // also good
    function getType() {
      // set the default type to 'no type'
      const type = this._type || 'no type';

      return type;
    }
    ```

  - [17.3](#17.3) <a name='17.3'></a> Prefixing your comments with `FIXME` or `TODO` helps other developers quickly understand if you're pointing out a problem that needs to be revisited, or if you're suggesting a solution to the problem that needs to be implemented. These are different than regular comments because they are actionable. The actions are `FIXME -- need to figure this out` or `TODO -- need to implement`.
  - [17.3](#17.3) <a name='17.3'></a> 문제를 지적하고 재고를 촉구하는 경우나 문제의 해결책을 제안하는 경우 등, 코멘트의 앞에  `FIXME` 나 `TODO` 를 붙이는 것으로 다른 개발자의 빠른 이해를 도울수 있습니다. 이런것들은 어떤 액션을 따른다는 의미로 통상의 코멘트와는 다릅니다. 액션이라는 것은 `FIXME -- 해결이 필요` 또는 `TODO -- 구현이 필요` 를 뜻합니다.

  - [17.4](#17.4) <a name='17.4'></a> Use `// FIXME:` to annotate problems.
  - [17.4](#17.4) <a name='17.4'></a> 문제에 대한 주석으로써 `// FIXME:` 를 사용해 주십시오.

    ```javascript
    class Calculator extends Abacus {
      constructor() {
        super();

        // FIXME: shouldn't use a global here
        // FIXME: 글로벌변수를 사용해서는 안됨.
        total = 0;
      }
    }
    ```

  - [17.5](#17.5) <a name='17.5'></a> Use `// TODO:` to annotate solutions to problems.
  - [17.5](#17.5) <a name='17.5'></a> 문제의 해결책에 대한 주석으로 `// TODO:` 를 사용해 주십시오.

    ```javascript
    class Calculator extends Abacus {
      constructor() {
        super();

        // TODO: total should be configurable by an options param
        // TODO: total 은 옵션 파라메터로 설정해야함.
        this.total = 0;
      }
    }
    ```

**[⬆ back to top](#목차)**


## 공백(Whitespace)

  - [18.1](#18.1) <a name='18.1'></a> Use soft tabs set to 2 spaces.
  - [18.1](#18.1) <a name='18.1'></a> 탭에는 스페이스 2개를 설정해 주십시오.

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

  - [18.2](#18.2) <a name='18.2'></a> Place 1 space before the leading brace.
  - [18.2](#18.2) <a name='18.2'></a> 주요 중괄호 ({}) 앞에는 스페이스를 1개 넣어 주십시오.

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

  - [18.3](#18.3) <a name='18.3'></a> Place 1 space before the opening parenthesis in control statements (`if`, `while` etc.). Place no space before the argument list in function calls and declarations.
  - [18.3](#18.3) <a name='18.3'></a> 제어구문 (`if` 문이나 `while` 문 등) 의 소괄호 (()) 앞에는 스페이스를 1개 넣어 주십시오. 함수선언이나 함수호출시 인수리스트의 앞에는 스페이스를 넣지 마십시오.

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

  - [18.4](#18.4) <a name='18.4'></a> Set off operators with spaces.
  - [18.4](#18.4) <a name='18.4'></a> 연산자 사이에는 스페이스를 넣어 주십시오.

    ```javascript
    // bad
    const x=y+5;

    // good
    const x = y + 5;
    ```

  - [18.5](#18.5) <a name='18.5'></a> End files with a single newline character.
  - [18.5](#18.5) <a name='18.5'></a> 파일 끝에는 개행문자를 1개 넣어 주십시오.

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

  - [18.6](#18.6) <a name='18.6'></a> Use indentation when making long method chains. Use a leading dot, which
    emphasizes that the line is a method call, not a new statement.
  - [18.6](#18.6) <a name='18.6'></a> 길게 메소드를 채이닝하는 경우는 인덴트를 이용해 주십시오. 행이 새로운 문이 아닌 메소드 호출인 것을 강조하기 위해서 선두에 점 (.) 을 배치해 주십시오.

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
      .enter().append('svg:svg')
        .classed('led', true)
        .attr('width', (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);
    ```

  - [18.7](#18.7) <a name='18.7'></a> Leave a blank line after blocks and before the next statement.
  - [18.7](#18.7) <a name='18.7'></a> 문의 앞과 블록의 뒤에는 빈행을 남겨 주십시오.

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

    // bad
    const arr = [
      function foo() {
      },
      function bar() {
      },
    ];
    return arr;

    // good
    const arr = [
      function foo() {
      },

      function bar() {
      },
    ];

    return arr;
    ```

  - [18.8](#18.8) <a name='18.8'></a> Do not pad your blocks with blank lines.
  - [18.8](#18.8) <a name='18.8'></a> 블록에 빈행을 끼워 넣지 마십시오.

    ```javascript
    // bad
    function bar() {

      console.log(foo);

    }

    // also bad
    if (baz) {

      console.log(qux);
    } else {
      console.log(foo);

    }

    // good
    function bar() {
      console.log(foo);
    }

    // good
    if (baz) {
      console.log(qux);
    } else {
      console.log(foo);
    }
    ```

  - [18.9](#18.9) <a name='18.9'></a> Do not add spaces inside parentheses.
  - [18.9](#18.9) <a name='18.9'></a> 소괄호()의 안쪽에 스페이스를 추가하지 마십시오.

    ```javascript
    // bad
    function bar( foo ) {
      return foo;
    }

    // good
    function bar(foo) {
      return foo;
    }

    // bad
    if ( foo ) {
      console.log(foo);
    }

    // good
    if (foo) {
      console.log(foo);
    }
    ```

  - [18.10](#18.10) <a name='18.10'></a> Do not add spaces inside brackets.
  - [18.10](#18.10) <a name='18.10'></a> 대괄호([])의 안쪽에 스페이스를 추가하지 마십시오.

    ```javascript
    // bad
    const foo = [ 1, 2, 3 ];
    console.log(foo[ 0 ]);

    // good
    const foo = [1, 2, 3];
    console.log(foo[0]);
    ```

  - [18.11](#18.11) <a name='18.11'></a> Add spaces inside curly braces.
  - [18.11](#18.11) <a name='18.11'></a> 중괄호({})의 안쪽에 스페이스를 추가해 주십시오.

    ```javascript
    // bad
    const foo = {clark: 'kent'};

    // good
    const foo = { clark: 'kent' };
    ```

**[⬆ back to top](#목차)**

## 콤마(Commas)

  - [19.1](#19.1) <a name='19.1'></a> Leading commas: **Nope.**
  - [19.1](#19.1) <a name='19.1'></a> 선두의 콤마 **하지마요**

    ```javascript
    // bad
    const story = [
        once
      , upon
      , aTime
    ];

    // good
    const story = [
      once,
      upon,
      aTime,
    ];

    // bad
    const hero = {
        firstName: 'Ada'
      , lastName: 'Lovelace'
      , birthYear: 1815
      , superPower: 'computers'
    };

    // good
    const hero = {
      firstName: 'Ada',
      lastName: 'Lovelace',
      birthYear: 1815,
      superPower: 'computers',
    };
    ```

  - [19.2](#19.2) <a name='19.2'></a> Additional trailing comma: **Yup.**
  - [19.2](#19.2) <a name='19.2'></a> 끝의 콤마 **좋아요**

  > Why? This leads to cleaner git diffs. Also, transpilers like Babel will remove the additional trailing comma in the transpiled code which means you don't have to worry about the [trailing comma problem](es5/README.md#commas) in legacy browsers.

  > 왜? 이것은 깨끗한 git의 diffs 로 이어집니다. 또한 Babel과 같은 트랜스파일러는 transpile 하는 사이에 쓸데없는 끝의 콤마를 제거합니다. 이것은 레거시브라우저에서의 [불필요한 콤마 문제](./README.md#commas)를 고민할 필요가 없다는것을 의미합니다.

    ```javascript
    // bad - git diff without trailing comma
    const hero = {
         firstName: 'Florence',
    -    lastName: 'Nightingale'
    +    lastName: 'Nightingale',
    +    inventorOf: ['coxcomb graph', 'modern nursing']
    };

    // good - git diff with trailing comma
    const hero = {
         firstName: 'Florence',
         lastName: 'Nightingale',
    +    inventorOf: ['coxcomb chart', 'modern nursing'],
    };

    // bad
    const hero = {
      firstName: 'Dana',
      lastName: 'Scully'
    };

    const heroes = [
      'Batman',
      'Superman'
    ];

    // good
    const hero = {
      firstName: 'Dana',
      lastName: 'Scully',
    };

    const heroes = [
      'Batman',
      'Superman',
    ];
    ```

**[⬆ back to top](#목차)**


## 세미콜론(Semicolons)

  - [20.1](#20.1) <a name='20.1'></a> **Yup.**
  - [20.1](#20.1) <a name='20.1'></a> **씁시다**

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

    // good (guards against the function becoming an argument when two files with IIFEs are concatenated)
    // good (즉시함수가 연결된 2개의 파일일때 인수가 되는 부분을 보호합니다.
    ;(() => {
      const name = 'Skywalker';
      return name;
    })();
    ```

    [Read more](http://stackoverflow.com/questions/7365172/semicolon-before-self-invoking-function/7365214%237365214).

**[⬆ back to top](#목차)**


## 형변환과 강제(Type Casting & Coercion)

  - [21.1](#21.1) <a name='21.1'></a> Perform type coercion at the beginning of the statement.
  - [21.1](#21.1) <a name='21.1'></a> 문의 선두에서 형의 강제를 행합니다.

  - [21.2](#21.2) <a name='21.2'></a> Strings:
  - [21.2](#21.2) <a name='21.2'></a> 문자열의 경우:

    ```javascript
    //  => this.reviewScore = 9;

    // bad
    const totalScore = this.reviewScore + '';

    // good
    const totalScore = String(this.reviewScore);
    ```

  - [21.3](#21.3) <a name='21.3'></a> Numbers: Use `Number` for type casting and `parseInt` always with a radix for parsing strings.
  - [21.3](#21.3) <a name='21.3'></a> 수치의 경우: `Number` 로 형변환하는 경우는 `parseInt` 를 이용하고, 항상 형변환을 위한 기수를 인수로 넘겨 주십시오.

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

  - [21.4](#21.4) <a name='21.4'></a> If for whatever reason you are doing something wild and `parseInt` is your bottleneck and need to use Bitshift for [performance reasons](http://jsperf.com/coercion-vs-casting/3), leave a comment explaining why and what you're doing.
  - [21.4](#21.4) <a name='21.4'></a> 무언가의 이유로 인해 `parseInt` 가 bottleneck 이 되어, [성능적인 이유](http://jsperf.com/coercion-vs-casting/3)로 Bitshift를 사용할 필요가 있는 경우
  하려고 했던 것에 대해, 왜(why) 와 무엇(what)의 설명을 코멘트로 해서 남겨 주십시오.

    ```javascript
    // good
    /**
     * parseInt was the reason my code was slow.
     * Bitshifting the String to coerce it to a
     * Number made it a lot faster.
     * parseInt 가 원인으로 느렸음.
     * Bitshift를 통한 수치로의 문자열 강제 형변환으로
     * 성능을 개선시킴.
     */
    const val = inputValue >> 0;
    ```

  - [21.5](#21.5) <a name='21.5'></a> **Note:** Be careful when using bitshift operations. Numbers are represented as [64-bit values](http://es5.github.io/#x4.3.19), but Bitshift operations always return a 32-bit integer ([source](http://es5.github.io/#x11.7)). Bitshift can lead to unexpected behavior for integer values larger than 32 bits. [Discussion](https://github.com/airbnb/javascript/issues/109). Largest signed 32-bit Int is 2,147,483,647:
  - [21.5](#21.5) <a name='21.5'></a> **주의:** bitshift를 사용하는 경우의 주의사항. 수치는 [64비트 값](http://es5.github.io/#x4.3.19)으로 표현되어 있으나 bitshift 연산한 경우는 항상 32비트 integer 로 넘겨집니다.([소스](http://es5.github.io/#x11.7)).
  32비트 이상의 int 를 bitshift 하는 경우 예상치 못한 현상을 야기할 수 있습니다.([토론](https://github.com/airbnb/javascript/issues/109)) 부호가 포함된 32비트 정수의 최대치는 2,147,483,647 입니다.

    ```javascript
    2147483647 >> 0 //=> 2147483647
    2147483648 >> 0 //=> -2147483648
    2147483649 >> 0 //=> -2147483647
    ```

  - [21.6](#21.6) <a name='21.6'></a> Booleans:
  - [21.6](#21.6) <a name='21.6'></a> 부울값의 경우:

    ```javascript
    const age = 0;

    // bad
    const hasAge = new Boolean(age);

    // good
    const hasAge = Boolean(age);

    // good
    const hasAge = !!age;
    ```

**[⬆ back to top](#목차)**


## 명명규칙(Naming Conventions)

  - [22.1](#22.1) <a name='22.1'></a> Avoid single letter names. Be descriptive with your naming.
  - [22.1](#22.1) <a name='22.1'></a> 1문자의 이름은 피해 주십시오. 이름으로부터 의도가 읽혀질수 있게 해주십시오.

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

  - [22.2](#22.2) <a name='22.2'></a> Use camelCase when naming objects, functions, and instances.
  - [22.2](#22.2) <a name='22.2'></a> 오브젝트, 함수 그리고 인스턴스에는 camelCase를 사용해 주십시오.

    ```javascript
    // bad
    const OBJEcttsssss = {};
    const this_is_my_object = {};
    function c() {}

    // good
    const thisIsMyObject = {};
    function thisIsMyFunction() {}
    ```

  - [22.3](#22.3) <a name='22.3'></a> Use PascalCase when naming constructors or classes.
  - [22.3](#22.3) <a name='22.3'></a> 클래스나 constructor에는 PascalCase 를 사용해 주십시오.

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

  - [22.4](#22.4) <a name='22.4'></a> Use a leading underscore `_` when naming private properties.
  - [22.4](#22.4) <a name='22.4'></a> private 프로퍼티명은 선두에 언더스코어 `_` 를사용해 주십시오.

    ```javascript
    // bad
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';

    // good
    this._firstName = 'Panda';
    ```

  - [22.5](#22.5) <a name='22.5'></a> Don't save references to `this`. Use arrow functions or Function#bind.
  - [22.5](#22.5) <a name='22.5'></a> `this` 의 참조를 보존하는것은 피해주십시오. arrow함수나 Function#bind 를 이용해 주십시오.

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

  - [22.6](#22.6) <a name='22.6'></a> If your file exports a single class, your filename should be exactly the name of the class.
  - [22.6](#22.6) <a name='22.6'></a> 파일을 1개의 클래스로 export 하는 경우, 파일명은 클래스명과 완전히 일치시키지 않으면 안됩니다.

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

  - [22.7](#22.7) <a name='22.7'></a> Use camelCase when you export-default a function. Your filename should be identical to your function's name.
  - [22.7](#22.7) <a name='22.7'></a> Default export가 함수일 경우, camelCase를 이용해 주십시오. 파일명은 함수명과 동일해야 합니다.

    ```javascript
    function makeStyleGuide() {
    }

    export default makeStyleGuide;
    ```

  - [22.8](#22.8) <a name='22.8'></a> Use PascalCase when you export a singleton / function library / bare object.
  - [22.8](#22.8) <a name='22.8'></a> singleton / function library / 빈오브젝트를 export 하는 경우, PascalCase를 이용해 주십시오.

    ```javascript
    const AirbnbStyleGuide = {
      es6: {
      }
    };

    export default AirbnbStyleGuide;
    ```


**[⬆ back to top](#목차)**


## 억세서(Accessors)

  - [23.1](#23.1) <a name='23.1'></a> Accessor functions for properties are not required.
  - [23.1](#23.1) <a name='23.1'></a> 프로퍼티를 위한 억세서 (Accessor) 함수는 필수는 아닙니다.

  - [23.2](#23.2) <a name='23.2'></a> If you do make accessor functions use getVal() and setVal('hello').
  - [23.2](#23.2) <a name='23.2'></a> 억세서 함수가 필요한 경우, `getVal()` 이나 `setVal('hello')` 로 해주십시오.

    ```javascript
    // bad
    dragon.age();

    // good
    dragon.getAge();

    // bad
    dragon.age(25);

    // good
    dragon.setAge(25);
    ```

  - [23.3](#23.3) <a name='23.3'></a> If the property is a `boolean`, use `isVal()` or `hasVal()`.
  - [23.3](#23.3) <a name='23.3'></a> 프로퍼티가 `boolean` 인 경우, `isVal()` 이나 `hasVal()` 로 해주십시오.

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

  - [23.4](#23.4) <a name='23.4'></a> It's okay to create get() and set() functions, but be consistent.
  - [23.4](#23.4) <a name='23.4'></a> 일관된 경우, `get()` 과 `set()` 으로 함수를 작성해도 좋습니다.

    ```javascript
    class Jedi {
      constructor(options = {}) {
        const lightsaber = options.lightsaber || 'blue';
        this.set('lightsaber', lightsaber);
      }

      set(key, val) {
        this[key] = val;
      }

      get(key) {
        return this[key];
      }
    }
    ```

**[⬆ back to top](#목차)**


## 이벤트(Events)

  - [24.1](#24.1) <a name='24.1'></a> When attaching data payloads to events (whether DOM events or something more proprietary like Backbone events), pass a hash instead of a raw value. This allows a subsequent contributor to add more data to the event payload without finding and updating every handler for the event. For example, instead of:
  - [24.1](#24.1) <a name='24.1'></a> (DOM이벤트나 Backbone events 와 같은 독자의) 이벤트로 payload의 값을 넘길 경우는 raw값 보다는 해시값을 넘겨 주십시오.
이렇게 함으로써, 이후 기여자가 이벤트에 관련한 모든 핸들러를 찾아서 갱신하는 대신 이벤트 payload에 값을 추가하는 것이 가능합니다. 예를들면 아래와 같이

    ```javascript
    // bad
    $(this).trigger('listingUpdated', listing.id);

    ...

    $(this).on('listingUpdated', function(e, listingId) {
      // do something with listingId
    });
    ```

    prefer:
    이쪽이 좋습니다:

    ```javascript
    // good
    $(this).trigger('listingUpdated', { listingId: listing.id });

    ...

    $(this).on('listingUpdated', function(e, data) {
      // do something with data.listingId
    });
    ```

  **[⬆ back to top](#목차)**


## jQuery

  - [25.1](#25.1) <a name='25.1'></a> Prefix jQuery object variables with a `$`.
  - [25.1](#25.1) <a name='25.1'></a> jQuery오브젝트의 변수는 선두에 `$` 를 부여해 주십시오.

    ```javascript
    // bad
    const sidebar = $('.sidebar');

    // good
    const $sidebar = $('.sidebar');

    // good
    const $sidebarBtn = $('.sidebar-btn');
    ```

  - [25.2](#25.2) <a name='25.2'></a> Cache jQuery lookups.
  - [25.2](#25.2) <a name='25.2'></a> jQuery의 검색결과를 캐시해 주십시오.

    ```javascript
    // bad
    function setSidebar() {
      $('.sidebar').hide();

      // ...stuff...

      $('.sidebar').css({
        'background-color': 'pink'
      });
    }

    // good
    function setSidebar() {
      const $sidebar = $('.sidebar');
      $sidebar.hide();

      // ...stuff...

      $sidebar.css({
        'background-color': 'pink'
      });
    }
    ```

  - [25.3](#25.3) <a name='25.3'></a> For DOM queries use Cascading `$('.sidebar ul')` or parent > child `$('.sidebar > ul')`. [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)
  - [25.3](#25.3) <a name='25.3'></a> DOM 검색에는 `$('.sidebar ul')` 이나 `$('.sidebar > ul')` 와 같은 Cascading 을 사용해 주십시오. [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)

  - [25.4](#25.4) <a name='25.4'></a> Use `find` with scoped jQuery object queries.
  - [25.4](#25.4) <a name='25.4'></a> 한정된 jQuery 오브젝트 쿼리에는 `find` 를 사용해 주십시오.

    ```javascript
    // bad
    $('ul', '.sidebar').hide();

    // bad
    $('.sidebar').find('ul').hide();

    // good
    $('.sidebar ul').hide();

    // good
    $('.sidebar > ul').hide();

    // good
    $sidebar.find('ul').hide();
    ```

**[⬆ back to top](#목차)**


## ECMAScript 5 Compatibility

  - [26.1](#26.1) <a name='26.1'></a> Refer to [Kangax](https://twitter.com/kangax/)'s ES5 [compatibility table](http://kangax.github.io/es5-compat-table/).

**[⬆ back to top](#목차)**

## ECMAScript 6 Styles

  - [27.1](#27.1) <a name='27.1'></a> This is a collection of links to the various es6 features.

1. [Arrow Functions](#arrow-functions)
1. [Classes](#constructors)
1. [Object Shorthand](#es6-object-shorthand)
1. [Object Concise](#es6-object-concise)
1. [Object Computed Properties](#es6-computed-properties)
1. [Template Strings](#es6-template-literals)
1. [Destructuring](#destructuring)
1. [Default Parameters](#es6-default-parameters)
1. [Rest](#es6-rest)
1. [Array Spreads](#es6-array-spreads)
1. [Let and Const](#references)
1. [Iterators and Generators](#iterators-and-generators)
1. [Modules](#modules)

**[⬆ back to top](#목차)**

## Testing

  - [28.1](#28.1) <a name="28.1"></a> **Yup.**

    ```javascript
    function () {
      return true;
    }
    ```

  - [28.2](#28.2) <a name="28.2"></a> **No, but seriously**:
   - Whichever testing framework you use, you should be writing tests!
   - Strive to write many small pure functions, and minimize where mutations occur.
   - Be cautious about stubs and mocks - they can make your tests more brittle.
   - We primarily use [`mocha`](https://www.npmjs.com/package/mocha) at Airbnb. [`tape`](https://www.npmjs.com/package/tape) is also used occasionally for small, separate modules.
   - 100% test coverage is a good goal to strive for, even if it's not always practical to reach it.
   - Whenever you fix a bug, _write a regression test_. A bug fixed without a regression test is almost certainly going to break again in the future.

**[⬆ back to top](#목차)**


## Performance

  - [On Layout & Web Performance](http://www.kellegous.com/j/2013/01/26/layout-performance/)
  - [String vs Array Concat](http://jsperf.com/string-vs-array-concat/2)
  - [Try/Catch Cost In a Loop](http://jsperf.com/try-catch-in-loop-cost)
  - [Bang Function](http://jsperf.com/bang-function)
  - [jQuery Find vs Context, Selector](http://jsperf.com/jquery-find-vs-context-sel/13)
  - [innerHTML vs textContent for script text](http://jsperf.com/innerhtml-vs-textcontent-for-script-text)
  - [Long String Concatenation](http://jsperf.com/ya-string-concat)
  - Loading...

**[⬆ back to top](#목차)**


## Resources

**Learning ES6**

  - [Draft ECMA 2015 (ES6) Spec](https://people.mozilla.org/~jorendorff/es6-draft.html)
  - [ExploringJS](http://exploringjs.com/)
  - [ES6 Compatibility Table](https://kangax.github.io/compat-table/es6/)
  - [Comprehensive Overview of ES6 Features](http://es6-features.org/)

**Read This**

  - [Standard ECMA-262](http://www.ecma-international.org/ecma-262/6.0/index.html)

**Tools**

  - Code Style Linters
    + [ESlint](http://eslint.org/) - [Airbnb Style .eslintrc](https://github.com/airbnb/javascript/blob/master/linters/.eslintrc)
    + [JSHint](http://jshint.com/) - [Airbnb Style .jshintrc](https://github.com/airbnb/javascript/blob/master/linters/jshintrc)
    + [JSCS](https://github.com/jscs-dev/node-jscs) - [Airbnb Style Preset](https://github.com/jscs-dev/node-jscs/blob/master/presets/airbnb.json)

**Other Style Guides**

  - [Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
  - [jQuery Core Style Guidelines](http://contribute.jquery.org/style-guide/js/)
  - [Principles of Writing Consistent, Idiomatic JavaScript](https://github.com/rwaldron/idiomatic.js)

**Other Styles**

  - [Naming this in nested functions](https://gist.github.com/cjohansen/4135065) - Christian Johansen
  - [Conditional Callbacks](https://github.com/airbnb/javascript/issues/52) - Ross Allen
  - [Popular JavaScript Coding Conventions on Github](http://sideeffect.kr/popularconvention/#javascript) - JeongHoon Byun
  - [Multiple var statements in JavaScript, not superfluous](http://benalman.com/news/2012/05/multiple-var-statements-javascript/) - Ben Alman

**Further Reading**

  - [Understanding JavaScript Closures](http://javascriptweblog.wordpress.com/2010/10/25/understanding-javascript-closures/) - Angus Croll
  - [Basic JavaScript for the impatient programmer](http://www.2ality.com/2013/06/basic-javascript.html) - Dr. Axel Rauschmayer
  - [You Might Not Need jQuery](http://youmightnotneedjquery.com/) - Zack Bloom & Adam Schwartz
  - [ES6 Features](https://github.com/lukehoban/es6features) - Luke Hoban
  - [Frontend Guidelines](https://github.com/bendc/frontend-guidelines) - Benjamin De Cock

**Books**

  - [JavaScript: The Good Parts](http://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742) - Douglas Crockford
  - [JavaScript Patterns](http://www.amazon.com/JavaScript-Patterns-Stoyan-Stefanov/dp/0596806752) - Stoyan Stefanov
  - [Pro JavaScript Design Patterns](http://www.amazon.com/JavaScript-Design-Patterns-Recipes-Problem-Solution/dp/159059908X)  - Ross Harmes and Dustin Diaz
  - [High Performance Web Sites: Essential Knowledge for Front-End Engineers](http://www.amazon.com/High-Performance-Web-Sites-Essential/dp/0596529309) - Steve Souders
  - [Maintainable JavaScript](http://www.amazon.com/Maintainable-JavaScript-Nicholas-C-Zakas/dp/1449327680) - Nicholas C. Zakas
  - [JavaScript Web Applications](http://www.amazon.com/JavaScript-Web-Applications-Alex-MacCaw/dp/144930351X) - Alex MacCaw
  - [Pro JavaScript Techniques](http://www.amazon.com/Pro-JavaScript-Techniques-John-Resig/dp/1590597273) - John Resig
  - [Smashing Node.js: JavaScript Everywhere](http://www.amazon.com/Smashing-Node-js-JavaScript-Everywhere-Magazine/dp/1119962595) - Guillermo Rauch
  - [Secrets of the JavaScript Ninja](http://www.amazon.com/Secrets-JavaScript-Ninja-John-Resig/dp/193398869X) - John Resig and Bear Bibeault
  - [Human JavaScript](http://humanjavascript.com/) - Henrik Joreteg
  - [Superhero.js](http://superherojs.com/) - Kim Joar Bekkelund, Mads Mobæk, & Olav Bjorkoy
  - [JSBooks](http://jsbooks.revolunet.com/) - Julien Bouquillon
  - [Third Party JavaScript](https://www.manning.com/books/third-party-javascript) - Ben Vinegar and Anton Kovalyov
  - [Effective JavaScript: 68 Specific Ways to Harness the Power of JavaScript](http://amzn.com/0321812182) - David Herman
  - [Eloquent JavaScript](http://eloquentjavascript.net/) - Marijn Haverbeke
  - [You Don't Know JS: ES6 & Beyond](http://shop.oreilly.com/product/0636920033769.do) - Kyle Simpson

**Blogs**

  - [DailyJS](http://dailyjs.com/)
  - [JavaScript Weekly](http://javascriptweekly.com/)
  - [JavaScript, JavaScript...](http://javascriptweblog.wordpress.com/)
  - [Bocoup Weblog](https://bocoup.com/weblog)
  - [Adequately Good](http://www.adequatelygood.com/)
  - [NCZOnline](https://www.nczonline.net/)
  - [Perfection Kills](http://perfectionkills.com/)
  - [Ben Alman](http://benalman.com/)
  - [Dmitry Baranovskiy](http://dmitry.baranovskiy.com/)
  - [Dustin Diaz](http://dustindiaz.com/)
  - [nettuts](http://code.tutsplus.com/?s=javascript)

**Podcasts**

  - [JavaScript Jabber](https://devchat.tv/js-jabber/)


**[⬆ back to top](#목차)**

## In the Wild

  This is a list of organizations that are using this style guide. Send us a pull request and we'll add you to the list.

  - **Aan Zee**: [AanZee/javascript](https://github.com/AanZee/javascript)
  - **Adult Swim**: [adult-swim/javascript](https://github.com/adult-swim/javascript)
  - **Airbnb**: [airbnb/javascript](https://github.com/airbnb/javascript)
  - **Apartmint**: [apartmint/javascript](https://github.com/apartmint/javascript)
  - **Avalara**: [avalara/javascript](https://github.com/avalara/javascript)
  - **Billabong**: [billabong/javascript](https://github.com/billabong/javascript)
  - **Blendle**: [blendle/javascript](https://github.com/blendle/javascript)
  - **ComparaOnline**: [comparaonline/javascript](https://github.com/comparaonline/javascript-style-guide)
  - **Compass Learning**: [compasslearning/javascript-style-guide](https://github.com/compasslearning/javascript-style-guide)
  - **DailyMotion**: [dailymotion/javascript](https://github.com/dailymotion/javascript)
  - **Digitpaint** [digitpaint/javascript](https://github.com/digitpaint/javascript)
  - **Ecosia**: [ecosia/javascript](https://github.com/ecosia/javascript)
  - **Evernote**: [evernote/javascript-style-guide](https://github.com/evernote/javascript-style-guide)
  - **ExactTarget**: [ExactTarget/javascript](https://github.com/ExactTarget/javascript)
  - **Expensify** [Expensify/Style-Guide](https://github.com/Expensify/Style-Guide/blob/master/javascript.md)
  - **Flexberry**: [Flexberry/javascript-style-guide](https://github.com/Flexberry/javascript-style-guide)
  - **Gawker Media**: [gawkermedia/javascript](https://github.com/gawkermedia/javascript)
  - **General Electric**: [GeneralElectric/javascript](https://github.com/GeneralElectric/javascript)
  - **GoodData**: [gooddata/gdc-js-style](https://github.com/gooddata/gdc-js-style)
  - **Grooveshark**: [grooveshark/javascript](https://github.com/grooveshark/javascript)
  - **How About We**: [howaboutwe/javascript](https://github.com/howaboutwe/javascript-style-guide)
  - **Huballin**: [huballin/javascript](https://github.com/huballin/javascript)
  - **HubSpot**: [HubSpot/javascript](https://github.com/HubSpot/javascript)
  - **Hyper**: [hyperoslo/javascript-playbook](https://github.com/hyperoslo/javascript-playbook/blob/master/style.md)
  - **InfoJobs**: [InfoJobs/JavaScript-Style-Guide](https://github.com/InfoJobs/JavaScript-Style-Guide)
  - **Intent Media**: [intentmedia/javascript](https://github.com/intentmedia/javascript)
  - **Jam3**: [Jam3/JavaScript-Code-Conventions](https://github.com/Jam3/JavaScript-Code-Conventions)
  - **JSSolutions**: [JSSolutions/javascript](https://github.com/JSSolutions/javascript)
  - **Kinetica Solutions**: [kinetica/javascript](https://github.com/kinetica/JavaScript-style-guide)
  - **Mighty Spring**: [mightyspring/javascript](https://github.com/mightyspring/javascript)
  - **MinnPost**: [MinnPost/javascript](https://github.com/MinnPost/javascript)
  - **MitocGroup**: [MitocGroup/javascript](https://github.com/MitocGroup/javascript)
  - **ModCloth**: [modcloth/javascript](https://github.com/modcloth/javascript)
  - **Money Advice Service**: [moneyadviceservice/javascript](https://github.com/moneyadviceservice/javascript)
  - **Muber**: [muber/javascript](https://github.com/muber/javascript)
  - **National Geographic**: [natgeo/javascript](https://github.com/natgeo/javascript)
  - **National Park Service**: [nationalparkservice/javascript](https://github.com/nationalparkservice/javascript)
  - **Nimbl3**: [nimbl3/javascript](https://github.com/nimbl3/javascript)
  - **Orion Health**: [orionhealth/javascript](https://github.com/orionhealth/javascript)
  - **Peerby**: [Peerby/javascript](https://github.com/Peerby/javascript)
  - **Razorfish**: [razorfish/javascript-style-guide](https://github.com/razorfish/javascript-style-guide)
  - **reddit**: [reddit/styleguide/javascript](https://github.com/reddit/styleguide/tree/master/javascript)
  - **REI**: [reidev/js-style-guide](https://github.com/reidev/js-style-guide)
  - **Ripple**: [ripple/javascript-style-guide](https://github.com/ripple/javascript-style-guide)
  - **SeekingAlpha**: [seekingalpha/javascript-style-guide](https://github.com/seekingalpha/javascript-style-guide)
  - **Shutterfly**: [shutterfly/javascript](https://github.com/shutterfly/javascript)
  - **Springload**: [springload/javascript](https://github.com/springload/javascript)
  - **StudentSphere**: [studentsphere/javascript](https://github.com/studentsphere/guide-javascript)
  - **Target**: [target/javascript](https://github.com/target/javascript)
  - **TheLadders**: [TheLadders/javascript](https://github.com/TheLadders/javascript)
  - **T4R Technology**: [T4R-Technology/javascript](https://github.com/T4R-Technology/javascript)
  - **VoxFeed**: [VoxFeed/javascript-style-guide](https://github.com/VoxFeed/javascript-style-guide)
  - **Weggo**: [Weggo/javascript](https://github.com/Weggo/javascript)
  - **Zillow**: [zillow/javascript](https://github.com/zillow/javascript)
  - **ZocDoc**: [ZocDoc/javascript](https://github.com/ZocDoc/javascript)

**[⬆ back to top](#목차)**

## Translation

  This style guide is also available in other languages:

  - ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Brazilian Portuguese**: [armoucar/javascript-style-guide](https://github.com/armoucar/javascript-style-guide)
  - ![bg](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Bulgaria.png) **Bulgarian**: [borislavvv/javascript](https://github.com/borislavvv/javascript)
  - ![ca](https://raw.githubusercontent.com/fpmweb/javascript-style-guide/master/img/catala.png) **Catalan**: [fpmweb/javascript-style-guide](https://github.com/fpmweb/javascript-style-guide)
  - ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Chinese (Simplified)**: [sivan/javascript-style-guide](https://github.com/sivan/javascript-style-guide)
  - ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Chinese (Traditional)**: [jigsawye/javascript](https://github.com/jigsawye/javascript)
  - ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **French**: [nmussy/javascript-style-guide](https://github.com/nmussy/javascript-style-guide)
  - ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Germany.png) **German**: [timofurrer/javascript-style-guide](https://github.com/timofurrer/javascript-style-guide)
  - ![it](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Italy.png) **Italian**: [sinkswim/javascript-style-guide](https://github.com/sinkswim/javascript-style-guide)
  - ![jp](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [mitsuruog/javascript-style-guide](https://github.com/mitsuruog/javascript-style-guide)
  - ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [tipjs/javascript-style-guide](https://github.com/tipjs/javascript-style-guide)
  - ![pl](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Poland.png) **Polish**: [mjurczyk/javascript](https://github.com/mjurczyk/javascript)
  - ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**: [uprock/javascript](https://github.com/uprock/javascript)
  - ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Spanish**: [paolocarrasco/javascript-style-guide](https://github.com/paolocarrasco/javascript-style-guide)
  - ![th](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Thailand.png) **Thai**: [lvarayut/javascript-style-guide](https://github.com/lvarayut/javascript-style-guide)

## The JavaScript Style Guide Guide

  - [Reference](https://github.com/airbnb/javascript/wiki/The-JavaScript-Style-Guide-Guide)

## Chat With Us About JavaScript

  - Find us on [gitter](https://gitter.im/airbnb/javascript).

## Contributors

  - [View Contributors](https://github.com/airbnb/javascript/graphs/contributors)


## License

(The MIT License)

Copyright (c) 2014 Airbnb

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.


**[⬆ back to top](#목차)**

## Amendments

We encourage you to fork this guide and change the rules to fit your team's style guide. Below, you may list some amendments to the style guide. This allows you to periodically update your style guide without having to deal with merge conflicts.

# };
