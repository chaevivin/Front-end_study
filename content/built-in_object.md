# 빌트인 객체

## 1. 자바스크립트 객체의 분류
- 표준 빌트인 객체 (standard built-in objects/native objects/ global objects) 
  - 표준 빌트인 객체는 ECMAScript 사양에 정의된 객체를 말하며 애플리케이션 전역의 공통 기능을 제공
  - 표준 빌트인 객체는 자바스크립트 실행 환경(브라우저나 Node.js 환경)과 관계없이 언제나 사용 가능
  - 전역 객체의 프로퍼티로 제공
  - 별도의 선언 없이 전역 변수처럼 언제나 참조 가능
- 호스트 객체 (host objects)
  - ECMAScript에 정의되어 있지 않지만 자바스크립트 환경 (브라우저나 Node.js 환경)에서 추가로 제공하는 객체
  - 브라우저 : DOM, BOM, Canvas, XMLHttpRequest, fetch, requestAnimationFrame, SVG, Web Storage, Web Component, Web Worker와 같은 클라이언트 사이드 Web API를 호스트 객체로 제공
  - Node.js : Node.js 고유의 API를 호스트 객체로 제공
- 사용자 정의 객체 (user-defined objects)
  - 표준 빌트인 객체와 호스트 객체처럼 기본 제공되는 객체가 아닌 사용자가 직접 정의한 객체

<br>
<br>

## 2. 표준 빌트인 객체 
- Object, String, Number, Boolean, Symbol, Date, Math, RegExp, Array, Map/Set, WeakMap, WeakSet, Function, Promise, Reflect, Proxy, JSON, Error 등 40여 개
- Math, Reflect, JSON 
  - 정적 메서드 제공
- Math, Reflect, JSON을 제외한 빌트인 객체
  - 인스턴스를 생성할 수 있는 생성자 함수 객체
  - 프로토타입 메서드와 정적 메서드를 제공
  - 빌트인 객체가 생성한 인스턴스의 프로토타입은 표준 빌트인 객체의 prototype 프로퍼티에 바인딩된 객체
```js
// String 객체 생성
const strObj = new String('Lee');    // String {"Lee"}
console.log(typeof strObj);    // object
console.log(Object.getPrototype(strObj) === String.prototype);    // true

// Number 객체 생성
const numObj = new Number(123);    // Number {123}
console.log(typeof numObj);    // object

// Boolean 객체 생성
const boolObj = new Boolean(true);    // Boolean {true}
console.log(typeof boolObj);     // object

// Function 객체 생성
const func = new Function('x', 'return x * x');    // f anonymois(x )
console.log(typeof func);    // function

// Array 객체 생성
const arr = new Array(1, 2, 3,);    // (3) [1, 2, 3]
console.log(typeof arr);    // object

// RegExp 객체 생성
const regExp = new RegExp(/ab+c/+i);    // /ab+c/i
console.log(typeof regExp);    // object

// Date 객체 생성
const date = new Date();    // Thr Jul 06 2023 15:24:25 GMT+0900 (대한민국 표준시)
cosole.log(typeof date);    // object
```

<br>

```js
// Number 객체 생성
const numObj = new Number(1.5);    // Number {1.5}

console.log(numObj.toFixed());    // 2
console.log(Number.isInteger(0.5));    // false
```
위의 코드를 살펴보면,
- toFixed는 Number.prototype의 프로토타입 메서드
  - Number.prototype.toFixed는 소수점 자리를 반올림하여 문자열로 반환
- isInteger는 Number의 정적 메서드
  - Number.isInteger는 인수가 정수인지 검사하여 그 결과를 Boolean으로 반환

<br>
<br>

## 3. 원시값과 래퍼 객체
```js
const str = 'hello';

console.log(str.length);    // 5
console.log(str.toUpperCase());    // HELLO
```
위의 코드를 살펴보면,
- 원시값은 객체가 아니므로 프로퍼티나 메서드를 가질 수 없음
- 하지만 원시값인 문자열이 마치 객체처럼 동작
  - 원시값에 대해 객체처럼 마침표 표기법 또는 대괄호 표기법으로 접근하면 자바스크립트 엔진이 일시적으로 연가된 객체로 변환
  - 원시값을 객체처럼 사용하면 자바스크립트 엔진은 암묵적으로 연관된 객체를 생성하여 생성된 객체로 프로퍼티에 접근하거나 메서드를 호출하고 다시 원시값으로 되돌림
