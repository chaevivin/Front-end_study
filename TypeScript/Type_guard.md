# 타입 가드
- 타입 가드는 여러 개의 타입으로 정의된 특정 값을 다룰 때 예기치 못한 곳에서 타입 에러가 발생할 때 사용한다.

<br>
<br>

## 1. 타입 가드란?
- **타입 가드(type guard)**: 여러 개의 타입으로 지정된 값을 특정 위치에서 원하는 타입으로 구분하는 것을 의미한다.
  - 타입 시스템 관점에서는 넓은 타입에서 좁은 타입으로 타입 범위를 좁힌다는 의미로 볼 수 있다.
  - 여러 타입이 있을 때 원하는 타입을 뽑기 위해 다른 타입을 막아 낸다(가드한다)는 의미이다.
  ```ts
  function updateInput(textInput: number | string | boolean) {
    // 타입 가드
    if (typeof textInput === 'number') {
      textInput
    }
  }
  ```
  - if문이 타입 가드 역할을 한다. 
  - if문 안에서 textInput 파라미터는 number 타입으로만 간주된다.
- 타입 가드를 사용하면 특정 위치에서 여러 개의 타입 중 하나의 타입으로 걸러 낼 수 있다.

<br>
<br>

## 2. 왜 타입 가드가 필요할까?
```ts
function updateInput(textInput: number | string | boolean) {
  textInput.toFixed(2);    // Error
}
```
위의 코드를 살펴보면,
- textInput 파라미터는 number, string, boolean의 유니언 타입이다.
- updateInput() 함수에 숫자를 넘겨 소수점 두 자리까지만 표기하고 싶다면 toFixed()라는 자바스크립트 내장 API를 사용할 수 있다. 하지만 이 API는 숫자 타입에서만 제공하기 때문에 유니언 타입에서는 에러가 발생한다.

<br>

### 2.1. 타입 단언으로 타입 에러 해결하기
- textInput 파라미터의 타입을 number로 강제한다.
  ```ts
  function updateInput(textInput: number | string | boolean) {
    (textInput as number).toFixed(2);
  }
  ```
  - as 키워드를 사용하여 textInput의 타입을 number로 단언했기 때문에 기존 타입 에러가 발생하지 않고 number 타입의 API 목록도 볼 수 있다.
- 이렇게 하면 타입 에러는 해결되지만 문제가 발생한다.
  - 실행 시점의 에러는 막을 수 없다.
  - 타입 단언을 계속해서 사용해야 한다.

<br>

### 2.2. 타입 단언으로 해결했을 때의 문제점
- 실행 시점의 에러는 막을 수 없다.
  ```ts
  function updateInput(textInput: number | string | boolean) {
    (textInput as number).toFixed(2);
  }

  updateInput('hello');    // Error
  ```
  - updateInput() 함수에 문자열을 넣으면 실행 에러가 발생한다.
  - 에러 발생 이유: toFixed() API는 숫자 데이터에서만 사용할 수 있는 API이기 때문에 문자열 데이터에서는 지원되지 않아 함수가 아니라는 에러가 발생한다.
  - 이 에러는 타입 단언의 문제점이기도 하다.
  - 이 에러를 해결하려면 또 다른 코드를 추가해야 한다.
- 타입 단언을 계속해서 사용해야 한다.
  ```ts
  function updateInput(textInput: number | string | boolean) {
    (textInput as number).toFixed(2);
    console.log((textInput as string).length);
  }
  ```
  - 함수 안에 문자열 길이를 출력하는 코드를 추가하였다.
  - 숫자 API인 toFixed()를 사용하려고 number로 한 번 타입 단언하고, 문자열 속성인 length 속성에 접근하려고 string으로 타입을 한 번 더 단언하였다.
  - 이렇게 하면 매번 특정 타입으로 인식시킬 때 as 키워드를 사용하여 타입을 단언하는 코드를 작성해야 한다.
  - 번거로울 뿐만 아니라 반복적으로 똑같은 코드를 작성해야 한다.

