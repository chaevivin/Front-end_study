# RegExp

## 1. 정규 표현식이란?
- 정규 표현식(regular expression) : 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어(formal language)
- 정규 표현식은 자바스크립트의 고유 문법이 아님
- 문자열을 대상으로 **패턴 매칭 기능** 제공
  - 패턴 매칭 기능 : 특정 패턴과 일치하는 문자열을 검색하거나 추출 또는 치환할 수 있는 기능
  ```js
  // 사용자로부터 입력받은 휴대폰 전화번호
  const tel = '010-1234-567팔';

  // 정규 표현식 리터럴로 휴대폰 전화번호 패턴 정의
  const regExp = /^\d{3}-\d{4}-\d{4}$/;

  // tel이 휴대폰 전화번호 패턴에 매칭하는지 테스트
  regExp.test(tel);    // false
  ```
- 정규표현식 장점
  - 반복문과 조건문 없이 패턴을 정의하고 테스트하는 것으로 간단히 체크 가능
- 정규표현식 단점
  - 주석이나 공백을 허용하지 않고 여러 가지 기호를 혼합하여 사용하기 때문에 가독성이 좋지 않음

<br>
<br>

## 2. 정규 표현식 생성
- 정규 표현식 객체를 생성 방법
  - 정규 표현식 리터럴
  - RegExp 생성자 함수 사용
- 정규 표현식 리터럴 : /regexp/i 
  - / : 시작, 종료 기호
  - regexp : 패턴
  - i : 플래그
  ```js
  const target = 'Is this all there is?';

  // 패턴 : is
  // 플래그 : i => 대소문자를 구별하지 않고 검색
  const regexp = /is/i;

  // test 메서드는 target 문자열에 대해 정규 표현식 regexp의 패턴을 검색하여 매칭 결과를 불리언으로 반환
  regexp.test(target);    // true
  ```
- RegExp 생성자 함수
  ```js
  /**
   * pattern: 정규 표현식 패턴
   * flags: 정규 표현식의 플래그 (g, i, m, u, y)
   */
  
  new RegExp(pattern[, flags])
  ```
  ```js
  const target = 'Is this all there is?';

  const regexp = new RegExp(/is/i);    // ES6

  regexp.test(target);    // true
  ```
  - RegExp 생성자 함수를 사용하면 변수를 사용해 동적으로 RegExp 객체 생성 가능
    ```js
    const count = (str, char) => (str.match(new RegExp(char, 'gi')) ?? []).length;

    count('Is this all there is?', 'is');    // 3
    count('Is this all there is?', 'xx');    // 0
    ```

<br>
<br>

## 3. RegExp 메서드

<br>

### 3.1. RegExp.prototype.exec
- 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 배열로 반환
- 매칭 결과가 없는 경우 null 반환
  ```js
  const target = 'Is this all there is?';
  const regExp = /is/;

  regExp.exec(target);    // ["is", index: 5, input: "Is this all there is?", groups: undefined]
  ```
- exec 메서드는 문자열 내의 모든 패턴을 검색하는 g플래그를 지정해도 첫 번째 매칭 결과만 반환하므로 주의 필요

<br>

### 3.2. RegExp.prototype.test
- 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환
  ```js
  const target = 'Is this all there is?';
  const regExp = /is/;

  regExp.test(target);    // true
  ```

<br>

### 3.3. RegExp.prototype.match
- String 표준 빌트인 객체가 제공하는 match 메서드는 대상 문자열과 인수로 전달받은 정규 표현식과의 매칭 결과를 배열로 반환
  ```js
  const target = 'Is this all there is?';
  const regExp = /is/;

  target.match(regExp);    // ["is", index: 5, input: "Is this all there is?", groups: undefined]
  ```
- match 메서드는 g 플래그가 지정되면 모든 매칭 결과를 배열로 반환
  ```js
  const target = 'Is this all there is?';
  const regExp = /is/g;

  traget.match(regExp);    // ["is", "is"]
  ```

<br>
<br>

## 4. 플래그
- 정규 표현식의 검색 방식을 설정하기 위해 사용
- 종류
  | 플래그 | 의미 | 설명 |
  | :--- | :--- | :--- |
  | i | Ignore case | 대소문자를 구별하지 않고 패턴 검색 |
  | g | Global | 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색 |
  | m | Multi line | 문자열의 행이 바뀌더라도 패턴 검색 계속 |
