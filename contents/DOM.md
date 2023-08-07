# DOM
- **DOM(Document Object Model)은 HTML 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 API, 즉 프로퍼티와 메서드를 제공하는 트리 자료구조**

<br>
<br>

## 1. 노드

<br>

### 1.1. HTML 요소와 노드 객체
- HTML 요소 : HTML 문서를 구성하는 개별적인 요소
  - HTML 요소는 렌더링 엔진에 의해 파싱되어 DOM을 구성하는 요소 노드 객체로 변환
  - HTML 요소의 어트리뷰트 -> 어트리뷰트 노드, HTML 요소의 텍스트 콘텐츠 -> 텍스트 노드
- HTML 문서는 HTML 요소들의 집합으로 이뤄지며, HTML 요소는 중첩 관계를 가짐
  - HTML 요소의 콘텐츠 영역에는 텍스트뿐만 아니라 다른 HTML 요소 포함 가능
  - HTML 요소 간에는 중첩 관계에 의해 부자 관계 형성
  - HTML 요소 간의 부자 관계를 반영하여 HTML 문서의 구성 요소인 HTML 요소를 객체화한 모든 노드 객체들을 트리 자료구조로 구성

<br>

**트리 자료구조**
- 트리 자료구조는 노드들의 계층적 구조로 이루어짐
  - 트리 자료구조는 부모 노드와 자식 노드로 구성되어 노드 간의 계층적 구조(부자, 형제 관계)를 표현하는 비선형 자료구조
- 하나의 최상위 노드에서 시작
  - 최상위 노드는 부모가 없으며, 루트 노드라고도 불림
  - 루트 노드는 0개 이상의 자식 노드를 가짐
- 리프 노드 : 자식 노드가 없는 노드
- **DOM : 노드 객체들로 구성된 자료구조**
  - 노드 객체의 트리로 구조화되어 있기 때문에 DOM을 **DOM 트리** 라고도 부름

<br>

### 1.2. 노드 객체의 타입
- 노드 객체는 종류가 있고 상속 구조를 가짐
- 노드 객체는 총 12개의 종류(노드 타입)
  - 문서 노드, 요소 노드, 어트리뷰트 노드, 텍스트 노드, Comment 노드, DocumentType 노드, DocumentFragment 노드 등

<BR>

**(1) 문서 노드(document node)**
- 문서 노드는 DOM 트리의 최상위에 존재하는 루트 노드로서 document 객체를 가리킴
- document 객체 : 브라우저가 렌더링한 HTML 문서 전체를 가리키는 객체, 전역 객체 window의 document 프로퍼티에 바인딩 되어 있음
- 문서 노드는 window.document 또는 document로 참조
- 문서 노드는 DOM 트리의 루트 노드이므로 DOM 트리의 노드들에 접근하기 위한 진입점 역할 담당
  - 요소, 어트리뷰트, 텍스트 노드에 접근하려면 문서 노드를 통해야 함

<BR>

**(2) 요소 노드(elelment node)**
- 요소 노드는 HTML 요소를 가리키는 객체
- 요소 노드는 HTML 요소 간의 중첩 관계에 의해 부자 관계를 가지며, 이 부자 관계를 통해 정보 구조화
- 요소 노드는 문서의 구조를 표현

<BR>

**(3) 어트리뷰트 노드(attribute node)**
- 어트리뷰트 노드는 HTML 요소의 어트리뷰트를 가리키는 객체
- 어트리뷰트 노드는 어트리뷰트가 지정된 HTML 요소의 요소 노드와 연결
  - 요소 노드는 부모 노드와 연결되어 있지만 어트리뷰트 노드는 부모 노드와 연결되어 있지 않고 요소 노드에만 연결
  - 어트리뷰트 노드는 부모 노드가 없으므로 요소 노드의 형제 노드 X
- 어트리뷰트 노드에 접근하여 어트리뷰트를 참조하거나 변경하려면 먼저 요소 노드에 접근

<BR>

**(4) 텍스트 노드(text node)**
- 텍스트 노드는 HTML 요소의 텍스트를 가리키는 객체
- 텍스트 노드는 문서의 정보를 표현
- 텍스트 노드는 요소 노드의 자식 노드, 자식 노드를 가질 수 없는 리프 노드
  - 텍스트 노드는 DOM 트리의 최종단
- 텍스트 노드에 접근하려면 먼저 부모 노드인 요소 노드에 접근

<br>

### 1.3. 노드 객체의 상속 구조
- DOM을 구성하는 노드 객체는 자신의 구조와 정보를 제어할 수 있는 DOM API 사용 가능
  - DOM API로 노드 객체는 자신의 부모, 형제, 자식 탐색 가능
  - DOM API로 노드 객체는 자신의 어트리뷰트와 텍스트 조작 가능
- DOM을 구성하는 노드 객체는 브라우저 환경에서 추가적으로 제공하는 호스트 객체
  - 노드 객체도 자바스크립트 객체이므로 프로토타입에 의한 상속 구조를 가짐
- 노드 객체의 상속 구조
  - 모든 노드 객체는 Object, EventTarget, Node 인터페이스 상속
  - 문서는 Document, HTMLDocument 인터페이스 상속
  - 어트리뷰트 노드는 Attr 인터페이스 상속
  - 텍스트 노드는 CharacterData 인터페이스 상속
  - 요소 노드는 Element 인터페이스 상속
    - 요소 노드는 HTMLElement와 태그의 종류별로 세분화된 HTMLHtmlElement, HTMLHeadElement, HTMLBodyElement, HTMLUListElement 등의 인터페이스 상속
