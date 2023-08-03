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