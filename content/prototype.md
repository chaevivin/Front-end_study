# 프로토타입
자바스크립트는 명령형(imperative), 함수형(functional), 프로토타입 기반(prototype-based) 객체지향 프로그래밍(OOP, Object Oriented Programming)을 지원하는 멀티 패러다임 프로그래밍 언어다.

자바스크립트는 객체 기반의 프로그래밍 언어이며 **자바스크립트를 이루고 있는 거의 "모든 것"이 객체다.** 원시 타입의 값을 제외한 나머지 값들(함수, 배열, 정규 표현식 등)은 모두 객체다.

<br>

## 1. 객체지향 프로그래밍
- **여러 개의 독립적 단위, 즉 객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임**
- 객체지향 프로그래밍은 특징이나 성질을 나타내는 **속성(attribute/property)** 을 가지고 있다. 
- **추상화(abstraction)** : 다양한 속성 중에서 프로그램에 필요한 속성만 간추려 표현하는 것

<br>

```js
const person = {
  name: 'Lee',
  address: 'Seoul'
};

console.log(person);    // {name: "Lee", address: "Seoul"}
```
위의 코드를 살펴보면,
- 이름과 주소 속성으로 표현된 객체 person
- 객체 : **속성을 통해 여러 개의 값을 하나의 단위로 구성한 복합적인 자료구조**

<br>

```js
const circle = {
  radius: 5,
  // 원의 지름
  getDiameter() {
    return 2 * this.radius;
  },

  // 원의 둘레
  getPerimeter() {
    return 2 * Math.PI * this.radius;
  },

  // 원의 넓이 
  getArea() {
    return Math.PI * this.radius ** 2;
  }
};

console.log(circle);    // {radius: 5, getDiameter: f, getPerimeter: f, getArea: f}

console.log(circle.getDiameter());    // 10
console.log(circle.getPerimeter());    // 31.415
console.log(circle.getArea());    // 78.539
```
위의 코드를 살펴보면,
- 원(객체)
  - 반지름(속성) : **원의 상태를 나타내는 데이터** -> 프로퍼티
  - 지름, 둘레, 넓이 : **동작** -> 메서드

이처럼 객체지향 프로그래밍은 객체의 **상태(state)** 를 나타내는 데이터와 상태 데이터를 조작할 수 있는 **동작(behavior)** 을 하나의 논리적 단위로 묶어 생각한다. 따라서 객체는 **상태 데이터와 동작을 하나의 논리적 단위로 묶은 복합적인 자료구조** 라 할 수 있다.

객체는 고유의 기능을 갖는 독립적인 부품으로 볼 수 있지만 자신의 고유한 기능을 수행하면서 다른 객체와 관계성을 가질 수 있다. 

<br>
<br>

## 2. 상속과 프로토타입
- 상속(inheritance) : 어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것
- 자바스크립트는 프로토타입 기반으로 상속을 구현하여 중복 제거
  - 중복 제거 방법 : 기존의 코드를 적극적으로 재사용
  - 코드를 재사용하면 개발 비용을 현저히 줄일 수 있음

<br>

```js
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
  this.getArea = function () {
    return Math.PI * this.radius ** 2;
  };
}

const circle1 = new Circle(1);
const circle2 = new Circle(2);

console.log(circle1.getArea === circle2.getArea);    // false
console.log(circle1.getArea());    // 3.145
console.log(circle2.getArea());    // 12.566
```
위의 코드를 살펴보면,
- Circle 생성자 함수는 인스턴스를 생성할 때마다 내용이 동일한 getArea 메서드 중복 생성
- 모든 인스턴스가 내용이 동일한 getArea 메서드를 중복 소유
  - 이는 메모리를 불필요하게 낭비
  - 인스턴스를 생성할 때마다 똑같은 메서드를 생성하므로 퍼포먼스에도 악영향

<br>