- 프로토타입 체인 관점에서 바라본 노드 객체의 상속 구조
  - 예를 들어, input 요소를 파싱하여 객체화한 input 요소 노드 객체
    - HTMLInputElement, HTMLElement, Element, Node, EventTarget, Object의 prototype에 바인딩되어 있는 프로토타입 객체 상속
    - input 요소 노드 객체는 프로토타입 체인에 있는 모든 프로토타입의 프로퍼티나 메서드를 상속받아 사용 가능
  ```html
  <!DOCTYPE html>
  <html>
  <body>
    <input type="text">
    <script>
      // input 요소 노드 객체 선택
      const $input = document.querySelector('input');

      // input 요소 노드 객체의 프로토타입 체인
      console.log(
        Object.getPrototypeOf($input) === HTMLInputElement.prototype,
        Object.getPrototypeOf(HTMLInputElement.prototype) === HTMLElement.prototype,
        Object.getPrototypeOf(HTMLElement.prototype) === Element.prototype,
        Object.getPrototypeOf(Element.prototype) === Node.prototype,
        Object.getPrototypeOf(Node.prototype) === EventTarget.prototype,
        Object.getPrototypeOf(EventTarget.prototype) === Object.prototype
      );    // 모두 true
    </script>
  </body>
  </html>
  ```
  - input 요소 노드 객체는 다양한 특성을 갖는 객체이며, 이러한 특성을 나타내는 기능들을 상속을 통해 제공 받음

    | input 요소 노드 객체의 특성 | 프로토타입을 제공하는 객체 |
    | :--- | :--- |
    | 객체 | Object |
    | 이벤트를 발생시키는 객체 | EventTarget |
    | 트리 자료구조의 노드 객체 | Node |
    | 브라우저가 렌더링할 수 있는 웹 문서의 요소(HTML, XML, SVG)를 표현하는 객체 | Element |
    | 웹 문서 요소 중에서 HTML 요소를 표현하는 객체 | HTMLElement |
    | HTML 요소 중에서 input 요소를 표현하는 객체 | HTMLInputElement |

<br>

- 노드 객체에는 노드 객체의 종류(노드 타입)에 상관없이 모든 노드 객체가 공통으로 갖는 기능도 있고 노드 타입에 따라 고유한 기능 존재
  - EventTarget : 이벤트 관련된 기능(EventTarget.addEventListener, EventTarget.removeEventListenter 등) 제공
  - Node : 트리 탐색 기능(Node.parentNode, Node.childNodes, Node.previousSibling, Node.nextSibiling 등), 노드 정보 제공 기능(Node.nodeType, Node.nodeName 등)
- HTML 요소가 객체화된 요소 노드 객체는 HTML 요소가 갖는 공통적인 기능을 가짐
  - HTMLElement 인터페이스가 기능 제공 : input, div 요소의 style 프로퍼티 등
- 요소 노드 객체는 HTML 요소의 종류에 따라 고유한 기능도 가짐
  - HTML 요소의 종류에 따라 다른 기능을 제공 : input 요소의 value 등
- 노드 객체는 공통된 기능일수록 프로토타입 체인의 상위에, 개별적인 고유 기능일수록 프로토타입 체인의 하위에 프로토타입 체인을 구축하여 노드 객체에 필요한 기능, 즉 프로퍼티 메서드를 제공하는 상속 구조를 가짐
- **DOM은 HTML 문서의 계층적 구조와 정보를 표현하고 노드 객체의 종류(노드 타입)에 따라 필요한 기능을 프로퍼티와 메서드의 집합인 DOM API로 제공**
  - **DOM API를 통해 HTML의 구조나 내용, 스타일 등을 동적으로 조작 가능**

<br>
<br>

## 2. 요소 노드 취득
- HTML의 구조나 내용 또는 스타일 등을 동적으로 조작하려면 먼저 요소 노드를 취득
- 요소 노드 취득은 HTML 요소를 조작하는 시작점
- DOM은 요소 노드를 취득할 수 있는 다양한 메서드 제공

<br>

### 2.1. id를 이용한 요소 노드 취득
- Document.prototype.getElementById
- 이 메서드는 인수로 전달한 id를 어트리뷰트 값을 갖는 하나의 요소 노드를 탐색하여 반환
- getElementById 메서드는 Document.prototype의 프로퍼티 -> 반드시 문서 노드인 document를 통해 호출
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <ul>
        <li id="apple">Apple</li>
        <li id="banana">Banana</li>
        <li id="orange">Orange</li>
      </ul>
      <script>
        // id 값이 banana인 요소 노드 탐색하여 반환
        // 두 번째 요소 노드 반환
        const $elem = document.getElementById('banana');

        // 취득한 요소 노드의 style.color 프로퍼티 값 변경
        $elem.style.color = 'red';
      </script>
    </body>
  </html>
  ```
- id값은 HTML 문서 내에서 유일한 값이어야 하며, class 어트리뷰트와 달리 공백 문자로 구분하여 여러 개의 값을 가질 수 없음
  - HTML 문서 내에서 중복된 id 값을 갖는 HTML 요소가 여러 개 존재하더라도 에러 발생 X
  - HTML 문서 내에는 중복된 id 값을 갖는 요소가 여러 개 존재 가능
  - 중복된 id를 가지고 있으면 getElementById 메서드는 id 값을 갖는 첫 번째 요소 노드만 반환 -> 언제나 단 하나의 요소 노드만 반환
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <ul>
        <li id="banan">Apple</li>
        <li id="banana">Banana</li>
        <li id="banana">Orange</li>
      </ul>
      <script>
        // id 값이 banana인 요소 노드 탐색하여 반환
        // 첫 번째 요소 노드 반환
        const $elem = document.getElementById('banana');

        // 취득한 요소 노드의 style.color 프로퍼티 값 변경
        $elem.style.color = 'red';
      </script>
    </body>
  </html>
  ```
- 인수로 전달된 id 값을 갖는 요소가 존재하지 않는 경우 null 반환
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <ul>
        <li id="apple">Apple</li>
        <li id="banana">Banana</li>
        <li id="orange">Orange</li>
      </ul>
      <script>
        // id 값이 banana인 요소 노드 탐색하여 반환
        const $elem = document.getElementById('grape');

        // 취득한 요소 노드의 style.color 프로퍼티 값 변경
        $elem.style.color = 'red';
        // TypeError: Cannot read property 'style' of null
      </script>
    </body>
  </html>
  ```
- HTML 요소에 id 어트리뷰트를 부여하면 id 값과 동일한 이름의 전역 변수가 암묵적으로 선언되고 해당 노드 객체가 할당되는 부수 효과 존재
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <div id="foo"></div>
      <script>
        // id값과 동일한 이름의 전역 변수가 암묵적으로 선언되고 해당 노드 객체가 할당
        console.log(foo === document.getElementById('foo'));    // true

        // 암묵적 전역으로 생성된 프로퍼티는 삭제되지만 전역 변수는 삭제 X
        delete foo;
        console.log(foo);    // <div id="foo"></div>
      </script>
    </body>
  </html>
  ```
