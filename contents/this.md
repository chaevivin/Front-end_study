# this

## 1. this 키워드
동작을 나타내는 메서드는 자신이 속한 객체의 상태, 즉 프로퍼티를 참조하고 변경할 수 있어야 한다. 이때 메서드가 자신이 속한 객체의 프로퍼티를 참조하려면 먼저 **자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 한다.**

<br>

```js
function Circle(radius) {
  // 이 시점에는 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없다.
  ????.radius = radius;
}

Circle.prototype.getDiameter = function () {
  // 이 시점에는 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없다.
  return 2 * ????.radius;
};

// 생성자 함수로 인스턴스를 생성하려면 먼저 생성자 함수를 정의해야 한다.
const circle = new Circle(5);
```
위의 코드를 살펴보면,
- 생성자 함수 방식으로 인스턴스를 생성하는 방식
- 생성자 함수 내부에서는 프로퍼티나 메서드를 추가하기 위해 자신이 생성할 인스턴스를 참조할 수 있어야 함.
- 하지만 먼저 생성자 함수를 정의한 후 new 연산자와 함께 생성자 함수를 호출하는 단계가 추가로 필요
  - 다시 말해, 생성자 함수로 인스턴스를 생성하려면 먼저 생성자 함수가 존재해야 함
- 생성자 함수를 정의하는 시점에는 인스턴스를 생성하기 이전이므로 생성자 함수가 생성할 인스턴스를 가리키는 식별자를 알 수 없음
- 따라서 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 식별자 필요 -> **this**

<br>

- **this는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수. this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조 가능**
- this는 자바스크립트 엔진에 의해 암묵적으로 생성, 코드 어디서든 참조 가능
- 함수 호출 -> arguments 객체와 this가 암묵적으로 함수 내부에 전달 -> 함수 내부에서 arguments 객체와 this를 지역 변수처럼 사용 가능
  - **단, this가 가리키는 값, 즉 this 바인딩은 함수 호출 방식에 의해 동적으로 결정**
  - 바인딩 : 식별자와 값을 연결하는 과정

<br>

```js
// 전역
console.log(this);    // window

// 일반 함수
function square(number) {
  console.log(this);     // window
  return number * number;
}
square(2);

// 객체 리터럴
const circle = {
  radius: 5,
  getDiameter() {
    console.log(this);    // {radius: 5, getDiamter: f}
    return 2 * this.radius;
  }
};

console.log(circle.getDiameter());    // 10

// 생성자 함수
function Circle(radius) {
  this.radius = radius;
  console.log(this);    // Circle {radius: 5}
}

Circle.prototype.getDiameter = function () {
  return 2 * this.radius;
};

// 인스턴스 생성
const circle = new Circle(5);
console.log(circle.getDiameter());    // 10
```
위의 코드를 살펴보면,
- 전역에서 this는 전역 객체 window를 가리킨다.
- 일반 함수 내부에서 this는 전역 객체 window를 가리킨다.
- 객체 리터럴의 메서드 내부에서 this는 메서드를 호출한 객체를 가리킨다.
- 생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스를 가리킨다.

<br>
<br>

## 2. 함수 호출 방식과 this 바인딩
**this 바인딩(this에 바인딩될 값)은 함수 호출 방식, 즉 함수가 어떻게 호출되었는지에 따라 동적으로 결정된다.**

<br>

### 2.1. 일반 함수 호출
**일반 함수로 호출하면 함수 내부의 this에는 전역 객체가 바인딩된다.**
```js
function foo() {
  console.log(this);    // window
  function bar() {
    console.log(this);    // window
  }
  bar();
}
foo();
```

<br>

단, this는 객체의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수이므로 객체를 생성하지 않는 일반 함수에서 this는 의미가 없다. 따라서 strict mode가 적용된 일반 함수 내부의 this에는 undefined가 바인딩된다.
```js
function foo() {
  'use strict';

  console.log(this);    // undefined
  function bar() {
    console.log(this);    // undefined
  }
  bar();
}
foo();
```

<br>

