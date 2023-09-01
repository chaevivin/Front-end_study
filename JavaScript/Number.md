# Number
- 표준 빌트인 객체인 Number는 원시 타입인 숫자를 다룰 때 유용한 프로퍼티와 메서드 제공

<br>
<br>

## 1. Number 생성자 함수
- 표준 빌트인 객체인 Number 객체는 생성자 함수 객체 -> new 연산자와 함께 호출하여 Number 인스턴스 생성 가능
- Number 생성자 함수에 인수를 전달하지 않고 new 연산자와 함께 호출하면 [[NumberData]] 내부 슬롯에서 0을 할당한 Number 래퍼 객체 생성
  ```js
  const numObj = new Number();
  console.log(numObj);    // Number {[[PrimitiveValue]]: 0}
  ```
  - ES5에서 [[NumberData]]는 [[PrimitiveValue]]라고 불림
- Number 생성자 함수의 인수로 숫자를 전달하면서 new 연산자와 함께 호출하면 [[NumberData]] 내부 슬롯에 인수로 전달받은 숫자를 할당한 Number 래퍼 객체 생성
  ```js
  const numObj = new Number(10);
  console.log(numObj);    // Number {[[PrimivieValue]]: 10}
  ```
- Number 생성자 함수의 인수로 숫자가 아닌 값을 전달하면 인수를 숫자로 강제 변환 후, [[NumberData]] 내부 슬롯에 변환된 숫자를 할당한 Number 래퍼 객체 생성
  - 인수를 숫자로 변환할 수 없다면 NaN 할당
  ```js
  let numObj = new Number('10');
  console.log(numObj);    // Number {[[PrimitiveValue]]: 10}

  numObj = new Number('Hello');
  console.log(numObj);    // Number {[[PrimitiveValue]]: NaN}
  ```

<br>
<br>

## 2. Number 프로퍼티

<br>

### 2.1. Number.EPSILON
- ES6에서 도입된 Number.EPSILON은 1과 1보다 큰 숫자 중에서 가장 작은 숫자와의 차이와 같음 
- Number.EPSILON = 약 2.22044 ... x 10^(-16)
- 부동소수점으로 인해 발생하는 오차를 해결하기 위해 사용
  - 부동소수점 산술 연산으로는 정확한 결과 X
  - 부동소수점을 표현하기 위해 사용하는 IEEE 754는 2진법으로 변환했을 때 무한소수가 되어 미세한 오차가 발생
  ```JS
  0.1 + 0.2;    // 0.3000000000000004
  0.1 + 0.2 === 0.3;    // false
  ```
  ```js
  function isEqual(a, b) {
    // a와 b를 뺀 값의 절대값이 Number.EPSION보다 작으면 같은 수로 인정
    return Math.abs(a - b) < Number.EPSION;
  }

  isEqual(0.1 + 0.2, 0.3);    // true
  ```

<br>

### 2.2. Number.MAX_VALUE
- 자바스크립트에서 표현할 수 있는 가장 큰 양수 값
- Number.MAX_VALUE보다 큰 숫자는 Infinity
  ```js
  Number.MAX_VALUE;    // 1.7976931348623157e+308
  Infinity > Number.MAX_VALUE;     // true
  ```

<br>

### 2.3. Number.MIN_VALUE
- 자바스크립트에서 표현할 수 있는 가장 작은 양수 값
- Number.MIN_VALUE보다 작은 숫자는 0
  ```JS
  Number.MIN_VALUE;    // 5e-324
  Number.MIN_VALUE > 0;    // true
  ```

<br>

### 2.4. Number.MAX_SAFE_INTEGER
- 자바스크립트에서 안전하게 표현할 수 있는 가장 큰 정수값
  ```JS
  Number.MAX_SAFE_INTEGER;    // 9007199254740991
  ```

<br>

### 2.5. Number.MIN_SAFE_INTEGER
- 자바스크립트에서 안전하게 표현할 수 있는 가장 작은 정수값
  ```js
  Number.MIN_SAFE_INTEGER;    // -9007199254740991
  ```

<br>

### 2.6. Number.POSITIVE_INFINITY
- 양의 무한대를 나타내는 숫자값 Infinity와 같음
  ```js
  Number.POSITIVE_INFINITY;    // Infinity
  ```

