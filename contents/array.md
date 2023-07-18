# 배열

## 1. 배열이란?
- 배열(array)은 여러 개의 값을 순차적으로 나열한 자료구조
```js
const arr = ['apple', 'banana', 'orange'];
```
- **요소** : 배열이 가지고 있는 값
  - 자바스크립트의 모든 값은 배열의 요소가 될 수 있음
  - 배열의 요소는 배열에서 자신의 위치를 나타내는 0 이상의 정수인 **인덱스(index)** 를 갖고 있음
  - 요소에 접근할 때는 대괄호 표기법 사용
  ```js
  arr[0]    // 'apple'
  arr[1]    // 'banana'
  arr[2]    // 'orange'
  ```
  - 배열은 요소의 개수, 즉 배열의 길이를 나타내는 **length 프로퍼티** 를 갖고 있음
  ```js
  arr.length    // 3
  ```
  - 배열은 length 프로퍼티를 갖기 때문에 for문을 통해 순차적으로 요소에 접근 가능
  ```js
  // 배열 순회
  for (let i = 0; i < arr.length; i++) {
    console.log(arr[i]);    // 'apple' 'banana' 'orange'
  }
  ```
- 자바스크립트에 배열이라는 타입 존재 X -> 배열은 객체 타입
  ```js
  typeof arr    // object
  ```
- 배열은 배열 리터럴, Array 생성자 함수, Array.of, Array.from 메서드로 생성
  - 배열의 생성자 함수는 Array, 배열의 프로토타입 객체는 Array.prototype
    - Array.prototype은 배열을 위한 빌트인 메서드 제공
  ```js
  const arr = [1, 2, 3];

  arr.constructor === Array    // true
  Object.getPrototypeOf(arr) === Array.prototype    // true
  ```
- 배열과 일반 객체의 차이점
  | 구분 | 객체 | 배열 |
  | :---: | :---: | :---: |
  | 구조 | 프로퍼티 키와 프로퍼티 값 | 인덱스와 요소 |
  | 값의 참조 | 프로퍼티 키 | 인덱스 |
  | 값의 순서 | X | O |
  | length 프로퍼티 | X | O |
  - 일반 객체와 배열의 가장 명확한 차이는 "값의 순서"와 "length 프로퍼티"
- 배열의 장점
  - 처음부터 순차적으로 요소에 접근 가능
  - 마지막부터 역순으로 요소에 접근 가능
  - 특정 위치부터 순차적으로 요소에 접근 가능 <br>
  -> 배열이 인덱스, 즉 값의 순서 length 프로퍼티를 갖기 때문에 가능

<br>
<br>

## 2. 자바스크립트 배열은 배열이 아니다
- 자료구조에서 말하는 배열 (**밀집 배열**)
  - 동일한 크기의 메모리 공간이 빈틈없이 연속적으로 나열된 자료구조
  - 배열의 요소는 하나의 데이터 타입으로 통일되어 있고 서로 연속적으로 인접
  - 인덱스를 통해 단 한번의 연산으로 임의의 요소에 접근 가능 -> 매우 효율적이고 고속으로 동작
  - 정렬되지 않은 배열에서 특정한 요소를 검색하는 경우 배열을 처음부터 모두 차례대로 검색해야 함
  - 배열에 요소를 삽입하거나 삭제하는 경우 배열의 요소를 연속적으로 유지하기 위해 요소 이동

<br>

- 자바스크립트의 배열
  - 배열의 요소를 위한 각각의 메모리 공간은 동일한 크기 X
  - 연속적으로 이어져 있지 않아도 됨 -> **희소 배열**
  - **자바스크립트의 배열은 일반적일 배열의 동작을 흉내 낸 특수한 객체**

<br>

```js
console.log(Object.getOwnPropertyDescriptors([1, 2, 3]));
/*
{
  '0': {value: 1, writable: true, enumerable: true, configurable: true}
  '1': {value: 2, writable: true, enumerable: true, configurable: true}
  '2': {value: 3, writable: true, enumerable: true, configurable: true}
  length: {value: 3, writable: true, enumerable: false, configurable: false}
}
*/
```
위의 코드를 살펴보면,
- 자바스크립트 배열은 인덱스를 나타내는 문자열을 프로퍼티 키로 가짐
- length 프로퍼티를 갖는 특수한 객체
- 자바스크립트 배열의 요소는 프로퍼티 값
- 자바스크립트에서 사용할 수 있는 모든 값은 객체의 프로퍼티 값이 될 수 있으므로 어떤 타입의 값이라도 배열의 요소가 될 수 있음
  ```js
  const arr = [
    'string',
    10,
    true,
    null,
    undefined,
    NaN,
    Infinity,
    [ ],
    { },
    function () {}
  ];
  ```

