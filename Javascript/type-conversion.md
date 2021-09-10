이번 장에서 타입 변환에 대해 자세히 다룹니다. 종류와 그 내용까지 말이죠.

## 타입 변환(Type Conversion)
> 자바스크립트의 모든 값은 타입이 존재하며, 이를 변환하는 방법에는 개발자에 의해 변환되는 방법과 개발자의 의도와 상관 없이 변환되는 방법이 있습니다.

- 타입 캐스팅(Type Casting, 또는 명시적 타입 변환(Explicit Coercion)) : 개발자가 의도적으로 값의 타입을 바꾸는 방법입니다.
  ```js
  let a = 10;
  let b = a.toString();     // 여기서 타입 캐스팅이 일어납니다.
  console.log(typeof b, b); // 'string', '10'
  console.log(typeof a, a); // 'number', 10
  ```

- 타입 강제 변환(Type Coercion, 또는 암묵적 타입 변환(Implicit Coercion)) : 자바스크립트 엔진에 의해 암묵적으로 타입이 자동 변환되는 방법입니다.
  ```js
  let a = 10;
  let b = a + '';
  console.log(typeof b, b); // 'string', '10'
  console.log(typeof a, a); // 'number', 10
  ```

<br>

위의 모든 타입 변환이 기존의 원시 값을 변경하는 것은 아닙니다! 원시 값은 불변이므로 변하지 않으니까요. 위의 예제를 다시 볼까요?

```js
let a = 10;               // 1번
let b = a + '';           // 2번
```
- `1번` : 숫자 10의 값을 메모리에 저장하고, a라는 변수가 가리키도록 합니다.
- `2번` : 자바스크립트 엔진은 a 변수의 숫자 값을 바탕으로 새로운 문자열 `'10'`을 생성하고, 표현식 `'10' + ''`를 평가합니다.
  - 이 때 암묵적으로 생성된 문자열 `'10'`을 a 변수에 재할당하지 않습니다.

이처럼 자바스크립트 엔진은 표현식을 에러 없이 평가하기 위해 아래와 같이 연산합니다.
1. 피연산자의 값을 암묵적으로 타입 변환하여 새로운 타입의 값 생성
2. 단 한 번 사용하고 버림

명시적 타입 변환이 개발자에 의해 명확히 알 수 있습니다. 그에 반해 암묵적 타입 변환은 자바스크립트 엔진에 의해 자동 변경되므로 `개발자의 예측`, 즉 `경험`을 요구하게 됩니다. 중요한 것은, 타입 변환의 종류가 아니라 개발자와의 협업에서 어떤 타입 변환을 사용해야 **가독성**과 **이해**를 높일 수 있느냐 입니다.

### 암묵적 타입 변환
> 자바스크립트 엔진에 의해 **자동**으로 변환되는 방법입니다. 문자열, 숫자, 불리언 타입 변환이 존재해요.

1. 문자열 타입 변환 : `(+)`연산자는 피연산자 중 하나 이상이 문자열이므로 문자열 연결 연산자로 동작합니다.
  - 단, ES6의 템플릿 리터럴은 **평가의 결과**를 `문자열`로 표현합니다.
    ```js
    let a = '10' + 2; // '102'
    let b = `10 + 2 = ${10 + 2}`;  // '10 + 2 = 12'
    ```
  - 더 많은 예제를 볼까요?

    | 표현식                              | 결과                                                 |
    | :---------------------------------- | ---------------------------------------------------- |
    | `console.log(0 + '');`              | '0'                                                  |
    | `console.log(-0 + '');`             | '0'                                                  |
    | `console.log(1 + '');`              | '1'                                                  |
    | `console.log(-1 + '');`             | '-1'                                                 |
    | `console.log(NaN + '');`            | 'NaN'                                                |
    | `console.log(Infinity + '');`       | 'Infinity'                                           |
    | `console.log(-Infinity + '');`      | '-Infinity'                                          |
    | `console.log(true + '');`           | 'true'                                               |
    | `console.log(false + '');`          | 'false'                                              |
    | `console.log(null + '');`           | 'null'                                               |
    | `console.log(undefined + '');`      | 'undefined'                                          |
    | `console.log((Symbol()) + '');`     | TypeError: Cannot convert a Symbol value to a string |
    | `console.log(({}) + '');`           | '[object Object]'                                    |
    | `console.log(Math + '');`           | '[object Math]'                                      |
    | `console.log([] + '');`             | ''                                                   |
    | `console.log([10, 20] + '');`       | '10,20'                                              |
    | `console.log((function(){}) + '');` | 'function(){}'                                       |
    | `console.log(Array + '');`          | 'function Array() { [native code] }'                 |

