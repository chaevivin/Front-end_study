# 제어문

제어문은 조건에 따라 코드 블록을 실행(조건문)하거나 반복 실행(반복문)할 때 사용한다. 

제어문은 코드의 흐름을 이해하기 어렵게 만들어 가독성을 해치는 단점이 있는데, 가독성이 좋지 않은 코드느 오류를 발생시키는 원인이 된다. forEach, map, filter, reduce 같은 고차 함수를 사용한 함수형 프로그래밍 기법에서 제어문의 사용을 억제하여 아러한 복잡성을 해결한다.

<br>
<br>

### 1. 블록문
블록문은 0개 이상의 문을 <b>중괄호</b>로 묶은 것으로, 코드 블록 또는 블록이라고 부른다. <b>자바스크립트는 블록문을 하나의 실행 단위로 취급한다.</b>

```js
// 블록문
{
  var foo = 10;
}
```

<br>
<br>

### 2. 조건문
조건문은 주어진 조건식의 평가 결과에 따라 코드 블록의 실행을 결정한다. <b>조건식은 불리언 값으로 평가될 수 있는 표현식이다. </b>

<br>

#### 2.1. if ... else문
if ... else문은 주어진 조건식의 평가 결과, 즉 논리적 참 또는 거짓에 따라 실행할 코드 블록을 결정한다.

```js
if (조건식1) {
  // 조건식1이 참이면 이 코드 블록 실행
} else if (조건식2) {
  // 조건식2가 참이면 이 코드 블록 실행
} else {
  // 조건식1, 조건식2 모두 거짓이면 이 코드 블록 실행
}
```

<br>

```js
// if ... else문
var x = 2;
var result;

if (x % 2) {    2 % 2 = 0, 0은 false로 암묵적 강제 변환된다.
  result = '홀수';
} else {
  result = '짝수';
}

console.log(result);    // 짝수
```
```js
// 삼항 조건 연산자
var x = 2;

var result = x % 2 ? '홀수' : '짝수';
console.log(result);    // 짝수
```
위의 코드를 살펴보면, if ... else문은 삼항 조건 연산자로 바꿀 수 있다.

<br>

#### 2.2. switch문
switch문은 주어진 표현식을 평가하여 그 값과 일치하는 포현식을 갖는 case문으로 실행 흐름을 옮긴다. switch문의 표현식과 일치하는 case문이 없다면 실행 순서는 default문으로 이동한다. (default문은 선택사항)

```js
switch (표현식) {
  case 표현식1:
    switch문의 표현식과 표현식1이 일치하면 실행될 문;
    break;
  case 표현식2:
    switch문의 표현식과 표현식2가 일치하면 실행될 문;
    break;
  default:
      switch문의 표현식과 일치하는 case문이 없을 때 실행될 문;
}
```

<br>

- if ... else문은 논리적 참, 거짓으로 실행할 코드 블록을 결정한다.
- switch문은 논리적 참, 거짓보다는 다양한 상황(case)에 따라 실행할 코드 블록을 결정할 때 사용한다.

<br>

<b>✨ case문 안에 break를 써야 하는 이유</b>
- case문 안에 break를 쓰지 않으면 switch문을 탈출하지 않고 switch문이 끝날 때까지 이후의 모든 case문과 default문을 실행하기 때문에 결과적으로 default문의 결과가 출력된다. 이를 <b>풀스루</b>라고 한다.
- 풀스루를 활용해 코드를 짤 수도 있다. (ex : 윤년 판별)

<br>
<br>

### 3. 반복문
반복문은 조건식의 평가 결과가 참인 경우 코드 블록을 실행한다. 그 후 조건식을 다시 평가하여 여전히 참인 경우 코드 블록을 다시 실행한다. 이는 조건식이 거짓일 때까지 반복된다.

<br>

#### 3.1. for문
for문은 조건식이 거짓으로 평가될 때까지 코드 블록을 반복 실행한다.

```js
for (변수 선언문 또는 할당문; 조건식; 증감식) {
  조건식이 참인 경우 실행될 문;
}
```

<br>

```js
for(;;) { ... }
```
위의 코드를 살펴보면, 변수 선언문, 조건식, 증감식은 모두 옵션이므로 모두 사용하지 않으면 무한루프가 된다.

<br>

#### 3.2. while문
while문은 주어진 조건식의 평가 결과가 참이면 코드 블록을 계속해서 반복 실행한다. for문은 반복 횟수가 명확할 때 주로 사용하고 while문은 반복 횟수가 불명확할 때 주로 사용한다.

```js
var count = 0;

// count가 3보다 작을 때까지 코드 블록 반복 실행
while (count < 3) {
  console.log(count);    // 0 1 2
  count ++;
}
```

<br>

```js
var count = 0;

while(true) { 
  console.log(count);    // 0 1 2
  count ++;

  if (count === 3) break;
}
```
위의 코드를 살펴보면,
1. 조건식의 평가 결과가 언제나 참이면 무한루프가 된다.
2. 무한루프에서 탈출하기 위해서는 코드 블록 내에 if 문으로 탈출 조건을 만들고 break문으로 코드 블록을 탈출한다.

<br>

#### 3.3. do ... while문
do ... while문은 코드 블록을 먼저 실행하고 조건식을 평가한다. 따라서 코드 블록은 무조건 한 번 이상 실행된다.

```js
var count = 0;

// count가 3보다 작을 때까지 코드 블록을 반복 실행한다.
do {
  console.log(count);    // 0 1 2
  count++;
}while (count < 3);
```

<br>
<br>

### 4. break문
break문은 반복문을 더 이상 진행하지 않아도 될 때 불필요한 반복을 회피할 수 있어 유용하다.

break문은 코드 블록을 탈출하는 것이 아니라 레이블문(식별자가 붙은 문), 반복문(for, for ... in, for ... of, while, do ... while) 또는 switch문의 코드 블록을 탈출한다. 

레이블문, 반복문, switch문의 코드 블록 외에 break를 사용하면 SyntaxError(문법 에러)가 발생한다.

<br>

```js
// outer라는 식별자가 붙은 레이블 for문
outer: for (var i = 0; i < 3; i++){
  for (var j = 0; j < 3; j++) {
    if (i + j === 3) break outer;
  }
}
```
위의 코드를 살펴보면, 
1. 내부 for문이 아닌 외부 for문을 탈출하려면 레이블 문을 사용한다.
2. 이 외의 경우에는 코드이 가독성이 나빠지기 때문에 레이블문 사용을 권장하지 않는다.

<br>
<br>

### 5. continue문
continue문은 반복문의 코드 블록 실행을 현 지점에서 중단하고 반복문의 증감식으로 실행 흐름을 이동시킨다. break문처럼 반복문을 탈출하지 않는다.

<br>

```js
var string = 'Hello World';
var search = 'l';
var count = 0;

for (var i = 0; i < string.length; i++) {
  if (string[i] !== search) continue;
  count ++;
}

console.log(count);
```
위의 코드를 살펴보면,
1. 문자열 'Hello World'에서 문자 'l'을 찾는 코드
2. if문에서 문자가 'l'이 아니면 실행을 중단하고 반복문의 증감식으로 이동한다. continue 밑 코드는 실행되지 않는다.

<br>

```js
const regexp = new RegExp(search, 'g');
console.log(string.match(regexp).length);    // 3
```
참고로 String.prototype.match 메서드를 사용해도 같은 동작을 한다.

<br>
<br>