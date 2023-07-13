# 클로저
클로저는 함수를 일급 객체로 취급하는 함수형 프로그래밍에서 사용되는 중요한 특성이다. MDN에서 클로저에 대해 다음과 같이 정의하고 있다.

> 클로저는 함수와 그 함수가 선언된 렉시컬 환경과의 조합이다.
- "그 함수가 선언된 렉시컬 환경" : 함수가 정의된 위치의 스코프, 즉 상위 스코프를 의미하는 실행 컨텍스트의 렉시컬 환경

<br>

```js
const x = 1;

function outerFunc() {
  const x = 10;

  function innerFunc() {
    console.log(x);    // 10
  }

  innerFunc();
}

outerFunc();
```
```js
const x = 1;

function outerFunc() {
  const x = 10;
  innerFunc();
}

function innerFunc() {
    console.log(x);    // 1
  }

outerFunc();
```
위의 코드를 살펴보면,
- innerFunc의 상위 스코프는 외부 함수 outerFunc의 스코프
  - 중첩 함수 innerFunc 내부에서 자신을 포함하고 있는 외부 함수 outerFunc의 변수 x에 접근 가능
- 만약 innerFunc 함수가 outerFunc 함수의 내부에서 정의된 중첩 함수가 아니라면 innerFunc 함수를 outerFunc 함수의 내부에서 호출한다 하더라도 접근 불가

이 같은 현상이 발생하는 이유는 **자바스크립트가 렉시컬 스코프를 따르는 프로그래밍 언어**이기 때문이다.

<br>
<br>

## 1. 렉시컬 스코프
- **자바스크립트 엔진은 함수를 어디서 호출했는지가 아니라 함수를 어디에 정의했는지에 따라 상위 스코프를 결정**
- **렉시컬 환경의 "외부 렉시컬 환경에 대한 참조"에 저장할 참조값, 즉 상위 스코프에 대한 참조는 함수 정의가 평가되는 시점에 정의된 환경(위치)에 의해 결정**

<br>

```js
const x = 1;

function foo() {
  const x = 10;
  bar();
}

function bar() {
  console.log(x);
}

foo();    // 1
bar();    // 1
```
위의 코드를 살펴보면,
- foo 함수와 bar 함수는 모두 전역에서 정의된 전역 함수
  - foo 함수와 bar 함수의 상위 스코프는 전역 
- 함수를 어디서 호출하는지는 함수의 상위 스코프 결정에 영향 X
  -  함수의 상위 스코프는 함수를 정의한 위치에 의해 정적으로 결정되고 변하지 X
- "함수의 상위 스코프를 결정한다" = "렉시컬 환경의 외부 렉시컬 환경에 대한 참조에 저장할 참조값을 결정한다"
  - "외부 렉시컬 환경에 대한 참조에 저장할 참조값" = 상위 렉시컬 환경에 대한 참조 = 상위 스코프

<br>
<br>

## 2. 함수 객체의 내부 슬롯 [[Environment]]
- 렉시컬 스코프가 가능하려면 함수는 자신이 호출되는 환경과는 상관없이 자신이 정의된 환경, 즉 상위 스코프를 기억해야 함. 이를 위해 **함수는 내부 슬롯 [[Environment]]에 자신이 정의된 환경, 즉 상위 스코프의 참조를 저장**
  - 함수 정의가 평가되어 함수 객체를 생성할 때 자신이 정의된 환경(위치)에 의해 결정된 상위 스코프의 참조를 함수 객체 자신의 내부 스롯 [[Environment]]에 저장
  - 내부 슬롯 [[Environment]]에 저장된 상위 스코프의 참조는 현재 실행 중인 실행 컨텍스트의 렉시컬 환경을 가리킴
    - 함수 정의가 평가되어 함수 객체를 생성하는 시점은 함수가 정의된 환경, 즉 상위 함수가 평가 또는 실행되고 있는 시점이며, 이때 현재 실행 중인 실행 컨텍스트는 상위 함수의 실행 컨텍스트
