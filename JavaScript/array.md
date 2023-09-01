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

<br>

## 8. 배열 메서드
- 배열 메서드는 결과물을 반환하는 패턴이 두 가지
  - **원본 배열(배열 메서드를 호출하는 배열, 즉 배열 메서드의 구현체 내부에서 this가 가리키는 객체)을 직접 변경하는 메서드**
  - **원본 배열을 직접 변경하지 않고 새로운 배열을 생성하여 반환하는 메서드**
- ES5부터 도입된 배열 메서드는 대부분 원본 배열을 직접 변경하지 않지만 초창기 배열 메서드는 원본 배열을 직접 변경하는 경우가 많음
- 원본 배열을 직접 변경하는 메서드는 외부 상태를 직접 변경하는 부수 효과가 있으므로 사용할 때 주의
- 가급적 원본 배열을 직접 변경하지 않는 메서드를 사용

<br>

### 8.1. Array.isArray
- Array 생성자 함수의 정적 메서드
- 전달된 인수가 배열이면 true, 배열이 아니면 false 반환
  ```js
  // true
  Array.isArray([]);
  Array.isArray([1, 2]);
  Array.isArray(new Array());

  // false
  Array.isArray();
  Array.isArray({});
  Array.isArray(null);
  Array.isArray(undefined);
  Array.isArray(1);
  Array.isArray('Array');
  Array.isArray(true);
  Array.isArray(false);
  Array.isArray({ 0: 1, length: 1 });
  ```

<br>

### 8.2. Array.prototype.indexOf
- 원본 배열에서 인수로 전달된 요소를 검색하여 인덱스 반환
- 원본 배열에 인수로 전달된 요소와 중복되는 요소가 여러 개 있다면 첫 번째로 검색된 인덱스 반환
- 원본 배열에 인수로 전달한 요소가 존재하지 않으면 -1 반환
- 두 번째 인수는 검색을 시작할 인덱스
  - 두 번째 인수를 생략하면 처음부터 검색
  ```js
  const arr = [1, 2, 2, 3];

  arr.indexOf(2);    // 1
  arr.indexOf(4);    // -1
  arr.indexOf(2, 2);    // 2
  ```
- indexOf 메서드는 배열에 특정 요소가 존재하는지 확인할 때 유용
  ```js
  const foods = ['apple', 'banana', 'orange'];

  if (foods.indexOf('orange') === -1) {
    // foods 배열에 orange가 존재하지 않으면 요소 추가
    foods.push('orange');
  }

  console.log(foods);    // ["apple", "banana", "orange"]
  ```
- indexOf 메서드 대신 ES7에 도입된 Array.prototype.includes 메서드를 사용하면 가독성이 더 좋음
  ```js
  const foods = ['apple', 'banana', 'orange'];

  if (!foods.includes('orange')) {
    // foods 배열에 orange가 존재하지 않으면 요소 추가
    foods.push('orange');
  }

  console.log(foods);    // ["apple", "banana", "orange"]
  ```

<br>

### 8.3. Array.prototype.push
- 인수로 전달받은 모든 값을 원본 배열의 마지막 요소로 추가하고 변경된 length 프로퍼티 값을 반환
  ```js
  const arr = [1, 2];

  let result = arr.push(3, 4);
  console.log(result);    // 4

  console.log(arr);    // [1, 2, 3, 4]
  ```
- push 메서드는 성능 면에서 좋지 않음
  - 마지막 요소로 추가할 요소가 하나뿐이라면 length 프로퍼티를 사용하여 배열의 마지막에 요소 직접 추가 -> push보다 빠름
  ```js
  const arr = [1, 2];

  arr[arr.length] = 3;
  console.log(arr);    // [1, 2, 3]
  ```
- push 메서드는 원본 배열을 직접 변경
  - push 메서드보다 ES6의 스프레드 문법 사용 권장
  - 스프레드 문법을 사용하면 함수 호출 없이 표현식으로 마지막에 요소 추가 가능하며 부수 효과 X
  ```js
  const arr = [1, 2];

  const newArr = [...arr, 3];
  console.log(newArr);    // [1, 2, 3]
  ```

<br>

### 8.4. Array.prototype.pop
- 원본 배열에서 마지막 요소를 제거하고 제거한 요소를 반환
- 원본 배열이 빈 배열이면 undefined 반환
- 원본 배열 직접 변경
  ```js
  const arr = [1, 2];

  let result = arr.pop();
  console.log(result);    // 2

  console.log(arr);    // [1]
  ```
- pop 메서드와 push 메서드를 사용하면 스택 구현 가능
  - 스택 : 데이터를 마지막에 밀어 넣고, 마지막에 밀어 넣은 데이터를 먼저 꺼내는 후입 선출(LIFO - Last In First Out)방식의 자료구조
  - 푸시(push) : 스택에 데이터를 밀어 넣는 것
  - 팝(pop) : 스택에서 데이터 꺼내는 것
  - 스택을 생성자 함수로 구현
    ```js
    const Stack = (function () {
      function Stack(array = []) {
        if (!Array.isArray(array)) {
          throw new TypeError(`${array} is not an array`);
        }
        this.array = array;
      }

      Stack.prototype = {
        constructor: Stack,
        push(value) {
          return this.array.push(value);
        },
        pop() {
          return this.array.pop();
        },
        entries() {
          retur [...this.array];    // 스택의 복사본 배열 반환
        }
      };

      return Stack;
    }());

    const stack = new Stack([1, 2]);
    console.log(stack.entries());    // [1, 2]

    stack.push(3);
    console.log(stack.entries());    // [1, 2, 3]

    stack.pop();
    console.log(stack.entries());    // [1, 2]
    ```
  - 스택을 클래스로 구현
    ```js
    class Stack {
      #array;    // private class member

      constructor(array = []) {
        if (!Array.isArray(array)) {
          throw new TypeError(`${array} is not an array`);
        }
        this.array = array;
      }

      push(value) {
        return this.#array.push(value);
      }

      pop() {
        return this.#array.pop();
      }

      entries() {
        return [...this.#array];
      }
    }

    const stack = new Stack([1, 2]);
    console.log(stack.entries());    // [1, 2]

    stack.push(3);
    console.log(stack.entries());    // [1, 2, 3]

    stack.pop();
    console.log(stack.entries());    // [1, 2]
    ```

<br>

### 8.5. Array.prototype.unshift
- 인수로 전달받은 모든 값을 원본 배열의 선두에 요소로 추가하고 변경된 length 프로퍼티 값을 반환
  ```js
  const arr = [1, 2];

  let result = arr.unshift(3, 4);
  console.log(result);    // 4

  console.log(arr);    // [3, 4, 1, 2]
  ```
