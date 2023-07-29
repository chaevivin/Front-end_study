# 이터러블

## 1. 이터레이션 프로토콜
- ES6에서 도입된 이터레이션 프로토콜은 순회 가능한(iterable) 데이터 컬렉션(자료구조)을 만들기 위해 ECMAScript 사양에 정의하여 미리 약속한 규칙
- ES6에서 순회 가능한 데이터 컬렉션을 이터레이션 프로토콜을 준수하는 이터러블로 통일하여 for ... of문, 스프레드 문법, 배열 디스트럭처링 할당의 대상으로 사용할 수 있도록 일원화
- 이터레이션 프로토콜
  - 이터러블 프로토콜 (iterable protocol)
    - Well-known Symbol인 Symbol.iterator를 프로퍼티 키로 사용한 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터 반환하는 규약
    - **이터러블 : 이터러블 프로토콜을 준수한 객체**
    - **이터러블은 for...of 문으로 순회 가능, 스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용 가능**
  - 이터레이터 프로토콜 (iterator protocol)
    - 이터러블의 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터 반환
    - 이터레이터는 next 메서드를 소유하고, next 메서드를 호출하면 이터러블을 순회하고, value와 done 프로퍼티를 갖는 **이터레이터 리절트 객체** 반환하는 규약
    - **이터레이터 : 이터레이터 프로토콜을 준수하는 객체**
    - 이터레이터는 이터러블의 요소를 탐색하기 위한 포인터 역할

<br>

### 1.1. 이터러블
- 이터러블 프로토콜을 준수하는 객체
- 이터러블은 Symbol.iterator를 프로퍼티 키로 사용한 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체
- 이터러블인지 확인하는 함수
  ```js
  const isIterable = v => v !== null && typeof v[Symbol.iterator] === 'function';

  // 배열, 문자열, Map, Set 등은 이터러블
  isIterable([]);    // true
  isIterable('');    // true
  isIterable(new Map());    // true
  isIterable(new Set());    // true
  isIterable({});    // false
  ```
- 이터러블은 for...of문으로 순회 가능하고, 스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용 가능
  ```js
  const array = [1, 2, 3];

  // 배열은 Array.prototype의 Symbol.iterator 메서드를 상속받는 이터러블
  console.log(Symbol.iterator in array);    // true

  // for...of 순회 
  for (const item of array) {
    console.log(item);
  }
  
  // 스프레드 문법의 대상
  console.log([...array]);    // [1, 2, 3]

  // 배열 디스트럭처링 할당의 대상
  const [a, ...rest] = array;
  console.log(a, rest);    // 1, [2, 3]
  ```
- Symbol.iterator 메서드를 직접 구현하지 않거나 상속받지 않은 일반 객체는 이터러블 프로토콜을 준수한 이터러블이 아니기 때문에 for...of문으로 순회하거나 스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용 불가능
  - TC39 프로세스의 stage 4 단계에 제안되어 있는 스프레드 프로퍼티 제안은 일반 객체에 스프레드 문법 사용 허용
  - 일반 객체도 이터러블 프로토콜을 준수하도록 구현하면 이터러블이 됨
  ```js
  const obj = { a: 1, b: 2 };

  // 일반 객체는 Symbol.iterator 메서드를 구현하거나 상속받지 X
  console.log(Symbol.iterator in obj);    // false

  // for...of 순회 불가능
  for (const item of obj) {
    console.log(item);    // TypeError: obj is not iterable
  }

  // 배열 디스트럭처링 할당 대상 X
  const [a, b] = obj;    // TypeError: obj is not iterable

  // 스프레드 문법 사용 가능
  console.log({ ...obj });    // { a: 1, b: 2 }
  ```

<br>

### 1.2. 이터레이터
- 이터러블의 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터 반환
- **이터러블의 Symbol.iterator 메서드가 반환한 이터레이터는 next 메서드를 갖고 있음**
  ```js
  const array = [1, 2, 3];

  const iterator = array[Symbol.iterator]();

  console.log('next' in iterator);    // true
  ```
