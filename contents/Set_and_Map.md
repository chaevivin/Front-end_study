# Set과 Map

## 1. Set
- **Set 객체는 중복되지 않는 유일한 값들의 집합**
- Set과 배열의 차이점
  | 구분 | 배열 | Set 객체 |
  | :--- | :--- | :--- |
  | 동일한 값을 중복하여 포함 가능 | O | X |
  | 요소 순서에 의미 O | O | X |
  | 인덱스로 요소 접근 가능 | O | X |
- Set은 수학적 집합을 구현하기 위한 자료구조
  - Set으로 교집합, 합집합, 차집합, 여집합 등 구현 가능

<br>

### 1.1. Set 객체 생성
- Set 객체는 Set 생성자 함수로 생성
- Set 생성자 함수에 인수를 전달하지 않으면 빈 Set 객체 생성
  ```js
  const set = new Set();
  console.log(set);    // Set(0) {}
  ```
- **Set 생성자 함수는 이터러블을 인수로 전달받아 Set 객체 생성**
  - **이터러블의 중복된 값은 Set 객체에 요소로 저장되지 않음**
  ```js
  const set1 = new Set([1, 2, 3, 3]);
  console.log(set1);    // Set(3) {1, 2, 3}

  const set2 = new Set('hello');
  console.log(set2);    // Set(4) {'h', 'e', 'l', 'o'}
  ```
- 중복을 허용하지 않는 Set 객체의 특성을 활용하여 배열에서 중복된 요소 제거 가능
  ```js
  // 배열의 중복 요소 제거
  const uniq = array => array.filter((v, i, self) => self.indexOf(v) === i);
  console.log(uniq([2, 1, 2, 3, 4, 3, 4]));    // [2, 1, 3 ,4]

  // Set을 사용한 배열의 중복 요소 제거
  const uniq = array => [...new Set(array)];
  console.log(uniq([2, 1, 2, 3, 4, 3, 4]));    // [2, 1, 3, 4]
  ```

<br>

### 1.2. 요소 개수 확인
- Set 객체의 요소 개수를 확인할 때는 Set.prototype.size 프로퍼티 사용
  ```js
  const { size } = new Set([1, 2, 3, 3]);
  console.log(size);    // 3
  ```
- size 프로퍼티는 setter 함수 없이 getter 함수만 존재하는 접근자 프로퍼티
  - size 프로퍼티에 숫자를 할당하여 Set 객체의 요소 개수 변경 불가
  - size 프로퍼티에 숫자를 할당하면 무시됨
  ```js
  const set = new Set([1, 2, 3]);

  console.log(Object.getOwnPropertyDescriptor(Set.prototype, 'size'));
  // {set: undefined, enumerable: false, configurable: true, get: f}

  set.size = 10;
  console.log(set.size);    // 3
  ```

<br>

### 1.3. 요소 추가
- Set 객체에 요소를 추가할 때는 Set.prototype.add 메서드 사용
  ```js
  const set = new Set();
  console.log(set);    // Set(0) {}

  set.add(1);
  console.log(set);    // Set(1) {1}
  ```
- add 메서드는 새로운 요소가 추가된 Set 객체 반환
  - add 메서드를 호출한 후에 add 메서드를 연속적으로 호출 가능
  ```js
  const set = new Set();

  set.add(1).add(2);
  console.log(set);    // Set(2) {1, 2}
  ```
- Set 객체에 중복된 요소의 추가 불가 
  - Set 객체에 중복된 요소를 추가하면 무시됨
  ```js
  const set = new Set();

  set.add(1).add(2).add(2);
  console.log(set);    // Set(2) {1, 2}
  ```
- 일치 비교 연산자 ===을 사용하면 NaN과 NaN을 다르다고 평가하지만 Set 객체는 NaN과 NaN을 같다고 평가하여 중복 추가 허용 X
- +0과 -0은 일치 비교 연산자 ===와 마찬가지로 같다고 평가하여 중복 추가 허용 X
  ```js
  const set = new Set();

  console.log(NaN === NaN);    // false
  console.log(0 === -0);    // true

  set.add(NaN).add(NaN);
  console.log(set);    // Set(1) {NaN}

  set.add(0).add(-0);
  console.log(set);    // Set(2) {NaN, 0}
  ```
