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

❓ **그럼 forEach와 map 둘의 차이는...?**

➡ 반환 형식
- forEach는 return한 내용이 반영되지는 않는다.
- map은 return한 내용을 새 배열/객체에 반영해준다.

---

## :two: URL method : **GET** __- 전체 조회하기__

```
const express = require("express");
const app = express();

app.listen(8888);

let user_1 = {
    name: "윈터",
    nickname: "winter",
    grade: "silver"
}

let user_2 = {
    name: "카리나",
    nickname: "carina",
    grade: "gold",
}

let user_3 = {
    name: "지젤",
    nickname: "GISELLE",
    grade: "platinum",
}

let db = new Map();

db.set(1, user_1);
db.set(2, user_2);
db.set(3, user_3);

app.use(express.json());
app.get('/users', (req, res) => {
    let users = {};
    db.forEach(user=> {
        users[user.name] = user;
    });
    res.json(users);
})
```
db 내의 각 객체를 forEach문으로 하나하나 꺼내어 새 object에 push하고 res.json으로 전송한다.

※ JSON.stringify() : JavaScript 값이나 객체를 JSON 문자열로 변환

---

## :three: URL method : **DELETE**

```
app.delete('/users/:id', (req, res) => {
    let {id} = req.params;
    id = parseInt(id);
    let msg = "";
    try {
        const name = db.get(id).name;
        db.delete(id);
        msg = `${name}님, 다음에 또 뵈어요`;
    } catch(error) {
        msg = `존재하지 않는 아이디입니다.`
    }
    res.json({ message : msg });
})
```

❓url이 GET method랑 겹치는데?

:right_arrow: method가 다르니 괜찮다!

아이디가 없을 경우에 대한 예외처리도 해주도록 한다.

---

## :four: URL method : **PUT**

id는 req.params로, 수정할 정보는 req.body로 받는다.

```
app.put('/users/:id', (req, res) => {
    const {id} = req.params;
    let user = db.get(parseInt(id));
    let msg = "";
    if (user) {
        const {new_nickname} = req.body;
        const old_nickname = user.nickname;
        user.nickname = new_nickname;
        msg = `${old_nickname}님, 닉네임이 ${user.nickname}로 변경되었습니다.`;
    } else {
        msg = "존재하지 않는 아이디입니다.";
    }
    res.json({message: msg})
})
```