- id 갑과 동일한 이름의 전역 변수가 이미 선언되어 있으면 이 전역 변수에 노드 객체 재할당 X
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <div id="foo"></div>
      <script>
        let foo = 1;

        // id 값과 동일한 이름의 전역 변수가 이미 선언되어 있으면 노드 객체 재할당 X
        console.log(foo);    // 1
      </script>
    </body>
  </html>
  ```

<br>

### 2.2. 태그 이름을 이용한 요소 노드 취득
- Document.prototype/Element.prototype.getElementsByTagName
- 이 메서드는 인수로 전달한 태그 이름을 갖는 모든 요소 노드들을 탐색하여 반환
- getElementsByTagName 메서드는 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 자체인 HTMLCollection 객체 반환
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <ul>
        <li id="apple">Apple</li>
        <li id="banana">Banana</li>
        <li id="orange">Orange</li>
      </ul>
      <script>
        // 태그 이름이 li인 요소 노드 모두 탐색하여 반환
        // 탐색된 노드들은 HTMLCollection 객체에 담겨 반환
        const $elems = document.getElementByTagNames('li');

        [...$elems].forEach(elem => { elem.style.color = 'red'; });
      </script>
    </body>
  </html>
  ```
  - getElementsByTagName 메서드가 반환하는 DOM 컬렉션 객체인 HTMLCollection 객체는 유사 배열 객체이면서 이터러블
- HTML 문서의 모든 요소 노드를 취득하라면 인수로 '*' 전달
  ```js
  const $all = document.getElementByTagName('*');
  ```
- getElementsByTagName 메서드는 Document.prototype에 정의된 메서드와 Element.prototype에 정의된 메서드가 있음
  - Document.prototype.getElementsByTagName 메서드
    - DOM의 루트 노드인 문서 노드, 즉 document를 통해 호출
    - DOM 전체에서 요소 노드 탐색하여 반환
  - Element.prototype.getElementsByTagName 메서드
    - 특정 요소 노드를 통해 호출
    - 특정 요소 노드의 자손 노드 중에서 요소 노드 탐색하여 반환
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <ul id="fruits">
        <li>Apple</li>
        <li>Banana</li>
        <li>Orange</li>
      </ul>
      <ul>
        <li>HTML</li>
      </ul>
      <script>
        const $lisFromDocument = document.getelementsByTagName('li');
        console.log($lisFromDocument);    // HTMLCollection(4) [li, li, li, li]
        
        // ul#fruits 요소의 자손 노드 중에서 태그 이름이 li인 요소 노드를 모두 탐색하여 반환
        const $fruits = document.getElementById('fruits');
        const $lisFromFruits = $fruits.getelementByTagName('li');
        console.log($lisFromFruits);    // HTMLCollection(3) [li, li, li]
      </script>
    </body>
  </html>
  ```
- 인수로 전달된 태그 이름을 갖는 요소가 존재하지 않는 경우 빈 HTMLCollection 객체 반환

<br>

### 2.3. class를 이용한 요소 노드 취득
- Document.prorotype/Element.prototype.getElementsByClassName
- 이 메서드는 인수로 전달한 class 어트리뷰트 값을 갖는 모든 요소 노드들을 탐색하여 반환
- 인수로 전달할 class 값은 공백으로 구분하여 여러 개의 class 지정 가능
- 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 HTMLCollection 객체 반환
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <ul>
        <li class="fruit apple">Apple</li>
        <li class="fruit banana">Banana</li>
        <li class="fruit orange">Orange</li>
      </ul>
      <script>
        // class값이 fruit인 요소 노드를 모두 탐색해 HTMLCollection 객체에 담아 반환
        const $elem = document.getElementsByClassName('fruit');

        // 취득한 모든 요소의 color 프로퍼티 값 변경
        [...$elems].forEach(elem => { elem.style.color = 'red'; });

        // class 값이 fruit apple인 요소 노드를 모드 탐색하여 HTMLCollection 객체에 담아 반환
        const $apples = document.getElementsByClassName('fruit apple');

        [...$apples].forEach(elem => { elem.style.color = 'blue'; });
      </script>
    </body>
  </html>
  ```
