# 이벤트

## 1. 이벤트 드리븐 프로그래밍
- 브라우저는 처리해야 할 특정 사건이 발생하면 이를 감지하여 이벤트를 발생시킴
  - 예 : 클릭, 키보드 입력, 마우스 이동 등
- 애플리케이션이 특정 타입의 이벤트에 대해 반응하여 어떤 일을 하고 싶다면 해당하는 타입의 이벤트가 발생했을 때 호출될 함수를 브라우저에게 알려 호출을 위임
  - **이벤트 핸들러(event handler)** : 이벤트가 발생했을 때 호출될 함수
  - **이벤트 핸들러 등록** : 이벤트가 발생했을 때 브라우저에게 이벤트 핸들러의 호출을 위임하는 것
- 브라우저는 사용자의 버튼 클릭을 감지하여 클릭 이벤트 발생 
  - 특정 버튼 요소에서 클릭 이벤트가 발생하면 특정 함수(이벤트 핸들러)를 호출하도록 브라우저에게 위임(이벤트 핸들러 등록)
  - 함수를 언제 호출할지 알 수 없으므로 개발자가 명시적으로 함수를 호출하는 것이 아니라 브라우저에게 함수 호출을 위임
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <button>Click me</button>
      <script>
        const $button = document.querySelector('button');

        // 사용자가 버튼을 클릭하면 함수를 호출하도록 요청
        $button.onClick = () => { alert('button click'); };
      </script>
    </body>
  </html>
  ```
  - 이벤트 핸들러 프로퍼티에 함수를 할당하면 해당 이벤트가 발생했을 때 할당한 함수가 브라우저에 의해 호출
- 이벤트와 그에 대응하는 함수(이벤트 핸들러)를 통해 사용자와 애플리케이션은 상호작용 가능
- **이벤트 드리븐 프로그래밍** : 프로그램의 흐름을 이벤트 중심으로 제어하는 프로그래밍 방식

<br>
<br>

## 2. 이벤트 타입
- 이벤트 타입은 이벤트 종류를 나타내는 문자열

<br>

### 2.1. 마우스 이벤트
| 이벤트 타입 | 이벤트 발생 시점 |
| :--- | :--- |
| click | 마우스 버튼을 클릭했을 때 |
| dbclick | 마우스 버튼을 더블 클릭했을 때 |
| mousedown | 마우스 버튼을 눌렀을 때 |
| mouseup | 누르고 있던 마우스 버튼을 놓았을 때 |
| mousemove | 마우스 커서를 움직였을 때 |
| mouseenter | 마우스 커서를 HTML 요소 안으로 이동했을 때(버블링되지 않음) |
| mouseover | 마우스 커서를 HTML 요소 안으로 이동했을 때(버블링됨) |
| mouseleave | 마우스 커서를 HTML 요소 밖으로 이동했을 때(버블링되지 않음) |
| mouseout | 마우스 커서를 HTML 요소 밖으로 이동했을 때(버블링됨) |

<br>

### 2.2. 키보드 이벤트
| 이벤트 타입 | 이벤트 발생 시점 |
| :--- | :--- |
| keydown | 모든 키를 눌렀을 때 발생 <br> ※ control, option, shift, tab, delete, enter, 방향 키와 문자, 숫자, 특수 문자 키를 눌렀을 때 발생. 단, 문자, 숫자, 특수 문자, enter 키를 눌렀을 때는 연속적으로 발생하지만 그 외의 키를 눌렀을 때는 한 번만 발생 |
| keypress | 문자 키를 눌렀을 때 연속적으로 발생 <br> ※ control, option, shift, tab, delete, enter, 방향 키 등을 눌렀을 때는 발생하지 않고 문자, 숫자, 특수 문자, enter 키를 눌렀을 때만 발생. 폐지(deprecated)되었으므로 사용 권장 X |
| keyup | 누르고 있던 키를 놓았을 때 한 번만 발생 <br> ※ keydown 이벤트와 마찬가지로 control, option, shift, tab, delete, enter, 방향 키와 문자, 숫자, 특수 문자 키를 놓았을 때 발생 |

<br>

### 2.3. 포커스 이벤트
| 이벤트 타입 | 이벤트 발생 시점 |
| :--- | :--- |
| focus | HTML 요소가 포커스 받았을 때(버블링되지 않음) |
| blur | HTML 요소가 포커스를 잃었을 때(버블링되지 않음) |
| focusin | HTML 요소가 포커스를 받았을 때(버블링됨) |
| focusout | HTML 요소가 포커스를 잃었을 때(버블링됨) |
- focusin, focusout 이벤트 핸들러를 이벤트 핸들러 프로퍼티 방식으로 등록하면 크롬, 사파리에서 정상 동작 X -> addEventListener 메서드 방식 사용

<br>

### 2.4. 폼 이벤트
| 이벤트 타입 | 이벤트 발생 시점 |
| :--- | :--- |
| submit | 1. form 요소 내의 input(text, checkbox, radio), selelct 입력 필드(textarea 제외)에서 엔터키를 눌렀을 때 <br> 2. form 요소 내의 submit 버튼(< button >, < input type='submit' >)을 클릭했을 때 <br> ※ submit 이벤트는 form 요소에서 발생 |
| reset | form 요소 내의 reset 버튼을 클릭했을 때(최근에는 사용 X) |

<br>

### 2.5. 값 변경 이벤트
| 이벤트 타입 | 이벤트 발생 시점 |
| :--- | :--- |
| input | input(text, checkbox, radio), select, textarea 요소 값이 입력되었을 때 |
| change | input(text, checkbox, radio), selelct, textarea 요소의 값이 변경되었을 때 <br> ※ change 이벤트는 input 이벤트와는 달리 HTML 요소가 포커스를 잃었을 때 사용자 입력이 종료되었다고 인식하여 발생. 즉, 사용자가 입력을 하고 있을 때는 input 이벤트가 발생하고 사용자 입력이 종료되어 값이 변경되면 change 이벤트 발생 |
| readystatechange | HTML 문서의 로드와 파싱 상태를 나타내는 document.readyState 프로퍼티 값('loading', 'interactive', 'complete')이 변경될 때 |

<br>

### 2.6. DOM 뮤테이션 이벤트
| 이벤트 타입 | 이벤트 발생 시점 |
| :--- | :--- |
| DOMContentLoaded | HTML 문서의 로드와 파싱이 완료되어 **DOM 생성이 완료**되었을 때 |

<br>

### 2.7. 뷰 이벤트
| 이벤트 타입 | 이벤트 발생 시점 |
| :--- | :--- |
| resize | 브라우저 윈도우(window)의 크기를 리사이즈할 때 연속적으로 발생 <br> ※ 오직 window 객체에서만 발생 |
| scroll | 웹페이지(document) 또는 HTML 요소를 스크롤할 때 연속적으로 발생 |

<br>

### 2.8. 리소스 이벤트
| 이벤트 타입 | 이벤트 발생 시점 |
| load | **DOMContentLoaded** 이벤트가 발생한 이후, 모든 리소스(이미지, 폰트)의 로딩이 완료되었을 때(주로 window 객체에서 발생) |
| unload | 리소스가 언로드될 때(주로 새로운 웹페이지를 요청한 경우) |
| abort | 리소스 로딩이 중단되었을 때 |
| error | 리소스 로딩이 실패했을 때 |

<br>
<br>

## 3. 이벤트 핸들러 등록
- 이벤트 핸들러 등록하는 방법은 3가지 -> 이벤트 핸들러 어트리뷰트 방식, 이벤트 핸들러 프로퍼티 방식, addEventListener 메서드 방식

<br>

### 3.1. 이벤트 핸들러 어트리뷰트 방식
- HTML 요소의 어트리뷰트 중에는 이벤트에 대응하는 이벤트 핸들러 어트리뷰트가 존재
- 이벤트 핸들러 어트리뷰트의 이름 = on 접두사 + 이벤트 종류를 나타내는 이벤트 타입
- 이벤트 핸들러 어트리뷰트 값으로 함수 호출문 등의 문을 할당하면 이벤트 핸들러 등록
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <button onclick="sayHi('Lee')">Click me</button>
      <script>
        function sayHi(name) {
          console.log(`Hi! ${name}`);
        }
      </script>
    </body>
  </html>
  ```
