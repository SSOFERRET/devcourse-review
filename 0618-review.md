# :fire: 06/18

## CI 파이프라인

- 파이프라인에 대한 추상적 수준에서의 정의 <br />
  docker build → docker push → kubectl create → kubectl expose

### Github 비공개 repo로 젠킨스 사용하기

- https보다 ssh 프로토콜을 더 많이 이용
- Username/passwd 에 의한 인증보다 SSH key 에 의한 인증을 더 많이 이용한다

아래 페이지를 참고하여 ssh 키 생성 후 깃허브 계정 설정에 세팅하면 된다.<br />
https://git-scm.com/book/ko/v2/Git-%EC%84%9C%EB%B2%84-SSH-%EA%B3%B5%EA%B0%9C%ED%82%A4-%EB%A7%8C%EB%93%A4%EA%B8%B0
