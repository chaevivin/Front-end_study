# 프로퍼티 어트리뷰트

## 1. 내부 슬롯과 내부 메서드
내부 슬롯, 내부 메서드 : 내부 슬롯과 내부 메서드는 자바스크립트 엔진의 구현 알고리즘을 설명하기 위해 ECMAScript 사양에서 사용하는 의사 프로퍼티(pseudo property)와 의사 메서드(pseudo method)다. ECMAScript 사양에 등장하는 이중 대괄호로 감싼 이름들이 내부 슬롯과 내부 메서드다.

<br>

내부 슬롯과 내부 메서드는 자바스크립트의 내부 로직이므로 내부 슬롯과 내부 메스드에 직접적으로 접근하거나 호출할 수 없다. 단, 일부에 한하여 간접적으로 접근할 수 있다.
```js
const o = {};

o.[[Prototype]]    // UnCaught SyntaxError: Unexpected token '['
o.__proto__    // Object.prototype
```
위의 코드를 살펴보면,
1. [[Prototype]] 내부 슬롯은 직접 접근할 수 없다.
2. [[Prototype]] 내부 슬롯은 __proto__를 통해 간접적으로 접근할 수 있다.

<br>
<br>

## 2. 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체
**자바스크립트 엔진은 프로퍼티를 생성할 때 프로퍼티의 상태를 나타내는 프로퍼티 어트리뷰트를 기본값으로 자동 정의한다.** 프로퍼티 상태란 프로퍼티 값, 값의 갱신 여부, 열거 가능 여부, 재정의 가능 여부를 말한다.

프로퍼티 어트리뷰트는 자바스크립트 엔진이 관리하는 내부 상태 값인 내부 슬롯 [[Value]], [[Writable]], [[Enumerable]], [[Configurable]]이다. 따라서 프로퍼티 어트리뷰트에 직접 접근할 수 없지만 Object.getOwnPropertyDescriptor 메서드를 사용하여 간접적으로 확인할 수 있다.

<br>

```js
const person = {
  name: 'Lee'
};

console.log(Object.getOwnPropertyDescriptor(person, 'name'));    // {value: "Lee", writable: true, enumerable: true, configurable: true}
```
위의 코드를 살펴보면,
1. Object.getOwnPropertyDescriptor 메서드를 호출할 때 첫 번째 매개변수에는 객체의 참조를 전달하고, 두 번째 매개변수에는 프로퍼티 키를 문자열로 전달한다. 
2. Object.getOwnPropertyDescript 메서드는 **프로퍼티 디스크립터 객체**를 반환한다.

<br>

```js
const person = {
  name: 'Lee'
};

person.age = 20;    // 프로퍼티 동적 생성

console.log(Object.getOwnPropertyDescriptors(person));    // name: {value: "Lee", writable: true, enumerable: true, configurable: true}, age: {value: 20, writable: true, enumerable: true, configurable: true}
```
ES8에 도입된 Object.getOwnPropertyDescriptors 메서드는 모든 프로퍼티의 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체를 반환한다.

<br>
<br>

## 3. 데이터 프로퍼티와 접근자 프로퍼티
- 데이터 프로퍼티 : 키와 값으로 구성된 일반적인 프로퍼티다. 지금까지 살펴본 모든 프로퍼티는 데이터 프로퍼티다.
- 접근자 프로퍼티 : 자체적으로 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수로 구성된 프로퍼티다.

<br>