- 주의할 점 : 이벤트 핸들러 어트리뷰트 값으로 함수 참조가 아닌 함수 호출문 등의 문을 할당
- 이벤트 핸들러를 등록할 때 콜백 함수와 마찬가지로 함수 참조를 등록해야 브라우저가 이벤트 핸들러 호출 가능 -> 하지만 이벤트 핸들러 어트리뷰트 값으로 함수 호출문을 할당
  - 함수 참조가 아니라 함수 호출문을 등록하면 함수 호출문의 평가 결과가 이벤트 핸들러로 등록
  - 함수가 아닌 값을 반환하는 함수 호출문을 이벤트 핸들러로 등록하면 브라우저가 이벤트 핸들러 호출 불가
- **이벤트 핸들러 어트리뷰트 값은 암묵적으로 생성될 이벤트 핸들러의 함수 몸체를 의미**
  - onclick="sayHi('Lee')" 어트리뷰트는 파싱되어 함수를 암묵적으로 생성하고, 이벤트 핸들러 어트리뷰트 이름과 동일한 onclick 이벤트 핸들러 프로퍼티에 할당
  - 위처럼 동작하는 이유는 이벤트 핸들러에 인수를 전달하기 위해서
  - 이벤트 핸들러 어트리뷰트 값으로 여러 개의 문 할당 가능
- HTML과 자바스크립트를 분리하는 것이 좋기 때문에 이 방법은 권장 X
- 하지만 CBD(Component Based Development) 방식의 Angular/React/Svelte/Vue.js 같은 프레임워크/라이브러리에서는 이벤트 핸들러 어트리뷰트 방식으로 이벤트 처리
  ```html
  <!-- Angular -->
  <button (click)="handleClick($event)">Save</button>

  <!-- React -->
  <button onClick={handleClick}>Save</button>

  <!-- Svelte -->
  <button on:click={handleClick}>Save</button>

  <!-- Vue.js -->
  <button v-on:click="handleClick($event)">Save</button>
  ```

<br>

### 3.2. 이벤트 핸들러 프로퍼티 방식
- window 객체와 Document, HTMLElement 타입의 DOM 노드 객체는 이벤트에 대응하는 이벤트 핸들러 프로퍼티를 가짐
- 이벤트 핸들러 프로퍼티의 키 = on 접두사 + 이벤트의 종류를 나타내는 이벤트 타입
- 이벤트 핸들러 프로퍼티에 함수를 바인딩하면 이벤트 핸들러가 등록
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <button>Click me</button>
      <script>
        const $button = document.querySelector('button');

        $button.onclick = function () {
          console.log('button click');
        };
      </script>
    </body>
  </html>
  ```
- 이벤트 핸들러를 등록하기 위해서는 **이벤트 타깃(button), 이벤트 타입(click), 이벤트 핸들러(function)** 지정
  - 이벤트 타깃 : 이벤트를 발생시킬 객체
  - 이벤트 타입 : 이벤트의 종류를 나타내는 문자열
- 장점 : 이벤트 핸들러 어트리뷰트 방식의 HTML과 자바스크립트가 뒤섞이는 문제 해결
- 단점 : 이벤트 핸들러 프로퍼티에 하나의 이벤트 핸들러만 바인딩 가능
  - 두 개의 이벤트 핸들러를 하나의 프로퍼티에 바인딩 했을 경우 마지막 이벤트 핸들러에 의해 재할당되어 실행 X
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <button>Click me</button>
      <script>
        const $button = document.querySelector('button');

        // 실행 X
        $button.onclick = function () {
          console.log('button clicked 1');
        };

        // 실행
        $button.onclick = function () {
          console.log('button clicked 2');
        };
      </script>
    </body>
  </html>
  ```

<br>

### 3.3. addEventListener 메서드 방식
- EventTarget.prototype.addEventListener
- 이 메서드를 사용하여 이벤트 핸들러 등록 가능
- 매개변수
  - 첫 번째 매개변수 : 이벤트 종류를 나타내는 문자여린 이벤트 타입 전달
    - 이벤트 핸들러 프로퍼티 방식과 달리 on 접두사 붙이지 않음
  - 두 번째 매개변수 : 이벤트 핸들러 전달
  - 마지막 매개변수 : 이벤트를 캐치할 이벤트 전파 단계(캡처링 또는 버블링)를 지정
    - 생략 or false 지정 : 버블링 단계에서 이벤트 캐치
    - true 지정 : 캡처링 단계에서 이벤트 캐치
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <button>Click me</button>
      <script>
        const $button = document.querySelector('button');

        $button.addEventListener('click', function () {
          console.log('button click');
        });
      </script>
    </body>
  </html>
  ```
- addEventListener 메서드에는 이벤트 핸들러를 인수로 전달
- addEventListener 메서드 방식은 이벤트 핸들러 프로퍼티에 바인딩된 이벤트 핸들러에 영향 X -> 하나의 이벤트 핸들러만 등록할 수 있는 프로퍼티 방식은 addEventListener를 사용하면 여러 개 등록 가능
  - 이벤트 핸들러는 등록된 순서대로 호출
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <button>Click me</button>
      <script>
        const $button = document.querySelector('button');

        // 이벤트 핸들러 프로퍼티 방식
        $button.onclick = function () {
          console.log('button clicked 2');
        };

        // addEventListener 메서드 방식
        $button.addEventListener('click', function () {
          console.log('button click');
        });
      </script>
    </body>
  </html>
  ```