- 이터레이터의 next 메서드는 이터러블의 각 요소를 순회하기 위한 포인터 역할
  - next 메서드를 호출하면 이터러블을 순차적으로 한 단계씩 순회하며 순회 결과를 나타내는 **이터레이터 리절트 객체**를 반환
  ```js
  const array = [1, 2, 3];

  const iterator = array[Symbol.iterator]();

  console.log(iterator.next());    // { value: 1, done: false }
  console.log(iterator.next());    // { value: 2, done: false }
  console.log(iterator.next());    // { value: 3, done: false }
  console.log(iterator.next());    // { value: undefined, done: true }
  ```
  - value 프로퍼티 : 현재 순회 중인 이터러블의 값
  - done 프로퍼티 : 이터러블의 순회 완료 여부

<br>
<br>

## 2. 빌트인 이터러블
- 자바스크립트는 이터레이션 프로토콜을 준수한 객체인 빌트인 이터러블 제공
  | 빌트인 이터러블 | Symbol.iterator 메서드 |
  | :--- | :--- |
  | Array | Array.prototype[Symbol.iterator] |
  | String | String.prototype[Symbol.iterator] |
  | Map | Map.prototype[Symbol.iterator] |
  | Set | Set.prototype[Symbol.iterator] |
  | TypedArray | TypedArray.prototype[Symbol.iterator] |
  | arguments | arguments[Symbol.iterator] |
  | DOM 컬렉션 | NodeList.prototype[Symbol.iterator] <br> HTMLCollection.prototype[Symbol.iterator] |

<br>
<br>

## 3. for...of 문
- for...of문은 이터러블을 순회하면서 이터러블의 요소를 변수에 할당
  ```js
  for (변수 선언문 of 이터러블) { ... }
  ```
- 내부적으로 이터레이터의 next 메서드를 호출하여 이터러블을 순회하며 next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티 값을 for...of 문의 변수에 할당 
  - 이터레이터 리절트 객체의 done 프로퍼티 값이 false이면 이터르블의 순회를 계속
  - 이터레이터 리절트 객체의 done 프로퍼티 값이 true이면 이터러블의 순회 중단
  ```js
  for (const item of [1, 2, 3]) {
    console.log(item);    // 1 2 3
  }
  ```
  ```js
  // for문으로 표현
  const iterable = [1, 2, 3];

  // 이터레이터 생성
  const iterator = iterable[Symbol.iterator]();

  for (;;) {
    // next 메서드 호출하여 이터러블 순회
    // next 메서드는 이터레이터 리절트 객체 반환
    const res = iterator.next();

    // done이 true이면 순회 중단
    if (res.done) break;

    // value 값을 item 변수에 할당
    const item = res.value;
    console.log(item);    // 1 2 3
  }
  ```

<br>
<br>

## 4. 이터러블과 유사 배열 객체
- 유사 배열 객체는 마치 배열처럼 인덱스로 프로퍼티 값에 접근 가능하고 length 프로퍼티를 갖는 객체
- 유사 배열 객체는 length 프로퍼티를 갖기 때문에 for문으로 순회 가능하고, 인덱스로 프로퍼티 값에 접근 가능
- 유사 배열 객체는 이터러블이 아닌 일반 객체이기 때문에 Symbol.iterator 메서드가 없어 for...of 문으로 순회 불가능
  ```js
  // 유사 배열 객체
  const arrayLike = {
    0: 1,
    1: 2,
    2: 3,
    length: 3
  };

  for (let i = 0; i < arrayLike.length; i++) {
    console.log(arrayLike[i]);    // 1 2 3
  }

  for (const item of arrayLike) {
    console.log(item);    // 1 2 3
  }
  // TypeError: arrayLike is not iterable
  ```
- 배열, arguments, NodeList, HTMLCollection은 유사 배열 객체이면서 이터러블 
  - ES6에서 위 객체들에 Symbol.iterator 메서드 구현하여 이터러블이 됨
