# 실행 컨텍스트

## 1. 소스코드의 타입
- ECMAScript 사양은 소스코드를 4가지 타입으로 구분 
- 4가지 타입의 소스코드는 실행 컨텍스트를 생성
- 4가지 타입으로 구분하는 이유
  - 소스코드의 타입에 따라 실행 컨텍스트를 생성하는 과정과 관리 내용이 다르기 때문

| 소스코드의 타입 | 설명 |
| :--- | :--- |
| 전역 코드 (global code) | 전역에 존재하는 소스코드를 말한다. 전역에 정의된 함수, 클래스 등의 내부 코드는 포함되지 않는다. |
| 함수 코드 (function code) | 함수 내부에 존재하는 소스코드를 말한다. 함수 내부에 중첩된 함수, 클래스 등의 내부 코드는 포함되지 않는다. |
| eval 코드 | 빌트인 전역 함수인 eval 함수에 인수로 전달되어 실행되는 소스코드를 말한다. |
| 모듈 코드 (module code) | 모듈 내부에 존재하는 소스코드를 말한다. 모듈 내부의 함수, 클래스 등의 내부 코드는 포함되지 않는다. |

<br>

1. 전역 코드
    - 전역 코드는 전역 변수를 관리하기 위해 최상위 스코프인 전역 스코프 생성
    - var 키워드로 선언된 전역 변수와 함수 선언문으로 정의된 전역 함수를 전역 객체의 프로퍼티와 메서드로 바인딩하고 참조하기 위해 전역 객체와 연결
    - 이를 위해 전역 코드가 평가되면 실행 컨텍스트 생성

2. 함수 코드
    - 함수 코드는 지역 스코프를 생성하고 지역 변수, 매개변수, arguments 객체를 관리
    - 생성한 지역 스코프를 전역 스코프에서 시작하는 스코프 체인의 일원으로 연결
    - 이를 위해 함수 코드가 평가되면 함수 실행 컨텍스트 생성

3. eval 코드
    - strict mode에서 자신만의 독자적인 스코프 생성
    - 이를 위해 eval 코드가 평가되면 eval 실행 컨텍스트 생성

4. 모듈 코드
    - 모듈별로 독립적인 모듈 스코프를 생성
    - 이를 위해 모듈 코드가 평가되면 모듈 실행 컨텍스트 생성

<br>
<br>

## 2. 소스코드의 평가와 실행
- 자바스크립트 엔진은 소스코드를 '소스코드의 평가'와 '소스코드의 실행' 과정으로 나누어 처리
- 순서 : 소스코드의 평가 (선언문) -> 선언문 이외의 소스코드 실행 (런타임) 
- 소스코드 평가
  - 실행 컨텍스트 생성
  - 변수, 함수 등의 선언문만 먼저 실행하여 생성된 변수나 함수 식별자를 키로 실행 컨텍스트가 관리하는 스코프에 등록
- 소스코드 실행
  - 소스코드 평가 과정이 끝나면 선언문을 제외한 소스코드 실행 = 런타임 시작
  - 소스코드 실행에 필요한 정보(변수나 함수의 참조)를 실행 컨텍스트가 관리하는 스코프에서 검색해서 취득
  - 소스코드의 실행 결과(변수 값의 변경 등)는 다시 실행 컨텍스트가 관리하는 스코프에 등록

<br>

```js
var x;
x = 1;
```
위의 코드를 살펴보면,
1. 소스코드 평가 : 자바스크립트 엔진은 변수 선언문 var x;를 먼저 실행. 
2. 이때 생성된 변수 식별자 x는 실행 컨텍스트가 관리하는 스코프에 등록되고 undefined로 초기화
3. 소스코드 실행 : 변수 선언문은 이미 실행 완료. 변수 할당문 x = 1;만 실행
4. 이때 x 변수가 선언된 변수인지 확인하기 위해 실행 컨텍스트가 관리하는 스코프에 x 변수가 등록되어 있는지 확인
5. x 변수는 소스코드 평가 과정에서 선언문이 실행되어 등록된 변수이기 때문에 값을 할당하고 할당 결과를 실행 컨텍스트에 등록하여 관리

