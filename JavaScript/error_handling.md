# 에러 처리

## 1. 에러 처리의 필요성
- 에러가 발생하지 않는 코드를 작성하는 것은 불가능 -> 에러는 언제나 발생 가능
- 발생한 에러에 대해 대처하지 않고 방치하면 프로그램은 강제 종료
  ```js
  console.log('[Start]');

  foo();    // ReferenceError: foo is not defined
  // 프로그램 종료

  // 에러에 의해 프로그램이 강제 종료되어 아래 코드 실행 X
  console.log('[End]');
  ```
- `try...catch`문을 사용해 발생한 에러에 적절하게 대응하면 프로그램이 강제 종료되지 않고 계속해서 코드 실행 가능
  ```js
  console.log('[Start]');

  try {
    foo();
  }catch {
    console.error('[에러 발생]', error);
    // [에러 발생] ReferenceError: foo is not defined
  }

  // 프로그램 강제 종료 X
  console.log('[End]');
  ```

<br>

- 직접적으로 에러를 발생하지 않는 예외(exception)적인 상황이 발생 -> 예외적인 상황에 적절하게 대응하지 않으면 에러로 이어질 가능성이 큼
  ```js
  // DOM에 button 요소가 존재하지 않으면 querySelector 메서드는 에러를 발생시키지 않고 null을 반환
  const $button = document.querySelector('button');    // null

  $button.classList.add('disabled');
  // TypeError: Cannot read property 'classList' of null
  ```
- `querySelector` 메서드는 인수로 전달한 문자열이 CSS 선택자 문법에 맞지 않는 경우 에러 발생시킴
  ```js
  const $elem = document.querySelector('#1');
  ```
  - `querySelector` 메서드는 인수로 전달한 CSS 선택자 문자열로 DOM에서 요소 노드를 찾을 수 없는 경우 에러를 발생시키지 않고 null 반환
  - 이때 `if`문으로 `querySelector` 메서드의 반환값을 확인하거나 단축 평가 또는 옵셔널 체이닝 연산자 `?.`를 사용하지 않으면 다음 처리에서 에러로 이어질 가능성이 큼
  ```js
  const $button = document.querySelector('button');    // null
  $button?.classList.add('disabled');
  ```

<br>

- 이처럼 에러나 예외적인 상황에 대응하지 않으면 프로그램은 강제 종료
- 에러나 예외적인 상황은 다양하기 때문에 아무런 조치 없이 프로그램이 강제 종료된다면 원인 파악하여 대응하기 어려움
- 우리가 작성한 코드에서는 언제나 에러나 예외적인 상황이 발생할 수 있다는 것을 전제하고 이에 대응하는 코드를 작성하는 것이 중요

<br>
<br>

## 2. try..catch..finally 문
- 기본적으로 에러 처리를 구현하는 방법은 두 가지
  - 예외적인 상황이 발생하면 반환하는 값(null 또는 -1)을 `if`문이나 단축 평가 또는 옵셔널 체이닝 연산자를 통해 확인해서 처리하는 방법
  - 에러 처리 코드를 미리 등록해두고 에러가 발생하면 에러 처리 코드로 점프하도록 하는 방법
- `try...catch...finally`문은 두 번째에 해당하는 방법 -> **에러 처리**
- `try...catch...finally`문은 3개의 코드 블록으로 구성
  - finally문은 불필요하다면 생략 가능
  - catch문도 생략 가능하지만 catch문이 없는 try문은 의미가 없으므로 생략 X
  ```js
  try {
    // 실행할 코드 (에러가 발생할 가능성이 있는 코드)
  } catch(err) {
    // try 코드 블록에서 에러가 발생하면 이 코드 블록의 코드가 실행
    // err에는 try 코드 블록에서 발생한 Error 객체가 전달
  } finally {
    // 에러 발생과 상관없이 반드시 한 번 실행
  }
  ```
  - `try...catch...finally`문을 실행하면 먼저 try 코드 블록이 실행
  - try 코드 블록에 포함된 문 중에서 에러가 발생하면 발생한 에러는 catch문의 err 변수에 전달 -> catch 코드 블록 실행
  - catch문의 err 변수는 try 코드 블록 내에 포함된 문 중에서 에러가 발생하면 생성되고 catch 코드 블록에서만 유효
  - finally 코드 블록은 에러 발생과 상관없이 반드시 한 번 실행
- `try...catch...finally`문으로 에러를 처리하면 프로그램 강제 종료 X
  ```js
  console.log('[Start]');

  try {
    foo();
  } catch (err) {
    console.error(err);    // ReferenceError: foo is not defined
  } finally {
    console.log('finally');
  }

  // 프로그램 강제 종료 X
  console.log('[End]');
  ```

<br>
<br>

## 3. Error 객체
- Error 생성자 함수는 에러 객체를 생성
- Error 생성자 함수에는 에러를 상세히 설명하는 에러 메시지를 인수로 전달 가능
  ```js
  const error = new Error('invalid');
  ```
- Error 생성자 함수가 생성한 에러 객체는 message 프로퍼티와 stack 프로퍼티를 가짐
  - message 프로퍼티의 값: Error 생성자 함수에 인수로 전달한 에러 메시지
  - stack 프로퍼티의 값: 에러를 발생시킨 콜 스택의 호출 정보를 나타내는 문자열, 디버깅 목적으로 사용