### 3.1. 데이터 프로퍼티
데이터 프로퍼티는 프로퍼티 어트리뷰트를 갖는다. 이 프로퍼티 어트리뷰트는 자바스크립트 엔진이 프로퍼티를 생성할 때 기본값으로 자동 정의된다.
| 프로퍼티 어트리뷰트 | 프로퍼티 디스크립터 객체의 프로퍼티 | 설명 |
| :--- | :--- | :--- |
| [[Value]] | value | - 프로퍼티 키를 통해 프로퍼티 값에 접근하면 반환되는 값이다. <br> - 프로퍼티 키를 통해 프로퍼티 값을 변경하면 [[Value]]에 값을 재할당한다. 이때 프로퍼티가 없으면 프로퍼티를 동적 생성하고 생성된 프로퍼티의 [[Value]]에 값을 저장한다. |
| [[Writable]] | writable | - 프로퍼티 값의 변경 가능 여부를 나타내며 불리언 값을 갖는다. <br> - [[Writable]]의 값이 false인 경우 해당 프로퍼티의 [[Value]]의 값을 변경할 수 없는 읽기 전용 프로퍼티가 된다. |
| [[Enumerable]] | enumberable | - 프로퍼티의 열거 가능 여부를 나타내며 불리언 값을 갖는다. <br> - [[Enumberalbe]]의 값이 false인 경우 해당 프로퍼티는 for...in문이나 Object.keys 메서드 등으로 열거할 수 없다. |
| [[Configurable]] | configurable | - 프로퍼티의 재정의 가능 여부를 나타내며 불리언 값을 갖는다. <br> - [[Configurable]]의 값이 false인 경우 해당 프로퍼티의 삭제, 프로퍼티 어트리뷰트 값의 변경이 금지된다. 단, [[Writable]]이 true인 경우 [[Value]]의 변경과 [[Writable]]을 false로 변경하는 것은 허용된다. |

<br>

### 3.2. 접근자 프로퍼티
접근자 프로퍼티는 자체적으로 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수로 구성된 프로퍼티다. 접근자 프로퍼티는 프로퍼티 어트리뷰트를 갖는다.
| 프로퍼티 어트리뷰트 | 프로퍼티 디스크립터 객체의 프로퍼티 | 설명 |
| :--- | :--- | :--- |
| [[Get]] | get | 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 읽을 때 호출되는 접근자 함수다. 즉, 접근자 프로퍼티 키로 프로퍼티 값에 접근하면 프로퍼티 어트리뷰트 [[Get]]의 값, 즉 getter 함수가 호출되고 그 결과 프로퍼티 값으로 반환된다. |
| [[Set]] | set | 접근자 프로퍼티를 통해 데이터 프로퍼티 값을 저장할 때 호출되는 접근자 함수다. 즉, 접근자 프로퍼티 키로 프로퍼티 값을 저장하면 프로퍼티 어트리뷰트 [[Set]]의 값, 즉 setter 함수가 호출되고 그 결과가 프로퍼티 값으로 지정된다. |
| [[Enumerable]] | enumerable | 데이터 프로퍼티 [[Enumberable]]과 같다. |
| [[Configurable]] | configurable | 데이터 프로퍼티 [[Configurable]]과 같다. |

<br>

```js
const person = {
  // 데이터 프로펕
  firstName: 'Chaebeen',
  lastName: 'Jeong',

  // 접근자 프로퍼티
  // getter 함수
  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  },

  // setter 함수
  set fullName() {
    [this.firstName, this.lastName] = name.split(' ');
  }
};

// 데이터 프로퍼티를 통한 프로퍼티 값 참조
console.log(person.firstName + ' ' + person.lastName);    // Chaebeen Jeong

// 접근자 프로퍼티를 통한 프로퍼티 값 저장
person.fullName = 'Daeman Jeong';
console.log(person);    // {firstName: "Daeman", lastName: "Jeong"}

// 접근자 프로퍼티를 통한 프로퍼티 값 참조
console.log(person.fullName);    // Daeman Jeong

let descriptor = Object.getOwnPropertyDescriptor(person, 'firstName');
console.log(descriptor);    // {value: "Daeman", writable: true, enumerable: true, configurable: true}

descriptor = Object.getOwnPropertyDescriptor(person, 'fullName');
console.log(descriptor);    // {get: f, set: f, enumerable: true, configurable: true}
```
접근자 프로퍼티는 자체적으로 값을 가지지 않고 데이터 프로퍼티의 값을 읽거나 저장할 때 관여한다. 

