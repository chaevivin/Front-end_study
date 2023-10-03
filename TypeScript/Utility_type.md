# 유틸리티 타입
- 유틸리티 타입은 이미 정의된 타입을 변형하여 사용할 때 유용하게 쓰인다. 

<br>
<br>

## 1. 유틸리티 타입이란?
- **유틸리티 타입(utility type)**: 이미 정의되어 있는 타입 구조를 변경하여 재사용하고 싶을 때 사용하는 타입이다.
- 타입스크립트에 내장된 타입이기 때문에 타입스크립트 설치 후 설정 파일의 lib 속성을 변경만 하면 사용할 수 있다.
  ```ts
  {
    "compilerOptions": {
      "lib": ["ESNext"]
    }
  }
  ```
  - `compilerOptions` 속성에 lib 속성을 추가한다.
  - lib 속성은 타입스크립트에서 미리 정의해 놓은 타입 선언 파일을 사용할 때 쓰는 옵션이다.
  - `ESNext`는 최신 자바스크립트 문법을 의미한다.

<br>
<br>

## 2. Pick 유틸리티 타입

<br>

### 2.1. Pick 타입 문법
- Pick 타입 문법을 다음과 같다.
  ```
  Pick<대상 타입, '대상 타입 속성 이름'>
  Pick<대상 타입, '대상 타입 속성 1 이름' | '대상 타입 속성 2 이름'>
  ```
  - 속성을 추출할 타입을 괄호의 첫 번째에 넣고 콤마를 찍은 후 두 번째 위치에 추출할 대상 타입의 속성 이름을 하나만 적거나 유니언 타입으로 여러 개를 적어도 된다.

<br>

### 2.2. Pick 타입 예시
- Pick 유틸리티 타입은 특정 타입의 속성을 뽑아서 새로운 타입을 만들어 낼 때 사용한다.
  ```ts
  interface Profile {
    id: string;
    address: string;
  }

  type ProfileId = Pick<Profile, 'id'>;
  ```
  - id와 address 속성을 갖는 인터페이스를 하나 선언하고 이 인터페이스의 id 속성을 뽑아 ProfileId라는 새로운 타입을 정의한다.
  - ProfileId는 마치 다음과 같이 정의된 것처럼 보인다.
    ```ts
    type ProfileId = {
      id: string;
    };
    ```
  - Pick이라는 유틸리티 타입을 사용하면 타입 별칭을 정의한 것과 동일한 효과가 나타난다.
- Pick 타입으로 속성을 여러 개 추출해서 타입을 정의할 수 있다.
  ```ts
  interface UserProfile {
    id: string;
    name: string;
    address: string;
  }

  type MyProfile = Pick<UserProfile, 'id' | 'name'>;

  const me: MyProfile = {
    id: '1', 
    name: 'dove',
  };
  ```
  - UserProfile 인터페이스에서 Pick으로 id와 name 속성을 뽑아 MyProfile 타입으로 정의하였다.
  - MyProfile은 마치 다음과 같이 정의된 것처럼 보인다.
    ```ts
    type MyProfile = {
      id: string;
      name: string;
    }
    ```

<br>
<br>

## 3. Omit 유틸리티 타입
- Omit 타입은 특정 타입에서 속성 몇 개를 제외한 나머지 속성으로 새로운 타입을 생성할 때 사용하는 유틸리티 타입이다.

<br>

### 3.1. Omit 타입 문법
- Omit 타입의 문법은 다음과 같다.
  ```
  Omit<대상 타입, '대상 타입의 속성 이름'>
  Omit<대상 타입, '대상 타입의 속성 1 이름' | '대상 타입의 속성 2 이름'>
  ```
  - Omit을 선언한 후 첫 번째 제네릭 타입에 대상 타입을 넘기고, 두 번째 제네릭 타입으로 제외할 속성 이름을 문자열 타입 또는 문자열 유니언 타입으로 선언해 주면 된다.

<br>

### 3.2. Omit 타입 예시
- Omit 타입
  ```ts
  interface UserProfile {
    id: string;
    name: string;
    address: string;
  }

  type User = Omit<UserProfile, 'address'>;
  ```
  - id, name, address 속성을 갖는 인터페이스를 하나 선언하고, address 속성을 제외한 나머지 속성들을 Omit 타입으로 새롭게 선언하였다.
  - UserProfile은 마치 다음과 같이 정의된 것처럼 보인다.
    ```ts
    type User = {
      id: string;
      name: string;
    };
    ```

<br>

### 3.3. Omit 타입과 Pick 타입 비교
- Omit 타입과 Pick 타입은 정확히 반대의 역할을 한다.
  ```ts
  interface UserProfile {
    id: string;
    name: string;
    address: string;
  }

  type User1 = Omit<UserProfile, 'address'>;
  type User2 = Pick<UserProfile, 'id' | 'name'>;
  ```
  - User1과 User2 타입은 타입 구조가 같다.
  - 어떤 유틸리티 타입을 쓸 지는 개인의 취향이나 선호도에 따라 정할 수 있지만 이때는 Omit 타입을 사용하는 것이 좋다.
    - 이유: 두 번째 제네릭 타입에 속성 이름을 여러 개 넣지 않고 1개만 넣어도 되기 때문이다.
  - 가급적이면 코드 양을 줄일 수 있는 유틸리티 타입을 선택하는 것이 좋다.

