자바스크립트는 클래스 기반의 객체 지향 프로그래밍 언어가 아니지만 프로토타입 기반의 아주 유연한 객체 지향 프로그래밍 스타일로 객체 지향 언어의 상속과 캡슐화 등의 추상적 개념도 구현할 수 있습니다. 우리는 이 장에서 ES6에 등장한 클래스(Class)에 대해 알아봅니다.

## 클래스와 생성자 함수
> ES6의 클래스가 프로토타입 기반 객체 지향 모델을 폐지한 것이 아닙니다. 클래스도 함수이기 때문이지요.

우리는 기존 객체 지향 프로그래밍을 구현할 때, 생성자 함수와 프로토타입, 클로저를 이용해왔습니다.
```js
var Person = (function () {
  // Constructor, 생성자 함수
  function Person(name) {
    this._name = name;
  }

  // public method
  Person.prototype.sayHi = function () {
    console.log('HELLO! ' + this._name);
  };

  // return constructor
  return Person;
}());

// 인스턴스 생성
var me = new Person('Amy');
me.sayHi();                        // HELLO! Amy

console.log(me instanceof Person); // true
```

이를 클래스로 구현해볼까요?
```js
class Person {
  // constructor, 생성자
  constructor(name) {
    this._name = name;
  }

  sayHi() {
    console.log(`HELLO! ${this._name}`);
  }
}

// 인스턴스 생성
const me = new Person('Amy');
me.sayHi();                        // HELLO! Amy

console.log(me instanceof Person); // true
```

동일한 동작을 하는 것처럼 보이죠? 구문도 단순화되었습니다. 즉, ES6의 클래스는 함수이며 기존 프로타입 기반 패턴에 **문법적 설탕(Syntactic sugar)** 이 됩니다. 단, 클래스와 생성자 함수가 동일하게 동작하지는 않으니까 주의(일각에선 이런 이유로 클래스를 문법적 설탕으로 인정하지 않아요)하세요. 

<br>

## 클래스 정의
> 클래스는 class 키워드를 사용하여 정의하며, 파스칼 케이스로 작성하는 것이 일반적입니다.

클래스를 어떻게 작성하는지는 언급하지 않겠습니다. 기존 프로토타입과 유사하기 때문이죠. 이 장에서는 클래스의 특징만 짚고 넘어갑니다.

1. 클래스는 클래스 선언문 이전에 참조할 수 없습니다.
   - 이는 let, const와 같습니다. 즉, 호이스팅되나 let, const 변수처럼 선언문 이전에 일시적 사각지대(TDZ)에 빠지기 때문이죠.
   - 호이스팅은 var, let, const, function, function*, class 키워드를 사용한 모든 선언문에 적용됩니다.

<br>

2. 표현식으로도 클래스를 정의할 수 있으며 익명일 수 있습니다.
    ```js
    const NamedClass = class UnNamedClass {};

    const name = new NamedClass();
    console.log(name);  // UnNamedClass {}

    new UnNamedClass(); // ReferenceError: UnNamedClass is not defined
    ```
   - 클래스가 할당된 변수를 사용하지 않고 기명 클래스로 생성한다면 에러가 발생합니다. 그 이유는 **클래스 표현식에 사용한 이름은 외부 코드에서 접근이 불가능하기 때문이죠**.

<br>

3. 생성자 함수와 마찬가지로 new 연산자와 함께 클래스를 호출하면 클래스의 인스턴스가 생성됩니다.
    ```js
    class NamedClass {};
    const name = NamedClass(); // TypeError: Class constructor NamedClass cannot be invoked without 'new'
    ```
   - 클래스를 new 연산자 없이 호출하면 타입 에러가 발생합니다.

<br>

4. 클래스는 클래스 내부의 캡슐화된 변수, **클래스 필드(Class Field)** 를 가집니다.
   - 데이터 멤버, 멤버 변수라고도 부르며 인스턴스의 프로퍼티 혹은 정적 프로퍼티가 될 수 있습니다.
   - this에 추가한 프로퍼티를 클래스 기반 객체지향 언어에서 클래스 필드라고 지칭하죠.

