# 클래스

## 1. 클래스란?
- 클래스: 여러 가지 유사한 객체를 쉽게 생성하는 자바스크립트 최신 문법(ES6+)이다.
- 자바스크립트 객체 
  ```js
  const titan = {
    name: '진격의 거인',
    genre: 'action'
  };

  const gintama = {
    name: '은혼',
    genre: 'comedy'
  };
  ```
  - 두 객체에는 name과 genre이라는 공통된 속성을 가지고 있다.
  - 이렇게 모양이 유사한 객체는 일일이 객체를 정의하기보다 생성자 함수를 사용하는 것이 유리하다.
    - 생성자 함수는 new라는 키워드를 붙여서 호출하면 새로운 객체를 생성한다.
    - 생성자 함수를 사용하면 더 쉽게 유사한 구조의 객체를 여러 개 만들 수 있다.
  ```js
  const Animation(name, genre) {
    this.name = name;
    this.genre = genre;
  }

  const titan = new Animation('진격의 거인', 'action');
  const gintama = new Animation('은혼', 'comedy');
  ```
  - 생성자 함수는 클래스로 표현할 수 있다.
    - 클래스는 생성자 함수라는 일반적인 관례를 문법 레벨로 끌어올린 것이다.
  ```js
  class Animation {
    constructor(name, genre) {
      this.name = name;
      this.genre = genre;
    }
  }
  ```

<br>
<br>

## 2. 클래스 기본 문법
- 생성자 함수
  ```js
  function Animation(name, genre) {
    this.name = name;
    this.genre = genre;
  }

  Animation.prototype.sayHi = function () {
    console.log('hi');
  }
  ```
  - Animation() 생성자 함수에 sayHi라는 속성 함수(속성 값으로 함수가 연결된 속성)를 추가한다.
  - 생성자 함수의 이름(name)에 '하이큐'라는 문자열을 넣고, 장르(genre)에 'sports'라는 문자열을 넣어 새로운 객체를 생성한다.
    ```js
    const haikyuu = new Animation('하이큐', 'sports');
    ```
    - 객체 안에 name과 genre라는 속성이 있고 객체의 프로토타입에 sayHi라는 속성 함수가 있다.
  - 생성된 객체의 속성과 속성 함수는 출력하여 사용할 수 있다.
    ```js
    console.log(haikyuu.name);    // 하이큐
    console.log(haikyuu.genre);    // sports
    haikyuu.sayHi();    // hi
    ```
  - name과 genre는 속성이므로 객체 속성 접근자로 접근하면 되고, sayHi()는 속성에 함수가 연결되어 있는 구조이므로 함수 형태로 호출하면 된다.
- 클래스
  ```js
  class Animation {
    constructor(name, genre) {
      this.name = name;
      this.genre = genre;
    }

    sayHi() {
      console.log('hi');
    }
  }
  ```
  - Animation 클래스는 name과 genre 값을 받아 객체를 생성할 수 있게 **생성자 메서드(constructor)** 를 선언하고, sayHi()라는 **클래스 메서드(class method)** 를 선언했다.
  - name과 genre 속성을 **클래스 필드(class field)** 또는 **클래스 속성(class property)** 이라고 한다.
  - 클래스는 new 키워드를 붙여 객체를 생성한다.
    - 클래스로 생성된 객체를 **클래스 인스턴스(class instance)** 라고 한다.
    ```js
    const haikyuu = new Animation('하이큐', 'sports');
    ```

<br>
<br>

