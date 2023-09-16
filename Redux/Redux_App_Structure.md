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