# 스프레드 문법
- ES6에서 도입된 스프레드 문법(전개 문법) ...은 하나로 뭉쳐 있는 여러 값들의 집합을 펼쳐서 개별적인 값들의 목록으로 만듦
- 스프레드 문법을 사용할 수 있는 대상 : for...of 문으로 순회할 수 있는 이터러블
  - Array, String, Map, Set, DOM 컬렉션(NodeList, HTMLCollection), arguments
  ```js
  console.log(...[1, 2, 3]);    // 1 2 3

  console.log(...'Hello');    // H e l l o

  console.log(...new Map([['a', '1'], ['b', '2']]));    // [ 'a', '1' ] [ 'b', '2' ]
  console.log(...new Set([1, 2, 3]));    // 1 2 3

  console.log(...{ a: 1, b: 2 });    // TypeError: Found non-callable @@iterator
  ```
  - 위 예제에서 ...[1, 2, 3]은 이터러블인 배열을 펼쳐서 요소들을 개별적인 값들의 목록 1 2 3으로 만드는데 이때 1 2 3은 값이 아니라 값들의 목록
- 스프레드 문법의 결과는 값이 아님 -> 스프레드 문법이 피연산자를 연산하여 값을 생성하는 연산자가 아님 -> 스프레드 문법의 결과는 변수에 할당 불가능
  ```js
  const list = ...[1, 2, 3];    // SyntaxError: Unexpected token ...
  ```
- 스프레드 문법의 결과물은 값으로 사용 불가능
- 쉼표로 구분한 값의 목록을 사용하는 문맥에서만 사용 가능
  - 함수 호출문의 인수 목록
  - 배열 리터럴의 요소 목록
  - 객체 리터럴의 프로퍼티 목록

<br>
<br>

## 1. 함수 호출문의 인수 목록에서 사용하는 경우
- 배열을 펼쳐서 개별적인 값들의 목록으로 만든 후, 이를 함수의 인수 목록으로 전달해야 하는 경우
  ```js
  const arr = [1, 2, 3];

  const max = Math.max(arr);    // NaN
  ```
  - Math.max 메서드는 가변 인자 함수인데 숫자가 아닌 배열을 인수로 전달하면 최대값을 구할 수 없으므로 NaN 반환
- 이 같은 문제를 해결하기 위해 요소들을 개별적인 값들의 목록으로 만든 후, Math.max 메서드의 인수로 전달
  ```js
  const arr = [1, 2, 3];

  const max = Math.max(...arr);    // 3
  ```
- 스프레드 문법은 Rest 파라미터와 형태가 동일하므로 주의 필요
  - Rest 파라미터
    - 함수에 전달된 인수들의 목록을 배열로 전달받기 위해 매개변수 이름 앞에 ...을 붙이는 것
  - 스프레드 문법
    - 여러 개의 값이 하나로 뭉쳐 있는 이터러블을 펼쳐서 개별적인 값들의 목록으로 만드는것
  - 서로 반대 개념
  ```js
  // Rest
  function foo(...rest) {
    console.log(rest);    // 1, 2, 3 -> [ 1, 2, 3 ]
  }

  foo(...[1, 2, 3]);    // [1, 2, 3] -> 1, 2, 3
  ```

<br>
<br>

## 2. 배열 리터럴 내부에서 사용하는 경우
- 스프레드 문법을 배열 리터럴에서 사용하면 ES5에서 사용하던 기존의 방식보더 더욱 간결하고 가독성이 좋음

<br>

### 2.1. concat
- ES5에서 2개의 배열을 1개의 배열로 결합하고 싶은 경우 배열 리터럴만으로 해결할 수 없고 concat 메서드 사용
  ```js
  // ES5
  var arr = [1, 2].concat([3, 4]);
  console.log(arr);    // [1, 2, 3,4]
  ```
- 스프레드 문법을 사용하면 배열 리터럴만으로 2개의 배열을 1개의 배열로 결합 가능
  ```js
  // ES6
  const arr = [...[1, 2], ...[3, 4]];
  console.log(arr);    // [1, 2, 3, 4]
  ```

<br>

### 2.2. splice
- ES5에서 어떤 배열의 중간에 다른 배열의 요소들을 추가하거나 제거하려면 splice 메서드 사용
  - splice 메서드에 세 번째 인수로 배열을 전달하면 배열 자체가 추가
  ```js
  // ES5
  var arr1 = [1, 4];
  var arr2 = [2, 3];

  arr.splice(1, 0, arr2);
  console.log(arr1);    // [1, [2, 3], 4]
  ```
  - 위 예제의 경우 세 번째 인수 [2, 3]을 2, 3으로 해체하여 전달해야 함 
- 위 문제를 해결하기 위해 스프레드 문법 사용
  ```js
  // ES6
  const arr1 = [1, 4];
  const arr2 = [2, 3];

  arr1.splice(1, 0, ...arr2);
  console.log(arr1);    // [1, 2, 3, 4]
  ```

<br>

### 2.3. 배열 복사
- ES5에서 배열을 복사하려면 slice 메서드 사용
  - 원본 배열의 각 요소를 얕은 복사하여 새로운 복사본 생성
  ```js
  // ES5
  var origin = [1, 2];
  var copy = origin.slice();

  console.log(copy);    // [1, 2]
  console.log(copy === origin);    // false
  ```