```js
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
}

Circle.prototype.getArea = function () {
  return Math.PI * this.radius ** 2;
};

const circle1 = new Circle(1);
const circle2 = new Circle(2);

console.log(circle1.getArea === circle2.getArea);    // true
console.log(circle1.getArea());    // 3.145
console.log(circle2.getArea());    // 12.566
```
위의 코드를 살펴보면,
- Circle 생성자 함수가 생성한 모든 인스턴스가 getArea 메서드를 공유해서 사용할 수 있도록 **프로토타입** 에 추가
- 프로토타입은 Circle 생성자 함수의 prototype 프로퍼티에 바인딩되어 있음
- Circle 생성자 함수가 생성한 모든 인스턴스는 부모 객체 역할을 하는 프로토타입 Circle.prototype으로부터 getArea 메서드를 상속받음
- Circle 생성자 함수가 생성하는 모든 인스턴스는 하나의 getArea 메서드를 공유

<br>

상속은 코드의 재사용 관점에서 매우 유용하다. 생성자 함수가 생성할 모든 인스턴스가 공통적으로 사용할 프로퍼티나 메서드를 프로토타입에 미리 구현해 두면 상위 객체인 프로토타입의 자산을 공유하여 사용할 수 있다.

<br>
<br>

## 3. 프로토타입 객체
- 프로토타입 객체(프로토타입) : 객체 간 상속을 구현하기 위해 사용
  - 어떤 객체의 상위(부모) 객체의 역할을 하는 객체로서 다른 객체에 공유 프로퍼티나 메서드를 제공
  - 프로토타입을 상속받은 하위(자식) 객체는 상위 객체의 프로퍼티를 자유롭게 사용 가능
- 모든 객체는 하나의 프로토타입을 갖는다. 
  - 프로토타입 참조가 null인 객체는 프로토타입이 없다.
- 객체와 프로토타입과 생성자 함수는 서로 연결

<br>

- 모든 객체는 [[Prototype]]이라는 내부 슬롯을 가진다.
- 이 내부 슬롯의 값은 프로토타입 참조다.
- 객체가 생성될 때 객체 생성 방식에 따라 프로토타입이 결정되고 [[Prototype]]에 저장된다.
- [[Prototype]]에 직접 접근 불가, \_\_proto\_\_ 접근자 프로퍼티를 통해 간접적으로 접근

<br>

### 3.1. \_\_proto\_\_ 접근자 프로퍼티
**모든 객체는 \_\_proto\_\_ 접근자 프로퍼티를 통해 자신의 프로토타입, 즉 [[Prototype]] 내부 슬롯에 간접적으로 접근할 수 있다.**

<br>

1. \_\_proto\_\_는 접근자 프로퍼티다.

접근자 프로퍼티는 자체적으로 값([[Value]] 프로퍼티 어트리뷰트)을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수, 즉 [[Get]], [[Set]] 프로퍼티 어트리뷰트로 구성된 프로퍼티다.

Object.prototype의 접근자 프로퍼티인 \_\_proto\_\_는 getter/setter 함수라고 부르는 접근자 함수를 통해 [[Prototype]] 내부 슬롯의 값, 즉 프로토타입을 취득하거나 할당한다.

- \_\_proto\_\_ 접근자 프로퍼티로 프로토타입 접근 -> \_\_proto\_\_ 접근자 프로퍼티의 getter 함수인 [[Get]] 호출
- \_\_proto\_\_ 접근자 프로퍼티로 새로운 프로토타입 할당 -> \_\_proto\_\_ 접근자 프로퍼티의 setter 함수인 [[Set]] 호출

```js
const obj = {};
const parent = { x: 1 };

// getter 함수인 get __proto__가 호출되어 obj 객체의 프로토타입 취득
obj.__proto__;
// setter 함수인 set __proto__가 호출되어 obj 객체의 포로토타입 교체
obj.__proto__ = parent;

console.log(obj.x);    // 1
```

<br>

2. \_\_proto\_\_ 접근자 프로퍼티는 상속을 통해 사용된다.

