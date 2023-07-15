# 클래스

## 1. 클래스는 프로토타입의 문법적 설탕인가?
- 자바스크립트는 프로토타입 기반 객체지향 언어
  - 프로토타입 기반 객체지향 언어 : 클래스가 필요 없는 프로그래밍 언어
  - ES5에서는 클래스 없이도 생성자 함수와 프로토타입을 통해 객체지향 언어의 상속 구현 가능
```JS
// ES5 생성자 함수
var Person = (function () {
  // 생성자 함수
  function Person(name) {
    this.name = name;
  }

  // 프로토타입 메서드
  Person.prototype.sayHi = function () {
    console.log('Hi! My name is ' + this.name);
  };

  // 생성자 함수 반환
  return Person;
}());

// 인스턴스 생성
var me = new Person('Lee');
me.sayHi();    // Hi! My name is Lee
```
- ES6에 도입된 클래스는 함수이며 기존 프로토타입 기반 패턴을 클래스 기반 패턴처럼 사용할 수 있도록 하는 문법적 설탕 역할 -> **새로운 객체 생성 메커니즘**
- 단, 클래스와 생성자 함수는 모두 프로토타입 기반의 인스턴스를 생성하지만 정확히 동일하게 동작 X
  - 클래스 생성자 함수보다 엄격하며 생성자 함수에서 제공하지 않는 기능도 제공
- 클래스 생성자 함수와 매우 유사하게 동작하지만 차이점 존재
  - 클래스를 new 연산자 없이 호출하면 에러가 발행. 하지만 생성자 함수를 new 연산자 없이 호출하면 일반 함수로서 호출
  - 클래스는 상속을 지원하는 extends와 super 키워드 제공. 하지만 생성자 함수는 extends와 super 키워드 제공 X
  - 클래스는 호이스팅이 발생하지 않는 것처럼 동작. 하지만 함수 선언문으로 정의된 생성자 함수는 함수 호이스팅이, 함수 표현식으로 정의한 생성자 함수는 변수 호이스팅이 발생
  - 클래스 내의 모든 코드에는 암묵적으로 strict mode가 지정되어 실행되며 strict mode 해제 불가. 하지만 생성자 함수는 암묵적으로 strict mode 지정 X
  - 클래스의 constructor, 프로토타입 메서드, 정적 메서드는 모두 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 false. 다시 말해, 열거 X

<br>
<br>

## 2. 클래스 정의
- 클래스는 class 키워드를 사용하여 정의
- 클래스 이름은 파스칼 케이스를 사용. 사용하지 않아도 에러 발생 X
```js
// 클래스 선언문
class Person {}
```
- 함수와 마찬가지로 표현식으로 클래스 정의 가능. 이때 클래스는 함수와 마찬가지로 이름을 가질 수도 있고, 갖지 않을 수도 있음
```js
// 익명 클래스 표현식
const Person = class {};

// 기명 클래스 표현식
const Person = class MyClass {};
```

<br>

- 클래스를 표현식으로 정의할 수 있다는 것 = 클래스가 값으로 사용할 수 있는 일급 객체라는 것
  - 무명의 리터럴로 생성 가능. 즉, 런타임에 생성 가능
  - 변수나 자료구조(객체, 배열 등)에 저장 가능
  - 함수의 매개변수에 전달 가능
  - 함수의 반환값으로 사용 가능
- 클래스는 함수이기 때문에 클래스는 값처럼 사용할 수 있는 일급 객체
- 클래스 몸체에는 0개 이상의 메서드만 정의 가능
  - 클래스 몸체에서 정의할 수 있는 메서드 : constructor(생성자), 프로토타입 메서드, 정적 메서드
- 클래스와 생성자 함수의 정의 방식은 형태적인 면에서 매우 유사
```js
// 클래스 선언문
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;    // name 프로퍼티는 public
  }

  // 프로토타입 메서드
  sayHi() {
    console.log(`Hi! My name is ${this.name}`);
  }

  // 정적 메서드
  static sayHello() {
    console.log('Hello!');
  }
}

// 인스턴스 생성
const me = new Person('Lee');

// 인스턴스의 프로퍼티 참조
console.log(me.name);    // Lee
// 프로토타입 메서드 호출
me.sayHi();    // Hi! My name is Lee
// 정적 메서드 호출
Person.sayHello();    // Hello!
```

<br>
<br>

## 3. 클래스 호이스팅
- 클래스는 함수로 평가
```js
class Person {}
console.log(typeof Person);    // function
```
- 클래스 선언문으로 정의한 클래스는 함수 선언문과 같이 소스코드 평가 과정, 즉 런타임 이전에 먼저 평가되어 함수 객체 생성
  - 이때 클래스가 평가되어 생성된 함수 객체는 생성자 함수로서 호출할 수 있는 함수, 즉 constructor
  -  생성자 함수로서 호출할 수 있는 함수는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 함께 생성
  - 프로토타입과 생성자 함수는 단독으로 존재 불가능. 언제나 쌍으로 존재
- 단, 클래스는 클래스 정의 이전에 참조 불가능
```js
console.log(Person);
// ReferenceError: Cannot access 'Person' before initialization

// 클래스 선언문
class Person {}
```

<br>

```js
const Person = '';

{
  // 호이스팅이 발생하지 않는다면 '' 출력
  console.log(Person);  // ReferenceError: Cannot access 'Person' before initialization

  // 클래스 선언문
  class Person {}
}
```
위의 코드를 살펴보면,
- 클래스 선언문도 변수 선언, 함수 정의와 마찬가지로 호이스팅 발생
- 단, 클래스는 let, const 키워드로 선언한 변수처럼 호이스팅
- 클래스 선언문 이전에 일시적 사각지대에 빠직 때문에 호이스팅이 발생하지 않는 것처럼 동작
- var, let, const, function, function*, class 키워드를 사용하여 선언된 모든 식별자는 호이스팅 -> 모든 선언문은 런타임 이전에 먼저 실행되기 때문

<br>
<br>

