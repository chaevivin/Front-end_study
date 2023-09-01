# REST API
- HTTP의 장점을 최대한 활용할 수 있는 아키텍처로서 REST(REpresentational State Transfer)를 소개
- HTTP 프로토콜을 의도에 맞게 디자인하도록 유도
- RESTful: REST의 기본 원칙을 성실히 지킨 서비스 디자인
- REST는 HTTP를 기반으로 클라이언트가 서버의 리소스에 접근하는 방식을 규정한 아키텍처
- REST API: REST를 기반으로 서비스 API를 구현하는 것

<br>
<br>

## 1. REST API의 구성
- REST API는 자원(resource), 행위(verb), 표현(representations) 3가지로 구성
- REST는 자체 표현 구조(self-descriptriveness)로 구성되어 REST API만으로 HTTP 요청 내용 이해 가능
  | 구성 요소 | 내용 | 표현 방법 |
  | :---: | :---: | :---: |
  | 자원 | 자원 | URI(엔드포인트) |
  | 행위 | 자원에 대한 행위 | HTTP 요청 메서드 |
  | 표현 | 자원에 대한 행위의 구체적 내용 | 페이로드 |

<br>
<br>

## 2. REST API 설계 원칙
- RESTful API를 설계하는 중심 규칙
  1. **URI는 리소스를 표현하는 데 집중**
  2. **행위에 대한 정의는 HTTP 요청 메서드**를 통해 해야함

<br>

**(1) URI는 리소스를 표현**
- URI는 리소스를 표현하는 데 중점을 두어야 함
- 리소스를 식별할 수 있는 이름은 동사보다 명사 사용
- 이름에 get 같은 행위에 대한 표현 X
  ```
  # bad
  GET /getTodos/1
  GET /todos/show/1

  # good
  GET /todos/1
  ```

<br>

**(2) 리소스에 대한 행위는 HTTP 요청 메서드로 표현**
- HTTP 요청 메서드: 클라이언트가 서버에게 요청의 종류와 목적(리소스에 대한 행위)를 알리는 방법
- 주로 5가지 요청 메서드를 사용하여 CRUD 구현
  | HTTP 요청 메서드 | 종류 | 목적 | 페이로드 |
  | :---: | :---: | :---: | :---: |
  | GET | index/retrieve | 모든/특정 리소스 취득 | X |
  | POST | create | 리소스 생성 | O |
  | PUT | replace | 리소스의 전체 교체 | O |
  | PATCH | modify | 리소스의 일부 수정 | O |
  | DELETE | delete | 모든/특정 리소스 삭제 | X |
- 리소스에 대한 행위는 URI에 표현 X
  - 리소스에 대한 행위 명확히 표현
  ```
  # bad
  GET /todos/delete/1

  # good
  DELETE/todos/1
  ```

<br>
<br>

## 3. JSON Server를 이용한 REST API 실습
- HTTP 요청을 전송하고 응답을 받으려면 서버가 필요
- JSON Server를 사용해 가상 REST API 서버를 구축하여 HTTP 요청을 전송하고 응답을 받는 실습

<br>

### 3.1. JSON Server 설치
- JSON Server는 json 파일을 사용하여 가상 REST API 서버를 구축할 수 있는 툴
- 사용법
  - npm으로 JSON Server 설치
  ```
  $ mkdir json-server-exam && cd json-server-exam
  $ npm init -y
  $ npm install json-server--save-dev
  ```

<br>

### 3.2. db.json 파일 생성
- 프로젝트 루트 폴더(/json-server-exam)에 db.json 파일 생성
  - db.json 파일은 리소스를 제공하는 데이터베이스 역할
  ```json
  {
    "todos": [
      {
        "id": 1,
        "content": "HTML",
        "completed": true
      },
      {
        "id": 2,
        "content": "CSS",
        "completed": false
      },
      {
        "id": 3,
        "content": "JavaScript",
        "completed": true
      },
    ]
  }
  ```

<br>

