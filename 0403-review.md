# :fire: 04/03 배운 내용 정리

## :one: express 함수의 미들웨어화

```
const validate = (req, res, next) => {
    //...
    //validate 통과되면 미들웨어에서 벗어나 다음 단계로 가라.
    return next(); 
}
```

express가 validate에 인수를 넘겨준다.

---

## :two: JWT

'로그인(인증) 세션이 만료되었습니다.'는 메시지는 에러에 의해 발생하는게 아니다!

### 🗒 인증

Authentication

관리자든 고객이든 인증을 통해서 사이트에 가입된 사용자라는 것을 증명하는 것.

**세션** : 로그인이 되어있는 상태

### 🗒 인가

Authorization

인증 뒤에, 페이지 접근 권한 여부 판단하는 것.

---

## :three: 쿠키와 세션

**역할** : 유저의 로그인을 유지시켜준다.

### ⭐ 쿠키

웹에서 서버와 클라이언트가 주고받는 데이터 중 하나.

사용자와 서버가 계속 쿠키를 주고 받으면서 로그인 상태임을 확인한다.

**장점**
    - 서버가 저장을 하지 않아도 된다. → 서버 저장 공간을 아낄 수 있다.
    - Stateless → RESTful

**단점** : 보안에 취약하다.

### :star: 세션

쿠키의 단점을 보완하기 위한 대안으로,

서버가 정보 저장 인증 번호를 생성하여 사용자와 서버는 이 번호을 쿠키 대신 주고 받는다.

**장점** : 보안이 비교적 좋다.

**단점** : 서버가 정보를 저장해야한다 → Stateless하지 못하다.

---

## :four: JWT

JSON Web Token

쿠키와 세션의 단점을 보완하기 위한 대안.

**개념** : JSON 형태의 데이터를 안전하게 전송하기 위한 (웹에서 사용하는) 토큰

= 토큰을 가진 사용자가 증명을 하기 위한 수단

cf. 토큰 : 인증용/인가용

**장점**
    - 암호화가 되어 보안에 강하다
    - HTTP 특징을 잘 따랐다. Statelss. 서버가 상태를 저장하지 않는다.
    - 서버 부담이 덜하다.

cf. 토큰을 발행하는 서버를 따로 만들 수 있다.

### 🗒 JWT의 구조

link : jwt.io

토큰이 온점으로 3등분으로 되어있다.
    - header : 토큰 암호화 알고리즘 종류, 타입
    - payload : 사용자 정보 (비밀번호 포함 안됨)
    - verify signature : 데이터를 보증해주는 서명값
        - 서버가 발행한 verify signature와 일치하지 않으면 서버는 이 토큰을 안 받아준다.
        - 페이로드 값이 바뀌면 이 서명값이 통째로 바뀌면서 보안을 해준다.

토큰을 해독하면 위 내용이 JSON 형태로 이루어져 있다.

### 🗒 JWT로 인증/인가하는 절차

```
npm i jsonwebtoken
```

![0403](https://github.com/SSOFERRET/devcourse-review/assets/148465774/635c46f1-f3f5-4239-b5cf-fc75687c01e3)



### 🗒 JWT 라이브러리 사용해보기

```
const jwt = require('jsonwebtoken');

let token = jwt.sign({foo:'bar'}, '----');
console.log(token);
```

서명 메소드이다.
token을 생성해준다 = jwt 서명을 했다. (페이로드, 나만의 암호키) + SHA256

```
let decoded = jwt.verify(token, '----');

console.log(decoded);
// { foo: 'bar', iat: 1712134829 }
// iat : 발행시간을 초로 표현함.
```

만약 검증에 성공하면, 페이로드 값을 확인할 수 있다.

### 🗒 .env

environment
환경 변수 설정값

**개념** : 개발을 하다가 외부에 유출되면 안되는 중요한 환경 변수를 따로 관리하기 위한 파일의 확장자 명.

※ 프로젝트 파일 최상단에 위치해야 한다.

```
npm i dotenv
```
```
const dotenv = require('dotenv');
dotenv.config();
/**                                 ↓ */
let token = jwt.sign({foo:'bar'}, process.env.PRIVATE_KEY);
```

실제 암호키 대신 환경변수를 인수로 넘겨준다.
