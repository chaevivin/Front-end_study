# 타입 단언
- 타입 단언은 이미 운영되고 있는 자바스크립트 애플리케이션에 타입스크립트를 점진적으로 적용할 때나 타입스크립트에 아직 익숙하지 않아 타입을 정확히 정의할 줄 모를 때 주로 사용한다.

<br>
<br>

## 1. 타입 단언이란?
- **타입 단언(type assertion)**: 타입스크립트의 타입 추론에 기대지 않고 개발자가 직접 타입을 명시하여 해당 타입으로 강제하는 것을 의미한다.
  ```ts
  const myName = 'chaebeen';
  ```
  - 변수를 선언하는 시점의 초기값으로 타입이 결정되기 때문에 myName 변수는 string 타입이다.
  ```ts
  const myName = 'chaebeen' as string;
  ```
  - myName 변수에 타입 단언 문법을 적용하였다.
  - 위 코드는 myName이라는 변수의 타입을 string 타입으로 간주한다는 의미이다.
  - `as` 키워드를 붙이면 타입스크립트가 컴파일할 때 해당 코드의 타입 검사는 수행하지 않는다.
  - 위 코드는 타입 단언을 붙이지 않아도 올바른 타입으로 추론되고 있기 때문에 타입 단언은 무의미하다.
  ```ts
  interface Person {
    name: string;
    age: number;
  }

  const been = {};
  been.name = '채빈';    // Error
  been.age = 20;        // Error
  ```
  - 사람을 의미하는 인터페이스 하나와 빈 객체를 생성하고, 빈 객체에 이름(name)과 나이(age) 속성을 추가한다.
  - 빈 객체에 속성을 추가하면 타입 에러가 발생한다.
    - 에러 발생 이유는 been 변수를 선언할 때 빈 객체로 초기화했기 때문이다.
    - 타입스크립트 컴파일러는 해당 객체에 어떤 속성이 들어갈 지 모르기 때문에 추가되는 속성들은 모두 있어서는 안 될 속성으로 간주한다.
  - 타입 에러를 해결하려면 객체를 선언하는 시점에 속성을 정의하거나 변수 타입을 Person 인터페이스로 정의한다.
    ```ts
    const been = {
      name: '채빈',
      age: 20
    };
    ```
    ```ts
    const been: Person = {
      name: '채빈',
      age: 20
    };
    ```
  - 만약 이미 운영 중인 서비스의 코드나 누군가가 만들어 놓은 코드라고 한다면 에러를 해결하는 데 변경해야 할 코드가 많아질 것이다. -> 이때 타입 단언 사용
    ```ts
    const been = {} as Person;
    been.name = '채빈';    
    been.age = 20;  
    ```
    - 변수를 선언할 때 빈 객체로 선언했지만 이 객체에 들어갈 속성은 Person 인터페이스의 속성이라고 타입스크립트 컴파일러에 말해 주는 것과 같은 효과이다.
- 타입 단언을 이용하면 타입스크립트 컴파일러가 알기 어려운 타입에 대해 힌트를 제공할 수 있다.
- 타입 단언을 이요하면 선언하는 시점에 속성을 정의하지 않고 추후에 추가할 수 있는 유연함을 가진다.

<br>
<br>

## 2. 타입 단언 문법

<br>

### 2.1. 타입 단언의 대상
- 타입 단언은 숫자, 문자열, 객체 등 원시 값 뿐만 아니라 변수나 함수의 호출 결과에도 사용할 수 있다.
  ```ts
  function getId(id) {
    return id;
  }

  const myId = getId('been') as number;
  ```
  - as 키워드를 사용하여 `getId('been')` 함수의 호출 결과를 number 타입으로 단언하였다.
  - getId 함수는 파라미터 타입을 따로 정의하지 않았기 때문에 모든 값을 받을 수 있는 any 타입으로 추론된다.
  - 하지만 `getId('been')` 함수의 호출 결과를 number 타입으로 단언하였기 때문에 myId 변수 타입은 number로 추론된다.

<br>

### 2.2. 타입 단언 중첩
- 타입 단언은 여러 번 중첩해서 사용할 수 있다.
  ```ts
  const num = (10  as any) as number;
  ```
  - 괄호 안의 코드는 숫자 10을 any 타입으로 단언했기 때문에 처음에는 num 변수 타입은 any가 된다.
  - 괄호 밖의 코드는 괄호 안의 코드를 number 타입으로 단언했기 때문에 최종적으로 number 타입이 된다.
  - 위 코드는 타입 단언이 필요한 코드가 아니다. 변수를 선언하고 숫자 10만 할당해도 정확하게 number 타입으로 추론된다.
- 가급적 타입 단언보다는 타입 추론에 의지하는 것이 좋다.

<br>

### 2.3. 타입 단언을 사용할 때 주의할 점

**(1) as 키워드는 구문 오른쪽에서만 사용한다.**
- 타입 단언은 변수 이름에서 사용할 수 없다.
  ```ts
  const num as number = 10;    // Error
  ```
- 타입 단언은 구문 오른쪽에 사용한다.
  ```ts
  const num = 10 as number;
  ```
- 코드는 단언보다 타입 표기로 타입을 정의해주는 것이 좋다.
  ```ts
  const num: number = 10;
  ```

<br>

**(2) 호환되지 않는 데이터 타입으로는 단언할 수 없다.**
- 타입 단언을 이용하면 어떤 값이든 원하는 타입으로 단언할 수 있을 것 같지만 실제로는 그렇지 않고 에러가 발생한다.
  ```ts
  const num = 10 as string;    // Error
  ```
  - number 형식을 string 형식으로 변환할 수 없다.