\_\_proto\_\_ 접근자 프로퍼티는 객체가 직접 소유하는 프로퍼티가 아니라 모든 객체의 프로토타입 객체인 Object.property의 프로퍼티다. 모든 객체는 **상속을 통해 Object.prototype.\_\_proto\_\_ 접근자 프로퍼티를 사용할 수 있다.**
```js
const person = { name: 'Lee' };

// 객체는 __proto__ 프로퍼티 소유 X
console.log(person.hasOwnProperty('__proto__'));    // false

// __proto__ 프로퍼티는 Object.property의 접근자 프로퍼티
console.log(Object.getOwnPropertyDescriptor(Object.prototype, '__proto__'));    // {get: f, set: f, enumerable: false, configurable: true}

// 모든 객체는 Object.prototype의 접근자 프로퍼티인 __proto__를 상속받아 사용 가능
console.log({}.__proto__ === Object.prototype);    // true
```

<br>

3. \_\_proto\_\_ 접근자 프로퍼티를 통해 프로토타입에 접근하는 이유

[[Prototype]] 내부 슬롯의 값, 즉 프로토타입에 접근하기 위해 접근자 프로퍼티를 사용하는 이유는 **상호 참조에 의해 프로토타입 체인이 생성되는 것을 방지** 하기 위해서다.

<br>

```js
const parent = {};
const child = {};

// child의 프로토타입 = parent
child.__proto__ = parent;
// parent 프로토타입 = child
parent.__proto__ = child;    // TypeError: Cyclic __proto__ value
```
위의 코드를 살펴보면,
- child와 parent는 서로를 프로토타입으로 설정 -> 순환 참조하는 프로토타입 체인 생성 (error 발생)
- 프로토타입 체인은 단방향 링크드 리스트로 구현되어야 한다. 즉, 검색 방향이 한쪽 방향으로 흘러가야 한다.
- 순환 참조하는 프로토타입 체인은 체프로토타입 체인 종점이 존재하지 않기 때문에 프로퍼티를 검색할 때 무한루프에 빠진다.
  - **무조건적으로 프로토타입을 교체할 수 없도록 \_\_proto\_\_ 접근자 프로퍼티를 통해 프로토타입에 접근 및 교체**

<br>

4. \_\_proto\_\_ 접근자 프로퍼티를 코드 내에서 직접 사용하는 것은 권장하지 않는다.

\_\_proto\_\_ 접근자 프로퍼티를 코드 내에서 직접 사용하는 것을 권장하지 않는 이유는 **모든 객체가 \_\_proto\_\_ 접근자 프로퍼티를 사용할 수 있는 것은 아니기 때문이다.**

직접 상속을 통해 Object.prototype을 상속받지 않은 객체를 생성할 수도 있기 때문이다.

<br>

```js
const obj = Object.create(null);
console.log(obj.__proto__);    // undefined

console.log(Object.getPrototypeOf(obj));    // null
```
위의 코드를 살펴보면,
- obj는 프로토타입 체인 종점 -> Object.prototype 상속 불가능 -> \_\_proto\_\_ 접근자 프로퍼티 사용 불가능
- \_\_proto\_\_ 접근자 프로퍼티 대신 프로토타입 참조를 취득하고 싶으면 **Object.getPrototypeOf 메서드 사용**
- 프로토타입 교체를 하고 싶으면 **Object.setPrototypeOf 메서드 사용**
```js
const obj = {};
const parent = { x: 1 };

// obj 객체의 프로토타입 취득
Object.getPrototypeOf(obj);    // obj.__proto__;
// obj 객체의 프로토타입 교체
Object.sedtPrototypeOF(obj, parent);    // obj.__proto__ = parent;

console.log(obj.x);    // 1
```

<br>

