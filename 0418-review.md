# :fire: 04/18 내용 정리

## :one: Node.js 비동기

```javascript
...
conn.query(sql, values, 
  (err, results) => {
    if(err) {
      console.log(err);
      return res.status(StatusCodes.BAD_REQUEST).end();
    }
    deliveryId = results.insertId;
// 1
    console.log(deliveryId) // 5
  });
// 2
  console.log(deliveryId) //undefined
```

위 코드에서 2번 콘솔 출력이 1번보다 먼저 나온다. deliveryId에 값이 할당되기 전에 2번의 콘솔 출력이 실행된다. 그 이유는 Node.js가 논블로킹 I/O 방식의 일처리를 채택하고 있기 때문에, 비동기적으로 동작하기 때문이다.

**비동기가 발생하는 경우**는 다음과 같은 코드가 사용되었을 때이다.

```javascript
// 비동기 발생 : 코드의 실행 완료까지 기다려야하는 시간이 생긴다.
setTimeOut()
setInterval()
query()
```

**비동기를 처리 방식**은 다음과 같다.

- 콜백 함수
- Promise(resolve, reject)
- then & catch
- async & await ← ★

---

## :two: Promise

### 🗒 Promise 객체

Promise는 객체다. 

```javascript
let promise = new Promise(function(resolve, reject) { //Promise()는 매개변수로 함수를 받는다. resolve와 reject도 콜백함수이다.
  // executor : Promise가 실행할 할 일.
  // executor 성공할 시 결과 값이 resolve 함수에 인가되어 호출되고, 실패할 시 에러 값이 reject 함수에 인가되어 호출된다.
    setTimeout(() => resolve("완료!"), 3000);
  }); 

promise.then(
  function(result){
    console.log(result);
  },
  function(error){}
);
//promise의 then() 메서드의 첫번째 매개변수는 resolve()에, 두번째 매개변수는 reject()에 연결되어 있다.
```

### 🗒 Promise chaining

Promise가 실행해야하는 일이 여러 개 있을 경우, 일을 순서대로 호출하기 위한 Promise 문법.

```javascript
let promise = new Promise(function(resolve, reject) {
    setTimeout(() => resolve("완료!"), 3000);
  }).then(
  function(result){
    console.log(result);
    return result; // << return으로 result를 다음 then으로 넘겨주어야 한다.
  },
  function(error){}
).then(
  function(result){
    console.log(result);
    return result;
  },
  function(error){}
).then(
  function(result){
    console.log(result);
  },
  function(error){}
) ;
```
