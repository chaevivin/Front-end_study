# 모듈

## 1. 모듈의 일반적 의미
- 모듈: 애플리케이션을 구성하는 개별적 요소로서 재사용 가능한 코드 조각
- 모듈은 기능을 기준으로 파일 단위로 분리
  - 모듈이 성립하려면 모듈은 자신만의 **파일 스코프(모듈 스코프)** 를 가져야 함
- 모듈의 자산(모듈에 포함되어 있는 변수, 함수, 객체 등)은 기본적으로 비공개 상태
  - 모듈의 모든 자산은 캡슐화되어 다른 모듈에서 접근 불가능
  - 모듈은 개별적 존재로서 애플리케이션과 분리되어 존재
- 애플리케이션과 완전히 분리되어 개별적으로 존재하는 모듈은 재사용이 불가능하므로 존재의 의미 X 
  - 모듈은 애플리케이션이나 다른 모듈에 의해 재사용되어야 의미가 있음
  
- **모듈은 공개가 필요한 자산에 한정하여 명시적으로 선택적 공개가 가능** -> **export**
  - 공개된 모듈의 자산은 다른 모듈에서 재사용 가능
  - 모듈 사용자: 공개된 모듈의 자산을 사용하는 모듈
- **모듈 사용자는 모듈이 공개(export)한 자산 중 일부 또는 전체를 선택해 자신의 스코프 내로 불러들여 재사용 가능** -> **import**
- 모듈은 애플리케이션과 분리되어 개별적으로 존재하다가 필요에 따라 다른 모듈에 의해 재사용
  - 모듈은 기능별로 분리되어 개별적인 파일로 작성
  - 코드의 단위를 명확히 분리하여 애플리케이션 구성 가능
  - 재사용성이 좋아서 개발 효율성과 유지 보수성 증가

<br>
<br>

## 2. 자바스크립트와 모듈
- 자바스크립트는 웹페이지의 단순한 보조 기능을 처리하기 위한 제한적인 용도를 목적으로 탄생
  - 자바스크립트는 모듈이 성립하기 위해 필요한 파일 스코프와 import, export 지원하지 않았음
- 클라이언트 사이드 자바스크립트는 script 태그를 사용하여 외부의 자바스크립트 파일을 로드할 수 있지만 파일마다 독립적인 파일 스코프 갖지 X
  - 자바스크립트 파일을 여러 개의 파일로 분리하여 script 태그로 로드해도 분리된 자바스크립트 파일들은 결국 하나의 자바스크립트 파일 내에 있는 것처럼 동작
  - 모든 자바스크립트 파일은 하나의 전역을 공유
    - 따라서 분리된 자바스크립트 파일들의 전역 변수가 중복되는 등의 문제 발생
    - 모듈 구현 불가
- CommonJS, AMD(Asynchronous Module Definition): 자바스크립트의 모듈 시스템을 위해 제안된 것
  - 브라우저 환경에서 모듈을 사용하기 위해 CommonJS 또는 AMD를 구현한 모듈 로더 라이브러리를 사용
  - 자바스크립트 런타임 환경인 Node.js는 ECMAScript 표준 사양은 아니지만 CommonJS 채택
    - Node.js 환경에서는 파일별로 독립적인 파일 스코프(모듈 스코프)를 가짐

<br>
<br>

## 3. ES6 모듈(ESM)
- ES6에서는 클라이언트 사이드 자바스크립트에서도 동작하는 모듈 기능 추가
- 사용 방법
  - script 태그에 `type="module"` 어트리뷰트 추가하면 로드된 자바스크립트 파일은 모듈로서 동작
  - 파일 확장자는 mjs 사용 권장: 일반적인 자바스크립트 파일이 아닌 ESM임을 명확히 하기 위해서
  - ESM은 기본적으로 `strict mode` 적용
  ```html
  <script type="module" src="app.mjs"></script>
  ```

<br>

### 3.1. 모듈 스코프
- ESM은 독자적인 모듈 스코프를 가짐
- ESM이 아닌 일반적인 자바스크립트 파일은 script 태그로 분리해서 로드해도 독자적인 모듈 스코프를 가지지 않음
  ```js
  // foo.js
  // x 변수는 전역 변수
  var x = 'foo';
  console.log(window.x);    // foo
  ```
  ```js
  // bar.js
  // x 변수는 전역 변수 -> foo에서 선언한 전역 변수 x와 중복된 선언
  var x = 'bar';
  
  // x 변수 값 재할당
  console.log(window.x);    // bar
  ```
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <script src="foo.js"></script>
      <script src="bar.js"></script>
    </body>
  </html>
  ```
  - HTML에서 script 태그로 분리해서 로드된 2개의 자바스크립트 파일은 하나의 자바스크립트 파일이 있는 것처럼 동작
  - 하나의 전역 공유 -> foo.js에서 선언한 x 변수와 bar.js에서 선언한 x 변수는 중복 선언되며 의도치 않게 x 변수의 값이 덮어써짐

<br>

- ESM은 파일 자체의 독자적인 모듈 스코프 제공
  - 모듈 내에서 var 키워드로 선언한 변수는 더는 전역 변수가 아니며 window 객체의 프로퍼티도 아님
  ```js
  // foo.mjs
  var x = 'foo';
  console.log(x);    // foo
  console.log(window.x);    // undefined
  ```
  ```js
  // bar.mjs
  var x = 'bar';
  console.log(x);    // bar
  console.log(window.x);    // undefined
  ```
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <script type="module" src="foo.mjs"></script>
      <script type="module" src="bar.mjs"></script>
    </body>
  </html>
  ```