### 3.2. 함수 객체의 prototype 프로퍼티
**함수 객체만이 소유하는 prototype 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다.**
```js
(function() {}).hasOwnProperty('prototype');    // true

// 일반 객체
({}).hasOwnProperty('prototype');    // false

// 화살표 함수
const Person = name => {
  this.name = name;
};
console.log(Person.hasOwnProperty('prototype'));    // false

// ES6 메서드 축약 표현
const obj = {
  foo() {}
};
console.log(obj.foo.hasOwnProperty('prototype'));    // false
```
위의 코드를 살펴보면,
- prototype 프로퍼티는 생성자 함수가 생성할 객체(인스턴스)의 프로토타입을 가리킨다.
- 생성자 함수로서 호출할 수 없는 함수, 즉 non-constructor이면 prototype 프로퍼티를 갖지 않고 프로토타입도 생성하지 않는다.

<br>

**모든 객체가 가지고 있는 \_\_proto\_\_ 접근자 프로퍼티와 함ㅅ후 객체만이 가지고 있는 prototype 프로퍼티는 결국 동일한 프로토타입을 가리킨다.** 하지만 프로퍼티를 사용하는 주체가 다르다.
| 구분 | 소유 | 값 | 사용 주체 | 사용 목적 |
| :--- | :--- | :--- | :--- | :--- |
| \_\_proto\_\_ 접근자 프로퍼티 | 모든 객체 | 프로토타입의 참조 | 모든 객체 | 객체가 자신의 프로토타입에 접근 또는 교체하기 위해 사용 |
| prototype 프로퍼티 | constructor | 프로토타입의 참조 | 생성자 함수 | 생성자 함수가 자신이 생성할 객체(인스턴스)의 프로토타입을 할당하기 위해 사용 |

<br>

### 3.3. 프로토타입의 constructor 프로퍼티와 생성자 함수
- 모든 프로토타입은 constructor 프로퍼티를 갖는다.
- constructor 프로퍼티는 자신을 참조하고 있는 생성자 함수를 가리킨다.
  - 이 연결은 생성자 함수가 생성될 때, 즉 함수 객체가 생성될 때 이루어진다.

<br>

```js
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person('Lee');

console.log(me.constructor === Person);    // true
```
위의 코드를 살펴보면,
1. Person 생성자 함수가 me 객체 생성
2. me 객체는 프로토타입의 constructor 프로퍼티를 통해 생성자 함수와 연결
3. me 객체에는 constructor 프로퍼티가 없지만, me 객체의 프로토타입인 Person.prototype의 constructor 프로퍼티를 상속받아 이용

<br>
<br>

## 4. 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입
constructor 프로퍼티가 가리키는 생성자 함수는 인스턴스를 생성한 생성자 함수다. 

하지만 리터럴 표기법에 의한 객체 생성 방식과 같이 명시적으로 new 연산자와 함께 생성자 함수를 호출하여 인스턴스를 생성하지 않는 객체 생성 방식도 있다.

```js
// 객체 리터럴
const obj = {};

// 함수 리터럴
const add = function (a, b) { return a + b; };

// 배열 리터럴
const arr = [1, 2, 3];

// 정규 표현식 리터럴
const regexp = /is/ig;
```

<br>

객체 리터럴에 의해 생성된 객체는 Object 생성자 함수가 생성한 객체가 아니다.
```js
const obj = {};
console.log(obj.constructor === Object);    // true
```
위의 코드를 살펴보면,
- obj 객체는 Object 생성자 함수로 생성한 객체가 아니라 객체 리터럴로 생성
- obj 객체의 생성자 함수는 Object 생성자 함수

<br>

리터럴 표기법에 의해 생성된 객체도 상속을 위해 프로토타입이 필요하다. 따라서 리터럴 표기법에 의해 생성된 객체도 가상적인 생성자 함수를 갖는다. 프로토타입은 생성자 함수와 함께 생성되며 prototype, constructor 프로퍼티에 의해 연결되어 있기 때문이다. 다시 말해, **프로토타입과 생성자 함수는 단독으로 존재할 수 없고 항상 쌍으로 존재한다.**

<br>

