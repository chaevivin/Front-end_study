# Redux App Structure

> **💡 이 챕터에서 배우는 내용** <br> - 전형적인 React + Redux 앱의 구조 <br> - Redux DevTools 확장 프로그램에서 상태 변화를 보는 방법

<br>
<br>

## 1. Counter 예제
- 버튼을 클릭하면 숫자가 증가하거나 감소하는 카운터 앱을 이용해서 React + Redux 애플리케이션의 중요 부분을 볼 것이다.

<br>

### 1.1. Counter App 사용하기
- 브라우저의 DevTools를 열고 "Redux" 탭을 선택한다.
- 그리고 "State" 버튼을 클릭하면 리덕스 스토어가 앱 상태 값으로 시작하고 있는 것을 볼 수 있다.
  ```
  {
    counter: {
      value: 0
    }
  }
  ```
  - DevTools는 앱을 사용할 때 스토어 상태값이 어떻게 변화하는 지 보여준다.
- "+" 버튼을 누르고 Redux DevTools의 "Diff" 탭을 보면 value 값이 1로 증가한 것을 볼 수 있다.
  - "+" 버튼을 누르면, "counter/increment" 타입의 액션이 스토어에게 디스패치된다.
  - 액션이 디스패치되면, `state.counter.value` 필드는 0에서 1로 변화한다.
- 그 다음, 아래 단계들을 시행한다.
  - "+" 버튼을 다시 누르면, 값이 2가 된다.
  - "-" 버튼을 한 번 누르면, 값이 1이 된다.
  - "Add Amount" 버튼을 누르면, 값이 3이 된다.
  - 텍스트 밗의 숫자 "2"를 "3"으로 변경한다.
  - "Add Async" 버튼을 누르면, 프로그레스 바가 채워지고 몇 초 후에 값이 6이 된다.
- Redux DevTools로 돌아가면, 버튼을 클릭할 때마다 디스패치된 총 다섯개의 액션들을 볼 수 있다. 왼쪽 리스트의 맨 마지막 "counter/incrementByAmount" 탭을 클릭하고, "Action" 탭을 클릭하면 아래와 같은 액션 객체를 볼 수 있다.
  ```
  { 
    type: 'counter/incrementByAmount',
    payload: 3
  }
  ```
- 만약 "Diff" 탭을 클릭하면, 액션의 응답으로 `state.counter.value` 필드가 3에서 6으로 변경된 것을 볼 수 있다.
- 앱의 내부에서 무슨 일이 일어나는지와 상태값이 어떻게 변화하는지 보는 능력은 매우 강력하다!
- DevTools는 앱을 디버깅하도록 도와주는 더 많은 명령들과 옵션들을 가지고 있다.
  - 오른쪽의 "Trace" 탭을 클릭하면, 액션이 스토어에 도착했을 때 실행 중이던 줄을 보여주는 소스 코드의 여러 부분들과 함께 자바스크립트 함수가 패널에서 추적들을 쌓는 것을 볼 수 있다.
  - 특히 한 줄은 하이라이트되어야 한다. 이 라인은 `<Counter>` 컴포넌트로부터 액션을 디스패치하는 코드이다.
  - 이는 어느 부분의 코드에서 특정 액션을 디스패치 했는지 추적하기 쉽게 해준다.

<br>
<br>

## 2. Application Contents
- 앱이 무슨 일을 하는 지 알아봤다. 이제 어떻게 동작하는 지 알아보자.
- 애플리케이션을 구성하는 중요 파일들
  - `/src`
    - `index.js`: 앱의 시작 부분
    - `App.js`: 리액트 컴포넌트의 최상위 레벨
    - `/app`
      - `store.js`: 리덕스 스토어 인스턴스 생성
    - `/features`
      - `/counter`
        - `Counter.js`: 카운터 기능의 UI를 보여주는 리액트 컴포넌트
        - `counterSlice.js`: 카운터 기능의 리덕스 로직
      
- 리덕스 스토어가 어떻게 생성되는 지 알아보자.

<br>