<br>

### 2.3. 타입 가드로 문제점 해결하기
- 타입 단언을 사용해서 생기는 문제점은 타입 가드로 해결할 수 있다.
  - 함수 안에서 타입별로 나누어 로직을 작성할 수 있다.
  ```ts
  function updateInput(textInput: number | string | boolean) {
    if (typeof textInput === 'number') {
      textInput.toFixed(2);
      return;
    }

    if (typeof textInput === 'string') {
      console.log(textInput.length);
      return;
    }
  }
  ```
  - 첫 번째 if문 안의 textInput은 number 타입이기 때문에 toFixed() 함수를 사용하여 소수점 둘째 자리까지 표현할 수 있다.
  - 두 번째 if문 안의 textInput은 string 타입이기 때문에 length로 문자열 길이를 출력할 수 있다.
  - 타입 단언보다 타입 가드를 사용하는 것이 유리해 보인다.

<br>
<br>

## 3. 타입 가드 문법
- typeof
- instanceof
- in

<br>

### 3.1. typeof 연산자
- typeof 연산자는 자바스크립트에서 특정 코드의 타입을 문자열 값으로 반환 해준다.
  ```js
  typeof 10;    // number
  typeof 'hello';    // string
  typeof function() {}    // function
  ```
- 타입 가드에서의 typeof 연산자
  ```ts
  function printText(text: string | number ){
    if (typeof text === 'string') {
      // ...
    }
  }
  ```
  - text 파라미터 타입은 string이나 number가 될 수 있다.
  - 타입이 string일 때만 특정 로직을 실행하게끔 하려면 if문과 typeof 연산자를 조합한다.
- typeof 연산자를 사용하여 특정 위치에서 원하는 타입으로 구분할 수 있다.

<br>

### 3.2. instanceof 연산자
- instanceof 연산자는 자바스크립트에서 변수가 대상 객체의 프로토타입 체인에 포함되는지 확인하여 true, false를 반환해준다.
  ```ts
  function Person(name, age) {
    this.name = name;
    this.age = age;
  }

  const chaebeen = new Person('채빈', 20);
  const levi = { name: '리바이', age: 30 };

  console.log(chaebeen instanceof Person);    // true
  console.log(levi instanceof Person);    // false
  ```
  - chaebeen 변수는 Person 생성자 함수의 인스턴스이기 때문에 true 값이 반환된다.
    - chaebeen 변수의 프로토타입 체인 구조: `chaebeen -> Person -> Object`
  - levi 변수는 Person 생성자 함수의 인스턴스가 아니기 때문에 false 값이 반환된다.
    - levi 변수의 프로토타입 체인 구조: `levi -> Object`
- 타입 가드에서의 instanceof 연산자
  ```ts
  class Person {
    name: string;
    age: number;

    constructor(name, age) {
      this.name = name;
      this.age = age;
    }
  }

  function fetchInfoByProfile(profile: Person | string) {
    // ...
  }
  ```
  - fetchInfoByProfile() 함수의 파라미터에 Person 클래스와 string을 유니언 타입으로 선언하였다.
  - fetchInfoByProfile() 함수는 파라미터의 값에 따라 다른 정보를 가져오는 함수이다.
  - 파라미터가 Person 클래스 모양의 객체라면 콘솔에 출력하고 string 타입이라면 경고창을 표시하려면 instanceof 연산자로 타입 가드를 할 수 있다.
    ```ts
    function fetchInfoByProfile(profile: Person | string) {
      if (profile instanceof Person) {
        // ...
      }
    }
    ```
    - if 문 블록 안에서는 profile이 Person 타입으로 추론된다. 그래서 Person 타입의 name과 age 속성에 접근할 수 있다.
- instanceof는 주로 클래스 타입이 유니언 타입으로 묶여 있을 때 타입을 구분하기 위해 사용한다.

<br>

