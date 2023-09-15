# 제네릭
- 타입스크립트에서 중복되는 코드를 효과적으로 줄이고 고급 문법을 작성할 수 있게 도와준다.

<br>
<br>

## 1. 제네릭이란?
- 제네릭은 타입을 미리 정의하지 않고 사용하는 시점에 원하는 타입을 정의해서 쓸 수 있는 문법이다.
  - 함수의 파라미터와 같은 역할을 한다.
    - 파라미터 역할: 함수의 인자에 넘긴 값을 함수의 파라미터로 받아 함수 내부에서 그대로 사용하는 방식
  ```ts
  function getText(text) {
    return text;
  }

  getText('hi');    // hi
  getText(10);    // 10
  ```
  - getText() 함수는 인자로 넘겨받은 텍스트를 타입에 관계 없이 그대로 반환해 준다. 
  - getText() 함수의 파라미터인 text는 함수를 호출할 때 어떤 값이든 인자로 받을 수 있다. 그리고 받은 값을 그대로 반환한다.
  - 타입스크립트에서 제네릭은 타입을 넘기고 그 타입을 그대로 반환받는다.

<br>
<br>

## 2. 제네릭 기본 문법
- 제네릭 기본 문법
  ```ts
  function getText<T>(text: T): T {
    return text;
  }
  ```
  - 함수 이름 오른쪽에 `<T>`라고 적는다.
  - 파리미터를 닫는 괄호 오른쪽에 콜론을 붙이고 T를 적는다.
  - 파라미터 타입을 T로 정한다.
  - getText() 함수를 실행할 때 아무 타입이나 넘길 수 있다.
    ```ts
    getText<string>('hi');    // hi
    ```
    - getText() 함수를 호출할 때 제네릭에 문자열 데이터 타입인 string 타입을 할당한다.
    - 이렇게 하면 다음과 같이 정의된 것 같은 효과가 생긴다.
      ```ts
      function getText(text: string): string {
        return text;
      }
      ```
- 제네릭을 사용하면 boolean, number, array, object 등 어느 타입이든 함수를 호출 할 때 타입을 지정해서 사용할 수 있다.
- 제네릭으로 넘긴 타입과 맞지 않는 데이터를 인자로 넘기면 타입 에러가 발생한다.

<br>
<br>

## 3. 왜 제니릭을 사용할까?

<br>

### 3.1. 중복되는 타입 코드의 문제점
- 타입을 미리 정의하지 않고 호출할 때 타입을 정의해서 사용하는 이유
  - 반복되는 타입 코드를 줄여 주기 때문이다.
    ```ts
    function getText(text: string): string {
      return text;
    }

    function getNumber(num: number): number {
      return num;
    }
    ```
    - 두 함수는 모두 텍스트를 넘겨 받아 그대로 반환해주는 코드인데 단지 타입이 다르다는 이유로 두 개의 코드를 작성해야 한다.
    - 결국 같은 동작을 하는 코드를 중복해서 선언한 꼴이다.
    - 만약 추가적인 타입을 처리하려면 또 다른 함수들을 선언해야 한다.
    ```ts
    function getBoolean(bool: boolean) {
      return bool;
    }

    function getArray(arr: []) {
      return arr;
    }

    function getObject(obj: {}) {
      return obj;
    }
    ```

<br>

### 3.2. any를 쓰면 되지 않을까?
- 기능은 같지만 타입 때문에 계속 반복 생성되는 함수를 보면 any를 쓰면 되지 않겠냐고 생각할 수 있다.
  ```ts
  function getText(text: any): any {
    return text;
  }

  getText('hi');
  getText(10);
  ```
  - any를 사용하면 어떤 타입의 텍스트든 모두 받을 수 있다.
  - 동일한 동작을 하는 함수가 타입 때문에 반복해서 생성되는 문제는 해결되었지만, any를 사용하기 때문에 타입스크립트의 장점들이 사라진다.
    - any는 자바스크립트 코드처럼 모든 타입을 다 취급할 수 있다.
    - 하지만 타입스크립트의 코드 자동 완성이나 에러의 사전 방지 혜택을 받을 수 없다.

<br>

### 3.3. 제네릭으로 해결되는 문제점
- 동일한 동작의 코드를 타입 때문에 중복 선언하는 문제점과 any 타입으로 선언하면서 생기는 문제점을 제네릭으로 모두 해결할 수 있다.
  ```ts
  function getText<T>(text: T): T {
    return text;
  }

  getText<string>('hi');
  getText<number>(100);
  ```