리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입
| 리터럴 표기법 | 생성자 함수 | 프로토타입|
| :---: | :---: | :---: |
| 객체 리터럴 | Object | Object.prototype |
| 함수 리터럴 | Function | Function.prototype |
| 배열 리터럴 | Array | Array.prototype |
| 정규 표현식 리터럴 | RegExp | RegExp.prototype |

<br>
<br>

## 5. 프로토타입 생성 시점
객체는 리터럴 표기법 또는 생성자 함수에 의해 생성되므로 결국 모든 객체는 생성자 함수와 연결되어 있다.

**프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성된다.** 생성자 함수는 사용자 정의 생성자 함수와 자바스크립트 빌트인 생성자 함수로 구분할 수 있다.

<br>

### 5.1. 사용자 정의 생성자 함수와 프로토타입 생성 시점
**생성자 함수로서 호출할 수 있는 함수, 즉 constructor는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 함께 생성된다.** 생성자 함수로서 호출할 수 없는 함수, 즉 non-constructor는 프로토타입이 생성되지 않는다.
```js
console.log(Person1.prototype);    // {constructor: f}

// 생성자 함수
function Person1(name) {
  this.name = name;
}

// non-constrctor
const Person2 = name => {
  this.name = name;
};

console.log(Person2.prototype);    // undefined
```
위의 코드를 살펴보면,
- 함수 선언문은 런타임 이전에 실행되어 함수 객체를 만든다.
- 이때 프로토타입도 함께 생성된다.
- 생성된 프로토타입은 Person1 생성자 함수의 prototype에 바인딩된다.
- 생성된 프로토타입은 오직 constructor 프로퍼티만을 갖는 객체다.
- 생성된 프로토타입의 프로토타입은 Object.prototype

<br>

이처럼 사용자 정의 생성자 함수는 자신이 평가되어 함수 객체로 생성되는 시점에 프로토타입도 더불어 생성되며, 생성된 프로토타입의 프로토타입은 언제나 Object.prototype이다.

<br>

### 5.2. 빌트인 생성자 함수와 프로토타입 생성 시점
- Object, String, Number, Function, Array, RegExp, Date, Promise 등과 같은 빌트인 생성자 함수는 함수가 생성되는 시점에 프로토타입이 생성 
- 모든 빌트인 생성자 함수는 전역 객체가 생성되는 시점에 생성
- 생성된 프로토타입은 빌트인 생성자 함수의 prototype 프로퍼티에 바인딩
- 객체가 생성되기 이전에 생성자 함수와 프로토타입은 이미 객채화되어 존재
- **이후 생성자 함수 또는 리터럴 표기법으로 객체를 생성하면 프로토타입은 생성된 객체의 [[Prototype]] 내부 슬롯에 할당**
- 생성된 객체는 프로토타입을 상속받음

<br>
<br>

## 6. 객체 생성 방식과 프로토타입 결정
객체 생성 방법
- 객체 리터럴
- Object 생성자 함수
- 생성자 함수
- Object.create 메서드
- 클래스(ES6)

-> 객체 생성 방식의 차이는 있으나 추상 연산 OrdinaryObjectCreate에 의해 생성된다는 공통점

<br>

OrdinaryObjectCreate
- 필수적으로 자신이 생성할 객체의 프로토타입을 인수로 전달받음
- 자신이 생성할 객체에 추가할 프로퍼티 목록을 옵션으로 전달 가능
- 빈 객체 생성 -> 객체에 추가할 프로퍼티 목록이 인수로 전달된 경우 -> 프로퍼티에 객체 추가
- 프로토타입은 추상 연산 OrdinaryObjectCreate에 전달되는 인수에 의해 결정. 
  - 인수는 객체가 생성되는 시점에 객체 생성 방식에 의해 결정

<br>

