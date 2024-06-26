# :fire: 03/27 배운 내용 정리

## :one: DBMS

#### 데이터베이스란

데이터를 효율적으로 관리하기 위하여 묶은 데이터의 집합체.

#### DBMS

Database Management System

데이터베이스를 관리하는 시스템. SQL 언어를 사용한다.

- Elasticsearch
- Oracle

## :two: RDBMS를 사용하는 이유

Relational DBMS

데이터 간의 관계를 토대로 데이터에 접근 할 수 있는 DBMS.

(예: 마인드맵, 배우 공유에 대한 정보에 접근할 때 도깨비 검색을 통함.)

## :three: PK, 데이터 중복, 정규화

#### 정규화 

테이블을 쪼갠다.

#### PK, FK

- Primary Key : PK 기본키

해당 테이블의 각 행을 고유하게 만들어주는 Key 값.

- Foreign Key : FK 외래키

A 테이블에서 B 테이블의 데이터를 찾아가고 싶을 때 사용하는 Key 값. 

(예)

| 게시글 번호 | 제목 | 내용 | 작성일자 | 작성자 | 작성자 생년월일|
|:-:         |:-:   | :-: | :-:     | :-:    | :-:           |
| 1 | 졸리다 | 냥냥냥 | 240327 | 김철수 | 000612 |
| 2 | 잠온다 | 왈왈왈 | 240327 | 김영희 | 010205 |
| 3 | 피곤타 | 꿀꿀꿀 | 240327 | 김철수 | 950303 |
| 4 | 피곤타 | 꿀꿀꿀 | 240327 | 김철수 | 000612 |

이 테이블을 게시글 테이블과 사용자 테이블로 한 차례 쪼갤 수 있다(정규화).

| 게시글 번호 | 제목 | 내용 | 작성일자 |작성자 번호<br>(FK)| 
|:-:|:-:|:-:|:-:|:-:|                 
| 1 | 졸리다 | 냥냥냥 | 240327 |1|                         
| 2 | 잠온다 | 왈왈왈 | 240327 |2|                         
| 3 | 피곤타 | 꿀꿀꿀 | 240327 |3|                         
| 4 | 피곤타 | 꿀꿀꿀 | 240327 |1| 

|사용자 번호<br>(PK)|작성자 | 작성자 생년월일|
|:-:|:-:|:-:|
|1| 김철수 | 000612 |
|2| 김영희 | 010205 |
|3| 김철수 | 950303 |

- 정규화의 장점 : 데이터 중복이 줄어든다.
- 정규화의 단점 : 데이터를 찾기 불편해진다.

## :four: 데이터베이스 연관관계

테이블 간 어떤 관계를 가지고 있는가?

- 1:1
- 1:N
- M:N

## :five: 블로그 회원 테이블 만들기

<블로그 테이블>
|블로그 번호|블로그 이름|구독자 수|게시글 수|블로그 주인|회원 id|비밀번호|연락처|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|1| 주식 블로그   |    1| 10 | 김철수 | chul | 4444 | 010-0000-0000|
|2| 주식 블로그   |   20| 20 | 박영희 | young| !!!! | 010-1111-1111|
|3| 사진 블로그   |  300| 30 | 김철수 | chul | 4444 | 010-0000-0000|
|4| 강아지 블로그 |  500| 100| 이영희 | bombom | #### | 010-5555-5555|

 ⬇ 정규화


<블로그테이블>
|블로그 번호|블로그 이름|구독자 수|게시글 수|회원 id|
|:-:|:-:|:-:|:-:|:-:|
|1| 주식 블로그   |    1| 10 |chul |
|2| 주식 블로그   |   20| 20 |young|
|3| 사진 블로그   |  300| 30 |chul |
|4| 강아지 블로그 |  500| 100|bombom |

<블로그 사용자 테이블>
|블로그 주인|회원 id|비밀번호|연락처|
|:-:|:-:|:-:|:-:|
| 김철수 | chul | 4444 | 010-0000-0000|
| 박영희 | young| !!!! | 010-1111-1111|
| 이영희 | bombom | #### | 010-5555-5555|