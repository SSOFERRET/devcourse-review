# :fire: 03/28 배운 내용 정리

## :one: 게시판 유저와 게시글 테이블 생성

- PK 표시 : PRIMARY KEY (칼럼명)
- 자동 숫자 올림 : auto_increment
    - auto_increment_lock_mode
- null 금지 : not null
- 작성일자 : created_at TIMESTAMP DEFAULT NOW()

```
MariaDB [board]> create table user(
    -> id int not null auto_increment,
    -> name varchar(30) not null,
    -> birth date,
    -> primary key(id)
    -> );

Query OK, 0 rows affected (0.018 sec)

MariaDB [board]> desc user
    -> ;
+-------+-------------+------+-----+---------+----------------+
| Field | Type        | Null | Key | Default | Extra          |
+-------+-------------+------+-----+---------+----------------+
| id    | int(11)     | NO   | PRI | NULL    | auto_increment |
| name  | varchar(30) | NO   |     | NULL    |                |
| birth | date        | YES  |     | NULL    |                |
+-------+-------------+------+-----+---------+----------------+
3 rows in set (0.001 sec)

MariaDB [board]> create table post (
    -> id int not null auto_increment,
    -> title varchar(100) not null,
    -> subscribe varchar(2000) not null,
    -> created_at timestamp default now(),
    -> userId int not null,
    -> primary key(id)
    -> );
Query OK, 0 rows affected (0.017 sec)

MariaDB [board]> desc post
    -> ;
+------------+---------------+------+-----+---------------------+----------------+
| Field      | Type          | Null | Key | Default             | Extra          |
+------------+---------------+------+-----+---------------------+----------------+
| id         | int(11)       | NO   | PRI | NULL                | auto_increment |
| title      | varchar(100)  | NO   |     | NULL                |                |
| subscribe  | varchar(2000) | NO   |     | NULL                |                |
| created_at | timestamp     | YES  |     | current_timestamp() |                |
| userId     | int(11)       | NO   |     | NULL                |                |
+------------+---------------+------+-----+---------------------+----------------+
5 rows in set (0.001 sec)
```
---

## :two: 테이블에 데이터 삽입

```
MariaDB [board]> insert into user (name, birth) values ( "김철수", "000612");
Query OK, 1 row affected (0.009 sec)

MariaDB [board]> insert into user (name, birth) values ( "김영희", "010205");
Query OK, 1 row affected (0.002 sec)

MariaDB [board]> insert into user (name, birth) values ( "김철수", "950303");
Query OK, 1 row affected (0.002 sec)

MariaDB [board]> select * from user;
+----+-----------+------------+
| id | name      | birth      |
+----+-----------+------------+
|  1 | 김철수    | 2000-06-12 |
|  2 | 김영희    | 2001-02-05 |
|  3 | 김철수    | 1995-03-03 |
+----+-----------+------------+
3 rows in set (0.000 sec)
```

---

## :three: timestamp

- date
    - 날짜만
    - YYYY-MM-DD

- datetime 
    - 날짜 + 시간
    - YYYY-MM-DD HH:MM:SS(24시간제)

- time
    - 시간만
    - HH:MM:SS

- **timestamp** : 자동 입력
    - 날짜 + 시간
    - YYYY-MM-DD HH:MM:SS(24시간제)
    - 시스템 시간대 정보에 맞게 일시를 저장한다.

cf. UTC : 한국 시간 - 9;

---

## :four: default

데이터 입력이 공란으로 들어왔을 때, 기본 값으로 셋팅.
null이라고 작성해서 넣으면 null이 셋팅된다.