접근자 프로퍼티 fullName으로 프로퍼티 값에 접근하면 내부적으로 [[Get]] 내부 메서드가 호출되어 다음과 같이 동작한다.
1. 프로퍼티 키가 유효한지 확인한다. 프로퍼티 키는 문자열 또는 심벌이어야 한다. 프로퍼티 키 "fullName"은 문자열이므로 유효한 프로퍼티 키다.
2. 프로토타입 체인에서 프로퍼티를 검색한다. person 객체에 fullName 프로퍼티가 존재한다.
3. 검색된 fullName 프로퍼티가 데이터 프로퍼티인지 접근자 프로퍼티인지 확인한다. fullName 프로퍼티는 접근자 프로퍼티다.
4. 접근자 프로퍼티 fullName은 프로퍼티 어트리뷰트 [[Get]]의 값, 즉 getter 함수를 호출하여 그 결과를 반환한다. 프로퍼티 fullName의 프로퍼티 어트리뷰트 [[Get]]의 값은 Object.getOwnPropertyDescriptor 메서드가 반환하는 프로퍼티 디스크립터 객체의 get 프로퍼티 값과 같다.

<br>

```js
// 접근자 프로퍼티
Object.getOwnPropertyDescriptor(Object.prototype, '__proto__');    // {get: f, set: f, enumerable: true, configurable: true}

// 데이터 프로퍼티
Object.getOwnPropertyDescriptor(function() {}, 'prototype');    // {value: {...}, writable: true, enumerable: true, configurable: true}
```

<br>
<br>

## 4. 프로퍼티 정의
프로퍼티 정의란 새로운 프로퍼티를 추가하면 프로퍼티 어트리뷰트를 명시적으로 정의하거나, 기존 프로퍼티의 프로퍼티 어트리뷰트를 재정의하는 것을 말한다.

Object.defineProperty 메서드를 사용하면 프로퍼티의 어트리뷰트를 정의할 수 있다. 인수로는 객체의 참조와 데이터 프로퍼티 키인 문자열, 프로퍼티 디스크립터 객체를 전달한다.

<br>

```js
const person = {};

// 데이터 프로퍼티 정의
Object.defineProperty(person, 'firstName', {
  value: 'Chaebeen',
  writable: true,
  enumerable: true,
  configurable: true
});

Object.defineProperty(person, 'lastName', {
  value: 'Jeong'
});

let descriptor = Object.getOwnPropertyDescriptor(person, 'firstName');
console.log('firstName', descriptor);    // firstName {value: "Chaebeen", writable: true, enumerable: true, configurable: true}

// 프로퍼티를 누락시키면 undefined, false가 기본값
descriptor = Object.getOwnPropertyDescriptor(person, 'lastName');
console.log('lastName', descriptor);    // lastName {value: "Jeong", writable: false, enumerable: false, configurable: false}

// enumerable: false이므로 열거되지 않는다.
console.log(Object.keys(person));    // ["firstName"]

// writable: false이므로 값을 변경할 수 없다.
person.lastName = 'Kim';

// configurable: false이므로 삭제할 수 없다.
delete person.lastName;

// configurable: false이므로 프로퍼티를 재정의할 수 없다.
Object.defineProperty(person, 'lastName', { enumerable: true });    // Uncaught TypeError: Cannot redefine property: lastName

// 접근자 프로퍼티 정의
Object.defineProperty(person, 'fullName', {
  // getter 함수
  get() {
    return `${this.firstName} ${this.lastName}`;
  },
  // setter 함수
  set(name) {
    [this.firstName, this.lastName] = name.split(' ');
  },
  enumerable: true,
  configurable: true
});

descriptor = Object.getOwnPropertyDescriptor(person, 'fullName');
console.log('fullName', descriptor);    // fullName {get: f, set: f, enumerable: true, configurable: true}

person.fullName = "Daeman Jeong";
console.log(person);    // {firstName: "Daeman", lastName: "Jeong"}
```

<br>

프로퍼티 디스크립터 객체의 기본값
| 프로퍼티 디스크립터 객체의 프로퍼티 | 대응하는 프로퍼티 어트리뷰트 | 생략했을 때의 기본값 |
| :---: | :---: | :---: |
| value | [[Value]] | undefined |
| get | [[Get]] | undefined |
| set | [[Set]] | undefined |
| writable | [[Writable]] | false |
| enumerable | [[Enumerable]] | false |
| configurable | [[Configurable]] | false |

