# 타입 모듈
- 모듈은 수많은 함수와 변수, 클래스 등을 논리적인 단위로 구분하여 필요할 때 가져다 쓸 수 있다.

<br>
<br>

## 1. 모듈이란?
- **모듈(module)**: 프로그래밍 관점에서 특정 기능을 갖는 작은 단위의 코드를 의미한다. 
- 수십 개의 파일과 많은 수의 코드 라인을 갖고 있다면 모듈이 필요하다.
  - 코드는 역할과 목적에 따라 구분되어 있는 것이 좋다.
  - 자바스크립트에서는 파일 목적에 따라 변수, 함수 등을 배치할 수 있다. 
  - 하나의 모듈에는 외부에서 가져다 쓸 수 있는 기능들이 있다.

<br>
<br>

## 2. 자바스크립트 모듈

<br>

### 2.1. 자바스크립트의 태생적 한계
- 자바스크립트는 태생적으로 모듈이라는 개념이 없던 프로그래밍 언어이다. 
  - 파일 단위로 변수나 함수가 구분되지 않았다.
- 자바스크립트는 파일 단위로 변수나 함수를 구분해도 기본적으로 모두 **전역 유효 범위(global scope)**를 갖는다.
  - 전역 유효 범위는 이름이 서로 충돌하지 않게 유일한 변수나 함수 이름을 고민해야 한다.
  - 애플리케이션의 규모가 커지고 코드가 많아질수록 이 문제점이 더욱 부각된다.

<br>

### 2.2. 자바스크립트 모듈화를 위한 시도들
- 자바스크립트 모듈화를 위해 Common.js와 Require.js가 도입되었다.
- Common.js
  - 브라우저뿐만 아니라 브라우저 이외의 환경인 서버, 데스크톱에서도 자바스크립트를 활용하고 고안된 스펙이자 그룹이다.
  - 서버 런타임 환경인 Node.js에서 가장 활발하게 사용되고 있는 스펙이다.
    - Node.js가 설치되어 있다면 별도의 도구나 라이브러리 없이도 문법을 사용하여 자바스크립트 모듈화를 실현할 수 있다.
  ```js
  // math.js
  function sum(a, b) {
    return a + b;
  }

  module.exports = {
    sum
  };

  // app.js
  const math = require('./math.js');

  console.log(math, sum(10, 20));    // 30
  ```
  - Common.js 문법을 이용하여 math.js를 모듈화하였다.
- Require.js
  - AMD(Asynchronous Module Definition)이라는 비동기 모듈 정의 그룹에서 고안된 라이브러리 중 하나이다.
  - 비동기 모듈: 애플리케이션이 시작되었을 때 모든 모듈을 가져오는 것이 아니라 필요할 때 순차적으로 해당 모듈을 가져온다는 의미이다.
  ```html
  <body>
    <!-- 라이브러리 파일 다운로드 후 다음과 같이 연결 -->
    <script src="require.js"></script>
    <script>
      require(["https://unpkg.com/vue@3/dist/vue.global.js"], function () {
        console.log('vue is loaded')
      });
    </script>
  </body>
  ```
  - Require.js 라이브러리를 내려받은 후 script 태그를 이용하여 로딩하고 require() 문법으로 외부 라이브러리를 가져온다. 

<br>
<br>

## 3. 자바스크립트 모듈화 문법

<br>

### 3.1. import, export
- ES6(ECMAScript 2015)부터 import와 export 문법을 지원한다.
  ```js
  // math.js
  function sum(a, b) {
    return a + b;
  }

  export { sum } 
  ```
  - math.js 파일에 두 수의 합을 구하는 sum() 함수를 선언하고 export로 해당 함수를 모듈화했다.
  - export로 함수를 지정했기 때문에 다른 파일에서 import로 함수를 불러와 사용할 수 있다.
  ```js
  // app.js
  import { sum } from './math.js';
  console.log(sum(10, 20));    // 30
  ```
  - app.js라는 파일에서 math.js의 sum() 함수를 가져와 사용한다.

<br>

