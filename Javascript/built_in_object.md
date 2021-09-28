자바스크립트의 객체는 안타깝게도 여러 개로 분류됩니다. 이번 장에서는 표준 빌트인 객체, 호스트 객체, 사용자 정의 객체는 알아 볼 겁니다.

## 자바스크립트 객체의 분류
> 언급했듯, 크게 3가지로 구분합니다.

- 표준 빌트인 객체(Standard Built-in Objects, Native Objects, Global Objects)
  - ECMAScript 사양에 정의된 객체로 애플리케이션 전역에 공통 기능을 제공합니다.
  - 자바스크립트 실행 환경(브라우저, Node.js)과 관계없이 언제나 사용할 수 있습니다.
  - 전역 객체의 프로퍼티로 제공됩니다.
  - 별도의 선언 없이 전역 변수처럼 언제나 참조할 수 있습니다.

<br>

- 호스트 객체(Host Objects)
  - ECMAScript 사양에 정의되지 않은, 자바스크립트 실행 환경(브라우저, Node.js)에 추가로 제공되는 객체입니다.

<br>

- 사용자 정의 객체(User-Defined Objects)
  - 사용자가 직접 정의한 객체입니다.

<br>

## 표준 빌트인 객체
> 40여 개의 표준 빌트인 객체를 제공합니다.

Math, Reflect, JSON을 제외한 표준 빌트인 객체는 모두 생성자 함수입니다.
- 생성자 함수 객체인 경우 : 프로토타입 메서드, 정적 메서드 둘 다 제공합니다.
- 생성자 함수 객체가 아닌 경우 : 정적 메서드만 제공합니다.

표준 빌트인 객체인 String을 생성자 함수로서 호출하여 생성한 String 인스턴스의 프로토타입은 String.prototype 이듯, 생성자 함수로 생성된 인스턴스의 프로토타입은 표준 빌트인 객체의 prototype 프로퍼티에 바인딩됩니다. 여기까지는 다른 생성자 함수와 같아 보이죠? 그러나 표준 빌트인 객체는 `인스턴스 없이도 호출 가능`한 빌트인 정적 메서드를 제공합니다.

그러나 언급하는 표준 빌트인 객체는 어디선가 많이 본 단어입니다. 바로 원시 값이죠. 이런 원시 값을 왜 객체로 생성할까요?

<br>

## 원시값과 래퍼 객체
> 원시 값은 객체가 아니므로 프로퍼티나 메서드를 가질 수 없지만, 래퍼 객체를 사용하면 마치 객체인 것처럼 사용할 수 있습니다.

아래의 코드를 봅시다.
```js
const str = 'hi';

console.log(str.length);        // 2
console.log(str.toUpperCase()); // HI
```

어떻게 이럴까요? 이게 가능한 이유는 이들 원시 값에 대해 마치 객체처럼 마침표 표기법(또는 대괄호 표기법)으로 접근하면 일시적으로 그들과 연관된 `객체`로 변환해 주기 때문입니다.

즉, 자바스크립트 엔진은 암묵적으로 연관된 객체를 생성하여 생성된 객체로 프로퍼티에 접근하거나 메서드를 호출하고, 다시 원시 값으로 되돌립니다. 이러한 임시로 생성된 객체를 **래퍼 객체(Wrapper Object)** 라고 합니다. 예를 들어 문자열에 대해 마침표 표기법으로 접근하면 래퍼 객체인 String 생성자 함수의 인스턴스가 생성되고, 문자열은 래퍼 객체의 `[[StringData]]` 내부 슬롯에 할당되는 것이죠. 그럼 String.prototype의 메서드를 상속받아 사용할 수 있게 됩니다.

래퍼 객체의 처리가 종료되면 래퍼 객체의 `[[StringData]]` 내부 슬롯에 할당된 원시 값을 원래의 상태로 되돌리고 래퍼 객체는 GC(Garbage Collection)의 대상이 됩니다. 이는 Number, Boolean도 동일하게 동작합니다. Symbol은 추후 자세히 알아보도록 하죠. 이 외의 원시 값인 null, undefined는 래퍼 객체를 생성하지 않으며, 객체처럼 사용하면 에러가 발생한다는 것을 유의하세요.

