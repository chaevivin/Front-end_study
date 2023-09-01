# 함수와 일급 객체

## 1. 일급 객체
다음과 같은 조건을 만족하는 객체를 일급 객체라 한다.
1. 무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성이 가능하다.
2. 변수나 자료구조(객체, 배열 등)에 저장할 수 있다.
3. 함수의 매개변수에 전달할 수 있다.
4. 함수의 반환값으로 사용할 수 있다.

<br>

자바스크립트 함수는 위의 조건을 모두 만족하므로 일급 객체다.
```js
// 1. 함수는 무명의 리터럴로 생성할 수 있다.
// 2. 함수는 변수에 저장할 수 있다.
// 런타임(할당 단계)에 함수 리터럴이 평가되어 함수 객체가 생성되고 변수에 할당
const increase = function (num) {
  return ++num;
}

const decrease = function (num) {
  return --num;
}

// 2. 함수는 객체에 저장할 수 있다.
const auxs = { increase, decrease };

// 3. 함수의 매개변수에 전달할 수 있다.
// 4. 함수의 반환값으로 사용할 수 있다.
function makeCounter(aux) {
  let num = 0;

  return function () {
    num = aux(num);
    return num;
  };
}

// 3. 함수는 매개변수에 함수를 전달할 수 있다.
const increaser = makeCounter(auxs.increase);
console.log(increaser());    // 1

const decreaser = makeCounter(auxs.decrease);
console.log(decreaser());    // -1
```

<br>

- 자바스크립트 함수는 일급 객체 = 함수를 객체처럼 사용 가능 
  - 객체는 값이므로 함수를 값처럼 사용 가능
  - 값으로 사용할 수 있는 곳(변수 할당문, 객체의 프로퍼티 값, 배열의 요소, 함수 호출의 인수, 함수 반환문)에서 리터럴로 정의 가능
  - 런타임에 함수 객체로 평가
  - 일반 객체처럼 함수의 매개변수에 함수 전달 가능 -> 함수형 프로그래밍
  - 함수를 함수의 반환값으로 사용 가능 -> 함수형 프로그래밍
- 일반 객체와 함수의 다른 점
  - 일반 객체는 호출할 수 없지만 함수는 호출 가능
  - 함수 객체는 일반 객체에는 없는 함수 고유의 프로퍼티 소유

<br>
<br>

## 2. 함수 객체의 프로퍼티
함수는 객체이기 때문에 프로퍼티를 가질 수 있다. 
```js
function square(number) {
  return number * number;
}

console.log(Object.getOwnPrototypeDescriptor(square));
/*
{
  length: {value: 1, writable: false, enumerable: false, configurable: true}
  name: {value: "square", writable: false, enumerable: false, configurable: true}
  arguments: {value: null, writable: false, enumerable: false, configurable: false}
  call: {value: null, writable: false, enumerable: false, configurable: false}
  prototype: {value: {...}, writable: true, enumerable: false, configurable: false}
}
*/

console.log(Object.getOwnPropertyDescriptor(square, '__proto__'));    // undefined
console.log(Object.getOwnPropertyDescriptor(Object.prototype, '__proto__'));    // {get: f, set: f, enumerable: false, configurable: true}
```
- 함수 객체의 고유한 데이터 프로퍼티
  - arguments, caller, length, name, prototype
- 함수 객체의 고유 프로퍼티 X -> **\_\_proto\_\_**
  - 접근자 프로퍼티
  - Object.prototype 객체의 프로퍼티를 상속
  - Object.prototype 객체의 \_\_proto\_\_ 접근자 프로퍼티는 모든 객체가 사용 가능

<br>

### 2.1. arguments 프로퍼티
arguments 프로퍼티 값은 arguments 객체다.
- 함수 호출 시 전달된 인수들의 정보를 담고 있음
- 순회 가능한 유사 배열 객체
- 함수 내부에서 지역 변수처럼 사용
- 함수 외부에서 참조 불가능

<br>

```js
function multiply(x, y) {
  console.log(arguments);
  return x * y;
}

console.log(multiply());    // NaN
console.log(multiply(1));    // NaN
console.log(multiply(1, 2));    // 2
console.log(multiply(1, 2, 3));    // 2
```
매개변수는 함수 몸체 내부에서 변수와 동일하게 취급된다. 즉, 함수가 호출되면 함수 몸체 내에서 암묵적으로 매개변수가 선언되고 undefined로 초기화된 인수가 할당된다.
- 선언된 매개변수 개수보다 인수를 적게 전달했을 경우
  - 자바스크립트는 매개변수 개수와 인수 개수가 일치하는지 확인하지 않음
  - 인수가 전달되지 않은 매개변수는 undefined로 초기화
