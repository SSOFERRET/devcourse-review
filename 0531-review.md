로그인과 전역상태

로그인 성공 -> 토큰 획득 -> 전역 상태 isLoggedIn:true
로그인 성공 -> 토큰 획득 -> 토큰 저장 localstorage <- http client headiers.Authorazation
---
zustand //
---
도서 목록 페이지
- 도서 목록을 fetch하여 화면에 렌더
- 페이지네이션을 구현
- 검색 겨로가가 없을 때 결고 없음 화면 노출
- 카테고리 및 신간 필터 기능을 제공
- 목록의 view는 그리드 형태, 목록 형태로 변경 가능.
