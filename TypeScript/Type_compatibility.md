# 타입 호환
- 타입 호환은 타입스크립트로 코딩할 때 에러를 더 정확히 분석할 수 있는 개념이다.

<br>
<br>

## 1. 타입 호환이란?
- **타입 호한(type compatibility)**: 서로 다른 타입이 2개 있을 때 특정 타입이 다른 타입에 포함되는지를 의미한다.
  ```ts
  let a: string = 'hi';
  let b: number = 10;

  b = a;    // Error
  ```
  - 숫자 타입인 변수 b에 문자열 타입인 변수 a를 할당하면 에러가 발생한다.
    - 에러 발생 이유: 문자열 타입은 숫자 타입에 할당할 수 없기 때문이다.
  - 자바스크립트에서는 별도의 에러 표시가 되지 않는다.
    - 자바스크립트는 미리 변수의 타입을 지정하지 않아도 실행하는 시점에 적절한 타입으로 변환해 주기 때문이다.
    - 자바스크립트에서 b 변수는 처음에 숫자 타입이었다가 `b = a`가 실행되면 문자열 'hi'가 할당되면서 문자 타입으로 변환된다. -> 타입 캐스팅(type casting)
  - 타입스크립트에서는 b의 타입과 a의 타입은 서로 호환되지 않는다.
  ```ts
  let a: string = 'hi';
  let b: 'hi' = 'hi';

  a = b;
  ```
  - 문자열 타입인 변수 a에 문자열 타입인 변수 b를 할당하면 에러가 발생하지 않는다.
  - 별도의 타입 에러가 발생하지 않으므로 a와 b의 타입은 서로 호환된다.
    - 타입 에러가 발생하지 않는 이유: string 타입이 'hi' 타입보다 더 큰 타입이고, string 타입이 'hi'를 포함할 수 있는 관계이기 때문이다.
  - 반대로 `b = a`로 코드를 변경하면 에러가 발생한다.
    - 타입 에러가 발생하는 이유: 'hi' 타입은 문자열 중에서 'hi'만 받을 수 있기 때문에 string 타입보다 받을 수 있는 문자열이 적기 때문이다.
- 타입 간 할당 가능 여부로 '타입이 호환된다.' 또는 '타입이 호환되지 않는다'고 표현할 수 있다.

<br>
<br>

## 2. 다른 언어와 차이점
- 타입스크립트의 타입 호환이라는 개념은 다른 언어와 차이가 있다.
  ```ts
  interface Ironman {
    name: string;
  }

  class Avengers {
    name: string;
  }

  let i: Ironman;
  i = new Avengers();
  ```
  - 문자열 타입의 name 속성을 갖는 인터페이스와 클래스를 각각 선언하고, i 변수에 Ironman 타입으로 지정한 후 Avengers 클래스를 하나 생성하였다.
  - Avengers 클래스가 명시적으로 Ironman 인터페이스를 상속받아 구현하지 않아 타입 에러가 발생해야 하지만 발생하지 않는다. -> 타입스크립트의 '구조적 타이핑' 때문

<br>

### 2.1. 구조적 타이핑
- **구조적 타이핑(structural typing)**: 타입 유형보다는 타입 구조로 호환 여부를 판별하는 언어적 특성을 의미한다.
  ```ts
  type Titan = {
    name: string;
  }

  interface Haikyuu {
    name: string;
  }

  let a: Titan = {
    name: '진격의 거인',
  };
  let b: Haikyuu = {
    name: '하이큐',
  };

  b = a;
  ```
  - 문자열 타입의 name 속성을 갖는 타입 별칭과 인터페이스를 선언하고, Haikyuu 인터페이스 타입을 가진 변수 b에 Titan 타입 별칭을 가진 변수 a를 할당하면 에러가 발생하지 않는다.
    - 에러가 발생하지 않는 이유: 타입스크립트는 타입 별칭이 인터페이스와 호환되는지가 아니라 **해당 타입이 어떤 타입 구조를 갖고 있는지로 타입 호환 여부를 판별한다.**
  - Titan과 Haikyuu는 모두 문자열 타입의 name 속성을 갖고 있기 때문에 타입 구조가 같다.
    - 타입 호환 여부를 판별할 때 똑같은 속성 이름과 똑같은 타입을 가지는지를 모두 확인한다.
- 타입스크립트는 타입의 정의된 생김새와 구조로 타입 호환 여부를 판별한다.

<br>
<br>

