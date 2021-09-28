ES5 표준까지해도 자바스크립트는 문제가 많았습니다. 그러나 이 시기에도 완성도 높은 프로그래밍을 위해 여러 방법을 강구했죠. 그 중 하나인 엄격(strict) 모드에 대해 알아봅니다.

## strict mode
> 자바스크립트 언어의 문법을 좀 더 엄격히 적용해 문제를 내포하는 코드에 대해 명시적 에러를 발생시킵니다.

다시 한 번 생각해봅시다. 자바스크립트 언어의 문제점은 뭐가 있었을까요? 대표적으로 꼽자면 var 키워드, 변수 호이스팅, 암묵적 전역(Implict global) 등이 있었죠. 암묵적 전역에 대해 코드를 살펴보고 빠르게 넘어가겠습니다.

- 암묵적 전역 : 아무리 봐도 에러인 아래의 코드는 너무나 잘 작동됩니다.
  ```js
  function foo() {
    x = 10;
  }
  foo();

  console.log(x); // 10
  ```
  - 코드의 문제를 이해하기 위해 자바스크립트 엔진의 동작 순서를 살펴볼까요
    1. foo 함수 내에서 선언하지 않은 x 변수에 값 10을 할당합니다.
    2. x 변수가 어디에서 선언되었는지 스코프 체인을 통해 검색합니다.
    3. foo 스코프에서 탐색하고, 상위 스코프인 전역 스코프에서 x 변수의 선언을 탐색합니다.
    4. 그 어디에도 존재하지 않아 ReferenceError를 발생시켜야하나 엔진은 동적으로 x 프로퍼티를 생성합니다.
    5. 전역 객체의 x 프로퍼티는 전역 변수처럼 사용할 수 있게 됩니다. 이러한 현상이 **암묵적 전역**입니다.

이 외에도 많은 문제로 인해 ES5에서 strict mode(엄격 모드)가 추가 되었으며, ESLint 같은 린트 도구로도 이를 강력하게 검사할 수 있습니다.

<br>

## strict mode 적용하기
> 전역의 선두 또는 함수 몸체 선두에 `'use strict';`를 작성합니다.

- 전역의 선두에 작성 시 전역 스코프에 적용됩니다.
  ```js
  'use strict';

  function foo(){
    x = 10; // ReferenceError: x is not defined
  }
  foo();
  ```

- 함수 몸체의 선두에 작성 시 해당 함수 스코프에서만 적용됩니다.
  ```js
  function foo(){
    'use strict';
    x = 10; // ReferenceError: x is not defined
  }
  foo();
  ```

- 선두가 아닌 곳에 작성 시제대로 동작하지 않습니다.
  ```js
  function foo(){
    x = 10;
    'use strict';
  }
  foo();
  ```

<br>

## 주의사항
### 전역에 적용하는 것은 피하기
> 전역에 적용한 strict mode는 스크립트 단위로 적용됩니다.

- 다른 스크립트에 영향을 주지 않고 해당 스크립트에 한정됩니다.
  ```html
  <!DOCTYPE html>
  <html lang="en">

  <head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
  </head>

  <body>
    <script type="text/javascript">
    'user strict';
    </script>
    <script type="text/javascript">
    x = 1;
    console.log(x);
    </script>
    <script type="text/javascript">
    'user strict';

    y = 1;
    console.log(y); // ReferenceError: y is not defined
    </script>
  </body>

  </html>
  ```
  - 스크립트 별로 적용하는 것은 오류를 발생시킬 수 있습니다. 특히 외부 라이브러리를 사용할 때는요.

<br>

### 함수 단위로 적용하는 것은 피하기
> 어떤 함수는 strict mode, 어떤 함수는 non-strict mode? 당연히 바람직하지 않으며, 그런다고 모든 함수에 일일이 strict mode를 적용하는 것도 옳지 않습니다.

- strict mode가 적용된 함수가 참조할 함수 외부의 컨텍스트에 strict mode를 적용하지 않았다면 문제가 발생할 수 있습니다.
  ```js
  (function(){
    var x = 10;

    function foo(){
      'use strict';

      x = 20;
    }
    foo();
  }());
  ```
  - `에러가 난다고 적혀있는데 정상적으로 작동하네요?🙄 무슨 일이지...`

그럼 strict mode로 코드를 엄격하게 관리해서 일어나는 일은 뭐가 있을까요?

<br>

## strict mode가 발생시키는 에러
> 암묵적 전역, 변수/함수/매개변수의 삭제, 매개변수 이름 중복, with문 사용 등이 대표적입니다.

### 암묵적 전역
> 선언하지 않은 변수를 참조하면 ReferenceError가 발생합니다.

```js
(function(){
  'use strict';
  x = 10;
  console.log(x); // ReferenceError: x is not defined
}());
```

<br>

### 변수/함수/매개변수의 삭제
> delete 연산자로 해당 프로퍼티를 삭제하면 SyntaxError가 발생합니다.

```js
(function(){
  'use strict';
  var x = 10;

  delete x;   // SyntaxError: Delete of an unqualified identifier in strict mode.

  function foo(a) {
    delete a; // SyntaxError: Delete of an unqualified identifier in strict mode.
  }

  delete foo; // SyntaxError: Delete of an unqualified identifier in strict mode.
}());
```

<br>

### 매개변수 이름 중복
> 중복된 이름을 사용하면 SytaxError가 발생합니다.

```js
(function(){
  'use strict';

  function foo(a, a) {
    return a + a; // SyntaxError: Duplicate parameter name not allowed in this context
  }

  console.log(foo(1, 2));
}());
```

<br>

### with문 사용
> 전달된 객체를 스코프 체인에 추가하는 with문을 사용하면 SyntaxError가 발생합니다.

```js
(function(){
  'use strict';

  with({ x: 1 }){
    console.log(x); // SyntaxError: Strict mode code may not include a with statement
  }
}());
```

마지막입니다! strict mode를 적용하면 변하는 동작이 존재할까요? 그 답은 `있습니다`이고, 이제 확인해 볼 거에요.

<br>

## strict mode 적용에 의한 변화
> 일반 함수의 this와 arguments 객체의 동작이 변경됩니다.

### 일반 함수의 this
> 일반 함수로 호출하면 this에 undefined가 바인딩됩니다. 생성자 함수가 아닌 일반 함수 내부에서는 this를 사용할 필요가 없기 때문이죠.

```js
(function(){
  'use strict';

  function foo(){
    console.log(this);  // undefined
  }
  foo();

  function Foo(){
    console.log(this);  // Foo
  }
  new Foo();
}());
```

<br>

### arguments 객체
> 매개변수에 전달된 인수를 재할당하여 변경해도 반영되지 않습니다.

```js
(function(x){
  'use strict';

  x = 10;
  console.log(arguments); // Arguments [callee: (...), Symbol(Symbol.iterator): ƒ]
}());
```

<hr>
<br>