- getElementsByClassName 메서드는 Document.prototype에 정의된 메서드와 Element.prototype에 정의된 메서드가 있음
  - Document.prototype.getElementsByClassName 메서드
    - DOM의 루트 노드인 문서, 즉 document를 통해 호출
    - DOM 전체에서 요소 노드를 탐색하여 반환
  - Element.prototype.getElementsByClassName 메서드
    - 특정 요소 노드를 통해 호출
    - 특정 요소 노드의 자손 노드 중에서 요소 노드를 탐색하여 반환
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <ul id="fruits">
        <li class="apple">Apple</li>
        <li class="banana">Banana</li>
        <li class="orange">Orange</li>
      </ul>
      <div class="banana">Banana</div>
      <script>
        // DOM 전체에서 class 값이 'banana'인 요소 노드를 모두 탐색하여 반환
        const $bananasFromDocument = document.getElementsByClassName('banana');
        console.log($banansFromDocument);    // HTMLCollection(2) {li.banana, div.banana}

        // #fruits 요소의 자손 노드 중에서 class 값이 'banana'인 요소 노드를 모두 탐색하여 반환
        const $fruits = document.getElementsByClassName('fruits');
        const $bananaFromFruits = $fruits.getElementsByClassName('banana');

        console.log($banansFromFruits);    // HTMLCollection {li.banana}
      </script>
    </body>
  </html>
  ```
- 인수로 전달된 class 값을 갖는 요소가 존재하지 않는 경우 getElementsByClassName 메서드는 빈 HTMLCollection 객체 반환

<br>

### 2.4. CSS 선택자를 이용한 요소 노드 취득
- CSS 선택자(selector)는 스타일을 적용하고자 하는 HTML 요소를 특정할 대 사용하는 문법
  ```css
  /* 전체 선택자: 모든 요소 선택 */
  * { ... }

  /* 태그 선택자: 모든 p 태그 요소를 모두 선택 */
  p { ... }

  /* id 선택자: id 값이 'foo'인 요소 모두 선택 */
  #foo { ... }

  /* class 선택자: class 값이 'foo'인 요소 모두 선택 */
  .foo { ... }

  /* 어트리뷰트 선택자: input 요소 중에 type 어트리뷰트 값이 'text'인 요소 모두 선택 */
  input[type=text] { ... }

  /* 후손 선택자: div 요소의 후손 요소 중 p 요소 모두 선택 */
  div p { ... }

  /* 자식 선택자: div 요소의 자식 요소 중 p 요소 모두 선택 */
  div > p { ... }

  /* 인접 형제 선택자: p 요소 형제 요소 중에 p 요소 바로 뒤에 위치하는 ul 요소 선택 */
  p + ul { ... }

  /* 일반 형제 선택자: p 요소 형제 요소 중에 p 요소 뒤에 위치하는 ul 요소 모두 선택 */
  p ~ ul { ... }

  /* 가상 클래스 선택자: hover 상태인 a 요소를 모두 선택 */
  a:hover { ... }

  /* 가상 요소 선택자: p 요소의 콘텐츠 앞에 위치하는 공간 선택. 일반적으로 content 프로퍼티와 함께 사용 */
  p::before { ... }
  ```
- Document.prototype/Element.prototype.querySelector 메서드
- 이 메서드는 인수로 전달할 CSS 선택자를 만족시키는 하나의 요소 노드를 탐색하여 반환
  - 인수로 전달한 CSS 선택자를 만족시키는 요소 노드가 여러 개인 경우 첫 번째 요소 노드만 반환
  - 인수로 전달된 CSS 선택자를 만족시키는 요소 노드가 존재하지 않는 경우 null 반환
  - 인수로 전달한 CSS 선택자가 문법에 맞지 않는 경우 DOMException 에러 발생
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <ul id="fruits">
        <li class="apple">Apple</li>
        <li class="banana">Banana</li>
        <li class="orange">Orange</li>
      </ul>
      <script>
        // class 어트리뷰트 값이 'banana'인 첫 번째 요소 노드를 탐색하여 반환
        const $elem = document.querySelector('.banana');

        $elem.style.color = 'red';
      </script>
    </body>
  </html>
  ```

<br>

- Document.prototype/Element.prototype.querySelectorAll 메서드
- 이 메서드는 인수로 전달한 CSS 선택를 만족시키는 모든 요소 노드를 탐색하여 반환
- querySelectorAll 메서드는 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 NodeList 객체 반환
  - NodeList 객체는 유사 배열 객체이면서 이터러블
  - 인수로 전달된 CSS 선택자를 만족시키는 요소가 존재하지 않는 경우 빈 NodeList 객체 반환
  - 인수로 전달한 CSS 선택자가 문법에 맞지 않는 경우 DOMException 에러 발생
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <ul id="fruits">
        <li class="apple">Apple</li>
        <li class="banana">Banana</li>
        <li class="orange">Orange</li>
      </ul>
      <script>
        // ul 요소의 자식 요소인 li 요소를 모두 탐색하여 반환
        const $elems = document.querySelector('ul > li');
        // 취득한 요소 노드들은 NodeList 객체에 담겨 반환
        console.log($elems);    // NodeList(3) [li.apple, li.banana, li.orange]

        $elem.forEach(elem => { elem.style.color = 'red'; });
      </script>
    </body>
  </html>
  ```
- HTML 문서의 모든 요소 노드를 취득하려면 querySelectorAll 메서드의 인수로 전체 선택자 '*'를 전달
  ```js
  const $all = document.querySelectorAll('*');
  ```

<br>

- querySelector, querySelectorAll 메서드는 Document.prototype에 정의된 메서드와 Element.prototype에 정의된 메서드가 있음
  - Document.prototype에 정의된 메서드
    - DOM의 루트 노드인 문서 노드, 즉 document를 통해 호출
    - DOM 전체에서 요소 노드 탐색하여 반환
  - Element.prototype에 정의된 메서드
    - 특정 요소 노드를 통해 호출
    - 특정 요소 노드의 자손 노드 중에서 요소 노드를 탐색하여 반환
- 단점
  - CSS 선택자 문법을 사용하는 두 메서드는 getElementById, getElementBy*** 메서드보다 다소 느림
- 장점
  - CSS 선택자 문법을 사용하여 좀 더 구체적인 조건으로 요소 노드 취득 가능
  - 일관된 방식으로 요소 노드 취득 가능
- id 어트리뷰트가 있는 요소 노드 취득 -> getElementById 메서드 사용, 그 외 -> querySelector, querySelectorAll 메서드 사용 권장

<br>

### 2.5. 특정 요소 노드를 취득할 수 있는지 확인
- Element.prototype.mathces
- 이 메서드는 인수로 전달한 CSS 선택자를 통해 특정 요소 노드를 취득할 수 있는지 확인
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <ul id="fruits">
        <li class="apple">Apple</li>
        <li class="banana">Banana</li>
        <li class="orange">Orange</li>
      </ul>
      <script>
        const $apple = document.querySelector('.apple');

        console.log($apple.matches('#fruits > li.apple'));    // true
        console.log($apple.mathces('#fruits > li.banana'));    // false
      </script>
    </body>
  </html>
  ```
- 이벤트 위임을 사용할 때 유용

<br>

### 2.6. HTMLCollection과 NodeList
- DOM 컬렉션 자체인 HTMLCollection과 NodeList는 DOM API가 여러 개의 결과값을 반환하기 위한 DOM 컬렉션 객체
- 모두 유사 배열 객체이면서 이터러블 
  - for...of 문으로 순회 가능
  - 스프레드 문법을 사용하여 간단히 배열로 변환 가능
- 노드 객체의 상태 변화를 실시간으로 반영하는 **살아있는 객체**
  - HTMLCollection은 언제나 live 객체로 동작
  - NodeList는 대부분의 경우 노드 객체의 상태 변화를 실시간으로 반영하지 않고 과거의 정적 상태를 유지하는 non-live 객체로 동작하지만 경우에 따라 live 객체로 동작할 때 있음