## 4. 인스턴스 생성
- 클래스는 생성자 함수이며 new 연산자와 함께 호출되어 인스턴스 생성
- 클래스는 인스턴스를 생성하는 것이 유일한 존재 이유이므로 반드시 new 연산자와 함께 호출
```js
class Person {}

// 인스턴스 생성
const me = new Person();
console.log(me);    // Person {}

const you = Person();    // TypeError: Class constructor Foo cannot be invoked without 'new'
```

<br>

```js
const Person = class MyClass {};

const me = new Person();
console.log(MyClass);    // ReferenceError: MyClass is not defined

const you = new MyClass();    // ReferenceError: MyClass is not defined
```
위의 코드를 살펴보면,
- 클래스 표현식으로 정의된 클래스의 경우 클래스를 가리키는 식별자(Person)를 사용해 인스턴스를 생성하지 않고 기명 클래스 표현식의 클래스 이름(MyClass)을 사용해 인스턴스를 생성하면 에러 발생
  - 기명 함수 표현식과 마찬가지로 클래스 표현식에서 사용한 클래스 이름은 외부 코드에서 접근 불가능하기 때문
  - MyClass는 함수와 동일하게 클래스 몸체 내부에서만 유효한 식별자

<br>
<br>

## 5. 메서드
- 클래스 몸체에는 0개 이상의 메서드만 정의 가능
  - 클래스 몸체에서 정의할 수 있는 메서드 : constructor(생성자), 프로토타입 메서드, 정적 메서드

<br>

### 5.1. constructor
- 인스턴스를 생성하고 초기화하기 위한 특수한 메서드
- 이름 변경 불가능
- 클래스가 평가되어 생성된 함수 객체나 클래스가 생성한 인스턴스 어디에도 constructor 메서드 존재 X
  - constructor는 메서드로 해석되는 것이 아니라 클래스가 평가되어 생성한 함수 객체 코드의 일부
  - 클래스 정의가 평가되면 constructor의 기술된 동작을 하는 함수 객체가 생성
```js
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }
}
```

<br>

- 클래스는 평가되어 함수 객체가 됨
- 클래스도 함수 객체 고유의 프로퍼티를 모두 갖고 있음
- 함수와 동일하게 프로토타입과 연결되어 있으며 자신의 스코프 체인을 구성
- 모든 함수 객체가 가지고 있는 prototype 프로퍼티가 가리키는 프로토타입 객체의 constructor 프로퍼티는 클래스 자신을 가리킴
  - 이는 클래스가 인스턴스를 생성하는 생성자 함수라는 것을 의미
  - new 연산자와 함께 클래스를 호출하면 클래스는 인스턴스 생성

<br>

```js
// 인스턴스 생성
const me = new Person('Lee');
console.log(me);
```
- Person 클래스의 constructor 내부에서 this에 추가한 name 프로퍼티가 클래스가 생성한 인스턴스의 프로퍼티로 추가
- 생성자 함수와 마찬가지로 contstructor 내부에서 this에 추가한 프로퍼티는 인스턴스 프로퍼티
- constructor 내부의 this는 생성자 함수와 마찬가지로 클래스가 생성한 인스턴스를 가리킴

<br>

construcotr와 생성자 함수와 차이점
- constructor는 클래스 내에 최대 한 개만 존재 가능
  - 만약 클래스가 2개 이상의 constructor을 포함하면 문법 에러 발생
  ```js
  class Person {
    constructor() {}
    constructor() {}
  }
  // SyntaxError: A class may only have one constructor
  ```
- constructor 생략 가능
  ```js
  class Person {}
  ```
- constructor 생략하면 클래스에 빈 constructor가 암묵적으로 정의. constructor를 생략한 클래스는 빈 constructor에 의해 빈 객체 생성
  ```js
  class Person {
    // 빈 constructor 암묵적으로 정의
    constructor() {}
  }

  // 빈 객체 생성
  const me = new Person();
  console.log(me);    // Person {}
  ```
- 프로퍼티가 추가되어 초기화된 인스턴스를 생성하려면 consturctor 내부에서 this에 인스턴스 프로퍼티 추가
  ```js
  class Person {
    constructor() {
      // 고정값으로 인스턴스 초기화
      this.name = 'Lee';
      this.address = 'Seoul';
    }
  }

  // 인스턴스 프로퍼티 추가
  const me = new Person();
  console.log(me);    // Person {name: "Lee", address: "Seoul"}
  ```
- 인스턴스를 생성할 때 클래스 외부에서 인스턴스 프로퍼티의 초기값을 전달하려면 constructor에 매개변수를 선언하고 인스턴스를 생성할 때 초기값을 전달. 이때 초기값은 constructor의 매개변수에게 전달
  ```js
  class Person {
    constructor(name, address) {
      // 인수로 인스턴스 초기화
      this.name = name;
      this.address = address;
    }
  }

  // 인수로 초기값 전달, 초기값은 constructor에 전달
  const me = new Person('Lee', 'Seoul');
  console.log(me);    // Person {name: "Lee", address: "Seoul"}
  ```

<br>

- constructor 내에서 인스턴스의 생성과 동시에 인스턴스 프로퍼티 추가를 통해 인스턴스의 초기화를 실행하기 때문에 인스턴스를 초기화하려면 constructor 생략 X
- constructor는 별도의 반환문 X
  - new 연산자와 함께 클래스가 호출되면 생성자 함수와 동일하게 암묵적으로 this, 즉 인스턴스를 반환하기 때문
- this가 아닌 다른 객체를 명시적으로 반환하면 this, 즉 인스턴스가 반환되지 못하고 return 문에 명시한 객체 반환
  ``` js
  class Person {
    constructor(name) {
      this.name = name;
      
      // 명시적으로 객체를 반환하면 암묵적인 this 반환 무시
      return {};
    }
  }

  // constructor에서 명시적으로 반환한 빈 객체가 반환
  const me = new Person('Lee');
  consoel.log(me);    // {}
  ```
- 명시적으로 원시값을 반환하면 원시값 반환은 무시되고 암묵적으로 this가 반환
  ```js
  class Person {
    constructor(name) {
      this.name = name;

      // 명시적으로 원시값을 반환하면 암묵적으로 this 반환
      return 100;
    }
  }

  const me = new Person('Lee');
  console.log(me);    // Person {name: "Lee"}
  ```