- 모듈 내에서 선언한 식별자는 모듈 외부에서 참조 불가
  - 모듈 스코프가 다르기 때문
  ```js
  // foo.mjs
  const x = 'foo';
  console.log(x);    // foo
  ```
  ```js
  // bar.mjs
  console.log(x);    // ReferenceError: x is not defined
  ```
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <script type="module" src="foo.mjs"></script>
      <script type="module" src="bar.mjs"></script>
    </body>
  </html>
  ```

<br>

### 3.2. export 키워드
- 모듈 내부에서 선언한 모든 식별자는 기본적으로 해당 모듈 내부에서만 참조 가능
- 모듈 내부에서 선언한 식별자를 외부에 공개하여 다른 모듈들이 재사용할 수 있게 하려면 export 키워드 사용
- export 키워드는 선언문 앞에 사용
  - 변수, 함수, 클래스 등 모든 식별자 export 가능
  ```js
  // lib.mjs
  // 변수의 공개
  export const pi = Math.PI;

  // 함수의 공개
  export function square(x) {
    return x * x;
  }

  // 클래스의 공개
  export class Person {
    constructor(name) {
      this.name = name;
    }
  }
  ```
  - export할 대상을 하나의 객체로 구성하여 한 번에 export 가능
  ```js
  // lib.mjs
  const pi = Math.PI;

  function square(x) {
    return x * x;
  }

  class Person {
    constructor(name) {
      this.name = name;
    }
  }

  export { pi, square, Person };
  ```

<br>

### 3.3. import 키워드
- 다른 모듈에서 공개한 식별자를 자신의 모듈 스코프 내부로 로드하려면 import 키워드 사용
- 다른 모듈이 export한 식별자 이름으로 import 해야 하며 ESM의 경우 파일 확장자 생략 불가
  ```js
  // app.mjs
  // 같은 폴더 내의 lib.mjs 모듈이 export한 이름으로 import
  // ESM의 경우 파일 확장자 생략 불가
  import { pi, square, Person } from './lib.mjs';

  console.log(pi);    // 3.141592653589793
  console.log(square(10));    // 100
  console.log(new Person('Lee'));    // Person { name: "Lee" }
  ```
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <script type="module" src="app.mjs"></script>
    </body>
  </html>
  ```
  - app.mjs는 애플리케이션의 진입점(entry point)이므로 반드시 script 태그로 로드
  - lib.mjs는 app.mjs의 import문에 로드되는 의존성(dependency)이므로 script 태그로 로드하지 않아도 됨
- 모듈이 export한 식별자 이름을 일일이 지정하지 않고 하나의 이름으로 한 번에 import 가능
  - import되는 식별자는 as 뒤에 지정한 이름의 객체에 프로퍼티로 할당
  ```js
  // app.mjs
  // lib.mjs 모듈이 export한 모든 식별자를 lib 객체의 프로퍼티로 모아 import
  import * as lib from './lib.mjs';

  console.log(lib.pi);    // 3.141592653589793
  console.log(lib.square(10));    // 100
  console.log(new lib.Person('Lee'));    // Person { name: "Lee" }
  ```
- 모듈이 export한 식별자 이름을 변경하여 import도 가능
  ```js
  // app.mjs
  // lib.mjs 모듈이 export한 식별자 이름을 변경하여 import
  import { pi as PI , square as sq, Person as P } from './lib.mjs';

  console.log(PI);    // 3.141592653589793
  console.log(sq(10));    // 100
  console.log(new P('Lee'));    // Person { name: "Lee" }
  ```
- 모듈에서 하나의 값만 export한다면 default 키워드 사용 가능
  - default 키워드를 사용하는 경우 기본적으로 이름 없이 하나의 값을 export
  ```js
  // lib.mjs
  export default x = x * x;
  ```
  - default 키워드를 사용하는 경우 var, let, const 키워드 사용 불가
  ```js
  // lib.mjs
  export default const foo = () => {};
  // SyntaxError: Unexpected token 'const'
  // export default () => {};
  ```
  - default 키워드와 함께 export한 모듈은 {} 없이 임의의 이름으로 import
  ```js
  // app.mjs
  import square from './lib.mjs';

  console.log(square(3));    // 9
  ```

<br>
<br>