# Using Redux Data

> **💡 이 챕터에서 배우는 내용** <br> - 다양한 리액트 컴포넌트에서 리덕스 데이터를 사용하는 방법 <br> - 액션을 디스패치하는 로직 구성하는 방법 <br> - 리듀서에서 복잡한 업데이트 로직을 작성하는 방법

<br>
<br>

## 1. 하나의 포스트 띄우기
- 우리는 리덕스 스토어에 새 포스를 추가할 수 있는 능력을 가지고 있기 때문에, 포스트 데이터를 다른 방법으로 사용하여 더 많은 기능을 추가할 수 있다.
- 현재, 우리의 포스트 항목들은 메인 피드 페이지에 보여지고 있지만 텍스트가 너무 길면 내용의 일부만 표시된다. 해당 페이지에 하나의 포스트만 볼 수 있는 기능이 있으면 도움이 될 것이다.

<br>

### 1.1. 하나의 포스트 페이지 생성
- 첫 번째, `posts` feature 폴더에 새 `SinglePostPage` 컴포넌트에 추가해야 한다. 페이지 URL이 `/posts/123` 일때(`123` 부분은 우리가 보여주려 하는 포스트의 ID) 이 컴포넌트를 보여주기 위해 리액트 라우터를 사용할 것이다.
  ```JS
  // features/posts/SinglePostPage.js
  import React from 'react'
  import { useSelector } from 'react-redux'

  export const SinglePostPage = ({ match }) => {
    const { postId } = match.params

    const post = useSelector(state => 
      state.posts.find(post => post.id === postId)
    )

    if (!post) {
      return (
        <section>
          <h2>Post not fount</h2>
        </section>
      )
    }

    return (
      <section>
        <article className="post">
          <h2>{post.title}</h2>
          <p className="post-content">{post.content}</p>
        </article>
      </section>
    )
  }
  ```
  - 리액트 라우터는 우리가 찾고 있는 URL 정보를 담고 있는 `match` 객체를 인자로 넘겨줄 것이다. 이 컴포넌트를 렌더링하기 위해 라우트를 셋업하면, 라우트에게 `postId`라는 변수로써 URL의 두 번째 부분을 분석하라고 말할 것이고, 해당 값을 `match.params`로부터 읽을 수 있다.
  - `postId` 값을 한 번 얻으면, 리덕스 스토어에서 올바른 포스트 객체를 찾기 위해 선택자 함수 내부에서 해당 값을 사용할 수 있다. `state.posts`가 모든 포스트 객체의 배열이어야 한다는 것을 알고 있다, 그래서 배열을 반복하고 우리가 찾는 ID로 포스트 항목을 반환하기 위해 `Array.find()` 함수를 사용할 수 있다.
  - **`useSelector`에서 반환된 값이 새 참조로 변경될 때마다 컴포넌트가 리렌더링된다** 라는 것은 매우 중요하다. 컴포넌트들은 실제로 필요할 때만 렌더링된다는 것을 보장하기 위해 항상 스토어에서 필요한 가능한 제일 작은 양의 데이터를 선택하는 것을 시도해야 한다. 
  - 스토어에서 일치하는 게시물 항목을 가지고 있을지도 모른다. 아마 사용자들은 URL을 직접적으로 입력하려고 하거나 로드된 올바른 데이터가 없을 것이다. 만약 위의 일이 일어나면, `find()` 함수는 실제 포스트 객체 대신에  `undefined`를 반환할 것이다. 우리의 컴포넌트는 위의 일을 방지하기 위해 체크해야 하고 페이지에 "Post not found" 메시지를 보여줌으로써 처리해야 한다.
  - 스토어에서 일치하는 포스트 객체를 가지고 있다고 가정해보자, `useSelector`는 포스트 객체를 반환할 것이고, 페이지의 포스트의 제목과 내용을 렌더링하기 위해 객체를 사용할 수 있다.
  - 메인 피드에 포스트 인용을 보여주기 위해 전체 `posts` 배열을 반복하는 부분이 우리가 가지고 있는 `<PostsList>` 컴포넌트의 몸체의 로직과 꽤 비슷하다는 것을 알아차렸을 것이다. 두 부분에서 사용되는 `Post` 컴포넌트를 추출하려고 할 것이다, 하지만 포스트 인용과 전체 포스트를 보여주는 방식에서 이미 다른점이 있다. 대게 똑같은 부분이 있더라도 계속 나눠서 작성하는 것이 더 좋은 방법이다. 그 후에 코드의 다른 부분이 매우 비슷하다면 재사용할 수 있는 컴포넌트로 추출할 것인지 나중에 결정해도 된다.

<br>

