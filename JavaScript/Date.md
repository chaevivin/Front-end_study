# Date
- 표준 빌트인 객체인 Date는 날짜와 시간(연, 월, 일, 시, 분, 초, 밀리초)을 위한 메서드를 제공

<br>
<br>

## 1. Date 생성자 함수
- Date는 생성자 함수
- Date 생성자 함수로 생성한 Date 객체는 기본적으로 현재 날짜와 시간을 나타내는 정수값을 갖고 있음
  - 이 값은 1970년 1월 1일 00:00:00(UTC)을 기점으로 Date 객체가 나타내는 시간까지의 밀리초를 나타냄
- 현재 날짜와 시간이 아닌 다른 날짜와 시간을 다루고 싶은 경우 Date 생성자 함수에 명시적으로 해당 날짜와 시간 정보를 인수로 지정
- Date 생성자 함수로 객체를 생성하는 방법
  - new Date()
  - new Date(milliseconds)
  - new Date(dateString)
  - new Date(year, month[, day, hour, minute, second, millisecond])

<br>

### 1.1. new Date()
- Date 생성자 함수를 인수 없이 new 연산자와 함께 호출하면 현재 날짜와 시간을 가지는 Date 객체를 반환
- Date 객체는 내부적으로 날짜와 시간을 나타내는 정수값을 갖지만 Date 객체를 콘솔에 출력하면 기본적으로 날짜와 시간 정보 출력
  ```js
  new Date();    // Tue Jul 25 2023 08:02:18 GMT+0900 (대한민국 표준시)
  ```
- Date 생성자 함수를 new 연산자 없이 호출하면 Date 객체를 반환하지 않고 날짜와 시간 정보를 나타내는 문자열 반환
  ```js
  Date();    // "Tue Jul 25 2023 08:02:18 GMT+0900 (대한민국 표준시)"
  ```

<br>

### 1.2. new Date(milliseconds)
- Date 생성자 함수에 숫자 타입의 밀리초를 인수로 전달하면 1970년 1월 1일 00:00:00(UTC)을 기점으로 인수로 전달된 밀리초만큼 경과한 날짜와 시간을 나타내는 Date 객체 반환
  ```js
  new Date(0);    // Thu Jan 01 1970 09:00:00 GMT+0900 (대한민국 표준시)

  // 8400000ms = 1day
  new Date(86400000);    // Fri Jan 02 1970 09:00:00 GMT+0900
  ```
  - 한국 표준시 KST는 협정 세계시 UTC에 9시간을 더한 시간

<br>

### 1.3. new Date(dateString)
- Date 생성자 함수에 날짜와 시간을 나타내는 문자열을 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date 객체 반환
  - 인수로 전달한 문자열은 Date.parse 메서드에 의해 해석 가능한 형식이어야 함
  ```js
  new Date('May 26, 2020 10:00:00');    // Tue May 26 2020 10:00:00 GMT+0900 (대한민국 표준시)

  new Date('2020/03/26/10:00:00');    // Tue May 26 2020 10:00:00 GMT+0900 (대한민국 표준시)
  ```

<br>

### 1.4. new Date(year, month[, hour, minute, second, millisecond])
- Date 생성자 함수에 연, 월, 일, 시, 분, 초, 밀리초를 의미하는 숫자를 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date 객체 반환
  - 연, 월을 반드시 지정
  - 지정하지 않은 옵션 정보는 0 또는 1로 초기화
- 인수 
  | 인수 | 내용 |
  | :--- | :--- |
  | year | 연을 나타내는 1900년 이후의 정수. 0부터 99는 1900부터 1999로 처리 |
  | month | 월을 나타내는 0 ~ 11까지의 정수 (주의: 0부터 시작. 0 = 1월) |
  | day | 일을 나타내는 1 ~ 31까지의 정수 |
  | hour | 시를 나타내는 0 ~ 23까지의 정수 |
  | minute | 분을 나타내는 0 ~ 59까지의 정수 |
  | second | 초를 나타내는 0 ~ 59까지의 정수 |
  | millisecond | 밀리초를 나타내는 0 ~ 999까지의 정수 |