<br>
<br>

## 4. Partial 유틸리티 타입
- Partial 타입은 특정 타입의 모든 속성을 모두 옵션 속성으로 변환한 타입을 생성해 준다.
- 주로 HTTP PUT처럼 데이터를 수정하는 **REST API**를 전송할 때 종종 사용되는 타입이다.

<br>

### 4.1. Partial 타입 문법
- Partial 타입은 Pick 타입, Omit 타입과는 다르게 대상 타입만 넘기면 된다.
  ```
  Partial<대상 타입>
  ```
- 객체 형태의 타입만 대상 타입으로 취급할 수 있다.
  ```ts
  interface Todo {
    id: string;
    title: string;
  }

  type OptionalTodo = Partial<Todo>
  ```
  - id와 title 속성을 가진 Todo 인터페이스를 선언하고 Partial 유틸리티 타입을 적용하였다.
  - OptionalTodo는 마치 다음과 같이 정의된 것처럼 보인다.
    ```ts
    type OptionalTodo = {
      id?: string;
      title?: string;
    }
    ```
    - OptionalTodo 타입을 이용하면 id와 title 속성을 선택적으로 적용하여 객체를 생성할 수 있다.
  - id와 title 속성이 모두 옵션 속성이기 때문에 빈 객체, id나 title 속성이 하나씩만 들어간 ㄴ객체, 둘 다 모두 들어간 객체를 선언할 수 있다.
    ```ts
    const nothing: OptionalTodo = {};
    const onlyId: OptionalTodo = { id: 'id' };
    const onlyTitle: OptionalTodo = { title: 'title' };
    const todo: OptionalTodo = { id: '1', title: 'ts' };
    ```

<br>

### 4.2. Partial 타입 예시
- Partial 타입은 특정 타입의 속성을 모두 선택적으로 사용할 수 있으므로 보통 데이터 수정 API를 다룰 때 사용한다. 
  ```ts
  interface Todo {
    id: string;
    title: string;
    checked: boolean;
  }

  function updateTodo(todo: Todo) {
    // ...
  }
  ```
  - id, title, checked 속성을 가진 Todo 인터페이스를 선언하였다.
  - updateTodo() 함수는 할 일 정보를 변경하여 서버에 전달해 주는 함수이다.
  - 서버 쪽에서는 id, title, checked 속성 중 변경된 속성만 넘겨 달라고 할 수도 있고, 데이터 전체를 넘겨 달라고 할 수도 있다. 
    ```ts
    // id 속성만 넘기는 경우
    function updateTodo(todo: { id: string }) {
      // ...
    }

    // id와 checked 속성만 넘기는 경우
    function updateTodo(todo: { id: string; checked: string }) {
      // ...
    }

    // 할 일 데이터에 정의된 값을 모두 넘기는 경우
     function updateTodo(todo: { id: string; checked: string; title: string }) {
      // ...
    }
    ```
    - 위 처럼 작성하면 Todo 인터페이스의 타입 코드를 재활용하지 않고 일일이 정의해야 한다.
    ```ts
    // id 속성만 넘기는 경우
    function updateTodo(todo: Pick<Todo, 'id'>) {
      // ...
    }

    // id와 checked 속성만 넘기는 경우
    function updateTodo(todo: Omit<Todo, 'title'>) {
      // ...
    }

     // 할 일 데이터에 정의된 값을 모두 넘기는 경우
     function updateTodo(todo: Todo) {
      // ...
    }
    ```
    - Pick이나 Omit 타입보다는 Partial 타입을 사용하면 이 세 가지 케이스를 모두 만족시킬 수 있다.
    ```ts
    function updateTodo(todo: Partial<Todo>) {
      // ...
    }
    ```
    - `Partial<Todo>`는 id, title, checked 속성이 모두 옵션 속성으로 변경되기 때문에 함수의 인자로 다양한 형태의 값을 넘길 수 있다.
- Partial 타입은 특정 타입의 속성을 모두 옵션 속성으로 변경해 준다. 따라서 데이터를 수정하는 API를 호출하거나 이미 정해진 데이터 타입을 다른 곳에서 선택적으로 재사용할 때 주로 쓰인다.

<br>
<br>

## 5. Exclude 유틸리티 타입
- Exclude 타입은 유니언 타입을 구성하는 특정 타입을 제외할 때 사용하다.
- Pick, Omit, Partial 타입은 객체 타입의 형태를 변형하여 새로운 객체 타입을 만드는 반면, Exclude 타입은 유니언 타입을 변형한다.

<br>