<br>
<br>

## 3. 실행 컨텍스트의 역할
```js
// 전역 변수 선언
const x = 1;
const y = 2;

// 함수 정의 
function foo(a) {
  // 지역 변수 선언
  const x = 10;
  const y = 20;

  // 메서드 호출
  console.log(a + x + y);    // 130
}

// 함수 호출
foo(100);

// 메서드 호출
console.log(x + y);    // 3
```
1. 전역 코드 평가
    - 전역 코드 평가 과정을 통해 전역 코드를 실행하기 위해 준비
    - 소스코드 평가 과정에서는 선언문만 먼저 실행
    - 전역 코드의 변수 선언문과 함수 선언문이 먼저 실행
    - 실행된 결과로 생성된 전역 변수와 전역 함수가 실행 컨텍스트가 관리하는 전역 스코프에 등록
    - 이때 var 키워드로 선언된 지역 변수와 함수 선언문으로 정의된 전역 함수는 전역 객체의 프로퍼티와 메서드가 됨

<br>

2. 전역 코드 실행
    - 전역 코드 평가가 끝나면 런타임 시작되어 전역 코드가 순차적으로 실행
    - 전역 변수에 값이 할당되고 함수 호출
    - 함수가 호출되면 순차적으로 실행되던 전역 코드 실행을 일시 중단하고 코드 실행 순서를 변경하여 함수 내부로 진입

<br>

3. 함수 코드 평가
    - 함수 호출로 함수 내부로 진입하면 함수 내부의 문들을 실행하기 전에 함수 코드 평가 
    - 매개변수와 지역 변수 선언문이 먼저 실행
    - 실행된 결과 생성된 매개변수와 지역 변수가 실행 컨텍스트가 관리하는 지역 스코프에 등록
    - 함수 내부에서 지역 변수처럼 사용할 수 있는 arguments 객체가 생성되어 지역 스코프에 등록
    - this 바인딩 결정

<br>

4. 함수 코드 실행
    - 함수 코드 평가 과정이 끝나면 런타임 시작 -> 함수 코드 순차적으로 실행
    - 매개변수와 지역 변수에 값 할당, console.log 메서드 호출
    - console.log 메서드를 호출하기 위해 식별자인 console을 스코프 체인으로 검색
      - 이를 위해 함수 코드의 지역 스코프는 상위 스코프인 전역 스코프와 연결
      - 하지만 console 식별자는 스코프 체인에 등록 X, 전역 객체에 프로퍼티로 존재
      - 전역 객체의 프로퍼티가 전역 변수처럼 전역 스코프를 통해 검색 가능
    - log 프로퍼티를 console 객체의 프로토타입 체인을 통해 검색
    - console.log 메서드에 인수로 전달된 표현식 a + x + y 평가
    - a, x, y 식별자는 스코프 체인을 통해 검색
    - console.log 메서드 실행 종료되면 함수 코드 실행 과정이 종료되고 전역 코드 실행을 계속

<br>

**코드가 실행되려면 스코프, 식별자, 코드 실행 순서 등의 관리 필요** -> **실행 컨텍스트가 관리**
1. 선언에 의해 생성된 모든 식별자(함수, 변수, 클래스 등)를 스코프를 구분하여 등록하고 상태 변화(식별자에 바인딩된 값의 변화)를 지속적으로 관리
2. 스코프는 중첩 관계에 의해 스코프 체인을 형성해야 한다. 즉, 스코프 체인을 통해 상위 스코프로 이동하며 식별자를 검색할 수 있어야 한다.
3. 현재 실행 중인 코드의 실행 순서를 변경(예를 들어, 함수 호출에 의한 실행 순서 변경)할 수 있어야 하며 다시 되돌아갈 수도 있어야 한다.

**실행 컨텍스트는 소스코드를 실행하는 데 필요한 환경을 제공하고 코드의 실행 결과를 실제로 관리하는 영역이다.**