- **함수 객체 내부 슬롯 [[Environment]]에 저장된 실행 중인 컨텍스트의 렉시컬 환경의 참조가 상위 스코프**
- **자신이 호출되었을 때 생성될 함수 렉시컬 환경의 "외부 렉시컬 환경에 대한 참조"에 저장될 참조값**
- **함수 객체는 내부 슬롯[[Environment]]에 저장한 렉시컬 환경의 참조, 즉 상위 스코프를 자신이 존재하는 한 기억**

<br>

```js
const x = 1;

function foo() {
  const x = 10;
  bar();
}

function bar() {
  console.log(x);
}

foo();    // 1
bar();    // 1
```
위의 코드를 살펴보면,
- foo 함수와 bar 함수는 모두 전역에서 함수 선언문으로 정의
  - foo 함수와 bar 함수는 모두 전역 코드가 평가되는 시점에 평가되어 함수 객체를 생성하고 전역 객체 window의 메서드
- 생성된 함수 객체의 내부 슬롯 [[Environment]]에는 함수 정의가 평가된 시점, 즉 전역 코드 평가 시점에 실행 중인 실행 컨텍스트의 렉시컬 환경인 전역 렉시컬 환경의 참조가 저장
- 함수가 호출되면 함수 내부로 코드의 제어권 이동 
- 함수 코드 평가
  1. 함수 실행 컨텍스트 생성
  2. 함수 렉시컬 환경 생성 <br>
    2.1. 함수 환경 레코드 생성 <br>
    2.2. this 바인딩 <br>
    2.3. 외부 렉시컬 환경에 대한 참조 결정
- 이때 함수 렉시컬 환경의 구성 요소인 **외부 렉시컬 환경에 대한 참조에는 함수 객체의 내부 슬롯 [[Environment]]에 저장된 렉시컬 환경의 참조가 할당**
- 함수 객체의 내부 슬롯 [[Envrionment]]에 저장된 렉시컬 환경의 참조는 함수의 상위 스코프를 의미

<br>
<br>

## 3. 클로저와 렉시컬 환경
```js
const x = 1;

// ①
function outer() {
  const x = 10;
  const inner = function () { console.log(x); };    // ②
  return inner;
}

const innerFunc = outer();    // ③
innerFunc();    // ④ 10
```
위의 코드를 살펴보면,
- outer 함수를 호출(③)하면 outer 함수는 중첩 함수 inner를 반환하고 생명 주기를 마감
  - 즉, outer 함수의 실행이 종료되면 outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 제거
- 이때 outer 함수의 지역 변수 x와 변수 값 10을 저장하고 있던 outer 함수의 실행 컨텍스트가 제거되었으므로 outer 함수의 지역 변수 x 또한 생명 주기 마감
- x의 생명 주기가 마감되어 유효하지 않으므로 x 변수에 접근할 수 없지만 ④의 경우 outer 함수의 지역 변수 x의 값인 10
- 이처럼 **외부 함수보다 중첩 함수가 더 오래 유지되는 경우 중첩 함수는 이미 생명 주기가 종료한 외부 함수의 변수 참조 가능**. 이러한 **중첩 함수를 클로저라고 부름**

<br>

- outer 함수가 평가되어 함수 객체를 생성할 때(①) 현재 실행 중인 실행 컨텍스트의 렉시컬 환경, 즉 전역 렉시컬 환경을 outer 함수 객체의 [[Environment]] 내부 슬롯에 상위 스코프로서 저장
- outer 함수 호출하면 outer 함수의 렉시컬 환경이 생성 -> outer 함수 객체의 [[Environment]] 내부 슬롯에 저장된 전역 렉시컬 환경을 outer 함수 렉시컬 환경의 "외부 렉시컬 환경에 대한 참조"에 할당
- 중첩 함수 inner가 런타임에 평가 -> 중첩 함수 inner는 자신의 [[Environment]] 내부 슬롯에 현재 실행 중인 실행 컨텍스트의 렉시컬 환경, 즉 outer함수의 렉시컬 환경을 상위 스코프로서 저장
- outer 함수 종료 -> inner 함수 반환하면서 outer 함수의 생명 주기 종료(③). 즉, outer 함수의 실행 컨텍스트가 실행 컨텍스트 스택에서 제거. **이때 outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 제거되지만 outer 함수의 렉시컬 환경까지 소멸하는 것은 아님**
- outer 함수의 렉시컬 환경은 inner 함수의 [[Enviroment]] 내부 슬롯에 의해 참조, inner 함수는 전역 변수 innerFunc에 의해 참조 -> 렉시컬 환경이 가비지 컬렉션의 대상 X
- outer 함수가 반환한 inner 함수를 호출(④)하면 inner 함수의 실행 컨텍스트 생성 -> 실행 컨텍스트 스택에 푸시 -> 렉시컬 환경의 외부 렉시컬 환경에 대한 참조에는 inner 함수 객체의 [[Enviroment]] 내부 슬롯에 저장되어 있는 참조값이 할당
- 중첩 함수 inner는 외부 함수 outer보다 오래 생존 
  - 외부 함수보다 더 오래 생존한 중첩 함수는 외부 함수의 생존 여부(실행 컨텍스트의 생존 여부)와 상관없이 자신이 정의된 위치에 의해 결정된 상위 스코프를 기억