- **래퍼 객체(wrapper object) : 문자열, 숫자, 불리언 값에 대해 객체처럼 접근하면 생성되는 임시 객체**

<br>

```js
const str = 'hi';

// ①
console.log(str.length);    // 2
console.log(str.toUpperCase());    // HI

// ②
console.log(typeof str);    // string
```
위의 코드를 살펴보면,
1. 문자열에 대해 마침표 표기법으로 접근 -> 래퍼 객체인 String 생성자 함수의 인스턴스 생성, 문자열은 래퍼 객체의 [[StringData]] 내부 슬롯에 할당
   - 문자열 래퍼 객체인 String 생성자 함수의 인스턴스는 String.prototype의 메서드를 상속받아 사용 가능
2. 래퍼 객체로 프로퍼티에 접근하거나 메서드를 호출
3. 래퍼 객체의 처리가 종료되면 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된 원시값으로 원래 상태, 즉 식별자가 원시값을 갖도록 되돌리고 래퍼 객체는 가비지 컬렉션의 대상이 됨

<br>

```js
// ① 식별자 str은 문자열을 값으로 가지고 있다.
const str = 'hello';

// ② 식별자 str은 암묵적으로 생성된 래퍼 객체를 가리킨다.
// 식별자 str의 값 'hello'는 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된다.
// 래퍼 객체에 name 프로퍼티가 동적 추가된다.
str.name = 'Lee';

// ③ 식별자 str은 다시 원래의 문자열, 즉 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된 원시값을 갖는다.
// 이때 ②에서 생성된 래퍼 객체는 아무도 참조하지 않는 상태이므로 가비지 컬렉션의 대상이 된다.

// ④ 식별자 str은 새롭게 암묵적으로 생성된 래퍼 객체를 가리킨다.
// 새롭게 생성되는 래퍼 객체에는 name 프로퍼티가 존재하지 않는다.
console.log(str.name);    // undefined

// ⑤ 식별자 str은 다시 원래의 문자열, 즉 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된 원시값을 갖는다.
// 이때 ④에서 생성된 래퍼 객체는 아무도 참조하지 않는 상태이므로 가비지 컬렉션의 대상이 된다.
console.log(typeof str, str);     // string hello
```

<br>

- 문자열, 숫자, 불리언, 심벌은 암묵적으로 생성되는 래퍼 객체의 의해 객체처럼 사용 가능하고, 표준 빌트인 객체 String, Number, Boolean, Symbol의 프로토타입 메서드 또는 프로퍼티 참조 가능
- 따라서 String, Number, Boolean 생성자 함수를 new 연산자와 함께 호출하여 문자열, 숫자, 불리언 인스턴스를 생성할 필요가 없으며 권장 X
- 문자열, 숫자, 불리언, 심벌 이외의 원시값, 즉 null과 undefined는 래퍼 객체 생성 X
  - null과 undefined 값을 객체처럼 사용하면 에러 발생

<br>
<br>

## 4. 전역 객체
- 전역 객체는 코드가 실행되기 이전 단계에 자바스크립트 엔진에 의해 어떤 객체보다도 먼저 생성되는 특수한 객체
- 어떠한 객체에도 속하지 않는 최상위 객체
- 자바스크립트 환경에 따라 지칭하는 이름이 제각각
  - 브라우저 환경 : window(또는 self, this, frames)
  - Node.js : global
  - globalThis : ES11에 도입된 globalThis는 브라우저 환경과 Node.js 환경에서 전역 객체를 가리키던 다양한 식별자를 통일한 식별자
- 표준 빌트인 객체와 환경에 따른 호스트 객체, 그리고 var 키워드로 선언한 전역 변수와 전역 함수를 프로퍼티로 갖음
- 전역 객체는 계층적 구조상 어떤 객체에도 속하지 않는 모든 빌트인 객체의 최상위 객체
  - 모든 빌트인 객체의 최상위 객체 ≠ 프로토타입 상속 관계상 최상위 객체
  - 모든 빌트인 객체의 최상위 객체 = 전역 객체는 어떤 객체의 프로퍼티도 아니며 객체의 계층적 구조상 표준 빌트인 객체와 호스트 객체를 프로퍼티로 소유