- 제네릭으로 받은 타입이 파라미터와 반환값에 모두 연결되어 있기 때문에 함수의 호출 결과 타입도 제네릭 타입을 따라간다.
  ```ts
  const myString = getText<string>('hi');
  const myNumber = getText<number>(100);

  console.log(typeof myString);    // string
  console.log(typeof myNumber);    // number
  ```
- 제네릭을 사용하면 타입 때문에 불필요하게 중복되던 코드를 줄일 수 있고 타입이 정확하게 지정되면서 타입스크립트의 이점을 모두 가져갈 수 있다.

<br>
<br>

## 4. 인터페이스에 제네릭 사용하기
- 제네릭은 인터페이스에서도 사용할 수 있다.
- 상품 목록과 상품의 재고를 보여 주는 드롭다운 UI를 인터페이스로 정의
  ```ts
  interface ProductDropdown {
    value: string;
    selected: boolean;
  }

  interface StockDropdown {
    value: number;
    selected: boolean;
  }
  ```
  - value에 다른 데이터 타입을 갖는 UI가 필요하다면 추가적으로 새로운 인터페이스를 정의해야 한다.
  ```ts
  interface AddressDropdown {
    value: { city: string; zipCode: string };
    selected: boolean;
  }
  ```
  - 모든 데이터 타입을 일일이 정의한다면 타입 코드가 많아져서 관리도 어렵고 번거로운 작업이 된다.
- 제네릭을 사용하면 이 문제를 해결할 수 있다.
  ```ts
  interface Dropdown<T> {
    value: T;
    selected: boolean;
  }
  ```
  - 인터페이스 이름 오른쪽에 `<T>`를 붙여 주고 인터페이스 내부 속성 중 제네릭으로 받은 타입을 사용할 곳에 T를 연결한다.
  - 타입을 유언하게 확장할 수 있고 비슷한 역할을 하는 타입 코드를 대폭 줄일 수 있다.
  ```ts
  // 드롭다운 유형별로 각각의 인터페이스를 연결
  const product: ProductDropdown;
  const stock: StockDropdown;
  const address: AddressDropdown;

  // 드롭다운 유형별로 하나의 제네릭 인터페이스를 연결
  const product: Dropdown<string>;
  const stock: Dropdown<number>;
  const address: Dropdown<{ city: string, zipCode: string }>;
  ```

<br>
<br>

## 5. 제네릭의 타입 제약
- 제네릭의 타입 제약은 제네릭으로 타입을 정의할 때 좀 더 정확한 타입을 정의할 수 있게 도와주는 문법이다.

<br>

### 5.1. extends를 사용한 타입 제약
- 제네릭의 장점: 타입을 미리 정의하지 않고 호출하는 시점에 타입을 정의해서 유연하게 확장할 수 있다.
  - 유연하게 확장: 타입을 별도로 제약하지 않고 아무 타입이나 받아서 쓸 수 있다는 의미이다.
  ```ts
  function embraceEverything<T>(thing: T): T {
    return thing;
  }

  embraceEverything<string>('hi');
  embraceEverything<number>(10);
  embraceEverything<boolean>(false);
  embraceEverything<{ name: string }>({ name: 'capt' });
  ```
  - embraceEverything() 함수는 제네릭을 선언했기 때문에 아무 타입이나 모두 넘길 수 있다.
  - 만약 모든 타입이 아니라 몇 개의 타입만 제네릭으로 받고 싶다면?
    - 문자열 타입만 받을 수 있도록 제네릭 제약
      ```ts
      function embraceEverything<T extends string>(thing: T): T {
        return thing;
      }
      ```
      - 제네릭을 선언하는 부분에 `<T extends 타입>`과 같은 형태로 코드를 작성한다.
      - 제약할 타입은 string이기 때문에 `<T extends string>`으로 정의한다.
      - 함수를 호출할 때 제네릭에 string 타입을 넘길 수 있다.
      ```ts
      embraceEverything<string>('hi');
      embraceEverything<number>(10);    // Error
      ```
      - string이 아닌 다른 타입을 제네릭으로 넘기려고 하면 에러가 발생한다.
- extends 키워드로 제네릭의 타입을 제약할 수 있다.

<br>

### 5.2. 타입 제약의 특징
- 일반적으로 타입을 제약할 때는 여러 개의 타입 중 몇 개만 쓸 수 있게 제약한다.
  - length 속성을 갖는 타입만 취급 -> string, array, object만 받을 수 있다.
    ```ts
    function legthOnly<T extends { length: number }>(value: T) {
      return value.length;
    }
    ```
    - legthOnly() 함수에서 제네릭의 타입을 length 속성을 갖는 타입을 제약한다.
    - 이 함수의 인자로 넘길 수 있는 데이터 타입은 문자열, 배열, length 속성을 갖는 객체이다.
    ```ts
    lengthOnly('hi');
    lengthOnly([1, 2, 3]);
    lengthOnly({ title: 'abc', length: 123 });
    lengthOnly(100);    // Error
    ```
