# :fire: 05/08 학습 내용

## :one: 객체 리터럴

- 특정 값을 나타내는 타입으로, 해당 값이 정확하게 일치해야한다.
- 코드 가독성이 높아지며, 잘못된 값이 들어오는 것을 막을 수 있다.

```typescript
interface Student {
  gender : 'male'|'female'|'gender neutral'; // ← 이외의 값이 입력되면 오류 발생.
}
```

- 인터페이스의 객체 리터럴은 자식 클래스 implements에 반영이 되지 않는다. 즉, 자식 클래스의 인터페이스에는 객체 리터럴을 따로 설정해 주어야한다.

---

## :two: 유니온, 타입별칭, 타입가드

### 📝 유니온
- 제한된 타입을 동시에 지정하고 싶을 때 사용.
- | 기호를 두고 여러 타입으로 지정할 수 있다.
```
let anyVal: number | string;
```

### 📝 타입 별칭
- type alias
- type 키워드를 사용해서 새로운 타입을 선언하는 것을 말한다.
```
type strOrNum = number | string;
let numStr : strOrNum = '100';
```

### 📝 타입 가드
- typeof 연산자를 이용해서 타입 검증을 수행할 수 있다.

---

## :three: Array와 Tuple

### 📝 Array
```
let numbers : number[] = [1,2,3,4,5];
let fruits : string[] = ['apple', 'banana', 'orange'];
let mixedArray : (number | string)[] = [1, 'two', 3, 'four'];
let infer = [1, 2, 3] // 타입스크립트의 타입 추론에 의해 사용 가능.
let readOnlyArray : ReadonlyArray<number> = [1, 2, 3];
```

### 📝 Tuple
- 자바스크립트에는 없는 데이터 타입이다.
- 길이가 고정적이며 타입의 순서가 정해져있다.
```
let greeting : [number, string, boolean] = [1, 'hello', true];
```