**실행 컨텍스트는 식별자를 등록하고 관리한느 스코프와 코드 실행 순서 관리를 구현한 내부 메커니즘으로, 모든 코드는 실행 컨텍스트를 통해 실행되고 관리된다.**

식별자와 스코프는 실행 컨텍스트의 **렉시컬 환경**으로 관리하고 코드 실행 순서는 **실행 컨텍스트 스택**으로 관리한다.

<br>
<br>

## 4. 실행 컨텍스트 스택
```js
const x = 1;

function foo() {
  const y = 2;

  function bar() {
    const z = 3;
    console.log(x + y + z);
  }
  bar();
}

foo();    // 6
```
위의 코드를 살펴보면,
- 위의 코드는 전역 코드와 함수 코드로 이루어짐
- 자바스크립트 엔진은 전역 코드를 평가 -> 전역 실행 컨텍스트 생성
- 함수 호출되면 함수 코드 평가 -> 함수 실행 컨텍스트 생성
- **실행 컨텍스트 스택** : 생성된 실행 컨텍스트는 스택 자료구조로 관리
- 코드가 실행되는 시간의 흐름에 따라 실행 컨텍스트 스택에는 실행 컨텍스트가 추가(push)되고 제거(pop)

<br>

1. 전역 코드의 평가와 실행
    - 자바스크립트 엔진은 전역 코드 평가 -> 전역 실행 컨텍스트 생성 -> 실행 컨텍스트 스택에 푸시
    - 전역 변수 x와 전역 함수 foo는 전역 실행 컨텍스트에 등록
    - 전역 코드가 실행되기 시작하여 전역 변수 x에 값이 할당되고 전역 함수 foo가 호출

2. foo 함수 코드의 평가와 실행
    - 전역 함수 foo가 호출 -> 전역 코드 실행 일시 중단 -> 코드의 제어권이 foo 함수 내부로 이동
    - 자바스크립트 엔진은 foo 함수 내부의 함수 코드 평가 -> foo 함수 실행 컨텍스트 생성 -> 실행 컨텍스트 스택에 푸시
    - foo 함수의 지역 변수 y와 중첩 함수 bar가 foo 함수 실행 컨텍스트에 등록
    - foo 함수 코드가 실행되기 시작하여 지역 변수 y에 값이 할당되고 중첩 함수 bar가 호출

3. bar 함수 코드의 평가와 실행
    - 중첩 함수 bar가 호출 -> foo 함수 코드의 실행 일시 중단 -> 코드의 제어권이 bar 함수 내부로 이동
    - 자바스크립트 엔진은 bar 함수 내부의 함수 코드 평가 -> bar 함수 실행 컨텍스트 생성 -> 실행 컨텍스트 스택에 푸시
    - bar 함수의 지역 변수 z가 bar 함수 실행 컨텍스트에 등록
    - bar 함수 코드가 실행되기 시작하여 지역 변수 z에 값이 할당되고 console.log 메서드를 호출하고 bar 함수 종료

4. foo 함수 코드로 복귀
    - 코드의 제어권이 다시 foo 함수로 이동
    - 자바스크립트 엔진은 bar 함수 실행 컨텍스트를 실행 컨텍스트 스택에서 팝하여 제거
    - foo 함수는 더 이상 실행할 코드가 없으므로 종료

5. 전역 코드로 복귀
    - 코드의 제어권이 다시 전역 코드로 이동
    - 자바스크립트 엔진은 foo 함수 실행 컨텍스트를 실행 컨텍스트 스택에서 팝하여 제거
    - 더 이상 실행할 전역 코드가 없으므로 전역 실행 컨텍스트도 실행 컨텍스트 스택에서 팝되어 제거
    - 실행 컨텍스트 스택에는 아무것도 없음

<br>