### 2.1. 리덕스 스토어 생성
- `app/store.js`를 연다.
  ```js
  import { configureStore } from '@reduxjs/toolkit'
  import counterReducer from '../features/counter/counterSlice'

  export default configureStroe({
    reducer: {
      counter: counterReducer
    }
  })
  ```
  - 리덕스 툴깃의 `configureStore` 함수를 사용하면 리덕스 스토어가 생성된다.
  `configureStore`은 전달하는 리듀서 인자를 요청한다.
  - 우리의 애플리케이션은 많은 다른 기능들에 의해 만들어질 것이고, 각각의 기능들은 고유의 리듀서 함수를 가지고 있을 것이다. `configureStore`을 호출하면, 객체의 모든 다른 리듀서들을 전달할 수 있다. 객체의 키 이름은 최후의 상태 값의 키들로 정의될 것이다.
  - 카운터 로직의 리듀서 함수를 내보내는 `features/counter/counterSlice.js` 파일을 가지고 있다. 여기에 `counterReducer` 함수를 임포트해올 수 있고, 스토어를 생성하면 포함시킬 수 있다.
  - `{counter: counterReducer}`처럼 객체를 넘기면, 리덕스 상태 객체의 `state.counter` 섹션을 원하고 액션이 전달될 때마다 `state.counter` 섹션을 업데이트할 지 여부와 방법을 결정하는 역할을 `counterReducer` 함수가 담당하기를 원한다는 의미이다.
  - 리덕스는 스토어 설정을 다른 종류의 플러그인들("미들웨어"와 "인핸서")로 할 수 있도록 한다. `counfigureStore`은 좋은 개발자 경험을 제공하기 위해 기본적으로 스토어 셋업에 몇몇의 미들웨어를 자동으로 추가하고, Redux DevTools Extension이 내용을 검사할 수 있도록 스토어를 설정한다.

<br>

**Redux Slices**
- **slice는 리덕스 리듀서 로직과 앱의 하나의 기능을 담당하는 액션의 집합이다.** 
- slice라는 이름은 루트 리덕스 상태 객체를 여러 개의 상태 "슬라이스"로 나누는 데에서 유래했다.
- 예를 들어, 블로깅 앱에서 스토어 셋업은 다음과 같을 것이다.
  ```js
  import { configureStore } from '@reduxjs/toolkit'
  import userReducre from '../features/users/usersSlice'
  import postsReducer from '../features/posts/postsSlice'
  import commentsReducer from '../features/comments/commentsSlice'

  export default configureStore({
    reducer: {
      users: usersReducer,
      posts: postsReducer,
      comments: commentReducer
    }
  })
  ```
  - `state.users`, `state.posts`, `state.comments`는 리덕스 상태의 각각의 분리된 "슬리이스"이다.
  - `useReducer`가 `state.users` 슬라이스가 업데이트하는 것에 반응하기 때문에, 이것을 "슬라이스 리듀서" 함수라고 부른다.

<br>

### 2.2. 슬라이스 리듀서와 액션 생성
- `counterReducer` 함수가 `features/counter/counterSlice.js`에서 온다는 것을 알았다. 지금부터 그 파일에 무엇이 있는지 자세히 알아보자.
  ```js
  // features/counter/counterSlice.js
  import { createSlice } from '@reduxjs/toolkit'

  export const counterSlice = createSlice({
    name: 'counter',
    initialState: {
      value: 0
    },
    reducers: {
      increment: state => {
        // 리덕스 툴킷을 사용하면 "변형" 로직을 작성할 수 있다.
        // 실제로 상태를 변형해 주진 않는다.
        // 왜냐하면 "초안 상태"의 변경을 감지하고, 해당 변경을 기반으로 한 새로운 불변의 상태를 생산하는 이머 라이브러리(Immer library)를 사용하기 때문이다.
        state.value += 1
      },
      decrement: state => {
        state.value -= 1
      },
      incrementByAmount: (state, action) => {
        state.value += action.payload
      }
    }
  })

  export const { increment, decrement, imcrementByAmount } = counterSlice.actions
  
  export default counterSlice.reducer
  ```
<br>

- 이전에, UI에서 다양한 버튼들을 클릭하는 것이 세 개의 다른 리덕스 액션 타입을 디스패치하는 것을 살펴보았다.
  - `{type: "counter/increment"}`
  - `{type: "counter/decrement}`
  - `{type: "counter/incrementByAmount}`