- ES6에서 도입된 Array.from 메서드를 사용하면 유사 배열 객체 또는 이터러블을 배열로 변환 가능
  - Array.from 메서드는 유사 배열 객체 또는 이터러블을 인수로 전달받아 배열로 변환하여 반환
  ```js
  const arrayLike = {
    0: 1, 
    1: 2,
    2: 3,
    length: 3
  };

  const arr = Array.from(arrayLike);
  console.log(arr);    // [1, 2, 3]
  ```

<br>
<br>

## 5. 이터레이션 프로토콜의 필요성
- 이터러블은 for...of 문, 스프레드 문법, 배열 디스트럭처링 할당과 같은 데이터 소비자에 의해 사용되므로 데이터 공급자 역할
  - 다양한 데이터 공급자가 각자의 순회 방식을 갖는다면 데이터 소비자는 다양한 데이터 곱자의 순회 방식을 모두 지원해야 하기 때문에 비효율적
  - 다양한 데이터 공급자가 이터레이션 프로토콜을 준수하도록 규정하면 데이터 소비자는 이터레이션 프로토콜만 지원하도록 구현하면 됨
    - 이터르블을 지원하는 데이터 소비자는 내부에서 Symbol.iterator 메서드를 호출해 이터레이터를 생성하고 이터레이터의 next 메서드를 호출하여 이터러블을 순회하며 이터레이터 리절트 객체 반환하고, value/done 프로퍼티 값을 취득
- 이터레이션 프로토콜은 다양한 데이터 공급자가 하나의 순회 방식을 갖도록 규정하여 데이터 소비자가 효율적으로 다양한 데이터 공급자를 사용할 수 있도록 **데이터 소비자와 데이터 공급자를 연결하는 인터페이스 역할**

<br>
<br>

## 6. 사용자 정의 이터러블

<br>

### 6.1. 사용자 정의 이터러블 구현
- 이터레이션 프로토콜을 준수하지 않는 일반 객체도 이터레이션 프로토콜을 준수하도록 구현하면 사용자 정의 이터러블이 됨
- 피보나지 수열을 구현한 사용자 정의 이터러블
  ```js
  const fibonacci = {
    [Symbol.iterator]() {
      let [pre, cur] = [0, 1];
      const max = 10;    // 수열의 최대값

      return {
        next() {
          [pre, cur] = [cur, pre + cur];
          return { value: cur, done: cur >= max };
        }
      };
    }
  };

  for (const num of fibonacci) {
    console.log(num);    // 1 2 3 5 8
  }

  const arr = [...fibonacci];
  console.log(arr);    // [ 1, 2, 3, 5, 8 ]

  const [first, second, ...rest] = fibonacci;
  console.log(first, second, rest);    // 1 2 [ 3, 5, 8 ]
  ```
  - 이터레이션 프로토콜을 준수하도록 Symbol.iterator 메서드를 구현
  - Symbol.iterator 메서드가 next 메서드를 갖는 이터레이터를 반환
  - 이터레이터의 next 메서드는 done과 value 프로퍼티를 가지는 이터레이터 리절트 객체 반환
  - for...of 문은 done 프로퍼티가 true가 될 때까지 반복

<br>

### 6.2. 이터러블을 생성하는 함수
- 앞에서 살펴본 피보나치 이터러블의 max는 고정된 값으로 외부에 전달 불가능
- 수열의 최대값을 인수로 전달받아 이터러블을 반환하는 함수를 만들면 외부로 전달 가능
  ```js
  const fibonacciFunc = function (max) {
    let [pre, cur] = [0, 1];

    return {
      [Symbol.iterator]() {
        return {
          next() {
            [pre, cur] = [cur, pre + cur];
            return { value: cur, done: cur >= max };
          }
        };
      }
    };
  };
  ```

<br>