- addEventListener 메서드를 통해 참조가 동일한 이벤트 핸들러를 중복 등록하면 하나의 이벤트 핸들러만 등록
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <button>Click me</button>
      <script>
        const $button = document.querySelector('button');

        const handleClick = () => console.log('button click');

        // 하나만 등록됨
        $button.addEventListener('click', handleClick);
        $button.addEventListener('click', handleClick);
      </script>
    </body>
  </html>
  ```

<br>
<br>

## 4. 이벤트 핸들러 제거
- EventTarget.prototype.removeEventListener
- addEventListener 메서드로 등록한 이벤트 핸들러 제거
- removeEventListener 메서드에 전달할 인수는 addEventListener와 동일
  - addEventListener 메서드에 전달한 인수와 removeEventListener 메서드에 전달한 인수가 일치하지 않으면 이벤트 핸들러 제거 X
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <button>Click me</button>
      <script>
        const $button = document.querySelector('button');

        const handleClick = () => console.log('button click');

        // 이벤트 핸들러 등록
        $button.addEventListener('click', handleClick);

        // 이벤트 핸들러 제거
        $button.removeEventListener('click', handleClick, true);    // 실패
        $button.removeEventListener('click', handleClick);    // 성공
      </script>
    </body>
  </html>
  ```
- removeEventListener 메서드에 인수로 전달한 이벤트 핸들러는 addEventListener 메서드에 인수로 전달한 등록 이벤트 핸들러와 동일한 함수이어야 하기 때문에 무명 함수를 이벤트 핸들러로 등록한 경우 제거 불가
  - 이벤트 핸들러를 제거하려면 이벤트 핸들러의 참조를 변수나 자료구조에 저장하고 있어야 함
  ```js
  // 제거 불가
  $button.addEventListener('click', () => console.log('button click'));
  ```
- 기명 이벤트 핸들러 내부에서 removeEventListener 메서드를 호출하여 이벤트 핸들러 제거 가능
  - 이벤트 핸들러는 단 한 번만 호출
  ```js
  $button.addEventListener('click', function foo() {
    console.log('button click');
    $button.removeEventListener('click', foo);
  });
  ```
- 기명 함수를 이벤트 핸들러로 등록할 수 없다면 호출된 함수, 즉 함수 자신을 가리키는 arguments.callee 사용 
  - arguments.callee는 코드 최적화를 방해하므로 strict mode에서는 사용 금지
  - 가급적 이벤트 핸들러의 참조를 변수나 자료구조에 저장하여 제거
  ```js
  // 무명 함수를 이벤트 핸들러로 등록
  $button.addEventListener('click', function () {
    console.log('button click');
    $button.removeEventListener('click', arguments.callee);
  });
  ```
- 이벤트 핸들러 프로퍼티 방식으로 등록한 이벤트 핸들러는 removeEventListener 메서드로 제거 불가
  - 이벤트 핸들러 프로퍼티 방식으로 등록한 이벤트 핸들러를 제거하려면 이벤트 핸들러 프로퍼티에 null 할당
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <button>Click me</button>
      <script>
        const $button = document.querySelector('button');

        const handleClick = () => console.log('button click');

        // 이벤트 핸들러 등록
        $button.onclick = handleClick;

        // 이벤트 핸들러 제거 불가
        $button.removeEventListener('click', handleClick);  
        
        // 이벤트 핸들러 제거
        $button.onclick = null;
      </script>
    </body>
  </html>
  ```

<br>
<br>

## 5. 이벤트 객체
- 이벤트가 발생하면 이벤트에 관련한 다양한 정보를 담고 있는 이벤트 객체 동적 생성
- **생성된 이벤트 객체는 이벤트 핸들러의 첫 번째 인수로 전달**
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <p>클릭하세요. 클릭한 곳의 좌표가 표시됩니다.</p>
      <em class="message"></em>
      <script>
        const $msg = document.querySelector('.message');

        function showCoords(e) {
          $msg.textContent = `clientX: ${e.clientX}, clientY: ${e.clientY}`;
        }

        document.onclick = showCoords;
      </script>
    </body>
  </html>
  ```
  - 클릭 이벤트에 의해 생성된 이벤트 객체는 이벤트 핸들러의 첫 번째 인수로 전달되어 매개변수 e에 암묵적으로 할당 -> 브라우저가 이벤트 핸들러를 호출할 때 이벤트 객체를 인수로 전달하기 때문
  - 이벤트 객체를 전달받으려면 이벤트 핸들러를 정의할 때 이벤트 객체를 전달받을 매개변수를 명시적으로 선언 (e)
- 이벤트 핸들러 어트리뷰트 방식으로 이벤트 핸들러를 등록했다면 event를 통해 이벤트 객체 전달받음
  - 이벤트 핸들러의 첫 번째 매개변수 이름은 반드시 event -> event가 아니면 이벤트 객체 전달받지 못함
  - 이벤트 핸들러의 첫 번째 매개변수 이름이 event로 암묵적으로 명명
  ```html
  <!DOCTYPE html>
  <html>
    <head>
      <style>
        html, body { height: 100%; }
      </style>
    </head>
    <body onclick="showCoords(event)">
      <p>클릭하세요. 클릭한 곳의 좌표가 표시됩니다.</p>
      <em class="message"></em>
      <script>
        const $msg = document.querySelector('.message');

        function showCoords(e) {
          $msg.textContent = `clientX: ${e.clientX}, clientY: ${e.clientY}`;
        }
      </script>
    </body>
  </html>
  ```

<br>

### 5.1. 이벤트 객체의 상속 구조
- 이벤트 객체는 상속 구조를 가짐
  - Event, UIEvent, MouseEvent 등 모두 생성자 함수 -> 생성자 함수를 호출하여 이벤트 객체 생성 가능
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <script>
        // Event 생성자 함수를 호출하여 foo 이벤트 타입의 Event 객체 생성
        let e = new Event('foo');
        console.log(e);    // Event {isTrusted: false, type: "foo", target: null, ...}
        console.log(e.type);    // "foo"
        console.log(e instanceof Event);    // true
        console.log(e instanceof Object);    // true

        // FocusEvent 생성자 함수를 호출하여 focus 이벤트 타입의 FocusEvent 객체 생성
        e = new FocusEvent('focus');
        console.log(e);    // FocusEvent {isTrusted: false, relatedTarget: null, view: null, ...}

        // MouseEvent 생성자 함수를 호출하여 click 이벤트 타입의 MouseEvent 객체 생성
        e = new MouseEvent('click');
        console.log(e);    // MouseEvent {isTrusted: false, screenX: 0, screenY: 0, clientX: 0, ...}

        // KeyboardEvent 생성자 함수를 호출하여 keyup 이벤트 타입의 KeyboardEvent 객체 생성
        e = new KeyboardEvent('keyup');
        console.log(e);    // KeyboardEvent {isTrusted: false, key: "", code: "", ctrlKey: false, ...}

        // InputEvent 생성자 함수를 호출하여 change 이벤트 타입의 InputEvent 객체 생성
        e = new InputEvent('change');
        console.log(e);    // InputEvent {isTrusted: false, data: null, inputType: "", ...}
      </script>
    </body>
  </html>
  ```
- 생성된 이벤트 객체는 생성자 함수와 더불어 생성되는 프로토타입으로 구성된 프로토타입 체인의 일원이 됨
- 이벤트 객체 중 일부는 사용자 행위에 의해 생성된 것이고 일부는 자바스크립트 코드에 의해 인위적으로 생성된 것
  - MouseEvent 타입의 이벤트 객체는 사용자가 마우스를 클릭하거나 이동했을 때 생성하는 이벤트 객체
  - CustomEvent 타입의 이벤트 객체는 자바스크립트 코드에 의해 인위적으로 생성한 이벤트 객체
- Event 인터페이스는 DOM 내에서 발생한 이벤트에 의해 생성되는 이벤트 객체
  - Event 인터페이스에는 모든 이벤트 객체의 공통 프로퍼티가 정의
  - FoucsEvent, MouseEvent, KeyboardEvent, WheelEvent 같은 하위 인터페이스에는 이벤트 타입에 따라 고유한 프로퍼티가 정의
  - 이벤트 객체의 프로퍼티는 발생한 이벤트 타입에 따라 달라짐
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <input type="text">
      <input type="checkbox">
      <button>Click</button>
      <script>
        const $input = document.querySelector('input[type=text]');
        const $checkbox = document.querySelector('input[type=checkbox]');
        const $button = document.querySelector('button');

        // load 이벤트가 발생하면 Event 타입의 이벤트 객체 생성
        window.onload = console.log;

        // change 이벤트가 발생하면 Event 타입의 이벤트 객체 생성
        $checkbox.onchange = console.log;

        // focus 이벤트가 발생하면 FocusEvent 타입의 이벤트 객체 생성
        $input.onfocus = console.log;

        // input 이벤트가 발생하면 InputEvent 타입의 이벤트 객체 생성
        $input.oninput = console.log;

        // keyup 이벤트가 발생하면 KeyboardEvent 타입의 이벤트 객체 생성
        $input.onkeyup = console.log;

        // click 이벤트가 발생하면 MouseEvent 타입의 이벤트 객체 생성
        $button.onclick = console.log;
      </script>
    </body>
  </html>
  ```

