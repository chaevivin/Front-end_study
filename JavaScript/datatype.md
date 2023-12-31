# 데이터 타입

자바스크립트(ES6)는 7개의 데이터 타입을 제공한다. 7개의 데이터 타입은 원시 타입(primitive type)과 객체 타입(object/reference type)으로 분류할 수 있다.

| 구분 | 데이터 타입 | 설명 |
| :--- | :--- | :--- |
| 원시 타입 | 숫자 타입 (number)| 숫자. 정수와 실수 구분 없이 하나의 숫자 타입만 존재 |
| 원시 타입 | 문자열 타입 (string) | 문자열 |
| 원시 타입 | 불리언 타입 (boolean) | 논리적 참(true)과 거짓(false) |
| 원시 타입 | undefined 타입 | var 키워드로 선언된 변수에 암묵적으로 할당되는 값 |
| 원시 타입 | null 타입 | 값이 없다는 것을 의도적으로 명시할 때 사용하는 값 |
| 원시 타입 | 심벌 타입 (symbol) | ES6에 추가된 7번째 타입 |
| 객체 타입 | 객체 타입 | 객체, 함수, 배열 등 |

<br>
<br>

### 1. 숫자 타입
하나의 숫자 타입만 존재한다. <b>모든 수를 실수로 처리한다.</b>

<br>

```js
var binary = 0b1000001;    // 2진수
var octal = 0o101;         // 8진수
var hex = 0x41;            // 16진수

console.log(binary);    // 65
console.log(octal);     // 65
console.log(hex);       // 65
console.log(binary === octal);    // true
console.log(octal === hex);       // true
```
위의 코드를 살펴보면, 
1. 2진수, 8진수, 16진수를 표현하기 위한 데이터 타입을 제공하지 않기 때문에 값을 참조하면 모두 10진수로 해석된다.
2. 표기법만 다를 뿐 모두 같은 값이다.

<br>

```js
console.log(1 === 1.0);    // true
```
위의 코드를 살펴보면, 숫자 타입은 모두 실수로 처리되기 때문에 1과 1.0은 같은 값이다.

<br>

```js
console.log(10 / 0);    // Infinity
console.log(10 / -0);   // -Infinity
console.log(1 * 'String');    // NaN
```
숫자 타입에는 세 가지 특별한 값이 있다.
- Infinity : 양의 무한대
- -Infinity : 음의 무한대
- NaN : 산술 연산 불가 (not-a-number)

<br>
<br>

### 2. 문자열 타입
문자열 타입은 텍스트 데이터를 나타내는데 사용한다. 문자열은 작은 따옴표(' '), 큰따옴표(" "), 백틱(\` `)으로 텍스트를 감싼다.

<br>

<b>자바스크립트의 문자열은 원시 타입이며, 변경 불가능한 값이다. </b>

<br>
<br>

### 3. 템플릿 리터럴
템플릿 리터럴은 ES6에 추가된 표기법으로 멀티라인 문자열, 표현식 삽입, 태그드 탬플릿 등 편리한 문자열 처리 기능을 제공한다. 백틱을 사용해 표현한다.

<br>

#### 3.1. 멀티라인 문자열
일반 문자열 내에서는 줄바꿈이 허용되지 않는다. 

```js
var str = 'Hello
world.';      
// SyntaxError: Invalid or unexpected token
```

<br>

문자열 내에서 공백을 표현하려면 백슬래시로 시작하는 이스케이프 시퀀스(escape sequence)를 사용해야 한다.
| 이스케이프 시퀀스 | 의미 |
| :--- | :--- |
| \0 | Null |
| \b | 백스페이스 |
| \f | 폼 피드(form feed) : 프린터로 출력할 경우 다음 페이지의 시작 지점으로 이동한다.|
| \n | 개행 : 다음 행으로 이동 |
| \r | 개행 : 커서를 처음으로 이동 |
| \t | 탭(수평) |
| \v | 탭(수직) |
| \uXXXX | 유니코드. 예를 들어 '\u0041'은 'A' |
| \\' | 작은따옴표 |
| \\" | 큰따옴표 |
| \\\ | 백슬래시 |

<br>

일반 문자열과 달리 <b>템플릿 리터럴 내에서는 이스케이프 시퀀스를 사용하지 않고도 줄바꿈이 허용</b>되며, 모든 공백도 있는 그대로 적용된다.

<br>

#### 3.2. 표현식 삽입
템플릿 리터럴 내에서는 표현식 삽입을 통해 간단히 문자열을 삽입할 수 있다.
```js
var first = 'Chae-been';
var last = 'Jeong';

console.log(`My name is ${first} ${last}`);    
// My name is Chae-been Jeong
```

<br>
<br>

### 4. 불리언 타입
불리언 타입의 값은 논리적 참, 거짓을 나타내는 true, false가 있다.
```js
var foo = true;
console.log(foo);    // true

foo = false;
console.log(foo);    // false
```

<br>
<br>

### 5. undefined 타입
변수를 처음 선언하고 값을 할당하지 않으면, 자바스크립트 엔진이 undefined로 초기화한다.
```js
var foo;
console.log(foo);    // undefined
```

<br>

<b>변수에 값이 없다는 것을 명시하고 싶을 때는 undefined가 아니라 null을 할당한다.</b>

<br>
<br>

### 6. null 타입
null은 변수에 값이 없다는 것을 의도적으로 명시할 때 사용한다. 변수에 null을 할당하는 것은 변수가 이전에 참조하던 값을 더 이상 참조하지 않겠다는 의미다.

<br>

