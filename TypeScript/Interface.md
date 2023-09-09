# 인터페이스

## 1. 인터페이스란?
- 타입스크립트에서 **인터페이스(interface)** 는 객체 타입을 정의할 때 사용하는 문법이다.
- 인터페이스로 타입을 정의할 수 있는 부분
  - 객체의 속성과 속성 타입
  - 함수의 파라미터와 반환 타입
  - 함수의 스펙 (파라미터의 개수와 반환값 여부 등)
  - 배열과 객체를 접근하는 방식
  - 클래스

<br>
<br>

## 2. 인터페이스를 이용한 객체 타입 정의
- 인터페이스를 객체 타입으로 정의
  ```js
  const person = { name: 'chaebeen', age: 20 };
  ```
  - 이 객체의 타입을 인터페이스로 정의
  ```js
  interface User {
    name: string;
    age: number;
  }
  ```
  - interface 예약어를 이용하여 User라는 인터페이스를 선언했다.
  - 인터페이스의 속성으로 name과 age를 각각 문자열과 숫자 타입으로 정의했다.
  ```ts
  interface User {
    name: string;
    age: number;
  }

  const person: User = { name: 'chaebeen', age: 20 };
  ```
  - person이라는 객체에 User라는 인터페이스를 지정했다.
  - 이 객체는 인터페이스의 타입에 맞게 문자열로 된 이름과 숫자로 된 나이가 정의되어 있기 때문에 에러가 발생하지 않는다.
  - 하지만 인터페이스 타입과 맞지 않는 객체에 인터페이스를 지정하면 에러가 발생한다.
  ```ts
  // TypeError
  const person1: User = { name: 'chaebeen', age: '20' };
  const person2: User = { name: 'chaebeen', age: 20, hobby: 'coding' };
  ```
- 인터페이스를 이용하여 객체의 속성과 들어갈 데이터 타입을 정확하게 정의할 수 있다.

<br>
<br>

## 3. 인터페이스를 이용한 함수 타입 정의
- 인터페이스는 객체의 타입을 정의할 때 사용하기 때문에 객체가 활용되는 모든 곳에 인터페이스를 사용할 수 있다.
- 객체는 함수의 파라미터로 사용되고 반환값으로도 사용할 수 있다.

<br>

### 3.1. 함수 파라미터 타입 정의
```ts
interface Person {
  name: string;
  age: number;
}

function logAge(someone: Person) {
  console.log(someone.age);
}

const user1 =  { name: 'chaebeen', age: 20 };
logAge(user1);    // 20

const user2 =  { name: 'chaebeen' };
logAge(user2);    // Error
```
위의 코드를 살펴보면,
- logAge()는 someone이라는 인자를 받아와 인자 안의 age 속성을 출력한다.
- 함수의 파라미터를 명시적으로 선언하기 위해 인터페이스를 사용하여 타입을 선언한다.
- user1을 logAge()에 넘기면 logAge() 함수의 파라미터 타입을 만족하기 때문에 에러 없이 실행된다.
- user2를 logAge()에 넘기면 user2에는 name 속성만 있기 때문에 logAge()의 파라미터 타입과 일치하지 않아 에러가 발생한다.

<br>

### 3.2. 함수 반환 타입 정의
```ts
interface Person {
  name: string;
  age: number;
}

function getPerson(someone: Person): Person {
  return someone;
}

const user = getPerson({ name: 'chaebeen', age: 20 });
```
위의 코드를 살펴보면,
- getPerson()은 someone이라는 인자를 받아와 someone을 반환한다.
- getPerson()의 반환값의 타입을 인터페이스로 정의해주었다.
- getPerson() 함수의 호출 결과를 변수에 할당하면 해당 변수가 Person 인터페이스 타입으로 추론된다.

<br>
<br>

## 4. 인터페이스의 옵션 속성
- 인터페이스로 정의된 객체의 속성을 선택적으로 사용하고 싶을 때 옵션 속성을 사용한다.
  - 2개의 속성을 가진 객체에서 1개의 속성만 필요할 때
  ```ts
  interface Person {
    name: string;
    age: number;
  }

  function logAge(someone: Person) {
    console.log(someone.age);
  }

  const user1 =  { age: 20 };
  logAge(user1);    // Error
  ```
  - 위 예제는 변수에 age 속성만 있기 때문에 에러가 발생한다.
  - 이 에러는 logAge()의 인자가 name과 age 속성을 가진 객체여야 하는데 age 속성만 정의된 객체를 받아서 발생한다.