- 자바스크립트는 Error 생성자 함수를 포함해 7가지의 에러 객체를 생성할 수 있는 Error 생성자 함수를 제공
  | 생성자 함수 | 인스턴스 |
  | :--- | :--- |
  | Error | 일반적 에러 객체 |
  | SyntaxError | 자바스크립트 문법에 맞지 않는 문을 해석할 때 발생하는 에러 객체 |
  | ReferenceError | 참조할 수 없는 식별자를 참조했을 때 발생하는 에러 객체 |
  | TypeError | 피연산자 또는 인수의 데이터 타입이 유효하지 않을 때 발생하는 에러 객체 |
  | RangeError | 숫자값의 허용 범위를 벗어났을 때 발생하는 에러 객체 |
  | URIError | encodeURI 또는 decodeURI 함수에 부적절한 인수를 전달했을 때 발생하는 에러 객체 |
  | EvalError | eval 함수에서 발생하는 에러 객체 |
  - 위 생성자 함수가 생성한 에러 객체의 프로토타입은 모두 Error.prototype 을 상속받음
  ```js
  1 @ 1;    // SyntaxError: Invalid of unexpected token
  foo();    // ReferenceError: foo is not defined
  null.foo;    // TypeError: Cannot reade property 'foo' of null
  new Array(-1);    // RangeError: Invalid array length
  decodeURIComponent('%');    // URIError: URI malformed
  ```

<br>
<br>

## 4. throw 문
- Error 생성자 함수로 에러 객체를 생성한다고 에러가 발생하는 것은 아님
  - 에러 객체 생성과 에러 발생은 의미가 다름
  ```js
  try {
    new Error('something wrong');
  } catch (error) {
    console.log(error);
  }
  ```
- 에러를 발생시키려면 try 코드 블록에서 throw.문으로 에러 객체를 던져야 함
  ```
  throw 표현식;
  ```
  - throw문의 표현식은 어떤 값이라도 상관없지만 일반적으로 에러 객체 지정
  - 에러를 던지면 catch문의 에러 변수가 생성되고 던져진 에러 객체가 할당 -> catch 코드 블록 실행 시작
  ```js
  try {
    // 에러 객체를 던지면 catch 코드 블록 실행 시작
    throw new Error('something wrong');
  } catch (error) {
    console.log(error);
  }
  ```

<br>

```js
// 외부에서 전달받은 콜백 함수를 n번만큼 반복 호출
const repeat = (n, f) => {
  // 매개변수 f에 전달된 인수가 함수가 아니면 TypeError 발생
  if (typeof f !== 'function') throw new TypeError('f must be a function');

  for (var i = 0; i < n; i++) {
    f(i);    // i를 전달하면서 f를 호출
  }
};

try {
  repeat(2, 1);    // 두 번째 인수가 함수가 아니므로 TypeError 발생(throw)
} catch(err) {
  console.error(err);    // TypeError: f must be a function
}
```
위의 코드를 살펴보면,
- 외부에서 전달받은 콜백 함수를 n번만큼 반복 호출하는 repeat 함수
  - repeat 함수는 두 번째 인수로 반드시 콜백 함수 전달받아야 함
  - 두 번째 인수가 함수가 아니면 TypeError 발생
- repeat 함수는 에러를 발생시킬 가능성이 있으므로 try 코드 블록 내부에서 호출

<br>
<br>

## 5. 에러의 전파
- 에러는 호출자(caller) 방향으로 전파
  - 콜 스택의 아래 방향(실행 중인 실행 컨텍스트가 푸시되기 직전에 푸시된 실행 컨텍스트 방향)으로 전파
  ```js
  const foo = () => {
    throw Error('foo에서 발생한 에러');    // ④
  };

  const bar = () => {
    foo();    // ③
  };

  const baz = () => {
    bar();    // ②
  };

  try {
    baz();    // ①
  } catch(err) {
    console.error(err);
  }
  ```
  - ①에서 baz 함수를 호출하면 ②에서 bar 함수가 호출되고 ③에서 foo 함수가 호출되고 ④에서 에러를 throw
  - 이때 foo 함수가 throw한 에러는 다음과 같이 호출자에게 전파되어 전역에서 캐치됨
    - foo 실행 컨텍스트(에러 발생) -> bar 실행 컨텍스트 -> baz 실행 컨텍스트 -> 전역 실행 컨텍스트
  - throw된 에러를 캐치하지 않으면 호출자 방향으로 전파
  - throw된 에러를 캐치하여 적절히 대응하면 프로그램을 강제 종료시키지 않고 코드의 실행 흐름 복구 가능
  - throw된 에러를 어디에서도 캐치하지 않으면 프로그램은 강제 종료
- 주의할 점: 비동기 함수인 setTimeout이나 프로미스 후속 처리 메서드의 콜백 함수는 호출자가 없음
  - setTimeout이나 프로미스 후속 처리 메서드의 콜백 함수는 태스크 큐나 마이크로태스크 큐에 일시 저장되었다가 콜 스택이 비면 이벤트 루프에 의해 콜 스택으로 푸시되어 실행
  - 이때 콜스택에 푸시된 콜백 함수의 실행 컨텍스트는 콜 스택의 가장 하부에 존재 -> 에러를 전파할 호출자 존재 X

<br>
<br>