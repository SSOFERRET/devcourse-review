# :fire: 04/22 내용 정리

## :one: jwt authorization

- 실제로는 user_id를 body에 전송 받아 신원 검증을 받는 경우는 없다. 로그인할 때 제공 받는 토큰 값을 Response Header를 통해 쿠키로 전달 받아 접근 허가 여부를 결정한다.

- postman을 통해 login API를 사용 후 console 창을 확인하면 Response Headers 안의 내용을 확인할 수 있다. 그 내용 중 setCookie가 해당 로그인을 통해 새로이 생성된 토큰 값이다. 
- Headers에 "Authorization" 키를 추가하고, 앞선 단계에서 받은 토큰 값을 Authorization의 값으로 설정해준다.
- 서버측은 Header를 통해 Authorazation 값을 전달 받아, jwt.verify() 함수로 토큰 값을 decode하여 접근 허가 여부를 결정한다.

```javascript

const express = require('express');
const app = express();
const dotenv = require('dotenv');
const jwt = require('jsonwebtoken');
dotenv.config();

app.listen(process.env.PORT);

// 토큰 발행
app.get('/jwt', function (req, res) {
    const token = jwt.sign({foo: 'bar'}, process.env.PRIVATE_KEY);
    res.cookie('jwt', token, {
        httpOnly:true
    });
    res.send('토큰 발행 완료');
});

// 토큰 decode
app.get('/jwt/decoded', function (req, res) {
    let token = req.headers["authorization"];
    console.log(token)
    const decoded = jwt.verify(token, process.env.PRIVATE_KEY);
    res.send(decoded);
});
```

## :two:  jwt 에러 처리 (try/catch, throw)

### TokenExpiredError
: 유효기간이 지난 토근 = 만료된 토근

### JsonWebTokenError
: 토큰 자체에 문제가 있을 때 

### try/catch
개발자가 예상하지 못한 에러를 처리하는 문법.
(실수, 사용자 입력을 잘못한 것, DB가 응답을 잘못한 것, ...)

```javascript
try {
  // 정상적으로 실행되는 코드;
} catch (err) {
  // 에러 처리;
}
```

### throw 문

사용자 정의 예외를 발생(throw)시키는 구문. 예외 처리를 해야하는 사항을 설정할 때 사용된다.

```javascript
function getRectArea(width, height) {
  if (isNaN(width) || isNaN(height)) {
    throw new Error('Parameter is not a number!');
  }
}

try {
  getRectArea(3, 'A');
} catch (e) {
  console.error(e);
  // Expected output: Error: Parameter is not a number!
}
```

---- 

## :three: instanceof 

생성자의 속성이 객체의 프로토타입 체인에 포함되어 있는지를 확안해주는 테스트 함수이다. Boolean 값으로 반환을 한다.

```
function Car(make, model, year) {
  this.make = make;
  this.model = model;
  this.year = year;
}
const auto = new Car('Honda', 'Accord', 1998);

console.log(auto instanceof Car);
// Expected output: true
```

금일 수업 중에는 토큰 값 예외 처리하는 중에 반환 받은 값이 err에 해당하는지 확인하기 위하여 사용되었다.

```javascript
if (authorization instanceof jwt.TokenExpiredError) {
    return res.status(StatusCodes.UNAUTHORIZED).json({
      "message" : "로그인 세션이 만료되었습니다. 다시 로그인 하세요."
    });
  } else if (authorization instanceof jwt.JsonWebTokenError) {
    return res.status(StatusCodes.UNAUTHORIZED).json({
      "message" : "잘못된 토큰입니다."
    }); 
  }
```
