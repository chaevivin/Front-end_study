# 프로미스
- 자바스크립트는 비동기 처리를 위한 하나의 패턴으로 콜백 함수 사용
  - 단점
    - 전통적인 콜백 패턴은 콜백 헬로 인해 가독성이 나쁨
    - 비동기 처리 중 발생한 에러의 처리가 곤란하며 여러 개의 비동기 처리를 한 번에 처리하는 데도 한계
- ES6에서는 비동기 처리를 위한 또 다른 패턴으로 프로미스 도입
  - 프로미스는 전통적인 콜백 패턴이 가진 단점을 보완하며 비동기 처리 시점을 명확하게 표현 가능

<br>
<br>

## 1. 비동기 처리를 위한 콜백 패턴의 단점

<br>

### 1.1. 콜백 헬
- GET 요청을 위한 비동기 함수
  - get 함수 내부의 onload 이벤트 핸들러는 get 함수가 종료된 이후 실행 
  ```js
  const get = url => {
    const xhr = new XMLHttpRequest();
    xhr.open('GET', url);
    xhr.send();

    xhr.onload = () => {
      if (xhr.status === 200) {
        // 서버의 응답을 콘솔에 출력
        console.log(JSON.parse(xhr.response));
      } else {
        console.error(`${xhr.status} ${xhr.statusText}`);
      }
    };
  };

  // id가 1인 post를 취득
  get('https://jsonplaceholder.typicode.com/posts/1');
  /*
  {
    "userId": 1,
    "id": 1,
    "title": "sunt aut facere ...",
    "body": "quia et suscipit ..."
  }
  */
  ```

<br>

- get 함수가 서버의 응답 결과를 반환하게 하려면?
  - get 함수는 비동기 함수
    - 비동기 함수 : 함수 내부에 비동기로 동작하는 코드를 포함한 함수
    - get 함수가 비동기 함수인 이유 : get 함수 내부의 **onload 이벤트 핸들러가 비동기로 동작**
    - **비동기 함수를 호출하면 함수 내부의 비동기로 동작하는 코드가 완료되지 않았다 해도 기다리지 않고 즉시 종료**
    - **비동기 함수 내부의 비동기로 동작하는 코드는 비동기 함수가 종료된 이후에 완료**
    - **비동기 함수 내부의 비동기로 동작하는 코드에서 처리 결과를 외부로 반환하거나 상위 스코프의 변수에 할당하면 기대한 대로 동작 X** 
  - setTimeout 함수는 비동기 함수
    - 콜백 함수의 호출이 비동기로 동작
    - 비동기 함수인 setTimeout 함수의 콜백 함수는 setTimeout 함수가 종료된 이후에 호출
    - setTimeout 함수 내부의 콜백 함수에서 처리 결과를 외부로 반환하거나 상위 스코프의 변수에 할당하면 기대한 대로 동작 X
    ```js
    let g = 0;

    // 비동기 함수인 setTimeout 함수는 콜백 함수의 처리 결과를 외부로 반환하거나 상위 스코프의 변수에 할당 X
    setTimeout(() => { g = 100; }, 0);
    console.log(g);    // 0
    ```
<br>

```js
const get = url => {
  const xhr = new XMLHttpRequest();
  xhr.open('GET', url);
  xhr.send();

  xhr.onload = () => {
    if (xhr.status === 200) {
      // 서버의 응답을 반환
      return JSON.parse(xhr.response);
    } 
    console.error(`${xhr.status} ${xhr.statusText}`);
  };
};

// id가 1인 post를 취득
get('https://jsonplaceholder.typicode.com/posts/1');
console.log(response);    // undefined
```
위의 코드를 살펴보면,
- get 함수가 호출되면 XMLHttpRequest 객체 생성 -> HTTP 요청 초기화 -> HTTP 요청 전송 -> xhr.onload 이벤트 핸들러 프로퍼티에 이벤트 핸들러를 바인딩 후 종료 
- get 함수에 명시적인 반환문이 없으므로 get 함수는 undefined 반환
- xhr.onload 이벤트 핸들러 프로퍼티에 바인딩한 이벤트 핸들러의 반환문은 get 함수의 반환문 X
  - get 함수는 반환문이 생략 -> undefined 반환
  - 함수의 반환값은 명시적으로 호출한 다음에 캐치 가능 -> onload 이벤트 핸들러는 get 함수가 호출하지 않기 때문에 onload 이벤트 핸들러의 반환값은 캐치 불가능

<br>