- 중첩 함수 inner의 내부에서는 상위 스코프를 참조할 수 있으므로 상위 스코프의 식별자 참조, 식별자의 값 변경 가능

<br>

자바스크립트의 모든 함수는 상위 스코프를 기억하므로 이론적으로 모든 함수는 클로저이지만 일반적으로 모든 함수를 클로저라고 하지는 않는다.
```html
<!DOCTYPE html>
<html>
<body>
  <script>
    function foo() {
      const x = 1;
      const y = 2;

      // 일반적으로 클로저라고 하지 않는다.
      function bar() {
        const z = 3;

        debugger;

        // 상위 스코프의 식별자를 참조하지 않는다.
        console.log(z);
      }

      return bar;
    }

    const bar = foo();
    bar();
  </script>
</body>
</html>
```
위 예제를 디버깅 모드를 실행하면,
> local <br>
  this: window <br>
  z: 3 <br>
  Script <br>
  bar: f bar() <br>
- 중첩 함수 bar는 외부 함수 foo보다 오래 유지되지만 상위 스코프의 어떤 식별자도 참조하지 않음
- 상위 스코프의 어떤 식별자도 참조하지 않는 경우 대부분의 모던 브라우저는 최적화를 통해 상위 스코프를 기억하지 않음
- 참조하지도 않는 식별자를 기억하는 것은 메모리 낭비이기 때문에 bar 함수는 클로저가 아님

<br>

```html
<!DOCTYPE html>
<html>
<body>
  <script>
    function foo() {
      const x = 1;

      // bar 함수는 클로저였지만 곧바로 소멸한다.
      // 이러한 함수는 일반적으로 클로저라고 하지 않는다.
      function bar() {
        debugger;

        // 상위 스코프의 식별자를 참조한다.
        console.log(x);
      }
      bar();
    }
    foo();
  </script>
</body>
</html>
```
위 예제를 디버깅 모드로 실행하면,
> Closure (foo) <br>
  x: 1
- 중첩 함수 bar는 상위 스코프의 식별자를 참조하고 있으므로 클로저
- 하지만 외부 함수 foo의 외부로 중첩 함수 bar가 반환 X. 즉, 외부 함수 foo보다 중첩 함수 bar의 생명 주기가 짧음
- 이런 경우 중첩 함수 bar는 클로저였지만 외부 함수보다 일찍 소멸되기 때문에 생명 주기가 종료된 외부 함수의 식별자를 참조할 수 있다는 클로저의 본질에 부합 X -> 중첩 함수 bar는 일반적으로 클로저라고 하지 않음

<br>

```html
<!DOCTYPE html>
<html>
<body>
  <script>
    function foo() {
      const x = 1;
      const y = 2;

      // 클로저
      // 중첩 함수 bar는 외부 함수보다 더 오래 유지되며 상위 스코프의 식별자를 참조한다.
      function bar() {
        debugger;

        console.log(x);
      }

      return bar;
    }

    const bar = foo();
    bar();
  </script>
</body>
</html>
```
위 예제를 디버깅 모드로 실행하면,
> Closure (foo) <br>
  x: 1
- 중첩 함수 bar는 상위 스코프의 식별자를 참조하고 있으므로 클로저이고 외부 하무의 외부로 반환되어 외부 함수보다 더 오래 살아남음

<br>