<br>

전역 객체의 특징
- 전역 객체는 개발자가 의도적으로 생성할 수 없다. 즉, 전역 객체를 생성할 수 있는 생성자 함수가 제공되지 않는다.
- 전역 객체의 프로퍼티를 참조할 때 window(또는 global)를 생략할 수 있다.
```js
// 문자열 'F'를 16진수로 해석하여 10진수로 변환하여 반환
window.parseInt('F', 16);    // 15
// window.parseInt는 parseInt 호출 가능
parseInt('F', 16);    // 15

window.parseInt === parseInt;    // true
```
- 전역 객체는 Object, String, Number, Boolean, Function, Array, RegExp, Date, Math, Promise 같은 모든 표준 빌트인 객체를 프로퍼티로 가지고 있다.
- 자바스크립트 실행 환경에 따라 프로퍼티와 메서드를 갖는다.
  - 브라우저 : DOM, BOM, Canvasm XMLHttpRequest, fetch requestAnimationFrame, SVG, Web Storage, Web Component, Web Worker 같은 클라이언트 사이드 Web API를 호스트 객체로 제공
  - Node.js : Node.js 고유의 API를 호스트 객체로 제공
- var 키워드로 선언한 전역 변수와 선언하지 않은 변수에 값을 할당한 암묵적 전역, 그리고 전역 함수는 전역 객체의 프로퍼티가 된다.
```js
// var 키워드로 선언한 전역 변수
var foo = 1;
console.log(window.foo);    // 1

// 암묵적 전역, bar는 전역 객체의 프로퍼티
bar = 2;    // window.bar = 2
console.log(window.bar);    // 2


// 전역 함수
function baz() { return 3; }
console.log(window.baz());    // 3
```
- let이나 const 키워드로 선언한 변수는 전역 객체의 프로퍼티가 아니다. 즉, window.foo와 같이 접근할 수 없다. let이나 const 키워드로 선언한 전역 변수는 보이지 않는 개념적인 블록(전역 렉시컬 환경의 선언적 환경 레코드)내에 존재하게 된다.
```js
let foo = 123;
console.log(window.foo);    // undefined
```
- 브라우저 환경의 모든 자바스크립트 코드는 하나의 전역 객체 window를 공유한다. 여러 개의 script 태그를 통해 자바스크립트 코드를 분리해도 하나의 전역 객체의 window를 공유하는 것은 변함이 없다. 이는 분리되어 있는 자바스크립트 코드가 하나의 전역을 공유한다는 의미다.

<br>

전역 객체는 몇 가지 프로퍼티와 메서드를 가지고 있다. 전역 객체의 프로퍼티와 메서드는 전역 객체를 가리키는 식별자, 즉 window나 global을 생략하여 참조/호출할 수 있으므로 전역 변수와 전역 함수처럼 사용할 수 있다.

<br>

### 4.1. 빌트인 전역 프로퍼티
빌트인 전역 프로퍼티는 전역 객체의 프로퍼티를 의미한다. 주로 애플리케이션 전역에서 사용하는 값을 제공한다.

<br>

1. Infinity : Infinity 프로퍼티는 무한대를 나타내는 숫자값 Infinity를 갖는다.
```js
console.log(window.Infinity === Infinity);    // true

// 양의 무한대
console.log(3/0);    // Infinity
// 음의 무한대
console.log(-3/0);    // -Infinity
// Infinity는 숫자값
console.log(typeof Infinity);    // number
```
2. NaN : NaN 프로퍼티는 숫자가 아님을 나타내는 숫자값 NaN을 갖는다. NaN 프로퍼티는 Number.NaN 프로퍼티와 같다.
```js
console.log(window.NaN);    // NaN

console.log(Number('xyz'));    // NaN
conosle.log(1 * 'string');    // NaN
console.log(typeof NaN);    // Number
```
3. undefined : undefined 프롶티는 원시 타입 undefined를 값으로 갖는다.
```js
console.log(window.undefined);    // undefined

var foo;
console.log(foo);    // undefined
console.log(typeof undefined);    // undefined
```