- **옵션 속성(optional property)**: 인터페이스로 정의된 객체의 속성을 선택적으로 사용하고 싶을 때 사용한다.
  - 인터페이스의 속성에 `?`를 붙여 사용한다.
  ```ts
  interface Person {
    name?: string;
    age: number;
  }

  function logAge(someone: Person) {
    console.log(someone.age);
  }

  const user1 =  { age: 20 };
  logAge(user1);    // 20
  ```
  - logAge() 함수의 인자에 Person 인터페이스를 만족하는 객체를 넘겨야 하지만 name 속성은 있어도 되고 없어도 되기 때문에 에러가 발생하지 않는다.
- 옵션 속성을 사용하지 않고 인터페이스 타입만 변경했을 때
  ```ts
  interface Person {
    age: number;
  }

  function logAge(someone: Person) {
    console.log(someone.age);
  }

  const user1 =  { age: 20 };
  logAge(user1);    // 20
  ```
  - 인터페이스의 name 속성을 사용하면 에러가 발생하지 않는다.
  ```ts
  interface Person {
    name: string;
    age: number;
  }

  function logAge(someone: Person) {
    console.log(someone.age);
  }

  function logPersonInfo(you: Person) {
    console.log(you.name);
    console.log(you.age);
  }

  const user1 =  { age: 20 };
  logAge(user1);    // Error
  ```
  - 하지만 logPersonInfo() 함수가 추가되면 logAge() 함수에서 에러가 발생한다.
  - logPersonInfo() 함수에서 name과 age 속성이 모두 필요하기 때문이다.
  - 인터페이스에 옵션 속성을 사용하면 에러가 발생하지 않는다.
- 옵션 속성은 상황에 따라서 유연하게 인터페이스 속성의 사용 여부를 결정할 수 있다.

<br>
<br>

## 5. 인터페이스 상속
- 인터페이스의 상속으로 타입 정의를 확장할 수 있다.
  - 상속: 객체 관 관계를 형성하는 방법이며, 상위(부모) 클래스의 내용을 하위(자식) 클래스가 물려받아 사용하거나 확장하는 기법을 의미한다.
- 자바스크립트에서도 클래스로 상속을 구현할 수 있다.
  ```js
  class Person {
    constructor(name, age) {
      this.name = name;
      this.age = age;
    }

    logAge() {
      console.log(this.age);
    }
  }

  class Developer extends Person {
    constructor(name, age, skill) {
      super(name, age);
      this.skill = skill;
    }

    logDeveloperInfo() {
      this.logAge();
      console.log(this.name);
      console.log(this.skill);
    }
  }
  ```
  - Person 클래스를 정의하고 Developer 클래스에서 상속받아 클래스 내부를 구현했다.
  - Person 클래스에서 name과 age 속성이 정의되어 있고 logAge() 메서드가 구현되어 있기 때문에 Developer에서 extends로 상속받으면서 이 속성과 메서드를 모두 사용할 수 있다.
  - Developer 클래스의 logDeveloperInfo() 메서드에서 this.logAge()로 Person 클래스의 logAge() 메서드에 접근할 수 있다.
  ```js
  const user = new Developer('chaebeen', 20, 'ts');
  user.logDeveloperInfo();    // 20, chaebeen, ts
  ```
  - Developer 클래스로 인스턴스를 생성하면 logDeveloperInfo() 메서드를 호출했을 때 나이, 이름, 기술이 출력된다.

<br>

### 5.1. 인터페이스 상속이란?
- 인터페이스를 상속받을 때는 `extends` 예약어를 사용한다.
  ```ts
  interface Person {
    name: string;
    age: number;
  }

  interface Developer extends Person {
    skill: string;
  }

  const user: Developer = {
    name: 'chaebeen',
    age: 20,
    skill: 'ts'
  }
  ```
  - Person 인터페이스를 선언하고 Developer 인터페이스에 extends로 상속했다.
  - Person 인터페이스에 정의된 name과 age 속성 타입이 Developer 인터페이스에 상속되었기 때문에 마치 Developer 인터페이스는 다음과 같이 정의한 효과가 나타난다.
    ```ts
    interface Developer {
      name: string;
      age: number;
      skill: string;
    }
    ```