## 3. 클래스의 상속
- 클래스의 상속(inheritance): 부모 클래스의 속성과 메서드 등을 자식 클래스에서도 사용할 수 있도록 물려주는 것을 의미한다.
  ```js
  class Person {
    constructor(name, skill) {
      this.name = name;
      this.skill = skill;
    }

    sayHi() {
      console.log('hi');
    }
  }

  class Developer extends Person {
    constructor(name, skill) {
      super(name, skill);
    }

    coding() {
      console.log('fun');
    }
  }
  ```
  - Developer 클래스에서 extends 키워드를 사용해서 Person 클래스를 상속받았다.
  - `super(name, skill)` 이 코드는 자식 클래스인 Developer 클래스에서 new 키워드로 객체를 생성할 때 부모 클래스인 Person 클래스의 생성자 메서드를 호출한다는 의미이다.
  - Developer 클래스로 새로운 객체(클래스 인스턴스) 생성
    ```js
    const chaebeen = new Developer('채빈', 'js');
    chaebeen.coding();    // fun 
    ```
    - 클래스로 생성한 객체를 chaebeen 변수에 담고 클래스 메서드 coding()을 호출하면 fun이 출력된다.
  - 변수로 부모 클래스의 메서드 호출
    ```js
    chaebeen.sayHi();    // hi
    ```
    - 부모 클래스 Person에 정의된 sayHi() 메서드를 호출하면 hi가 출력된다.
  - 클래스 속성에 접근
    ```js
    console.log(chaebeen.name);    // 채빈
    console.log(chaebeen.skill);    // js
    ```
    - chaebeen 변수에서 name과 skill 속성에 모두 접근할 수 있다.
  - 상속을 하면 클래스 인스턴스뿐만 아니라 자식 클래스 코드 내부에서도 부모 클래스의 속성이나 메서드를 접근할 수 있다.
    ```js
    class Person {
      constructor(name, skill) {
        this.name = name;
        this.skill = skill;
      }

      sayHi() {
        console.log('hi');
      }
    }

    class Developer extends Person {
      constructor(name, skill) {
        super(name, skill);
      }

      coding() {
        console.log('fun doing ' + this.skill + 'by ' + this.name);
      }
    }

    const chaebeen = new Developer('채빈', 'TypeScript');
    chaebeen.coding();    // fun doing TypeScript by 채빈
    ```
    - 상속받은 자식 클래스에서 부모 클래스의 속성과 메서드를 사용한다.
    - 콘솔창에 'hi'가 출력되는 이유
      - 자식 클래스인 Developer의 생성자 메서드에 부모 클래스의 메서드인 this.sayHi();를 정의했기 때문이다. 
- 클래스를 상속받으면 기존 클래스에 정의된 속성과 메서드를 재활용할 수 있어 객체 지향 프로그래밍에서 유용하다.

<br>
<br>

## 4. 타입스크립트의 클래스
- 클래스에 타입을 정의하는 방법
  ```js
  class Chatgpt {
    constructor(name) {
      this.name = name;
    }

    sum(a, b) {
      return a + b;
    }
  } 
  ```
  ```ts
  // 위 클래스에 타입 적용
   class Chatgpt {
    constructor(name: string) {
      this.name = name;    // Error
    }

    sum(a: number, b: number): number {
      return a + b;
    }
  } 
  ```
  - 생성자 메서드 함수의 파라미터인 name의 타입은 string, sum() 클래스 메서드 함수의 파리미터와 반환 타입은 모두 number이다.
  - 하지만 이 코드를 실행하면 타입 에러가 발생한다.
    - 타입스크립트로 클래스를 작성할 때는 생성자 메서드에서 사용될 클래스 속성들을 미치 정의해 주어야 한다.
    ```ts
    class Chatgpt {
      name: string;
      constructor(name: string) {
        this.name = name;    \
      }

      sum(a: number, b: number): number {
        return a + b;
      }
    } 
    ```

<br>
<br>

## 5. 클래스 접근 제어자
- **클래스 접근 제어자(access modifier)**: 클래스 속성의 노출 범위를 정의할 수 있다.
  - 복잡한 기능을 구현할 때 유용하다.
  - 의도치 않은 에러가 발생할 확률을 낮출 수 있다.

<br>

### 5.1. 클래스 접근 제어자의 필요성
```ts
class Animation {
  name: string;
  genre: string;

  constructor(name: string, genre: string) {
    this.name = name;
    this.genre = genre;
  }
}

const titan = new Animation('진격의 거인', 'action');
console.log(titan.name);    // 진격의 거인
```
위의 코드를 살펴보면,
- 클래스로 생성한 객체는 titan이라는 변수에 담겨 있어서 `titan.name`으로 접근하면 클래스로 생성할 때 초깃값으로 넘겼던 문자열 '진격의 거인'이 출력된다.
- 이렇게 생성된 객체는 자유롭게 속성을 변경할 수 있다.
  ```ts
  const titan = new Animation('진격의 거인', 'action');
  console.log(titan.name);    // 진격의 거인
  titan.name = '주술회전';
  console.log(titan.name);    // 주술회전
  ```
  - 클래스 속성의 내용이 변경되었을 때 영향을 주는 로직이 따로 없기 때문에 문제가 없다.

<br>

