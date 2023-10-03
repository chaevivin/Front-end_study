# 맵드 타입
- **맵드 타입(mapped type)**: 이미 정의된 타입을 가지고 새로운 타입을 생성할 때 사용하는 타입 문법을 의미한다.
- 유틸리티 타입은 내부적으로 맵드 타입을 이용해서 구현되었다.

<br>
<br>

## 1. 맵드 타입 첫 번째 예시: in
```ts
type AniNames = 'titan' | 'haikyuu' | 'gintama';
type AniYears = {
  [year in AniNames]: number;
};
```
위의 코드를 살펴보면,
- AniNames는 3개의 유니언 타입에 맵드 타입 문법을 적용하여 AniYears라는 새로운 타입을 정의하였다.
- AniYears 타입은 애니메이션 이름을 속성 이름(key)으로 하고 애니메이션의 연재 시작연도를 속성 값(value)으로 정의한다.
- AniYears는 마치 다음과 같이 정의된 것처럼 보인다.
  ```ts
  type AniYears = {
    titan: number;
    haikyuu: number;
    gintama: number;
  }
  ```

<br>

- 이미 정의된 타입으로 새로운 타입을 생성하려면 `[year in AniNames]` 형태의 문법을 사용해야 한다.
- 자바스크립트의 for...in 반복문을 사용하는 것처럼 AniNames에 있는 타입을 하나씩 순회하여 속성 값 타입과 연결한 것과 같다.
  - for...in 반복문은 객체의 키, 값 쌍을 순회할 때 흔히 사용되는 문법이다.
  ```js
  const obj = { a: 10, b: 20, c: 30 };
  for (const key in obj) {
    console.log(key);    // a, b, c 순서대로 출력
  }
  ```
  - 객체의 키가 3개이므로 반복문은 세 번 순회한다.

<br>
<br>

## 2. map() API로 이해하는 맵드 타입
- map() API는 자바스크립트 배열에서 사용할 수 있는 내장 API이다.
  - map() API는 특정 배열의 각 요소를 변환하여 새로운 배열로 만들어준다.
  ```ts
  const arr = [1, 2, 3];
  const doubledArr = arr.map(function(num) {
    return num * 2;
  });
  
  console.log(doubledArr);    // [2, 4, 6]
  ```
  - arr 배열에 map() API를 적용하여 새로운 doubledArr 배열을 생성하였다.
  - map() API는 인자로 함수를 하나 넘기고 각 요소를 어떻게 변환할지 정의한다.
  - map() API의 특징은 기존 배열 값을 변경하지 않고 새로운 배열을 생성한다는 것이다.
- 맵드 타입을 map() API로 비슷하게 구현
  ```ts
  const anime = ['titan', 'haikyuu', 'gintama'];
  const aniYears = anime.map(function(ani) {
    return {
      [ani]: 2000
    }
  });
  ```
  - anime 배열에 map() API를 사용하여 이름을 키로 갖고 연도를 값을 갖는 객체로 반환하였다.
  - 새로 정의된 aniYears은 마치 다음과 같이 정의된 것처럼 보인다.
    ```ts
    const aniYears = [
      { titan: 2000 },
      { haikyuu: 2000 },
      { gintama: 2000 }
    ];
    ```
- 맵드 타입이나 map() API나 기존에 정의된 타입 또는 값을 반환하여 새로운 타입 또는 값을 생성한다는 점에서 성격이 동일하다.

<br>
<br>

## 3. 맵드 타입 두 번째 예시: keyof
```ts
interface Animation {
  name: string;
  year: number;
}

type AniPropCheck = {
  [A in keyof Animation]: boolean;
};
```
위의 코드를 살펴보면,
- name과 year 속성을 갖는 Animation 인터페이스에 맵드 타입을 적용하여 각 속성의 유무를 나타내는 AniPropCheck 타입을 선언하였다.
- AniPropCheck는 마치 다음과 같이 정의된 것처럼 보인다.
  ```ts
  type AniPropCheck = {
    name: boolean;
    skill: boolean;
  }
  ```
  - Animation 인터페이스의 속성들 타입은 원래 string과 number이었는데 맵드 타입을 적용하여 마치 boolean으로 바뀐 것과 같은 효과가 나타난다.
  - 맵드 타입을 적용했기 때문에 Animation 인터페이스 타입 정의를 바꾸는 것이 아니라 AniPropCheck라는 새로운 타입을 생성한다.
