# Symbol

## 1. 심벌이란?
- ES6에 도입된 7번째 데이터 타입으로 변경 불가능한 원시 타입의 값
- 심벌 값은 다은 값과 중복되지 않는 유일무이한 값
- 주로 이름의 충돌 위험이 없는 유일한 프로퍼티 키를 만들기 위해 사용
- 하휘 호환성을 보장하기 위해 도입

<br>
<br>

## 2. 심벌 값의 생성

<br>

### 2.1. Symbol 함수
- 심벌 값은 Symbol 함수를 호출하여 생성
- 문자열, 숫자, 불리언, undefined, null 타입처럼 다른 원시값은 리터럴 표기법을 통해 값을 생성
- 생성된 심벌 값은 외부로 노출되지 않아 확인 불가, **다른 값과 절대 중복되지 않는 유일무의한 값**
  ```js
  const mySymbol = Symbol();
  console.log(typeof mySymbol);    // symbol

  console.log(mySymbol);    // Symbol()
  ```
- Symbol 함수는 String, Number, Boolean 생성자 함수와 달리 new 연산자와 함께 호출 X
  - new 연산자와 함께 생성자 함수 또는 클래스를 호출하면 객체(인스턴스)가 생성되지만 심벌 값은 변경 불가능한 원시 값
  ```js
  new Symbol();    // TypeError: Symbol is not a constructor
  ```
- Symbol 함수에는 선택적으로 문자열을 인수로 전달 가능
  - 이 문자열은 생성된 심벌 값에 대한 설명으로 디버깅 용도로만 사용되며, 심벌 값 생성에 영향 X
  - 심벌 값에 대한 설명이 같더라도 심벌 값은 유일무이한 값
  ```js
  const mySymbol1 = Symbol('mySymbol');
  const mySymbol2 = Symbol('mySymbol');

  console.log(mySymbol1 === mySymbol2);    // false
  ```
- 심벌 값도 문자열, 숫자, 불리언과 같이 객체처럼 접근하면 암묵적으로 래퍼 객체 생성
  ```js
  const mySymbol = Symbol('mySymbol');

  console.log(mySymbol.description);    // mySymbol
  console.log(mySymbol.toString());    // Symbol(mySymbol)
  ```
- 심벌 값은 암묵적으로 문자열이나 숫자 타입으로 변환 X
  ```js
  const mySymbol = Symbol();

  console.log(mySymbol + '');    // TypeError: Cannot convert a Symbol value to a string
  console.log(+mySymbol);    // TypeError: Cannot convert a Symbol value to a string
  ```
- 불리언 타입으로는 암묵적 타입 변환 가능
  - if문 등에서 존재 확인 가능
  ```js
  const mySymbol = Symbol();

  console.log(!!mySymbol);    // true

  if (mySymbol) console.log('mySymbol is not empty');
  ```

<br>

### 2.2. Symbol.for / Symbol.keyFor 메섣,
- Symbol.for 메서드는 인수로 전달받은 문자열을 키로 사용하여 키와 심벌 값의 쌍들이 저장되어 있는 전역 심벌 레지스트리에서 해당 키와 일치하는 심벌 값을 검색
  - 검색에 성공하면 새로운 심벌 값을 생성하지 않고 검색된 심벌 값을 반환
  - 검색에 실패하면 새로운 심벌 값을 생성하여 Symbol.for 메서드의 인수로 전달된 키로 전역 심벌 레지스트리에 저장한 후, 생성된 심벌 값 반환
  ```js
  const s1 = Symbol.for('mySymbol');
  const s2 = Symbol.for('mySymbol');

  console.log(s1 === s2);    // true
  ```
- Symbol 함수는 호출될 때마다 유일무이한 심벌 값을 생성하지만 자바스크립트 엔진이 관리하는 심벌 값 저장소인 전역 심벌 레지스트리에서 심벌 값을 검색할 수 있는 키를 지정할 수 없으므로 전역 심벌 레지스트리에 등록되어 관리 X
- Symbol.for 메서드를 사용하면 애플리케이션 전역에서 중복되지 않는 유일무이한 상수인 심벌 값을 단 하나만 생성하여 전역 심벌 레지스트리를 통해 공유 가능
- Symbol.keyFor 메서드를 사용하면 전역 심벌 레지스트리에 저장된 심벌 값의 키 추출 가능
  ```js
  const s1 = Symbol.for('mySymbol');
  Symbol.keyFor(s1);    // mySymbol

  const s2 = Symbol('foo');
  Symbol.keyFor(s2);    // undefined
  ```

<br>
<br>

## 3. 심벌과 상수
- 위, 아래, 왼쪽, 오른쪽을 나타내는 상수를 정의
  ```js
  const Direction = {
    UP: 1,
    DOWN: 2,
    LEFT: 3,
    RIGHT: 4
  };

  // 변수에 상수 할당
  const myDirection = Direction.UP;

  if (myDirection === Direction.UP) {
    console.log('You are going UP.');
  }
  ```
  - 문제는 상수 값 1, 2, 3, 4가 변경될 수 있으며, 다른 변수 값과 중복될 수도 있음
  - 이러한 경우 변경/중복될 가능성이 있는 무의미한 상수 대신 중복될 가능성이 없는 유일무이한 심벌 값을 사용
  ```js
  const Direction = {
    UP: Symbol('up'),
    DOWN: Symbol('down'),
    LEFT: Symbol('left'),
    RIGHT: Symbol('right')
  };

  // 변수에 상수 할당
  const myDirection = Direction.UP;

  if (myDirection === Direction.UP) {
    console.log('You are going UP.');
  }
  ```

