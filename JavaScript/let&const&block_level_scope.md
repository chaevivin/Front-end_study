# let, const 키워드와 블록 레벨 스코프

## 1. var 키워드로 선언한 변수의 문제점

<br>

### 1.1. 변수 중복 선언 허용
var 키워드로 선언한 변수는 중복 선언이 가능하다.
```js
var x = 1;
var y = 1;

var x = 100;
var y;

console.log(x);    // 100
console.log(y);    // 1
```
위의 코드를 살펴보면,
1. 변수 x, y는 모두 중복 선언되었는데, x는 초기화문이 있고, y는 초기화문이 없다.
2. 자바스크립트 엔진에 의해 초기화문이 있는 변수 선언문은 var 키워드가 없는 것처럼 동작하고, 초기화문이 없는 변수 선언문은 무시된다.

만약 동일한 이름의 변수가 이미 선언되어 있는 것을 모르고 변수를 중복 선언하면서 값까지 할당했다면 의도치 않게 먼저 선언된 변수 값이 변경되는 부작용이 있다.

<br>

### 1.2. 함수 레벨 스코프
var 키워드로 선언한 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정한다. 함수 레벨 스코프는 전역 변수를 남발할 가능성을 높여 의도치 않게 전역 변수가 중복 선언되는 경우가 발생한다.

<br>

### 1.3. 변수 호이스팅
var 키워드로 변수를 선언하면 변수 호이스팅에 의해 변수 선언문이 스코프의 선두로 끌어 올려진 것처럼 동작한다. 즉, 변수 호이스팅에 의해 var 키워드로 선언한 변수는 변수 선언문 이전에 참조할 수 있다. 단, 할당문 이전에 변수를 참조하면 언제나 undefined를 반환한다. 

<br>

```js
console.log(foo);    // undefined

foo = 123;
console.log(foo);    // 123

var foo;
```
위의 코드를 살펴보면,
1. 변수 선언은 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 실행된다. 그래서 처음에 foo를 출력하면 변수는 선언이 되었지만 값이 할당되지 않았기 때문에 undefined가 출력된다.
2. 두번째로 foo를 출력하면 123이라는 값이 할당되었기 때문에 123이 출력된다.

<br>
<br>

## 2. let 키워드

<br>

### 2.1. 변수 중복 선언 금지
var 키워드로 이름이 동일한 변수를 선언하면 아무런 에러가 발생하지 않는다. 하지만 let 키워드로 변수를 중복 선언하면 문법 에러가 발생한다.
```js
var foo = 123;
var foo = 456;

let bar = 123;
let bar = 456;    // SyntaxError: Identifier 'bar' has already been declared
```

<br>

### 2.2. 블록 레벨 스코프
var 키워드로 선언한 변수는 오로지 함수의 코드 블록만 지역 스코프로 인정하는 함수 레벨 스코프를 따른다. 하지만 let 키워드로 선언한 변수는 모든 코드 블록(함수, if문, for문, while문, try/catch문 등)을 지역 스코프로 인정하는 **블록 레벨 스코프**를 따른다.
```js
let foo = 1;    // 전역 변수

{
  let foo = 2;    // 지역 변수
  let bar = 3;    // 지역 변수
}

console.log(foo);    // 1
console.log(bar);    // ReferenceError: bar is not defined;
```

<br>

### 2.3. 변수 호이스팅
var 키워드로 선언한 변수와 달리 let 키워드로 선언한 변수는 변수 호이스팅이 발생하지 않는 것처럼 동작한다.

<br>

```js
console.log(foo);    // ReferenceError: foo is not defined

let foo;
console.log(foo);    // undefined

foo = 1;
console.log(foo);    // 1
```
**let 키워드로 선언한 변수는 "선언 단계"와 "초기화 단계"가 분리되어 진행된다.**  즉, 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 선언 단계가 먼저 실행되지만 초기화 단계는 변수 선언문에 도달했을 때 실행된다.

let 키워드로 선언한 변수는 스코프의 시작 지점부터 초기화 단계 시작 지점(변수 선언문)까지 변수를 참조할 수 없다. 스코프의 시작 지점부터 초기화 시작 지점까지 변수를 참조할 수 없는 구간을 **일시적 사각지대(Temporal Dead Zone, TDZ)** 라고 부른다.

<br>