- 플래그는 옵션이므로 선택적으로 사용 가능하고 순서와 상관없이 하나 이상의 플래그를 동시에 설정 가능
- 어떠한 플래그를 사용하지 않은 경우 대소문자를 구별해서 패턴 검색
- 문자열에 패턴 검색 매칭 대상이 1개 이상 존재해도 첫 번째 매칭한 대상만 검색하고 종료
  ```js
  const target = 'Is this all there is?';

  // is 문자열을 대소문자 구별하여 한 번만 검색
  target.match(/is/);    // ["is", index: 5, input: "Is this all there is", groups: undefined]

  // is 문자열을 대소문자를 구별하지 않고 한 번만 검색
  target.match(/is/i);    // ["Is", index: 0, input: "Is this all there is", groups: undefined]

  // is 문자열을 대소문자 구별하여 전역 검색
  target.match(/is/g);    // ["is". "is"]

  // is 문자열을 대소문자 구별하지 않고 전역 검색
  target.match(/is/ig);    // ["Is", "is", "is"]
  ```

<br>
<br>

## 5. 패턴
- 정규 표현식의 패턴은 문자열의 일정한 규칙을 표현하기 위해 사용
- 패턴은 /로 열고 닫으며 문자열의 따옴표 생략
  - 따옴표를 포함하면 따옴표까지 패턴에 포함되어 검색
- 패턴은 특별한 의미를 가지는 메타문자 또는 기호로 표현 가능
- 어떤 문자열에 패턴과 일치하는 문자열이 존재할 때 '정규 표현식과 매치한다'고 표현

<br>

### 5.1. 문자열 검색
- 정규 표현식의 패턴에 문자 또는 문자열을 지정하면 검색 대상 문자열에서 패턴으로 지정한 문자 또는 문자열 검색
- 검색 대상 문자열과 플래그를 생략한 정규 표현식의 매칭 결과를 구하면 대소문자를 구별하여 정규 표현식과 매치한 첫 번째 결과만 반환
  ```js
  const target = 'Is this all there is?';

  // is 문자열과 매치하는 패턴. 대소문자 구별
  const regExp = /is/;

  // target과 정규 표현식이 매치하는지 테스트
  regExp.test(target);    // true

  // target과 정규 표현식의 매칭 결과
  target.match(regExp);    // ["is", index: 5, input: "Is this all there is?", groups: undefined]
  ```
- 대소문자를 구별하지 않고 검색하려면 i 플래그 사용
  ```js
  const target = 'Is this all there is?';

  // is 문자열과 매치하는 패턴. 대소문자 구별 X
  const regExp = /is/i;

  target.match(regExp);    // ["Is", index: 0, input: "Is this all there is?", groups: undefined]
  ```
- 검색 대상 문자열 내에서 패턴과 모든 문자열을 전역 검색하려면 g 플래그 사용
  ```js
  const target = 'Is this all there is?';

  // is 문자열과 매치하는 패턴
  // 대상 문자열 내에서 일치하는 모든 문자열 전역 검색
  const regExp = /is/g;

  target.match(regExp);    // ["Is", "is", "is"]
  ```

<br>

### 5.2. 임의의 문자열 검색
- .은 임의의 문자 한 개 의미
- 문자 내용은 무엇이든 상관 없음
  ```js
  const target = 'Is this all there is?';

  // 임의의 3자리 문자열을 대소문자 구별하여 전역 검색
  const regExp = /.../g;

  target.match(regExp);    // ["Is", "thi", "s a", "ll ", "the", "re ", "is?"]
  ```

<br>

### 5.3. 반복 검색
- {m,n}은 앞선 패턴이 최소 m번, 최대 n번 반복되는 문자열 의미
- 콤마 뒤에 공백이 있으면 정상 동작하지 않으므로 주의
  ```js
  const target = 'A AA B BB Aa Bb AAA';

  // 'A'가 최소 1번, 최대 2번 반복되는 문자열 전역 검색
  const regExp = /A{1,2}/g;

  target.match(regExp);    // ["A", "AA", "A", "AA", "A"]
  ```
- {n}은 앞선 패턴이 n번 반복되는 문자열을 의미
  - {n} = {n,n}
  ```js
  const target = 'A AA B BB Aa Bb AAA';

  // 'A'가 2번 반복되는 문자열 전역 검색
  const regExp = /A{2}/g;

  target.match(regExp);    // ["AA", "AA"]
  ```