- 우리는 액션들은 타입 필드가 있는 순수 객체이고, 타입 필드는 항상 문자열이라는 것을 알고 있다. 그리고 우리는 일반적으로 액션 객체를 생성하고 반환하는 함수인 "액션 생성자(action creator)"를 가지고 있다. 그렇다면 이 액션 객체와, 타입 문자열들과 액션 생성자들은 어디에서 정의되는 것일까?
- 우리는 이것들을 매번 직접 작성할 수도 있지만 지루할 것이다. 게다가, 리덕스에서 진짜로 중요한 것은 리듀서 함수와 새로운 상태를 계산하는 로직이다.
- 리덕스 툴킷은 `createSlice`라는 함수를 가지고 있는데, 이 함수는 액션 타입 문자열, 액션 생성자 함수, 액션 객체를 생성해주는 일을 담당한다. 우리는 슬라이스에 이름을 붙여주고, 리듀서 함수를 가지고 있는 객체를 작성하면, 자동으로 상응하는 액션 코드를 생성해준다. `name` 옵션의 문자열은 각각의 액션 타입의 첫 번째 부분에서 사용되고 각각의 리듀서 함수의 키 이름은 두 번째 부분에서 사용된다. 그래서, `"counter"` 이름 + `"increment"` 리듀서 함수는 `{type: "counter/increment"}`의 액션 타입을 생성했다.
- `name`필드 이외에도, `createSlice`는 처음 호출될 때 상태가 있도록 리듀서의 초기 상태값을 전달해야 한다. 이때, 우리는 0에서 부터 시작하는 `value` 필드를 가진 객체를 제공한다.
- 여기서 우리는 3개의 리듀서 함수를 볼 수 있고, 이 함수들은 다양한 버튼들을 클릭할 때 디스패치되는 세 개의 액션 타입들에 상응한다.
- `createSlice`는 자동으로 우리가 작성한 리듀서 함수와 같은 이름의 액션 생성자를 생성한다. 우리는 그 중 하나를 호출하고 어떤 값을 반환하는지 봄으로써 확인할 수 있다.
  ```ts
  console.log(counterSlice.action.increment())
  // {type: "counter/increment"}
  ```
- 모든 액션 타입들에 응답하는 방법을 아는 슬라이스 리듀서 함수 또한 생성한다.
  ```ts
  const newState = counterSlice.reducer(
    { value: 10 },
    conterSlice.action.increment()
  )
  console.log(newState)
  // {value: 11}
  ```

<br>

### 2.3. 리듀서 규칙
- 리듀서는 특별한 규칙들을 따라야 한다.
  - 리듀서는 `state`와 `action` 인자에 기반한 새로운 상태 값만 계산해야 한다.
  - 이미 존재하는 `state`를 수정해서는 안된다. 대신에, 이미 존재하는 `state`를 복사하고 복사한 값을 바꿈으로써 불변하는 업데이트를 만들어야 한다.
  - 리덕스는 비동기 로직이나 다른 "부작용"을 해서는 안된다.
- 왜 위의 규칙들이 중요할까? 이유를 살펴보자.
  - 리덕스의 목표 중 하나는 코드를 실용적으로 만드는 것이다. 함수의 출력값이 입력된 인자에 의해서만 계산될 때, 코드가 어떻게 동작하는지 이해하기 쉽고 테스트하기 쉽다.
  - 반면에, 함수가 바깥의 변수에 의존하거나 랜덤하게 행동하면, 함수를 동작하면 어떤 일이 일어나는 지 알 수 없을 것이다.
  - 만약 함수가 인자를 포함한 다른 값들을 수정한다면, 이로 인해 애플리케이션이 예기치 않게 작동하는 방식이 변경될 수 있다. 이는 "상태값을 업데이트 했지만, UI가 업데이트되어야 하는데 되지 않는다!"같은 버그의 일반적인 원인이다.
  - 리덕스 데브툴스의 몇몇 기능들은 리듀서가 위의 규칙들을 올바르게 따라가는 것에 의존한다.
- "불변 업데이트"에 대한 규칙은 특별히 중요하다.

<br>

