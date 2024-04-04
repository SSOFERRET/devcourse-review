# :fire: 04/04 배운 내용 정리

## :one: cookie로 응답하기

response로 토큰을 전달하려면 다음 함수를 사용하면 된다.

```
res.cookie("token", token);
```

첫번째 매개변수 String은 쿠키가 만들 상자의 이름을 뜯한다.

두번째 매개변수는 실제 토큰 값이다.

Postman에서 Body와는 별개로 Cookie로 전달됨을 확인할 수 있다.

**cookie-parser**
req에 있는 쿠키를 꺼내서 사용할 수 있게 도와주는 라이브러리.
req를 받을 때 필요하다.

## :two: cookie 설정하기

### Postman Cookies의 칼럼

Cookies 칼럼 중에 HttpOnly와 Secure는 보안과 관련된 환경 요소.

- HttpOnly
    - http에서만 사용할지/https에서도 같이 사용할지
    - = 프론트엔드가 아니라 API 호출"만" 허락할 것인가?
    - XSS 공격과 같은 프론트엔드에의 공격을 막기 위함.
    - cookie() 메소드의 매개변수로 설정할 수 있다.

```
res.cookie("token", token, {
    httpOnly : true
})
```

## :three: JWT 유효 기간 설정

token 발행하는 sign() 메소드에 옵션으로 설정할 수 있다.

```
const token = jwt.sign({
    //...사용자 정보
    }, process.env.PRIVATE_KEY, {
        expiresIn : '5m',
        issuer : "-----"
    });
```

발행된 토큰을 긁어 jwt.io에서 확인하면 발행 시간과 종료시간이 토큰 내용 안에 포함되어있음을 확인할 수 있다. 

issuer는 발행자 이름이다.

