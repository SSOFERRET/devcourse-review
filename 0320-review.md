# :fire: 03/20까지 배운 내용 정리하기 (2~5장)

---

## :two: 웹의 이해

## :notebook: 웹

### 웹이란?
- 인터넷: 국제적인 광범위한 연결망.
- 웹 : 인터넷에 연결된 컴퓨터를 통해 사람들이 정보를 공유할 수 있는 공간.
- 하이퍼텍스트 : 다른 페이지로 이동하는 기능을 가진 텍스트

### 웹의 구조
- 클라이언트 : 서비스를 요청하는 컴퓨터
- 서버 : 서비스를 제공하는 컴퓨터

- 클라이언트와 서버는 HTTP라는 프로토콜을 통해 데이터를 주고 받는다.
    - HTTP : HyperText Transfer Protocol
    - 프로토콜 : 정해진 약속.약속된 틀.

### 웹 개발 직무의 이해
- 프론트엔드 : 클라이언트 측의 GUI 작업.
- 백엔드 : 서버와의 대화, 데이터 가공 처리를 작업.
- 풀스택 : 프론트엔드 일과 백엔드 일 둘 다 함.

---

## :notebook: 프론트엔드

### HTML vs CSS vs Javascript

- HTML : 구조
- CSS : 장식
- Javascript: 동작

### HTML

- Hyper Text Markup Language
- 태그 <> : HTML을 이루는 구성요소.

### CSS

- Cascade Style Sheet.
- 구성요소들을 장식해준다.
- 적용방법
    - 인라인 : HTML 태그 안에 작성.
    - 내부 스타일 시트 : HTML head 안에 작성. 태그명 style.
    - 외부 스타일 시트 : 스타일 시트 코드를 외부 파일(.css)에 작성. 
        외부에서 css 가져오기 : <link rel="stylesheet" href="login.css">

