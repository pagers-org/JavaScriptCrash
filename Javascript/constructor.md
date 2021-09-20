우리는 이미 객체를 배우면서 많은 걸 깨우쳤습니다. 하지만 조금 깊게 알아봐야 합니다. 이번 장이 끝나면 우리는 ES6에서 왜 이런 기능이 추가되었는지 알 수 있어요!

## 생성자 함수(constructor)에 의한 객체 생성
> 객체는 객체 리터럴 이외에도 다양한 방법으로 생성할 수 있습니다.

객체 리터럴을 사용하여 객체를 생성하는 방식과 생성자 함수를 사용하여 객체를 생성하는 방식의 장단점을 살펴볼까요?

### Object 생성자 함수
> new 연산자와 함께 Object 생성자 함수를 호출하여 빈 객체를 생성하고 반환합니다. 이후 동적으로 프로퍼티를 추가할 수 있습니다.

```js
// 빈 객체 생성
const person = new Object();
// const person = {}; 와 동일한 효과
```

`new` 연산자를 사용하여 객체를 생성하는 함수가 생성자 함수(Constructor)이며 자바스크립트는 Object 생성자 함수 이외에도 `String`, `Number`, `Boolean`, `Function`, `Array`, `Date`, `RegExp`, `Promise` 등의 빌트인(Built-in) 생성자 함수를 제공합니다. 이 생성자 함수로 생성된 객체를 인스턴스(Instance)라고 하죠.

오, 그런데 객체 리터럴이 좀 더 편해보이죠? 반드시 이 생성자 함수를 사용해 빈 객체를 생성해야 하는 것은 아닙니다.

<br>

### 생성자 함수
> 다른 선언 방식은 여러 문제점, 불편한 부분이 존재했습니다.

- `객체 리터럴`에 의한 객체 생성 방식
  ```js
  const circle1 = {
    radius: 5,
    getD() {
      return 2 * this.radius;
    },
  };
  const circle2 = {
    radius: 6,
    getD() {
      return 2 * this.radius;
    },
  };
  ```
  - `{}`로 객체를 만드는 것은 직관적이며 간편합니다. 그러나 단 하나의 객체만을 생성하므로, 동잃한 프로퍼티를 갖는 객체를 여러 개 생성해야 하는 경우 비효율적일 수 밖에 없습니다.

- `생성자 함수`에 의한 객체 생성 방식
  ```js
  // 생성자 함수
  function Circle(radius) {
    this.radius = radius;
    this.getD = function () {
      return 2 * this.radius;
    };
  }

  const circle1 = new Circle(5);
  const circle2 = new Circle(6);
  ```
  - 마치 객체(인스턴스)를 생성하기 위한 템플릿(클래스)처럼 생성자 함수를 사용하여 프로퍼티 구조가 동일한 객체 여러 개를 간편하게 생성할 수 있습니다.

자, 생성자 함수에 대해 자세히 알아볼까요? 생성자 함수는 위에서 살펴보았듯 객체(인스턴스)를 생성하는 함수입니다. 다른 언어와는 달리 `일반 함수와 동일한 방법으로 정의`한 뒤, **new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작**합니다. new 연산자를 호출하지 않으면 일반 함수로 동작하죠.

```js
// 일반 함수로 호출
const circle3 = Circle(15);

// 일반 함수인 Circle은 반환문(return)이 없습니다.
console.log(circle3); // undefined

// 일반 함수인 Circle 내의 this는 전역 객체를 가리킵니다.
console.log(radius);  // 15
```

`this`가 전역 객체를 가리킨다고 적었습니다. 생소한 개념이 다시 고개를 내밀었네요. this에 대해서는 다른 장에서 자세히 다룹니다. 그러나 이 this가 `바인딩(Binding)`되는 개념은 이해하고 넘어갈 필요가 있습니다.

> this
- 객체 자신의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수(Self-Reference Variable)입니다.
- this가 가리키는 값인 this 바인딩(`식별자와 값을 연결하는 과정`)은 함수 호출 방식에 따라 동적으로 결정됩니다.

  |    함수 호출 방식    |     this가 가리키는 값(this 바인딩)      |
  | :------------------: | :--------------------------------------: |
  |  일반 함수로서 호출  |           전역 객체(`window`)            |
  |   메서드로서 호출    | 메서드를 호출한 객체(`마침표 앞의 객체`) |
  | 생성자 함수로서 호출 |     생성자 함수가 생성할 `인스턴스`      |

- 코드로 확인해보면 아래와 같아요.
  ```js
  // 함수는 다양한 방식으로 호출될 수 있으므로
  // 함수로 선언합니다.
  function foo() {
    console.log(this);
  }

  // 1. 일반 함수로서 호출
  // 전역 객체는 브라우저 : window, Node.js : global 입니다.
  foo();                      // window

  // ES6 프로퍼티 축약 표현
  const obj = { foo };
  // 2. 메서드로서 호출
  obj.foo();                  // obj

  // 3. 생성자 함수로서 호출
  const instance = new foo(); // instance
  ```

<br>

### 생성자 함수의 인스턴스 생성 과정
> 생성자 함수가 인스턴스를 생성하는 것은 **필수**, 생성된 인스턴스를 초기화하는 것은 *옵션*입니다.

자바스크립트 엔진은 암묵적인 처리를 통해 인스턴스를 생성하고 반환합니다. new 연산자와 함께 생성자 호출하면 아래와 같이 흐릅니다.
1. 인스턴스 생성
   - `런타임 이전`에 암묵적으로 빈 객체를 생성(완성되지 않았지만 이 빈 객체가 생성자 함수로 생성한 인스턴스에요)한 뒤, this에 바인딩합니다.
