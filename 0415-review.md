# :fire: 04/15 내용 정리

## :one: Workbench - read-only table 편집 권한 얻기

PK가 없는 테이블의 경우 편집 권한이 제한된다.

---

## :two: SQL - count()

어떤 조건을 만족하는 행의 개수를 count 하여 출력 받을 수 있다.

```
select count(*) from BookShop.likes where liked_book_id=1;
// id 값이 1인 책의 좋아요 수를 세어달라.
```

---

## :three: SQL - 서브쿼리와 AS

likes 테이블에서 확인할 수 있는 데이터인 도서별 좋아요 수를, books 테이블을 select할 때 같이 확인하고 싶다면 다음과 같은 sql을 작성하면 된다.

```
select *, (select count(*) from BookShop.likes
where BookShop.likes.liked_book_id=BookShop.books.id) as likes
from BookShop.books;
// books 테이블의 전체 칼럼을 셀렉트
// (likes 테이블에서 해당 책 아이디와 일치하는 값의 데이터 행 수를 likes 칼럼으로 추가하여 출력하라)
// * 출력할 칼럼명을 입력하는 위치에 likes 칼럼에 대한 요구를 입력함.
```

- as는 새로 추가할 칼럼 명.
- 서브쿼리 : 쿼리 안에 쿼리. 쿼리의 결과값을 상위 쿼리에 적용한다.

---

## :four: SQL - exists

어떤 조건을 만족하는 행이 존재하는지 확인하는 구문

```
select exists
(select * from BookShop.likes
where BookShop.likes.user_id=1
and BookShop.likes.liked_book_id=BookShop.books.id)
// 존재 여부에 따라 1 또는 0으로 출력
```