## 3. 객체 타입의 호환
- 객체 타입은 타입 유형에 관계없이 동일한 이름의 속성을 갖고 있고 해당 속성의 타입이 같으면 호환 가능하다.
  ```ts
  type Person = {
    name: string;
  };

  interface Developer {
    name: string;
  }

  let jeong: Person = {
    name: '정'
  };

  let been: Developer = {
    name: '빈'
  };

  been = jeong;
  jeong = been;
  ```
  - Person 타입 별칭과 Developer 인터페이스가 모두 동일한 이름의 속성을 갖고 있고 해당 속성 타입이 갖기 때문에 호환 가능하다.
  - 두 타입 간 동일한 타입을 가진 속성이 1개라도 있다면 호환 가능하다.
  ```ts
  type Person = {
    name: string;
  };

  interface Developer {
    name: string;
    skill: string;
  }

  let jeong: Person = {
    name: '정'
  };

  let been: Developer = {
    name: '빈',
    skill: 'ts'
  };

  jeong = been;
  been = jeong;    // Error
  ```
  - 앞의 코드에서 Developer 인터페이스에 skill이라는 속성을 추가로 정의하였다.
  - jeong 변수에 been 변수를 할당하여도 타입 에러가 발생하지 않는다.
    - 타입 에러가 발생하지 않는 이유: Developer 타입이 Person 타입에 호환되기 때문이다.
    - Developer 타입에 skill 속성이 하나 더 선언되어 있지만 Person 타입 입장에서는 호환되는 데 필요한 조건인 문자열 타입의 name 속성이 정의되어 있기 때문에 호환되는 것으로 간주한다.
  - been 변수에 jeong 변수를 할당하면 타입 에러가 발생한다.
    - 타입 에러가 발생하는 이유: been 변수는 Developer 타입으로 선언되었기 때문에 최소한 name과 skill 속성이 모두 선언되어야 한다. 하지만 jenong 변수는 Person 타입으로 선언되어 있어 name 속성만 가지고 있으므로 Developer 타입이 되는 최소 조건을 만족하지 못하기 때문이다.
    - 이 타입 에러를 해결하려면 Person 타입에 skill 속성을 추가하거나 Devleoper 타입의 skill 속성을 옵셔널(?)로 변경한다.
- 객체 타입은 인터페이스, 타입 별칭 등 타입 유형이 아니라 최소한의 타입 조건을 만족했는지에 따라 호환 여부가 판별된다.

<br>
<br>

## 4. 함수 타입의 호환
- 함수 타입의 호환을 알아보자.
  ```ts
  let add = function(a: number, b: number) {
    return a + b;
  };

  let sum = function(x: number, y: number) {
    return x + y;
  }

  add = sum;
  sum = add;
  ```
  - 파라미터 2개를 더하는 add()와 sum() 함수를 함수 표현식으로 정의하고, number 타입을 가진 2개의 파라미터를 정의하였다.
  - 두 함수를 각각의 변수에 할당하여도 에러가 발생하지 않는다. = 두 함수의 타입은 서로 호환된다.
  - 함수 타입도 구조적 타이핑 관점에서 함수 구조가 유사하면 호환된다.
  ```ts
  let getNumber = function(num: number) {
    return num;
  };

  let sum = function(x: number, y: number) {
    return x + y;
  };

  getNumber = sum;    // Error
  ```
  - 숫자 하나를 받아 반환해주는 getNumber() 함수에 sum() 함수를 할당하면 타입 에러가 발생한다. = 호환되지 않는다.
    - 에러가 발생하는 이유: 함수의 역할이 달라졌기 때문이다.
    ```ts
    console.log(getNumber(10));    // 10
    getNumber = sum;
    console.log(getNumber(10));    // NaN
    ```
    - 첫 번째 `getNumber(10)` 코드의 호출 결과는 10이다.
    - 그 다음 `getNumber = sum`으로 할당하면 sum() 함수의 로직이 getNumber() 함수에 대입된다.
      ```ts
      let getNumber = function(x, y) {
        return x + y;
      }
      ```
      - 자바스크립트 관점에서 보면 이렇게 작성된 것과 동일한 효과가 나타난다.
    - sum() 함수를 getNumber() 함수에 할당하고 난 이후에도 동일하게 인자를 1개 넘겨서 실행하면 `10 + undefined`의 결과를 반환하는 것과 같아진다.
    - 결론적으로 getNumber() 함수의 로직을 보장하고자 타입 레벨에서 에러를 표시해 주어야 한다.
  ```ts
  let getNumber = function(num: number) {
    return num;
  };

  let sum = function(x: number, y: number) {
    return x + y;
  };

  sum = getNumber;    
  ```
  - sum() 함수에 getNumber() 함수를 할당하면 타입 에러가 발생하지 않는다.
    - 에러가 발생하지 않는 이유: getNumber()의 파라미터의 개수가 sum()의 파라미터 개수보다 적기 때문이다.
    ```ts
    console.log(sum(10, 20));    // 30
    sum = getNumber;
    console.log(sum(10, 20));    // 10
    ```
    - 첫 번째 `sum(10, 20)`의 결과는 파라미터 2개를 더한 값이 30이다.
    - getNumber() 함수를 sum() 함수에 할당한다.
      ```ts
      let sum = function(num) {
        return num;
      };
      ```
      - 자바스크립트 관점에서 보면 이렇게 작성된 것과 동일한 효과가 나타난다.
    - 두 번째 `sum(10, 20)`을 호출하면 두 번째 인자는 사용하지 않고 첫 번째 인자를 그대로 반환하여 10을 출력한다.
