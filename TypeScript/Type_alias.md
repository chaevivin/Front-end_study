# 타입 별칭

## 1. 타입 별칭이란?
- **타입 별칭(type alias)**: 특정 타입이나 인터페이스 등을 참조할 수 있는 타입 변수를 의미한다.
  - 타입에 의미를 부여해서 별도의 이름으로 부르는 것이다.
  - 자바스크립트의 변수처럼 해당 타입이 어떤 역할을 하는지 이름을 짓고 싶을 때 사용할 수도 있고, 여러 번 반복되는 타입을 변수화해서 쉽게 표기하고 싶을 때도 사용한다.
  ```ts
  type MyName = string;
  const name: MyName = 'chaebeen';
  ```
  - MyName이라는 타입 별칭을 선언하고 string 타입을 할당한다.
  - name 변수에 string 타입이 아니라 MyName이라는 타입 별칭을 지정한다,
- 장점
  - 타입 별칭을 사용하면 반복되는 타입 코드를 줄여 줄 수 있다.
  ```ts
  function logText(text: string | number) {
    // ...
  } 

  const message: string | number = 'hi';
  logText(message);
  ```
  - logText() 함수의 파라미터 타입에 문자열과 숫자를 지정하고 이 함수를 호출하려고 message 변수를 정의해서 호출한다,
  - logText() 함수의 파라미터와 message 변수에 같은 타입인 `string | number` 타입이 반복되기 때문에 코드 별칭을 사용하여 줄일 수 있다.
  ```ts
  type MyMessage = string | number;
  function logText(text: MyMessage) {
    // ...
  } 

  const message: MyMessage = 'hi';
  logText(message);
  ```
  - `string | number` 타입을 MyMessage라는 타입 별칭으로 정의하고 logText() 함수와 message 변수에 지정한다.
  - 단순히 반복되는 코드를 줄였을 뿐만 아니라 `string | number` 타입이 메시지에 사용되는 타입이라는 의미도 부여한다.
- 타입 별칭을 사용하면 타입에 의미를 담아 여러 곳에 재사용할 수 있다.
- 타입을 선언하고 다시 다른 타입으로 할당하면 중복으로 타입 에러가 발생한다.
  ```ts
  type MyName = string;
  type MyName = number;    // Error
  ```

<br>
<br>

## 2. 타입 별칭과 인터페이스의 차이점
- 타입 별칭과 인터페이스는 모두 객체 타입을 정의할 수 있다.
  ```ts
  type User = {
    id: string;
    name: string;
  }

  interface User {
    id: string;
    name: string;
  }
  ```

<br>

### 2.1. 코드 에디터에서 표기 방식 차이
- 타입을 정의하고 사용하는 관점에서 가장 쉽게 구분되는 점은 코드 에디터에 표시되는 정보이다.
- 타입 별칭
  ```ts
  type User = {
    id: string;
    name: string;
  }

  const chaebeen: User;
  ```
  - 타입 별칭의 이름은 User로 정의하고 변수에 연결했다.
  - 변수에 연결한 타입 별칭에 마우스 커서를 올리면 타입의 모든 정보가 미리보기 화면으로 표시된다.
- 인터페이스
  ```ts
  interface Admin {
    id: string;
    name: string;
  }

  const chaebeen: Admin;
  ```
  - Admin 인터페이스를 선언하고 변수에 연결했다.
  - 변수에 연결한 인터페이스에 마우스 커서를 올리면 Admin이 인터페이스라는 정보만 표시된다.
- 변수에 연결된 타입이 구체적으로 어떤 모양인지 파악할 때는 타입 별칭이 더 좋아 보이지만 무조건 타입 별칭만 쓰는 것은 좋지 않다.

<br>

