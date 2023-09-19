# 타입 추론
- 비주얼 스튜디오 코드에서 코드를 작성할 때 부가적인 코드 정보를 표시해 주거나 자동완성을 할 수 있는 이유는 타입스크립트 내부적으로 타입을 추론하기 때문이다.

<br>
<br>

## 1. 타입 추론이란?
- **타입 추론**: 타입스크립트가 코드를 해석하여 적절한 타입을 정의하는 동작을 의미한다.
- 변수를 하나 선언하고 값을 할당하면 해당 변수의 타입은 자동으로 추론된다.
  ```ts
  const a = 10;
  ```
  - a 변수의 타입은 number로 정의된다.
  - a 변수에 마우스 커서를 올리면 number라고 타입이 지정되어 있다.
- 변수를 초기화하거나 함수의 파라미터에 기본값을 설정하거나 반환값을 설정했을 때 지정된 값을 기반으로 적당한 타입을 제시하고 정의해 주는 것을 타입 추론이라고 한다.

<br>
<br>

## 2. 변수의 타입 추론 과정
- 변수의 타입 추론 과정
  ```ts
  const a;
  ```
  - 변수에 값을 할당하지 않고 선언만 하면 any 타입으로 추론된다.
  - a 변수가 선언되는 시점에 값이 할당되지 않아 어떤 값이 들어올지 모르기 때문에 any로 지정된다.
  ```ts
  const a = 'hi'
  ```
  - a 변수의 타입은 문자열 타입인 string이다.
- 변수를 선언할 때 할당된 초기값에 따라서 타입이 추론된다.
  - 변수를 먼저 선언한 후 값을 변경하면 해당 데이터에 맞는 타입으로 변경되지 않는다.
  ```ts
  const a;    // any
  a = 10;
  ```
  - 타입스크립트는 `const a;`가 선언되고 나면 이후에 어떤 값으로 변경될지 알 수 없다.
  - 코드를 1줄씩 해석하는 타입스크립트는 `const a;` 이후에 어떤 코드가 올 지 변수가 선언되는 시점에는 알 수 없다.
- 변수 타입은 선언하는 시점에 할당된 값을 기반으로 추론된다.

<br>
<br>

## 3. 함수의 타입 추론: 반환 타입
- 함수의 타입 추론
  ```ts
  function sum(a: number, b: number): number {
    return a + b;
  }

  const result = sum(1, 2);    // number
  ```
  - sum() 함수는 두 숫자를 받아와서 합을 구하고 숫자를 반환하는 함수이다.
  - 함수를 호출하여 반환된 결과 값을 변수에 할당하면 number 타입으로 추론된다.
  - 변수의 타입 추론 과정과 마찬가지로 함수도 주어진 입력 값에 따라 함수의 반환 타입이 추론된다.
  ```ts
  function sum(a: number, b: number) {
    return a + b;
  }

  const result = sum(1, 2);    // number
  ```
  - sum() 함수에서 반한 타입을 제거하였다.
  - result 변수는 그대로 number 타입으로 추론된다.
    - sum() 함수의 반환 타입이 number 타입으로 추론되기 때문에 result는 number 타입으로 추론된다.
    - sum() 함수의 파라미터가 모두 number 타입으로 지정되어 있고, 숫자를 더한 결과는 숫자이기 때문에 number로 정의된다.
  ```ts
  function sum(a: number, b: number) {
    return a == b;
  }

  // function sum(a: number, b: number): boolean
  ```
  - sum() 함수에서 파라미터를 더하지 않고 비교 연산자 ==를 사용하여 값을 비교 하였다.
  - 반환값은 true이거나 false이기 때문에 반환값은 boolean으로 추론된다.
- 함수의 파라미터 타입과 내부 로직으로 반환 타입이 자동으로 추론된다.

<br>
<br>