- constructor 내부에서 명시적으로 this가 아닌 다른 값을 반환하는 것은 기본 동작을 훼손하기 때문에 constructor 내부에서 return문은 반드시 생략

<br>

### 5.2. 프로토타입 메서드
- 생성자 함수를 사용하여 인스턴스를 생성하는 경우 프로토타입 메서드를 생성하기 위해서는 명시적으로 프로토타입에 메서드를 추가
  ```js
  // 생성자 함수
  function Person(name) {
    this.name = name;
  }

  // 프로토타입 메서드
  Person.prototype.sayHi = function () {
    console.log(`Hi! My name is ${this.name}`);
  };

  const me = new Person('Lee');
  me.sayHi();    // Hi! My name is Lee
  ```
- 클래스 몸체에서 정의한 메서드는 생성자 함수에 의한 객체 생성 방식과는 다르게 클래스의 prototype 프로퍼티에 메서드를 추가하지 않아도 **기본적으로 프로토타입 메서드**가 된다.
  ```js
  class Person {
    // 생성자
    constructor(name) {
      // 인스턴스 생성 및 초기화
      this.name = name;
    }

    // 프로토타입 메서드
    sayHi() {
      console.log(`Hi! My name is ${this.name}`);
    }
  }

  const me = new Person('Lee');
  me.sayHi();    // Hi! My name is Lee
  ```
- 생성자 함수와 마찬가지로 클래스가 생성한 인스턴스는 프로토타입 체인의 일원
  ```js
  // me 객체의 프로토타입은 Person.prototype
  Object.getPrototypeOf(me) === Person.prototype;    // true
  me instanceof Person;    // true

  // Person.prototype의 프로토타입은 Object.prototype
  Object.prototypeOf(Person.prototype) === Object.prototype;    // true
  me instanceof Object;    // true

  // me 객체의 constructor는 Person 클래스
  me.constructor === Person;    // true
  ```

<br>

- 클래스 몸체에서 정의한 메서드는 인스턴스의 프로토타입에 존재하는 프로토타입 메서드
  - 인스턴스는 프로토타입 메서드를 상속받아 사용 가능
- 프로토타입 체인은 기존의 모든 객체 생성 방식(객체 리터럴, 생성자 함수, Object.create 메서드 등)뿐만 아니라 클래스에 의해 생성된 인스턴스에도 동일하게 적용
  - 생성자 함수의 역할을 클래스가 할 뿐
- 클래스는 생성자 함수와 같이 인스턴스를 생성하는 생성자 함수. 다시 말해, 생성자 함수와 마찬가지로 프로토타입 기반의 객체 생성 메커니즘

<br>

### 5.3. 정적 메서드
- 인스턴스를 생성하지 않아도 호출할 수 있는 메서드
- 생성자 함수 : 정적 메서드를 생성학 위해 명시적으로 생성자 함수에 메서드 추가
  ```js
  // 생성자 함수
  function Person(name) {
    this.name = name;
  }

  // 정적 메서드
  Person.sayHi = function () {
    console.log('Hi');
  };

  // 정적 메서드 호출
  Person.sayHi();    // Hi
  ```
- 클래스 : 메서드에 static 키워드를 붙이면 정적 메서드
  ```js
  class Person {
    // 생성자
    constructor(name) {
      // 인스턴스 생성 및 초기화
      this.name = name;
    }

    // 정적 메서드
    static sayHi() {
      console.log('Hi');
    }
  }
  ```
- 정적 메서드는 클래스에 바인딩된 메서드 
  - 클래스는 함수 객체로 평가되므로 자신의 프로퍼티/메서드 소유 가능
  - 클래스는 클래스 정의가 평가되는 시점에 함수 객체가 되므로 인스턴스와 달리 별다른 생성 과정 필요 X
  - 정적 메서드는 클래스 정의 이후 인스턴ㅅ를 생성하지 않아도 호출 가능
- 정적 메서드는 프로토타입 메서드처럼 인스턴스로 호출하지 않고 클래스로 호출
  ```js
  Person.sayHi();    // Hi
  ```
- 정적 메서드는 인스턴스로 호출 불가능
  - 정적 메서드가 바인딩된 클래스는 인스턴스의 프로토타입 체인 상에 존재하지 않기 때문
  - 다시 말해, 인스턴스의 프로토타입 체인 상에는 클래스가 존재하지 않기 때문에 인스턴스로 클래스의 메서드 상속 불가능
  ```js
  // 인스턴스 생성
  const me = new Person('Lee');
  me.sayHi();    // TypeError: me.sayHi is not a function
  ```

<br>

### 5.4. 정적 메서드와 프로토타입 메서드의 차이
1. 정적 메서드와 프로토타입 메서드는 자신이 속해 있는 프로토타입 체인이 다르다.
2. 정적 메서드는 클래스로 호출하고 프로토타입 메서드는 인스턴스로 호출한다.
3. 정적 메서드는 인스턴스 프로퍼티를 참조할 수 없지만 프로토타입 메서드는 프로퍼티를 참조할 수 있다.

<br>

```js
class Square {
  // 정적 메서드
  static area(width, height) {
    return width * height;
  }
}

console.log(Square.area(10, 10));    // 100
```
```js
class Square {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }

  // 프로토타입 메서드
  area() {
    return this.width * this.height;
  }
}

const square = new Square(10, 10);
console.log(square.area());    // 100
```
위의 코드를 살펴보면,
- 정적 메서드 area는 인스턴스 프로퍼티 참조 X
- 인스턴스 프로퍼티를 참조해야 한다면 정적 메서드 대신 프로토타입 메서드 사용
- 메서드 내부의 this는 메서드를 호출한 객체, 즉 메서드 이름 앞의 마침표 연산자 앞에 기술한 객체에 바인딩
  - 프로토타입 메서드는 인스턴스로 호출해야 하므로 프로토타입 메서드 내부의 this는 프로토타입 메서드를 호출한 인스턴스를 가리킴 -> area 내부의 this는 square 객체를 가리킴
  - 정적 메서드는 클래스로 호출해야 하므로 정적 메서드 내부의 this는 인스턴스가 아닌 클래스를 가리킴