```html
<!DOCTYPE html>
<html>
  <body>
    <input type="text">
    <script>
      document.querySelector('input').oninput = function () {
        console.log(this.value);
        // 이벤트 핸들러에서의 반환은 의미 X
        return this.value;
      };
    </script>
  </body>
</html>
```
```js
let todos;

// GET 요청을 위한 비동기 함수
const get = url => {
  const xhr = new XMLHttpRequest();
  xhr.open('GET', url);
  xhr.send();

  xhr.onload = () => {
    if (xhr.status === 200) {
      // 서버의 응답을 상위 스코프의 변수에 할당
      todos = JSON.parse(xhr.response);
    } else {
      console.error(`${xhr.status} ${xhr.statusText}`);
    }
  };
};

// id가 1인 post를 취득
get('https://jsonplaceholder.typicode.com/posts/1');
console.log(todos);    // undefined
```
위의 코드를 살펴보면,
- 서버의 응답을 상위 스코프의 변수에 할당
- 이 또한 기대한 대로 동작 X
- xhr.onload 이벤트 핸들러 프로퍼티에 바인딩한 이벤트 핸들러는 언제나 마지막 console.log가 종료된 이후 호출 -> todos에 서버의 응답 결과 할당 이전
- xhr.onload 이벤트 핸들러에서 서버의 응답을 상위 스코프의 변수에 할당하면 처리 순서 보장 X
  - 비동기 함수 get이 호출되면 함수 코드를 평가하는 과정에서 get 함수의 실행 컨텍스트 생성 -> 실행 컨텍스트 스택(콜 스택)에 푸시 -> 함수 코드 실행 과정에서 xhr.onload 이벤트 핸들러 프로퍼티에 이벤트 핸들러 바인딩 
  - get 함수가 종료하면 get 함수의 실행 컨텍스트가 콜 스택에서 팝되고, 곧바로 마지막 console.log 실행 -> console.log의 실행 컨텍스트가 생성되어 실행 컨텍스트 스택에 푸시 
  - 서버로부터 응답이 도착하면 xhr 객체에서 load 이벤트가 발생 -> **xhr.onload 이벤트 핸들러 프로퍼티에 바인딩한 이벤트 핸들러 즉시 실행 X** -> **xhr.onload 이벤트 핸들러는 load 이벤트가 발생하면 일단 태스크 큐에 저장되어 대기하다가, 콜 스택이 비면 이벤트 루프에 의해 콜 스택으로 푸시되어 실행**
    - 이벤트 핸들러 실행 과정: 이벤트 핸들러의 평가 -> 이벤트 핸들러의 실행 컨텍스트 생성 -> 콜 스텍에 푸시 -> 이벤트 핸들러 실행 과정
  - xhr.onlaod 이벤트가 실행되는 시점에는 콜 스택이 빈 상태여야 하므로 마지막 console.log는 이미 종료된 이후
    - xhr.onload 이벤트 핸들러에서 상위 스코프의 변수에 서버의 응답 결과를 할당하기 이전에 console.log가 먼저 호출되어 undefined 출력
- **비동기 함수는 비동기 처리 결과를 외부에 반환 X, 상위 스코프의 변수에 할당 X**
- **비동기 함수의 처리 결과(서버의 응답 등)에 대한 후속 처리는 비동기 함수 내부에서 수행해야 함**
- **비동기 함수를 범용적으로 사용하기 위해 비동기 함수에 비동기 처리 결과에 대한 후속 처리를 수행하는 콜백 함수를 전달하는 것이 일반적**
- **필요에 따라 비동기 처리가 성공하면 호출된 콜백 함수와 비동기 처리가 실패햐면 호출될 콜백 함수 전달 가능**

<br>

```js
const get = (url, successCallback, failureCallback) => {
  const xhr = new XMLHttpRequest();
  xhr.open('GET', url);
  xhr.send();

  xhr.onload = () => {
    if (xhr.status === 200) {
      // 서버의 응답을 콜백 함수에 인수로 전달하면서 호출하여 응답에 대한 후속 처리
      successCallback(JSON.parse(xhr.response));
    } else {
      // 에러 정보를 콜백 함수에 인수로 전달하면서 호출하여 에러 처리
      failureCallback(xhr.status);
    }
  };
};

// id가 1인 post를 취득
// 서버의 응답에 대한 후속 처리를 위한 비동기 함수인 get에 전달
get('https://jsonplaceholder.typicode.com/posts/1', console.log, console.error);
/*
  {
    "userId": 1,
    "id": 1,
    "title": "sunt aut facere ...",
    "body": "quia et suscipit ..."
  }
*/
```
위의 코드를 살펴보면,
- **콜백 헬** 발생
  - 콜백 함수를 통해 비동기 처리 결과에 대한 후속 처리를 수행하는 비동기 함수가 비동기 처리 결과를 가지고 또다시 비동기 함수를 호출해야 한다면 콜백 함수 호출이 중첩되어 복잡도가 높아지는 현상

<br>

```js
const get = (url, callback) => {
  const xhr = new XMLHttpRequest();
  xhr.open('GET', url);
  xhr.send();

  xhr.onload = () => {
    if (xhr.status === 200) {
      // 서버의 응답을 콜백 함수에 인수로 전달하면서 호출하여 응답에 대한 후속 처리
      callback(JSON.parse(xhr.response));
    } else {
      console.error(`${xhr.status} ${xhr.statusText}`);
    }
  };
};

const url = 'https://jsonplaceholder.typicode.com';

// id가 1인 post를 취득
get(`${url}/posts/1`, ({ userId }) => {
  console.log(userId);    // 1
  // post의 userId를 사용하여 user 정보 취득
  get(`${url}/users/${userId}`, userInfo => {
    console.log(userInfo);    // {id: 1, name: "Leanne Graham", username: "Bret", ...}
  });
});
```
위의 코드를 살펴보면,
- GET 요청을 통해 서버로부터 응답(id가 1인 post)을 취득하고 이 데이터를 사용하여 또다시 GET 요청 
- 콜백 헬을 가독성을 나쁘게 하며 실수를 유발하는 원인
- 콜백 헬이 발생하는 전형적인 사례
  ```JS
  get('/step1', a => {
    get(`/step2/${a}`, b => {
      get(`/step3/${b}`, c => {
        get(`/step4/${c}`, d => {
          console.log(d);
        });
      });
    });
  });
  ```

<br>