- **실행 컨텍스트 스택은 코드의 실행 순서를 관리**
- 소스코드가 평가 -> 실행 컨텍스트 생성 -> 실행 컨텍스트 최상위에 쌓임
- **실행 컨텍스트 스택의 최상위에 존재하는 실행 컨텍스트는 현재 실행 중인 코드의 실행 컨텍스트**
- **실행 중인 실행 컨텍스트(running execution context)** : 실행 컨텍스트 스택의 최상위에 존재하는 실행 컨텍스트

<br>
<br>

## 5. 렉시컬 환경
- 렉시컬 환경은 식별자와 식별자에 바인딩된 값, 그리고 상위 스코프에 대한 참조를 기록하는 자료구조
- 실행 컨텍스트를 구성하는 컴포넌트
- 스코프와 식별자 관리
- 키와 값을 갖는 객체 형태의 스코프(전역, 함수, 블록 스코프)를 생성하여 식별자를 키로 등록하고 식별자에 바인딩된 값 관리
- 렉시컬 환경은 스코프를 구분하여 식별자를 등록하고 관리하는 정장소 역할을 하는 렉시컬 스코프의 실체

<br>

- 실행 컨텍스트 = LexicalEnvironment 컴포넌트 + VaraibleEnvironment 컴포넌트
  - 생성 초기 LexicalEnvironment 컴포넌트와 VariableEnvironment 컴포넌트는 하나의 동일한 렉시컬 환경 참조
  - 몇 가지 상황 후 VariableEnvironment 컴포넌트를 위한 새로운 렉시컬 환경 생성 -> 이때부터 LexicalEnvironment 컴포넌트와 VariableEnvironment 컴포넌트 내용이 달라질 수 있음

<br>

- 렉시컬 환경 : 환경 레코드(EnvironmentRecord) + 외부 렉시컬 환경에 대한 참조(OuterLexicalEnvironmentReference)
  - 환경 레코드 : 스코프에 포함된 식별자를 등록하고 식별자에 바인딩된 값을 관리하는 저장소. 환경 레코드는 소스코드의 타입에 따라 관리하는 내용에 차이가 있음.
  - 외부 렉시컬 환경에 대한 참조 : 외부 렉시컬 환경에 대한 참조는 상위 스코프를 가리킴. 외부 렉시컬 환경에 대한 참조를 통해 단방향 링크드 리스트인 스코프 체인을 구현
    - 상위 스코프 : 외부 렉시컬 환경, 즉 해당 실행 컨텍스트를 생성한 소스코드를 포함하는 상위 코드의 렉시컬 환경 

<br>
<br>

## 6. 실행 컨텍스트의 생성과 식별자 검색 과정
```js
var x = 1;
const y = 2;

function foo(a) {
  var x = 3;
  const y = 4;

  function bar(b) {
    const z = 5;
    console.log(a + b + x + y + z);
  }
  bar(10);
}

foo(20);    // 42 = 20 + 10 + 3 + 4 + 5
```

<br>

### 6.1. 전역 객체 생성
- 전역 객체는 전역 코드 평가되기 이전에 생성
- 전역 객체에는 빌트인 전역 프로퍼티와 빌트인 전역 함수, 표준 빌트인 객체가 추가
- 동작 환경(클라이언트 사이드 or 서버 사이드)에 클라이언트 사이드 Web API(DOM, BOM, Canvas, XMLHttpRequest, fetch, requestAnimationFrame, SVG, Web Storage, Web Component, Web Worker 등) 또는 특정 환경을 위한 호스트 객체 포함
- 전역 객체도 Object.prototype 상속 받음. 즉, 전역 객체도 프로토타입 체인의 일원
```js
// Object.prototype.toString
window.toString();    // "[object Window]"

window.__proto__.__proto__.__proto__.__proto__ === Object.prototype;    // true
```

<br>

### 6.2. 전역 코드 평가
- 소스코드가 로드되면 자바스크립트 엔진은 전역 코드 평가
- 평가 순서
  - 전역 실행 컨텍스트 생성
  - 전역 렉시컬 환경 생성 
    - 전역 환경 레코드 생성 
      - 객체 환경 레코드 생성 
      - 선언적 환경 레코드 생성 
    - this 바인딩 
    - 외부 렉시컬 환경에 대한 참조 결정