<br>

**(1) HTMLCollection**
- getElemntsByTagName, getElementsByClassName 메서드가 반환
- 노드 객체의 상태 변화를 실시간으로 반영하는 살아 있는 DOM 컬렉션 객체
- 살아있는 객체라고도 불림
- HTMLCollection 객체를 for문으로 순회하면서 노드 객체의 상태를 변경해야 할 때 주의 필요
  ```html
  <!DOCTYPE html>
    <html>
      <head>
        <style>
          .red { color: red; }
          .blue { color: blue; }
        </style>
      </head>
      <body>
        <ul id="fruits">
          <li class="red">Apple</li>
          <li class="red">Banana</li>
          <li class="red">Orange</li>
        </ul>
        <script>
          // class값이 'red'인 요소 노드를 모두 탐색하여 HTMLCollection 객체에 담아 반환
          const $elems = document.getElementsByClassName('red');
          // 이 시점에 HTMLCollection 객체에는 3개의 요소 노드가 담겨 있음
          console.log($elems);    // HTMLCollection(3) {li.red, li.red. li.red}

          // HTMLCollection 객체의 모든 요소의 class 값을 'blue'로 변경
          for (let i = 0; i < $elems.length; i++) {
            $elems[i].className = 'blue';
          }

          // HTMLCollection 객체의 요소가 3개에서 1개로 변경
          console.log($elems);    // HTMLCollection(1) {li.red}
        </script>
      </body>
    </html>
  ```
  - 예상한 결과 : 모든 li 요소의 class 값이 'blue'로 변경
  - 위 예제를 실행해보면 두 번째 요소만 class 값이 변경되지 않음
  - 변경되지 않은 이유 -> $elems.length는 3이므로 for문의 코드 블록 3번 반복
    1. 첫 번째 반복 (i === 0) 
        - $elems[0]은 첫 번째 li 요소
        - 이 요소는 className 프로퍼티에 의해 class값이 'red' -> 'blue' 변경
        - 첫 번째 li 요소는 class 값이 변경되었으므로 getElementByClassName 메서드의 인자로 전달한 'red'와 일치하지 않기 때문에 $elems에서 실시간으로 제거
    2. 두 번째 반복 (i === 1)
        - 첫 번째 반복에서 첫 번째 li 요소가 $elems에서 제거
        - **$elems[1]은 세 번째 li 요소**
        - 세 번째 li 요소의 class 값은 'blue'로 변경되고 HTMLCollection 객체인 $elems에서 실시간으로 제외
    3. 세 번째 반복 (i === 2)
        - 첫 번째, 두 번째 반복에서 첫 번째, 세 번째 li 요소가 $elems에서 제거
        - $elems에는 두 번째 요소 노드만 남음
        - $elems.length는 1이므로 for문의 조건식 i < $elems.length가 false로 평가되어 반복 종료
        - 남아 있는 두 번째 li 요소의 class 값 변경 X
- for문을 역방향으로 순회하면 문제 회피 가능
  ```js
  for (let i = $elems.length - 1; i >= 0; i--) {
    $elems[i].className = 'blue';
  }
  ```
- while 문을 사용하여 HTMLCollection 객체에 노드 객체가 남아 있지 않을 때까지 무한 반복하는 방법으로 문제 회피 가능
  ```js
  let i = 0;
  while($elems.length > i) {
    $elems[i].className = 'blue';
  }
  ```
- 더 간단한 방법은 HTMLCollection 사용 X -> 배열의 고차 함수 사용
  ```js
  [...$elems].forEach(elem => elem.className = 'blue');
  ```

<br>

**(2) NodeList**
- HTMLCollection 객체의 부작용을 해결하기 위해 getElementByTagName, getElementByClassName 메서드 대신 querySelectorAll 사용
- querySelectorAll 메서드는 DOM 컬렉션 객체인 NodeList 객체 반환
  - NodeList 객체는 실시간으로 노드 객체의 상태 변경을 반영하지 않는 객체
- querySelectorAll이 반환하는 NodeList 객체는 NodeList.prototype.forEach 메서드를 상속받아 사용 가능
  - NodeList.prototype은 forEach, item, entries, keys, values 메서드 제공
  ```js
  const $elems = documents.querySelectorAll('.red');

  $elems.forEach(elem => elem.className = 'blue');
  ```
- NodeList 객체는 대부분의 경우 노드 객체의 상태 변경을 실시간으로 반영하지 않고 과거의 정적 상태를 유지하는 non-live 객체로 동작
- **childNodes 프로퍼티가 반환하는 NodeList 객체는 HTMLCollection 객체와 같이 실시간으로 노드 객체의 상태 변경을 반영하는 live 객체로 동작하므로 주의 필요**
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <ul id="fruits">
        <li>Apple</li>
        <li>Banana</li>
      </ul>
    </body>
    <script>
      const $fruits = document.getElementById('fruits');

      // childNodes 프로퍼티는 NodeList 객체(live) 반환
      const { childNodes } = $fruits;
      console.log(childNodes instanceOf NodeList);    // true

      // 공백 텍스트 노드 포함 5개
      console.log(childNodes);    // NodeList(5) [text, li, text, li, text]

      for (let i = 0; i < childNodes.length; i++) {
        // removeChild 메서드는 $fruits 요소의 자식 노드를 DOM에서 삭제
        // removeChild 메서드가 호출될 때마다 NodeList 객체인 childNodes가 실시간으로 변경
        // 따라서 첫 번째, 세 번째, 다섯 번째 요소만 삭제
        $fruits.removeChild(childNodes[i]);
      }

      console.log(childNodes);    // NodeList(2) [li, li]
    </script>
  </html>
  ```
- **노드 객체의 상태 변경과 상관없이 안전하게 DOM 컬렉션을 사용하려면 HTMLCollection이나 NodeList 객체를 배열로 변환하여 사용하는 것을 권장**
  - HTMLCollection이나 NodeList 객체를 배열로 변환하면 배열의 유용한 고차 함수(forEach, map, filter, reduce 등)를 사용할 수 있는 장점 포함
- HTMLCollection과 NodeList 객체는 모두 유사 배열 객체이면서 이터러블
  - 스프레드 문법이나 Array.from 메서드를 사용하여 간단히 배열로 변환 가능
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <ul id="fruits">
        <li>Apple</li>
        <li>Banana</li>
      </ul>
    </body>
    <script>
      const $fruits = document.getElementById('fruits');

      // childNodes 프로퍼티는 NodeList 객체(live) 반환
      const { childNodes } = $fruits;
     
     // 스프레드 문법을 사용하여 NodeList 객체를 배열로 변환
     [...childNodes].forEach(childNode => {
      $fruits.removeChild(childNode);
     });

     // $fruits 요소의 모든 자식 노드가 삭제
     console.log(childNodes);    // NodeList []
    </script>
  </html>
  ```

