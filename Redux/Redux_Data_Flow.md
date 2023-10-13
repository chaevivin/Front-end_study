# Basic Redux Data Flow

> **💡 이 챕터에서 배우는 내용** <br> - `createSlice`로 리덕스 스토어에 리듀서 로직의 "슬라이스"를 추가하는 방법 <br> - `useSelector` 훅으로 컴포넌트의 리덕스 데이터 읽기 <br> - `useDispatch` 훅으로 컴포넌트의 액션을 디스패치하기

<br>
<br>

# 1. 메인 포스트 피드
- 소셜 미디어 피드 앱을 만들면서 리덕스의 데이터 흐름을 알아볼 것이다.
- 소셜 미디어 피드 앱의 주요한 특징은 포스트의 리스트이다.

<br>

## 1.1. 포스트 슬라이스 생성
- 첫 번째 단계는 포스트 데이터를 가지고 있는 새로운 리덕스 "슬라이스"를 생성하는 것이다.
- 리덕스 스토어에 해당 데이터를 가지고 있으면, 페이지에 해당 데이터를 보여주기 위한 리액트 컴포넌트를 만들 수 있다.
- `src`파일 안에, 새로운 `features` 폴더를 만들고, `features` 폴더 안에 `posts` 폴더를 넣고, `postsSlice.js` 파일을 생성한다.
- 포스트 데이터를 어떻게 다루는 지 아는 리듀서 함수를 만들기 위해 리덕스 툴킷의 `createSlice` 함수를 사용할 것이다.
- 리듀서 함수에는 일부 초기 데이터가 포함되어야 앱이 시작했을 때 리덕스 스토어가 해당 값을 불러온다.
- 지금부터, UI를 추가함으로써 시작하기 위해 가짜 포스트 객체에 대한 배열을 생성할 것이다.
- `createSlice`를 임포트하고, 초기 포스트 배열을 정의하고, 그 배열을 `createSlice`에 전달하고, `createSlice`가 생성될 수 있도록 포스트 리듀서 함수를 내보낸다.
  ```js
  // features/posts/postsSlice.js

  import { createSlice } from '@reduxjs/toolkit'

  const initialStte = [
    { id: '1', title: 'First Post!', content: 'Hello' },
    { id: '2', title: 'Second Post', content: 'More text' }
  ]

  const postsSlice = createSlice({
    name: 'posts',
    initialState,
    reducers: {}
  })

  export default postsSlice.reducer
  ```
  - 새로운 슬라이스를 만들때마다, 리덕스 스토어에 리듀서 함수를 추가해야 한다. 리덕스 스토어를 이미 만들었지만 그 안에는 아직 어떤 데이터도 없다. `app/store.js`를 열고, `postReducer` 함수를 임포트하고, `postsReducer`가 `posts`라는 이름의 리듀서 필드에 전달되기 위해서 `configureStore`에 대한 호출을 업데이트한다. 
    ```js
    // app/store.js

    import { configureStore } from '@reduxjs/toolkit'

    import postsReudcer from '../features/posts/postsSlice'

    export default configureStore({
      reducer: {
        posts: postsReducer
      }
    })
    ```
    - 이것은 리덕스에게 `posts` 필드를 가지기 위해 최상위 레벨의 상태 객체를 원한다는 것을 말해주고, 액션이 디스패치될 때 `state.posts`의 모든 데이터는 `postsReducer` 함수에 의해 업데이트될 것이다.
    - 리덕스 데브툴 익스텐션을 열고 현재 상태 내용들을 보면 이 작업들을 확인할 수 있다.
    ![57](https://github.com/chaevivin/Front-end_study/assets/83055813/7c038eb2-bd5e-46ce-82fa-9ec0d101b026)

<br>

### 1.2. 포스트 리스트 표시
- 이제 스토어에 어느정도 포스트 데이터가 있으므로 포스트 리스트를 보여주는 리액트 컴포넌트를 만들 수 있다. 
  - 피드 포스트 기능과 관련된 모든 코드는 `posts` 폴더에 있어야 하고, `PsotsList.js`라는 새로운 파일을 만들어 준다.
- 포스트 리스트를 렌더링할 때, 어딘가에서 데이터를 가져올 필요가 있다. 
  - 리액트 컴포넌트는 리액트-리덕스 라이브러리의 `useSelector` 훅을 사용함으로써 리덕스 스토어에서 데이터를 읽을 수 있다.
  - 우리가 작성한 "선택자 함수"는 전체 리덕스 `상태` 객체를 매개변수로 사용하여 호출될 것이고, 컴포넌트가 스토어로부터 필요한 구체적인 데이터를 반환해야 한다.
- 초기의 `PostsList` 컴포넌트는 리덕스 스토어의 `state.posts` 값을 읽을 것이고, 포스트 배열을 반복해서 각각을 화면에 보여줄 것이다.
  ```js
  // features/posts/PostsList.js

  import React from 'react'
  import { useSelector } from 'react-redux'

  export const PostsList = () => {
    const posts = useSelector(state => state.posts)

    const renderedPosts = posts.map(post => (
      <article className="post-excerpt" key={post.id}>
        <h3>{post.title}</h3>
        <p className="post-content">{post.content.substring(0, 100)}</p>
      </article>
    ))

    return (
      <section className="posts-list">
        <h2>Posts</h2>
        {renderedPosts}
      </section>
    )
  }
  ```
- 그 다음 `App.js`에서 라우팅을 업데이트해서 "welcome" 메세지 대신에 `PostsList` 컴포넌트가 나오도록 한다. 
  - `App.js`에서 `PostsList` 컴포넌트를 임포트하고, 환영 텍스트를 `<PostsList />`로 바꾼다. 
  - React Fragment로 감싸준다. 왜냐하면 나중에 메인 페이지에 다른 것들을 추가할 것이기 때문이다.
  ```js
  // App.js

  import React from 'react'
  import {
    BrowserRouter as Router,
    Switch,
    Rout,
    Redirect
  } from 'react-router-dom'

  import { Navbar } from './app/Navbar'

  import { PostsList } from './features/posts/PostsList'

  function App() {
    return (
      <Router>
        <Navbar />
        <div className="App">
          <Switch>
            <Route
              exact
              path="/"
              render={() => (
                <React.Fragment>
                  <PostsList />
                </React.Fragment>
              )}
            />
            <Redirect to="/" />
          </Switch>
        </div>
      </Router>
    )
  }

  export default App
  ```
  - 이렇게 추가하고 나면, 앱의 메인 페이지는 아래처럼 보일 것이다.
  ![58](https://github.com/chaevivin/Front-end_study/assets/83055813/93eb3da4-5162-46b8-b022-388a0f86ff40)
- 지금까지 리덕스 스토어에 데이터를 추가하고, 리액트 컴포넌트로 화면에 보일 수 있도록 했다.

<br>

### 1.3. 새로운 포스트 추가
- 사람들이 작성한 포스트를 보는 것은 좋지만, 우리만의 포스트를 작성해야 한다. 포스트를 작성하고 저장할 수 있는 "Add New Post" 폼을 만들어보자.
- 처음에는 빈 폼을 만들고 페이지에 추가할 것이다. 그리고, 폼을 리덕스 스토어에 연결해 "Save Post" 버튼이 클릭될 때마다 새로운 포스트들이 추가되도록 할 것이다.

<br>

**(1) 새로운 포스트 폼 추가**
-  `posts` 폴더에 `AddPostForm.js`를 생성한다. 포스트 제목으로 텍스트 입력과 포스트의 바디로 텍스트 구역을 추가할 것이다.
  ```js
  // features/posts/AddPostForm.js

  import React, { useState } from 'react'

  export const AddpostForm = () => {
    const [title, setTitle] = useState('')
    const [content, setContent] = useState('')

    const onTitleChanged = e => setTitle(e.target.value)
    const onContentChanged = e => setContent(e.target.value)

    return (
      <section>
        <h2>Add a new Post</h2>
        <form>
          <label htmlFor="postTitle">Post Title:</label>
          <input
            type="text"
            id="postTitle"
            name="postTitle"
            value={title}
            onChange={onTitleChanged}
          />
          <label htmlFor="postContent">Content:</label>
          <textarea
            id="postContent"
            name="postContent"
            value={content}
            onChange={onContentChanged}
          />
          <button type="button">Save Post</button>
        </form>
      </section>
    )
  }
  ```
  - 이 컴포넌트를 `App.js`에 임포트하고, `<PostsList />` 컴포넌트 위에 추가한다.
  ```js
  // App.js

  <Route 
    exact
    path="/"
    render={() => (
      <React.Fragment>
        <AddPostForm />
        <PostsList />
      </React.Fragment>
    )}
  />
  ```
  - 페이지의 헤더 바로 밑에서 이 폼을 볼 수 있을 것이다.

<br>

**(2) 포스트 항목 저장**
- 이제, 리덕스 스토어에 새 포스트 항목을 추가하여 포스트 슬라이스를 업데이트해보자.
- 포스트 슬라이스는 포스트 데이터에 대한 모든 업데이트를 처리해야 한다. `createSlice` 호출 부분에 `reducers`라고 불리는 객체가 있다. 지금은 비어 있다. 포스트가 추가되는 것을 다루기 위해 안에 리듀서 함수를 추가해야 한다.
- `reducers`안에 2개의 인자(현재 `state`값과 디스패치된 `action` 객체)를 받는 `postAdded`라는 이름의 함수를 추가한다. 포스트 슬라이스가 책임져야 할 데이터에 대해서만 알고 있기 때문에, `state` 인자는 전체 리덕스 상태 객체가 아니라 그 자체로 포스트의 배열이 될 것이다.
- `action` 객체는 `action.payload` 필드로 새 포스트 항목을 가질 것이다. 그리고 우리는 `state` 배열에 새 포스트 객체를 넣을 것이다.
- `postAdded` 리듀서 함수를 적을 때, `createSlice`는 자동으로 같은 이름의 "액션 생성자" 함수를 생성할 것이다. 액션 생성자를 내보낼 수 있고, 사용자가 "Save Post" 버튼을 누를 때 액션이 디스패치되기 위해서 UI 컴포넌트에서 사용할 수 있다.
  ```js
  // features/posts/postsSlice.js

  const postsSlice = createSlice({
    name: 'posts',
    initialState,
    reducers: {
      postAdded(state, action) {
        state.push(action.payload)
      }
    }
  })

  export const { postAdded } = postsSlice.actions

  export default postsSlice.reducer
  ```
  - 주의: **리듀서 함수는 복사본을 만듦으로써 항상 불변하게 새로운 상태 값을 생성한다.** 변하는 함수인 `Array.push()`를 호출하거나 `createSlice()` 안에 `state.someField = someValue` 같은 객체 필드를 수정하는 것이 안전하다. 왜냐하면 이머 라이브러리를 사용해서 해당 변동사항을 내부적으로 안전한 불변하는 업데이트로 바꾸기 때문이다.

<br>

**(3) "Post Added" 액션 디스패치**
- `AddPostForm`은 텍스트 입력값과 아직 아무 것도 안하는 "Save Post" 버튼을 가지고 있다. `postAdded` 액션 생성자를 디스패치하고, 사용자가 쓴 제목과 내용을 포함하는 새 포스트 객체를 넘겨주기 위한 클릭 핸들러를 추가해야 한다.
- 또한 포스트 객체는 `id` 필드가 필요하다. 지금은 초기의 테스트 포스트가 아이디로 가짜 숫자를 사용하고 있다. 다음으로 증가하는 아이디 숫자가 어떤 것이어야 하는지 알아내는 코드를 작성해야 하지만 대신에 유니크한 랜덤 아이디를 생성하는 것이 더 좋아 보인다. 리덕스 툴킷은 우리가 사용할 수 있는 `nanoid` 함수를 가지고 있다.
- 컴포넌트로 액션을 디스패치하기 위해, 스토어의 `dispath` 함수에 접근해야 한다. 리액트-리덕스로부터 `useDispatch` 훅을 호출함으로써 얻을 수 있다. 또한 이 파일에 `postAdded` 액션 생성자를 임포트해야 한다.
- 컴포넌트에 사용할 수 있는 `dispatch` 함수를 가져오면, 클릭 핸들러에서 `dispatch(postAdded())`를 호출할 수 있다. 리액트 컴포넌트의 `useState` 훅으로 제목과 내용 값을 가져올 수 있고, 새로운 아이디를 생성할 수 있고, `postAdded()`에 전달한 새 포스트 객체를 같이 넣을 수 있다.
  ```js
  // features/posts/AddPostForm

  import React, { useState } from 'react'
  import { useDispatch } from 'react-redux'
  import { nanoid } from '@reduxjs/toolkit'

  import { postAdded } from './postsSlice'
  
  export const AddPostForm = () => {
    const [title, setTitle] = useState('')
    const [content, setContent] = useState('')

    const dispatch = useDispatch()

    const onTitleChanged = e => setTitle(e.target.value)
    const onContentChanged = e => setContent(e.target.value)

    const onSavePostClicked = () => {
      if (title && content) {
        dispatch(
          postAdded({
            id: nanoid(),
            title,
            content
          })
        )

        setTitle('')
        setContent('')
      }
    }

    return (
      <section>
        <h2>Add a  new Post</h2>
        <form>
          {/* 폼 입력 생략 */}
          <button type="button" onClick={onSavePostClicked}>
            Save Post
          </button>
        </form>
      </section>
    )
  }
  ```
- 이제 제목과 텍스트를 타이핑하고 "Save Post"를 클릭해보자. 포스트 리스트에 포스트에 해당하는 새로운 아이템이 추가되는 것을 볼 수 있다.

<br>

- 위의 예제는 완전한 리덕스 데이터 흐름 사이클을 보여준다.
  - 포스트 리스트가 `useSelector`로 스토어로부터 초기 포스트 집합을 읽고 초기 UI를 렌더링한다.
  - 새 포스트 항목으로 데이터를 포함하는 `postAdded` 액션을 디스패치한다.
  - 포스트 리듀서는 `postAdded` 액션을 보고 새로운 항목으로 포스트 배열을 업데이트한다.
  - 리덕스 스토어는 UI에게 몇몇 데이터가 변경되었다고 알려준다.
  - 포스트 리스트는 업데이트된 포스트 배열을 읽고, 새 포스트를 보여주기 위해 자기 자신을 리렌디링한다.
- 우리가 이후에 추가할 모든 새 기능들은 여기서 본 기본 패턴들을 따를 것이다.: 상태의 슬라이스 추가, 리듀서 함수 작성, 액션 디스패치 그리고 리덕스 스토어의 데이터를 기반으로한 UI 렌더링.
- 디스패치한 액션을 보기 위해 리덕스 데브툴스 익스텐션을 체크할 수 있고, 액션의 응답으로 리덕스 상태가 어떻게 업데이트되는지 볼 수 있다. 액션 리스트의 `"posts/postAdded"`를 클릭하면, "Action" 탭은 아래처럼 보일 것이다.
  ![59](https://github.com/chaevivin/Front-end_study/assets/83055813/1d365a9c-9e4b-4bf8-8014-92d10eacb785)
- "Diff" 탭은 또한 `state.posts`가 인덱스 2인 새로 추가된 아이템을 가지고 있다는 것을 보여준다.
- `AddPostForm` 컴포넌트는 사용자가 타이핑한 제목과 내용 값의 트랙을 가지고 있기 위해 안에 리액트 `useState` 훅을 가지고 있다는 것을 주의해라. **리덕스 스토어는 애플리케이션에서 "글로벌"로 여겨지는 데이터만 가지고 있다는 것을 명심해라.** 이때는, `AddPostForm`만 입력값 필드의 최신 값에 대해 알 필요가 있다. 그래서 리덕스 스토어에서 임시의 데이터를 지키려고 하기 보다 리액트 컴포넌트 상태의 데이터를 지키고 싶어한다. 사용자가 폼을 다 사용했을 때, 사용자의 입력값을 기반으로한 최종 값을 스토어에 업데이트하기 위해 리덕스 액션을 디스패치한다.

<br>
<br>

## 2. 정리
- **리덕스 상태는 "리듀서 함수"에 의해 업데이트된다.**
  - 리듀서는 이미 존재하는 상태값을 복사하고 복사한 값을 새로운 데이터로 수정함으로써 항상 불변하게 새로운 상태를 계산한다.
  - 리덕스 툴킷의 `createSlice` 함수는 "슬라이스 리듀서"를 생성하고 안전하고 불변한 업데이트로 바꿔주는 "돌연변이" 코드를 작성할 수 있도록 해준다.
  - 슬라이스 리듀서 함수는 `configureStore`에 `reducer` 필드를 추가하면 리덕스 스토어 안에 데이터와 상태 필드 이름들을 정의한다.
- **리액트 컴포넌트들은 `useSelector` 훅을 이용해 스토어의 데이터를 읽는다.**
  - 선택자 함수는 전제 `state` 객체를 받고, 값을 반환해야 한다.
  - 선택자들은 리덕스 스토어가 업데이트될 때마다 재시작하고, 그들이 반환한 데이터가 변경되면 컴포넌트가 리렌더링된다.
- **리액트 컴포넌트들은 스토어를 업데이트하기 위해 `useDispatch` 훅을 이용해 액션을 디스패치한다.**
  - `createSlice`는 우리가 슬라이스에 추가한 각각의 리듀서에 액션 생성자 함수를 생성할 것이다.
  - 액션을 디스패치하기 위해 `dispatch(someActionCreator())`을 컴포넌트 안에 호출한다.
  - 그러면 리듀서들은 실행하고, 이 액션들이 관련있는지 확인하고, 만약 적절하다면 새 상태값을 반환한다.
  - 폼 입력값과 같은 임시의 데이터는 리액트 컴포넌트 상태로 유지되어야 한다. 사용자가 폼을 다 작성하면 스토어를 업데이트하기 위해 리덕스 액션을 디스패치한다.

<br>
<br>