```ts
class WaterPurifier {
  waterAmount: number;

  constructor(waterAmount: number) {
    this.waterAmount = waterAmount;
  }

  wash() {
    if(this.waterAmount > 0) {
      console.log('정수기 동작 성공');
    }
  }
}

const purifier = new WaterPurifier(30);
purifier.wash();    // 정수기 동작 성공
```
위의 코드를 살펴보면,
- 정수기를 의미하는 클래스 코드로, 클래스 생성자 메서드로 클래스를 생성할 때 물의 양을 입력받을 수 있다.
- wash() 클래스 메서드는 물이 조금이라도 있어야 동작한다.
- purifier 객체를 사용하여 wash() 메서드 함수를 실행할 수 있다.
- 만약 purifier 객체에 접근하여 물의 양을 0으로 바꾸고 wash() 메서드를 실행하면 정상적으로 동작하지 않는다.
  ```ts
  const purifier = new WaterPurifier(30);
  purifier.wash();    // 정수기 동작 성공
  purifier.waterAmount(0);
  purifier.wash();   
  ```
  - 클래스 접근 제어자를 이용하면 이러한 에러를 방지할 수 있다.

<br>

### 5.2. 클래스 접근 제어자: public, private, protected
- 클래스 접근 제어자에는 3개의 종류가 있다.
  - public
  - private
  - protected

<br>

**(1) public**
- public 접근 제어자는 클래스 안에 선언된 속성과 메서드를 어디서든 접근할 수 있게 한다.
- 클래스 속성과 메서드에 별도로 속성 접근 제어자를 선언하지 않으면 기본적으로 public이다.
  ```ts
  class WaterPurifier {
    public waterAmount: number;

    constructor(waterAmount: number) {
      this.waterAmount = waterAmount;
    }

    public wash() {
      if(this.waterAmount > 0) {
        console.log('정수기 동작 성공');
      }
    }
  }
  ```
  - waterAmount와 wash()에 public 접근 제어자가 붙어 있기 때문에 클래스로 생성한 객체의 속성과 메서드를 어디에서나 접근할 수 있다. 
    ```ts
    const purifier = new WaterPurifier(50);
    console.log(purifier.waterAmount);    // 50
    purifier.wash();    // 정수기 동작 성공
    ```

<br>

**(2) private**
- private 접근 제어자는 클래스 코드 외부에서 클래스의 속성과 메서드를 접근할 수 없다.
- public과 반대되는 개념으로 클래스 안 로직을 외부 세상에서 단절시켜 보호할 때 주로 사용한다.
  ```ts
  class Animation {
    private name: string;
    private genre: string;

    constructor(name: string, genre: string) {
      this.name = name;
      this.genre = genre;
    }

    private sayHi() {
      console.log('hi');
    }
  }
  ```
  - 클래스 속성인 name과 skill과 클래스 메서드인 sayHi()에 private 접근 제어자를 지정한다.
  - Animation 클래스로 객체를 하나 생성하고 name 속성을 출력하면 타입 에러가 발생한다.
    ```ts
    const titan = new Animation('진격의 거인', 'action');
    console.log(titan.name);    // Error
    ```
    - 클래스의 name 속성이 private으로 정의되어 있는데, 외부에서 해당 속성을 사용하려고 해서 에러가 발생했다.
    - private으로 지정된 name 속성은 클래스 안에서만 엑세스하라고 안내한다.
  - private 접근 제어자를 사용하면 클래스 코드 바깥에서는 해당 속성이나 메서드를 접근할 수 없다.
    - private이 적용된 클래스 메서드 sayHi()도 마찬가지

<br>

**(3) protected**
- protected 접근 제어자는 선언된 속성이나 메서드는 클래스 코드 외부에서 사용할 수 없다. 하지만 상속받은 클래스에서는 사용할 수 있다.
  ```ts
  class Person {
    private name: string;
    private skill: string;

    constructor(name: string, skill: string) {
      this.name = name;
      this.skill = skill;
    }

    protected sayHi(): void {
      console.log('hi');
    }
  }

  class Developer extends Person {
    constructor(name: string, skill: string) {
      super(name, skill);
      this.sayHi();
    }

    coding(): void {
      console.log('fun doing ' + this.skill + 'by ' + this.name);    // Error
    }
  }
  ```
  - Person 클래스의 name과 skill 속성 타입을 모두 string으로 지정하고 접근 제어자로 private을 선언했다. 그리고 클래스 메서드 sayHi()에는 protected를 선언했다.
  - Person 클래스를 상속받은 Developer 클래스는 생성자 메서드에서 부모 클래스의 생성자 메서드를 호출하고, 부모 클래스의 sayHi() 메서드를 호출했다. 그리고 coding() 메서드에서는 부모 클래스의 skill과 name 속성에 접근했다.
  - Developer의 coding 메서드에서 skill과 name에서 타입 에러가 발생한다.
    - this.skill과 this.name은 Person 클래스에 private으로 정의된 속성을 클래스 외부에서 접근하려고 했기 때문에 발생한다.
    - skill과 name은 private이기 때문에 Person 클래스에서만 사용해야 한다.
  - sayHi() 메서드는 protected로 설정되어 있기 때문에 클래스를 상속받은 자식 클래스에서 사용해도 에러가 발생하지 않는다.
  - 부모 클래스로 객체를 생성하고 메서드를 호출하면 에러가 발생한다.
    ```ts
    const chaebeen = new Person('채빈', 'TypeScript');
    chaebeen.sayHi();
    ```
    - sayHi() 메서드는 보호된 속성이므로 private과 마찬가지로 클래스 외부에서 사용할 수 없다. 
    - 보호된 속성은 해당 클래스와 하위 클래스에서만 사용할 수 있다.
  - 자식 클래스로 객체를 생성하고 메서드를 호출하면 에러가 발생하지 않는다.
    ```ts
    const chaebeen = new Developer('채빈', 'TypeScript');
    chaebeen.coding();
    ```
    - Devleoper의 coding() 메서드는 접근 제어자를 설정하지 않았기 때문에 public이 설정된다.

