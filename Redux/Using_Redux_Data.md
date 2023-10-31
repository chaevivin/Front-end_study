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

## 2. 포스트 편집
- 