<br>
<br>

## 3. 노드 탐색
- 요소 노드를 취득한 다음, 취득한 요소 노드를 기점으로 DOM 트리의 노드를 옮겨 다니며 부모, 형제, 자식 노드 등을 탐색해야 할 때가 있음
  ```HTML
  <ul id="fruits">
    <li class="apple">Apple</li>
    <li class="banana">Banana</li>
    <li class="orange">Orange</li>
  </ul>
  ```
  - ul#fruits 요소는 3개의 자식 요소를 가짐
  - 먼저 ul#fruits 요소 노드를 취득한 다음, 자식 노드를 모두 탐색하거나 자식 노드 중 하나만 탐색 가능
  - li.banana 요소는 2개의 형제 요소와 부모 요소를 가짐
  - 먼저 li.banana 요소 노드를 취득한 다음, 형제 노드를 탐색하거나 부모 노드 탐색 가능
- DOM 트리 상의 노드를 탐색할 수 있도록 Node, Element 인터페이스는 트리 탐색 프로퍼티 제공
  - parentNode, previousSibling, firstChild, childNodes 프로퍼티 -> Node.prototype이 제공
  - 프로퍼티 키에 Element가 포함된 previousElementSibling, nextElementSilbing, children 프로퍼티 -> Element.prototype이 제공
- 노드 탐색 프로퍼티는 모드 접근자 프로퍼티
  - 노드 탐색 프로퍼티는 setter 없이 getter만 존재하여 참조만 가능한 읽기 전용 프로퍼티
  - 읽기 전용 프로퍼티에 값을 할당하면 에러 없이 무시됨

<br>

### 3.1. 공백 텍스트 노드
- 공백 텍스트 노드 : HTML 요소 사이의 스페이스, 탭, 줄바꿈(개행) 등의 공백 문자는 텍스트 노드 생성
- 텍스트 에디터에서 HTML 문서에 스페이스 키, 탭 키, 엔터 키 등을 입력하면 공백 문자 추가 -> HTML 문서에도 공백 문자 포함
- HTML 문서의 공백 문자는 공백 텍스트 노드 생성
  - 노드를 탐색할 때는 공백 문자가 생성한 공백 텍스트 노드에 주의
  - 인위적으로 HTML 문서의 공백 문자를 제거하면 공백 텍스트 노드를 생성하지 않지만 가독성이 좋지 않아 권장 X

<br>

### 3.2. 자식 노드 탐색
- 자식 노드를 탐색하기 위해서는 노드 탐색 프로퍼티 사용
  | 프로퍼티 | 설명 |
  | :--- | :--- |
  | Node.prototype.childNodes | 자식 노드를 모두 탐색하여 DOM 컬렉션 객체인 NodeList에 담아 반환. **childNodes 프로퍼티가 반환한 NodeList에는 요소 노드뿐만 아니라 텍스트 노드도 포함될 수 있음** |
  | Element.prototype.children | 자식 노드 중에서 요소 노드만 모두 탐색하여 DOM 컬렉션 객체인 HTMLCollection에 담아 반환. **children 프로퍼티가 반환한 HTMLCollection에는 텍스트 노드 포함 X** |
  | Node.prototype.firstChild | 첫 번째 자식 노드를 반환. firstChild 프로퍼티가 반환한 노드는 텍스트 노드이거나 요소 노드 |
  | Node.prototype.lastChild | 마지막 자식 노드를 반환. lastChild 프로퍼티가 반환한 노드는 텍스트 노드이거나 요소 노드 |
  | Element.prototype.firstElementChild | 첫 번째 자식 요소 노드를 반환. firstElementChild 프로퍼티는 요소 노드만 반환 |
  | Element.prototype.lastElementChild | 마지막 자식 요소 노드 반환. lastElemntChild 프로퍼티는 요소 노드만 반환 |
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <ul id="fruits">
        <li class="apple">Apple</li>
        <li class="banana">Banana</li>
        <li class="orange">Orange</li>
      </ul>
    </body>
    <script>
      const $fruits = document.getElementById('fruits');

      // #fruits 요소의 모든 자식 노드 탐색
      console.log($fruits.childNodes);    // NodeList(7) [text, li.apple, text, li.banana, text, li.orange, text]

      // #fruits 요소의 모든 자식 노드 탐색
      console.log($fruits.children);    // HTMLCollection(3) [li.apple, li.banana, li.orange]

      // #fruits 요소의 첫 번째 자식 노드 탐색
      console.log($fruits.firstChild);    // #text

      // #fruits 요소의 마지막 자식 노드 탐색
      console.log($fruits.lastChild);    // #text

      // #fruits 요소의 첫 번재 자식 노드 탐색
      console.log($fruits.firstElementChild);    // li.apple

      // #fruits 요소의 마지막 자식 노드 탐색
      console.log($fruits.lastElementChild);    // li.orange
    </script>
  </html>
  ```

<br>

### 3.3. 자식 노드 존재 확인
- Node.prototype.hasChildNodes
- 이 메서드를 사용하면 자식 노드가 존재하는지 확인 가능
- 자식 노드가 존재하면 true, 존재하지 않으면 false 반환
- hasChildNodes 메서드는 childNodes 프로퍼티와 마찬가지로 텍스트 노드를 포함하여 자식 노드의 존재 확인
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <ul id="fruits">
      </ul>
    </body>
    <script>
      const $fruits = document.getElementById('fruits');

      // #fruits 요소에 자식 노드가 존재하는지 확인
      console.log($fruits.hasChildNodes());    // true
    </script>
  </html>
  ```