### 3.3. JSON Server 실행
- 터미널에 명령을 입력하여 JSON Server 실행
- JSON Server가 데이터베이스 역할을 하는 db.json 파일의 변경을 감지하게 하려면 watch 옵션 추가
  ```
  ## 기본 포트(3000) 사용 / watch 옵션 적용
  $ json-server--watch db.json
  ```
  - 기본 포트를 변경하려면 port 옵션 추가
  ```
  ## 포트 변경 / watch 옵션 적용
  $ json-server--watch db.json--port 5000
  ```
- 위와 같이 매번 명령어를 입력하는 것보다 package.json 파일의 scripts를 수정하여 JSON Server 실행
  - package.json 파일에서 불필요한 항목 삭제
  ```json
  {
    "name": "json-server-exam",
    "version": "1.0.0",
    "scripts" {
      "start": "json-server --watch db.json"
    },
    "devDependencies": {
      "json-server": "^0.16.1"
    }
  }
  ```
  - 터미널에서 npm start 명령어를 입력하여 JSON Server 실행

<br>

### 3.4. GET 요청
- todos 리소스에서 모든 todo를 취득(index)
- JSON Server의 루트 폴더(/json-sever-exam)에 public 폴더를 생성하고 JSON Server를 중단한 후 재실행
- public 폴더 다음 get_index.html을 추가하고 브라우저에 http://localhost:3000/get_index.html로 접속
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <pre></pre>
      <script>
        // XMLHttpRequest 객체 생성
        const xhr = new XMLHttpRequest();

        // HTTP 요청 초기화
        // todos 리소스에서 모든 todo를 취득(index)
        xhr.open('GET', '/todos');

        // HTTP 요청 전송
        xhr.send();

        // load 이벤트는 요청이 성공적으로 완료된 경우 발생
        xhr.onload = () => {
          // status 프로퍼티 값이 200이면 정상적으로 응답된 상태
          if (xhr.status === 200) {
            document.querySelector('pre').textContent = xhr.response;
          } else {
            console.error('Error', xhr.status, shr.statusText);
          }
        };
      </script>
    </body>
  </html>
  ```
- todos 리소스에 id를 사용하여 특정 todo를 취득(retrive)
- public 폴더 다음에 get_retrive.html을 추가하고 브라우저에서 http://localhost:3000/get_retrive.html로 접속
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <pre></pre>
      <script>
        // XMLHttpRequest 객체 생성
        const xhr = new XMLHttpRequest();

        // HTTP 요청 초기화
        // todos 리소스에서 id를 사용하여 특정 todo를 취득(retrive)
        xhr.open('GET', '/todos/1');

        // HTTP 요청 전송
        xhr.send();

        // load 이벤트는 요청이 성공적으로 완료된 경우 발생
        xhr.onload = () => {
          // status 프로퍼티 값이 200이면 정상적으로 응답된 상태
          if (xhr.status === 200) {
            document.querySelector('pre').textContent = xhr.response;
          } else {
            console.error('Error', xhr.status, shr.statusText);
          }
        };
      </script>
    </body>
  </html>
  ```

<br>