<br>

2. 숫자 타입 변환 : `(-, *, /)`는 모두 산술 연산자로, 숫자 값을 만드는 역할이며 모든 피연산자는 문맥상 모두 숫자 타입으로 변환됩니다.
  - 단, 비교 연산자 `(>, <, >=, <=, ==)`는 피연산자의 **크기**를 비교하므로 동일하게 숫자 타입 변환이 발생합니다.
    ```js
    let a = '1' > 0;  // true
    let b = '0' == 0; // true
    ```

  - 더 많은 예제를 볼까요?

    | 표현식                          | 결과                                                 |
    | :------------------------------ | ---------------------------------------------------- |
    | `console.log(+'');`             | 0                                                    |
    | `console.log(+'0');`            | 0                                                    |
    | `console.log(+'1');`            | 1                                                    |
    | `console.log(+'string');`       | NaN                                                  |
    | `console.log(+true);`           | 1                                                    |
    | `console.log(+false);`          | 0                                                    |
    | `console.log(+null);`           | 0                                                    |
    | `console.log(+undefined);`      | NaN                                                  |
    | `console.log(+Symbol());`       | TypeError: Cannot convert a Symbol value to a number |
    | `console.log(+{});`             | NaN                                                  |
    | `console.log(+[]);`             | 0                                                    |
    | `console.log(+[10, 20]);`       | NaN                                                  |
    | `console.log(+(function(){}));` | NaN                                                  |

3. 불리언 타입 변환 : 제어문, 삼항 조건 연산자는 논리적으로 평가되어야 합니다. 모든 피연산자는 모두 불리언 타입으로 변환됩니다.
  - 특이하게도 자바스크립트 엔진은 불리언 타입이 아닌 값을 **Truthy Value**(참으로 평가되는 값), **Falsy Value**(거짓으로 평가되는 값)으로 구분합니다. Falsy Value의 종류는 아래와 같으며, Falsy Value가 아닌 모든 값들은 Truthy Value입니다.
    - `false`
    - `undefined`
    - `null`
    - `0, -0`
    - `NaN`
    - `''(empty string)`

  - 더 많은 예제를 볼까요?

    | 표현식               | 결과  |
    | :------------------- | ----- |
    | `if ('') {}`         | true  |
    | `if (true) {}`       | true  |
    | `if (0) {}`          | false |
    | `if ('str') {}`      | true  |
    | `if (null) {}`       | false |
    | `if (!false) {}`     | true  |
    | `if (!undefined) {}` | true  |
    | `if (!null) {}`      | true  |
    | `if (!0) {}`         | true  |
    | `if (!NaN) {}`       | true  |
    | `if (!'') {}`        | true  |

  - 추가적으로, Falsy Value와 Truthy Value를 반환하는 형태는 아래와 같습니다.
    ```js
    if(!target) {}   // target이 Falsy Value면 true, Truthy Value면 false
    if(!!value) {}   // target이 Truthy Value면 true, Falsy Value면 false
    ```

<br>

### 명시적 타입 변환
> 개발자의 의도에 따라 **강제적**으로 변환하는 방법입니다. 표준 빌트인 생성자 함수, 메서드를 사용하여 변환합니다.

생소한 단어가 나왔죠? 빌트인(built-in)이란 자바스크립트에서 기본적으로 제공하는 기능을 일컫습니다.

표준 빌트인 생성자 함수는 원시 값을 감싸는 Wrapper 객체입니다. String, Number, Boolean을 new 없이 호출하는 형태죠. 표준 빌트인 메서드는 빌트인 객체의 공통적인 기능을 정의한 집합입니다. 우선은 이렇게만 알아두시고, 자세한 내용은 빌트인을 공부하는 장에서 이해하죠!