- 자식 노드 중에 텍스트 노드가 아닌 요소 노드가 존재하는지 확인하려면 hasChildNodes 메서드 대신 children.length 또는 Element 인터페이스의 childElement 프로퍼티 사용
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <ul id="fruits">
      </ul>
    </body>
    <script>
      const $fruits = document.getElementById('fruits');

      console.log($fruits.hasChildNodes());    // true

      // 자식 노드 중에 텍스트 노드가 아닌 요소 노드가 존재하는지 확인
      console.log(!!$fruits.children.length);    // 0 -> false
      console.log(!!$fruits.childElementCount);    // 0 -> false
    </script>
  </html>
  ```

<br>

### 3.4. 요소 노드의 텍스트 노드 탐색
- 요소 노드의 텍스트 노드는 요소 노드의 자식 노드
  - 요소 노드의 텍스트 노드는 firstChild 프로퍼티로 접근 가능
  - firstChild 프로퍼티는 첫 번째 자식 노드 반환
  - firstChild 프로퍼티가 반환한 노드는 텍스트 노드이거나 요소 노드
  ```html
  <!DOCTYPE html>
  <html>
  <body>
    <div id='foo'>Hello</div>
    <script>
      console.log(document.getElementById('foo').firstChild);    // #text
    </script>
  </body>
  </html>
  ```

<br>

### 3.5. 부모 노드 탐색
- Node.prototype.parentNode
- 이 프로퍼티를 사용하면 부모 노드 탐색 가능
- 텍스트 노드는 DOM 트리의 최종단 노드인 리프 노드이므로 부모 노드가 텍스트 노드인 경우 X
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <ul id="fruits">
        <li class="apple">Apple</li>
        <li class="banana">Banana</li>
        <li class="orange">Orange</li>
      </ul>
    </body>
    <script>
      // 노드 탐색의 기점이 되는 .banana 요소 노드 취득
      const $banana = document.querySelector('.banana');

      // .banana 요소 노드의 부모 노드 탐색
      console.log($banana.parentNode);    // ul#fruits
    </script>
  </html>
  ```

<br>

### 3.6. 형제 노드 탐색
- 어트리뷰트 노드는 요소 노드와 연결되어 있지만 부모 노드가 같은 형제 노드가 아니기 때문에 반환되지 않음
  - 아래 프로퍼티는 텍스트 노드 또는 요소 노드만 반환

    | 프로퍼티 | 설명 |
    | :--- | :--- |
    | Node.prototype.previousSibling | 부모 노드가 같은 형제 노드 중에서 자신의 이전 형제 노드를 탐색하여 반환. previousSilbing 프로퍼티가 반환하는 형제 노드는 요소 노드뿐만 아니라 텍스트 노드일 수도 있음 |
    | Node.prototype.nextSibling | 부모 노드가 같은 형제 노드 중에서 자신의 다음 형제 노드를 탐색하여 반환. nextSibling 프로퍼티가 반환하는 형제 노드는 요소 노드뿐만 아니라 텍스트 노드일 수도 있음 |
    | Element.prototype.previousElementSilbing | 부모 노드가 같은 형제 요소 노드 중에서 자신의 이전 형제 요소 노드를 탐색하여 반환. previousElemntSibling 프로퍼티는 요소 노드만 반환 |
    | Element.prototype.nextElementSibling | 부모 노드가 같은 형제 요소 노드 중에서 자신의 다음 형제 요소 노드를 탐색하여 반환. nextElementSibling 프로퍼티는 요소 노드만 반환 |

  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <ul id="fruits">
        <li class="apple">Apple</li>
        <li class="banana">Banana</li>
        <li class="orange">Orange</li>
      </ul>
    </body>
    <script>
      const $fruits = document.getElementById('fruits');

      // #fruits 요소의 첫 번째 자식 노드 탐색
      const { firstChild } = $fruits;
      console.log(firstChild);    // #text

      // #fruits 요소의 첫 번째 자식 노드(텍스트 노드)의 다음 형제 노드 탐색
      const { nextSibling } = nextSibling;
      console.log(nextSibling);    // li.apple

      // li.apple 요소의 이전 형제 노드 탐색
      const { previousSibling } = nextSilbing;
      console.log(previousSibling);    // #text

      // #fruits 요소의 첫 번째 자식 요소 노드 탐색
      const { firstElementChild } = $fruits;
      console.log(firstElementChild);    // li.apple

      // #fruits 요소의 첫 번째 자식 요소 노드의 다음 형제 노드 탐색
      const { nextElementSibling } = fistElemntChild;
      console.log(nextElementSibling);    // li.banana

      // li.banana 요소의 이전 형제 요소 노드 탐색
      const { previousElementSibling } = nextElementSibling;
      console.log(previousElemntSibling);    // li.apple
    </script>
  </html>
  ```

<br>
<br>

## 4. 노드 정보 취득
- 노드 객체에 대한 정보를 취득하려면 다음과 같은 노드 정보 프로퍼티를 사용
  | 프로퍼티 | 설명 |
  | :--- | :--- |
  | Node.prototype.nodeType | 노드 객체의 종류(노드 타입)를 나타내는 상수를 반환. 노드 타입 상수는 Node에 정의 <br> - Node.ELEMENT_NODE : 요소 노드 타입을 나타내는 상수 1 반환 <br> - Node.TEXT_NODE : 텍스트 노드 타입을 나타내는 상수 3을 반환 <br> - Node.DOCUMENT_NODE : 문서 노드 타입을 나타내는 상수 9를 반환 |
  | Node.prototype.nodeName | 노드의 이름을 문자열로 반환 <br> - 요소 노드 : 대문자 문자열로 태그 이름 반환 <br> - 텍스트 노드 : 문자열 '#text' 반환 <br> - 문서 노드 : 문자열 '#document' 반환 |
  ```html
  <!DOCTYPE html>
  <html>
  <body>
    <div id="foo">Hello</div>
  </body>
  <script>
    console.log(document.nodeType);    // 9
    console.log(document.nodeName);    // #document

    // 요소 노드의 노드 정보 취득
    const $foo = document.getElementById('foo');
    console.log($foo.nodeType);    // 1
    console.log($foo.nodeName);    // DIV

    // 텍스트 노드의 노드 정보 취득
    const $textNode = $foo.firstChild;
    console.log($textNode.nodeType);    // 3
    console.log($textNode.nodeName);    // #text
  </script>
  </html>
  ```

## 5. 요소 노드의 텍스트 조작

<br>

### 5.1. nodeValue
- Node.prototype.nodeValue 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티
  - 참조와 할당 모두 가능
- 노드 객체의 nodeValue 프로퍼티를 참조하면 노드 객체의 값 반환
  - 노드 객체의 값 : 텍스트 노드의 텍스트
  - 텍스트가 아닌 노드(문서 노드나 요소 노드)의 nodeValue 프로퍼티를 참조하면 null 반환
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <div id="foo">Hello</div>
    </body>
    <script>
      // 문서 노드의 nodeValue 프로퍼티를 참조
      console.log(document.nodeValue);    // null

      // 요소 노드의 nodeValue 프로퍼티를 참조
      const $foo = document.getElementById('foo');
      console.log($foo.nodeValue);    // null

      // 텍스트 노드의 nodeValue 프로퍼티 참조
      const $textNode = $foo.firstChild;
      console.log($textNode.nodeValue);    // Hello
    </script>
  </html>
  ```
