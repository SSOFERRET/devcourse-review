# :fire: 04/10 배운 내용 정리

## :one: node.js 패키지 구조

- apps.js : 프로젝트의 메인 라우터 역할
- /routes : 하위 라우터 역할을 하는 파일의 모음

#### ✅라우터가 로직까지 수행할 때 단점

1. 코드가 복잡해진다.
2. 가독성이 나쁘다
3. 트러블 슈팅이 어렵다.
**즉, 유지 보수하기 어렵다.**

#### ✅해결책

- 콜백 함수를 분리하자(컨트롤러).

---

## :two: 회원가입 시 비밀번호 암호화

**모듈 crypto**
- node.js의 기본 모듈
- 암호화 담당

```javascript
const salt = crypto.randomBytes(10).toString('base64');
// 64만큼의 길이로 바이트 값을 랜덤으로 만들어준다.

const hashPassword = crypto.pbkdf2Sync(password, salt, 10000, 64, 'sha512').toString('base64');
// password-based key derivation function 2의 준말.
// salt 값을 토대로 알고리즘을 적용하여 패스워드를 암호화한다.
```

- 회원가입할 때 암호화된 비밀번호와 함께 salt 값도 같이 저장.
- 로그인할 때 입력된 비밀번호를 저장된 salt 값으로 암호화하고 DB에 저장된 비밀번호와 비교한다.