### 1.2. 포스트 라우트 추가하기
- `<SinglePostPage>` 컴포넌트가 있으므로, 컴포넌트를 보여주기 위해 라우트를 정의할 수 있고, 앞의 페이지 피드에 각각의 포스트 링크를 추가할 수 있다.
- `SinglePostPage`를 `App.js`에 임포트하고 라우트를 추가한다.
  ```js
  // App.js
  import { PostsList } from './features/posts/PostsList'
  import { AddPostForm } from './features/posts/AddPostForm'
  import { SinglePostPage } from './featrues/posts/SinglePostPage'

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
                  <AddPostForm />
                  <PostsList />
                </React.Fragment>
              )}
            />
            <Route exact path="/posts/:postId" component={SinglePostPage} />
            <Redirect to="/">
          </Switch>
        </div>
      </Rounter>
    )
  }
  ```
- `<PostsList>`에서 특정 포스트를 라우트하는 `<Link>`를 포함하기 위해 리스트 레더링 로직을 업데이트한다.
  ```js
  // features/posts/PostsList.js
  import React from 'react'
  import { useSelector } from 'react-redux'
  import { Link } from 'react-router-dom'

  export const PostsList = () => {
    const posts = useSelector(state => state.posts)

    const renderedPosts = posts.map(post => (
      <article className="post-excerpt" key={post.id}>
        <h3>{post.title}</h3>
        <p className="post-content">{post.content.substring(0, 100)}</p>
        <Link to={`/posts/${post.id}`} className="button muted-button">
          View Post
        </Link>
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
- 이제 클릭하면 다른 페이지로 넘어갈 수 있기 때문에, `<Navbar>` 컴포넌트에서 또한 메인 포스트 페이지로 돌아오는 링크가 있으면 도움이 될 것이다.
  ```js
  // app/Navbar.js
  import React from 'react'

  import { Link } from 'react-router-dom'

  export const Navbar = () => {
    return (
      <nav>
        <section>
          <h1>Redux 핵심 Example</h1>

          <div className="navContent">
            <div className="navLinks">
              <Link to="/">Posts</Link>
            </div>
          </div>
        </section>
      </nav>
    )
  }
  ```

<br>
<br>

## 2. 포스트 수정
- 사용자는 포스트 저장을 끝내고 저장했는데 어딘가에서 실수를 발견했을 때 정말 짜증날 것이다. 포스트 생성 후 편집할 수 있는 기능은 유용할 것이다.
- 이미 존재하는 포스트 아이디를 가지고 스토어로부터 해당 포스트를 읽고, 사용자가 포스트의 제목과 내용을 수정할 수 있도록 하고 스토어에 포스트 업데이트 내용을 저장할 수 있는 `<EditPostForm>` 컴포넌트를 추가해보자.

<br>

### 2.1. 포스트 항목 업데이트
- 첫 번째로, 새 리듀서 함수와 액션을 생성하기 위해 `postSlice`를 업데이트해서 스토어가 실제로 포스트를 어떻게 업데이트하는지 알 수 있도록 해야 한다.
- `createSlice()` 호출 내부에서 `reducers` 객체 안에 새 함수를 추가해야 한다. 해당 리듀서의 이름은 어떤 일이 일어나는지 잘 설명할 수 있어야 한다는 것을 명심하라. 왜냐하면 해당 액션이 디스패치될 때마다 리덕스 데브툴스의 액션 타입 문자열의 부분에서 리듀서 이름을 볼 것이기 때문이다. 첫 번째 리듀서는 `postAdded`라고 불렀기 때문에 이번에는 `postUpdated`라고 부를 것이다.
- 포스트 객체를 업데이트하기 위해 알아야 할 것
  - 업데이트되는 포스트의 아이디. 상태에서 올바른 포스트 객체를 찾을 수 있다.
  - 사용자가 입력한 새 `title`과 `content` 항목
- 리덕스 액션 객체들은 일반적으로 설명 문자열을 나타내는 `type` 필드가 요구되고 어떤 일이 일어났는지에 대한 더 많은 정보같은 다른 항목들도 가지고 있어야 한다. 관례에 따라, 일반적으로 `action.payload`라고 불리는 필드에 추가 정보를 두지만 `payload` 필드가 무엇을 포함하고 있는지 결정하는 것에 달렸다. (문자열, 숫자, 객체, 배열이나 다른 것들) 이 상황에서는 필요한 3개의 정보를 가지고 있기 때문에 `payload` 필드에는 3개의 정보를 포함하는 객체를 두기로 하자. 이것은 액션 객체가 `{type: 'posts/postUpdated', payload: {id, title, content}}` 처럼 보인다는 것을 의미한다.
- 기본적으로 `createSlice`에 의해 생성되는 액션 생성자들은 하나의 인자를 넘길것이라고 예상하고 해당 값은 `action.payload`로 액션 객체에 넣어진다. 그래서 인자로서 해당 필드들을 포함하는 객체를 `postUpdated` 액션 생성자에 넘길 수 있다.
- 우리는 리듀서는 액션이 디스패치될 때 상태가 실제로 어떻게 업데이트되는지 결정하는 책임을 가지고 있다는 것 또한 알고 있다. 그것을 고려하면, 리듀서가 아이디를 기반해 올바른 포스트 객체를 찾아야 하고 구체적으로 해당 포스트에서 `title`과 `content` 필드를 업데이트할 수 있어야 한다.
- 마지막으로, `createSlice`가 우리를 위해 생성한 액션 생성자 함수를 내보내야 사용자가 포스트를 저장할 때 UI가 새로운 `postUpdated` 액션을 디스패치할 수 있다.
- 모든 요구 사항들을 고려하면, `postSlice` 정의가 완료된 후의 모습은 다음과 같다.
  ```js
  // features/posts/postsSlice.js

  const postsSlice = createSlice({
    name: 'posts',
    initialState,
    reducers: {
      postAdded(state, action) {
        state.push(action.payload)
      },
      postUpdated(state, action) {
        const { id, title, content } = action.payload
        const existingPost = state.find(post => post.id === id)
        if (existingPost) {
          existingPost.title = title
          existingPost.content = content
        }
      }
    }
  })

  export const { postAdded, postUpdated } = postsSlice.actions

  export default postsSlice.reducer
  ```

<br>

### 2.2. 포스트 수정 폼 만들기
- 전에 만들었던 새 `<EditPostForm>` 컴포넌트는 `<AddPostForm>`과 비슷해 보이지만, 로직은 조금 다르다. 스토어에서 올바른 `post` 객체를 검색해야 하고 해당 객체를 컴포넌트에서 상태 필드를 초기화하는데 사용해서 사용자가 변화를 만들 수 있도록 해야 한다. 사용자가 할 일을 다하면 바뀐 제목과 내용 값들을 스토어에 다시 저장할 것이다. 하나의 포스트 페이지로 바꾸고 해당 포스트를 보여주기 위해 리액트 라우터의 히스토리 API(history API)를 사용할 것이다.  
  ```js
  // featrues/posts/EditPostForm.js

  import React, { useState } from 'react'
  import { useDispatch, useSelector } from 'react-redux'
  import { useHistory } from 'react-router-dom'

  import { postUpdated } from './postsSlice'

  export const EditPostForm = ({ match }) => {
    const { postId } = match.params

    const post = useSelector(state =>
      state.posts.find(post => post.id === postId)
    )

    const [title, setTitle] = useState(post.title)
    const [content, setContet] = useState(post.contet)

    const distpatch = useDispatch()
    const history = useHistory()

    const onTitleChanged = e => setTitle(e.target.value)
    const onContentChanged = e => setContent(e.target.value)

    const onSavePostClicked = () => {
      if (title && content) {
        dispatch(postUpdated({ id: postId, title, content }))
        history.push(`/posts/${postId}`)
      }
    }

    return (
      <section>
        <h2>Edit Post</h2>
        <form>
          <label htmlFor="postTitle">Post Title:</label>
          <input 
            type="text"
            id="postTitle"
            name="postTitle"
            placeholder="What's on your mind?"
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
        </form>
        <button type="button" onClick={onSavePostClicked}>
          Save Post
        </button>
      </section>
    )
  }
  ```
- `SinglePostPage`처럼 `App.js`에 임포트해와야 하고 루트 파라미터인 `postId`로 이 컴포넌트를 렌더링할 루트를 추가해야 한다.
  ```js
  // App.js

  import { PostsList } from './features/posts/PostsList'
  import { AddPostForm } from './features/posts/AddPostForm'
  import { SingePostPage } from './features/posts/SinglePostPage'
  import { EditPostForm } from './features/posts/EditPostForm'

  function App() {
    return (
      <Router>
        <Navbar />
        <div className="App">
          <Switch>
            <Route
              exact
              path='/'
              render={() => (
                <React.Fragment>
                  <AddPostForm />
                  <PostsList />
                </React.Fragment>
              )}
            />
            <Route exact path='/posts/:postId' component={SinglePostPage} />
            <Route exact path='/editPost/:postId' component={EditPostForm} />
            <Redirect to='/' />
          </Switch>
        </div>
      </Router>
    )
  }
  ```
- `EditPostForm`으로 가는 `SinglePostPage`에 새 링크도 추가해야 한다.
  ```js
  // features/post/SinglePostPage.js
  
  import { Link } from 'react-router-dom'

  export const SingePostPage = ({ match }) => {
    // omit other contents
    <p className="post-content">{post.content}</p>
    <Link to={`/editPost/${post.id}`} className="button">
      Edit Post
    </Link>
  }
  ```

<br>

### 2.3. 액션 페이로드 준비
- `createSlice`의 액션 생성자가 일반적으로 `action.payload`가 되는 하나의 인자를 기대하는 것을 보았다. 이것은 가장 일반적인 사용패턴으로 단순화하지만 가끔 액션 객체의 내용을 준비하기 위해 더 많은 작업을 해야될 때도 있다. `postAdded` 액션 같은 경우에는 새 포스트를 위해 유니크한 아이디를 생성해야 하고 페이로드는 `{id, title, content}`처럼 생긴 객체여야 한다.
- 지금부터 우리의 리액트 컴포넌트에 아이디를 생성하고 페이로드 객체를 생성하고 `postAdded`에 페이로드 객체를 넘길 것이다. 하지만 다른 컴포넌트에서 똑같은 액션을 디스패치해야 한다거나, 페이로드를 준비중인 로직이 복잡하다면 어떻게 해야 할까? 액션을 디스패치하고 싶은 곳에 매번 로직을 복사해야 하고 해당 액션에 대한 페이로드가 어떻게 생겨야 하는지 컴포넌트에게 정확하게 알리도록 노력해야 한다.
- 주의: 만약 액션이 유티크한 아이디나 다른 랜덤한 값을 포함하고 있어야 한다면 항상 그것을 먼저 생성하고 액션 객체에 넣어야 한다. **리듀서는 랜덤값을 절대 계산하지 않는다. 왜냐하면 결과를 예측하기 어렵게 만들기 때문이다.**
- 만약 `postAdded` 액션 생성자를 손으로 직접 작성한다면 셋업 로직을 안에다가 넣어야 한다.
  ```js
  // 직접 작성한 액션 생성자
  function postAdded(title, content) {
    const id = nanoid()
    return {
      type: 'posts/postAdded'
      payload: { id, title, content }
    }
  }
  ```
- 하지만 리덕스 툴킷의 `createSlice`는 이 액션 생성자들을 생성해준다. 우리가 직접 작성하지 않아도 되기 때문에 코드가 짧아진다. 하지만 `action.payload`의 내용을 직접 작성하는 방법을 알아야 한다.
- 다행히 `createSlice`는 리듀서를 작성할 때 "준비 콜백(prepare callback)"함수를 정의할 수 있도록 해준다. "준비 콜백"함수는 여러개의 인자를 가질 수 있고 유니크한 아이디처럼 랜덤 값을 생성할 수 있고 어떤 값이 액션 객체로 들어가야 하는지 결정하기 위해 필요한 어떠한 동기 로직이든 실행할 수 있다. 그러면 `payload` 필드 안에 객체를 반환해야 한다. (반환 객체는 액션에게 추가 설명을 해주는 값을 추가하는데 사용할 수 있는 `meta` 필드와 이 액션이 어떤 종류의 에러를 나타내던지 불리언 값이어야 하는 `error` 필드를 가지고 있다.)
- `createSlice`의 `reducers` 필드 안에 `{reducer, prepare}`처럼 생긴 객체로 된 하나의 필드를 정의해야 한다.
  ```js
  // features/posts/postsSlice.js

  const postsSlice = createSlice({
    name: 'posts'
    initialState,
    reducers: {
      postAdded: {
        reducer(state, action) {
          state.push(action.payload)
        },
        prepare(title, content) {
          return {
            payload: {
              id: nanoid(),
              title,
              content
            }
          }
        }
      }
      // other reducers here
    }
  })
  ```
- 이제 우리의 컴포넌트는 페이로드 객체가 어떻게 생겨야 하는지 걱정할 필요가 없다. 액션 생성자는 올바르게 관리할 것이다. 그래서 컴포넌트를 업데이트해서 `postAdded`를 디스패치 했을 때 인자로서 `title`과 `content`를 넘겨주어야 한다.
  ```js
  // features/posts/AddPostForm.js
  
  const onSavePostClicked = () => {
    if (title && content) {
      dispatch(postAdded(title, cotent))
      setTitle('')
      setContent('')
    }
  }
  ```

<br>
<br>

## 3. 사용자와 포스트
- 지금까지는 하나의 상태 슬라이스만 가지고 있다. 로직은 `postsSlice.js`에 정의되어 있고, 데이터는 `state.posts`에 저장되어 있고, 모든 컴포넌트들은 포스트 기능과 관련이 있다. 실제 앱은 많은 다른 종류의 상태 슬라이스와 리덕스 로직과 리액트 컴포넌트에 대한 여러 "기능 폴더"들을 가지고 있을 것이다.
- 다른 여러 사람들이 없다면 "소셜 미디어" 앱을 가질 수 없다. 우리의 앱에 사용자 리스트를 추적하고 해당 데이터를 활용하여 포스트 관련 기능을 업데이트할 수 있는 기능들을 추가해 보자.

<br>

### 3.1. 사용자 슬라이스 추가
- "사용자"의 개념과 "포스트"의 개념은 다르기 때문에 사용자와 관련된 코드와 데이터는 포스트에 관한 코드와 데이터로부터 분리해야 한다. `features/users` 폴더를 추가하고 이곳에 `useSlice` 파일을 추가하라. 포스트 슬라이스처럼 데이터를 가지고 작업할 수 있도록 초기 항목들을 추가할 것이다.
  ```js
  // features/users/usersSlice.js

  import { createSlice } from '@reduxjs/toolkit'

  const initialState = [
    { id: '0', name: 'Tianna Jenkins' },
    { id: '1', name: 'Kevin Grant' },
    { id: '2', name: 'Madison Price' }
  ]

  const usersSlice = createSlice({
    name: 'users',
    initialState,
    reducers: {}
  })

  export default usersSlice.reducer
  ```
  - 실제로 데이터를 업데이트할 필요가 없기 때문에 `reducers` 필드를 빈 객체로 둘 것이다. (이 부분은 나중에 다시 살펴볼 예정)
- 이전처럼 스토어 파일에서 `usersReducer`를 임포트하고 스토어 세팅에 추가할 것이다.
```js
// app/store.js

