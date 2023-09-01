# 타입 변환과 단축 평가

## 1. 타입 변환
타입 변환이란 <b>기존 원시 값을 사용해 다른 타입의 새로운 원시 값을 생성하는 것이다.</b>

<br>

```js
var x = 10; 

// 명시적 타입 변환
// 숫자를 문자로 타입 캐스팅
var str = x.toString();
console.log(typeof str, str);    // string 10

// x 변수 값이 변경된 것은 아니다
console.log(typeof x, x);    // number 10

// 암묵적 타입 변환
var str = x + '';
console.log(typeof str, str);    // string 10

// x 변수의 값이 변경된 것은 아니다
console.log(typeof x, x);    // number 10
```
- 명시적 타입 변환(explicit coercion) 또는 타입 캐스팅(type casting) : 개발자가 의도적으로 값의 타입을 변환하는 것
- 암묵적 타입 변환(implicit coercion) 또는 타입 강제 변환(type coercion) : 자바스트립트 엔진에 의해 암묵적으로 타입이 자동 변환되는 것

<br>
<br>

## 2. 암묵적 타입 변환
자바스크립트는 가급적 에러를 발생시키지 않도록 암묵적 타입 변환을 통해 표현식을 평가한다. 암묵적 타입 변환이 발생하면 문자열, 숫자, 불리언과 같은 원시 타입 중 하나로 타입을 자동 변환한다. 

<br>

### 2.1. 문자열 타입으로 변환
- \+ 연산자: \+ 연산자는 피연산자 중 하나 이상이 문자열이면 문자열 연결 연산자로 동작한다.

```js
// 숫자 타입
0 + '';    // "0"
-0 + '';    // "0"
1 + '';    // "1"
-1 + '';    // "-1"
NaN + '';    // "NaN"
Infinity + '';    // "Infinity"
-Infinity + '';    // "-Infinity"

// 불리언 타입
true + '';    // "true"
false + '';    // "false"

// null 타입
null + '';    // null

// undefined 타입
undefined + '';    // undefined

// 심벌 타입
(Symbol()) + '';    // TypeError: Cannot convert a Symbol value to a string

// 객체 타입
({}) + '';    // "[object Object]"
Math + '';    // "[object Object]"
[] + '';    // ""
[10, 20] + '';    // "10, 20"
(function(){}) + '';    // "function(){}"
Array + '';    // "function Array() { [native code]}"
```

<br>

### 2.2. 숫자로 타입 변환
- 산술 연산자 : 산술 연산자의 피연산자 중에서 숫자 타입이 아닌 피연산자를 숫자 타입으로 암묵적 타입 변환한다. 이때 피연산자를 숫자 타입으로 변환할 수 없는 경우는 산술 연산을 수행할 수 없으므로 표현식의 평가 결과는 NaN이다.
```js
1 - '1'    // 0
1 * '10'    // 10
1 / 'one'    // NaN
```

<br>

- 비교 연산자 : 비교 연산자의 피연산자 중에서 숫자 타입이 아닌 피연산자를 숫자 타입으로 암묵적 타입 변환한다.
```js
'1' > 0    // true
```

<br>

- \+ 단항 연산자 : + 단항 연산자는 피연산자가 숫자 타입의 값이 아니면 숫자 타입의 값으로 암묵적 타입 변환한다.
```js
// 문자열 타입
+''    // 0
+'0'    // 0
+'1'    // 1
+'string'    // NaN

// 불리언 타입
+true    // 1
+false    // 0

// null 타입
+null    // 0

// undefined 타입
+undefined   // NaN

// 심벌 타입
+Symbol()    // TypeError: Cannot convert a Symbol value to a number

// 객체 타입
+{}    // NaN
+[]    // 0
+[10, 20]    // NaN
+(function(){})    // NaN
```

<br>

### 2.3. 불리언 타입으로 변환
- 제어문, 삼항 연산자의 조건식 : if문이나 for문과 같은 제어문 또는 삼항 연산자의 조건식은 불리언 값, 즉 논리적 참/거짓으로 평가되어야 하는 표현식이다.

  자바스크립트 엔진은 불리언 타입이 아닌 값을 Truthy 값(참으로 평가되는 값) 또는 Falsy 값(거짓으로 평가되는 값)으로 구분한다.
  
  <b>즉, 제어문의 조건식과 같이 불리언 값으로 평가되어야 할 문맥에서 Truthy 값은 true, Falsy 값은 false로 암묵적 타입 변환한다.</b>

```js
if ('')       console.log('1');
if (true)     console.log('2');
if (0)        console.log('3');
if ('str')    console.log('4');
if (null)     console.log('5');

// 2 4
```

<br>

- false로 평가되는 Falsy 값 
  - false
  - undefined
  - null
  - 0, -0
  - NaN
  - '' (빈문자열)
- true로 평가되는 Truthy 값 : Falsy 값 외의 모든 값

<br>
<br>