- Set 객체는 객체나 배열과 같이 자바스크립트의 모든 값을 요소로 저장 가능
  ```js
  const set = new Set();

  set
    .add(1)
    .add('a')
    .add(true)
    .add(undefined)
    .add(null)
    .add({})
    .add([])
    .add(() => {});
  
  console.log(set);    // Set(8) {1, "a", true, undefined, null, {}, [], () => {}}
  ```

<br>

### 1.4. 요소 존재 여부 확인
- Set 객체에 특정 요소가 존재하는지 확인하려면 Set.prototype.has 메서드 사용
- has 메서드는 특정 요소의 존재 여부를 나타내는 불리언 값 반환
  ```js
  const set = new Set([1, 2, 3]);

  console.log(set.has(2));    // true
  console.log(set.has(4));    // false
  ```

<br>

### 1.5. 요소 삭제
- Set 객체의 특정 요소를 삭제하려면 Set.prototype.delete 메서드 사용
- delete 메서드는 삭제 성공 여부를 나타내는 불리언 값 반환
- delete 메서드에는 인덱스가 아니라 삭제하는 요소값을 인수로 전달
  - Set 객체는 인덱스를 갖지 않기 때문
  ```js
  const set = new Set([1, 2, 3]);

  set.delete(2);
  console.log(set);    // Set(2) {1, 3}

  set.delete(1);   
  console.log(set);    // Set(1) {3}
  ```
- 존재하지 않는 Set 객체의 요소를 삭제하려 하면 무시됨
  ```js
  const set = new Set([1, 2, 3]);

  set.delete(0);
  console.log(set);    // Set(3) {1, 2, 3}
  ```
- delete 메서드는 삭제 성공 여부를 나타내는 불리언 값을 반환하기 때문에 연속적으로 호출 불가
  ```js
  const set = new Set([1, 2, 3]);

  set.delete(1).delete(3);    // TypeError: set.delete(...).delete is not a function
  ```

<br>

### 1.6. 요소 일괄 삭제
- Set 객체의 요소를 일괄 삭제하려면 Set.prototype.clear 메서드 사용
- clear 메서드는 언제나 undefined 반환
  ```js
  const set = new Set([1, 2, 3]);

  set.clear();
  console.log(set);    // Set(0) {}
  ```

<br>

### 1.7. 요소 순회
- Set 객체의 요소를 순회하려면 Set.prototype.forEach 메서드 사용
- Set.prototype.forEach 메서드는 Array.prototype.forEach 메서드와 유사하게 콜백 함수와 forEach 메서드의 콜백 함수 내부에서 this로 사용할 객체(옵션)를 인수로 전달
- 콜백 함수가 전달받는 인수 
  - 첫 번째 인수 : 현재 순회 중인 요소값
  - 두 번째 인수 : 현재 순회 중인 요소값
  - 세 번째 인수 : 현재 순회 중인 Set 객체 자체
  - 첫 번째 인수와 두 번째 인수는 같은 값 -> Array.prototype.forEach 메서드와 인터페이스 통일 목적
  ```js
  const set = new Set([1, 2, 3]);

  set.forEach((v, v2, set) => console.log(v, v2, set));
  /*
  1 1 set(3) {1, 2, 3}
  2 2 set(3) {1, 2, 3}
  3 3 set(3) {1, 2, 3}
  */
  ```
- **Set 객체는 이터러블** 
  - Set 객체는 Set.prototype의 Symbol.iterator 메서드를 상속받는 이터러블
  - 이터러블인 Set 객체는 for...of 문으로 순회 가능
  - 이터러블인 Set 객체는 스프레드 문법의 대상이 될 수 있음
  - 이터러블인 Set 객체는 배열 디스트럭처링 할다의 대상이 될 수 있음
  ```js
  const set = new Set([1, 2, 3]);

  console.log(Symbol.iterator in set);    // true

  for (const value of set) {
    console.log(value);    // 1 2 3
  }

  console.log([...set]);    // [1, 2, 3]

  const [a, ...rest] = set;
  console.log(a, rest);    // 1, [2, 3]
  ```