### 3.2. export default 문법
- export에 default를 붙이면 해당 파일에서 하나의 대상만 내보내겠다는 말과 같다.
  ```js
   // math.js
  function sum(a, b) {
    return a + b;
  }

  export default sum;

  // app.js
  import sum from './math.js';
  console.log(sum(10, 20));    // 30
  ```
  - `export default sum`을 하면 import 구문으로 sum() 함수 하나만 가져올 수 있다.
- import 구문에 {} 를 붙이지 않아도 된다.
  ```js
  // default를 사용하지 않았을 때
  import { sum } from './math.js';

  // default를 사용했을 때
  import sum from './math.js';
  ```

<br>

### 3. import as 문법
- import에 as 키워드를 이용하면 가져온 변수나 함수의 이름을 해당 모듈 내에서 변경하여 사용할 수 있다.
  ```js
   // math.js
  function sum(a, b) {
    return a + b;
  }

  export default { sum };

  // app.js
  import { sum as add } from './math.js';
  console.log(add(10, 20));    // 30
  ```
  - math.js 파일에서는 sum() 함수를 내보내고, app.js 파일에서는 이 함수를 가져와 add로 이름을 변경하였다.
  - `import { sum as add }`는 sum 함수를 가져와 이 파일 안에서는 add라는 이름을 쓰겠다는 의미이다.
  - sum() 함수의 이름을 app.js 파일에서 add()로 바꾸어 사용했을 뿐, sum() 함수 이름 자체가 바뀌는 것이 아니다.

<br>

### 3.4. import * 문법
- 특정 파일에서 내보낸 기능이 많아 import 구문으로 가져와야 할 것이 많다면 * 키워드를 사용하여 편리하게 가져올 수 있다.
  ```js
  // math.js
  function sum(a, b) {
    return a + b;
  }

  function substract(a, b) {
    return a - b;
  }

  function divide(a, b) {
    return a / b;
  }

  export { sum, substract, divide }

  // app.js
  import * as myMath from './math.js'
  console.log(myMath.sum(10, 20));    // 30
  console.log(myMath.substract(30, 10));    // 20
  console.log(myMath.divide(4, 2));    // 2
  ```
  - math.js 파일에서 3개의 함수를 내보낸 후 app.js 파일에서 `* as myMath`라는 문법으로 모두 가져와 사용한다.
  - `import * as myMath from './math.js'`
    - math.js 파일에서 export 키워드로 지정한 모든 변수와 함수를 myMath라는 이름을 붙여 사용하겠다는 의미이다.
    - myMath를 네임스페이스(namespace)라고 보거나 아래의 객체처럼 생각할 수 있다.
      ```js
      const myMath = {
        sum: function() {
          // ...
        },
        substract: function() {
          // ...
        },
        divide: function() {
          // ...
        }
      };
      ```
- 익스포트할 대상이 많거나 별도의 네임스페이스를 지정하여 사용하고 싶을 때는 `*` 키워드를 사용한다.

<br>

### 3.5. export 위치
- export는 특정 파일에서 다른 파일이 가져다 쓸 기능을 내보낼 때 사용하는 키워드이다.
- 변수, 함수, 클래스에 모두 사용할 수 있다.
  ```js
  const pi = 3.14;
  const getHi = () => {
    return 'hi'
  };
  class Person {
    // ...
  }

  export { pi, getHi, Person }
  ```
  - 변수, 함수, 클래스를 선언하고 export로 다른 모듈에서 사용할 수 있게 내보낸다.
  - 파일의 맨 마지막 줄에 export로 내보낼 대상을 정의하는 것이 관례이다.
  - 내보낼 대상 앞에 바로 export를 붙여도 된다.
  ```js
  export const pi = 3.14;
  export const getHi = () => {
    return 'hi'
  };
  export class Person {
    // ...
  }
  ```
  - 두 가지 방법 중 한 가지로 일관되게 코드를 작성하는 것이 좋다.
    - 섞어 쓰면 어떤 모듈이 익스포트 대상인지 분간하기 어렵기 때문이다.

<br>
<br>

## 4. 타입스크립트 모듈
- 타입스크립트 파일에 작성된 변수, 함수, 클래스 등 기능을 import, export 문법으로 내보내거나 가져올 수 있다.
  ```ts
   // math.ts
  function sum(a: number, b: number) {
    return a + b;
  }

  export { sum };

  // app.ts
  import sum from './math';
  console.log(sum(10, 20));    // 30
  ```