- this를 사용하지 않는 메서드는 정적 메서드로 사용하는 것이 좋음

<br>

```js
Math.max(1, 2, 3);    // 3
Number.isNaN(NaN);    // true
JSON.strigify({ a: 1 });    // "{"a": 1}"
Object.is({}, {});    // false
Reflect.has({ a: 1 }, 'a');    // true
```
위의 코드를 살펴보면,
- 표준 빌트인 객체 Math, Number, JSON, Object, Reflect 등은 다양한 정적 메서드 가지고 있음
  - 이들 정적 메서드는 애플리케이션 전역에서 사용할 유틸리티 함수

<br>

- 클래스 또는 생성자 함수를 하나의 네임스페이스로 사용하여 정적 메서드를 모아 높으면 이름 충돌 가능성을 줄여 주고 관련 함수들을 구조화할 수 있는 효과
- 정적 메서드는 애플리케이션 전역에서 사용할 유틸리티 함수를 전역 함수로 정의하지 않고 메서드로 구조화할 때 유용

<br>

### 5.5. 클래스에서 정의한 메서드의 특징
1. function 키워드를 생략한 메서드 축약 표현을 사용
2. 객체 리터럴과 다르게 클래스 메서드를 정의할 때 콤파 불필요
3. 암묵적으로 strict mode 실행
4. for ... in문이나 Object.keys 메서드 등으로 열거 불가능. 즉, 프로퍼티 열거 가능 여부를 나타내며, 불리언 값을 갖는 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 false
5. 내부 메서드 [[Construct]]를 갖지 않는 non-constructor. 따라서 new 연산자와 함께 호출 불가능

<br>
<br>

## 6. 클래스의 인스턴스 생성 과정
- new 연산자와 함께 클래스를 호출하면 클래스의 내부 메서드 [[Construct]]가 호출
- 클래스는 new 연산자 없이 호출 가능
- 인스턴스 생성 과정
  1. 인스턴스 생성과 this 바인딩
     - new 연산자와 함께 클래스를 호출하면 constructor의 내부 코드가 실행되기에 앞서 암묵적으로 빈 객체 생성
     - 이 빈 객체가 클래스가 생성한 인스턴스
     - 이때 클래스가 생성한 인스턴스의 프로토타입으로 클래스의 prototype 프로퍼티가 가리키는 객체가 설정 
     - 암묵적으로 생성된 빈 객체, 즉 인스턴스는 this에 바인딩
     - 따라서 constructor 내부의 this는 클래스가 생성한 인스턴스를 가리킴
  2. 인스턴스 초기화
      - constructor의 내부 코드가 실행되어 this에 바인딩되어 있는 인스턴스 초기화
      - 즉, this에 바인딩되어 있는 인스턴스에 프로퍼티를 추가하고 constructor가 인수로 전달받은 초기값으로 인스턴스의 프로퍼티 값을 초기화
      - constructor가 생략되었다면 이 과정도 생략
  3. 인스턴스 반환
      - 클래스의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환
  ```js
  class Person {
    constructor(name) {
      // 1
      console.log(this);    // Person {}
      console.log(Object.getPrototypeOf(this) === Person.prototype);    // true

      // 2
      this.name = name;

      // 3
    }
  }
  ```

<br>
<br>

## 7. 프로퍼티

<br>

### 7.1. 인스턴스 프로퍼티
- 인스턴스 프로퍼티는 constructor 내부에서 정의
  ```js
  class Person {
    constructor(name) {
      // 인스턴스 프로퍼티
      this.name = name;    // name 프로퍼티는 public
    }
  }

  const me = new Person('Lee');
  console.log(me);    // Person {name: "Lee"}
  console.log(me.name);    // Lee
  ```
- constructor 내부에서 this에 인스턴스 프로퍼티를 추가
  - 이로써 클래스가 암묵적으로 생성한 빈 객체, 즉 인스턴스에 프로퍼티가 추가되어 인스턴스가 초기화
- constructor 내부에서 this에 추가한 프로퍼티는 언제나 클래스가 생성한 인스턴스의 프로퍼티

<br>

### 7.2. 접근자 프로퍼티
- 자체적으로는 값([[Value]] 내부 슬롯)을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수로 구성된 프로퍼티
```js
const person = {
  // 데이터 프로퍼티
  firstName: 'Chaebeen',
  lastName: 'Jeong',

  // fullName은 접근자 프로퍼티
  // getter 함수
  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  },

  // setter 함수
  set fullName(name) {
    // 배열 디스트럭처링 할당
    [this.firstName, this.lastName] = name.split(' ');
  }
};

// 데이터 프로퍼티를 통한 프로퍼티 값의 참조
console.log(`${person.firstName} ${person.lastName}`);    // Chaebeen Jeong

// 접근자 프로퍼티를 통한 프로퍼티 값의 저장
// fullName에 값을 저장하면 setter 함수 호출
person.fullName = 'Daeman Jeong';
console.log(person);    // {firstName: "Daeman", lastName: "Jeong"}

// 접근자 프로퍼티를 통한 프로퍼티 값의 참조
// fullName에 접근하면 getter 함수 호출
console.log(person.fullName);    // Daeman Jeong

console.log(Object.getOwnPropertyDescriptor(person, 'fullName'));    // {get: f, set: f, enumerable: true, configurable: true}
```
위의 코드를 살펴보면,
- 접근자 프로퍼티는 get, set, enumerable, configurable 프로퍼티 어트리뷰트를 갖고 있음

<br>