- Set 객체는 요소의 순서에 의미를 갖지 않지만 Set 객체를 순회하는 순서는 요소가 추가된 순서를 따름
  - 이터러블의 순회와 호환성을 유지하기 위해

<br>

### 1.8. 집합 연산
- Set 객체를 통해 교집합, 합집합, 차집합 등 구현 가능

<br>

**(1) 교집합**
- 교집합 A∩B는 집합 A와 집합 B의 공통 요소로 구성
  ```js
  Set.prototype.intersection = function (set) {
    const result = new Set();

    for (const value of set) {
      // 2개의 set의 요소가 공통되는 요소이면 교집합의 대상
      if (this.has(value)) result.add(value);
    }

    return result;
  };

  const setA = new Set([1, 2, 3, 4]);
  const setB = new Set([2, 4]);

  // setA와 setB의 교집합
  console.log(setA.intersection(setB));    // Set(2) {2, 4}
  // setB와 setA의 교집합
  console.log(setB.intersection(setA));    // Set(2) {2, 4}
  ```
- 또 다른 방법
  ```js
  Set.prototype.intersection = function (set) {
    return new Set([...this].filter(v => set.has(v)));
  };

  const setA = new Set([1, 2, 3, 4]);
  const setB = new Set([2, 4]);

  // setA와 setB의 교집합
  console.log(setA.intersection(setB));    // Set(2) {2, 4}
  // setB와 setA의 교집합
  console.log(setB.intersection(setA));    // Set(2) {2, 4}
  ```

<br>

**(2) 합집합**
- 합집합 A∪B는 집합 A와 집합 B의 중복 없는 모든 요소로 구성 
  ```js
  Set.prototype.union = function (set) {
    // this(Set 객체) 복사
    const result = new Set(this);

    for (const value of set) {
      // 합집합은 2개의 Set 객체의 모든 요소로 구성된 집합
      // 중복된 요소 미포함
      result.add(value);
    }

    return result;
  };

  const setA = new Set([1, 2, 3, 4]);
  const setB = new Set([2, 4]);

  // setA와 setB의 합집합
  console.log(setA.union(setB));    // Set(4) {1, 2, 3, 4}
  // setB와 setA의 합집합
  console.log(setB.union(setA));    // Set(4) {1, 2, 3, 4}
  ```
- 또 다른 방법
  ```js
  Set.prototype.union = function (set) {
    return new Set([...this, ...set]);
  };

  const setA = new Set([1, 2, 3, 4]);
  const setB = new Set([2, 4]);

  // setA와 setB의 합집합
  console.log(setA.union(setB));    // Set(4) {1, 2, 3, 4}
  // setB와 setA의 합집합
  console.log(setB.union(setA));    // Set(4) {1, 2, 3, 4}
  ```

<br>

**(3) 차집합**
- 차집합 A-B는 집합 A에는 존재하지만 집합 B에는 존재하지 않는 요소로 구성
  ```js
  Set.prototype.difference = function (set) {
    // this(Set 객체)를 복사
    const result = new Set(this);

    for (const value of set) {
      // 차집합은 어느 한쪽 집합에는 존재하지만 다른 한쪽 집합에는 존재하지 않는 요소로 구성된 집합
      result.delete(value);
    }

    return result;
  };

  const setA = new Set([1, 2, 3, 4]);
  const setB = new Set([2, 4]);

  // setA와 setB의 차집합
  console.log(setA.difference(setB));    // Set(2) {1, 3}
  // setB와 setA의 차집합
  console.log(setB.difference(setA));    // Set(0) {}
  ```
- 또 다른 방법
  ```js
  Set.prototype.difference = function (set) {
    return new Set([...this].filter(v => !set.has(v)));
  };

  const setA = new Set([1, 2, 3, 4]);
  const setB = new Set([2, 4]);

  // setA와 setB의 차집합
  console.log(setA.difference(setB));    // Set(2) {1, 3}
  // setB와 setA의 차집합
  console.log(setB.difference(setA));    // Set(0) {}
  ```