let 키워드로 선언한 변수는 변수 호이스팅이 발생하지 않는 것처럼 보인다. 하지만 그렇지 않다.
```js
let foo = 1;    // 전역 변수

{
  console.log(foo);    // ReferenceError: Cannot access 'foo' before initialization
  let foo = 2;    // 지역 변수
}
```
let 키워드로 선언한 변수의 경우 변수 호이스팅이 발생하지 않는다면 위 예제는 변수 foo의 값을 출력해야 한다. 하지만 let 키워드로 선언한 변수도 여전히 호이스팅이 발생하기 때문에 참조 에러가 발생한다.

<br>

### 2.4. 전역 객체와 let
var 키워드로 선언한 전역 변수와 전역 함수, 그리고 선언하지 않은 변수에 값을 할당한 암묵적 전역은 전역 객체 window의 프로퍼티가 된다. 전역 객체의 프로퍼티를 참조할 때 window를 생략할 수 있다.

let 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다. let 전역 변수는 보이지 않는 개념적인 블록내에 존재한다.
```js
let x = 1;

console.log(window.x);    // undefined
console.log(x);    // 1
```

<br>
<br>

## 3. const 키워드
const 키워드는 상수를 선언하기 위해 사용한다. const 키워드의 특징은 let 키워드와 대부분 동일하다.

<br>

### 3.1. 선언과 초기화
**const 키워드로 선언한 변수는 반드시 선언과 동시에 초기화해야 한다.**
```js
const bar = 1;
const foo;    // SyntaxError: Missing initializer in const declaration
```

<br>

const 키워드로 선언한 변수는 블록 레벨 스코프를 가지며, 변수 호이스팅이 발생하지 않는 것처럼 동작한다.
```js
{
  // 변수 호이스팅이 발생하지 않는 것처럼 동작한다.
  console.log(foo);    // ReferenceError: Cannot access 'foo' before initialization
  const foo = 1;
  console.log(foo);    // 1
}

// 블록 레벨 스코프를 갖는다.
console.log(foo);    // ReferenceError: foo is not defined
```

<br>

### 3.2. 재할당 금지
**const 키워드로 선언한 변수는 재할당이 금지된다.**
```js
const foo = 1;
foo = 2;    // TypeError: Assignment to constant variable
```

<br>

### 3.3. 상수
const 키워드로 선언한 변수에 원시 값을 할당한 경우 변수 값을 변경할 수 없다. 변수의 상대 개념인 **상수는 재할당이 금지된 변수를 말한다.** 상수는 상태 유지와 가독성, 유지보수의 평의를 위해 적극적으로 사용해야 한다.

**const 키워드로 선언된 변수에 원시 값을 할당한 경우 원시 값은 변경할 수 없는 값이고 const 키워드에 의해 재할당이 금지되므로 할당된 값을 변경할 수 있는 방법은 없다.** 

일반적으로 상수의 이름은 대문자로 선언해 상수임을 명확히 나타내고 스네이크 케이스로 표현한다.
```js
const TAX_RATE = 0.1;

let preTaxPrice = 100;

let afterTaxPrice = preTaxPrice + (preTaxPrice * TAX_RATE);

console.log(afterTaxPrice);    // 110
```

<br>

### 3.4. const 키워드와 객체 
**const 키워드로 선언된 변수에 객체를 할당한 경우 값을 변경할 수 있다.** 변경 불가능한 값인 원시 값은 재할당 없이 변경(교체)할 수 있는 방법이 없지만 변경 가능한 값이 객체는 재할당 없이도 직접 변경이 가능하기 때문이다.
```js
const person = {
  name: 'Lee'
};

person.name = 'Kim';

console.log(person);    // {name: "Kim"}
```
**const 키워드는 재할당을 금지할 뿐 "불변"을 의미하지는 않는다.** 다시 말해, 새로운 값을 재할당 하는 것은 불가능하지만 프로퍼티 동적 생성, 삭제, 프로퍼티 값의 변경을 통해 객체를 변경하는 것은 가능하다. 이때 객체가 변경되더라도 변수에 할당된 참조 값은 변하지 않는다.

<br>
<br>

## 4. var vs. let vs. const
변수 선언에는 기본적으로 const를 사용하고 let은 재할당이 필요한 경우에 한정해 사용하는 것이 좋다. const 키워드를 사용하면 의도치 않은 재할당을 방지하기 때문에 안전하다.
- ES6를 사용한다면 var 키워드는 사용하지 않는다.
- 재할당이 필요한 경우에 한정해 let 키워드를 사용한다. 이때 변수의 스코프는최대한 좁게 만든다.
- 변경이 발생하지 않고 읽기 전용으로 사용하는(재할당이 필요 없는 상수) 원시 값과 객체에는 const 키워드를 사용한다. const 키워드는 재할당을 금지하므로 let, var 키워드보다 안전하다.

<br>
<br>