```js
class Person {
  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }

  // fullName은 접근자 프로퍼티
  // getter 함수
  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  }

  // setter 함수
  set fullName(name) {
    [this.firstName, this.lastName] = name.split(' ');
  }
}

const me = new Person('Chaebeen', 'Jeong');

// 데이터 프로퍼티를 통한 프로퍼티 값의 참조
console.log(`${me.firstName} ${me.lastName}`);    // Chaebeen Jeong

// 접근자 프로퍼티를 통한 프로퍼티 값의 저장
// fullName에 값을 저장하면 setter 함수 호출
me.fullName = 'Daeman Jeong';
console.log(me);    // {firstName: "Daeman", lastName: "Jeong"}

// 접근자 프로퍼티를 통한 프로퍼티 값의 참조
// fullName에 접근하면 getter 함수 호출
console.log(me.fullName);    // Daeman Jeong

console.log(Object.getOwnPropertyDescriptor(Person.prototype, 'fullName'));    // {get: f, set: f, enumerable: false, configurable: true}
```
위의 코드를 살펴보면,
- 접근자 프로퍼티는 클래스에서도 사용 가능

<br>

- 접근자 프로퍼티는 자체적으로는 값을 갖지 않고 다른 데이터 프로퍼티 값을 읽거나 저장할 때 사용하는 접근자 함수, 즉 getter 함수와 setter 함수로 구성
- getter 
  - 인스턴스 프로퍼티에 접근할 때마다 프로퍼티 값을 조작하거나 별도의 행위가 필요할 때 사용
  - 메서드 이름 앞에 get 키워드를 사용해 정의
  - 무언가를 취득할 때 사용하므로 반드시 무언가를 반환
- setter
  - 인스턴스 프로퍼티에 값을 할당할 때마다 프로퍼티 값을 조작하거나 별도의 행위가 필요할 때 사용
  - 메서드 이름 앞에 set 키워드를 사용해 정의
  - 무언가를 프로퍼티에 할당할 때 사용하므로 반드시 매개변수 존재
    - setter는 단 하나의 값만 할당받기 때문에 단 하나의 매개변수만 선언 가능
- getter와 setter 이름은 인스턴스 프로퍼티처럼 사용
  - getter는 호출하는 것이 아니라 프로퍼티처럼 참조하는 형식으로 사용, 참조 시에 내부적으로 getter 호출
  - setter도 호출하는 것이 아니라 프로퍼티처럼 값을 할당하는 형식으로 사용, 할당 시에 내부적으로 setter 호출
- 클래스의 메서드는 기본적으로 프로토타입 메서드
  - 클래스의 접근자 프로퍼티 또한 인스턴스의 프로퍼티가 아니라 프로토타입의 프로퍼티

<br>

### 7.3. 클래스 필드 정의 제안
- 클래스 필드(또는 멤버) : 클래스 기반 객체지향 언어에서 클래스가 생성할 인스턴스의 프로퍼티를 가리키는 용어
- 자바스크립트의 클래스에서 인스턴스 프로퍼티를 선언하고 초기화하려면 반드시 constructor 내부에서 this에 프로퍼티를 추가해야 함
- 자바스크립트의 클래스 몸체에는 메서드만 선언 가능. 클래스 필드를 선언하면 문법 에러 발생

<br>

```js
class Person {
  // 클래스 필드 정의
  name = 'Lee';
}

const me = new Person('Lee');
console.log(me);    // Person {name: "Lee"}
```
위 예제를 최신 브라우저나 최신 Node.js에서 실행하면 정상 동작 -> 이유는?
- 자바스크립트에서도 인스턴스 프로퍼티를 클래스 필드처럼 정의할 수 있는 새로운 표준 사양이 제안됨

<br>

- 클래스 몸체에서 클래스 필드를 정의하는 경우 this에 클래스 필드 바인딩 X. this는 클래스의 constructor와 메서드 내에서만 유효
  ```js
  class Person {
    this.name = '';    // SyntaxError: Unexpected token '.'
  }
  ```
- 클래스 필드를 참조하는 경우 클래스 기반 객체지향 언어와 다르게 this를 반드시 사용
  ```js
  class Person {
    // 클래스 필드
    name = 'Lee';

    constructor() {
      console.log(name);    // ReferenceError: name is not defined
    }
  }

  new Person();
  ```
- 클래스 필드에 초기값을 할당하지 않으면 undefined  
  ```js
  class Person {
    name;
  }

  const me = new Person();
  console.log(me);    // Person {name: undefined}
  ```
- 인스턴스를 생성할 때 외부의 초기값으로 클래스 필드를 초기화해야 할 필요가 있다면 constructor에서 클래스 필드 초기화
  ```js
  class Person {
    name;

    constructor(name) {
      // 클래스 필드 초기화
      this.name = name;
    }
  }

  const me = new Person('Lee');
  console.log(me);    // Person {name: "Lee"}
  ```
- 함수는 일급 객체이므로 함수를 클래스 필드에 할당 가능. 따라서 클래스 필드를 통해 메서드 정의 가능 
  - 클래스 필드에 함수를 할당하는 경우, 이 함수는 프로토타입 메서드가 아닌 인스턴스 메서드 -> 모든 클래스 필드는 인스턴스 프로퍼티가 되기 때문
  - 따라서 클래스 필드에 함수를 할당하는 것은 권장 X
  ```js
  class Person {
    // 클래스 필드에 문자열 할당
    name = 'Lee';

    // 클래스 필드에 함수 할당
    getName = function () {
      return this.name;
    }
  }

  const me = new Person();
  console.log(me);    // Person {name: "Lee", getName: f}
  console.log(me.getName());    // Lee
  ```

<br>

- 클래스 필드 정의 제안으로 인해 인스턴스 프로퍼티를 정의하는 방법은 두 가지
  - 인스턴스를 생성할 때 외부 초기값으로 클래스 필드를 초기화할 필요가 있다면 constructor에 인스턴스 프로퍼티를 정의하는 기존 방식 사용
  - 인스턴스를 생성할 때 외부 초기값으로 클래스 필드를 초기화할 필요가 없다면 기존의 constructor에서 인스턴스 프로퍼티를 정의하는 방식과 클래스 필드 정의 제안 모두 사용 가능

<br>

### 7.4. private 필드 정의 제안
- 자바스크립트는 캡슐화를 완전하게 지원 X -> 인스턴스 프로퍼티는 public
- 현재 TC39 프로세스에는 private 필드를 정의할 수 있는 새로운 표준 사양이 제안

