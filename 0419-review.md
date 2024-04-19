# :fire: 04/19 내용 정리

## :one: MySQL 데이터 삭제

- DELETE
  > DELETE FROM 테이블명 (WHERE 조건);
  - 조건이 없으면 모든 행이 삭제. 테이블은 남아 있다.
  - auto_increament의 순서가 초기화 되지 않는다.
- DROP
  > DROP TABLE 테이블명;
  - 테이블을 통째로 삭제.
- TRUNCATE
  > TRUNCATE 테이블명;
  - 모든 행이 삭제. 테이블은 남아 있다.
  - **auto_increament의 순서가 초기화 된다.** ← ★DELETE와의 차이점★
 
### TRUNCATE의 1701 오류 해결 방법

> Error Code: 1701. Cannot truncate a table referenced in a foreign key constraint ...

```sql
SET FOREIGN_KEY_CHECKS = 0;
--foreign key를 참조하지 않도록 설정 후 TRUNCATE를 실행한다. 작업이 끝나면 반드시 FOREIGN_KEY_CHECK = 1로 되돌려준다.
```
