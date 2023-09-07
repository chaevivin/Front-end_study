# 타입스크립트 기초: 변수와 함수의 타입 정의

## 1. 변수에 타입을 정의하는 방법
- 변수에 타입을 선언하고 싶다면 변수 뒤에 `: 타입 이름`을 추가한다.
  ```js
  // 
  const name = 'chaebeen';
  ```
  ```ts
  // ts
  const name: string = 'chaebeen';
  ```
  - name 변수의 타입은 문자열이고 값으로 'captain'이라는 문자열을 갖는다.
- 변수 이름 뒤에 콜론(:)을 붙여서 해당 변수의 타입을 정의할 수 있다.
  - 콜론(:)을 **타입 표기(type annotation)** 라고 한다.
  - 타입 표기는 변수뿐만 아니라 함수에도 사용할 수 있다.

<br>
<br>

## 2. 기본 타입
- 변수나 함수의 타입을 정의할 때 사용할 수 있는 타입들
  - string
  - number
  - boolean
  - object
  - Array
  - tuple
  - any
  - null
  - undefined

<br>

### 2.1. 문자열 타입 string
- string은 문자열을 의미하는 타입이다.
  ```ts
  const name: string = 'chaebeen';
  ```
  - name 변수의 타입이 string으로 지정되어 있기 때문에 이 변수는 문자열만 취급할 수 있다.
  - 문자열이 아닌 다른 값을 할당하면 타입 에러가 발생한다.

<br>

### 2.2. 숫자 타입 number
- number는 숫자를 의미하는 타입이다.
  ```ts
  const age: number = 20;
  ```
  - age 변수의 타입이 number로 지정되어 있기 때문에 이 변수는 숫자만 취급할 수 있다.
  - 초깃값으로 20을 넣었다면 이후에 값을 변경할 때도 숫자만 할당할 수 있다.

<br>

### 2.3. 진위 타입 boolean
- boolean은 진위 값(true, false)을 의미하는 타입이다.
  ```ts
  const isLogin: boolean = false;
  ```
  - isLogin은 사용자의 로그인 여부를 파악하는 데 사용하는 변수이다.
  - 로그인이 되었으면 true, 로그인이 되지 않았으면 false를 할당한다.
  - 참과 거짓을 구분하는 값을 다루는 경우 boolean으로 타입을 선언한다.

<br>

### 2.4. 객체 타입 object
- object는 객체 유형의 데이터를 의미하는 타입이다.
  ```ts
  const person: object = { name: 'chaebeen', age: 20 };
  ```
  - person 변수는 name과 age 속성을 갖는 객체이다.
  - 타입스크립트의 장점을 극대화하려면 가급적 타입을 최대한 구체적으로 선언해야 한다. 위 예시보다는 객체의 타입을 더 구체적으로 명시해야 한다.

<br>

### 2.5. 배열 타입 Array
- Array는 배열을 의미하는 타입이다.
- 배열 타입은 배열의 데이터 타입에 따라 선언하는 방식이 다르다.
  ```
  Array<배열의 데이터 타입>
  배열의 데이터 타입[]
  ```
  ```ts
  // 문자열 배열
  let companies: Array<string> = ['네이버', '삼성', '구글'];
  let companies: string[] = ['네이버', '삼성', '구글'];

  // 숫자 배열
  let cards: Array<number> = [1, 2, 3, 4];
  let cards: number[] = [1, 2, 3, 4];
  ```
  - 배열의 데이터 타입: 배열을 구성할 요소의 타입
  - `Array<string>`보다 `string[]` 형태의 문법을 사용하는 것을 권장한다. -> 키보드 입력도 더 적고 직관적

<br>

### 2.6. 튜플 타입 tuple
- 튜플은 특정 형태를 갖는 배열을 의미한다.
- 배열의 길이가 고정되고 각 요소의 타입이 정의된 배열을 튜플이라고 한다.
  ```ts
  const items: [string, number] = ['hi', 1];
  ```
  - items 변수는 배열의 길이가 2고 첫 번째 요소는 문자열, 두 번째 요소는 숫자인 타입으로 정의되었다.
  - 정해진 순서와 타입에 맞지 않게 값이 취급된다면 에러가 발생한다.

<br>

### 2.7. any
- any 타입은 아무 데이터나 취급하겠다는 의미이다.
- 타입스크립트에서 자바스크립트의 유연함을 취하려고 할 때 사용하는 타입이다.
  ```ts
  let myName: any = 'chaebeen';
  myName = 100;
  const age: any = 20;
  ```
  - myName 변수는 any 타입으로 지정되었기 때문에 초기에는 문자열을 갖고 있지만 이후에 다른 데이터 타입의 값으로 변경할 수 있다.
