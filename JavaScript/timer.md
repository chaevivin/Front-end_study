# 타이머

## 1. 호출 스케줄링
- 함수를 명시적으로 호출하면 함수가 즉시 실행되지만 함수 호출 예약 가능
- 호출 스케줄링 : 함수를 명시적으로 호출하지 않고 일정 시간이 경과된 이후에 호출되도록 함수 호출을 예약하려면 타이머 함수 사용
- 타이머 생성할 수 있는 타이머 함수 : setTimeout, setInterval
  - 일정 시간이 경과된 이후 콜백 함수가 호출되도록 타이머를 생성
  - 타이머 함수가 생성한 타이머가 만료되면 콜백 함수 호출
  - setTimeout 함수가 생성한 타이머는 단 한 번 동작 -> 콜백 함수는 타이머가 만료되면 단 한 번 호출
  - setInterval 함수가 생성한 타이머는 반복 동작 -> 콜백 함수는 타이머가 만료될 때마다 반복 호출
- 타이머 제거할 수 있는 타이머 함수 : clearTimeout, clearInterval
- 타이머 함수는 호스트 객체
  - ECMAScript 사양에 정의된 빌트인 함수 X
  - 브라우저 환경과 Node.js 환경에서 모두 전역 객체의 메서드로서 타이머 함수를 제공
- 자바스크립트 엔진은 **싱글 스레드**로 동작
  - 싱글 스레드로 동작 : 단 하나의 실행 컨텍스트 스택을 갖기 때문에 두 가지 이상의 태스크를 동시에 수행 X
  - 그래서 타이머 함수 setTimeOut과 setInterval은 **비동기 처리 방식**으로 동작

<br>
<br>

## 2. 타이머 함수

<br>

### 2.1. setTimeout/clearTimeout
- setTimeout
  - 이 함수는 두 번째 인수로 전달받은 시간(ms, 1/1000초)으로 단 한 번 동작하는 타이머 생성
  - 타이머가 만료되면 첫 번째 인수로 전달받은 콜백 함수가 호출
  - 콜백 함수는 두 번째 인수로 전달받은 시간 이후 단 한 번 실행되도록 호출 스케줄링
  ```js
  const timeoutId = setTimeout(func|code[, delay, param1, param2, ...]);
  ```
  | 매개변수 | 설명 |
  | :--- | :--- |
  | func | 타이머가 만료된 뒤 호출될 콜백 함수 <br> ※ 콜백 함수 대신 코드를 문자열로 전달 가능. 이때 코드 문자열은 타이머가 만료된 뒤 해석되고 실행. 이는 흡사 eval 함수와 유사하며 권장 X |
  | delay | 타이머가 만료 시간(밀리초 단위). setTimeout 함수는 delay 시간으로 단 한 번 동작하는 타이머를 생성. 인수 전달을 생략한 경우 기본값 0 지정. <br> ※ delay 시간이 설정된 타이머가 만료되면 콜백 함수가 즉시 호출되는 것이 보장되지 않음. delay 시간은 태스크 큐에 콜백 함수를 등록하는 시간을 지연할 뿐임. <br> ※ delay가 4ms 이하인 경우 최소 지연 시간 4ms가 지정 |
  | param1, param2, ... | 호출 스케줄링된 콜백 함수에 전달해야 할 인수가 존재하는 경우 세 번째 이후의 인수로 전달 가능 <br> ※ IE9  이하에서는 콜백 함수에 인수 전달 불가 |

  <br>

  ```js
  // 1초(1000ms) 후 타이머가 만료되면 콜백 함수가 호출
  setTimeout(() => console.log('Hi!'), 1000);

  // 1초(1000ms) 후 타이머가 만료되면 콜백 함수가 호출
  // 이때 콜백 함수에 'Lee'가 인수로 전달
  setTimeout(() => console.log(`Hi! ${name}.`), 1000, 'Lee');

  // 두 번째 인수(delay)를 생략하면 기본값 0이 지정
  setTimeout(() => console.log('Hello'));
  ```
- setTimeout 함수는 생성된 타이머를 식별할 수 있는 고유한 타이머 id 반환
  - setTimeout 함수가 반환한 타이머 id는 브라우저 환경인 경우 숫자,  Node.js 환경인 경우 객체

<br>

- clearTimeout
  - 이 함수는 호출 스케줄링 취소
  - setTimeout 함수가 반환한 타이머 id를 clearTimeout 함수의 인수로 전달하여 타이머 취소 가능
  ```js
  const timerId = setTimeout(() => console.log('Hi'), 1000);

  // 타이머 취소
  // 타이머가 취소되면 setTimeout 함수의 콜백 함수 실행 X
  clearTimeout(timerId);
  ```

<br>

