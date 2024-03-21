# :fire: 03/21 배운 내용 정리하기
## :one: forEach와 map

forEach와 map
배열 또는 객체에서 요소를 하나 꺼낸 다음
매개변수로 그 요소를 전달하여 호출되는 콜백함수

```
// forEach
const arr1 = ['a', 'b', 'c', 'd']

arr1.forEach((v, i, a) => {
    console.log(`v: ${v}`);
    console.log(`i: ${i}`);
    console.log(`a: ${a}`);
})

const obj1 = new Map();
obj1.set(1, 'a');
obj1.set(2, 'b');
obj1.set(3, 'c');

obj1.forEach((v, k, o) => {
    console.log(`v: ${v}`);
    console.log(`k: ${k}`);
    console.log(`o: ${o}`);
})
```

```
// map
const arr2 = ['a', 'b', 'c', 'd']

arr2.map((v, i, a) => {
    console.log(`v: ${v}`);
    console.log(`i: ${i}`);
    console.log(`a: ${a}`);
})

const obj2 = new Map();
obj2.set(1, 'a');
obj2.set(2, 'b');
obj2.set(3, 'c');

obj2.forEach((v, k, o) => {
    console.log(`v: ${v}`);
    console.log(`k: ${k}`);
    console.log(`o: ${o}`);
})
```

- v = value
- i = index
- k = key
- a = array 통째로
- o = object

forEach와 map 둘 다 매개변수의 순서가 동일하게 적용된다.

❓ 그럼 forEach와 map 둘의 차이는...?
➡ 반환 형식
- forEach는 return한 내용이 반영되지는 않는다.
- map은 return한 내용을 새 배열/객체에 반영해준다.