### 3.3. in 연산자
- in 연산자는 자바스크립트에서 객체에 속성이 있는지 확인할 때 사용한다.
  - 객체에 특정 속성이 있으면 true, 그렇지 않으면 false를 반환한다.
  ```js
  const book = {
    name: 'harry potter',
    rank: 1
  };

  console.log('name' in book);    // true
  console.log('address' in book);    // false
  ```
  - name과 rank를 속성으로 갖는 book 객체를 선언하고 in 연산자를 사용하여 book 객체에 name 속성이 있는지 확인하였다. 
- 타입 가드에서의 in 연산자
  ```ts
  interface Book {
    name: string;
    rank: number;
  }

  interface OnlineLecture {
    name: string;
    url: string;
  }

  function learnCourse(material: Book | OnlineLecture) {
    material.url;    // Error
  }
  ```
  - 책(Book)과 온라인 강의(OnlineLecture)를 의미하는 인터페이스 2개를 선언하고, 수업을 듣는다는 의미인 learnCourse() 함수를 선언하였다.
  - learnCourse() 함수의 파라미터 타입은 Book 인터페이스와 OnlineLecture 인터페이스의 유니언 타입이다.
  - 온라인 강의의 url을 사용하면 타입 에러가 발생한다.
  - 온라인 강의 url에 접근하기 위해 in 연산자를 사용할 수 있다.
    ```ts
    function learnCourse(material: Book | OnlineLecture) {
      if ('url' in material) {
        // ...
      }
    }
    ```
    - if 문에 in 연산자를 사용하여 material 파라미터에 url 속성이 있는지 체크한다.
    - url 속성은 OnlineLecture에 있기 때문에 material 타입이 OnlineLecture로 추론된다.
  - 두 인터페이스에 공통으로 있는 name 속성을 in 연산자로 체크하면?
    ```ts
    function learnCourse(material: Book | OnlineLecture) {
      if ('name' in material) {
        // ...
      }
    }
    ```
    - 특정 타입으로 구분해 주지 않는다. -> 타입 가드 역할을 하지 못한다.
- in 연산자를 사용하여 인터페이스 2개가 유니언 타입으로 연결되어 있을 때 특정 인터페이스로 구분할 수 있다.
- 타입 가드로 특정 타입을 거럴 내려면 해당 타입이 다른 타입과 구분되는 유일한 특징을 조건으로 걸어야 한다.

<br>
<br>

## 4. 타입 가드 함수
- 타입 가드 함수: 타입 가드 역할을 하는 함수를 의미한다.
- 객체 유니언 타입 중 하나를 구분하는 데 사용하며, in 연산자와 역할을 같지만 좀 더 복잡한 경우에도 사용할 수 있다.
- 타입 가드 함수는 is 연산자를 사용하여 여러 개의 타입 중 하나로 구분한다.
  ```ts
  function isPerson(someone: Person | Developer): someone is Person {
    // ...
  }
  ```
  - Person 타입과 Developer 타입 중 Person 타입으로 구분하는 타입 가드 함수이다. 

<br>

### 4.1. 타입 가드 함수 예시
- in 연산자로 타입 가드
  ```ts
  interface Person {
    name: string;
    age: number;
  }

  interface Developer {
    name: string;
    skill: string;
  }

  function greet(someone: Person | Developer) {
    if ("age" in someone) {
      console.log("사람의 나이는 ", someone.age);
    } else {
      console.log("개발자의 스킬은 ", someone.skill);
    }
  }
  ```
  - in 연산자를 사용하여 타입 가드를 적용하였다.
  - greet() 함수는 파라미터가 Person 타입일 때는 나이를 출력하고, Developer 타입일 대는 스킬을 출력한다.