- unshift 메서드는 원본 배열 직접 변경
  - 스프레드 문법 사용 권장
  ```js
  const arr = [1, 2];

  const newArr = [3, 4, ...arr];
  console.log(newArr);    // [3, 4, 1, 2]
  ```

<br>

### 8.6. Array.prototype.shift
- 원본 배열에서 첫 번재 요소를 제거하고 제거한 요소를 반환
- 원본 배열이 빈 배열이면 undefined 반환
- shift 메서드는 원본 배열 직접 변경
  ```js
  const arr = [1, 2];

  let result = arr.shift();
  console.log(result);    // 1

  console.log(arr);    // [2]
  ```
- shift 메서드와 push 메서드를 사용하면 큐 구현 가능
  - 큐 : 데이터를 마지막에 밀어 넣고, 처음 데이터, 즉 가장 먼저 밀어 넣은 데이터를 먼저 꺼내는 선입 선출(FIFO - First In First Out) 방식의 자료구조
  - 큐를 생성자 함수로 구현
    ```js
    const Queue = (function() {
      function Queue(array = []) {
        if (!Array.isArray(array)) {
          throw new TypeError(`${array} is not an array.`);
        }
        this.aray = array;
      }

      Queue.prototype = {
        constructor: Queue,
        enqueue(value) {
          return this.array.push(value);
        },
        dequeue() {
          return this.array.shift();
        },
        entries() {
          return [...this.array];
        }
      };

      return Queue;
    }());

    const queue = new Queue([1, 2]);
    console.log(queue.entries());    // [1, 2]

    queue.enqueue(3);
    console.log(queue.entries());    // [1, 2, 3]

    queue.dequeue();
    console.log(queue.entries());    // [2, 3]
    ```
  - 큐를 클래스로 구현
    ```js
    class Queue {
      #array;    // private class member

      constructor(array = []) {
        if (!Array.isArray(array)) {
          throw new TypeError(`${array} is not an array.`);
        }
        this.#array = array;
      }

      enqueue(value) {
        return this.#array.push(value);
      }

      dequeue() {
        return this.#array.shift();
      }

      entries() {
        return [...this.#array];
      }
    }

    const queue = new Queue([1, 2]);
    console.log(queue.entries());    // [1, 2]

    queue.enqueue(3);
    console.log(queue.entries());    // [1, 2, 3]

    queue.dequeue();
    console.log(queue.entries());    // [2, 3]
    ```

<br>

### 8.7. Array.prototype.concat
- 인수로 전달된 값들(배열 또는 원시값)을 원본 배열의 마지막 요소로 추가한 새로운 배열을 반환
- 인수로 전달한 값이 배열인 경우, 배열을 해체하여 새로운 배열의 요소로 추가
- 인수 여러 개 전달 가능. 
- 원본 배열 변경 X
  ```js
  const arr1 = [1, 2];
  const arr2 = [3, 4];

  let result = arr1.concat(arr2);
  console.log(result);    // [1, 2, 3, 4]

  result = arr1.concat(3);
  console.log(result);    // [1, 2, 3]

  result = arr1.concat(arr2, 5);
  console.log(result);    // [1, 2, 3, 4, 5]

  console.log(arr1);    // [1, 2]
  ```
- push와 unshift 메서드는 concat 메서드로 대체 가능하지만 차이 존재
  - push와 unshift 메서드는 원본 배열을 직접 변경하지만 concat 메서드는 원본 배열을 변경하지 않고 새로운 배열을 반환
  - push와 unshift 메서드를 사용할 경우 원본 배열을 반드시 변수에 저장해야 하고, concat 메서드를 사용할 경우 반환값을 반드시 변수에 할당 
  ```js
  const arr1 = [3, 4];

  arr1.unshift(1, 2);
  console.log(arr1);    // [1, 2, 3, 4]

  arr1.push(5, 6);
  console.log(arr1);    // [1, 2, 3, 4, 5, 6]

  const arr2 = [3, 4];

  // arr1.unshift(1, 2) 대체
  let result = [1, 2].concat(arr2);
  console.log(result);    // [1, 2, 3, 4]

  // arr1.push(5, 6) 대체
  result = result.concat(5, 6);
  console.log(result);    // [1, 2, 3, 4, 5, 6]
  ```
  - 인수로 전달받은 값이 배열인 경우 push와 unshift 메서드는 배열을 그대로 원본 배열의 마지막/첫 번째 요소로 추가하지만 concat 메서드는 인수로 전달받은 배열을 해체하여 마지막 요소로 추가
  ```js
  const arr = [3, 4];

  arr.unshift([1, 2]);
  arr.push([5, 6]);
  console.log(arr);    // [[1, 2], 3, 4, [5, 6]]

  let result = [1, 2].concat([3, 4]);
  result = result.concat([5, 6]);

  console.log(result);    // [1, 2, 3, 4, 5, 6]
  ```
- concat 메서드는 ES6의 스프레드 문법으로 대체 가능
  - push/unsfhit, concat 메서드를 사용하는 대신 ES6의 스프레드 문법을 이관성 있게 사용하는 것을 권장
  ```js
  let result = [1, 2].concat([3, 4]);
  console.log(result);    // [1, 2, 3, 4]

  result = [...[1, 2], ...[3, 4]];
  console.log(result);    // [1, 2, 3, 4]
  ```

<br>

### 8.8. Array.prototype.splice
- 원본 배열의 중간에 요소를 추가하거나 중간에 있는 요소를 제거하는 경우 사용
- splice 메서드는 3개의 매개변수가 있으며 원본 배열을 직접 변경
  - start
    - 원본 배열의 요소를 제거하기 시작할 인덱스
    - start만 지정하면 원본 배열의 start부터 모든 요소 제거
    - start가 음수인 경우 배열의 끝에서 인덱스를 나타냄 (start가 -1이면 마지막 요소, -n이면 마지막에서 n번째 요소)
  - deleteCount
    - 원본 배열의 요소를 제거하기 시작할 인덱스인 start부터 제거할 요소 개수
    - deleteCount가 0인 경우 아무런 요소도 제거 X
  - items
    - 제거한 위치에 삽입할 요소들의 목록
    - 생략한 경우 원본 배열에서 요소들을 제거
  ```js
  const arr = [1, 2, 3, 4];

  // 인덱스 1부터 2개 삭제 후 그 자리에 20, 30 삽입
  const result = arr.splice(1, 2, 20, 30);
  
  // 제거하는 요소 반환
  console.log(result);    // [2, 3]
  console.log(arr);    // [1, 20, 30, 4]
  ```
