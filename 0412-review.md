# :fire: 04/12 내용 정리

## :one: SQL - database paging

한 페이지에 뿌릴 수 있는 만큼만 정보를 쪼개서 건내주는 것.

```sql
SELECT * FROM BookShop.books limit 3 offset 0;
# limit : 출력할 행의 개수
# offset : 시작 지점
# 인덱스 0번부터 3장씩 정보를 보여줘.

SELECT * FROM BookShop.books limit 0, 3;
# offset을 생략가능하다. 위와 아래 구문은 똑같이 동작한다.
```

limit 값 offset 값은 프론트엔드에서 결정해 넘겨 받아야한다.
주로 request query로 limit 값과 page 값을 받고, page 값을 토대로 offset 값을 계산한다.

sql 문에 조건을 붙이는 순서 중요하다.
- limit offset은 제일 마지막에 붙이자.
- join 구문은 select 구문 바로 다음에 붙이자.

---

## :two: SQL - 시간 설정

- 일정 시간 이전을 출력하려면 date_sub()
- 일정 시간 이후를 출력하려면 date_add()

```sql
select date_add("2024-01-01", interval 1 month);
select date_add(now(), interval 1 month);
select date_sub("2024-01-01", interval 1 day);
```

- 일정 기간 사이의 시간을 출력하려면 between ~ and ~

```
select * from BookShop.books
where pub_date between date_sub(now(), interval 1 month) and now();
```