### 2.4. 리듀서와 불변 업데이트
- 지금까지 "불변"(이미 존재하는 객체/배열 값 수정)과 "불변성"(값을 변하지 않는 것처럼 대하는 것)에 대해 이야기 했다.
- 리덕스에서 **리듀서는 원래의/현재의 상태 값을 절대 변경할 수 없다.**
  ```js
  // // ❌ Illegal
  state.value = 123
  ```
  - 리덕스에서 상태를 변경하지 않아야 하는 이유
    - 최근 값을 표시하기 위해 UI가 적절히 업데이트 되지 않는 버그가 발생한다.
    - 상태가 왜 그리고 어떻게 업데이트되는지 이해하기 어려워진다.
    - 테스트 코드를 작성하기 어려워진다.
    - "시간 여행 디버깅(time-travel debugging)"을 올바르게 사용할 수 있는 능력을 깨트린다.
    - 리덕스의 의도된 정신고 사용 패턴에 어긋난다.
- 원래의 값을 변경하지 못한다면, 어떻게 업데이트된 상태값을 반환할 수 있을까?
  - **리듀서는 원래 값의 복사본을 만들고, 해당 복사본을 변경할 수 있다.**
  ```js
  // ✅ 안전한 방법
  return {
    ...state,
    value: 123
  }
  ```
- 자바스크립트의 배열과 객체의 스프레드 연산자와 원래 값의 복사본은 반환하는 함수들을 사용하여 불변하는 업데이트를 작성하는 방법을 이미 알아봤다. 하지만, "직접 불변 업데이트를 작성하는 방법은 기억하고 제대로 사용하기 어렵다"라고 생각할 것이다.
  - 불변 업데이트 로직을 직접 손으로 쓰는 것은 어렵다, 그리고 리듀서에서 실수로 상태를 변경하는 것은 리듀서 사용자들이 흔히 하는 실수이다.
- **그래서 리덕스 툴킷의 `createSlice` 함수는 더 쉽게 불변 업데이트를 작성할 수 있도록 해준다.**
  - `createSlice`는 Immer라고 불리는 라이브러리를 사용한다. Immer는 데이터를 래핑하기 위해 `Proxy`라고 불리는 특별한 JS 툴을 사용하고, 래핑된 데이터를 "변동"하는 코드를 작성할 수 있도록 해준다. 하지만, **Immer는 마치 직접 불변 업데이트 로직을 작성하는 것처럼 만들려고 하는 모든 변동사항을 추적하고, 안전하게 불변 업데이트 값을 반환하기 위해 변동 사항 리스트를 사용한다.**
  - 이 코드 대신에,
    ```js
    function handwrittenReducer(state, action) {
      return {
        ...state,
        first: {
          ...state.first,
          second: {
            ...state.first.second,
            [action.someId]: {
              ...state.first.second[action.someId],
              fourth: action.someValue
            }
          }
        }
      }
    }
    ```
  - 이렇게 작성할 수 있다.
    ```js
    function reducerWithImmer(state, action) {
      state.first.second[action.someId].fourth = action.someValue
    }
    ```
  - 주의
    - **리덕스 툴킷의 `createSlice`와 `createReducer`에서만 "변동"로직을 작성할 수 있다. 왜냐하면 내부에서 Immer를 사용하기 때문이다! 리듀서에서 Immer 없이 변동 로직을 작성한다면 상태값을 변동시킬 것이고 결국 버그가 발생하게 된다!**
- counter slice의 실제 리듀서
  ```js
  // fetures/counter/counterSlice.js
  export const counterSlice = createSlice({
    name: 'counter',
    initialState: {
      value: 0
    },
    reducers: {
      increment: state => {
        // 리덕스 툴킷은 리듀서에서 "변동" 로직을 작성할 수 있도록 해준다.
        // 실제로 상태값을 변경하는 것은 아니다.
        // 왜냐하면 "초안 상태"의 변경 사항을 감지하고, 해당 변경 사항에 기반해 새로운 불변 상태를 생성하는 Immer 라이브러리를 사용하기 때문이다.
        state.value += 1
      },
      decrement: state => {
        state.value -= 1
      },
      incrementByAmount: (state, action) => {
        state.value += action.payload
      }
    }
  })
  ```
  - `increment` 리듀서는 `state.value`로 항상 1을 더한다. 왜냐하면 Immer는 초기 `state` 객체의 변경 사항을 알고 있으므로 여기서 아무것도 반환할 필요가 없기 때문이다. 똑같이, `decrement` 리듀서는 1을 뺀다.
  - 두개의 리듀서 모두 우리가 실제 코드에서 `action` 객체를 볼 필요가 없다. 어쨋든 전달되지만 필요하지 않기 때문에, 리듀서에 `action`을 파라미터로 선언하지 않아도 된다.
  - 반면에, `incrementByAmount` 리듀서는 알아야 할 것이 있다. -> 카운터 값에 얼마나 추가되어야 하는지. 그래서, `state`와 `action` 인자를 가진 리듀서를 선언한다. 이때는, 텍스트 박스에 타이핑한 양이 `action.payload` 필드에 넣어져야 `state.value`에 더할 수 있다는 것을 알아야 한다.