### 6.1. 객체 리터럴에 의해 생성된 객체의 프로토타입
- 객체 리터럴이 평가 -> 추상 연산 OrdinaryObjectCreate에 의해 Object 생성자 함수와 Object.prototype과 생성된 객체 사이에 연결 만들어짐
- 객체 리터럴에 의해 생성된 객체는 Object.prototype을 프로토타입으로 가짐 -> Object.prototype 상속받음
- 객체 리터럴에 의해 생성된 객체는 constructor 프로퍼티와 hasOwnProperty 메서드 등을 소유 X. 하지만 Object.prototype의 constructor 프로퍼티와 hasOwnProperty 메서드 사용 가능
```js
const obj = { x: 1 };

console.log(obj.constructor === Object);    // true
console.log(obj.hasOwnProperty('x'));    // true
```

<br>

### 6.2. Object 생성자 함수에 의해 생성된 객체의 프로토타입
- Object 생성자 함수 호출 -> 추상 연산 OrdinaryObjectCreate 호출 -> 생성자 함수와 Object.prototype과 생성된 객체 사이에 연결이 만들어짐
- 이때 OrdinaryObjectCreate에 전달되는 프로토타입은 Object.prototype
- Object 생성자 함수에 의해 생성되는 객체의 프로토타입은 Object.prototype
```js
const obj = new Object();
obj.x = 1;

console.log(obj.constructor === Object);    // true
console.log(obj.hasOwnProperty('x'));    // true
```

<br>

객체 리터럴을 사용한 객체 생성 방식과 Object 생성자 함수에 의한 객체 생성 방식의 차이점
- 프로퍼티를 추가하는 방식
  - 객체 리터럴 : 객체 리터럴 내부에 프로퍼티 추가
  - Object 생성자 함수 : 빈 객체 생성 후 프로퍼티 추가

<br>

### 6.3. 생성자 함수에 의해 생성된 객체의 프로토타입
- 생성자 함수 호출하여 인스턴스 생성 -> 추상 연산 OrdinaryObjectCreate 호출 -> 생성자 함수와 생성자 함수의 prototype 프로퍼티에 바인딩 되어 있는 객체와 생성된 객체 사이에 연결이 만들어짐
- 이때 OrdinaryObjectCreate에 전달되는 프로토타입은 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체
- 생성자 함수와 생성된 프로토타입의 프로퍼티는 constructor 밖에 없다.

<br>

```js
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi! My name is ${this.name}`);
};

const me = new Person('Lee');
const you = new Person('Kim');

me.sayHello();    // Hi! My name is Lee
you.sayHello();    // Hi! My name is Kim
```
위의 코드를 살펴보면,
- 프로토타입은 객체이기 때문에 Person.prototype에 프로퍼티를 추가
- Person 생성자 함수를 통해 생성된 객체는 프로토타입의 sayHello 메서드 상속받아 사용
- 추가된 프로퍼티는 프로토타입 체인에 즉각 반영

<br>
<br>

## 7. 프로토타입 체인
```js
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
   console.log(`Hi! My name is ${this.name}`);
};

const me = new Person('Lee');

console.log(me.hasOwnProperty('name'));    // true
```
위의 코드를 살펴보면,
- Person 생성자 함수에 의해 생성된 객체 me는 Object.prototype의 메서드인 hasOwnProperty 호출 가능
- me 객체가 Person.prototype, Object.prototype 상속
- me 객체의 프로토타입은 Person.prototype
- Person.prototype의 프로토타입은 Object.prototype

<br>

**프로토타입 체인 : 자바스크립트는 객체의 프로퍼티와 메서드에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티가 없다면 [[Prototype]] 내부 슬롯의 참조를 따라 자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색하는 것을 말한다. 프로토타입 체인은 자바스크립트가 객체지향 프로그래밍의 상속을 구현하는 메커니즘이다.**

- 프로토타입 체인의 최상위 객체 : Object.prototype
  - **Object.prototype을 프로토타입 체인의 종점(end of prototype chain)이라고 부름**
- 모든 객체는 Object.prototype을 상속
- Object.prototype의 프로토타입, 즉 [[Prototype]] 내부 슬롯의 값은 null
- Object.prototype에서도 프로퍼티를 겁색할 수 없는 경우는 undefined 반환

<br>

프로토타입 체인과 스코프 체인
- 자바스크립트 엔진은 프로토타입 체인을 따라 프로퍼티와 메서드 검색
- 자바스크립트 엔진은 객체 간의 상속 관계로 이루어진 프로토타입의 계층적인 구조에서 객체의 프로퍼티 검색
- **프로토타입 체인은 상속과 프로퍼티 검색을 위한 메커니즘**
- 프로퍼티가 아닌 식별자는 스코프 체인에서 검색
- 자바스크립트 엔진은 함수의 중첩 관계로 이루어진 스코프의 계층적인 구조에서 식별자 검색
- **스코프 체인은 식별자 검색을 위한 메커니즘**
- **스코프 체인과 프로토타입 체인은 서로 연관없이 별도로 동작하는 것이 아니라 서로 협력하여 식별자와 프로퍼티를 검색하는데 사용**

<br>
<br>

## 8. 오버라이딩과 프로퍼티 섀도잉
```js
const Person = (function () {
  // 생성자 함수
  function Person(name) {
    this.name = name;
  }

  // 프로토타입 메서드
  Person.prototype.sayHello = function () {
    console.log(`Hi! My name is ${this.name}`);
  };

  // 생성자 함수를 반환
  return Person;
}());