<br>

5. constructor는 인스턴스를 생성하고 클래스 필드를 초기화하기 위한 메서드입니다.
    ```js
    class NamedClass {};
    const name = NamedClass(); // TypeError: Class constructor NamedClass cannot be invoked without 'new'
    // NamedClass는 생성자 함수(constructor)입니다.
    console.log(Object.getPrototypeOf(name).constructor === NamedClass); // true
    ```
    - new 연산자와 함께 호출되는 것이 constructor로 파라미터에 전달한 값은 클래스 필드에 할당할 수 있습니다.
    - constructor는 인스턴스의 생성과 동시에 클래스 필드의 생성과 초기화를 실행합니다.
    - constructor는 클래스 내에 한 개만 존재하며 2개 이상일 경우 문법 에러(SyntaxError)가 발생합니다.
    - constructor를 생략하면 클래스에 constructor() {}, 즉 빈 객체를 포함하는 것처럼 동작합니다.

<br>

6. 클래스 필드에는 메서드만 선언할 수 있습니다.
    ```js
    class NamedClass {
      name = ''; // SyntaxError

      // 클래스 필드의 선언과 초기화는 반드시 constructor에서 실시합니다.
      constructor(name) {
        this.name = name;
      }
    }
    ```

    > 현재는 정상적으로 동작하는데, TC39 프로세스의 stage 3(candidate) 단계에 있는 클래스 몸체에서 직접 인스턴스 필드를 선언하고 private 인스턴스 필드를 선언할 수 있는 프로포절(Class field declarations proposal)의 필드 정의를 최신 브라우저와 최신 Node.js가 구현하였기 때문입니다.

    <br>

    - constructor 내부에서 선언한 클래스 필드는 클래스가 생성할 인스턴스를 가리키는 this에 바인딩되며, 클래스 인스턴스를 통해 외부에서 직접 참조할 수 있습니다. public한 멤버로서 취급되는 것이죠.

    <br>

    > 최신 브라우저와 Node.js 12버전 이상에서는 여러 속성들을 지원하고 있습니다. 자세한 내용은 [여기](https://github.com/tc39/proposals/blob/master/README.md)를 참조해주세요.

<br>

7. getter/setter로 클래스 필드의 값을 참조하거나, 조작할 수 있습니다.
   - 우리가 알고 있는 그 접근자 프로퍼티가 맞습니다.

<br>

8. 정적(static) 메서드를 정의할 수 있으며, 클래스 인스턴스가 아니라 클래스 이름으로 호출할 수 있습니다.
   - 클래스 인스턴스를 생성하지 않아도 됩니다. 이는 정적 메서드에 this를 사용할 수 없다는 것입니다.
    ```js
    class NamedClass {
      constructor(prop) {
        this.prop = prop;
      }

      static staticMethod() {
        // 정적 메서드는 this를 사용할 수 없습니다.
        // 정적 메서드 내부에서 this는 클래스의 인스턴스가 아닌 클래스 자신을 가리킵니다.
        return 'staticMethod';
      }

      prototypeMethod() {
        return this.prop;
      }
    }

    // 정적 메서드는 클래스 이름으로 호출합니다.
    console.log(NamedClass.staticMethod());

    const name = new NamedClass(123);
    // 정적 메서드는 인스턴스로 호출할 수 없습니다.
    console.log(name.staticMethod()); // Uncaught TypeError: NamedClass.staticMethod is not a function
    ```
    - 이유는 기존 프로토타입 기반의 문법적 설탕이 클래스이기 때문입니다. 위 예제를 ES5로 표현해 볼까요?
      ```js
      var NamedClass = (function () {
        // 생성자 함수
        function NamedClass(prop) {
          this.prop = prop;
        }

        NamedClass.staticMethod = function () {
          return 'staticMethod';
        };

        NamedClass.prototype.prototypeMethod = function () {
          return this.prop;
        };

        return NamedClass;
      }());

      var name = new NamedClass(123);
      console.log(name.prototypeMethod());    // 123
      console.log(NamedClass.staticMethod()); // staticMethod
      console.log(name.staticMethod());       // Uncaught TypeError: name.staticMethod is not a function
      ```
    - prototype 프로퍼티는 함수 객체가 생성자로 사용될 때, 이 함수를 통해 생성된 객체의 부모 역할을 하는 프로토타입 객체를 가리키므로 **정적 메서드인 staticMethod는 생성자 함수 NamedClass의 메서드(함수는 객체이므로 메서드를 가질 수 있어요)이고, 일반 메서드인 prototypeMethod는 프로토타입 객체 NamedClass.prototype의 메서드입니다. 따라서 staticMethod는 name에서 호출할 수 없는 것이죠**!

<br>

9. extends 키워드로 클래스를 상속(Class Inheritance) 할 수 있습니다.
   - 오버라이딩(Overriding), 오버로딩(Overloading)이 프로토타입 체인에 의해 구현될 수 있습니다.
   - super() 키워드로 부모 클래스를 참조(Reference)하거나 부모 클래스의 constructor를 호출할 수 있습니다.
     - 자식 클래스에서 constructor를 선언하지 않으면 부모의 constructor를 바라봅니다.
     - 그러나 자식 클래스에서 constructor를 선언했음에도 super() 키워드를 사용하지 않는다면 참조 에러가 발생합니다.

super 키워드는 양이 많아 단원을 분리합니다.

<br>

## super 키워드
> super 키워드는 함수처럼 호출하거나 this와 같이 식별자처럼 참조할 수 있는 특수한 키워드입니다.

super 키워드는 아래와 같이 동작합니다.
1. super를 **호출**하면 슈퍼 클래스의 constructor()를 호출합니다.
2. super를 **참조**하면 슈퍼 클래스의 메서드를 호출할 수 있습니다.

자세히 알아볼까요?

<br>

### super 호출
> new 연산자와 함께 서브 클래스를 호출하면서 전달한 인수는 super 호출을 통해 슈퍼 클래스의 constructor()에 전달됩니다.

슈퍼 클래스에서 추가한 프로퍼티와 서브 클래스에서 추가한 프로퍼티를 갖는 인스턴스를 생성한다면 서브 클래스의 constructor를 생략할 수 없습니다.

또한 new 연산자와 함께 서브 클래스를 호출하면서 전달한 인수를 슈퍼 클래스의 constructor에 super 키워드를 통하여 전달할 수 있습니다.
```js
// 슈퍼 클래스
class Base {
  constructor(a, b) { // ④
    this.a = a;
    this.b = b;
  }
}

// 서브 클래스
class Derived extends Base {
  // 암묵적으로 constructor가 정의되지만 직접 입력할 수 있습니다.
  // constructor(...args) { super(...args); }
  constructor(a, b, c) {
    super(a, b);
    this.c = c;
  }
}

const derived = new Derived(1, 2, 3);
console.log(derived); // Derived {a: 1, b: 2, c: 3}
```

이 때 주의사항은 아래와 같습니다.
1. 서브 클래스에서 constructor를 생략하지 않는 경우, 서브 클래스의 constructor에선 반드시 super를 호출해야 합니다.
    ```js
    class Base {}

    class Derived extends Base {
      constructor() {
        // ReferenceError: Must call super constructor in derived class before accessing 'this' or returning from derived constructor
        console.log('constructor call');
      }
    }

    const derived = new Derived();
    ```

<br>

2. 서브 클래스의 constructor에서 super를 호출하기 전에 this를 참조할 수 없습니다.
    ```js
    class Base {}

    class Derived extends Base {
      constructor() {
        // ReferenceError: Must call super constructor in derived class before accessing 'this' or returning from derived constructor
        this.a = 1;
        super();
      }
    }

    const derived = new Derived(1);
    ```

<br>

3. super는 반드시 서브 클래스의 constructor에서만 호출해야 합니다. 서브 클래스가 아닌 클래스의 constructor나 함수에서 super를 호출하면 에러가 발생해요.
    ```js
    // 슈퍼 클래스
    class Base {
      constructor(name) {
        this.name = name;
      }

      sayHi() {
        return `Hi! ${this.name}`;
      }
    }

    // 서브 클래스
    class Derived extends Base {
      sayHi() {
        // super.sayHi는 슈퍼 클래스의 프로토타입 메서드를 가리킵니다.
        return `${super.sayHi()}. how are you doing?`;
      }
    }

    const derived = new Derived('Lee');
    console.log(derived.sayHi()); // Hi! Lee. how are you doing?
    ```

<br>

### super 참조
> super는 자심을 참조하고 있는 메서드가 바인딩된 객체의 프로토타입을 가리킵니다.

super를 참조로 사용하는 형태들을 볼까요?

- 서브 클래스의 프로토타입 메서드 내에서 `super.메서드`는 슈퍼 클래스의 프로토타입 메서드를 가리킵니다.
  ```js
  // 슈퍼 클래스
  class Base {
    constructor(name) {
      this.name = name;
    }

    sayHi() {
      return `Hi! ${this.name}`;
    }
  }

  // 서브 클래스
  class Derived extends Base {
    sayHi() {
      // super.sayHi는 슈퍼 클래스의 프로토타입 메서드를 가리킵니다.
      return `${super.sayHi()}. how are you doing?`;
    }
  }

  const derived = new Derived('Lee');
  console.log(derived.sayHi()); // Hi! Lee. how are you doing?
  ```
  - 단, super가 슈퍼 클래스의 메서드가 바인딩 된 객체인 슈퍼 클래스의 prototype 프로퍼티에 바인딩된 프로토타입을 참조할 수 있어야 합니다. 아래 처럼요.
    ```js
    // 슈퍼 클래스
    class Base {
      constructor(name) {
        this.name = name;
      }

      sayHi() {
        return `Hi! ${this.name}`;
      }
    }

    class Derived extends Base {
      sayHi() {
        // __super는 Base.prototype을 가리킵니다.
        const __super = Object.getPrototypeOf(Derived.prototype);
        return `${__super.sayHi.call(this)} how are you doing?`;
      }
    }
    ```
  - 이렇게 동작하기 위해 메서드는 내부 슬롯 `[[HomeObject]]`를 가지며 자신을 바인딩하고 있는 객체를 가리킵니다.
    - 단, ES6의 메서드 축약 표현으로 정의된 함수만 `[[HomeObject]]`를 갖습니다.
      ```js
      const obj = {
        // [[HomeObject]]를 갖습니다.
        foo() {},
        // [[HomeObject]]를 갖지 않습니다.
        bar: function () {}
      };
      ```

  <br>

  > 결국 super 참조를 의사 코드로 표현하면 아래와 같습니다.
  ```js
  super = Object.getPrototypeOf([[HomeObject]])
  ```
  1. `[[HomeObject]]`는 메서드 자신을 바인딩하고 있는 객체를 가리킵니다.
  2. `[[HomeObject]]`를 통해 메서드 자신을 바인딩하고 있는 객체의 프로토타입을 찾을 수 있습니다.
  3. 예로 들자면, Derived 클래스의 sayHi 메서드는 Derived.prototype에 바인딩되어 있습니다.
     - 따라서 Derived 클래스의 sayHi 메서드의 `[[HomeObject]]`는 Derived.prototype이고,     이를 통해 Derived 클래스의 sayHi 메서드 내부의 super 참조가 Base.prototype으로 결정됩니다.
     - 최종적으로 super.sayHi는 Base.prototype.sayHi를 가리키게 돼죠.

<br>

- 서브 클래의 정적 메서드 내에서 `super.메서드`는 슈퍼 클래스의 정적 메서드를 가리킵니다.
  ```js
  // 슈퍼 클래스
  class Base {
    static sayHi() {
      return 'Hi!';
    }
  }

  // 서브 클래스
  class Derived extends Base {
    static sayHi() {
      // super.sayHi는 슈퍼 클래스의 정적 메서드를 가리킵니다.
      return `${super.sayHi()} how are you doing?`;
    }
  }

  console.log(Derived.sayHi()); // Hi! how are you doing?
  ```

<br>

### 상속 클래스의 인스턴스 생성 과정
> 클래스가 단독으로 인스턴스를 생성하는 과정보다 상속을 통해 인스턴스를 생성하는 과정이 더 복잡합니다.

서브 클래스가 new 연산자와 함께 호출되면 아래의 과정을 통해 인스턴스를 생성합니다.

- **서브 클래스의 super 호출**
  - 자바스크립트 엔진은 클래스 평가 시 슈퍼 클래스와 서브 클래스를 구분하기 위해 `base` 또는 `derived`를 값으로 갖는 내부 슬롯 `[[ConstructorKind]]`를 갖습니다.
  - 다른 클래스를 상속받지 않는 클래스는 내부 슬롯 `[[ConstructorKind]]`의 값이 `base`인 반면, 다른 클래스를 상속받는 클래스는 `derived`로 설정되며 이를 통해 동작이 구분됩니다.
  - 상속받는 클래스가 new 연산자와 함께 호출되면 자신이 직접 인스턴스를 생성하지 않고 슈퍼 클래스에 인스턴스 생성을 위임합니다. 이게 바로 서브 클래스의 constructor에서 반드시 super를 호출해야 하는 이유죠.
  - super가 호출되면 슈퍼 클래스의 constructor가 호출됩니다. 서브 클래스의 constructor 내부에 super 호출이 없다면 에러가 발생하는 이유는 실제 인스턴스를 생성하는 주체는 슈퍼 클래스이므로, 슈퍼 클래스의 constructor가 호출되지 않으면 인스턴스를 생성할 수 없기 때문이죠.

<br>

- **슈퍼 클래스의 인스턴스 생성과 this 바인딩**
  - 슈퍼 클래스의 constructor 내부의 코드가 실행되기 이전에 암묵적으로 빈 객체를 생성하는데, 이 빈 객체가 클래스의 인스턴스입니다. 그리고 암묵적으로 생성된 빈 객체는 this에 바인딩 되죠. 결국 슈퍼 클래스의 constructor 내부의 this는 생성된 this를 가리키게 됩니다.
  - 이 때 인스턴스는 슈퍼 클래스가 생성한 것이지만, new 연산자와 함게 호출된 클래스는 서브 클래스입니다. 즉, new 연산자와 함께 호출된 함수를 가리키는 `new.target`은 서브 클래스를 가리키며, 인스턴스는 `new.target`이 가리키는 서브 클래스가 생성한 것으로 처리됩니다.
  - 결국 생성된 인스턴스의 프로토타입은 슈퍼 클래스의 prototype 프로퍼티가 가리키는 객체가 아니라 `new.target`이 가리키는 서브 클래스의 prototype 프로퍼티의 객체가 되는 것이죠.

<br>

- **슈퍼 클래스의 인스턴스 초기화**
  - 슈퍼 클래스의 constructor가 실행되고 this에 바인딩된 인스턴스를 초기화합니다.

<br>

- **서브 클래스 constructor로의 복귀와 this 바인딩**
  - super 호출이 종료되고 제어 흐름이 서브 클래스로 돌아오면 super가 반환한 인스턴스가 this에 바인딩됩니다.
  - 서브 클래스는 별도의 인스턴스를 생성하지 않고 super가 반환한 인스턴스를 this에 바인딩하여 그대로 사용합니다.
  - 이처럼 super가 호출되지 않으면 인스턴스가 생성되기는 커녕 this 바인딩도 이루어지지 않습니다. 서브 클래스의 constructor에서 super를 호출하기 전에는 this를 참조할 수 없는 이유죠.

<br>

- **서브클래스의 인스턴스 초기화**
  - 서브 클래스의 constructor가 실행되고 this에 바인딩된 인스턴스에 프로퍼티를 추가합니다.

<br>

- **인스턴스 반환**
  - 클래스의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환됩니다.

<hr>
<br>