메서드 내에서 정의한 중첩 함수도 일반 함수로 호출되면 중첩 함수 내부의 this에는 전역 객체가 바인딩된다.
```js
// var 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티
// const 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티 X
var value = 1;

const obj = {
  value: 100,
  foo() {
    console.log(this);    // {value: 100, foo: f}
    console.log(this.value);    // 100

    // 메서드 내에서 정의한 중첩 함수
    function bar() {
      console.log(this);    // window
      console.log(this.value);    // 1
    }

    bar();
  }
};

obj.foo();
```

<br>

콜백 함수가 일반 함수로 호출된다면 콜백 함수 내부의 this에도 전역 객체가 바인딩된다. 어떤 함수라도 일반 함수로 호출되면 this에는 전역 객체가 바인딩된다.
```js
var value = 1;

const obj = {
  value: 100,
  foo() {
    console.log(this);    // {value: 100, foo: f}
    // 콜백 함수
    setTimeOut(function () {
      console.log(this);    // window
      console.log(this.value);    // 1
    }, 100);
  }
};

obj.foo();
```

<br>

일반 함수로 호출된 모든 함수 내부의 this가 전역 객체를 바인딩할 때 문제점
- 중첩 함수 또는 콜백 함수는 외부 함수인 메서드와 중첩 함수 또는 콜백 함수의 this가 일치하지 않는다는 것은 줍첩 함수 또는 콜백 함수를 헬퍼 함수로 동작하기 어렵게 만든다.
- 메서드 내부의 중첩 함수나 콜백 함수의 this 바인딩을 메서드의 this 바인딩과 일치시키는 방법
  - this 바인딩을 다른 변수에 할당 후 중첩 함수나 콜백 함수 내에서 this 대신 다른 변수를 참조
  ```js
  var value = 1;

  const obj = {
    value: 100,
    foo() {
      const that = this;
      setTimeOut(function() {
        console.log(that.value);    // 100
      }, 100);
    }
  };

  obj.foo();
  ```
  - this를 명시적으로 바인딩할 수 있는 Function.prototype.apply, Function.prototype.call, Function.prototype.bind 메서드 제공
  ```js
  var value = 1;

  const obj = {
    value: 100,
    foo() {
      setTimeOut(function() {
        console.log(this.value);    // 100
      }.bind(this), 100);
    }
  };

  obj.foo();
  ```
  - 화살표 함수를 사용해서 this 바인딩 일치
  ```js
  var value = 1;

  const obj = {
    value: 100,
    foo() {
      setTimeOut(() => console.log(this.value), 100);    // 100
    }
  };

  obj.foo();
  ```

<br>

### 2.2. 메서드 호출
메서드 내부의 this에는 메서드를 호출한 객체, 즉 메서드를 호출할 때 메서드 이름 앞의 마침표 연산자 앞에 기술한 객체가 바인딩된다. 주의할 것은 메서드 내부의 this는 메서드를 소유한 객체가 아닌 메서드를 호출한 객체에 바인딩된다는 것이다.

<br>

```js
const person = {
  name: 'Lee',
  getName() {
    return this.name;
  }
};

console.log(person.name);    // Lee

const anotherPerson = {
  name: 'Kim'
};

anotherPerson.getName = person.getName;
console.log(anotherPerson.getName());    // Kim

const getName = person.getName;
console.log(getName());    // ''
```
위의 코드를 살펴보면,
- getName 메서드는 person 객체의 메서드로 정의
- 메서드는 프로퍼티에 바인딩된 함수
- 메서드는 객체에 포함된 것이 아니라 독립적으로 존재하는 별도의 객체
  - 즉, person 객체의 getName 프로퍼티가 가리키는 함수 객체는 person 객체에 포함된 것이 아니라 독립적으로 존재하는 별도의 객체
- getName 프로퍼티가 함수 객체를 가리키고 있을 뿐임
- getName 프로퍼티가 가리키는 함수 객체, 즉 getName 메서드는 다른 객체의 프로퍼티에 할당하는 것으로 다른 객체의 메서드가 될 수도 있고 일반 변수에 할당하여 일반 함수로 호출 가능 
- 메서드 내부의 this는 프로퍼티로 메서드를 가리키고 있는 객체와는 관계가 없고 메서드를 호출한 객체에 바인딩