<br>

### 4.2. 빌트인 전역 함수
빌트인 전역 함수는 애플리케이션 전역에서 호출할 수 있는 빌트인 함수로서 전역 객체의 메서드다.

<br>

1. eval 
  - 자바스크립트 코드를 나타내는 문자열을 인수로 전달받는다. 
  - 전달받은 문자열 코드가 표현식이라면 eval 함수는 문자열 코드를 런타임에 평가하여 값을 생성하고, 전달받은 인수가 표현식이 아닌 문이라면 eval 함수는 문자열 코드를 런타임에 실행한다. 
```js
// 표현식인 문
eval('1 + 2;');    // 3
// 표현식이 아닌 문
eval('var x = 5;');    // undefined

// eval 함수에 의해 런타임에 변수 선언문이 실행되어 x 변수가 선언
console.log(x);    // 5

// 객체 리터럴은 반드시 괄호로 둘러싼다.
const o = eval('({ a: 1 })');
console.log(o);    // {a: 1}

// 함수 리터럴은 반드시 괄호로 둘러싼다.
const f = eval('(function() { return 1; })');
console.log(f());    // 1
```
- 문자열 코드가 여러 개의 문으로 이루어져 있다면 모든 문을 실행한 다음, 마지막 결과값을 반환한다.
```js
eval('1 + 2; 3 + 4;');    // 7
```
- eval 함수는 자신이 호출된 위치에 해당하는 **기존의 스코프를 런타임에 동적으로 수정한다.**
    - 단, strict mode에서 eval 함수는 기존의 스코프를 수정하지 않고 eval 함수 자신의 자체적인 스코프를 생성한다.
```js
const x = 1;

function foo() {
  // eval 함수는 런타임에 foo 함수의 스코프를 동적으로 수정
  eval('var x = 2;');
  console.log(x);    // 2
}

foo();
console.log(x);    // 1
```
```js
const x = 1;

function foo() {
  'use strict';

  eval('var x = 2; console.log(x);');    // 2
  console.log(x);    // 1
}

foo();
console.log(x);    // 1
```
- 인수로 전달받은 문자열 코드가 let, const 키워드를 사용한 변수 선언문이라면 암묵적으로 strict mode가 적용된다.
```js
const x = 1;

function foo() {
  eval('var x = 2; console.log(x);');    // 2
  eval('const x = 3; console.log(x);');    // 3
  console.log(x);    // 2
}

foo();
console.log(x);    // 1
```
- eval 함수를 통해 사용자로부터 입력받은 콘텐츠를 실행하는 것은 보안에 매우 취약하다.
- eval 함수를 통해 실행되는 코드는 자바스크립트 엔진에 의해 최적화가 수행되지 않으므로 일반적인 코드 실행에 비해 처리 속도가 느리다.
- **따라서 eval 함수 사용은 금지해야 한다.**

<br>

2. isFinite
- 전달받은 인수가 유한수이면 true를 반환, 무한수이면 false를 반환
- 전달받은 인수의 타입이 숫자가 아닌 경우, 숫자로 타입 변환후 검사 수행
  - 인수가 NaN으로 평가되는 값이라면 false 반환
```js
isFinite(0);    // true
isFinite(2e64);    // true
isFinite('10');    // true: '10' -> 10
isFinite(null);    // true: null -> 0

isFinite(Infinity);    // false
isFinite(-Infinity);    // false

isFinite(NaN);    // false
isFininte('Hello');    // false
isFinite('2005/12/12');    // false
```

<br>

3. isNaN
- 전달받은 인수가 NaN인지 검사하여 그 결과를 불리언 타입으로 반환
- 전달받은 인수의 타입이 숫자가 아닌 경우 숫자로 타입 변환 후 검사 수행
```js
isNaN(NaN);    // true
isNaN(10);    // false

isNaN('blabla');    // true: 'blabla' -> NaN
isNaN('10');    // false: '10' -> 10
isNaN('10.12');    // false: '10.12' -> 10.12
isNaN('');    // false: '' -> 0
isNaN(' ');    // fasle: ' ' -> 0

isNaN(true);    // false: true -> 1
isNaN(null);    // false: null -> 0

isNaN(undefined);    // true: undefined -> NaN

isNaN({});    // true: {} -> NaN

isNaN(new Date());    // false: new Date() -> Number
isNaN(new Date().toString());    // true: String -> NaN
```

