CRA  = create-react-app
:  초기 설정과 구성을 자동화해서 빠르게 리액트 앱을
웹팩과 바벨 사용
핫 모듈 리로딩 
process.env.KEY : 노드 계열에서 프로세스를 환경 변수

vite
빠른 속도와 효율
ESBuild
웹팩 대신 롤업
모듈 단위로 빌드를 해서 브라우저 제공하기 때문에 속도가 빠르다.

---
npx create-react-app book-store-c --template typscript
-----
프로젝트 폴더 구조
1. pages - 라우트에 대응하는 페이지 컴포넌트(컨테이너)
2. components - 공통 컴포넌트, 각 페이지에서 사용되는 컴포넌트
3. utils - 유틸리티
4. hooks - 리액트 훅
5. model - 모델 (타입)
6. api - api 연동을 위한 fetcher 등 
---
package.json script에 react cli를 추가하기
npm run build - spa(single page app)
```
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject",
    "typecheck": "tsc --noEmit --skipLibCheck" // tsc를 통해 타입 체크를 할 수 있다. 정확히는 컴파일을 위한 명령어인데, 컴파일 과정에서 타입체크를 해주기 때문에 이렇게 많이 사용된다. // noEmit은 js로의 컴파일은 생략하겠다는 옵션이다. // skipLibCheck는 외부 라이브러리 타입체크는 생략하겠다는 옵션이다.
  }
```
---
tsconfig.json
----
레이아웃은 왜 필요할까
1. 프로젝트의 기본적인 화면 구조를 잡는다.
2. 반복적으로 들어가야하는 헤더, 푸터 등을 매 화면마다 제공한다.
3. 상황과 필요에 따라 레이아웃이 변경될 수 있도록 대비한다.
---
picocss
public >index.html에 
    <link rel="stylesheet"href="https://cdn.jsdelivr.net/npm/@picocss/pico@1/css/pico.min.css">

---
ReactNode
---
props
----
글로벌스타일과 스타일드 컴포턴트
글로발 스타일
- 프로젝트 전체에 적용되는, 프로젝트의 일관된 스타일링
- 'user agent stylesheet'로 표시되는 브라우저의 기본 스타일이 차이를 만든다.
- 브라우저 간의 스타일 차이를 극복하기 위해 사용.

<css의 글로벌 스타일>
- 에릭마이어의 reset css
- normalize.css
- sanitize.css
--- 
스타일드 컴포넌트
- css-in-js는 왜 필요할가 => 관심사의 분리를 위함
 - 전역 충돌
 - 의존성 관리 어려움
 - 불필요한 코드, 오버라이딩
 - 압축
 - 상태 공유 어려움
 - 순서와 명시도
 - 캡슐화

style>global.ts 파일을 만들어 css및 스타일드 컴포넌트 적용, index.tsx의 root.render메소드에 적용해줌.
```
root.render(
  <React.StrictMode>
    <GlobalStyle />
    <App />
  </React.StrictMode>
);
```
---
테마 만들기
- UI UX의 일관성 유지
- 유지보수 용이
- 확장성
- 재사용성
- 사용자 정의

테마 프로바이더 > 컴포넌트 > color > 그린 테마 / 블루 테마
```
const HeaderStyle = styled.header`
    background-color: ${(props) => props.theme.color.background};
    h1 {
        color: ${(props) => props.theme.color.primary};
    }
`
```
--- 
테마 스위처
Theme Switcher with Context API
- 토글 UI를 통해 웹사이트의 색상 테마를 바꿀 수 있다.
- 색상 테마는 전역 상태로 존재한다.
- 사용자가 선택한 테마는 로컬 스토리지에 저장한다.
