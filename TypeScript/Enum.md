# 이넘
- 자바스크립트에는 이넘이 없지만 타입스크립트에서는 이넘을 지원한다.
- 특정 데이터 집합을 표현할 때 사용하기 좋다.

<br>
<br>

## 1. 이넘이란?
- **이넘(enum)**: 특정 값의 집합을 의미하는 데이터 타입이다.
  - 상수 집합이라고도 표현한다.
- 상수: 변하지 않는 고정 값
  - 상수는 단순히 고정된 값을 저장하는 것뿐만 아니라 이 값이 어떤 의미를 갖는지 알려 줌으로써 가독성을 높일 수 있다.
  - 변수의 역할인 값에 의미를 부여한느 것과 같은 맥락이다.
  - 상수는 보통 모두 대문자로 작성해 일반 변수와 구분한다.
- 여러 개의 상수를 하나의 단위로 묶으면 이넘이 된다,
  - 비슷한 성격이나 같은 범주에 있는 상수를 하나로 묶어 더 큰 단위의 상수로 만드는 것이 이넘의 역할이다.
  ```
  애니메이션
  - 진격의 거인
  - 주술회전
  - 은혼
  ```
  ```ts
  enum Animation {
    AttackOnTitan,
    JujutsuKaisen,
    Gintama
  }
  ```
  - 이넘 값의 각 속성을 사용하는 방법
    ```ts
    const action = Animation.AttackOnTitan;
    const comedy = Animation.Gintama;
    ```
    - 객체의 속성에 접근하듯이 이넘의 이름을 쓰고 `.` 접근자를 이용하여 속성 이름을 붙인다.
    - action과 comedy에는 각각 0과 2가 대입된다.

<br>
<br>

## 2. 숫자형 이넘
- 이넘에 선언된 속성은 기본적으로 숫자 값을 가진다.
- 이넘을 선언하면 첫 번째 속성부터 0, 1, 2, 3이 할당된다.
  ```ts
  enum Direction {
    Up,    // 0
    Down,    // 1
    Left,    // 2
    Right    // 3
  }

  console.log(Direction.Up);    // 0
  ```
  - 이넘의 첫 번째 속성인 Up의 값은 0이고, 두 번째 속성인 Down 값은 1이다.
  - 순서대로 1씩 증가하여 마지막 속성인 Right 값은 3이다.
  - 속성 값이 숫자로 지정되는 이유는 타입스크립트 내부 규칙 때문이다.
  - 이넘 코드를 자바스크립트 코드로 변경하면 속성 값으로 숫자가 할당된다는 것을 알 수 있다.
    ```js
    Direction["Up"] = 0
    ```
    ```js
    // JavaScript
    Direction[Direction["Up"] = 0 ] = "Up"
    // 아래처럼 동작
    Direction[0] = "Up"
    ```
    - `Direction["Up"] = 0`은 0이기 때문에 `Direction[0] = "Up"`처럼 동작한다.
  - 결과적으로 Direction 변수는 다음과 같이 정의된다.
    ```js
    Direction.Up = 0;
    Direction[0] = "Up";
    ```
    - Direction 변수의 Up 속성에는 숫자 값 0이 할당되고, 0 속성에는 문자열 Up이 할당된다.
  - 이렇게 이넘의 속성과 값이 거꾸로 연결되어 할당되는 것을 **리버스 매핑(reverse mapping)** 이라고 한다.
- 이넘 속성의 초기값을 변경할 수 있다.
  ```ts
  enum Direction {
    Up = 10,
    Down,    // 11
    Left,    // 12
    Right,    // 13
  }
  ```
  - 기본값이 0이 아니라 10으로 설정되고, 1씩 증가하여 Down 속성은 11, Left 속성은 12, Right 속성은 13을 갖는다.
  - 첫 번째 속성의 시작 값을 변경하더라도 순서대로 선언된 이넘 속성의 값은 1씩 증가하는 규칙이 있다.
  - 실제 코드를 작성할 때는 명시적으로 값을 설정하는 것이 컴파일 결과를 보지 않아도 값을 빠르게 파악할 수 있다.
  ```ts
  enum Direction {
    Up = 10,
    Down = 11,
    Left = 12,   
    Right = 13  
  }
  ```

<br>
<br>