<br>

### 2.7. Number.NEGATIVE_INFINITY
- 음의 무한대를 나타내는 숫자값 -Infinity와 같음
  ```js
  Number.NEGATIVE_INFINITY    // -Infinity
  ```

<br>

### 2.8. Number.NaN
- 숫자가 아님을 나타내는 숫자값
- Number.NaN = window.NaN
  ```js
  Number.NaN;    // NaN
  ```

<br>
<br>

## 3. Number 메서드

<br>

### 3.1. Number.isFinite
- ES6에서 도입된 Number.isFinite 정적 메서드는 인수로 전달된 숫자값이 정상적인 유한수, 즉 Infinity 또는 -Infinity가 아닌지 검사하여 그 결과를 불리언 값으로 반환
  - 인수가 정상적인 유한수이면 true 반환
  - 인수가 무한수이면 false 반환
  ```js
  Number.isFinite(0);    // true
  Number.isFinite(Number.MAX_VALUE);    // true
  Number.isFinite(Number.MIN_VALUE);    // true

  Number.isFinite(Infinity);    // false
  Number.isFinite(-Infinity);    // false
  ```
- 인수가 NaN이면 false 반환
  ```js
  Number.isFinite(NaN);    // false
  ```
- Number.isFinite 메서드와 빌트인 전역 함수 isFinite 차이
  - 빌트인 전역 함수 isFinite : 전달받은 인수를 숫자로 암묵적 타입 변환하여 검사 수행
  - Number.isFinite : 전달받은 인수를 숫자로 암묵적 타입 변환 X -> 숫자가 아닌 값은 언제나 false
  ```js
  Number.isFinite(null);    // false

  // null -> 0
  isFinite(null);    // true
  ```

<br>

### 3.2. Number.isInteger
- ES6에서 도입된 Number.isInteger 정적 메서드는 인수로 전달된 숫자값이 정수인지 검사하여 그 결과를 불리언 값으로 반환
- 검사하기 전에 인수를 숫자로 암묵적 타입 변환 X
  - 인수가 정수이면 true
  ```js
  Number.isInteger(0);    // true
  Number.isInteger(123);    // true
  Number.isInteger(-123);    // true

  Number.isInteger(0.5);    // false
  Number.isInteger('123');     // false
  Number.isInteger(false);    // false
  Number.isInteger(Infinity);    // false
  Number.isInteger(-Infinity);     // false
  ```

<br>

### 3.3. Number.isNaN
- ES6에서 도입된 Number.isNaN 정적 메서드는 인수로 전달된 숫자값이 NaN인지 검사하여 그 결과를 불리언 값으로 반환
  - 인수가 NaN이면 true 반환
  ```js
  Number.isNaN(NaN);    // true
  ```
- Number.isNaN 메서드와 빌트인 전역 함수 isNaN과 차이점
  - 빌트인 전역 함수 isNaN : 전달받은 인수를 숫자로 암묵적 타입 변환하여 검사 수행
  - Number.isNaN : 전달받은 인수를 숫자로 암묵적 타입 변환 X -> 숫자가 아닌 인수는 false
  ```js
  Number.isNaN(undefined);    // false

  // undefined -> NaN
  isNaN(undefined);    // true
  ```

<br>

### 3.4. Number.isSafeInteger
- ES6에서 도입된 Number.isSafeInteger 정적 메서드는 인수로 전달된 숫자값이 안전한 정수인지 검사하여 그 결과를 불리언 값으로 반환
- 안전한 정수값은 -(2^53 - 1)과 (2^53 - 1) 사이의 정수값
- 검사전에 인수를 숫자로 암묵적 타입 변환 X
  ```js
  Number.isSafeInteger(0);    // true
  Number.isSafeInteger(100000000000000);    // true
  Number.isSafeInteger(100000000000001);    // false
  Number.isSafeInteger(0.5);    // false
  Number.isSafeInteger('123');     // false
  Number.isSafeInteger(false);    // false
  Number.isSafeInteger(Infinity);    // false
  ```

<br>