```
MariaDB [board]> insert into user values (
    -> "김철수", "000612");
ERROR 1136 (21S01): Column count doesn't match value count at row 1
MariaDB [board]> insert into user (name, birth) values ( "김철수", "000612");
Query OK, 1 row affected (0.009 sec)

MariaDB [board]> insert into user (name, birth) values ( "김영희", "010205");
Query OK, 1 row affected (0.002 sec)

MariaDB [board]> insert into user (name, birth) values ( "김철수", "950303");
Query OK, 1 row affected (0.002 sec)

MariaDB [board]> select * from user;
+----+-----------+------------+
| id | name      | birth      |
+----+-----------+------------+
|  1 | 김철수    | 2000-06-12 |
|  2 | 김영희    | 2001-02-05 |
|  3 | 김철수    | 1995-03-03 |
+----+-----------+------------+
3 rows in set (0.000 sec)

MariaDB [board]> insert into post (title, subscribe, userId) values(
    -> "졸리다" , "냥냥냥", 1);
Query OK, 1 row affected (0.009 sec)

MariaDB [board]> insert into post (title, subscribe, userId) values( "잠온다" , "왈왈왈", 2);
Query OK, 1 row affected (0.003 sec)

MariaDB [board]> insert into post (title, subscribe, userId) values( "피곤타" , "꿀꿀꿀", 3);
Query OK, 1 row affected (0.009 sec)

MariaDB [board]> insert into post (title, subscribe, userId) values( "피곤타" , "왱왱왱", 1);
Query OK, 1 row affected (0.003 sec)

MariaDB [board]> select * from post;
+----+-----------+-----------+---------------------+--------+
| id | title     | subscribe | created_at          | userId |
+----+-----------+-----------+---------------------+--------+
|  1 | 졸리다    | 냥냥냥    | 2024-03-28 07:55:19 |      1 |
|  2 | 잠온다    | 왈왈왈    | 2024-03-28 07:55:37 |      2 |
|  3 | 피곤타    | 꿀꿀꿀    | 2024-03-28 07:55:48 |      3 |
|  4 | 피곤타    | 왱왱왱    | 2024-03-28 07:55:56 |      1 |
+----+-----------+-----------+---------------------+--------+
4 rows in set (0.000 sec)
```

---

## :five: 테이블에 칼럼 추가

```
MariaDB [board]> alter table post
    -> add column updated_at datetime
    -> default now()
    -> on update now();
Query OK, 0 rows affected (0.023 sec)
Records: 0  Duplicates: 0  Warnings: 0

MariaDB [board]> desc post
    -> ;
+------------+---------------+------+-----+---------------------+-------------------------------+
| Field      | Type          | Null | Key | Default             | Extra                         |
+------------+---------------+------+-----+---------------------+-------------------------------+
| id         | int(11)       | NO   | PRI | NULL                | auto_increment                |
| title      | varchar(100)  | NO   |     | NULL                |                               |
| subscribe  | varchar(2000) | NO   |     | NULL                |                               |
| created_at | timestamp     | YES  |     | current_timestamp() |                               |
| userId     | int(11)       | NO   |     | NULL                |                               |
| updated_at | datetime      | YES  |     | current_timestamp() | on update current_timestamp() |
+------------+---------------+------+-----+---------------------+-------------------------------+
6 rows in set (0.001 sec)

MariaDB [board]> update post
    -> set subscribe="맴맴맴"
    -> where id=2;
Query OK, 1 row affected (0.009 sec)
Rows matched: 1  Changed: 1  Warnings: 0

MariaDB [board]> select * from post;
+----+-----------+-----------+---------------------+--------+---------------------+
| id | title     | subscribe | created_at          | userId | updated_at          |
+----+-----------+-----------+---------------------+--------+---------------------+
|  1 | 졸리다    | 냥냥냥    | 2024-03-28 07:55:19 |      1 | 2024-03-28 08:01:49 |
|  2 | 잠온다    | 맴맴맴    | 2024-03-28 07:55:37 |      2 | 2024-03-28 08:03:31 |
|  3 | 피곤타    | 꿀꿀꿀    | 2024-03-28 07:55:48 |      3 | 2024-03-28 08:01:49 |
|  4 | 피곤타    | 왱왱왱    | 2024-03-28 07:55:56 |      1 | 2024-03-28 08:01:49 |
+----+-----------+-----------+---------------------+--------+---------------------+
4 rows in set (0.000 sec)

MariaDB [board]> insert into post (title, subscribe, userId) values("잠온다", "곽곽곽", 2);
Query OK, 1 row affected (0.011 sec)

MariaDB [board]> select * from post;
+----+-----------+-----------+---------------------+--------+---------------------+
| id | title     | subscribe | created_at          | userId | updated_at          |
+----+-----------+-----------+---------------------+--------+---------------------+
|  1 | 졸리다    | 냥냥냥    | 2024-03-28 07:55:19 |      1 | 2024-03-28 08:01:49 |
|  2 | 잠온다    | 맴맴맴    | 2024-03-28 07:55:37 |      2 | 2024-03-28 08:03:31 |
|  3 | 피곤타    | 꿀꿀꿀    | 2024-03-28 07:55:48 |      3 | 2024-03-28 08:01:49 |
|  4 | 피곤타    | 왱왱왱    | 2024-03-28 07:55:56 |      1 | 2024-03-28 08:01:49 |
|  5 | 잠온다    | 곽곽곽    | 2024-03-28 08:07:39 |      2 | 2024-03-28 08:07:39 |
+----+-----------+-----------+---------------------+--------+---------------------+
5 rows in set (0.000 sec)
```