### 5.1. Exclude 타입 문법
- Exclude 타입은 첫 번째 제네릭 타입에 변형할 유니언 타입을 넣고, 두 번째 제네릭 타입으로 제외할 타입 이름을 문자열 타입으로 적거나 문자열 유니언 타입으로 넣는다.
  ```
  Exclude<대상 유니언 타입, '제거할 타입 이름'>
  Exclude<대상 유니언 타입, '제거할 타입 이름 1' | '제거할 타입 이름 2'> 
  ```

<br>

### 5.2. Exclude 타입 예시
- Exclude 타입
  ```ts
  type Languages = 'C' | 'Java' | 'TypeScript' | 'React';
  type TrueLanguages = Exclude<Languages, 'React'>;
  ```
  - 문자열 타입을 유니언 타입으로 갖는 Languages 타입에 Exclude 유틸리티 타입을 적용하였다.
  - TrueLanguages는 마치 다음과 같이 정의된 것처럼 보인다.
    ```ts
    type TrueLanguages = 'C' | 'Java' | 'TypeScript';
    ```
- 제외할 타입을 하나가 아니라 여러 개 넘길 수 있다.
  ```ts
  type Languages = 'C' | 'Java' | 'TypeScript' | 'React';
  type WebLanguage = Exclude<Languages, 'C' | 'Java' | 'React'>;
  ```
  - WebLanguages는 마치 다음과 같이 정의된 것처럼 보인다.
    ```ts
    type WebLanguage = 'TypeScript';
    ```

<br>
<br>

## 6. Record 유틸리티 타입
- Record 타입은 타입 1개를 속성의 키(key)로 받고 다른 타입 1개를 속성 값(value)으로 받아 객체 타입으로 변환해 준다.
  - 마치 map() API와 역할이 비슷하다.
  - map() 처럼 실제 값을 변경하는 것이 아니라 타입만 변환해 준다.

<br>

### 6.1. Record 타입 문법
- Record 첫 번째 제네릭 타입에는 객체 속성의 키(key)로 사용할 타입을 넘기고, 두 번째 타입에는 객체 속성의 값(value)으로 사용할 타입을 넘긴다.
  ```
  Record<객체 속성의 키로 사용할 타입, 객체 속성의 값으로 사용할 타입>
  ```
  - 첫 번째 제네릭 타입에는 string, number, 유니언, number 유니언 등이 들어갈 수 있다.
  - 두 번째 제네릭 타입에는 아무 타입이나 넣을 수 있다.

<br>

### 6.2. Record 타입 첫 번째 예시
- Record 타입
  ```ts
  type Animations = {
    name: string;
    year: number;
  };
  type AniNames = 'titan' | 'haikyuu' | 'gintama';

  type Anime = Record<AniNames, Animations>;
  ```
  - Record 타입의 첫 번째 제네릭 타입으로 속성의 키 값인 AniNames 타입을 넣고, 두 번째 제네릭 타입에 속성 값이 될 Animations 타입을 넣었다.
  - Anime는 마치 다음과 같이 정의된 것처럼 보인다.
    ```ts
    type Anime = {
      titan: Animations;
      haikyuu: Animations;
      gintama: Animations;
    }
    ```
    - Anime 타입의 형태는 객체고 키 값은 첫 번째 제네릭 타입으로 받았던 AniNames의 문자열 타입 titan, haikyuu, gintama이다. 
    - 속성 값의 타입은 두 번째 제네릭 타입으로 받았던 Animations 타입의 형태를 갖는다.
  - Anime 타입으로 여러 객체를 선언할 수 있다.
    ```ts
    type Anime = Record<AniNames, Animations>;

    const favorite: Anime = {
      titan: {
        name: '진격의 거인',
        year: 2009
      },
      haikyuu: {
        name: '하이큐',
        year: 2012
      },
      gintama: {
        name: '은혼',
        year: 2003
      }
    };
    ```

<br>

### 6.3. Record 타입의 두 번째 
- Record 타입의 입력 값을 좀 더 단순한 형태의 데이터 타입으로 넣어도 된다.
  ```ts
  type PhoneBook = Record<string, string>;
  ```
  - Record 타입의 첫 번째와 두 번째 제네릭 타입으로 모두 문자열을 넘겨서 PhoneBook이라는 타입을 생성한다.
  - PhoneBook은 마치 다음과 같이 정의된 것처럼 보인다.
    ```ts
    type PhoneBook = {
      [x: string]: string;
    }
    ```
    - 문자열 키를 여러 개 정의할 수 있다.
    - 키와 값 모두 문자열로 선언한다는 것을 의미한다.
  - 인덱스 시그니처로 정의되었기 때문에 키, 값을 여러개 넣거나 하나만 정의해도 된다.
    ```ts
    const myPhone: PhoneBook = {
      me: '010-1111-2222',
    };

    const companyPhones: PhoneBook = {
      ceo: '010-0000-0000',
      hr: '010-3333-3333',
      engineering: '010-8888-9999'
    };
    ```

<br>
<br>