- 타입스크립트 모듈을 다룰 때 추가로 알아야 할 점은 타입을 내보내고 가져올 수 있다는 것이다.
  ```ts
  // hero.ts
  interface Hulk {
    name: string;
    skill: string;
  }

  export { Hulk }

  // app.ts
  import { Hulk } from './hero';

  const banner: Hulk = {
    name: 'banner',
    skill: '화내기'
  }
  ```
  - 타입스크립트의 인터페이스, 타입 별칭 등을 내보내어 사용할 수 있다.

<br>
<br>

## 5. 타입스크립트 모듈 유효 범위
- 타입스크립트 모듈을 다룰 때는 변수나 함수의 유효 범위를 알아야 한다.
- 타입스크립트는 자바스크립트와 마찬가지로 변수를 선언할 때 기본적으로 전역 변수로 선언된다.
  ```ts
  // util.ts
  const num = 10;

  // app.ts
  const a = num;
  ```
  - 각각 다른 파일인 util.ts와 app.ts에 변수 num과 a를 선언하고 초깃값을 할당하면 num에는 10이 a에는 num의 값이 할당된다.
  - 다른 파일에 선언된 변수들이 타입스크립트의 모듈 관점에서 전역으로 등록되어 있다. 때문에 같은 이름으로 함수나 타입 별칭 등 재선언이 불가능한 코드를 작성하면 에러가 발생한다.
    ```ts
    // util.ts
    type Person = {
      name: string;
    }

    // app.ts
    const capt: Person = {
      name: '캡틴'
    };

    type Person = {    // Error
      name: string;
      skill: string;
    }
    ```
    - util.ts 파일에서 Person 타입을 선언하고 app.ts에서 Person 타입은 선언하면 에러가 발생한다.
      - 에러 발생 이유: 타입스크립트는 어느 파일에서 변수나 타입을 선언하든 전역 변수로 간주하기 때문에 같은 프로젝트 내에서는 이미 선언된 이름을 사용할 수 없기 때문이다.
      - var나 interface 등 재선언이나 병합 선언이 가능한 코드는 별도로 에러가 표시되지 않는다.
    ```ts
     // util.ts
    interface Person = {
      name: string;
    }

    // app.ts
    const capt: Person = {
      name: '캡틴'
    };

    interface Person = {   
      name: string;
      skill: string;
    }
    ```
    - util.ts와 app.ts에서 똑같은 이름으로 인터페이스를 각각 선언하면 에러가 발생하지 않는다.
      - 에러가 발생하지 않는 이유: 인터페이스는 같은 이름으로 여러 개 선언되면 해당 인터페이스들은 병합되기 때문이다.

<br>
<br>

## 6. 타입스크립트 모듈화 문법

<br>

### 6.1. import type 문법
- 타입 코드를 가져올 때 type이라는 키워드를 추가로 사용할 수 있다.
  ```ts
  // anime.ts
  interface Titan {
    name: string;
    genre: string;
  }

  export { Titan };

  // app.ts
  import type { Titan } from './anime';

  const titans: Titan = {
    name: '진격의 거인',
    genre: '액션'
  };
  ```
  - `import type`으로 anime.ts의 Titan 인터페이스를 가져와 사용하였다.
- 타입을 다른 파일에서 import로 가져오는 경우 import type을 사용하여 타입 코드인지 아닌지 명시할 수 있다.

<br>

### 6.2. import inline type 문법
- `import inline type` 문법은 변수, 함수 등 실제 값으로 쓰는 코드와 타입 코드를 같이 가져올 때 사용할 수 있다.
- 여러 개를 가져올 때 어떤 코드가 타입인지 구분할 수 있다.
  ```ts
  // anime.ts
  interface Titan {
    name: string;
    genre: string;
  }

  function fun() {
    return '';
  };

  const levi = {
    name: '리바이'
  };

  export { Titan, fun, levi };

  // app.ts
  import { type Titan, fun, levi } from './anime';

  const titans: Titan = {
    name: '진격의 거인',
    genre: '액션'
  };
  ```
  - hero.ts 파일에서 인터페이스, 함수, 변수를 모두 export로 내보낸 후 app.ts 파일에서 import 구문으로 가지고 온다.
  - import 구문 3개를 가져오는데, 타입인 경우 앞에 type을 붙여 명시적으로 타입이라는 것을 강조해 준다.