- any는 이미 작성된 자바스크립트 코드를 타입스크립트로 변환할 때 유용하게 사용할 수 있다.
- any 타입은 타입스크립트를 처음 시작하는 사람에게 유용한 타입이다.
- 타입스크립트 사용 경험이 많아지면 any보다 더 적절한 데이터 타입을 정의할 수 있고, 타입스크립트의 장점을 더 누릴 수 있다.

<br>

### 2.8. null과 undefined
- 자바스크립트에서 null은 의도적인 빈 값으로 개발자가 의도적으로 값을 비어두고 싶을 때 사용한다.
- undefined는 변수를 선언할 때 값을 할당하지 않으면 기본적으로 할당되는 초기값이다.
- 타입스크립트에서는 이 두 값을 타입으로 정의할 수 있다.
  ```ts
  const empty: null = null;
  const nothingAssigned: undefined;
  ```
  - empty 변수에는 null 값을 할당했기 때문에 null 타입을 지정한다.
  - nothingAssigned 변수는 선언만 하고 값을 할당하지 않았기 때문에 undefined가 초깃값으로 할당될 것이기 때문에 undefined 타입을 지정한다.
- null과 undefined 타입은 타입스크립트 설정 파일의 strict 옵션에 따라서 사용 여부가 결정된다. strict 옵션이 꺼져 있을 때는 신경 쓰지 않아도 되는 타입이다.

<br>
<br>

## 3. 함수에 타입을 정의하는 방법
- 함수는 반복되는 코드를 줄이고 데이터를 전달 및 가공하는 데 사용한다.

<br>

### 3.1. 함수란?
- 자바스크립트에서 함수는 function **예약어** 와 함수 이름으로 함수를 선언할 수 있고, 함수 본문에 return을 추가해서 값을 반환하거나 함수 실행을 종료할 수 있다.
  ```js
  function sayHi() {
    return 'hi';
  }
  ```
- 함수는 입력 값에 따라 출력 값이 달라진다.
  ```js
  function sayWord(word) {
    return word;
  }

  sayWord('hello');    // hello
  sayWord('bye');    // bye
  ```
  - word는 함수의 **파라미터(매개변수)** 이고, 함수를 호출할 때 넘겨준 입력 값을 받는 역할을 한다.
  - 함수를 호출할 때 넘긴 문자열 'hello'와 'bye'를 **인자** 라고 한다.

<br>

### 3.2. 함수의 타입 정의: 파라미터와 반환값
- 함수의 타입을 정의할 때는 먼저 입력값과 출력 값에 대한 타입을 정의한다.
  - 반환값의 타입을 문자열로 지정
    ```ts
    function sayWord(word): string {
      return word;
    }
    ```
    - 함수의 반환값 타입은 함수 이름 오른쪽에 `: 타입이름`으로 지정한다.
  - 파라미터 타입을 문자열로 지정
    ```ts
    function sayWord(word: string): string {
      return word;
    }
    ```
    - 함수의 파라미터 타입은 파라미터 오른쪽에 `: 타입이름`으로 지정한다.

<br>
<br>

## 4. 타입스크립트 함수의 인자 특징
- 자바스크립트 함수에서는 파라미터와 인자의 개수가 일치하지 않아도 프로그래밍상 문제가 없었다.
  ```js
  function sayWord(word) {
    return word;
  }

  sayWord('hi', 'cat');    // hi
  ```
  - 파리미터의 숫자보다 인자의 개수가 많더라도 에러가 발생하지 않고 정상적으로 동작한다.
- 타입스크립트에서는 파라미터와 인자의 개수가 다르면 에러가 발생한다.
  - 부가적인 정보가 표시되기 때문에 함수를 정의된 스펙에 맞게 올바르게 사용할 수 있다.

<br>
<br>

## 5. 옵셔널 파라미터
- 파라미터의 개수만큼 인자를 넘기고 싶지 않을 때는 어떻게 해야 할까?
  ```ts
  function sayMyName(firstName: string, lastName: string): string {
    return 'my name: ' + firstName + ' ' + lastName;
  }

  sayMyName('Chaebeen', 'Jeong');    // my name: Chaebeen Jeong
  sayMyName('Chaebeen');    // TypeError
  ```
  - 성과 이름을 입력받아 이름을 반환해주는 함수 
  - 함수를 호출할 때 파라미터 개수만큼 인자를 넘겨주지 않으면 타입 에러가 발생한다.
- 함수의 인자를 선택적으로 넘기고 싶을 때는 **옵셔널 파라미터(optional parameter)** 를 사용한다.
  - 옵셔널 파라미터는 `?`로 표기한다.
  - 선택적으로 사용하고 싶은 파라미터의 오른쪽에 `?`를 붙인다.
  ```ts
  function sayMyName(firstName: string, lastName?: string): string {
    return 'my name: ' + firstName + ' ' + lastName;
  }

  sayMyName('Chaebeen');    // my name: Chaebeen
  ```

<br>
<br>