<br>

프로토타입 메서드 내부에서 사용된 this도 일반 메서드와 마찬가지로 해당 메서드를 호출한 객체에 바인딩된다.
```js
function Person(name) {
  this.name = name;
}

Person.prototype.getName = function() {
  return this.name;
};

const me = new Person('Lee');
console.log(me.getName());    // Lee

Person.prototype.name = 'Kim';
console.log(Person.prototype.getName());    // Kim
```
위의 코드를 살펴보면,
- 처음 getName 메서드를 호출한 객체는 me다. 따라서 getName 메서드 내부의 this는 me를 가리킨다.
- 두번째 getName 메서드를 호출한 객체는 Person.prototype이다. Person.prototype도 객체이므로 직접 메서드를 호출할 수 있다. 따라서 getName 메서드 내부의 this는 Person.prototype을 가리킨다.

<br>

### 2.3. 생성자 함수 호출
생성자 함수 내부의 this에는 생성자 함수가 생성할 인스턴스가 바인딩된다.
```js
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

const circle1 = new Circle(5);
const circle2 = new Circle(10);

console.log(circle1.getDiameter());    // 10
console.log(circle2.getDiameter());    // 20
```

<br>

```js
const circle3 = Circle(15);
console.log(circle3);    // undefined
console.log(radius);    // 15
```
위의 코드를 살펴보면,
- new 연산자와 함께 호출하지 않으면 생성자 함수로 동작 X. 즉, 일반적인 함수 호출
- 일반 함수로 호출된 Circle에는 반환문이 없으므로 암묵적으로 undefined 반환
- 일반 함수로 호출된 Circle 내부의 this는 전역 객체를 가리킴

<br>

### 2.4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출
- apply, call, bind 메서드는 Function.prototype 메서드
- 이 메서드들은 모든 함수가 상속받아 사용 가능
- Function.prototype.apply, Function.prototype.call 메서드는 this로 사용할 객체와 인수 리트를 인수로 전달받아 함수 호출
  ```js
  function getThisBinding() {
    return this;
  }

  const thisArg = { a: 1 };

  console.log(getThisBinding());    // window

  console.log(getThisBinding.apply(thisArg));    // {a: 1}
  console.log(getThisBinding.call(thisArg));    // {a: 1}
  ```
  - **apply와 call 메서드의 본질적인 기능은 함수 호출하는 것**
  - apply와 call 메서드는 함수를 호출하면서 첫 번째 인수로 전달한 특정 객체를 호출한 함수의 this에 바인딩
  - apply : 호출할 함수의 인수를 배열로 묶어 전달
  - call : 호출할 함수의 인수를 쉼표로 구분한 리스트 형식으로 전달
  - apply와 call 메서드는 arguments 객체와 같은 유사 배열 객체에 배열 메서드 사용 가능
- Function.prototype.bind 메서드는 함수 호출 X
  - 첫 번째 인수로 전달한 값으로 this 바인딩이 교체된 함수를 새롭게 생성해 반환
  ```js
  function getThisBinding() {
    return this;
  }

  const thisArg = { a: 1 };

  console.log(getThisBinding.bind(thisArg));    // getThisBinding
  console.log(getThisBinding.bind(thisArg)());    // {a: 1}
  ```
  - bind 메서드는 메서드의 this와 메서드 내부의 중첩 함수 또는 콜백 함수의 this가 불일치하는 문제 해결

<br>

정리
| 함수 호출 방식 | this 바인딩 |
| :---: | :---: |
| 일반 함수 호출 | 전역 객체 |
| 메서드 호출 | 메서드를 호출한 객체 |
| 생성자 함수 호출 | 생성자 함수가 생성할 인스턴스 |
| Function.prototype.apply/call/bind 메서드에 의한 간접 호출 | Function.prototype.apply/call/bind 메서드에 첫 번째 인수로 전달한 객체 |

<br>
<br>