- `keyof`는 특정 타입의 키 값만 모아 문자열 유니언 타입으로 변환해 주는 키워드이다.
  ```ts
  interface Animation {
    name: string;
    year: number;
  }

  type AniNames1 = keyof Animation;
  type AniNames2 = 'name' | 'year';
  ```
  - AniNames1과 AniNames2 타입은 같은 타입이다.
  - Animation 인터페이스의 키인 name과 year 속성은 문자열 유니언 타입으로 변환한 타입이 AniNames1이다.
  - AniNames2 타입은 AniNames1 타입의 결고를 눈으로 더 쉽게 확인할 수 있도록 keyof Animation의 결과를 일일이 나열하였다.
- AniPropCheck는 다음과 같이 쓸 수도 있다.
  ```ts
  type AniPropCheck = {
    [A in 'name' | 'year']: boolean;
  };
  ```
  - `keyof Animation`을 좀 더 보기 쉽게 문자열 유니언 타입을 풀어 썼다.
- 맵드 타입을 이용하면 유니언 타입을 이용하여 객체 형태의 타입으로 변환할 수 있고, 객체 형태의 타입에서 일부 타입 정의만 변경한 새로운 객체 타입을 정의할 수 있다.

<br>
<br>

## 4. 맵드 타입을 사용할 때 주의할 점
- 인덱스 시그니처 문법 안에서 사용하는 in 앞의 타입 이름은 마음대로 지을 수 있다.
  ```ts
  type AniNames = 'titan' | 'haikyuu' | 'gintama';
  type AniYears = {
    [Name in AniNames]: number;
  };

  // 1
  type AniYears = {
    [aniName in AniNames]: number;
  };
  // 2
  type AniYears = {
    [name in AniNames]: number;
  };
  ```
  - in 앞에 오는 타입 변수는 순회할 타입 변수이므로 마음대로 작명할 수 있지만 최대한 역할을 나타낼 수 있는 이름으로 짓는 것이 좋다.
- 맵드 타입의 대상이 되는 타입 유형
  - 인터페이스와 타입 별칭으로 정의된 타입 모두 맵드 타입으로 변환할 수 있다.
    ```ts
    // 인터페이스 타입으로 맵드 타입 생성
    interface Animation {
      name: string;
      year: number;
    }

    type AniPropCheck = {
      [A in keyof Animation]: boolean;
    };

    // 타입 별칭으로 맵드 타입 생성
    type Animation = {
      name: string;
      year: number;
    }

    type AniPropCheck = {
      [A in keyof Animation]: boolean;
    };
    ```
  - string 타입에 맵드 타입 문법을 적용하여 새로운 타입을 생성할 수도 있다.
    ```ts
    type UserName = string;
    type AddressBook = {
      [U in UserName]: number;
    };
    ```
    - UserName이라는 string 타입을 이용하여 AddressBook이라는 타입을 생성하였다.
    - AddressBook은 마치 다음과 같이 정의된 것처럼 보인다.
      ```ts
      type AddressBook = {
        [x: string]: number;
      }
      ```
    - AddressBook 타입으로 정의된 객체의 속성 키에는 어떤 문자열이든 들어갈 수 있고 속성 값만 number 타입이 되면 된다.
      ```ts
      const titanAddress: AddressBook = {
        levi: 12341234,
        eren: 987654321
      };
      ```
  - 객체의 속성 이름(key)은 문자, 숫자 등으로 선언할 수 있고 boolean 타입으로 선언할 수 없다.
    ```ts
    type Login = boolean;
    type LoginAuth = {
      [L in Login]: string;    // Error
    };
    ```

<br>
<br>

## 5. 매핑 수정자
- **매핑 수정자(mapping modifier)**: 맵드 타입으로 타입을 변환할 때 속성 성질을 변환할 수 있도록 도와주는 문법이다.
- 매핑 수정자에는 `+`, `-`, `?`, `readonly` 등이 있다.
- `?` 매핑 수정자
  ```ts
  type Animation = {
    name: string;
    year: number;
  };

  type AniOptional = {
    [A in keyof Animation]?: string;
  };
  ```
  - Animation 타입에 맵드 타입과 매핑 수정자를 적용하여 Animation 속성을 모두 옵션 속성으로 변환한다.
  - 속성 이름 선언 부분에 `?`를 붙여서 속성을 모두 선택적으로 사용할 수 있는 옵션 속성으로 변환하였다.
  - AniOptional은 마치 다음과 같이 정의된 것처럼 보인다.
    ```ts
    type AniOptional = {
      name?: string;
      year?: number;
    }
    ```