### 3.5. Number.prototype.toExponential
- toExponential 메서드는 숫자를 지수 표기법으로 변환하여 문자열로 반환
  - 지수 표기법 :  매우 크거나 작은 숫자를 표현할 때 주로 사용하며 e(Exponent) 앞에 있는 숫자에 10의 n승을 곱하는 형식으로 수를 나타내는 방식
- 인수로 소수점 이하로 표현할 자릿수 전달 가능
  ```js
  (77.1234).toExponential();    // 7.71234e+1
  (77.1234).toExponential(4);    // 7.7123e+1
  (77.1234).toExponential(2);    // 7.71e+1
  ```
- 숫자 리터럴과 함께 Number 프로토타입 메서드를 사용할 경우 에러 발생
  ```js
  77.toExponential();    // SyntaxError: Invalid or unexpected token
  ```
  - 숫자 뒤의 .은 의미가 모호 -> 부동 소수점 숫자의 소수 구분 기호이거나 객체 프로퍼티에 접근하기 위한 접근 연산자
  - 자바스크립트 엔진은 숫자 뒤의 .을 부동 소수점 숫자의 소수 구분 기호로 해석
    - 77.toExponential()에서 77은 Number 래퍼 객체
    - 따라서 77 뒤의 .을 소수 구분 기호로 해석하면 뒤에 이어지는 toExponential을 프로퍼티로 해석할 수 없어 에러 발생
  ```js
  77.1234.toExponential();    // 7.71234e+1
  ```
  - 77뒤의 . 뒤에는 숫자가 이어지므로 .은 명백하게 부동 소수점 숫자의 소수 구분 기호
    - 숫자에 소수점 하나만 존재하므로 두번째 .은 프로퍼티 접근 연산자로 해석 
    - 따라서 숫자 리터럴과 함께 메서드를 사용할 경우 혼란을 방지하기 위해 그룹 연산자 사용 권장
    ```js
    (77).toExponential();    // 7.7e+1
    ```
    - 자바스크립트 숫자는 정수 부분과 소수 부분 사이에 공백을 포함할 수 없기 때문에 숫자 뒤의 . 뒤에 공백이 오면 .을 프로퍼티 접근 연산자로 해석하기 때문에 이 방법도 가능
    ```js
    77 .toExponential();    // 7.7e+1
    ```

<br>

### 3.6. Number.prototype.toFixed
- 숫자를 반올림하여 문자열로 반환
- 반올림하는 소수점 이하 자릿수를 나타내는 0~20 사이의 정수값을 인수로 전달 가능
  - 인수를 생략하면 기본값 0이 지정
  ```js
  (12345.6789).toFixed();    // 12346
  // 소수점 이하 1자릿수 유효, 나머지 반올림
  (12345.6789).toFixed(1);    // 12345.7
  // 소수점 이하 2자릿수 유효, 나머지 반올림
  (12345.6789).toFixed(2);    // 12345.68
  // 소수점 이하 3자릿수 유효, 나머지 반올림
  (12345.6789).toFixed(3);     // 12345.679
  ```

<br>

### 3.7. Number.prototype.toPrecision
- 인수로 전달받은 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림하여 문자열로 반환
- 인수로 전달받은 전체 자릿수로 표현할 수 없는 경우 지수 표기법으로 결과 반환
  - 전체 자릿수를 나타내는 0~21 사이의 정수값을 인수로 전달 가능
  - 인수를 생략하면 기본값 0이 지정
  ```js
  (12345.6789).toPrecision();    // 12345.6789
  // 전체 1자릿수 유효, 나머지 반올림
  (12345.6789).toPrecision(1);    // 1e+4
  // 전체 2자릿수 유효, 나머지 반올림
  (12345.6789).toPrecision(2);    // 1.2e+4
  // 전체 3자릿수 유효, 나머지 반올림
  (12345.6789).toPrecision(3);    // 12345/7
  ```

<br>

### 3.8. Number.prototype.toString
- 숫자를 문자열로 변환하여 반환
- 진법을 나타내는 2~36 사이의 정수값을 인수로 전달 가능
  - 인수를 생략하면 기본값 10진법 지정
  ```js
  (10).toString();    // "10"
  (16).toString(2);    // "10000"
  (16).toString(8);    // "20"
  (16).toString(16);    // "10"
  ```

<br>
<br>