### 1.2. 에러 처리의 한계
- 비동기 처리를 위한 콜백 패턴의 문제점 중에서 가장 심각한 것은 에러 처리가 곤란하다는 것
  ```js
  try {
    setTimeout(() => { throw new Error('Error'); }, 1000);
  } catch(e) {
    // 에러를 캐치하지 못한다
    console.error('캐치한 에러', e);
  }
  ```
  - try 코드 블록 내에서 호출한 setTimeout 함수는 1초 후에 콜백 함수가 실행되도록 타이머를 설정하고, 이후 콜백 함수는 에러를 발생시킴
  - 에러는 catch 코드 블록에서 캐치되지 않는 이유
    - 비동기 함수인 setTimeout이 호출되면 setTimeout 함수의 실행 컨텍스트가 생성되어 콜 스택에 푸시되어 실행
    - setTimeout은 비동기 함수이므로 콜백 함수가 호출되는 것을 기다리지 않고 즉시 종료되어 콜 스택에서 제거 -> 타이머가 만료되면 setTimeout 함수의 콜백 함수는 태스크 큐로 푸시되고 콜 스택이 비어졌을 때 이벤트 루프에 의해 콜 스택으로 푸시되어 실행
    - setTimeout 함수의 콜백 함수가 실행될 때 setTimeout 함수는 이미 콜 스택에서 제거된 상태 = setTimeout 함수의 콜백 함수를 호출한 것이 setTimeout 함수가 아님
      - setTimeout 함수의 콜백 함수의 호출자(caller)가 setTimeout 함수라면 콜 스택의 현재 실행 중인 실행 컨텍스트가 콜백 함수의 실행 컨텍스트일 때 현재 실행 중인 실행 컨텍스트의 하위 실행 컨텍스트가 setTimeout 함수여야 함
    - **에러는 호출자 방향으로 전파**
      - 콜 스택의 아래 방향(실행 중인 실행 컨텍스트가 푸시되기 직전에 푸시된 실행 컨텍스트 방향)으로 전파
    - setTimeout 함수의 콜백 함수를 호출한 것은 setTimeout 함수가 아니기 때문에 setTimeout 함수의 콜백 함수가 발생시킨 에러는 catch 목록에서 캐치 X
- 비동기 처리를 위한 콜백 패턴은 콜백 헬이나 에러 처리가 곤란하다는 문제 -> 프로미스가 도입

<br>
<br>

## 2. 프로미스의 생성
- Promise 생성자 함수를 new 연산자와 함께 호출하면 프로미스(Promise 객체) 생성
- ES6에 도입된 Promise는 호스트 객체가 아닌 ECMAScript 사양에 정의된 표준 빌트인 객체
- Promise 생성자 함수는 비동기 처리를 수행할 콜백 함수(ECMAScript 사양에서는 executor 함수라고 부름)를 인수로 전달받는데 이 콜백 함수는 resolve와 reject 함수를 인수로 전달받음
  ```js
  // 프로미스 생성
  const promise = new Promise((resolve, reject) => {
    // Promise 함수으 콜백 함수 내부에서 비동기 처리 수행
    if (/* 비동기 처리 성공 */) {
      resolve('result');
    } else {    /* 비동기 처리 실패 */
      reject('failure reason');
    }
  });
  ```
  - Promise 생성자 함수가 인수로 전달받은 콜백 함수 내부에서 비동기 처리 수행
  - 비동기 처리가 성공하면 resolve 함수 호출
  - 비동기 처리가 실패하면 reject 함수 호출

<br>

```js
// GET 요청을 위한 비동기 함수
const promiseGet = url => {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();
    xhr.open('GET', url);
    xhr.send();

    xhr.onload = () => {
      if (xhr.status === 200) {
        // 성공적으로 응답을 전달받으면 resolve 함수를 호출
        resolve(JSON.parse(xhr.response));
      } else {
        // 에러 처리를 위해 reject 함수 호출
        reject(new Error(xhr.status));
      }
    };
  });
};

// promiseGet 함수는 프로미스 반환
promiseGet('https://jsonplaceholder.typicode.com/posts/1');
```
위의 코드를 살펴보면,
- 앞에서 살펴본 비동기 함수 get을 프로미스를 사용해 다시 구현
- 비동기 함수인 promiseGet은 함수 내부에서 프로미스를 생성하고 반환
- 비동기 처리는 Promise 생성자 함수가 인수로 전달받은 콜백 함수 내부에서 수행

<br>

- 프로미스는 현재 비동기 처리가 어떻게 진행되고 있는지를 나타내는 상태(state) 정보를 가짐
  | 프로미스의 상태 정보 | 의미 | 상태 변경 조건 |
  | :---: | :---: | :---: |
  | pending | 비동기 처리가 아직 수행되지 않은 상태 | 프로미스가 생성된 직후 기본 상태 |
  | **fulfilled** | 비동기 처리가 수행된 상태(성공) | resolve 함수 호출 |
  | **rejected** | 비동기 처리가 수행된 상태(실패) | reject 함수 호출 |
- 생성된 직후의 프로미스는 기본적으로 pending상태
- 이후 비동기 처리가 수행되면 처리 결과에 따라 프로미스 상태가 변경
  - 비동기 처리 성공: resolve 함수를 호출해 프로미스를 fulfilled 상태로 변경
  - 비동기 처리 실패: reject 함수를 호출해 프로미스를 rejected 상태로 변경
- **프로미스 상태는 resolve 또는 reject 함수를 호출하는 것으로 결정**
- settled 상태: fulfilled 또는 rejected 상태
  - fulfilled 또는 rejected 상태와 상관없이 pending이 아닌 상태로 비동기 처리가 수행된 상태