## 3. 명시적 타입 변환
개발자의 의도에 따라 명시적으로 타입을 변경하는 방법은 다양하다. 표준 빌트인 생성자 함수(String, Number, Boolean)를 new 연산자 없이 호출하는 방법과 빌트인 메서드를 사용하는 방법, 그리고 앞에서 살펴본 암묵적 타입 변환을 이용하는 방법이 있다.

<br>

### 3.1. 문자열 타입으로 변환
```js
// 숫자 -> 문자열
String(1);    // "1"
String(NaN);    // "NaN"
// 불리언 -> 문자열
String(true);    // "true"
String(false);    // "false"

// 숫자 -> 문자열
(1).toString();    // "1"
(NaN).toString();    // "NaN"
// 불리언 -> 문자열
(true).toString();
(false).toString();

// 숫자 -> 문자열
1 + '';    // "1"
NaN + '';    // "NaN"
// 불리언 -> 문자열
true + '';    // "true"
false + '';    // "false"
```
위의 코드를 살펴보면,
1. String 생성자 함수를 new 연산자 없이 호출하는 방법
2. Object.prototype.toString 메서드를 사용하는 방법
3. 문자열 연결 연산자를 이용하는 방법

<br>

### 3.2. 숫자 타입으로 변환
```js
// 문자열 -> 숫자
Number('0');    // 0
Number('-1');    // -1
// 불리언 -> 숫자
Number(true);    // 1
Number(false);    // 0

// 문자열 -> 숫자
parseInt('0');    // 0
parseInt('-1');    // -1

// 문자열 -> 숫자
+'0';    // 0
+'-1';    // -1
// 불리언 -> 숫자
+true;    // 1
+false;    // 0

// 문자열 -> 숫자
'0' * 1;    // 0
'-1' * 1;    // -1
// 불리언 -> 숫자
true * 1;    // 1
false * 1;    // 0
```
위의 코드를 살펴보면,
1. Number 생성자 함수를 new 연산자 없이 호출하는 방법
2. parseInt, parseFloat 함수를 사용하는 방법 (문자열만 숫자 타입으로 변환 가능)
3. \+ 단항 산술 연산자를 이용하는 방법
4. \* 산술 연산자를 이용하는 방법

<br>

### 3.3. 불리언 타입으로 변환
```js
// 문자열 -> 불리언
Boolean('x');    // true
Boolean('');    // false
Boolean('false');    // true
// 숫자 -> 불리언
Boolean(0);    // false
Boolean(1);    // true
Boolean(NaN);    // false
Boolean(Infinity);    // true
// null -> 불리언
Boolean(null);    // null
// undefined -> 불리언
Boolean(undefined);    // false
// 객체 -> 불리언
Boolean({});    // true
Boolean([]);    // true

// 문자열 -> 불리언
!!'x';    // true
!!'';    // false
!!'false';    // true
// 숫자 -> 불리언
!!0;    // false
!!1;    // true
!!NaN;    // false
!!Infinity;    // true
// null -> 불리언
!!null;    // false
// undefined -> 불리언
!!undefined;    // false
// 객체 -> 불리언
!!{};    // true
!![];    // true
```
위의 코드를 살펴보면,
1. Boolean 생성자 함수를 new 연산자 없이 호출하는 방법
2. ! 부정 논리 연산자를 두 번 사용하는 방법

<br>
<br>

## 4. 단축 평가 ⭐

<br>

### 4.1. 논리 연산자를 사용한 단축 평가
논리 연산자를 사용한 단축 평가는 논리 연산의 결과를 결정하는 피연산자를 타입 변환하지 않고 그대로 반환하는 것을 말한다. <b>단축 평가는 표현식을 평가하는 도중에 평가 결과가 확정된 경우 나머지 평가 과정을 생략하는 것을 말한다.</b>

<br>

```js
'Cat' && 'Dog'    // "Dog"
'Cat' || 'Dog'    // "Cat"
```
위의 코드를 살펴보면,
1. 논리곱(&&) 연산자는 두 개의 피연산자가 모두 true로 평가될 때 true를 반환한다. 논리곱 연산자는 <b>좌항에서 우항으로 평가가 진행된다.</b>

    첫 번째 피연산자 'Cat'은 Truthy 값이므로 true로 평가된다. 하지만 이 시점까지는 표현식을 평가할 수 없고 두 번째를 평가해야 표현식을 평가할 수 있다. 
    
    다시 말해, 두 번째 피연산자가 논리곱 연산자 표현식의 평가 결과를 결정한다. <b>이때 논리곱 연산자는 논리 연산의 결과를 결정하는 두 번째 피연산자, 즉 문자열 'Dog'를 그대로 반환한다.</b>

2. 논리합(||) 연산자는 두 개의 피연산자 중 하나만 true로 평가되어도 true를 반환한다. 논리합 연산자도 좌항에서 우항으로 평가가 진행된다.

    첫 번째 피연산자 'Cat'은 Truthy 값이므로 true로 평가된다. 이 시점에서 두 번째 피연산자까지 평가해 보지 않아도 위 표현식을 평가할 수 있다. <b>이때 논리합 연산자는 논리 연산의 결과를 결정한 첫 번째 피연산자, 즉 문자열 'Cat'을 그대로 반환한다.</b>

