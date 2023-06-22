# 객체 리터럴

## 1. 객체란?
자바스크립트는 객체 기반의 프로그래밍 언어이며, 원시 값을 제외한 나머지 값(함수, 배열, 정규 표현식 등)은 모두 객체다

원시 타입은 단 하나의 값만 나타내지만 <b>객체 타입은 다양한 타입의 값(원시 값 또는 다른 객체)을 하나의 단위로 구성한 복합적인 자료구조다.</b> 또한 원시 값은 변경 불가능한 값(immutable value)이지만 객체는 변경 가능한 값(mutable value)이다.

<br>

객체는 0개 이상의 프로퍼티로 구성된 집합이며 프로퍼티는 키(key)와 값(value)으로 구성된다.
```js
var person = {
  name: 'Lee',    // 프로퍼티
  age: 20    // age : 키, 20 : 값
};
```

<br>

자바스크립트에서 사용할 수 있는 모든 값(함수 포함)은 프로퍼티 값이 될 수 있다. 프로퍼티 값이 함수일 경우, 일반 함수와 구분하기 위해 메서드(method)라 부른다.
```js
var counter = {
  num: 0,    // 프로퍼티
  increase: function() {    // 메서드
    this.num++;
  }
};
```
- 프로퍼티 : 객체의 상태를 나타내는 값(data)
- 메서드 : 프로퍼티(상태 데이터)를 참조하고 조작할 수 있는 동작(behavior)

<br>

이처럼 객체는 객체의 상태를 나타내는 값(프로퍼티)과 프로퍼티를 참조하고 조작할 수 있는 동작(메서드)을 모두 포함할 수 있기 때문에 <b>상태와 동작을 하나의 단위로 구조화할 수 있어 유용하다.</b>

<br>
<br>

## 2. 객체 리터럴에 의한 객체 생성
자바스크립트는 프로토타입 기반 객체지향 언어로서 클래스 기반 객체지향 언어와는 달리 다양한 객체 생성 방법을 지원한다.
- 객체 리터럴
- Object 생성자 함수
- 생성자 함수
- Object.create 메서드
- 클래스(ES6)

위 방법 중에서 가장 일반적이고 간당한 방법은 객체 리터를을 사용하는 방법이다. 객체 리터럴은 객체를 생성하기 위한 표기법이다.

<br>

```js 
var person = {
  name: 'Lee',
  sayHello: function() {
    console.log(`Hello! My name is ${this.name}`);
  }
};

console.log(typeof person);    // object
console.log(person);    // {name: "Lee", sayHello: f}
```
객체 리터럴은 중괄호내에 0개 이상의 프로퍼티를 정의한다. 변수에 할당되는 시점에 자바스크립트 엔진은 객체 리터럴을 해석해 객체를 생성한다.

<br>

중괄호 내에 프로퍼티를 정의하지 않으면 빈 객체가 생성된다.
```js
var empty = {};    // 빈 객체
console.log(typeof empty);    // object
```
객체 리터럴은 값으로 평가되는 표현식이다. 따라서 객체 리터럴의 닫는 중괄호 뒤에는 세미콜론을 붙인다.

<br>

<b>객체 리터럴은 자바스크립트의 유연함과 강력함을 대표하는 객체 생성 방식이다.</b> 객체를 생성하기 위해 클래스를 먼저 정의하고 new 연산자와 함께 생성자를 호출할 필요가 없다. 숫자 값이나 문자열을 만드는 것과 유사하게 리터럴로 객체를 생성한다. 객체 리터럴에 프로퍼티를 포함시켜 생성함과 동시에 프로퍼티를 만들 수도 있고, 객체를 생성한 이후에 프로퍼티를 동적으로 추가할 수도 있다.

<br>
<br>

## 3. 프로퍼티
객체는 프로퍼티의 집합이며, 프로퍼티는 키와 값으로 구성된다.
```js
var person = {
  // 프로퍼티 키 : name, 프로퍼티 값 : Lee
  name: 'Lee',
  // 프로퍼티 키 : age, 프로퍼티 값 : 20
  age: 20
};
```
프로퍼티를 나열할 때는 쉼표(,)로 구분한다.
- 프로퍼티 키 : 빈 문자열을 포함하는 모든 문자열 또는 심벌 값
- 프로퍼티 값 : 자바스크립트에서 사용할 수 있는 모든 값 

<br>

프로퍼티 키는 프로퍼티 값에 접근할 수 있는 이름으로서 식별자 역할을 하지만 반드시 식별자 네이밍 규칙을 따라야 하는 것은 아니다. <b>하지만 식별자 네이밍 규칙을 따르지 않는 이름에는 반드시 따옴표를 사용해야 한다.</b>