<br>

- 일반 배열과 자바스크립트 배열의 장단점
  - 일반 배열은 인덱스로 요소에 빠르게 접근 가능. 하지만 특정 요소를 검색하거나 요소를 삽입 또는 삭제하는 경우에는 비효율적
  - 자바스크립트 배열은 해시 테이블로 구현된 객체이므로 인덱스로 요소에 접근하는 경우 일반 배열보다 성능적으로 느림. 하지만 특정 요소를 검색하거나 요소를 삽입 또는 삭제하는 경우 일반 배열보다 빠른 성능
    - 인덱스로 배열 요소에 접근할 때 느릴 수 밖에 없는 구조적인 단점을 보완하기 위해 대부분의 모던 자바스크립트 엔진은 배열을 일반 객체와 구별하여 좀 더 배열처럼 동작하도록 최적화

<br>
<br>

## 3. length 프로퍼티와 희소 배열
- length 프로퍼티는 요소의 개수, 즉 배열의 길이를 나타내는 0 이상의 정수를 값으로 갖고 있음
  - 빈 배열일 경우 length 값 = 0
  - 빈 배열이 아닐 경우 length 값 = 가장 큰 인덱스 + 1
  ```js
  [].length    // 0
  [1, 2, 3].length    // 3
  ```
- length 프로퍼티의 값은 0 ~ 2^32 - 1 (4,294,296 - 1) 미만의 양의 정수
  - 가장 작은 인덱스 : 0
  - 가장 큰 인덱스 : 2^32 - 2 (4,294,294)
- length 프로퍼티 값은 배열에 요소를 추가하거나 삭제하면 자동 갱신
- length 프로퍼티 값은 요소의 개수, 즉 배열의 길이를 바탕으로 결정되지만 임의의 숫자 값을 명시적으로 할당 가능
  - length 프로퍼티 값보다 작은 숫자 값을 할당하면 배열의 길이가 줄어듦
  ```js
  const arr = [1, 2, 3, 4, 5];

  arr.length = 3;

  console.log(arr);    // [1, 2, 3]
  ```
  - length 프로퍼티 값보다 큰 숫자 값을 할당하면 length 프로퍼티 값은 변경되지만 실제로 배열의 길이가 늘어나지 X
  ```js
  const arr = [1];

  arr.length = 3;

  console.log(arr.length);    // 3
  console.log(arr);    // [1, <2 empty items>]
  ```
  - 값 없이 비어 있는 요소를 위해 메모리 공간을 확보하지 않으며 빈 요소 생성 X
  ```js
  console.log(Object.getOwnPropertyDescriptor(arr));
  /*
  {
    '0': {value: 1, writable: true, enumerable: true, configurable: true}
  length: {value: 3, writable: true, enumerable: false, configurable: false}
  }
  */
  ```

<br>

- 희소 배열 : 배열의 요소가 연속적으로 위치하지 않고 일부가 비어 있는 배열
  - 중간, 앞, 뒤 모두 비어 있을 수 있음
  ```js
  // 희소 배열
  const sparse = [, 2, , 4];

  console.log(sparse.length);    // 4
  console.log(sparse);    // [empty, 2, empty, 4]

  console.log(Object.getOwnPropertyDescirptors(sparse));
  /*
  {
    '1': {value: 2, writable: true, enumerable: true, configurable: true}
    '3': {value: 4, writable: true, enumerable: true, configurable: true}
  length: {value: 4, writable: true, enumerable: false, configurable: false}
  }
  */
  ```
  - **희소 배열은 length와 배열 요소의 개수가 불일치**
    - **희소 배열의 length > 희소 배열의 실제 요소 개수**
- 희소 배열 사용 권장 X
  - 의도적으로 희소 배열을 만들어야 하는 상황 발생 X
  - 희소 배열은 연속적인 값의 집합이라는 배열의 기본적인 개념과 맞지 않고 성능에도 좋지 않은 영향을 줌