<br>

### 2.5. Thunks로 비동기 로직 작성하기
- 지금까지 애플리케이션의 모든 로직은 동기화 되었다. 액션은 디스패치되었고, 스토어는 리듀서를 작동하고 새로운 상태값을 계산하고, 디스패치 함수는 종료한다. 하지만, 자바스크립트 언어는 많은 비동기 코드를 작성하는 방법을 가지고 있고, 앱들은 일반적으로 API로부터 데이터를 패칭해오는 것 같은 비동기 로직을 가지고 있다. 우리는 리덕스 앱에 비동기 로직을 놓아야 할 자리가 필요하다.
- **thunk**는 비동기 로직을 가지고 있는 리덕스 함수의 특별한 종류이다. Thunks는 2가지 함수를 사용하여 작성할 수 있다.
  - thunk 함수 안에서는 `dispatch`와 `getState`를 인자로 받는다.
  - 생성자 함수 바깥에서는 thunk 함수를 생성하고 반환한다.
- `counterSlice`에서 내보낸 다음 함수는 thunk 액션 생성자의 예제이다.
  ```js
  // features/counter/counterSlice.js

  // 아래의 함수는 thunk라고 불리고 비동기 로직을 수행할 수 있도록 해준다.
  // 일반 액션처럼 디스패치될 수 있다: `dispatch(incrementAsync(10))`
  // 이는 첫 번째 인자로 `dispatch` 함수와 thunk를 호출할 것이다.
  // 그런 다음 비동기 코드를 실행할 수 있고 다른 액션들이 디스패치 될 수 있다.
  export const incrementAsync = amount => dispatch => {
    setTimeout(() => {
      dispatch(incrementByAmount(amount))
    }, 1000)
  }
  ```
  - 전형적인 리덕스 액션 생성자에서 똑같은 방식으로 사용할 수 있다.
    ```js
    store.dispatch(incrementAsync(5))
    ```
- 하지만, thunks를 사용하는 것은 `redux-thunk` 미들웨어(리덕스용 플러그인 유형)가 thunks가 생성될 때 리덕스 스토어에 추가되어야 한다. 다행히도, 리덕스 툴킷의 `configureStore` 함수는 자동으로 이미 이를 설정해 두었기 때문에, thunks를 그냥 사용할 수 있다.
- 서버로부터 데이터를 패치하기 위해 AJAX를 호출할 때, thunk에 호출을 둘 수 있다. 어떻게 정의하는지 알아보기 위해 조금 긴 예제를 살펴보자.
  ```js
  // features/counter/counterSlice.js

  // "thunk 생성자" 함수 바깥
  const fetchUserById = userId => {
    // "thunk 함수" 내부
    return async (dispatch, getState) => {
      try {
        // thunk에서 비동기 호출 
        const user = await userAPI.fetchById(userId)
        // 응답을 받을 때 액션 디스패치
        dispatch(userLoaded(user))
      } catch (err) {
        // 에러가 발생하면 여기에서 처리
      }
    }
  }
  ```

<br>