### 2.2. setInterval/clearInterval
- setInterval
  - 이 함수는 두 번째 인수로 전달받은 시간(ms, 1/1000초)으로 반복 동작하는 타이머 생성
  - 이후 타이머가 만료될 때마다 첫 번째 인수로 전달받은 콜백 함수 반복 호출 -> 타이머 취소될 때까지 계속
  - 콜백 함수는 두 번째 인수로 전달받은 시간이 경과할 때마다 반복 실행되도록 호출 스케줄링
  - 함수에 전달할 인수는 setTimeout 함수와 동일
  ```js
  const timerId = setInterval(func|code[, delay, param1, param2, ...]);
  ```
  - 생성된 타이머를 식별할 수 있는 고유한 타이머 반환
  - setInterval 함수가 반환한 타이머 id는 브라우저 환경인 경우 숫자, Node.js 환경인 경우 객체

<br>

- clearInterval
  - 이 함수는 호출 스케줄링 취소
  - setInterval 함수가 반환한 타이머 id를 clearInterval 함수의 인수로 전달하여 타이머 취소 가능
  ```js
  let count = 1;

  const timeoutId = setInterval(() => {
    console.log(count);    // 1 2 3 4 5
    // count가 5가 되면 타이머 취소
    // 타이머가 취소되면 setInterval 함수의 콜백 함수 실행 X
    if (count++ === 5) clearInterval(timeoutId);
  }, 1000);
  ```

<br>
<br>

## 3. 디바운스와 스로틀
- 디바운스와 스로틀 : 짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서 과도한 이벤트 핸들러의 호출을 방지하는 프로그래밍 기법
  - scroll, resize, input, mousemove 같은 이벤트는 짧은 시간 간격으로 연속해서 발생
  - 이러한 이벤트에 바인딩한 이벤트 핸들러는 과도하게 호출되어 성능에 문제 발생 가능

<br>

```html
<!DOCTYPE html>
<html>
  <body>
    <button>click</button>
    <pre>일반 클릭 이벤트 카운터 <span class="normal-msg">0</span></pre>
    <pre>디바운스 클릭 이벤트 카운터 <span class="debounce-msg">0</span></pre>
    <pre>스로틀 클릭 이벤트 카운터 <span class="throttle-msg">0</span></pre>
    <script>
      const $button = document.querySelector('button');
      const $normalMsg = document.querySelector('.normal-msg');
      const $debounceMsg = document.querySelector('.debounce-msg');
      const $throttleMsg = document.querySelector('.throttle-msg');

      const debounce = (callback, delay) => {
        let timerId;
        return event => {
          if (timerId) clearTimeout(timerId);
          timerId = setTimeout(callback, delay, event);
        };
      };

      const throttle = (callback, delay) => {
        let timerId;
        return event => {
          if (timerId) return;
          timerId = setTimeout(() => {
            callback(event);
            timerId = null;
          }, delay, event);
        };
      };

      $button.addEventListener('click', () => {
        $normalMsg.textContent = +$normalMsg.textContent + 1;
      });

      $button.addEventListener('click', debounce(() => {
        $debounceMsg.textContent = +$debounceMsg.textContent + 1;
      }, 500));

      $button.addEventListener('click', throttle(() => {
        $throttleMsg.textContent = +$throttleMsg.textContent + 1;
      }, 500));
    </script>
  </body>
</html>
```
<div align="center">