2. 인스턴스 초기화
   - 생정자 함수에 기술된 코드가 실행되어 this에 바인딩된 인스턴스를 초기화합니다.
   - 인스턴스에 프로퍼티나 메서드를 추가하고, 생성자 함수의 인수로 전달받은 초기값을 인스턴스 프로퍼티에 할당하여 초기화하거나 고정값을 할당합니다.
3. 인스턴스 반환
   - 생성자 함수 내부의 모든 처리를 끝내고, 완성된 인스턴스에 바인딩 된 this가 암묵적으로 반환됩니다.
   - 이때 `return`에 객체를 반환하도록 명시되어 있다면 `암묵적인 this` 반환이 이 무시되므로 `생략`하거나, `return this`를 사용하세요.

이론을 이해했다면 코드로 확인해볼까요?
```js
function Circle(radius) {
  // 1. 암묵적으로 인스턴스가 생성되고 this 에 바인딩
  console.log(this); // Circle {}

  // 2. this에 바인딩 된 인스턴스를 초기화
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };

  // 3. this 가 암묵적으로 this 반환
  // 참조 값 반환시 암묵적 this 반환이 무시됩니다.
  return {};

  // 원시 값 반환시 무시되고 암묵적 this 반환이 이루어집니다.
  return 100;
}

const circle = new Circle(1);
console.log(circle); // Circle { radius: 1, getDiameter: [Function (anonymous)] }
```

<br>

### 내부 메서드 `[[Call]]`과 `[[Construct]]`
> 일반 객체는 호출이 불가능하지만 함수는 호출(Call)할 수 있는 이유가 드디어 등장합니다.

함수는 객체이지만 일반 객체와는 달리 `호출`할 수 있습니다. 그 이유는 함수 객체는 일반 객체의 내부 슬롯과 내부 메서드를 포함하여 함수로서 동작하기 위한 내부 슬롯(`[[Environment]]`, `[[FormalParameters]]` 등)과 내부 메서드(`[[Call]]`과 `[[Construct]]`)를 추가로 가지고 있습니다.

정리하자면 아래와 같습니다.

```js
function foo() {}

// 1. 일반 함수로서 호출 : [[Call]]이 호출됩니다.
foo();

// 2. 생성자 함수로서 호출: [[Constructor]]가 호출됩니다.
new foo();
```


- **일반 함수**로서 호출 : 함수 객체 내부 메서드인 `[[Call]]` 호출
  - `callable`이라 부릅니다.
- **생성자 함수**로서 호출 : 내부 메서드 `[[Construct]]` 호출
  - `constructor`라고 부릅니다.
  - `[[Construct]]`가 없는 함수 객체를 `non-constructor`라고 부릅니다.

함수로서 기능하는 객체는 반드시 callable이 되어야 합니다. 따라서 모든 함수 객체는 내부 메서드 `[[Call]]`을 갖고 있으므로 호출이 가능하죠. 그러나 모든 함수 객체가 `[[Constructor]]`를 갖는 것은 아닙니다.

즉, 함수 객체는 callable이면서 constructor이거나 non-constructor입니다.

<br>

### construtor와 non-contructor의 구분
> 대체 어떻게 구분할까요? 이렇게 복잡한데 말이죠.

자바스크립트는 함수 정의 방식에 따라 구분합니다.
- construtor : 함수 선언문, 함수 표현식, 클래스(class)
- non-construtor : ES6 메서드 축약표현, 화살표 함수(Arrow Function)

정리해봅시다! `new` 연산자로 호출하는 함수가 non-construtor이면 안되고, construtor 함수라도 `new` 연산자로 호출하지 않으면 `[[Call]]`호출이 되니 주의해야 합니다.

추가적으로 construtor라는 것을 확실히 이해시키기 위해 생성자 함수의 이름은 파스칼 케이스를 사용하여 선언하는 것을 권장합니다.

<br>

### new 연산자
> new 연산자와 함께 함수를 호출하면 함수 객체의 내부 메서드 `[[Construct]]`가 호출됩니다.

즉, new 연산자와 함께 호출되는 함수는 constructor이어야 하는 것이죠! 이는 위의 내용을 추가적으로 설명한 것이니 같이 이해하면 됩니다.

우리는 여기서 이런 문제점을 맞닥뜨리게 됩니다. 생성자 함수가 new 연산자 없이 호출되는 경우 치명적인 에러가 발생할 수 있습니다. 이걸 어떻게 강제해야 할까요?

<br>

### `new.target`과 스코프 세이프 생성자 패턴(Scope-safe Constructor)
> 개발자의 실수를 방지하기 위한 해결책입니다.

생성자 함수라도 `new` 없이 호출할 수 있기에, 무조건 `[[Construct]]`로 호출하려면 재귀적으로 구현할 수 있습니다.

```js
function Circle(radius) {
  // 1. new.target 으로 생성자 함수로 호출되었는지 여부를 알 수 있습니다.
  // new 연산자와 함께 호출되지 않았다면 new.target은 undefined입니다.
  if (!new.target) {
    return new Circle(radius);
  }

  // 2. new로 호출하지 않으면 this 가 다른 객체에 바인딩 되는 것을 이용합니다.
  // ES6의 new.target을 지원하지 않는 브라우저를 위한 스코프 세이프 생성자 패턴입니다.
  if (!(this instanceof Circle)) {
    return new Circle(radius);
  }
}
```

<hr>
<br>