<br>

```js
class Person {
  // private 필드 정의
  #name = '';

  constructor(name) {
    // private 필드 참조
    this.#name = name;
  }
}

const me = new Person('Lee');

console.log(me.#name);    // SyntaxError: Private field '#name' must be declared in an enclosing class
```
- private 필드의 선두에는 #
- private 필드를 참조할 때도 #
  - private 필드는 클래스 외부에서 참조 불가능

<br>

- public 필드는 어디서든 참조할 수 있지만 private 필드는 클래스 내부에서만 참조 가능

| 접근 가능성 | public | private |
| :---: | :---: | :---: |
| 클래스 내부 | O | O |
| 자식 클래스 내부 | O | X |
| 클래스 인스턴스를 통한 접근 | O | X |

<br>

```js
class Person {
  // private 필드 정의
  #name = '';

  constructor(name) {
    this.#name = name;
  }

  // name은 접근자 프로퍼티
  get name() {
    // private 필드 참조하여 반환
    return this.#name.trim();
  }
}

const me = new Person(' Lee ');
console.log(me.name);    // Lee
```
위의 코드를 살펴보면,
- 접근자 프로퍼티를 통해 private 필드에 간접적으로 접근 가능

<br>

```js
class Person {
  constructor(name) {
    this.#name = name;
    // SyntaxError: Private field '#name' must be declared in an enclosing class
  }
}
```
위의 코드를 살펴보면,
- private 필드는 반드시 클래스 몸체 내부에서 정의
- private 필드를 직접 constructor에 정의하면 에러 발생

<br>

### 7.5. static 필드 정의 제안
- static 키워드를 사용하여 정적 메서드를 정의할 수 있지만 정적 필드 정의 불가능
- 현재 TC39 프로세스에서 새로운 표준 사양인 "Static class features"가 제안 -> static public 필드, static private 필드, static private 메서드 정의 가능
```js
class MyMath {
  // static public 필드 정의
  static PI = 22 / 7;

  // static private 필드 정의
  static #num = 10;

  // static 메서드
  static increment() {
    return ++MyMath.#num;
  }
}

console.log(MyMath.PI);    // 3.142857142857143
console.log(MyMath.increment());    // 11
```

<br>
<br>

## 8. 상속에 의한 클래스 확장

<br>

### 8.1. 클래스 상속과 생성자 함수 상속
- **상속에 의한 클래스 확장은 기존 클래스를 상속받아 새로운 클래스를 확장하여 정의**하는 것
- 클래스와 생성자 함수는 모두 인스턴스를 생성할 수 있는 함수이지만 클래스는 상속에 의한 클래스 확장이 가능한 반면 생성자 함수는 불가능
- 상속에 의한 클래스 확장은 코드 재사용 관점에서 유용

<br>

```js
class Animal {
  constructor(age, weight) {
    this.age = age;
    this.weight = weight;
  }

  eat() { return 'eat'; }
  move() { return 'move'; }
}

// 상속을 통해 Animal 클래스 확장
class Bird extends Animal {
  fly() { return 'fly'; }
}

const bird = new Bird(1, 5);

console.log(bird);    // Bird {age: 1, weight: 5}
console.log(bird instanceof Bird);    // true
console.log(bird instanceof Animal);    // true

console.log(bird.eat());    // eat
console.log(bird.move());    // move
console.log(bird.fly());    // fly
```
- 클래스는 상속을 통해 다른 클래스를 확장할 수 있는 문법인 extends 키워드 제공
  - extends 키워드를 사용한 클래스 확장은 간편하고 직관적
  - 하지만 생성자 함수는 상속을 통해 다른 클래스를 확장할 수 있는 문법 제공 X

<br>

### 8.2. extends 키워드
- 상속을 통해 클래스를 확장하려면 extends 키워드를 사용하여 상속받은 클래스 정의
- extends 키워드 역할 : 수퍼클래스와 서브클래스 간의 상속 관계를 설정하는 것
  - 클래스도 프로토타입을 통해 상속 관계 구현
  - 수퍼클래스와 서브클래스는 인스턴스의 프로토타입 체인, 클래스 간의 프로토타입 체인 생성 -> 프로토타입 메서드, 정적 메서드 모두 상속 가능

<br>

```js
// 수퍼(베이스/부모) 클래스
class Base {}

// 서브(파생/자식) 클래스
class Derived extends Base {}
```
- 서브클래스(subclass) : 상속을 통해 확장된 클래스
  - 파생 클래스(derived class), 자식 클래스(child class)
- 수퍼클래스(superclass) : 서브클래스에게 상속된 클래스
  - 베이스 클래스(base class), 부모 클래스(parent class)

<br>

### 8.3. 동적 상속
- extends 키워드는 클래스뿐만 아니라 생성자 함수를 상속받아 클래스 확장 가능
  - 단, extends 키워드 앞에는 반드시 클래스가 와야 함
  ```js
  // 생성자 함수
  function Base(a) {
    this.a = a;
  }

  // 생성자 함수를 상속받는 서브클래스
  class Derived extends Base {}

  const derived = new Derived(1);
  console.log(derived);    // Derived {a: 1}
  ```
- exteneds 키워드 다음에는 클래스뿐 아니라 [[Constuct]] 내부 메서드를 갖는 함수 객체로 평가될 수 있는 모든 표현식 사용 가능 -> 동적으로 상속받을 대상 결정 가능
  ```js
  function Base1() {}

  class Base2 {}

  let condition = true;

  // 조건에 따라 동적으로 상속 대상을 결정하는 서브클래스
  class Derived extends (condition ? Base1 : Base2) {}

  const derived = new Derived();
  console.log(derived);    // Derived {}

  console.log(derived instanceof Base1);    // true
  console.log(derived instanceof Base2);    // false
  ```

<br>