import { configureStore } from '@reduxjs/toolkit'

import postsReducer from '../features/posts/postsSlice'
import usersReducer from '../features/users/usersSlice'

export default configureStore({
  reducer: {
    posts: postsReducer,
    users: usersReducer
  }
})
```

<br>

### 3.2. 포스트 작성자 추가
- 앱의 모든 포스트는 사용자 중 한 사람에 의해 작성하였기 때문에 새 포스트를 추가할때마다 해당 포스트를 누가 작성했는지 추적해야 한다. 실제 앱에서는 현재 로그인한 사용자를 추적하는`state.currentUser` 종류의 필드를 가지고 있고 사용자가 포스트를 추가할때마다 해당 정보를 사용한다.
- 이 예제를 더 간단하게 하기 위해 드롭다운 리스트로 사용자를 선택할 수 있도록 `<AddPostForm>` 컴포넌트를 업데이트하고 포스트의 한 부분으로 사용자의 아이디를 추가할 것이다. 포스트 객체가 사용자의 아이디를 포함하고 있으면 사용자 아이디를 사용자의 이름을 확인하고 UI의 개별 포스트에 보여주는데 사용할 것이다.
- 첫번째로, 인자로 사용자 아이디를 받는 `postAdded` 액션 생성자를 업데이트하고 액션에 추가해야 한다. (예시 사용자 아이디 중 하나가 포함된 `post.user` 필드를 갖기 위해 `initialState`의 이미 존재하는 포스트 항목 또한 업데이트할 것이다.)
  ```js
  // featrues/posts/postsSlice.js

  const postsSlice = createSlice({
    name: 'posts',
    initialState,
    reducers: {
      postAdded: {
        reducer(state, action) {
          state.push(action.payload)
        },
        prepare(title, content, userId) {
          return {
            payload: {
              id: nanoid(),
              title,
              content,
              user: userId
            }
          }
        }
      }
      // 다른 리듀서들
    }
  })
  ```
- 이제 `<AddPostForm>`에서 `useSelector`로 스토어의 사용자 리스트를 읽을 수 있고 드롭다운으로 사용자 리스트를 보여줄 수 있다. 그 다음 선택된 사용자의 아이디를 습득하여 `postAdded` 액션 생성자에게 넘겨줄 것이다. 이 작업을 하는 동안 폼에 확인하는 로직을 추가하여 제목과 내용 인풋들이 실제 텍스트를 가지고 있을 때 사용자가 `Save Post` 버튼만 클릭할 수 있도록 할 것이다.
  ```js
  // features/posts/AddPostForm.js

  import React, { useState } from 'react'
  import { useDispatch, useSelector } from 'react-redux'
  import { postAdded } from './postSlice'

  export const AddPostForm = () => {
    const [title, setTitle] = useState('')
    const [content, setContent] = useState('')
    const [userId, setUserId] = useState('')

    const dispatch = useDispatch()
    const users = useSelector(state => state.users)

    const onTitleChanged = e => setTitle(e.target.value)
    const onContentChanged = e => setContent(e.target.value)
    const onAuthorChanged = e => setUserId(e.target.value)

    const onSavePostClicked = () => {
      if (title && content) {
        dispatch(postAdded(title, content, userId))
        setTitle('')
        setContent('')
      }
    }

    const canSave = Boolean(title) && Boolean(content) && Boolean(userId)

    const usersOptions = users.map(user => (
      <option key={user.id} value={user.id}>
        {user.name}
      </option>
    ))

    return (
      <section>
        <h2>Add a New Post</h2>
        <form>
          <label htmlFor="postTitle">Post Title:</label>
          <input
            type="text"
            id="postTitle"
            name="postTitle"
            placeholder="What's on your mind?"
            value={title}
            onChange={onTitleChanged}
          />
          <label htmlFor="postAuthor">Author:</label>
          <select id="postAuthor" value={userId} onChange={onAuthorChanged}>
            <option value=""></option>
            {usersOptions}
          </select>
          <label htmlFor="postContent">Content:</label>
          <textarea
            id="postContent"
            name="postContent"
            value={content}
            onChange={onContentChanged}
          />
          <button type="button" onClick={onSavePostClicked} disabled={!canSave}>
            Save Post
          </button>
        </form>
      </section>
    )
  }
  ```
- 이제 포스트 리스트 아이템과 `<SinglePostPage>` 안에 있는 포스트 저자의 이름을 보여주어야 한다. 여러 곳에서 똑같은 종류의 정보를 보여줄 수 있도록 인자로 사용자 아이디를 받고 올바른 사용자 객체를 보고 사용자 이름을 포맷하는 `PostAuthor` 컴포넌트를 만들어야 한다.
  ```js
  // features/posts/PostAuthor.js

  import React from 'react'
  import { useSelector } from 'react-redux'

  export const PostAuthor = ({ userId }) => {
    const author = useSelector(state =>
      state.users.find(user => user.id === userId)
    )

    return <span>by {author ? author.name : 'Unknown author'}</span>
  }
  ```
- 지금까지 진행하는 동안 각 컴포넌트들은 똑같은 패턴을 따르고 있다는 것에 유의하라. 리덕스 스토어로부터 데이터를 읽어야 하는 어떤 컴포넌트든 `useSelector` 훅을 사용할 수 있고, 필요한 데이터의 특정 부분만 추출할 수 있다. 또한 많은 컴포넌트들은 리덕스 스토어에서 동시에 똑같은 데이터에 접근할 수 있다.
- 이제 `PostsList.js`와 `SinglePostPage.js`에서 `PostAuthor` 컴포넌트를 임포트할 수 있고 `<PostAuthor userId={post.user} />`로 렌더링할 수 있고 포스트 항목을 추가할때마다 선택된 사용자의 이름을 렌더링된 포스트 안에 보여줄 수 있다.

<br>
<br>

## 4. 추가 포스트 기능
- 이 시점에서는 포스트를 생성하고 수정할 수 있다. 더 유용한 포스트 피드를 만들기 위해 추가 로직을 작성해 보자.

<br>

### 4.1. 포스트의 날짜 저장
- 소셜 미디어 피드들은 전형적으로 언제 해당 포스트가 만들어졌는지에 의해 분류되고 "5시간 전" 같은 설명으로 포스트 생성 날짜를 보여준다. 해당 기능을 만들기 위해 포스트 항목의 `data` 필드 추적을 시작해야 한다.
- `post.user` 필드 처럼 액션이 디스패치될 때마다 `post.data`가 항상 포함되어 있도록 `postAdded`의 준비 콜백을 업데이트할 것이다. 하지만 이것은 전달되는 다른 파라미터가 아니다. 액션이 언제 디스패치되었는지의 정확한 타임스탬프를 항상 사용하길 원하기 때문에 준비 콜백이 스스로 그것을 관리하도록 할 것이다.
- 주의: **리덕스 액션과 상태는 객체, 배열, 원시값 같은 순수 자바스크립트 값만 포함해야 한다. 리덕스에 클래스 인스턴스나 함수나 다른 직렬화할 수 있는 값을 넣지 않아야 한다.**
- 리덕스 스토어에 `Date` 클래스 인스턴스만 넣을 수 없기 때문에 타임스탬프 문자열로 `post.date` 값을 추적할 것이다.
  ```js
  // features/posts/postsSlice.js

  postAdded: {
    reducer(state, action) {
      state.push(action.payload)
    },
    prepare(title, content, userId) {
      return {
        payload: {
          id: nanoid(),
          date: new Date().toISOString(),
          title,
          content,
          user: userId,
        },
      }
    },
  },
  ```
- 포스트 작성자처럼 `<PostsList>`와 `<SinglePostPage>` 컴포넌트에 관련 타임스탬프 설명을 보여주어야 한다. 관련 설명으로 포맷팅된 타임스탬프 문자열을 관리하는 `<TimeAgo>` 컴포넌트를 추가할 것이다. 여기에 사용할 `date-fns` 같은 라이브러리들은 날짜를 파싱하고 포맷팅하는 유용한 유틸리티 기능을 가지고 있다.
  ```js
  // features/posts/TimeAgo.js

  import React from 'react'
  import { parseISO, formatDistanceToNow } from 'date-fns'

  export const TimeAgo = ({ timestamp }) => {
    let timeAgo = ''
    if (timestamp) {
      const date = parseISO(timestamp)
      const timePeriod = formatDistanceToNow(date)
      timeAgo = `${timePeriod} ago`
    }

    return (
      <span title={timestamp}>
        &npsp; <i>{timeAgo}</i>
      </span>
    )
  }
  ```

<br>

### 4.2. 포스트 리스트 분류
- `<PostsList>`는 현재 포스트가 리덕스 스토어에 저장된 순서로 포스트가 보여지고 있다. 이 예제는 가장 초기의 포스트가 먼저 오고, 포스트가 추가되면 해당 포스트는 포스트 배열 가장 끝에 추가된다. 이는 최신 포스트가 항상 페이지의 제일 밑에 있다는 것을 의미한다.
- 전형적으로, 소셜 미디어 피드들은 최신 포스트를 제일 먼저 보여주고 오래된 포스트는 스크롤하면 볼 수 있다. 데이터가 스토어에 오래된 순서대로 보관된다고 하더라도 `<PostsList>` 컴포넌트의 데이터를 재정렬해서 최신 포스트가 먼저올 수 있게 할 수 있다. 이론적으로 `state.posts` 배열이 이미 정렬되었다는 것을 알고 있기 때문에 그저 리스트를 역정렬할 수도 있다. 하지만 확실히 하기 위해 앞으로 가서 우리가 정렬하는 편이 좋다.
- `array.sort()`는 기존 배열을 바꾸기 때문에 `state.posts`의 복사본을 만들고 해당 복사본을 정렬해야 한다. `post.date` 필드가 날짜 타임스탬프 문자열로 보관되는 것을 알고 있고, 올바른 순서로 포스트를 정렬하기 위해 해당 문자열을 직접 비교할 수 있다.
  ```js
  // features/posts/PostsList.js

  // 날짜 문자열 시간 순서대로 포스트 역정렬
  const orderedPosts = posts.slice().sort((a, b) => b.date.localeCompare(a.date))

  const renderedPosts = orderedPosts.map(post => {
    return (
      <article className="post-excerpt" key={post.id}>
        <h3>{post.title}</h3>
        <div>
          <PostAuthor userId={post.user} />
          <TimeAgo timestamp={post.date} />
        </div>
        <p className="post-content">{post.content.substring(0, 100)}</p>
        <Link to={`/posts/${post.id}`} className="button muted-button">
          View Post
        </Link>
      </article>
    )
  })
  ```
- `postsSlice.js`의 `initialState`에 `data` 필드를 추가해야 한다. 현재 날짜/시간에서 분을 빼기 위해 `date-fns`를 여기서 다시 사용하여 서로 다르도록 할 것이다.
```js
// features/posts/postsSlice.js

