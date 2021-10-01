객체는 상태(state)를 나타내는 프로퍼티(Property)와 동작(behavior)을 나타내는 메서드(Method)를 하나의 논리적 단위로 묶은 복합적 자료구조라 했죠? 이때 메서드가 내부 프로퍼티를 제어하기 위해선 자신이 속한 객체를 가리키는 `무언가`가 존재해야 합니다. 저희는 이 무언가인 **this**에 대해 알아봅니다.

## this 키워드
> 자기 참조 변수(self-referencing variable)로 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리킵니다.

잠깐 객체 리터럴의 경우를 생각해봅시다. 우리는 객체를 생성하여 내부 메서드를 정의하면 메서드 내부에서 자신이 속한 객체의 식별자를 참조할 수 있었습니다.

- `객체 리터럴 방식`으로 선언한 경우
  ```js
  const circle = {
    // 프로퍼티: 객체 고유의 상태 데이터
    radius: 5,
    // 메서드: 상태 데이터를 참조하고 조작하는 동작
    getDiameter() {
      // 자신이 속한 객체인 circle을 참조합니다.
      return 2 * circle.radius;
    }
  };

  console.log(circle.getDiameter()); // 10
  ```
  - `getDiameter`를 호출하면 circle을 재귀적으로 호출하죠? 즉, 이 참조 표현식이 평가되는 시점은 getDiameter 메서드가 호출되어 함수 몸체가 실행되는 시점입니다.

객체 리터럴은 변수에 할당되기 전에 평가되는 것은 계속 언급해왔죠? 즉 getDiameter가 호출되는 시점에는 객체가 생성된 시점이므로 메서드 내부에서 해당 식별자(위 예제의 경우 circle)를 참조할 수 있는 것이죠. 하지만 내가 속한 객체를 재귀적으로 참조한다는 것은 당연히 권장되지 않습니다.

- `생성자 함수 방식`으로 선언한 경우
  ```js
  function Circle(radius) {
    // 이 시점에는 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없죠?
    ????.radius = radius;
  }

  Circle.prototype.getDiameter = function () {
    // 여기도 마찬가지입니다.
    return 2 * ????.radius;
  };

  // 생성자 함수로 인스턴스를 생성합니다.
  const circle = new Circle(5);
  ```
  - 생성자 함수 내부에서는 자신이 생성한 인스턴스를 참조할 수 있어야 합니다. 그러나 생성자 함수가 호출되지 않는 한 인스턴스가 생성되지 않죠.
  - 결국 생성자 함수를 정의하는 시점에는 이 생성자 함수가 어떤 인스턴스를 생성하는지, 식별자는 무엇인지 알 수 없습니다.

결국 자신이 속한 객체나 생성한 인스턴스를 가리킬 식별자가 필요하다는 것입니다. 이게 바로 `this`에요. 자기 참조 변수(self-referencing variable)로서 this를 통해 해당 객체나 인스턴스의 프로퍼티와 메서드를 참조할 수 있게 되는 것입니다. 그 특징은 아래와 같습니다.
1. 자바스크립트 엔진에 의해 암묵적으로 생성됩니다.
2. 따라서 어디서든 참조할 수 있습니다.
3. 함수를 호출하면 arguments 객체와 this가 암묵적으로 전달됩니다.
4. this도 지역변수처럼 사용할 수 있습니다.
5. **this가 가리키는 값, `this 바인딩`은 함수 호출 방식에 따라 동적으로 결정됩니다**.

앞선 예제를 this로 수정해볼까요?

- `객체 리터럴 방식`으로 선언한 경우
  ```js
  const circle = {
    // 프로퍼티: 객체 고유의 상태 데이터
    radius: 5,
    // 메서드: 상태 데이터를 참조하고 조작하는 동작
    getDiameter() {
      // 자신이 속한 객체인 circle을 참조합니다.
      return 2 * this.radius;
    }
  };

  console.log(circle.getDiameter()); // 10
  ```

- `생성자 함수 방식`으로 선언한 경우
  ```js
  function Circle(radius) {
    // 이 시점에는 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없죠?
    this.radius = radius;
  }

  Circle.prototype.getDiameter = function () {
    // 여기도 마찬가지입니다.
    return 2 * this.radius;
  };

  // 생성자 함수로 인스턴스를 생성합니다.
  const circle = new Circle(5);
  ```

this 바인딩이란 식별자와 값을 연결하는 과정으로, this(키워드이나 식별자로 분류돼요)와 this가 가리킬 객체를 연결하는 것입니다. `클래스 기반 언어`는 이 this가 가리키는 대상이 `클래스가 생성하는 인스턴스`이지만 자바스크립트는 함수 호출 방식에 따라 다르다는 것을 유의해주세요.