<br>

### 5.2. 이벤트 객체의 공통 프로퍼티
- Event 인터페이스, 즉 Event.prototype에 정의되어 있는 이벤트 관련 프로퍼티는 모든 파생 이벤트 객체에 상속됨
  - Event 인터페이스의 이벤트 관련 프로퍼티는 모든 이벤트 객체가 상속받는 공통 프로퍼티
  - 이벤트 객체의 공통 프로퍼티

  | 공통 프로퍼티 | 설명 | 타입 |
  | :--- | :--- | :--- |
  | type | 이벤트 타입 | string |
  | target | 이벤트를 발생시킨 DOM 요소 | DOM 요소 노드 |
  | currentTarget | 이벤트 핸들러가 바인딩된 DOM 요소 | DOM 요소 노드 |
  | eventPhase | 이벤트 전파 단계 <br> 0: 이벤트 없음, 1: 캡처링 단계, 2: 타깃 단계, 3: 버블링 단계 | number |
  | bubbles | 이벤트를 버블링으로 전파하는지 여부. 다음 이벤트는 bubbles: false로 버블링하지 않음. <br> - 포커스 이벤트 focus/blur <br> - 리소스 이벤트 load/unload/abort/error <br> - 마우스 이벤트 mouseenter/mouseleave | boolean |
  | cancleable | preventDefault 메서드를 호출하여 이벤트의 기본 동작을 취소할 수 있는지 여부. 다음 이벤트는 cancelable: false로 취소 불가능 <br> - 포커스 이벤트 focus/blur <br> - 리소스 이벤트 load/unload/abort/error <br> - 마우스 이벤트 dbclick/mouseenter/mouseleave | boolean |
  | defaultPrevented | preventDefault 메서드를 호출하여 이벤트를 취소했는지 여부 | boolean |
  | isTrusted | 사용자의 행위에 의해 발생한 이벤트인지 여부. 예를 들어, click 메서드 또는 dispatchEvent 메서드를 통해 인위적으로 발생시킨 이벤트의 경우 isTrutsed는 false | boolean |
  | timeStamp | 이벤트가 발생한 시각(1970/01/01/00:00:0부터 경과한 밀리초) | number |

<br>

### 5.3. 마우스 정보 취득
- click, dbclick, mousedown, mouseup, mousemove, mouseenter, mouseleave 이벤트가 발생하면 생성되는 MouseEvent 타입의 이벤트 객체는 고유의 프로퍼티를 가짐
  - 마우스 포인터 좌표 정보를 나타내는 프로퍼티 : screenX/screenY, clientX/clientY, pageX/pageY, offsetX/offsetY
    - clientX/clientY는 뷰포트를 기준으로 마우스 포인터 좌표를 나타냄
  - 버튼 정보를 나타내는 프로퍼티 : altKey, ctrlKey, shiftKey, button

<br>

### 5.6. 키보드 정보 취득
- keydown, keyup, keypress 이벤트가 발생하면 생성되는 KeyboardEvent 타입의 이벤트 객체는 altKey, ctrlKey, shiftKey, metaKey, key, keyCode 같은 고유의 프로퍼티를 가짐

<br>
<br>

## 6. 이벤트 전파
- 이벤트 전파 : DOM 트리 상에 존재하는 DOM 요소 노드에서 발생한 이벤트는 DOM 트리를 통해 전파
  ```HTML
  <!DOCTYPE html>
  <html>
    <body>
      <ul id="fruits">
        <li id="apple">Apple</li>
        <li id="banana">Banana</li>
        <li id="orange">Orange</li>
      </ul>
    </body>
  </html>
  ```
  - ul 요소의 두 번째 자식 요소인 li 요소를 클릭하면 클릭 이벤트가 발생
  - **이때 생성된 이벤트 객체는 이벤트를 발생시킨 DOM 요소인 이벤트 타깃을 중심으로 DOM 트리를 통해 전파**
- 이벤트 전파 단계 : 이벤트 객체가 전파되는 방향 기준
  - 캡처링 단계 : 이벤트가 상위 요소에서 하위 요소 방향으로 전파
  - 타깃 단계 : 이벤트가 이벤트 타깃에 도달
  - 버블링 단계 : 이벤트가 하위 요소에서 상위 요소 방향으로 전파
  ```HTML
  <!DOCTYPE html>
  <html>
    <body>
      <ul id="fruits">
        <li id="apple">Apple</li>
        <li id="banana">Banana</li>
        <li id="orange">Orange</li>
      </ul>
      <script>
        const $fruits = document.getElementById('fruits');

        // #fruits 요소의 하위 요소인 li 요소를 클릭한 경우
        $fruits.addEventListener('click', e => {
          console.log(`이벤트 단계: ${e.eventPhase}`);    // 버블링 단계
          console.log(`이벤트 타깃: ${e.target}`);    // [object HTMLLIElement]
          console.log(`커런트 타깃: ${e.currentTarget}`);    // [object HTMLUListElement]
        });
      </script>
    </body>
  </html>
  ```
  - ul 요소의 하위 요소인 li 요소를 클릭하여 이벤트 발생
  - 이벤트 타깃(event.target) -> li 요소, 커런트 타깃(event.currentTarget) -> ul 요소
  - li 요소를 클릭하면 클릭 이벤트가 발생하여 클릭 이벤트 객체가 생성되고 클릭된 li 요소가 이벤트 타깃이 됨
    - 캡처링 단계 : 클릭 이벤트 객체는 window에서 시작해서 이벤트 타깃 방향으로 전파
    - 타깃 단계 : 이벤트 객체는 이벤트를 발생시킨 이벤트 타깃에 도달
    - 버블링 단계 : 이벤트 객체는 이벤트 타깃에서 시작해서 window 방향으로 전파