- splice 메서드에 3개의 인수를 모두 전다하면 첫 번째 인수부터 두 번째 인수 만큼 원본 배열에서 요소를 제거. 그리고 세 번째 인수 위치에 삽입할 요소들을 원본 배열에 삽입
- splice 메서드이 두 번재 인수, 즉 제거할 요소의 개수를 0으로 지정하면 아무런 요소도 제거하지 않고 새로운 요소 삽입
  ```js
  const arr = [1, 2, 3, 4];

  const result = arr.splice(1, 0, 100);

  console.log(arr);    // [1, 100, 2, 3, 4]
  console.log(reuslt);    // []
  ```
- splice 메서드의 세 번째 인수, 즉 제거한 위치에 추가할 요소들의 목록을 전달하지 않으면 원본 배열에서 지정된 요소를 제거
  ```js
  const arr = [1, 2, 3, 4];

  const result = arr.splice(1, 2);

  console.log(arr);    // [1, 4]
  console.log(result);    // [2, 3]
  ```
- splice 메서드의 두 번째 인수를 생략하면 첫 번째 인수로 전달된 시작 인덱스부터 모든 요소 제거
  ```js
  const arr = [1, 2, 3, 4];

  const result = arr.splice(1);

  console.log(arr);    // [1]
  console.log(result);    // [2, 3, 4]
  ```
- 배열의 특정 요소를 제거하려면 indexOf 메서드를 통해 특정 요소의 인덱스를 취득한 다음 splice 메서드 사용
  ```js
  const arr = [1, 2, 3, 1, 2];

  function remove(array, item) {
    // 인덱스 취득
    const index = aray.indexOf(item);

    // 제거할 item이 있다면 제거
    if (index !== -1) array.splice(index, 1);

    return array;
  }

  console.log(remove(arr, 2));    // [1, 3, 1, 2]
  console.log(remove(arr, 10));    // [1, 3, 1, 2]
  ```
- filter 메서드를 사용하여 특정 요소 제거 가능하지만 중복된 경우도 모두 제거
  ```js
  const arr = [1, 2, 3, 1, 2];

  function removeAll(array, item) {
    return array.filter(v => v !== item);
  }

  console.log(removeAll(arr, 2));    // [1, 3, 1]
  ```

<br>

### 8.9. Array.prototype.slice
- 인수로 전달된 범위의 요소들을 복사하여 배열로 반환
- 원본 배열 변경 X
- slice 메서드는 두 개의 매개변수가 있음
  - start
    - 복사를 시작할 인덱스
    - 음수인 경우 배열의 맨 마지막 인덱스
  - end
    - 복사를 종료할 인덱스
    - 이 인덱스에 해당하는 요소는 복사되지 않음
    - end는 생략 가능하며 생략 시 기본값은 length 프로퍼티 값
  ```js
  const arr = [1, 2, 3];

  arr.slice(0, 1);    // [1]
  arr.slice(1, 2);    // [2]

  console.log(arr);    // [1, 2, 3]
  ```
- slice 메서드는 첫 번째 인수로 전달받은 인덱스부터 두 번째 인수로 전달받은 인덱스 이전(end 미포함)까지 요소들을 복사하여 배열로 반환
- slice 메서드의 두 번째 인수를 생략하면 첫 번째 인수로 전달받은 인덱스부터 모든 요소를 복사하여 배열로 반환
  ```js
  const arr = [1, 2, 3];

  arr.slice(1);    // [2, 3]
  ```
- slice 메서드의 첫 번째 인수가 음수인 경우 배열의 끝에서부터 요소를 복사하여 배열로 반환
  ```js
  const arr = [1, 2, 3];

  arr.slice(-1);    // [3]
  arr.slice(-2);    // [2, 3]
  ```
- slice 메서드의 인수를 모두 생략하면 원본 배열의 복사본을 생성하여 반환
  ```js
  const arr = [1, 2, 3];

  const copy = arr.slice();
  console.log(copy);    // [1, 2, 3]
  console.log(arr === copy);    // false
  ```
- 이때 생성된 복사본은 얕은 복사를 통해 생성
  - 얕은 복사 : 객체를 프로퍼티 값으로 갖는 객체의 경우 한 단계까지만 복사
  - 깊은 복사 : 객체를 프로퍼티 값으로 갖는 객체의 경우 중첩되어 있는 객체까지 모두 복사
- slice 메서드가 복사본을 생성하는 것을 이용하여 arguments, HTMLCollection, NodeList 같은 유사 배열 객체를 배열로 반환 가능 (ES5)
  ```js
  function sum() {
    var arr = Array.prototype.slice.call(arguments);
    console.log(arr);    // [1, 2, 3]

    return arr.reduce(function (pre,cur) {
      return pre + cur;
    }, 0);
  }

  console.log(sum(1, 2, 3));    // 6
  ```
- Array.from 메서드를 사용하면 더욱 간단하게 유사 배열 객체를 배열로 반환 가능. Array.from 메서드는 유사 배열 객체 또는 이터러블 객체를 배열로 변환
  ```js
  function sum() {
    const arr = Array.from(arguments);
    console.log(arr);    // [1, 2, 3]

    return arr.reduce((pre, cur) => pre + cur, 0);
  }

  console.log(sum(1, 2, 3));    // 6
  ```
- arguments 객체는 유사 배열 객체이면서 이터러블 객체. 이터러블 객체는 ES6의 스프레드 문법을 사용하여 간단하게 배열로 변환 가능
  ```js
  function sum() {
    const arr = [...arguments];
    console.log(arr);    // [1, 2, 3]

    return arr.reduce((pre, cur) => pre + cur, 0);
  }

  console.log(sum(1, 2, 3));    // 6
  ```

<br>

### 8.10. Array.prototype.join
- 원본 배열의 모든 요소를 문자열로 변환 후, 인수로 전달받은 문자열, 즉 구분자로 연결한 문자열을 반환
- 구분자는 생략 가능하며 기본 구분자는 콤마(,)
  ```js
  const arr = [1, 2, 3, 4];

  arr.join();    // '1, 2, 3, 4';
  arr.join('');    // '1234'
  arr.join(':');    //'1:2:3:4'
  ```

<br>

### 8.11. Array.prototype.reverse
- 원본 배열의 순서를 반대로 뒤짚음
- 원본 배열 변경
- 반환값은 변경된 배열
  ```js
  const arr = [1, 2, 3];
  const result = arr.reverse();

  console.log(arr);    // [3, 2, 1]
  console.log(result);    // [3, 2, 1]
  ```

<br>