<br>

**1. 전역 실행 컨텍스트 생성**
- 비어 있는 전역 실행 컨텍스트 생성 -> 실행 컨텍스트 스택에 푸시
- 전역 실행 컨텍스트는 실행 컨텍스트 스택의 최상위, 즉 실행 중인 컨텍스트가 됨

<br>

**2. 전역 렉시컬 환경 생성**
- 전역 렉시컬 환경을 생성하고 전역 실행 컨텍스트에 바인딩
- 환경 레코드, 외부 렉시컬 환경에 대한 참조로 구성

<br>

**3. 전역 환경 레코드 생성**
- 전역 환경 레코드는 전역 변수를 관리하는 전역 스코프, 전역 객체의 빌트인 전역 프로퍼티와 빌트인 전역 함수, 표준 빌트인 객체 제공
- ES6 이전 : 모든 전역 변수가 전역 객체 프로퍼티 -> 전역 객체가 전역 환경 레코드 역할 수행
- ES6 이후 : let, const 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 되지 않고 개념적인 블록 내에 존재
- var 키워드로 선언한 변수와 let, const 키워드로 선언한 변수를 구분하여 관리하기 위해 전역 스코프 역할을 하는 **전역 환경 레코드는 객체 환경 레코드와 선언적 환경 레코드로 구성**
  - 객체 환경 레코드 : 기존의 전역 객체가 관리하던 var 키워드로 선언한 전역 변수, 함수 선언문으로 정의한 전역 함수, 빌트인 전역 프로퍼티와 빌트인 전역 함수, 표준 빌트인 객체 관리
  - 선언적 환경 레코드 : let, const 키워드로 선언한 전역 변수 관리
  - 전역 환경 레코드의 객체 환경 레코드와 선언적 환경 레코드는 서로 협력하여 전역 스코프와 전역 객체(전역 변수의 전역 객체 프로퍼티화)를 관리

<br>

**2.1.1. 객체 환경 레코드 생성**
- 객체 환경 레코드는 BindingObject라고 부르는 객체와 연결
  - **BindingObject : "전역 객체 생성"에서 생성된 전역 객체**
- **전역 코드 평가 과정에서 var 키워드로 선언한 전역 변수, 함수 선언문으로 정의된 저역 함수는 전역 환경 레코드의 객체 환경 레코드에 연결된 BindingObject를 통해 전역 객체의 프로퍼티와 메서드가 됨**
- 이때 등록된 식별자를 전역 환경 레코드의 객체 환경 레코드에서 검색 -> 전역 객체의 프로퍼티 검색하여 반환
- var 키워드로 선언된 전역 변수와 함수 선언문으로 선언된 전역 함수가 전역 객체의 프로퍼티와 메서드가 되고 전역 객체를 가리키는 식별자(window) 없이 전역 객체의 프로퍼티 참조할 수 있는 메커니즘

<br>

```js
var x = 1;
const y = 2;

function foo (a) {
  ...
}
```
위의 코드를 살펴보면,
- x는 var 키워드로 선언한 변수 
  - "선언 단계"와 "초기화 단계" 동시에 진행 -> 변수 호이스팅이 발생하는 이유
  - 전역 코드 평가 시점에 객체 환경 레코드에 바인딩된 BindingObject를 통해 전역 객체에 변수 식별자를 키로 등록 -> 암묵적으로 undefined 바인딩
  - 코드 실행 단계에서 변수 선언문 이전에도 참조 가능
  - var 키워드로 선언한 변수에 할당한 함수 표현식도 동일하게 작동
- 함수 선언문으로 정의한 함수가 평가 -> 함수 이름과 동일한 이름의 식별자를 객체 환경 레코드에 바인딩된 BindingObject를 통해 전역 객체에 키로 등록 -> 생성된 함수 객체에 즉시 할당
  - 함수 선언문으로 정의한 함수는 함수 선언문 이전에 호출 가능