- 클로저 : 외부 함수보다 중첩 함수가 더 오래 유지되는 경우 이미 생명 주기가 종료한 외부 함수의 변수 참조 가능한 중첩 함수
- **클로저는 중첩 함수가 상위 스코프의 식별자를 참조하고 있고 중첩 함수가 외부 함수보다 더 오래 유지되는 경우에 한정하는 것이 일반적**
  - 클로저인 중첩 함수가 상위 스코프의 식별자 중 하나만 참조한다고 가정했을 때, 대부분의 모던 브라우저는 최적화를 통해 상위 스코프의 식별자 중에서 클로저가 참조하고 있는 식별자만 기억
  - 상위 스코프의 식별자 중에서 기억해야 할 식별자만 기억하므로 메모리를 낭비하지 않음
- **자유 변수 (free variable)** : 클로저에 의해 참조되는 상위 스코프의 변수

<br>
<br>

## 4. 클로저의 활용
- **클로저는 상태(state)를 안전하게 변경하고 유지하기 위해 사용**
- **상태를 안전하게 은닉**하고 **특정 함수에게만 상태 변경을 허용**하기 위해 사용

<br>

```js
// 카운트 상태 변수
let num = 0;

// 카운트 상태 변경 함수
const increase = function () {
  // 카운트 상태를 1만큼 증가
  return ++num;
};

console.log(increase());    // 1
console.log(increase());    // 2
console.log(increase());    // 3
```
위의 코드를 살펴보면,
- 코드는 잘 동작하지만 오류를 발생시킬 가능성 내포하고 있는 좋지 않은 코드
  - 이유는 위 예제가 바르게 동작하려면 다음 전제 조건이 필요
    1. 카운트 상태(num 변수 값)는 increase 함수가 호출되기 전까지 변경되지 않고 유지되어야 함
    2. 이를 위해 카운트 상태(num 변수 값)는 increase 함수만이 변경할 수 있어야 함
- 하지만 카운트 상태는 전역 변수를 통해 관리되고 있기 때문에 언제든지 누구나 접근 및 변경 가능(암묵적 결합) -> 의도치 않게 상태 변경 가능성 -> 오류 가능성

<br>

```js
// 카운트 상태 변경 함수
const increase = (function () {
  // 카운트 상태 변수
  let num = 0;

  // 클로저
  return function () {
    // 카운트 상태를 1만큼 증가
    return ++num;
  };
}());

console.log(increase());    // 1
console.log(increase());    // 2
console.log(increase());    // 3
```
위의 코드를 살펴보면,
- 코드가 실행되면 즉시 실행 함수 호출 -> 즉시 실행 함수가 반환한 함수가 increase에 할당 -> increase 변수에 할당된 함수는 자신이 정의된 위치에 의해 결정된 상위 스코프인 즉시 실행 함수의 렉시컬 환경을 기억하는 클로저
- 즉시 실행 함수는 호출된 이후 소멸 -> 즉시 실행 함수가 반환된 클로저는 increase 변수에 할당되어 호출
  - 이때 즉시 실행 함수가 반환한 클로저는 자신이 정의된 위치에 의해 결정된 상위 스코프인 즉시 실행 함수의 렉시컬 환경을 기억
  - 따라서 즉시 실행 함수가 반환한 클로저는 카운트 상태를 유지하기 위한 자유 변수 num을 언제 어디서 호출하든지 참조하고 변경 가능
- 즉시 실행 함수는 한 번만 실행되므로 increase가 호출될 때마다 num 변수가 재차 초기화 X
- num 변수는 외부에서 직접 접근할 수 없는 은닉된 private 변수이므로 전역 변수를 사용했을 때와 같이 의도치 않은 변경을 걱정할 필요 X

<br>

```js
const counter = (function () {
  // 카운트 상태 변수
  let num = 0;

  // 클로저인 메서드를 갖는 객체 반환
  // 객체 리터럴은 스코프 생성 X
  // 따라서 아래 메서드들의 상위 스코프는 즉시 실행 함수의 렉시컬 환경
  return {
    // num: 0,  // 프로퍼티는 public하므로 은닉 X
    increase() {
      return ++num;
    },
    decrease() {
      return num > 0 ? --num : 0;
    }
  };
}());

console.log(increase());    // 1
console.log(increase());    // 2

console.log(decrease());    // 1
console.log(decrease());    // 0
```
위의 코드를 살펴보면,
- 즉시 실행 함수가 반환하는 객체 리터럴은 즉시 실행 함수의 실행 단계에서 평가되어 객체가 됨
  - 객체의 메서드도 함수 객체로 생성
  - 객체 리터럴의 중괄호는 코드 블록이 아니므로 별도의 스코프 생성 X
