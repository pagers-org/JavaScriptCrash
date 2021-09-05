하나 이상의 표현식을 산술, 할당, 비교, 논리, 타입, 지수 연산 등을 수행하여 하나의 값으로 만드는 행위입니다. 코드의 핵심이니 잘 알아둘까요?

## 연산자(Operator)
> 연상의 대상은 피연산자(operand)이며, 값으로 평가될 수 있는 표현식이어야 합니다.

연산자는 **피연산자를 연산해 새로운 값을 만드는** 역할입니다. 연산자의 종류별로 알아두어야 할 내용을 짚어보죠.

### 산술 연산자(Arithmetic Operator)
> 수학적 계산을 수행하여 새로운 `숫자` 값을 만듭니다. 불가능할 경우 `NaN`을 반환합니다.

피연산자의 개수에 따라 이항, 단항 산술 연산자로 구분됩니다.

- 이항 산술 연산자 : 피연산자의 값을 변경하는 부수 효과(side effect)가 없습니다. 항상 새로운 값을 만들 뿐이죠.
  ```js
  const a = 5;
  const b = 2;
  console.log(5 + 2); // 덧셈, 7 반환
  console.log(5 - 2); // 뺄셈, 3 반환
  console.log(5 * 2); // 곱셈, 10 반환
  console.log(5 / 2); // 몫 연산, 2.5 반환
  console.log(5 % 2); // 나머지 연산, 1 반환
  ```

- 단항 산술 연산자  : 피연산자 한 개를 산술 연산하여 숫자 값을 만듭니다.
  ```js
  /*
   * ++ : 증가, 피연산자 값을 변경하는 부수효과(암묵적 할당)가 일어납니다.
   * -- : 감소, 피연산자 값을 변경하는 부수효과(암묵적 할당)가 일어납니다.
   */

  // 선할당, 후증가(postfix increment operator)
  let a = 0, result = 0;
  result = x++;
  console.log(result, x); // 0 1

  // 선증가, 후할당(prefix increment operator)
  x = 0, result;
  result = ++x; 
  console.log(result, x); // 1 1

  // 선할당, 후감소(postfix decrement operator)
  x = 0, result;
  result = x--;
  console.log(result, x); // 0 -1

  // 선감소, 후할당(prefix decrement operator)
  x = 0, result;
  result = --x;
  console.log(result, x); // -1 -1

  /*
   * + : 다른 타입이라면 숫자 타입으로 반환한 값을 생성하여 반환합니다.
   * - : 숫자 타입에는 부호 반전, 다른 타입에는 부호를 반전한 숫자 타입으로 반환합니다.
   */
  
  // (+) 숫자 타입에 사용
  let a = 10;
  console.log(+a);    // 10
  console.log(+(-a)); // -10

  // (+) 문자열 타입에 사용
  let b = '10';
  console.log(+10);   // 10
  b = 'Hi';
  console.log(+b);    // NaN

  // (+) 불리언 타입에 사용
  b = true;
  console.log(+b);    // 1
  b = false;
  console.log(+b);    // 0

  // (-) 숫자 타입에 사용
  a = 10;
  console.log(-a);    // -10
  console.log(-(-a)); // 10

  // (-) 문자열 타입에 사용
  b = '10';
  console.log(-10);   // -10
  b = 'Hi';
  console.log(-b);    // NaN

  // (-) 불리언 타입에 사용
  b = true;
  console.log(-b);    // -1
  b = false;
  console.log(-b);    // -0, 0으로 타입 변환
  ```

- 문자열 연결 연산자 : + 연산자는 피연산자 중 하나 이상이 문자열이라면 문장 연결 연산자로 동작합니다. 그 외는 산술 연산자로 동작합니다.
  ```js
  let a = '1';
  console.log(a + 2); // '12'
  console.log(2 + a); // '21'

  a = 1;
  console.log(a + true);  // 2, 1로 타입 변환
  console.log(a + false); // 1, 0으로 타입 변환

  console.log(a + null);  // 1, 0으로 타입 변환

  console.log(a + undefined); // NaN, 타입 변환 X
  ```
  - 이 코드를 보면 자바스크립트 엔진이 어떻게 암묵적으로 타입을 자동 변환하는지 알 수 있습니다.
    - 이를 강제 변환(Type Coercion)이라고도 합니다. 상단의 단항 연산자도 암묵적 타입 변환이 발생한 것입니다!

<br>

### 할당 연산자(Assignment Operator)
> 피연산자의 평가 결과를 좌항의 변수에 할당합니다.

`a = 0` 처럼 좌항 변수에 값을 할당하는 것이 할당 연산자입니다. 당연히 부수 효과가 존재하겠죠? 종류는 무엇이 있을까요?

- 숫자 타입에 대한 할당 : 산술 연산
  ```js
  let a;
  a = 10;
  a += 10;
  a -= 10;
  a *= 10;
  a /= 10;
  a %= 10;
  ```

- 문자열 타입에 대한 할당 : 문자열 연결 연산
  ```js
  let a = 'S';
  a += 'tring';
  ```

결과는 직접 확인해보세요. 이해를 필요로 하는 내용이 아니므로 간략하게 넘어갑니다.

할당 연산자를 사용한 **할당문**에 가장 필요한 내용은 `표현식과 문`입니다. 즉, 할당문은 **값으로 평가되는 표현식인 문으로 할당된 값으로 평가** 되는 문입니다. 이 특징을 이용해서 아래과 같은 연산이 가능합니다.

- 연쇄 할당하기
  ```js
  let a, b, c;
  a = b = c = 10;
  console.log(a, b, c); // 10 10 10
  ```

<br>