- 프로미스는 pending 상태에서 fulfilled 또는 rejected 상태, 즉 settled 상태로 변화 가능하지만 일단 settled 상태가 되면 다른 상태로 변화 불가
- 프로미스는 비동기 처리 상태와 더불어 비동기 처리 결과도 상태로 가짐
  ```js
  // fulfilled된 프로미스
  const fulfilled = new Promise(resolve => resolve(1));

  // rejected된 프로미스
  const rejected = new Promise((_, reject) => reject(new Error('error occurred')));
  ```
  - 비동기 처리가 성공하면 프로미스는 pending 상태에서 fulfilled 상태로 변화하고 비동기 처리 결과인 1을 값으로 가짐
  - 비동기 처리가 실패하면 프로미스는 pending 상태에서 rejected 상태로 변화하고 비동기 처리 결과인 Error 객체를 값으로 가짐
  - **프로미스는 비동기 처리 상태와 처리 결과를 관리하는 객체**

<br>
<br>

## 3. 프로미스의 후속 처리 메서드
- 프로미스의 비동기 처리 상태가 변화하면 이에 따른 후속 처리 필요
- 프로미스는 후속 메서드 then, catch, finally 제공
- **프로미스의 비동기 처리 상태가 변화하면 후속 처리 메서드에 인수로 전달한 콜백 함수가 선택적으로 호출** 
  - 이때 후속 처리 메서드의 콜백 함수에 프로매스의 처리 결과가 인수로 전달됨
- 모든 후속 처리 메서드는 프로미스를 반환하며, 비동기로 동작

<br>

### 3.1. Promise.prototype.then
- then 메서드는 두 개의 콜백 함수를 인수로 전달받ㅇ,ㅁ
  - 첫 번째 콜백함수: 프로미스가 fulfilled 상태(resolve 함수가 호출된 상태)가 되면 호출. 이때 콜백 함수는 프로미스의 비동기 처리 결과를 인수로 전달받음
  - 두 번째 콜백함수: 프로미스가 rejected 상태(rejet 함수가 호출된 상태)가 되면 호출. 이때 콜백 함수는 프로미스의 에러를 인수로 전달받음
  - 첫 번째 콜백 함수는 비동기 처리가 성공했을 때 호출되는 성공 처리 콜백 함수, 두 번째 콜백 함수는 비동기 처리가 실패했을 때 호출되는 실패 처리 콜백 함수
  ```js
  // fulfilled
  new Promise(resolve => resolve('fulfilled'))
    .then(v => console.log(v), e => console.error(e));    // fulfilled

  // rejected
  new Promise((_, reject) => reject(new Error('rejected')))
    .then(v => console.log(v), e => console.error(e));    // Error: rejected
  ```
- then 메서드는 언제나 프로미스 반환
- then 메서드의 콜백 함수가 프로미스를 반환하면 그 프로미스를 그대로 반환하고, 콜백 함수가 프로미스가 아닌 값을 반환하면 그 값을 암묵적으로 resolve 또는 reject하여 프로미스를 생성해 반환

<br>

### 3.2. Promise.prototype.catch
- catch 메서드는 한 개의 콜백 함수를 인수로 전달 받은
- catch 메서드의 콜백 함수는 프로미스가 rejected 상태인 경우만 호출
  ```js
  // rejected
  new Promise((_, reject) => reject(new Error('rejected')))
    .catch(e => console.error(e));    // Error: rejected
  ```
- catch 메서드는 then(undefined, onRejected)과 동일하게 동작하므로 then 메서드와 마찬가지로 언제나 프로미스 반환
  ```js
  // rejected
  new Promise((_, reject) => reject(new Error('rejected')))
    .then(undefined, e => console.error(e));    // Error: rejected
  ```

<br>

### 3.3. Promise.prototype.finally
- finally 메서드는 한 개의 콜백 함수를 인수로 전달받음
- finally 메서드의 콜백 함수는 프로미스의 성공(fulfilled) 또는 실패(rejected)와 상관없이 무조건 한 번 호출
- finally 메서드는 프로미스의 상태와 상관없이 공통적으로 수행해야 할 처리 내용이 있을 때 유용
- finally 메서드는 언제나 프로미스를 반환
  ```js
  new Promise(() => {})
    .finally(() => console.log('finally'));    // finally
  ```

<br>

- 프로미스로 구현한 비동기 함수 get을 사용해 후속 처리 구현
```js
const promiseGet = url => {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();
    xhr.open('GET', url);
    xhr.send();

    xhr.onload = () => {
      if (xhr.status === 200) {
        // 성공적으로 응답을 전달받으면 resolve 함수를 호출
        resolve(JSON.parse(xhr.response));
      } else {
        // 에러 처리를 위해 reject 함수 호출
        reject(new Error(xhr.status));
      }
    };
  });
};

// promiseGet 함수는 프로미스 반환
promiseGet('https://jsonplaceholder.typicode.com/posts/1')
  .then(res => console.log(res))
  .catch(err => console.error(err))
  .finally(() => console.log('bye'));
```

<br>
<br>

## 4. 프로미스의 에러 처리
- 프로미스는 에러를 문제없이 처리 가능
- 비동기 처리에서 발생한 에러는 then 메서드의 두 번째 콜백 함수로 처리 가능
  ```js
  const wrongUrl = 'https://jsonplaceholder.typicode.com/XXX/1';

  // 부적절한 URL이 저장되었기 때문에 에러 발생
  promiseGet(wrongUrl).then(
    res => console.log(res),
    err => console.error(err)
  );    // Error: 404
  ```