<br>

4. parseFloat
- 전달받은 문자열 인수를 부동 소수점 숫자, 즉 실수로 해석하여 반환한다.
- 공백으로 구분된 문자열은 첫 번째 문자열만 변환
- 첫 번째 문자열을 숫자로 변환할 수 없다면 NaN 반환
- 앞뒤 공백 무시
```js
parseFloat('3.14');    // 3.14
parseFloat('10.00');    // 10

parseFloat('34 45 56');    // 34
parseFloat('40 years');    // 40

parseFloat('He was 40');    // NaN

parseFloat(' 60 ');    // 60
```

<br>

5. parseInt
- 전달받은 문자열 인수를 정수로 해석하여 반환
```js
parseInt('10');    // 10
parseInt('10.123');    // 10
```
- 전달받은 인수가 문자열이 아니라면 문자열로 변환한 다음, 정수로 해석하여 반환
```js
parseInt(10);    // 10
parseInt(10.123);    // 10
```
- 두 번째 인수로 진법을 나타내는 기수(2 ~ 36) 전달 가능
  - 첫 번째 인수로 전달된 문자열을 해당 기수의 숫자로 해석하여 반환
  - 반환값은 언제나 10진수
  - 기수를 생략하면 10진수로 해석하여 반환
```js
parseInt('10');    // 10
parseInt('10', 2);    // 2
parseInt('10', 8);    // 8
parseInt('10', 16);    // 16
```
- 기수를 지정하여 10진수 숫자를 해당 기수의 문자열로 변환하여 반환하고 싶을 때는 Number.prototype.toString 메서드 사용
```js
const x = 15;

x.toString(2);    // '1111'
parseInt(x.toString(2), 2);    // 15

x.toString(8);    // '17'
parseInt(x.toString(8), 8);    // 15

x.toString(16);    // 'f'
parseInt(x.toString(16), 16);    // 15

x.toString();    // '15'
parseInt(x.toString());    // 15
```
- 두 번째 인수로 진법을 나타내는 기수를 지정하지 않더라도 첫 번째 인수로 전달된 문자열이 "0x" 또는 "0X"로 시작하는 16진수 리터럴이라면 16진수로 해석하여 10진수 정수로 반환
```js
parseInt('0xf');    // 15
parseInt('f', 16);    // 15
```
- 2진수 리터럴과 8진수 리터럴은 제대로 해석 X
```js
parseInt('0b10');    // 0
parseInt('0o10');    // 0
```
- 첫 번째 인수로 전달한 문자열의 첫 번째 문자가 해당 지수의 숫자로 변환될 수 없다면 NaN 반환
```js
parseInt('A0');    // NaN
parseInt('20', 2);    // NaN
```
- 첫 번째 인수로 전달한 문자열의 두 번째 문자부터 해당 진수를 나타내는 숫자가 아닌 문자와 마주치면 이 문자와 계속되는 문자들은 전부 무시되며 해석된 정수값만 반환
```js
parseInt('1A0');    // 1
parseInt('102', 2);    // 2
parseInt('58', 8);    // 5
parseInt('FG', 16);    // 15
```
- 첫 번째 인수로 전달한 문자열에 공백이 있다면 첫 번째 문자열만 해석하여 반환하며 앞뒤 공백 무시, 만일 첫 번째 문자열을 숫자로 해석할 수 없는 경우 NaN 반환
```js
parseInt('34 45 56');    // 34
parseInt('40 years');    // 40
parseInt('He was 40');    // NaN
parseInt(' 60 ');    // 60
```

<br>