<br>
<br>

## 4. 심벌과 프로퍼티 키
- 객체의 프로퍼티 키는 빈 문자열을 포함하는 모든 문자열 또는 심벌 값으로 만들 수 있으며, 동적으로 생성 가능 
- 심벌 값을 프로퍼티 키로 사용하려면 프로퍼티 키로 사용할 심벌 값에 대괄호 사용
- 프로퍼티에 접근할 때도 대괄호를 사용
  ```js
  const obj = {
    [Symbol.for('mySymbol')]: 1
  };

  obj[Symbol.for('mySymbol')];    // 1
  ```
- **심벌 값은 유일무이한 값이므로 심벌 값으로 프로퍼티 키를 만들면 다른 프로퍼티 키와 절대 충돌 X**
  - 기존 프로퍼티 키와 충돌하지 않는 것은 물론, 미래에 추가될 어떤 프로퍼티 키와도 충돌할 위험 X

<br>
<br>

## 5. 심벌과 프로퍼티 은닉
- 심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티는 for ... in 문이나 Object.keys, Object.getOwnPropertyNames 메서드로 찾기 불가능
- 심벌 값을 프로퍼티 키로 사용하여 프로퍼티를 생성하면 외부에 노출할 필요가 없는 프로퍼티 은닉 가능
  ```js
  const obj = {
    [Symbol('mySymbol')]: 1
  };

  for (const key in obj) {
    console.log(key);    // 아무것도 출력되지 않는다.
  }

  console.log(Object.keys(obj));    // []
  console.log(Object.getOwnPropertyNames(obj));    // []
  ```
- ES6에서 도입된 Object.getOwnPropertySymbols 메서드를 사용하면 심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티 찾기 가능
  ```js
  const obj = {
    [Symbol('mySymbol')]: 1
  };

  console.log(Object.getOwnPropertySymbols(obj));    // [Symbol(mySymbol)]

  // getOwnPropertySymbols 메서드로 심벌 값도 찾을 수 있다
  const symbolKey1 = Object.getOwnPropertySymbols(obj)[0];
  console.log(obj[symbolKey1]);    // 1
  ```

<br>
<br>

## 6. 심벌과 표준 빌트인 객체 확장
- 일반적으로 표준 빌트인 객체에 사용자 정의 메서드를 직접 추가하여 확장하는 것은 권장 X -> 표준 빌트인 객체는 읽기 전용으로 사용
  ```js
  // 권장 X
  Array.prototype.sum = function () {
    return this.reduce((acc, cur) => acc + cur, 0);
  };

  [1, 2].sum();    // 3
  ```
  - 이유는 개발자가 직접 추가한 메서드와 미래 표준 사양으로 추가될 메서드의 이름 중복 가능성 때문
- 중복될 가능성이 없는 심벌 값으로 프로퍼티 키를 생성하여 표준 빌트인 객체를 확장했을 때 장점
  - 표준 빌트인 객체의 기존 프로퍼티 키와 충돌하지 않음
  - 표준 사양의 버전이 올라감에 따라 추가될지 모르는 어떤 프로퍼티 키와도 충돌할 위험이 없어 안전하게 객체 확장 가능
  ```js
  // 권장
  Array.prototype[Symbol.for('sum')] = function () {
    return this.reduce((acc, cur) => acc + cur, 0);
  };

  [1, 2][symbol.for('sum')]();    // 3
  ```

<br>
<br>

## 7. Well-known Symbol
- 자바스크립트가 기본 제공하는 빌트인 심벌 값 존재
  - ECMAScript 사양에서는 **Well-known Symbol**이라 부름
  - 빌트인 심벌 값은 Symbol 함수의 프로퍼티에 할당
- Well-known Symbol은 자바스크립트 엔진의 내부 알고리즘에 사용
- 예를 들어, Array, String, Map, Set, TypedArray, arguments, NodeList, HTMLCollection과 같이 for ... of문으로 순회 가능한 빌트인 이터러블
  - Well-known Symbol인 Symbol.iterator를 키로 갖는 메서드를 가짐
  - Symbol.iterator를 호출하면 이터레이터를 반환
  - 빌트인 이터러블은 이터레이션 프로토콜 준수
- 만약 빌트인 이터러블이 아닌 일반 객체를 이터러블처럼 동작하도록 구현하고 싶다면 이터레이션 프로토콜을 따르면 됨
  - ECMAScript 사양에 규정되어 있는 대로 Well-known Symbol인 Symbol.iterator를 키로 갖는 메서드를 객체에 추가하고 이터레이터를 반환하도록 구현하면 그 객체는 이터러블
  ```js
  // 1 ~ 5 범위의 정수로 이루어진 이터러블
  const iterable= {
    [Symbol.iterator]() {
      let cur = 1;
      const max = 5;
      // Symbol.iterator 메서드는 next 메서드를 소유한 이터레이터를 반환
      return {
        next() {
          return { value: cur++, done: cur > max + 1 };
        }
      };
    }
  };

  for (const num of iterable) {
    console.log(num);    // 1 2 3 4 5
  }
  ```
- 이터레이션 프로토콜을 준수하기 위해 일반 객체에 추가해야 하는 메서드의 키 symbol.iteraotr는 기존 프로퍼티 키 또는 미래에 추가될 프로퍼티 키와 중복 X

<br>
<br>