- 비동기 처리에서 발생한 에러는 프로미스의 후속 처리 메서드 catch를 사용해 처리 가능
  ```js
  const wrongUrl = 'https://jsonplaceholder.typicode.com/XXX/1';

  // 부적절한 URL이 저장되었기 때문에 에러 발생
  promiseGet(wrongUrl)
    .then(res => console.log(res))
    .catch(err => console.error(err));    // Error: 404
  ```
  - catch 메서드를 호출하면 내부적으로 then(undefined, onRejected) 호출되므로 다음과 같이 처리
  ```js
  const wrongUrl = 'https://jsonplaceholder.typicode.com/XXX/1';

  // 부적절한 URL이 저장되었기 때문에 에러 발생
  promiseGet(wrongUrl)
    .then(res => console.log(res))
    .then(undefined, err => console.error(err));    // Error: 404
  ```
- 단, then 메서드의 두 번째 콜백 함수는 첫 번째 콜백 함수에서 발생한 에러를 캐치하지 못하고 코드가 복잡해져서 가독성이 좋지 않음
  ```js
  promiseGet('https://jsonplaceholder.typicode.com/todos/1').then(
    res => console.xxx(res),
    err => console.error(err)
  );    // 두 번째 콜백 함수는 첫 번째 콜백 함수에서 발생한 에러 캐치 X
  ```
- catch 메서드를 모든 then 메서드를 호출한 이후에 호출하면 비동기 처리에서 발생한 에러(rejected 상태) 뿐만 아니라 then 메서드 내부에서 발생한 에러까지 모두 캐치 가능
  ```js
  promiseGet('https://jsonplaceholder.typicode.com/todos/1')
    .then(res => console.xxx(res))
    .catch(err => console.error(err));    // TypeError: console.xxx is not a function
  ```
  - then 메서드에 두 번째 콜백 함수를 전달하는 것보다 catch 메서드를 사용하는 것이 가독성이 좋고 명확
- 에러 처리는 then 메서드에서 하치 말고 catch 메서드에서 하는 것을 권장

<br>
<br>

## 5. 프로미스 체이닝
- 프로미스는 then, catch, finally 후속 처리 메서드를 통해 콜백 헬을 해결
- 프로미스 체이닝(promise chaining): then, catch, finally 후속 처리 메서드는 언제나 프로미스를 반환하므로 연속적으로 호출 가능
  ```js
  const url = 'https://jsonplaceholder.typicode.com';

  // id가 1인 post의 userId 취득
  promiseGet(`${url}/posts/1`)
    // 취득한 post의 userId로 user 정보를 취득
    .then(({userId}) => promiseGet(`${url}/users/${userId}`))
    .then(userInfo => console.log(userInfo))
    .catch(err => console.error(err));
  ```
  - then -> then -> catch 순으로 후속 처리 메서드 호출
  - 후속 처리 메서드의 콜백 함수는 프로미스의 비동기 처리 상태가 변경되면 선택적으로 호출

  | 후속 처리 메서드 | 콜백 함수의 인수 | 후속 처리 메서드의 반환값 |
  | :--- | :--- | :--- |
  | then | promiseGet 함수가 반환한 프로미스가 resolve한 값(id가 1인 post) | 콜백 함수가 반환한 프로미스 |
  | then | 첫 번째 then 메서드가 반환한 프로미스가 resolve한 값(post의 userId로 취득한 user 정보) | 콜백 함수가 반환한 값(undefined)을 resolve한 프로미스 |
  | catch <br> ※ 에러가 발생하지 않으면 호출되지 않음 | promiseGet 함수 또는 앞선 후속 처리 메서드가 반환한 프로미스가 reject한 값 | 콜백 함수가 반환한 값(undefined)을 resolve한 프로미스 |
- 프로미스는 프로미스 체이닝을 통해 비동기 처리 결과를 전달받아 후속 처리를 하므로 비동기 처리를 위한 콜백 패턴에서 발생하던 콜백 헬 발생 X
  - 프로미스도 콜백 패턴을 사용하기 때문에 콜백 함수 사용
- 콜백 패턴은 가독성이 좋지 않음 -> async/await를 통해 해결
  - async/awiat: ES8에 도입된 것으로 프로미스의 후속 처리 메서드 없이 마치 동기 처리처럼 프로미스가 처리 결과를 반환하도록 구현 가능
  ```js
  const url = 'https://jsonplaceholder.typicode.com';

  (async () => {
    // id가 1인 post의 userId를 취득
    const { userId } = await promiseGet(`${url}/posts/1`);
    
     // 취득한 post의 userId로 user 정보 취득
     const userInfo = await promiseGet(`${url}/users/${userId}`);

     console.log(userInfo);
  })();
  ```

<br>
<br>

## 6. 프로미스의 정적 메서드
- Promise는 주로 생성자 함수로 사용되지만 함수도 객체이므로 메서드를 가질 수 있음
- Promise는 5가지 정적 메서드 제공

<br>

### 6.1. Promise.resolve / Promise.reject
- 이 메서드는 이미 존재하는 값을 래핑하여 프로미스를 생성하기 위해 사용
- Promise.resolve
  - 이 메서드는 인수로 전달받은 값을 resolve하는 프로미스를 생성
  ```js
  // 배열을 resolve하는 프로미스를 생성
  const resolvedPromise = Promise.resolve([1, 2, 3]);
  resolvedPromise.then(console.log);    // [1, 2, 3]
  ```
  ```js
  // 위와 동일하게 동작
  const resolvedPromise = new Promise(resolve => resolve([1, 2, 3]));
  resolvedPromise.then(console.log);    // [1, 2, 3]
  ```
- Promise.reject
  - 이 메서드는 인수로 전달받은 값을 reject하는 프로미스 생성
  ```js
  // 에러 객체를 reject하는 프로미스를 생성
  const rejectedPromise = Promise.reject(new Error('Error!'));
  rejectedPromise.catch(console.log);    // Error: Error!
  ```
  ```js
  // 위와 동일하게 동작
  const rejectedPromise = new Promise((_, reject) => reject(new Error('Error!')));
  rejectedPromise.catch(console.log);    // Error: Error!
  ```