> strict mode는 this 바인딩에도 영향이 있습니다. [여기](https://github.com/FECrash/JavascriptCrash/blob/main/Javascript/strict_mode.md#%EC%9D%BC%EB%B0%98-%ED%95%A8%EC%88%98%EC%9D%98-this)를 참조해주세요.

this는 자기 참조 변수입니다. 일반 함수 내부에서는 사용할 필요가 없기 떄문에 undefined가 할당되는게 정상(strict mode처럼요)이지만 기본 자바스크립트 엔진은 전역 객체에 바인딩된 상태이므로 `window`를 출력한다는 것을 알아두세요.

<br>

자바스크립트에서는 동일한 함수도 다양한 방식으로 호출할 수 있습니다. 함수가 동일한데 다양한 방식으로 호출할 수 있다니요...? 무슨 종류가 있을까요? 여기서는 종류와 바인딩이 어떻게 결정되는지 자세히 알아봅니다.

<br>

## 함수 호출 방식과 this 바인딩
> this 바인딩은 함수가 **어떻게** 호출 되었는지에 따라 동적으로 결정됩니다.

> 잠깐! ✋ **렉시컬 스코프와 this 바인딩은 결정 시기가 달라요.**
> - 렉시컬 스코프(Lexical Scope)는 함수 정의가 평가되고 함수 객체가 생성되는 시점에 상위 스코프를 결정합니다.
> - this 바인딩은 함수 호출 시점에 결정됩니다.

함수를 호출하는 방식과 this 바인딩은 아래처럼 다양합니다.

<div align=center>

|함수 호출 방식|this 바인딩|
|----|----|
|`일반 함수` 호출|**전역** 객체|
|`메서드` 호출|메서드를 **호출**한 객체|
|`생성자 함수` 호출|생성자 함수가 **미래에 생성할 인스턴스**|
|Function.prototype.apply/call/bind<br>메서드에 의한 `간접` 호출|Function.prototype.apply/call/bind<br>메서드에 첫 번째 인수로 전달된 객체|

</div>

<br>

### 일반 함수 호출
> 기본적으로 **전역 객체(global object)** 에 this가 바인딩됩니다.

전역 함수와 내부 함수 상관 없이 일반 함수로 호출하면 **함수 내부의 this에는 전역 객체가 바인딩**됩니다. 그것이 메서드 내에서 정의한 내부 함수나 콜백 함수라도 동일합니다.

```js
// 1. 일반 함수에 정의한 내부 함수
function foo() {
  console.log(this);    // window
  function bar() {
    console.log(this);  // window
  }
  "use strict";
  function bar2(){
    console.log(this);  // undefined
  }
  bar();
}
foo();

// 전역 객체의 프로퍼티로서 할당되는 var 키워드
var value = 1;

// 2. 메서드에서 정의한 내부 함수
const obj = {
  value: 100,
  foo() {
    console.log(this);          // {value: 100, foo: ƒ}
    console.log(this.value);    // 100

    function bar() {
      console.log(this);        // window
      console.log(this.value);  // 1
    }

    bar();
  }
};

obj.foo();

// 3. 메서드 내의 콜백 함수
const obj = {
  value: 100,
  foo() {
    console.log(this);          // {value: 100, foo: ƒ}
    setTimeout(function () {
      console.log(this);        // window
      console.log(this.value);  // 1
    }, 100);
  }
};

obj.foo();
```

결국 **일반 함수로 호출된 모든 함수(내부, 콜백 등) 내부의 this는 전역 객체가 바인딩**됩니다.

당연히 this가 전역 객체에만 바인딩 되는 것은 문제가 있으므로, 메서드의 내부 함수나 콜백 함수의 this 바인딩을 메서드의 this와 일치시키기 위해서는 `this 식별자를 변수에 할당`하거나 `apply/call/bind 메서드`, `화살표 함수`를 사용해주세요.

- this를 변수에 할당하기
  ```js
  var value = 1;

  const obj = {
    value: 100,
    foo() {
      const that = this;

      setTimeout(function () {
        console.log(that.value); // 100
      }, 100);
    }
  };

  obj.foo();
  ```

- apply/call/bind 메서드 사용하기
  ```js
  var value = 1;

  const obj = {
    value: 100,
    foo() {
      setTimeout(function () {
        console.log(this.value); // 100
      }.bind(this), 100);
    }
  };

  obj.foo();
  ```

- 화살표 함수 사용하기 : 화살표 함수의 this는 상위 스코프의 this와 바인딩됩니다.
  ```js
  var value = 1;

  const obj = {
    value: 100,
    value2 : this.value // 1
    foo() {
      setTimeout(() => console.log(this.value), 100); // 100
    }
  };

  obj.foo();
  ```

<br>

### 메서드 호출
> 메서드를 호출할 때 마침표 연산자 앞에 기술한 객체가 바인딩됩니다.

메서드 내부의 this는 프로퍼티로 메서드를 가리키는 객체와는 **관계가 없고**, 메서드를 호출한 객체에 바인딩 되는 것입니다.
```js
// 메서드 내부의 this는 메서드를 호출한 객체에 바인딩됩니다.
const person = {
  name: 'amy',
  getName() {
    return this.name;
  }
};

// 메서드 getName을 호출한 객체는 person이므로
console.log(person.getName());        // amy

const anotherPerson = {
  name: 'james'
};

// getName 메서드를 anotherPerson 객체의 메서드로 할당
anotherPerson.getName = person.getName;

// getName 메서드를 호출한 객체는 anotherPerson입니다.
console.log(anotherPerson.getName()); // james

// getName 메서드를 변수에 할당
const getName = person.getName;

// getName 메서드를 일반 함수로 호출
console.log(getName());               // ''
```

일반 함수로 호출된 `getName` 함수 내부의 `this.name`은 전역 객체의 프로퍼티 `window.name`과 같고, 브라우저의 window.name은 브라우저 창의 이름을 나타내는 빌트인 프로퍼티입니다. 따라서 기본값은 `''`이 돼요(Node.js는 `undefined`).

<br>

이는 프로토타입 내부에 사용된 this 바인딩도 동일합니다.
```js
function Person(name) {
  this.name = name;
}

Person.prototype.getName = function () {
  return this.name;
};

const me = new Person('amy');
console.log(me.getName());               // amy

Person.prototype.name = 'james';
console.log(Person.prototype.getName()); // james
```

<br>

### 생성자 함수 호출
> 생성자 함수가 **미래에 생성할 인스턴스**가 바인딩됩니다.

```js
function Circle(radius) {
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

const circle1 = new Circle(5);
console.log(circle1.getDiameter()); // 10

const circle2 = new Circle(10);
console.log(circle2.getDiameter()); // 20
```

단, new 연산자 없이 호출하면 일반 함수이므로... 어떻게 동작하게 되는진 예상되시죠?

<br>

### Function.prototype.apply/call/bind 메서드에 의한 간접 호출
> Function.prototype의 메서드로 모든 함수가 상속받아 사용할 수 있습니다.

apply, call은 `this로 사용할 객체와 인수 리스트`를, bind는 `this만`을 인수로 받습니다.

- apply : `Function.prototype.apply(thisArg[, argsArray])`
  - thisArg : this로 사용할 객체
  - argsArray : 함수에 전달할 인수 리스트

- call : `Function.prototype.call(thisArg[, arg1[, arg2[, ...]]])`
  - thisArg : this로 사용할 객체
  - arg1, arg2, ... : 함수에 전달할 인수 리스트

- bind : `Function.prototype.bind(thisArg)`
  - thisArg : this로 사용할 객체

> 자세한 사용법은 MDN을 참고해주세요.
> - [apply()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)
> - [call()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function/call)
> - [bind()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)

<br>

apply와 call 메서드는 본래 함수를 호출하는 것이지만, 첫 번째 인수로 전달한 특정 객체를 호출한 함수의 this에 바인딩하죠. 인수를 전달하는 방식이 다를 뿐이지 동작 방식은 같습니다.

- 다음은 getThisBinding 함수를 호출하면서 인수로 전달한 객체를 getThisBinding 함수의 this에 바인딩시키는 코드입니다.
  ```js
  function getThisBinding() {
    console.log(arguments);
    return this;
  }

  // this로 사용할 객체
  const thisArg = { a: 1 };

  // 1. apply 메서드는 호출할 함수의 인수를 배열로 묶어 전달
  console.log(getThisBinding.apply(thisArg, [1, 2, 3]));
  // Arguments(3) [1, 2, 3, callee: ƒ, Symbol(Symbol.iterator): ƒ]
  // {a: 1}

  // 2. call 메서드는 호출할 함수의 인수를 쉼표로 구분한 리스트 형식으로 전달
  console.log(getThisBinding.call(thisArg, 1, 2, 3));
  // Arguments(3) [1, 2, 3, callee: ƒ, Symbol(Symbol.iterator): ƒ]
  // {a: 1}
  ```
  - 이들의 대표적인 용도는 **유사 배열 객체에 배열 메서드를 사용하는 경우**입니다.
      ```js
      // 1. arguments 객체를 배열로 변환
      function convertArgsToArray() {
        console.log(arguments);

        // Array.prototype.slice를 인수없이 호출하면 배열의 복사본을 생성
        const arr = Array.prototype.slice.call(arguments);
        // const arr = Array.prototype.slice.apply(arguments);
        console.log(arr);

        return arr;
      }

      convertArgsToArray(1, 2, 3); // [1, 2, 3]
      ```

<br>

bind 메서드는 메서드의 this와 메서드의 내부/콜백 함수의 this가 불일치하는 문제를 해결하기 위해 유용하게 사용됩니다.

- 다음은 bind 메서드로 this를 일치시키는 코드입니다.
  ```js
  const person = {
    name: 'amy',
    foo(callback) {
      // bind 메서드로 callback 함수 내부의 this 바인딩 전달
      setTimeout(callback.bind(this), 100);
    }
  };

  person.foo(function () {
    console.log(`Hi! ${this.name}.`); // Hi! amy.
  });
  ```

<hr>
<br>