# Redux Overview and Concepts

> **💡 이 챕터에서 배우는 내용** <br> - Redux가 무엇이고 이를 사용하려는 이유 <br> - 중요 Redux 용어와 개념들 <br> - Redux 앱에서 data가 흐르는 방식

<br>
<br>

## 1. Redux란?
- **Redux는 "actions"라고 불리는 이벤트를 사용하여 애플리케이션 상태를 관리하고 업데이트하기 위한 패턴 및 라이브러리이다.**
- 리덕스는 상태가 예측 가능한 방식으로만 업데이트될 수 있도록 보장하는 규칙을 이용하여, 전체 애플리케이션에서 사용되는 상태에 대한 중앙 집중식 저장소 역할을 한다.

<br>

### 1.1. Redux를 사용해야 하는 이유
- 리덕스는 애플리케이션의 많은 부분에서 필요한 "전역" 상태를 관리할 수 있도록 도와준다.
- **리덕스에 의해 제공되는 패턴과 도구들은 상태가 애플리케이션 상에서 언제, 어디서, 왜, 어떻게 업데이트되는지, 그리고 애플리케이션 로직이 상태가 업데이트될 때 어떤 일이 일어나는 지 이해하기 쉽도록 도와준다.**
- 리덕스는 예측 가능하고 테스트 가능한 코드를 작성할 수 있도록 도와주고, 이는 예상한대로 애플리케이션이 작동할 것이라는 확신을 갖게 해준다.

<br>

### 1.2. Redux를 언제 사용해야 할까?
- 리덕스는 공유된 상태 관리를 다루는데 도움을 주지만, 다른 도구들처럼 장단점이 있다.
  - 단점
    - 더 많은 개념들을 배워야 하고, 더 많은 코드를 작성해야 한다.
    - 코드에 간접적인 내용을 추가하고 특정 제한 사항에 따르도록 요청한다.
- 리덕스가 유용한 때
  - 앱의 많은 곳에서 사용되는 많은 양의 애플리케이션 상태가 있을 때
  - 앱의 상태가 자주 업데이트될 때
  - 상태를 업데이트하는 로직이 복잡할 때
  - 앱에 중간 또는 대규모의 코드베이스가 있고, 많은 사람들과 일할 때
- 모든 앱에서 리덕스가 필요하진 않다. 어떤 앱을 만드는지 생각해보고 어떤 도구를 가지고 문제를 해결할 것인지 결정해야 한다.

<br>

### 1.3. Redux 라이브러리들과 도구들
- 리덕스는 작은 독립형 자바스크립트 라이브러리이다. 하지만, 일반적으로 여러 패키지들과 함께 사용된다.
- React-Redux
  - 리덕스는 어떤 UI 프레임워크든지 통합가능하고, 대부분 React와 자주 사용된다. 
  - React-Redux는 공식적인 패키지로, 리액트 컴포넌트가 상태의 부분을 읽고, 스토어를 업데이트하기 위해 액션들을 파견함으로써 리액트 컴포넌트가 리덕스 스토어와 상호작용할 수 있도록 해준다.
- Redux-Toolkit
  - 리덕스 툴킷은 리덕스 로직 작성에 권장되는 접근법이다.
  - 리덕스 툴킷은 리덕스 앱을 만들기 위해서 필수라고 생각되는 패키지와 함수들을 포함하고 있다.
  - 리덕스 툴킷은 권장 모범 사례를 기반으로 구축되어 있고, 대부분의 리덕스가 하는 작업들을 간소화하고, 일반적인 실수를 예방하고, 리덕스 애플리케이션 작성을 더 쉽게 만들어준다.
- Redux DevTools Extension
  - 리덕스 데브툴스 익스텐션은 리덕스 스토어의 모든 상태 변화 히스토리를 보여준다.
  - "time-travel 디버깅" 같은 강력한 기술 사용을 포함하여 효과적으로 애플리케이션을 디버깅할 수 있도록 해준다.

<br>
<br>

## 2. Redux 용어와 개념
- 실제 코드로 들어가기 전에, Redux를 사용하기 위해 알아야 할 용어와 개념에 대해 알아보자.

<br>

### 2.1. 상태 관리(State Management)
- React Counter 컴포넌트
  - 컴포넌트 상태의 숫자를 추적하고, 버튼이 클릭되면 숫자가 증가한다.
  ```js
  function Counter() {
    // State: 카운터 값
    const [counter, setCounter] = useState(0);

    // Action: 어떤 일이 일어나면 상태를 업데이트하는 코드
    const increment = () => {
      setCounter(prevCounter => prevCounter + 1);
    };

    // View: UI 정의
    return (
      <div>
        Value: {counter} <button onClick={increment}>Increment</button>
      </div>
    )
  }
  ```