<br>

Object.defineProperty 메서드는 한번에 하나의 프로퍼티만 정의할 수 있다. Object.defineProperties 메서드를 사용하면 여러 개의 프로퍼티를 한 번에 정의할 수 있다.
```js
const person = {};

Object.defineProperties(person, {
  // 데이터 프로퍼티 정의
  firstName: {
    value: 'Chaebeen',
    writable: true,
    enumerable: true,
    configurable: true
  },
  lastName: {
    value: 'Jeong',
    writable: true,
    enumerable: true,
    configurable: true
  },
  // 접근자 프로퍼티 정의
  fullName: {
    // getter 함수
    get() {
      return `${this.firstName} ${this.lastName}`;
    },
    // setter 함수
    set(name) {
      [this.firstName, this.lastName] = name.split(' ');
    },
    enumerable: true,
    configurable: true
  }
});

person.fullName = "Daeman Jeong";
console.log(person);    // {firstName: "Daeman", lastName: "Jeong"}
```

<br>
<br>

## 5. 객체 변경 방지
객체는 변경 가능한 값이므로 재할당 없이 직접 변경할 수 있다. 자바스크립트는 객체의 변경을 방지하는 다양한 메서드를 제공한다. 객체 변경 메서드들은 객체의 변경을 금지하는 강도가 다르다.
| 구분 | 메서드 | 프로퍼티 추가 | 프로퍼티 삭제 | 프로퍼티 값 읽기 | 프로퍼티 값 쓰기 | 프로퍼티 어트리뷰트 재정의 |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| 객체 확장 금지 | Object.preventExtensions | X | O | O | O | O |
| 객체 밀봉 | Object.seal | X | X | O | O | X |
| 객체 동결 | Object.freeze | X | X | O | X | X |

<br>

### 5.1. 객체 확장 금지
Object.preventExtensions 메서드는 객체의 확장을 금지한다. **확장이 금지된 객체는 프로퍼티 추가가 금지된다.** 프로퍼티는 프로퍼티 동적 추가와 Object.defineProperty 메서드로 추가할 수 있다. 이 두가지 추가 방법이 모두 금지된다. 확장이 가능한 객체인지 여부는 Object.isExtensible 메서드로 확인할 수 있다.

<br>

### 5.2. 객체 밀봉
Object.seal 메서드는 객체를 밀봉한다. **밀봉된 객체는 읽기와 쓰기만 가능하다.** 밀봉된 객체인지 여부는 Object.isSealed 메서드로 확인할 수 있다.

<br>

### 5.3. 객체 동결
Object.freeze 메서드는 객체를 동결한다. **동결된 객체는 읽기만 가능하다.** 동결된 객체인지 여부는 Object.isFrozen 메서드로 확인할 수 있다.

<br>

### 5.4. 불변 객체
앞의 변경 방지 메서드들은 얕은 변경 방지(shallow only)로 직속 프로퍼티만 변경이 방지되고 중첩 객체까지는 영향을 주지 못한다. 따라서 Object.freeze 메서드로 객체를 동결하여도 중첩 객체까지 동결할 수 없다. 

객체의 중첩 객체까지 동결하여 변경이 불가능한 읽기 전용의 **불변 객체**를 구현하려면 객체를 값으로 갖는 모든 프로퍼티에 대해 재귀적으로 Object.freeze 메서드를 호출해야 한다.

```js
function deepFreeze(target) {
  // 객체이고 동결되지 않은 객체만 동결
  if (target && typeof target === 'object' && !Object.isFrozen(target)) {
    Object.freeze(target);
    Object.keys(target).forEach(key => deepFreeze(target[key]));    // 재귀적으로 동결
  }
  return target;
}

const person = {
  name: 'Lee',
  address: { city: 'Seoul' }
};

// 깊은 객체 동결
deepFreeze(person);

console.log(Object.isFrozen(person));    // true
// 중첩 객체까지 동결
console.log(Object.isFrozen(person.address));    // true

person.address.city = 'Busan';
console.log(person);    // {name: "Lee", address: {city: "Seoul"}}
```

<br>
<br>