### 2.6. 리액트 카운터 컴포넌트
- 이전에, 독립형 리액트 `<Counter>` 컴포넌트를 살펴보았다. React + Redux 앱은 `<Counter>` 컴포넌트와 비슷하지만 다른 점이 있다.
- `Counter.js` 컴포넌트 파일 부터 살펴보자.
  ```js
  // features/counter/Counter.js

  import React, { useState } from 'react'
  import { useSelector, useDispatch } from 'react-redux'
  import {
    decrement,
    increment,
    incrementByAmount,
    incrementAsync,
    selectCount
  } from './counterSlice'
  import styles from './Counter.module.css'

  export function Counter() {
    const count = useSelector(selectCount)
    const dispatch = useDispatch()
    const [incrementAmount, setIncrementAmount] = useState('2')

    return (
      <div>
        <div className={styles.row}>
          <button
            className={styles.button}
            aria-label="Increment value"
            onClick={() => dispatch(increment())}
          >
            +
          </button>
          <span className={styles.value}>{count}</span>
          <button
            className={styles.button}
            aria-label="Decrement value"
            onClick={() => dispatch(decrement())}
          >
            -
          </button>
        </div>
        {/* omit additional rendering output here */}
      </div>
    )
  }
  ```
  - 순수 리액트 예제와 같이, `useState` 훅으로 데이터를 저장하는 `Counter` 컴포넌트 함수를 가지고 있다.
  - 하지만, 우리의 컴포넌트는 실제의 현재 카운터 값을 상태값으로 저장하는 것처럼 보이지 않는다. `count`라고 불리는 변수가 있지만 `useState` 훅에서 발생하지는 않았다.
- 리액트는 `useState`와 `useEffect` 같이 몇몇의 빌트인 훅을 가지고 있는 반면에, 다른 라이브러리들은 커스텀 로직을 빌드하기 위해 리액트 훅을 사용하는 그들만의 커스텀 훅을 만들 수 있다.
- 리액트-리덕스 라이브러리는 리액트 컴포넌트가 리덕스 스토어와 상호작용하도록 하는 일련의 커스텀 훅들을 가지고 있다.
  - **`useSelector`로 데이터 읽기**
    - 첫 번째로, `useSelector` 훅은 컴포넌트가 리덕스 스토어 상태에서 필요한 데이터 조각을 추출할 수 있다.
    - 이전에, `상태`를 인자로 취급하고 상태 값의 일부를 반환하는 "선택자" 함수를 작성할 수 있다는 점에 대해 알아보았다.
    - `counterSlice.js`는 밑에서 이 선택자 함수를 가진다.
      ```js
      // features/counter/counterSlice.js

      // 아래의 함수는 선택자라고 불리고 값을 상태로부터 선택할 수 있도록 한다.
      // 선택자는 슬라이스 파일 대신에 사용되는 곳인 인라인에서 정의될 수도 있다.
      // 예를 들어, `useSelector((state) => state.counter.value)`
      export const selectCount = state => state.counter.value
      ```
    - 리덕스 스토어에 접근할 때, 현재의 카운터 값을 검색할 수 있다.
      ```js
      const coint = selectCount(store.getState())
      console.log(count)    // 0
      ```
    - 우리의 컴포넌트들은 리덕스 스토어에 집적 이야기 할 수 없기 때문에, 컴포넌트 파일에 임포트해 올 수 없다. 하지만, `useSelector`는 우리를 위해 뒤에서 리덕스 스토어에게 이야기하는 일을 처리한다. 만약 선택자 함수를 전달하면, 우리를 위해 `someSelector(store.getState())`를 호출하고 결과값을 반환한다.
    - 그래서 아래의 코드로 현재의 스토어 카운터를 받아올 수 있다.
      ```js
      const count = useSelector(selectCount)
      ``` 
    - 또한, 이미 내보낸 선택자만 사용하지 않아도 된다. 예를 들어, `useSelector`로 선택자 함수를 인라인 인자로 작성할 수 있다.
      ```js
      const countPlusTwo = useSelector(state => state.counter.value + 2)
      ```
    - 액션이 디스패치되고 리덕스 스토어가 업데이트될 때마다, `useSelector`는 선택자 함수를 재실행할 것이다. 만약 선택자가 지난 번과 다르게 다른 값을 반환했다면, `useSelector`는 새로운 값으로 컴포넌트를 리렌더할 수 있도록 해줄 것이다.
  - **`useDispatch`로 액션 디스패치**
    - 비슷하게, 리덕스 스토어에 접근할 때 `store.dispatch(increment())`같이 액션 생성자를 사용하여 액션을 디스패치할 수 있다는 것을 우리는 알고 있다. 스토어 자체에 접근할 수 없기 때문에 `dispatch` 메서드에만 접근할 수 있는 방법이 필요하다.
    - `useDispatch` 훅은 `dispatch` 메서드에 접근하고, 리덕스 스토어로부터 실제 `dispatch` 메서드를 제공한다.
      ```js
      const dispatch = useDispatch()
      ```
    - 거기에서,버튼을 누르는 것처럼 사용자가 무엇인가를 하면 액션을 디스패치할 수 있다.
      ```jsx
      // features/counter/Counter.js

      <button
        className={styles.button}
        aria-label="Increment value"
        onClick={() => dispatch(increment())}
      >
        +
      </button>
      ```

