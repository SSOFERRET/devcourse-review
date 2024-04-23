# :fire: 04/23 내용 정리

## :one: pagination 구현 위해 필요한 정보

- 현재 페이지
- 전체 페이지가 몇 페이지인지

도서의 요약 설명 배열과 는 별개로, json 형태로 페이지 정보를 전달해준다.
```
{
  books: [
    {
      id: 도서 id,
      title: 도서 제목
      ...
    },
    ...
  ],
  pagination: {
    totalCount: 총 도서수, // 총 페이지 수는 총 도서수를 토대로 프론트엔드 측에서 게산 가능하다.
    currentPage: 현재 페이지
  }
}
```

---

## :two: SQL - SQL_CALC_FOUND_ROWS

- select 쿼리 결과의 전체 row 수를 저장하게 하는 기능.
- LIMIT을 설정해도 그와 상관 없이 전체의 row를 게산해준다. 이 특징이 자칫하면 쿼리 성능을 떨어뜨릴 수 있다.
- **FOUND_ROWS()**와 쌍으로 움직인다. SQL_CALC_FOUND_ROWS가 저장한 수를 반환하는 역할을 한다.

```
SELECT SQL_CALC_FOUND_ROWS * FROM BookShop.books LIMIT 4 OFFSET 0;
SELECT FOUND_ROWS();
```

오늘 코드에 적용하면 아래와 같은 SQL이 짜여진다.

```
select SQL_CALC_FOUND_ROWS *,
  (select count(*) from BookShop.likes where BookShop.likes.liked_book_id=BookShop.books.id)
  as likes, 
  (select exists
    (select * from BookShop.likes
    where BookShop.likes.user_id=1 and BookShop.likes.liked_book_id=BookShop.books.id)
  )
  as my_like,
  (select category_name from BookShop.category where BookShop.books.category_id = BookShop.category.id)
  as category_name
  from BookShop.books limit 8 offset 0;

select found_rows();
```