### 2.2. 사용할 수 있는 타입 차이
- 타입 별칭과 인터페이스를 구분 짓는 또 다른 차이점은 정의할 수 있는 타입 종류에 있다.
  - 인터페이스: 주로 객체의 타입 정의
  - 타입 별칭: 일반 타입에 이름을 짓는 데 사용, 유니언 타입, 인터섹션 타입 등에 사용
  ```ts
  type ID = string;
  type Product = TShirt | Shoes;
  type Teacher = Person & Adult;
  ```
  - 인터페이스는 위 같은 타입을 정의할 수 없다.
  - 타입 별칭은 제네릭이나 유틸리티 타입 등 다양한 타입에 사용할 수 있다.
  ```ts
  type Gilbut<T> = {
    book: T;
  }

  type MyBeer = Pick<Beer, 'brand'>;
  ```
  - 인터페이스와 타입 별칭의 정의를 함께 사용할 수 있다.
  ```ts
  interface Person {
    name: string;
    age: number;
  }

  type Adult = {
    old: boolean;
  }

  type Teacher = Person & Adult;
  ```
- 타입 별칭의 장점이 더 많은 것 같지만 실제로 웹 서비스를 제작할 때 인터페이스도 많이 사용한다,

<br>

### 2.3. 타입 확장 관점에서 차이
- 타입 확장: 이미 정의되어 있는 타입들을 조합해서 더 큰 의미의 타입을 생성하는 것을 의미한다. 
- 타입 별칭과 인터페이스는 타입을 확장하는 방식이 다르다.
  - 인터페이스 타입 확장
    - 인터페이스는 타입을 확장할 때 상속이라는 개념을 이용한다.
    - extends 키워드를 사용해 부모 인터페이스의 타입을 자식 인터페이스에서 상속해서 사용할 수 있다.
  ```ts
  interface Person {
    name: string;
    age: number;
  }

  interface Developer extends Person {
    skill: string;
  }

  const chaebeen: Developer = {
    name: 'chaebeen',
    age: 20,
    skill: '웹개발'
  };
  ```
  - 타입 별칭
    - 인터섹션 타입으로 객체 타입을 2개 합쳐서 사용할 수 있다.
  ```ts
  type Person = {
    name: string;
    age: number;
  }

  type Developer = {
    skill: string;
  } 

  const chaebeen: Person & Developer = {
    name: 'chaebeen',
    age: 20,
    skill: '웹개발'
  };
  ```
  - 인터섹션 타입을 별도의 타입 별칭으로 정의하여 사용할 수도 있다.
  ```ts
  type Person = {
    name: string;
    age: number;
  }

  type Developer = {
    skill: string;
  } 

  type Chaebeen = Person & Developer;

  const chaebeen: Chaebeen = {
    name: 'chaebeen',
    age: 20,
    skill: '웹개발'
  };
  ```
- 작성된 타입을 어떻게 조합하느냐에 따라 인터페이스를 사용하기도 하고 타입 별칭을 사용하기도 한다.
- **선언 병합(declaration merging)**: 인터페이스의 선언 병합은 동일한 이름으로 인터페이스를 여러 번 선언했을 때 해당 인터페이스의 타입 내용을 합치는 것을 말한다.
  - 인터페이스는 동일한 이름으로 선언하면 인터페이스 내용을 합치는 특성이 있다.
  ```ts
  interface Person {
    name: string;
    age: number;
  }

  interface Person {
    address: string;
  }

  const chaebeen: Person = {
    name: 'chaebeen',
    age: 20,
    address: 'suwon'
  };
  ```

<br>
<br>

## 3. 타입 별칭은 언제 쓰는 것이 좋을까?
- 2021년 이전의 타입 스크립트 공식 문서에서는 '좋은 소프트웨어는 확장이 용이해야 한다.'는 관점에서 타입 별칭보다 인터페이스의 사용을 권장했다.
- 2023년 현재의 공식 문서에서는 이 내용이 없다.
  - 소프트웨어 설계 원칙을 근거로 내세우지 않고 일단 인터페이스를 주로 사용해보고 타입 별칭이 필요할 때 타입 별칭을 쓰라고 안내한다.