- 이벤트 핸들러 어트리뷰트/프로퍼티 방식으로 등록한 이벤트 핸들러 -> 타깃 단계와 버블링 단계의 이벤트만 캐치
- addEventListener 메서드 방식으로 등록한 이벤트 핸들러 -> 타깃 단계와 버블링 단계뿐만 아니라 캡처링 단계의 이벤트도 선별적으로 캐치 가능
  - 캡처링 단계의 이벤트를 캐치하려면 addEventListener 메서드의 3번째 인수로 true 전달
  - 3번째 인수를 생략하거나 false 전달하면 타깃 단계와 버블링 단계의 이벤트만 캐치 가능

<br>

```html
<!DOCTYPE html>
  <html>
    <body>
      <ul id="fruits">
        <li id="apple">Apple</li>
        <li id="banana">Banana</li>
        <li id="orange">Orange</li>
      </ul>
      <script>
        const $fruits = document.getElementById('fruits');
        const $banana = document.getElementById('banana');

        // #fruits 요소의 하위 요소인 li 요소를 클릭한 경우 캡처링 단계의 이벤트를 캐치
        $fruits.addEventListener('click', e => {
          console.log(`이벤트 단계: ${e.eventPhase}`);    // 캡처링 단계
          console.log(`이벤트 타깃: ${e.target}`);    // [object HTMLLIElement]
          console.log(`커런트 타깃: ${e.currentTarget}`);    // [object HTMLUListElement]
        });

        // 타깃 단계의 이벤트를 캐치
        $banana.addEventListener('click', e => {
          console.log(`이벤트 단계: ${e.eventPhase}`);    // 타깃 단계
          console.log(`이벤트 타깃: ${e.target}`);    // [object HTMLLIElement]
          console.log(`커런트 타깃: ${e.currentTarget}`);    // [object HTMLUListElement]
        });

        // 버블링 단계의 이벤트를 캐치
        $fruits.addEventListener('click', e => {
          console.log(`이벤트 단계: ${e.eventPhase}`);    // 버블링 단계
          console.log(`이벤트 타깃: ${e.target}`);    // [object HTMLLIElement]
          console.log(`커런트 타깃: ${e.currentTarget}`);    // [object HTMLUListElement]
        });
      </script>
    </body>
  </html>
```
위 예제를 살펴보면,
- 이벤트 핸들러가 캡처링 단계의 이벤트를 캐치하도록 설정 
  - 이벤트 핸들러는 window에서 시작해서 이벤트 타깃 방향으로 전파되는 이벤트 객체 캐치
  - 이벤트를 발생시킨 이벤트 타깃과 이벤트 핸들러가 바인딩된 커런트 타깃이 같은 DOM 요소라면 이벤트 핸들러는 타깃 단계의 이벤트 객체 캐치
- **이벤트는 이벤트를 발생시킨 이벤트 타깃은 물론 상위 DOM 요소에서도 캐치 가능**
  - DOM 트리를 통해 전파되는 이벤트 패스에 위치한 모든 DOM 요소에서 캐치 가능

<br>

- 대부분의 이벤트는 캡처링과 버블링을 통해 전파
  - 버블링으로 전파되지 않는 이벤트
    - 포커스 이벤트 : focus/blur
    - 리소스 이벤트 : load/unload/abort/error
    - 마우스 이벤트 : mouseenter/mouseleave
  - 버블링되지 않는 이벤트는 이벤트 객체의 공통 프로퍼티 event.bubbles의 값이 모두 false
  - 버블링되지 않는 이벤트는 이벤트 타깃의 상위 요소에서 이벤트를 캐치하려면 캡처링 단계의 이벤트를 캐치
  - 버블링되지 않는 이벤트를 상위 요소에서 캐치할 경우 대체할 수 있는 이벤트 존재
    - focus/blur -> focusin/focusout
    - mouseenter/mouseleave -> mouseover/mouseout
- 캡처링 단계의 이벤트와 버블링 단계의 이벤트를 캐치하는 이벤트 핸들러가 혼용되는 경우
  ```html
  <!DOCTYPE html>
  <html>
    <head>
    <style>
        html, body { height: 100%; }
      </style>
    <body>
      <p>버블링과 캡처링 이벤트 <button>버튼</button></p>
      <script>
        // 버블링 단계의 이벤트를 캐치
        document.body.addEventListener('click', () => {
          console.log('Handler for body');
        });

        // 캡처링 단계의 이벤트를 캐치
        document.querySelector('p').addEventListener('click', () => {
          console.log('Handler for paragraph');
        });

        // 타깃 단계의 이벤트를 캐치
        document.querySelector('button').addEventListener('click', () => {
          console.log('Handler for button');
        });
      </script>
    </body>
  </html>
  ```
  - body 요소는 버블링 단계의 이벤트만 캐치
  - p 요소는 캡처링 단계의 이벤트만 캐치
  - button 요소에서 클릭 이벤트가 발생했을 때
    - 캡처링 단계를 캐치하는 p 요소의 이벤트 핸들러 호출
    - 그 후 버블링 단계의 이벤트를 캐치하는 body요소의 이벤트 핸들러 순차적으로 호출
    ```
    Handler for paragraph
    Handler for button
    Handler for body
    ```
  - p 요소에서 클릭 이벤트가 발생하면 캡처링 단계를 캐치하는 p 요소의 이벤트 핸들러가 호출
  - 버블링 단계를 캐치하는 body 요소의 이벤트 핸들러가 순차적으로 호출
    ```
    Handler for paragraph
    Handler for body
    ```

<br>
<br>

## 7. 이벤트 위임
- 사용자가 내비게이션 아이템(li 요소)을 클릭하여 선택하면 현재 선택된 내비게이션 아이템에 active 클래스를 추가하고 그 외의 모든 내비게이션 아이템의 active 클래스를 제거하는 예제
  ```html
  <!DOCTYPE html>
  <html>
    <head>
      <style>
        #fruits {
          display: flex;
          list-style-type: none;
          padding: 0;
        }

        #fruits li {
          width: 100px;
          cursor: pointer;
        }

        #fruits .active {
          color: red;
          text-decoration: underline;
        }
      </style>
    </head>
    <body>
      <nav>
        <ul id="fruits">
          <li id="apple" class="active">Apple</li>
          <li id="banana">Banana</li>
          <li id="orange">Orange</li>
        </ul>
      </nav>
      <div>선택된 내비게이션 아이템: <em class="msg">apple</em></div>
      <script>
        const $fruits = document.getElementById('fruits');
        const $msg = document.querySelector('.msg');

        function activate({ target }) {
          [...$fruits.children].forEach($fruits => {
            $fruits.classList.toggle('active', $fruits === target);
            $msg.textContent = target.id;
          });
        }

        // 모든 li 요소에 이벤트 핸들러 등록
        document.getElementById('apple').onclick = activate;
        document.getElementById('banana').onclick = activate;
        document.getElementById('orange').onclick = activate;
      </script>
    </body>
  </html>
  ```
  - 모든 내비게이션 아이템이 클릭 이벤트에 반응하도록 모든 내비게이션 아이템에 이벤트 핸들러인 activate 등록
  - 만약 내비게이션 아이템이 100개라면 100개의 이벤트 핸들러 등록 -> 많은 DOM 요소에 이벤트 핸들러를 등록하므로 성능 저하의 원인, 유지 보수에도 부적합