<br>

### 5.2. 인터페이스를 상속할 때 참고 사항
- 상위 인터페이스의 타입을 하위 인터페이스에서 상속받아 타입을 정의할 때 상위 인터페이스의 타입과 호환 되어야 한다.
  - 호환이 된다는 것은 상위 클래스에서 정의된 타입을 사용해야 한다는 의미이다.
  ```ts
  interface Person {
    name: string;
    age: number;
  }

  interface Developer extends Person {    // Error
    name: number;
  }
  ```
  - Developer 인터페이스에서 Person 인터페이스를 상속받았는데 Person 인터페이스에서 선언된 name 속성 타입을 Developer 인터페이스에서 다른 타입으로 정의하자 에러가 발생했다.
  - 에러 내용은 'Developer 인터페이스에서 정의한 name 속성의 number 타입이 Person 인터페이스에서 정의한 name 속성의 string 타입과 호환되지 않는다.'
  - 인터페이스를 상속하여 사용할 때는 부모 인터페이스에 정의된 타입을 자식 인터페이스에서 모두 보장해주어야 한다.
- 상속을 여러 번 할 수 있다.
  ```ts
  interface Hero {
    power: boolean;
  }

  interface Person extends Hero {
    name: string;
    age: number;
  }

  interface Developer extends Person {
    skill: string;
  }

  const ironman: Developer = {
    name: '아이언맨',
    age: 21,
    skill: '만들기',
    power: true
  }
  ```
  - Hero 인터페이스를 Person 인터페이스가 상속받고, Person 인터페이스를 Developer 인터페이스가 상속받는다.
  - ironman 변수에 Developer 인터페이스를 정의하면 인터페이스 3개에 제공하는 속성을 모두 타입에 맞게 선언해 주어야 한다. 
  
<br>
<br>

## 6. 인터페이스를 이용한 인덱싱 타입 정의
- **인덱싱**: 객체의 특정 속성을 접근하거나 배열의 인덱스로 특정 요소에 접근하는 동작을 의미한다.
  ```ts
  const user = {
    name: 'chaebeen',
    admin: true
  };
  console.log(user['name']);    // chaebeen

  const companies = ['삼성', '네이버', '구글'];
  console.log(companies[2]);    // 구글
  ```

<br>

### 6.1. 배열 인덱싱 타입 정의
- 배열을 인덱싱할 때 인터페이스로 인덱스와 요소의 타입을 정의할 수 있다.
  ```ts
  interface StringArray {
    [index: number]: string;
  }

  const companies: StringArray = ['삼성', '네이버', '구글'];
  ```
  - `[index: number]`는 어떤 숫자든 모든 속성의 이름이 될 수 있다는 의미이다.
  - `[index: number]: string`에서 속성 이름은 숫자고 그 속성 값으로 문자열 타입이 와야 한다는 의미이다.
    ```ts
    companies[0];    // 삼성
    companies[1];    // 네이버
    ```
  - StringArray 인터페이스는 배열의 인덱스가 0이 될 수도 있고 3, 99, 2000 등 어떤 숫자든 될 수 있다.
  - StringArray 인터페이스의 인덱스 타입을 문자열로 변경하면 타입 에러가 발생한다.
  ```ts
  interface StringArray {
    [index: string]: string;
  }

  // Error
  const companies: StringArray = ['삼성', '네이버', '구글'];
  ```
  - 배열의 인덱스는 숫자여야 하는데 문자열로 인덱스 타입을 강제하여 배열 정의에 위배되었다.
    ```ts
    companies['첫번째'];     // undefined
    companies[0];    // 삼성
    ```
  - 배열은 숫자 인덱스를 이용해 특정 요소에 접근할 수 있다.
  - 하지만 StringArray 인터페이스의 인덱스 타입을 string으로 변경해 버렸기 때문에 배열인 companies 변수에서 에러가 발생했다.
  - 인덱스 타입이 number가 아닌 string 타입으로 선언된 StringArray 인터페이스는 더 이상 배열의 타입으로 사용할 수 없다.

<br>