<br>

### 6.2. Promise.all
- 이 메서드는 여러 개의 비동기 처리를 모두 병렬 처리할 때 사용
  ```js
  const requestData1 = () => 
    new Promise(resolve => setTimeout(() => resolve(1), 3000));    // 첫 번째 비동기 처리에서 3초 소요
  const requestData2 = () => 
    new Promise(resolve => setTimeout(() => resolve(2), 2000));    // 두 번째 비동기 처리에서 2초 소요
  const requestData3 = () => 
    new Promise(resolve => setTimeout(() => resolve(3), 1000));    // 세 번째 비동기 처리에서 1초 소요

  // 세 개의 비동기 처리를 순차적으로 처리
  const res = [];
  requestData1()
    .then(data => {
      res.push(data);
      return requestData2();
    })
    .then(data => {
      res.pusth(data);
      return requestData3();
    })
    .then(data => {
      res.push(data);
      console.log(res);    // [1, 2, 3] => 약 6초 소요
    })
    .catch(console.error);
  ```
  - 세 개의 비동기 처리는 서로 의존하지 않고 개별적으로 수행
  - 앞선 비동기 처리 결과를 다음 비동기 처리가 사용 X -> 순차적으로 처리할 필요 X

<br>

```js
const requestData1 = () => 
  new Promise(resolve => setTimeout(() => resolve(1), 3000));    // 첫 번째 비동기 처리에서 3초 소요
const requestData2 = () => 
  new Promise(resolve => setTimeout(() => resolve(2), 2000));    // 두 번째 비동기 처리에서 2초 소요
const requestData3 = () => 
  new Promise(resolve => setTimeout(() => resolve(3), 1000));    // 세 번째 비동기 처리에서 1초 소요

// 세 개의 비동기 처리를 병렬로 처리
Promise.all([requestData1(), requestData2(), requestData3()])
  .then(console.log)    // [ 1, 2, 3 ] => 약 3초 소요
  .catch(console.error);
```
위의 코드를 살펴보면,
- Promise.all 메서드를 사용해 세 개의 비동기 처리를 병렬로 처리
- Promise.all 메서드는 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받음 -> 전달받은 모든 프로미스가 모두 fulfilled 상태 -> 모든 처리 결과를 배열에 저장해 새로운 프로미스 반환
- 위의 각 프로미스가 다음과 같이 동작
  - 첫 번째 프로미스는 3초 후에 1을 resolve
  - 두 번째 프로미스는 2초 후에 2를 resolve
  - 세 번째 프로미스는 1초 후에 3을 resolve
- Promise.all 메서드는 인수로 전달받은 배열의 모든 프로미스가 모두 fulfilled 상태가 되면 종료
  - 따라서 Promise.all 메서드가 종료하는 데 걸리는 시간은 가장 늦게 fulfilled 상태가 되는 프로미스의 처리 시간보다 조금 더 김
- 모든 프로미스가 fulfilled 상태가 되면 resolve된 처리 결과를 모두 배열에 저장해 새로운 프로미스 반환
  - 첫 번째 프로미스가 가장 나중에 fulfilled 상태가 되어도 Promise.all 메서드는 첫 번째 프로미스가 resolve한 처리 결과부터 차례대로 배열에 저장해 그 배열을 resolve하는 새로운 프로미스 반환 -> 처리 순서 보장

<br>

- Promise.all 메서드는 인수로 전달받은 배열의 프로미스가 하나라도 rejected 상태가 되면 나머지 프로미스가 fulfilled 상태가 되는 것을 기다리지 않고 즉시 종료
  ```js
  Promise.all([
    new Promise((_, reject) => setTimeout(() => reject(new Error('Error1')), 3000)),
    new Promise((_, reject) => setTimeout(() => reject(new Error('Error2')), 2000)),
    new Promise((_, reject) => setTimeout(() => reject(new Error('Error3')), 1000))
  ])
    .then(console.log) 
    .error(console.log);    // Error: Error3
  ```
  - 세 번째 프로미스가 가장 먼저 rejected 상태가 되므로 세 번째 프로미스가 reject한 에러가 catch 메서드로 전달
- Promise.all 메서드는 인수로 전달받은 이터러블의 요소가 프로미스가 아닌 경우 Promise.resolve 메서드를 통해 프로미스를 래핑
  ```js
  Promise.all([
    1,    // Promise.resolve(1)
    2,    // Promise.resolve(2)
    3,    // Promise.resolve(3)
  ])
    .then(console.log)    // [1, 2, 3]
    .error(console.log); 
  ```

<br>