![4](https://github.com/chaevivin/React-study/assets/83055813/318d996d-dcfe-443d-a132-1007b748e099)

일반적인 이벤트 핸들러와 디바운스, 스로틀된 이벤트 핸들러의 호출 빈도
</div>

- 디바운스와 스로틀은 이벤트를 처리할 때 매우 유용
- 디바운스와 스로틀의 구현에는 타이머 함수가 사용

<br>

### 3.1. 디바운스
- 디바운스(debounce)는 짧은 시간 간격으로 이벤트가 연속해서 발생하면 이벤트 핸들러를 호출하지 않다가 일정 시간이 경과한 이후에 이벤트 핸들러가 한 번만 호출되도록 함
- 디바운스는 짧은 시간 간격으로 발생하는 이벤트를 그룹화해서 마지막 한 번만 이벤트 핸들러가 호출되도록 함

<br>

```html
<!DOCTYPE html>
<html>
  <body>
    <input type="text">
    <div class="msg"></div>
    <script>
     const $input = document.querySelector('input');
     const $msg = document.querySelector('.msg');

     const debounce = (callback, delay) => {
      let timerId;
      // debounce 함수는 timerId를 기억하는 클로저 반환
      return event => {
        // delay가 경과하기 이전에 이벤트가 발생하면 이전 타이머를 취소하고 새로운 타이머 재설정
        // 따라서 delay보다 짧은 간격으로 이벤트가 발생하면 callback은 호출 X
        if (timerId) clearTimeout(timerId);
        timerId = setTimeout(callback, delay, event);
      };
     };

     // debounce 함수가 반환하는 클로저가 이벤트 핸들러로 등록됨
     // 300ms보다 짧은 간격으로 input 이벤트가 발생하면 debounce 함수의 콜백 함수는
     // 호출되지 않다가 300ms 동안 input 이벤트가 더 이상 발생하지 않으면 한 번만 호출
     $input.oninput = debounce(e => {
      $msg.textContent = e.target.value;
     }, 300);
    </script>
  </body>
</html>
```
위의 코드를 살펴보면,
- input 이벤트는 사용자가 텍스트 입력 필드에 값을 입력할 때마다 연속해서 발생
- input의 이벤트 핸들러가 사용자가 입력 필드에 입력한 값으로 Ajax 요청과 같은 무거운 처리를 수행한다면 사용자가 아직 입력을 완료하지 않았어도 Ajax 요청 -> 서버에 부담을 주는 불필요한 처리, 사용자가 입력을 완료했을 때 한 번만 Ajax 요청을 전송하는 것이 바람직
- 사용자가 입력을 완료했는지 여부는 정확히 알 수 없으므로 일정 시간 동안 텍스트 입력 필드에 값을 입력하지 않으면 입력이 완료된 것으로 간주
  - debounce 함수가 반환한 함수는 debounce 함수에 두 번째 인수로 전달한 시간(delay)보다 짧은 간격으로 이벤트가 발생하면 이전 타이머를 취소하고 새로운 타이머 재설정
  - delay보다 짧은 간격으로 이벤트가 연속해서 발생하면 debounce 함수의 첫 번째 인수로 전달한 콜백 함수는 호출되지 않다가 delay 동안 input 이벤트가 더 이상 발생하지 않으면 한 번만 호출

<br>

- 디바운스는 짧은 시간 간격으로 이벤트가 연속해서 발생하면 이벤트 핸들러를 호출하지 않다가 일정 시간 동안 이벤트가 발생하지 않으면 이벤트 핸들러가 한 번만 호출되도록 함
- resize 이벤트 처리, input 요소에 입력된 값으로 ajax 요청하는 입력 필드 자동완성 UI 구현, 버튼 중복 클릭 방지 처리 등에 유용하게 사용
- 위 예제는 완전하지 않으므로 실무에서는 Underscore의 debounce 함수난 Lodash의 debounce 함수 사용 권장

<br>

### 3.2. 스로틀
- 스로틀(throttle)은 짧은 시간 간격으로 이벤트가 연속해서 발생하더라도 일정 시간 간격으로 이벤트 핸들러가 최대 한 번만 호출되도록 함
- 스로틀은 짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서 일정 시간 단위로 이벤트 핸들러가 호출되도록 호출 주기를 만듦

<br>

```html
<!DOCTYPE html>
<html>
  <body>
    <div class="container">
      <div class="content"></div>
    </div>
    <div>
      일반 이벤트 핸들러가 scroll 이벤트를 처리한 횟수
      <span class="normal-count">0</span>
    </div>
    <div>
      스로틀 이벤트 핸들러가 scroll 이벤트를 처리한 횟수
      <span class="throttle-count">0</span>
    </div>
    <script>
      const $container = document.querySelector('.container');
      const $normalCount = document.querySelector('.normal-count');
      const $throttleCount = document.querySelector('.throttle-count');

      const throttle = (callback, delay) => {
        let timerId;
        // throttle 함수는 timerId를 기억하는 클로저 반환
        return event => {
          // delay가 경과하기 이전에 이벤트가 발생하면 아무것도 하지 않다가
          // delay가 경과했을 때 이벤트가 발생하면 새로운 타이머 재설정
          // 따라서 delay 간격으로 callback 호출
          if (timerId) setTimeout(() => {
            callback(event);
            timerId = null;
          }, delay, event);
        };
      };

      let normalCount = 0;
      $container.addEventListener('scroll', () => {
        $normalCount.textContent = ++normalCount;
      });

      let throttleCount = 0;
      // throttle 함수가 반환하는 클로저가 이벤트 핸들러로 등록
      $container.addEventListener('scroll', () => {
        $throttleCount.textContent = ++throttleCount;
      }, 100);
    </script>
  </body>
</html>
```
위의 코드를 살펴보면,
- scroll 이벤트는 사용자가 스크롤할 때 짧은 시간 간격으로 연속해서 발생
- 짧은 시간 간격으로 연속해서 발생하는 이벤트의 과도한 이벤트 핸들러의 호출을 방지하기 위해 throttle 함수는 이벤트를 그룹화해서 일정 시간 단위로 이벤트 핸들러가 호출되도록 호출 주기를 만듦
- throttle 함수가 반환한 함수는 throttle 함수에 두 번째 인수로 전달한 시간(delay)이 경과하기 이전에 이벤트가 발생하면 아무것도 하지 않다가 delay 시간이 경과했을 때 이벤트가 발생하면 콜백 함수를 호출하고 새로운 타이머 재설정
- delay 시간 간격으로 콜백 함수 호출

<br>

- 스로틀은 scroll 이벤트 처리나 무한 스크롤 UI 구현 등에 유용하게 사용
- 위 예제는 완전하지 않으므로 실무에서는 Underscroe의 throttle 함수나 Lodash의 throttle 함수 사용 권장

<br>
<br>