- 이는 다음과 같은 부분으로 구성된 독립형 앱이다.
  - The **state(상태)**, 앱을 구동하는 진실의 원천
  - The **view(뷰)**, 현재 상태를 기반으로 한 UI 선언적 설명
  - The **actions(액션)**, 사용자의 입력을 기반으로 앱에서 발생하고, 상태를 업데이트하는 이벤트
- 위의 코드는 **"단방향 데이터 플로우(one-way data flow)"** 의 예시이다.
  - 상태는 특정한 시점의 앱 상태를 설명한다.
  - UI는 상태를 기반으로 렌더링된다.
  - 어떤 일이 일어나면(버튼 클릭), 상태는 어떤 일이 발생했는 지를 기반으로 업데이트된다.
  - UI는 새로운 상태를 기반으로 리렌더된다.
  <img width="640" alt="33" src="https://github.com/chaevivin/Front-end_study/assets/83055813/a07516a5-63a1-4297-a287-da83c44bd941">
- 하지만, 이 간단함은 **똑같은 상태를 공유하고 사용해야 하는 여러개의 컴포넌트** 가 있을 때 무너질 수 있다. 특히, 컴포넌트들이 애플리케이션의 다른 부분에 있을 때 무너지기 쉽다. 가끔, 부모 컴포넌트로 "상태 끌어올리기"를 하면 해결될 수 있으나, 항상 해결되는 것은 아니다.
- 이 문제를 해결하는 한 가지 방법은 컴포넌트로부터 공유된 상태를 추출하여 컴포넌트 트리 바깥의 중앙에 위치한 곳에 놓는 것이다. 이렇게 하면, 컴포넌트 트리는 하나의 큰 "view"가 되고, 컴포넌트 트리의 어떠한 컴포넌트라도 상태에 접근하거나 액션을 유발할 수 있다.
- 상태 관리와 관련된 개념을 정의 및 분리하고 뷰와 상태 사이의 독립성을 유지하는 규칙을 시행함으로써, 코드를 더 구조적이고 유지 보수성이 좋도록 할 수 있다. 
- 이는 리덕스의 기본 아이디어이다.
  - 애플리케이션에서 전역 상태를 포함하는 하나의 중앙 위치와 코드를 예측할 수 있도록 하기 위해 상태를 업데이트할 때 따라야 하는 특정 패턴이다.

<br>

### 2.2. 불변성 (Immutability)
- 불변성은 절대 변할 수 없다는 것을 의미한다.
- 자바스크립트 객체와 배열은 기본적으로 모두 변할 수 있다.
  - 만약 객체를 생성한다면, 해당 필드의 내용을 바꿀 수 있다.
  - 만약 배열을 생성한다면, 해당 필드의 내용을 바꿀 수 있다.
  ```js
  const obj = { a: 1, b: 2 };
  // 바깥에서는 여전히 똑같은 객체이지만, 내용은 변했다.
  obj.b = 3;

  const arr = ['a', 'b'];
  // 똑같은 방법으로, 배열의 내용을 변경할 수 있다.
  arr.push('c');
  arr[1] = 'd';
  ```
  - 이를 객체 또는 배열 변경이라고 부른다. 
    - 메모리에서는 똑같은 객체와 배열 참조이지만, 객체 안의 내용은 변했다.
- **값을 불변하게 업데이트하기 위해서는, 코드는 존재하는 객체와 배열의 복사본을 만들고, 복사본을 수정해야 한다.**
  - 자바스크립트의 배열/객체 스프레드 연산자를 사용하면 이렇게 할 수 있다, 또한 원본 배열을 변경하는 대신에 새로운 배열의 복사본을 반환하는 배열 메서드도 있다.
  ```js
  const obj = {
    a: {
      // obj.a.c를 안전하게 업데이트 하기 위해, 각각을 복사해야 한다.
      c: 3
    },
     b: 2
  };

  const obj = {
    // obj 복사
    ...obj,
    // a 오버라이팅
    a: {
      // obj.a 복사
      ...obj.a,
      // c 오버라이팅
      c: 42
    }
  }

  const arr = ['a', 'b'];
  // arr의 새로운 복사본을 만들고 끝에 "c" 추가
  const arr2 = arr.concat('c');

  // 또는 원본 배열의 복사본 생성
  const arr3 = arr.slice();
  // 그리고 복사본 변경
  arr3.push('c');
  ```
- **리덕스는 모든 상태가 불변적으로 업데이트될 것이라고 예상한다.**
  - 나중에 이 부분이 어디서, 어떻게 중요한지와 불변 업데이트 로직을 더 쉽게 작성할 수 있는 방법을 알아볼 것이다.

<br>