- 스프레드 문법을 사용하면 더욱 간결하고 가독성이 좋음
  - 원본 배열의 각 요소를 얕은 복사하여 새로운 복사본 생성
  ```js
  // ES6
  const origin = [1, 2];
  const copy = [...origin];

  console.log(copy);    // [1, 2]
  console.log(copy === origin);    // false
  ```

<br>

### 2.4. 이터러블을 배열로 변환
- ES5에서 이터러블을 배열로 변환하려면 Function.prototype.apply 또는 Function.prototype.call 메서드를 사용하여 slice 메서드 호출
  ```js
  // ES5
  function sum() {
    // 이터러블이면서 유사 배열 객체인 arguments를 배열로 변환
    var args = Array.prototype.slice.call(arguments);

    return args.reduce(function (pre, cur) {
      return pre + cur;
    }, 0);
  }

  console.log(sum(1, 2, 3));    // 6
  ```
- 이 방법은 이터러블이 아닌 유사 배열 객체도 배열로 변환 가능
  ```js
  // 이터러블이 아닌 유사 배열 객체
  const arrayLike = {
    0: 1,
    1: 2,
    2: 3,
    length: 3
  };

  const arr = Array.prototype.slice.call(arrayLike);    // [1, 2, 3]
  console.log(Array.isArray(arr));    // true
  ```
- 스프레드 문법을 사용하면 좀 더 간편하게 변환 가능
  - arguments 객체는 이터러블이면서 유사 배열 객체이기 때문에 스프레드 문법 대상
  ```js
  function sum() {
    return [...arguments].reduce((pre, cur) => pre + cur, 0);
  }

  console.log(sum(1, 2, 3));    // 6
  ```
- 더 나은 방법은 Rest 파라미터 이용하는 방법
  - Rest 파라미터 args는 함수에 전달된 인수들의 목록을 배열로 전달받음
  ```js
  const sum = (...args) => args.reduce((pre, cur) => pre + cur, 0);

  console.log(sum(1, 2, 3));    // 6
  ```
- 단, 이터러블이 아닌 유사 배열 객체는 스프레드 문법의 대상 X
- 이터러블이 아닌 유사 배열 객체를 배열로 변경하려면 ES6에서 도입된 Array.from 메서드 사용
  - Array.from 메서드는 유사 배열 객체 또는 이터러블을 인수로 전달받아 배열로 변환하여 반환
  ```js
  const arrayLike = {
    0: 1,
    1: 2,
    2: 3,
    length: 3
  };

  Array.from(arrayLike);    // [1, 2, 3]
  ```

<br>
<br>

## 3. 객체 리터럴 내부에서 사용하는 경우
- 스프레드 프로퍼티를 사용하면 객체 리터럴의 프로퍼티 목록에서도 스프레드 문법 사용 가능
  - 스프레드 문법의 대상은 이터러블이어야 하지만 스프레드 프로퍼티 제안은 일반 객체를 대상으로도 스프레드 문법 사용 허용
  ```js
  // 스프레드 프로퍼티
  // 객체 복사 (얕은 복사)
  const obj = { x: 1, y: 2 };
  const copy = { ...obj };
  console.log(copy);    // { x: 1, y: 2 }
  console.log(obj === copy);    // false

  // 객체 병합
  const merged = { x: 1, y: 2, ...{ a: 3, b: 4 }};
  console.log(merged);    // { x: 1, y: 2, a: 3, b: 4 }
  ```
- 스프레드 프로퍼티가 제안되기 이전에는 ES6에서 도입된 Object.assign 메서드 사용하여 여러 개의 객체를 병합하거나 특정 프로퍼티 변경 또는 추가
  - 프로퍼티가 중복되는 경우 뒤에 위치한 프로퍼티가 우선권을 가짐
  ```js
  // 객체 병합
  const merged = Object.assign({}, { x: 1, y: 2 }, { y: 10, z: 3 });
  console.log(merged);    // { x: 1, y: 10, z: 3 }

  // 특정 프로퍼티 변경
  const changed = Object.assign({}, { x: 1, y: 2 }, { y: 100 });
  console.log(changed);    // { x: 1, y: 100 }

  // 프로퍼티 추가
  const added = Object.assign({}, { x: 1, y: 2 }, { z: 0 });
  console.log(added);    // { x: 1, y: 2, z: 0 }
  ```
- 스프레드 프로퍼티는 Object.assign 메서드 대체 가능
  - 프로퍼티가 중복되는 경우 뒤에 위치한 프로퍼티가 우선권을 가짐
  ```js
  // 객체 병합
  const merged = { ...{ x: 1, y: 2 }, ...{ y: 10, z: 3 }};
  console.log(merged);    // { x: 1, y: 10, z: 3 }

  // 특정 프로퍼티 변경
  const changed = { ...{ x: 1, y: 2 }, y: 100 };
  console.log(changed);    // { x: 1, y: 100 }

  // 프로퍼티 추가
  const added = { ...{ x: 1, y: 2 }, z: 0};
  console.log(added);    // { x: 1, y: 2, z: 0 }
  ```

<br>
<br>