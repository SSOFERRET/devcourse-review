# FE 구조 설계서와 상세 설계서

## 구조 설계서의 개관 사항

- 개요
  - Main Framework: React.js v18
  - State Management Library: TanStack React Query
  - Style Library: styled-components
  - Dev.Server URL: http://localhost:3000
  - NPM Command: package.json
    - start: 로컬 개발 서버 실행
    - build: 프로덕션 배포를 위한 Webpack 빌드

## FE 패키지 구조

- 디렉토리: /src
  - index.tsx: 스크립트 진입점 파일
  - App.tsx: 메인 앱 컴포넌트
  - App.css: 메인 스타일시트
  - router.tsx: 라우터 파일
  - settings.ts: 환경 변수 파일
  - pages/: 페이지 컴포넌트 모음
  - components/: 세부 컴포넌트 모음
  - hooks/: Hook ahdma
  - apis/:API 호출 함수 모음
  - types/: 모델 타입 모음
  - utils/: 유틸리티 함수 및 객체 모음
  - assets/: 아이콘 및 이미지 모음

- 디렉토리: /src/pages
  |파일명|경로|내용|
  |:--:|:--:|:--:|
  |Index.tsx|"/"|메인 랜딩 페이지|
  |Login.tsx|"/login"|로그인 페이지|
  |Join.tsx|"/join"|회원가입 페이지|
  |Error.tsx|"/error"|에러 페이지(404, 401...)|

- 디렉토리: /src/pages/notes
  |파일명|경로|내용|
  |:--:|:--:|:--:|
  |Index.tsx|"/notes"|노트 목록 페이지|
  |Detail.tsx|"/notes/{:noteId}"|노트 상세 및 편집 페이지|

....

## 패키지 구조
- Page Components
- (Smaller) Components
- API 함수
- React Query Hooks
- Utility