- 텍스트 노드의 nodeValue 프로퍼티를 참조할 때만 텍스트 노드의 값을 반환
- 텍스트 노드의 nodeValue 프로퍼티에 값을 할당하면 텍스트 노드의 값, 즉 텍스트 변경 가능
  - 요소 노드의 텍스트를 변경하려면 처리 필요
    1. 텍스트를 변경할 요소 노드를 취득한 다음, 취득한 요소 노드의 텍스트 노드를 탐색. 텍스트 노드는 요소 노드의 자식 노드이므로 firstChild 프로퍼티를 사용하여 탐색
    2. 탐색한 텍스트 노드의 nodeValue 프로퍼티를 사용하여 텍스트 노드의 값 변경
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <div id="foo">Hello</div>
    </body>
    <script>
      // 1. #foo 요소 노드의 자식 노드읜 텍스트 노드 취득
      const $textNode = document.getElementById('foo').firstChild;

      // 2. nodeValue 프로퍼티를 사용하여 텍스트 노드의 값 변경
      $textNode.nodeValue = 'World';

      console.log($textNode.nodeValue);    // World
    </script>
  </html>
  ```

<br>

### 5.2. textContent
- Noede.prototype.textContent 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티로서 요소 노드의 텍스트와 모든 자손 노드의 텍스트를 모두 취득하거나 변경
- 요소 노드의 textContent 프로퍼티를 참조하면 요소 노드의 콘텐츠 영역 내의 텍스트 모두 반환
  - 요소 노드의 childNodes 프로퍼티가 반환한 모든 노드들의 텍스트 노드의 값(텍스트)을 모두 반환, HTML 마크업은 무시
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <div id="foo">Hello <span>world!</span></div>
    </body>
    <script>
      console.log(document.getElementById('foo').textContent);    // Hello world!
    </script>
  </html>
  ```
- nodeValue 프로퍼티를 사용하면 textContent 프로퍼티를 사용할 때보다 코드가 복잡
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <div id="foo">Hello <span>world!</span></div>
    </body>
    <script>
      // #foo 요소 노드는 텍스트 노드가 아니다.
      console.log(document.getElementById('foo').nodeValue);    // null

      // #foo 요소 노드의 자식 노드인 텍스트 노드의 값을 취득
      console.log(document.getElementById('foo').firstChild.nodeValue);    // Hello
      
      // span 요소 노드의 자식 노드인 텍스트 노드의 값을 취득
      console.log(document.getElementById('foo').lastChild.firstChild.nodeValue);    // world!
    </script>
  </html>
  ```
- 요소 노드의 콘텐츠 영역에 자식 요소 노드가 없고 텍스트만 존재한다면 firstChild.nodeValue와 textContent 프로퍼티는 같은 결과 반환 -> textContent 사용 권장
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <!-- 요소 노드의 콘텐츠 영역에 다른 요소 노드가 없고 텍스트만 존재 -->
      <div id="foo">Hello</div>
    </body>
    <script>
      const $foo = document.getElementById('foo');

      console.log($foo.textConent === $foo.firstChild.nodeValue);    // true
    </script>
  </html>
  ```
- 요소 노드의 textContent 프로퍼티에 문자열을 할당하면 요소 노드의 모든 자식 노드가 제거되거 할당한 문자열이 텍스트로 추가
  - 할당한 문자열에 HTML 마크업이 포함되어 있더라도 문자열 그대로 인식되어 텍스트로 취급 -> HTML 마크업 파싱 X
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <div id="foo">Hello <span>world!</span></div>
    </body>
    <script>
      document.getElementById('foo').textContent = 'Hi <span>there!</span>';
    </script>
  </html>
  ```
- innerText 프로퍼티 : textContent 프로퍼티와 유사하게 동작
  - innerText 프로퍼티 사용을 권장하지 않는 이유
    - innerText 프로퍼티는 CSS에 순종적. 예를 들어, innerText 프로퍼티는 CSS에 의해 비표시로 지정된 요소 노드의 텍스트 반환 X
    - innerText 프로퍼티는 CSS를 고려해야 하므로 textContent 프로퍼티보다 느림  

<br>
<br>

## 6. DOM 조작
- DOM 조작 : 새로운 노드를 생성하여 DOM에 추가하거나 기존 노드를 삭제 또는 교체하는 것
- DOM 조작에 의해 DOM에 새로운 노드가 추가되거나 삭제되면 리플로우와 리페인트가 발생하는 원인이 되므로 성능에 영향
  - 복잡한 콘텐츠를 다루는 DOM 조작은 성능 최적화를 위해 주의

<br>

### 6.1. innerHTML
- Element.prototype.innerHTML
- 이 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티로서 요소 노드의 HTML 마크업을 취득하거나 변경