<br>

### 2.7. 컴포넌트 상태와 폼
- 지금쯤이면 "항상 내 앱의 모든 상태를 리덕스 스토어에 넣어야 하는가?"라는 의문이 생길 것이다.
  - 답은 **아니다. 앱 전반적으로 필요한 전역 상태는 리덕스 스토어에 있어야 한다. 한 곳에서만 필요한 상태는 컴포넌트 상태에 있어야 한다.**
  - 예를 들어, 카운터에 값을 추가하기 위해 사용자가 다음 숫자를 입력해야 하는 텍스트 박스가 있다고 하자.
    ```js
    // features/counter/Counter.js

    const [incrementAmount, setIncrementAmount] = useState('2')

    // later
    return (
      <div className={styles.row}>
        <input
          className={styles.textbox}
          aria-label="Set increment amount"
          value={incrementAmount}
          onChange={e => setIncrementAmount(e.target.value)}
        />
        <button
          className={styles.button}
          onClick={() => dispatch(incrementByAmount(Number(incrementAmount) || 0))}
        >
          Add Amount
        </button>
        <button
          className={styles.asyncButton}
          onClick={() => dispatch(incrementAsync(Number(incrementAmount) || 0))}
        >
          Add Async
        </button>
      </div>
    )
    ```
    - 현재 인풋의 `onChange` 핸들러에서 액션을 디스패치하고 리듀서에 저장함으로써 숫자 문자열을 리덕스 스토어에 저장한다. 하지만, 그것은 어떠한 이익도 없다. `<Counter>` 컴포넌트 안에서 텍스트 문자열이 사용되는 곳은 여기 밖에 없다. (당연히 이 예제의 다른 컴포넌트인 `<App>`이 있다. 하지만 많은 컴포넌트를 가진 큰 애플리케이션이라면, `<Counter>`에서만 입력값을 처리한다.)
    - 그래서 `<Counter>` 컴포넌트의 `useState` 훅에서 해당 값을 유지하는 것이 합리적이다.
- 비슷하게, `isDropdownOpen`이라고 불리는 불리언 플래그를 가지고 있다면 앱의 어떤 컴포넌트도 해당 플래그를 처리하지 않을 것이다. 실제로는 이 컴포넌트에 대해 로컬로 유지해야 한다.
- **리액트 + 리덕스 앱에서 전역 상태는 리덕스 스토어로 들어가야 하고, 지역 상태는 리액트 컴포넌트 안에 있어야 한다.**
- 어디에 두어야 할 지 모르겠다면 리덕스에 어떤 종류의 데이터를 넣어야 하는지 결정할 수 있는 아래의 일반적인 법칙을 참고해라.
  - 애플리케이션의 다른 파트에서 이 데이터를 처리하는가?
  - 원래의 데이터를 기반으로 추가로 파생된 데이터를 만들 필요가 있는가?
  - 여러 컴포넌트를 작동하기 위해 똑같은 데이터를 사용해야 하는가?
  - 이 상태를 특정 시점으로 복원할 수 있다는 것이 가치가 있는가? (즉, 시간 여행 디버깅)
  - 데이터를 캐시하고 싶은가? (즉, 다시 요청하는 대신에 이미 있는 상태를 사용해라)
  - (교환될 때 내부 상태를 잃어버리는) UI 컴포넌트를 핫리로딩하는 동안에 이 데이터를 일관되게 유지하고 싶은가?
- 이 법칙은 또한 리덕스에서 일반적으로 폼에 대해 생각하는 방법에 대한 좋은 예이기도 하다. **대부분의 폼 상태는 아마 리덕스에서 유지되지 않아야 한다.** 대신에, 편집하는 동안 폼 컴포넌트의 데이터를 유지하고, 사용자가 완료하면 스토어를 업데이트하기 위해 리덕스 액션을 디스패치하라.
- 다음으로 넘어가기 전에 기억해야 할 것이 있다. `counterSlice.js`의 `incrementAsync` thunk를 기억하는가? 다른 일반적인 액션 생성자를 디스패치하는 것과 똑같은 방법을 사용하고 있다는 것을 주의하라. 이 컴포넌트는 일반 액션을 디스패치하는지 비동기 로직을 시작하는지 상관하지 않는다. 이 컴포넌트는 우리가 버튼을 클릭할 때 어떤 것을 디스패치한다는 것만 알고 있다.

