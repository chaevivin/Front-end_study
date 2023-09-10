# 연산자를 사용한 타입 정의
- 자바스크립트 OR 연산자인 ||와 AND 연산자인 && 기호를 따서 타입을 정의할 수 있다.

<br>
<br>

## 1. 유니언 타입
- **유니언 타입(union type)**: 여러 개의 타입 중 한 개만 쓰고 싶을 때 사용하는 문법이다.
  ```ts
  function logText(text: string) {
    console.log(text);
  }

  logText('hi');    // hi
  ```
  - logText()는 간단한 문자열을 넘겨 호출하는 함수이다.
  - 문자열이 아니라 다른 타입의 데이터를 넘기고 싶을 때는 유니언 타입을 사용하면 에러가 발생하지 않고 정상적으로 동작한다.
  ```ts
  function logText(text: string | number) {
    console.log(text);
  }

  logText('hi');    // hi
  logText(1);    // 1
  ```

<br>
<br>

## 2. 유니언 타입의 장점
- 유니언 타입을 사용해서 같은 동작을 하는 함수의 코드 중복을 줄일 수 있다.
  ```ts
  function logText(text: string) {
    console.log(text);
  }

  function logText(text: number) {
    console.log(text);
  }
  ```
  ```ts
  function logText(text: string | number) {
    console.log(text);
  }
  ```
  - 유니언 타입을 사용하지 않으면 문자열을 출력하는 함수, 숫자를 출력하는 함수를 작성해야 하지만, 유니언 타입을 사용하면 하나의 함수만 사용할 수 있다.
- 여러 개의 타입을 받기 위해 any 타입을 사용했을 때보다 더 타입을 정확하게 사용할 수 있다.
  ```ts
  function logText(text: any) {
    console.log(text);
  }
  ```
  - any 타입은 타입이 없는 것과 마찬가지이기 때문에 타입스크립트의 장점을 살리지 못한다. 
    - 타입스크립트의 장점: 타입이 정해져 있을 때 자동으로 속성과 API를 자동 완성하는 특징
    - any 타입으로 선언한 경우에는 예를 들어, toString()이라는 API 스펙을 정확히 몰랐을 때 toStirng라는 오탈자가 발생하더라도 에러를 미리 발견하기 어렵다.
    - 유니언 타입으로 지정된 코드에서는 에러가 발생해 오탈자를 미리 발견할 수 있다.

<br>
<br>

## 3. 유니언 타입을 사용할 때 주의할 점
```ts
interface Person {
  name: string;
  age: number;
}

interface Developer {
  name: string;
  skill: string;
}

function introduce(someone: Person | Developer) {
  console.log(someone);
  console.log(someone.age);    // Error
}
```
위의 코드를 살펴보면,
- introduce()는 두 인터페이스 중 1개를 받아 콘솔에 출력하는 함수이다.
- Person의 age 속성을 출력하면 에러가 발생한다.
  - 함수의 파라미터에 유니언 타입을 사용하면 함수에 어떤 값이 들어올 지 알 수 없기 때문이다.
- 함수 내부에서 파라미터 타입 종류에 따라 특정 로직을 실행하고 싶다면 in 연산자를 사용한다.
  ```ts
  function introduce(someone: Person | Developer) {
    if ('age' in someone) {
      console.log(someone.age);
      return;
    }
    if ('skill' in someone) {
      console.log(someone.skill);
      return;
    }
  }
  ```
  - in 연산자는 객체에 특정 속성이 있는지 확인하여 속성이 있으면 true, 속성이 없으면 false를 반환한다.
  - someone 파라미터에 Person 타입의 데이터가 들어오면 첫 번째 if문이 실행되고, Developer 타입의 데이터가 들어오면 두 번째 if문이 실행된다.

<br>

```ts
function logText(text: string | number) {
  if (typeof text === 'string') {
    console.log(text.toUpperCase());
  }
  if (typeof text === 'number') {
    console.log(text.toLocaleString());
  }
}
```
위의 코드를 살펴보면,
- logText()는 파라미터 값이 문자라면 모두 대문자로 변경해서 출력하고, 숫자라면 사용하고 있는 국가 언어에 맞추어 숫자 형식을 변경해주는 함수이다.
- typeof 연산자로 타입을 구분했다.
  - typeof는 해당 데이터가 어떤 데이터 타입을 갖고 있는지 문자열로 반환해준다.
- 파라미터에 문자열 데이터가 들어오면 첫 번째 if문 블록 안 코드가 실행되기 때문에 if문 안에서는 파라미터인 text가 문자열로 간주된다.
- 파라미터에 숫자 데이터가 들어오면 두 번째 if문 블록 안 코드가 실행되기 때문에 if문 안에서는 파라미터인 text가 숫자로 간주된다.

<br>

- 함수의 파라미터에서 유니언 타입을 선언하면 함수 안에서는 두 타입의 공통 속성과 메서드만 자동 완성된다.
- 특정 타입의 속성과 메서드를 사용하고 싶으면 typeof나 in 연산자를 사용하여 타입을 구분 후 코드를 작성해야 한다.

<br>
<br>

## 4. 인터섹션 타입
- **인터섹션 타입(intersection type)**: 타입 2개를 하나로 합쳐서 사용할 수 있는 타입이다,
  - 보통 인터페이스 2개를 합치거나 타입 정의 여러 개를 하나로 합칠 때 사용한다.
  ```ts
  interface Avenger {
    name: string;
  }

  interface Hero {
    skill: string;
  }

  function introduce(someone: Avenger & Hero) {
    console.log(someone.name);    
    console.log(someone.skill);
  }

  introduce({ name: 'chaebeen', skill: 'ts' });
  ```
  - name 속성을 갖는 Avenger 인터페이스와 skill 속성을 갖는 Hero 인터페이스를 선언하고 introduce() 함수의 파라미터에 인터섹션 타입(&)으로 정의했다.
  - someone 파라미터는 Avenger 타입과 Hero 타입의 name과 skill 속성을 사용할 수 있다.
  - introduce() 함수를 호출할 때 name과 skill 속성을 모두 가진 객체를 인자로 넘길 수 있다.
  - 여기에서 name이나 skill 속성 중 하나만 객체에 넘기면 에러가 발생한다.
    - 이 에러는 introduce() 함수의 파라미터가 Avenger와 Hero 타입의 인터섹션 타입으로 정의되어 있기 때문에 두 타입의 모든 속성을 만족하는 객체를 인자로 넘겨야 하기 때문에 발생한다.
- 인터섹션 타입은 이미 있는 타입 2개를 합칠 때 사용한다.

<br>
<br>