5. encodeURI / decodeURI
- encodeURI : 완전한 URI를 문자열로 전달받아 이스케이프 처리를 위해 인코딩
  - URI : 인터넷에 있는 자원을 나타내는 유일한 주소
  - 하위개념으로 URL, URN이 존재
  - 인코딩 : URI 문자들을 이스케이프 처리하는 것을 의미
  - 이스케이프 처리 : 네트워크를 통해 정보를 공유할 때 어떤 시스템에서도 읽을 수 있는 아스키 문자 셋으로 변환하는 것
    - UTF-8 한글 표현 : 1문자당 3바이트
    - UTF-8 특수문자 : 1문자당 1~3바이트
    - URL은 아스키 문자 셋으로만 구성되어야 함 -> 의미를 갖고 있는 문자(?, %, #), 올 수 없는 문자(한글, 공백 등), 시스템에 의해 해석될 수 있는 문자(<, >) 이스케이프 처리
    - 알파벳, 0 ~ 9의 숫자, -_.!*'() 문자는 이스케이프 처리에서 제외
- decodeURI : 인코딩된 URI를 인수로 전달받아 이스케이프 처리 이전으로 디코딩
```js
// 완전한 URI
const uri = 'http://example.com?name=정채빈&job=programmer&frontend';

const enc = encodeURI(uri);  
console.log(enc);    // http://example.com?name=%EC%9D%B4%EC%9B%AA%DS%A8&job=programmer&frontend

const dec = decodeURI(enc);
console.log(dec);    // http://example.com?name=정채빈&job=programmer&frontend
```

<br>

6. encodeURIComponent / decodeURIComponent
- encodeURIComponent : URI 구성 요소(component)를 인수로 전달받아 인코딩
- decodeURIComponent : 매개변수로 전달된 URI 구성 요소를 디코딩
- encodeURIComponent와 encodeURI 차이점
  - encodeURIComponent : 인수로 전달된 문자열을 URI의 구성요소인 쿼리 스트링의 일부로 간주 -> 쿼리 스트링 구분자로 사용되는 =, ?, &까지 인코딩
  - encodeURI : 매개변수로 전달된 문자열을 완전한 URI로 간주 -> 쿼리 스트링 구분자로 사용되는 =, ?, & 인코딩 X
```js
// URI 쿼리 스트링
const uriComp = 'name=정채빈&job=programmer&frontend';

let enc = encodeURIComponent(uriComp);
console.log(enc);    // name%3D%EC%9D%B4%EC%9B%AA%DS%A8%26job%3Dprogrammer%26frontend

let dec = decodeURIComponent(enc);
console.log(dec);    // 정채빈&job=programmer&frontend
```

<br>

### 4.3. 암묵적 전역
```js
var x = 10;    // 전역 변수

function foo () {
  y = 20;    // window.y = 20;
}

console.log(x + y);    // 30
```
위의 코드를 살펴보면,
- y는 선언하지 않은 식별자지만 마치 선언된 전역 변수처럼 동작
  - 선언하지 않은 식별자에 값을 할당하면 전역 객체의 프로퍼티가 되기 때문
- **암묵적 전역** : foo 함수 호출 -> 자바스크립트 엔진은 y 변수에 값을 할당하기 위해 스코프 체인을 통해 선언된 변수인지 확인 -> foo 함수의 스코프와 전역 스코프 어디에서도 y 변수 선언 X -> 참조 에러 발생 -> 자바스크립트 엔진은 y = 20을 window.y = 20으로 해석 -> 전역 객체에 프로퍼티 동적 생성 -> y는 전역 객체의 프로퍼티가 되어 전역 변수처럼 동작
- y는 변수 선언 없이 단지 전역 객체의 프로퍼티로 추가되었을 뿐 y는 변수가 아니므로 변수 호이스팅이 발생 X
```js
console.log(x);    // undefined
console.log(y);    // ReferenceError: y is not defined

var x = 10;

function foo() {
  y = 20;    // window.y = 20;
}
foo();

console.log(x + y);    // 30
```
- 변수가 아니라 단지 프로퍼티인 y는 delete 연산자로 삭제 가능하지만 전역 변수는 프로피티이잠 delete 연산자로 삭제 불가능
```js
var x = 10;

function foo() {
  y = 20;    // window.y = 20
  console.log(x + y);    
}
foo();    // 30

console.log(window.x);    // 10
console.log(window.y);    // 20

delete x;    // 삭제 X
delete y;    // 삭제

console.log(window.x);    // 10
console.log(window.y);    // undefined
```

<br>
<br>