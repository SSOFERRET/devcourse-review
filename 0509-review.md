# :fire: 05/09 학습 내용

## :one: 리액트란 무엇인가

- 자바스크립트 라이브러리의 하나.
- 사용자 인터페이스를 만들기 위해 페이스북에서 개발.
- 싱글 페이지 어플리케이션 및 모바일 어플리케이션 개발 가능.
- 2013년 발표되어 오픈소스화 됨.

### :memo: 리액트의 동작 원리

- 초기 렌더링  
- 가상 DOM 변경
- 재조정
- 실제 DOM 업데이트

### :memo: 리액트 시작하기

> https://ko.legacy.reactjs.org/


Create React App으로 간단하게 프로젝트 시작을 할 수 있다.

> https://ko.legacy.reactjs.org/docs/create-a-new-react-app.html#create-react-app

```
npx create-react-app my-app
cd my-app
npm start
```

---

## :two: jsx문법

- 자바스크립트의 확장 문법.
- HTML 문법을 JavaScript 코드 내부에 쓴 것이다.

```
function App() {
  let name = "리액트";
  let port = undefined;
  return (
    <div className="container">
      <h1>Hello, {name}!!</h1>
      <p>반갑습니다.</p>
      <p>
        {
          /* 주석문 문법은 이와 같다. */
          port || '포트를 설정하지 않았습니다.'
        }
      </p>
    </div>
  )
}
```

![4](https://github.com/SSOFERRET/devcourse-review/assets/148465774/a4290514-fbb1-49fa-acaa-e04befc4ed7c)