### 8.4. 서브클래스의 constructor
- 클래스에서 constructor를 생략하면 클래스에 비어있는 constructor가 암묵적으로 정의
- 서브클래스에서 constructor를 생략하면 클래스에 아래와 같은 constructor가 암묵적으로 정의
  - args는 new 연산자와 함께 클래스를 호출할 때 전달할 인수의 리스트
  ```js
  constructor(...args) { super(...args); }
  ```
- super()는 수퍼클래스의 constructor(super-constructor)를 호출하여 인스턴스 생성

<br>

```js
// 수퍼클래스
class Base {}

// 서브클래스
class Derived extends Base {}
```
- 수퍼클래스와 서브클래스 모두 constructor 생략
```js
// 수퍼클래스
class Base {
  constructor() {}
}

// 서브클래스
class Derived extends Base {
  constructor(...args) { super(...args); }
}

const derived = new Derived();
console.log(derived);    // Derived {}
```
- 위의 클래스에는 다음과 같이 암묵적으로 constructor가 정의
- 수퍼클래스와 서브클래스 모두 constructor를 생략하면 빈 객체 생성 
  - 프로퍼티를 소유하는 인스턴스를 생성하려면 constructor 내부에서 인스턴스 프로퍼티 추가

<br>

### 8.5. super 키워드
- super 키워드는 함수처럼 호출할 수도 있고 this와 같이 식별자처럼 참조할 수 있는 특별한 키워드
- 동작 방법
  - super를 호출하면 수퍼클래스의 constructor(super-constructor)를 호출
  - super를 호출하면 수퍼클래스의 메서드 호출 가능

<br>

**super 호출**
- **super를 호출하면 수퍼클래스의 constructor(super-constructor) 호출**

```js
// 수퍼클래스
class Base {
  constructor(a, b) {
    this.a = a;
    this.b = b;
  }
}

// 서브클래스
class Derived extends Base {}

const derived = new Derived(1, 2);
console.log(derived);    // Derived {a: 1, b: 2}
```
위의 코드를 살펴보면,
- 수퍼클래스의 constructor 내부에서 추가한 프로퍼티를 그대로 갖는 인스턴스를 생성한다면 서브클래스의 constructor 생략 가능
- 이때 new 연산자와 함께 서브클래스를 호출하면서 전달한 인수는 모두 서브클래스에서 암묵적으로 정의된 constructor의 super 호출을 통해 수퍼클래스의 constructor에 전달

<br>

```js
// 수퍼클래스
class Base {
  constructor(a, b) {    // 4
    this.a = a;
    this.b = b;
  }
}

// 서브클래스
class Derived extends Base {
  constructor(a, b, c) {    // 2
    super(a, b);    // 3
    this.c = c;
  }
}

const derived = new Derived(1, 2, 3);    // 1
console.log(derived);    // Derived {a: 1, b: 2, c: 3}
```
위의 코드를 살펴보면,
- 수퍼클래스에 추가한 프로퍼티와 서브클래스에서 추가한 프로퍼티를 갖는 인스턴스를 생성한다면 서브클래스의 constructor 생략 불가능
- 이때 new 연산자와 함께 서브클래스를 호출하면서 전달한 인수 중에 수퍼클래스의 constructor에 전달할 필요가 있는 인수는 서브클래스의 constructor에서 호출하는 super를 통해 전달
- new 연산자와 함께 Derived 클래스를 호출(1)하면서 전달한 인수 1, 2, 3은 Derived 클래스의 constructor에 전달(2)되고 super 호출(3)을 통해 Base 클래스의 constructor(4)에 일부가 전달

<br>

super를 호출할 때 주의 사항
1. 서브클래스에서 constructor를 생략하지 않는 경우 서브클래스의 constructor에서는 반드시 super 호출
    ```js
    class Base {}

    class Derived extends Base {
      constructor {
        // ReferenceError: Must call super constructor in derived class before accessing 'this' or returning from derived constructor
        console.log('constructor call');
      }
    }

    const derived = new Derived();
    ```
2. 서브클래스의 constructor에서 super를 호출하기 전에는 this 참조 불가능
    ```js
    class Base {}

    class Derived extends Base {
      constructor() {
        // ReferenceError: Must call super constructor in derived class before accessing 'this' or returning from derived constructor
        this.a = 1;
        super();
      }
    }

    const derived = new Derived(1);
    ```
3. super는 반드시 서브클래스의 constructor에서만 호출. 서브클래스가 아닌 클래스의 constructor나 함수에서 super 호출하면 에러 발생
    ```js
    class Base {
      constructor() {
        super();    // SyntaxError: 'super' keyword unexpected here
      }
    }

    function Foo() {
      super();    // SyntaxError: 'super' keyword unexpected here
    }
    ```

<br>

**super 참조**
- **메서드 내에서 super를 참조하면 수퍼클래스의 메서드 호출 가능**

<br>

1. 서브클래스의 프로토타입 메서드 내에서 super.sayHi는 수퍼클래스의 프로토타입 메서드 sayHi를 가리킴
    ```js
    // 수퍼클래스
    class Base {
      constructor(name) {
        this.name = name;
      }

      sayHi() {
        return `Hi! ${this.name}`;
      }
    }

    // 서브클래스
    class Derived extends Base {
      sayHi() {
        return `${super.sayHi()}. how are doing?`;
      }
    } 

    const derived = new Derived('Lee');
    console.log(derived.sayHi());    // Hi! Lee. how are you doing?
    ```
    - super 참조를 통해 수퍼클래스의 메서드를 참조하려면 수퍼클래스의 prototype 프로퍼티에 바인딩된 프로토타입을 참조할 수 있어야 함.

    <br>

    ```js
    // 수퍼클래스
    class Base {
      constructor(name) {
        this.name = name;
      }

      sayHi() {
        return `Hi! ${this.name}`;
      }
    }

    // 서브클래스
    class Derived extends Base {
      sayHi() {
        const __super = Object.getPrototypeOf(Derived.prototype);
        return `${__super.sayHi.call(this)} how are you doing?`;
      }
    } 
    ```
    - super는 자신을 참조하고 있는 메서드(Derived의 sayHi)가 바인딩되어 있는 객체(Derived.prototype)의 프로토타입(Base.prototype)을 가리킴
    - 따라서 super.sayHi -> Base.prototype.sayHi
    - 단, Base.prototype.sayHi를 호출할 때 정확하게 호출하기 위해서 call 메서드를 사용해 this를 전달해야 함

