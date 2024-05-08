# :fire: 05/08 학습 내용

## :one: 타입스크립트는 왜 필요한가

- 자바스크립트 단점
  - 스크립트 언어라 협업에 적합하지 않다.
  - 코드가 지저분하다.
- 타입스크립트 = 자바스크립트 + 타입 체크
- 자바스크립트 환경에서 타입스크립트를 적용하면 동작 안한다. 타입스크립트 코드를 자바스크립트로 컴파일해야 하기 때문이다.

---

## :two: 타입스크립트 사용하기

- 설치는 npm install typescript
- tsc --init으로 폴더 설정 초기화
- tsc 는 js로 컴파일 해주는 CLI이다.
- tsc -w를 입력해두면 변경사항 있을 시 자동으로 즉시 컴파일하여 반영해준다.

---

## :three: 데이터 타입과 추론

- 데이터 타입이 왜 중요한가
  - 데이터 값 입력할 때 잘못된 데이터 타입을 입력하는 실수가 발생했을때, 데이터 타입을 구분하지 않는 자바스크립트의 경우 이를 예방하지 못한다.
  - 이 문제를 해결하기 위해 나온 것이 타입스크립트.

- 타입스크립트의 데이터 타입
  - 기본 데이터 타입
    - number
    - string
    - null
    - undefined
  - 객체 타입
    - object
    - array
    - tuple: 각 요소가 다른 타입을 가질 수 있는 배열을 나타내는 타입
  - 특수 타입
    - any: 어떤 타입이든 할당될 수 있는 타입
    - unknown: 타입을 미리 알 수 없는 경우 사용되는 타입

---

## :four: 타입 명시

- 변수를 선언할 때 변수 값의 타입을 명시함으로써 변수의 데이터 타입을 지정한다.
```typescript
let stdId : number = 1111;
let stdName : string = 'Lee';

function Plus(a : number, b : number) :number { 
  return a + b;
}

```

---

## :five: 인터페이스

- 데이터 타입 묶음을 만들어주어 코드를 깔끔하게 하기 위해 사용한다.
```typescript
interface Student {
  stdId : number;
  stdName : string;
  age : number;
  gender? : string; // ← 물음표 키워드로써 선택적 property로 설정. gender 멤버가 누락되어도 Student 데이터 타입에 오류가 나지 않는다.
  course : string;
  completed : boolean;
  setName(name : string) : void; // 메소드도 인터페이스에 포함 가능하다.
  getName : () => void;
}

function getInfo(id : number) : Student {
  return {
    stdId : id,
    stdName : 'lee',
    age : 20,
    gender : 'female',
    course : 'javascript',
    completed : true
  }
}

function setInfo(student : Student) : void {
    console.log(student);
}

setInfo({
    stdId: 911010,
    stdName: 'lee',
    age: 20,
    gender : 'female',
    course: 'javascript',
    completed: true
})
```

---

## :six: 열거형

- 입력 값에 제한을 두기 위해 사용한다. enum과 사용 이유가 비슷하다.
- 사용자 정의 타입이다.

```typescript
enum GenderType { 
  Male = 1,
  Female = 2,
  GenderNeutral = 0
}
```