- 연, 월을 지정하지 않은 경우 1970년 1월 1일 00:00:00(UTC)을 나타내는 Date 객체 반환
  ```js
  new Date(2020, 2);    // Sun Mar 01 2020 00:00:00 GMT+0900 (대한민국 표준시)

  new Date(2020, 2, 26, 10, 00, 00, 0);    // Thu Mar 26 2020 10:00:00 GMT+0900 (대한민국 표준시)

  // 가독성이 더 좋음
  new Date('2020/3/26/10:00:00');    // Thu Mar 26 2020 10:00:00 GMT+0900 (대한민국 표준시)
  ```

<br>
<br>

## 2. Date 메서드

<br>

### 2.1. Date.now
- 1970년 1월 1일 00:00:00(UTC)을 기점으로 현재 시간가지 경과한 밀리초를 숫자로 반환
  ```js
  const now = Date.now();    // 1690283754207
  new Date(now);    // Tue Jul 25 08:17:19 GMT+0900 (대한민국 표준시)
  ```

<br>

### 2.2. Date.parse
- 1970년 1월 1일 00:00:00(UTC)을 기점으로 인수로 전달된 지정 시간(new Date(dateString)의 인수와 동일한 형식)까지의 밀리초를 숫자로 반환
  ```js
  // UTC
  Date.parse('Jan 2, 1970 00:00:00 UTC');    // 86400000

  // KST
  Date.parse('Jan 2, 1070 09:00:00');    // 86400000

  // KST
  Date.parse('1970/01/02/09:00:00');    // 86400000
  ```

<br>

### 2.3. Date.UTC
- 1970년 1월 1일 00:00:00(UTC)을 기점으로 인수로 전달된 지정 시간까지의 밀리초를 숫자로 반환
- new Date(year, month[, hour, minute, second, millisecond])와 같은 형식을 사용
- Date.UTC 메서드의 인수는 로컬 타임(KST)이 아닌 UTC로 인식
  ```js
  Date.UTC(1970, 0, 2);    // 86400000
  Date.UTC('1970/1/2');    // NaN
  ```

<br>

### 2.4. Date.prototype.getFullYear
- Date 객체의 연도를 나타내는 정수 반환
  ```js
  new Date('2020/07/24').getFullYear();    // 2020
  ```

<br>

### 2.5. Date.prototype.setFullYear
- Date 객체에 연도를 나타내는 정수 설정
- 연도 이외의 옵션으로 월, 일도 설정 가능
  ```js
  const today = new Date();

  // 년도 지정
  today.setFullYear(2000);
  today.getFullYear();    // 2000

  // 년도/월/일 지정
  today.setFullYear(1900, 0, 1);
  today.getFullYear();    // 1900
  ```

<br>

### 2.6. Date.prototype.getMonth
- Date 객체의 월을 나타내는 0 ~ 11까지의 정수 반환
  - 1월은 0, 12월은 11
  ```js
  new Date('2020/07/24').getMonth();    // 6
  ```

<br>

### 2.7. Date.prototype.setMonth
- Date 객체에 월을 나타내는 0 ~ 11의 정수 설정
- 월 이외의 옵션으로 월도 설정 가능
  ```js
  const today = new Date();

  // 월 지정
  today.setMonth(0);    // 1월
  today.getMonth();    // 0

  // 월/일 지정
  today.setMonth(11, 1);    // 12월 1일
  today.getMonth();    // 11
  ```

<br>

### 2.8. Date.prototype.getDate
- Date 객체의 날짜(1 ~ 31)를 나타내는 정수 반환
  ```js
  new Date('2020/07/24').getDate();    // 24
  ```

<br>

### 2.9. Date.prototype.setDate
- Date 객체에 날짜(1 ~ 31)를 나타내는 정수 설정
  ```js
  const today = new Date();

  // 날짜 지정
  today.setDate(1);
  today.getDate();   // 1
  ```

<br>