### 6.2. 객체 인덱싱 타입 정의
- 객체 인덱싱의 타입도 정의할 수 있다.
  ```ts
  interface SalayMap {
    [level: string]: number;
  }

  const salary: SalaryMap = {
    junior: 100
  };

  const money = salary['junior'];
  ```
  - SalaryMap 인터페이스는 속성 이름이 문자열 타입이고 속성 값이 숫자 타입인 모든 속성 이름/속성 값 쌍을 허용하겠다는 의미이다.
  - salary 객체의 속성에 접근하면 속성 값의 타입이 number이다.
  - SalaryMap 인터페이스의 속성 값 타입을 string으로 변경
  ```ts
  interface SalayMap {
    [level: string]: string;
  }

  const salary: SalaryMap = {
    junior: '100'
  };

  const money = salary['junior'];
  ```
  - salary 객체의 속성에 접근하면 속성 값의 타입이 string이다. 

<br>

### 6.3. 인덱스 시그니처란?
- **인덱스 시그니처(index signature)**: 정확한 속성 이름을 명시하지 않고 속성 이름의 타입과 속성 값의 타입을 정의하는 문법
- 단순히 객체와 배열을 인덱싱할 대 활용되고 객체의 속성 타입을 유연하게 정의할 때도 사용한다.
  ```ts
  interface SalaryInfo {
    junior: string;
  }

  const salary: SalaryInfo = {
    junior: '100',
  };
  ```
  - junior라는 속성이 있고 속성 값이 문자열이기 때문에 명시적으로 `junior: string`이라는 타입을 정의할 수 있다.
  - 만약 주니어뿐만 아니라 미드, 시니어 등 여러 레벨의 급여를 추가한다고 가정해보자.
  ```ts
  const salary: SalaryInfo = {
    junior: '100',
    mid: '200',
    senior: '300'
  };
  ```
  - 앞서 정의한 SalaryInfo의 인터페이스 타입과 객체 타입이 맞지 않기 때문에 에러가 발생한다.
  - 인터페이스 정의를 객체 내용에 맞추어 다음과 같이 수정해야 타입 에러가 발생하지 않는다.
  ```ts
  interface SalaryInfo {
    junior: string;
    mid: string;
    senior: string;
  }
  ```
  - 여기에 또 급여 정보가 추가된다면 다시 인터페이스를 수정해야 하는 번거로운 작업을 반복해야 한다.
  - 이때 인덱스 시그니처 방식을 이용하면 무수히 많은 속성을 추가할 수 있다.
  ```ts
  interface SalaryInfo {
    [level: string]: string;
  }

  const salary: SalaryInfo = {
    junior: '100',
    mid: '200',
    senior: '300',
    ceo: '0',
    newbie: '50'
  };
  ```
  - 속성 이름이 문자열이고 속성 값의 타입이 문자열이기만 하면 몇 개든 모두 추가할 수 있는 장점이 생긴다.

<br>

### 6.4. 인덱스 시그니처는 언제 쓸까?
- 객체의 속성 이름과 개수가 구체적으로 정의되어 있다면 인터페이스에서 속성 이름과 속성 값의 타입을 명시하는 것이 더 효과적이다.
  ```ts
  interface User {
    id: string;
    name: string;
  }

  const chaebeen: User = {
    id: '1',
    name: '채빈'
  };
  ```
- 인덱스 시그니처가 적용되어 있는 경우 코드를 작성할 때 구체적으로 어떤 속성이 제공될 지 알 수 없어 코드 자동 완성이 되지 않는다.
- 인터페이스에서는 인덱스 시그니처와 속성을 같이 섞어서 정의할 수 있다.
  ```ts
  interface User {
    [property: string]: string;
    id: string;
    name: string;
  }

  const chaebeen: User = {
    id: '1',
    name: '채빈'
  };
  ```
  - chaebeen 객체에는 id와 name 속성을 정의할 수 있고, 속성 이름과 값이 모두 문자열인 속성 쌍도 계속 추가할 수 있다.
  ```ts
  const chaebeen: User = {
    id: '1',
    name: '채빈',
    address: '판교'
  };
  ```
- 속성 이름은 모르지만 속성 이름의 타입과 값의 타입을 아는 경우에는 인덱스 시그니처를 활용한다,

<br>
<br>