<br>

- 이벤트 위임 : 여러 개의 하위 DOM 요소에 각각 이벤트 핸들러를 등록하는 대신 하나의 상위 DOM 요소에 이벤트 핸들러를 등록하는 방법
  - 이벤트 위임을 통해 상위 DOM 요소에 이벤트 핸들러를 등록하면 여러 개의 하위 DOM 요소에 이벤트 핸들러를 등록할 필요 X
  - 동적으로 하위 DOM 요소를 추가하더라도 일일이 추가된 DOM 요소에 이벤트 핸들러를 등록할 필요 X
  ```html
  <!DOCTYPE html>
  <html>
    <head>
      <style>
        #fruits {
          display: flex;
          list-style-type: none;
          padding: 0;
        }

        #fruits li {
          width: 100px;
          cursor: pointer;
        }

        #fruits .active {
          color: red;
          text-decoration: underline;
        }
      </style>
    </head>
    <body>
      <nav>
        <ul id="fruits">
          <li id="apple" class="active">Apple</li>
          <li id="banana">Banana</li>
          <li id="orange">Orange</li>
        </ul>
      </nav>
      <div>선택된 내비게이션 아이템: <em class="msg">apple</em></div>
      <script>
        const $fruits = document.getElementById('fruits');
        const $msg = document.querySelector('.msg');

        function activate({ target }) {
          // 이벤트를 발생시킨 요소(target)가 ul#fruits의 자식 요소가 아니라면 무시
          if (!target.matches('#fruits > li')) return;

          [...$fruits.children].forEach($fruits => {
            $fruits.classList.toggle('active', $fruits === target);
            $msg.textContent = target.id;
          });
        }

        // 이벤트 위임 : 상위 요소(ul#fruits)는 하위 요소의 이벤트 캐치
        $fruits.onclick = activate;
      </script>
    </body>
  </html>
  ```
- 이벤트 위임을 통해 하위 DOM 요소에서 발생한 이벤트를 처리할 때 주의할 점
  - 상위 요소에 이벤트 핸들러를 등록하기 때문에 이벤트 타깃, 즉 이벤트를 실제로 발생시킨 DOM 요소가 개발자가 기대한 DOM 요소가 아닐 수도 있음
  - 따라서 이벤트에 반응이 필요한 DOM 요소에 한정하여 이벤트 핸들러가 실행되도록 이벤트 타깃 검사 필요
- Element.prototype.matches
  - 이 메서드는 인수로 전달된 선택자에 의해 특정 노드를 탐색 가능한 지 확인
    ```js
    function activate({ target }) {
      // 이벤트를 발생시킨 요소(target)가 ul#fruits의 자식 요소가 아니라면 무시
      if (!target.matches('#fruits > li')) return;
      ...
    ```
- 이벤트 위임을 통해 DOM 요소에 이벤트를 바인딩한 경우 이벤트 객체의 target 프로퍼티와 currentTarget 프로퍼티는 다른 DOM 요소를 가리킬 수 있음
  - $fruits 요소의 하위 요소에서 클릭 이벤트가 발생한 경우 이벤트 객체의 currentTarget 프로퍼티와 target 프로퍼티는 다른 DOM 요소를 가리킴

<br>

## 8. DOM 요소의 기본 동작 조작

<BR>

### 8.1. DOM 요소의 기본 동작 중단
- DOM 요소는 저마다 기본 동작이 있음
- 이벤트 객체의 preventDefault 메서드는 이러한 DOM 요소의 기본 동작 중단
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <a href="https://www.google.com">go</a>
      <input type="checkbox">
      <script>
        document.querySelector('a').onclick = e => {
          // a 요소의 기본 동작 중단
          e.preventDefault();
        };

        document.querySelector('input[type=checkbox]').onclick = e => {
          // checkbox 요소의 기본 동작 중단
          e.preventDefault();
        };
      </script>
    </body>
  </html>
  ```

<br>

### 8.2. 이벤트 전파 방지
- 이벤트 객체의 stopPropagation 메서드는 이벤트 전파 중지
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <div class="container">
        <button class="btn1">Button1</button>
        <button class="btn2">Button2</button>
        <button class="btn3">Button3</button>
      </div>
      <script>
        // 이벤트 위임. 클릭된 하위 버튼 요소의 color를 변경
        document.querySelector('.container').onclick = ({ target }) => {
          if (!target.matches('.container > button')) return;
          target.style.color = 'red';
        };

        // .btn2 요소는 이벤트를 전파하지 않으므로 상위 요소에서 이벤트 캐치 불가
        document.querySelector('.btn2').onclick = e => {
          e.stopPropagation();    // 이벤트 전파 중단
          e.target.style.color = 'blue';
        };
      </script>
    </body>
  </html>
  ```
  - 상위 DOM 요소인 container 요소에 이벤트 위임
    - 하위 DOM 요소에서 발생한 클릭 이벤트를 상위 DOM 요소인 container 요소가 캐치하여 이벤트 처리
    - 하위 요소 중에서 btn2 요소는 자체적으로 이벤트 처리 -> btn2 요소는 자신이 발생시킨 이벤트가 전파되는 것을 중단하여 자신에게 바인딩된 이벤트 핸들러만 실행
- stopPropagation 메서드는 하위 DOM 요소의 이벤트를 개별적으로 처리하기 위해 이벤트 전파 중단

<br>
<br>

## 9. 이벤트 핸들러 내부의 this

<br>

### 9.1. 이벤트 핸들러 어트리뷰트 방식
```html
<!DOCTYPE html>
<html>
  <body>
    <button onclick="handleClick()">Click</button>
    <script>
      function handleClick() {
        console.log(this);    // window
      }
    </script>
  </body>
</html>
```
위 예제를 살펴보면,
- handleClick 함수 내부의 this는 전역 객체 window를 가리킴
- 이벤트 핸들러 어트리뷰트의 값으로 지정한 문자열은 암묵적으로 생성되는 이벤트 핸들러 문
  - handleClick 함수는 이벤트 핸들러에 의해 일반 함수로 호출
  - 일반 함수로서 호출되는 함수 내부의 this는 전역 객체를 가리킴

<br>

```html
<!DOCTYPE html>
<html>
  <body>
    <button onclick="handleClick(this)">Click</button>
    <script>
      function handleClick(button) {
        console.log(button);    // 이벤트를 바인딩한 button 요소
        console.log(this);    // window
      }
    </script>
  </body>
</html>
```
위 예제를 살펴보면,
- 이벤트 핸들러를 호출할 때 인수로 전달한 this는 이벤트를 바인딩한 DOM 요소를 가리킴
- handleClick 함수에 전달한 this는 암묵적으로 생성된 이벤트 핸들러 내부의 this
  - 이벤트 핸들러 어트리뷰트 방식에 의해 암묵적으로 생성된 이벤트 핸들러 내부의 this는 이벤트를 바인딩한 DOM 요소를 가리킴
  - 이벤트 핸들러 프로퍼티 방식과 동일