- 타입 가드 함수
  ```ts
  function isPerson(someone: Person | Developer): someone is Person {
    return (someone as Person).age !== undefined;
  }
  ```
  - isPerson() 함수는 타입 가드 함수로, 파라미터는 Person 타입과 Developer 타입의 유니언 타입으로 정의되어있다.
  - 유니언 타입으로 정의된 파라미터는 별도의 타입 가드를 적용하지 않으면 함수 내부에서 공통 속성만 접근할 수 있다.
  - 따라서 타입 단언(as)으로 Person 타입으로 추론을 강제한 후에 age 속성에 접근한다. 
  - 함수의 반환 타입은 is 연산자를 사용하여 someone 파라미터가 Person 타입인지 체크한다.
  - 타입 가드 함수의 역할
    - **Person과 Developer 타입의 값을 받아 Person 타입의 속성이 있는지 확인한 후 속성이 있다면 Person 타입으로 간주하라.**
- 기존 코드에 적용
  ```ts
  interface Person {
    name: string;
    age: number;
  }

  interface Developer {
    name: string;
    skill: string;
  }

  function isPerson(someone: Person | Developer): someone is Person {
    return (someone as Person).age !== undefined;
  }

  function greet(someone: Person | Developer) {
    if (isPerson(someone)) {
      console.log("사람의 나이는 ", someone.age);
    } else {
      console.log("개발자의 스킬은 ", someone.skill);
    }
  }
  ```
  - greet() 함수의 if문에서 isPerson() 타입 가드 함수를 사용해서 someone 파라미터의 타입이 Person 타입인지 아닌지 구분한다.
  - isPerson() 타입 가드 함수의 결과는 Person 타입이므로 if 문 안에서는 someone 파라미터가 Person 타입으로 추론되고, else 문에서는 isPerson() 함수 조건을 만족하지 않기 때문에 Developer 타입으로 간주된다.

<br>

### 4.2. 복잡한 경우의 타입 가드 예시
- 타입 가드 함수가 꼭 필요한 경우
  ```ts
  interface Hero {
    name: string;
    nickname: string;
  }

  interface Person {
    name: string;
    age: number;
  }

  interface Developer {
    name: string;
    age: string;
    skill: string;
  }

  function greet(someone: Hero | Person | Developer){
    if ('age' in someone) {
      console.log(someone.age);
    }
  }
  ```
  - Hero, Person, Developer 인터페이스 타입을 각각 선언하고, greet() 함수의 파라미터에 유니언 타입으로 연결하였다.
  - greet() 함수에서 파라미터가 Person 타입인 경우 나이를 출력할 수는 있지만 Developer 타입에도 age 속성이 있기 때문에 숫자 관련 내장 API나 내장 속성은 사용할 수 없다. -> in 연산자로는 원하는 타입으로 구분할 수 없다.
  ```ts
  function isPerson(someone: Hero | Person | Developer): someone is Person {
    return typeof (someone as Person).age === 'number';
  }

  function greet(someone: Hero | Person | Developer){
    if (isPerson(someone)) {
      console.log(someone.age);
    }
  }
  ```
  - isPerson() 함수는 Hero, Person, Developer 유니언 타입을 파라미터로 받고, Person 타입인지 아닌지를 결과값으로 반환한다.
  - isPerson() 함수는 인자로 받은 객체의 age 속성 타입이 number면 Person 타입이라고 구분해 주는 타입 가드 함수이다.
  - greet() 함수의 if 문 안에서 someone 파라미터 타입은 Person 타입으로 간주된다.
    - someone.age 속성의 타입은 number이다.
    - age 속성으로 number 관련 내장 API나 내장 속성에 접근할 수 있다. 
- 타입 가드 함수를 사용하면 여러 가지 타입이 얽혀 있을 때 쉽게 타입을 구분할 수 있다.

<br>
<br>