- increase, decrease 메서드의 상위 스코프는 increase, decrease 메서드가 평가되는 시점에 실행 중인 실행 컨텍스트인 즉시 실행 함수 실행 컨텍스트의 렉시컬 환경
- 따라서 increase, decrease 메서드가 언제 어디서 호출되든 상관없이 increase, decrease 함수는 즉시 실행 함수의 스코프의 식별자 참조 가능

<br>

```js
// 함수를 인수로 전달받고 함수를 반환하는 고차 함수
// 이 함수는 카운트 상태를 유지하기 위한 자유 변수 counter를 기억하는 클로저를 반환
function makeCounter(aux) {
  // 카운트 상태를 유지하기 위한 자유 변수
  let counter = 0;

  // 클로저 반환
  return function () {
    // 인수로 전달받은 보조 함수에 상태 변경 위임
    counter = aux(counter);
    return counter;
  };
}

// 보조 함수
function increase(n) {
  return ++n;
}

// 보조 함수
function decrease(n) {
  return --n;
}

// 함수로 함수 생성
// makeCounter 함수는 보조 함수를 인수로 전달받아 함수 반환
const increaser = makeCounter(increase);    // ①
console.log(increaser());    // 1
console.log(increaser());    // 2

// increaser 함수와는 별개의 독립된 렉시컬 환경을 갖기 때문에 카운트 상태 연동 X
const decreaser = makeCounter(decrease);    // ②
console.log(decreaser());    // -1
console.log(decreaser());    // -2
```
위의 코드를 살펴보면,
- makeCounter 함수가 반환하는 함수는 자신이 생성됐을 때의 렉시컬 환경인 makeCouter 함수의 스코프에 속한 couter 변수를 기억하는 클로저
- **makeCouter 함수를 호출해 함수를 반환할 대 반환된 함수는 자신만의 독립된 렉시컬 환경을 갖음**
- ①에서 makeCouter 함수 호출 -> makeCounter 함수의 실행 컨텍스트 생성 -> makeCounter 함수는 함수 객체를 생성하여 반환 후 소멸
  - makeCouter 함수가 반환한 함수는 makeCounter 함수의 렉시컬 환경을 상위 스코프로서 기억하는 클로저이며, 전역 변수인 increaser에 할당
  - 이때 makeCouter 함수의 실행 컨텍스트 소멸, makeCounter 함수 실행 컨텍스트의 렉시컬 환경은 makeCounter 함수가 반환한 함수의 [[Environment]] 내부 슬롯에 의해 참조되고 있기 때문에 소멸 X

<br>

```js
const counter = (function () {
  let counter = 0;

  return function (aux) {
    counter = aux(counter);
    return counter;
  };
}());

function increase(n) {
  return ++n;
}

function decrease(n) {
  return --n;
}

// 보조 함수를 전달하여 호출
console.log(counter(increase));    // 1
console.log(counter(increase));    // 2

// 자유 변수 공유
console.log(counter(decrease));    // 1
console.log(counter(decrease));    // 0
```
위의 코드를 살펴보면,
- 앞의 예제에서 전역 변수 increaser와 decreaser에 할당된 함수는 각자 자신만의 독립된 렉시컬 환경을 갖기 때문에 자유 변수 counter가 연동 X
- 이를 위해 makeCounter 함수를 두 번 호출하지 않고 보조 함수를 전달하여 한번만 호출

<br>
<br>

## 5. 캡슐화와 정보 은닉
- 캡슐화(encapsulation)
  - 객체의 상태를 나타내는 프로퍼티와 프로퍼티를 참조하고 조작할 수 있는 동작인 메서드를 하나로 묶는 것
  - 객체의 특정 프로퍼티나 메서드를 감출 목적으로 사용 -> 정보 은닉