- 한 파일에서 import로 여러 개의 값과 코드를 가져올 때 `import { type }` 형태를 이용하여 가져온 코드가 타입인지 아닌지 명시할 수 있다.

<br>

### 6.3. import와 import.type 중 어떤 문법을 써야 할까?
- import와 import type 중 어떤 것을 써야 할까?
  - **코딩 컨벤션(coding convention)** 에 따라야 한다.
- export만 제대로 지정해 둔다면 import 하는 쪽에서 별도로 파일에 일일이 import 구문을 작성하지 않고 ctrl + space를 사용하여 자동 완성으로 코드를 완성하는 것이 좋다.
  ```ts
  // anime.ts
  interface Titan {
    name: string;
    genre: string;
  }

  export { Titan }

  // app.ts
  const titans: Titan
  ```
  - 타입스크립트 프로젝트에서 모듈을 내보내고 다른 파일에서 이 모듈을 사용할 때 내부적으로 해당 모듈에 대한 정보를 표시해준다.
  - 미리보기 메뉴에서 제시한 '가져오기 추가' 기능을 선택하면 import 코드가 자동으로 추가된다.
    ```ts
    import { Titan } from './anime';
    ```
  - 자동 완성으로 작성된 구문에는 type 키워드가 별도로 붙지 않는다.
- 실무에서 많은 코드가 import 구문 안에서 변수/함수와 타입을 구분하지 않는다. -> 자동 완성이 주는 편리함을 따라간다면 type을 붙이지 않고, 코드 역할을 좀 더 명확하게 하겠다면 type을 붙이면 된다.

<br>
<br>

## 7. 모듈화 전략: Barrel
- **배럴(Barrel)**: 여러 개의 파일에서 가져온 모듈을 마치 하나의 통처럼 관리하는 방시이다.
  - 여러 개의 파일에서 모듈을 정의하여 가져올 때 사용하면 좋은 전략이다.
  ```ts
  // ./anime/titan.ts
  interface Titan {
    name: string;
  }

  export { Titan }

  // ./anime/haikyuu.ts
  interface Haikyuu {
    name: string;
  }

  export { Haikyuu }

  // ./anime/gintama.ts
  interface Gintama {
    name: string;
  }

  export { Gintama }

  // app.ts
  import { Titan } from './anime/titan';
  import { Haikyuu } from './anime/haikyuu';
  import { Gintama } from './anime/gintama';

  const titan: Titan = { name: '진격의 거인' };
  const haikyuu: Haikyuu = { name: '하이큐' };
  const gintama: Gintama = { name: '은혼' };
  ```
  - anime 폴더에 있는 파일 3개에서 각각 인터페이스를 하나씩 내보내고 app.ts 파일에서 모두 가져온다.
  - 비슷한 여러 개의 모듈을 사용하면 import 구문이 많아져서 가독성이 떨어진다. -> anime 폴더에 파일 3개 모듈을 한곳에 모아 주는 중간 파일을 생성할 수 있다.
    ```ts
    // ./anime/index.ts
    import { Titan } from './titan';
    import { Haikyuu } from './haikyuu';
    import { Gintama } from './gintama';

    export { Titan, Haikyuu, Gintama };

    // app.ts
    import { Titan, Haikyuu, Gintama } from './anime';

    const titan: Titan = { name: '진격의 거인' };
    const haikyuu: Haikyuu = { name: '하이큐' };
    const gintama: Gintama = { name: '은혼' };
    ```
    - 앞의 코드와 다르게 app.ts의 import 구문이 1줄만 있다.
    - `./anime`는 실제로 `./anime/index.ts` 파일을 가리킨다.
    - index.ts 파일에서 내보낸 Titan, Haikyuu, Gintama 타입을 모두 정상적으로 가져온다.
- 여러 개의 모듈을 다룰 때는 마치 하나의 통에 가지런히 정리하듯이 배럴 모듈화 전략을 사용하여 코드 가독성을 높일 수 있다.

<br>
<br>