```js
var person = {
  firstName: 'Chae-been',   // 식별자 네이밍 규칙을 준수하는 프로퍼티 키
  'last-name': 'Jeong'    // 식별자 네이밍 규칙을 준수하지 않는 프로퍼티 키
};
```
따라서 가급적 식별자 네이밍 규칙을 준수하는 프로퍼티 키를 사용할 것을 권장한다.

<br>

```js
var foo = {
  '': ''    
};

console.log(foo);    // {"": ""}
```
빈 문자열을 프로퍼티 키로 사용해도 에러가 발생하지 않지만, 키로서의 의미를 갖지 못하므로 권장하지 않는다.

<br>

```js
var foo = {
  0: 1,
  1: 2,
  2: 3
};

console.log(foo);    // {0: 1, 1: 2, 2: 3}
```
프로퍼티 키에 문자열이나 심벌 값 외의 값을 사용하면 암묵적 타입 변환을 통해 문자열이 된다. 예를 들어, 프로퍼티 키로 숫자 리터럴을 사용하면 따옴표는 붙지 않지만 내부적으로는 문자열로 변환된다.

<br>

```js
var foo = {
  var: '',
  function: ''
};

console.log(foo);    // {var: "", function: ""}
```
var, function과 같은 예약어를 프로퍼티 키로 사용해도 에러가 발생하지 않지만, 예상치 못한 에러가 발생할 여지가 있으므로 권장하지 않는다.

<br>

```js
var foo = {
  name: 'Lee',
  name: 'Kim'
};

console.log(foo);    // {name: "Kim"}
```
이미 존재하는 프로퍼티 키를 중복 선언하면 나중에 선언한 프로퍼티가 먼저 선언한 프로퍼티를 덮어쓴다. 이때 에러가 발생하지 않는다는 점에 주의하자.

<br>
<br>

## 4. 메서드
자바스크립트에서 사용할 수 있는 모든 값은 프로퍼티 값으로 사용할 수 있다. 자바스크립트의 함수는 객체(일급 객체)다. 따라서 함수는 값으로 취급할 수 있기 때문에 프로퍼티 값으로 사용할 수 있다.

프로퍼티 값이 함수일 경우 일반 함수와 구분하기 위해 메서드(method)라 부른다. 즉, 메서드는 객체에 묶여 있는 함수를 의미한다.
```js
var circle = {
  radius: 5,    // 프로퍼티

  // 원의 지름
  getDiameter: function() {    // 메서드
    return 2 * this.radius;    // this => circle
  }
};

console.log(circle.getDiameter());    // 10
```

<br>
<br>

## 5. 프로퍼티 접근
프로퍼티에 접근할 수 있는 2가지 방법
- 마침표 표기법(dot notation) : 마침표 프로퍼티 접근 연산자(.) 사용
- 대괄호 표기법(bracket notation) : 대괄호 프로퍼티 접근 연산자([ ... ])를 사용

프로퍼티 키가 식별자 네이밍 규칙을 준수하는 이름이면 마침표 표기법과 대괄호 표기법을 모두 사용할 수 있다.

<br>

```js
var person = {
  name: 'Lee'
};

// 마침표 표기법
console.log(person.name);    // Lee

// 대괄호 표기법
console.log(person['name']);    // Lee
console.log(person[name]);    // ReferenceError: name is not defined
```
대괄호 표기법을 사용하는 경우, <b>대괄호 프로퍼티 접근 연산자 내부에 지정하는 프로퍼티 키는 반드시 따옴표로 감싼 문자열이어야 한다.</b> 대괄호 프로퍼티 접근 연산자 내에 따옴표로 감싸지 않은 이름을 프로퍼티 키로 사용하면 자바스크립트 엔진은 식별자로 해석하여 에러를 발생시킨다.

<br>

```js
var person = {
  name: 'Lee'
};

console.log(person.age);    // undefined
```
<b>객체에 존재하지 않는 프로퍼티에 접근하면 undefined를 반환한다.</b> 이때 ReferenceError가 발생하지 않는데 주의하자.

<br>

```js
var person = {
  'last-name': 'Lee',
  1: 10
};

person.'last-name';    // SyntaxError: Unexpected string
person.last-name;    // 브라우저 환경 : NaN, Node.js 환경 : ReferenceError: name is noe defined

person[last-name];    // ReferenceError: last is not defined
person['last-name'];    // Lee

person.1;    // SyntaxError: Unexpected number
person.'1';    // SyntaxError: Unexpcted string
person[1];    // 10: person[1] -> person['1']
person['1'];    // 10
```
<b>프로퍼티 키가 식별자 네이밍 규칙을 준수하지 않는 이름</b>, 즉 자바스크립트에서 사용 가능한 유효한 이름이 아니면 <b>반드시 대괄호 표기법을 사용</b>해야 한다. 단, 프로퍼티 키가 숫자로 이뤄진 문자열인 경우 따옴표를 생략할 수 있다.