<br>

**2.1.2. 선언적 환경 레코드 생성**
- let, const 키워드로 선언한 전역 변수는 선언적 환경 레코드에 등록되고 관리
- const 키워드로 선언한 변수는 "선언 단계"와 "초기화 단계" 분리되어 진행
  - 초기화 단계(런타임)에 실행 흐름이 변수 선언문이 도달하기 전까지 **일시적 사각지대**에 도달
- let, const 키워드로 선언한 변수도 호이스팅 발생
  - 하지만 let, const 키워드로 선언한 변수는 런타임에 컨트롤이 변수 선언문에 도달하기 전까지 일시적 사각지대에 빠지기 때문에 도달 불가능

<br>

**2.2. this 바인딩**
- 전역 환경 레코드의 [[GlobalThisValue]] 내부 슬롯에 this 바인딩
- 전역 코드에서 this를 참조하면 전역 환경 레코드의 [[GlobalThisValue]] 내부 슬롯에 바인딩되어 있는 객체 반환
- 전역 환경 레코드를 구성하는 객체 환경 레코드와 선언적 환경 레코드에는 this 바인딩 X
- this 바인딩은 전역 환경 레코드와 함수 환경 레코드에만 존재

<br>

**2.3. 외부 렉시컬 환경에 대한 참조 결정**
- 외부 렉시컬 환경에 대한 참조는 현재 평가 중인 소스코드를 포함하는 외부 소스코드의 렉시컬 환경, 즉 상위 스코프를 지칭
- 이를 통해 단방향 링크드 리스트인 스코프 체인 구현
- 전역 코드를 포함하는 소스코드는 없으므로 전역 렉시컬 환경의 외부 렉시컬 환경에 대한 참조에 null 할당
  - 전역 렉시컬 환경이 스코프 체인의 종점에 존재를 의미

<br>

### 6.3. 전역 코드 실행
- 전역 코드가 순차적으로 실행 시작 -> 변수 할당문이 실행되어 전역 변수에 값 할당
- 변수 할당문 또는 함수 호출문을 실행하려면 변수 또는 함수 이름이 선언된 식별자인지 확인
  - **식별자 결정** : 어느 스코프의 식별자를 참조하면 되는지 결정
- **식별자 결정을 위해 식별자를 검색할 때는 실행 중인 실행 컨텍스트에서 식별자를 검색**
  - 선언된 식별자는 실행 컨텍스트의 렉시컬 환경의 환경 레코드에 등록
- 실행 중인 실행 컨텍스트는 전역 실행 컨텍스트이므로 전역 렉시컬 환경에서 식별자 검색
  - 검색할 수 없으면 외부 렉시컬 환경에 대한 참조가 가리키는 렉시컬 환경, 즉 상위 스코프로 이동하여 식별자 검색
- 전역 렉시컬 환경은 스코프 체인의 종점이므로 전역 렉시컬 환경에서 검색할 수 없는 식별자는 참조 에러 발생 -> 식별자 결정에 실패했기 때문
- 실행 컨텍스트는 소스코드를 실행하기 위해 필요한 환경을 제공하고 코드의 실행 결과를 실제로 관리하는 영역

<br>

### 6.4. foo 함수 코드 평가
- foo 함수가 호출되면 전역 코드의 실행을 일시 중단 -> foo 함수 내부로 코드의 제어권이 이동 -> 함수 코드 평가 시작
- 함수 코드 평가 순서
  - 함수 실행 컨텍스트 생성
  - 함수 렉시컬 환경 생성
    - 함수 환경 레코드 생성
    - this 바인딩
    - 외부 렉시컬 환경에 대한 참조 결정

<br>

**1. 함수 실행 컨텍스트 생성**
- foo 함수 실행 컨텍스트 생성
- 생성된 함수 실행 컨텍스트는 함수 렉시컬 환경이 완성된 다음 실행 컨텍스트 스택에 푸시
- foo 함수 실행 컨텍스트는 스택의 최상위, 즉 실행 중인 실행 컨텍스트