<br>

**(4) 부분집합과 상위 집합**
- 집합 A가 집합 B에는 포함되는 경우(A ⊆ B) 집합 A는 집합 B의 부분 집합(subset)이며, 집합 B는 집합 A의 상위 집합(superset)
  ```js
  // this가 상위 집합인지 확인
  Set.prototype.isSuperset = function (subset) {
    for (const value of subset) {
      // superset의 모든 요소가 subset의 모든 요소를 포함하는지 확인
      if (!this.has(value)) return false;
    }

    return true;
  };

  const setA = new Set([1, 2, 3, 4]);
  const setB = new Set([2, 4]);

  // setA가 setB의 상위 집합인지 확인
  console.log(setA.isSuperset(setB));    // true
  // setB가 setA의 상위 집합인지 확인
  console.log(setB.isSuperset(setA));    // false
  ```
- 또 다른 방법
  ```js
  // this가 상위 집합인지 확인
  Set.prototype.isSuperset = function (subset) {
    const supersetArr = [...this];
    return [...subset].every(v => supersetArr.includes(v));
  };

  const setA = new Set([1, 2, 3, 4]);
  const setB = new Set([2, 4]);

  // setA가 setB의 상위 집합인지 확인
  console.log(setA.isSuperset(setB));    // true
  // setB가 setA의 상위 집합인지 확인
  console.log(setB.isSuperset(setA));    // false
  ```

<br>
<br>

## 2. Map
- **Map 객체는 키와 값의 쌍으로 이루어진 컬렉션**
- Map과 객체의 차이점
  | 구분 | 객체 | Map 객체 |
  | :---: | :---: | :---: |
  | 키로 사용할수 있는 값 | 문자열 또는 심벌 값 | 객체를 포함한 모든 값 |
  | 이터러블 | X | O |
  | 요소 개수 확인 | Object.keys(obj).length | map.size |

<br>

### 2.1. Map 객체의 생성
- Map 객체는 Map 생성자 함수로 생성
- Map 생성자 함수에 인수를 전달하지 않으면 빈 Map 객체 생성
  ```js
  const map = new Map();
  console.log(map);    // Map(0) {}
  ```
- **Map 생성자 함수는 이터러블을 인수로 받아 Map 객체 생성**
  - **인수로 전달되는 이터러블은 키와 값의 쌍으로 이루어진 요소로 구성**
  ```js
  const map1 = new Map(['key1', 'value1'], ['key2', 'value2']);
  console.log(map1);    // Map(2) {"key1" => "value1", "key2" => "value2"}

  const map2 = new Map([1, 2]);    // TypeError: Iterator value 1 is not an entry object
  ```
- Map 생성자 함수의 인수로 전달한 이터러블에 중복된 키를 갖는 요소가 존재하면 값이 덮어써지기 때문에 중복된 키를 갖는 요소 존재 불가
  ```js
  const map = new Map(['key1', 'value1'], ['key1', 'value2']);
  console.log(map);    // Map(1) {"key1" => "value2"}
  ```

<br>

### 2.2. 요소 개수 확인
- Map 객체의 요소 개수를 확인할 때는 Map.prototype.size 프로퍼티 사용
  ```js
  const { size } = new Map(['key1', 'value1'], ['key2', 'value2']);
  console.log(size);    // 2
  ```
- size 프로퍼티는 setter 함수 없이 getter 함수만 존재하는 접근자 프로퍼티
  - size 프로퍼티에 숫자를 할당하여 Map 객체의 요소 개수 변경 불가능
  - size 프로퍼티에 숫자를 할당하면 무시됨
  ```js
  const map = new Map(['key1', 'value1'], ['key2', 'value2']);

  console.log(Object.getOwnPropertyDescriptor(Map.prototype, 'size')); 
  // {set: undefined, enumerable: false, configurable: true, get: f}

  map.size = 10;
  console.log(map.size);    // 2
  ```

<br>

### 2.3. 요소 추가
- Map 객체에 요소를 추가할 때는 Map.prototype.set 메서드 사용
  ```js
  const map = new Map();
  console.log(map);    // Map(0) {}

  map.set('key1', 'value1');
  console.log(map);    // Map(1) {"key1" => "value1"}
  ```