import { createSlice, nanoid } from '@reduxjs/toolkit'
import { sub } from 'date-fns'

const initialState = [
  {
    // 생략된 필드
    content: 'Hello!',
    date: sub(new Date(), { minutes: 10 }).toISOString()
  },
  {
    // 생략된 필드
    content: 'More text',
    date: sub(new Date(), { minutes: 5 }).toISOString()
  }
]
```

<br>

### 4.3. 포스트 리액션 버튼
- 이 섹션에서 새 기능 하나를 더 추가할 것이다. 지금은 우리의 포스트들은 지루하다. 이 포스트들을 좀 더 재미있게 만들어야 한다. 우리의 포스트에 친구들이 리액션 이모지를 남길 수 있는 것 보다 더 좋은 방법이 있을까?
- `<PostsList>`와 `<SinglePost>`의 각 포스트 밑에 이모지 리액션 버튼을 추가할 것이다. 리액션 버튼 중 하나를 클릭할 때마다 리덕스 스토어에서 해당 포스트와 맞는 카운터 필드를 업데이트해야 한다. 리액션 카운터 데이터가 리덕스 스토어 안에 있기 때문에 앱의 다른 부분간에 바꾸는 것은 해당 데이터를 사용하는 컴포넌트에서 꾸준히 똑같은 값을 보여주어야 한다.
- 포스트 작성자와 타임스탬프처럼 포스트를 보여주는 어떤 곳이든 해당 기능을 사용하길 바란다. 그래서 인자로 `post`를 받는  `<ReactionButtons>` 컴포넌트를 만들 것이다. 각각의 버튼에 현재 리액션 수와 함께 버튼을 보이는 것부터 시작할 것이다.
  ```js
  // features/posts/ReactionButton.js

  import React from 'react'

  const reactionEmoji = {
    thumbsUp: '👍',
    hooray: '🎉',
    heart: '❤️',
    rocket: '🚀',
    eyes: '👀'
  }

  export const ReactionButtons = ({ post }) => {
    const reactionButtons = Object.entries(reactionEmoji).map(([name, emoji]) => {
      return (
        <button key={name} type="button" className="muted-button reaction-button">
          {emoji} {post.reactions[name]}
        </button>
      )
    })

    return <div>{reactionButtons}</div>
  }
  ```
- 데이터에 `post.reactions` 필드가 아직 없어서 `initialState` 포스트 객체와 `reactions: {thumbsUp: 0, hooray: 0, heart: 0, rocket: 0, eyes: 0}`처럼 모든 포스트 안에 데이터를 가지고 있도록 `postAdded` 준비 콜백 함수를 업데이트해야 한다. 
- 이제 사용자가 리액션 버튼을 클릭할 때 포스트의 리액션 수 업데이트를 관리하기 위한 새 리듀서를 정의할 수 있다.
- 포스트 수정처럼 포스트의 아이디와 사용자가 어떤 리액션 버튼을 클릭했는지 알아야 한다. `action.payload`를 `{id, reaction}` 같은 객체로 만들 것이다. 리듀서는 올바른 포스트 객체를 찾을 수 있고 올바른 리액션 필드를 업데이트할 수 있을 것이다.
  ```js
  const postsSlice = createSlice({
    name: 'posts',
    initialState,
    reducers: {
      reactionAdded(state, action) {
        const { postId, reaction } = action.payload
        const existingPost = state.find(post => post.id === postId)
        if (existingPost) {
          existingPost.reactions[reaction]++
        }
      }
      // 다른 리듀서들
    }
  })

  export const { postAdded, postUpdated, reactionAdded } = postsSlice.actions
  ```
- 이전에도 보았지만 `createSlice`는 리듀서에서 "변동하는" 로직을 작성할 수 있도록 해준다. 만약 `createSlice`와 이머 라이브러리를 사용하지 않는다면 `existingPost.reactions[reaction]++`는 물론 기존의 `post.reactions` 객체를 변동하지만 우리의 앱의 모든 곳에 버그를 발생시킬 수 있다. 왜냐하면 리듀서의 룰을 지키지 않았기 때문이다. 하지만 `createSlice`를 사용하고 있기 때문에 간단한 방법으로 더 복잡한 업데이트 로직을 작성할 수 있고 이머가 해당 코드를 안전하고 불변하는 업데이트로 바꿔주는 일을 할 수 있다.
- **우리의 액션 객체는 단지 무슨 일이 일어나는지 설명하는데 필요한 최소한의 정보만 가지고 있다는 것**에 유의하라. 업데이트가 필요한 포스트와 어떤 리액션 이름이 클릭되었는지 알고 있다. 우리는 새 리액션 카운터 값을 계산할 수 있고 값을 액션에 넣을 수 있지만, **가능한 작게 액션 객체를 두고 리듀서에서 상태 업데이트 계산을 하는 것이 좋다.** 이는 **리듀서가 새 상태값을 계산하는데 필요한 더 많은 로직을 담고 있다는 것**을 의미한다.
- 정보: 이머를 사용할 때 기존의 상태 객체를 "변동"시키거나 새 상태 값을 반환할 수 있지만 동시에는 할 수 없다.
- 마지막 단계로 사용자가 버튼을 클릭했을 때 `reactionAdded` 액션을 디스패치 하기 위해 `<ReactionButtons>`를 업데이트할 것이다.
  ```js
  // features/posts/ReactionButton.js

  import React from 'react'
  import { useDispatch } from 'react-redux'
  import { reactionAdded } from './postsSlice'

  const reactionEmoji = {
    thumbsUp: '👍',
    hooray: '🎉',
    heart: '❤️',
    rocket: '🚀',
    eyes: '👀'
  }

  export const ReactionButtons = ({ post }) => {
    const dispatch = useDispatch()

    const reactionButtons = Object.entries(reactionEmoji).map(([name, emoji]) => {
      return (
        <button 
          key={name} 
          type="button" 
          className="muted-button reaction-button"
          onClick={() => 
            dispatch(reactionAdded({ postId: post.id, reaction: name }))
          }
        >
          {emoji} {post.reactions[name]}
        </button>
      )
    })

    return <div>{reactionButtons}</div>
  }
  ```
-  이제 리액션 버튼을 클릭할 때마다 카운터가 증가한다. `<PostsList>`의 리액션 버튼을 클릭한 다음 `<SinglePostPage>`에서 포스트 자체를 보는 것처럼 앱의 다른 부분을 둘러보아도 언제든 이 포스트를 보아도 올바른 카운터 값이 표시되어야 한다.

<br>
<br>

# 5. 정리
- **어떤 리액트 컴포넌트든 필요하면 리덕스 스토어에서 데이터를 사용할 수 있다.**
  - 어떤 컴포넌트든 리덕스 스토어에 있는 데이터라면 읽을 수 있다.
  - 여러 컴포넌트들은 동시에 똑같은 데이터를 읽을 수 있다.
  - 컴포넌트들은 스스로 렌더링하는데 필요한 가장 작은 양의 데이터를 추출할 수 있다.
  - 컴포넌트들은 어떤 UI가 렌더링이 필요한지 결정하기 위해 인자, 상태, 리덕스 스토어의 값들을 결합할 수 있다. 스토어의 여러 조각의 데이터를 읽을 수 있고 보여주기 위해 필요에 따라 데이터를 재구성할 수 있다.
  - 어떤 컴포넌트든 상태를 업데이트하기 위해 액션을 디스패치 할 수 있다.
- **리덕스 액션 생성자는 올바른 내용의 액션 객체를 준비할 수 있다.**
  - `createSlice`와 `createAction`은 액션 페이로드를 반환하는 "준비 콜백"을 받을 수 있다.
  - 고유의 아이디와 다른 랜덤 값들은 리듀서에서 계산되는 것이 아니라 액션에 넣어야 한다.
- **리듀서들은 실제 상태 업데이트 로직을 가지고 있어야 한다.**
  - 리듀서들은 다음 상태를 계산하는데 필요한 어떤 로직이든 포함하고 있어야 한다.
  - 액션 객체는 어떤 일이 일어나는지 설명하는 충분한 정보를 가지고 있어야 한다.

<br>
<br>