- **배열에는 같은 타입의 요소를 연속적으로 위치시키는 것이 최선**

<br>
<br>

## 4. 배열 생성

<br>

### 4.1. 배열 리터럴
- 배열 리터럴은 0개 이상의 쉼표로 구분하여 대괄호로 묶음
- 배열 리터럴은 객체 리터럴과 달리 프로퍼티 키가 없고 값만 존재
  ```js
  const arr = [1, 2, 3];
  console.log(arr.length);    // 3
  ```
- 배열 리터럴에 요소를 하나도 추가하지 않으면 배열의 길이, 즉 length 프로퍼티 값이 0인 빈 배열 생성
  ```js
  const arr = [];
  console.log(arr.length);    // 0
  ```
- 배열 리터럴에 요소 생략하면 희소 배열 생성
  ```js
  const arr = [1, , 3];

  console.log(arr.legnth);    // 3
  console.log(arr);    // [1, empty, 3]
  console.log(arr[1]);    // undefined
  ```
  - 위 예제의 arr은 인덱스가 1인 요소를 갖지 않는 희소 배열
  - arr[1]이 undefined인 이유 : 객체인 arr에 프로퍼티 키가 '1'인 프로퍼티가 존재하지 않기 때문

<br>

### 4.2. Array 생성자 함수
- Array 생성자 함수를 통해 배열 생성 가능
- Array 생성자 함수는 전달된 인수의 개수에 따라 다르게 동작하므로 주의 필요
  - 전달된 인수가 1개이고 숫자인 경우 length 프로퍼티 값이 인수인 배열 생성
    - 이때 생성된 배열은 희소 배열이고 length 프로퍼티 값은 0이 아니지만 실제로 배열의 요소는 존재하지 않음
    - 생성자 함수에 전달할 인수는 0 또는 2^32 - 1 (4,294,967,295) 이하인 양의 정수 -> 전달된 인수가 범위를 벗어나면 RangeError 발생
  ```js
  const arr = new Array(10);

  console.log(arr);    // [empty * 10]
  console.log(arr.length);    // 10
  ```
  ```js
  new Array(4294967295);
  new Array(4294967296);    // RangeError: Invalid array length
  new Array(-1);    // RangeError: Invalid array length
  ```
  - 전달된 인수가 없는 경우 빈 배열 생성. 배열 리터럴 []과 동일
  ```js
  new Array();    // []
  ```
  - 전달된 인수가 2개 이상이거나 숫자가 아닌 경우 인수를 요소로 갖는 배열 생성
  ```js
  new Array(1, 2, 3);    // [1, 2, 3]
  new Array({});    // [{}]
  ```
- Array 생성자 함수는 new 연산자와 함께 호출하지 않아도 (일반 함수로서 호출해도) 배열을 생성하는 생성자 함수로 동작 -> Array 생성자 함수 내부에서 new.target을 확인하기 때문

<br>

### 4.3. Array.of
- ES6에 도입된 Array.of 메서드는 전달된 인수를 요소로 갖는 배열을 생성
- Array 생성자 함수와 다르게 전달된 인수가 1개이고 숫자이더라도 인수를 요소로 갖는 배열 생성
  ```js
  Array.of(1);    // [1]
  Array.of(1, 2, 3);    // [1, 2, 3]
  Array.of('string');    // ['string']
  ```
  
<br>

### 4.4. Array.from
- ES6에 도입된 Array.from 메서드는 유사 배열 겍체 또는 이터러블 객체를 인수로 전달받아 배열로 변환하여 반환
  ```js
  // 유사 배열 객체 -> 배열
  Array.from({ length: 2, 0: 'a', 1: 'b' });    // ['a', 'b']

  // 이터러블 객체 -> 배열
  Array.from('Hello');    // ['H', 'e', 'l', 'l', 'o']
  ```
- Array.from을 사용하면 두 번째 인수로 전달한 콜백 함수를 통해 값을 만들면서 요소 채우기 가능
  - Array.from 메서드는 두 번째 인수로 전달한 콜백 함수에 첫 번째 인수에 의해 생성된 배열의 요소값과 인덱스를 순차적으로 전달하면서 호출하고, 콜백 함수의 반환값으로 구성된 배열을 반환
  ```js
  Array.from({ length: 3 });    // [undefined, undefined, undefined]
  Array.from({ length: 3}, (_, i) => i);    // [0, 1, 2]
  ```
  - Array.from에 length만 존재하는 유사 배열 객체를 전달하면 undefined로 요소를 채움
  - Array.from은 두 번째 인수로 전달한 콜백 함수의 반환값으로 구성된 배열 반환