const me = new Person('Lee');

// 인스턴스 메서드
me.sayHello = function() {
  console.log(`Hey! My name is ${this.name}`);
};

me.sayHello();    // Hey! My name is Lee

delete me.sayHello;
me.sayHello();    // Hi! My name is Lee

delete me.sayHello;
me.sayHello();    // Hi! My name is Lee

delete Person.prototype.sayHello;
me.sayHello();    // TypeError: me.sayHello is not a function
```
위의 코드를 살펴보면,
1. 프로토타입 프로퍼티와 같은 이름의 프로퍼티를 인스턴스에 추가하면 프로토타입 체인을 따라 프로토타입 프로퍼티를 검색하여 프로토타입을 덮었는 것이 아니라 인스턴스 프로퍼티로 추가
2. 인스턴스 메서드 sayHello는 프로토타입 메서드 sayHello 오버라이딩
    - 오버라이딩 : 상위 클래스가 가지고 있는 메서드를 하위 클래스가 재정의하여 사용
3. 프로토타입 메서드 sayHello는 가려짐
    - 상속 관계에 의해 프로퍼티가 가려지는 현상을 **프로퍼티 섀도잉**이라 함
4. 프로퍼티 삭제도 마찬가지로 인스턴스 메서드 sayHello가 삭제
5. sayHello를 다시 삭제해도 프로토타입 메서드 sayHello는 삭제되지 않음
    - 하위 객체를 통해 프로토타입의 프로퍼티를 변경하거나 삭제는 불가능
    - get 액세스 허용, set 액세스 허용 X
6. 프로토타입 프로퍼티를 변경하거나 삭제하려면 프로토타입에 직접 접근

<br>
<br>

## 9. 프로토타입의 교체
- 부모 객체인 프로토타입을 동적으로 변경 가능
- 위를 활용하여 객체 간의 상속 관계를 동적으로 변경 가능
- 프로토타입은 생성자 함수 또는 인스턴스에 의해 교체

<br>

### 9.1. 생성자 함수에 의한 프로토타입의 교체 
```js
const Person = (function () {
  function Person(name) {
    this.name = name;
  }

  // ①
  Person.prototype = {
    sayHello() {
      console.log(`Hi! My name is ${this.name}`);
    }
  };

  return Person;
}());

const me = new Person('Lee');