<br>

### 2.8. 스토어 제공
- 지금까지 우리는 컴포넌트가 리덕스 스토어와 이야기하기 위해 `useSelector`와 `useDispatch` 훅을 사용하는 것을 보았다. 하지만 스토어를 임포트하지 않는다면 훅들이 어떤 리덕스 스토어와 이야기해야 하는지 알 수 있을까?
- 이 애플리케이션의 모든 부분들을 보았다. 이제 이 애플리케이션의 처음 부분으로 돌아가 마지막 남은 퍼즐이 어떻게 맞춰지는지 봐야 할 시간이다.
  ```js
  // index.js

  import React from 'react'
  import ReactDOM from 'react-dom'
  import './index.css'
  import App from './App'
  import store from './app/store'
  import { Provider } from 'react-redux'
  import * as serviceWorker from './serviceWorker'

  ReactDOM.render(
    <Provider store={store}>
      <App />
    </Provider>,
    document.getElementById('root')
  )
  ```
  - 리액트에게 루트 컴포넌트인 `<App>` 렌더링을 시작해달라고 말하기 위해 항상 `ReactDOM.render(<App />)`을 호출해야 한다. `useSelector` 같은 훅들이 잘 동작하기 위해서는, 리덕스 스토어를 뒤에서 전달하여 접근할 수 있도록 `<Provider>`라고 불리는 컴포넌트를 사용해야 한다.
  - 스토어를 `app/store.js`에 이미 생성해 두었기 때문에 여기서는 임포트 해왔다. `<Provider>` 컴포넌트는 전체 `<App>` 주위에 두고 스토어에 전달한다. (`<Provider store={store}>`)
  - 이제, `useSelector`이나 `useDispatch`를 호출하는 어떠한 리액트 컴포넌트라도 `<Provider>`에게 주었다고 리덕스 스토어와 이야기할 것이다.

<br>
<br>

## 3. 정리
- **리덕스 툴킷의 `configureStore` API를 사용하여 리덕스 스토어를 생성할 수 있다.**
  - `configureStore`는 `reducer` 함수를 기명 인자로 받아들인다.
  - `configureStore`는 자동으로 좋은 기본 세팅으로 스토어를 세팅한다.
- **리덕스 로직은 일반적으로 "슬라이스"라고 불리는 파일로 조직되어 있다.**
  - "슬라이스"는 리듀서 로직을 포함하고 액션은 리덕스 상태의 특정한 기능이나 섹션과 관련있다.
  - 리덕스 툴킷의 `createSlice` API는 액션 생성자와 당신이 제공한 각각의 리듀서 함수의 액션 타입을 생성한다.
- **리덕스 리듀서는 특정한 법칙을 따라야 한다.**
  - `state`와 `action` 인자를 기반으로 한 새로운 상태 값만 계산해야 한다.
  - 이미 존재하는 상태를 복사하여 불변의 업데이트를 만들어야 한다.
  - 비동기 로직이나 다른 "부작용"을 포함할 수 없다.
  - 리덕스 툴킷의 `createSlice` API는 "돌연변이" 불변의 업데이트를 허용하기 위해 Immer를 사용한다.
- **비동기 로직은 일반적으로 "thunks"라고 불리는 특별한 함수에 의해 작성된다.**
  - Thunks는 `dispatch`와 `getState`를 인자로 받는다.
  - 리덕스 툴킷은 기본적으로 `redux-thunk` 미들웨어를 실행한다.
- **리액트-리덕스는 리액트 컴포넌트가 리덕스 스토어와 상호작용할 수 있도록 한다.**
  - `<Provider store={store}>`로 앱을 감싸는 것은 모든 컴포넌트가 스토어를 사용할 수 있도록 해준다.
  - 전역 상태는 리덕스 스토어로 들어가야 하고, 지역 상태는 리액트 컴포넌트 안에 있어야 한다.

<br>
<br>