- {n,}은 앞선 패턴이 최소 n번 이상 반복되는 문자열을 의미
  ```js
  const target = 'A AA B BB Aa Bb AAA';

  // 'A'가 최소 2번 이상 반복되는 문자열 전역 검색
  const regExp = /A{2,}/g;

  target.match(regExp);    // ["AA", "AAA"]
  ```
- +는 앞선 패턴이 최소 한번 이상 반복되는 문자열을 의미
  - \+ = {1,}
  ```js
  const target = 'A AA B BB Aa Bb AAA';

  // 'A'가 최소 한 번 이상 반복되는 문자열 전역 검색
  const regExp = /A+/g;

  target.match(regExp);    // ["A", "AA", "A", "AAA"]
  ```
- ?는 앞선 패턴이 최대 한 번(0번 포함) 이상 반복되는 문자열을 의미
  - ? = {0,1}
  ```js
  const target = 'color colour';

  // 'colo' 다음 'u'가 최대 한 번 이상 반복되고 'r'이 이어지는 문자열 전역 검색
  const regExp = /colou?r/g;

  target.match(regExp);    // ["color", "colour"]
  ```

<br>

### 5.4. OR 검색
- |은 or의 의미
  ```js
  const target = 'A AA B BB Aa Bb';

  // 'A' 또는 'B'를 전역 검색
  const regExp = /A|B/g;

  target.match(regExp);    // ["A", "A", "A", "B", "B", "B", "A", "B"]
  ```
- 분해되지 않은 단어 레벨로 검색하기 위해서는 +를 함께 사용
  ```js
  const target = 'A AA B BB Aa Bb';

  // 'A' 또는 'B'가 한 번 이상 반복되는 문자열 전역 검색
  const regExp = /A+|B+/g;

  target.match(regExp);    // ["A", "AA', "B", "BB", "A", "B"]
  ```
  - 위 예제ㅐ를 더 간단히 표현
  ```js
  const target = 'A AA B BB Aa Bb';

  // 'A' 또는 'B'가 한 번 이상 반복되는 문자열 전역 검색
  const regExp = /[AB]+/g;

  target.match(regExp);    // ["A", "AA', "B", "BB", "A", "B"]
  ```
- 범위를 지정하려면 [] 내에 - 사용
  ```js
  const target = 'A AA BB ZZ Aa Bb';

  // 'A' ~ 'Z'가 한 번 이상 반복되는 문자열 전역 검색
  const regExp = /[A-Z]+/g;

  target.match(regExp);    // ["A", "AA", "BB", "ZZ", "A", "B"]
  ```
- 대소문자를 구별하지 않고 알파벡 검색하는 방법
  ```js
  const target = 'AA BB Aa Bb 12';

  // 'A' ~ 'Z' 또는 'a' ~ 'z'가 한 번 이상 반복되는 문자열 전역 검색
  const regExp = /[A-Za-z]+/g;

  target.match(regExp);    // ["AA", "BB", "Aa", "Bb"]
  ```
- 숫자를 검색하는 방법
  ```js
  const target = 'AA BB 12,345';

  // '0' ~ '9'가 한 번 이상 반복되는 문자열을 전역 검색
  const regExp = /[0-9]+/g;

  target.match(regExp);    // ["12", "345"]
  ```
  - 쉼표 포함하면 매칭 결과가 분리되지 않음
  ```js
  onst target = 'AA BB 12,345';

  // '0' ~ '9' 또는 ','가 한 번 이상 반복되는 문자열 전역 검색
  const regExp = /[0-9,]+/g;

  target.match(regExp);    // ["12,345"]
  ```
- \d는 숫자를 의미
  - \d = [0-9]
  - \D는 문자를 의미
  ```js
  const target = 'AA BB 12,345';

  // '0' ~ '9' 또는 ','가 한 번 이상 반복되는 문자열 전역 검색
  let regExp = /[\d,]+/g;
  target.match(regExp);    // ["12,345"]

  // '0' ~ '9'가 아닌 문자 또는 ','가 한 번 이상 반복되는 문자열 전역 검색
  regExp = /[\D,]+/g;
  target.match(regExp);    // ["AA BB ", ","]
  ```
