자바스크립트는 왜 이렇게 다른 언어들과 다르게 작동할까요? 그 원인 중 하나를 자세히 살펴봅니다!

## 스코프(Scope)
> 변수의 생존범위를 말하며, 여러 규칙에 따라 동작합니다.

스코프는 자바스크립트의 코드 범위를 표현하는 단어이며, 전역과 지역으로 나뉩니다.

### 전역 스코프(Global Scope)
> 어떤 블록으로도 감싸져 있지 않은 경우, 전역 스코프 안에 존재합니다.

제어문, 함수는 중괄호(`{}`)로 표기합니다. 즉 중괄호 밖에 있는 모든 변수는 **전역 변수**로서 존재하게 됩니다.

### 지역 스코프(Local Scope)
> 어떤 블록으로 감싸져 있는 경우, 지역 스코프 안에 존재합니다.

중괄호 안에 있는 모든 변수는 **지역 변수**로서 존재하게 됩니다. 여기가 조금 아리송한데요, 지역 스코프에는 **함수 레벨 스코프(var)** 와 **블록 레벨 스코프(let, const)** 가 존재합니다.

- 함수 레벨 스코프(Function Level Scope)
    ```js
    var a = 1;
    function change(){
      var a = 10;
      console.log(a);
    }

    change();       // 10
    console.log(a); // 10
    ```
    - `var` 키워드의 특징으로, 함수 밖에서 선언한 함수 레벨 스코프 변수는 전역 범위를 가지며 함수 밖을 제외한 어디서든 접근할 수 있습니다.
    - 이는 결국 `메모리 누수`, `어려운 디버깅` 등의 문제점을 가지고 있으며, 블록 레벨 스코프가 탄생하는 원인이 되죠.

- 블록 레벨 스코프(Block Level Scope)
    ```js
    let a = 1;
    function change(){
      let a = 10;
      console.log(a);
    }

    change();       // 10
    console.log(a); // 1
    ```

<br>
<hr>
<br>

> 2021-09-11, <a href="https://github.com/karohani">karohani</a>와 스터디 중 식별된 추가적으로 공부가 필요한 내용들

## :white_check_mark: this가 바인딩 되는 시점은 렉시컬 스코프 정의와 다르다?
### 의문점
- 자바스크립트의 렉시컬 스코프(Lexical Scope, Lexical Envi)는 **선언된 시점**에서 스코프를 갖는데, `this`는... 다른가요? 아래 예제 코드를 보죠!

```js
const whatThis1 = function(){
  console.log(this);
}
const obj = {
  whatThis2: function(){
    console.log(this);
  }
}

console.log(whatThis2());     // Window
console.log(obj.whatThis2()); // {whatThis2: ƒ}
```

### 정리 된 내용
- *정리해야함*

- 참조 [Link](https://velog.io/@woobuntu/%EC%8B%A4%ED%96%89-%EC%BB%A8%ED%85%8D%EC%8A%A4%ED%8A%B8%EB%A0%89%EC%8B%9C%EC%BB%AC-%ED%99%98%EA%B2%BD%EA%B3%BC-this)

<hr>
<br>