<br>

**2. 함수 렉시컬 환경 생성**
- foo 함수 렉시컬 환경 생성 -> foo 함수 실행 컨텍스트에 바인딩
- 렉시컬 환경 = 환경 레코드 + 외부 렉시컬 환경에 대한 참조

<br>

**2.1. 함수 환경 레코드 생성**
- 함수 환경 레코드는 매개변수, arguments 객체, 함수 내부에서 선언한 지역 변수와 중첩 함수를 등록하고 관리

<br>

**2.2. this 바인딩**
- 함수 환경 레코드의 [[ThisValue]] 내부 슬롯에 this 바인딩
  - [[Thisvalue]] 내부 슬롯에 바인딩될 객체는 함수 호출 방식에 따라 결정
- 함수 내부에서 this를 참조하면 함수 환경 레코드의 [[ThisValue]] 내부 슬롯에 바인딩되어 있는 객체가 반환

<br>

**2.3. 외부 렉시컬 환경에 대한 참조 결정**
- 외부 렉시컬 환경에 대한 참조에 foo 함수 정의가 평가된 시점에 실행 중인 실행 컨텍스트의 렉시컬 환경의 참조가 할당
  - foo 함수는 전역 함수 -> 전역 코드 평가 시점에 평가
  - 이 시점에서 실행중인 실행 컨텍스트는 전역 실행 컨텍스트
  - 외부 렉시컬 환경에 대한 참조에는 전역 렉시컬 환경의 참조가 할당

<br>

- 자바스크립트 엔진은 함수 정의를 평가하여 함수 객체를 생성할 때 현재 실행 중인 실행 컨텍스트의 렉시컬 환경, 즉 상위 스코프를 함수 객체의 내부 슬롯 [[Environment]]에 저장
- 함수 렉시컬 환경의 외부 렉시컬 환경에 대한 참조에 할당되는 것은 함수의 상위 스코프를 가리키는 함수 객체의 내부 슬롯 [[Environment]]에 저장된 렉시컬 환경의 참조
- 함수 객체의 내부 슬롯 [[Environment]]가 렉시컬 스코프를 구현하는 메커니즘

<br>

### 6.5. foo 함수 코드 실행
- 런타임 시작 -> foo 함수의 소스코드 실행
- 매개변수에 인수 할당, 변수 할당문이 실행되어 지역변수에 값 할당, bar 함수 호출
- **식별자 결정을 위해 실행 중인 실행 컨텍스트의 렉시컬 환경에서 식별자를 검색하기 시작**
  - 현재 실행 중인 실행 컨텍스트는 foo 함수 실행 컨텍스트
  - foo 함수 렉시컬 환경에서 식별자 x, y를 검색하기 시작
- 검색할 수 없으면 외부 렉시컬 환경에 대한 참조가 가리키는 렉시컬 환경으로 이동하여 식별자 검색
- 검색된 식별자에 값 바인딩

<br>

### 6.6. bar 함수 코드 평가
- bar 함수 호출 -> bar 함수 내부로 코드의 제어권이 이동
- bar 함수 코드 평가 시작
- 실행 컨텍스트와 렉시컬 환경의 생성 과정은 foo 함수 코드 평가와 동일

<br>