person.last-name의 실행 결과가 Node.js와 브라우저 환경에서 다른 이유는?
1. 자바스크립트 엔진이 먼저 person.last를 평가 -> undefined
2. person.last-name은 undefined-name으로 평가
3. 그 다음 자바스크립트 엔진이 name이라는 식별자 찾음
4. Node.js : name이라는 식별자 선언이 없으므로 ReferenceError
5. 브라우저 : name이라는 전역 변수가 암묵적으로 존재(창(window) 이름)하고, 기본값은 빈 문자열. 따라서 person.last-name은 undefined-''과 같으므로 NaN.

<br>
<br>

## 6. 프로퍼티 값 갱신
이미 존재하는 프로퍼티에 값을 할당하면 프로퍼티 값이 갱신된다.
```js
var person = {
  name: 'Lee'
};

person.name = 'Kim';    // 프로퍼티 값 갱신

console.log(person);    // {name: "Kim"}
```

<br>
<br>

## 7. 프로퍼티 동적 생성
존재하지 않는 프로퍼티에 값을 할당하면 프로퍼티가 동적으로 생성되어 추가되고, 프로퍼티 값이 할당된다.
```js
ver person = {
  name: 'Lee'
};

person.age = 20;    // 프로퍼티 동적 생성

console.log(person);    // {name: "Lee", age: 20}
```

<br>
<br>

## 8. 프로퍼티 삭제
delete 연산자는 객체의 프로퍼티를 삭제한다. 만약 존재하지 않는 프로퍼티를 삭제하면 아무런 에러 없이 무시된다.
```js
var person = {
  name: 'Lee'
};

person.age = 20;    // 프로퍼티 동적 생성

delete person.age;    // age 프로퍼티 삭제
delete person.address;    // 무시

console.log(person);    // {name: "Lee"}
```

<br>
<br>

## 9. ES6에 추가된 객체 리터럴의 확장 기능

<br>

### 9.1. 프로퍼티 축약 표현
ES6에서는 프로퍼티 값으로 변수를 사용하는 경우 변수 이름과 프로퍼티 키가 동일한 이름일 때 프로퍼티 키를 생략할 수 있다. 이때 프로퍼티 키는 변수 이름으로 자동 생성된다.
```js
// ES6
let x = 1, y = 2;

// 프로퍼티 축약 표현
const obj = { x, y };

console.log(obj);    // {x: 1, y: 2}
```

<br>

### 9.2. 계산된 프로퍼티 이름
문자열 또는 문자열로 타입 변환할 수 있는 값으로 평가되는 표현식을 사용해 프로퍼티 키를 동적으로 생성할 수도 있다. 단, 프로퍼티 키로 사용할 표현식을 대괄호로 묶어야 한다. 이를 계산된 프로퍼티 이름이라고 한다.

<br>

- ES5
```js
var prefix = 'prop';
var i = 0;

var obj = {};

// 계산된 프로퍼티 이름으로 프로퍼티 키 동적 생성
obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;

console.log(obj);    // {prop-1: 1, prop-2: 2, prop-3: 3}
```
계산된 프로퍼티 이름으로 프로퍼티 키를 동적 생성하려면 객체 리터럴 외부에서 대괄호 표기법을 사용해야 한다.

<br>

- ES6
```js
const prefix = 'prop';
let i = 0;

// 계산된 프로퍼티 이름으로 프로퍼티 키 동적 생성
const obj = {
  [`${prefix}-${++i}`]: i,
  [`${prefix}-${++i}`]: i,
  [`${prefix}-${++i}`]: i
};

console.log(obj);    // {prop-1: 1, prop-2: 2, prop-3: 3}
```

<br>

### 9.3. 메서드 축약 표현
- ES5
```js
var obj = {
  name: 'Lee',
  sayHi: function() {
    console.log('Hi! ' + this.name);
  }
};

obj.sayHi();    // Hi! Lee
```
메서드를 정의하려면 프로퍼티 값으로 함수를 할당한다.

<br>

- ES6
```js
const obj = {
  name: 'Lee',
  // 메서드 축약 표현
  sayHi() {
    console.log('Hi! ' + this.name);
  }
};

obj.sayHi();    // Hi! Lee
```
ES6에서는 메서드를 정의할 때 function 키워드를 생략한 축약 표현을 사용할 수 있다. ES6의 메서드 축약 표현으로 정의한 메서드는 프로퍼티에 할당한 함수와 다르게 동작한다. ("메서드" 참고)

<br>
<br>