- `-` 매핑 수정자
  - `-`매핑 수정자를 사용하면 옵션 속성 `?`나 `readonly` 등 일반 속성 이외에 추가된 성질을 모두 제거할 수 있다.
  ```ts
  type AniOptional = {
    name?: string;
    year?: number;
  };

  type AniRequired<T> = {
    [Property in keyof T]-?: T[Property];
  };

  const titan: AniRequired<AniOptional> = {
    name: '진격의 거인',
    year: 2000
  };
  ```
  - AniRequired 타입은 제네릭을 받고 제네릭으로 받은 타입을 이용하여 맵드 타입으로 변환해 주면서 속성 부분에 `-?`를 붙여 주었다. 이는 제네릭 타입으로 받은 속성의 옵션 속성을 모두 제거하겠다는 의미이다.
  - 속성 선언 부분에 타입 변수 이름을 Property로 짓고 속성 값의 타입을 T[Property]로 정의하여 제네릭으로 넘겨받은 타입의 속성 이름과 속성 값 타입이 그대로 연결되도록 선언하였다.
  - `AniRequired<AniOptional>` 타입의 속성이 모두 필수 값인지 확인하기 위해 titan 변수의 내용을 전부 지우면 에러가 발생한다.
  
<br>
<br>

## 6. 맵드 타입으로 직접 유틸리티 타입 만들기
- Partial 타입 직접 구현
  ```ts
  interface Todo {
    id: string;
    title: string;
  }

  type MyPartial = {
    [Property in keyof Todo]?: Todo[Property];
  };
  ```
  - MyPartial 타입 코드를 보면 Todo 인터페이스의 키 속성을 모두 순회하면서 각 속성을 모두 `?`를 이용해서 옵션 속성으로 변환하였다.
  - 순회간 각 속성의 데이터 타입은 변경하지 않고 `Todo<Property>`로 그대로 연결해 주었다.
  - MyPartial은 다음과 같이 정의된 것처럼 보인다.
    ```ts
    type MyPartial = {
      id?: string;
      title?: string;
    }
    ```
  - 다른 객체 타입에 Partial 타입 효괴를 적용하려면 또 다시 구현해야 한다.
    ```ts
    // Person 타입
    interface Person {
      name: string;
      age: number;
    }

    type PersonPartial = {
      [Property in keyof Person]?: Person[Property];
    };

    // Hero 타입
    type Hero = {
      name: string;
      skill: string;
    }

    type HeroPartial = {
      [Property in keyof Hero]?: Hero[Property];
    }
    ```
  - 특정 객체 타입에 대해 일일이 Partial 타입 역할을 하는 코드를 중복해서 작성하는 것보다, 어떤 타입이 오든 Partial 타입의 효과를 동일하게 적용할 수 있다.
    ```ts
    type MyPartial<Type> = {
      [Property in keyof Type]?: Type[Property];
    };
    ```
    - MyPartial 타입에 제네릭 타입을 받을 수 있도록 `<Type>`을 추가하였다.
    - 제네릭으로 넘겨받은 타입의 속성을 모두 옵션 속성으로 변환해 준다.
    - 어떤 타입이 오더라도 동일하게 Partial 효과를 적용할 수 있다.
      ```ts
      type TodoPartial = MyPartial<Todo>;
      type PersonPartial = MyPartial<Person>;
      type HeroPartial = MyPartial<Hero>;
      ```
  - Property와 Type을 모두 축약어 P, T로 줄일 수 있다.
    ```ts
    type MyPartial<Type> = {
      [Property in keyof Type]?: Type[Property];
    };

    type MyPartial<T> = {
      [P in keyof T]?: T[P];
    };
    ```
- 맵드 타입과 매핑 수정자를 이용하면 타입스크립트의 내장 유틸리티 타입을 쉽게 구현할 수 있다.

<br>
<br>