## 3. 문자형 이넘
- 문자형 이넘: 이넘의 속성 값에 문자열을 연결한 이넘을 의미한다.
- 숫자형 이넘과 다르게 모든 속성 값을 다 문자열로 지정해 주어야 하고, 선언된 속성 순서대로 값이 증가하는 규칙도 없다.
- Direction 이넘을 문자형 이넘으로 바꾸기
  ```ts
  enum Direction {
    Up = 'Up',
    Down = 'Down',
    Left = 'Left',   
    Right = 'Right'  
  }
  ```
  - 방향 값은 숫자로 관리되기보다는 문자열로 관리되는 것이 더 명시적이다.
  - 이넘의 Up 속성값을 출력
    ```ts
    console.log(Direction.Up);    // 'Up'
    ```
- 실무에서는 이넘 값을 숫자로 관리하기보다 문자열로 관리하는 사례가 더 많다.
- 속성 이름과 값은 동일한 문자열로 관리하는 것도 일반적인 코딩 규칙이다.
- 이넘은 파스칼 케이스, 대문자, 언더스코어를 모두 사용해도 괜찮다.
  ```ts
   enum Direction {
    UP = 'UP',
    DOWN = 'DOWN',
    LEFT = 'LEFT',   
    RIGHT = 'RIGHT'  
  }

  enum ArrowKey {
    KEY_UP = 'KEY_UP',
    KEY_DOWN = 'KEY_DOWN'
  }
  ```

<br>
<br>

## 4. 알아 두면 좋은 이넘의 특징

<br>

### 4.1. 혼합 이넘
- 이넘은 숫자와 문자열을 섞어서 선언할 수 있다.
  ```ts
  enum Answer {
    Yes = 'Yes',
    No = 1
  }
  ```
  - 이넘 값은 인관되기 숫자나 문자열 둘 중 하나의 데이터 타입으로 관리하는 것이 좋다.

<br>

### 4.2. 다양한 이넘 속성 값 정의 방식
- 이넘의 속성 값은 고정 값뿐만 아니라 다양한 형태로 값을 할당할 수 있다.
  - 먼저 선언되어 있는 이넘의 속성을 활용할 수 있다.
  - 덧셈 연산자를 사용하여 계산한 값을 속성 값으로 할당할 수 있다.
  ```ts
  enum Authorization {
    User,
    Admin,
    SuperAdmin = User + Admin,
    God = 'abc'.length
  }
  ```
  - User와 Admin은 이넘의 기본 규칙에 따라 값이 0과 1이다.
  - SuperAdmin 속성은 User과 Admin을 더한 값인 1이다.
  - God 속성은 'abc' 문자열의 길이이므로 3이다.
- 이넘 속성 값을 할당할 때 연산자를 사용할 수 있지만 활용도는 높지 않다.
- 이넘은 각 속성 이름만 보고도 값을 추측할 수 있는 문자열 값을 많이 사용한다. 

<br>

### 4.3. const 이넘
- const 이넘: 이넘을 선언할 때 앞에 const를 붙인 이넘을 의미한다.
- const를 이넘 앞에 붙이는 이유는 컴파일 결과물의 코드양을 줄이기 위해서이다. 
- 일반 이넘 코드 컴파일 결과
  ```ts
  enum logLevel {
    Debug = 'Debug',
    Info = 'Info',
    Error = 'Error'
  }
  ```
  ```js
  // 결과
  "use strict";
  var logLevel;
  (function (logLevel) {
    logLevel["Debug"] = "Debug";
    logLevel["Info"] = "Info";
    logLevel["Error"] = "Error";
  })(logLevel || (logLevel = {}));
  ```
  - logLevel 이넘을 코드에서 활용하려면 logLevel이라는 객체를 내부적으로 선언해서 이넘 속성 값들을 연결해 주어야 한다.
  - 이넘 코드는 컴파일될 때 객체가 이넘의 속성 이름과 값을 연결해 주는 객체를 생성한다.
- const 이넘 코드 컴파일 결과
  ```ts
  const enum logLevel {
    Debug = 'Debug',
    Info = 'Info',
    Error = 'Error'
  }

  const appLevel = logLevel.Error;
  ```
  ```js
  // 결과
  "use strict";
  const appLevel = "Error"    /* logLevel.Error */
  ```
  - const 이넘은 객체를 생성하지 않고 이넘이 사용되는 곳에서 속성 값을 바로 연결해 준다.
- const 이넘은 고정 값만 할당할 수 있다. (연산자 등 사용 X)

<br>
<br>