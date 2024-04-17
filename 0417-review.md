# :fire: 04/17 내용 정리

## :one: 방금 insert한 데이터 PK 가져오기

- LAST_INSERT_ID() : 최신 데이터가 아닌 이전 값을 들고오는 오류가 종종 있다.
- MAX()

```
// SQL 입력하고
INSERT INTO orders (book_title, total_quantity, total_price, user_id, delivery_id) 
VALUES ("어린왕자들", 3, 60000, 1, delivery_id);

// id값 중에 max인 것(가장 최신 것)을 javascript 변수로 받는다.
SELECT max(id) FROM orders;
```
