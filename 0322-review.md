# :fire: 03/22 배운 내용 정리하기

## :one: 핸들러

요청에 의해 호출되는 메소드.

```
app.get('/', (req, res) => {});

// 여기서 두 번째 매개변수인 콜백 함수가 핸들러.
```

지금 우리가 배우고 있는 내용 안에서만 한하자면, HTTP request가 날아왔을 때 자동으로 호출되는 메소드를 말한다.

---

## :two: 예외처리

응답이 불가능한 요청이 들어왔지만 서버가 이걸 분간하지 못하는 case를 프로그래머가 직접 http status code 실패로 지정해준다.

=> '예외를 터뜨리다.'
```
app.get("/users/:id", (req, res) => {
    const user = db.get(parseInt(req.params.id));
    if (user) {
        res.status(201).json(user);
    } else {

    /** 예외 터뜨리기!!! */
        res.status(404).json(
            { message : "요청하신 유저를 찾을 수 없습니다."}
        );
    }
})
```
```
app.post("/users", (req, res) => {
    const nickname = req.body.nickname;
    if (nickname) {
        db.set(db.size + 1, nickname);
        res.json(
            {message : `${nickname}님, 환영합니다.`}
        );
    } else {

    /** 예외 터뜨리기!!! */
        res.status(400).json(
            { message : "요청을 수행할 수 없습니다. 닉네임을 입력하세요."}
        )
    }
})
```

:star: 클라이언트(사용자, 화면)과 소통을 정확하기 위함!


---

## :three: 미니 프로젝트

#### 프로젝트 구조

- 회원
    - 로그인         : (POST/login)
    - 회원 가입      : (POST/join)
    - 회원 정보 조회 : (GET/bloggers/:id)
    - 회원 탈퇴      : (DELETE/bloggers/:id)

    - 채널 관리
        - 채널 생성 (계정 1개 당 채널 100개까지 가능하다.)
        - 채널 수정
        - 채널 삭제

```
const express = require("express");
const app = express();
app.listen(8888);
app.use(express.json());

const db_blogger = new Map();

//login
app.post('/login', (req, res) => {
    const {id, password} = req.body;
    const blogger = db_blogger.get(id);
    if (blogger && password === blogger.password) {
        res.status(200).json(blogger);
    } else if (blogger && password !== blogger.password) {
        res.status(404).json(
            { message : "비밀번호가 틀렸습니다." }
        );
    } else if (!blogger) {
        res.status(400).json(
            { message : "존재하지 않는 ID입니다." }
        );
    }
});

//register
app.post('/register', (req, res) => {
    const blogger = req.body;
    const id = db_blogger.get(blogger.id);

    if (!db_blogger.get(blogger.id)) {
        db_blogger.set(blogger.id, blogger);
        res.status(201).json(
            { message : `${blogger.bloggerName}님, 가입을 환영합니다.`}
        );
        console.log(db_blogger);
    } else {
        res.status(400).json(
            { message : "이미 존재하는 ID입니다. 다른 ID를 입력해주십시오."}
        );
    }
});

//get
app.get('/bloggers/:id', (req, res) => {
    const {id} = req.params;
    const blogger = db_blogger.get(id);
    if (blogger) {
        res.status(200).json(blogger);
    }
});

//delete
app.delete('/bloggers/:id', (req, res) => {
    const {id} = req.params;
    const blogger = db_blogger.get(id);
    if (blogger) {
        db_blogger.delete(id);
        res.status(200).json({ message : `${blogger.bloggerName}님, 안녕히 가세요.` });
    } else if (!blogger) {
        res.status(400).json(
            { message : "존재하지 않는 ID입니다." }
        );
    }
});

```

---

### :plus: json array 

프론트엔드 방향으로 json으로 보낼 때 json array 형태로 많이 전송함.


### :plus: find()

배열 안에 있는 요소 중, 괄호 안의 내용과 일치하는 요소를 찾아서 보여준다.

### :plus: == 와 === 의 차이
- ==  : 자료형 상관 없이 데이터 내용이 같은가
- === : 자료형과 데이터 내용이 일치하는가