<br>

### 9.2. 이벤트 핸들러 프로퍼티 방식과 addEventListener 메서드 방식
- 이벤트 핸들러 프로퍼티 방식과 addEventListener 메서드 방식 모두 이벤트 핸들러 내부의 this는 이벤트를 바인딩한 DOM 요소를 가리킴
  - 이벤트 핸들러 내부의 this = 이벤트 객체의 currentTarget 프로퍼티
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <button class="btn1">0</button>
      <button class="btn2">0</button>
      <script>
        const $button1 = document.querySelector('.btn1');
        const $button2 = document.querySelector('.btn2');

        // 이벤트 핸들러 프로퍼티 방식
        $button1.onclick = function (e) {
          // this는 이벤트를 바인딩한 DOM 요소를 가리킴
          console.log(this);    // $button1
          console.log(e.currentTarget);    // $button1
          console.log(this === e.currentTarget);    // true

          ++this.textContent;    // 1증가
        }

        // addEventListener 방식
        $button2.addEventListener('click', function (e) {
          // this는 이벤트를 바인딩한 DOM 요소를 가리킴
          console.log(this);    // $button2
          console.log(e.currentTarget);    // $button2
          console.log(this === e.currentTarget);    // true

          ++this.textContent;    // 1증가
        });
      </script>
    </body>
  </html>
  ```

<br>

- 화살표 함수로 정의한 이벤트 핸들러 내부의 this는 상위 스코프의 this를 가리킴
  - 화살표 함수는 함수 자체의 this 바인딩을 갖지 않음
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <button class="btn1">0</button>
      <button class="btn2">0</button>
      <script>
        const $button1 = document.querySelector('.btn1');
        const $button2 = document.querySelector('.btn2');

        // 이벤트 핸들러 프로퍼티 방식
        $button1.onclick = e => {
          // 화살표 함수 내부의 this는 상위 스코프의 this를 가리킴
          console.log(this);    // window
          console.log(e.currentTarget);    // $button1
          console.log(this === e.currentTarget);    // false

          ++this.textContent;    // NaN (undefined + 1)
        }

        // addEventListener 방식
        $button2.addEventListener('click', e => {
          // 화살표 함수 내부의 this는 상위 스코프의 this를 가리킴
          console.log(this);    // window
          console.log(e.currentTarget);    // $button2
          console.log(this === e.currentTarget);    // false

          ++this.textContent;    // NaN (undefined + 1)
        });
      </script>
    </body>
  </html>
  ```

<br>

- 클래스에서 이벤트 핸들러 바인딩하는 경우 this에 주의
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <button class="btn">0</button>
      <script>
        class App {
          constructor() {
            this.$button = document.querySelector('.btn');
            this.count = 0;

            // increase 메서드를 이벤트 핸들러로 등록
            this.$button.onclick = this.increase;
          }

          increase() {
            // 이벤트 핸들러 increase 내부의 this는 DOM 요소(this.$button)를 가리킴
            // 따라서 this.$button은 this.$button.$button과 같음
            this.$button.textContent = ++this.count;    // TypeError: Cannot set property 'textContent' of undefined
          }
        }

        new App();
      </script>
    </body>
  </html>
  ```
  - increase 메서드 내부의 this는 클래스가 생성할 인스턴스를 가리키지 않음
  - 이벤트 핸들러 내부의 this는 이벤트를 바인딩한 DOM 요소를 가리키기 때문에 increase 메서드 내부의 this는 this.$button을 가리킴
  - increase 메서드를 이벤트 핸들러로 바인딩할 때 bind 메서드를 사용해 this를 전달하여 increase 메서드 내부의 this가 클래스가 생성할 인스턴스를 가리키도록 해야 함
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <button class="btn">0</button>
      <script>
        class App {
          constructor() {
            this.$button = document.querySelector('.btn');
            this.count = 0;

            // increase 메서드를 이벤트 핸들러로 등록
            // this.$button.onclick = this.increase;

            // increase 메서드 내부의 this가 인스턴스를 가리키도록 해야 함
            this.$button.onclick = this.increase.bind(this);
          }

          increase() {
            this.$button.textContent = ++this.count;
          }
        }

        new App();
      </script>
    </body>
  </html>
  ```