### 비교 연산자(Comparison Operator)
> 좌항과 우항의 피연산자를 비교하여 불리언 값으로 결과를 반환합니다.

제어문이나 조건식에서 자주 사용하는 연산입니다. 종류를 알아봅니다.

- 동등/일치 비교 연산자
  - 동등 비교 : 값만을 비교하며, **==** 는 긍정, **!=** 는 부정입니다.
    - `x == y`, `x != y`
    - 동등 비교 연산자는 좌항, 우항의 피연산자를 비교하기 전에 **암묵적 타입 변환**을 통해 타입을 일치시키고 값을 비교합니다. 즉, '1' == 1이 true가 될 수 있다는 의미이므로, 값을 비교할 때는 최대한 **일치 비교 연산자를 사용** 합니다.
  - 일치 비교 : 값과 타입을 비교하며 **===** 는 긍정, **!==** 는 부정입니다.
    - `x === y`, `x !== y`
    - 일치 비교 연산자는 좌항, 우항의 피연산자가 값과 타입이 같을 때에만 비교의 결과를 반환합니다.
      - 예외적으로 NaN === NaN은 false로서 NaN의 특성 때문에 이런 현상이 발생하는 것임을 알아두세요.
    - ES6에서 추가된 `Object.is` 메소드는 정확한 비교 결과를 반환합니다.
      ```js
      0 == -0;              // true
      0 === -0;             // true
      Object.is(0, -0);     // false

      NaN === NaN;          // false
      Object.is(NaN, NaN);  // true
      ```

- 대소 관계 비교 연산자 : 좌항과 우항을 기준으로 이상(>=), 이하(<=), 초과(>), 미만(<)을 비교하는 연산입니다.

<br>

### 삼항 조건 연산자(Ternary Operator)
> 조건식의 평가 결과에 따라 반환할 값을 결정하는 자바스크립트 유일한 삼항 연산자입니다.

```js
function isString(a){
  // 조건식으로 풀면
  if(typeof a === 'string') return true;
  else return false;
  // 삼항 조건 연산자로 풀면
  return typeof a === 'string' ? true : false;
}
```

삼항 조건 연산자 표현식은 값으로 평가할 수 있는 표현식인 문이므로, 값처럼 사용할 수 있습니다.

<br>

### 논리 연산자
> 좌항과 우항의 피연산자를 논리 연산합니다. 부정 논리 연산자의 경우 우항이 대상입니다.

```js
let a = true;
console.log(a && true);
console.log(a || false);
console.log(!a);
```

- 피연산자가 불리언 타입이 아닐 경우의 예외입니다. 자바스크립트가 암묵적 타입 변환으로 다른 타입을 어떻게 표현하는지 알아두세요.
  ```js
  let a = 0;
  console.log(!a);  // true
  a = 'Hi';
  console.log(!a);  // false
  ```

- 드모르간의 법칙을 활용하면 가독성을 좀 더 높일 수 있습니다. 아래처럼 말이죠.
  ```js
  !(a || b) === (!a && !b)
  !(a && b) === (!a || !b)
  ```

<br>

### 쉼표 연산자(Comma Operator)
> 왼쪽 피연산자부터 차례대로 평가한 후 마지막 피연산자 평가 결과를 반환합니다.

```js
let a, b, c;
a = 1, b = 2, c = 3;              // 3
console.log(a = 4, b = 5, c = 6); // 4 5 6
```

<br>

### 그룹 연산자(Group Operator)
> 소괄호(`()`)로 피연산자를 감싸 연산 우선순위를 가장 높게 평가합니다.

<br>

### typeof 연산자
> 피연산자의 데이터 타입을 **문자열**로 반환합니다.

typeof 연산자가 반환하는 결과는 총 7가지('string', 'number', 'boolean', 'undefined', 'symbol', 'object', 'function')입니다.

주의사항을 살펴보도록 하죠.
1. null을 반환하는 경우는 존재하지 않으며, null을 비교하면 'object'를 반환합니다. 즉 일치 연산자로 null 타입을 확인해야 합니다.
  ```js
  let a = null;
  console.log(typeof a === null); // false
  console.log(a === null);        // true
  ```
2. 선언하지 않은 식별자를 typeof로 연산하면 ReferenceError가 아닌 undefined가 발생합니다.
  ```js
  typeof a; // undefined
  ```

<br>

### 지수 연산자(Exponentiation Operator)
> ES7에 도입되었습니다. 좌항의 피연산자가 밑(base), 우항의 피연산자가 지수(exponent)로 거듭 제곱하여 숫자 값을 반환합니다.

지수 연산자는 이항 연산자 중 우선순위가 가장 높으며, 할당 연산자와 함께 사용할 수 있습니다.

- 도입 이전 : Math 라이브러리의 pow 메서드를 활용했습니다.
  ```js
  Math.pow(2, 2); // 4
  Math.pow(2, 3); // 8
  ```

- 도입 이후 : `**`를 사용합니다.
  ```js
  2 ** 2; // 4
  2 ** 3; // 8

  // 음수 거듭제곱 시 그룹 연산자를 사용합니다.
  -5 ** 2;    // SyntaxError
  (-5) ** 2;  // 25
  ```

그 외, 연산자 우선순위는 이해와 암기가 아닙니다. 참조를 자주 해보세요. 아래에 링크를 준비해두었습니다.

연산자 우선순위 [Link](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)

<hr>
<br>
<hr>

논리 연산자를 좀 더 효율적으로 사용할 수 있는 단축 평가에 대해 알아봅니다.

## 단축 평가(Short-circuit Evaluation)
> 논리합(||)과 논리곱(&&) 연산자로 표현식의 평가 결과를 단축하여 사용하는 방법입니다.