### 2.10. Date.prototype.getDay
- Date 객체의 요일(0 ~ 6)을 나타내는 정수 반환
- 반환값
  | 요일 | 반환값 |
  | :---: | :---: |
  | 일요일 | 0 |
  | 월요일 | 1 |
  | 화요일 | 2 |
  | 수요일 | 3 |
  | 목요일 | 4 |
  | 금요일 | 5 |
  | 토요일 | 6 |
  ```js
  new Date('2020/07/24').getDay();    // 5
  ```

<br>

### 2.11. Date.prototype.getHours
- Date 객체의 시간(0 ~ 23)을 나타내는 정수 반환
  ```js
  new Date('2020/07/24/12:00').getHours();    // 12
  ```

<br>

### 2.12. Date.prototype.setHours
- Date 객체에 시간(0 ~ 23)을 나타내는 정수 설정
- 시간 이외에 옵션으로 분, 초, 밀리초 설정 가능
  ```js
  const today = new Date();

  // 시간 지정
  today.setHours(7);  
  today.getHours();    // 7

  // 시간/분/초/밀리초 지정
  today.setHours(0, 0, 0, 0);    // 00:00:00:00
  today.getHours();    // 0
  ```

<br>

### 2.13. Date.prototype.getMinutes
- Date 객체의 분(0 ~ 59)을 나타내는 정수 반환
  ```js
  new Date('2020/07/24/12:30').getMinutes();    // 30
  ```

<br>

### 2.14. Date.prototype.setMinutes
- Date 객체의 분(0 ~ 59)을 나타내는 정수 설정
- 분 이외의 옵션으로 초, 밀리초 설정 가능
  ```js
  const today = new Date();

  // 분 지정
  today.setMinutes(50);
  today.getMinutes();    // 50

  // 분/초/밀리초 지정
  today.setMinutes(5, 10, 999);    // HH:05:10:999
  today.getMinutes();    // 5
  ```

<br>

### 2.15. Date.prototype.getSeconds
- Date 객체의 초(0 ~ 59)를 나타내는 정수 반환
  ```js
  new Date('2020/07/24/12:30:10').getSeconds();    // 10
  ```

<br>

### 2.16. Date.prototype.setSeconds
- Date 객체에 초(0 ~ 59)를 나타내는 정수 설정
- 초 이외에 옵션으로 밀리초 설정 가능
  ```js
  const today = new Date();

  // 초 지정
  today.setSeconds(30);
  today.getSeconds();    // 30

  // 초/밀리초 지정
  today.setSeconds(10, 0);    // HH:MM:10:000
  today.getSeconds();    // 10
  ```

<br>

### 2.17. Date.prototype.getMilliseconds
- Date 객체의 밀리초(0 ~ 999)를 나타내는 정수 반환
  ```js
  new Date('2020/07/24/12:30:10:150').getMilliseconds();    // 150
  ```

<br>

### 2.18. Date.prototype.setMilliseconds
- Date 객체에 밀리초(0 ~ 999)를 나타내는 정수 설정
  ```js
  const today = new Date();

  // 밀리초 지정
  today.setMilliseconds(123);
  today.getMilliseconds();    // 123
  ```

<br>

### 2.19. Date.prototype.getTime
- 1970년 1월 1일 00:00:00(UTC)를 기점으로 Date 객체의 시간까지 경과된 밀리초 반환
  ```js
  new Date('2020/07/24/12:30').getTime();    // 1595561400000
  ```

<br>

### 2.20. Date.prototype.setTime
- Date 객체에 1970년 1월 1일 00:00:00(UTC)를 기점으로 경과된 밀리초 지정
  ```js
  const today = new Date();

  today.setTime(86400000);    // 86400000ms = 1day
  console.log(today);    // Fri Jan 1970 09:00:00 GMT+0900 (대한민국 표준시)
  ```

<br>