### 6.3. 이터러블이면서 이터레이터인 객체를 생성하는 함수
- fibonacciFunc 함수는 이터러블을 반환
- 이터레이터를 생성하려면 이터러블의 Symbol.iterator 메서드를 호출
  ```js
  const iterable = fibonacciFunc(5);
  const iterator = iterable[Symbol.iterator]();

  console.log(iterator.next());    // { value: 1, done: false }
  console.log(iterator.next());    // { value: 2, done: false }
  console.log(iterator.next());    // { value: 3, done: false }
  console.log(iterator.next());    // { value: undefined, done: true }
  ```
- 이터러블이면서 이터레이터인 객체를 생성하면 Symbol.iterator 메서드를 호출하지 않아도 됨
  ```js
  {
    [Symbol.iterator]() { return this; },
    next() {
      return { value: any, done: boolean };
    }
  }
  ```
  - 위 객체는 Symbol.iterator 메서드와 next 메서드를 소유한 이터러블이면서 이터레이터
  - Symbol.iterator 메서드는 this를 반환하므로 next 메서드를 갖는 이터레이터 반환

<br>

- fibonacciFunc 함수를 이터러블이면서 이터레이터인 객체를 생성하여 반환하는 함수로 변경
  ```js
  const fibonacciFunc = function (max) {
    let [pre, cur] = [0, 1];

    return {
      [Symbol.iterator]() { return this; },
      next() {
        [pre, cur] = [cur, pre + cur];
        return { value: cur, done: cur >= max };
      }
    };
  };

  // iter는 이터러블이면서 이터레이터
  let iter = fibonacciFunc(10);

  for (const num of iter) {
    console.log(num);    // 1 2 3 5 8
  }

  iter = fibonacciFunc(10);

  console.log(iter.next());    // { value: 1, done: false }
  console.log(iter.next());    // { value: 2, done: false }
  console.log(iter.next());    // { value: 3, done: false }
  console.log(iter.next());    // { value: 5, done: false }
  console.log(iter.next());    // { value: 8, done: false }
  console.log(iter.next());    // { value: undefined, done: true }
  ```

<br>

### 6.4. 무한 이터러블과 지연 평가
- 무한 이터러블을 생성하는 함수로 무한 수열 구현 가능
  ```js
  // 무한 이터러블을 생성하는 함수
  const fibonacciFunc = function () {
    let [pre, cur] = [0, 1];

    return {
      [Symbol.iterator]() { return this; },
      next() {
        [pre, cur] = [cur, pre + cur];
        // 무한을 구현해야하므로 done 프로퍼티 생략
        return { value: cur };
      }
    };
  };

  for (const num of fibonacciFunc()) {
    if (num > 10000) break;
    console.log(num);    // 1 2 3 5 8 ... 4181 6785
  }

  const [f1, f2, f3] = fibonacciFunc();
  console.log(f1, f2, f3);    // 1 2 3
  ```
- 배열이나 문자열 등은 모든 데이터를 메모리에 미리 확보한 다음 데이터를 공급하지만 위의 이터러블은 **지연 평가**를 통해 데이터 생성
  - 지연 평가 : 데이터가 필요한 시점 이전까지는 미리 데이터를 생성하지 않다가 데이터가 필요한 시점이 되면 그때야 비로소 데이터를 생성하는 기법
  - 평가 결과가 필요할 때까지 평가를 늦추는 기법
- fibonacciFunc 함수가 생성한 무한 이터러블은 데이터를 공급하는 메커니즘을 구현한 것으로 데이터 소비자인 for...of 문이나 배열 디스트럭처링 할당 등이 실행되기 이전까지 데이터 생성 X
  - for...of문의 경우 이터러블을 순회할 때 내부에서 이터레이터의 next 메서드를 호출하는데 바로 이때 데이터 생성
  - next 메서드가 호출되기 이전까지는 데이터 생성 X
  - 데이터가 필요할 때까지 데이터의 생성을 지연하다가 데이터가 필요한 순간 데이터 생성
- 지연 평가 장점
  - 불필요한 데이터를 미리 생성하지 않고 데이터를 필요한 순간에 생성
  - 빠른 실행 속도
  - 불필요한 메모리 소비 X
  - 무한 표현 가능

<br>
<br>