## 5. 구별된 유니언 타입
- **구별된 유니언 타입(discrimiated unions)**: 유니언 타입을 구성하는 여러 개의 타입을 특정 속성의 유무가 아니라 특정 속성 값으로 구분하는 타입 가드 문법을 의미한다.
  ```ts
  interface Person {
    name: string;
    age: number;
    industry: 'common';
  }

  interface Developer {
    name: string;
    age: string;
    industry: 'tech';
  }
  ```
  - Person 타입과 Developer 타입의 속성을 모두 동일하게 name, age, industry로 선언하였다.
  - 해당 타입 2개를 갖는 유니언 타입을 특정 타입으로 걸러내는 방법
    - 두 타입의 속성 이름은 모두 같기 때문에 in 연산자는 사용할 수 없다.
    - 구별된 유니언 타입을 사용할 수 있다.
      ```ts
      function greet(someone: Person | Developer) {
        if (someone.industry === 'common') {
          // ...
        }
      }
      ```
      - if문 안은 industry 속성 값이 'common', 즉 someone의 타입이 Person이다.
- 속성의 유뮤가 아니라 속성의 문자열 타입 값을 비교해서 타입을 구분해 내는 것이 구별된 유니언 타입이다.

<br>
<br>

## 6. switch문과 연산자
- 타입 가드는 if문 말고도 switch문이나 비교, 논리 연산자로도 적용할 수 있다.

<br>

### 6.1. switch문
- switch문으로 타입 가드 적용하기
  ```ts
  interface Person {
    name: string;
    age: number;
    industry: 'common';
  }

  interface Developer {
    name: string;
    age: string;
    industry: 'tech';
  }

  function greet(someone: Person | Developer) {
    switch(someone.industry) {
      case 'common':
        console.log(someone.age.toFixed(2));
        break;
      case 'tech':
        console.log(someone.age.split(' '));
        break;
    }
  }
  ```
  - someone 파라미터 industry 속성 값이 문자열 common인지 tech인지에 따라 각 case 블록 안 타입은 Person 또는 Developer이다.
  - switch문 첫 번째 case에서는 industry 속성 값이 common인지 확인하고, 맞다면 해당 case 블록 안 타입은 Person이다.
  - switch문 두 번째 case에서는 industry 속성 값이 tech인지 확인하고, 맞다면 해당 case 블록 안 타입은 Developer이다.
  - 만약 Peson과 Developer 타입 중 어느 타입에도 해당되지 않는 조건 값이 존재한다면 에러가 발생한다.

<br>

### 6.2. 논리, 비교 연산자
- 논리, 비교 연산자로 타입 가드 적용하기
  ```ts
  function sayHi(message: string | null) {
    if (message.length >= 3) {
      console.log(message);
    }
  }
  ```
  - 함수에 입력받은 문자열 길이가 3 이상일 때 해당 문자열을 출력해준다.
  - 프로젝트의 타입스크립트 타입 검사 레벨(strict)을 올리면 에러가 발생한다.
    - 에러 발생 이유: 파리미터 타입이 string과 null 유니언 타입인데 null 타입이라면 length 속성에 접근할 수 없기 때문이다.
    - 해결 방법: if문을 쓰거나 타입 단언 문법인 `!`을 사용하거나 논리 연산자를 사용할 수 있다.
      ```ts
      function sayHi(message: string | null) {
        // if문
        if (message === null) {
          return;
        }

        if (message.length >= 3) {
          console.log(message);
        }
      }
      ```
      - message가 null일 대 함수를 반환해 주면 첫 번째 if문 아래는 모두 message가 string 타입으로 추론된다.
      ```ts
      function sayHi(message: string | null) {
        if (message!.length >= 3) {
          console.log(message);
        }
      }
      ```
      - message 값이 null이 아니라는 의미의 `!` 연산자를 붙였기 때문에 string 타입으로 간주된다. string 타입으로 간주되면 length 속성에 접근할 수 있다.
      ```ts
      function sayHi(message: string | null) {
        if (message && message.length >= 3) {
          console.log(message);
        }
      }
      ```
      - `&&` 연산자는 AND 연산자로 A이면서 B일 때 true를 반환한다.
      - if문에 message가 있으면(message가 null이 아니면) message의 length 속성이 3 이상인지 체크한다.

<br>
<br>