- 함수의 타입 호환은 '기존 함수 코드의 동작을 보장해줄 수 있는가?'라는 관점으로 이해하는 것이 좋다.
- 특정 함수 타입의 부분 집합에 해당하는 함수는 호환되지만, 더 크거나 타입을 만족하지 못하는 함수는 호환되지 않는다.

<br>
<br>

## 5. 이넘 타입의 호환
- 이넘 타입은 값 여러 개를 하나로 묶어서 사용해야 할 때 활용되는 타입이다.

<br>

### 5.1. 숫자형 이넘과 호환되는 number 타입
- 숫자형 이넘은 숫자와 호환된다.
  ```ts
  enum Language {
    C,    // 0
    Java,    // 1
    TypeScript    // 2
  }

  let a: number = 10;
  a = Language.C;
  ```
  - 숫자 타입의 a 변수를 선언하고 초깃값으로 10을 할당한 후 Language 이넘의 첫 번째 속성 C를 할당하면 에러가 발생하지 않는다.

<br>

### 5.2. 이넘 타입 간 호환 여부
- 이넘에도 구조적 타이핑 개념이 적용되지 않는다.
  ```ts
  enum Language {
    C,   
    Java,   
    TypeScript    
  }

  enum Programming {
    C,
    Java,
    TypeScript
  }

  let langC: Language.C;
  langC = Programming.C;    // Error
  ```
  - 동일한 타입 구조를 갖는 이넘 Language와 Prgramming을 선언한다. 두 이넘은 속성 개수, 속성 순서가 동일하고 이넘 이름만 다르다.
  - Language의 속성 C를 타입으로 갖는 langC 변수에 Programming의 속성 C를 할당하면 타입 에러가 발생한다.
- 이넘 타입은 같은 속성과 값을 가졌더라도 이넘 타입 간에는 서로 호환되지 않는다.

<br>
<br>

## 6. 제네릭 타입의 호환
- 제네릭의 타입 호환은 제네릭으로 받은 타입이 해당 타입 구조에 사용되었는지에 따라 결정된다.
  ```ts
  interface Empty<T> {

  }

  let empty1: Empty<string>;
  let empty2: Empty<number>;

  empty2 = empty1;
  empty1 = empty2;
  ```
  - Empty라는 빈 인터페이스를 선언하고 제네릭으로 타입을 넘겨받을 수 있게 정의하였다.
    - 제네릭으로 받은 타입이 Empty 인터페이스의 타입 구조에 전혀 영향을 미치지 않는다.
  - empty1과 empty2는 Empty 인터페이스의 string과 number를 제네릭 타입으로 넘겨 받는다.
  - empty1과 empty2를 각각에 할당하면 에러가 발생하지 않는다. = 서로 호환이 된다.
    - 에러가 발생하지 않는 이유: **제네릭으로 받은 타입이 해당 타입 구조에서 사용되지 않는다면 타입 호환에 영향을 미치지 않는다.**
  ```ts
  interace NoEmpty<T> {
    data: T;
  }

  let noEmpty1 = NoEmpty<string>;
  let noEmpty2 = NoEmpty<number>;

  noEmpty1 = noEmpty2;    // Error
  noEmpty2 = noEmpty1;    // Error
  ```
  - NoEmpty 인터페이스를 제네릭으로 받은 타입을 data라는 속성의 타입으로 사용하도록 정의하였다.
  - 인터페이스에 string과 number 타입을 각각 넘겨서 noEmpty1과 noEmpty2 변수를 선언하고 각각을 서로에게 할당하면 에러가 발생한다. = 서로 호환되지 않는다.
    - 에러가 발생한 이유: 제네릭 타입으로 넘긴 타입이 다르기 때문이다.
    ```ts
    // noEmpty1
    interface NoEmpty {
      data: string;
    }

    // noEmpty2
    interface NoEmpty {
      data: number;
    }
    ```
    - 두 변수에 string과 number를 제네릭 타입으로 넘기면 인터페이스 구조가 달라진다.
    - noEmpty1 변수의 타입과 noEmpty2 변수의 타입 구조가 다르기 때문에 서로 타입이 호환되지 않는다.
- 제네릭의 타입 호환 여부를 살펴볼 때는 제네릭으로 받은 타입이 해당 타입 구조 내에서 사용되었는지 확인하면 된다.

<br>
<br>