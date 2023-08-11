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