- 클래스 필드에 할당한 화살표 함수를 이벤트 핸들러로 등록하여 이벤트 핸들러 내부의 this가 인스턴스를 가리키도록 할 수 있음
  - 이벤트 핸들러 increase는 프로토타입 메서드가 아니라 인스턴스 메서드
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <button class="btn">0</button>
      <script>
        class App {
          constructor() {
            this.$button = document.querySelector('.btn');
            this.count = 0;

            // 화살표 함수인 increase를 이벤트 핸들러로 등록
            this.$button.onclick = this.increase;
          }

          // 클래스 필드 정의
          // increase는 인스턴스 메서드이며 내부의 this는 인스턴스를 가리킴
          increase = () => this.$button.textContent = ++this.count;
        }

        new App();
      </script>
    </body>
  </html>
  ```

<br>
<br>

## 10. 이벤트 핸들러에 인수 전달
- 함수에 인수를 전달하려면 함수를 호출할 때 전달
- 이벤트 핸들러 어트리뷰트 방식은 함수 호출문을 사용할 수 있기 때문에 인수 전달 가능
- 이벤트 핸들러 프로퍼티 방식과 addEventListener 메서드 방식의 경우 이벤트 핸들러를 브라우저가 호출하기 때문에 함수 호출문이 아닌 함수 자체를 등록 -> 인수 전달 불가
  - 이벤트 핸들러 내부에서 함수를 호출하면서 인수 전달하면 가능
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <label>User name <input type='text'></label>
      <em class='message'></em>
      <script>
        const MIN_USER_NAME_LENGTH = 5;    // 이름의 최소 길이
        const $input = document.querySelector('input[type=text]');
        const $msg = document.querySelector('.message');

        const checkUserNameLength = min => {
          $msg.textContent = $input.value.length < min ? `이름은 ${min}자 이상 입력해주세요` : '';
        };

        // 이벤트 핸들러 내부에서 함수 호출하면서 인수 전달
        $input.onblur = () => {
          checkUserNameLength(MIN_USER_NAME_LEGTH);
        };
      </script>
    </body>
  </html>
  ```
  - 이벤트 핸들러를 반환하는 함수를 호출하면서 인수 전달 가능
    - checkUserNameLength 함수는 함수를 반환하기 때문에 $input.onblur에는 checkUserNameLength 함수가 반환하는 함수 바인딩
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <label>User name <input type='text'></label>
      <em class='message'></em>
      <script>
        const MIN_USER_NAME_LENGTH = 5;    // 이름의 최소 길이
        const $input = document.querySelector('input[type=text]');
        const $msg = document.querySelector('.message');

        // 이벤트 핸들러를 반환하는 함수
        const checkUserNameLength = min => e => {
          $msg.textContent = $input.value.length < min ? `이름은 ${min}자 이상 입력해주세요` : '';
        };

        // 이벤트 핸들러를 반환하는 함수
        $input.onblur = checkUserNameLength(MIN_USER_NAME_LEGTH);
      </script>
    </body>
  </html>
  ```

<br>
<br>

## 11. 커스텀 이벤트

<br>

### 11.1. 커스텀 이벤트 생성
- 이벤트가 발생하면 암묵적으로 생성되는 이벤트 객체는 발생한 이벤트 종류에 따라 이벤트 타입 결정
- 하지만 Event, UIEvent, MouseEvent 같은 이벤트 생성자 함수를 호출하여 명시적으로 생성한 이벤트 객체는 임의의 이벤트 타입 지정 가능 
- 커스텀 이벤트 : 개발자의 의도로 생성된 이벤트
- 이벤트 생성자 함수는 첫 번째 인수로 이벤트 타입을 나타내는 문자열을 전달받음
  - 이벤트 타입을 나타내는 문자열
    - 기존 이벤트 타입
    - 임의의 문자열 사용하여 새로운 이벤트 타입 지정 -> CustomEvent 이벤트 생성자 함수 사용
    ```js
    // KeyboardEvent 생성자 함수로 keyup 이벤트 타입의 커스텀 이벤트 객체 생성
    const keyboardEvent = new KeyboardEvent('keyup');
    console.log(keyboardEvent.type);    // keyup

    // CustomEvent 생성자 함수로 foo 이벤트 타입의 커스텀 이벤트 객체 생성
    const customEvent = new CustomEvent('foo');
    console.log(customEvent.type);    // foo
    ```
- 생성된 커스텀 이벤트 객체는 버블링되지 않으며 preventDefault 메서드로 취소 불가능
  - 커스텀 이벤트 객체는 bubbles와 cancelable 프로퍼티 값 = false
  ```js
  // MouseEvent 생성자 함수로 click 이벤트 타입의 커스텀 이벤트 객체 생성
  const customEvent = new MouseEvent('click');
  console.log(customEvent.type);    // click
  console.log(customEvent.bubbles);    // false
  console.log(customEvent.cancelable);    // false
  ```
  - 커스텀 이벤트 객체의 bubbles와 cancelable 프로퍼티를 true로 설정하려면 이벤트 생성자 함수의 두 번째 인수로 bubbles 또는 cancelable 프로퍼티를 갖는 객체 전달
  ```js
  // MouseEvent 생성자 함수로 click 이벤트 타입의 커스텀 이벤트 객체 생성
  const customEvent = new MouseEvent('click', {
    bubbles: true,
    cancelable: true
  });

  console.log(customEvent.bubbles);    // true
  console.log(customEvent.cancelable);    // true
  ```
  - 커스텀 이벤트 객체에는 bubbles 또는 cancelable 프로퍼티뿐만 아니라 이벤트 타입에 따라 가지는 이벤트 고유의 프로퍼티 값 지정 가능
  ```js
  // MouseEvent 생성자 함수로 click 이벤트 타입의 커스텀 이벤트 객체 생성
  const mouseEvent = new MouseEvent('click', {
    bubbles: true,
    cancelable: true,
    clientX: 50,
    clientY: 100
  });

  console.log(mouseEvent.clientX);    // 50
  console.log(mouseEvent.clientY);    // 100

  // keyboardEvent 생성자 함수로 keyup 이벤트 타입의 커스텀 이벤트 객체 생성
  const keyboardEvent = new KeyboardEvent('keyup', { key: 'Enter' });

  console.log(keyboardEvent.key);    // Enter
  ```
- 이벤트 생성자 함수로 생성한 커스텀 이벤트는 inTrusted 프로퍼티 값이 언제나 false
  - 커스텀 이벤트가 아닌 사용자 행위에 의해 발생한 이벤트에 의해 생성된 이벤트 객체의 isTrusted 프로퍼티 값은 언제나 true
  ```js
  // InputEvent 생성자 함수로 foo 이벤트 타입의 커스텀 이벤트 객체 생성
  const customEvent = new InputEvent('foo');
  console.log(customEvent.isTrusted);    // false
  ```

<br>

### 11.2. 커스텀 이벤트 디스패치
- 생성된 커스텀 이벤트는 dispatchEvent 메서드로 디스패치(이벤트 발생시티는 행위) 가능
- dispatchEvent 메서드에 이벤트 객체를 인수로 전달하면서 호출하면 인수로 전달한 이벤트 타입의 이벤트 발생
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <button class='btn'>click</button>
      <script>
        const $button = document.querySelector('.btn');

        // 버튼 요소에 foo 커스텀 이벤트 핸들러 등록
        // 커스텀 이벤트를 디스패치하기 전에 이벤트 핸들러 등록
        $button.addEventListener('click', e => {
          console.log(e);    // MouseEvent {isTrusted: false, screenX: 0, ... }
          alert(`${e} clicked`);
        });

        // 커스텀 이벤트 생성
        const customEvent = new MouseEvent('click');

        // 커스텀 이벤트 디스패치(동기 처리). click 이벤트 발생
        $button.dispatchEvent(customEvent);
      </script>
    </body>
  </html>
  ```
- 이벤트 핸들러는 비동기 처리 방식으로 동작
- dispatchEvent 메서드는 이벤트 핸들러를 동기 처리 방식으로 호출
  - dispatchEvent 메서드를 호출하면 커스텀 이벤트에 바인딩된 이벤트 핸들러를 직접 호출하는 것과 같음
  - dispatchEvent 메서드로 이벤트를 디스패치 하기 전에 커스텀 이벤트를 처리할 이벤트 핸들러를 등록
- CustomEvent 이벤트 생성자 함수에는 두 번째 인수로 이벤트와 함께 전달하고 싶은 정보를 담은 detail 프로퍼티를 포함하는 객체 전달 가능
  - 이 정보는 이벤트 객체의 detail 프로퍼티(e.detail)에 담겨 전달
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <button class='btn'>click</button>
      <script>
        const $button = document.querySelector('.btn');

        // 버튼 요소에 foo 커스텀 이벤트 핸들러 등록
        // 커스텀 이벤트를 디스패치하기 전에 이벤트 핸들러 등록
        $button.addEventListener('click', e => {
          // e.detail에는 CustomEvent 함수의 두 번째 인수로 전달한 정보 담겨있음
          alert(e.detail.message);
        });

        // CustomEvent 생성자 함수로 foo 이벤트 타입의 커스텀 이벤트 객체 생성
        const customEvent = new CustomEvent('foo', {
          detail: { message: 'Hello' }    // 이벤트와 함께 전달하고 싶은 정보
        });

        // 커스텀 이벤트 디스패치
        $button.dispatchEvent(customEvent);
      </script>
    </body>
  </html>
  ```
- 임의의 이벤트 타입을 지정하여 커스텀 이벤트 객체를 생성한 경우 반드시 addEventListener 메서드 방식으로 이벤트 핸들러 등록
  - 이벤트 핸들러 어트리뷰트/프로퍼티 방식을 사용하지 못하는 이유
    - 'on + 이벤트 타입'으로 이루어진 이벤트 핸들러 어트리뷰트/프로퍼티가 요소 노드에 존재 X -> 이벤트 핸들러 등록 불가

<br>
<br>