## 4. 함수의 타입 추론: 파라미터 타입
- 함수의 파라미터 타입 추론
  ```ts
  function getA(a) {
    return a;
  }

  // function getA(a: any): any
  ```
  - getA() 함수는 a 파라미터를 받아 그대로 반환해준다.
  - 함수의 파라미터 타입을 지정하지 않았으므로 기본 타입은 any가 된다.
  - any로 지정되어 있는 파라미터를 그대로 반환해 주었기 때문에 파라미터 타입과 반환 타입이 모두 any로 지정되어 있다.
  ```ts
  function getA(a: number) {
    return a;
  }
  ```
  - getA() 함수에 마우스 커서를 올리면 파라미터 타입의 반환 타입이 number로 추론된다.
  ```ts
  function getA(a = 10) {
    return a;
  }

  // function getA(a?: number): number

  const result = getA();
  console.log(result);    // 10
  ```
  - `a = 10`은 getA() 함수를 호출했을 때 인자가 비어 있으면 a에 10을 할당한다.
  - getA() 함수에 마우스 커서를 올리면 파라미터와 반환값 모두 number 타입으로 지정된다.
  - 함수의 파라미터에 값을 넘기거나 넘기지 않아도 되기 때문에 옵셔널 파라미터를 의미하는 `?`가 붙는다.
  ```ts
  function getA(a: number) {
    let c = 'hi';
    return a + c;
  }

  // function getA(a: number): string
  ```
  - c 변수는 문자열 값을 할당했기 때문에 문자열 타입으로 추론된다.
  - getA() 함수의 반환값은 숫자 타입인 a 파라미터 + 문자열 타입인 c 변수를 더한 결과인 문자열 타입이다.

<br>
<br>

## 5. 인터페이스와 제네릭의 추론 방식
- 인터페이스와 제네릭의 추론 방식
  ```ts
  interface DropDown<T> {
    title: string;
    value: T;
  }
  ```
  - 제네릭으로 받은 타입은 인터페이스 속성 value에 연결된다.
  ```ts
  let shoppingItem: DropDown<number> = {

  }
  ```
  - shoppingItem이라는 변수를 선언하고 DropDown 인터페이스 타입을 정의하였다.
  - 그리고 인터페이스의 제네릭 타입으로는 number를 정의하였다,
  - 객체의 괄호 안에서 ctrl + space를 누르면 자동 완성할 수 있는 속성들을 확인할 수 있다.
  - 인터페이스의 title 속성은 문자열 타입으로 선언했기 때문에 string 타입으로 추론된다.
  - value 속성은 제네릭 타입으로 number로 정의하였기 때문에 number 타입으로 추론된다.
    ```ts
    interface DropDown {
      title: string;
      value: number;
    }
    ```
- 인터페이스에 제네릭을 사용할 때도 타입스크립트 내부적으로 적절한 타입을 추론해 준다.

<br>
<br>

## 6. 복잡한 구조에서 타입 추론 방식
- 인터페이스에 제네릭을 2개 연결
  ```ts
  interface Dropdown<T> {
    title: string;
    value: T;
  }

  interface DetailedDropdown<K> extends Dropdown<K> {
    tag: string;
    description: string;
  }
  ```
  - DetailedDropdown 인터페이스는 Dropdown 인터페이스를 상속 받는다.
  - Dropdown 인터페이스에 제네릭을 선언했고 Dropdown 인터페이스에도 제네릭을 선언하였다.
  ```ts
  let shoppingDetailItem: DetailedDropdown<number> = {

  }
  ```
  - shoppingDetailItem 변수를 객체로 선언하고 DetailedDropdown 인터페이스 타입을 지정하였다. 그리고 DetailedDropdown 인터페이스의 제네릭에는 number 타입을 넘겼다.
  - 객체 괄호 안에서 ctrl + space를 누르면 객체에서 사용할 수 있는 속성이 미리보기로 보인다.
  - Dropdown 인터페이스를 상속받은 DetailedDropdown 인터페이스를 타입으로 정의했기 때문에 description, tag, title, value 속성을 사용할 수 있다.
    ```ts
    interface DetailedDropdown {
      tag: string;
      description: string;
      title: string;
      value: number;
    }
    ```
    - value 속성이 number 타입으로 추론되는 이유: DetailedDropdown 인터페이스에 넘긴 제네릭 타입이 Dropdown 인터페이스의 제네릭 타입으로 전달되었기 때문이다.
    - DetailedDropdown에서는 제네릭으로 받은 타입을 사용하지 않고 Dropdown 인터페이스로 넘겨주는 역할을 한다. 

<br>
<br>