<br>

| 단축 평가 표현식 | 평가 결과 |
| :---: | :---: |
| true \|\| anything | true |
| false \|\| anything | anything |
| true && anything | anything |
| false && anything | false |

<br>

단축 평가를 사용하면 if문을 대체할 수 있다.
- 어떤 조건이 Truthy일 때 무언가를 해야 한다면 논리곱 연산자 표현식으로 if문을 대체할 수 있다.
```js
var done = true;
var message='';

// 주어진 조건이 true일 때
if (done) message = '완료';

// done이 true라면 message에 '완료' 할당
message = done && '완료';
console.log(message);    // 완료
```

<br>

- 조건이 Falsy 값일 때 무언가를 해야 한다면 논리합(||) 연산자 표현식으로 if문을 대체할 수 있다.
```js
var done = true;
var message = '';

// 주어진 조건이 false일 때
if (!done) message = '미완료'

// done이 false라면 message에 '미완료'를 할당
message = done || '미완료';
console.log(message);    // 미완료
```

<br>

단축 평가는 다음과 같은 상황에서 유용하게 사용된다.
- 객체를 가리키기를 기대하는 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티를 참조할 때
  - 만약 객체를 가리키기를 기대하는 변수의 값이 객체가 아니라 null 또는 undefined인 경우 객체의 프로퍼티를 참조하면 타입 에러가 발생하고 프로그램이 강제 종료되지만 단축 평가를 사용하면 에러를 발생시키지 않는다.
```js
var elem1 = null;
var value1 = elem1.value1;    // TypeError: Cannot read property 'value' of null

// 단축 평가 사용
var elem2 = null;
var value2 = elem2 && elem2.value2;    // null
```

<br>

- 함수 매개변수에 기본값을 설정할 때
  - 함수를 호출할 때 인수를 전달하지 않으면 매개변수에는 undefined가 할당된다. 이때 단축 평가를 사용해 매개변수의 기본값을 설정하면 undefined로 인해 발생할 수 있는 에러를 방지할 수 있다.
```js
function getStringLength(str) {
  str = str || '';
  return str.length;
}

getStringLength();    // 0
getStringLength('hi');    // 2

// ES6의 매개변수 기본값 설정
function getStringLength(str = '') {
  return str.length;
}

getStringLength();    // 0
getStringLength('hi');    // 2
```

<br>

### 4.2. 옵셔널 체이닝 연산자
ES11에 도입된 옵셔널 체이닝 연산자 ?.는 좌항의 피연산자가 null 또는 undefined인 경우 undefined를 반환하고, 그렇지 않으면 우항의 프로퍼티 참조를 이어간다.
```js
var elem = null;

var value = elem?.value;
console.log(value);    // undefined
```

<br>

옵셔널 체이닝 연산자는 논리곱 연산자를 사용해 단축 평가를 통해 변수가 null 또는 undefined인지 확인하는 방법보다 유용하다.
```js
var str = '';

// 문자열 길이를 참조한다.
var length = str && str.length;

// 문자열 길이를 참조하지 못한다.
console.log(length);    // ''
```
- 논리곱 연산자는 좌항 피연산자가 false로 평가되는 Falsy 값(false, undefined, null, 0, -0, NaN, '')이면 좌항 피연산자를 그대로 반환한다. 

  좌항 피연산자가 Falsy값인 0이나 ''인 경우도 마찬가지다. 하지만 0이나 ''은 객체로 평가될 때도 있다.

```js
var str = '';

var length = str?.length;
console.log(length);    // 0
```
- 옵셔널 체이닝 연산자는 좌항 피연산자가 false로 평가되는 Falsy값이라도 null 또는 undefined가 아니면 우항의 프로퍼티 참조를 이어간다.

<br>

### 4.3. null 병합 연산자
ES11에서 도입된 null 병합 연산자 ??는 좌항의 피연산자가 null 또는 undefined인 경우 우항의 피연산자를 반환하고, 그렇지 않으면 좌항의 피연산자를 반환한다. <b>null 병합 연산자는 변수에 기본값을 설정할 때 유용하다.</b>
```js
var foo = null ?? 'default string';
console.log(foo);    // default string
```

<br>

null 병합 연산자는 논리합 연산자를 사용한 단축 평가를 통해 변수에 기본값을 설정하는 방법보다 유용하다.
```js
var foo = '' || 'default string';
console.log(foo);    // default string
```
- 논리합 연산자를 사용한 단축 평가의 경우 좌항의 연산자가 false로 평가되는 Falsy 값이면 우항의 피연산자를 반환한다. 

  만약 Falsy 값인 0이나 ''도 기본값으로서 유효하다면 예기치 못한 동작이 발생할 수 있다.

<br>

```js
var foo = '' ?? 'default string';
console.log(foo);    // ""
```
- null 병합 연산자는 좌항의 피연산자가 false로 평가되는 Falsy 값이라도 null 또는 undefined가 아니면 좌항의 피연산자를 그대로 반환한다.

<br>
<br>