- 스타일 시트 호명 방법
    - HTML 태그명 호명(태그명 {})
    - 클래스명 호명(.클래스명 {}) : 여러 요소들이 속성 공유.
    - ID명 호명 (#ID명 {}) : 고유한 속성.

### Javascript

- HTML 요소를 선택하여 제어할 수 있는 스크립트 언어.
- Node.js(런타임 플랫폼)의 출현으로 스크립트 언어의 한계를 뛰어넘어 프로그래밍 언어로 활약 중.

- 스크립트 언어 : 프로그램 내부 요소 중 하나로, 프로그램의 어떤 요소를 제어하는 역할을 담당하는 언어.

- Javascript 적용 방법 
    - 인라인 : 태그 안에 속성 선택 후 작성. (속성명 on~으로 시작함.) 
    - 내부 스크립트 : body 태그 안 마지막에 script 태그
    - 외부 스크립트 : .js 파일로 별개로 분리.
        외부에서 스크립트 가져오기 : <script type="text/javascript" src="login.js"></script>

- HTML의 구성요소를 Javascript에서 찾기
    - id로 찾기 : document.getElementById('아이디명')
    - class로 찾기 : document.getElementsByClassName('클래스명')
    - tag로 찾기 : document.getElementsByTagName('태그명')

---
## :notebook: 백엔드

### 백엔드의 구조

- 웹 서버 : 정적 페이지에 대응. 동적 페이지는 웹 애플리케이션 서버로 넘긴다.
- 웹 애플리케이션 서버 : 동적 페이지에 대응. 데이터베이서와 소통하며 조회/연산 처리를 한다.
- 데이터베이스 : 데이터를 통합하여 효율적으로 관리한다.

### Node.js
- 자바스크립트를 웹 브라우저가 아닌 장에서도 동작할 수 있도록 만들어주는 런타임 플랫폼.

### 데이터베이스

- 데이터를 통합하여 효율적으로 관리하기 위한 데이터의 집합체.
- 중복 방지, 효율적, 빠름.

### DBMS 

- Database Management System.
- SQL 언어를 사용하여 데이터 처리, 검색, 연산, 조회.

### Docker

- 응용 프로그램들을 격리하는 기술.
- 컨테이너 : 실행되는 소프트웨어를 담는 그릇.

### SQL 

- Structured Query Language. 데이터베이스에 연산 요청하기 위한 언어.
    - 데이터 삽입 : INSERT
    - 데이터 조회 : SELECT
    - 데이터 수정 : UPDATE
    - 데이터 삭제 : DELETE

---

## :three: 백엔드의 기초

## :notebook: API

### API 

- Application Programming Interface
- 라이브러리에 접근하기 위한 규칙을 정의한 것.
- interface : 어떤 두 가지 요소 사이를 중재하고 매개해주는 역할.

### REST API

- 인터넷망 속 가상공간인 웹에서 데이터를 주고 받기 위한 규약인 HTTP를 잘 지킨 API
    - head : 통신상태(HTTP code), 응답 형태
    - body : 데이터 전달 요청, 데이터 전달
    - URL : Uniform Resource Locator
        인터넷상에서 웹페이지 위치를 알려주는 데이터 연산을 서버에게 요청하는 방법. method 키워드는 HTTP method로 별개로 입력해준다.

- HTTP method : HTTP에 담을 수 있는 method(목적)
    - 조회 = GET
    - 생성 = POST
    - 수정 = PUT/PATCH 
    - 삭제 = DELETE    

    cf. patch: 값이 바뀐것만 부분 수정l
        put : 값이 있든 없든 덮어쓰기.

---

## :notebook: Node.js

- javascript를 웹 브라우저 밖에서도 구동할 수 있는 환경을 만들어주는 런타임 플랫폼.
- React, Vue 등등 프론트엔드 프레임워크가 Node 기반으로 제작됨.

- 특징 :
    - 싱글스레드: 스레드 1개로 동작한다.
    - 이벤트 기반: 호출된 일만 실행한다.
    - 논블로킹 I/O : 스레드 1개가 일을 순차적으로 하는게 아니라, 중간중간 비는 시간에 다른 일을 한다. 비동기적.

- npm : Node.js를 설치하면 자동으로 설치되는, 외부 모듈을 가지고 와 내 프로젝트에 설치할 수 있도록 도와주는 역할을 해주는 프로그램.

- packge.json/ package-lock.json : npm이 가지고 온 외부 모듈의 목록을 보여주는 파일. 

---

## :notebook: http와 express

둘 다 웹 서버 운영을 담당하는 서버 모듈이다.

- http : node.js 기본 내장 모듈
- express : 외부 모듈. http를 보다 간결한 코드로 제어하게 도와준다.

### json

- javascript object notation
- 한 객체가 가지고 있는 데이터 덩어리. 효율적인 데이터 통신을 위해 사용.
- json 안 하나의 데이터는 key값과 value값의 쌍으로 이루어져있다.

---

## :notebook: Express: GET

- GET : 데이터 찾기를 담당하는 메소드.

### url 구조

- express.get으로 조회하고자 하는 정보는 url로 주고 받는다.
- req.params : url로 받은 object 정보를 QueryString으로 보여준다.

### 비구조화

- 배열이나 객체의 속성을 해체하여 그 값을 개별 변수에 담을 수 있게 하는 자바스크립트 표현식.

### Query

- 파일의 내용 등을 알기 위해 몇개의 code나 key를 기초로 질의하는 것.

### 네이밍 케이스

- kebab-case : HTML 태그의 id,  class 속성 등에 사용.
- PascalCase : class명 등에 사용.
- snake_case : 리소스 파일, 레이아웃 파일 이름 등에 사용.
- camelCase : 메소드와 변수 이름에 사용.

### Map()

- javascript에서 지원하는 객체 형태.
- Object와 다른 점 (아래 내용은 map의 특징이다.)
    - key type : string 외에도 사용 가능
    - 정렬 : 삽입한 순서대로 정렬된다.
    - 순회 : 데이터를 한바퀴 돌아보는 순회 기능이 있다.
    - 퍼포먼스 : 빈번한 추가 및 삭제에 최적화되어있다.
    - 지원 : JSON.stringify() 및 JSON.parse()를 지원하지 **않는다.**
    - 상속 : Object.prototyped의 상속에서 자유롭다.

- method
    - set() : 삽입, 수정(덮어쓰기)
    - get() : 찾기
    - delete() : 삭제

---

## :notebook: Express: POST

- POST : 새로운 데이터 등록 및 생성을 담당하는 메소드.
- post를 사용하면 서버와 클라이언트는 해당 데이터를 body에 숨겨서 주고 받게 된다. 
- req.body에서 QueryString 얻는다.


### Postman

- post method로 주고 받은 데이터는 숨겨져있으므로 웹 브라우저 상으로는 확인할 길이 없다. 이럴 때 사용하는 툴이 Postman이다.