```js
var foo = 'Lee';
foo = null;
```
위의 코드를 살펴보면,
1. 이전 참조를 제거해서 foo 변수는 더 이상 'Lee'를 참조하지 않는다.
2. 유용해 보이지 않는다. 변수의 스코프를 좁게 만들어 변수 자체를 재빨리 소멸시키는 편이 낫다.

<br>

```js
var element = document.querySelector('.myClass');
console.log(element);    // null
```
위의 코드를 살펴보면,
1. HTML 문서에서 myClass 클래스를 갖는 요소가 없다.
2. <b>함수가 유효한 값을 반환할 수 없는 경우이므로 element는 명시적으로 null을 반환한다.</b>

<br>
<br>

### 7. 심벌 타입 ⭐
심벌 값은 다른 값과 중복되지 않는 유일무이한 값이다. 따라서 주로 이름이 충돌할 위험이 없는 객체의 유일한 프로퍼티 키를 만들기 위해 사용한다.

<br>

```js
// 심벌 값 생성
var key = Symbol('key');
console.log(typeof key);    // symbol

// 객체 생성
var obj = {};

// 프로퍼티 키로 사용
obj[key] = 'value';
console.log(obj[key]);    // value
```
위의 코드를 살펴보면,
1. 심벌은 Symbol 함수를 호출해 생성한다.
2. 이때 생성된 심벌 값은 외부에 노출되지 않으며, 다른 값과 절대 중복되지 않는 유일무이한 값이다.

<br>
<br>

### 8. 객체 타입
자바스크립트는 객체 기반의 언어이며, 자바스크립트를 이루고 있는 거의 모든 것이 객체이다.

<br>
<br>

### 9. 데이터 타입의 필요성

<br>

#### 9.1. 데이터 타입에 의한 메모리 공간의 확보와 참조
자바스크립트 엔진은 데이터 타입, 즉 값의 종류에 따라 정해진 크기의 메모리 공간을 확보한다. <b>즉, 변수에 할당되는 값의 데이터 타입에 따라 확보해야 할 메모리 공간의 크기가 결정된다.</b>

예를 들어, level이라는 변수에 숫자 타입의 값 3이 저장되어 있다고 가정해보자. 자바스크립트 엔진은 리터럴 3을 숫자 타입의 값으로 해석하고 3을 저장하기 위해 8바이트 메모리 공간을 확보한다. 그리고 3을 2진수로 저장한다.

<br>

#### 9.2. 데이터 타입에 의한 값의 해석
모든 값은 데이터 타입을 가지며, 메모리에 2진수, 즉 비트의 나열로 저장된다. 메모리에 저장된 값은 데이터 타입에 따라 다르게 해석될 수 있다. 예를 들어, 메모리에 저장된 값 0100 0001을 숫자로 해석하면 65, 문자열로 해석하면 'A'다.

<br>

자바스크립트에서 데이터 타입이 필요한 이유
- 값을 저장할 때 확보해야 하는 <b>메모리 공간의 크기</b>를 결정하기 위해
- 값을 참조할 때 한 번에 읽어 들어야 할 <b>메모리 공간의 크기</b>를 결정하기 위해
- 메모리에서 읽어 들인 <b>2진수를 어떻게 해석할 지 결정</b>하기 위해

<br>
<br>

### 10. 동적 타이핑

<br>

#### 10.1. 동적 타입 언어와 정적 타입 언어
- 동적 타이핑 (dymanic typing)
  - 자바스크립트 변수는 선언이 아닌 할당에 의해 타입이 결정(타입 추론)된다. 그리고 재할당에 의해 변수의 타입은 언제든지 동적으로 변할 수 있다.
  - 동적 타입 언어(dynamic/weak type) : 자바스크립트, 파이썬, PHP, 루비, 리스프, 펄 등
- 정적 타이핑 (static typing)
  - 변수를 선언할 대 변수에 할당할 수 있는 값의 종류, 즉 데이터 타입을 사전에 선언(명시적 타입 선언)해야 한다.
  - 정적 타입 언어는 변수의 타입을 변경할 수 없으며, 변수에 선언한 타입에 맞는 값만 할당할 수 있다.
  - 정적 타입 언어(static/strong type) : C, C++, 자바, 코틀린, 고, 하스켈, 러스트, 스칼라 등

<br>

```js
//동적 타입 언어
var foo = 3;
console.log(typeof foo);    // number

foo = 'Hello';
console.log(typeof foo);    // string
```
```c
// 정적 타입 언어
char c;    // char에는 정수 타입의 값(문자)만 할당

int num;    // int에는 정수 타입의 값만 할당
```

<br>

#### 10.2. 동적 타입 언어와 변수
<b>동적 타입 언어는 유연성은 높지만 신뢰성은 떨어진다.</b> 변수 값은 언제든지 변경될 수 있기 때문에 복잡한 프로그램에서는 변화하는 변수의 값을 추적하기 어려울 수 있다. 그리고 개발자의 의도와 상관없이 자바스크립트 엔진에 의해 암묵적으로 타입이 자동으로 변환되기도 한다.

이러한 이유로 변수를 사용할 때는 주의사항을 지켜야 한다.
- 변수는 꼭 필요한 경우에 한해 제한적으로 사용한다.
- 변수의 유효 범위(스코프)는 최대한 좁게 만들어 변수의 부작용을 억제한다.
- 전역 변수는 최대한 사용하지 않는다.
- 변수보다는 상수를 사용해 값의 변경을 억제한다.
- 모든 식별자(변수, 함수, 클래스 이름)는 목적이나 의미를 파악할 수 있도록 네이밍한다.

<br>
<br>