<br>

### 5.3. 클래스 접근 제어자로 정수기 문제 해결하기
```ts
class WaterPurifier {
  waterAmount: number;

  constructor(waterAmount: number) {
    this.waterAmount = waterAmount;
  }

  wash() {
    if(this.waterAmount > 0) {
      console.log('정수기 동작 성공');
    }
  }
}

const purifier = new WaterPurifier(30);
purifier.wash();    // 정수기 동작 성공
purifier.waterAmount = 0;
purifier.wash();
```
위 코드를 살펴보면,
- 정수기 코드의 문제점은 클래스 외부에 노출되지 안ㄹ아야 할 속성이 노출되어 치명적인 에러가 발생한다는 것이다.
- 정수기 클래스에서 외부에 꼭 노출해야 할 동작은 wash() 메서드이다.
- 동작에 영향을 미치는 물의 양(waterAmount)은 외부에서 마음대로 제어하면 안된다.
  - 접근 제어자로 문제 해결
    ```ts
    class WaterPurifier {
      private waterAmount: number;

      constructor(waterAmount: number) {
        this.waterAmount = waterAmount;
      }

      public wash() {
        if(this.waterAmount > 0) {
          console.log('정수기 동작 성공');
        }
      }
    }

    const purifier = new WaterPurifier(30);
    purifier.wash();    // 정수기 동작 성공
    purifier.waterAmount = 0;    // Error
    ```
    - waterAmount는 private으로 설정하고, wash()는 public으로 설정
    - 클래스로 생성한 객체에서 waterAmount에 접근할 수 없다.

<br>

### 5.4. 클래스 접근 제어자를 사용할 때 주의점
- 클래스 접근 제어자를 사용할 때 주의해야 할 점은 접근 범위에 따라 실행까지 막아 주지 않는다는 것이다.
  ```ts
  class WaterPurifier {
    private waterAmount: number;

    constructor(waterAmount: number) {
      this.waterAmount = waterAmount;
    }

    public wash() {
      if(this.waterAmount > 0) {
        console.log('정수기 동작 성공');
      }
    }
  }

  const purifier = new WaterPurifier(30);
  purifier.wash();    // 정수기 동작 성공
  purifier.waterAmount = 0;    // Error
  purifier.wash();    // 정수기 동작 성공
  ```
  - `purifier.waterAmount = 0`에서 에러가 발생하지만 wash 메서드는 한 번밖에 실행되지 않는다.
    - 그 이유는 타입스크립트의 접근 제어자가 지정되어 있더라도 실행 시점의 에러까지는 보장해주지 못하기 때문이다.
    - 타입스크립트는 실행하는 시점이 아니라 그 전 단계인 컴파일할 때 미리 에러를 발견하는데 목적이 있기 때문이다.
- private의 실행 결과까지도 크래스 접근 제어자와 일치시키고 싶다면 자바스크립트의 private 문법(#)을 사용하면 된다.
  ```ts
  class WaterPurifier {
    #waterAmount: number;

    constructor(waterAmount: number) {
      this.#waterAmount = waterAmount;
    }

    public wash() {
      if(this.#waterAmount > 0) {
        console.log('정수기 동작 성공');
      }
    }
  }

  const purifier = new WaterPurifier(30);
  purifier.wash();    // 정수기 동작 성공
  purifier.#waterAmount = 0;    // Error
  purifier.wash();    // 정수기 동작 성공
  ```
  - 정수기 클래스에 private 접근 제어자 대신 #을 적용한다.
  - #waterAmount는 private 속성이므로 외부에서 접근할 수 없다는 에러가 발생한다.
  - private 접근 제어자를 붙였을 때와는 다르게 실행되지 않는다. 
  - 실행되더라도 private 속성 자체에 접근할 수 없다.

<br>
<br>