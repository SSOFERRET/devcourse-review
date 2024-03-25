# 03/25 배운 내용 정리

## :one: 빈 객체 확인하는 방법 3가지

1. 객체.keys() ← BEST!
2. for
3. lodash 모듈 : isEmpty


Object.keys()는 object의 keys를 배열로 반환한다.


```
const str1 = "one";
const str2 = "";

const isEmpty =(obj)=> {
    if (obj.constructor === Object) {
        return !Object.keys(obj).length;
    }
}

console.log(isEmpty(str1)); // false
console.log(isEmpty(str2)); // true
```


## :two: 라우트

같은 url을 공유하는 메소드끼리 묶어줄 수 있다.

```
app
    .route('/channels')
    .get((req, res) => {
        res.send("전체 조회");
    })
    .post((req, res) => {
        res.send("새 채널 생성")  ;     
    })

app
    .route('/channels/:id')
    .get((req, res) => {
        res.send("개별 조회");
    })
    .put((req, res) => {
        res.send("개별 수정");
    })
    .delete((req, res) => {
        res.send("개별 삭제");
    })
```