### 8.12. Array.prototype.fill
- ES6에 도입된 fill 메서드는 인수로 전달받은 값을 배열의 처음부터 끝까지 요소로 채움
- 원본 배열 변경
  ```js
  const arr = [1, 2, 3];

  arr.fill(0);

  console.log(arr);    // [0, 0, 0]
  ```
- 두 번째 인수로 요소 채우기 시작할 인덱스 전달   
  ```js
  const arr = [1, 2, 3];

  arr.fill(0, 1); 

  console.log(arr);    // [1, 0, 0]
  ```
- 세 번째 인수로 요소 채우기를 멈출 인덱스 전달 
  ```js
  const arr = [1, 2, 3, 4, 5];

  arr.fill(0, 1, 3);

  console.log(arr);    // [1, 0, 0, 4, 5]
  ```
- fill 메서드를 사용하면 배열을 생성하면서 특정 값으로 요소 채우기 가능
  ```js
  const arr = new Array(3);
  console.log(arr);    // [empty * 3]

  const result = arr.fill(1);    
  
  console.log(arr);    // [1, 1, 1]
  console.log(result);    // [1, 1, 1]
  ```
- fill 메서드로 요소를 채울 경우 모든 요소를 하나의 값으로만 채우기 가능하지만 Array.from 메서드를 사용하면 두 번째 인수로 전달한 콜백 함수를 통해 요소값을 만들면서 배열 채우기 가능
  - Array.from 메서드는 두 번째 인수로 전달한 콜백 함수에 첫 번째 인수에 의해 생성된 배열의 요소값과 인덱스를 순차적으로 전달하면서 호출하고, 콜백 함수의 반환값으로 구성된 배열 반환
  ```js
  const sequences = (length = 0) => Array.from({ length }, (_, i) => i);

  console.log(sequences(3));    // [0, 1, 2]
  ```

<br>

### 8.13. Array.prototype.includes
- ES7에서 도입된 includes 메서드는 배열 내에 특정 요소가 포함되어 있는지 확인하여 true 또는 false 반환
- 첫 번째 인수로 검색할 대상 지정
  ```js
  const arr = [1, 2, 3];

  arr.includes(2);    // true
  arr.includes(100);    // false
  ```
- 두 번째 인수로 검색을 시작할 인덱스 전달 가능
  - 두 번째 인수를 생략할 경우 기본값 0 설정
  - 두 번째 인수에 음수를 전달하면 length 프로퍼티 값과 음수 인덱스를 합산하여 검색 시작 인덱스 설정
  ```js
  const arr = [1, 2, 3];

  arr.includes(1, 1);    // false
  arr.includes(3, -1);    // true
  ```
- 배열에서 인수로 전달된 요소를 검색하여 인덱스를 반환하는 indexOf 메서드를 사용하면 배열 내에 특정한 요소가 포함되어 있는지 확인 가능
  - 하지만 indexOf 메서드를 사용하면 반환값이 -1인지 확인해 보아야 하고 배열에 NaN이 포함되어 있는지 확인할 수 없음
  ```js
  [NaN].indexOf(NaN) !== -1;    // false
  [NaN].includes(NaN);    // true
  ```

<br>

### 8.14. Array.prototype.flat
- ES10에서 도입된 flat 메서드는 인수로 전달한 깊이만큼 재귀적으로 배열을 평탄화
  ```js
  [1, [2, 3, 4, 5]].flat();    // [1, 2, 3, 4, 5]
  ```
- 중첩 배열을 평탄화할 깊이를 인수로 전달 가능
  - 인수를 생략할 경우 기본값은 1
  - 인수로 Infinity를 전달하면 중첩 배열을 모두 평탄화
  ```js
  [1, [2, [3, [4]]]].flat();    // [1, 2, [3, [4]]]
  [1, [2, [3, [4]]]].flat(1);    // [1, 2, [3, [4]]]

  [1, [2, [3, [4]]]].flat(2);    // [1, 2, 3, [4]]

  [1, [2, [3, [4]]]].flat().flat();    // [1, 2, 3, [4]]

  [1, [2, [3, [4]]]].flat(Infinity);    // [1, 2, 3, 4]
  ```

<br>
<br>

## 9. 배열 고차 함수
- 고차 함수(Higher-Order Function, HOF) : 함수를 인수로 전달받거나 함수를 반환하는 함수
  - 자바스크립트의 함수는 일급 객체이므로 함수를 값처럼 인수로 전달하거나 반환 가능
  - 외부 상태의 변경이나 가변 데이터를 피하고 **불변성을 지향**하는 함수형 프로그래밍에 기반
- 함수형 프로그래밍 = 순수 함수 + 보조 함수 -> 로직 내에 존재하는 **조건문과 반복문을 제거**하여 복잡성 해결, **변수의 사용을 억제**하여 상태 변경을 피하려는 프고래밍 패러다임
  - 함수형 프로그래밍은 **순수 함수를 통해 부수 효과를 최대한 억제**하여 오류를 피하고 프로그램의 안정성을 높이려는 노력의 일환
- 자바스크립트는 고차 함수를 다수 지원
  - 특히 배열은 매우 유용한 고차 함수 제공

<br>

### 9.1. Array.prototype.sort
- sort 메서드는 배열의 요소를 정렬
- 원본 배열을 직접 변경
- 정렬된 배열 반환
- 기본적으로 오름차순으로 요소 정렬 (한글 문자열인 요소도 포함)
  ```js
  const fruits = ['Banana', 'Orange', 'Apple'];

  fruits.sort();

  console.log(fruits);    // ['Apple', 'Banana', 'Orange']
  ```
- 내림차순으로 정렬하는 방법
  1. sort 메서드를 사용하여 오름차순으로 정렬
  2. reverse 메서드를 사용하여 요소의 순서 뒤집음
  ```js
  const fruits = ['Banana', 'Orange', 'Apple'];

  // 오름차순 정렬
  fruits.sort();
  console.log(fruits);    // ['Apple', 'Banana', 'Orange']

  // 내림차순 정렬
  fruits.reverse();
  console.log(fruits);    // ['Orange', 'Banana', 'Apple']
  ```
