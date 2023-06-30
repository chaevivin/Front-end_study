# 생성자 함수에 의한 객체 생성

## 1. Object 생성자 함수
new 연산자와 함께 Object 생성자 함수를 호출하면 빈 객체를 생성하여 반환하고, 프로퍼티 또는 메서드를 추가할 수 있다.
```js
// 빈 객체 생성
const person = new Object();

// 프로퍼티 추가
person.name = 'Lee';
person.sayHello = function() {
  console.log('Hi! My name is ' + this.name);
};

console.log(person);    // {name: "Lee", sayHello: f}
person.sayHello();    // Hi! My name is Lee
```

<br>

생성자 함수(constructor)란 new 연산자와 함께 호출하여 객체(인스턴스)를 생성하는 함수를 말한다. 생성자 함수에 의해 생성된 객체를 인스턴스(instance)라 한다.

자바스크립트는 Object 생성자 함수 이외에도 String, Number, Boolean, Function, Array, Date, RegExp, Promise 등의 빌트인 생성자 함수를 제공한다.
```js
// 정규 표현식 생성
const regExp = new RegExp(/ab+c/i);
console.log(typeof regExp);    // object
console.log(regExp);    // /ab+c/i

// Date 객체 생성
const date = new Date();
console.log(typeof date);    // object
console.log(date);    // Fri June 30 2023 17:11:33 GMT+0900 (대한민국 표준시)
```

<br>
<br>

## 2. 생성자 함수

<br>

### 2.1. 객체 리터럴에 의한 객체 생성 방식의 문제점 
객체 리터럴에 의한 객체 생성 방식은 직관적이고 간편하다. 하지만 **객체 리터럴에 의한 객체 생성 방식은 단 하나의 객체만 생성한다.** 따라서 동일한 프로퍼티를 갖는 객체를 여러 개 생성해야 하는 경우 매번 같은 프로퍼티를 기술해야 하기 때문에 비효율적이다.
```js
const circle1 = {
  radius: 5,
  getDiameter() {
    return 2 * this.radius;
  }
};

console.log(circle1.getDiameter());    // 10

const circle2 = {
  radius: 10,
  getDiameter() {
    return 2 * this.radius;
  }
};

console.log(circle2.getDiameter());    // 20
```

<br>

객체는 프로퍼티를 통해 객체의 고유 상태(state)를 표현한다. 그리고 메서드를 통해 상태 데이터인 프로퍼티를 참조하고 조작하는 동작을 표현한다. 따라서 프로퍼티는 객체마다 프로퍼티 값이 다를 수는 있지만 메서드는 내용이 동일한 경우가 일반적이다.

하지만 객체 리터럴에 의해 객체를 생성하는 경우 프로퍼티 구조가 동일함에도 불구하고 매번 같은 프로퍼티와 메서드를 기술해야 한다. 만약 수십 개의 객체를 생성해야 한다면 문제가 크다.

<br>

### 2.2. 생성자 함수에 의한 객체 생성 방식의 장점
생성자 함수에 의한 객체 생성 방식은 마치 객체(인스턴스)를 생성하기 위한 템플릿(클래스)처럼 **생성자 함수를 사용하여 프로퍼티 구조가 동일한 객체 여러 개를 간편하게 생성할 수 있다.**
```js
// 생성자 함수
function Circle(radius) {
  // this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// 인스턴스 생성
const circle1 = new Circle(5);
const circle2 = new Circle(10);

console.log(circle1.getDiameter());    // 10
console.log(circle2.getDiameter());    // 20
```

<br>

> this는 객체 자신의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수다. this가 가리키는 값, 즉 **this 바인딩은 함수 호출 방식에 따라 동적으로 결정된다.** 

| 함수 호출 방식 | this가 가리키는 값 (this 바인딩) |
| :---: | :---: |
| 일반 함수로서 호출 | 전역 객체 |
| 메서드로서 호출 | 메서드를 호출한 객체 (마침표 앞의 객체) |
| 생성자 함수로서 호출 | 생성자 함수가 생성할 인스턴스 |

<br>

생성자 함수는 이름 그대로 객체(인스턴스)를 생성하는 함수다. 일반 함수와 동일한 방법으로 생성자 함수를 정의하고 **new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작한다.** 만약 new 연산자와 함께 생성자 함수를 호출하지 않으면 생성자 함수가 아니라 일반 함수로 동작한다.
```js
// 일반 함수로 동작
const circle3 = Circle(15);

// 일반 함수로 동작하기 때문에 반환문이 없으므로 undefined
console.log(circle3);    // undefined

// 일반 함수에서 this는 전역 객체
console.log(radius);    // 15
```

<br>