### 6.6. bar 함수 코드 실행
- 런타임 시작 -> bar 함수 소스코드 실행 시작
- 매개변수에 인수 할당, 변수 할당문이 실행되어 지역 변수에 값 할당, console.log 메서드 실행
- 순서
  - console 식별자 검색
    - console 식별자를 스코프 체인에서 검색
      - 스코프 체인은 현재 실행 중인 실행 컨텍스트의 렉시컬 환경에서 시작 -> 외부 렉시컬 환경에 대한 참조로 이어지는 렉시컬 환경의 연속
      - 식별자를 검색할 때는 현재 실행 중인 실행 컨텍스트의 렉시컬 환경에서 검색 시작
    - 실행 중인 실행 컨텍스트는 bar 함수 실행 컨텍스트 -> bar 함수 렉시컬 환경에서 console 식별자 검색 -> 없으므로 스코프 체인의 상위 스코프, 즉 외부 렉시컬 환경에 대한 참조가 가리키는 foo 함수 렉시컬 환경으로 이동하여 다시 검색 -> 없으므로 상위 스코프인 전역 렉시컬 환경으로 이동하여 다시 검색
    - 전역 렉시컬 환경의 객체 환경 레코드의 BindingObject를 통해 전역 객체에서 console 찾음
  - log 메서드 검색
    - console 식별자에 바인딩된 객체, 즉 console 객체에서 log 메서드 검색
    - console 객체의 프로토타입 체인을 통해 메서드 검색
    - log 메서드는 상속된 프로퍼티가 아니라 console 객체가 직접 소유하는 프로펕;
  - 표현식 a + b + x + y + z의 평가
    - 표현식을 평가하기 위해 식별자 a, b, x, y, z 검색
    - 식별자는 스코프 체인에서 검색
    - a 식별자 : foo 함수 렉시컬 환경
    - b 식별자 : bar 함수 렉시컬 환경
    - x 식별자, y 식별자 : foo 함수 렉시컬 환경
    - z 식별자 : bar 함수 렉시컬 환경
  - console.log 메서드 호출
    - 표현식이 평가되어 생성한 값을 console.log 메서드에 전달하여 호출

<br>

### 6.8. bar 함수 코드 실행 종료
- console.log 메서드 호출 후 더이상 실행할 코드 X -> bar 함수 코드 실행 종료
- 실행 컨텍스트 스택에서 bar 함수 실행 컨텍스트가 팝되어 제거
  - 실행 컨텍스트 스택에서 제거되었다고 함수 렉시컬 환경까지 즉시 소멸 X
  - 렉시컬 환경은 실행 컨텍스트에 의해 참조되기는 하지만 독립적인 객체
  - 누군가에 의해 참조되지 않을 때 가비지 컬렉터에 의해 메모리 공간의 확보가 해제되어 소멸
- foo 실행 컨텍스트가 실행 중인 실행 컨텍스트 등극

<br>

### 6.9. foo 함수 코드 실행 종료
- bar 함수 종료 후 더이상 실행할 코드 X -> foo 함수 코드 실행 종료
- 실행 컨텍스트 스택에서 foo 함수 실행 컨텍스트가 팝되어 제거
- 전역 실행 컨텍스트가 실행 중인 실행 컨텍스트 등극

<br>

### 6.10. 전역 코드 실행 종료
- foo 함수 종료 후 더 이상 실행할 전역 코드 X -> 전역 코드 실행 종료
- 실행 컨텍스트 스택에서 전역 실행 컨텍스트가 팝되어 제거
- 실행 컨텍스트 스택에는 아무것도 안남은 상태가 됨

<br>
<br>

## 7. 실행 컨텍스트와 블록 레벨 스코프
let, const 키워드로 선언한 변수는 모든 코드 블록을 지역 스코프로 인정하는 블록 레벨 스코프를 따른다.

<br>

```js
let x = 1;

if (true) {
  let x = 10;
  console.log(x);    // 10
}

console.log(x);    // 1
```
위의 코드를 살펴보면,
- if 문의 코드 블록 내에서 let 키워드로 변수 선언
- if 문의 코드 블록이 실행되면 if 문의 코드 블록을 위한 블록 레벨 스코프를 생성
- 이를 위해 선언적 환경 레코드를 갖는 렉시컬 환경을 새롭게 생성하여 기존의 전역 렉시컬 환경을 교체
- 새롭게 생성된 if 문의 코드 블록을 위한 렉시컬 환경의 외부 렉시컬 환경에 대한 참조는 if문이 실행되기 이전의 렉시컬 환경을 가리킴
- 이는 블록 레벨 스코프를 생성하는 모든 블록문에 적용

<br>
<br>