<br>
<br>

## 5. 배열 요소의 참조
- 배열의 요소를 참조할 때에는 대괄호 표기법 사용
  - 대괄호 안에는 인덱스
  - 정수로 평가되는 표현식이라면 인덱스 대신 사용 가능
  - 인덱스는 값을 참조할 수 있다는 의미에서 객체의 프로퍼티 키와 같은 역할을 함
  ```js
  const arr = [1, 2];

  console.log(arr[0]);    // 1
  console.log(arr[1]);    // 2
  ```
- 존재하지 않는 요소에 접근하면 undefined 반환
  ```js
  const arr = [1, 2];

  console.log(arr[2]);    // undefined
  ```
- 배열은 인덱스를 나타내는 문자열을 프로퍼티 키로 갖는 객체 
  - 존재하지 않는 프로퍼티 키로 객체의 프로퍼티에 접근했을 때 undefined를 반환하는 것처럼 배열도 존재하지 않는 요소를 참조하면 undefined 반환
  - 같은 이유로 희소 배열에 존재하지 않는 요소를 참조해도 undefined 반환

<br>
<br>

## 6. 배열 요소의 추가와 갱신
- 객체에 프로퍼티를 동적으로 추가할 수 있는 것처럼 배열에도 요소를 동적으로 추가 가능
- 존재하지 않는 인덱스를 사용해 값을 할당하면 새로운 요소 추가
  - length 프로퍼티 값은 자동 갱신
  ```js
  const arr = [0];

  // 배열 요소 동적 추가
  arr[1] = 1;

  console.log(arr);    // [0, 1]
  console.log(arr.length);    // 2
  ```
- 만약 현재 배열의 length 프로퍼티 값보다 큰 인덱스로 새로운 요소를 추가하면 희소 배열이 됨
  - 인덱스로 요소에 접근하여 명시적으로 값을 할당하지 않는 요소는 생성되지 않음
  ```js
  arr[100] = 100;

  console.log(arr);    // [0, 1, empty * 98, 100]
  console.log(arr.length);    // 100
  ```
- 이미 요소가 존재하는 요소에 값을 재할당하면 요소 값 갱신
  ```js
  // 요소 값 갱신
  arr[1] = 10;

  console.log(arr);    // [0, 10, empty * 98, 100]
  ```
- 만약 정수 이외의 값을 인덱스처럼 사용하면 요소가 생성되는 것이 아니라 프로퍼티가 생성
  - 추가된 프로퍼티는 length 프로퍼티 값에 영향 X
  ```js
  const arr = [];

  // 배열 요소 추가
  arr[0] = 1;
  arr['1'] = 2;

  // 프로퍼티 추가
  arr['foo'] = 3;
  arr.bar = 4;
  arr[1.1] = 5;
  arr[-1] = 6;

  console.log(arr);    // [1, 2, foo: 3, bar: 4. '1.1': 5, '-1': 6]
  console.log(arr.length);    // 2
  ```

<br>
<br>

## 7. 배열 요소의 삭제
- 배열은 객체이기 때문에 배열의 특정 요소를 삭제하기 위해 delete연산자 사용 
  ```js
  const arr = [1, 2, 3];

  delete arr[1];
  console.log(arr);    // [1, empty, 3]

  console.log(arr.length);    // 3
  ```
- delete 연산자는 객체의 프로퍼티를 삭제
  - arr에서 프로퍼티 키가 '1'인 프로퍼티 삭제 -> 희소 배열이 되며 length 프로퍼티 값은 변하지 않음
  - 따라서 희소 배열을 만드는 delete 연산자는 사용 권장 X
- 희소 배열을 만들지 않으면서 배열의 특정 요소를 완전히 삭제하려면 Array.prototype.splice 메서드 사용
  ```js
  const arr = [1, 2, 3];

  // Array.prototype.splice(삭제를 시작할 인덱스, 삭제할 요소 수)
  arr.splice(1, 1);
  console.log(arr);    // [1, 3]

  // length 프로퍼티 자동 갱신
  console.log(arr.length);    // 2
  ```