<br>

## 전역 객체
> 코드가 실행되기 이전 단계에 자바스크립트 엔진에 의해 어떤 객체보다 먼저 생성되는 특수한 객체로 어떤 객체에 속하지 않은 **최상위 객체**입니다.

브라우저 환경에서는 winow, self, this, frames가, Node.js 환경에서는 global이 전역 객체를 가리키는 만큼 환경마다 이름이 다릅니다. 그 중 ES11에 도입된 globalThis는 이 식별자를 통일한 식별자로 ECMAScript 표준입니다.

```js
// ES11에 도입된 globalThis

// 브라우저
console.log(globalThis === this);   // true
console.log(globalThis === window); // true
console.log(globalThis === self);   // true
console.log(globalThis === frames); // true

// Node.js
console.log(globalThis === this);   // true
console.log(globalThis === global); // true
```

전역 객체는 `표준 빌트인 객체`와 환경에 따른 `호스트(클라이언트 Web API, Node.js Host API) 객체`, `var 키워드로 선언한 전역 변수/함수`를 프로퍼티로 갖습니다. 즉, 전역 객체 자신은 어떤 객체의 프로퍼티도 아니고, 객체의 계층적 구조상 이들을 프로퍼티로 소유하는 것이죠. 전역 객체에 대해 정리하자면 아래와 같습니다.
1. 전역 객체는 개발자가 생성할 수 없습니다. 이 말은 전역 객체를 생성할 수 있는 생성자 함수가 제공되지 않음을 의미합니다.
2. 전역 객체의 프로퍼티를 참조할 때 window(또는 global)을 생략할 수 있습니다.
3. 전역 객체는 표준 빌트인 객체를 프로퍼티로 소유합니다.
4. 자바스크립트 실행 환경에 따라 추가적인 프로퍼티와 메서드를 갖습니다.
5. var 키워드로 선언한 전역 변수, 암묵적 전역, 전역 함수를 프로퍼티로 소유합니다.
6. let, const 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아닙니다(이들은 전역 렉시컬 환경의 선언적 환경 레코드인 개념적 블록에 존재합니다).
7. 브라우저 환경의 모든 자바스크립트 코드는 단 하나의 전역 객체 window를 공유합니다. 이 말은 분리된 자바스크립트 코드도 하나의 전역을 공유한다는 의미입니다.

전역 객체의 대표적인 프로퍼티를 몇개 살펴볼까요?

<br>

### 빌트인 전역 프로퍼티
> 전역 객체의 프로퍼티로, 주로 애플리케이션 전역에 사용하는 값을 제공합니다.

- `Infinity` : 무한대를 나타내는 숫자값 `Infinity`를 갖습니다.
  ```js
  console.log(window.Infinity === Infinity);  // true

  console.log(3/0);             // Infinity, 양의 무한대
  console.log(-3/0);            // -Infinity, 음의 무한대
  console.log(typeof Infinity); // number
  ```

<br>

- `NaN` : 숫자가 아니(Not-a-Number)라는 숫자값 NaN을 갖습니다.
  ```js
  console.log(window.NaN);    // NaN
  console.log(Number('xyz')); // NaN
  console.log(1*'str');       // NaN
  console.log(typeof NaN);    // Number
  ```

<br>

- `undefined` : 원시 값인 undefined를 값으로 갖습니다.
  ```js
  console.log(window.undefined);  // undefined
  var foo;
  console.log(foo);               // undefined
  console.log(typeof undefined);  // undefined
  ```

<br>

### 빌트인 전역 함수
> 애플리케이션 전역에서 호출할 수 있는 빌트인 함수로서 전역 객체의 메서드입니다.