- \w는 알파벳, 숫자, 언더스코어 의미
  - \w = [A-Za-z0-9_]
  - \W는 알파벳, 숫자, 언더스코어가 아닌 문자 의미
  ```js
  const target = 'Aa Bb 12,345 _$%&';

  // 알파벳, 숫자, 언더스코어, ','가 한 번 이상 반복되는 문자열 전역 검색
  let regExp = /[\w,]+/g;
  target.match(regExp);    // ["Aa", "Bb", "12,345", "_"]

  // 알파벳, 숫자, 언더스코어가 아닌 문자 또는 ','가 한 번 이상 반복되는 문자열 전역 검색
  regExp = /[\W,]+/g;
  target.match(regExp);    // [" ", " ", ",", "$%&"]
  ```

<br>

### 5.5. NOT 검색
- [...] 내의 ^은 not을 의미
  ```js
  const target = 'AA BB 12 Aa Bb';

  // 숫자를 제외한 문자열 전역 검색
  const regExp = /[^0-9]+/g';

  target.match(regExp);    // ["AA BB ", " Aa Bb"]
  ```

<br>

### 5.6. 시작 위치로 검색
- [...] 밖의 ^은 문자열의 시작을 의미
  ```js
  const target = 'https://www.google.com';

  // 'https'로 시작하는지 검색
  const regExp = /^https/;

  regExp.test(target);    // true
  ```

<br>

### 5.7. 마지막 위치로 검색
- $는 문자열의 마지막을 의미
  ```js
  const target = 'https://www.google.com;

  // 'com'으로 끝나는지 검색
  const regExp = /com$/;

  regExp.test(target);    // true
  ```

<br>
<br>

## 6. 자주 사용하는 정규 표현식

<br>

### 6.1. 특정 단어로 시작하는지 검사
- 'http://' 또는 'https://'로 시작하는지 검사
  ```js
  const url = 'https://www.google.com';

  /^https?:\/\//.test(url);    // true
  /^(http|https):\/\//.test(url);    // true
  ```

<br>

### 6.2. 특정 단어로 끝나는지 검사
- 'html'로 끝나는지 검사
  ```js
  const fileName = 'index.html';

  /html$/.test(fileName);    // true
  ```

<br>

### 6.3. 숫자로만 이루어진 문자열인지 검사
- 숫자로만 이루어진 문자열인지 검사
  ```js
  const target = '12345';

  /^\d+$/.test(target);    // true
  ```

<br>

### 6.4. 하나 이상의 공백으로 시작하는지 검사
- 하나 이상의 공백으로 시작하는지 검사
  - \s는 여러가지 공백 문자를 의미
  ```js
  const target = ' Hi';

  /^[\s]+/.test(target);    // true
  ```

<br>

### 6.5. 아이디로 사용 가능한지 검사
- 알파벳 대소문자 또는 숫자로 시작하고 끝나며 4 ~ 10자리인지 검사
  ```js
  const id = 'abc123';

  /^[A-Za-z0-9]{4,10}$/.test(id);    // true
  ```

<br>

### 6.6. 메일 주소 형식에 맞는지 검사
- 메일 주소 형식에 맞는지 검사
  ```js
  const email = 'chaevivin@gmail.com';

  /^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}$/.test(email)    // true
  ```

<br>

### 6.7. 핸드폰 번호 형식에 맞는지 검사
- 핸드폰 번호 형식에 맞는지 검사
  ```js
  const cellphone = '010-1234-5678';

  /^\d{3}-\d{3,4}-\d{4}$/.test(cellphone);    // true
  ```

<br>

### 6.8. 특수 문자 포함 여부 검사
- 특수 문자가 포함되어 있는지 검사
  - 특수 문자는 A-Za-z0-9 이외의 문자
  ```js
  const target = 'abc#123';

  (/[^A-Za-z0-9]/gi).test(target);    // true
  ```
  - 이 방식은 특수 문자를 선택적으로 검색할 수 있음
  ```js
  (/[\{\}\[\]\/?.,;:|\)*~`!^\-_+<>@\#$%&\\\=\(\'\"]/gi).test(target);    // true
  ```
- 특수 문자를 제거할 때는 String.prototype.replace 메서드 사용
  ```js
  target.replace(/[^A-Za-z0-9]/gi, '');    // abc123
  ```

<br>
<br>