- 타입 별칭으로만 타입 정의가 가능한 곳에서 타입 별칭을 사용하고, 백엔드와의 인터페이스를 정의하는 곳에서는 인터페이스를 이용한다.

<br>

### 3.1. 타입 별칭으로만 정의할 수 있는 타입들
- 인터페이스가 아닌 타입 별칭으로만 정의할 수 있는 타입
  - 주요 데이터 타입, 인터섹션 타입, 유니언 타입
  ```ts
  type MyString = string;
  type StringOrNumber = string | number;
  type Admin = Person & Developer
  ```
  - 제네릭, 유틸리티 타입, 맵드 타입과도 연동하여 사용할 수 있다.
    - 제네릭은 인터페이스와 타입 별칭에 모두 사용할 수 있다.
    - 유틸리티 타입이나 맵트 타입은 타입 별칭으로만 정의할 수 있기 때문에 인터페이스에서 사용할 수 없다.
  ```ts
  // 제네릭
  type Dropdown<T> = {
    id: string;
    title: T;
  }

  // 유틸리티 타입
  type Admin = { name: string, age: number; role: string; }
  type OnlyName = Pick<Admin, 'name'>

  // 맵드 타입
  type Pricker<T, K extends keyof T> = {
    [P in K]: T[P];
  };
  ```
- 인터페이스는 객체 타입을 정의할 때 사용한다.

<br>

### 3.2. 백엔드와의 인터페이스 정의
- 백엔드에서 프론트엔드로 데이터를 어떻게 넘길지 정의하는 작업이 필요한데 이 작업을 인터페이스를 정의한다고 말한다.
  - 여기서 인터페이스는 타입스크립트의 인터페이스가 아니라 영역 간 접점(데이터)을 서로 맞추는 작업을 의미한다.
  - 이 데이터를 정의하면서 프론트엔드에서는 서버에 데이터를 요청받고 받은 결과를 화면에서 처리할 수 있도록 API 함수를 설계한다.
- 자바스크립트로 API 함수를 구현
  - 자바스크립트에서 API 함수를 구현할 때는 JSDoc으로 함수를 명세하기도 한다.
  ```js
  /**
   * @typedef {Object} User
   * @property {string} id - 사용자 아이디
   * @property {string} name - 사용자 이름
   */

   /**
    * @returns {User} 1번 사용자
    */
    function fetchData() {
      return axios.get('http://localhost:3000/users/1');
    }
  ```
  - 첫 번째 사용자 정보를 받아 오는 API 함수의 결과를 JSDoc으로 명세한다.
  - API 함수의 결과는 id와 name 속성을 갖는 사용자 객체이다.
- 타입스크립트로 API 함수 구현  
  - 타입 별칭으로 API 함수의 응답 형태를 정의
    ```ts
    type User = {
      id: string;
      name: string;
    }

    function fetchData(): User {
      return axios.get('http://localhost:3000/users/1');
    }
    ```
  - 인터페이스로 API 함수의 응답 형태를 정의
    ```ts
    interface User = {
      id: string;
      name: string;
    }

    function fetchData(): User {
      return axios.get('http://localhost:3000/users/1');
    }
    ```
  - 타입스크립트에서 API 함수를 구현할 때는 타입 별칭보다 인터페이스를 이용하는 것이 더 좋다.
    - 객체의 속성에 다른 속성들이 추가되거나 다른 객체 정보와 결합하여 표시되어야 한다면 기존 타입의 확장이라는 측면에서 인터페이스로 정의하는 것이 더 수월하다.
    ```ts
    interface Admin = {
      role: string;
      department: string;
    }

    // 상속을 통한 인터페이스 확장
    interface User extends Admin {
      id: string;
      name: string;
    }

    // 선언 병합을 통한 타입 확장
    interface User = {
      skill: string;
    }
    ```
    ```ts
    interface User = {
      id: string;
      name: string;
      role: string;
      department: string;
      skill: string;
    }
    ```