- `eval` : 자바스크립트 코드를 나타내는 문자열을 인수로 전달받고 이를 평가하여 값을 생성합니다.
  ```js
  eval('1 + 2;');     // 3
  eval('var x = 5;'); // undefined
  console.log(x);     // 5

  // 객체 리터럴은 반드시 괄호로 감싸야 합니다.
  const o = eval('({ a: 1 })');
  console.log(o);     // { a: 1 }

  // 함수 리터럴은 반드시 괄호로 감싸야 합니다.
  const f = eval('(function() { return 1; })');
  console.log(f());   // 1
  ```
  - 사용하지 마세요! 최적화가 진행되지 않아 속도가 느리고, 런타임에 평가하여 값을 생성하기 때문에 코드 변조의 위험이 있습니다.

<br>

- `isFinite` : 전달받은 인수가 정상적인 유한수인지 검사하여 유한수이면 true, 무한수면 false를 반환합니다.
  ```js
  console.log(isFinite(0));         // true
  console.log(isFinite('10'));      // true
  console.log(isFinite(Infinity));  // false
  ```

<br>

- `isNaN` : 전달받은 인수가 NaN인지 검사하여 그 결과를 불리언 타입으로 반환합니다.
  ```js
  console.log(isNaN(NaN));                    // true
  console.log(isNaN(10));                     // false
  console.log(isNaN(new Date()));             // false
  console.log(isNaN(new Date().toString()));  // true
  ```

<br>  

- `parseFloat` : 전달받은 문자열 인수를 부동 소수점 숫자, 즉 실수로 해석하여 반환합니다.
  ```js
  console.log(parseFloat('3.14'));      // 3.14
  console.log(parseFloat('10.00'));     // 10
  console.log(parseFloat('34 45 66'));  // 34
  ```
  - 공백 구분 문자열은 첫 번째 문자열만 반환합니다.

<br>

- `parseInt` : 전달받은 문자열 인수를 정수로 해석하여 반환합니다.
  ```js
  console.log(parseFloat('10'));      // 10
  console.log(parseFloat('10.123'));  // 10
  ```

<br>

아래 함수들을 알아보기 앞서 URI와 인코딩에 대해 간략적으로 알아볼까요?

> URI는 인터넷에 있는 자원을 나타내는 주소입니다. 우리가 흔히 하는 URI의 하위 개념으로 URL, URN이 있죠.

> 인코딩(Encoding)이란 URI 문자들을 이스케이프 처리하는 것입니다. 네트워크로 정보를 공유할 때 어떤 시스템에서도 읽을 수 있는 아스키 문자 셋(ASCII Character set)으로 변환하는 거죠. 한글을 포함한 대부분의 외국어나 특수 문자의 경우 URL에 포함될 수 없기에 이를 이스케이프 처리하여 문제를 예방하는 것입니다.

그럼 함수는 뭘까요?

<br>

- `encodeURI/decodeURI` : 전달된 문자열을 완전한 URI로 간주하여 쿼리스트링 구분자 `=`, `?`, `&`는 인코딩하지 않습니다.
  1. encodeURI는 URI를 문자열로 전달받아 인코딩합니다.
  2. decodeURI는 인코딩된 URI를 인수로 전달받아 디코딩합니다.


  ```js
  const url = 'http://localhost:8080/자바스크립트/공부를위한/저장소/FECrash';
  const encUrl = encodeURI(url);
  console.log(encodeURI(url));
  console.log(decodeURI(encUrl));
  ```

<br>

- `encodeURIComponent/decodeURIComponent` : URI의 구성 요소인 쿼리 스트링의 일부로 간주하여 `=`, `?`, `&`까지 인코딩합니다.
  1. encodeURIComponent 함수는 URI 구성 요소를 인수로 전달받아 인코딩합니다.
  2. decodeURIComponent 함수는 매개변수 전달된 URI 구성 요소를 디코딩합니다.

<br>

### 암묵적 전역
> [strict mode](https://github.com/FECrash/JavascriptCrash/blob/main/Javascript/strict_mode.md#strict-mode) 페이지를 참고하세요.

<hr>
<br>