### 2.21. Date.prototype.getTimezoneOffset
- UTC와 Date 객체에 지정된 로캘(locale) 시간과 차이를 분 단위로 반환
  - UTC = KST - 9
  ```js
  const today = new Date();    // today의 지정 로캘은 KST

  today.getTimezoneOffset() / 60;    // -9
  ```

<br>

### 2.22. Date.prototype.toDateString
- 사람이 읽을 수 있는 형식의 문자열로 Date 객체의 날짜 반환
  ```js
  const today = new Date('2020/7/24/12:30');

  today.toString();    // Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
  today.toDateString();    // Fri Jul 24 2020
  ```

<br>

### 2.23. Date.prtotype.toTimeString
- 사람이 읽을 수 있는 형식의 문자열로 Date 객체의 시간을 표현한 문자열 반환
  ```js
  const today = new Date('2020/7/24/12:30');   

  today.toString();    // Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
  today.toTimeString();    // 12:30:00 GMT+0900 (대한민국 표준시)
  ```

<br>

### 2.24. Date.prototype.toISOString
- ISO 8601 형식으로 Date 객체의 날짜와 시간을 표현한 문자열 반환
  ```js
  const today = new Date('2020/7/24/12:30');

  today.toString();    // Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
  today.toISOString();    // 2020-07-24T03:30:00.000Z

  today.toISOString().slice(0, 10);    // 2020-07-24
  today.toISOString().slice(0, 10).replace(/-/g, '');    // 20200724
  ```

<br>

### 2.25. Date.prototype.toLocaleString
- 인수로 전달한 로캘을 기준으로 Date 객체의 날짜와 시간을 표현한 문자열 반환
- 인수를 생략한 경우 브라우저가 동작 중인 시스템의 로캘 적용
  ```js
  const today = new Date('2020/7/24/12:30');

  today.toString();    // Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
  today.toLocaleString();    // 2020. 7. 24. 오후 12:30:00
  today.toLocaleString('ko-KR');    // 2020. 7. 24. 오후 12:30:00
  today.toLocaleString('en-US');    // 7/24/2020, 12:30:00 PM
  today.toLocaleString('ja-JP');    // 2020/07/24 12:30:00
  ```

<br>

### 2.26. Date.prototype.toLocaleTimeString
- 인수로 전달한 로캘을 기준으로 Date 객체의 시간을 표현한 문자열 반환
- 인수를 생략한 경우 브라우저가 동작 중인 시스템의 로캘 적용
  ```js
  const today = new Date('2020/7/24/12:30');

  today.toString();    // Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
  today.toLocaleTimeString();    // 오후 12:30:00
  today.toLocaleTimeString('ko-KR');    // 오후 12:30:00
  today.toLocaleTimeString('en-US');    // 12:30:00 PM
  today.toLocaleTimeString('ja-JP');    // 12:30:00
  ```

<br>
<br>

## 3. Date를 활용한 시계 예제
- 현재 날짜와 시간을 초 단위로 반복 출력
  ```js
  (function printNow() {
    const today = new Date();

    const dayNames = [
      '(일요일)',
      '(월요일)',
      '(화요일)',
      '(수요일)',
      '(목요일)',
      '(금요일)',
      '(토요일)'
    ];

    // getDay 메서드는 해당 요일을 나타내는 정수 반환
    const day = dayNames[today.getDay()];

    const year = today.getFullYear();
    const month = today.getMonth() + 1;
    const date = today.getDate();
    let hour = today.getHours();
    let minute = today.getMinutes();
    let second = today.getSeconds();
    const ampm = hour >= 12 ? 'PM' : 'AM';

    // 12시간제로 변경
    hour %= 12;
    hour = hour || 12;    // hour가 0이면 12를 재할당

    // 10 미만인 분과 초를 2자리로 변경
    minute = minute < 10 ? '0' + minute : minute;
    second = second < 10 ? '0' + second : second;

    const now = `${year}년 ${month}월 ${date}일 ${day} ${hour}:${minute}:${second} ${ampm}`;

    console.log(now);

    // 1초마다 printNow 함수 재귀 호출
    setTimeout(printNow, 1000);
  }());
  ```

<br>
<br>