### 2.3. 용어(Terminology)
- 계속하기 전에 알아야 할 Redux 용어들이 있다.
- Actions
  - 액션은 `type` 필드를 가진 순수 자바스크립트 객체이다.
  - **액션은 애플리케이션에서 발생하는 무언가를 설명하는 이벤트이다.**
  - `type` 필드는 `"todos/todoAdded"`같은 설명적인 이름을 액션에 제공하는 문자열이어야 한다.
  - 타입 문자열은 대부분은 `"domain/eventName"`처럼 작성한다.
    - 첫 번째 부분은 액션이 속해 있는 특징이나 카테고리
    - 두 번째 부분은 일어난 구체적인 일
  - 액션 객체는 무슨 일이 일어났는지에 대한 추가적인 정보를 포함하는 다른 필드를 가질 수 있다. 관례적으로, 이 정보는 `payload`라고 불리는 필드에 넣는다.
  - 전형적인 액션 객체
    ```js
    const addTodoAction = {
      type: 'todos/toAdded',
      payload: 'Buy milk'
    };
    ```
- Action Creators
  - 액션 생성자는 액션 객체를 생성하고 반환하는 함수이다.
  - 이 액션 생성자를 사용하면 매번 직접 액션 객체를 쓰지 않아도 된다.
  ```js
  const addTodo = text => {
    return {
      type: 'todos/todoAdded',
      payload: text
    };
  };
  ```
- Reducers
  - 리듀서는 현재 상태와 액션 객체를 받고, 필요에 따라 어떻게 상태를 업데이트할 것인지 결정하고, 새로운 상태를 반환하는 함수이다.
    - `(state, action) => newState`
  - **리듀서는 받아온 액션(이벤트) 타입을 기반으로 이벤트를 처리하는 이벤트 리스너로 생각할 수 있다.**
  > "리듀서" 함수는 `Array.reduce()` 메서드에 전달하는 콜백 함수의 종류와 비슷하기 때문에 이러한 이름을 갖게 되었다.
  - 리듀서는 특정한 규칙들을 따라야 한다.
    - 리듀서는 `state`와 `action`인자를 기반으로 한 새로운 상태 값만 계산한다.
    - 리듀서는 기존의 상태를 수정하도록 허용하지 않는다. 대신에, 그들은 기존의 상태를 복사하고 복사된 값을 변경하는 불변한 업데이트를 만들어야 한다.
    - 리듀서는 동기적인 로직을 하거나 무작위 값을 계산하거나 다른 부작용을 절대 유발하지 않는다.
  - 나중에 리듀서의 규칙에 대해 왜 중요하고 어떻게 정확하게 따라가는지에 대해 알아볼 것이다.
  - 리듀서 함수의 로직 안에서는 일반적으로 다음의 단계를 따라간다.
    - 리듀서가 이 액션에 관심이 있는지 확인한다.
      - 만약 관심있어 한다면, 상태의 복사본을 만들고, 새로운 값의 복사본을 업데이트하고 반환한다.
    - 그렇지 않으면, 변경되지 않은 기존의 상태를 반환한다.
    - 예제
      ```js
      const initialState = { value: 0 };

      function counterReducer(state = initialState, action) {
        // 리듀서가 이 액션에 관심이 있는지 확인한다.
        if (action.type === 'counter/increment') {
          // 만약 관심있어 한다면, 상태의 복사본을 만든다.
          return {
            ...state,
            // 새로운 값의 복사본을 업데이트한다.
            value: state.value + 1
          };
        }
        // 그렇지 않으면, 변경되지 않은 기존의 상태를 반환한다.
        return state;
      }
      ```
  - 리듀서는 새로운 상태 (if/else, switch문, 반복문 등)를 결정하기 위해 내부의 모든 로직을 사용할 수 있다.
- Store 
  - 현재 Redux 애플리케이션 상태는 스토어라고 불리는 객체에 살고 있다.
  - 스토어는 리듀서를 전달하여 생성되고, 현재 상태 값을 반환하는 `getState` 메서드를 가지고 있다.
  ```js
  import { configureStore } from '@reduxjs/toolkit';

  const store = configureStore({ reducer: counterReducer });

  console.log(store.getState());    // {value: 0}
  ```
