# Redux 시작하기
> Redux는 자바스크립트 앱을 위한 예측 가능한 상태 컨테이너다.

- Redux는 일관적으로 동작하고, 서로 다른 환경(서버, 클라이언트, 네이터브)에서 작동하고, 테스트하기 쉬운 앱을 작성하도록 도와준다.

<br>

## 1. 설치

### 1.1. Redux Toolkit
- Redux Toolkit은 Redux 로직을 작성하기 위해 공식적으로 추천하는 방법
- Redux 앱을 만들기에 필수적으로 여기는 패키지와 함수를 포함
- RTK는 저장소 준비, 리듀서 생산과 불변 수정 로직 작성, 상태 "조각" 전부를 한번에 작성 등 일반적인 작업들을 단순화해주는 유틸리티 포함

<br>

- Redux Toolkit은 NPM에서 패키지로 받아 모듈 번들러나 Node 앱에서 사용 가능
  ```bash
  # NPM
  npm install @reduxjs/toolkit

  # Yarn
  yarn add @reduxjs/toolkit
  ```

<br>

### 1.2. React Redux 앱 만들기
- React와 Redux로 새 앱을 만들기 위해 추천하는 방법은 Create React App를 위한 공식 Redux+JS 템플릿 사용하는 것
- 이를 통해 RTK와 React Redux가 React 컴포넌트와 통합 가능
  ```
  npx create-react-app my-app --template redux
  ```

<br>

### 1.3. Redux 코어
- Redux 코어 라이브러리는 NPM에서 패키지로 받아 모듈 번들러나 Node 앱에서 사용 가능
  ```bash
  # NPM
  npm install redux

  # Yarn
  yarn add redux
  ```

<br>
<br>

## 2. 기본 예제
- 앱의 상태 전부는 하나의 저장소(store) 안에 있는 객체 트리에 저장된다.
- 상태 트리를 변경하는 유일한 방법은 무엇이 일어날지 서술하는 객체인 액션(action)을 보내는 것이다.
- 액션이 상태 트리를 어떻게 변경할지 명시하기 위해 리듀서(reducers)를 작성해야 한다.
  ```js
  import { createStore } from 'redux';

  /**
   * (state, action) => state 형태의 순수 함수인 리듀서
   * 리듀서는 액션이 어떻게 상태를 다음 상태로 변경하는지 서술
   *
   * 상태의 모양은 기본형(primitive), 배열, 객체, Immutalbe.js 자료구조 등 다양
   * 중요한 점은 상태 객체를 변경해서는 안되며, 상태가 바뀐다면 새로운 객체를 반환해야 한다는 것
   *
   * 이 예제에서는 'switch' 구문과 문자열을 사용했지만 (함수 맵 같은) 다른 컨벤션을 따라도 된다.
   */
   function counter(state = 0, action) {
    switch (action.type) {
      case 'INCREMENT':
        return state + 1
      case 'DECREMENT':
        return state - 1
      default:
        return state
    }
   }

   // 앱의 상태를 보관하는 Redux 저장소를 만든다.
   // API로는 { subscribe, dispatch, getState }가 있다.
   let store = createStore(counter);

   // subscribe()를 이용해 상태 변화에 따라 UI가 변경될 수 있다.
   // 보통은 subscribe()를 직접 사용하기 보다는 뷰 바인딩 라이브러리를 사용한다.
   // 현재 상태를 localStorage에 영속적으로 저장할 때도 편리하다.
   store.subscribe(() => console.log(store.getState()))

   // 내부 상태를 변경하는 유일한 방법은 액션을 보내는 것뿐이다.
   // 액션은 직렬화할수도, 로깅할수도, 저장할수도 있으며 나중에 재실행할 수도 있다.
   store.dispatch({ type: 'INCREMENT' })    // 1
   store.dispatch({ type: 'INCREMENT' })    // 2
   store.dispatch({ type: 'DECREMENT' })    // 1
  ```
- 상태를 바로 변경하는 대신, **액션**이라 불리는 평범한 객체를 통해 일어날 변경을 명시한다.
- 그리고 각각의 액션이 전체 애플리케이션의 상태를 어떻게 변경할지 결정하는 특별한 함수인 **리듀서**를 작성한다.

<br>

- 보통의 Redux 앱에는 하나의 루트 리듀서 함수를 가진 단 하나의 저장소가 있다.
- 앱이 커짐에 따라 루트 리듀서를 상태 트리의 서로 다른 부분에서 개별적으로 동작하는 작은 리듀서들로 나눌 수 있다.
  - React 앱을 하나의 루트 컴포넌트에서 시작해서 여러 작은 컴포넌트의 조합으로 바꾸는 것과 동일하다.
- 이런 아키텍처가 간단한 앱에서는 너무 과한 것처럼 보이지만, 크고 복잡한 앱에서는 이 패턴의 확장성이 잘 드러난다.
- 액션에 따른 모든 변경을 추적할 수 있기 때문에, 매우 강력한 개발자 도구를 가능하게 해주기도 한다.
- 사용자 세션을 기록한 다음 액션 하나하나를 다시 실행해 볼 수 있다.

<br>
<br>