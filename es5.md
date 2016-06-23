[원문:https://github.com/airbnb/javascript](https://github.com/airbnb/javascript)

# Airbnb JavaScript 스타일 가이드() {


## <a name='TOC'><a name='TOC'>목차</a>

  1. [Types](#types)
  1. [Objects](#objects)
  1. [Arrays](#arrays)
  1. [Strings](#strings)
  1. [Functions](#functions)
  1. [Properties](#properties)
  1. [Variables](#variables)
  1. [Hoisting](#hoisting)
  1. [Conditional Expressions & Equality](#conditionals)
  1. [Blocks](#blocks)
  1. [Comments](#comments)
  1. [Whitespace](#whitespace)
  1. [Commas](#commas)
  1. [Semicolons](#semicolons)
  1. [Type Casting & Coercion](#type-coercion)
  1. [Naming Conventions](#naming-conventions)
  1. [Accessors](#accessors)
  1. [Constructors](#constructors)
  1. [Events](#events)
  1. [Modules](#modules)
  1. [jQuery](#jquery)
  1. [ES5 Compatibility](#es5)
  1. [Testing](#testing)
  1. [Performance](#performance)
  1. [Resources](#resources)
  1. [In the Wild](#in-the-wild)
  1. [Translation](#translation)
  1. [The JavaScript Style Guide Guide](#guide-guide)
  1. [Contributors](#contributors)
  1. [License](#license)

## <a name='types'>Types</a> [원문](https://github.com/airbnb/javascript#types)

  - **Primitives**: primitive type은 그 값을 직접 조작합니다.

    + `string`
    + `number`
    + `boolean`
    + `null`
    + `undefined`

    ```javascript
    var foo = 1,
        bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```
  - **Complex**: 참조형(Complex)은 참조를 통해 값을 조작합니다.

    + `object`
    + `array`
    + `function`

    ```javascript
    var foo = [1, 2],
        bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

    **[[↑]](#TOC)**

## <a name='objects'>Objects</a> [원문](https://github.com/airbnb/javascript#objects)

  - Object를 만들 때는 리터럴 구문을 사용하십시오.

    ```javascript
    // bad
    var item = new Object();

    // good
    var item = {};
    ```

  - [예약어(reserved words)](http://es5.github.io/#x7.6.1)를 키로 사용하지 마십시오. 이것은 IE8에서 동작하지 않습니다. [More info](https://github.com/airbnb/javascript/issues/61)

    ```javascript
    // bad
    var superman = {
      default: { clark: 'kent' },
      private: true
    };

    // good
    var superman = {
      defaults: { clark: 'kent' },
      hidden: true
    };
    ```

  - 예약어 대신 알기 쉬운 동의어(readable synonyms)를 사용하십시오.

    ```javascript
    // bad
    var superman = {
      class: 'alien'
    };

    // bad
    var superman = {
      klass: 'alien'
    };

    // good
    var superman = {
      type: 'alien'
    };

    ```
    **[[↑]](#TOC)**

## <a name='arrays'>Arrays</a> [원문](https://github.com/airbnb/javascript#arrays)

  - 배열을 만들 때 리터럴 구문을 사용하십시오.

    ```javascript
    // bad
    var items = new Array();

    // good
    var items = [];
    ```

  - 길이를 알 수없는 경우는 Array#push를 사용하십시오.
  
    ```javascript
    var someStack = [];


    // bad
    someStack[someStack.length] = 'abracadabra';

    // good
    someStack.push('abracadabra');
    ```

  - 배열을 복사 할 필요가있는 경우 Array#slice를 사용하십시오. [jsPerf](http://jsperf.com/converting-arguments-to-an-array/7)

    ```javascript
    var len = items.length,
        itemsCopy = [],
        i;

    // bad
    for (i = 0; i < len; i++) {
      itemsCopy[i] = items[i];
    }

    // good
    itemsCopy = items.slice();
    ```

  - Array와 비슷한(Array-Like)한 Object를 Array에 변환하는 경우는 Array#slice를 사용하십시오. 

    ```javascript
    function trigger() {
      var args = Array.prototype.slice.call(arguments);
      ...
    }
    ```

**[[↑]](#TOC)**


## <a name='strings'>Strings</a> [원문](https://github.com/airbnb/javascript#strings)

  - 문자열은 작은 따옴표`''`를 사용하십시오.

    ```javascript
    // bad
    var name = "Bob Parr";

    // good
    var name = 'Bob Parr';

    // bad
    var fullName = "Bob " + this.lastName;

    // good
    var fullName = 'Bob ' + this.lastName;
    ```

  - 80 문자 이상의 문자열은 문자열 연결을 사용하여 여러 줄에 걸쳐 기술 할 필요가 있습니다.
  - Note : 문자열 연결을 많이하면 성능에 영향을 줄 수 있습니다. [jsPerf](http://jsperf.com/ya-string-concat) & [Discussion](https://github.com/airbnb/javascript/issues/40)

    ```javascript
    // bad
    var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

    // bad
    var errorMessage = 'This is a super long error that \
    was thrown because of Batman. \
    When you stop to think about \
    how Batman had anything to do \
    with this, you would get nowhere \
    fast.';


    // good
    var errorMessage = 'This is a super long error that ' +
      'was thrown because of Batman.' +
      'When you stop to think about ' +
      'how Batman had anything to do ' +
      'with this, you would get nowhere ' +
      'fast.';
    ```

  - 프로그램에서 문자열을 생성 할 필요가 있는 경우 (특히 IE는) 문자열 연결 대신 Array#join을 사용하십시오. [jsPerf](http://jsperf.com/string-vs-array-concat/2).

    ```javascript
    var items,
        messages,
        length,
        i;

    messages = [{
        state: 'success',
        message: 'This one worked.'
    },{
        state: 'success',
        message: 'This one worked as well.'
    },{
        state: 'error',
        message: 'This one did not work.'
    }];

    length = messages.length;

    // bad
    function inbox(messages) {
      items = '<ul>';

      for (i = 0; i < length; i++) {
        items += '<li>' + messages[i].message + '</li>';
      }

      return items + '</ul>';
    }

    // good
    function inbox(messages) {
      items = [];

      for (i = 0; i < length; i++) {
        items[i] = messages[i].message;
      }

      return '<ul><li>' + items.join('</li><li>') + '</li></ul>';
    }
    ```

    **[[↑]](#TOC)**


## <a name='functions'>Functions</a> [원문](https://github.com/airbnb/javascript#functions)

  - 함수식(Function expressions)

    ```javascript
    // 익명함수식(anonymous function expression)
    var anonymous = function() {
      return true;
    };

    // 명명된 함수식(named function expression)
    var named = function named() {
      return true;
    };

    // 즉시실행 함수식(immediately-invoked function expression (IIFE))
    (function() {
      console.log('Welcome to the Internet. Please follow me.');
    })();
    ```

  - (if 및 while 등) 블록 내에서 변수에 함수를 할당하는 대신 함수를 선언하지 마십시오. 브라우저는 허용하지만 (마치 'bad news bears'처럼) 모두 다른 방식으로 해석됩니다.
  - **Note:** ECMA-262에서는`block`은 statements의 목록에 정의되어 있습니다 만, 함수 선언은 statements가 없습니다. [이 문제는 ECMA-262의 설명을 참조하십시오. ](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97).

    ```javascript
    // bad
    if (currentUser) {
      function test() {
        console.log('Nope.');
      }
    }

    // good
    var test;
    if (currentUser) {
      test = function test() {
        console.log('Yup.');
      };
    }
    ```

  - 매개 변수(parameter)에 `arguments`를 절대 지정하지 마십시오. 이것은 함수 범위로 전달 될`arguments`객체의 참조를 덮어 쓸 것입니다.

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

    **[[↑]](#TOC)**



## <a name='properties'>Properties</a> [원문](https://github.com/airbnb/javascript#properties)

  - 속성에 액세스하려면 도트`.`를 사용하십시오.

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    // bad
    var isJedi = luke['jedi'];

    // good
    var isJedi = luke.jedi;
    ```

  - 변수를 사용하여 속성에 접근하려면 대괄호`[]`을 사용하십시오.

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    function getProp(prop) {
      return luke[prop];
    }

    var isJedi = getProp('jedi');
    ```

    **[[↑]](#TOC)**


## <a name='variables'>Variables</a> [원문](https://github.com/airbnb/javascript#variables)

  - 변수를 선언 할 때는 항상 `var`를 사용하십시오. 그렇지 않으면 전역 변수로 선언됩니다. 전역 네임 스페이스를 오염시키지 않도록 Captain Planet도 경고하고 있습니다.

    ```javascript
    // bad
    superPower = new SuperPower();

    // good
    var superPower = new SuperPower();
    ```

  - 여러 변수를 선언하려면 하나의 `var`를 사용하여 변수마다 줄바꿈하여 선언하십시오.

    ```javascript
    // bad
    var items = getItems();
    var goSportsTeam = true;
    var dragonball = 'z';

    // good
    var items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';
    ```

  - 정의되지 않은 변수를 마지막으로 선언하십시오. 이것은 나중에 이미 할당된 변수 중 하나를 지정해야하는 경우에 유용합니다.

    ```javascript
    // bad
    var i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // bad
    var i, items = getItems(),
        dragonball,
        goSportsTeam = true,
        len;

    // good
    var items = getItems(),
        goSportsTeam = true,
        dragonball,
        length,
        i;
    ```

  - 변수의 할당은 스코프의 시작 부분에서 해주십시오. 이것은 변수 선언과 Hoisting 관련 문제를 해결합니다.

    ```javascript
    // bad
    function() {
      test();
      console.log('doing stuff..');

      //..other stuff..

      var name = getName();

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // good
    function() {
      var name = getName();

      test();
      console.log('doing stuff..');

      //..other stuff..

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // bad
    function() {
      var name = getName();

      if (!arguments.length) {
        return false;
      }

      return true;
    }

    // good
    function() {
      if (!arguments.length) {
        return false;
      }

      var name = getName();

      return true;
    }
    ```

    **[[↑]](#TOC)**


## <a name='hoisting'>Hoisting</a> [원문](https://github.com/airbnb/javascript#hoisting)

  - 해당 스코프의 시작 부분에 Hoist된 변수선언은 할당되지 않습니다. 

    ```javascript
    // (notDefined가 전역 변수에 존재하지 않는다고 가정했을 경우)
    // 이것은 동작하지 않습니다.
    function example() {
      console.log(notDefined); // => throws a ReferenceError
    }

    // 그 변수를 참조하는 코드 후에 그 변수를 선언 한 경우
    // 변수가 Hoist된 상태에서 작동합니다.
    // Note : `true`라는 값 자체는 Hoist되지 않습니다.
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

    // 인터 프린터는 변수 선언을 스코프의 시작 부분에 Hoist합니다.
    // 위의 예는 다음과 같이 다시 작성할 수 있습니다.
    function example() {
      var declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }
    ```

  - 익명 함수의 경우 함수가 할당되기 전에 변수가 Hoist될 수 있습니다.

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function() {
        console.log('anonymous function expression');
      };
    }
    ```

  - 명명 된 함수의 경우도 마찬가지로 변수가 Hoist될 수 있습니다. 함수 이름과 함수 본체는 Hoist되지 않습니다.

    ```javascript
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      superPower(); // => ReferenceError superPower is not defined

      var named = function superPower() {
        console.log('Flying');
      };
    }

    // 함수이름과 변수이름이 같은 경우에도 같은 일이 일어납니다.
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      var named = function named() {
        console.log('named');
      }
    }
    ```

  - 함수 선언은 함수이름과 함수본문이 Hoist됩니다.

    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }
    ```

  - 더 자세한 정보는 [Ben Cherry](http://www.adequatelygood.com/)의 [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting)를 참조하십시오.

    **[[↑]](#TOC)**



## <a name='conditionals'>Conditional Expressions & Equality(조건식과 등가식)</a> [원문](https://github.com/airbnb/javascript#conditionals)

  - `==` 나 `!=` 보다는 `===` 와 `!==` 를 사용해 주십시오
  - 조건식은`ToBoolean` 메소드에 의해 엄밀하게 비교됩니다. 항상 이 간단한 규칙에 따라 주십시오.

    + **Objects** 는 **true** 로 평가됩니다.
    + **undefined** 는 **false** 로 평가됩니다.
    + **null** 는 **false** 로 평가됩니다.
    + **Booleans** 는 **boolean형의 값** 으로 평가됩니다.
    + **Numbers** 는 **true** 로 평가됩니다. 하지만 **+0, -0, or NaN** 의 경우는 **false** 입니다.
    + **Strings** 는 **true** 로 평가됩니다. 하지만 빈문자 `''` 의 경우는 **false** 입니다.


    ```javascript
    if ([0]) {
      // true
      // Array는 Object 이므로 true 로 평가됩니다.
    }
    ```

  - 짧은형식을 사용하십시오.

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

  - 더 자세한 정보는 Angus Croll 의 [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108)를 참고해 주십시오.

    **[[↑]](#TOC)**


## <a name='blocks'>Blocks</a> [원문](https://github.com/airbnb/javascript#blocks)

  - 복수행 블록은 중괄호 ({})를 사용하십시오.

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

    **[[↑]](#TOC)**


## <a name='comments'>Comments</a> [원문](https://github.com/airbnb/javascript#comments)

  - 복수행의 코멘트는 `/** ... */` 를 사용해 주십시오. 그 안에는 설명과 모든 매개 변수와 반환 값에 대한 형식과 값을 설명합니다.

    ```javascript
    // bad
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param <String> tag
    // @return <Element> element
    function make(tag) {

      // ...stuff...

      return element;
    }

    // good
    /**
     * make() returns a new element
     * based on the passed in tag name
     *
     * @param <String> tag
     * @return <Element> element
     */
    function make(tag) {

      // ...stuff...

      return element;
    }
    ```

  - 한 줄 주석에는`//`를 사용하십시오. 코멘트를 추가하고 싶은 코드의 상단에 작성하십시오. 또한 주석 앞에 빈 줄을 넣어주십시오.

    ```javascript
    // bad
    var active = true;  // is current tab

    // good
    // is current tab
    var active = true;

    // bad
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      var type = this._type || 'no type';

      return type;
    }

    // good
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      var type = this._type || 'no type';

      return type;
    }
    ```

  - 문제를 지적하고 재고를 촉구하거나 문제에 대한 해결책을 제시하는 등 의견의 앞에 `FIXME` 나 `TODO`를 붙이는 것으로 다른 개발자의 빠른 이해를 도울 수 있습니다. 이러한 어떤 액션을 동반한다는 의미에서 일반 코멘트와는 다릅니다. 액션은 `FIXME - 해결책이 필요` 또는 `TODO - 구현이 필요` 입니다.

  - 문제에 대한 코멘트로 `// FIXME :`를 사용하십시오.

    ```javascript
    function Calculator() {

      // FIXME: 전역 변수를 사용해서는 안됩니다.
      total = 0;

      return this;
    }
    ```

  - 문제 해결책에 대한 코멘트로 `// TODO :`를 사용하십시오.

    ```javascript
    function Calculator() {

      // TODO: total은 옵션 매개 변수로 설정되어야 함.
      this.total = 0;
      return this;
    }
  ```

    **[[↑]](#TOC)**


## <a name='whitespace'>Whitespace</a> [원문](https://github.com/airbnb/javascript#whitespace)

  - 탭에는 공백 2개를 설정하십시오.

    ```javascript
    // bad
    function() {
    ∙∙∙∙var name;
    }

    // bad
    function() {
    ∙var name;
    }

    // good
    function() {
    ∙∙var name;
    }
    ```
  - 중괄호({})의 앞에 공백을 하나 넣어주십시오.

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
      breed: 'Bernese Mountain Dog'
    });

    // good
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });
    ```
  - 파일의 마지막에는 빈 줄을 하나 넣어주십시오.

    ```javascript
    // bad
    (function(global) {
      // ...stuff...
    })(this);
    ```

    ```javascript
    // good
    (function(global) {
      // ...stuff...
    })(this);

    ```

  - 메소드 체인이 길어지는 경우 적절히 들여쓰기(indentation) 하십시오.

    ```javascript
    // bad
    $('#items').find('.selected').highlight().end().find('.open').updateCount();

    // good
    $('#items')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount();

    // bad
    var leds = stage.selectAll('.led').data(data).enter().append('svg:svg').class('led', true)
        .attr('width',  (radius + margin) * 2).append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);

    // good
    var leds = stage.selectAll('.led')
        .data(data)
      .enter().append('svg:svg')
        .class('led', true)
        .attr('width',  (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);
    ```

    **[[↑]](#TOC)**

## <a name='commas'>Commas</a> [원문](https://github.com/airbnb/javascript#commas)

  - 선두의 comma는 **하지마십시오.**

    ```javascript
    // bad
    var once
      , upon
      , aTime;

    // good
    var once,
        upon,
        aTime;

    // bad
    var hero = {
        firstName: 'Bob'
      , lastName: 'Parr'
      , heroName: 'Mr. Incredible'
      , superPower: 'strength'
    };

    // good
    var hero = {
      firstName: 'Bob',
      lastName: 'Parr',
      heroName: 'Mr. Incredible',
      superPower: 'strength'
    };
    ```

　- 말미의 불필요한 쉼표도 **하지 마십시오.** 이것은 IE6/7과 quirksmode의 IE9에서 문제를 일으킬 수 있습니다.
  또한 ES3의 일부 구현에서 불필요한 쉼표가 있는 경우, 배열 길이를 추가합니다.
  이것은 ES5에서 분명해졌습니다.([source](http://es5.github.io/#D)):

  > 제 5 판에서는 말미의 불필요한 쉼표가 있는 ArrayInitialiser (배열 초기화 연산자)라도 배열에 길이를 추가하지 않는다는 사실을 명확히하고 있습니다. 이것은 제 3 판에서 의미적인 변경은 아닙니다만, 일부의 구현은 이전부터 이것을 오해하고 있었을지도 모릅니다.

    ```javascript
    // bad
    var hero = {
      firstName: 'Kevin',
      lastName: 'Flynn',
    };

    var heroes = [
      'Batman',
      'Superman',
    ];

    // good
    var hero = {
      firstName: 'Kevin',
      lastName: 'Flynn'
    };

    var heroes = [
      'Batman',
      'Superman'
    ];
    ```

    **[[↑]](#TOC)**


## <a name='semicolons'>Semicolons</a> [원문](https://github.com/airbnb/javascript#semicolons)

  - **네!(Yup.)**

    ```javascript
    // bad
    (function() {
      var name = 'Skywalker'
      return name
    })()

    // good
    (function() {
      var name = 'Skywalker';
      return name;
    })();

    // good
    ;(function() {
      var name = 'Skywalker';
      return name;
    })();
    ```

    **[[↑]](#TOC)**


## <a name='type-coercion'>Type Casting & Coercion(강제)</a> [원문](https://github.com/airbnb/javascript#type-coercion)

  - 문의 시작 부분에서 형을 강제합니다.
  - Strings:

    ```javascript
    //  => this.reviewScore = 9;

    // bad
    var totalScore = this.reviewScore + '';

    // good
    var totalScore = '' + this.reviewScore;

    // bad
    var totalScore = '' + this.reviewScore + ' total score';

    // good
    var totalScore = this.reviewScore + ' total score';
    ```

  - 숫자는`parseInt`를 사용하십시오. 항상 형변환을 위한 기수(radix)를 인수로 전달하십시오.

    ```javascript
    var inputValue = '4';

    // bad
    var val = new Number(inputValue);

    // bad
    var val = +inputValue;

    // bad
    var val = inputValue >> 0;

    // bad
    var val = parseInt(inputValue);

    // good
    var val = Number(inputValue);

    // good
    var val = parseInt(inputValue, 10);
    ```

  - 어떤 이유에 의해 `parseInt` 가 병목이 되고, [성능적인 이유](http://jsperf.com/coercion-vs-casting/3)로 Bitshift를 사용할 필요가 있을 경우,
  하려고 하는것에 대해, why(왜)와 what(무엇)의 설명을 코멘트로 남겨주십시오.

    ```javascript
    // good
    /**
     * parseInt가 병목을 일으키므로
     * Bitshift로 문자열을 수치로 강제적으로 변환하는 방법으로
     * 성능을 개선시킵니다.
     */
    var val = inputValue >> 0;
    ```

  - Booleans:

    ```javascript
    var age = 0;

    // bad
    var hasAge = new Boolean(age);

    // good
    var hasAge = Boolean(age);

    // good
    var hasAge = !!age;
    ```

    **[[↑]](#TOC)**


## <a name='naming-conventions'>Naming Conventions</a> [원문](https://github.com/airbnb/javascript#naming-conventions)

  - 한문자 이름은 피하십시오. 이름에서 의도를 읽을 수 있도록 하십시오.

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

  - Object, 함수, 그리고 인스턴스로는 camelCase를 사용하십시오.

    ```javascript
    // bad
    var OBJEcttsssss = {};
    var this_is_my_object = {};
    var this-is-my-object = {};
    function c() {};
    var u = new user({
      name: 'Bob Parr'
    });

    // good
    var thisIsMyObject = {};
    function thisIsMyFunction() {};
    var user = new User({
      name: 'Bob Parr'
    });
    ```

  - Class와 생성자에는 PascalCase를 사용하십시오.

    ```javascript
    // bad
    function user(options) {
      this.name = options.name;
    }

    var bad = new user({
      name: 'nope'
    });

    // good
    function User(options) {
      this.name = options.name;
    }

    var good = new User({
      name: 'yup'
    });
    ```

  - private 속성 이름은 밑줄 `_` 을 사용하십시오.

    ```javascript
    // bad
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';

    // good
    this._firstName = 'Panda';
    ```

  - `this`의 참조를 저장할 때 `_this` 를 사용하십시오.

    ```javascript
    // bad
    function() {
      var self = this;
      return function() {
        console.log(self);
      };
    }

    // bad
    function() {
      var that = this;
      return function() {
        console.log(that);
      };
    }

    // good
    function() {
      var _this = this;
      return function() {
        console.log(_this);
      };
    }
    ```

  - 함수에 이름을 붙여주십시오. 이것은 stack traces를 추적하기 쉽게하기 때문입니다.

    ```javascript
    // bad
    var log = function(msg) {
      console.log(msg);
    };

    // good
    var log = function log(msg) {
      console.log(msg);
    };
    ```

    **[[↑]](#TOC)**


## <a name='accessors'>Accessors</a> [원문](https://github.com/airbnb/javascript#accessors)

  - 속성을 위한 접근자(Accessor) 함수는 필요 없습니다.
  - 접근자 함수가 필요한 경우 `getVal()` 이나 `setVal('hello')` 라고 사용합니다.

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

  - 속성이 boolean의 경우 `isVal()` 이나 `hasVal()` 라고 사용합니다.
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

  - 일관된다면 `get()` 이나 `set()` 이라는 함수를 작성해도 좋습니다.

    ```javascript
    function Jedi(options) {
      options || (options = {});
      var lightsaber = options.lightsaber || 'blue';
      this.set('lightsaber', lightsaber);
    }

    Jedi.prototype.set = function(key, val) {
      this[key] = val;
    };

    Jedi.prototype.get = function(key) {
      return this[key];
    };
    ```

    **[[↑]](#TOC)**


## <a name='constructors'>Constructors</a> [원문](https://github.com/airbnb/javascript#constructors)

  - 새 Object에서 프로토타입을 재정의하는 것이 아니라, 프로토타입 객체에 메서드를 추가해 주십시오. 프로토타입을 재정의하면 상속이 불가능합니다. 프로토타입을 리셋하는것으로 베이스 클래스를 재정의 할 수 있습니다.

    ```javascript
    function Jedi() {
      console.log('new jedi');
    }

    // bad
    Jedi.prototype = {
      fight: function fight() {
        console.log('fighting');
      },

      block: function block() {
        console.log('blocking');
      }
    };

    // good
    Jedi.prototype.fight = function fight() {
      console.log('fighting');
    };

    Jedi.prototype.block = function block() {
      console.log('blocking');
    };
    ```

  - 메소드의 반환 값으로 `this`를 반환함으로써 메소드 체인을 할 수 있습니다.

    ```javascript
    // bad
    Jedi.prototype.jump = function() {
      this.jumping = true;
      return true;
    };

    Jedi.prototype.setHeight = function(height) {
      this.height = height;
    };

    var luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20) // => undefined

    // good
    Jedi.prototype.jump = function() {
      this.jumping = true;
      return this;
    };

    Jedi.prototype.setHeight = function(height) {
      this.height = height;
      return this;
    };

    var luke = new Jedi();

    luke.jump()
      .setHeight(20);
    ```


  - 독자적인 toString()을 만들 수도 있지만 올바르게 작동하는지, 부작용이 없는 것만은 확인해 주십시오.

    ```javascript
    function Jedi(options) {
      options || (options = {});
      this.name = options.name || 'no name';
    }

    Jedi.prototype.getName = function getName() {
      return this.name;
    };

    Jedi.prototype.toString = function toString() {
      return 'Jedi - ' + this.getName();
    };
    ```

    **[[↑]](#TOC)**

## <a name='events'>Events</a>

  - (DOM 이벤트나 Backbone events와 같은 고유의) 이벤트 탑재체(payloads)의 값을 전달하는 경우 원시 값(raw value) 대신 해시 인수(hash)를 전달합니다.
이렇게하는 것으로 나중에 개발자가 이벤트와 관련된 모든 핸들러를 찾아 업데이트 하지 않고 이벤트 탑재체(payloads)에 값을 추가 할 수 있습니다. 예를 들어, 이것 대신 :

    ```js
    // bad
    $(this).trigger('listingUpdated', listing.id);

    ...

    $(this).on('listingUpdated', function(e, listingId) {
      // do something with listingId
    });
    ```

    이쪽을 선호합니다.:

    ```js
    // good
    $(this).trigger('listingUpdated', { listingId : listing.id });

    ...

    $(this).on('listingUpdated', function(e, data) {
      // do something with data.listingId
    });
    ```

  **[[↑]](#TOC)**

## <a name='modules'>Modules</a> [원문](https://github.com/airbnb/javascript#modules)

  - 모듈의 시작은 `!` 로 시작하십시오. 이것은 문말에 세미콜론을 넣는것을 잊은 모듈을 연결할때 런타임 오류가 발생하지 않기 때문입니다.
  - 파일 이름은 camelCase를 사용하여 같은 이름의 폴더에 저장해주십시오. 또한 단독으로 공개할 경우 이름을 일치시켜주십시오.
  - noConflict() 라는 명칭으로 (이름이 겹쳐 덮어 써지기 전의) 모듈을 반환하는 메서드를 추가해주십시오.
  - 항상 모듈의 시작 부분에서` 'use strict';`를 선언해주십시오.

    ```javascript
    // fancyInput/fancyInput.js

    !function(global) {
      'use strict';

      var previousFancyInput = global.FancyInput;

      function FancyInput(options) {
        this.options = options || {};
      }

      FancyInput.noConflict = function noConflict() {
        global.FancyInput = previousFancyInput;
        return FancyInput;
      };

      global.FancyInput = FancyInput;
    }(this);
    ```

    **[[↑]](#TOC)**


## <a name='jquery'>jQuery</a> [원문](https://github.com/airbnb/javascript#jquery)

  - jQuery Object의 변수 앞에는 `$`을 부여해 주십시오.

    ```javascript
    // bad
    var sidebar = $('.sidebar');

    // good
    var $sidebar = $('.sidebar');
    ```

  - jQuery 쿼리결과를 캐시해주십시오.

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
      var $sidebar = $('.sidebar');
      $sidebar.hide();

      // ...stuff...

      $sidebar.css({
        'background-color': 'pink'
      });
    }
    ```

  - DOM 검색은 Cascading `$('.sidebar ul')` 이나 parent > child `$('.sidebar > ul')` 를 사용해주십시오. [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)
  - jQuery Object 검색은 스코프가 붙은 `find`를 사용해주십시오.

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
    $sidebar.find('ul');
    ```

    **[[↑]](#TOC)**


## <a name='es5'>ECMAScript 5 Compatibility</a> [원문](https://github.com/airbnb/javascript#es5)

  - [Kangax](https://twitter.com/kangax/)의 ES5 [compatibility table](http://kangax.github.com/es5-compat-table/)를 참조해 주십시오.

  **[[↑]](#TOC)**


## <a name='testing'>Testing</a> [원문](https://github.com/airbnb/javascript#testing)

  - **네!(Yup.)**

    ```javascript
    function() {
      return true;
    }
    ```

    **[[↑]](#TOC)**


## <a name='performance'>Performance</a> [원문](https://github.com/airbnb/javascript#performance)

  - [On Layout & Web Performance](http://kellegous.com/j/2013/01/26/layout-performance/)
  - [String vs Array Concat](http://jsperf.com/string-vs-array-concat/2)
  - [Try/Catch Cost In a Loop](http://jsperf.com/try-catch-in-loop-cost)
  - [Bang Function](http://jsperf.com/bang-function)
  - [jQuery Find vs Context, Selector](http://jsperf.com/jquery-find-vs-context-sel/13)
  - [innerHTML vs textContent for script text](http://jsperf.com/innerhtml-vs-textcontent-for-script-text)
  - [Long String Concatenation](http://jsperf.com/ya-string-concat)
  - Loading...

  **[[↑]](#TOC)**


## <a name='resources'>Resources</a> [원문](https://github.com/airbnb/javascript#resources)


**Read This**

  - [Annotated ECMAScript 5.1](http://es5.github.com/)

**Other Styleguides**

  - [Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
  - [jQuery Core Style Guidelines](http://docs.jquery.com/JQuery_Core_Style_Guidelines)
  - [Principles of Writing Consistent, Idiomatic JavaScript](https://github.com/rwldrn/idiomatic.js/)

**Other Styles**

  - [Naming this in nested functions](https://gist.github.com/4135065) - Christian Johansen
  - [Conditional Callbacks](https://github.com/airbnb/javascript/issues/52)
  - [Popular JavaScript Coding Conventions on Github](http://sideeffect.kr/popularconvention/#javascript)

**Further Reading**

  - [Understanding JavaScript Closures](http://javascriptweblog.wordpress.com/2010/10/25/understanding-javascript-closures/) - Angus Croll
  - [Basic JavaScript for the impatient programmer](http://www.2ality.com/2013/06/basic-javascript.html) - Dr. Axel Rauschmayer

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
  - [JSBooks](http://jsbooks.revolunet.com/)

**Blogs**

  - [DailyJS](http://dailyjs.com/)
  - [JavaScript Weekly](http://javascriptweekly.com/)
  - [JavaScript, JavaScript...](http://javascriptweblog.wordpress.com/)
  - [Bocoup Weblog](http://weblog.bocoup.com/)
  - [Adequately Good](http://www.adequatelygood.com/)
  - [NCZOnline](http://www.nczonline.net/)
  - [Perfection Kills](http://perfectionkills.com/)
  - [Ben Alman](http://benalman.com/)
  - [Dmitry Baranovskiy](http://dmitry.baranovskiy.com/)
  - [Dustin Diaz](http://dustindiaz.com/)
  - [nettuts](http://net.tutsplus.com/?s=javascript)

  **[[↑]](#TOC)**

## <a name='in-the-wild'>In the Wild</a>

  This is a list of organizations that are using this style guide. Send us a pull request or open an issue and we'll add you to the list.

  - **Aan Zee**: [AanZee/javascript](https://github.com/AanZee/javascript)
  - **Airbnb**: [airbnb/javascript](https://github.com/airbnb/javascript)
  - **American Insitutes for Research**: [AIRAST/javascript](https://github.com/AIRAST/javascript)
  - **Compass Learning**: [compasslearning/javascript-style-guide](https://github.com/compasslearning/javascript-style-guide)
  - **ExactTarget**: [ExactTarget/javascript](https://github.com/ExactTarget/javascript)
  - **GeneralElectric**: [GeneralElectric/javascript](https://github.com/GeneralElectric/javascript)
  - **GoodData**: [gooddata/gdc-js-style](https://github.com/gooddata/gdc-js-style)
  - **Grooveshark**: [grooveshark/javascript](https://github.com/grooveshark/javascript)
  - **How About We**: [howaboutwe/javascript](https://github.com/howaboutwe/javascript)
  - **Mighty Spring**: [mightyspring/javascript](https://github.com/mightyspring/javascript)
  - **MinnPost**: [MinnPost/javascript](https://github.com/MinnPost/javascript)
  - **ModCloth**: [modcloth/javascript](https://github.com/modcloth/javascript)
  - **National Geographic**: [natgeo/javascript](https://github.com/natgeo/javascript)
  - **National Park Service**: [nationalparkservice/javascript](https://github.com/nationalparkservice/javascript)
  - **Razorfish**: [razorfish/javascript-style-guide](https://github.com/razorfish/javascript-style-guide)
  - **Shutterfly**: [shutterfly/javascript](https://github.com/shutterfly/javascript)
  - **Userify**: [userify/javascript](https://github.com/userify/javascript)
  - **Zillow**: [zillow/javascript](https://github.com/zillow/javascript)
  - **ZocDoc**: [ZocDoc/javascript](https://github.com/ZocDoc/javascript)

## <a name='translation'>Translation</a>

  This style guide is also available in other languages:

  - :de: **German**: [timofurrer/javascript-style-guide](https://github.com/timofurrer/javascript-style-guide)
  - :jp: **Japanese**: [mitsuruog/javacript-style-guide](https://github.com/mitsuruog/javacript-style-guide)
  - :br: **Portuguese**: [armoucar/javascript-style-guide](https://github.com/armoucar/javascript-style-guide)
  - :cn: **Chinese**: [adamlu/javascript-style-guide](https://github.com/adamlu/javascript-style-guide)
  - :es: **Spanish**: [paolocarrasco/javascript-style-guide](https://github.com/paolocarrasco/javascript-style-guide)
  - :kr: **Korean**: [tipjs/javascript-style-guide](https://github.com/tipjs/javascript-style-guide)

## <a name='guide-guide'>The JavaScript Style Guide Guide</a>

  - [Reference](https://github.com/airbnb/javascript/wiki/The-JavaScript-Style-Guide-Guide)

## <a name='authors'>Contributors</a>

  - [View Contributors](https://github.com/airbnb/javascript/graphs/contributors)


## <a name='license'>License</a>

(The MIT License)

Copyright (c) 2012 Airbnb

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

**[[↑]](#TOC)**

# };