- sort 메서드의 기본 정렬 순서는 유니코드 코드 포인트 순서를 따름
  - 그래서 문자열은 의도한 대로 정렬할 수 있지만 숫자는 불가능
  ```js
  const points = [40, 100, 1, 5, 2, 25, 10];

  points.sort();

  console.log(points);    // [1, 10, 100, 2, 25, 40, 5]
  ```
  - ⭐ 숫자를 정렬할 때는 sort 메서드에 **정렬 순서를 정의하는 비교 함수를 인수로 전달**
    - 비교 함수는 양수나 음수 또는 0을 반환
    - 비교 함수의 반환값이 음수일 때 : 비교 함수의 첫 번째 인수를 우선하여 정렬
    - 비교 함수의 반환값이 0일 때 : 정렬 X
    - 비교 함수의 반환값이 양수일 때 : 두 번째 인수를 우선하여 정렬
  ```js
  const points = [40, 100, 1, 5, 2, 25, 10];

  // 오름차순 정렬 -> 비교 함수의 반환값이 음수이면 a를 우선하여 정렬
  points.sort((a, b) => a - b);
  console.log(points);    // [1, 2, 5, 10, 25, 40, 100]

  // 최소값, 최대값
  console.log(points[0], points[points.length - 1]);    // 1 100

  // 내림차순 정렬 -> 비교 함수의 반환값이 음수이면 b를 우선하여 정렬
  points.sort((a, b) => b - a);
  console.log(points);    // [100, 40, 25, 10, 5, 2, 1]

  // 최소값, 최대값
  console.log(points[points.length - 1], points[0]);    // 1 100
  ```
- 객체를 요소로 갖는 배열 정렬
  ```js
  const todos = [
    { id: 4, content: 'JavaScript' },
    { id: 1, content: 'HTML' },
    { id: 2, content: 'CSS' }
  ];

  // 비교 함수, 매개변수 key는 프로퍼티 키
  function compare(key) {
    // 프로퍼티 값이 문자열인 경우 - 산술 연산으로 비교하면 NaN이 나오므로 비교 연산 사용
    // 비교 함수는 양수/음수/0을 반환하면 되므로 - 산술 연산 대신 비교 연산 사용
    return (a, b) => (a[key] > b[key] ? 1 : (a[key] < b[key] ? -1 : 0));
  }

  // id를 기준으로 오름차순 정렬
  todos.sort(compare('id')); 
  console.log(todos);
  /*
    [
      { id: 1, content: 'HTML' }
      { id: 2, content: 'CSS' }
      { id: 4, content: 'JavaScript' }
    ]
  */

  // content를 기준으로 오름차순 정렬
  todos.sort(compare('content'));
  console.log(todos);
  /*
    [
      { id: 2, content: 'CSS' }
      { id: 1, content: 'HTML' }
      { id: 4, content: 'JavaScript' }
    ]
  */
  ```

<br>

### 9.2. Array.prototype.forEach
- forEach 메서드는 for문을 대체할 수 있는 고차 함수
- 자신의 내부에서 반복물을 실행 -> 반복문을 추상화한 고차 함수로서 내부에서 반복문을 통해 자신을 호출한 배열을 순회하면서 수행해야 할 처리를 콜백 함수로 전달받아 반복 호출
  ```js
  const numbers = [1, 2, 3];
  const pows = [];

  // for문으로 배열 순회
  for (let i = 0; i < numbers.length; i++) {
    pows.push(numbers[i] ** 2);
  }

  console.log(pows);    // [1, 4, 9]
  ```
  ```js
  const numbers = [1, 2, 3];
  const pows = [];

  // forEach문으로 배열 순회
  numbers.forEach(item => pows.push(item ** 2));

  console.log(pows);    // [1, 4, 9]
  ```
  - numbers 배열의 모든 요소를 순회하며 콜백 함수를 반복 호출
- forEach 메서드의 콜백 함수는 forEach 메서드를 호출한 배열의 요소값, 인덱스, forEach를 호출한 배열 자체, 즉 this를 순차적으로 전달받음
  ```js
  [1, 2, 3].forEach((item, index, arr) => {
    console.log(`요소값: ${item}, 인덱스: ${index}, this: ${JSON.stringify(arr)}`);
  });
  /*
    요소값: 1, 인덱스: 0, this: [1, 2, 3]
    요소값: 2, 인덱스: 1, this: [1, 2, 3]
    요소값: 3, 인덱스: 2, this: [1, 2, 3]
  */
  ```
- forEach 메서드는 원본 배열 변경 X
  - 콜백 함수를 통해 원본 배열 변경 가능
  ```js
  const numbers = [1, 2, 3];

  numbers.forEach((item, index, arr) => { arr[index] = item ** 2 });
  console.log(numbers);    // [1, 4, 9]
  ```
- forEach 메서드의 반환값은 언제나 undefined
  ```js
  const result = [1, 2, 3].forEach(console.log);
  console.log(result);    // undefined
  ```
- forEach 메서드의 두 번째 인수로 forEach 메서드의 콜백 함수 내부에서 this로 사용할 객체 전달 가능
  - 콜백 함수는 일반 함수로 호출되므로 콜백 함수 내부의 this는 undefined를 가리킴 (클래스 내부에 암묵적으로 strict mode 적용)
  ```js
  class Numbers {
    numberArray = [];

    multiply(arr) {
      arr.forEach(function (item) {
        // TypeError: Cannot read property 'numberArray' of undefined
        this.numberArray.push(item * item);
      });
    }
  }

  const numbers = new Numbers();
  numbers.multipy([1, 2, 3]);
  ```
  - forEach의 콜백 함수 내부의 this와 multiply 메서드 내부의 this를 일치시키려면 forEach의 두 번째 인수로 forEach 콜백 함수 내에서 this로 사용할 객체 전달
  ```js
  class Numbers {
    numberArray = [];

    multiply(arr) {
      arr.forEach(function (item) {
        this.numberArray.push(item * item);
      }, this);    // this로 사용할 객체 전달
    }
  }

  const numbers = new Numbers();
  numbers.multipy([1, 2, 3]);
  console.log(numbers.numberArray);    // [1, 4, 9]
  ```
  - 더 나은 방법은 ES6의 화살표 함수 사용하는 방법
    - 화살표 함수는 함수 자체의 this 바인딩을 갖지 않기 때문에 this를 참조하면 상위 스코프, 즉 multiply의 this를 참조
  ```js
  class Numbers {
    numberArray = [];

    multiply(arr) {
      arr.forEach(item => this.numberArray.push(item * item));
    }
  }

  const numbers = new Numbers();
  numbers.multipy([1, 2, 3]);
  console.log(numbers.numberArray);    // [1, 4, 9]
  ```