- 선언된 매개변수 개수보다 인수를 더 많이 전달한 경우
  - 초과된 인수는 무시
  - **초과된 인수는 버리지 않고 암묵적으로 arguments 객체의 프로퍼티로 보관**

<br>

arguments 객체는 매개변수 개수를 확정할 수 없는 **가변 인자 함수**를 구현할 때 유용하다. arguments는 유사 배열 객체이기 때문에 for문으로 순회가 가능하다.
```js
function sum() {
  let res = 0;

  for (let i = 0; i < arguments.length; i++) {
    res += arguments[i];
  }

  return res;
}

console.log(sum());    // 0
console.log(sum(1, 2));    // 3
console.log(sum(1, 2, 3));    // 6
```

<br>

유사 배열 객체는 배열이 아니므로 배열 메서드를 사용할 경우 에러가 발생한다. 따라서 배열 메서드를 사용하려면 Function.prototype.call, Function.prototype.apply를 사용해 간접 호출해야 한다.
```js
function sum() {
  // arguments 객체를 배열로 변환
  const array = Array.prototype.slice.call(arguments);
  return array.reduce(function (pre, cur) {
    return pre + cur;
  }, 0);
}

console.log(sum());    // 0
console.log(sum(1, 2));    // 3
console.log(sum(1, 2, 3));    // 6
```

<br>

배열 메서드를 따로 호출해야 하는 번거러움을 해결하기 위해 ES6에 Rest 파라미터를 도입했다.
```js
function sum(...args) {
  return args.reduce((pre, cur) => pre + cur, 0);
}

console.log(sum());    // 0
console.log(sum(1, 2));    // 3
console.log(sum(1, 2, 3));    // 6
```

<br>

### 2.2. length 프로퍼티
length 프로퍼티는 함수를 정의할 때 선언한 매개변수의 개수를 가리킨다.
```js
function foo() {}
console.log(foo.length);    // 0

function bar(x) {
  return x;
}
console.log(bar.length);    // 1

function baz(x, y) {
  return x * y;
}
console.log(baz.length);    // 2
```
- arguments 객체의 length 프로퍼티는 인자의 개수
- 함수 객체의 length 프로퍼티는 매개변수의 개수

<br>

### 2.3. name 프로퍼티
name 프로퍼티는 함수 이름을 나타낸다.
- ES5에서 name 프로퍼티는 빈 문자열을 값을 갖는다.
- ES6에서 name 프로퍼티는 함수 객체를 가리키는 식별자를 값으로 갖는다.
```js
// 기명 함수 표현식
var namedFunc = function foo() {};
console.log(namedFunc.name);    // foo

// 익명 함수 표현식
var annonymousFunc = function() {};
console.log(annonymousFunc.name);    // annonymousFunc

// 함수 선언문
function bar() {}
console.log(bar.name);    // bar
```

<br>

### 2.4. \_\_proto\_\_ 접근자 프로퍼티
모든 객체는 [[Prototype]]이라는 내부 슬롯을 갖는다. [[Prototype]] 내부 슬롯은 객체지향 프로그래밍의 상속을 구현하는 프로토타입 객체를 가리킨다.

<br>

\_\_proto\_\_ 프로퍼티는 [[Prototype]] 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티다.
- 내부 슬롯에는 직접 접근 불가
- 간접적으로 접근 가능
```js
const obj = { a: 1 };

console.log(obj.__proto__ === Object.prototype);    // true

console.log(obj.hasOwnProperty('a'));    // true
console.log(obj.hasOwnProperty('__proto__'));    // false
```
위의 코드를 살펴보면,
- 객체 리터럴 방식으로 생성한 객체의 프로토타입 객체는 Object.prototype이다.
- 객체 리터럴 방식으로 생성한 객체는 프로토타입 객체인 Object.prototype의 프로퍼티를 상속받는다.
- hasOwnProperty 메서드는 Object.prototype의 메서드다.
- hasOwnProperty 메서드는 인수로 전달받은 프로퍼티 키가 객체 고유의 프로퍼티 키인 경우에만 true, 상속받은 프로토타입 키인 경우 false

<br>

### 2.5. prototype 프로퍼티
prototype 프로퍼티는 생성자 함수로 호출할 수 있는 함수 객체, 즉 constructor만이 소유하는 프로퍼티다.
```js
(function () {}).hasOwnProperty('prototype');    // true

({}).hasOwnProperty('property');     // false
```
위의 코드를 살펴보면,
- 함수 객체는 prototype 프로퍼티를 소유한다.
- 일반 객체는 prototype 프로퍼티를 소유하지 않는다.
- prototype 프로퍼티는 생성자 함수로 호출될 때 생성자 함수가 생성할 인스턴스의 프로토타입 객체를 가리킨다.

<br>
<br>