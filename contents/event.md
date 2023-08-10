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
      <button onClick="sayHi('Lee')">Click me</button>
      <script>
        function sayHi(name) {
          console.log(`Hi! ${name}`);
        }
      </script>
    </body>
  </html>
  ```
- 주의할 점 : 이벤트 핸들러 어트리뷰트 값으로 함수 참조가 아닌 함수 호출문 등의 문을 할당