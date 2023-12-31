# strict mode

## 1. strict mode란?
```js
function foo() {
  x = 10;
}
foo();

console.log(x);
```
위의 코드를 살펴보면,
- foo 함수 내에서 선언하지 않은 x 변수에 값 10을 할당
- 자바스크립트 엔진은 스코프 체인을 통해 foo 함수의 스코프에서 x 변수 선언 검색 -> 실패
- 자바스크립트 엔진은 전역 스코프에서 x 변수 선언 검색 -> 실패
- **암묵적 전역** : 자바스크립트 엔진은 암묵적으로 전역 객체에 x 프로퍼티를 동적 생성하고 전역 객체의 x 프로퍼티는 전역 변수처럼 사용 가능

<br>

암묵적 전역은 개발자 의도와 상관없이 발생했기 때문에 오류를 발생시키는 원인이 될 가능성이 크다. 오류를 줄여 안정적인 코드를 생산하기 위해 ES5부터 strict mode가 추가되었다.

자바스크립트 언어의 문법을 좀 더 엄격히 적용하여 오류를 발생시킬 가능성이 높거나 자바스크립트 엔진의 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생시킨다.

ESLint 같은 린트 도구를 사용해도 strict mode와 유사한 효과를 얻을 수 있다. 린트 도구는 정적 분석 기능을 통해 소스코드를 실행하기 전에 소스코드를 스캔하여 문법적 오류만이 아니라 잠재적 오류까지 찾아내고 오류의 원인을 리프팅해주는 도구다.

<br>
<br>

## 2. strict mode의 적용
strict mode를 적용하려면 전역의 선두 또는 함수 몸체의 선두에 'use strict';를 추가한다. 
```js
'use strict';

function foo() {
  x = 10;    // ReferenceError: x is not defined
}
foo();
```
- 전역의 선두에 추가하면 스크립트 전체에 strict mode 적용
```js
function foo() {
  'use strict';

  x = 10;    // ReferenceError: x is not defined
}
foo();
```
- 함수 몸체의 선두에 추가하면 해당 함수와 중첩 함수에 strict mode 적용
```js
function foo() {
  x = 10;    // 에러 발생 X
  'use strict';
}
foo();
```
- 코드의 선두에 위치시키지 않으면 strict mode 제대로 동작 X

<br>
<br>

## 3. 전역에 strict mode를 적용하는 것은 피하자
전역에 적용한 strict mode는 스크립트 단위로 적용된다.
```html
<!DOCTYPE html>
<html>
<body>
    <script>
      'use strict';
    </script>
    <script>
      x = 1;    // 에러 발생 X
      console.log(x);    // 1
    </script>
    <script>
      'use strict';

      y = 1;    // ReferenceError: y is not defiend
    </script>
</body>
</html>
```
스크립트 단위로 적용된 strict mode는 다른 스크립트에 영향을 주지 않고 해당 스크립트에 한정되어 적용된다.

strict mode와 non-strict mode 스크립트를 혼용하면 오류를 발생시킬 수 있다. 이러한 경우 즉시 실행 함수로 스크립트 전체를 감싸서 스코프를 구분하고 즉시 실행 함수 선두에 strict mode를 적용한다.
```js
(function () {
  'use strict';

  // ...
}());
```

<br>
<br>

## 4. 함수 단위로 strict mode를 적용하는 것도 피하자
어떤 함수는 strict mode를 적용하고 어떤 함수는 strict mode를 적용하지 않는 것은 바람직하지 않으며 모든 함수에 일일이 strict mode를 적용하는 것은 번거로운 일이다. 그리고 strict mode가 적용된 함수가 참조할 함수 외부의 컨텍스트에 strict mode를 적용하지 않는다면 이 또한 문제가 발생할 수 있다.
```js
(function () {
  // non-strict mode
  var let = 10;    // 에러 발생 X

  function foo() {
    'use strict';

    let = 20;    // SyntaxError: Unexpected strict mode reserved word
  }
  foo();
}());
```
**따라서 strict mode는 즉시 실행 함수로 감싼 스크립트 단위로 적용하는 것이 바람직하다.**

<br>
<br>

## 5. strict mode가 발생시키는 에러

<br>

### 5.1. 암묵적 전역
선언하지 않은 변수를 참조하면 ReferenceError 발생
```js
(function () {
  'use strict';

  x = 1;
  console.log(x);    // ReferenceError: x is not defined
}());
```

<br>

### 5.2. 변수, 함수, 매개변수의 삭제
delete 연산자로 변수, 함수, 매개변수를 삭제하면 SyntaxError가 발생
```js
(function () {
  'use strict';

  var x = 1;
  delete x;    // SyntaxError: Delete of an unqualified identifier in strict mode.

  function foo(a) {
    delete a;    // SyntaxError: Delete of an unqualified identifier in strict mode.
  }
  delete foo;    // SyntaxError: Delete of an unqualified identifier in strict mode.
}());
```

<br>

### 5.3. 매개변수 이름 중복
중복된 매개변수 이름을 사용하면 SyntaxError 발생
```js
(function () {
  'use strict';

  // SyntaxError: Duplicate parameter name not allowed in this context
  function foo(x, x) {
    return x + x;
  }
  console.log(foo(1, 2));
}());
```

<br>

### 5.4. with문의 사용
with문을 사용하면 SyntaxError가 발생
- with문은 전달된 객체를 스코프 체인에 추가
- with문은 동일한 객체의 프로퍼티를 반복해서 사용할 때 객체 이름을 생략할 수 있어서 코드가 간단해지는 효과
- with문은 성능과 가독성이 나빠지는 문제점 -> 사용 권장 X
```js
(function () {
  'use strict';

  // SyntaxError: Strict mode code may not inclued a with statement
  with({ x: 1 }) {
    console.log(x);
  }
}());
```

<br>
<br>

## 6. strict mode 적용에 의한 변화

<br>

### 6.1. 일반 함수의 this
strict mode에서 함수를 일반 함수로서 호출하면 this에 undefined가 바인딩된다. 생성자 함수가 아닌 일반 함수 내부에서는 this를 사용할 필요가 없기 때문이다. 이때 에러는 발생하지 않는다.
```js
(function () {
  'use strict';

  function foo() {
    console.log(this);    // undefined
  }
  foo();

  function Foo() {
    console.log(this);    // Foo
  }
  new Foo();
}());
```

<br>

### 6.2. arguments 객체
strict mode에서는 매개변수에 전달된 인수를 재할당하여 변경해도 arguments 객체에 반영되지 않는다.
```js
(function (a) {
  'use strict';

  a = 2;
  console.log(arguments);    // { 0: 1. length: 1 }
}(1));
```

<br>
<br>