console.log(me.constructor === Person);    // fasle
console.log(me.constructor === Obejct);    // true
```
위의 코드를 살펴보면,
- ①에서 Person.prototype에 객체 리터럴을 할당. 이는 Person 생성자 함수가 생성할 객체의 프로토타입을 객체 리터럴로 교체한 것.
- 프로토타입으로 교체한 객체 리터럴에는 constructor 프로퍼티가 없음
  - constructor 프로퍼티는 자바스크립트 엔진이 프로토타입을 생성할 때 암묵적으로 추가한 프로퍼티
- me 객체의 생성자 함수를 검색하면 Person이 아닌 Object
- 프로토타입을 교체하면 constructor 프로퍼티와 생성자 함수 간의 연결 파괴

<br>

```js
const Person = (function () {
  function Person(name) {
    this.name = name;
  }

  // 프로토타입 교체
  Person.prototype = {
    // 연결 다시 설정
    constructor: Person,
    sayHello() {
      console.log(`Hi! My name is ${this.name}`);
    }
  };

  return Person;
}());

const me = new Person('Lee');

console.log(me.constructor === Person);    // true
console.log(me.constructor === Obejct);    // false
```
위의 코드를 살펴보면,
- 프로토타입으로 교체한 객체 리터럴에 constructor 프로퍼티 추가 -> 프로토타입의 constructor 프로퍼티 되살림 -> constructor 프로퍼티와 생성자 함수 간의 연결 되살림

<br>

### 9.2. 인스턴스에 의한 프로토타입 교체
- 프로토타입은 생성자 함수의 prototype 프로퍼티와 \_\_proto\_\_ 접근자 프로퍼티를 통해 접근 가능
- 생성자 함수의 prototype 프로퍼티에 다른 임의의 객체를 바인딩하는 것은 **생성할 인스턴스의 프로토타입을 교체하는 것**
- \_\_proto\_\_ 접근자 프로퍼티를 통해 프로토타입을 교체하는 것은 **이미 생성된 객체의 프로토타입을 교체하는 것**

<br>

```js
function Person(name) {
  this.name = name;
}

const me = new Person('Lee');

// 프로토타입으로 교체할 객체
const parent = {
  sayHello() {
    console.log(`Hi! My name is ${this.name}`);
  }
};

Object.setPrototypeOf(me, parent);
// me.__proto__ = parent; 와 동일

me.sayHello();    // Hi! My name is Lee

console.log(me.constructor === Person);    // false
console.log(me.constructor === Object);    // true
```
위의 코드를 살펴보면,
- setPrototypeOf를 사용하여 me 객체의 프로토타입을 parent 객체로 교체
- 프로토타입으로 교체한 객체에는 constructor 프로퍼티가 없으므로 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴
- 프로토타입의 constructor 프로퍼티로 me 객체의 생성자 함수를 검색하면 Person이 아닌 Object가 나옴

<br>

생성자 함수에 의한 프로토타입 교체와 인스턴스에 의한 프로토타입 교체의 차이점
- 생성자 함수 : 생성자 함수의 prototype 프로퍼티가 교체된 프로토타입을 가리킨다.
- 인스턴스 : 생성자 함수의 prototype 프로퍼티가 교체된 프로토타입을 가리키지 않는다.

<br>

```js
function Person(name) {
  this.name = name;
}

const me = new Person('Lee');

// 프로토타입으로 교체할 객체
const parent = {
  // 연결 다시 설정
  constructor: Person,
  sayHello() {
    console.log(`Hi! My name is ${this.name}`);
  }
};

// 연결 다시 설정
Person.prototype = parent;

Object.setPrototypeOf(me, parent);
// me.__proto__ = parent; 와 동일

me.sayHello();    // Hi! My name is Lee

console.log(me.constructor === Person);    // true
console.log(me.constructor === Object);    // false

console.log(Person.prototype === Object.getPrototypeOf(me));    // true
```
위의 코드를 살펴보면,
- 프로토타입으로 교체할 객체에서 constructor 프로퍼티와 생성자 함수 간의 연결 설정
- 생성자 함수의 prototype 프로퍼티와 프로토타입 간의 연결 설정
- 생성자 함수의 prototype 프로퍼티가 교체된 프로퍼티를 가리킴

<br>

프로토타입 교체를 통해 객체 간의 상속 관계를 동적으로 변경하는 것은 꽤나 번거롭기 때문에 직접 교체하지 않는 것이 좋다.