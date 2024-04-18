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

### 🗒 비동기 처리를 위한 콜백 패턴의 단점
1. 비동기 처리 다음에 또 비동기 처리가 여러 차례 필요하게 된다면 함수 호출이 중첩되어 복잡도가 높아진다 → 콜백 헬
```javascript
// 콜백 헬 예시
step1(function (value1) {
    step2(function (value2) {
        step3(function (value3) {
            step4(function (value4) {
                step5(function (value5) {
                    step6(function (value6) {
                        // Do something with value6
                    });
                });
            });
        });
    });
});
```
2. 에러 처리가 곤란하다.
```javascript
try {
  setTimeout(() => {throw new Error('Error');}, 1000);
} catch (e) {
  console.error('에러 발생'), e); // 실상 이 코드는 에러를 캐치하지 못한다.
}
```
setTimeout은 비동기 함수이기 때문에 콜백 함수가 호출되는 것을 기다리지 않고 즉시 종료가 된다.

콜백 함수가 실행될 때 setTimeout은 이미 콜 스택에서 제거된 상태인데, 이는 콜백 함수를 호출한 것이 setTimeout 함수가 아니라는 뜻이다.

에러는 호출자 방향으로 전파되는데, setTimeout 함수는 호출자가 아니므로 콜백 함수가 발생시킨 에러는 catch 블록에서 캐치되지 않는다.

---

## :two: Promise

### 🗒 Promise 객체, 그의 메서드(then, catch, finally)

Promise는 객체다. 

- then 메서드는 두 개의 콜백 함수를 인수로 받는다.
  - 첫번째 콜백함수는 Promise가 성공하여 resolve 함수가 호출되었을 때 호출된다.
  - 두번재 콜백함수는 Promise에서 에러가 나 reject 함수가 호출되었을 때 호출된다.

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

- catch 메서드는 한 개의 콜백 함수를 인수로 받는다. 이 메서드의 콜백 함수는 Promise가 rejected 되었을 때만 호출된다.

```javascript
let promise = new Promise(function(_, reject) {
    reject(new Error('Error'));
  }); 

promise.catch(e => console.log(e)) // Error: Error
```

- finally 메서드는 한 개의 콜백 함수를 인수로 받는다. 이 메서드의 콜백 함수는 Promise가 성공 여부와 상관 없이 무조건 호출된다.

Promise의 then, catch, finally는 모두 프로미스를 반환한다.


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
);

/**
* 완료!
* 완료!
* 완료!
*/
```

---

## :three: async & await

Promise 객체를 좀 더 쉽고 편하게 사용할 수 있는 문법이다.

### 🗒 async : 무조건 Promise 객체를 반환

```javascript
async function f() {
  return 7;
  // 위의 구문은 아래와 같이 동작한다.
  // return Promise.resolve(7);
  // 즉, async 함수는 무조건 Promise 객체를 반환한다. 반환 값이 아니면, Promise.resolve()로 감싼다.
}

f().then(
  function(result) {
    console.log("promise resolve : ", result);
  },
  function(error) {
    console.log("promise reject : ", error);
  }
);

// promise resolve : 7
```

### 🗒 await : async를 좀 더 특별하게

await는 Promise 객체를 기다려주는 함수이다. async 안에서만 사용할 수 있다.

Promise()의 then 메서드의 기능을 함수 안으로 들인 것과 같이 기능한다.

```javascript
async function f() {
    let promise = new Promise(function(resolve, reject) {
        setTimeout(() => resolve("완료"), 3000);
    });
    let result = await promise;
    console.log(result);
}
f();
```

여러 일을 동기적으로 호출하고 싶을 때에는 아래처럼 await를 여러차례 사용해주면 된다.

```javascript
async function f() {
  let promise1 = new Promise(function(resolve, reject) {
    setTimeout(() => resolve("첫번째 쿼리"), 3000);
  });
  let result = await promise1;
  console.log(result);

  let promise2 = new Promise(function(resolve, reject) {
    setTimeout(() => resolve("두번째 쿼리"), 3000);
  });
  result = await promise2;
  console.log(result);

  let promise3 = new Promise(function(resolve, reject) {
    setTimeout(() => resolve("세번째 쿼리"), 3000);
  });
  result = await promise3;
  console.log(result);
}

f();
```

---

## :four: query 순서대로 실행하기

mysql에서는 query를 Promise 객체로 제공할 수 있다.
그 방법은 npm의 mysql2 문서로 확인할 수 있다. 