<br>

2. 서브클래스의 정적 메서드 내에서 super.sayHi는 수퍼클래스의 정적 메서드 sayHi를 가리킴
    ```js
    // 수퍼클래스
    class Base {
      static sayHi() {
        return 'Hi!';
      }
    }

    // 서브클래스
    class Derived extends Base {
      static sayHi() {
        return `${super.sayHi()} how are you doing?`;
      }
    }

    console.log(Derived.sayHi());    // Hi! how are you doing?
    ```

<br>

### 8.6. 상속 클래스의 인스턴스 생성 과정
```js
// 수퍼클래스
class Rectangle {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }

  toString() {
    return `width = ${this.width}, height = ${this.height}`;
  }
}

// 서브클래스
class ColorRectangle extends Rectangle {
  constructor(width, height, color) {
    super(width, height);
    this.color;
  }

  // 메서드 오버라이딩
  toString() {
    return super.toString() + `, color = ${this.color}`;
  }
}

const colorRectangle = new ColorRectange(2, 4, 'red');
console.log(colorRectangle);    // ColorRectangle {width: 2, height: 4, color: "red"}

// 상속을 통해 getArea 호출
console.log(colorRectangle.getArea());    // 8
// 오버라이딩된 toString 호출
console.log(colorRectangle.toString());    // width = 2, height = 4, color = red
```
- Rectangle : 직사각형을 추상화한 클래스
- ColorRectangle : 상속을 통해 Rectangle 클래스 확장

<br>

서브클래스 ColorRectangle이 new 연산자와 함께 호출되면 다음 과정을 통해 인스턴스 생성

<br>

1. 서브클래스의 super 호출
    - 자바스크립트 엔진은 [[ConstructorKind]] 내부 슬롯을 가짐 : 클래스를 평가할 때 수퍼클래스와 서브클래스를 구분하기 위해 "base" 또는 "derived"를 값으로 갖는 내부 슬롯
    - base : 다른 클래스를 상속받지 않는 클래스(그리고 생성자 함수)의 설정값
    - derived : 다른 클래스를 상속받는 서브클래스의 상속값
    - **서브클래스의 constructor에서 super을 호출해야 하는 이유 : 서브클래스는 자신이 직접 인스턴스를 생성하지 않고 수퍼클래스에게 인스턴스 생성 위임하기 때문에**

<br>

2. 수퍼클래스의 인스턴스 생성과 this 바인딩
    - 수퍼클래스의 constructor 내부 코드가 실행되기 전 생성되는 빈 객체 -> 클래스가 생성한 인스턴스 -> 해당 인스턴스는 this에 바인딩 -> 따라서 수퍼클래스의 constructor 내부의 this는 생성된 인스턴스를 가리킴
    ```js
    // 수퍼클래스
    class Rectangle {
      constructor(width, height) {
        console.log(this);
        console.log(new.target);    // ColorRectangle
      }
    }
    ```
    - new 연산자와 함께 호출된 함수를 가리키는 new.target은 서브클래스를 가리킴
    - 따라서 **인스턴스는 new.target이 가리키는 서브클래스가 생성한 것으로 처리**
    - 생성된 인스턴스의 프로토타입은 서브클래스의 prototype 프로퍼티가 가리키는 객체(ColorRectangle.prototype)

<br>

3. 수퍼클래스의 인스턴스 초기화
    - 수퍼클래스의 constructor가 실행 -> this 바인딩되어 있는 인스턴스 초기화
    - this에 바인딩되어 있는 인스턴스에 프로퍼티 추가 후, constructor가 인수로 전달받은 초기값으로 인스턴스의 프로퍼티 초기화

<br>

4. 서브클래스의 constructor로 복귀와 this 바인딩
    - super 호출이 종료 -> 제어 흐름이 서브클래스의 constructor로 돌아옴
    - **이때 super가 반환한 인스턴스가 this에 바인딩**
    - **서브클래스는 별도의 인스턴스를 생성하지 않고 super가 반환한 인스턴스를 this에 바인딩하여 그대로 사용**
    - **서브클래스의 constructor에서 super를 호출하기 전에 this를 참조할 수 없는 이유 : super가 호출되지 않으면 인스턴스 생성되거나 this 바인딩 불가능**

<br>

5. 서브클래스의 인스턴스 초기화
    - super 호출 이후 서브클래스의 constuctor에 기술되어 있는 인스턴스 초기화 실행
    - this에 바인딩되어 있는 인스턴스에 프로퍼티를 추가
    - constructor가 인수로 전달받은 초기값으로 인스턴스의 프로퍼티 초기화

<br>

6. 인스턴스 반환
    - 클래스의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환

<br>

### 8.7. 표준 빌트인 생성자 함수 확장
- String, Number, Array 같은 표준 빌트인 객체도 [[Construct]] 내부 메서드를 갖는 생성자 함수이므로 extends 키워드를 사용하여 확장 가능

<br>

```js
// Array 생성자 함수를 상속받아 확장
class MyArray extends Array {
  // 중복된 배열 요소 제거 후 반환
  uniq() {
    return this.filter((v, i, self) => self.indexOf(v) === i);
  }

  // 모든 배열 요소의 평균
  average() {
    return this.reduce((pre, cur) => pre + cur, 0) / this.length;
  }
}

const myArray = new MyArray(1, 1, 2, 3);
console.log(myArray);    // MyArray(4) [1, 1, 2, 3]

console.log(myArray.uniq());    // MyArray(3) [1, 2, 3]
console.log(myArray.average());    // 1.75
```
위의 코드를 살펴보면,
- MyArray 클래스가 생성한 인스턴스는 Array.prototype과 MyArray.prototype의 모든 메서드 사용 가능
- Array.prototype의 메서드 중에서 map, filter와 같이 새로운 배열을 반환하는 메서드가 MyArray 클래스의 인스턴스를 반환

<br>
<br>