### 2.3. 생성자 함수의 인스턴스 생성 과정
생성자 함수의 역할은 프로퍼티 구조가 동일한 인스턴스를 생성하기 위한 템플릿(클래스)으로서 동작하여 **인스턴스를 생성(필수)** 하는 것과 **생성된 인스턴스를 초기화(인스턴스 프로퍼티 추가 및 초기값 할당, 옵션)** 하는 것이다. 

<br>

```js
// 생성자 함수
function Circle(radius) {
  // 인스턴스 초기화
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// 인스턴스 생성
const circle1 = new Circle(5);
```
자바스크립트 엔진은 코드를 작성하지 않아도 암묵적인 처리를 통해 인스턴스를 생성하고 반환한다. 

<br>

1. 인스턴스 생성과 this 바인딩
    - 암묵적으로 빈 객체가 생성된다. 이 빈 객체가 생성자 함수가 생성한 인스턴스다. 그리고 인스턴스는 this에 바인딩된다. 이 처리는 런타임 이전에 실행된다.
2. 인스턴스 초기화
    - 생성자 함수의 코드가 한 줄씩 실행되어 this에 바인딩되어 있는 인스턴스를 초기화한다. 즉, this에 바인딩되어 있는 인스턴스에 프로퍼티나 메서드를 추가하고 생성자 함수가 인수로 전달받은 초기값을 인스턴스 프로퍼티에 할당하여 초기화하거나 고정값을 할당한다.
3. 인스턴스 반환
    - 생성자 함수 내부의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다. 만약 this가 아닌 다른 객체를 명시적으로 반환하면 this가 반환되지 못하고 return문에 명시한 객체가 반환된다. 하지만 명시적으로 원시 값을 반환하면 원시 값 반환은 무시되고 암묵적으로 this가 반환된다.
```js
function Circle(radius) {
  // 1. 암묵적으로 빈 객체 생성 후 this에 바인딩

  // 2. this에 바인딩된 인스턴스 초기화
  this.radius = radius;
  this.getDiameter = function() {
    return 2 * this.radius;
  };

  // 3. 암묵적으로 this 반환
  // 원시 값 반환 무시, this 반환
  return 100;
}

// 인스턴스 생성
const circle = new Circle(1);
console.log(circle);    // Circle {radius: 1, getDiameter: f}
```
이처럼 생성자 함수 내부에서 명시적으로 this가 아닌 다른 값을 반환하는 것은 생성자 함수의 기본 동작을 훼손한다. 따라서 **생성자 함수 내부에서 return문은 반드시 생략**해야 한다.

<br>

### 2.4. 내부 메서드 [[Call]]과 [[Const]]
함수 선언문 또는 함수 표현식으로 정의한 함수는 일반적인 함수로서 호출할 수 있고 생성자 함수로서 호출할 수 있다. (생성자 함수로서 호출 : new 연산자와 함께 호출하여 객체 생성)

함수 객체는 일반 객체가 가지고 있는 내부 슬롯과 내부 메서드를 모두 가지고 있기 때문에 일반 객체와 동일하게 동작할 수 있다.
```js
function foo() {}

foo.prop = 10;

foo.method = function () {
  console.log(this.prop);
};

foo.method();    // 10
```

<br>

함수는 객체이지만 일반 객체와는 다르다. **일반 객체는 호출할 수 없지만 함수는 호출할 수 있다.** 따라서 함수 객체는 일반 객체가 가지고 있는 내부 슬롯과 내부 메서드는 물론, 함수로서 동작하기 위해 [[Environment]], [[FormalParameters]] 등의 내부 슬롯과 [[Call]], [[Construct]] 같은 내부 메서드를 추가로 가지고 있다.

함수가 일반 함수로서 호출되면 함수 객체의 내부 메서드 [[Call]]이 호출되고 new 연산자와 함께 생성자 함수로서 호출되면 내부 메서드 [[Construct]]가 호출된다.
```js
function foo() {}

// 일반적인 함수로서 호출 : [[Call]] 호출
foo();

// 생성자 함수로서 호출 : [[Construct]] 호출
new foo();
```

<br>

- callable
  - 내부 메서드 [[Call]]을 갖는 함수 객체
  - 호출할 수 있는 객체, 즉 함수를 말한다.
- constructor
  - 내부 메서드 [[Construct]]를 갖는 함수 객체
  - 생성자 함수로서 호출할 수 있는 함수
- non-constructor
  - [[Constrcut]]를 갖지 않는 함수 객체
  - 객체를 생성자 함수로서 호출할 수 없는 함수

<br>

**함수 객체는 호출할 수 있지만 모든 함수 객체를 생성자 함수로서 호출할 수 있는 것은 아니다.** 그래서 함수 객체는 callable이어야 하고 [[Call]] 내부 메서드를 갖고 있다. 하지만 모든 함수 객체가 [[Construct]]를 갖는 것은 아니기 때문에 constructor일 수도 있고 non-constructor일 수도 있다.