1. 문자열 타입 변환
  - `String 생성자 함수`를 사용하는 방법으로, new 연산자를 호출하지 않습니다.
    ```js
    let a = String(1);                    // '1'
    ```
  - `Object.prototype.toString 메서드`를 사용하는 방법입니다.
    ```js
    let a = Object.prototype.toString(1); // '1'
    ```
  - 암묵적 타입 변환인 `(+)` 문자열 연결 연산자를 사용하는 방법입니다. 예제는 다루지 않습니다. 위에 있으니까요!

  - 더 많은 예제를 볼까요?

    | 표현식                                | 결과       |
    | :------------------------------------ | ---------- |
    | `console.log(String(1));`             | '1'        |
    | `console.log(String(NaN));`           | 'NaN'      |
    | `console.log(String(Infinity));`      | 'Infinity' |
    | `console.log(String(true));`          | 'true'     |
    | `console.log(String(false));`         | 'false'    |
    | `console.log((1).toString());`        | '1'        |
    | `console.log((NaN).toString());`      | 'NaN'      |
    | `console.log((Infinity).toString());` | 'Infinity' |
    | `console.log((true).toString());`     | 'true'     |
    | `console.log((false).toString());`    | 'false'    |

2. 숫자 타입 변환
  - `Number 생성자 함수`를 사용하는 방법으로, new 연산자를 호출하지 않습니다.
    ```js
    let a = Number('1');        // 1
    ```
  - `parseInt, parseFloat 메서드`를 사용하는 방법입니다.
    ```js
    let a = parseInt('1');      // 1
    let b = parseFloat('1.00'); // 1.00
    ```
  - 단항 산술 연산자 `(+, *)`를 사용하는 방법입니다.
    ```js
    let a = +'0';               // 0
    let b = '0' * 1;            // 0
    ```

  - 더 많은 예제를 볼까요?

    | 표현식                              | 결과  |
    | :---------------------------------- | ----- |
    | `console.log(Number('0'));`         | 0     |
    | `console.log(Number('-1'));`        | -1    |
    | `console.log(Number('10.53'));`     | 10.53 |
    | `console.log(Number(true));`        | 1     |
    | `console.log(Number(false));`       | 0     |
    | `console.log(parseInt('0'));`       | 0     |
    | `console.log(parseInt('-1'));`      | -1    |
    | `console.log(parseFloat('10.53'));` | 10.53 |
    | `console.log(+'0');`                | 0     |
    | `console.log(+'-1');`               | -1    |
    | `console.log(+'10.53');`            | 10.53 |
    | `console.log(+true);`               | 1     |
    | `console.log(+false);`              | 0     |
    | `console.log('0' * 1);`             | 0     |
    | `console.log('-1' * 1);`            | -1    |
    | `console.log('10.53' * 1);`         | 10.53 |
    | `console.log(true * 1);`            | 1     |
    | `console.log(false * 1);`           | 0     |

3. 불리언 타입 변환
  - `Boolean 생성자 함수`를 사용하는 방법으로, new 연산자를 호출하지 않습니다.
    ```js
    let a = Boolean('X');   // true
    let a = Boolean('');    // false
    ```
  - `부정 논리 연산자(!)`를 **두 번** 사용하는 방법입니다.
    ```js
    let a = !!'X';          // true
    let a = !!'';           // false
    ```

  - 더 많은 예제를 볼까요?

    | 표현식                             | 결과  |
    | :--------------------------------- | ----- |
    | `console.log(Boolean('x'));`       | true  |
    | `console.log(Boolean(''));`        | false |
    | `console.log(Boolean('false'));`   | true  |
    | `console.log(Boolean(0));`         | false |
    | `console.log(Boolean(1));`         | true  |
    | `console.log(Boolean(NaN));`       | false |
    | `console.log(Boolean(Infinity));`  | true  |
    | `console.log(Boolean(null));`      | false |
    | `console.log(Boolean(undefined));` | false |
    | `console.log(Boolean({}));`        | true  |
    | `console.log(Boolean([]));`        | true  |
    | `console.log(!!'x');`              | true  |
    | `console.log(!!'');`               | false |
    | `console.log(!!'false');`          | true  |
    | `console.log(!!0);`                | false |
    | `console.log(!!1);`                | true  |
    | `console.log(!!NaN);`              | false |
    | `console.log(!!Infinity);`         | true  |
    | `console.log(!!null);`             | false |
    | `console.log(!!undefined);`        | false |
    | `console.log(!!{});`               | true  |
    | `console.log(!![]);`               | true  |

<hr>
<br>