- 타입 단언을 할 때 해당 값의 실제 데이터 타입을 수용할 수 있는 데이터 타입으로 단언해야 한다.
  ```ts
  const num = 10 as string;    // Error

  const num = 10 as any;
  const num = 10 as number;
  ```
- 타입이라는 것은 해당 값에 대한 부가 정보지 타입을 as로 변경한다고 해서 값 자체는 바뀌지 않는다.

<br>

**(3) 타입 단언 남용하지 않기**
- 코드에 정확한 타입을 선언하면서 타입을 맞추기 보다는 타입 단언이 훨씬 간편하고 쉽게 느껴질 수 있다.
- 하지만 타입 단언은 코드를 실행하는 시점에서 아무런 역할을 하지 않기 때문에 에러 측면에서 취약하다.
  - 타입 에러는 해결되지만 실행 에러는 해결되지 못한다.
  ```ts
  interface Profile {
    name: string;
    id: string;
  }

  function getProfile() {
    // ...
  }

  const myProfile = getProfile() as Profile;
  renderId(myProfile.id);
  ```
  - 서버에서 프로필 하나를 받아 오는 getProfile() 함수를 선언하고, 받아 온 프로필 아이디를 화면에 그리는 rederId() 함수  
    - getProfile() 함수의 로직이 복잡해서 반환 타입을 정의하기 어렵다고 가정한다.
    - getProfile() 함수의 결과는 name과 id를 갖는 객체로 가정한다.
  - as 키워드를 사용해서 getProfile() 함수의 결과를 Profile 인터페이스 타입으로 단언하고 myProfile 변수에 할당하였다.
  - 타입 단언으로 myProfile 변수가 Profile 인터페이스로 간주되면 rederId() 함수의 인자로 넘길때 id 속성에 접근할 수 있다. -> 하지만 에러 발생
    - 에러 발생 이유: myProfile 변수는 객체가 아닌데 id 속성에 접근했기 때문이다.
  - 에러가 발생할 때 타입 에러가 발생하지는 않지만 실행 에러가 발생하게 된다. 결국 타입 단언을 남용하면 실행 시점의 에러에 취약해질 수 있다.

<br>
<br>

## 3. null 아님 보장 연산자: !
- **null 아님 보상 연산자(non null assertion)**: null 타입을 체크할 때 유용하게 쓰는 연산자이다.
- 타입 단언의 한 종류로 as 키워드와는 용도가 다르다.
  - 값이 null이 아님을 보장해준다.
- null 처리가 중요한 이유
  ```js
  // JavaScript
  function shuffleBooks(books) {
    const result = books.shuffle();
    return result;
  }

  shuffleBooks();    // Error
  ```
  - 이 함수는 책 목록을 받아 순서를 랜덤으로 섞는다.
  - 이 함수를 호출하면 에러가 발생한다.
    - 에러 발생 이유: 함수에 인자를 넣어야 하는데 아무것도 넣지 않았기 때문이다. 
    - 타입스크립트에서는 타입 에러로 알려주지만 자바스크립트에서는 별다른 경고가 없다.
  - 에러 방지를 위해 자바스크립트에서는 null 값 체크 코드를 작성하였다.
    ```ts
    function shuffleBooks(books) {
      if (books === null || books === undefined) {
        return;
      }
      const result = books.shuffle();
      return result;
    }
    ```
    - 함수의 books 파라미터가 null이거나 undefined이면 함수의 로직을 실행하지 않고 종료한다.
    - 이렇게 하면 예상치 못한 함수의 입력 값에 대처할 수 있다.
- 타입스크립트
  ```ts
  interface Books {
    shuffle: Function
  }

  function shuffleBooks(books: Books) {
    const result = books.shuffle();
    return result;
  }
  ```
  - Books라는 인터페이스를 선언하고 shuffleBooks() 함수의 파라미터 타입으로 지정하였다.
  - 인터페이스에 shuffle 속성이 있고 호출할 수 있는 형태인 Function 타입으로 정의되어 있기 때문에 타입 관점에서는 문제가 없다.
  - 하지만 인자에 null 값이 오려면 파라미터 타입을 유니언 타입으로 바꿔주어야 하지만 에러가 발생한다.
    ```ts
    function shuffleBooks(books: Books | null) {
      const result = books.shuffle();    // Error
      return result;
    }
    ```
    - 에러 발생 이유: books에 null 값이 들어올 수도 있기 때문에 `books.shuffle()` 코드는 위험하기 때문이다.
  - 에러를 해결하려면 null 체크 코드를 추가하면 된다. 하지만 매번 코드를 작성하면 꽤 번거롭다.
    - 타입스크립트 타입 체크 레벨을 엄격(strict) 모드로 바꾸면 꽤 많은 웹 API가 null을 반환하기 때문이다.

<br>

- null 체크 로직을 넣는 것이 번거롭고 값이 null이 아니라는 확신이 있다면 null 아님 보장 연산자(!)를 사용한다.
  ```ts
  function shuffleBooks(books: Books) {
    const result = books!.shuffle();
    return result;
  }
  ```
  - null 아님 보장 연산자를 사용하면 books 파라미터는 null이 아님을 타입스크립트에게 알려주는 것과 같다.
- null 아님 보장 연산자는 타입 관점에서 null이 아니라고 보장하는 것이지 애플리케이션을 실행할 때 실제로 null 값이 들어오면 실행 에러가 발생하므로 주의가 필요하다.
- as나 !를 사용한 타입 단언이 편리하기는 하지만 실행 시점의 에러는 막아 주지 않기 때문에 가급적이면 타입 추론에 의지하는 것이 좋다.

<br>
<br>