- 정보 은닉
  - 외부에 공개할 필요가 없는 구현의 일부를 외부에 공개하지 않도록 감추어 적절치 못한 접근으로부터 객체의 상태가 변경되는 것을 방지해 정보 보호
  - 객체 간의 상호 의존성, 즉 결합도를 낮춤
- 자바스크립트는 public, private, protected 같은 접근 제한자 제공 X -> 객체의 모든 프로퍼티와 메서드는 기본적으로 public

<br>

```js
function Person(name, age) {
  this.name = name;    // public
  let _age = age;    // private

  // 인스턴스 메서드
  this.sayHi = function () {
    console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
  };
}

const me = new Person('Lee', 20);
me.sayHi();    // Hi! My name is Lee. I am 20.
console.log(me.name);    // Lee
console.log(me._age);    // undefined
```
위의 코드를 살펴보면,
- name 프로퍼티는 외부로 공개되어 있어 자유롭게 참조하거나 변경 가능 -> name 프로퍼티는 public
- _age 변수는 Person 생성자 함수의 지역 변수이므로 Person 생성자 함수 외부에서 참조하거나 변경 불가능 -> _age 변수는 private
- sayHi 메서드는 인스턴스 메서드이므로 Person 객체가 생성될 때마다 중복 생성

<br>

```js
function Person(name, age) {
  this.name = name;    // public
  let _age = age;    // private
}

// 프로토타입 메서드
Person.prototype.sayHi = function () {
  console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
};
```
위의 코드를 살펴보면,
- sayHi 메서드를 프로토타입 메서드로 변경하여 중복 생성 방지
- 하지만 Person 생성자 함수의 지역 변수 _age 참조 불가능

<br>

```js
const Person = (function () {
  let _age = 0;    // private

  // 생성자 함수
  function Person(name, age) {
    this.name = name;    // public
    _age = age;
  }

  // 프로토타입 메서드
  Person.prototype.sayHi = function () {
    console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
  };

  // 생성자 함수 반환
  return Person;
}());

const me = new Person('Lee', 20);
me.sayHi();    // Hi! My name is Lee. I am 20.
console.log(me.name);    // Lee
console.log(me._age);    // undefined

const you = new Person('Kim', 30);
you.sayHi();    // Hi! My name is Kim. I am 30.

me.sayHi();    // Hi! My name is Lee. I am 30.
```
위의 코드를 살펴보면,
- 즉시 실행 함수가 반환하는 Person 생성자 함수와 Person 생성자 함수의 인스턴스가 상속받아 호출할 Person.prototype.sayHi 메서드는 즉시 실행 함수가 종료된 이후 호출
- Person 생성자 함수와 sayHi 메서드는 이미 종료되어 소멸한 즉시 실행 함수의 지역 변수 _age를 참조할 수 있는 클로저
- Person 생성자 함수가 여러 개의 인스턴스를 생성할 경우 _age 변수의 상태가 유지되지 않는 문제점 발생  
  - Person.prototype.sayHi 메서드가 단 한 번 생성되는 클로저이기 때문
  - Person.prototype.sayHi 메서드는 즉시 실행 함수가 호출될 때 생성 -> 자신의 상위 스코프인 즉시 실행 함수의 실행 컨텍스트의 렉시컬 환경의 참조를 [[Environment]]에 저장하여 기억 -> Person 생성자 함수의 모든 인스턴스가 상속을 통해 호출할 수 있는 Person.prototype.sayHi 메서드의 상위 스코프는 어떤 인스턴스로 호출하더라도 하나의 동일한 상위 스코프를 사용

<br>

- 자바스크립트는 정보 은닉을 완전하게 지원 X
- 인스턴스 메서드를 사용한다면 자유 변수를 통해 private을 흉내 낼 수 있지만 프로토타입 메서드를 사용하면 이마저도 불가능
- TC39 프로세스의 stage 3(candidate)에는 클래스에 private 필드를 정의할 수 있는 새로운 표준 사양 제안

<br>
<br>