- forEach 메서드도 내부에서는 반복문(for문)을 통해 배열을 순회할 수 밖에 없지만, 반복문을 내부로 은닉하여 로직의 흐름을 이해하기 쉽게 하고 복잡성 해결
- forEach 메서드는 for문과 달리 break, continue 문 사용 불가 -> 배열의 모든 요소를 빠짐없이 순회하며 중간에 순회 중단 불가
  ```js
  [1, 2, 3].forEach(item => {
    console.log(item);
    if (item > 1) break;    // SyntaxError: Illegal break statement
  });

  [1, 2, 3].forEach(item => {
    console.log(item);
    if (item > 1) continue;    // SyntaxError: Illegal continue statement: no surrounding iteration statement
  });
  ```
- 희소 배열의 경우 존재하지 않는 요소는 순회 대상에서 제외
  - for문은 존재하지 않는 요소도 순회
  ```js
  const arr = [1, , 3];

  for (let i = 0; i < arr.length; i++) {
    console.log(arr[i]);    // 1, undefined, 3
  }

  arr.forEach(v => console.log(v));    // 1, 3
  ```
- forEach 메서드는 for문에 비해 성능이 좋지 않지만 가독성이 더 좋기 때문에 복잡한 코드나 높은 성능이 필요한 경우가 아니라면 forEach 메서드 사용 권장

<br>

### 9.3. Array.prototype.map
- 자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달받은 콜백 함수를 반복 호출
- **콜백 함수의 반환값들로 구성된 새로운 배열을 반환**
- 원본 배열 변경 X
  ```js
  const number = [1, 4, 9];

  const roots = numbers.map(Math.sqrt);

  console.log(roots);    // [1, 2, 3]
  console.log(number);    // [1, 4, 9]
  ```
- forEach 메서드와 map 메서드의 공통점
  - 자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달받은 콜백 함수를 반복 호출
- forEach 메서드와 map 메서드의 차이점
  - forEach 메서드는 undefined 반환 -> **단순히 반복문을 대체하기 위한 고차 함수**
  - map 메서드는 콜백 함수의 반환값들로 구성된 새로운 배열 반환 -> **요소값을 다른 값으로 매핑한 새로운 배열을 생성하기 위한 고차 함수**
- **map 메서드가 생성하여 반환하는 새로운 배열의 length 프로퍼티 값 = map 메서드를 호출한 배열의 length 프로퍼티 값**
  - **map 메서드를 호출한 배열과 map 메서드가 생성하여 반환한 배열은 1:1 매핑**
- map 메서드의 콜백 함수는 map 메서드를 호출한 배열의 요소값, 인덱스, map 메서드를 호출한 배열 자체, 즉 this를 순차적으로 전달 받음
  ```js
  [1, 2, 3].map((item, index, arr) => {
    console.log(`요소값: ${item}, 인덱스: ${index}, this: ${JSON.stringify(arr)}`);
    return item;
  });
  /*
    요소값: 1, 인덱스: 0, this: [1, 2, 3]
    요소값: 2, 인덱스: 1, this: [1, 2, 3]
    요소값: 3, 인덱스: 2, this: [1, 2, 3]
  */
  ```
- map 메서드의 두 번째 인수로 map 메서드의 콜백 함수 내부에서 this로 사용할 객체 전달 가능
  ```js
  class Prefixer {
    constructor(prefix) {
      this.prefix = prefix;
    }

    add(arr) {
      return arr.map(function (item) {
        return this.prefix + item;
      }, this);
    }
  }

  const prefixer = new Prefixer('-webkit-');
  console.log(prefixer.add(['transition', 'user-select']));    // ['-webkit-transition', '-webkit-user-select']
  ```
- 더 나은 방법은 ES6의 화살표 함수 사용
  ```JS
  class Prefixer {
    constructor(prefix) {
      this.prefix = prefix;
    }

    add(arr) {
      return arr.map(item => this.prefix + item);
    }
  }

  const prefixer = new Prefixer('-webkit-');
  console.log(prefixer.add(['transition', 'user-select']));    // ['-webkit-transition', '-webkit-user-select']
  ```

<br>

### 9.4. Array.prototype.filter
- 자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달받은 콜백 함수를 반복 호출
- **콜백 함수의 반환값이 true인 요소로만 구성된 새로운 배열 반환**
- 원본 배열 변경 X
  ```js
  const numbers = [1, 2, 3, 4, 5];

  const odds = numbers.filter(item => item % 2);
  console.log(odds);    // [1, 3, 5]
  ```
- filter 메서드는 자신을 호출한 배열에서 필터링 조건을 만족하는 특정 요소만 추출하여 새로운 배열을 만들고 싶을 때 사용
- **filter 메서드가 생성하여 반환한 새로운 배열의 length 프로퍼티 값은 filter 메서드를 호출한 배열의 length 프로퍼티 값과 같거나 작음**
- filter  메서드의 콜백 함수는 filter 메서드를 호출한 배열의 요소값, 인덱스, filter 메서드를 호출한 배열 자체, 즉 this를 순차적으로 전달 받음
  ```js
  [1, 2, 3].filter((item, index, arr) => {
    console.log(`요소값: ${item}, 인덱스: ${index}, this: ${JSON.stringify(arr)}`);
    return item % 2;
  });
  /*
    요소값: 1, 인덱스: 0, this: [1, 2, 3]
    요소값: 2, 인덱스: 1, this: [1, 2, 3]
    요소값: 3, 인덱스: 2, this: [1, 2, 3]
  */
  ```
- filter 메서드의 두 번째 인수로 filter 메서드의 콜백 함수 내부에서 this로 사용할 객체 전달 가능 -> 더 나은 방법은 화살표 함수 사용하는 것
- filter 메서드는 자신을 호출한 배열에서 특정 요소를 제거하기 위해 사용
  ```js
  class Users {
    constructor() {
      this.users = [
        { id: 1, name: 'Lee' }
        { id: 2, name: 'Kim' }
      ];
    }

    // 요소 추출
    findById(id) {
      // id가 일치하는 사용자만 반환
      return this.users.filter(user => user.id === id);
    }

    // 요소 제거
    remove(id) {
      // id가 일치하지 않는 사용자 제거
      this.users = this.users.filter(user => user.id !== id);
    }
  }

  const users = new Users();

  let user = users.findById(1);
  console.log(user);    // [{ id: 1, name: 'Lee' }]

  users.remove(1);

  user = users.findById(1);
  console.log(user);    // []
  ```
- filter 메서드를 사용해 특정 요소를 제거할 경우 특정 요소가 중복되어 있다면 중복된 요소가 모두 제거
  - 특정 요소를 하나만 제거하려면 indexOf 메서드를 통해 특정 요소의 인덱스를 취한 다음 splice 메서드 사용

<br>