- set 메서드는 새로운 요소가 추가된 Map 객체를 반환하기 때문에 set 메서드를 호출한 후에 set 메서드를 연속적으로 호출 가능
  ```js
  const map = new Map();

  map
    .set('key1', 'value1')
    .set('key2', 'value2');

  console.log(map);    // Map(2) {"key1" => "value1", "key2" => "value2"}
  ```
- Map 객체에는 중복된 키를 갖는 요소가 존재할 수 없기 때문에 중복된 키를 갖는 요소를 추가하면 값이 덮어씌어짐
  - Map 객체에 중복된 키를 갖는 요소를 추가하면 무시됨
  ```js
  const map = new Map();

  map
    .set('key1', 'value1')
    .set('key1', 'value2');

  console.log(map);    // Map(1) {"key1" => "value2"}
  ```
- Map 객체는 NaN과 NaN을 같다고 평가하여 중복 추가 허용 X
- +0과 -0은 일치 비교 연산자 ===와 마찬가지로 같다고 평가하여 중복 추가 허용 X
  ```js
  const map = new Map();

  console.log(NaN === NaN);    // false
  console.log(-0 === 0);    // true

  map.set(NaN, 'value1').set(NaN, 'value2');
  console.log(map);    // Map(1) {NaN => "value2"}

  map.set(0, 'value1').set(-0, 'value2');
  console.log(map);    // Map(2) {NaN => "value2", 0 => "value2"}
  ```
- Map 객체는 키 타입에 제한 X
  - 객체를 포함한 모든 값을 키로 사용 가능
  ```js
  const map = new Map();

  const lee = { name: 'Lee' };
  const kim = { name: 'Kim' };

  map
    .set(lee, 'developer')
    .set(kim, 'designer');

  console.log(map);
  // Map(2) { {name: "Lee"} => "developer", {name: "kim"} => "designer" }
  ```

<br>

### 2.4. 요소 취득
- Map 객체에서 특정 요소를 취득하려면 Map.prototype.get 메서드 사용
- get 메서드의 인수로 키를 전달하면 Map 객체에서 인수로 전달한 키를 갖는 값 반환
- Map 객체에서 인수로 전달한 키를 갖는 요소가 존재하지 않으면 undefined 반환
  ```js
  onst map = new Map();

  const lee = { name: 'Lee' };
  const kim = { name: 'Kim' };

  map
    .set(lee, 'developer')
    .set(kim, 'designer');
  
  console.log(map.get(lee));    // developer
  console.log(map.get('key'));    // undefined
  ```

<br>

### 2.5. 요소 존재 여부 확인
- Map 객체에 특정 요소가 존재하는지 확인하려면 Map.prototype.has 메서드 사용
- has 메서드는 특정 요소의 존재 여부를 나타내는 불리언 값 반환
  ```js
  const lee = { name: 'Lee' };
  const kim = { name: 'Kim' };

  const map = new Map([[lee, 'developer'], [kim, 'designer']]);

  console.log(map.has(lee));    // true
  console.log(map.has('key'));    // false
  ```

<br>

### 2.6. 요소 삭제
- Map 객체의 요소를 삭제하려면 Map.prototype.delete 메서드 사용
- delete 메서드는 삭제 성공 여부를 나타내는 불리언 값 반환
  ```js
  const lee = { name: 'Lee' };
  const kim = { name: 'Kim' };

  const map = new Map([[lee, 'developer'], [kim, 'designer']]);

  map.delete(kim);
  console.log(map);    // Map(1) { {name: "Lee"} => "developer" }
  ```
- 존재하지 않는 키로 Map 객체의 요소를 삭제하려면 에러 없시 무시됨
  ```js
  const map = new Map([['key1', 'value1']]);

  map.delete('key2');
  console.log(map);    // Map(1) {"key1" => "value1"}
  ```