- Dispatch
  - 리덕스 스토어는 `dispatch`라고 불리는 메서드를 가지고 있다,
  - **상태를 업데이트할 수 있는 유일한 방법은 `store.dispatch()`를 호출하고 액션 객체를 전달하는 것이다.**
  - 스토어는 리듀서 함수를 실행하고 내부에 새로운 상태 값을 저장하고, `getState()`를 호출하여 업데이트된 값을 검색할 수 있다.
  ```js
  store.dispatch({ type: 'counter/increment' });

  console.log(store.getState());    // {value: 1}
  ```
  - **액션을 디스패치하는 것을 애플리케이션 상에서 "이벤트를 발생"하는 것처럼 생각할 수 있다.**
    - 어떤 일이 일어나면, 우리는 스토어가 알길 바란다.
    - 리듀서는 이벤트 리스너처럼 동작하고, 관심있는 액션이 있으면 응답으로 상태를 업데이트한다.
  - 올바른 액션을 디스패치 하기 위해 일반적으로 액션 생성자를 호출한다.
    ```js
    const increment = () => {
      return {
        type: 'counter/increment'
      };
    };

    store.dispatch(increment());

    console.log(store.getState());    // {value: 2}
    ```
- Selectors
  - 선택자는 스토어 상태 값의 특정한 정보의 부분들을 추출하는 방법을 아는 함수이다.
  - 애플리케이션의 규모가 커질수록, 이는 똑같은 데이터를 읽어야 하는 앱의 다른 부분의 반복되는 로직을 피하는데 도움을 준다.
  ```js
  const selectCounterValue = state => state.value;

  const currentValue = selectCounterValue(store.getState());
  console.log(currentValue);    // 2
  ```

<br>

### 2.4. Redux 애플리케이션 데이터 흐름
- "단방향 데이터 흐름"은 앱을 업데이트하는 일련의 과정을 설명한다.
  - 상태는 특정한 시점의 앱 상태를 설명한다.
  - UI는 상태를 기반으로 렌더링된다.
  - 어떤 일이 일어나면(버튼 클릭), 상태는 어떤 일이 발생했는 지를 기반으로 업데이트된다.
  - UI는 새로운 상태를 기반으로 리렌더된다.
- 구체적으로 리덕스에서는 일련의 과정들을 더 자세히 분석할 수 있다.
  - 초기 설정
    - 리덕스 스토어는 루트 리듀서 함수를 사용하여 생성된다.
    - 스토어는 루트 리듀서를 한 번 호출하고, 반환값을 초기 상태로 저장한다.
    - UI가 처음 렌더링 됐을 때, UI 컴포넌트는 리덕스 스토어의 현재 상태에 접근하고 어떤 것을 렌더링할 것인지 정하기 위해 데이터를 사용한다. 또한, UI 컴포넌트는 미래의 스토어 업데이트를 구독하여 상태가 변화했는지 알 수 있다.
  - 업데이트
    - 사용자가 버튼을 클릭한 것처럼 앱에서 어떤 일이 일어난다.
    - 앱의 코드는 `dispatch({type: 'counter/increment'})` 처럼 리덕스 스토어에 액션을 디스패치한다.
    - 스토어는 이전의 상태와 현재 상태를 가지고 다시 리듀서 함수를 실행하고, 반환값을 새로운 상태로 저장한다.
    - 스토어는 구독했던 UI의 모든 부분들에게 스토어가 업데이트 되었다고 알린다.
    - 스토어의 데이터가 필요한 각각의 UI 컴포넌트는 필요한 상태의 부분이 변경되었는지 확인한다.
    - 데이터가 변경된 각각의 컴포넌트는 새로운 데이터로 강제로 리렌더된다.
- 데이터 흐름 그림
  ![34](https://github.com/chaevivin/Front-end_study/assets/83055813/044d6f84-44c9-4ef9-a4b5-8a8a19626a65)

<br>
<br>

## 3. 정리
- **리덕스는 전역 애플리케이션 상태를 관리하기 위한 라이브러리이다.**
  - 리덕스는 일반적으로 리덕스와 리액트를 통합하기 위한 React-Redux 라이브러리로 사용된다.
  - 리덕스 툴킷은 리덕스 로직을 작성하는 데 권장하는 방식이다.
- **리덕스는 "단방향 데이터 흐름" 앱 구조에서 사용한다.**
  - 상태는 특정한 시점의 앱 상태를 설명하고, UI는 해당 상태에 기반해 렌더링된다.
  - 앱에서 어떤 일이 일어나면,
    - UI는 액션을 디스패치한다.
    - 스토어는 리듀서를 실행하고, 상태는 어떤 일이 일어났는지에 기반해 업데이트된다.
    - 스토어는 상태가 변경됐다고 UI에게 알린다.
  - UI는 새로운 상태에 기반해 리렌더된다.
- **리덕스는 여러 타입의 코드를 사용한다.**
  - Actions는 `type` 필드의 순수 객체이고, 앱에서 "어떤 일이 일어났는지"를 설명한다.
  - Reducers는 이전 상태 + 액션에 기반한 새로운 상태 값을 계산하는 함수이다.
  - Redux Store는 액션이 디스패치 될때마다 루트 리듀서를 실행한다.

<br>
<br>