<br>

### 2.5. constructor와 non-constructor의 구분
자바스크립트 엔진은 함수 정의를 평가하여 함수 객체를 생성할 때 함수 정의 방식에 따라 함수를 constructor와 non-constructor로 구분한다.
- costructor : 함수 선언문, 함수 표현식, 클래스(클래스도 함수다)
- non-constructor : 메서드(ES6 메서드 축약 표현), 화살표 함수

ECMAScript 사양에서 메서드란 ES6의 메서드 축약 표현만을 의미한다. 다시 말해 함수 정의 방식에 따라 constructor와 non-constructor를 구분한다. 주의할 것은 생성자 함수로서 호출될 것을 기대하고 정의하지 않은 일반 함수(callable이면서 constructor)에 new 연산자를 붙여 호출하면 생성자 함수처럼 동작할 수 있다는 것이다.

<br>

```js
// 일반 함수 정의 : 함수 표현식, 함수 선언문
function foo() {}
const bar = function () {};
// 메서드 x
const baz = {
  x: function () {}
};

// 일반 함수로 정의된 함수만이 constructor
new foo();    // foo {}
new bar();    // bar {}
new.baz.x();    // x {}

// 화살표 함수
const arrow = () => {};

new arrow();    // TypeError: arrow is not a constructor

const obj = {
  x() {}
};

new obj.x();    // TypeError: obj.x is not a constructor
```

<br>

### 2.6. new 연산자
new 연산자와 함께 함수를 호출하면 해당 함수는 생성자 함수로 동작한다. 다시 말해, 함수 객체의 내부 메서드 [[Call]]이 호출되는 것이 아니라 [[Construct]]가 호출된다. new 연산자와 함께 호출하는 함수는 constructor이다.

반대로 new 연산자 없이 생성자 함수를 호출하면 일반 함수로 호출된다. 다시 말해, 함수 객체의 내부 메서드 [[Construct]]가 호출되는 것이 아니라 [[Call]]이 호출된다.

**생성자 함수는 일반적으로 첫 문자를 대문자로 기술하는 파스칼 케이스로 명명하여 일반 함수와 구별할 수 있도록 한다.**

<br>

### 2.7. new.target
new.target은 this와 유사하게 constructor인 모든 함수 내부에서 암묵적인 지역 변수와 같이 사용되며 메타 프로퍼티라고 부른다.

함수 내부에서 new.target을 사용하면 new 연산자와 함께 생성자 함수로서 호출되었는지 확인할 수 있다. **new 연산자와 함께 생성자 함수로서 호출되면 함수 내부의 new.target은 함수 자신을 가리킨다. new 연산자 없이 일반 함수로서 호출된 함수 내부의 new.target은 undefined다.**

<br>

```js
// 생성자 함수
function Circle(radius) {
  if (!new.target) {
    return new Circle(radius);
  }
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

const circle = Circle(5);
console.log(circle.getDiameter());
```
위의 코드를 살펴보면,
1. Circle 함수가 new 연산자와 함께 호출되지 않았다면 new.target은 undefined이다.
2. new 연산자와 함께 생성자 함수를 재귀 호출하여 생성된 인스턴스를 반환한다.
3. new 연산자 없이 생성자 함수를 호출해도 new.target을 통해 생성자 함수로서 호출된다.

new 연산자와 함께 생성자 함수에 의해 생성된 객체(인스턴스)는 프로토타입에 의해 생성자 함수와 연결된다. 이를 이용해 new 연산자와 함께 호출되었는지 확인할 수 있다.

<br>

대부분 빌트인 생성자 함수(Object, String, Number, Boolean, Function, Array, Date, RegExp, Promise 등)는 new 연산자와 함께 호출되었는지를 확인한 후 적절한 값을 반환한다.

<br>

예를 들어, Object와 Function 생성자 함수는 new 연산자 없이 호출해도 new 연산자와 함께 호출했을 때와 동일하게 동작한다.
```js
let obj = new Object();
console.log(obj);    // {}

obj = Object();
console.log(obj);    // {}

let f = new Function('x', 'return x ** x');
console.log(f);    // f annonymous(x) { return x ** x}

f = new Function('x', 'return x ** x');
console.log(f);    // f annonymous(x) { return x ** x}
```

<br>

하지만 String, Number, Boolean 생성자 함수는 new 연산자와 함께 호출했을 때 String, Number, Boolean 객체를 생성하여 반환하지만 new 연산자 없이 호출하면 문자열, 숫자, 불리언 값을 반환한다. 이를 통해 데이터 타입을 변환하기도 한다.
```js
const str = String(123);
console.log(str, typeof str);    // 123 string

const num = Number('123');
console.log(num, typeof num);    // 123 number

const bool = Boolean('true');
console.log(bool, typeof bool);    // true boolean
```

<br>
<br>