```js

```
위의 코드를 살펴보면,
- 깃허브 아이디로 깃허브 사용자 이름을 취득하는 3개의 비동기 처리를 모두 병렬로 처리하는 예제
  ```js
  // GET 요청을 위한 비동기 함수
  const promiseGet = url => {
    return new Promise((resolve, reject) => {
      const xhr = new XMLHttpRequest();
      xhr,open('GET', url);
      xhr.send();

      xhr.onload = () => {
        if (xhr.status === 200) {
          // 성공적으로 응답을 전달받으면 resolve 함수 호출
          resolve(JSON.parse(xhr.response));
        } else {
          // 에러 처리를 위해 reject 함수 호출
          reject(new Error(xhr.status));
        }
      };
    });
  };

  const githubIds = ['sdfsds', 'hdfjowasd', 'rfghjo'];

  Promise.all(githubIds.map(id => promiseGet(`https://api.github.con/users/${id}`)))
    // ['sdfsds', 'hdfjowasd', 'rfghjo'] => Promise [userInfo, userInfo, userInfo]
    .then(users => users.map(user => user.name))
    // [userInfo, userInfo, userInfo]
    // -> Promise ['gggg', 'dddd', 'qqqq']
    .then(console.log)
    .catch(console.error);
  ```
  - Promise.all 메서드는 promiseGet 함수가 반환한 3개의 프로미스로 이루어진 배열을 인수로 전달받고 이 프로미스들이 모두 fulfilled한 상태가 되면 처리 결과를 배열에 저장해 새로운 프로미스를 반환
  - Promise.all 메서드가 반환한 프로미스는 세 개의 사용자 객체로 이루어진 배열을 담고 있음 -> 이 배열은 첫 번째 then 메서드에 인수로 전달됨

<br>

### 6.3. Promise.race
- Promise.race 메서드는 Promise.all 메서드와 동일하게 프로미스 요소를 갖는 배열 등의 이터러블을 인수로 전달받음
- Promise.race 메서드는 Promise.all 메서드처럼 모든 프로미스가 fulfilled 상태가 되는 것을 기다리는 것이 아니라 가장 먼저 fulfilled 상태가 된 프로미스 처리 결과를 resolve하는 새로운 프로미스 반환
  ```js
  Promise.race([
    new Promise(resolve => setTimeout(() => resolve(1), 3000)),    // 1
    new Promise(resolve => setTimeout(() => resolve(2), 2000)),    // 2
    new Promise(resolve => setTimeout(() => resolve(3), 1000))    // 3
  ])
    .then(console.log)    // 3
    .error(console.log); 
  ```
- 프로미스가 rejected 상태가 되면 Promise.all 메서드와 동일하게 처리
  - Promise.race 메서드에 전달된 프로미스가 하나라도 rejected 상태가 되면 에러를 reject 하는 새로운 프로미스를 즉시 반환
  ```js
  Promise.race([
    new Promise((_, reject) => setTimeout(() => reject(new Error('Error1')), 3000)),
    new Promise((_, reject) => setTimeout(() => reject(new Error('Error2')), 2000)),
    new Promise((_, reject) => setTimeout(() => reject(new Error('Error3')), 1000))
  ])
    .then(console.log) 
    .error(console.log);    // Error: Error3
  ```
<br>

### 6.4. Promise.allSettled
- 이 메서드는 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받음
  - 이 전달받은 프로미스가 모두 settled 상태(비동기 처리가 수행된 상태, fulfilled 또는 rejected 상태)가 되면 처리 결과를 배열로 반환
- ES11(ECMAScript 2020)에 도입 
  ```js
  Promise.allSettled([
    new Promise(resolve => setTimeout(() => resolve(1), 2000)),
    new Promise((_, reject) => setTimeout(() => reject(new Error('Error')), 1000))
  ])
    .then(console.log);
  /*
  [
    {status: "fulfilled", value: 1},
    {status: "rejected", reason: Error: Error at <anonymous>:3:54}
  ]
  */
  ```
  - Promise.allSettled 메서드가 반환한 배열에는 fulfilled 또는 rejected 상태와 상관없이 Promise.allSettled 메서드가 인수로 전달받은 모든 프로미스들의 처리 결과가 모두 담겨 있음
- 프로미스 처리 결과를 나타내는 객체
  - 프로미스가 fulfilled 상태인 경우: 비동기 처리 상태를 나타내는 status 프로퍼티와 처리 결과를 나타내는 value 프로퍼티를 가짐
  - 프로미스가 rejected 상태인 경우: 비동기 처리 상태를 나타내는 status 프로퍼티와 에러를 나타내는 reason 프로퍼티를 가짐

<br>
<br>

## 7. 마이크로태스크 큐
```js
setTimeout(() => console.log(1), 0);

Promise.resolve()
  .then(() => console.log(2))
  .then(() => console.log(3));