### 9.5. Array.prototype.reduce
- 자신을 호출한 배열을 모든 요소를 순회하며 인수로 전달받은 콜백 함수를 반복 호출하고 콜백 함수의 반환값을 다음 순회 시에 콜백 함수의 첫 번째 인수로 전달하면서 콜백 함수로 호출하여 **하나의 결과값을 만들어 반환**
- 원본 배열 변경 X
- reduce 메서드는 첫 번째 인수로 콜백 함수, 두 번째 인수로 초기값 전달 받음
- reduce 메서드의 콜백 함수에는 4개의 인수, 초기값 또는 콜백 함수의 이전 반환값, reduce 메서드를 호출한 배열의 요소값과 인덱스, reduce 메서드를 호출한 배열 자체, 즉 this가 전달
  ```js
  // 1부터 4 누적 값
  const sum = [1, 2, 3, 4].reduce((accumulator, currentValue, index, array) => accumulator + currentValue, 0);

  console.log(sum);    // 10
  ```
- reduce 메서드의 콜백 함수는 4개의 인수를 전달받아 배열의 length 만큼 총 4회 호출
  - 콜백 함수로 전달되는 인수와 콜백 함수의 반환값

    | 구분 | accumulator | currentValue | index | array | 콜백 함수의 반환값 |
    | :---: | :---: | :---: | :---: | :---: | :---: |
    | 첫 번째 순회 | 0 (초기값) | 1 | 0 | [1, 2, 3, 4] | 1 (accumulator + value) |
    | 두 번째 순회 | 1 | 2 | 1 | [1, 2, 3, 4] | 3 (accumulator + value) |
    | 세 번째 순회 | 3 | 3 | 2 | [1, 2, 3, 4] | 6 (accumulator + value) |
    | 네 번째 순회 | 6 | 4 | 3 | [1, 2, 3, 4] | 10 (accumulator + value) |
  - reduce 메서드는 초기값과 배열의 첫 번재 요소값을 콜백 함수에게 인수로 전달하면서 호출하고 다음 순회에는 콜백 함수의 반환값과 두 번째 요소값을 콜백 함수의 인수로 전달하면서 호출 -> 이 과정을 반복하여 **reduce 메서드는 하나의 결과값을 반환**
- reduce 메서드는 자신을 호출한 배열의 모든 요소를 순회하며 하나의 결과값을 구해야 하는 경우에 사용
- reduce의 다양한 활용법
  - 평균 구하기
    ```js
    const values = [1, 2, 3, 4, 5, 6];

    const average = values.reduce((acc, cur, i, { length }) => {
      // 마지막 순회가 아니면 누적값을 반환하고 마지막 순회면 누적값으로 평균을 구해 반환
       return i === length - 1 ? (acc + cur) / length : acc + cur;
    }, 0);

    console.log(average);    // 3.5
    ```
  - 최대값 구하기
    ```js
    const values = [1, 2, 3, 4, 5];

    const max = values.reduce((acc, cur) => (acc > cur ? acc : cur), 0);
    console.log(max);    // 5
    ```
    - 최대값을 구할 때는 Math.max 메서드 사용
      ```js
      const values = [1, 2, 3, 4, 5];

      const max = Math.max(...values);
      console.log(max);    // 5
      ```
  - 요소의 중복 횟수 구하기
    ```js
    const fruits = ['banana', 'apple', 'orange', 'orange', 'apple'];

    const count = fruits.reduce((acc, cur) => {
      acc[cur] = (acc[cur] || 0) + 1;
      return acc;
    }, {});

    console.log(count);    // { banan: 1, apple: 2, orange: 2}
    ```
    - 첫 번째 순회 시 acc는 {}이고 cur은 첫 번째 요소인 'banana'
    - 초기값으로 전달받은 빈 객체에 요소값이 cur을 프로퍼티 키로, 요소의 개수를 프로퍼티 값으로 할당
    - 만약 프로퍼티 값이 undefined(처음 등장하는 요소)이면 프로퍼티 값을 1로 초기화
    - 콜백 함수는 총 5번 호출 : {banan: 1} -> {banan:1, apple: 1}, {banan: 1, apple:1, orange: 1}, {banan: 1, apple:1, orange: 2} -> {banan: 1, apple: 2, orange: 2}
  - 중첩 배열 평탄화
    ```js
    const values = [1, [2, 3], 4, [5, 6]];

    const flatten = values.reduce((acc, cur) => acc.concat(cur), []);
    // [1] -> [1, 2, 3] -> [1, 2, 3, 4] -> [1, 2, 3, 4, 5, 6]

    console.log(flatten);    // [1, 2, 3, 4, 5, 6]
    ```
    - 중첩 배열을 평탄화할 때는 reduce 메서드보다 ES10에서 도입된 Array.prototype.flat 메서드 사용
  - 중복 요소 제거
    ```js
    const values = [1, 2, 1, 3, 5, 4, 5, 3, 4, 4];

    const result = values.reduce(
      (unique, val, i, _values) => _values.indexOf(val) === i ? [...unique, val] : unique, []
    );

    console.log(result);    // [1, 2, 3, 5, 4]
    ```
    - 현재 순회 중인 요소의 인덱스 i가 val의 인덱스와 같다면 val은 처음 순회하는 요소
    - 현재 순회 중인 요소의 인덱스 i가 val의 인덱스와 다르다면 val은 중복된 요소
    - 처음 순회하는 요소만 초기값 []가 전달된 unique 배열에 담아 반환하면 중복된 요소 제거
    - 중복 요소를 제거할 때는 filter나 Set 사용 -> Set 사용 권장 
      ```js
      const values = [1, 2, 1, 3, 5, 4, 5, 3, 4, 4];

      const result = [...new Set(values)];
      console.log(result);    // [1, 2, 3, 5, 4]
      ```
- reduce 메서드의 두 번째 인수로 전달하는 초기값은 옵션 -> 생략 가능
  ```js
  const sum = [1, 2, 3, 4].reduce((acc, cur) => acc + cur);
  console.log(sum);    // 10
  ```
- **reduce 메서드를 호출할 때는 언제나 초기값을 전달하는 것이 안전**
  - 빈 배열로 reduce 메서드를 호출하면 에러가 발생하지만 초기값을 전달하면 에러 발생 X
  ```js
  const sum = [].reduce((acc, cur) => acc + cur);
  // TypeError: Reduce of empty array with no initial value
  ```
  ```js
  const sum = [].reduce((acc, cur) => acc + cur, 0);
  console.log(sum);    // 0
  ```

<br>

