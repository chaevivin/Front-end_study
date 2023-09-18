# 실습 프로젝트: 전화번호부 앱

## 1. 인터페이스
```ts
interface PhoneNumberDictionary {
  [phone: string]: {
    num: number;
  };
}

interface Contact {
  name: string;
  address: string;
  phones: PhoneNumberDictionary;
}

enum PhoneType {
  Home = 'home',
  Office = 'office',
  Studio = 'studio',
}
```
- PhoneNumberDictionary
  - 전화번호 목록
  - 인터페이스의 인덱스 시그니처 문법(`[phone: string]`) 사용
    - 문자열로 정의된 속성은 어떤 이름이든 속성으로 사용할 수 있다.
- Contact
  - 이름, 주소, 전화번호 목록
- PhoneType
  - phones의 유형에는 phone, office, studio가 있다.
  - 에러 발생을 방지하기 위해 이넘 타입 코드를 추가한다.

<br>

```ts
const homePhone = {
  home: {
    num: 91099998888
  }
}

const officesPhone = {
  KoreaOffice: {
    num: 91022227777
  },
  USAOffice: {
    num: 91066663333
  }
}
```

<br>

## 2. api 함수
```ts
function fetchContacts(): Promise<Contact[]> {
  const contacts: Contact[] = [
    {
      name: 'Tony',
      address: 'Malibu',
      phones: {
        home: {
          num: 11122223333,
        },
        office: {
          num: 44455556666,
        },
      },
    },
    {
      name: 'Banner',
      address: 'New York',
      phones: {
        home: {
          num: 77788889999,
        },
      },
    },
    {
      name: 'chaebeen',
      address: 'suwon',
      phones: {
        home: {
          num: 213423452,
        },
        studio: {
          num: 314882045,
        },
      },
    },
  ];
  return new Promise(resolve => {
    setTimeout(() => resolve(contacts), 2000);
  });
}
```
- fetchContacts() 함수를 호출하면 2초 후 contacts 변수에 담긴 배열이 반환된다.
- fetchContacts() 함수의 반환값은 Promise이므로 반환값의 타입을 Promise로 지정해준다.

<br>

### 3. 전화번호부 클래스
```ts
class AddressBook {
  contacts: Contact[] = [];

  constructor() {
    this.fetchData();
  }

  fetchData(): void {
    fetchContacts().then(response => {
      this.contacts = response;
    });
  }

  findContactByName(name: string): Contact[] {
    return this.contacts.filter(contact => contact.name === name);
  }

  findContactByAddress(address: string): Contact[] {
    return this.contacts.filter(contact => contact.address === address);
  }

  findContactByPhone(phoneNumber: number, phoneType: string): Contact[] {
    return this.contacts.filter(
      contact => contact.phones[phoneType].num === phoneNumber
    );
  }

  addContact(contact: Contact): void {
    this.contact.push(contact);
  }

  displayListByName(): string[] {
    return this.this.contacts.map(contact => contact.name);
  }

  displayListByAddress(): string[] {
    return this.contacts.map(contact => contact.address);
  }
}

new AddressBook();
```
- contacts: 클래스에 전반적으로 조작할 전화번호부 목록이 저장
- constructor(): 클래스가 new AddressBook() 으로 초기화되었을 때 fetchData() 라는 클래스 메서드를 사용하여 전화번호부 데이터를 contacts 속성에 저장
- findContactByName(name): 입력받은 이름으로 연락처를 찾는 메서드
  - filter()을 사용했기 때문에 결과 값으로 1개 또는 여러 개의 요소가 담긴 배열이 된다. 따라서 반환 타입은 Contact[]이다.
- findContactByAddress(address): 주소로 연락처를 찾는 메서드
- findContactByPhone(phoneNumber, phoneType): 전화번호부와 번호 유형으로 연락처를 찾는 메서드
- addContact(contact): 새 연락처를 전화번호부에 추가하는 메서드
- displayListByName(): 전화번호부 목록의 이름만 추 출해서 화면에 표시하는 메서드(화면 조작 관련 코드 없음)
  - 배열의 형태를 가공하는 map()을 사용했기 때문에 반환 타입은 배열 타입인 string[]이다.
- displayListByAddress(): 전화번호부 목록의 주소를 화면에 표시하는 메서드(화면 조작 관련 코드 없음)

<br>
<br>