---

## :six: 테이블에 FK 칼럼 추가

- MUL : 중복가능하다는 표시.

```
MariaDB [board]> alter table post
    -> add foreign key(userId)
    -> references user(id);
Query OK, 5 rows affected (0.034 sec)
Records: 5  Duplicates: 0  Warnings: 0

MariaDB [board]> desc post;
+------------+---------------+------+-----+---------------------+-------------------------------+
| Field      | Type          | Null | Key | Default             | Extra                         |
+------------+---------------+------+-----+---------------------+-------------------------------+
| id         | int(11)       | NO   | PRI | NULL                | auto_increment                |
| title      | varchar(100)  | NO   |     | NULL                |                               |
| subscribe  | varchar(2000) | NO   |     | NULL                |                               |
| created_at | timestamp     | YES  |     | current_timestamp() |                               |
| userId     | int(11)       | NO   | MUL | NULL                |                               |
| updated_at | datetime      | YES  |     | current_timestamp() | on update current_timestamp() |
+------------+---------------+------+-----+---------------------+-------------------------------+
6 rows in set (0.011 sec)
```

---

## :seven: JOIN

정규화되어 뿔뿔이 흩어진 데이터를, select된 항목에 한하여 데이터를 한데 묶은 테이블로 보여주는 기능이다.

```
MariaDB [board]> select post.id, title, subscribe, created_at, updated_at, name, birth from post left
    -> join user on post.userId = user.id;
+----+-----------+-----------+---------------------+---------------------+-----------+------------+
| id | title     | subscribe | created_at          | updated_at          | name      | birth      |
+----+-----------+-----------+---------------------+---------------------+-----------+------------+
|  1 | 졸리다    | 냥냥냥    | 2024-03-28 07:55:19 | 2024-03-28 08:01:49 | 김철수    | 2000-06-12 |
|  2 | 잠온다    | 맴맴맴    | 2024-03-28 07:55:37 | 2024-03-28 08:03:31 | 김영희    | 2001-02-05 |
|  3 | 피곤타    | 꿀꿀꿀    | 2024-03-28 07:55:48 | 2024-03-28 08:01:49 | 김철수    | 1995-03-03 |
|  4 | 피곤타    | 왱왱왱    | 2024-03-28 07:55:56 | 2024-03-28 08:01:49 | 김철수    | 2000-06-12 |
|  5 | 잠온다    | 곽곽곽    | 2024-03-28 08:07:39 | 2024-03-28 08:07:39 | 김영희    | 2001-02-05 |
+----+-----------+-----------+---------------------+---------------------+-----------+------------+
5 rows in set (0.000 sec)
```