```
위의 코드를 살펴보면,
- 프로미스의 후속 처리 메서드도 비동기로 동작하므로 1 -> 2 -> 3 순으로 출력될 것처럼 보이지만 2 -> 3 -> 1 순으로 출력
  - 이유는 프로미스의 후속 처리 메서드의 콜백 함수는 태스크 큐가 아니라 마이크로태스크 큐에 저장
- 태스크 큐와 마이크로태스크 큐는 별도의 큐
  - 마이크로태스크 큐에는 프로미스의 후속 처리 메서드의 콜백 함수가 일시 저장
  - 태스크 큐에는 그 외의 비동기 함수의 콜백 함수나 이벤트 핸들러에 일시 저장
- **마이크로태스크 큐는 태스크 큐보다 우선순위가 높음**
  - 이벤트 루프는 콜 스택이 비면 먼저 마이크로태스크 큐에서 대기하고 있는 함수를 가져와 실행
  - 마이크로태스크 큐가 비면 태스크 큐에서 대기하고 있는 함수를 가져와 실행

<br>
<br>

## 8. fetch
- fetch 함수는 XMLHttpRequest 객체와 마찬가지로 HTTP 요청 전송 기능을 제공하는 클라이언트 사이드 Web API
- fetch 함수는 사용법이 간단하고 프로미스를 지원하기 때문에 비동기 처리를 위한 콜백 패턴의 단점에서 자유로움
- fetch 함수는 HTTP 요청을 전송할 URL과 HTTP 요청 메서드, HTTP 요청 헤더, 페이로드 등을 설정한 객체 전달
  ```
  const promise = fetch(url [, options])
  ```
- **fetch 함수는 HTTP 응답을 나타내는 Response 객체를 래핑한 Promise 객체를 반환**
- fetch 함수로 GET 요청 전송
  - fetch 함수에 첫 번째 인수로 HTTP 요청을 전송할 URL만 전달하면 GET 요청 전송
  ```js
  fetch('https://jsonplaceholder.typicode.com/todos/1')
    .then(response => console.log(response));
  ```
  - fetch 함수는 HTTP 응답을 나타내는 Response 객체를 래핑한 프로미스를 반환하므로 후속 처리 메서드 then을 통해 프로미스가 resolve한 Response 객체를 전달받음
  - Response 객체는 HTTP 응답을 나타내는 다양한 프로퍼티 제공
- Response.prototype에는 Response 객체에 포함되어 있는 HTTP 응답 몸체를 위한 다양한 메서드를 제공
  - fetch 함수가 반환한 프로미스가 래핑하고 있는 MIME 타입이 application/json인 HTTP 응답 몸체를 취득하려면 Response.prototype.json 메서드 사용
  - Response.prototype.json 메서드는 Response 객체에서 HTTP 응답 몸체를 취득하여 역직렬화
  ```JS
  fetch('https://jsonplaceholder.typicode.com/todos/1')
    .then(response => response.json())
    // json은 역직렬화된  HTTP 응답 몸체
    .then(json => console.log(json));
  ```
<br>

- fetch 함수를 사용할 때는 에러 처리에 주의
  ```js
  const wrongUrl = 'https://jsonplaceholder.typicode.com/XXX/1';

  // 부적절한 URL이 지정되었기 때문에 404 Not Found 에러 발생
  fetch(wrongUrl)
    .then(() => console.log('ok'))
    .catch(() => console.log('error'));
  ```
  - 부적절한 URL이 지정되었기 때문에 404 Not Found 에러가 발생하고 catch 후속 처리 메서드에 의해 'error'가 출력될 것처럼 보이지만 'ok' 출력
  - **fetch 함수가 반환하는 프로미스는 기본적으로 404 Not Found나 500 Internal Server Error와 같은 HTTP 에러가 발생해도 에러를 reject 하지 않고 불리언 타입의 ok 상태를 false로 설정한 Response 객체를 resolve**
  - **오프라인 등의 네트워크 장애나 CORS 에러에 의해 요청이 완료되지 못한 경우에만 프로미스를 reject**
- fetch 함수를 사용할 때는 fetch 함수가 반환한 프로미스가 resolve한 불리언 타입의 ok 상태를 확인해 명시적으로 에러 처리 필요
  ```js
  const wrongUrl = 'https://jsonplaceholder.typicode.com/XXX/1';

  // 부적절한 URL이 지정되었기 때문에 404 Not Found 에러 발생
  fetch(wrongUrl)
    // response는 HTTP 응답을 나타내는 Response 객체
    .then(response => {
      if (!response.ok) throw new Error(response.statusText);
      return response.json();
    })
    .then(todo => console.log(todo))
    .catch(err => console.error(err));
  ```
- axios는 모든 HTTP 에러를 reject하는 프로미스를 반환하기 때문에 모든 에러를 catch에서 처리 가능
  - axios는 인터셉터, 요청 설정 등 fetch보다 다양한 기능 지원
- fetch 함수를 통해 HTTP 요청 전송
  - fetch 함수에 첫 번째 인수로 HTTP 요청을 전송할 URL과 두 번째 인수로 HTTP 요청 메서드, HTTP 요청 헤더, 페이로드 등을 설정한 객체 전달
  ```JS
  const request = {
    get(url) {
      return fetch(url);
    },
    post(url, payload) {
      return fetch(url, {
        method: 'POST',
        headers: { 'content-Type': 'application/json' },
        body: JSON.stringify(payload)
      });
    },
    patch(url, payload) {
      return fetch(url, {
        method: 'PATCH',
        headers: {'content-Type': 'application/json' },
        body: JSON.stringify(payload)
      });
    },
    delete(url) {
      return fetch(url, { method: 'DELETE' });
    }
  };
  ```

<br>

**(1) GET 요청**
```js
request.get('https://jsonplaceholder.typicode.com/todos/1')
  .then(response => {
    if (!reponse.ok) throw new Error(reponse.statusText);
    return response.json();
  })
  .then(todos => console.log(todos))
  .catch(err => console.error(err));
// {userId: 1, id: 1, title: "delectus aut autem", completed: false}
```

<br>

**(2) POST 요청**
```js
request.post('https://jsonplaceholder.typicode.com/todos/1', {
  userId: 1,
  title: 'JavaScript',
  completed: false
}).then(response => {
    if (!reponse.ok) throw new Error(reponse.statusText);
    return response.json();
  })
  .then(todos => console.log(todos))
  .catch(err => console.error(err));
// {userId: 1, title: "JavaScript", completed: false, id: 201}
```

<br>

**(3) PATCH 요청**
```js
request.patch('https://jsonplaceholder.typicode.com/todos/1', {
  completed: true
}).then(response => {
    if (!reponse.ok) throw new Error(reponse.statusText);
    return response.json();
  })
  .then(todos => console.log(todos))
  .catch(err => console.error(err));
// {userId: 1, id: 1, title: "delectus aut autem", completed: true}
```

<br>

**(4) DELETE 요청**
```js
request.delete('https://jsonplaceholder.typicode.com/todos/1')
  .then(response => {
    if (!reponse.ok) throw new Error(reponse.statusText);
    return response.json();
  })
  .then(todos => console.log(todos))
  .catch(err => console.error(err));
// {}
```

<br>
<br>