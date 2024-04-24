# 🔥 04/24 내용 정리
## 1️⃣ response format

변수명 케이스를 일관성 있게 보내줌이 좋다.
- 데이터베이스에서는 snake를 사용한다.
- 프론트엔드 단에서는 camel을 사용한다.

json 전송 단계에서 프론트엔드에 맞게 camel로 변경해줌이 좋다.

<방법>
- json key 값 변경
- query문에서 칼럼명 설정해주기

---

## :two: 코드 고도화

- response 포맷 케이스 통일하라. 
- status code 설정하라.

- 데이터베이스 중복 코드는 모듈화하라.

  ex. UserController : User 데이터 전용 모듈을 만들어 CRUD를 하도록 한다.

  cf. DB 모듈: mysql => 몽구스, 시퀄라이즈

- 패키지 구조
  - Router: 경로(URI,URL)와 HTTP method로 요청에 따른 경로를 찾아주는 역할
  - Controller: 경로를 찾아오면 무슨 일을 해야하는지 알려준다. 길 매니저 역할
  - Service: 직접 일하기. 비즈니스 조직. ex) 데이터베이스 어떤 쿼리를 부를지 결정하기 등등.
  - Model: 데이터베이스와의 소통. ex) 쿼리 집합

- :star: 예외 처리 (try/catch)
  - 유효성 검사 추가
  - jwt
    - access token: BookShop 프로젝트에서 사용한 거. (로그인 인증/인가 시 사용)
    - refresh token: 로그인을 연장

- 랜덤 데이터 (외부) API를 활용해서 isbn 샘플 데이터들을 채워볼 수 있다. (후에 학습할 예정)
- nodemon 모듈: 변화 저장 시 자동 저장 모듈 

---

## :three: 초미니 프로젝트: 랜덤 데이터 API 사용하여 가짜 사용자 정보 생성 API 만들기!

- 개요: 랜덤 데이터를 생성해주는 외부 API가 있다! 이를 기반으로 우리 모든 사이트에 사용되는 주요기능인 '사용자 정보 생성' API를 만들어보자.
- 내용
  - 랜덤 데이터 생성 API 사용하기
  - 가짜 사용자 정보를 생성하는 Express 웹/앱 API 만들어보기

### 랜덤 데이터 생성 API

- faker.js ← 좀 더 간편함.
- mockaroo