### 9.6. Array.prototype.some
- 자신을 호출한 배열의 요소를 순회하면서 인수로 전달된 콜백 함수 호출
- some 메서드는 콜백 함수의 반환값이 단 한번이라도 참이면 true, 모두 거짓이면 false 반환
  - 배열의 요소 중에 콜백 함수를 통해 정의한 조건을 만족하는 요소가 1개 이상 존재하는지 확인하여 그 결과를 불리언 타입으로 반환
- some 메서드를 호출한 배열이 빈 배열인 경우 false 반환
- some 메서드의 콜백 함수는 some 메서드를 호출한 요소값과 인덱스, some 메서드를 호출한 배열 자체, 즉 this를 순차적으로 전달받음
  ```js
  // 배열의 요소 중 10보다 큰 요소가 1개 이상 존재하는지 확인
  [5, 10, 15].some(item => item > 10);    // true

  // 배열의 요소 중 0보다 작은 요소가 1개 이상 존재하는지 확인
  [5, 10, 15].some(item => item < 0);    // false

  // 배열의 요소 중 'banana'가 1개 이상 존재하는지 확인
  ['apple', 'banana', 'mango'].some(item => item === 'banana');    // true

  // some 메서드를 호출한 배열이 빈 배열인 경우 false 반환
  [].some(item => item > 3);    // false
  ```
- some 메서드의 두 번째 인수로 some 메서드의 콜백 함수 내부에서 this로 사용할 객체 전달 가능 -> 더 나은 방법은 화살표 함수 사용하는 것

<br>

### 9.7. Array.prototype.every
- 자신을 호출한 배열의 요소를 순회하면서 인수로 전달된 콜백 함수를 호출
- every 메서드는 콜백 함수의 반환값이 모두 참이면 true, 단 한번이라도 거짓이면 false 반환
  - 배열의 모든 요소가 콜백 함수를 통해 정의한 조건을 모두 만족하는지 확인하여 그 결과를 불리언 타입으로 반환
- every 메서드를 호출한 배열이 빈 배열일 경우 true 반환
- every 메서드의 콜백 함수는 every 메서드를 호출한 요소값과 인덱스, every 메서드를 호출한 배열 자체, 즉 this를 순차적으로 전달받음
  ```js
  // 배열의 모든 요소가 3보다 큰지 확인
  [5, 10, 15].every(item => item > 3);    // true

  // 배열의 모든 요소가 10보다 큰지 확인
  [5, 10, 15].every(item => item > 10);    // false

  // every 메서드를 호출한 배열이 빈 배열일 경우 true 반환
  [].every(item => item > 3);    // true
  ```
- every 메서드의 두 번째 인수로 every 메서드의 콜백 함수 내부에서 this로 사용할 객체 전달 가능 -> 더 나은 방법은 화살표 함수를 사용하는 것

<br>

### 9.8. Array.prototype.find
- ES6에 도입된 find 메서드는 자신을 호출한 배열의 요소를 순회하면서 전달된 콜백 함수 호출하여 반환값이 true인 첫 번째 요소 반환
- 콜백 함수의 반환값이 true인 요소가 존재하지 않는다면 undefined 반환
- find 메서드의 콜백 함수는 find 메서드를 호출한 요소값, 인덱스, find 메서드를 호출한 배열 자체, 즉 this를 순차적으로 전달 받음
  ```js
  const users = [
    { id: 1, name: 'Lee' },
    { id: 1, name: 'Kim' },
    { id: 1, name: 'Jeong' },
    { id: 1, name: 'Park' }
  ];

  // id가 2인 첫 번째 요소 반환. find 메서드는 배열이 아니라 요소 반환
  users.find(user => user.id === 2);    // {id: 2, name: 'Kim'}
  ```
- find 메서드는 콜백 함수의 반환값이 true인 첫 번째 요소를 반환하므로 find의 결과값은 배열이 아닌 해당 요소값
  ```js
  // filter 메서드는 배열 반환
  [1, 2, 2, 3].filter(item => item === 2);    // [2, 2]

  // find 메서드는 요소 반환
  [1, 2, 2, 3].find(item => item === 2);    // 2
  ```
- find 메서드는 두 번째 인수로 find 메서드의 콜백 함수 내에서 this로 사용할 객체 전달 가능 -> 더 나은 방법은 화살표 함수 사용하는 것

<br>

### 9.9. Array.prototype.findIndex
- ES6에 도입된 findIndex 메서드는 자신을 호출한 배열의 요소를 순회하면서 인수로 전달된 콜백함수를 호출하여 반환값이 true인 첫 번째 요소의 인덱스 반환
- 콜백 함수의 반환값이 true인 요소가 존재하지 않는다면 -1 반환
- findIndex 메서드의 콜백 함수는 findIndex 메서드를 호출한 요소값, 인덱스, findIndex 메서드를 호출한 배열 자체, 즉 this를 순차적으로 전달받음
  ```js
  const users = [
    { id: 1, name: 'Lee' },
    { id: 1, name: 'Kim' },
    { id: 1, name: 'Jeong' },
    { id: 1, name: 'Park' }
  ];

  users.findIndex(user => user.id === 2);    // 1
  user.findIndex(user => user.name === 'Park');    // 3

  // 위와 같이 프로퍼티 키와 값으로 요소의 인덱스를 구하는 경우 다음과 같이 콜백 함수 추상화 가능
  function predicate(key, value) {
    // key와 value를 기억하는 클로저 반환
    return item => item[key] === value;
  }

  users.findIndex(predicate('id', 2));    // 1
  users.findIndex(predicate('name', 'Park'));    // 3
  ```
- findIndex 메서드의 두 번째 인수로 findIndex 메서드의 콜백 함수 내부에서 this로 사용할 객체 전달 가능 -> 더 나은 방법은 화살표 함수 사용하는 것

<br>

### 9.10. Array.prototype.flatMap
- ES10에 도입된 flatMap 메서드는 map 메서드를 통해 생성된 새로운 배열을 평탄화
- map 메서드와 flat 메서드를 순차적으로 실행하는 효과
  ```js
  const arr = ['hello', 'world'];

  // map -> flat 순차적으로 실행
  arr.map(x => x.split('')).flat();    // ['h', 'e', 'l', 'l', 'o', 'w', 'o', 'r', 'l', 'd']

  arr.flatMap(x => x.split(''));    // ['h', 'e', 'l', 'l', 'o', 'w', 'o', 'r', 'l', 'd']
  ```
- flatMap 메서드는 flat 메서드처럼 인수를 전달하여 평탄화 깊이 전달 불가능 -> 1단계만 평탄화
- map 메서드를 통해 생성된 중첩 배열의 평탄화 깊이를 지정해야 하면  flatMap 메서드보다는 map 메서드 + flat 메서드 각각 호출

<br>
<br>