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
   
## 패키지 구조
- Page Components
- (Smaller) Components
- API 함수
- React Query Hooks
- Utility

### FE 패키지 구조

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

- 디렉토리: /src/components
  |파일명|내용|역할|
  |:--:|:--:|:--:|
  |LoginForm.tsx|이메일/패스워드 인풋 및 로그인/회원가입 버튼|로그인을 위한 입력 폼 및 버튼 제공|
  |JoinForm.tsx|이메일/패스워드/패스워드확인 인풋 및 회원가입 버튼|회원가입을 위한 폼 및 버튼 제공|
  |NoteList.tsx|사용자가 작성한 노트의 목록|노트를 클릭하여 해당 노트를 상세 조회하거나 편집하는 페이지로 이동|
  |NoteTitleInput.tsx|노트 제목 인풋|노트 제목을 작성하는 인풋 컴포넌트|
  |NoteContentEditor.tsx|노트 내용 에디터 인풋|tiptap 라이브러리를 통해 노트 내용을 작성하는 인풋 컴포넌트|
  

- 디렉토리: /src/components/boundaries
   |파일명|역할|
  |:--:|:--:|
  |AppErrorBoundary.tsx|react-error-boundary 라이브러리를 통해 앱의 오류를 처리하는 컴포넌트|
  |RouteErrorBoundary.tsx|react-router-dom에서 발생하는 라우팅 에러를 처리하는 컴포넌트|

- 디렉토리: /src/components/hocs
   |파일명|역할|
  |:--:|:--:|
    |withUnauthenticated.tsx|로그인된 상태면 래핑한 컴포넌트를 렌더링하지 않고 노트 목록 URL로 리다이렉트를 시키는 HOC|
  |withAuthenticatedUser.tsx|로그인하지 않은 상태면 래핑한 컴포넌트를 렌더링하지 않고, 로그인 URL로 리다이렉트시키는 HOC. 로그인 되어있으면 래핑한 컴포넌트의 Props로 User 정보를 내려준다|
  |withCurrentNote.tsx|현재 URL을 기반으로 노트 정보를 불러오고, 래핑한 컴포넌트의 props로 노트의 정보를 내려준다|

- 디렉토리: /src/apis
  - fetchCurrentUser
  - requestLogin
  - requestLogout
  - requestJoin
  - fetchNotes
  - fetchNote
  - createNote
  - updateNote
  - deleteNote