### 3.5. POST 요청
- todos 리소스에 새로운 todo를 생성
- POST 요청 시에는 setRequestHeader 메서드를 사용하여 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입 지정
- public 폴더 다음 post.html을 추가하고 브라우저에 http://localhost:3000/post.html로 접속
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <pre></pre>
      <script>
        // XMLHttpRequest 객체 생성
        const xhr = new XMLHttpRequest();

        // HTTP 요청 초기화
        // todos 리소스에 새로운 todo 생성
        xhr.open('POST', '/todos');

        // 요청 몸체에 담아 서버로 전송할 MIME 타입 지정
        xhr.setRequestHeader('content-type', 'application/json');

        // HTTP 요청 전송
        // 새로운 todo를 생성하기 위해 페이로드를 서버에 전송
        xhr.send(JSON.stringify({ id: 4, content: 'Angular', completed: false }));

        // load 이벤트는 요청이 성공적으로 완료된 경우 발생
        xhr.onload = () => {
          // status 프로퍼티 값이 200(OK) 또는 201(Created)이면 정상적으로 응답된 상태
          if (xhr.status === 200 || xhr.status === 201) {
            document.querySelector('pre').textContent = xhr.response;
          } else {
            console.error('Error', xhr.status, shr.statusText);
          }
        };
      </script>
    </body>
  </html>
  ```

<br>

### 3.6. PUT 요청
- PUT은 특정 리소스 전체를 교체할 때 사용
- todos 리소스에서 id로 todo를 특정하여 id를 제외한 리소스 전체 교체
- PUT 요청 시에는 setRequestHeader 메서드를 사용하여 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입 지정
- public 폴더에 put.html 추가하고 브라우저에서 http://localhost:3000/put.html로 접속
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <pre></pre>
      <script>
        // XMLHttpRequest 객체 생성
        const xhr = new XMLHttpRequest();

        // HTTP 요청 초기화
        // todos 리소스에서 id로 todo를 특정하여 id를 제외한 리소스 전체 교체
        xhr.open('PUT', '/todos/4');

        // 요청 몸체에 담아 서버로 전송할 MIME 타입 지정
        xhr.setRequestHeader('content-type', 'application/json');

        // HTTP 요청 전송
        // 리소스 전체를 교체하기 위해 페이로드를 서버에 전송
        xhr.send(JSON.stringify({ id: 4, content: 'React', completed: true }));

        // load 이벤트는 요청이 성공적으로 완료된 경우 발생
        xhr.onload = () => {
          // status 프로퍼티 값이 200이면 정상적으로 응답된 상태
          if (xhr.status === 200) {
            document.querySelector('pre').textContent = xhr.response;
          } else {
            console.error('Error', xhr.status, shr.statusText);
          }
        };
      </script>
    </body>
  </html>
  ```

<br>

### 3.7. PATCH 요청
- PATCH는 특정 리소스의 일부를 수정할 때 사용
- todos 리소스의 id로 todo를 특정하여 completed만 수정
- PATCH 요청 시에는 setReqeustHeader 메서드를 사용하여 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입 지정
- public 폴더에 patch.html을 추가하고 브라우저에서 http://localhost:3000/patch.html 접속
```html
<!DOCTYPE html>
  <html>
    <body>
      <pre></pre>
      <script>
        // XMLHttpRequest 객체 생성
        const xhr = new XMLHttpRequest();

        // HTTP 요청 초기화
        // todos 리소스의 id로 todo를 특정하여 completed만 수정
        xhr.open('PATCH', '/todos/4');

        // 요청 몸체에 담아 서버로 전송할 MIME 타입 지정
        xhr.setRequestHeader('content-type', 'application/json');

        // HTTP 요청 전송
        // 리소스를 수정하기 위해 페이로드를 서버에 전송
        xhr.send(JSON.stringify({ completed: false }));

        // load 이벤트는 요청이 성공적으로 완료된 경우 발생
        xhr.onload = () => {
          // status 프로퍼티 값이 200이면 정상적으로 응답된 상태
          if (xhr.status === 200) {
            document.querySelector('pre').textContent = xhr.response;
          } else {
            console.error('Error', xhr.status, shr.statusText);
          }
        };
      </script>
    </body>
  </html>
```
<br>

### 3.8. DELETE 요청
- todos 리소스에서 id를 사용하여 todo를 삭제
- public 폴더 다음 delete.html을 추가하고 브라우저에서 http://localhost:3000/delete.html로 접속
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <pre></pre>
      <script>
        // XMLHttpRequest 객체 생성
        const xhr = new XMLHttpRequest();

        // HTTP 요청 초기화
        // todos 리소스에 id를 사용하여 todo 삭제
        xhr.open('DELETE', '/todos/4');

        // HTTP 요청 전송
        xhr.send();

        // load 이벤트는 요청이 성공적으로 완료된 경우 발생
        xhr.onload = () => {
          // status 프로퍼티 값이 200이면 정상적으로 응답된 상태
          if (xhr.status === 200) {
            document.querySelector('pre').textContent = xhr.response;
          } else {
            console.error('Error', xhr.status, shr.statusText);
          }
        };
      </script>
    </body>
  </html>
  ```

<br>
<br>