- 제네릭의 타입 제약은 하나의 특정 타입뿐만 아니라 특정 범위에 해당하는 여러 개의 타입을 대상으로 지정할 수 있다.

<br>

### 5.3. keyof를 사용한 타입 제약
- keyof는 특정 타입의 키 값을 추출해서 문자열 유니언 타입으로 변환해 준다.
  ```ts
  type DeveloperKeys = keyof { name: string; skill: string; }
  ```
  - keyof 키워드를 사용하여 객체의 키를 DeveloperKeys라는 타입 별칭에 담아 둘 수 있다.
  - DeveloperKeys 타입은 키가 유니언 타입으로 변환되어 있다.
  - keyof의 대상이 되는 객체에 name과 skill이라는 속성(키 값)이 있기 때문에 이 키를 모두 유니언 타입으로 연결해서 반환해 준다.
- 제네릭 타입 제약의 keyof
  ```ts
  function printKeys<T extends keyof { name: string, skill: string; }>(value: T) {
    console.log(value);
  }

  printKeys('address');    // Error
  printKeys('name');
  ```
  - 제네릭을 정의하는 부분에 extends와 keyof를 조합해서 name과 skill 속성을 갖는 객체의 키만 타입으로 받겠다고 정의했다.
  - 이 함수의 제네릭은 파라미터인 value에 연결되어 있기 때문에 함수를 호출할 때 넘길 수 있는 인자는 문자열 name과 skill이다.
- extens를 이용해서 제네릭의 타입을 제약할 때 keyof를 함께 사용하여 타입의 제약 조건을 까다롭게 만들 수 있다.

<br>
<br>

## 6. 제네릭을 처음 사용할 때 주의해야 할 사고방식
- 제네릭을 처음 사용할 때 가장 헷갈리는 부분은 함수 안에서 제네릭으로 받은 타입을 다룰 때이다.
  ```ts
  function printTextLength<T>(text: T) {
    console.log(text.length);    // Error
  }

  printTextLength<string>('hello');
  ```
  - printTextLength() 함수는 인자로 넘긴 텍스트 길이를 출력한다.
  - 함수는 제네릭으로 선언되었고 별도로 타입을 제약하지 않아서 문자열을 인자로 넘길 수 있다.
  - 하지만 에러가 발생한다.
    - 타입스크립트 컴파일러 관점에서는 printTextLength() 함수에 어떤 타입이 들어올지 모르기 때문에 함부로 타입을 가정하지 않는다.
    - 따라서 함수 안에서 제네릭으로 지정된 text 파라미터를 다룰 때 코드 자동 완성이나 타입이 미리 정의된 효과는 얻을 수 없다.
    - 함수의 인자에 어떤 타입을 넘기든 제네릭으로 정의된 함수 안에서는 어떤 타입이 들어올지 알지 못한다.
  - 제네릭에게 어떤 타입이 들어올지 알리는 방법
    - printTextLength() 함수는 텍스트 길이를 출력하기 때문에 취급할 데이터에 length 속성이 있으면 된다.
    - 따라서 제네릭으로 사용할 타입에 힌트를 줄 수 있다.
    ```ts
    function printTextLength<T extends { length: number }>(text: T) {
      console.log(text.length);
    }
    ```
    - extends 키워드를 사용하여 제네릭으로 받을 수 있는 타입을 제한한다.
    - length 속성이 있는 데이터 타입만 제네릭 타입으로 넘길 수 있다.
    ```ts
    function printTextLenght<T>(text: T[]) {
      console.log(text.length;)
    }
    ```
    - 제네릭으로 받은 타입을 배열 형태로 정의하는 방법도 있다.
    - `T[]`에서는 제네릭으로 받은 타입을 배열의 데이터 타입으로 쓴다.
    - 배열 형태로 정의하면 함수를 호출할 때 문자열이나 배열 형태의 데이터를 넣어야 한다.
      ```ts
      printTextLength<string>(['a', 'b', 'c']);    // 3
      printTextLength<number>([100]);    // 1
      printTextLength([true, false]);    // 2
      ```
- 파라미터에 제네릭 타입이 연결되어 있으면 함수를 호출할 때 명시적으로 제네릭에 타입을 선언하지 않아도 된다.

<br>
<br>