## 6. 자주 발생하는 실수
```js
var funcs = [];

for (var i = 0; i < 3; i++) {
  funcs[i] = function () { return i; };    // ①
}

for (var j = 0; j < funcs.length; j++) {
  console.log(funcs[j]());    // ②
}
```
위의 코드를 살펴보면,
- 첫 번째 for문의 코드 블록 내(①)에서 함수가 funcs 배열의 요소로 추가
- 두 번째 for문의 코드 블록 내(②)에서 funcs 배열의 요소로 추가된 함수를 순차적으로 호출
- 0, 1, 2 반환 X -> 3 출력
  - for 문의 변수 선언문에서 var 키워드로 선언한 i 변수는 함수 레벨 스코프를 갖기 때문에 전역 변수 
  - 전역 변수 i에는 0, 1, 2가 순차적으로 할당
  - funcs 배열의 요소로 추가한 함수를 호출하면 전역 변수 i를 참조하여 i의 값 3이 출력

<br>

```js
var funcs = [];

for (var i = 0; i < 3; i++) {
  funcs[i] = (function (id) {    // ①
    return function () {
      return id;
    };
  }(i));
}

for (var j = 0; j < funcs.length; j++) {
console.log(funcs[j]());    // ②
}
```
위의 코드를 살펴보면,
- ①에서 즉시 실행 함수는 전역 변수 i에 현재 할당되어 있는 값을 인수로 전달받아 매개변수 id에 할당한 후 중첩 함수를 반환 후 종료
  - 즉시 실행 함수가 반환한 함수는 funcs 배열에 순차적으로 저장
  - 이때 즉시 실행 함수의 매개변수 id는 즉시 실행 함수가 반환한 중첩 함수의 상위 스코프에 존재
  - 즉시 실행 함수가 반환한 중첩 함수는 자신의 상위 스코프(즉시 실행 함수의 렉시컬 환경)를 기억하는 클로저이고, 매개변수 id는 즉시 실행 함수가 반환한 중첩 함수에 묶여 있는 자유 변수가 되어 그 값이 유지
- 0 1 2 반환

<br>

```js
const funcs = [];

for (let i = 0; i < 3; i++) {
  funcs[i] = function () { return i; };   
}

for (let j = 0; j < funcs.length; j++) {
  console.log(funcs[j]());    // 0 1 2
}
```
위의 코드를 살펴보면,
- 첫 번째 예제는 자바스크립트의 함수 레벨 스코프 특성으로 인해 for문의 변수 선언문에서 var 키워드로 선언한 변수가 전역 변수가 되기 때문에 발생하는 현상 -> let 키워드 사용하여 문제 해결
- for문의 변수 선언문에서 let 키워드로 선언한 변수를 사용하면 for문의 코드 블록이 반복 실행될 때마다 for문 코드 블록의 새로운 렉시컬 환경 생성
- for문의 코드 블록 내에서 정의한 함수가 있다면 이 함수의 상위 스코프는 for문의 코드 블록이 반복 실행될 때마다 생성된 for문 코드 블록의 새로운 렉시컬 환경
- 함수의 상위 스코프는 for문의 코드 블록이 반복 실행될 때마다 식별자의 값을 유지해야 함 -> 이를 위해 for문이 반복될 때마다 독립적인 렉시컬 환경을 생성하여 식별자 값 유지

<br>

1. for문 초기화 문의 식별자를 등록
    - for문의 변수 선언문에서 let 키워드로 선언한 초기화 변수를 사용한 for문이 평가 -> 새로운 렉시컬 환경 생성, 초기화 변수 식별자와 값 등록 -> 새롭게 생성된 렉시컬 환경을 현재 실행 중인 실행 컨텍스트의 렉시컬 환경으로 교체
2. 1번째 반복에 의해 생성된 렉시컬 환경
3. 2번째 반복에 의해 생성된 렉시컬 환경
4. 3번째 반복에 의해 생성된 렉시컬 환경
    - for문의 코드 블록이 반복 실행되기 시작하면 새로운 렉시컬 환경 생성 -> for문 코드 블록 내의 식별자와 값(증감문 반영 이전) 등록 -> 새롭게 생성된 렉시컬 환경을 현재 실행 중인 컨텍스트의 렉시컬 환경으로 교체
5. for문 실행 이전으로 복귀
    - for문의 코드 블록의 반복 실행이 모두 종료 -> for문이 실행되기 이전의 렉시컬 환경을 실행 중인 실행 컨텍스트의 렉시컬 환경으로 되돌림

<br>
<br>