- delete 메서드는 삭제 성공 여부를 나타내는 불리언 값을 반환하기 때문에 연속적으로 호출 불가
  ```js
  const lee = { name: 'Lee' };
  const kim = { name: 'Kim' };

  const map = new Map([[lee, 'developer'], [kim, 'designer']]);

  map.delete(lee).delete(kim);    // TypeError: map.delete(...).delete is not a function
  ```

<br>

### 2.7. 요소 일괄 삭제
- Map 객체의 요소를 일괄 삭제하려면 Map.prototype.clear 메서드 사용
- clear 메서드는 언제나 undefined 반환
  ```js
  const lee = { name: 'Lee' };
  const kim = { name: 'Kim' };

  const map = new Map([[lee, 'developer'], [kim, 'designer']]);

  map.clear();
  console.log(map);    // Map(0) {}
  ```

<br>

### 2.8. 요소 순회
- Map 객체의 요소를 순회하려면 Map.prototype.forEach 메서드 사용
- Map.prototype.forEach 메서드는 Array.prototype.forEach 메서드와 유사하게 콜백 함수와 forEach 메서드의 콜백 함수 내부에서 this로 사용할 객체(옵션)를 인수로 전달
- 콜백 함수가 전달받는 인수
  - 첫 번째 인수 : 현재 순회 중인 요소 값
  - 두 번째 인수 : 현재 순회 중인 요소 키
  - 세 번째 인수 : 현재 순회 중인 Map 객체 자체
  ```js
  const lee = { name: 'Lee' };
  const kim = { name: 'Kim' };

  const map = new Map([[lee, 'developer'], [kim, 'designer']]);

  map.forEach((v, k, map) => console.log(v, k, map));
  /*
  developer {name: "Lee"} Map(2) {
    {name: "Lee"} => "developer",
    {name: "Kim"} => "designer"
  }
  designer {name: "Kim"} Map(2) {
    {name: "Lee"} => "developer",
    {name: "Kim"} => "designer"
  }
  */
  ```
- **Map 객체는 이터러블**
  - Map 객체는 Map.prototype의 Symbol.iterator 메서드를 상속받는 이터러블
  - 이터러블인 Map 객체는 for...of 문으로 순회 가능
  - 이터러블인 Map 객체는 스프레드 문법의 대상이 될 수 있음
  - 이터러블인 Map 객체는 배열 디스트럭처링 할당의 대상이 될 수 있음
  ```js
  const lee = { name: 'Lee' };
  const kim = { name: 'Kim' };

  const map = new Map([[lee, 'developer'], [kim, 'designer']]);

  console.log(Symbol.iterator in map);    // true

  for (const entry of map) {
    console.log(entry);    // [{name: "Lee"}, "developer"] [{name: "Kim"}, "designer"]
  }

  console.log([...map]);
  // [[{name: "Lee"}, "developer"], [{name: "Kim"}, "designer"]]

  const [a, b] = map;
  console.log(a, b);    // [{name: "Lee"}, "developer"] [{name: "Kim"}, "designer"]
  ```
- Map 객체는 이터러블이면서 동시에 이터레이터인 객체를 반환하는 메서드 제공
  | Map 메서드 | 설명 |
  | :--- | :--- |
  | Map.prototype.keys | Map 객체에서 요소키를 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체 반환 |
  | Map.prototype.values | Map 객체에서 요소값을 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체 반환 |
  | Map.prototype.entries | Map 객체에서 요소키와 요소값으로 갖는 이터러블이면서 동시에 이터레이터인 객체 반환 |
  ```js
  const lee = { name: 'Lee' };
  const kim = { name: 'Kim' };

  const map = new Map([[lee, 'developer'], [kim, 'designer']]);

  for (const key of map.keys()) {
    console.log(key);    // {name: "Lee"} {name: "Kim"}
  }

  for (const value of map.values()) {
    console.log(value);    // developer designer
  }

  for (const entry of map.entries()) {
    console.log(entry);    //  [{name: "Lee"}, "developer"] [{name: "Kim"}, "designer"]
  }
  ```
- Map 객체는 요소의 순서에 의미를 가지 않지만 Map 객체를 순회하는 순서는 요소가 추가된 순서 따름
  - 다른 이터러블의 순회와 호환성을 유지하기 위한 목적

<br>
<br>